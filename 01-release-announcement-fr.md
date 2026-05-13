# GitHub — Publication 1 sur 3 · Bloc d'annonce de version / README

**Utilisation :** corps d'une version GitHub, discussion épinglée ou en haut du README du dépôt.
**Mots-clés :** UAP, UFO, archives PURSUE, documents déclassifiés, open data, recherche plein texte, OCR, traduction automatique, LLM local, Ollama, edge computing, API public, Hono, TypeScript, Python
**Liens :** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — une plateforme multilingue et interrogeable pour les archives PURSUE UAP

**En direct :** https://www.ufolens.com  ·  **API :** https://www.ufolens.com/api/v1  ·  **Archives sources :** https://www.war.gov/ufo

`ufolens.com` republie les archives **PURSUE** de documents déclassifiés UAP / UFO du Département de la Guerre des États-Unis sous la forme d'une plateforme de connaissances : recherche plein texte, traduction automatique à travers le corpus, exploration via carte et chronologie, et un(e) JSON API public(que). Les documents sources sont des œuvres du gouvernement fédéral des États-Unis et appartiennent au domaine public aux États-Unis ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Ce projet **n'est pas affilié au gouvernement des États-Unis**, n'utilise aucun insigne officiel et ne lève jamais les caviardages.

### Architecture

```
Local machine (Apple Silicon, residential IP)        Edge network
─────────────────────────────────────────           ─────────────────────────
pipeline/  (Python 3.10, stdlib-only core)           worker/  (TypeScript, Hono.js)
  fetch → OCR → translate → publish  (forward-only)    /{lang}/...   pages
  OCR: open-source engine (Tesseract CLI fallback)     /api/v1/...   public API
  translate / NER: local LLM (Gemma via Ollama)        /admin        operator console
  state: SQLite manifest                             backed by: edge SQL DB, object
        │                                              storage (source PDFs), KV cache
        └── publishes a bundle: SQL + asset manifest + cache-purge list ──┘
```

- **Coût IA cloud nul par document.** OCR et la traduction s'exécutent localement ; la machine à états à sens unique (`discovered → downloaded → ocr_done → translated → published`) garantit qu'aucun document n'est retraité sauf s'il a été modifié.
- **Le cœur du pipeline ne possède aucune dépendance tierce** — les modules d'analyse / manifeste / delta s'exécutent et sont testés sur un Python vierge sans aucune installation pip ; les étapes OCR/traduction se dégradent progressivement en l'absence de packages optionnels.
- **Le site Edge** applique des en-têtes de sécurité stricts + CSP (pas de `unsafe-inline` ; JSON-LD en ligne avec épinglage sha256), la négociation de langue via `Accept-Language` + un mappage par pays, un cache de page KV de 30 jours et un cron quotidien de maintenance.
- **Mises à jour incrémentielles :** un détecteur de delta compare l'index source et ne réinjecte que les modifications dans le pipeline.

### Pour les développeurs

L' API public à l'adresse https://www.ufolens.com/api/v1 renvoie les documents et les métadonnées au format JSON. L'accès anonyme est limité en débit ; veuillez demander une clé pour les niveaux chercheur/développeur. Consultez la section API du site pour connaître les points de terminaison et les limites.

### État

Code terminé ; site déployé à l'adresse https://www.ufolens.com.. La base de données de production est alimentée en exécutant le pipeline hors ligne et en publiant le bundle (`cli_publish run --remote`). La documentation complète de conception se trouve dans `docs/20260511/`.

### Licence / Limites

- Documents sources : œuvres du gouvernement fédéral des États-Unis, domaine public aux États-Unis.
- Code propre à cette plateforme : voir `LICENSE`.
- Le site envoie `Tdm-Reservation: 1` et `X-Robots-Tag: noai, noimageai` — indexables par les moteurs de recherche, avec une exclusion du scraping et de l'entraînement d'IA.
- Les séquences vidéo sont attribuées à DVIDS / AARO et ne sont pas revendiquées par ce projet.

Les issues et PRs sont les bienvenues. Veuillez lire `CLAUDE.md` et `docs/20260511/00-*` avant de proposer des modifications structurelles.