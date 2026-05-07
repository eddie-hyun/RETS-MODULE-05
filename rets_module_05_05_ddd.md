# rets_module_05_05_ddd.md
# RETS-MODULE-05 데이터 설계서 (DDD)

> 산출물 유형: MD05 — 데이터 설계서 (Data Design Document)
> 모듈: RETS-MODULE-05 요구사항 기반 테스트케이스 생성 도구
> Team ID: team2
> 작성일: 2026-05-07
> 버전: 1.0
> 선행 참고: rets_module_05_03_srs.xlsx (SRS), rets_02c_req_schema.json

---

## 1. 개요

### 1.1. 목적

본 문서는 RETS-MODULE-05에서 처리하는 주요 데이터 객체의 구조, 속성, 관계, 제약조건, 입출력 관점의 데이터 항목을 정의하고, 각 데이터 객체와 SRS 요구사항 간의 추적 관계를 정리한다.

### 1.2. 범위

RETS-MODULE-05는 백엔드 없는 단일 HTML 파일로 동작하므로, 모든 데이터는 **브라우저 세션 메모리(In-Memory)** 에 저장된다. 영속 저장소(DB, localStorage)는 사용하지 않으며, 파일 I/O(JSON 입력, CSV 출력)만을 통해 외부와 데이터를 교환한다.

### 1.3. 데이터 객체 목록

| 객체 ID | 객체명 | 설명 | SRS 추적 |
|---------|--------|------|---------|
| DDO-001 | Requirement | 로드된 DEV-REQ 요구사항 항목 | DEV_FR_001, DEV_FR_006_01 |
| DDO-002 | TestCase | 생성·편집된 테스트케이스 항목 | DEV_FR_002, DEV_FR_003 |
| DDO-003 | Mapping | 요구사항↔테스트케이스 매핑 관계 | DEV_FR_004 |
| DDO-004 | ChangeRecommendation | LLM 변경 권고 항목 | DEV_FR_006_02 |
| DDO-005 | LLMSession | LLM Chat 세션 정보 | DEV_FR_002_01 |
| DDO-006 | AppConfig | 앱 설정 (team_id, 언어, 테마) | DEV_FR_009 |

---

## 2. 데이터 객체 상세

### DDO-001. Requirement (요구사항)

**설명**: RETS 공통 스키마(`rets_02c_req_schema.json`)의 DEV 타입 요구사항을 파싱하여 메모리에 저장하는 객체.

**SRS 추적**: DEV_FR_001_01, DEV_FR_001_02, DEV_FR_001_03, DEV_FR_006_01

| 필드명 | 타입 | 필수 | 설명 | 제약조건 |
|--------|------|------|------|---------|
| req_id | string | ✅ | 요구사항 고유 ID | 패턴: `^DEV_(FR\|NFR)_\d{3}(_\d{2}){0,4}$` |
| req_type | enum | ✅ | 요구사항 유형 | 고정값: `"DEV"` |
| req_version | string | ✅ | 버전 | 예: `"1.0"` |
| req_level | integer | ✅ | 계층 깊이 | 1~5, req_id 계층과 일치 |
| req_name | string | ✅ | 요구사항명 | 최대 200자 |
| functional_class | enum | ✅ | 기능/비기능 구분 | `"기능"` or `"비기능"` |
| req_category | enum | ✅ | 요구사항 유형 | 11가지 카테고리 |
| description | string | ✅ | 상세 설명 | 최대 2000자 |
| acceptance_criteria | string[] | — | 완료 기준 목록 | |
| priority | enum | — | 우선순위 | MUST / SHOULD / COULD / WONT |
| importance | enum | — | 중요도 | Critical / High / Medium / Low |
| req_status | enum | ✅ | 요구사항 상태 | Draft / InReview / Approved / ... |
| dev_status | enum | — | 개발 상태 | Todo / InProgress / Done / ... |
| source_rfp_ids | string[] | — | 근거 RFP ID 목록 | |
| is_changed | boolean | — | 변경 플래그 | 기본값: false (모듈 내부 확장 필드) |
| created_by | string | ✅ | 발의자 | |
| last_modified_date | string | ✅ | 최종 수정일 | ISO 8601 (YYYY-MM-DD) |

**입출력**:
- 입력: JSON 파일 (`rets_02c_req_schema.json` DEV 타입) → 파싱 후 배열로 저장
- 출력: 없음 (읽기 전용 객체, 변경 플래그만 내부 갱신)

**관계**:
- 1:N → Mapping (요구사항 1건에 N개의 매핑)
- 1:N → TestCase (간접 관계, Mapping 통해)
- 1:N → ChangeRecommendation

---

### DDO-002. TestCase (테스트케이스)

**설명**: LLM이 생성하거나 사용자가 수동으로 추가한 테스트케이스.

**SRS 추적**: DEV_FR_002_02, DEV_FR_002_03, DEV_FR_003_01, DEV_FR_003_02, DEV_FR_003_03, DEV_FR_008_01

| 필드명 | 타입 | 필수 | 설명 | 제약조건 |
|--------|------|------|------|---------|
| tc_id | string | ✅ | 테스트케이스 고유 ID | 패턴: `TC-{req_id}-{seq3}` 예) TC-DEV_FR_001_01-001 |
| source_req_id | string | ✅ | 생성 기반 요구사항 ID | DDO-001.req_id 참조 |
| scenario_type | enum | ✅ | 시나리오 유형 | `Normal` / `Exception` / `Boundary` |
| title | string | ✅ | 테스트 제목 | 최대 300자 |
| precondition | string | — | Given: 전제조건 | |
| input | string | — | When: 입력값/액션 | |
| steps | string | — | 수행 절차 (단계별) | |
| expected_result | string | ✅ | Then: 예상결과 | |
| status | enum | ✅ | TC 상태 | `Draft` / `Accepted` / `Rejected` / `Needs_Update` / `Deprecated` |
| source | enum | ✅ | 생성 출처 | `LLM` / `Manual` |
| created_at | string | ✅ | 생성 시각 | ISO 8601 datetime |
| last_modified_at | string | — | 최종 수정 시각 | ISO 8601 datetime |

**CSV 출력 스키마** (파일명: `rets_module_05_tc_list.csv`):

```
tc_id, req_id, scenario_type, title, precondition, input, steps, expected_result, status
```

**관계**:
- N:1 → Requirement (source_req_id 통해)
- 1:N → Mapping

---

### DDO-003. Mapping (추적 매핑)

**설명**: 요구사항과 테스트케이스 사이의 양방향 추적 관계를 표현하는 연결 객체.

**SRS 추적**: DEV_FR_004_01, DEV_FR_004_02, DEV_FR_004_01_01

| 필드명 | 타입 | 필수 | 설명 | 제약조건 |
|--------|------|------|------|---------|
| mapping_id | string | ✅ | 매핑 고유 ID | 패턴: `MAP-{seq5}` 예) MAP-00001 |
| req_id | string | ✅ | 연결된 요구사항 ID | DDO-001.req_id 참조 |
| tc_id | string | ✅ | 연결된 TC ID | DDO-002.tc_id 참조 |
| mapping_type | enum | ✅ | 매핑 관계 유형 | `primary` (주 매핑) / `secondary` (참조 매핑) |
| created_at | string | ✅ | 매핑 생성 시각 | ISO 8601 datetime |

**비즈니스 규칙**:
- (req_id, tc_id) 조합은 유일해야 한다 (중복 매핑 불가)
- req_id와 tc_id 모두 실제 존재하는 객체를 참조해야 한다 (참조 무결성)

**CSV 출력 스키마** (파일명: `rets_module_05_trace_matrix.csv`):

```
req_id, req_name, tc_id, tc_title, mapping_type
```

**관계**:
- N:1 → Requirement
- N:1 → TestCase

---

### DDO-004. ChangeRecommendation (변경 권고)

**설명**: 요구사항 변경 시 LLM이 영향받는 TC에 대해 제시하는 권고 항목.

**SRS 추적**: DEV_FR_006_02, DEV_FR_006_02_01

| 필드명 | 타입 | 필수 | 설명 | 제약조건 |
|--------|------|------|------|---------|
| rec_id | string | ✅ | 권고 고유 ID | 패턴: `REC-{seq5}` |
| changed_req_id | string | ✅ | 변경된 요구사항 ID | DDO-001.req_id 참조 |
| affected_tc_id | string | ✅ | 영향받는 TC ID | DDO-002.tc_id 참조 |
| recommendation | enum | ✅ | 권고 유형 | `Keep` / `Update` / `Deprecate` / `Add_New` |
| rationale | string | ✅ | 권고 근거 (LLM 텍스트) | |
| status | enum | ✅ | 권고 처리 상태 | `Pending` / `Accepted` / `Rejected` |
| created_at | string | ✅ | 생성 시각 | ISO 8601 datetime |

**관계**:
- N:1 → Requirement (changed_req_id)
- N:1 → TestCase (affected_tc_id)

---

### DDO-005. LLMSession (LLM 세션)

**설명**: LLM 릴레이 서버와의 Chat 세션 정보를 관리하는 객체 (모듈 내 단일 세션).

**SRS 추적**: DEV_FR_002_01

| 필드명 | 타입 | 필수 | 설명 | 제약조건 |
|--------|------|------|------|---------|
| session_id | string | ✅ | LLM 릴레이 서버에서 반환한 세션 ID | |
| team_id | string | ✅ | 팀 식별자 | 예: `"team2"` |
| status | enum | ✅ | 세션 상태 | `idle` / `active` / `error` |
| initialized_at | string | — | 세션 초기화 시각 | ISO 8601 datetime |
| system_prompt | string | — | 사용된 시스템 프롬프트 전문 | |
| last_request_at | string | — | 마지막 요청 시각 | |

---

### DDO-006. AppConfig (앱 설정)

**설명**: 사용자가 설정한 앱 수준의 구성 정보.

**SRS 추적**: DEV_FR_009_01, DEV_FR_009_02, DEV_FR_009_03

| 필드명 | 타입 | 필수 | 설명 | 제약조건 |
|--------|------|------|------|---------|
| team_id | string | ✅ | LLM 호출에 사용할 팀 ID | 기본값: `"team2"` |
| language | enum | ✅ | UI 표시 언어 | `"ko"` / `"en"`, 기본값: `"ko"` |
| theme | enum | ✅ | UI 테마 | `"light"` / `"dark"`, 기본값: `"light"` |

---

## 3. 데이터 관계도

```
┌─────────────────────────────────────────────────────────┐
│                      메모리 상태                          │
│                                                          │
│  ┌──────────────┐          ┌──────────────┐             │
│  │ Requirement  │          │  TestCase    │             │
│  │  (DDO-001)   │          │  (DDO-002)   │             │
│  │  req_id (PK) │          │  tc_id (PK)  │             │
│  │  is_changed  │          │  source_req_id│            │
│  └──────┬───────┘          └──────┬───────┘             │
│         │  1                       │  1                  │
│         │                          │                     │
│         └──────────┐   ┌───────────┘                    │
│                  N │   │ N                               │
│              ┌─────▼───▼──────┐                         │
│              │    Mapping     │                          │
│              │   (DDO-003)    │                          │
│              │  mapping_id(PK)│                          │
│              │  req_id (FK)   │                          │
│              │  tc_id (FK)    │                          │
│              └────────────────┘                          │
│                                                          │
│  ┌──────────────┐          ┌──────────────┐             │
│  │  Requirement │─────────▶│ Change       │             │
│  │  (변경 시)   │  1:N      │ Recommendation│            │
│  └──────────────┘          │  (DDO-004)   │             │
│                            └──────┬───────┘             │
│                                   │ N:1                 │
│                            ┌──────▼───────┐             │
│                            │  TestCase    │             │
│                            │  (DDO-002)   │             │
│                            └──────────────┘             │
│                                                          │
│  ┌──────────────┐  ┌──────────────┐                     │
│  │ LLMSession   │  │  AppConfig   │                     │
│  │  (DDO-005)   │  │  (DDO-006)   │                     │
│  └──────────────┘  └──────────────┘                     │
└─────────────────────────────────────────────────────────┘
```

---

## 4. 입출력 데이터 명세

### 4.1. 입력 데이터

| 항목 | 형식 | 설명 | 참조 스키마 |
|------|------|------|------------|
| DEV-REQ 목록 | JSON (파일) | 개발 요구사항 목록 | rets_02c_req_schema.json (DEV 타입) |
| team_id | string (UI 입력) | LLM 호출용 팀 ID | — |
| 언어 설정 | enum (UI 선택) | "ko" / "en" | — |
| 테마 설정 | enum (UI 선택) | "light" / "dark" | — |

### 4.2. 출력 데이터

| 항목 | 형식 | 파일명 | 스키마 |
|------|------|--------|--------|
| 테스트케이스 목록 | CSV | rets_module_05_tc_list.csv | DDO-002 기반 자체 정의 |
| 추적 매트릭스 | CSV | rets_module_05_trace_matrix.csv | DDO-003 기반 자체 정의 |

### 4.3. LLM 입출력 데이터

**LLM 입력 메시지 구조** (JSON 요청):
```json
{
  "team_id": "team2",
  "message": "다음 요구사항에 대해 테스트케이스를 생성하세요:\n{req_description}\n\n반환 형식(JSON 배열):\n[{\"scenario_type\":\"Normal|Exception|Boundary\",\"title\":\"...\",\"precondition\":\"...\",\"input\":\"...\",\"steps\":\"...\",\"expected_result\":\"...\"}]"
}
```

**LLM 응답 파싱 대상** (Response.text에서 JSON 추출):
```json
[
  {
    "scenario_type": "Normal",
    "title": "유효한 JSON 파일 업로드 시 요구사항 목록 표시",
    "precondition": "앱이 실행 중이고 유효한 DEV-REQ JSON 파일이 준비되어 있다",
    "input": "유효한 JSON 파일 선택",
    "steps": "1. 파일 선택 → 2. 파싱 완료 대기",
    "expected_result": "요구사항 목록 그리드에 파싱된 항목이 표시된다"
  }
]
```

---

## 5. 데이터 제약사항 및 유효성 규칙

| 규칙 ID | 대상 객체 | 규칙 설명 |
|---------|-----------|-----------|
| DC-001 | DDO-001 | req_id는 `^DEV_(FR\|NFR)_\d{3}(_\d{2}){0,4}$` 패턴을 만족해야 한다 |
| DC-002 | DDO-001 | req_level은 req_id의 계층 깊이(SEQ 이후 `_XX` 수 + 1)와 일치해야 한다 |
| DC-003 | DDO-002 | tc_id는 `TC-{req_id}-{seq3}` 형식이어야 한다 |
| DC-004 | DDO-002 | scenario_type은 Normal / Exception / Boundary 중 하나이어야 한다 |
| DC-005 | DDO-003 | (req_id, tc_id) 조합은 중복될 수 없다 (유일성) |
| DC-006 | DDO-003 | req_id는 DDO-001에 존재하는 ID를 참조해야 한다 (참조 무결성) |
| DC-007 | DDO-003 | tc_id는 DDO-002에 존재하는 ID를 참조해야 한다 (참조 무결성) |
| DC-008 | DDO-004 | recommendation은 Keep / Update / Deprecate / Add_New 중 하나이어야 한다 |
| DC-009 | DDO-006 | team_id는 비어있지 않아야 한다 |
| DC-010 | DDO-006 | language는 "ko" 또는 "en"이어야 한다 |

---

## 6. SRS 요구사항 추적성

| DDO | SRS req_id | 추적 관계 |
|-----|------------|-----------|
| DDO-001 | DEV_FR_001_01 | JSON 파싱 결과를 Requirement 배열로 저장 |
| DDO-001 | DEV_FR_001_02 | Requirement 배열을 그리드로 렌더링 |
| DDO-001 | DEV_FR_001_03 | Requirement 항목 CRUD |
| DDO-001 | DEV_FR_006_01 | Requirement.is_changed 필드로 변경 플래그 관리 |
| DDO-002 | DEV_FR_002_02 | LLM 응답 파싱 후 TestCase 배열 생성 |
| DDO-002 | DEV_FR_002_03 | Given-When-Then 필드로 TestCase 구조화 |
| DDO-002 | DEV_FR_003_01 | Accept → status='Accepted', Reject → status='Rejected' |
| DDO-002 | DEV_FR_003_02 | TestCase 필드 인라인 편집 |
| DDO-002 | DEV_FR_008_01 | TestCase 배열 → CSV 출력 |
| DDO-003 | DEV_FR_004_01 | Mapping 배열로 추적 매트릭스 렌더링 |
| DDO-003 | DEV_FR_004_02 | Mapping 항목 추가/제거 |
| DDO-003 | DEV_FR_008_02 | Mapping 배열 → 추적 매트릭스 CSV 출력 |
| DDO-004 | DEV_FR_006_02 | LLM 권고 결과를 ChangeRecommendation으로 저장 |
| DDO-004 | DEV_FR_006_02_01 | 수락 → affected_tc.status 갱신 |
| DDO-005 | DEV_FR_002_01 | LLM Chat 세션 초기화 및 관리 |
| DDO-006 | DEV_FR_009_01 | team_id 저장 |
| DDO-006 | DEV_FR_009_02 | language 설정 저장 |
| DDO-006 | DEV_FR_009_03 | theme 설정 저장 |

---

*본 문서는 RETS PoC 프로젝트 STEP 2 단계의 산출물이며, SRS 변경 시 함께 업데이트된다.*
*스키마 정의 파일: rets_module_05_05_ddd_schema.json 참조*
*SRS 추적성 업데이트: rets_module_05_03_srs.xlsx의 ddd_id 컬럼에 DDO-00X 반영 완료.*
