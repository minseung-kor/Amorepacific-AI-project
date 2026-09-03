# Amorepacific AI 프로젝트

> **M365 Copilot 활용 확산을 위한 AI Agent 기반 조직문화 변화관리 PoC**

조직에 AI 도구가 도입되어도 구성원이 실제 업무방식을 바꾸지 않는 문제를 해결하기 위해,  
**행동 로그와 Pulse Check 데이터를 기반으로 AI 활용 저항 요인을 진단하고, 현재 업무 맥락에 맞는 행동 개입을 생성하는 AI Agent Prototype**을 설계·구현한 프로젝트입니다.

본 프로젝트는 아모레퍼시픽 조직문화 변화관리 프로젝트에서 시작되었으며, 실제 사내 데이터가 아닌 **Synthetic / Dummy Data**를 활용하여 구현 가능성을 검증했습니다.

---

## 1. Project Background

Microsoft 365 Copilot과 같은 생성형 AI 도구가 조직에 도입되더라도 단순한 라이선스 제공이나 교육만으로는 실제 업무방식의 변화까지 이어지기 어렵습니다.

본 프로젝트에서는 문제를 단순히

> **"Copilot 사용률이 낮다"**

로 정의하지 않고,

> **"구성원이 왜 AI를 사용하지 않는가?"**

라는 관점에서 접근했습니다.

이를 위해 구성원의 **인식 데이터(Pulse Check)**와 **행동 데이터(M365/Copilot 활용 로그)**를 결합하여 AI 활용을 방해하는 원인을 진단하고, 각 원인에 맞는 Intervention을 현재 업무 안에서 제공하는 변화관리 구조를 설계했습니다.

---

## 2. Core Framework

전체 변화관리 구조는 다음 Cycle을 기반으로 합니다.

```text
Measurement
    ↓
Diagnosis
    ↓
Prescription
    ↓
Intervention
    ↓
Re-measurement
    ↓
Repeat
```

### Measurement

AI 활용과 관련된 다양한 데이터를 수집합니다.

- M365 / Copilot 활용 행동 데이터
- Pulse Check 설문 데이터
- Leadership Log
- Workflow Meta
- Collaboration Log
- Task Context

### Diagnosis

수집된 데이터를 기반으로 개인 및 팀의 AI 활용 저항 요인을 진단합니다.

### Prescription

진단된 Resistance Factor에 대응하는 Intervention Module을 자동으로 매칭합니다.

### Intervention

현재 수행 중인 업무 Context를 결합하여 LLM이 직원에게 실행 가능한 **Daily Mission**을 생성합니다.

### Re-measurement

개입 이후 동일한 기준으로 다시 측정하여 Resistance 변화와 AI 활용 행동 변화를 확인하는 구조입니다.

---

## 3. Resistance Framework

AI를 사용하지 않는 원인을 개인과 팀 차원으로 나누어 구조화했습니다.

### Individual Resistance

개인 단위는 총 12개의 Resistance Factor로 구성했습니다.

#### Need Gap
- Need Relevance Gap
- Value Awareness Gap
- Value Experience Gap
- Comparative Value Gap

#### Skill Gap
- Feature Knowledge Gap
- Prompting Gap
- Task Application Gap

#### Trust Gap
- AI Output Trust Gap
- Security Concern
- Responsibility Concern

#### Habit Gap
- Routine Inertia
- Switching Cost

### Team Resistance

팀 단위는 Leadership과 Collaboration 관점의 8개 Resistance Factor로 구성했습니다.

#### Leadership Gap
- Role Modeling Gap
- Direction Gap
- Reinforcement Gap
- Psychological Safety Gap

#### Collaboration Gap
- Sharing Gap
- Knowledge Diffusion Gap
- Peer Learning Gap
- Collective Adoption Gap

총 **20개의 Resistance Factor**와 이에 대응하는 **20개의 Intervention Module**을 설계했습니다.

---

## 4. Data

본 프로젝트에서는 실제 아모레퍼시픽 내부 데이터를 사용하지 않았습니다.

프로젝트의 분석 및 Agent 작동 가능성을 검증하기 위해 직접 설계한 **Synthetic / Dummy Data**를 사용했습니다.

주요 데이터는 다음과 같습니다.

```text
직원마스터
Adoption_Log
Pulse_Check
Leadership_Log
Workflow_Meta
Collaboration_Log
Task Context
Intervention Module Library
```

### Adoption Log

구성원의 AI 활용 행동을 나타냅니다.

예시:

- 월간 로그인 횟수
- 활성일수
- 지속 사용 주
- 전체 담당 업무 수
- AI 활용 업무 수
- AI 활용 업무 비율
- 기능별 Copilot 사용
- Word / PowerPoint / Excel / Outlook / Copilot Chat 활용

### Pulse Check

AI 활용과 관련한 구성원의 인식을 측정합니다.

예시:

- AI 필요성
- AI 가치 인식
- 활용 역량
- 결과 신뢰
- 보안 우려
- 책임 인식
- 활용 습관
- 리더십
- 협업 문화

### Task Context

AI Agent가 직원의 현재 업무 맥락을 파악하기 위한 데이터입니다.

예시:

- Task Name
- Task Purpose
- Task Description
- Expected Output
- Main Tool
- Available AI Tool
- Task Status
- Priority
- Due Time
- Collaboration Type
- Sensitivity Level

---

## 5. Individual Diagnosis Logic

현재 개인 단위 Resistance Diagnosis Pipeline을 Python으로 구현했습니다.

직원 ID를 입력하면 해당 직원의 Pulse Check와 Adoption Log를 조회하고 12개의 Individual Resistance Score를 계산합니다.

```text
Employee ID
    ↓
Pulse Check
+
Adoption Log
    ↓
12 Resistance Scores
    ↓
Primary Resistance
```

행동 데이터로 관찰 가능한 Factor는 Pulse와 행동 기반 Proxy를 함께 사용합니다.

현재 PoC에서는 다음과 같은 행동 지표를 활용합니다.

```text
Feature Knowledge Gap
→ Copilot 기능 및 앱 활용 다양성

Task Application Gap
→ AI 활용 업무 비율

Routine Inertia
→ 활성일수 + 지속사용주
```

행동 데이터로 직접 관찰하기 어려운 Factor는 Pulse Score를 중심으로 진단합니다.

행동 Proxy가 존재하는 경우 기본 결합 방식은 다음과 같습니다.

```text
Resistance Score
= Pulse Score × 0.7
+ Behavior Score × 0.3
```

※ Threshold, Goal 및 일부 가중치는 PoC 검증을 위한 가정값이며 실제 기업 적용 시 데이터 기반 Calibration이 필요합니다.

---

## 6. Automatic Module Matching

Python이 가장 높은 Resistance Factor를 **Primary Resistance**로 선정한 뒤, Intervention Module Library에서 대응 Module을 자동으로 조회합니다.

예시:

```text
Employee
→ AP0002

Primary Resistance
→ Switching Cost

Resistance Score
→ 62.5

Assigned Module
→ M12 Low-Friction Adoption Module
```

Module Library에는 각 Resistance Factor별로 다음 정보가 정의되어 있습니다.

- Module ID
- Module Name
- Module Objective
- Target Behavior
- Allowed Intervention
- Forbidden Intervention
- Mission Boundary
- Success Criteria

따라서 LLM이 임의로 개입 방법을 결정하는 것이 아니라, **Python이 먼저 진단과 처방 범위를 결정한 뒤 LLM은 그 범위 안에서 개인화만 수행하도록 설계했습니다.**

---

## 7. Task Context Integration

Primary Resistance와 Intervention Module이 결정되면 해당 직원의 현재 업무를 Task Context에서 조회합니다.

현재 구현에서는 다음 우선순위로 Task를 선택합니다.

```text
In Progress Task
    ↓
Planned Task
    ↓
No suitable task
```

예시:

```text
Current Task
→ 주간 마케팅 회의 준비

Main Tool
→ Teams

Available AI Tool
→ M365 Copilot (Teams 내)
```

이 정보를 Resistance 및 Module 정보와 결합하여 LLM에 전달할 **Agent Input JSON**을 생성합니다.

---

## 8. AI Intervention Agent

Python과 LLM의 역할을 명확하게 분리했습니다.

### Python

Python은 다음 의사결정을 담당합니다.

```text
Data Loading
    ↓
Resistance Diagnosis
    ↓
Primary Resistance Selection
    ↓
Module Matching
    ↓
Task Context Selection
    ↓
Agent Input Generation
    ↓
LLM Output Validation
```

### LLM

LLM은 Python이 결정한

```text
Primary Resistance
+
Intervention Module
+
Current Task Context
```

를 기반으로 현재 업무 안에서 수행 가능한 행동 Mission을 생성합니다.

즉,

> **Python = 무엇이 문제인지, 어떤 방향으로 개입할지 결정**  
> **LLM = 그 방향을 현재 업무에 맞는 구체적인 행동으로 개인화**

하는 구조입니다.

---

## 9. End-to-End Individual Pipeline

현재 개인 단위 Intervention Pipeline을 End-to-End로 연결했습니다.

```text
Synthetic Data
        ↓
Python Resistance Diagnosis
        ↓
Primary Resistance Selection
        ↓
Intervention Module Matching
        ↓
Current Task Context
        ↓
Agent Input JSON
        ↓
OpenAI API
        ↓
Structured Mission Generation
        ↓
Python Validation
        ↓
Employee Mission Popup
        ↓
Interaction Log
```

직원 ID만 변경하면 해당 직원의 데이터에 따라 Resistance, Module, Task, Mission이 자동으로 달라집니다.

예를 들어:

```text
AP0002
→ Switching Cost
→ M12 Low-Friction Adoption
→ 주간 마케팅 회의 준비
```

반면 다른 직원은:

```text
AP0003
→ Comparative Value Gap
→ M04 Comparative Value Module
→ 소비자 반응·리뷰 분석
```

과 같이 서로 다른 Intervention Pipeline이 생성됩니다.

---

## 10. Structured LLM Output

LLM의 응답은 자유 텍스트로만 받지 않고 Pydantic Schema를 활용하여 구조화했습니다.

주요 Output은 다음과 같습니다.

```text
response_status
module_id
target_resistance
selected_task_id

mission_title
popup_message
popup_reason

mission_action
execution_timing
execution_method
ai_tool

completion_criteria
verification_method
feedback_rule
next_action
```

이를 통해 LLM의 출력을 후속 프로그램에서 다시 사용할 수 있도록 구성했습니다.

---

## 11. Guardrails & Validation

LLM이 진단 결과를 임의로 변경하거나 실행할 수 없는 Mission을 생성하는 것을 방지하기 위해 Python-side Validation Logic을 구현했습니다.

주요 검증 항목은 다음과 같습니다.

- Output Module ID = Input Module ID
- Output Resistance = Primary Resistance
- Task ID 일치 여부
- Available AI Tool 일치 여부
- Response Status 검증
- Completion Criteria 존재 여부
- Verification Method 존재 여부
- Mission 부담 수준 검증
- NO_MISSION 상태 검증

Validation을 통과한 Mission만 직원에게 전달됩니다.

```text
LLM Mission
    ↓
Python Validation
    ↓
PASS
    ↓
Employee Popup
```

---

## 12. Employee Mission Popup

생성된 Intervention은 일반 직원이 쉽게 이해할 수 있도록 간단한 팝업 형태로 제공합니다.

직원 화면에는 다음 정보만 노출합니다.

```text
현재 업무
+
짧은 AI Mission
+
Mission 이유
+
[나중에] / [지금 해보기]
```

직원에게는 다음과 같은 내부 진단 정보는 노출하지 않습니다.

- Resistance Score
- Module ID
- Diagnosis Logic
- Validation Logic

예시 Mission:

> 회의 준비 중 Teams의 Copilot에 이번 회의에서 확인할 핵심 질문을 요청하고, 필요한 항목만 회의 준비 메모에 반영해보세요.

---

## 13. Interaction Log

직원이 Mission Popup에서 행동을 선택하면 결과를 로그로 저장합니다.

현재 기록되는 정보는 다음과 같습니다.

```text
employee_id
module_id
target_resistance
selected_task_id
mission_title
action
timestamp
```

Action 예시:

```text
STARTED
POSTPONED
```

이를 통해 향후 Intervention 실행 여부와 Re-measurement를 연결할 수 있도록 설계했습니다.

---

## 14. Dashboard

진단 결과와 조직 내 AI 활용 수준을 시각화하기 위해 HTML Dashboard Prototype을 제작했습니다.

Dashboard UI는 분석 결과와 화면 요구사항을 정의한 뒤 **생성형 AI를 활용하여 HTML 형태로 제작**했습니다.

따라서 본 프로젝트에서 직접 구현한 핵심은 Front-end 개발 자체보다는 다음 구조에 있습니다.

```text
Data Structure
    ↓
Diagnosis Logic
    ↓
Intervention Logic
    ↓
AI Agent Pipeline
```

Dashboard는 해당 분석 결과를 이해하기 쉽게 전달하는 시각화 도구로 활용했습니다.

---

## 15. Prototype Scope

본 프로젝트는 **Proof of Concept(PoC)** 단계입니다.

현재 실제 아모레퍼시픽 Microsoft 365 Tenant 또는 Microsoft Graph API와 직접 연결되어 있지 않습니다.

대신 Synthetic Data를 이용하여 다음 구조의 기술적 작동 가능성을 검증했습니다.

```text
Synthetic M365 Data
+
Pulse Check
+
Task Context
    ↓
Diagnosis
    ↓
Prescription
    ↓
Context-aware Intervention
```

---

## 16. Future Development

향후 실제 기업 환경에서는 다음 구조로 확장할 수 있습니다.

```text
M365 Activity Logs
+
Copilot Usage Logs
+
Outlook Calendar
    ↓
Behavior Pattern Detection
    ↓
Current Work Context
    ↓
Context-aware Mission
    ↓
Interaction Log
    ↓
Re-measurement
```

예를 들어 반복되는 업무 패턴과 다음 날 Calendar Event를 함께 분석하여, 직원이 실제 업무를 수행하는 시점에 적절한 Mission을 제공하는 방식으로 확장할 수 있습니다.

현재 PoC에서는 실제 M365 및 Calendar 권한이 없기 때문에 해당 데이터 구조를 Synthetic Data로 재현하는 방식의 확장을 고려하고 있습니다.

---

## 17. Repository Structure

```text
Amorepacific-AI-프로젝트/

├── README.md
│
├── 데이터/
│   ├── Synthetic Copilot Data
│   ├── AI Agent Module Library
│   └── Task Context Synthetic Data
│
├── SRC/
│   ├── Individual Diagnosis / Agent Pipeline
│   └── Prototype Notebook
│
├── 대시보드/
│   └── HTML Dashboard
│
├── 문서/
│   └── Final Presentation
│
└── 데모/
    └── AI Agent Demo Video
```

※ Repository 내부 파일명 및 코드 구조는 Prototype 정리 과정에서 지속적으로 개선하고 있습니다.

---

## 18. My Role

프로젝트에서 다음 영역을 중심으로 수행했습니다.

- AI 활용 저항요인 Diagnosis Framework 설계
- Synthetic Data 구조 설계 및 생성
- Pulse Check + 행동로그 기반 Python 분석
- Individual Resistance Diagnosis Logic 구현
- Primary Resistance 자동 선정
- Intervention Module Matching Logic 구현
- Task Context 기반 Agent Input 설계
- OpenAI API 기반 Intervention Mission 생성
- Structured LLM Output 구현
- Python-side Validation Logic 구현
- 직원 Mission Popup 구현
- Interaction Log 구현
- Dashboard 요구사항 설계 및 생성형 AI를 활용한 HTML 시각화
- AI Agent Demo Video 제작
- 최종 발표 자료 및 발표 구성

---

## 19. Key Learning

본 프로젝트를 통해 단순히 AI Tool을 사용하는 것보다,

> **문제를 측정 가능한 형태로 정의하고, 데이터를 기반으로 원인을 진단한 뒤 실제 행동 변화까지 연결하는 구조를 만드는 것**

이 더 중요하다는 점을 배웠습니다.

또한 AI를 단순한 결과물 생성 도구로 사용하는 것이 아니라,

```text
Problem Definition
    ↓
Data
    ↓
Decision Logic
    ↓
AI
    ↓
Action
```

이라는 전체 흐름 안에서 활용하는 방법을 실험했습니다.

특히 프로젝트 초기에는 각각의 데이터 분석, Agent 생성, Dashboard 등이 독립적으로 존재했지만, 이후 Python 기반 Diagnosis부터 Module Matching, Task Context, LLM Mission Generation, Validation, Popup까지 하나의 End-to-End Pipeline으로 연결하며 Prototype을 확장했습니다.

---

## Disclaimer

본 Repository에 포함된 데이터, 직원 정보, 조직 상황, AI 활용 수치 및 Task Context는 프로젝트 검증을 위해 생성한 **Synthetic / Dummy Data**입니다.

실제 아모레퍼시픽 내부 성과 데이터, 실제 직원 개인정보 또는 실제 Microsoft 365 사용 로그를 포함하지 않습니다.

본 프로젝트는 교육 및 PoC 목적으로 제작되었습니다.
