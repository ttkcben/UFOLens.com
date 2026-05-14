# GitHub — Póstrzédny pòst 2 z 3 · Wezwanié do wspòłrobòtë / "dobré pierszé zadania"

**Ùżëj jakno:** przëpiktą diskùsëją ("Wspòłrobòta & dobré pierszé zadania") abò wprowadzenié do CONTRIBUTING.md.
**Klëczowé słowa:** open source, wspòłrobòta, dobré pierszé zadanié, i18n, lokalizacëjô, OCR, Python, TypeScript, Vitest, pytest, przistãpnosc, UAP, òtemkłé dané
**Hiperłącza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Wspòłrobòta przë ufolens.com

[ufolens.com](https://www.ufolens.com) przemiéaniô [archiwùm PURSUE UAP](https://www.war.gov/ufo) Departamentu Wòjnë USA w przeszëkiwalną, wielojãzyczną platfòrmã z [publicznym API](https://www.ufolens.com/api/v1). To są dwie pòłowë — môlowi rurocąg ingestëji w Pythonie (`pipeline/`) ë krawãdzywô aplikacëjô TypeScript/Hono (`worker/`) — chtërné zbiegają sã w jednym interfejsie: pùblikòwóny paczce SQL + aktywa.

Nie trzëbùjesz żôdnëch chmùrowëch pòswiôdczeniów, żebë sã przyczynic. Rdzenowé mòdułë rurocągu są blós na stdlib, a testë Workera dzejają w pamiãcë.

### Pòstawienié

```bash
# rurocąg
python3 -m pytest pipeline/tests/          # wszëtkò pòwinno bëc na zelono, bez instalacëji z pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Gdze pòmòc je nôbarżi przëdatnô

**i18n / lokalizacëjô** — `worker/src/i18n/ui-strings.json` to zdrój tekstów interfejsu. Recenzjô òd rodzëmegò ùżëtkòwnika kòżdegò jãzyka, co nie je anielsczi, je baro cennô: wëłapac niezgrabné maszinowé dolmaczenia, pòprôwic problemë z RTL/układem, ùlepszëc krancowé przëpôdczi negòcjacëji jãzyka.

**Jakosc OCR** — lepszé przedspracowanié stôrëch maszinopisowëch skanów przed OCR; ramë testòwé pòrównëjące òtemkłi motor z alternatiwą Tesseract na próbkach stron.

**Przistãpnosc** — auditowanié wërenderowónëch stron (`worker/src/render/`) wedle WCAG; CSP je scësłé (bez `unsafe-inline`), wię rozrzeszenia mùszą dzejac w tim ògrańczenim.

**Ergonómia API** — `worker/src/routes/` — paginacëjô, filtrowanié, òpis OpenAPI, przëkłôdowi klientë.

**Odpornosc rurocągu** — wicy grackëch sczéżków degradacëji, lepszé rapòrtowanié pòstãpù, krancowé przëpôdczi wykrywaniô deltë (`pipeline/lib/delta.py`).

**Dokùmentacëjô** — `docs/20260511/` (繁體中文; `00-*` to indeks). Dolmaczenia projektowëch dokùmentów na anielsczi są mile widzóné.

### Zasadë

- Wszëtczi sczéżczi relatiwné — projekt mùszi bëc przenosny midzë maszinama. Żôdnëch zakòdowónëch na stałé absolutnëch sczéżków.
- Nie dodôwôj zaleznoscë pip do *rdzenowégò* mòdułu rurocągu. Fakultatiwné etapë mògą ùżëwac fakultatiwnëch paczétów ë mùszą grackò sã degredowac bez nich.
- Nie osłabiwôj automatu stanów blós do przodku — to je stróp kòsztów.
- Nie dodôwôj òficjalnëch insygniów rządu USA ë nic, co bë òdwrôcało zdrzódłowé redakcëje.
- Zjinaczi w schemë D1 dotikają **dwóch** lopków: `pipeline/lib/manifest_schema.sql` ë `db/schema.sql`.
- Testë z nowim kòdem. Wiadomòscë w kòmitach wedle Conventional Commits.

Przeczytôj `CLAUDE.md` ë `docs/20260511/00-*` pierszi, a pò tim òtemknij problem, żebë przedyskùtowac co strukturalnégò przed PR.

