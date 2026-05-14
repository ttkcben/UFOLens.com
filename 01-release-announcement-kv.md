# GitHub — 1-а гижӧд 3-ысь · Релиз / README announcement block

**Кыдзи вӧдитчыны:** GitHub Release-лэн тэчасон, доска вылын лӧсьӧдӧм дисуксияӧн либӧ репозиторийлӧн README-файллӧн помӧн.
**Ключовӧй кывъяс:** UAP, UFO, PURSUE archive, declassified documents, open data, full-text search, OCR, machine translation, local LLM, Ollama, edge computing, public API, Hono, TypeScript, Python
**Гиперссылкаяс:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo · https://www.copyright.gov/title17/92chap1.html#105

---

## ufolens.com — PURSUE UAP архивлы уна кывъя, корсяна платформа

**Улын:** https://www.ufolens.com · **API:** https://www.ufolens.com/api/v1 · **Исходнӧй архив:** https://www.war.gov/ufo

`ufolens.com` выльысь публикуйтӧ АӦШ-лэн Война Департаментлӧн **PURSUE** UAP / UFO гижӧдъяслӧн архивсӧ тӧдӧмлун платформаӧн: став текстӧдыс корсьӧм, став корпусӧд машинаясӧн вуджӧдӧм, карта + кадлӧн лентаясӧн туялӧм да устӧг JSON API. Исходнӧй документъяс — АӦШ-лэн федеральнӧй правительстволӧн уджъяс да АӦШ-ын найӧ абуӧсь авторскӧй право улын ([17 U.S.C. §105](https://www.copyright.gov/title17/92chap1.html#105)). Тайӧ проектнас **АӦШ-лэн правительствоыскӧд нинӧм йитӧд абу**, сійӧ оз вӧдитчы ни официальнӧй инсигниянас, ни некыдзи оз бердӧд вылӧ лэдзӧмторъяс.

### Архитектура

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

- **Нинӧм облачнӧй-AI дон оз лок документяс.** OCR да вуджӧдӧм мунӧны пытшкын; `discovered → downloaded → ocr_done → translated → published` вылӧ гӧгӧрвоана состояньӧ машинаяс гарантируйтӧны, мый ни ӧти документ оз ло выльысь обработайтӧм, если сійӧ абу вежсьӧма.
- **Трубопроводлӧн ядроыслӧн абуӧсь нёльӧд мортлӧн зависимосьяс** — парсинг / манифест / дельта модульяс уджалӧны да тестятся сӧстӧм Python вылын, кытчӧ нинӧм оз ло pip-ӧн инсталлируйтӧма; OCR/вуджӧдӧм этапъяс лӧсьӧдчӧны ыркыдпырысь, кор оз лоны опциональнӧй пакетъяс.
- **Edge сайт** вӧдитчӧ строгӧй безопасносьтлӧн заголовокъясӧн + CSP-ӧн (абу `unsafe-inline`; инлайн JSON-LD sha256-ӧн пинӧдӧма), кывйӧн гӧгӧрвоӧм `Accept-Language` + канмуяслӧн маппингӧдз, 30-лунъя KV страница кэшӧн да лун выв cron-ӧн.
- **Содтасян вежсьӧмъяс:** дельта детектор диффуйтӧ исходнӧй индекссӧ да сетӧ сӧмын вежсьӧмъяссӧ бӧр трубопроводӧ.

### Разработчикъяслы

Устӧг API https://www.ufolens.com/api/v1 вылын сетӧ документъяс да метаданнӧйяс JSON-ӧн. Анонимнӧй доступлӧн ӧдӧн-ӧдӧн лимит; корӧй ключ туялысь/разработчик ярусъяслы. Эндпойнтъяс да лимитъяс йылысь видзӧдӧй сайтлӧн API юкӧнысь.

### Статус

Код быдса; сайт лӧсьӧдӧма https://www.ufolens.com вылын. Производственнӧй база данныхсӧ тыртӧны офлайн трубопроводсӧ уджӧдӧмӧн да пакетсӧ водзӧ публикуйтӧмӧн (`cli_publish run --remote`). Быдса дизайн-документъяс олӧны `docs/20260511/` папкаын.

### Лицензия / границкаяс

- Исходнӧй документъяс: АӦШ-лэн федеральнӧй правительстволӧн уджъяс, АӦШ-ын устӧг.
- Тайӧ платформалӧн аслас код: видзӧдӧй `LICENSE`.
- Сайт ыстӧ `Tdm-Reservation: 1` да `X-Robots-Tag: noai, noimageai` — корсян системаясӧн индексируйтӧма, AI-вежӧдӧм/скрапингысь отказайтчӧма.
- Видеоматериалъяс DVIDS / AARO-лы кывъямаӧсь да тайӧ проектӧн оз лоны признайтӧмаӧсь.

Вочаӧсь юалӧмъяс да PR-яс. Медводз лыддьӧй `CLAUDE.md` да `docs/20260511/00-*`, медводз структурнӧй вежсьӧмъяс йылысь юалӧмъяссӧ восьтӧмӧн.

