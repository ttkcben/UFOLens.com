# GitHub — Post 2 z 3 · Zaproszenie do współpracy / "dobre pierwsze zadania"

**Użyj jako:** przypięta dyskusja ("Współpraca i dobre pierwsze zadania") lub wstęp do `CONTRIBUTING.md`.
**Słowa kluczowe:** open source, współpraca, dobre pierwsze zadanie, i18n, lokalizacja, OCR, Python, TypeScript, Vitest, pytest, dostępność, UAP, otwarte dane
**Hiperłącza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Współpraca przi ufolens.com

[ufolens.com](https://www.ufolens.com) zamienia archiwum [PURSUE UAP](https://www.war.gov/ufo) Departamentu Wojny USA w przeszukiwalnõ, wielojęzycznõ platformã z [publicznym API](https://www.ufolens.com/api/v1). Skłodo sie z dwóch połówek — lokalnego pipeline'u do pobierania w Pythonie (`pipeline/`) i aplikacji brzegowej w TypeScript/Hono (`worker/`) — kere spotykajōm sie na jednym interfejsie: opublikowanym pakiecie SQL + zasoby.

Niy potrzebujesz żadnych poświadczeń chmurowych, coby wnieść swój wkład. Główne moduły pipeline'u to ino stdlib, a testy Workera działajōm na pamięci wirtualnej.

### Konfiguracja

```bash
# pipeline
python3 -m pytest pipeline/tests/          # powinno być wszystko na zielono, bez instalacji pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Kaj pomoc jest nojbardzij przidatno

**i18n / lokalizacja** — `worker/src/i18n/ui-strings.json` to źrōdło tekstōw interfejsu. Przeglōnd przez rodzimego użytkownika kożdego jynzyka inkszego niż angelski jest barzo cenny: wyłapać niezręczne maszynowe tłumaczenia, poprawić problemy z RTL/układem, ulepszyć skrajne przypadki negocjacji języka.

**Jakość OCR** — lepsze przetwarzanie wstępne starych, maszynopisanych skanōw przed OCR; uprzęż do oceny porównująca silnik open-source z fallbackiem Tesseract na próbkach stron.

**Dostępność (Accessibility)** — audyt renderowanych stron (`worker/src/render/`) pod kątem WCAG; CSP jest rygorystyczne (bez `unsafe-inline`), więc rozwiązania muszą działać w jego ramach.

**Ergonomia API** — `worker/src/routes/` — paginacja, filtrowanie, opis OpenAPI, przykładowe klienty.

**Solidność pipeline'u (Pipeline robustness)** — więcej ścieżek z gracją degradacji, lepsze raportowanie postępów, skrajne przypadki wykrywania delta (`pipeline/lib/delta.py`).

**Dokumenty** — `docs/20260511/` (繁體中文; `00-*` to indeks). Tłumaczenia dokumentacji projektowej na angelski sōm mile widziane.

### Podstawowe zasady

- Wszystkie ścieżki względne — projekt musi być przenośny między maszynami. Żadnych na stałe zakodowanych ścieżek bezwzględnych.
- Niy dodowej zależności pip do głównego modułu pipeline'u. Opcjonalne etapy mogōm używać opcjonalnych pakietōw i muszōm z gracją degradować bez nich.
- Niy osłabiej maszyny stanu ino do przodku — to jest sufit kosztów.
- Niy wprowodzej oficjalnych insygniów rządu USA i niy dodowej niczego, co odwraco redakcje źrōdłowe.
- Zmiany w schemacie D1 dotyczōm **dwóch** plików: `pipeline/lib/manifest_schema.sql` i `db/schema.sql`.
- Testy z nowym kodym. Wiadomości commitów w stylu Conventional Commits.

Przeczytej najpierw `CLAUDE.md` i `docs/20260511/00-*`, a potym otwōrz zgłoszenie, coby omówić coś strukturalnego przed PR.
