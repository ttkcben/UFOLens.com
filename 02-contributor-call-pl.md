# GitHub — Post 2 z 3 · Wezwanie do współpracy / "dobre pierwsze zadania"

**Zastosowanie:** jako przypięta dyskusja ("Współpraca i dobre pierwsze zadania") lub wprowadzenie do `CONTRIBUTING.md`.
**Słowa kluczowe:** open source, wkład, dobre pierwsze zadanie, i18n, lokalizacja, OCR, Python, TypeScript, Vitest, pytest, dostępność, UAP, otwarte dane
**Hiperłącza:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Współtworzenie ufolens.com

[ufolens.com](https://www.ufolens.com) przekształca [archiwum PURSUE UAP](https://www.war.gov/ufo) Departamentu Wojny USA w przeszukiwalną, wielojęzyczną platformę z [publicznym API](https://www.ufolens.com/api/v1). Składa się z dwóch części — lokalnego potoku ingestującego w Pythonie (`pipeline/`) i aplikacji brzegowej w TypeScript/Hono (`worker/`) — które spotykają się w jednym interfejsie: opublikowanym pakiecie SQL + zasoby.

Nie potrzebujesz żadnych poświadczeń chmurowych, aby wnosić wkład. Główne moduły potoku opierają się wyłącznie na bibliotece standardowej, a testy Workera działają na pamięci ulotnej.

### Konfiguracja

```bash
# potok (pipeline)
python3 -m pytest pipeline/tests/          # wszystkie testy powinny przejść, bez potrzeby instalacji przez pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Gdzie pomoc jest najbardziej przydatna

**i18n / lokalizacja** — `worker/src/i18n/ui-strings.json` jest źródłem ciągów tekstowych interfejsu użytkownika. Przegląd każdego języka innego niż angielski przez native speakera jest niezwykle cenny: wyłapywanie niezręcznych wyników tłumaczenia maszynowego, poprawianie problemów z RTL/układem, ulepszanie skrajnych przypadków negocjacji języka.

**Jakość OCR** — lepsze wstępne przetwarzanie starych, maszynopisanych skanów przed OCR; uprząż do oceny porównująca silnik open-source z awaryjnym Tesseract na przykładowych stronach.

**Dostępność** — audyt wyrenderowanych stron (`worker/src/render/`) pod kątem WCAG; CSP jest rygorystyczne (bez `unsafe-inline`), więc rozwiązania muszą działać w tych ramach.

**Ergonomia API** — `worker/src/routes/` — paginacja, filtrowanie, opis OpenAPI, przykładowi klienci.

**Solidność potoku** — więcej ścieżek łagodnego degradowania, lepsze raportowanie postępów, skrajne przypadki detekcji różnic (`pipeline/lib/delta.py`).

**Dokumentacja** — `docs/20260511/` (繁體中文; `00-*` to indeks). Tłumaczenia dokumentacji projektowej na język angielski są mile widziane.

### Podstawowe zasady

- Wszystkie ścieżki względne — projekt musi być przenośny między maszynami. Żadnych na sztywno zakodowanych ścieżek bezwzględnych.
- Nie dodawaj zależności pip do *głównego* modułu potoku. Opcjonalne etapy mogą używać opcjonalnych pakietów i muszą łagodnie degradować bez nich.
- Nie osłabiaj maszyny stanów działającej tylko w przód — to jest pułap kosztów.
- Nie wprowadzaj oficjalnych insygniów rządu USA i nie dodawaj niczego, co odwraca redakcje źródłowe.
- Zmiany w schemacie D1 dotyczą **dwóch** plików: `pipeline/lib/manifest_schema.sql` i `db/schema.sql`.
- Testy z nowym kodem. Komunikaty commitów zgodne z Conventional Commits.

Przeczytaj najpierw `CLAUDE.md` i `docs/20260511/00-*`, a następnie otwórz zgłoszenie, aby omówić wszelkie zmiany strukturalne przed utworzeniem PR.

