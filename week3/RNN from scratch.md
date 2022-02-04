## RNN from scratch



### 도입

RNN모델은 시점정보에 따라서 입력을 순차적으로 받아 출력을 생성한다는 개념을 구현하여 딥러닝을 활용한 자연어처리 분야에서 활용 분야의 다양화와 성능 향상에 획기적인 발전을 일으켰습니다. 그러나 최근 PLM (Pre-trained Language Model)의 발전에 따라서 RNN 형태의 모델이 사용되지 않고 있습니다.

RNN모델은 왜 자연어처리 분야에서 발전을 이룩했고, 또 어떤 한계점이 존재하기에 PLM모델로 대체되었는지를 알아보고자 하였습니다.



#### contents

1. RNN 기본 구조
2. RNN의 순전파 (forward)
3. RNN의 역전파 (backpropagation)
4. Truncated BPTT



#### RNN 기본 구조

RNN모델의 구조가 가지는 특징은 다음과 같습니다.

![image-20220121110854512](https://s2.loli.net/2022/01/21/WoxJauiTgEBbrf9.png)

- 시점 t 마다 input이 들어가고 hidden output이 생성되는 모델

- 각 시점의 hidden output은 이전 hidden output과  현재 시점의 input을 이용하여서  생성

  

#### RNN의 순전파 (forward)

시점정보를 어떠한 방식으로 모델이 학습할 것인지에 관해서 RNN은 순차적인 학습방법을 제안합니다. 

RNN의 순전파는 t시점의 입력 $x_t$를 받아서 현 시점의 은닉 $h_t$를 생성합니다. 이 때 $h_t$ 의 생성에는 $x_t$ 뿐만 아니라 이전 시점 은닉 $h_{t-1}$ 도 사용됩니다. 

순전파 과정에서 사용되는 파라미터들과 생성되는 결과물에 대한 설명을 notation기준으로 작성하였습니다.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b5/Recurrent_neural_network_unfold.svg/800px-Recurrent_neural_network_unfold.svg.png)

- 생성 결과물

$x_t (n, d)$ : t 시점의 입력 token. d는 token의 길이를 의미한다

$h_t (h, h) = \sigma (Vh_{t-1} + Ux_t + b_h)$  : t 시점의 hidden output. h는 은닉층의 차원을 의미한다

$o_t (n, a) = \sigma(Wh_t + b_y)$ : t  시점의 output. a는 output의 차원을 의미한다



- 사용 parameter

$U (d, h)$ : 입력과 hidden layer로의 연결을 매개하는 parameter

$V(h, h)$ : 이전 시점 hidden output과 현 시점 hidden output을 매개하는 parameter

$W(h, a)$ : 현 시점 hidden output과 output 생성을 매개하는 parameter



#### RNN의 역전파 (BPTT, BackPropagation Through Time)

RNN은 순차적으로 순전파를 진행하여 결과물을 생성한다고 하였습니다. 그리고 그에 사용되는 학습 파라미터들은 $U, V, W$ 입니다.

따라서 역전파과정에서는 $U, V, W$ 파라미터들이 업데이트 되어야하며, 각 파라미터들은 시점 즉 순서에 따라서 업데이트 되는 정도가 달라야 할 것입니다. 

이러한 RNN모델의 역전파 특성을 고려하여 역전파 과정을 살펴보겠습니다.



[Backpropagation in RNNs - YouTube](https://www.youtube.com/watch?v=I4_lyJVnpMg&t=14s)

![image-20220121145427105](https://s2.loli.net/2022/01/21/uO4snkiK3WHzPg6.png)

- RNN의 오차 역전파를 위해서는 $U, V, W$가 모두 업데이트 되어야 한다
- 그런데 여기서 $V$ 는 $h_t$, $h_{t-1}$ 생성에 모두 관여한다
- $h_t$, $h_{t-1}$ 는 RNN 학습의 핵심이 되므로 본 장은 $V$ 의 역전파 과정을 이해하는 것에 중점을 둔다



**1. 기본 미분 공식**
$$
\mathrm{chain rule} : \frac{\partial L}{\partial W} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial z} \cdot \frac{\partial z}{\partial W}
$$
지수함수 미분

$f(x) = e^{x}$


$$
\frac {\partial f(x)}{\partial x} = e^x
$$
$f(x) = e^{ax}$
$$
\frac {\partial f(x)}{\partial x} = a\cdot e^x=
$$
로그함수 미분

$f(x) = log x$
$$
\frac {\partial f(x)}{\partial x} = \frac {1}{x}
$$
합성함수 미분

$y = f(g(x))$

$y' = f'(g(x))\cdot g'(x)$



sigmoid 함수 미분

$f(x) = \frac{1}{1 + e^{-x}}$
$$
\frac {\partial f(x)}{\partial x} = f(x)(1 - f(x))
$$


cross-entropy 함수 미분

$L(x) = -[y\cdot ln \hat{y} + (1-y)ln (1 - \hat{y})]$
$$
\frac {\partial L(x)}{\partial x} = y - \hat{y}
$$



**2. $V$ 의 역전파 과정 살펴보기**

$\frac{\partial L}{\partial V}$ = ?



1. chain rule 적용하기

$$
\frac{\partial L_t}{\partial V} = \frac{\partial L_t}{\partial \hat{y_t}} \frac{\partial \hat{y_t}}{\partial h_t} \frac{\partial h_t}{\partial V}
$$

2. chain rule 분해하기

   ![image-20220121160137160](https://s2.loli.net/2022/01/21/GQAzSEWVy9gwXnx.png)

   $\frac{\partial h_t}{\partial V} = \frac{\partial h_t}{\partial h_{t-1}} \cdot \frac{\partial h_{t-1}}{\partial h_{t-2}} \cdot \frac{\partial h_{t-2}}{\partial h_{t-3}} \cdot ... \frac{\partial h_{t-n}}{\partial V}$

- why?
  - $h_t (h, h) = \sigma (Vh_{t-1} + Ux_t + b_h)$
  - $h_t$를 $V$로 미분하는 과정에서 $h_{t-1}$ 이 필요하다. 그런데 $h_{t-1}$의 미분에는 이전 시점의 은닉 $h_{t-2}$가 개입하게 된다. 따라서 우리가 t 시점의 은닉을 미분하기 위해서는 시점의 수 t만큼 미분이 계속해서 진행된다.

3. 최종 미분 식

   
   $$
   \frac{\partial L_t}{\partial V} = \sum_{k = 0}^{t} \frac{\partial L_t}{\partial \hat{y_t}} \frac{\partial \hat{y_t}}{\partial h_t} (\prod_{j = k+1}^{t} \frac {\partial h_j}{\partial h_{j-1}}) \frac{\partial h_k}{\partial V}
   $$



**문제점**

- t가 길어질수록 $\prod_{j = k+1}^{t} \frac {\partial h_j}{\partial h_{j-1}}$ 의 연산량이 매우 증가하게 된다

  ![image-20220121160120055](https://s2.loli.net/2022/01/21/kagIzWNJfG825mi.png)

- 해당 연산 과정, 즉 곱셈과정에서 0에 가까운 gradient가 곱해지면 이후의 오차 역전파가 진행되지 못하는 문제 발생

  -> gradient vanishing 문제



**해결 방법**

#### Truncated BPTT

![image-20220121160249830](https://s2.loli.net/2022/01/21/61EX3xvORPKUad9.png)

시점 t가 증가함에 따라서 역전파 과정에 $V$를 계산하는 것에 매우 많은 자원이 소모될 수 있음을 우리는 알게되었습니다. 이에 연산의 어려움과 gradient vanishing 문제를 해결하기 위하여 Truncated BPTT가 고안되었습니다.

RNN의 역전파 BPTT를 사용자가 설정한 총 시점 t에 관해서 계산하는 것이 아니라 t를 몇 개의 구간을 나눠서(truncation) 역전파를 따로 진행하는 것을 말합니다.

이를 통해서 파라미터 업데이트 과정에 연산량이 과다해지는 부분을 어느정도 해소할 수 있었습니다.
