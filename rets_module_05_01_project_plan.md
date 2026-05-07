# rets_module_05_01_project_plan.md
# RETS-MODULE-05 PoC 프로젝트 계획서 (PP)

> 산출물 유형: MD01 — PoC 프로젝트 계획서 (Project Plan)
> 모듈: RETS-MODULE-05 요구사항 기반 테스트케이스 생성 도구
> Team ID: team2
> 작성일: 2026-05-07
> 버전: 1.0

---

## 1. 프로젝트 개요

### 1.1. 사업 배경

본 프로젝트는 사내 요구 공학 도구 스위트(RETS; Requirements Engineering Tool Suite) 구축 사업의 일환으로, **RETS-MODULE-05 요구사항 기반 테스트케이스 생성 도구** 개발을 담당한다.

요구사항과 테스트케이스 간 연결이 부재하거나 단절될 경우, 검증 누락·테스트 재작업·사업 품질 리스크가 증가한다. 본 모듈은 정의된 개발요구사항(DEV-REQ)으로부터 실행 및 검증 가능한 테스트케이스를 자동 생성하고, 양방향(요구사항 ↔ 테스트케이스) 추적을 지원하여 이 문제를 해소한다.

### 1.2. 프로젝트 목표

- LLM 기반으로 정상·예외·경계 시나리오와 Given-When-Then 형식의 테스트케이스를 자동 생성
- 요구사항↔테스트케이스 양방향 추적 매트릭스 및 커버리지 관리 기능 제공
- 요구사항 변경 발생 시 영향을 받는 테스트케이스를 자동 식별하고 LLM 기반 변경 가이드 제공
- 추적 관계를 네트워크 그래프로 시각화하여 직관적 탐색 지원
- 단일 Vanilla HTML 파일 형태의 실행 가능한 PoC 웹앱 제출

### 1.3. 프로젝트 범위

| 구분 | 내용 |
|------|------|
| 개발 모듈 | RETS-MODULE-05 요구사항 기반 테스트케이스 생성 도구 |
| 구현 형태 | 단일 HTML 파일 (Vanilla JS + CSS 내장) |
| LLM 연동 | LLM 릴레이 서버(ai-relay.mooo.com) 활용, team_id: team2 |
| 입력 | 개발요구사항(DEV-REQ) 목록 JSON (rets_02c_req_schema.json 준수) |
| 출력 | 테스트케이스 목록(CSV), 요구사항↔테스트케이스 양방향 추적 매트릭스(CSV) |
| 제외 범위 | 백엔드 서버, 데이터베이스, 타 모듈과의 실시간 연동 |

---

## 2. 팀 구성 및 역할 분담

### 2.1. 팀 정보

| 항목 | 내용 |
|------|------|
| Team ID | team2 |
| 담당 모듈 | RETS-MODULE-05 |
| 팀원 수 | 4~5명 |

### 2.2. 역할 분담 (R&R)

| 역할 | 주요 책임 | 비고 |
|------|-----------|------|
| PM / 요구사항 분석 | 프로젝트 계획 수립, RFP 분석, MD01·MD02·MD10 주관 | 팀장 겸임 가능 |
| 기능 설계 / 문서화 | SRS·TDD·DDD·SSD 작성, 요구사항-테스트케이스 추적 설계 | |
| UI/UX 설계 | 화면 설계서(SSD) 작성, 디자인 시스템 적용, 사용자 흐름 정의 | |
| 개발 (프론트엔드) | HTML·Vanilla JS 구현, LLM API 연동, 그래프 시각화 구현 | 1~2명 |
| QA / 테스트 | 벤치마크 테스트 수행(MD08), Playwright 자동화, 사용자 가이드(MD09) 작성 | |

> 팀 규모에 따라 1인이 2개 이상의 역할을 겸임할 수 있으며, 상황에 따라 유연하게 조정한다.

### 2.3. 협업 방식

- **버전 관리**: Git 기반 브랜치 전략 (main / develop / feature/*)
- **산출물 관리**: 본 GitHub 저장소 내 폴더 구조에 따라 단계별 커밋
- **커뮤니케이션**: 팀 내 정기 싱크(daily/weekly) + 비동기 코멘트
- **코드 리뷰**: PR 기반, 최소 1인 이상 리뷰 후 머지

---

## 3. 개발 일정 (2주 스프린트)

### 3.1. 전체 일정

| 주차 | 기간 | 주요 활동 | 산출물 |
|------|------|-----------|--------|
| STEP 0 | D-Day (사전) | RFP 이해 및 범위 정리 | — |
| STEP 1 | 1주차 1~2일 | 프로젝트 계획 수립, 제품 요구사항 정의 | MD01, MD02 |
| STEP 2 | 1주차 3~5일 | SRS 상세화, 테스트 설계, 데이터/화면 설계 | MD03, MD04, MD05, MD06 |
| STEP 3 | 2주차 1~4일 | 웹앱 구현, 벤치마크 테스트, 사용자 가이드 | MD07, MD08, MD09 |
| STEP 4 | 2주차 5일 | 제안서 및 발표자료 작성 | MD10, MD11 |
| STEP 5 | 2주차 종료 | PoC 발표 세션 | — |

### 3.2. 세부 마일스톤

| 마일스톤 | 예상 완료 | 비고 |
|----------|-----------|------|
| MD01, MD02 완료 | D+2 | 이 문서 포함 |
| MD03 (SRS) 완료 | D+4 | 기능/비기능 요구사항 전체 명세 |
| MD04, MD05, MD06 완료 | D+5 | 테스트·데이터·화면 설계 |
| MD07 (앱 구현) v1.0 완료 | D+9 | PoC 가능 수준 |
| MD08, MD09 완료 | D+10 | 테스트 결과 + 사용자 가이드 |
| MD10, MD11 완료 | D+12 | 제안서 + 발표자료 |
| PoC 발표 | D+14 | 최종 시연 |

---

## 4. 산출물 목록

| 산출물 ID | 산출물명 | 파일명 | 포맷 | 생성 단계 | 우선순위 |
|-----------|----------|--------|------|-----------|----------|
| MD01 | PoC 프로젝트 계획서 | rets_module_05_01_project_plan.md | Markdown | STEP 1 | 필수 |
| MD02 | 제품 요구사항 정의서 | rets_module_05_02_prd.md | Markdown | STEP 1 | 필수 |
| MD03 | 소프트웨어 요구사항 명세서 | rets_module_05_03_srs.xlsx | XLSX | STEP 2 | 필수 |
| MD04 | 테스트 시나리오-케이스 설계서 | rets_module_05_04_tdd.md | Markdown | STEP 2 | 필수 |
| MD05 | 데이터 설계서 | rets_module_05_05_ddd.md + rets_module_05_05_ddd_schema.json | MD + JSON | STEP 2 | 필수 |
| MD06 | 시스템 화면 설계서 | rets_module_05_06_ssd.md | Markdown | STEP 2 | 필수 |
| MD07 | 실행 가능 모듈 소스코드 | rets_module_05_07_app.html | HTML | STEP 3 | 필수 |
| MD08 | 벤치마크 테스트 수행 결과 | rets_module_05_08_test_results.md | Markdown | STEP 3 | 선택 |
| MD09 | 웹 기반 사용자 가이드 | rets_module_05_09_user_guide.html | HTML | STEP 3 | 필수 |
| MD10 | 사업 제안서 | rets_module_05_10_proposal_doc.docx | DOCX | STEP 4 | 필수 |
| MD11 | PoC 발표자료 | rets_module_05_11_poc_presentation.pptx | PPTX | STEP 4 | 필수 |
| MD12 | Skill 문서 | rets_module_dev_skill.md | Markdown | STEP 4 | 선택 |

---

## 5. 상위 수준 모듈 개발 요구사항

### 5.1. 기능 요구사항 (상위)

| ID | 요구사항 명 | 설명 |
|----|-------------|------|
| FR-01 | DEV-REQ 불러오기 | JSON 형식의 개발요구사항 목록을 로드하여 파싱·표시 |
| FR-02 | LLM 기반 테스트 시나리오·케이스 자동 생성 | 각 요구사항 별 정상/예외/경계 시나리오와 Given-When-Then 테스트케이스를 LLM이 제안 |
| FR-03 | 테스트케이스 수락/거부 및 편집 | LLM 제안 결과를 사용자가 accept/reject/edit 가능 |
| FR-04 | 양방향 추적 매트릭스 관리 | 요구사항↔테스트케이스 매핑을 테이블 형태로 관리·편집 |
| FR-05 | 커버리지 현황 대시보드 | 전체 요구사항 대비 테스트케이스 커버리지를 시각화 |
| FR-06 | 변경 영향 분석 | 요구사항 변경 시 연관 테스트케이스 자동 식별 + LLM 기반 변경 권고 |
| FR-07 | 추적성 네트워크 그래프 시각화 | 요구사항-시나리오-테스트케이스를 노드/엣지로 시각화, 포커스·줌·팬 지원 |
| FR-08 | 파일 출력 | 테스트케이스 목록(CSV), 추적 매트릭스(CSV) 내보내기 |

### 5.2. 비기능 요구사항 (상위)

| ID | 요구사항 명 | 설명 |
|----|-------------|------|
| NFR-01 | 단일 파일 구현 | Vanilla JS, 단일 HTML 파일로 구현 (script·css 내장) |
| NFR-02 | 비동기 처리 | LLM 호출 등 1초 초과 가능 작업은 비동기 처리 |
| NFR-03 | 다국어 지원 | 한국어/영어 UI 전환 지원 |
| NFR-04 | 라이트/다크 모드 | 테마 전환 버튼 제공 |
| NFR-05 | 디자인 시스템 준수 | rets_02b_design_requirements.md 및 design_system.html 기준 적용 |
| NFR-06 | LLM 인간 개입 | LLM 반환값에 대해 사람이 수락/거부 가능하도록 설계 |

---

## 6. 리스크 및 대응 계획

| # | 리스크 | 발생 가능성 | 영향도 | 대응 계획 |
|---|--------|------------|--------|-----------|
| R-01 | LLM 릴레이 서버 장애/응답 지연 | 중 | 높음 | 오프라인 Mock 응답 모드 구현, 타임아웃 처리 |
| R-02 | LLM 응답 JSON 파싱 오류 | 높음 | 중 | 응답 파싱 유효성 검사 로직 추가, 재시도 메커니즘 |
| R-03 | 단일 HTML 파일 용량 과대 | 중 | 중 | 그래프 라이브러리 경량화, 불필요 코드 제거 |
| R-04 | 요구사항 스키마 변경 | 낮음 | 높음 | 스키마 버전 체크 로직, 필드 누락 시 graceful degradation |
| R-05 | 2주 일정 내 전 기능 완성 불가 | 중 | 높음 | MoSCoW 우선순위 기반 기능 절감 계획 수립, MVP 범위 사전 정의 |
| R-06 | 네트워크 그래프 대용량 데이터 성능 저하 | 중 | 중 | 노드 수 제한 옵션 제공, 렌더링 지연 시 페이지네이션 처리 |
| R-07 | 팀원 이탈/업무 집중 분산 | 낮음 | 중 | 크로스 트레이닝, 문서화 통한 지식 공유 |

---

## 7. 참고 문서

| 문서 | 경로 |
|------|------|
| RFP 원문 | /00_rfp_requirements/rets_00_rfp.pdf |
| 산출물 흐름도 | /00_rfp_requirements/rets_01_deliverables_flow.png |
| LLM 릴레이 서버 개발 가이드 | /00_rfp_requirements/rets_02a_llm_relay_server_dev_guide.md |
| LLM 시스템 프롬프트 | /00_rfp_requirements/rets_02a_llm_system_prompt.md |
| LLM API OpenAPI Doc | /00_rfp_requirements/rets_02a_llm_api_openapi_doc.json |
| 디자인 요구사항 | /00_rfp_requirements/rets_02b_design_requirements.md |
| 디자인 시스템 HTML | /00_rfp_requirements/rets_02b_design_system_html/02b_design_system_ref.html |
| 요구사항 스키마 | /00_rfp_requirements/rets_02c_req_schema.json |
| 요구사항 스키마 설명서 | /00_rfp_requirements/rets_02c_req_schema_explanation.md |
| 벤치마크 RFP 요구사항 | /02_benchmarks/rets_benchmark_01_rfp_req.json |
| 벤치마크 개발 요구사항 | /02_benchmarks/rets_benchmark_02_dev_req.json |

---

*본 문서는 RETS PoC 프로젝트 STEP 1 단계의 산출물이며, 프로젝트 진행에 따라 업데이트될 수 있다.*
