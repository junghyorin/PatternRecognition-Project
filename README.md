# PatternRecognition-Project
2025/ 패턴인식 프로젝트

## 1. Models

본 프로젝트에서는 분류 성능 비교 및 앙상블을 위해 다음 머신러닝 모델을 사용하였다.

* Random Forest
* XGBoost
* Logistic Regression

데이터는 **Stratified Split**을 적용하여 학습/테스트 세트로 분할하였으며, 이는 클래스 불균형 상황에서도 분포를 유지하기 위함이다.

---

### Random Forest

* 다수의 결정트리를 결합한 앙상블 모델
* Bagging 기반으로 과적합과 분산을 감소시킴
* 주요 하이퍼파라미터: `n_estimators`, `max_depth`
* **Optuna**를 사용하여 효율적으로 하이퍼파라미터 탐색 수행

---

### XGBoost

* Gradient Boosting 기반의 트리 앙상블 모델
* 정규화, 병렬 처리, 결측치 처리 등으로 성능과 속도 개선
* 주요 하이퍼파라미터: `max_depth`, `learning_rate`, `subsample`, `colsample_bytree`, `gamma`, `reg_alpha`, `reg_lambda`, `n_estimators`
* Optuna를 통해 최적 파라미터 탐색 후 최종 모델 학습

---

### Logistic Regression

* 앙상블(스태킹)에서 최종 분류기로 사용
* 다른 모델들의 예측을 선형 결합하여 안정적이고 해석 가능한 결과 제공
* 확률 출력이 가능하여 의사결정에 유리함

---

## 2. Dataset

### train_final.csv

* `id`, `shares` 컬럼 제거
* 수치형 변수는 0, 평균 또는 중앙값으로 결측치 처리
* 범주형 변수(`weekday`, `data_channel`)는 One-Hot Encoding 적용
* Boolean 컬럼은 0/1 정수형으로 변환

### train4.csv

* `id`, `shares` 컬럼 제거
* 수치형 변수는 평균 또는 중앙값으로 결측치 처리
* 범주형 변수는 `.cat.codes`로 인코딩
* 수치형 변수에 대해 이상치 클리핑 적용

---

## Final Model

* **XGBoost + Feature Selection**을 최종 모델로 사용
* Stratified K-Fold Cross Validation 적용
* StandardScaler로 feature scaling 수행
* XGBoost feature importance 기준 상위 25개 feature 선택
* 최적 파라미터 및 평균 best iteration으로 전체 학습 데이터 재학습

본 접근은 과적합을 방지하면서도 안정적인 일반화 성능을 확보하는 것을 목표로 한다.

