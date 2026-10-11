<div align="center">

![박창후 게임 클라이언트 개발자](./assets/header.svg)

[![C++](https://img.shields.io/badge/C%2B%2B-00599C?style=flat-square&logo=cplusplus&logoColor=white)](#기술-역량) [![Unreal Engine](https://img.shields.io/badge/Unreal_Engine-0E1128?style=flat-square&logo=unrealengine&logoColor=white)](#기술-역량) [![C#](https://img.shields.io/badge/C%23-436B50?style=flat-square)](#기술-역량) [![Unity](https://img.shields.io/badge/Unity-333333?style=flat-square&logo=unity&logoColor=white)](#기술-역량)

[이메일](mailto:pencil0303@naver.com) · [개발 블로그](https://hoo2001.tistory.com/) · [시연 영상](https://www.youtube.com/@changhoo01/featured)

</div>

## 소개

안녕하세요. **Unreal Engine과 C++를 중심으로 게임 클라이언트를 개발하는 박창후**입니다.

게임의 규칙과 데이터가 플레이어의 화면에 어떻게 전달되는지 고민하며 개발합니다. TFT 모작에서는 전용 서버 기반 게임 흐름과 상태 동기화를, 명조 모작에서는 공통 능력치 구조와 전투 UI를, Minecraft 모작에서는 데이터 기반 블록 관리와 인벤토리를 구현했습니다.

SLZ 디지털 트윈스 팀에서 Unreal Engine 시각화 기능을 개발한 경험이 있습니다. 문제가 생기면 객체의 역할과 데이터 갱신 흐름을 따라 원인을 찾고, 수정 과정과 결과를 블로그와 시연 영상으로 기록합니다.

## 대표 프로젝트

이미지를 클릭하면 시연 영상으로 이동합니다. 아래 내용은 각 프로젝트에서 **제가 담당한 작업**입니다.

<table>
  <tr>
    <td width="33%" align="center" valign="top">
      <a href="https://www.youtube.com/watch?v=5CpVqWKCEx4&t=985s"><img src="./assets/tft-ltf.png" width="260" alt="TFT 모작 LTF의 천사·악마 룰렛 결과 화면"></a>
      <br><b>TFT 모작 · LTF</b>
      <br><sub>UE 5.6 · C++ · 5인 팀</sub>
      <br><sub>2026.06.30 — 2026.07.13</sub>
      <br><br>전용 서버 게임 루프<br>로비 전환 · 룰렛 · HUD
      <br><br><b>포텐업 최종 프로젝트 최우수상</b>
      <br><br><a href="https://hoo2001.tistory.com/62">개발 기록</a> · <a href="https://oasis-alloy-d6e.notion.site/LTF-3b26d02f3953803689bae6cf72957a61?pvs=73">회고록</a>
    </td>
    <td width="33%" align="center" valign="top">
      <a href="https://www.youtube.com/watch?v=9skfkhyv1f4&t=106s"><img src="./assets/myeongjo.png" width="260" alt="명조 모작의 보스전과 전투 UI 화면"></a>
      <br><b>명조 모작</b>
      <br><sub>UE 5.6 · C++ · 3인 팀</sub>
      <br><sub>2026.05.06 — 2026.05.27</sub>
      <br><br>Stat Component · 전투 UI<br>보스전 연출 · 월드 구성
      <br><br><a href="https://github.com/pch2001/Myeongjo">저장소</a> · <a href="https://hoo2001.tistory.com/61">개발 기록</a>
    </td>
    <td width="33%" align="center" valign="top">
      <a href="https://www.youtube.com/watch?v=8Dv6HJHVmgk"><img src="./assets/minecraft.png" width="260" alt="Minecraft 모작의 인벤토리와 핫바 화면"></a>
      <br><b>Minecraft 모작</b>
      <br><sub>UE 5.6 · Blueprint · 개인</sub>
      <br><sub>2026.03.24 — 2026.04.06</sub>
      <br><br>ISM 기반 블록 관리<br>DataTable · 인벤토리 · 핫바
      <br><br><a href="https://github.com/pch2001/Minecraft">저장소</a> · <a href="https://hoo2001.tistory.com/47">개발 기록</a>
    </td>
  </tr>
</table>

### TFT 모작 · 서버 상태와 화면 표현 연결

- **게임 흐름**: GameMode에서 라운드를 진행하고, GameState에 공유 상태를, PlayerState에 플레이어별 데이터를 배치했습니다. 라운드 종류와 진행 시간은 DataTable로 관리했습니다.
- **레벨 전환과 동기화**: ServerTravel과 Seamless Travel을 연결하고, `CopyProperties()`·`OverrideWith()`로 로비의 이름·아이콘·보드 정보를 유지했습니다. 챔피언 생성 요청과 이동 상태는 Server RPC·Replication으로 전달했습니다.
- **룰렛과 HUD**: 천사·악마·꽝 룰렛을 기획·개발했습니다. 결과는 서버에서 결정하고 클라이언트에서 연출했습니다. `ELTFHUDMode`와 `ApplyHUDMode()`로 화면 전환 규칙을 모았습니다.
- **문제 해결**: 두 시스템이 동시에 위치를 갱신해 발생한 기물 떨림을 분석하고, 위치 갱신을 한곳으로 통합했습니다.

### 명조 모작 · 공통 능력치 구조와 전투 연출

- **컴포넌트 구조 개선**: 부모·자식 클래스에서 Stat Component가 중복 생성되는 문제를 `SetDefaultSubobjectClass`로 수정했습니다. 생성 책임을 부모에 유지하고 캐릭터마다 하나의 Stat을 참조하도록 정리했습니다.
- **전투 피드백**: 능력치와 게이지를 UMG에 연결하고, 피해량 표시와 락온 대상 강조를 구현했습니다. Level Sequence·Animation Blueprint·Widget Blueprint로 보스전과 시네마틱을 연결했습니다.
- **월드 구성**: World Partition으로 월드를 셀 단위로 관리하고, PCG의 거리·LOD 기준을 조정해 식생의 생성·표시 범위를 관리했습니다.

### Minecraft 모작 · 데이터 기반 블록과 인벤토리

- **블록 관리**: 개별 블록 Actor를 유형별 ISM으로 통합했습니다. WorldManager에서 블록 데이터와 인스턴스 인덱스를 관리해 설치·파괴·선택 기능을 유지했습니다.
- **데이터 분리**: CSV·DataTable의 `RowName`을 공통 식별자로 사용했습니다. 블록·아이템의 정의와 월드에서 변하는 상태를 나눠, 데이터 행과 에셋을 추가하는 방식으로 콘텐츠를 확장하도록 구성했습니다.
- **상태와 UI 갱신**: Per Instance Custom Data로 파괴 진행률과 조준 강조를 표시했습니다. 인벤토리 슬롯 데이터가 바뀌면 Dispatcher로 인벤토리와 핫바를 함께 갱신했습니다.

## 프로젝트 이력

### 🎮 2026 · Unreal Engine과 C++ 시스템 개발

- **TFT 모작 · LTF** — 전용 서버 게임 루프, 로비 전환, 룰렛, HUD 구현 · 최우수상
- [**명조 모작**](https://github.com/pch2001/Myeongjo) — 능력치 컴포넌트, 전투 UI, 시퀀스, 월드 구성
- [**Minecraft 모작**](https://github.com/pch2001/Minecraft) — Blueprint 기반 블록 조작, ISM, 인벤토리
- [**이스라엘 디펜스 · C++ 콘솔 게임**](https://www.youtube.com/watch?v=bPuzSlUIzpk) — A* 경로 탐색, 충돌·피격 처리, 라운드와 상점 시스템 · 2026.02 / 2026.03
- [**C++ 콘솔 엔진 · Hoo_ConsoleEngine**](https://github.com/pch2001/Hoo_ConsoleEngine) — 컴포넌트 기반 물리, 충돌 레이어, 레벨 관리 구현

### 🥽 2025 · 멀티플레이와 직무 경험

- [**인생게임 · 캡스톤 디자인**](https://github.com/Jinmiru/sw-capstone-design) — UE 5.4 · 3인 개발팀 · 2025.02 — 2025.06<br>MetaHuman 성장 단계별 외형 변화, 직업 5종 스킬, 전용 서버와 HUD 구현 · SW 캡스톤 디자인 경진대회 동상 · [시연 영상](https://www.youtube.com/watch?v=R9aDqsHigTw&t=26s)
- [**MaCook · 메이플스토리 직무 멘토링**](https://github.com/pch2001/MaCook) — 2025.08.22 — 2025.08.25<br>메이플스토리 IP의 글로벌 진출을 주제로 Unity 프로토타입 개발

### 🌱 2024 — 2025 · 게임 개발과 팀 운영

- [**BreathLine · 창업동아리 GR팀**](https://github.com/pch2001/BreathLine) — Unity · 8인 팀 · 2024.03 — 2025.09<br>사회적 메시지를 담은 2D 기능성 게임의 플레이어 스크립트·최종 보스·시나리오 구조 개발. 팀 대표로 주간 회의, 일정 관리, 캐릭터 아트 외주 조율과 전시·공모전 참여 진행 · [시연 영상](https://www.youtube.com/watch?v=r_FlLbikoHo)

## 경력과 교육

| 기간 | 소속 | 경험 |
| --- | --- | --- |
| 2026.01.19 — 2026.07.16 | **원티드 포텐업 · 게임 개발자 양성과정 4th** | 960시간 교육 수료. C++ 자체 엔진 제작과 UE 5.6 프로젝트 3개 진행 |
| 2020.03 — 2026.03 | **한림대학교 · 콘텐츠IT학과** | 학사 · VR/AR/게임 개발 트랙 · 학점 3.93 / 4.5 |
| 2025.09 — 2025.12 | **SLZ · 디지털 트윈스 팀** | Unreal Engine 개발자로 날씨 인포그래픽, AI 자동 라벨링, 3D 경로 편집과 실시간 시각화 기능 개발 |
| 2025.08.22 — 2025.08.25 | **넥슨코리아 · 메이플스토리 직무 멘토링** | 현직자 직무 교육, 게임잼형 프로젝트와 피드백 |


## 수상과 활동

- **원티드 포텐업 최종 프로젝트 대회 · 최우수상** — TFT 모작 LTF · 2026
- **SW 캡스톤 디자인 경진대회 · 동상** — 인생게임 · 한림대학교 · 2025.06.05
- **AI 활용 공모전 · 장려상** — 한림대학교 · 2025.05.20
- **SW 코딩테스트 · 2등** — 한림대학교 · 2024.11.27
- **우수동아리 전시회 · 동상** — MonsterHunter 기획·개발 및 전시 · 한림대학교 · 2024.11.07

## 기술 역량

| 분야 | 프로젝트에서 사용한 기술 |
| --- | --- |
| 언어와 엔진 | C++ · C# · Unreal Engine 5.4 / 5.6 · Blueprint · Unity |
| 멀티플레이 | 전용 서버 · Server / Client RPC · Multicast · Replication · Seamless Travel |
| 게임 구조와 데이터 | GameMode · GameState · PlayerState · Actor Component · CSV / DataTable |
| UI와 연출 | UMG · Widget Blueprint · Level Sequence · Animation Blueprint · 머티리얼 |
| 월드와 블록 관리 | World Partition · PCG · ISM · Per Instance Custom Data |
| 협업과 기록 | Git / GitHub · 팀 일정 관리 · 직군 간 협업 · 개발 블로그 · 시연 영상 |

---

개발 과정과 문제 해결 기록은 [블로그](https://hoo2001.tistory.com/)에, 프로젝트 실행 결과는 [YouTube](https://www.youtube.com/@changhoo01/featured)에 정리하고 있습니다.
