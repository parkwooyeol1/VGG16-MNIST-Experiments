# MNIST 분류 실험: VGG16 Fine-Tuning 전략 비교

이 프로젝트는 VGG16 모델을 MNIST 데이터셋에 적용하여 **세 가지 Fine-Tuning 전략**을 비교.  
각 전략별 정확도 분석합니다.

---

## 개요

목표: MNIST 손글씨 숫자 데이터셋에서 VGG16을 활용하여 다양한 Fine-Tuning 방법이 모델 성능과 학습 속도에 미치는 영향을 분석.

세 가지 실험 케이스:

### 1. CASE 1: 마지막 레이어 교체 + 전체 학습
- pretrained VGG16의 마지막 레이어를 제거하고 MNIST 클래스(10)용 Linear 레이어 추가
- Convolution + Classifier 모든 레이어 학습
- 장점: 모델의 전체 용량 활용 → 정확도 높을 가능성 있음
- 단점: 학습 속도 느림, 작은 데이터에서는 과적합 가능

### 2. CASE 2: 마지막 레이어 추가만, 전체 학습
- pretrained Convolution layer는 유지
- 마지막 Linear 레이어만 MNIST 클래스 수에 맞게 변경
- Classifier 전체 학습
- 장점: CASE1보다 학습 안정적, 속도는 비슷
- 단점: 정확도는 CASE1보다 약간 낮을 수 있음

### 3. CASE 3: Convolution Freeze + 마지막 레이어만 학습
- pretrained Convolution layer freeze (`requires_grad=False`)
- 마지막 Linear 레이어만 학습
- 장점: 학습 속도 빠름, 작은 데이터셋에서도 충분한 성능
- 단점: 복잡한 패턴 학습에는 제한 가능


## 분석

1. **CASE1**  
   - 학습 Loss는 점진적으로 감소하며, Validation Loss도 대부분 안정적  
   - Validation Accuracy 최고 99.38% 달성  
   - Early Stopping 5 Epoch에서 작동 → 모델이 이미 충분히 학습됨  
   - 특징: 전체 학습했지만 Early Stopping 덕분에 빠르게 종료  

2. **CASE2**  
   - 학습 Loss 지속적으로 감소  
   - Validation Accuracy 최고 99.45% → 성능 거의 최상  
   - Epoch 10까지 학습, 아직 Early Stopping 작동 안 함 → **추가 학습 여력이 있음**  
   - 특징: case1보다 Epoch 많지만 Val Accuracy 큰 차이 없음, 더 학습하면 성능 개선 가능  

3. **CASE3**   
   - Validation Accuracy 최고 98.86% 
   - Early Stopping 4 Epoch → 가장 빨리 학습 종료  
   - 특징: Conv layer freeze로 연산량 감소 → 학습 속도 빠르지만 성능 감소  


---


## 결론

- Conv freeze 후 마지막 레이어만 학습한 **CASE3**는 학습 속도가 빠르지만 정확도는 약간 낮음  
- 전체 학습한 **CASE1/2**는 최고 정확도를 달성  
- CASE2는 **아직 추가 학습 여력이 있어** 학습 시간을 늘리면 더 높은 정확도 도달 가능  


---


## 참고

- 원본 그레이스케일 이미지를 VGG16 입력 크기(224×224)로 리사이즈 후 3채널 변환
- 학습 / 검증 비율: 80% / 20%
- 데이터셋: **MNIST 손글씨 숫자 데이터셋**(Train: 48,000, Validation: 12,000, Test: 10,000)  
- 모델: Pretrained VGG16 (PyTorch)  
- 학습 환경: CUDA GPU (RTX 3080 Ti)
---
