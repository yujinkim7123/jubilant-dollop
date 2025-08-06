### 색맹증 색인식 프로그램 결과 보고서

[cite_start]본 보고서는 **LG 부트캠프 07기 03반 8팀**에서 수행한 **색맹증 색인식 프로그램** 프로젝트의 결과를 요약합니다[cite: 1, 2, 4, 7].

---

### 1. 프로젝트 개요 및 목표

#### 1.1. 주제 및 결과 요약
* [cite_start]**주제**: 색맹증 색인식 프로그램[cite: 2].
* [cite_start]**목표**: 실시간 이미지 분할(Real-time segmentation)을 통해 색맹 환자에게 정교한 보정 필터 알고리즘을 적용하는 것입니다[cite: 21].
* [cite_start]**결과**: 이미지 분할(Image Segmentation)을 활용한 맞춤형 색 보정 기능과 온디바이스 모니터링 대시보드를 제작했습니다[cite: 21].

#### 1.2. 개발 목표 및 결과
* [cite_start]**초기 목표**: 100% 온디바이스(on-device) 실시간 보정 필터 구현과 모델 재학습(relearn) 대시보드 제작이었습니다[cite: 23].
* [cite_start]**개발 결과**: 온디바이스 실시간 보정 필터는 구현했으나, 모델 재학습 대시보드는 일정 기준 이상의 성능 저하 시에만 작동하도록 구현했습니다[cite: 23].

---

### 2. 핵심 기술

#### 2.1. 기술 구성도
* [cite_start]**Main Server**: 고성능 모델과 True Label Data를 관리하고, Hub로 재배포합니다[cite: 25].
* [cite_start]**Hub**: 디바이스와 서버 간의 중간다리 역할을 하며, 경량화 모델과 고성능 모델을 관리합니다[cite: 25, 27].
* [cite_start]**On-device**: 실시간으로 이미지를 촬영하고, 분할(segmentation) 및 필터 적용 후 결과를 Hub로 전송합니다[cite: 25].

#### 2.2. 주요 기술
* **Hub**:
    * [cite_start]**자동 모니터링 및 재학습**: 온디바이스 모델의 **Distribution Shift**를 방지합니다[cite: 28].
    * [cite_start]**개인정보 보호**: 사생활 관련 데이터가 중앙 서버로 전송되지 않도록 방지합니다[cite: 28].
    * [cite_start]**Behavior**: 온디바이스 모델 성능 모니터링, 고성능 모델을 이용한 **오토라벨링(Soft Target Mask)**, **Knowledge Distillation**을 이용한 경량화 모델 재학습, **Shadow Test** 후 재배포 등의 기능을 수행합니다[cite: 28].
* **온디바이스 모델 성능 모니터링**:
    * [cite_start]디바이스에서 전송된 데이터를 활용하여 경량화 모델과 고성능 모델의 출력 마스크(Output Mask)를 추출합니다[cite: 32].
    * [cite_start]고성능 모델의 출력(Output)을 정답 라벨(True Label)로 간주하고, 경량화 모델의 출력과 비교하여 정확도를 계산합니다[cite: 33].
    * [cite_start]정확도가 특정 기준치(Threshold)보다 낮아지면 **자동으로 재학습이 시작**되고 사용자에게 메일로 알림이 전송됩니다[cite: 34, 35].
* **모델 재학습 (Knowledge Distillation)**:
    * [cite_start]**Knowledge Distillation**은 큰 모델(teacher model)의 지식을 작은 모델(student model)로 전이시키는 학습 기법입니다[cite: 38, 87].
    * [cite_start]학습 데이터는 정확도가 낮게 측정된 데이터와 서버에서 받은 정답 라벨(True Label) 데이터를 일정 비율로 섞어 **Concatenated Dataset**을 구성합니다[cite: 41, 42].
    * [cite_start]고성능 모델을 '교사(Teacher)', 경량화 모델을 '학생(Student)'으로 하여 학습을 진행합니다[cite: 43].
* **A/B Test (Shadow Test)**:
    * [cite_start]새롭게 학습된 모델이 기존 모델보다 항상 성능이 좋다고 보장할 수 없기 때문에, 디바이스에 재배포하기 전에 두 모델의 성능 테스트를 진행합니다[cite: 46, 47].
    * [cite_start]이 테스트는 학습에 사용되지 않은 최신 데이터를 이용하여 진행되며, 새로운 모델이 일정 기간 동안 기존 모델보다 지속적으로 더 좋은 성능을 보일 경우에만 배포됩니다[cite: 48, 49].
* **DeepLab V3+**:
    * [cite_start]이미지 각 픽셀을 특정 카테고리로 분류하는 최첨단 **심층 학습 모델**입니다[cite: 77].
    * [cite_start]`Atrous Convolution`, `Atrous Spatial Pyramid Pooling (ASPP)`, `Fully Convolutional Network (FCN)` 등의 특징이 있습니다[cite: 80, 81, 83].

---

### 3. 결과 분석 및 향후 과제

#### 3.1. 결과 분석
* [cite_start]**목표 달성**: 프로젝트의 목표로 했던 기능은 모두 달성했습니다[cite: 95].
* [cite_start]**아쉬운 점**: 프로젝트 기간의 제약으로 인해 예외 처리, 모듈화 등 코드의 **강건성(robustness)**이 부족했고, 리소스 부족으로 모델 재학습 과정에서 어려움이 있었습니다[cite: 96, 97].

#### 3.2. 향후 연구 과제
* [cite_start]**모델 경량화**: 양자화(quantization), 프루닝(pruning) 등을 통해 모델을 더욱 경량화해야 합니다[cite: 101].
* [cite_start]**데이터 관리**: 많이 사용된 데이터 순으로 삭제하거나 정리하여 메모리 효율성을 확보해야 합니다[cite: 102].
* [cite_start]**재학습 프로세스 개선**: 모델 구조나 하이퍼파라미터(hyperparameter)를 탐색할 수 있도록 재학습 과정을 개선해야 합니다[cite: 103].
* [cite_start]**재학습 로직 추가**: 일정 스텝 동안 성능이 이전 모델보다 좋지 않으면 재학습을 중단하는 로직을 추가해야 합니다[cite: 105].
* [cite_start]**코드 개선**: 일부 하드 코딩된 부분을 수정해야 합니다[cite: 106].
