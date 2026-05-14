# GitHub — 3개 중 3번째 게시물 · 아키텍처 노트 (ADR 스타일 토론)

**사용처:** "Show and tell" / "Architecture" 아래의 토론 또는 `docs/` ADR 초안.
**키워드:** architecture, ADR, forward-only state machine, local LLM, Ollama, OCR, edge computing, CSP, security headers, data pipeline, cost engineering, SQLite manifest, D1, R2, KV
**하이퍼링크:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## ufolens.com이 현재 방식으로 구축된 이유

[ufolens.com](https://www.ufolens.com)([PURSUE UAP 아카이브](https://www.war.gov/ufo)의 검색 가능하고 다국어를 지원하는 재구축 버전)을 형성한 세 가지 결정에 대한 노트입니다. 의견이나 반론을 환영합니다.

### 1. 파이프라인은 의도적으로 전진 전용 상태 머신으로 설계되었습니다

상태: `discovered → downloaded → ocr_done → translated → published`. 문서는 할 일이 있을 때만 앞으로 이동합니다. 발행된 콘텐츠는 델타 감지기가 소스의 실제 변경을 감지하지 않는 한 절대 재처리되지 않습니다.

**이유:** OCR + 번역은 비용이 많이 드는 작업이며, 아카이브는 시간이 지남에 따라 커집니다. "안전을 위해 모든 것을 다시 실행하는" 파이프라인은 무한한 비용을 발생시킬 수 있습니다. 뒤로 가는 전환을 불가능하게 만들면 걷잡을 수 없는 비용 청구를 막을 수 있습니다. 비용 상한선은 운영자의 주의력이 아닌 상태 그래프의 속성입니다.

**대가:** 스키마 마이그레이션과 의도적인 재처리는 일부러 번거롭게 만들었습니다. 수용 가능한 트레이드오프입니다.

### 2. OCR과 번역은 클라우드 API가 아닌 로컬 LLM에서 실행됩니다

OCR: 오픈소스 엔진, Tesseract CLI 폴백. 번역 + NER: Apple Silicon 노트북의 Ollama를 통한 Gemma.

**이유:** 문서당 한계 비용이 0입니다. 재현 가능합니다(고정 모델 + 프롬프트). 그리고 가져오기 단계는 이미 가정용 IP에서 실행되어야 하므로(소스가 Akamai Bot Manager 뒤에 있어 `curl`이 403을 받음), 어차피 노트북이 과정에 포함됩니다.

**대가:** 번역 품질이 최신 모델보다 낮습니다. 원본 영어를 항상 한 번의 클릭으로 볼 수 있는 참조 자료에서는 괜찮습니다. 우리는 번역이 권위 있다고 주장하지 않습니다.

### 3. 두 부분은 발행된 번들이라는 단 하나의 인터페이스만 공유합니다

파이프라인은 프로덕션 데이터베이스에 직접 쓰지 않습니다. `{ SQL, 애셋 매니페스트, 캐시 퍼지 목록 }`을 내보냅니다. "발행" = 해당 번들을 전방으로 적용하는 것입니다(SQL을 엣지 SQL DB로 푸시, 애셋을 객체 스토리지에 동기화, 명명된 캐시 키를 퍼지).

**이유:** 로컬 측과 엣지 측이 독립적으로 발전할 수 있습니다. 번들은 검토 가능합니다. 그리고 "데이터 배포"는 매번 같은 형태입니다. Worker는 작은 TypeScript/Hono 앱이며 — 엄격한 CSP(no `unsafe-inline`; 인라인 JSON-LD는 sha256으로 고정), `Accept-Language` + 국가→언어 협상, 30일 KV 페이지 캐시, 매일 실행되는 하우스키핑 cron — 데이터가 어떻게 만들어졌는지 알 필요가 없습니다.

**대가:** D1 스키마 변경은 두 개의 파일(`pipeline/lib/manifest_schema.sql`, `db/schema.sql`)을 수정합니다. 저렴한 보험입니다.

### 동작에 내장된 절대 원칙

- 미국 정부와 제휴하지 않음; 공식 상징 없음.
- 원본의 편집된 부분은 보존되며, 절대 되돌리지 않음.
- 비디오 출처는 DVIDS / AARO로 명시.
- 사이트 전체에 `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` 적용 — 검색 인덱싱 가능, AI 스크래핑 거부.

라이브: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

