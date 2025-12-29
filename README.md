# [Maple-Vision-Search]

**Semantic Game Scene Retrieval & Situation Analysis System using YOLOv8 and CLIP**

본 프로젝트는 메이플스토리의 방대한 게임 내 시각 데이터를 객체 탐지(Object Detection)와 비전-언어 모델(VLM)을 결합하여 사용자의 자연어 쿼리에 맞는 최적의 장면을 검색하고 분석하는 지능형 검색 엔진입니다.

## 1. Project Overview

* **배경**: 라이브 서비스 중인 게임에서 발생하는 방대한 스크린샷 및 영상 데이터에서 특정 상황(예: 특정 보스 사냥, 아이템 강화 성공 등)을 키워드만으로 검색하는 것은 기존 태깅 시스템으로는 한계가 있음.
* **목표**: YOLOv8을 통한 정밀 객체 탐지와 CLIP의 시맨틱 임베딩을 결합하여, 자연어 기반의 멀티모달 검색 파이프라인 구축.
* **핵심 가치**: 데이터 구축 비용 절감을 위한 **Synthetic Data Pipeline** 구축 및 의미론적 맥락 이해를 통한 **Semantic Retrieval** 구현.

## 2. Tech Stack

| Category | Technology Stack |
| --- | --- |
| **Object Detection** | **YOLOv8** (Ultralytics) |
| **VLM / Retrieval** | **OpenAI CLIP**, **Faiss** (Vector DB) |
| **LLM (Captioning)** | **GPT-4o-mini** (for Metadata Augmentation) |
| **Data Engineering** | **WzComparerR2** (Asset Extraction), Python OpenCV |
| **Environment** | Conda (Python 3.10), PyTorch, CUDA |

## 3. System Architecture

본 시스템은 총 4단계의 파이프라인으로 구성됩니다.

1. **High-Fidelity Data Generation**:
* 메이플스토리 `.wz` 리소스를 직접 추출하여 배경과 객체(캐릭터, 몬스터, UI)를 합성.
* 자동 라벨링을 통해 100% 정확도의 Ground Truth 데이터셋 확보.


2. **Object Detection (YOLOv8)**:
* 게임 화면 내 주요 객체 식별 및 위치 좌표() 추출.


3. **Semantic Indexing**:
* 추출된 메타데이터를 LLM을 통해 자연어 설명으로 변환.
* CLIP 모델을 사용하여 이미지와 텍스트 설명을 동일한 벡터 공간으로 임베딩.


4. **Vector Search & Retrieval**:
* 사용자 자연어 쿼리와 데이터베이스 내 이미지 벡터 간의 **Cosine Similarity** 계산.
* 



## 4. Key Features

* **Domain-Specific Detection**: 캐릭터 동작(공격, 대기, 점프) 및 특정 몬스터(핑크빈 등)에 특화된 탐지 모델.
* **Zero-shot Semantic Search**: 학습 데이터에 명시되지 않은 표현(예: "긴박한 보스전")도 CLIP의 사전 학습 지식을 활용해 의미적으로 유사한 장면 검색 가능.
* **Situation Analysis**: 단순 객체 존재 여부를 넘어 보스 체력, 강화 성공 이펙트 등 UI 상황을 종합하여 게임 내 사건 분석.

## 5. Repository Structure

```text
maple-vision-search/
├── data/
│   ├── raw/           # WzComparerR2로 추출한 원본 리소스 (.gitkeep)
│   └── processed/     # 합성 데이터 및 YOLO 라벨 (.gitignore)
├── models/            # YOLOv8 가중치 (.pt) 및 CLIP 모델
├── scripts/           # 데이터 생성, 학습, 검색 통합 스크립트
├── notebook/          # 실험 및 시각화 결과물 (Jupyter Notebook)
└── maple.yaml         # YOLOv8 Dataset Configuration
