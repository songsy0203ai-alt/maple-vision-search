# 🍁Maple-Vision-Search

**Semantic Game Scene Retrieval & Situation Analysis System using YOLOv8 and CLIP**

본 프로젝트는 메이플스토리의 방대한 게임 내 시각 데이터를 객체 탐지(Object Detection)와 비전-언어 모델(VLM)을 결합하여 사용자의 자연어 쿼리에 맞는 최적의 장면을 검색하고 분석하는 지능형 검색 엔진입니다.

## 1. Project Overview

* **배경**: 라이브 서비스 중인 게임에서 발생하는 방대한 스크린샷 및 영상 데이터에서 특정 상황(예: 특정 보스 사냥, 아이템 강화 성공 등)을 키워드만으로 검색하는 것은 기존 태깅 시스템으로는 한계가 있음.
* **목표**: YOLOv8을 통한 정밀 객체 탐지와 CLIP의 시맨틱 임베딩을 결합하여, 자연어 기반의 멀티모달 검색 파이프라인 구축.
* **핵심 가치**: 데이터 구축 비용 절감을 위한 **Synthetic Data Pipeline** 구축 및 게임 플레이 화면의 의미론적 맥락 이해를 통한 **Semantic Retrieval** 구현.

---

## 2. System Pipeline

본 시스템은 데이터 생성부터 최종 검색까지 유기적으로 연결된 4단계 파이프라인으로 구성됩니다.

1. **Data Generation**: 렌더링 좌표 기반의 합성 데이터를 생성하여 정밀한 Ground Truth 확보.
2. **Object Detection**: YOLOv8을 활용해 이미지 내 주요 객체(Player, Mob)의 위치 및 상태 탐지.
3. **Vector Indexing**: CLIP 모델로 이미지와 탐지된 메타데이터를 벡터화하여 고차원 임베딩 공간에 저장.
4. **Semantic Search**: 사용자의 자연어 질문을 벡터로 변환한 후, 코사인 유사도 기반의 근사 최근접 이웃(ANN) 검색 수행.

---

## 3. Technical Stack

| Category | Technology | Description |
| --- | --- | --- |
| **Object Detection** | **YOLOv8** | 실시간 객체 탐지 및 바운딩 박스 추출 |
| **VLM / Retrieval** | **OpenAI CLIP** | 이미지-텍스트 멀티모달 임베딩 및 유사도 비교 |
| **Vector DB** | **Faiss** | 대규모 벡터 데이터의 효율적 인덱싱 및 유사도 검색 |
| **Data Engineering** | **Python, OpenCV** | 합성 데이터 생성 및 전처리 파이프라인 구현 |
| **Framework** | **PyTorch** | 딥러닝 모델 학습 및 추론 환경 |

---

## 4. 데이터 구축 전략 (Data Construction Strategy)

### **High-Fidelity Data Generation**

* **합성 데이터 생성(Synthetic Data Generation) 파이프라인**: 게임 맵의 렌더링 데이터를 활용하여 배경 이미지 위에 캐릭터와 몬스터 객체를 합성하는 방식으로 학습 데이터를 구축함.
* **검증된 방법론 차용**: 렌더링 좌표를 기반으로 바운딩 박스를 자동 생성하는 **[MapleStoryDetectionSampleGenerator]**의 설계 방식을 채택함. 이는 수동 라벨링 과정에서 발생할 수 있는 인적 오류를 방지하고, 픽셀 단위의 정밀한 주석(Annotation)을 통해 신뢰도 높은 Ground Truth 데이터셋을 확보하기 위함임.
* **도메인 적응(Domain Adaptation)**: 합성 시 다양한 배경 이미지와 객체의 조합을 통해 실제 게임 화면에서 발생할 수 있는 가려짐(Occlusion)이나 배경 복잡도에 대응함.

---

## 5. 실무적 활용 방안 (Practical Use Cases)

### **A. 운영 및 개발 효율화 (Staff Side)**

* **QA 리포팅 자동화**: "캐릭터가 지형지물 아래로 빠진 장면" 등 특정 렌더링 이상 현상을 자연어로 검색하여 전수 조사 및 수정 작업 시간 단축.
* **이상 패턴 분석**: 비정상적인 객체 집중이나 특정 스킬 이펙트가 반복되는 로그 이미지를 시맨틱하게 분류하여 어뷰징 조사 도구로 활용.

### **B. 사용자 경험 고도화 (Player Side)**

* **지능형 하이라이트 탐색**: 저장된 수만 장의 스크린샷 중 "핑크빈 레이드 성공 순간"과 같이 기억에 의존한 묘사만으로 원하는 이미지를 즉시 탐색.
* **플레이 데이터 요약**: 사냥한 몬스터나 방문한 맵 정보를 기반으로 플레이 로그를 시각적으로 자동 분류 및 아카이빙.

---

## 6. Mathematical Foundations

검색 엔진은 사용자 쿼리 벡터 와 이미지 벡터  사이의 코사인 유사도를 기반으로 작동합니다.

---

## 7. Directory Structure

```text
maple-vision-search/
├── data/
│   ├── raw/           # 원본 에셋 데이터
│   └── processed/     # 합성된 학습용 데이터셋
├── models/            # 학습된 YOLOv8 가중치 및 CLIP 설정
├── scripts/           # 데이터 생성 및 학습 통합 스크립트
├── notebook/          # 실험 및 성능 평가 리포트
└── maple.yaml         # YOLOv8 데이터셋 구성 파일

```

