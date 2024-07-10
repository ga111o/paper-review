---
marp: true
theme: gaia
paginate: true
---

### Contrastive Language–Image Pre-Training

# CLIP 논문 리뷰

---

### CLIP

https://arxiv.org/abs/2103.00020
https://arxiv.org/pdf/2103.00020

2021년 openai에서 발표한 이미지 캡셔닝 뉴럴 네트워크

---

### CLIP

<br>

<center>
<img src="./benchmark.png" style="width:1000px">
</center>

기존 캡션 모델에 비해 월등히 뛰어난 성능을 보이며, 당당히 sota자리를 차지함.

---

0. Abstract
1. Introduction and Motivating Work
2. Approach
3. Experiments
4. Comparison to Human Performance
5. Data Overlap Analysis
6. Limitations
7. Broader Impacts
8. Related Work
9. Conclusion

---

#### 0. Abstract

1. 기존 컴퓨터 비전의 한계

2. 기존 지도학습 방식의 한계

3. raw text 학습의 이점

4. zero-shot 전이

5. 모델 성능 측정

---

#### 0. Abstract

#### State-of-the-art computer vision systems are trained to predict a fixed set of predetermined object categories

최첨단 컴퓨터 비전 시스템은 미리 정해진 고정된 집합의 카테고리를 예측

- 전통적인 컴퓨터 비전:
  - 특정 작업을 위해 학습된 데이터셋 사용
  - 고정된 카테고리를 사용하고 특정 데이터셋에 특화

---

#### 0. Abstract

##### 기존 지도 학습 방식의 한계

- 제한된 형식의 지도학습은 모델 일반화와 사용성을 제한
- 새로운 카테고리를 추가하는 것이 어려움
- 라벨링 및 데이터셋 구성에 많은 시간과 비용 소요

---

#### 0. Abstract

##### Learning directly from raw text about images

이미지와 함께 raw text를 바로 학습시키는 것은 지도학습의 훨씬 더 넓은 source를 활용하는 유망한 대안

- 이미지와 이미지 캡션 쌍을 함께 학습
- 모델의 범용성과 확장성을 크게 향상

---

#### 0. Abstract

##### Pre-training task

- 이미지에 대한 캡션을 예측하는 것이 목표
- 기존에는 SOTA 데이터셋으로 학습했음.
- clip은 인터넷에서 수집된 4억 개의 이미지-텍스트 쌍 데이터셋 사용

---

#### 0. Abstract

##### zero-shot 전이

- pre-training 이후, 자연어는 학습된 visual concepts를 참조하거나 새로운 것을 설명
- 모델을 하위 task로 zero-shot 전이 가능

---

#### 0. Abstract

##### 모델 성능

- 30개 이상의 다른 computer vision datasets 벤치마킹
- OCR, 비디오 속 액션 인식, geo-localization, fine-grained object classification 등

- 특정 task에 대한 별도의 학습 없이도 지도학습 모델과 경쟁할만한 성능

---

#### 0. Abstract

##### 3줄 요약

1. 우리 모델 zero-shot 성능 좋아요
2. 우리 모델 pre-training만으로도 성능 잘 뽑혀요
3. 우리 모델 다양한 task에서도 잘 작동해요

---

###### 1. Introduction and Motivating Work

##### 자연어에서의 Pre-training 방법

- **NLP 분야의 변화**: raw text를 직접 사용하여 모델을 학습하는 pre-training 방법이 자연어처리 분야에서 중요한 변화를 이끌어냄.
- **장점**: pre-training의 장점을 강조하며, GPT-3와 같은 모델이 여러 supervised 모델들과 경쟁 가능함을 언급.

---

###### 1.1. Introduction

- **자기 회귀 모델**: 비종속 작업 목표를 사용하여 모델 용량과 필요 컴퓨팅 자원을 늘리면서 성능을 향상.

- **입출력 인터페이스 표준화**: text2text는 입출력 인터페이스를 텍스트로 표준화하여, 작업 비종속 목표가 후속 작업 데이터셋으로 제로샷 전이가 가능토록 함.

- **웹 데이터셋**: NLP 분야에서는 크라우드 소싱 데이터셋보다 웹크롤링으로 가져온 데이터로 pre-training 돌리는 게 성능 잘나옴

- **제안**: 컴퓨터 비전에서도 기존 크라우드 소싱 데이터셋 대신, 웹에서 수집한 대규모 데이터셋을 사용하는 것을 제안.

---

###### 1.2. Motivating Work

Mori et al. (1999)
`The olfactory bulb: coding and processing of odor molecule information`

- 텍스트 문서와 이미지를 묶어 데이터로 사용

- 모델이 명사랑 형용사 예측하도록 학습시키는 방법으로 콘텐츠 기반 이미지 검색을 향상시키려 함.

<br>

이 외에도 4개의 관련 연구를 시간 순서대로 보임.

---

###### 1. Introduction and Motivating Work

##### 자연어 감독을 통한 이미지 표현 학습

- 자연어 감독을 통한 이미지 표현 학습은 흥미롭긴 하지만, 실제 성능은 다른 방식보다 낮음.

<small>

- zero-shot ImageNet 정확도
  - 자연어 supervision: 11.5% (Li et al. (2017))
  - 최신 연구: 88.4% (Xie et al. (2020))
  - 기존 연구: 50% (Deng et al. (2012))

</small>

---

###### 1. Introduction and Motivating Work

##### Static Softmax Classifier의 한계

1. 약한 감독을 사용하면 성능이 5% 이상 향상하는 것을 확인

2. 노이즈 많은 라벨을 예측하도록 사전 학습하니 성능이 큰 폭으로 증가
   <br>

- 1과2는 제한된 양의 gold-label을 사용하는 것과, 사실상 무한한 양의 raw text를 사용하는 것 사이의 실용적인 중간 지점

---

###### 1. Introduction and Motivating Work

##### Static Softmax Classifier의 한계

1. 약한 감독을 사용하면 성능이 5% 이상 향상하는 것을 확인

2. 노이즈 많은 라벨을 예측하도록 사전 학습하니 성능이 큰 폭으로 증가
   <br>

- 두 연구 모두 static softmax classifier 사용
  -> 유연성과 zero-shot 능력에 한계

---

###### 1. Introduction and Motivating Work

##### 약한 Supervision VS 자연어 Supervision

- 약한 Supervision: 수백만~수십억 개의 이미지로 모델 트레이닝
  -> accelerator로 수 년을 돌려야 함

- 자연어 Supervision: 수십만 개의 이미지로 모델 트레니이
  -> accelerator로 수 일을 돌려야 함.

---

###### 1. Introduction and Motivating Work

##### 연구 목적

- 이 연구에서는 weak supervision과 자연어 supervision 간의 차이를 줄이고자 함
- 대규모 자연어 supervision으로 트레이닝된 이미지 분류기 연구

---

###### 1. Introduction and Motivating Work

##### CLIP

- 인터넷에서 수집한 4억 개의 이미지-텍스트 쌍으로 학습
- CLIP은 효율적인 자연어 감독을 통한 학습 방법을 사용한 모델
  -> 단순화된 ConVIRT라고 볼 수 있음

---

###### 1. Introduction and Motivating Work

##### CLIP의 확장성

- 각각 2배의 컴퓨팅 크기의 8가지 모델을 훈련
- 전이 성능이 컴퓨팅 크기에 따라 부드럽게 예측 가능한 함수 형태를 띄는 것을 발견.

---

###### 1. Introduction and Motivating Work

##### Pre-training task, zero-shot 전이, 모델 성능

- Abstract에서 언급한 내용들 반복.

---

###### 1. Introduction and Motivating Work

##### 선형 프로브 표현 학습

- 선형 프로브 표현 학습 분석
- 기존 최고의 ImageNet 모델보다 성능 우수하며, 효율성도 좋음

---

###### 1. Introduction and Motivating Work

##### 견고성

- CLIP 모델의 zero-shot이 다른 supervised 모델보다 더 견고함.
- zero-shot 평가에서 비종속작업 모델의 능력을 더 잘 대표함.
