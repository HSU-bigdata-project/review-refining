# 한성대학교 빅데이터 프로그래밍 프로젝트

배달의민족 리뷰 데이터에서 리뷰 이벤트 참여 여부를 판별하고, 모델 예측 확률을 활용해 별점을 정제하는 것을 목표로 하는 프로젝트입니다.

## 로드맵

```text
1. 데이터 수집 및 라벨링
   - 배달의민족 리뷰 엑셀 데이터 통합
   - MENU 컬럼의 리뷰 이벤트 관련 표현을 기준으로 이벤트 리뷰 label 생성

2. 데이터 전처리
   - 리뷰 원문 정리
   - KcBERT 입력용 cleaned_review_text 생성
   - rating, text_length, emoji_count, photo_count 등 메타데이터 생성

3. KcBERT 특징 추출
   - beomi/kcbert-base를 특징 추출기로 사용
   - 리뷰 텍스트별 768차원 CLS 임베딩 생성

4. PCA 차원 축소 및 Feature Fusion
   - train 데이터 기준으로 PCA fit
   - 누적 설명 분산 90% 수준으로 임베딩 차원 축소
   - 정규화한 메타데이터와 PCA 임베딩 결합
   - train / validation / test를 70% / 15% / 15%로 분할

5. 5-Fold 교차 검증 및 SMOTE
   - train 데이터 안에서 5-Fold 구성
   - 각 fold의 학습 데이터에만 SMOTE 적용
   - 검증 fold, validation, test에는 SMOTE 미적용

6. 모델 학습 및 평가
   - 베이스라인: TF-IDF + Random Forest 등
   - 비교 실험: 메타데이터 only, KcBERT only, hybrid feature
   - 제안 모델: Scikit-learn MLP Classifier
   - 주요 지표: F1-Score, PR-AUC

7. 별점 정제
   - 모델이 예측한 이벤트 리뷰 확률을 기반으로 별점 가중 평균 산출
```

## 프로젝트 구조

```text
.
├── 01data-process.ipynb            # 원본 엑셀 통합, 라벨링, 텍스트/메타데이터 전처리
├── 02KcBERT_extract.ipynb          # KcBERT CLS 임베딩 추출
├── 03PCA_Feature Fusion.ipynb      # PCA 차원 축소, 메타데이터 정규화, feature fusion
├── 04SMOTE_5Fold.ipynb             # 5-Fold 분할 및 fold별 train SMOTE 적용 준비
├── custom/
│   └── run_train_mlp_runpod.ipynb  # 외부 GPU/RunPod 실험용 노트북
├── csv/
│   ├── preprocessed_reviews.csv
│   ├── reviews_embeddings_extract.csv
│   ├── final_hybrid_train.csv
│   ├── final_hybrid_val.csv
│   └── final_hybrid_test.csv
├── reviews/                        # 원본 리뷰 엑셀 데이터
├── requirements.txt
├── 7_빅데이터프로그래밍_수행계획서_오남.pdf
└── 7분반_중간발표_오남.pdf
```

## 실행 순서

노트북은 아래 순서대로 실행합니다.

```text
01data-process.ipynb
-> 02KcBERT_extract.ipynb
-> 03PCA_Feature Fusion.ipynb
-> 04SMOTE_5Fold.ipynb
```

산출물 흐름은 다음과 같습니다.

```text
reviews/*.xlsx
-> csv/preprocessed_reviews.csv
-> csv/reviews_embeddings_extract.csv
-> csv/final_hybrid_train.csv
-> csv/final_hybrid_val.csv
-> csv/final_hybrid_test.csv
```

`04SMOTE_5Fold.ipynb`는 베이스라인 모델 설정 전 단계까지만 포함합니다. 5-Fold로 나눈 각 학습 fold에만 SMOTE를 적용하고, 결과는 노트북 메모리의 `fold_datasets` 변수에 저장됩니다.

## 환경 설정

Python 3.10 이상 사용을 권장합니다. `02KcBERT_extract.ipynb`는 Hugging Face에서 KcBERT 모델을 내려받기 때문에 최초 실행 시 인터넷 연결이 필요합니다. 필요한 패키지는 `requirements.txt`에 정리되어 있습니다.

### macOS: uv 사용

```bash
cd /Users/hyunseop/Developer/uni/big-data
uv python install 3.11
uv add -r project/requirements.txt
uv run jupyter lab project
```

### Windows: pip 사용

Git Bash 또는 Bash 터미널 기준입니다.

```bash
cd path/to/big-data/project
python -m pip install -r requirements.txt
jupyter lab
```

노트북 셀에서 아래 명령으로 현재 커널에 패키지를 설치할 수 있습니다.

```python
%pip install -r requirements.txt
```

설치 후에는 커널을 재시작해야 새 패키지가 반영됩니다.
