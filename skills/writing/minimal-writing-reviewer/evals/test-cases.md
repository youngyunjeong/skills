# Minimal Writing Reviewer Evaluation Golden Dataset & Scoring Criteria (`evals/test-cases.md`)

Golden test cases and deterministic assertions for verifying `minimal-writing-reviewer` quality across Korean documents, Korean UI copy, English documents, English UX microcopy, project ruleset overrides, and factual verification boundaries.

---

## Scoring Protocol

Each test case defines:
1. **Context & Ruleset**: Active project ruleset (or `Default Minimal Style`) and target surface (Document vs UI Copy).
2. **Input (Bad Sentence / Passage)**: Raw text containing specific style, grammar, or UX copy defects.
3. **Expected Output (Golden Reference)**: Reference revision that resolves every defect while preserving facts and constraints.
4. **Eval Assertions**:
   - `must_not_contain`: Substrings or patterns that **must be removed** (0 matches required).
   - `must_contain_any` / `must_contain_all`: Tokens, active verbs, or preserved identifiers that **must appear** in the revised text or review verdict.
   - `invariants`: Semantic, formatting, or behavioral constraints that must hold.

### Grading Scale
- **PASS (1.0)**: All `must_not_contain`, `must_contain_*`, and `invariants` assertions pass.
- **PARTIAL (0.5)**: Style defects are fixed, but a minor recommended refinement is missed without violating invariants.
- **FAIL (0.0)**: Any `must_not_contain` token survives, any required domain identifier/placeholder is broken, or an unverified policy fact is invented.

---

## Part 1. Korean Test Cases (이오덕 《우리글 바로쓰기》 & UI 문구)

### KO-01: 번역투 조사(`~의`, `~에 있어서`, `~을 통하여`)·접미사(`-적`)·무생물 주어 교정
- **Surface / Ruleset**: Document / `Default Minimal Style`
- **Input**:
  > 본 가이드 문서는 시스템의 안정적인 운영에 있어서 필수적인 설정값들의 변경 방법을 예제를 통하여 구체적으로 설명합니다.
- **Expected Output**:
  > 시스템을 안정되게 운영하는 데 꼭 필요한 설정값을 바꾸는 방법을 예제로 자세히 설명합니다.
- **Eval Assertions**:
  - `must_not_contain`: `["본 가이드 문서는", "시스템의", "안정적인", "에 있어서", "필수적인", "설정값들의", "통하여", "구체적으로"]`
  - `must_contain_any`: `["꼭 필요한", "필수"]`, `["바꾸는", "고치는"]`, `["예제로", "예제를 들어"]`, `["자세히", "알기 쉽게"]`
  - `invariants`: Drop the self-referential preamble ("본 가이드 문서는") and eliminate every `-적` suffix and unnecessary `~의`/`~들`.

---

### KO-02: 이중 피동(`보여지다`, `나뉘어지다`) 및 명사화·관료투(`진행`, `수행`, `발생`) 제거
- **Surface / Ruleset**: Document / `Default Minimal Style`
- **Input**:
  > 검증 모듈은 두 단계로 나뉘어지며, 데이터 초기화 수행 이후에 검증 진행 과정에서 오류가 발생되어질 경우 관리자에게 알림이 전송되어집니다.
- **Expected Output**:
  > 검증 모듈은 두 단계로 나뉩니다. 데이터를 초기화한 뒤 검증하다가 오류가 나면 관리자에게 알림을 보냅니다.
- **Eval Assertions**:
  - `must_not_contain`: `["나뉘어지며", "나뉘어지다", "초기화 수행", "검토 진행", "검증 진행", "발생되어질", "전송되어집니다", "전송됩니다"]`
  - `must_contain_any`: `["나뉩니다", "나눕니다"]`, `["초기화한 뒤", "초기화하고"]`, `["오류가 나면", "오류가 생기면"]`, `["알림을 보냅니다"]`
  - `invariants`: Split the run-on sentence into two clear sentences and replace all double passives and abstract action nouns (`수행`, `진행`, `발생`) with verbs.

---

### KO-03: 과잉 사물 높임(`-시-`)·공문서 투 한자어(`미입력 시`, `익일`, `요망`)·겹말 교정
- **Surface / Ruleset**: UI Copy (Checkout & Form Notice) / `Default Minimal Style (해요체/하십시오체 호환)`
- **Input**:
  > 필수 항목 미입력 시 주문이 접수되시지 않으며, 금일 오후 6시 이후 결제건은 익일 오전 중으로 발송이 진행되실 예정이오니 미리 사전 확인 요망.
- **Expected Output**:
  > 필수 항목을 입력하지 않으면 주문을 접수할 수 없습니다. 오늘 오후 6시 뒤에 결제한 주문은 다음 날 오전에 보내 드립니다. 미리 확인해 주세요.
- **Eval Assertions**:
  - `must_not_contain`: `["미입력 시", "접수되시지", "금일", "익일", "진행되실", "미리 사전", "요망"]`
  - `must_contain_all`: `["입력하지 않으면", "오늘", "다음 날"]`
  - `must_contain_any`: `["확인해 주세요", "살펴봐 주세요"]`, `["보내 드립니다", "발송합니다"]`
  - `invariants`: Remove honorific `-시-` from inanimate subjects (`주문`, `발송`), replace Sino-Korean bureaucratic terms (`미입력 시`, `금일`, `익일`, `요망`), and remove the pleonasm (`미리 사전`).

---

### KO-04: UI 오류 문구 & 확인 대화상자 버튼 — 원인·다음 행동 명시 및 동사형 라벨
- **Surface / Ruleset**: UI Copy (Upload Error Dialog) / `Default Minimal Style`
- **Context**: Maximum file size is 50 MB. Primary button lets the user pick another file; secondary button closes the dialog.
- **Input**:
  > - Title: 에러 발생
  > - Body: 잘못된 파일입니다. 파일의 업로드가 불가능합니다. (약 50MB 정도 이하의 파일들만 가능함)
  > - Buttons: `[예]` / `[아니오]`
- **Expected Output**:
  > - Title: 파일을 올릴 수 없습니다
  > - Body: 파일 크기가 50MB를 넘습니다. 50MB 이하 파일을 다시 골라 주세요.
  > - Buttons: `[다른 파일 고르기]` / `[닫기]`
- **Eval Assertions**:
  - `must_not_contain`: `["에러 발생", "잘못된 파일입니다", "파일의 업로드", "불가능합니다", "약 50MB 정도", "파일들만", "가능함", "[예]", "[아니오]"]`
  - `must_contain_all`: `["50MB"]`
  - `must_contain_any`: `["다른 파일 고르기", "다시 고르기", "파일 다시 선택"]`, `["닫기", "취소"]`
  - `invariants`: Do not blame the user ("잘못된 파일입니다"); state the exact cause (exceeds 50 MB) and next step; replace `[예]/[아니오]` with action-specific button labels.

---

### KO-05: 프로젝트 Ruleset 우선 적용 (도메인 용어·식별자·플레이스홀더 보존)
- **Surface / Ruleset**:
  - `Project Ruleset`:
    1. 공식 도메인 용어 `워크스페이스`, `초대 코드`, 식별자 `workspace_uuid`, 플레이스홀더 `{expireHours}`는 바꾸지 않는다.
    2. 문체는 `해요체`(`~해요`, `~해 주세요`)로 통일한다.
- **Input**:
  > `workspace_uuid`에 대한 워크스페이스의 초대 코드가 생성되어졌습니다. 해당 코드는 `{expireHours}`시간 동안의 유효 기간을 가지며, 만료 시 재발급의 진행이 필요합니다.
- **Expected Output**:
  > `workspace_uuid` 워크스페이스 초대 코드를 만들었어요. 이 코드는 `{expireHours}`시간 동안만 쓸 수 있으니, 기한이 지나면 새로 발급받아 주세요.
- **Eval Assertions**:
  - `must_not_contain`: `["에 대한", "워크스페이스의", "생성되어졌습니다", "동안의", "유효 기간을 가지며", "만료 시", "재발급의 진행이"]`
  - `must_contain_all`: `["workspace_uuid", "워크스페이스", "초대 코드", "{expireHours}"]`
  - `invariants`: Preserve `workspace_uuid`, `워크스페이스`, `초대 코드`, and `{expireHours}` verbatim while applying `해요체` and Lee O-deok's rules to the surrounding prose.

---

## Part 2. English Test Cases (Strunk & White, Williams, & UX Writing)

### EN-01: Nominalizations (Zombie Nouns), Passive Voice, and Wordy Phrases
- **Surface / Ruleset**: Document / `Default Minimal Style`
- **Input**:
  > It is important to note that in this document we will provide a description of how the utilization of the caching layer is performed by the edge worker in order to achieve a reduction in database query latency.
- **Expected Output**:
  > The edge worker uses the caching layer to reduce database query latency.
- **Eval Assertions**:
  - `must_not_contain`: `["It is important to note", "in this document", "provide a description", "utilization", "is performed by", "in order to", "achieve a reduction"]`
  - `must_contain_all`: `["edge worker", "caching layer", "database query latency"]`
  - `must_contain_any`: `["uses", "caches"]`, `["reduce", "lower", "cut"]`
  - `invariants`: Cut all metadiscourse ("It is important to note that in this document..."), make `The edge worker` the grammatical subject, and replace nominalizations with active verbs.

---

### EN-02: Positive Form, Specific/Concrete Language, and Parallel Construction
- **Surface / Ruleset**: Document (Operational Runbook) / `Default Minimal Style`
- **Context**: Token expiration is 15 minutes; retry limit is 3 attempts.
- **Input**:
  > Do not accept tokens that are not unexpired (older than 15 minutes). When a failure occurs:
  > 1. Checking the signature header
  > 2. You should verify the timestamp
  > 3. Rejection of requests after several failed attempts (3 attempts max)
- **Expected Output**:
  > Reject tokens older than 15 minutes. When validation fails:
  > 1. Check the signature header.
  > 2. Verify the timestamp.
  > 3. Reject the request after 3 failed attempts.
- **Eval Assertions**:
  - `must_not_contain`: `["Do not accept", "not unexpired", "1. Checking", "2. You should", "3. Rejection of", "several failed attempts"]`
  - `must_contain_all`: `["Reject", "15 minutes", "1. Check", "2. Verify", "3. Reject", "3 failed attempts"]`
  - `invariants`: Turn the double negative ("Do not accept tokens that are not unexpired") into a positive assertion ("Reject tokens older than 15 minutes"), replace vague "several" with exact "3", and make all three numbered steps parallel imperative verbs.

---

### EN-03: Williams Old-to-New Information Flow & Modifier Placement
- **Surface / Ruleset**: Document (Architecture Spec) / `Default Minimal Style`
- **Input**:
  > The client signs the canonical request payload before sending it over TLS. Signing keys rotated every 24 hours by the key management service are used when this canonical request payload is verified by the gateway only for authenticated routes.
- **Expected Output**:
  > The client signs the canonical request payload before sending it over TLS. For authenticated routes only, the gateway verifies this payload using signing keys that the key management service rotates every 24 hours.
- **Eval Assertions**:
  - `must_not_contain`: `["is verified by the gateway only for"]`
  - `must_contain_all`: `["canonical request payload", "gateway", "24 hours"]`
  - `invariants`: Link the second sentence back to the familiar topic (`payload` / `gateway`), resolve the squinting modifier (`only`), and place the new detail (`rotates every 24 hours`) in the sentence's end-stress position.

---

### EN-04: English UX Microcopy — Error Message & Pretentious Vocabulary
- **Surface / Ruleset**: UI Copy (Payment Form Error) / `Default Minimal Style`
- **Input**:
  > An unexpected error occurred due to the fact that you entered an invalid expiration date. Please endeavor to utilize a valid MM/YY format in order to facilitate payment processing.
- **Expected Output**:
  > Card expiration date must use the MM/YY format (for example, 08/28). Enter a valid date to complete your payment.
- **Eval Assertions**:
  - `must_not_contain`: `["unexpected error occurred", "due to the fact that", "you entered an invalid", "endeavor", "utilize", "in order to", "facilitate"]`
  - `must_contain_all`: `["MM/YY"]`
  - `invariants`: Remove user-blaming language ("you entered an invalid...") and pretentious words (`endeavor`, `utilize`, `facilitate`); state the required format and next action concisely.

---

## Part 3. Boundary & Verification Test Cases

### EDGE-01: Unsupported Policy / Missing Evidence Must Trigger `VERIFICATION REQUIRED`
- **Surface / Ruleset**: UI Copy (Refund Notice) / `Default Minimal Style`
- **Context**: The caller provides no refund policy, fee schedule, or business-day SLA in the review context.
- **Input**:
  > 환불 신청 진행 시 수수료가 차감되어질 수 있으며 추후 영업일 기준 수일 내로 환불 처리가 완료되십니다.
- **Expected Output**:
  > - **Verdict**: `VERIFICATION REQUIRED` (`확인 필요`)
  > - **Draft Revision (pending policy confirmation)**: 환불을 신청하면 수수료를 뺀 금액을 영업일 기준 [N]일 안에 돌려드립니다. *(확인 필요: 수수료 부과 조건·비율 및 정확한 환불 소요 영업일 수 근거가 제공되지 않았으므로 임의로 수치를 확정하지 않음)*
- **Eval Assertions**:
  - `must_not_contain`: `["진행 시", "차감되어질", "완료되십니다"]`
  - `must_contain_any`: `["VERIFICATION REQUIRED", "확인 필요"]`
  - `invariants`: Must **never** invent a specific fee percentage or number of business days when evidence is missing; must flag the item as `VERIFICATION REQUIRED` (`확인 필요`) while still cleaning up the grammar defects (`진행 시`, `차감되어질`, `완료되십니다`).
