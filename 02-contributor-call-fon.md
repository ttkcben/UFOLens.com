# GitHub — Wema 2gɔ́ tɔn dó 3 mɛ · Nùɖitɔ́ ylɔ́ylɔ́ / "nǔgbó yɔ́yɔ́ nukɔntɔn lɛ́ɛ"

**Nǔ zánzán:** goxɔ ɖéé ɖɔ è slɔ́ ("Alɔdido & nǔgbó yɔ́yɔ́ nukɔntɔn lɛ́ɛ") alǒ CONTRIBUTING.md bǐbɛ̌mɛ.
**Nùnywɛ́xó kléwúnkléwún lɛ́ɛ:** open source, alɔdido, nǔgbó yɔ́yɔ́ nukɔntɔn, i18n, nǔsɔ́ɖònǔmɛ, OCR, Python, TypeScript, Vitest, pytest, nǔkpɔ́n tɔn, UAP, nùkúnnúmɔ jɛ̌gbɔ́jí tɔn
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Alɔdido nú ufolens.com

[ufolens.com](https://www.ufolens.com) nɔ́ huzu U.S. axɔ́súɖuto tɔn [PURSUE UAP gbɛdido](https://www.war.gov/ufo) dó gbɛdohɛn ayihun-jlɛ̌-kpɔn tɔn, gbèdagbe susu tɔn ɖéé kpódó [API gbɔ́ngbɔ́n tɔn](https://www.ufolens.com/api/v1). Adà we wɛ — Python ingest pipeline kɔ́mputá tɔn (`pipeline/`) kpódó TypeScript/Hono edge app (`worker/`) — nɔ́ kplé ɖò nǔɖe ɖokpó jí: SQL + assets bundle ɖéé è sɔ́ ɖ'ayǐ.

Hwiɖesunɔ má byɔ́ nǔɖe sín nǔɖe ɖěɖě ó nú hwiɖesunɔ ná d’alɔ. Pipeline ɔ tɔn core modules lɛ́ɛ nyí stdlib-only, bɔ Worker kpɔ́n susu lɛ́ɛ nɔ́ zɔn ɖò in-memory agbasa mɛ.

### Nǔɖoɖo tɔn

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Fí e alɔdido ɖò nǔɖe mɛ hugǎn ɔ

**i18n / nǔsɔ́ɖònǔmɛ** — `worker/src/i18n/ui-strings.json` wɛ nyí UI wěma lɛ́ɛ sín asú. Gbèɖótɔ́ nùɖe tɔn e má nyí English gbè ǎ ɔ sín nǔkpɔ́n ɖěbǔ ɖò nǔɖe mɛ tawun: mɔ mɛ́kini tɔn e má sɔgbe ǎ ɔ, ɖɔɖó RTL/layout nǔgbó lɛ́ɛ, kpódó nǔgbó yɔ́yɔ́ gbèdagbe susu tɔn lɛ́ɛ.

**OCR jlɛ̌jlɛ̌ tɔn** — typewritten scans xóxó lɛ́ɛ sín nǔɖoɖo nukɔn tɔn có è wá OCR; nǔkpɔ́n tɔn e nɔ́ kpɔ́n open-source mɛ́kini ɔ kpódó Tesseract fallback ɔ kpódó ɖò wémata kpɔ́ndéwú lɛ́ɛ jí.

**Nǔkpɔ́n tɔn** — kpɔ́n wémata e è sɔ́ ɖ'ayǐ lɛ́ɛ (`worker/src/render/`) kpódó WCAG kpódó; CSP ɔ syɛn tawun (é má nyí `unsafe-inline` ó), enɛ́ ɔ, nǔɖe lɛ́ɛ ɖó na wà azɔ̌ ɖò mɛ.

**API ergonomics** — `worker/src/routes/` — wémata bǐbɛ̌má, nǔsɔ́ɖònǔmɛ, OpenAPI nǔɖe, kpódó nǔɖe kpɔ́ndéwú lɛ́ɛ.

**Pipeline syɛnsyɛn tɔn** — nǔgbó yɔ́yɔ́ graceful-degradation tɔn lɛ́ɛ, nǔgbó yɔ́yɔ́ nǔgbó tɔn, delta-detection nǔgbó yɔ́yɔ́ lɛ́ɛ (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` wɛ nyí index ɔ). Nǔgbógbó wěma lɛ́ɛ sín nǔgbɔ́jelɔ́ dó English gbè mɛ wɛ nyí nùɖé.

### Sɛ́n e ɖò ayǐ lɛ́ɛ

- Nǔbǐ ɖò alɔkpa ɔ mɛ — azɔ̌ ɔ ɖó na sixú zɔn gbɔn mɛ́kini lɛ́ɛ bǐ mɛ. Ali e ɖò fínɛ́ ɔ má ɖò fínɛ́ ó.
- Má sɔ́ nǔɖe dó pipeline *core* module mɛ ó. Adà e sixú nyí nǔɖe lɛ́ɛ sixú zán nǔɖe lɛ́ɛ, bó ɖó na gbɔjɛ́ kpódó nǔɖe lɛ́ɛ ɖò fínɛ́ hwenu e ye má ɖò fínɛ́ ǎ ɔ.
- Má sɔ́ forward-only state machine ɔ sínsɛ́n gbeɖé ó — enɛ́ ɔ wɛ nyí así tɔn.
- Má sɔ́ nǔɖe sín nǔɖe sín U.S. axɔ́súɖuto tɔn ɖěbǔ dó mɛ ó, má ka sɔ́ nǔɖe e nɔ́ lɔ́ asú redactions lɛ́ɛ gúdo ɔ ɖěbǔ dó mɛ ó.
- D1 schema hǔzúhúzú lɛ́ɛ nɔ́ jɛ wěma **we** wu: `pipeline/lib/manifest_schema.sql` kpódó `db/schema.sql`.
- Kpɔ́n susu kpódó kɔ́dù yɔ́yɔ́. Wěma e è nɔ́ sɛ́dó lɛ́ɛ.

Xà `CLAUDE.md` kpódó `docs/20260511/00-*` nukɔn, enɛ́ ɔ gúdo ɔ, hun nǔgbó ɖéé nú è nǎ sixú ɖɔ xó dó nǔɖe e ɖò gbɛdido mɛ ɔ jí có PR ɔ.

