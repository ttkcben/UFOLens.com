# GitHub — 3개 중 2번째 게시물 · 기여자 모집 / "좋은 첫 이슈"

**사용처:** 고정된 토론("기여 및 좋은 첫 이슈") 또는 CONTRIBUTING.md 소개.
**키워드:** open source, contributing, good first issue, i18n, localization, OCR, Python, TypeScript, Vitest, pytest, accessibility, UAP, open data
**하이퍼링크:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com에 기여하기

[ufolens.com](https://www.ufolens.com)은 미국 국방부의 [PURSUE UAP 아카이브](https://www.war.gov/ufo)를 검색 가능하고 다국어를 지원하는 플랫폼으로 변환하며 [공개 API](https://www.ufolens.com/api/v1)를 제공합니다. 이 프로젝트는 로컬 Python 수집 파이프라인(`pipeline/`)과 TypeScript/Hono 엣지 앱(`worker/`)이라는 두 부분으로 구성되며, 발행된 SQL + 애셋 번들 인터페이스에서 만납니다.

기여하는 데 클라우드 자격 증명이 필요하지 않습니다. 파이프라인의 핵심 모듈은 표준 라이브러리만 사용하며, Worker 테스트는 인메모리 스토리지를 대상으로 실행됩니다.

### 설정

```bash
# pipeline
python3 -m pytest pipeline/tests/          # should be all green, no pip install needed

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### 도움이 필요한 주요 분야

**i18n / 현지화** — `worker/src/i18n/ui-strings.json`은 UI 문자열의 소스입니다. 영어가 아닌 로케일에 대한 원어민 검토는 매우 중요합니다. 어색한 기계 번역 결과물을 찾아내고, RTL/레이아웃 문제를 수정하며, 언어 협상 엣지 케이스를 개선할 수 있습니다.

**OCR 품질** — OCR 전 오래된 타자 스캔본의 전처리 개선, 샘플 페이지에서 오픈소스 엔진과 Tesseract 폴백을 비교하는 평가 하네스.

**접근성** — 렌더링된 페이지(`worker/src/render/`)를 WCAG에 맞춰 감사합니다. CSP가 엄격하므로(`unsafe-inline` 없음) 해결책은 그 안에서 작동해야 합니다.

**API 인체공학** — `worker/src/routes/` — 페이지네이션, 필터링, OpenAPI 설명, 예제 클라이언트.

**파이프라인 견고성** — 더 많은 정상 성능 저하 경로, 더 나은 진행 상황 보고, 델타 감지 엣지 케이스(`pipeline/lib/delta.py`).

**문서** — `docs/20260511/` (繁體中文; `00-*`가 인덱스임). 설계 문서를 영어로 번역하는 것을 환영합니다.

### 기본 규칙

- 모든 경로는 상대 경로 — 프로젝트는 여러 머신에서 이식 가능해야 합니다. 하드코딩된 절대 경로 금지.
- 파이프라인 *코어* 모듈에 pip 의존성을 추가하지 마세요. 선택적 단계는 선택적 패키지를 사용할 수 있으며, 해당 패키지 없이도 정상적으로 성능이 저하되어야 합니다.
- 전진 전용 상태 머신을 약화시키지 마세요 — 그것이 비용 상한선입니다.
- 공식 미국 정부 상징을 도입하지 말고, 원본의 편집된 부분을 되돌리는 어떤 것도 추가하지 마세요.
- D1 스키마 변경은 **두 개**의 파일을 수정합니다: `pipeline/lib/manifest_schema.sql`과 `db/schema.sql`.
- 새 코드에는 테스트를 포함하세요. Conventional-commit 메시지를 사용하세요.

PR을 보내기 전에 `CLAUDE.md`와 `docs/20260511/00-*`를 먼저 읽고, 구조적인 변경 사항에 대해서는 이슈를 열어 논의해 주세요.

