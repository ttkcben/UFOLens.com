# GitHub — Bäitrag 3 vun 3 · Architektur-Notizen (ADR-Stil Diskussioun)

**Benotzen als:** eng Diskussioun ënner "Show and tell" / "Architektur", oder als `docs/` ADR Ufank.
**Schlësselwierder:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Firwat ufolens.com esou gebaut ass, wéi et ass

Notizen zu den dräi Entscheedungen, déi [ufolens.com](https://www.ufolens.com) (déi duerchsichbar, méisproocheg Neioplag vum [PURSUE UAP-Archiv](https://www.war.gov/ufo)) geprägt hunn. Kommentarer / Géigepropositioune si wëllkomm.

### 1. D'Pipeline ass eng Forward-only State Machine — mat Absicht

Zoustänn: `discovered → downloaded → ocr_done → translated → published`. En Dokument beweegt sech nëmme virun, an nëmmen dann, wann et Aarbecht ze maachen ass. Verëffentlechten Inhalt gëtt ni nei veraarbecht, ausser en Delta-Detekter gesäit, datt d'Quell tatsächlech geännert huet.

**Grond:** OCR + Iwwersetzung sinn déi deier Operatiounen, an d'Archiv wiisst mat der Zäit. Eng Pipeline, déi "alles nei ausféiert, fir sécher ze sinn" huet onbegrenzte Käschten. Andeems een Iwwergäng no hannen onméiglech mécht, gëtt eng explodéierend Rechnung onméigleg. D'Käschte-Plafong ass eng Eegeschaft vum Zoustands-Graph, net vun der Opmierksamkeet vum Bedreiwer.

**Käschten:** Schema-Migratiounen an absichtlecht Neiveraarbechte si bewosst komplizéiert. En akzeptabelen Kompromëss.

### 2. OCR an Iwwersetzung lafen op enger lokaler LLM, net op enger Cloud-API

OCR: Open-Source-Engine, Tesseract CLI Fallback. Iwwersetzung + NER: Gemma via Ollama, op engem Apple Silicon Laptop.

**Grond:** null marginal Käschte pro Dokument; reproduzéierbar (fixe Modell + Prompts); an de Fetch-Schrëtt muss souwisou vun enger Privat-IP aus lafen (d'Quell ass hannert dem Akamai Bot Manager — `curl` kritt e 403), also ass souwisou e Laptop am Spill.

**Käschten:** d'Iwwersetzungsqualitéit ass ënner där vun engem Spëtzemodell. Fir e Referenzcorpus, wou dat englescht Original ëmmer ee Klick ewech ass, ass dat an der Rei. Mir behaapten net, datt d'Iwwersetzunge autoritativ sinn.

### 3. Déi zwou Halschenten deelen genee eng Interface: e publizéierte Bündel

D'Pipeline schreift ni direkt an d'Produktiouns-Datebank. Si gëtt `{ SQL, Asset-Manifest, Cache-Purge-Lëscht }` aus. "Verëffentlechen" = dee Bündel no vir applizéieren (SQL an d'Edge-SQL-DB pushen, Assets mam Object Storage synchroniséieren, déi genannte Cache-Schlëssele läschen).

**Grond:** déi lokal Säit an d'Edge-Säit kënne sech onofhängeg entwéckelen; de Bündel kann iwwerpréift ginn; an "Daten deployen" huet all Kéier déi selwecht Form. De Worker ass eng kleng TypeScript/Hono-App — streng CSP (keng `unsafe-inline`; inline JSON-LD ass mat sha256 ugepinn), `Accept-Language` + Land→Sprooch-Verhandlung, 30-Deeg KV Säite-Cache, deeglechen Housekeeping-Cron — an e muss ni wëssen, wéi d'Date gemaach goufen.

**Käschten:** eng D1 Schema-Ännerung betrëfft zwee Fichieren (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Eng bëlleg Versécherung.

### Net-verhandelbar Prinzipien, déi am Verhalen agebaut sinn

- Net mat der US-Regierung affiliéiert; keng offiziell Insignien.
- D'Redaktioune vun der Quell bleiwe behalen, gi ni réckgängeg gemaach.
- Videoen ginn DVIDS / AARO zougeschriwwen.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` op der ganzer Websäit — indexéierbar fir Sichmaschinnen, ofgemellt vum AI-Scraping.

Live: https://www.ufolens.com · API: https://www.ufolens.com/api/v1
