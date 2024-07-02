### CLIP

[paper link](https://arxiv.org/pdf/2103.00020)

> 용어
>
> > transfer: 사전 학습된 지식을 새로운 작업에 적용하는 것<br>
> > supervision (learning): 지도 (학습)<br>
> > pre-training: 모델이 특정한 작업에 초점을 맞추기 전에, 대규모의 일반적인 데이터셋을 사용하여 기본적인 패턴과 구조를 학습하는 -

0. Abstract

   > State-of-the-art computer vision systems are trained to predict a fixed set of predetermined object categories

   최첨단 컴퓨터 비전 시스템은 미리 정해진 고정된 집합의 카테고리를 예측.

   - 전통적인 컴퓨터 비전은 특정 작업을 위해 학습된 데이터셋을 사용. ex) 이미지 분류 모델은 고양이와 개를 구분하기 위해 고양이와 개 이미지를 포함한 데이터셋으로 학습함. 이러한 모델들은 고정된 카테고리를 사용하고 특정 데이터셋에 특화되어있음.

   - 미리 정해진 고정된 집합의 카테고리: [강아지, 고양이, 자동차 ...]

   - 특정 데이터셋에 특화: 데이터셋에 포함된 객체들만 인식 가능. 그 외 새로은 객체를 인식하기 위해서는 모델을 다시 학습해야 함.

   > this restrict form of supervision limits their generality and usability since additional labled data is needed to specify any other visual concept

   > 이러한 restrict form의 supervision은 limits한다. 그들의 generality와 usability를. since 추가적인 labeled data가 필요되어질 때. 어떠한 다른 visual concept를.

   이러한 제한된 형식의 supervision은 그들의 일반화와 사용성을 제한한다. 제한적인 레이블된 데이터는 어떠한 다른 시각적인 개념을 특정하는데 필요되어지기 때문에.

   - 즉, 기존 supervision learining의 한계를 말하고 있음.

   - 기존 지도 학습 방식에서는 모델이 특정 작업을 수행할 수 있도록 하기 위해서, 그 작업에 해당하는 레이블이 지정된 데이터셋을 사용하여 모델을 학습시는데, 위에서 말했다시피, 미리 정해진 고정된 집합의 카테고리 외의 새로운 카테고리를 추가하는 게 힘듦. 라벨링 다시 해야하고, 데이터셋도 필요하고 해서.. 물론 그 과정에서 엄청난 시간과 돈이 들어감.

   > learning directly from raw text about image는 a promising alternative which leverages a much broader source of supervision

   이미지와 함께 raw text를 바로 학습시키는 것은 supervision의 훨씬 더 넓은 source를 활용하는 것에 유망한 대체이다.

   즉, 이미지와 그 이미지에 대한 설명이 함께 제공되는 데이터셋을 사용하는 것은 훨씬 더 넓은 범위의 supervison을 활용하는 것에 대한 유망한 대안임을 주장.

   - 즉, 이미지에 라벨을 붙이고 학습시키는 것보다, 이미지와 이미지의 캡션 쌍을 함께 학습시키는 것이 훨씬 성능이 좋을 거다라고 말하는 건가

   - 기존에는 `{강아지: [강아지 이미지1, 강아지 이미지2 ... 강아지 이미지 n]}`으로 학습시켰다면, 이 논문에서는 `{강아지 이미지1: "들판을 뛰어다니는 강아지", 강아지 이미지2: "간식을 먹는 강아지" ... }`와 같이 나타낸다는 것.

   - 즉, 이미지와 함께 raw text를 바로 학습시키는 방식이 기존의 전통적인 supervision learning 방식에 비해 더 유망한 대안임을 나타냄. -> 텍스트 데이터를 통해 훨씬 더 넓은 범위의 visual concept 자체를 학습할 수 있음 -> 모델의 범용성 & 확장성?을 크게 향상시킬 수 있음.

   > we demonstrate that _the simple pre-training task of predicting which caption goes with which image_ is an efficient and scalable way to learn SOTA image representations from scratch on a dataset of 400 million (img-text) pairs collected from the internet.

   > 문장 왤케 길어

   > 우리는 (해당 논문에서) 시연한다. 간단한 pre-training task의 예측이, 어떠한 캡션이 어울리는지 with 어떠한 이미지가 효과적이고 확장 가능한 ,방식으로 SOTA 이미지 표현을, 학습하는 방법 / from scratch on 400 million의 이미지-텍스트 쌍이 모여진 데이터셋에서, 인터넷으로부터

   우리는 어떠한 이미지에 어떠한 캡션이 어울리는지 예측하는 간단한 pre-trained task가 SOTA 이미지 표현을 학습하는 효과적이고 확장 가능한 방법임을 시연한다. / from 인터넷에서 400 million의 이미지-텍스트 쌍이 collected된 데이터셋을 가져와서.

   - 대충 이 논문에서 `위에서 말한 거 테스트 해봐신디 생각보다 좋더라` 라는 것을 말하고 싶은 거 같음.

   > After pre-training, natural language is used to reference learned visual concepts (or describe new ones) enabling zero-shot transfer of the model to downstream tasks.

   > pre-training 이후, 자연어는 used된다. to reference learned visual concepts (or descrive new ones) enabling zero-shot transfer of the model to downstream tasts.

   pre-training 이후, 자연어는 학습된 visual concepts를 참조하거나, 새로운 것을 설명하는 것으로 모델을 하위 task로 zero-shot transfer가 가능하도록 사용된다.

   - `자연어가 학습된 시각적 개념을 참조하는 데 사용된다`가 뭘 뜻하는 거지... 여기서 말하는 `refernece`는 뭐 다른 뜻이 있는 건가..

   - 여튼, pre-training 해서 보니까 zero-shot transfer가 돼서 여러가지로 활용 가능하다고 말하는 거 같음.

   > We study the performance of this approach by benchmarking on over 30 different existing computer vision datasets, spanning tasks such as OCR, action recognition in videos, geo-localization, and many types of fine-grained object classification

   > 우리는 이 접근 performance를 연구했다. / 30개 이상의 다른, 존재하는 computer vision datasets를 벤치마킹해서. / ocr같은 spanning task나, 비디오 속 액션 인식이나, geo-localization이나, 많은 타입의 fine-grained object classification이나.

   우리는 30개 이상의 다른 computer vision datasets를 벤치마킹 해서 이러한 접근의 성능을 연구했다. / ocr같은 spanning task나, 비디오 속 액션 인식이나, geo-localization이나, 많은 타입의 fine-grained object classification이나...

   - SOTA 찍은 거 보면 평가도 잘 나왔겠죠 뭐.

   - 평가는 어련히 잘 하지 않았을까

   > The model transfers non-trivially to most tasks and is often competitive with a fully supervised baseline without the need for any dataset specific training.

   > 모델은 transfer한다. non trivially to most task가.. and model은 주로 competitive한다. with a fully supervised baseline without the need for any dataset specific training

   > 모델은 transfer한다. 사소하지 않은 주요 task에서. and 모델은 주로 완전한 supervised 기준으로 경쟁한다. 어떠한 dataset에 특정한 training이 필요하지 않도록.

   모델은 사소하지 않은 주요 task에서 transfer한다. 그리고, 모델은 주로 어떠한 데이터셋에 특정한 training이 필요하지 않도록 완전한 supervise 기준으로 경쟁한다.

   - 모델이 pre-trained만 했음에도, 즉, 특정 task에 대한 별도의 train이 없어도 supervised 모델과 경쟁할만한 성능을 보임.

   - 3줄요약

     - 우리 모델 zero-shot 쩔어요.
     - 우리 모델 pre-training만 해도 성능 잘나와요.
     - 우리 모델 어따가 붙이든 잘 작동해요.

   > training과 learning 차이점?

   > For instance, we match the accuracy of the original ResNet-50 on ImageNet zero-shot without needing to use any of the 1.28 million training examples it was trained on.

   > 예를들어, 우리는 ~~~의 정확성을 match한다. without needing to use any of ~~training examples. it was trained on.

   예를들어, 우리는 ImageNet zero-shot에 원래 ResNet-50의 accuracy를 match한다. / 1.28 million의 training examples를 사용하지 않고도

   - 한줄요약
     - 우리 모델 짱짱좋음

    <br>

   뭐야 abstract 하는데만 1시간 넘게 걸리네... 모델 어케 굴러가는지 나오면 한 페이지 하는데 한나절은 걸릴듯....😢

   논문 리뷰 생각보다 많이많이 힘들구나

---
