# Activation Function

---

#### Neuron

neural network에는 *weight, bias* and their respective activation function에 따라 작동하는 neuron 있음



#### Activation function이 하는 일?

input의 "weighted sum"을 계산하고, bias를 더하여 뉴런의 activation 여부를 결정(범위가 -inf에서 +inf까지 가능)

-> neuron은 value의 범위를 모르므로 neuron의 activation 여부를 결정하기 위한 목적으로 activation function이 사용됨

> *목적* : 뉴런의 output **non-linearity** 도입



#### Activation Function

##### Step function

![img](https://miro.medium.com/max/650/0*8U8_aa9hMsGmzMY2.)

Threshold 이상이면 1 ( *activated* ) / 아니면 0 ( *not activated* )

이진 분류기에서 여러 개의 클래스가 모두 threshold 이상이라 output이 1이면 하나의 클래스를 결정할 수 없음

-> “activated” or not ( binary ) 보다 intermediate ( analog ) activation values를 원함



##### Linear Function( A = cx )

neuron의 weighted sum에 비례하는 직선 함수

기울기(c)가 x의 영향을 받지 않고 일정함

1) prediction에서 error가 발생할 경우 backpropagation에 의한 변화가 일정하고 input x에 대해 영향받지 않음

2) 각 layer는 linear function에 의해 activation 되는데 아무리 layer가 많아도 마지막 layer의 activation function은 첫번째 layer의 linear function일 뿐

   -> N 개의 layer를 single layer로 변경 가능하므로 layer를 쌓는 이유가 없어짐



##### Sigmoid Function

![img](https://miro.medium.com/max/1200/0*5euYS7InCmDP08ir.)



Nonlinear 형태로 layer 쌓을 수 있게 됨

-2 ~ +2 사이는 가파르기 때문에 x 값이 조금만 바뀌어도 y값이 크게 변경 

-> y 값을 곡선의 양 끝으로 가져오는 경향 있음을 의미

**-> 장점 1) prediction을 명확히 구분 가능하게 해줌**



**장점 2)** **activation function의 output 범위가** Linear function(-inf, +inf) 과 달리 **(0, 1) 범위에 있음**



- 시그모이드 단점

function의 양쪽 끝으로 갈 수록 y값이 x의 변화에 덜 반응함 -> 해당 영역의 gradient 작음

--> **“vanishing gradients”**문제 야기시킴

양 끝으로 갈 수록 gradient 가 작거나 사라졌기 때문에 네트워크가 더 이상 학습할 수 없게 되거나, 아주 느리게 학습



***! vanishing gradients***

- gradient: 미분값 즉 변화량

- 변화량이 매우 작다면

  - network 를 효과적으로 학습시키지 못함
  - error rate가 충분히 낮아지지 못한채 수렴해버리는 문제가 발생

  

- Vanishing gradient problem은 activation function을 선택하는 문제에 의존적으로 발생

- Sigmoid, Tanh 같은 activation function은 non-linear 방식으로 input을 매우 작은 output range로 매핑( **squashing** )

  - 매우 넓은 input space가 극도록 작은 범위로 매핑

    -> input space에서 큰 변화가 있더라 하더라도, output에서 작은 변화( gradient가 작기 때문 )

  - 레이어를 더 많이 쌓을 수록 더 작은 region으로 매핑되게 됨

  - 그 결과 첫 레이어 input에 매우 큰 변화가 있더라도 output을 크게 변화시키지 못함



- gradient가 거의 0에 가깝게 되어버리면(vanish되어버리면), network는 매우 느리게 배울 수 밖에 없게되고, (global minimum이 아닌) local minimum에 도달하게 됨



##### Tanh Function

![img](https://miro.medium.com/max/800/0*YJ27cYXmTAUFZc9Z.)

sigmoid와 유사한 특성 가짐

1) Non-linear 이므로 layer 쌓을 수 있음
2) activation 범위가 (-1, 1) 으로 제한
3) **sigmoid 보다 tanh의 기울기가 더 큼**
4) sigmoid와 같이 vanishing gradients 문제 발생





##### ReLu

$$
A(x) = max(0, x)
$$

![img](https://miro.medium.com/max/622/0*vGJq0cIuvTB9dvf5.)



tanh는 거의 모든 neuron이 활성화 되기 때문에 비용이 많이 듦

-> 활성화가 효율적으로 일어나기를 바람

ReLu는 거의 50%가 활성화 되지 않으므로 네트워크가 가벼움( 계산 비용 적게 듦)

x가 음수인 부분에서 기울기가 0이 되기 때문에 descent 중에서 weight가 조정되지 않음 

-> neuron이 error / input의 변화에 응답하지 않음

-> 네트워크의 상당 부분이 수동적이게 됨

해결 : 음수 부분에서 y = 0.01x 와 같이 약간 기울어진 선으로 만듦(*leaky ReLu*)





----

[1] [Understanding Activation Functions in Neural Networks](https://medium.com/the-theory-of-everything/understanding-activation-functions-in-neural-networks-9491262884e0)

[2] [Vanishing gradient problem](https://ydseo.tistory.com/41)







