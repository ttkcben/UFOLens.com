# GitHub — Nsɛm a ɛtɔ so 2 wɔ 3 mu · Nsɛm a wɔde frɛ nkurɔfo ma wɔboa / "nsɛm pa a edi kan"

**Fa di dwuma sɛ:** Nsɛm a wɔapini ("Contributing & good first issues") anaa CONTRIBUTING.md ntshayɛ.
**Atitiriw a wɔde di dwuma:** open source, boa, asɛm pa a edi kan, i18n, mpɔtam hɔ nsakrae, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, data a ɛda adi
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mmoa a wɔde ma wɔ ufolens.com ho

[ufolens.com](https://www.ufolens.com) dan U.S. War Department no [PURSUE UAP archive](https://www.war.gov/ufo) no ma ɛyɛ beae a wotumi hwehwɛ ade, kasa pii, na ɛwɔ [public API](https://www.ufolens.com/api/v1). Ɛyɛ fã abien — Python ingest pipeline a ɛwɔ mpɔtam hɔ (`pipeline/`) ne TypeScript/Hono edge app (`worker/`) — a ɛhyia wɔ interface biako so: SQL + agyapade bundle a wɔatintim.

Wonhia cloud adanse krataa biara na ama woatumi aboa. Pipeline no modules atitiriw no yɛ stdlib-only na Worker sɔhwɛ ahorow no di dwuma tia in-memory storage.

### Nsiesiei

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Baabi a mmoa ho hia paa

**i18n / localization** — `worker/src/i18n/ui-strings.json` ne UI strings no fibea. Kasa foforo biara a ɛnyɛ Brɔfo kasafo a ɔte ase no a ɔbɛhwɛ mu no bo yɛ den: hu mfiridwuma mu nneɛma a ɛyɛ nwonwa, siesie RTL/layout ho nsɛm, ma kasa mu apereperedi ho nsɛm a ɛyɛ hu no tu mpɔn.

**OCR su pa** — nkrataa dedaw a wɔde afiri akyerɛw a wɔde ayɛ nhwehwɛmu ansa na wɔreyɛ OCR no a wɔbɛsiesie no yiye; nhwehwɛmu a wɔde toto open-source engine no ne Tesseract fallback a ɛwɔ nkratafa a wɔayɛ ho nhwɛso no mu.

**Accessibility** — hwɛ nkratafa a wɔayɛ (`worker/src/render/`) no mu tia WCAG; CSP no mu yɛ den (no `unsafe-inline`), enti ɛsɛ sɛ ano aduru ahorow no di dwuma wɔ saa adwuma no mu.

**API ergonomics** — `worker/src/routes/` — nkratafa a wɔde kyekyɛ nneɛma mu, nsɔe, OpenAPI nkyerɛkyerɛmu, nhwɛsofo a wɔyɛ adwuma.

**Pipeline a ɛyɛ den** — akwan pii a ɛma obi brɛ ase yiye, amanneɛbɔ a eye a ɛfa nkɔso ho, nsɛm a ɛfa nsonsonoe a wohu ho (`pipeline/lib/delta.py`).

**Docs** — `docs/20260511/` (繁體中文; `00-*` ne index no). Wɔma kwan ma wɔkyerɛ nsiesiei ho nkrataa no ase kɔ Engiresi kasa mu.

### Mmara a ɛwɔ fam

- Akwan nyinaa fa biribi ho — ɛsɛ sɛ adwuma no tumi fa mfiri ahorow so. Akwan a edi mũ a wɔde ahyɛ mu denneennen nni hɔ.
- Mfa pip a egyina so no nka pipeline *core* module no ho. Gyinapɛn ahorow a wopɛ no betumi de packages a wopɛ no adi dwuma, na ɛsɛ sɛ ɛbrɛ ase yiye a enni mu.
- Mma forward-only state machine no nyɛ mmerɛw — ɛno ne sika a wɔsɛe no ano.
- Mfa U.S. aban agyiraehyɛde biara nka ho, na mfa biribiara a ɛbɛma wɔasan akyerɛw nsɛm a wɔde akyerɛw no akyi no nka ho.
- D1 adansi mu nsakrae no ka fael **abien** ho: `pipeline/lib/manifest_schema.sql` ne `db/schema.sql`.
- Sɔhwɛ ahorow a wɔde code foforo ayɛ. Nsɛm a wɔde kyerɛw nsɛm a wɔtaa de di dwuma.

Kenkan `CLAUDE.md` ne `docs/20260511/00-*` kan, afei bue asɛm bi na wo ne wo ho nni nkɔmmɔ wɔ biribiara a ɛfa adansi ho ansa na PR no reba.

