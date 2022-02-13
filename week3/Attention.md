# Attention

---

Encode-Decode 과정으로 translation 수행했을 때 문제점

![img](https://lh3.googleusercontent.com/-e9nZHuOfYoI/X7o2lW8rkqI/AAAAAAAAOlI/83ixePcGfWQ3ZHoegDuhVJzLh7nYdt8QACLcBGAsYHQ/w586-h293/image.png)

- 1개의 벡터로 문장 표현하므로 정보의 손실 발생
- Vanishing gradient 여전히 존재



##### Attention

기본 아이디어: **decoder에서 단어를 출력할 때 마다 입력 sequence를 한번씩 참고** 

- ex)  '너'를 예측하기 위해 'I', 'love', 'you'를 한번씩 참고하면서 연관성이 있는 단어가 있는지 **집중(attention)**하는 방법



##### Attention 원리

![img](https://lh3.googleusercontent.com/-AGTfLtkCIXQ/X7pGnrUCsII/AAAAAAAAOl8/3HejK0NMAxIFHSGKVWY_Ns4NganqzhNhwCLcBGAsYHQ/w332-h352/image.png)



- 일반적인 Encoder-Decoder RNN의 context vector는 마지막 hidden state가 그대로 Decoder에 입력됨

  

Attention

- Decoder의 hidden state와 Encoder의 hidden state의 **Attention score α**를 구함

  - α가 클수록  Decoder이 hidden state와 Encoder의 hidden state와이 관계가 강함
  - α의 합은 1이 되도록 softmax 

- Encoder의 hidden state와 Attention score를 곱하여 contect vector( c_t ) 구함

  

![img](https://lh3.googleusercontent.com/-u3EHvSdEZ-w/X7pMnvQ0FjI/AAAAAAAAOmw/83T2JKtfcd4wZRgyRP2HpsI9Ep7rZd9xwCLcBGAsYHQ/w384-h217/image.png)

- s_t를 예측하기 위해 s_t-1, y_t-1, c_t가 필요

- 여기서 c_t는 s_t-1과 h_t간의 amount of well matched의 벡터
- **s_t는 y_t를 예측하기 위해 ht'에 얼마나 주목(attention)할 것인가를 반영**
  - 비교 기준(s_t-1): Query
  - 비교 대상(h): Key
  - 출력되는 값: Value
- 긴 문장이어도 attention score를  통해 잘 예측할 수 있는성능 보임



**Attention(Q, K, V) = Attention Value**

- 어텐션 함수는 '쿼리(Query)'에 대해서 모든 '키(Key)'와의 유사도를 각각 구함
- 구해낸 이 유사도를 키와 맵핑되어있는 각각의 '값(Value)'에 반영
- 유사도가 반영된 '값(Value)'을 모두 더해서 리턴 **-> Attention Value**



> Attention: 단어와 단어 사의 상관관계를 나타내는 가중치



##### Transformer에 사용되는 3가지 attention

![img](https://wikidocs.net/images/page/31379/attention.PNG)



Encoder Self-Attention -> 인코더에서 이루어짐

Decoder Self-Attention, Encoder-Decoder Attention -> 디코더에서 이루어짐

- 셀프 어텐션은 본질적으로 Query, Key, Value가 동일한 경우

  - 벡터의 값이 같다는 것이 아닌 벡터의 출처가 같다는 것

- 3번째 그림은 Query가 디코더의 벡터인 반면, Key와 Value가 인코더의 벡터이므로 셀프 어텐션이라고 부르지 X

  









---

[1] [Attention in RNN-Encoder-Decoder](https://sonsnotation.blogspot.com/2020/11/11-attention-transformer-models.html)

[2] [트랜스포머(Transformer)](https://wikidocs.net/31379)

