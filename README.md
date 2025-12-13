# Computer_vision_term_project
team2: 김민솔, 정은수, 한송희

----
## 주제: Detect Ai vs. Human generated images
### 목표: 인공지능이 생성한 이미지와 인간이 생성한 이미지를 분류하는 모델 개발
이를 통해 fake image로 인한 사회적 피해를 줄이는 것이 목표


### 데이터셋: Kaggle의 Ai vs. human-generated images competition의 데이터를 사용
- 파일 경로를 나타내는 "file_name"과 ai/human-generated를 구분하는 "label"로 구성
- 최슨 SOTA 생성 모델을 통해 만들어진 Ai image를 사용
- 다양한 카테고리 이미지로 구성(1/3은 humans가 포함된 이미지)
- train: 79,950장/ test: 19,986장

### 최종 모델 설명: DCT를 사용해 주파수를 변환한 이미지와 기본 RGB이미지를 받는 CNN을 결합한 모델
- **data augmentation**: 훈련 데이터셋에 이미지 증강 및 ai 아티팩트 변환을 적용하여 모델의 일반화 성능을 강화함
  Resize, RandomResizedCrop, RandomHorizontalFlip, RandomAutocontrast, RandomAdjustSharpness, GaussianBlur, ColorJitter, Salt and pepper noise, jpeg compression, gasuuian noise 등을 사용
- **domain separation**: 기존 RGB이미지에서 DCT(Discrete Cosine Transform)을 사용해 주파수 특징 tensor를 추출하여 각각을 모델에 입력
- **RGB Feature Extractor**: 사전훈련된 ConvNeXt Base모델로 전이학습
- **DeeperFrequencyCNN**: DCT를 통해 얻어진 주파수 도메인 특징을 입력받아 ai생성 이미지 특유의 frequency를 탐지하도록 CNN을 정의
- **Ensemble model**: 두 CNN이 반환한 128차원 특징 vector를 결합
- **Optimizer**: AdamW optimizer, weight decay(과적합 방지)
- **loss function**: label smoothing을 이용한 cross entropy loss


### 결과
private score: 0.77139
public score: 0.75963

### live demo
실제 사기에 사용된 ai 이미지와 실제 사진을 입력하여 ai/human을 구분하도록하는 프로그램
<img width="1202" height="1132" alt="image" src="https://github.com/user-attachments/assets/ab17c971-441a-443e-9340-03972834b8ac" />

