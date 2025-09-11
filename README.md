# MNIST 분류 실험: VGG16 Fine-Tuning 비교

VGG16 모델을 MNIST 데이터셋에 적용하여 **세 가지 Fine-Tuning 전략**을 비교.
각 케이스별 정확도 비교.


---
## 개요

목표: MNIST 손글씨 숫자 데이터셋에서 VGG16을 활용하여 다양한 Fine-Tuning 방법이 모델 성능과 학습 속도에 미치는 영향을 분석.

세 가지 실험 케이스:

### 1. CASE 1: 마지막 레이어 교체 + 전체 학습
- pretrained VGG16의 마지막 레이어를 제거하고 MNIST 클래스(10)용 Linear 레이어 추가
- Convolution + Classifier 모든 레이어 학습


### 2. CASE 2: 마지막 레이어 교체만, 전체 학습
- pretrained Convolution layer는 유지
- 마지막 Linear 레이어만 MNIST 클래스 수에 맞게 변경


### 3. CASE 3: Convolution Freeze + 마지막 레이어만 학습
- pretrained Convolution layer freeze (`requires_grad=False`)
- 마지막 Linear 레이어만 학습


---

## 데이터셋

- **MNIST 손글씨 숫자 데이터셋**
- 원본 그레이스케일 이미지를 VGG16 입력 크기(224×224)로 리사이즈 후 3채널 변환
- 학습 / 검증 비율: 80% / 20%
