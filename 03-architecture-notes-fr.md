# GitHub — Publication 3 sur 3 · Notes d'architecture (Discussion de type ADR)

**Utilisation :** Une discussion sous « Démonstration » / « Architecture », ou une base pour un ADR dans `docs/`.
**Mots-clés :** architecture, ADR, machine à états à progression unique, LLM local, Ollama, OCR, edge computing, CSP, en-têtes de sécurité, pipeline de données, ingénierie des coûts, manifeste SQLite, D1, R2, KV
**Hyperliens :** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Pourquoi ufolens.com est conçu de cette manière

Notes sur les trois décisions qui ont façonné [ufolens.com](https://www.ufolens.com) (la reconstruction consultable et multilingue des [archives UAP PURSUE](https://www.war.gov/ufo)). Les commentaires et les contestations sont les bienvenus.

### 1. Le pipeline est une machine à états à progression unique — intentionnellement

États : `découvert → téléchargé → ocr_terminé → traduit → publié`. Un document ne fait que progresser, et uniquement lorsqu'il y a du travail à effectuer. Le contenu publié n'est jamais retraité, à moins qu'un détecteur de delta ne constate que la source a réellement changé.

**Pourquoi :** L'OCR et la traduction sont les opérations coûteuses, et les archives s'enrichissent avec le temps. Un pipeline qui « relance tout pour être sûr » a un coût illimité. Rendre les transitions arrière impossibles rend impossible une facture hors de contrôle. Le plafonnement des coûts est une propriété du graphe d'états, pas de la vigilance de l'opérateur.

**Coût :** les migrations de schéma et le retraitement intentionnel sont délibérément complexes. Un compromis acceptable.

### 2. L'OCR et la traduction s'exécutent sur un LLM local, pas sur une API cloud

OCR : moteur open-source, avec Tesseract CLI comme solution de repli. Traduction + NER : Gemma via Ollama, sur un ordinateur portable Apple Silicon.

**Pourquoi :** coût marginal nul par document ; reproductible (modèle + prompts fixes) ; et l'étape de récupération doit déjà s'exécuter depuis une IP résidentielle (la source est derrière Akamai Bot Manager — `curl` reçoit un 403), donc un ordinateur portable est de toute façon déjà dans la boucle.

**Coût :** la qualité de la traduction est inférieure à celle d'un modèle de pointe. Pour un corpus de référence où l'anglais original est toujours à un clic de distance, c'est acceptable. Nous ne prétendons pas que les traductions font autorité.

### 3. Les deux moitiés partagent exactement une interface : un paquet (bundle) publié

Le pipeline n'écrit jamais directement dans la base de données de production. Il émet `{ SQL, manifeste d'assets, liste de purge de cache }`. La « publication » consiste à appliquer ce paquet (pousser le SQL vers la base de données SQL edge, synchroniser les assets avec le stockage objet, purger les clés de cache nommées).

**Pourquoi :** la partie locale et la partie edge peuvent évoluer indépendamment ; le paquet est examinable ; et le « déploiement des données » a la même forme à chaque fois. Le Worker est une petite application TypeScript/Hono — CSP stricte (pas de `unsafe-inline` ; le JSON-LD en ligne est épinglé par sha256), négociation `Accept-Language` + pays→langue, cache de page KV de 30 jours, tâche cron de maintenance quotidienne — et il n'a jamais besoin de savoir comment les données ont été produites.

**Coût :** une modification du schéma D1 touche deux fichiers (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Une assurance peu coûteuse.

### Principes non négociables intégrés dans le comportement

- Non affilié au gouvernement américain ; aucun insigne officiel.
- Les passages censurés dans la source sont préservés, jamais inversés.
- Vidéo attribuée à DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` à l'échelle du site — indexable par les moteurs de recherche, avec option de retrait du scraping par IA.

En ligne : https://www.ufolens.com · API : https://www.ufolens.com/api/v1

