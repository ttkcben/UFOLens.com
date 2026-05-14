# GitHub — Nkrataa 2 a ɛwɔ 3 mu · Nsɛm a wɔde frɛ nkurɔfo ma wɔboa / "nsɛm a ɛyɛ mfiase pa"

**Fa di dwuma sɛ:** Nkitahodi a wɔde ahyɛ hɔ ("Contributing & good first issues") anaasɛ CONTRIBUTING.md mfiase.
**Nneɛma a ɛho hia:** open source, mmoa, nsɛm a ɛyɛ mfiase pa, i18n, mpɔtam hɔ nkyerɛase, OCR, Python, TypeScript, Vitest, pytest, akwankyerɛ, UAP, data a ɛda adi
**Hyperlinks:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Mmoa a wɔde bɛma ufolens.com

[ufolens.com](https://www.ufolens.com) dan U.S. Akode Tuo Dwumadibea no [PURSUE UAP kyerɛwtohɔ](https://www.war.gov/ufo) no ma ɛyɛ beae a wɔhwehwɛ mu, kasa ahodoɔ pii a [API a ɛda adi](https://www.ufolens.com/api/v1) ka ho. Ɛyɛ afã abien — Python ingest pipeline a ɛwɔ mpɔtam hɔ (`pipeline/`) ne TypeScript/Hono edge app (`worker/`) — a ɛhyia wɔ beaeɛ baako: SQL + assets bundle a wɔatintim.

Wunhia cloud credentials biara na woatumi de wo ho ahyɛ mu. Pipeline no core modules no yɛ stdlib-only na Worker sɔhwɛ no yɛ adwuma tia in-memory storage.

### Nhyehyɛe

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Baabi a mmoa hia paa

**i18n / localization** — `worker/src/i18n/ui-strings.json` ne UI nsɛm no fibea. Kasa biara a ɛnyɛ Borɔfo no, sɛ ɔkasafoɔ ankasa hwɛ mu a, ɛsom bo paa: hu mfiridwuma mu nkyerɛase a ɛnteɛ, siesie RTL/nhyehyɛe ho nsɛm, na ma kasa mu nkitahodi a ɛyɛ den no tu mpɔn.

**OCR yiyedi** — nkrata a wɔde ahyɛ mu dedaadie no a wɔyɛ ho adwuma yiye ansa na OCR no; sɔhwɛ a wɔde toto open-source engine no ne Tesseract fallback no wɔ nkrata bi so.

**Akwankyerɛ** — hwɛ nkrata a wɔayɛ no (`worker/src/render/`) no tia WCAG; CSP no yɛ den (nni `unsafe-inline`), enti ɛsɛ sɛ ano aduru no yɛ adwuma wɔ saa anohyetoɔ no mu.

**API a ɛyɛ mmerɛ** — `worker/src/routes/` — nkrata, nsɛm a wɔhwehwɛ mu, OpenAPI nkyerɛkyerɛmu, sɔhwɛfoɔ.

**Pipeline a ɛyɛ den** — akwan a ɛbrɛ ase yiye, amanneɛbɔ a ɛkɔ anim, delta-detection nsɛm a ɛyɛ den (`pipeline/lib/delta.py`).

**Nkrata** — `docs/20260511/` (繁體中文; `00-*` ne index no). Yɛsrɛ wo, sɛ wotumi kyerɛ design docs no ase kɔ Borɔfo mu a, yɛbɛgye atom.

### Mmara a ɛwɔ hɔ

- Akwan nyinaa fa abusua mu — ɛsɛ sɛ adwuma no tumi fa mfidie ahodoɔ so. Akwan a wɔde ahyɛ hɔ a ɛyɛ den nni hɔ.
- Mfa pip dependency nka pipeline *core* module ho. Gyinapɛn a wopaw no tumi de packages a wopaw no di dwuma, na ɛsɛ sɛ ɛbrɛ ase yiye a enni hɔ.
- Mma forward-only state machine no nyɛ mmerɛ — ɛno ne sika a wɔbɔ wo no ano.
- Mfa U.S. aban agyiraehyɛde biara nka ho, na mfa biribiara a ɛbɛma wɔahu nkrata a wɔde ahintaw no nka ho.
- D1 schema nsesaeɛ ka fael **abien**: `pipeline/lib/manifest_schema.sql` ne `db/schema.sql`.
- Sɔhwɛ a wɔde code foforɔ ayɛ. Conventional-commit nsɛm.

Kenkan `CLAUde.md` ne `docs/20260511/00-*` ansa na woabue asɛm biara a ɛfa nhyehyɛeɛ ho ansa na PR no aba.

