# Transformer

Overview

![](https://heekangpark.github.io/assets/img/nlp/attention-transformer.png)

**[Attention is all you need](https://arxiv.org/abs/1706.03762)** << 바로 그 논문

Goal : Non-recurrent Seq-to-Seq model

- Seq-to-Seq like Encoder-Decoder model이 자주 사용됨

- 이 경우 Recursive한 RNN의 특성 상, 최종 context vector에 Long term depency 문제가 존재

- CNN은 kernel 간 정보 공유가 안되는 문제

- -> Attention으로만 네트워크 구성

  

Input : 임의 길이 $N$의 문장 

1. Embedding

Input을 $N\cross d$ array $X$ (`<B/Sos> ... ... ... <Pad>`)로 Tokenizing/Embedding

- $N$ : 총 토큰 개수
- $d$ : 입력 임베딩 차원

2. Positional Encoding

 Input에 대해 서로 상대적인 Position 정보를 추가.

$\omega_k = \cfrac{1}{10000^{2k/d}}$로도 표현

 $f(p) = \begin{dcases}sin(\frac{p}{10000^{i/d}}) & (i=2k)\\cos(\frac{p}{10000^{(i-1)/d}}) & (i=2k+1)\end{dcases}$

를 기존 벡터에 **더해서** 표현.

이후 Encoding된 정보를 Encoder/Decoder로.

> **Note**: 아래 링크에서 자세한 정보 보기

3. Multi-head attention

   Multi-head인 이유는 Scaled Dot Product Attention을 여러개(n) 합쳐놔서.

   

   ```mermaid
   graph BT
   
   query_in["Query"]
   key_in["Key"]
   value_in["Value"]
   linear_q["Linear(d fa:fa-times dq)"]
   linear_k["Linear(d fa:fa-times dk)"]
   transpose["Transpose"]
   linear_v["Linear(d fa:fa-times dv)"]
   matmul_qk["MatMul=Attention score matrix"]
   scale["Scale"]
   mask(["(Mask pad token)"])
   softmax["Softmax=Attention distribution"]
   matmul_v["MatMul=Context vector"]
   concat["Cat"]
   linear_o["Linear(ndv fa:fa-times dm)"]
   output["Mult-Head attention output"]
   
   query_in -- N fa:fa-times d --> linear_q
   key_in -- N fa:fa-times d --> linear_k
   value_in -- N fa:fa-times d --> linear_v
   subgraph Multi-Head attention
   subgraph Scaled Dot Product Attention
   linear_k --N fa:fa-times dk--> transpose --dk fa:fa-times N--> matmul_qk
   linear_q -- N fa:fa-times dk --> matmul_qk -- N fa:fa-times N --> scale --N fa:fa-times N--> mask --N fa:fa-times N--> softmax --N fa:fa-times N--> matmul_v
   linear_v -- N fa:fa-times dv--> matmul_v
   end
   matmul_v --N fa:fa-times dv--> concat --N fa:fa-times ndv--> linear_o
   end
   linear_o --N fa:fa-times dm--> output
   
   ```

   1. Linear layer $\$ 

      $X \times W = X_1$, 

      - $X$ : 입력 임베딩 $N \times d$ matrix 

      - $W$ : $X$ 를 $Q,K,V$로 만드는 $d\times(\Sigma d_{q|k|v})$ matrix. $\left[\begin{matrix} Wq &Wk& Wv\end{matrix}\right]$
        - Each element size : $d \times d_{q|k|v}$
      - $X_1$ : $N \cross (\Sigma d_{{q|k|v}})$ output matrix $\left[\begin{matrix} Q \\ K \\ V \end{matrix}\right]$
        - Each element size : $N \cross d_{q|k|v}$
      - $d_q, d_k, d_v$ : 내부 Q,K,V의 차원
        - 다만 이걸 통해서 $d_m$을 결정하는게 아니라, $d_{q|k|v} = d_m/n_{head}$로 결정.
        - 논문에서는 통일해 $d_k$로 표현함

   2. Scaled Dot Product Attention

      $\text{Attention}(Q, K, V) = \text{Softmax}(\text{Pad}(\cfrac{QK^T}{\sqrt{d_k}})V$

      이 과정을 Attention filter size(num_head)만큼 반복.

      - Q : Query. 우리가 찾고자 하는 정보, 현재 시점의 token
      - K : Key. 정보를 찾는 영역, attention하고자 하는 token
      - V : Value. 얻어낸 Inference에 대한 가중치, attention한 context의 weight
      - Transformer의 Encoder에선 $Q=K=V$로, Self-attention으로 입력에서 각 토큰(정보) 간의 영향과 연결을 만드는 걸 목표로.

      1. $QK^T$ : make $N \cross N$Attention filter(score) matrix. 
	      1. = Correlation & Weight to value
      2. $/\sqrt{d_k}$  : Scaling as dimension
      3. $\text{Pad}$ : `<Pad>`였던 단어를 모두 -inf로 보냄(softmax 시 0으로)
      4. $\text{Softmax}$ : make Attention distribution
      5. $\cross V$ : make $N \cross d_k$Context vector

   3. Concatenate

      Columnwise concatenation, $N\cross(n_{head}*d_k)$ output

   4. Linear layer

      $(n_{head}*d_v)\cross d_m$인  Weight $W_0$와 Matmul, Target dimension(Model dimension) $d_m$로 만들기. 

      Final output : $N \cross d_m$

4. Residual Connection

   사진상 Add & Norm 옆으로 들어가는 block 전 입력들이 모두 Residual connection. Add가 결국 이 부분이다.

   ```mermaid
   flowchart BT
   an["Add & Norm"]
   Input --> Block --> an
   Input -- Residual Connection --> an
   ```

   

   $H(x) = x + F(x)$

   Multi-head attention 결과를 이전 입력과 더해줌.

   즉, $d_m = d$

5. Layer Normalization

   위의 Add & Norm의 Norm 부분. 전체적인 가중치를 말 그대로 정규화.

   $\gamma = 1, (1\cross d_{model})$,  $\beta = 0, (1 \cross d_{model})$ 로 Initalization하고 $\gamma, \beta$를 최적화.

   각 row($i : (0, N)$)에 대해서,

   $LN_i = \gamma \cfrac{x_i-\mu_i}{\sqrt{\sigma_i^2 + \epsilon}} + \beta$, $\mu$는 행의 평균, $\sigma$는 행의 표준편차.

6. Feed Forward(Position-Wide FFNN)

   $FFNN(x) = Max(0, x\mathbb{W}_1+b_1)\mathbb{W}_2+b_2$

   - $x$ : $N \cross d_m$ 입력 matrix
   - $\mathbb{W}_1$ : $d_m \cross d_{ff}$ linear layer weight(Squeeze)
   - $b_1$ : linear layer bias
   - $\mathbb{W}_2$ : $d_{ff} \cross d_m$ linear layer weight(Excite)
   - $b_2$ : linear layer bias
   - $d_{ff}$ : 내부 hidden dimension, 2048

   결국 SE-layer처럼 2개의 linear layer로 $d_{ff}$로 보냈다 $d_m$으로 돌려주는 식. $Max(0, ...)$는 ReLU랑 같다.

이 과정을 반복해서 Encoding 후에, Decoder network로 보낸다. Decoder에서는 Encoder와 거이 똑같지만, Masked attention과 함께 Query의 값을 Encoder에서 받아오게 된다.

- Masked Multi-Head Attention
  - Attention score($QK^{T}$) 계산 후에, 마스킹을 해준다.
  - 목적은 앞의 단어에서 뒤 단어를 훔쳐보지 못하게 하는 느낌.
  - Lower triangle Matrix를 elementwise multiplication
- Multi-Head attention
  - Query는 이전 Masked attention에서 얻은 값, Key와 Value는 Encoder network에서 가져온다.

Appendix-Tokens

- `<unk>` : 토크나이저가 알 수 없는 토큰
- `<pad>` : 입력 크기가 정해져 있을 때, 남는 공간에 할당하는 토큰
- `<bos>, <go>` : Begin of string, Encoder 입력 시 가장 처음 주는 토큰
- `<eos>` : End of string, Decoder 출력 시 마지막에 주는 토큰



Links

https://ratsgo.github.io/nlpbook/docs/language_model/tr_self_attention/

https://wikidocs.net/31379

https://velog.io/@jiyoung/Transformer-구현하고-이해하기2

https://velog.io/@jhbale11/어텐션-매커니즘Attention-Mechanism이란-무엇인가#23-scaled-dot-product-attention의-연산-결과는-어떻게-되는가

https://omicro03.medium.com/attention-is-all-you-need-transformer-paper-정리-83066192d9ab

https://cpm0722.github.io/pytorch-implementation/transformer