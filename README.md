<p align="center">
  <img src="Docs/wiki/images/banner.png" alt="UE5 Procedural Level Generation" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Unreal%20Engine-5.8-0E1128?logo=unrealengine&logoColor=white" alt="Unreal Engine 5.8">
  <img src="https://img.shields.io/badge/C%2B%2B-Runtime%20Plugin-00599C?logo=cplusplus&logoColor=white" alt="C++">
  <img src="https://img.shields.io/badge/Multiplayer-Seed%20Replication-8A4FFF" alt="Seed Replication">
  <img src="https://img.shields.io/badge/Python-Editor%20Test-3776AB?logo=python&logoColor=white" alt="Python">
  <a href="https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki"><img src="https://img.shields.io/badge/📖%20Wiki-가이드%20보기-2EA44F" alt="Wiki"></a>
</p>

# 🧩 UE5 Procedural Level Generation

Unreal Engine 5 **모듈 조립형 절차적 맵 생성** 기술 템플릿.
미리 만든 방 모듈을 시드(seed)에 따라 매번 다른 순서와 연결로 이어 붙여 맵을 만듭니다. **방은 손으로, 연결은 규칙으로.**

---

## 🗺 Gallery: 시드만 바꾼 결과

| seed 535117761 | seed 2037773057 |
|:---:|:---:|
| ![](Docs/wiki/images/seed_535117761_top.png) | ![](Docs/wiki/images/seed_2037773057_top.png) |
| **seed 1873600385** | **seed 1040415745** |
| ![](Docs/wiki/images/seed_1873600385_top.png) | ![](Docs/wiki/images/seed_1040415745_top.png) |

모두 같은 방 모듈 8종과 같은 규칙으로 만들었고, **시드 번호만 다릅니다.** 첫 적용 게임 [꿈 수거반](https://github.com/JiwonEnv/Unreal-Engine-5-Oil-Pastel-Dreamcore-Game)에서 찍은 화면입니다. → [갤러리 자세히](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/01-생성-갤러리)

---

## 🧩 Features

| 기능 | 설명 |
|---|---|
| 🎲 시드 재현 | 같은 시드는 어느 컴퓨터에서든 같은 맵. 버그 리포트에 시드만 남기면 그대로 다시 만듭니다 |
| 🚪 문 소켓 연결 | 위치·방향·폭이 맞는 문끼리만 연결. 맞물리는 문은 이어서 고리 동선을 만듭니다 |
| 📦 겹침 검사 | 방마다 경계 박스로 검사하고, 겹치면 버리고 다시 시도 |
| 🧭 흐름 고정 | 시작 · 허브 · 끝 방은 고정, 끝 방은 시작에서 가장 먼 문에. 그 사이만 랜덤 |
| ✅ 결과 검증 | 역할별 개수, 겹침, 양방향 문, 도달 가능, 막다른 통로를 검사해 통과한 맵만 짓습니다 |
| 🌐 멀티플레이 | 서버는 시드 하나만 복제하고 모두가 같은 맵을 짓습니다. 실패하면 다른 시드로 재시도 |
| 🤖 내비메시 대기 | 생성 뒤 모든 방에 내비메시가 생겼는지 확인하는 `IsNavigationReady` |
| 📍 콘텐츠 배치 | 모듈마다 태그 붙은 스폰 포인트(`Item` · `HideSmall` · `Guard` …)와 소품 상자 |
| 🧪 자동 검사 | 에디터 200시드 검사 · 게임 실행 `-plgselftest` · 일부러 실패시키는 테스트 키트 |

---

## ⚡ 생성 흐름

```mermaid
flowchart LR
    A[시드] --> B[시작 방<br/>원점]
    B --> C[허브 연결]
    C --> D[빈 문에 방·통로 붙이기<br/>겹치면 버리고 재시도]
    D --> E[끝 방<br/>가장 먼 문]
    E --> F[막다른 통로 정리<br/>테마 배정]
    F --> G[검증 통과 시<br/>그레이박스 빌드]
    G --> H[내비메시 · 콘텐츠 배치]
```

| 단계 | 하는 일 | 위치 |
|---|---|---|
| 📐 규격 | 피벗 · 50cm 그리드 · 문 소켓 · 벽 두께 | `Docs/ModuleSpec.md` |
| 🧠 조립 | 엔진 객체에 의존하지 않는 순수 함수 | `PLGLayout.h/.cpp` |
| 🏗 빌드 | 시드 복제, 그레이박스, 에디터 미리보기, 스폰 포인트 | `PLGGenerator.h/.cpp` |
| 🧪 검증 | 200시드 에디터 검사, 게임 자가 검사 | Python · `-plgselftest` |

---

## 📖 Docs

| | 페이지 | 내용 |
|:---:|---|---|
| 🏠 | [Wiki Home](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki) | 처음이라면 여기부터 |
| 🗺 | [생성 갤러리](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/01-생성-갤러리) | 시드별 결과와 개념 예제 |
| 🤔 | [언제 쓰나](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/02-언제-쓰나) | 선택 기준 · 장단점 · 관리 원칙 · UE5 도구 비교 |
| 🚀 | [바로 써보기](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/03-바로-써보기) | 플러그인 넣고 시드 돌리기 |
| 🛠 | [만드는 방법](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/04-만드는-방법) | STEP 1~9 단계별 설명 |
| 📐 | [모듈 규격](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/05-모듈-규격) | 피벗 · 그리드 · 문 소켓 · 샘플 키트 |
| 🧪 | [검증과 디버그](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/06-검증과-디버그) | 200시드 검사 · 자가 검사 · 명령줄 |
| 🩹 | [문제 해결](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/07-문제-해결) | 증상별 원인과 해결 |
| 📍 | [진행 상황](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/08-진행-상황) | STEP별 구현 상태 |
| 🗓 | [변경 이력](https://github.com/JiwonEnv/UE5-Procedural-Level-Generation/wiki/09-변경-이력) | 날짜별 변경 |

---

## 📁 폴더 구조

| 폴더 | 내용 |
|---|---|
| `Plugins/ProcLevelGen` | 다른 프로젝트에 가져갈 생성기 본체 (C++). 규격은 `Plugins/ProcLevelGen/Docs/ModuleSpec.md` |
| `Docs/wiki` | GitHub Wiki 원본 (페이지 · 이미지) |

플러그인은 [꿈 수거반](https://github.com/JiwonEnv/Unreal-Engine-5-Oil-Pastel-Dreamcore-Game) 안에서 만들고 게임에서 바로 테스트합니다. 바뀔 때마다 이 저장소에도 같은 내용으로 커밋합니다.

---

## 🛠 Tech Stack

| 분야 | 사용 |
|---|---|
| 🎮 Engine | Unreal Engine 5.8 |
| 🧠 Generation | C++ Runtime Plugin · `FRandomStream` 시드 난수 · BFS 거리 계산 |
| 🏗 Build | Instanced Static Mesh 그레이박스 · `CallInEditor` 미리보기 |
| 🌐 Network | 시드 복제(`ReplicatedUsing`) · 서버 재시도 |
| 🤖 AI | Runtime NavMesh · 내비 준비 확인 |
| 🧪 Test | Python 에디터 스크립트 · 게임 명령줄 자가 검사 · PowerShell 테스트 러너 |
| 🌐 Workflow | Git · Git LFS · GitHub Wiki |
