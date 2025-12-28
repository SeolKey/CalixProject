# 🎮 Calix Project

대학교 졸업작품 Unity 게임 프로젝트

## 📋 프로젝트 개요

Calix는 Unity 엔진을 기반으로 개발된 액션 RPG 게임입니다. 플레이어는 다양한 스킬과 무기를 활용하여 적을 처치하고, 아이템을 수집하며 스테이지를 진행합니다.

## 🏗️ 프로젝트 구조

```mermaid
graph TB
    A[Calix Project] --> B[Assets]
    A --> C[ProjectSettings]
    A --> D[Packages]
    
    B --> E[01.Scenes<br/>게임 씬]
    B --> F[02.Scripts<br/>스크립트]
    B --> G[03.Prefabs<br/>프리팹]
    B --> H[04.Images<br/>이미지]
    B --> I[05.Sounds<br/>사운드]
    B --> J[06.Asset_Download<br/>에셋]
    B --> K[07.Animations<br/>애니메이션]
    B --> L[08.Models<br/>모델]
    B --> M[09.Particles<br/>파티클]
    B --> N[10.Renderer<br/>렌더링]
    
    F --> F1[Player<br/>플레이어 시스템]
    F --> F2[Enemy<br/>적 AI 시스템]
    F --> F3[Skill<br/>스킬 시스템]
    F --> F4[UI Script<br/>UI 시스템]
    F --> F5[Item Script<br/>아이템 시스템]
    F --> F6[Sound<br/>사운드 시스템]
    F --> F7[Camera<br/>카메라 시스템]
    F --> F8[Weapon<br/>무기 시스템]
```

## 🎯 게임 시스템 아키텍처

```mermaid
graph LR
    GM[GameManager<br/>게임 전체 관리] --> PS[Player System<br/>플레이어]
    GM --> ES[Enemy System<br/>적]
    GM --> SM[SkillManager<br/>스킬 관리]
    GM --> UI[UIManager<br/>UI 관리]
    
    PS --> PC[PlayerController<br/>이동/공격]
    PS --> PST[PlayerStats<br/>스탯]
    PS --> PA[PlayerAttacker<br/>공격 패턴]
    
    ES --> EC[EnemyController<br/>AI/패턴]
    ES --> EST[CharStats<br/>적 스탯]
    ES --> EP[EnemyPattern<br/>공격 패턴]
    
    SM --> SD[SkillData<br/>스킬 데이터]
    SM --> SB[SkillButton<br/>스킬 버튼]
    
    UI --> INV[Inventory<br/>인벤토리]
    UI --> SHOP[Shop<br/>상점]
    UI --> HPBAR[HP Bar<br/>체력바]
    
    IS[Item System] --> IT[Item<br/>아이템 데이터]
    IS --> ID[ItemDrop<br/>드롭 시스템]
    IS --> IP[ItemPickUp<br/>획득 시스템]
```

## 🎮 게임 플로우

```mermaid
flowchart TD
    Start([게임 시작]) --> Menu[메인 메뉴]
    Menu --> Tutorial[튜토리얼]
    Menu --> Stage1[스테이지 1]
    
    Tutorial --> Stage1
    Stage1 --> Stage2{다음 스테이지}
    
    Stage2 -->|일반 맵| NormalMap[일반 맵<br/>MapScene1-4]
    Stage2 -->|보스 맵| BossMap[보스 스테이지<br/>06_BossStage]
    Stage2 -->|상점| Shop[상점<br/>Stage6_shop]
    
    NormalMap --> Combat[전투]
    BossMap --> Combat
    
    Combat --> Drop{아이템 드롭}
    Drop -->|드롭| PickUp[아이템 획득]
    Drop -->|없음| Portal[포탈 이동]
    PickUp --> Portal
    
    Portal --> NextStage{다음 스테이지}
    NextStage -->|진행| Stage2
    NextStage -->|최종 보스| FinalBoss[최종 보스<br/>Stage7_Boss]
    
    FinalBoss --> Win{승리?}
    Win -->|승리| GameClear[게임 클리어]
    Win -->|패배| GameOver[게임 오버]
    
    Combat --> Death{플레이어 사망?}
    Death -->|사망| GameOver
    Death -->|생존| Portal
    
    GameOver --> Restart[재시작 화면]
    GameClear --> Restart
    Restart --> Menu
```

## 👤 플레이어 시스템

```mermaid
stateDiagram-v2
    [*] --> Idle: 게임 시작
    
    Idle --> Move: 이동 입력
    Idle --> Attack: 공격 입력
    Idle --> Skill: 스킬 입력
    Idle --> Dodge: 회피 입력
    Idle --> Jump: 점프 입력
    
    Move --> Attack: 공격 입력
    Move --> Skill: 스킬 입력
    Move --> Dodge: 회피 입력
    Move --> Jump: 점프 입력
    
    Attack --> Combo: 콤보 가능
    Attack --> Idle: 공격 종료
    
    Combo --> Attack: 다음 공격
    Combo --> Idle: 콤보 종료
    
    Skill --> Cooldown: 스킬 종료
    Cooldown --> Idle: 쿨다운 완료
    
    Dodge --> Idle: 회피 종료
    Jump --> Idle: 착지
    
    Attack --> Hit: 적 타격
    Skill --> Hit: 스킬 타격
    Hit --> Idle
    
    Idle --> Dead: HP 0
    Move --> Dead: HP 0
    Attack --> Dead: HP 0
    Dead --> [*]
```

## ⚔️ 스킬 시스템

```mermaid
graph TB
    SM[SkillManager] --> S1[Rapid Assault<br/>신속 공격]
    SM --> S2[Fly Mech<br/>비행 메카]
    SM --> S3[One Slash<br/>원 슬래시]
    SM --> S4[Blood Rain<br/>피의 비]
    
    S1 --> L1[레벨 1]
    S2 --> L2[레벨 1]
    S3 --> L3[레벨 1]
    S4 --> L4[레벨 1]
    
    L1 --> E1{장착 여부}
    L2 --> E2{장착 여부}
    L3 --> E3{장착 여부}
    L4 --> E4{장착 여부}
    
    E1 -->|장착| Ready1[사용 가능]
    E2 -->|장착| Ready2[사용 가능]
    E3 -->|장착| Ready3[사용 가능]
    E4 -->|장착| Ready4[사용 가능]
    
    Ready1 --> CD1[쿨다운]
    Ready2 --> CD2[쿨다운]
    Ready3 --> CD3[쿨다운]
    Ready4 --> CD4[쿨다운]
```

## 🎒 아이템 시스템

```mermaid
graph LR
    IT[Item] --> RT[Red Type<br/>체력 증가]
    IT --> YT[Yellow Type<br/>공격력 증가]
    IT --> BT[Blue Type<br/>방어력/크리티컬]
    
    RT --> R3[3개 세트<br/>체력 +100]
    RT --> R6[6개 세트<br/>체력 +200]
    
    YT --> Y3[3개 세트<br/>공격력 +10]
    YT --> Y6[6개 세트<br/>공격력 +20]
    
    BT --> B3[3개 세트<br/>방어력 +10<br/>크리티컬 +20%]
    BT --> B6[6개 세트<br/>방어력 +20<br/>크리티컬 +30%]
    
    R3 --> PS[PlayerStats]
    R6 --> PS
    Y3 --> PS
    Y6 --> PS
    B3 --> PS
    B6 --> PS
    
    PS --> HP[Max Health]
    PS --> ATK[Attack Damage]
    PS --> DEF[Defense]
    PS --> CRIT[Critical Chance]
```

## 🗺️ 스테이지 구조

```mermaid
graph TD
    Start[00_MainMenu 1<br/>메인 메뉴] --> Tutorial[03__01_tutorial<br/>튜토리얼]
    
    Tutorial --> Stage1[Stage1<br/>스테이지 1]
    
    Stage1 --> Map1[MapScene1<br/>맵 1]
    Map1 --> Map2{랜덤 선택}
    
    Map2 -->|선택 1| Map2_1[MapScene2_1]
    Map2 -->|선택 2| Map2_2[MapScene2_2]
    Map2 -->|선택 3| Map2_3[MapScene2_3]
    
    Map2_1 --> Map3{랜덤 선택}
    Map2_2 --> Map3
    Map2_3 --> Map3
    
    Map3 -->|선택 1| Map3_1[MapScene3_1]
    Map3 -->|선택 2| Map3_2[MapScene3_2]
    Map3 -->|선택 3| Map3_3[MapScene3_3]
    
    Map3_1 --> Map4[MapScene4<br/>맵 4]
    Map3_2 --> Map4
    Map3_3 --> Map4
    
    Map4 --> Bonus[BonusStage<br/>보너스 스테이지]
    Bonus --> Shop[Stage6_shop<br/>상점]
    Shop --> Boss[Stage7_Boss<br/>최종 보스]
    
    Boss --> Clear[게임 클리어]
```

## 🛠️ 주요 기능

### 게임 관리
- **GameManager**: 게임 상태 관리, 씬 전환, 나노 카운트 관리
- **EventSystem**: 게임 이벤트 시스템 관리

### 플레이어 시스템
- 이동, 점프, 대시, 회피
- 근접 공격 및 콤보 시스템
- 무기 교체 시스템
- 스탯 관리 (체력, 공격력, 방어력, 크리티컬)

### 적 AI 시스템
- NavMesh 기반 패트롤 및 추적
- 공격 패턴 시스템
- 적 타입별 행동 패턴

### 스킬 시스템
- 4가지 스킬 (Rapid Assault, Fly Mech, One Slash, Blood Rain)
- 스킬 레벨 및 장착 시스템
- 쿨다운 관리

### 아이템 시스템
- 3가지 타입 (Red, Yellow, Blue)
- 세트 효과 시스템 (3개, 6개)
- 아이템 드롭 및 획득

### UI 시스템
- 인벤토리
- 스킬 선택 및 쿨다운 표시
- HP/나노 카운트 표시
- 상점 UI
- 게임 오버/클리어 UI

## 📦 사용 기술

- **Unity Engine**: 2021.x 이상
- **C#**: 게임 로직 구현
- **Cinemachine**: 카메라 시스템
- **NavMesh**: 적 AI 경로 탐색
- **HDRP/URP**: 렌더링 파이프라인
- **TextMeshPro**: UI 텍스트

## 🎨 에셋 구조

```
Assets/
├── 01.Scenes/          # 게임 씬 파일
├── 02.Scripts/         # C# 스크립트
├── 03.Prefabs/         # 프리팹
├── 04.Images/          # 이미지 리소스
├── 05.Sounds/          # 사운드 파일
├── 06.Asset_Download/  # 외부 에셋
├── 07.Animations/      # 애니메이션
├── 08.Models/          # 3D 모델
├── 09.Particles/       # 파티클 효과
└── 10.Renderer/        # 렌더링 설정
```

## 🎮 조작 방법

- **이동**: WASD 키
- **공격**: 마우스 클릭
- **스킬**: R, F 키
- **회피**: Shift 키
- **점프**: Space 키
- **인벤토리**: I 키
- **일시정지**: ESC 키

## 📝 개발 정보

- **프로젝트 타입**: 대학교 졸업작품
- **엔진**: Unity
- **언어**: C#
- **플랫폼**: PC (Windows)

## 🔧 빌드 및 실행

1. Unity Hub에서 프로젝트 열기
2. Unity 버전 확인 (2021.x 이상 권장)
3. 프로젝트 빌드 또는 에디터에서 실행

## 📄 라이선스

이 프로젝트는 졸업작품으로 제작되었습니다.

---

**Calix Project** - Unity Action RPG Game
