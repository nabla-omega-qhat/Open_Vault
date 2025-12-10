= BPNet

# Abstract
- TF binding motif의 arrangement(syntax)는 cis-regulation 관점에서 중요하지만, 아직 모호한 부분이 있음
- 우린 BPNet을 통해, DNA seq–> base-resolution ChIP-Nexus profile of pluripotency TF( cell pluripotency와 연관된 TF들(OCT4, Sox2, NANOG))
- 그리고 **Interpretation tool** 로 predictive motif representation과, cooperative TE의 soft syntax rule을 알아본다.
	- 놀랍게도, Nanog는 helical periodicity를 가지고 있으며 TF들이 방향성이 있게 협력한다는 점도 알아냄(CRISPR로 검증)

# Main
- genome의 regulatory code를 알아내는건 중요
	-  = gene expression, genetic variation, somatic mutation 연관됨
- 수많은 cell type과 tissue에 대해 enhancer들을 매핑했지만, 아직 critical base를 찾거나 regulatory 정보를 아는건 아직 진행중
- 짧은 seq motif가 TF binding에 중요하다는건 알려졌지만, motif combination이나 syntactic arragnment의 영향(in vivo)는 잘 모름
	- 예로, 2개 이상의 strictly spaced motif간에는 composite motif가 형성되어 DNA-mediated cooperation이 이와 연관된 TF들과 함께 발생할 수 있음
	- 하지만 less strict motif(soft motif) spacing의 enhancer/TF cooperativity 영향은 몰루
	- 이런 cis-regulatory code는 아직 풀어야 함(be eludicated)

- 실험적인 방법으로 enhancer seq를 변경하는 방법/증명들은 sequence motif의 존재를 강하게 보여줌
- 하지만 genome-wide 분석에서는 통계학적으로 과추정된 syntax rule을 보이거나 그 존재에 대해 의문을 가짐. 혹은 진화적인 enhancer function의 제약을 제시
	- motif instance들은 과추정된 시퀀스의 PWM(Position weight matrix)으로부터 정의된다는 제약도 존재
	- 패턴이 계산적으로 도출되었음에도 실험적으로 그 기작이나 TF coop까지 설명하긴 어려움
		- 예로, 과추정된 strict motif spacing은 종종 다수의 TF-binding motif가 포함된 retrotrnasposion과 연관됨
	- 반대로 실험적인 TF binding data가 존재(ChIP-seq)할 때, 이로부터 motif syntax를 찾는 일 또한 낮은 해상도와 putative binding(peakcaller의) 문제가 있음

- 따라서 cis-regulatory motif를 찾는 general method가 중요함.
- CNN은 최근 다양한 molecular phenotype 추정에 쓰이고 있고, TF도 그 중 하나.
- CNN은 hierarchial layer를 통해 biological assumption 없이 nonlinear/complex 추정이 가능함
	- 하지만 복잡성=해석 어려움
	- 물론 CNN + XAI로 TF-binding motif를 시각화하려는 노력도 있지만, 규칙을 어떤 motif syntax가 TF binding과 연관되었는지로부터 추정하는 방법을 설명하진 못함.
-

- 다른 limit은 CNN의 resolution.
	- TF binding을 추정하는 SOTA들은 binary binding을 추정하거나 low-resolution(100-200bp)
	- 당연히 이건 다시 TF motif syntax를 찾기 어려워지고, ChIP-seq에서 보여주는 TF Coop도 보기 어려움
	- 예로 TF가 다른 TF motif에 indirectly binding하는 경우가 있음.
	- 이런 TF coop은 ChIP-seq 발전(ChIP-exo, ChIP-nexus)에 따라 더 잘 보이고 있음
		- ChIP-exo : exonuclase digestion 추가
		- ChIP-nexus : 다른 TF coop의 증거도 해준대오
	- 그러니 ChIP-nexus 데이터를 쓰면 coop/cis-regulatory syntax를 high-resolution으로 볼 수 있을 것

- motif syntax를 찾기 위해서 BPNet을 만들었음. (CNN 기반)
- cis-regulatory sequence + TF binding profile @ base resolution
- target은 pluripotency를 가진 Oct4, Sox2, Nanog, Klf4, mouse ESC(embryonic stem cell), ChIP-nexus로.
	- replicated expreiment와 같은, 높은 prediction perfmormance
	- model interpretation으로 new motif를 찾고(statistic하지 않게) predictive influence 설명
	- 모델을 in silico oracle로 써서 motif pair간 거리가 TF coop 에 주는 영향을 봄
		- strict motif spacing들이 주로 retrotransposon 영향임도 보고
		- TF coop이 선호되는 soft motif xyntax에 영향을 받는다는걸 확인
		- TF binding coop의 unexpected rule도 봄(Nanog의 broad preference w/ helical periodicity)

- 결과로 end-to-end NN w/ high-resol genomic data w/ interpretation tool이 discovery에도 도움 줌

# Results
## BPNet predicts TF binding profiles from sequence
- ChIP-nexus를 Oct4, Sox2, Nanog, Klf4를 Mouse ESC 실험.
	- = genome-wide, strand specific base-resolution TF profiles
	- sterotypical footprint 뵝고, higher resolution(than ChIP-seq), increased specificity.
		- Sox2 모티프는 더 sharp chip-nexus footprint 보임
	- 147K genomic region of 1kb length < exhibit chip-nexus signal

- Fig1

- z