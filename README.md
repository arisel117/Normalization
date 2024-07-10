

* * *
# 주요 Normalization 소개

</br>

## 정규화란?
- 데이터셋의 Numerical Value 범위 차이를 왜곡하지 않고, 공통 척도로 변경하는 일련의 프로세스

## 정규화 / 표준화 방법론
- Min-Max Normalization
  - 데이터의 값의 범위를 (0 ~ 1) 혹은 (-1 ~ 1)의 범위로 조정하는 방법
  - Outlier(이상치)에 영향을 많이 받는 문제가 있음

- Z-Score Standardization
  - 데이터의 값을 분포가 평균이 0, 편차가 1이 되도록 조정하는 방법
  - Outlier(이상치)에 영향을 상대적으로 적게 받음, 값의 범위가 제한되지 않음

- L1 Regularization (Lasso Regularization)
  - 실제 값과 예측 값 사이의 오차들의 절댓값 합
  - 맨허튼 거리, 택시 거리로 많이 알려져 있으며, 택시가 도시의 블록 사이를 이동해 다른 지점으로 이동하는 것과 같음
  - 두 벡터 간의 최단 거리를 찾는데 사용되나, 블록 사이로 이동하기 때문에 경로는 다양하게 나타날 수 있음
  - 다양한 경로가 존재 할 수 있기 때문에, 중요한 가중치만 남길 수 있는 Feature Selection이 가능함
  - 비 중요 변수를 우선적으로 줄이기 때문에, 변수간 상관 관계가 높으면 성능이 하락함
  - L2 대비 Outlier에 더 Robust하여, Sparse Model에 적합함
  - 0에서 미분이 불가능하므로, Gradient-Based Learning시 사용에 주의가 필요함

- L2 Regularization (Ridge Regularization, Euclidean Norm)
  - 실제 값과 예측 값 오차들의 제곱 합
  - 두 벡터를 직선으로 연결한 최단 거리로 단 하나만 존재 할 수 있음
  - 다양한 경로가 존재 할 수 없기 때문에, Feature Selection이 불가능함
  - 크기가 큰 변수를 우선적으로 줄이기 때문에, 변수간 상관 관계가 높아도 성능이 우수함
  - Weight Decay를 통해 가중치를 전반적으로 작게 만들어 학습 효과가 L1 대비 우수함

* * *

</br>


* * *

## 배치 정규화 소개
- 이에 대해서는 자세한 추가 설명 필요!!





