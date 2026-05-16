# GitHub — Publication 1 sur 3 · Bloc d'annonce de version / README

**Utilisation :** Corps d'une version GitHub, une discussion épinglée, ou en haut du README du dépôt.
**Mots-clés :** UAP, UFO, archives PURSUE, documents déclassifiés, données ouvertes, recherche plein texte, OCR, traduction automatique, LLM local, Ollama, edge computing, API publique, Hono, TypeScript, Python
**Hyperliens :** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — une plateforme multilingue et consultable pour les archives UAP PURSUE

**En ligne :** https://www.ufolens.com  ·  **API :** https://www.ufolens.com/api/v1  ·  **Archive source :** https://www.war.gov/ufo

`ufolens.com` republie les archives **PURSUE** de documents déclassifiés UAP / UFO du Département de la Guerre des États-Unis en tant que plateforme de connaissances : recherche plein texte, traduction automatique sur l'ensemble du corpus, exploration par carte et par chronologie, et une API JSON publique. Les documents sources sont des œuvres du gouvernement fédéral américain et sont, aux États-Unis, dans le domaine public ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ce projet n'est **pas affilié au gouvernement américain**, n'utilise aucun insigne officiel et n'annule jamais les passages censurés.

### Architecture

```
Machine locale (Apple Silicon, IP résidentielle)     Réseau en périphérie (Edge)
──────────────────────────────────────────          ──────────────────────────
pipeline/  (Python 3.10, cœur stdlib-only)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish (progression unique) /{lang}/...   pages
  OCR: moteur open-source (fallback Tesseract CLI)     /api/v1/...   API publique
  translate / NER: LLM local (Gemma via Ollama)        /admin        console opérateur
  état: manifeste SQLite                               soutenu par: BDD SQL edge,
        │                                                stockage objet (PDF sources), cache KV
        └─ publie un paquet : SQL + manifeste d'assets + liste de purge de cache ─┘
```

- **Aucun coût d'IA cloud par document.** L'OCR et la traduction s'exécutent localement ; la machine à états à progression unique (`découvert → téléchargé → ocr_terminé → traduit → publié`) garantit qu'aucun document n'est retraité sauf s'il a changé.
- **Le cœur du pipeline n'a aucune dépendance tierce** — les modules d'analyse / de manifeste / de delta s'exécutent et sont testés sur un Python propre sans aucune installation via pip ; les étapes d'OCR/traduction se dégradent progressivement lorsque les paquets optionnels sont absents.
- **Le site en périphérie (edge)** applique des en-têtes de sécurité stricts + CSP (pas de `unsafe-inline` ; le JSON-LD en ligne est épinglé par sha256), la négociation de langue via `Accept-Language` + le mappage par pays, un cache de page KV de 30 jours, et une tâche cron de maintenance quotidienne.
- **Mises à jour incrémentielles :** un détecteur de delta compare l'index source et ne réinjecte que les changements dans le pipeline.

### Pour les développeurs

L'API publique sur https://www.ufolens.com/api/v1 renvoie les documents et les métadonnées au format JSON. L'accès anonyme est limité en débit ; demandez une clé pour les niveaux chercheur/développeur. Consultez la section API sur le site pour les points de terminaison et les limites.

### Statut

Code terminé ; site déployé sur https://www.ufolens.com. La base de données de production est remplie en exécutant le pipeline hors ligne et en publiant le paquet (`cli_publish run --remote`). La documentation de conception complète se trouve dans `docs/20260511/`.

### Licence / limites

- Documents sources : œuvres du gouvernement fédéral américain, domaine public aux États-Unis.
- Le code propre à cette plateforme : voir `LICENSE`.
- Le site envoie `Tdm-Reservation: 1` et `X-Robots-Tag: noai, noimageai` — indexable par les moteurs de recherche, mais désactivé pour l'entraînement et le scraping par IA.
- Les séquences vidéo sont attribuées à DVIDS / AARO et ne sont pas revendiquées par ce projet.

Les issues et les PR sont les bienvenues. Veuillez lire `CLAUDE.md` et `docs/20260511/00-*` avant de proposer des changements structurels.

