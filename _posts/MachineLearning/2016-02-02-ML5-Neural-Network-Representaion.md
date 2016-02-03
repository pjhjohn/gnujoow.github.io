---
layout: post
title: ML5::Neural Networks:Representation
category: [Machine Learning]
tag: [Machine Learning, Neural Networks]
---

이번 포스팅과 그리고 다음에 올릴 포스팅에서 **신경망(Neural Network)**에 대해서 알아본다. 신경망은 꽤 오래된 아이디어이고 잠시 사람들 관심 밖의 아이디어였지만 오늘날에는 여러 기계학습문제에 관련해서 중요한 축으로 자리잡았다. 우리는 이미 Linear Regression이랑 Logistic regression을 배웠는데 (게다가 교수님이 우리가 많이 안다고 하셨는데) 왜 신경망 알고리즘을 알아야 할까? 다음 예를 보면서 알아보도록 하자.

## Non-linear Hypotheses


![예1](/assets/posts/MachineLearning/ml5-0.png)

위와 같은 training set을 가진 classification 문제에서 우리는 Logistic regression알고리즘을 적용해서 문제를 풀었다. 결과가 아주 마음에 든다. 하지만 Logistic regression은 특징이 1개 혹은 2개일 때 잘 분류한다. 그러나 많은 기계학습 문제들이 그렇듯이 특징이 1~2개 보다 많은경우가 많다. 집 값문제만 하더라도 사이즈뿐만 아니라 방의 갯수, 건설년도, 위치등 많은 특징들에 의해서 가격이 결정된다.

만약 집값을 결정하는 특징이 100가지라면 아래와 같이 여러 quardatic function으로 가설함수를 할 수 있을 것이다.

<div>
$
x_1^2, x_1x_2, ... , x_1x_{100}, x_2^2, ..., x_2x_{100}, ...
$
</div>

만약 특징(\\(n\\))이 100개라면 약 \\(\frac{n^2}{2}\\)이니까 5000개가 된다. 너무 많다... 따라서 아래와 같은 함수들로 표현하는 것을 고려해 볼 수 있다.

<div>
$
x_1^2, x_2^2, ... x_{100}^2
$
</div>

그러나 이 함수들로 경계를 표현하기에는 training set을 특징들을 정확히 표현 할 수 없다.

거의 모든 머신러닝 문제들은 **특징(n)**이 매우 크다. 다음 예를 살펴보자.

컴퓨터공학이나 전자공학을 전공했다면 [영상처리](https://ko.wikipedia.org/wiki/%EC%98%81%EC%83%81_%EC%B2%98%EB%A6%AC), 혹은 [컴퓨터비전](http://terms.naver.com/entry.nhn?docId=818632&cid=50376&categoryId=50376)라는 단어를 들어본적이 있을것이다. 우리가 눈으로 보기에는 간단한 문제들이 왜 컴퓨터한테는 어려울까?

어떤 자동차사진을 주고 우리가 느끼기엔 아! 자동차라고 인지 할 수 있는데 컴퓨터는 왜 인지하기 어려운 것일까

![예1](/assets/posts/MachineLearning/ml5-1.png)

작은 부분 그러니까 자동차의 문을 확대해서 보면 우리는 자동차의 문이 보이지만 컴퓨터에게는 숫자들의 행렬이 보인다. 컴퓨터 비전의 문제는 이러한 행렬을 보면서 이것이 자동차의 문이다! 라고 인지하는 것이다.

![예1](/assets/posts/MachineLearning/ml5-2.png)

여러 그림들의 특정 위치 pixel1, pixel2를 가지고 자동차인지 아닌지 구별하고 그래프로 나타내보자. 원래 이미지의 pixel1의 특징과 pixel2의 특징이 어디서 나타나는지 그래프로 표현해보고 차인지 아닌지 더 많은 예를 통해서 그려보면 아래와 같은 그래프가 나타나게 된다. 그럼 이 그래프를 2클래스로 나눌수 있다. 그럼 feature space의 dimension은 어떻게 될까? 가로세로가 각각 50인 그림이 있다면 총 그림에 2500개의 픽셀이 존재한다. 그럼 2500개의 특징이 존재한다. 이것을 Quadratic features로 나타내면 \\(/frac{n^2}{2}\\)이므로 약 3백만개의 특징이 존재하며 컴퓨터가 일일이 계산하기에는 너무 큰 시간이 소요된다.

따라서 특징이 많은 모델에 대해서 logistic regression을 적용하기에는 적절하지 않다. 

## Neurons and the brain

**신경망 알고리즘(Neural Network Algorithm)**은 기계가 사람의 뇌를 흉내내려고 만든 오래된 알고리즘이다.

80대 그리고 90년도 초에 널리쓰였지만 여러가지 이유로 90년 말에 들어서 인기가 줄어들었다. 아마도 주된 이유는 컴퓨터의 메모리와 속도가 지금보다 느렸기 때문일것이다.

최근에 들어 컴퓨터의 속도가 빨라지고 메모리가 커짐에 따라 비용(소요시간과 메모리소모)이 크다고 여겨졌던 신경망 알고리즘이 잘 작동할 수 있는 환경이 되었고 여러 application등에서 널리 쓰이게 됬다.

사람의 뇌를 상상해보자 감각을 느끼고 보는것을 배우고 수학도 배우고 미적분도 하고 정말 컴퓨터가 상상하기 어려울 정도로 여러가지일들을 배우고 또 수행한다. 이런 복잡함 일들을 컴퓨터가 할 수 있으면 좋겠지만 컴퓨터는 한가지일에 대해서 배울수 있다.


![예1](/assets/posts/MachineLearning/ml5-3.png)

그림에 보이는 뇌의 붉은 부분은 우리 뇌에서 청각을 담담하는 부분이다. 우리의 귀에서 부터 청각신호가 저 붉은 부분으로 전달되면 붉은 부분이 누구의 목소리인지 구분하게 된다. 신경과학자들은 귀와 연결된 신경을 절단하고 눈과 연결시켰는데 청각을 담당하는 저 부분이 보는 법을 배우게 됬다고 한다. 신기방기

![예1](/assets/posts/MachineLearning/ml5-4.png)

뇌에 붉게 표시된 영역읜 체지성(눈,귀,코를 제외한 신체감각)영역이다 마찬가지로 이 부분을 체지성 신경과 끊고 시각신경과 연결하면 체지성영역은 보는 법을 배우게 된다. 신기방기...

뇌가 스스로 다른 감각에 대해서 배우는 것처럼 neural network알고리즘도 새로운 타입의 데이터에 대해서 배우는것이 목표라고 생각한다.


## Model Representation I

그럼 신경망(Neural Network) 알고리즘을 사용할때 가설함수(hyoithesis function)의 표현은 어떻게 될까? 알아보도록하자. 신경망은 뉴런을 자극하는것과 뇌에서 뉴런들의 망처럼 개발됬다. 가설함수의 표현을 알아보기 전에 뇌를 구성하는 뉴런이라는 단위가 어떻게 생겼는지 살펴보자.

![예1](/assets/posts/MachineLearning/ml5-5.png)

이쯤되니 뇌를 배우는건지 알고리즘을 배우는지 헷갈린다 ㅎㅎ...
뉴런에서 중요한것은 두부분이다. 뉴런이 몸통이라고 할 수 있는 Nucleus을 살펴보자.Nuclues는 입력이라고 할 수있는, 즉 자극을 받아들이는 Dendrite가 있다(Input wire). 그리고 받아들인 자극을 다른 뉴런으로 보내는 Axon이 있다(Output wire).


![예1](/assets/posts/MachineLearning/ml5-6.png)