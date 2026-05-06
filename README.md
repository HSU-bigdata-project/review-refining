# 한성대학교 빅데이터 프로그래밍 프로젝트

배달의민족 리뷰 데이터를 전처리하고, KcBERT 기반 임베딩과 PCA feature fusion을 거쳐 MLP 모델로 감성/리뷰 분류 실험을 진행하는 프로젝트입니다.

## 프로젝트 구조

```text
.
├── 01data-process.ipynb          # 리뷰 데이터 정제 및 전처리
├── 02KcBERT_extract.ipynb        # KcBERT 임베딩 추출
├── 03PCA_Feature Fusion.ipynb    # PCA 기반 feature fusion
├── 04MLP.ipynb                   # MLP 모델 학습 및 평가
├── KcBERT_extract-original.ipynb # KcBERT 임베딩 추출 원본 노트북
├── MLP_balanced.ipynb            # balanced data 기반 MLP 실험
├── reviews/                      # 원본 리뷰 엑셀 데이터
└── 7_빅데이터프로그래밍_수행계획서_오남.pdf # 기존 수행 계획서
```

## 실행 순서

1. `01data-process.ipynb`에서 원본 리뷰 데이터를 정제합니다.
2. `02KcBERT_extract.ipynb`에서 리뷰 텍스트 임베딩을 추출합니다.
3. `03PCA_Feature Fusion.ipynb`에서 PCA 기반 feature를 결합합니다.
4. `04MLP.ipynb` 또는 `MLP_balanced.ipynb`에서 모델을 학습하고 성능을 확인합니다.

## 데이터

원본 리뷰 데이터는 `reviews/` 디렉터리에 있습니다.
전처리 결과, 임베딩 CSV, 학습/검증/테스트 분할 파일은 노트북 실행 과정에서 생성되는 산출물입니다.

## Git 관리
`.gitignore`에는 캐시, 가상환경, 모델 체크포인트, 대용량 생성 산출물을 제외하도록 설정되어 있습니다.
