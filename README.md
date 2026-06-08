# 한성대학교 빅데이터 프로그래밍 프로젝트

배달의민족 리뷰 데이터에서 이벤트성 리뷰를 판별하고, 이벤트 리뷰 확률을 활용해 가게 별점을 정제하는 프로젝트입니다.

## 프로젝트 구조 및 흐름

```text
01data_process.ipynb
   - 배달의민족 리뷰 엑셀 데이터 통합
   - MENU 컬럼의 리뷰 이벤트 관련 표현을 기준으로 label 생성
   - 리뷰 텍스트 정제 및 메타데이터 생성

02KcBERT_extract.ipynb
   - preprocessed_reviews.csv의 cleaned_review_text 사용
   - KcBERT CLS 임베딩 768차원 추출

03PCA_Feature_Fusion.ipynb
   - PCA 차원 축소와 메타데이터 결합 구조 확인
   - final_hybrid_*.csv는 저장하지 않음

04SMOTE_5-fold_model_train.ipynb
   - SMOTE, 5-Fold CV, MLP grid 탐색
   - 프로젝트 제안 모델 저장

05baseline_compare_metadata_effect.ipynb
   - TF-IDF, metadata-only, KcBERT-only, hybrid 모델 비교
   - metadata ablation 실험 수행

06model_selection_error_analysis.ipynb
   - validation F1 기준 최종 선택 모델 확정
   - 선택 모델과 프로젝트 제안 모델을 각각 저장
   - 오답 유형과 KcBERT-only 대비 hybrid 보완 효과 분석

07rating_refinement_comparison.ipynb
   - 후보 2: 이벤트 확률 기반 가중 평균으로 별점 정제
   - 제안 모델과 선택 모델의 정제 결과 비교

08end_to_end_demo.ipynb
   - 저장된 제안 모델 기반 end-to-end 데모
   - 단일 리뷰 input() 입력과 CSV 여러 리뷰 입력 지원
```

## 실행 순서

```text
01 -> 02 -> 03 -> 04 -> 05 -> 06 -> 07 -> 08
```

핵심 산출물은 다음과 같습니다.

```text
csv/preprocessed_reviews.csv
csv/reviews_embeddings_extract.csv
outputs/proposed_mlp_final_model.joblib
outputs/final_selected_model.joblib
outputs/final_proposed_model.joblib
csv/end-to-end-test.csv
outputs/rating_refinement_*.csv
```

## End-to-End 데모

08번 코드는 End-to-End 데모 코드입니다.  
해당 코드는 `outputs/final_proposed_model.joblib`만 사용합니다.

- 단일 리뷰: `input()`으로 리뷰 내용, 사진 수, 별점을 입력해 이벤트 확률을 확인합니다.
- 여러 리뷰: `csv/end-to-end-test.csv`를 읽어 리뷰별 이벤트 확률과 후보 2 정제 별점을 계산합니다.
- 현재 데모 CSV는 실제 크롤링된 리뷰 20개와 임의 작성 리뷰 10개로 총 30개로 구성되어 있습니다.

## 환경 설정

Python 3.10 이상 사용을 권장합니다. `02KcBERT_extract.ipynb`과 `08end_to_end_demo.ipynb`는 Hugging Face에서 KcBERT 모델을 내려받기 때문에 최초 실행 시 인터넷 연결이 필요합니다. 필요한 패키지는 `requirements.txt`에 정리되어 있습니다.

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

`csv/end-to-end-test.csv`만 데모 입력 파일로 Git에 포함하고, 나머지 `*.csv`, `*.joblib`, `custom/`은 `.gitignore`에 포함되어 기본적으로 추적되지 않습니다. 새 환경에서는 실행 순서대로 노트북을 실행해 CSV/joblib 산출물을 재생성해야 합니다.
