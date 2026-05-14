# GitHub — Post 2 de 3 · Chamada a contributors / "buenos primers issues"

**Uso:** como una Discusión fixada ("Contribuindo y buenos primers issues") u una introducción a CONTRIBUTING.md.
**Parolas clau:** codigo ubierto, contribuir, buen primer issue, i18n, localización, OCR, Python, TypeScript, Vitest, pytest, accesibilidat, UAP, datos ubiertos
**Vinclos:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Contribuindo a ufolens.com

[ufolens.com](https://www.ufolens.com) convierte l'[archivo PURSUE UAP](https://www.war.gov/ufo) d'o Departamento de Guerra d'os EE.UU. en una plataforma con capacidat de busca y multilingüe con una [API publica](https://www.ufolens.com/api/v1). Son dos mitaz —un pipeline d'inchesta local en Python (`pipeline/`) y una aplicación edge en TypeScript/Hono (`worker/`)— que s'achuntan en una sola interfaz: un paquet publicato de SQL + activos.

No necesita garra credencial d'a nube ta contribuir. Os modulos centrals d'o pipeline son nomás con a libreria estandar y as prebas d'o Worker s'executan contra un almagazenamiento en memoria.

### Configuración

```bash
# pipeline
python3 -m pytest pipeline/tests/          # habría de salir todo verde, no cal instalar pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### An l'aduya ye más útil

**i18n / localización** — `worker/src/i18n/ui-strings.json` ye a fuent d'as cadenas de texto d'a UI. A revisión por fablants nativos de qualsiquier local que no sía anglés ye de gran valor: pillar resultaus d'a maquina que suenen raros, apanyar problemas de RTL/disenyo, y millorar casos extremos en a negociación d'idioma.

**Calidat de l'OCR** — millor preprocesamiento d'escaneos antigos feitos a maquina antes de l'OCR; un arnés d'avaluación que compare o motor de codigo ubierto contra o fallback de Tesseract en pachinas de muestra.

**Accesibilidat** — auditar as pachinas renderizadas (`worker/src/render/`) contra WCAG; o CSP ye estricto (sin `unsafe-inline`), asinas que as solucions han de funcionar dentro d'ixo.

**Ergonomía de l'API** — `worker/src/routes/` — pachinación, filtrado, descripción OpenAPI, eixemplos de clients.

**Robustez d'o pipeline** — más rotas de degradación elegant, millors informes de progreso, casos extremos en a detección de deltas (`pipeline/lib/delta.py`).

**Documentación** — `docs/20260511/` (繁體中文; `00-*` ye l'endice). As traduccions d'os documentos de disenyo a l'anglés son bienplegadas.

### Reglas basicas

- Totas as rotas relativas — o prochecto ha de poder-se portiar entre maquinas. Denguna rota absoluta codificada a fuego.
- No adhibir dependencias de pip a un modulo *central* d'o pipeline. As fases opcionals pueden usar paquetz opcionals, y han de degradar-se con elegancia sin éls.
- No debilitar a maquina d'estaus de nomás entabant — ixe ye o teito de coste.
- No introducir insignias oficials d'o gubierno d'os EE.UU., y no adhibir cosa que revierta as redaccions d'a fuent.
- Os cambios en o esquema de D1 afectan a **dos** fichers: `pipeline/lib/manifest_schema.sql` y `db/schema.sql`.
- Prebas con o nuevo codigo. Mensaches de Conventional Commits.

Lea `CLAUDE.md` y `docs/20260511/00-*` primero, y dimpués ubra un issue ta discutir qualsiquier cosa estructural antes d'o PR.

