<<<<<<< HEAD
# “혼자 공부하는 머신러닝,딥러닝 2025 by Haeseon Park.”

## Table of Contents
# Machine learning
- 01-3 마켓과 머신러닝
- 02-1 훈련 세트와 테스트 세트
- 02-2 데이터 전처리
- 03-1 k-최근접 이웃 회귀
- 03-2 선형 회귀
- 03-3 특성 공학과 규제
- 04-1 로지스틱 회귀
- 04-2 확률적 경사 하강법
- 05-1 결정 트리
- 05-2 교차 검증과 그리드 서치
- 05-3 트리의 앙상블
- 06-1 군집 알고리즘
- 06-2 k-평균
- 06-3 주성분 분석
# Deep learning
- 07-1 인공 신경망
- 07-2 심층 신경망
- 07-3 신경망 모델 훈련
- for images
- 08-1 합성곱 신경망의 구성요소
- 08-2 합성곱 신경망을 사용한 이미지 분류
- 08-3 합성곱 신경망의 시각화
- for texts
- 09-1 순차 데이터와 순환 신경망
- 09-2 순환 신경망으로 IMDB 리뷰 분류하기
- 09-3 LSTM과 GRU 셀
- for language model
- 10-1 어텐션 메커니즘과 트랜스포머
- 10-2 트랜스포머로 상품 설명 요약하기
- 10-3 대규모 언어 모델로 텍스트 생성하기
=======
# MNIST 분류 실험: VGG16 Fine-Tuning 비교

VGG16 모델을 MNIST 데이터셋에 적용하여 **세 가지 Fine-Tuning 전략**을 비교.

각 케이스별 정확도 비교.


---
## 개요

목표: MNIST 손글씨 숫자 데이터셋에서 VGG16을 활용하여 다양한 Fine-Tuning 방법이 모델 성능에 미치는 영향을 분석.

세 가지 실험 케이스:

### 1. CASE 1: 마지막 레이어 교체 + 전체 학습
- pretrained VGG16의 마지막 레이어를 제거하고 MNIST 클래스(10)용 Linear 레이어 추가
- Convolution + Classifier 모든 레이어 학습


### 2. CASE 2: 마지막 레이어 추가만 + 전체 학습
- pretrained Convolution layer는 유지
- 마지막 Linear 레이어만 MNIST 클래스 수에 맞게 변경


### 3. CASE 3: Convolution Freeze + 마지막 레이어만 학습
- pretrained Convolution layer freeze (`requires_grad=False`)
- 마지막 Linear 레이어만 학습


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
   - 특징: case1보다 Epoch 많지만 Val Accuracy 큰 차이 없음, 더 학습하면 성능 소폭 개선 가능  

3. **CASE3**  
   - Validation Accuracy 최고 98.86% → 전체 학습 대비 약간 낮음  
   - Early Stopping 4 Epoch → 가장 빨리 학습 종료  
   - 특징: Conv layer freeze로 연산량 감소 → 학습 속도 빠르지만 성능 약간 감소  

---

## 결론

- Conv freeze 후 마지막 레이어만 학습한 **CASE3**는 학습 속도가 빠르지만 정확도는 약간 낮음  
- 전체 학습한 **CASE1/2**는 최고 정확도를 달성  
- CASE2는 **아직 추가 학습 여력이 있어** 학습 시간을 늘리면 더 높은 정확도 도달 가능

  
---


## 참고
- **MNIST 손글씨 숫자 데이터셋**
- 원본 그레이스케일 이미지를 VGG16 입력 크기(224×224)로 리사이즈 후 3채널 변환
- 학습 / 검증 비율: 80% / 20%
- Train : 48,000장
- Validation : 12,000장
- Test : 10,000장
- Total : 70,000장
- 모델: Pretrained VGG16 (PyTorch)  
- 학습 환경: CUDA GPU (RTX 3080 Ti)
>>>>>>> 37d7cfaa9ef45f9419dd5a26694de50b8bfb00cb
