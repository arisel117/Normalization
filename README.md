

* * *
# 주요 Normalization 소개

</br>

## 정규화란?
- 데이터셋의 Numerical Value 범위 차이를 **왜곡하지 않고**, **공통 척도로 변경**하는 일련의 프로세스

## 정규화 / 표준화 방법론
- **Min-Max Normalization**
  - 데이터의 값의 **범위를 (0 ~ 1) 혹은 (-1 ~ 1)의 범위로 조정**하는 방법
  - **Outlier(이상치)에 영향을 많이 받는 문제**가 있음

- **Z-Score Standardization**
  - 데이터의 값을 **분포가 평균이 0, 편차가 1이 되도록 조정**하는 방법
  - **Outlier(이상치)에 영향을 상대적으로 적게 받음**, 값의 범위가 제한되지 않음

- **L1 Regularization (Lasso Regularization)**
  - 실제 값과 예측 값 사이의 오차들의 절댓값 합
  - 맨허튼 거리, 택시 거리로 많이 알려져 있으며, 택시가 도시의 블록 사이를 이동해 다른 지점으로 이동하는 것과 같음
  - 두 벡터 간의 최단 거리를 찾는데 사용되나, 블록 사이로 이동하기 때문에 경로는 다양하게 나타날 수 있음
  - 다양한 경로가 존재 할 수 있기 때문에, 중요한 가중치만 남길 수 있는 **Feature Selection이 가능**함
  - 비 중요 변수를 우선적으로 줄이기 때문에, 변수간 상관 관계가 높으면 성능이 하락함
  - L2 대비 Outlier에 더 Robust하여, **Sparse Model에 적합함**
  - 0에서 미분이 불가능하므로, **Gradient-Based Learning시 사용에 주의가 필요**함

- **L2 Regularization (Ridge Regularization, Euclidean Norm)**
  - 실제 값과 예측 값 오차들의 제곱 합
  - 두 벡터를 직선으로 연결한 최단 거리로 단 하나만 존재 할 수 있음
  - 다양한 경로가 존재 할 수 없기 때문에, Feature Selection이 불가능함
  - 크기가 큰 변수를 우선적으로 줄이기 때문에, **변수간 상관 관계가 높아도 성능이 우수**함
  - Weight Decay를 통해 가중치를 전반적으로 작게 만들어 학습 효과가 L1 대비 우수함

* * *

</br>


* * *
[<img src="https://github.com/arisel117/Normalization/blob/main/Norm.PNG?raw=true">](https://arxiv.org/abs/1803.08494)
* 이 때, N은 Batch Size, C는 Channel Size, (H, W)는 Spatial Size를 의미함, 각 방법론은 파란색 부분 만큼을 묶어서 연산하게됨

## 배치 정규화 (Batch Normalization)
- 학습 과정에서 데이터가 신경망을 타고 흐르면서 레이어와 레이어 사이의 데이터 분포가 달라지는 현상이 발생함
  특히, **Batch 단위로 다른 분포를 가지는 데이터를 일정한 평균과 분산을 가지도록 정규화**하여 안정화시키는 방법
  이는 심층 신경망에서 가중치의 미세한 변화가 중첩되어 레이어의 깊이가 깊어질수록 그 변화가 더욱 커지기 때문으로, **Internal Covariate Shift가 발생**하게됨
- 추론 과정에서는 Batch 단위로 평균과 분산을 구하기가 어려우므로, 학습 단계에서 사용된 적절한 평균과 분산을 저장하고, 추론 시에 이를 사용함
- 주요 장점
  - 학습 속도 향상
  - **초기화 민감도 감소**
  - **일반화 성능 향상**
- 주요 단점
  - **학습시 Batch Size에 매우 민감함**
    - Batch Size가 너무 작으면 통계치가 불안정해질 수 있고, 너무 크면 메모리 사용량이 증가함
  - **RNN(LSTM)에 적용하기 어려워** 이 경우에는 Layer Normalization, Instance Normalization 등이 더 적합함
  - 학습/추론시 연산 비용이 증가
    - 특히, 분산 학습(Distributed Learning)에서 각 노드 간의 동기화가 필요하여, 분산 시스템에서는 추가적인 복잡성을 유발 할 수 있음
    - 모델의 구조가 복잡해지며, 이는 모델의 파라미터 수를 증가시키고, 최적화 과정에 추가적인 부담을 줄 수 있음

## Instance Normalization
- (1 Batch 단위의) **각 입력 인스턴스(데이터) 별로 정규화**를 진행
- 주로 이미지 스타일 변환 모델에 사용됨
- 각 인스턴스를 독립적으로 정규화하므로, Batch Size에 영향을 받지 않으나, Batch 내 다른 데이터와의 차이가 제거 될 수 있음
  
## Layer Normalization
- **데이터 포인트의 모든 피쳐를 대상으로 정규화**를 진행 (동일한 층의 뉴런간 정규화)
- 주로 RNN, Transformer에서 사용됨
- 입력 데이터 Size 및 Batch Size에 영향을 받지 않음
- CNN과 같은 모델에서는 성능이 떨어질 수 있음

## [Group Normalization](https://arxiv.org/abs/1803.08494)
- **Channel Axis에서 일정 Group Size로 나누어 정규화**를 진행
- Instance Normalization과 Layer Normalization을 적절히 섞어 Computer Vision 분야에서 보다 우수한 성능을 보임
  - Group Size == 1 이면, Group Normalization == Layer Normalization이 됨
  - Group Size == Channel Size 이면, Group Normalization == Instance Normalization이 됨
- Batch Size에 영향을 받지 않음

## [Weight Normalization](https://arxiv.org/abs/1602.07868)
- Feature Activation을 정규화하는 것이 아니라, 레이어의 **Weight를 정규화**하는 방법
- 평균을 0으로 보장해주지 못하기 떄문에, Mean-Only Batch Normalization과 함께 사용할 것이 권장됨
- 관련 연구들
  - [스펙트럼 정규화 (Spectral Normalization)](https://arxiv.org/abs/1802.05957) 
    - GAN 학습시 Discriminator의 학습을 안정화고, Lipschitz Constant의 Tuning이 없이도 잘 작동하도록 도와주는 Weight Normalization 기법
  - [Adaptive Instance Normalization (AdaIN)](https://arxiv.org/abs/1703.06868)
    - content 와 style input 이 주어졌을 때, 두 입력의 평균과 분산을 style input과 동일하도록 조절하는 방법

## [Weight Standardization](https://arxiv.org/abs/1903.10520)
- Feature Activation을 정규화하는 것이 아니라, 레이어의 **가중치(Conv Filter)를 대상으로 정규화**하는 방법
- Group Normalization이 작은 Batch Size에서 유용하지만, 큰 Batch Size에선 Batch Normalization의 성능에 미치지 못함
- 큰 Batch Size에서 Mini-Batch Dependency를 완전히 제거하면서, 큰 Batch Size를 가지는 환경에서 Batch Normalization 보다 우수한 성능을 보임
- Residual Networks 구조에서 효과적임

## [Layer-wise Adaptive Rate Scaling (LARS)](https://arxiv.org/abs/1708.03888)
- LR이 클때 학습 안정성을 분석하기 위해 Weights와 Gradient Update의 Normalization 비율을 측정해 작으면 학습이 불안정하고, 크면 학습이 이루어지지 않음을 발견함
  Weight가 아닌 Layer마다 LR을 다르게 사용하고, Update되는 정도가 Weight Norm에 따라 달라지도록 하는 방법
- 큰 Batch Size를 사용하면 작은 Batch Size를 사용 할 때보다 Epoch당 Iteration이 적어지게 되고, 이를 보완하기 위해 더 큰 LR(Learning Rate)를 사용하게 됨
  그러나, 큰 LR을 이용하면 학습이 어려워지고, 초기 단계에서 모델이 발산(Gradient Exploding)하는 문제가 생길 수 있음
  그래서 초기에는 작은 LR을 사용하고, 몇 Epoch동안에 Linear Scaling하는 Linear Scaling LR with Warm-up 방법이 사용되었으나, 여전히 Batch Size가 크면 높은 LR 때문에 학습이 제대로 이루어지지 않는 문제가 있음
- 기존 방식들은 100 Epoch을 기준으로 학습시키면 어떤 방법을 사용해도 Baseline에 미치지 못했지만, Epoch을 늘리면 Baseline의 성능과 거의 유사해짐
  **큰 배치를 사용하면 전체 데이터셋의 True Gradient와 유사해지므로 추가적인 이득이 있을것 같지만, 작은 배치를 사용했을때와 큰 차이가 없음**


