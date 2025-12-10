https://academic.oup.com/bioinformatics/article/27/8/1157/227268?login=true
# Abstract
- HMM to reduce false SNP calls, by misalignment
- using per-Base-Alignment quality

# Introduction
- one of source of SNP error : indels
- currently, we re-align and filter SNP around indels but it is intensive

- Terminology
	- *read mapping* : interval between first and last reference base in alignment
	- *read alignment* : coordinate pair of read and reference base

- Wrong alignment caused by ambiguity of indel, and it deceive SNP callers

- Model HMM and compute BAQ(per-Base Alignment Quality) to evaluate probablity of misalignment, improve SNP accuracy

# Methods

## The profile HMM for computing BAQ

- Reference nucldotide sequence $x = r_1 r_2 r_3… r_L$
- Read sequence $y = c_0 c_1 … c_{l+1}$
	- $c_0$ = `^`, marker for start
	- $c_{l+1}$ = `$`, marker for end
- Substitution probility of $c_i$ : $\epsilon_i$, ($i = 1…l$)
	- = Max(Scale mutation rate, Sequencing error probablity)
		- calculated from base quality
- State Index : M(Match), I(Insertion), D(Deletion), S(Start), E(end)
	- transition matrix $(a_{ij})_{5\times5}$ 
	- =$\left[\begin{matrix}(1-2\alpha)(1-\gamma) & \alpha(1-\gamma) & \alpha(1-\gamma) & 0 & \gamma \\ (1-\beta)(1-\gamma) & \beta(1-\gamma) & 0 & 0 & \gamma \\ 1-\beta & 0 & \beta & 0 & 0 \\ (1-\alpha)/L & \alpha/L & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0  \end{matrix}\right]$
	- $\alpha$ = site-independent gap open probability
		- S/M → I/D : 뭔가 Match 외 확률이 일어날 정도
	- $\beta$ = gap extension probability
		- D→D / I→I : D/I가 연장될 정도
	- $\gamma = 1/(2l)$ = controling length
		- 전체 노드 순환이 L이 되도록 확률 조절, M/I → D
- Emission probability
	- S emits `^`
	- E emits `$`
	- D is silent
	- Emission probability from $I_k$ = 0.25
		- A / T / C / G
	- Emission probability from $M$ = $\{\epsilon_i\}, i = 1,…,l$
		- $P(c_i|M_k) = e_{ki} = \begin{cases} 1-\epsilon_i & \text{if } r_k = c_i \\ \epsilon_i/3 & \text{otherwise } \end{cases}$
		- k번째 위치에서 Matching할 때, c_i 염기가 방출될 확률 c_ki는
		- 실제로 matching한다면 1-epsilon(error가 아닐 확률)
		- 아니라면 epsilon/3 (error이고, 원래가 아닌 염기가 나올 확률)

## The forward and backward algorithms
- Forward, recurrence equation
	- At initalization step
		- $f_{0,S} = 1$
			- 상기 모델에서 0번째 위치가 S일 확률 = 1
		- $f_{1,M_k} = e_{k1}a_{30}$
			- 1번째가 Match일 확률 : e(emission prob.) 와 transition
		- $f_{i, I_k} = a_{31}/4$
			- 1번째가 Insertion일 확률 : transition
	- for $i \in [2,l]$, $k\in[1,L]$
		- $f_{i,M_k} = e_{ki}[a_{00}f_{i-1,M_{k-1}}+a_{10}f_{i-1,I_{k-1}}+a_{20}f)_{i-1,D_{k-1}}]$
			- emission prob 에 각 이전 transition
		- $f_{i,I_k} = (a_{01}f_{i-1,M_k} + a_{11}f_{i-1,I_k})/4$
		- $f_{i,D_k} = a_{02}f_{i,M_{k-1}}+a_{22}f_{i,D_{k-1}}$
	- for i = l+1(end)
		- $f_{l+1, E} = \Sigma_k(a_{04}f_{l,M_k} + a_{14}f_{l,I_k})$
- Backward
	- Init
		- $b_{l+1,E} = 1$
			- 상기 모델에서 l+1이 E일 확률 (1)
		- $b_{l,M_k} = a_{04}$
			- 마지막이 Match일 확률 = transition
		- $b_{l,I_k} = a_{14}$
	- for …
- 

- Check Viterbi algorithm for real implementation
## Computing BAQ
- Alignment $A$ = set of coordinate pair $\{(i_1, k_i)… (i_p, k_p)\}$
- $\kappa _i = \kappa ^A _i = \begin{cases} k & \text{if } (i,k) \in A \\ 0 & \text{otherwise} \end{cases}$
	- =coordinate of reference base that read base c_i aligned to
- $BAQ = Q(i|A) = -10\text{log}_{10}[1-\text{Pr}\{i\text{-th read base aligned to }\kappa_i | A\}] =  -10\text{log}_{10}[1-\cfrac{f_i , M_{\kappa_{i}} \cdot b_i , M_{\kappa_i}}{f_{l+1}, E}]$
	- fi,M , b,i,M are forward and backward probs.
	- As Q(i|A) above 40, so logarithm scaled.
	- In SNP calling, update i-th base quality to $\text{min}\{q_i,Q(i|A)\}$
		- q_i : original base quality
		- = capped BAQ?

## Numerical stability and banded acceleration
- To avoid floating point underflow @ long sequence,
- $\tilde{f}_{i,\bar{k}} = \cfrac{f_{i , \bar{k}}}{\prod_{j=0}^{i} s_j}$, $\tilde{b}_{i,\bar{k}} = \cfrac{b_{i,\bar{k}}}{\prod_{j=i}^{l+1}s_j}$
	- $\bar{k}$ = any state
	- s_i : scaling factor to make $\Sigma_\bar{k}\tilde{f}_{i,\bar{k}} = 1$
	- 