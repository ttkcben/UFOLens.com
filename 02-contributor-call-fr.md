# GitHub — Publication 2 sur 3 · Appel à contribution / « Bonnes premières issues »

**Utilisation :** Une discussion épinglée (« Contribuer & bonnes premières issues ») ou une introduction à CONTRIBUTING.md.
**Mots-clés :** open source, contribution, bonne première issue, i18n, localisation, OCR, Python, TypeScript, Vitest, pytest, accessibilité, UAP, données ouvertes
**Hyperliens :** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuer à ufolens.com

[ufolens.com](https://www.ufolens.com) transforme les [archives UAP PURSUE](https://www.war.gov/ufo) du Département de la Guerre des États-Unis en une plateforme multilingue et consultable dotée d'une [API publique](https://www.ufolens.com/api/v1). Le projet se compose de deux moitiés — un pipeline d'ingestion local en Python (`pipeline/`) et une application edge en TypeScript/Hono (`worker/`) — qui se rejoignent sur une seule interface : un paquet (bundle) SQL + assets publié.

Vous n'avez besoin d'aucune information d'identification cloud pour contribuer. Les modules principaux du pipeline n'utilisent que la bibliothèque standard (stdlib-only) et les tests du Worker s'exécutent sur un stockage en mémoire.

### Installation

```bash
# pipeline
python3 -m pytest pipeline/tests/          # tous les tests devraient passer, aucune installation pip n'est nécessaire

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Où votre aide est la plus utile

**i18n / localisation** — `worker/src/i18n/ui-strings.json` est la source des chaînes de l'interface utilisateur. La relecture par un locuteur natif de toute langue autre que l'anglais est très précieuse : détecter les traductions automatiques maladroites, corriger les problèmes de RTL/mise en page, améliorer les cas limites de négociation de langue.

**Qualité de l'OCR** — un meilleur pré-traitement des anciens documents dactylographiés avant l'OCR ; un harnais d'évaluation comparant le moteur open-source à la solution de repli Tesseract sur des pages échantillons.

**Accessibilité** — auditer les pages rendues (`worker/src/render/`) par rapport aux normes WCAG ; la CSP est stricte (pas de `unsafe-inline`), donc les solutions doivent fonctionner dans ce cadre.

**Ergonomie de l'API** — `worker/src/routes/` — pagination, filtrage, description OpenAPI, exemples de clients.

**Robustesse du pipeline** — plus de chemins de dégradation progressive, meilleur rapport d'avancement, cas limites de détection de delta (`pipeline/lib/delta.py`).

**Documentation** — `docs/20260511/` (en 繁體中文 ; `00-*` est l'index). Les traductions de la documentation de conception en anglais sont les bienvenues.

### Règles de base

- Tous les chemins d'accès doivent être relatifs — le projet doit être portable d'une machine à l'autre. Pas de chemins absolus codés en dur.
- N'ajoutez pas de dépendance pip à un module *central* du pipeline. Les étapes optionnelles peuvent utiliser des paquets optionnels, et doivent se dégrader progressivement en leur absence.
- N'affaiblissez pas la machine à états à progression unique — c'est ce qui plafonne les coûts.
- N'introduisez pas d'insignes officiels du gouvernement américain, et n'ajoutez rien qui annule les passages censurés de la source.
- Les modifications du schéma D1 touchent **deux** fichiers : `pipeline/lib/manifest_schema.sql` et `db/schema.sql`.
- Des tests avec le nouveau code. Des messages de commit conventionnels.

Lisez d'abord `CLAUDE.md` et `docs/20260511/00-*`, puis ouvrez une issue pour discuter de tout changement structurel avant la PR.

