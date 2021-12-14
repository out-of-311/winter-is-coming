## Why we use Distributed Representation?



### 서론

우리가 현재 당연하게 사용하는 실수 형태의 Representation은 왜 사용하게 된걸까? 그리고 우리는 왜 이 방법을 사용해야하는걸까? 그 단서를 Count-based Representation, NNLM, word2vec 모델에서 찾아보자



### 본론

우리는 다양한 NLP task를 하기 위해 언어 모델(Language Model)을 사용한다. 언어 모델의 정의는 다음과 같다. 

> 언어 모델(Language Model, LM)은 언어라는 현상을 모델링하고자 단어 시퀀스(또는 문장)에 확률을 할당(assign)하는 모델입니다.
>
>
> 출처 : [1) 언어 모델(Language Model)이란? - 딥 러닝을 이용한 자연어 처리 입문 (wikidocs.net)](https://wikidocs.net/21668)

언어모델은 전통적으로 **Statistical Language Model (SLM, 통계적 언어 모델)** 이 존재했으며 현재에는 **Neural Language Model (NLM, 인공신경망을 이용한 언어 모델)** 이 주로 사용되고 있습니다. 그 둘의 차이는 어디서 기인하는걸까?



언어모델이 풀고자 하는 문제는 다음의 확률을 구하는 것이다. 
$$
p(w_1, ... , w_t) = \prod_{t=1}^{T}p(w_t | w_1, ... , w_t)
$$
여기서 $\prod_{t=1}^{T}p(w_t | w_1, ... , w_t)$ 는 
$$
p(w_1)P(w_2 | w_1)P(w_3 | w_1, w_2)P(w_4 | w_1, w_2, w_3)...p(w_t|w_t-1, ..., w_2, w_1)
$$
즉, 우리는 어떤 단어들이 출현할 확률의 조합으로 어떤 문장이 출현할 확률을 구하고 싶은 것이다. 그렇다면 개별 단어의 출현확률, 혹은 이전 단어를 고려한 다음 단어의 출현확률은 어떻게 구할 수 있을까?

이에 대한 답을 찾기 위해서 아래의 내용들을 바탕으로 학습을 진행하였다. 

> [pilsung-kang/Text-Analytics: Unstructured Data Analysis (Graduate) @Korea University (github.com)](https://github.com/pilsung-kang/text-analytics)
>
> [04: Text Representation I - Classic Methods - YouTube](https://www.youtube.com/watch?v=DMNUVGbLp-0)
>
> [05-1: Text Representation II - Distributed Representation Part 1 (NNLM) - YouTube](https://www.youtube.com/watch?v=bvSHJG-Fz3Y)
>
> [05-2: Text Representation II - Distributed Representation Part 2 (Word2Vec) - YouTube](https://www.youtube.com/watch?v=s2KePv-OxZM)
>
> [n-gram extraction | LOVIT x DATA SCIENCE](https://lovit.github.io/nlp/2018/10/23/ngram/)
>
> [Neural Probabilistic Language Model · ratsgo's blog](https://ratsgo.github.io/from frequency to semantics/2017/03/29/NNLM/)
>
> [2) 통계적 언어 모델(Statistical Language Model, SLM) - 딥 러닝을 이용한 자연어 처리 입문 (wikidocs.net)](https://wikidocs.net/21687)
>
> [3) N-gram 언어 모델(N-gram Language Model) - 딥 러닝을 이용한 자연어 처리 입문 (wikidocs.net)](https://wikidocs.net/21692)
>
> [02) 워드투벡터(Word2Vec) - 딥 러닝을 이용한 자연어 처리 입문 (wikidocs.net)](https://wikidocs.net/22660)



#### Count-based Representation

- Count-based Representation은 SLM 언어모델의 바탕이 되는 어떤 단어에 대한 representation 생성 방법을 말합니다.
- 가장 정통적인 방법으로 이 방법에는 BoW (Bag of Words), TF-IDF, 그리고 n-gram 기법이 존재합니다.
- Count-based Representation에서 생성되는 output의 형태는 다음과 같습니다.

![K-pilsung-crepresent](https://s2.loli.net/2021/12/07/kOwQIdhzeR8Gfcq.png)

- 즉 문서 $D$ 가 row를 구성하고 단어 $W$가 column을 구성하는, 혹은 그 역의 형태로도 구성할 수 있는 feature table을 생성합니다. 

- 이런 table을 구성할 수 있는 가장 기초적인 방법은 one-hot-encoding 부터 Count-Vector, TF-IDF, n-gram으로 다양하게 존재할 수 있습니다.

  - 그러나 다양한 형태에도 불구하고 table안의 value가 가지는 값의 의미는 결국 **Frequency, 등장 횟수**와 연관이 됩니다.

- 따라서 SLM 언어모델에서의 단어의 등장확률은 다음과 같이 예측할 수 있습니다.
  $$
  P\text{(is|An adorable little boy}) = \frac{\text{count(An adorable little boy is})}{\text{count(An adorable little boy })}
  $$

  - 즉 단순히 주어진 corpus내에서 각 단어가 등장한 확률들의 결합확률을 구하면 됩니다.

- 그러나 우리가 쉽게 예상할 수 있듯이 다음의 문제점들이 존재합니다.

  - sparsity : 3가지 측면의 문제가 있습니다.

    - OOV : 우리가 가지고 있는 corpus는 세상의 모든 단어, 내용들을 담을 수 없습니다. 일종의 거대한 sample일 뿐입니다. 그런데 Count-Representation은 등장한 단어에 관해서만 representation을 산출할 수 있습니다. 따라서 corpus에 한번도 등장하지 않은 단어에 대한 확률을 예측할 수 없습니다.
    - Too many dimensions : 개별 단어별 출현 빈도를 파악하기 위해서는 어떤 Corpus에 등장하는 모든 단어 $V$에 관해서 계산을 하고 table을 생성해야합니다. 그런데 이 경우 table의 column이 매우 비대해지는 문제가 존재합니다. column이 비대해지면 그만큼 많은 공간이 0 으로 구성되겠죠.
    - Huge Computational Resource: 주어진 Corpus내에서 구성하는 $V$ 의 단어들을 이용하여 table을 구성하고 이를 이용하여 연산을 하는 것은 매우 많은 컴퓨터 자원을 소요하는 일입니다. 

    ![image-20211203165810838](https://s2.loli.net/2021/12/07/s94d58GYjwzuXtV.png)

  - Vector doesn't represent semantic meaning

    - Count-vector, TF-IDF는 결국 어떤 단어별로 몇 번 등장했는 지를 다르게 나타내는 vector일 뿐입니다.

    - 이는 각 단어간의 어떤 관계, 의미에 상관이 없는 요소이므로 우리가 진정 알고자 하는 단어들 간의 어떤 관계, 혹은 의미는 Count-based Representation으로 알 수 없습니다.

      ![image-20211203165913154](https://s2.loli.net/2021/12/07/lymhLC3WNHs5R8b.png)

      - 이 설명자료와 같이 hotel, motel에 대한 유사도, 내적을 구하고자 하여도 어떠한 값을 산출할 수 없습니다. 그리고 설령 산출한다고 하여도 그것이 의미론적인 유사도를 나타낸다고는 말하기 어렵겠죠.

- 위의 문제들을 고려했을 때 우리는 보다 효율적이고 효과적인 방법으로 LM을 구성할 필요가 생깁니다.



#### Distributed Representation

##### NNLM

**Fighting the curse of dimensionality**

- LM은 어떤 단어, 문장의 동시 등장 확률을 알고 싶다고 하였다.
  $$
  p(w_1, ... , w_t) = \prod_{t=1}^{T}p(w_t | w_1, ... , w_t)
  $$
  그리고 개별 확률은 다음과 같다.

$p(w_1) p(w_2 | w_1) p(w_3 | w_1, w_2) p(w_4 | w_1, w_2, w_3)...p(w_t|w_t-1, ..., w_2, w_1)$
				

우리는 여기서 이전 단어를 고려한 개별 단어의 등장확률 $p(w_3 | w_1, w_2)$ 를 다음의 목표를 추구하며 생성하고자 한다.

1) 각 단어를 실수 차원 $\mathbb{R}$ 벡터 공간으로 표현할 수 있어야 한다 (Continuous Representation)
2) 단어 벡터를 생성하는 함수 $g$와 생성된 벡터를 이용하여 출현 확률을 계산할 수 있는 함수 $f$를 동시에 학습하고자 한다. 
   1) 이 때 $f$ 를 통해 나온 output값은 0 이상의 값이여야 한다.
   2) 특정 단어 t를 예측하고자 할 때 이전 t-1, ... t-n까지의 단어들의 결합확률의 총 값은 1이 되어야 한다.

그래서 우리는 이러한 목적을 달성하는 어떤 모델을 구성하여서 다음의 task를 하고자 하는 것이다. 

![image-20211203171643494](https://s2.loli.net/2021/12/07/FRPd3WzQDlm87ei.png)

- NNLM에서 확률을 구하는 방법은 다음과 같다.
  $$
  p(w_t = j | w_1, ... , w_{t-1}) = \frac{exp(p^{j} \cdot g(x_1, ..., x_{t-1}))}{\sum_{j\prime \in V} exp(p^{j\prime} \cdot g(x_1, ..., x_{t-1}))}
  $$
  그리고 개별 확률을 구하기 위해서 representation을 얻는 방법은 다음과 같다. 
  $$
  f(i, w_{t-1} ... w_{t-n+1}) = g(i, C(w_{t-1}), ..., C(w_{t-n+1}))
  $$
  목적함수 $f$는 $g$와 $c$로 구성된다. 목적함수는 $i$번째 단어의 출현확률을 구하기 위해서 $t$ 이전 시점까지의 단어를 고려하여서 연산을 수행한다. 지금부터 이를 살펴보겠다.

  - $C(w_{t-1}), ..., C(w_{t-n+1})$

    - C는 단어 $w$를 사용자가 설정한 d 차원으로 mapping하는 실수 vector이다. 
    - 따라서 table C의 shape은 $V \cdot m$ 형태를 가진다. V은 단어의 개수, m는 차원 수이다. 그리고 m는 V보다 작은 수이다.
    - 모든 단어에 관한 

    ![image-20211203174755677](https://s2.loli.net/2021/12/07/LQPjdCsF6XYcaZz.png)

  - $g(i, C(w_{t-1}), ..., C(w_{t-n+1}))$

    - $g$는 feed-forward network를 의미한다. 이를 통해 우리는 i번째 단어의 출현확률을 구하게 된다. 
    - 최종 산출 output $y$는 다음과 같다.

  - $y = U \cdot tanh(d + Hx)$

    - 즉 $x$는 $C(w_{t-1})$  를 의미, $Hx$는 $g(i, C(w_{t-1}), ..., C(w_{t-n+1}))$를 의미함을 알 수 있다.

![image-20211203175611003](https://s2.loli.net/2021/12/07/dwt8kFHTbmqihle.png)