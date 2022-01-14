# Recurrent Neural Network (RNN)

---



### 정의

입력과 출력을 시퀀스(Sequence, 연관된 연속의 데이터) 단위로 처리하는 모델

![RNN 구조](https://s2.loli.net/2022/01/14/mh7BdXQVeT68kst.png)



#### RNN 계산식

직전 데이터(t-1)과 현재 데이터(t) 간에 상관관계를 고려해서 다음 데이터(t+1)를 예측, 과거의 데이터도 반영한 신경망 모델 만듦

![스크린샷 2022-01-14 오후 3.08.58](https://s2.loli.net/2022/01/14/mTOUVfrelwELRKo.png)
$$
\phi : 로지스틱 시그모이드와 같은 비선형 함수
$$


#### 이전 Deep Neural Network (DNN) 와의 차이점 - 파라미터

DNN의 경우 파라미터가 모두 독립적 vs RNN의 경우 모두 공유



#### RNN 의 단점(Long-Term Dependencies)

관련 정보와 그 정보를 사용하는 지점 사이 거리가 멀어지는 경우 학습 능력이 현저히 떨어짐

**Why?** RNN의 Backpropagation 알고리즘인 BPTT(Backpropagation Through Time) 가 Long-Term Dependency 학습에 어려움이 있음 

-> weight 업데이트 할 때 1보다 작은 값들이 계속 곱해지면서 기울기가가 사라리는 **Vanishing Gradient Promblem** 발생



#### LSTM

- RNN 알고리즘 중 하나

- RNN의 단점 해결 
  - 활성함수로 ReLU 사숑, weight를 단위 행렬로 초기화하면 long-term 학습 O

![img](https://s2.loli.net/2022/01/14/wGNOJmgLs1YozHa.png)



1. **Input** -> 2. **(Cell) state** -> 3. **Hidden State** -> 4. **Gates**(Forget Gate, Input Gate, Output Gate) -> 5. **Output**(ht) -> 6. **Next (Cell) State** -> 7. Next Hidden State



**Gates**에서 관계 있는 신호(time dependency 있는)가 전파되어 왔을 때는 웨이트를 크게 해서 활성화 해야 함

관계 없는((time dependency 없는) 신호가 전파됐을 때는 웨이트를 작게 해서 비활성인 상태를 유지 해야 함

--> 뉴런이 동일한 웨이트에 연결되어 있으면 두 가지 경우에 서로 상쇄하는 형태의 웨이트 변경이 이뤄지므로  장기의존성 학습이 잘 실행되지 않음



- Forget Gate
  - activation function으로 시그모이드 사용 : output 값 0 or 1
  - 0 -> 이전 cell state 값 모두 '0'되므로 미래의 결과에 영향 X
  - 1 -> cell state 값 그대로 보내서 미래의 결과에 영향 O
  - **현재 input 과  이전 output을 고려하여 cell state의 값을 버릴지 말지 결정**



- Input Gate
  - 현재 cell state 값에 얼마나 더할지 말지를 결정: tanh 사용하므로 -1 ,1 사이 값
    - **이전  cell state의 값을 얼만큼 버릴지 결정**



- Update 
  - Input_gate * current_state + forget * previous_state



- Output Gate
  - **최종적으로 얻은 cell state 값을 얼만큼 output 할 지 결정**
  - Output_gate * update_state





---

[출처]

[1] [순환신경망(RNN)과 LSTM, GRU](https://needjarvis.tistory.com/684)

[2] [Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling](https://arxiv.org/pdf/1412.3555.pdf?ref=hackernoon.com)

[3] [RNN:: LSTM 톺아보기](https://wegonnamakeit.tistory.com/7)



