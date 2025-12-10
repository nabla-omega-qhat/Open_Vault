https://www.nature.com/articles/s41467-025-57271-1
TotalReCall

# Abstract
- L1의 somatic mobility <(implicated)> cancer etiology
- Paired TCGA WGS + RNA
- TotalReCall : detect L1 Retrotransposition pipeline
- also, model p53’s dual regulatory role.
	- *TP53* mutation disrupt L1, LFS(Li-Fraumeni Syndrome) rel.

# Introduction
- >50% human genome : repeat seq
	- are silenced by epigenetic + other path. 
	- = Oncogenic process disrupts this pathway
- Cancer에서 이런 repeat들이 RNA로 발현되면, protein이 되거나 genome instability, cancer immunogenicity에 연관
- 그 중 L1, reinsert 가능성이 존재(retrotransposition)
	- ~6kb length, >50k copy, ~20% human genome.
		- 대부분은 upstream truncated(3’)
	- full-length L1s는 100개 정도
	- 이 과정에서 L1RNA는 protein coding + insert template로 기능

- L1의 영향을 보기 위해선ㄴ 이의 activation을 quantificaiton해야 함
	- = retransposition, expression
	- intact l1은 mutation→duplication 과정에서 독립적, heterogenous하게 영향
- 여기선 L1 loci가 서로 다르다는(expression 등)을 제시
- in-vivo retrotranspotition ~ L1 RNA expression level

- short-read로 L1 activity를 계측하는건 어려움.
	- 비슷한 L1 sequence들.
	- 기존 xTea와 함께, retrotransposition consensus를 call하는 program

- Fig1
	- Retrotransposition (a-d)
		- d : twin priming, reversed l1 repaired
	- e, f : repaired genome structure(TSD - L1 - polyA - TSD)
	- g : mapping read. w/ split reads

- Large-scale TCGA 셋에서 L1 RT burden region을 4669 T/NT pair(31 tumor types)에서 찾음
- 추가로, L1 expression 이ㅏ L1 retrotransposition과 연관. 그리고 시간이 지나면서 이런 retrotransposiition 이 쌓임도 확인
- 각 loci들은 heterogenious하게 동작하는 걸로 보임.(기존 in-vitro exp와 동일)

- L1의 pathogenic feature로, 여러 단계의 regulation mechanic 이 있는건 당연
- 우리 분석에서 이런 mechanic을 밝힌 부분
	- *TP53* mutation rel w/ L1 retrotransposition
	- 추가로 최근 p53이 직접 L1 RNA level을 조절하는 걸로 밝힘
	- 통계적으로 우린 위와 같은 L1 RNA transcription 을 p53이 그 level과 관계 없이 조절하는걸 보임.
	- LFS(germline *TP53* variant)에서 L1 activity를 본 결과
		- LFS tumor가 non-LFS tumor보다 더 많음을 확인
		- 추가로 연관 gene도 밝힘

# Results
## Many cancers are significantly enriched for L1 retrotransposition
- TotalReCall + xTea로 64k somatic L1 retrotransposition 확인(in 4669 T/NT WGS)
	- 40%는 inversion L1 포함
		- 두 프로그램 모두 확인
		- 그리고 Genome in a Bottle의 long-read dataset으로도 검증
		- Cancer간 inversion reaate는 비슷(low @ prostate cancer, High @ uterine tumor)
		- somatic inversion-contain L1 > germline
	- 356k canonical(no-inversion) 에서 6%의 canonical insertion이 full-length
- Fig2

- 이 방법을 통해 tumor-specefic somatic(non-germline) L1 retrotransposition 찾기가 목표
	- 그래서 germline을 찾으려곤 안 했는데, control과 case sample 모두에서 나타나는 non-reference insertion인 pesudo-germline call을 분류하긴 함
	- 이런 insertion들의 길이나 allele frequency는 알려진 L1과 유사

- 샘플 단위로 RT 위치를 나누면 consistent. = increased sensitivity
- 여러 cancer type에 대해서 비교 : least 5 samples, Median RT n : 1 and background 0 , 
	- 이 방법으로 L1 RT enrichment를 cancer type에 대해 확인

- 15% 정도의 insertion이 3’ transduction을 포함
	- 그리고 그 중 절반정도는 genomic source도 찾음
	- 이들을 transduction-bearing RTs(TRTs)라고 부르고, 726개의 unique genomic source를 찾았으며 이들은 polymorphic non-refernc eL1s가 많았음. > Suppl. data 1

- 다른 논문에서 밝힌 198개의 active genomic loci가 있었는데 우린 102개의 retrotransposition을 찾음
	- 20개는 서열 확인 못함
	- 10개는 in-vitro에서 확인됨
	- 13개는 in-vivo cancer에서 확인되지 않음
	- 137 loci는 sequence-resolved
		- 이 중 68개는 hg38 reference gnome에 있음
		- 87개는 L1EM 내 active element로 존재. = RNA 결과와 비교 가능?
			- 19개는 L1PA2, 41은 reference genome sequence와 intact하지 않아 보임

## High L1 RNA corresponds to high L1 RT burden
- TCGA에서, RNA-0seq 데이터도 있음. (8998 individuals, 32 tumor types w/ normal from 719 individuals, 32 tumor types)
- 여기서 locus-level L1 RNA active expression(L1 promotor로 transcription된 RNA) 를 L1EM으로 quantification. 이후 TPM으로 loci들을 aggregation.
- 이를 통해 total relative abundance of active L1 RNA in each sample. 계산(suppl. Method + Suppl Fig2)

- Retrotransposition의 source가 L1PA2로 annotation된 경우 중 Intact ORF가 없는 경우가 reference genome에 있어, L1Hs, L1PA2로 quantification된 RNA expression을 L1EM으로 합쳐 potential functional loci로 보기로 함
	- 이렇게 합쳐진 expression은 esophageal(식도) carcinoma, lung squamous cell carcinoma(편평세포암), stomach adenocarcinoma에서 높게 나옴
	- Mann-whitney U test 상 높은건 편형세포.
	- normal에서 L1 RNA가 가장 높은건 protstate tissue
- Fig 3

- expression analysis는 우리가 찾은 결과와 이전 알려진 위치에서 비슷한 결과 얻음 ?
	- subset of 121 element?
- Aggregate L1 RNA expression은 1483개의 superset과, 121개의 subset에서 거이 비슷한 proportion을 보임
	-  L1EM은 “only”(L1의 poly-A site에서 끝나는 transcript)와 “run-on”(L1의 poly-A를 지나 genomic sequence downstream, 3’을 포함한 RNA)의 quantification을 제공하는데 양쪽 다 해서 비슷한 결과 얻음

- L1 retrotransposition과 L1 expression을 연관짓기 위해서, 같은 tumor type에서 DNA, RNA statistic을 합침
	- 11개의 per-sample median RT burden > 0인 tumor type에서 L1 expression이 다른 그룹에 비해 유의하게 높았음
	- 3879개의 tumor sample이 WGS와 RNA-seq 데이터가 모두 있어 sample level RT와 L1 expression 비교 가능
		-  이 중 5개 이상 sample이 있는 30개의 tumor type에서 RT와 L1 expression으로 순위를 매기니 비슷하게 순위 나옴
		- 이 과정에서 sample의 RT, L1 esxpression을 sequencing qualty로 보정했음.
		- 둘이 연관 있음을 보임.
- Fig 4

## Locus-level analysis of L1
- 찾은 RT 중 일부만이 progenitor를 특정했지만, RT burden과 RNA expression을 locus level로 비교 가능했음
- Tumor type에 무관하게 대부분의 L1은 RNA expression minimal, TRT burden avg 0
- non-zero expression인 element는 tumor type 따라 달랐음
	- 비슷한 TRT, RNA 패턴을 보이는 loci끼리 clustering(Suppl.3, Methods)
	- expression 따라 비슷한 패턴 보임. 높은 expresion은 앞서 말한 esophageal, lung sqamous cancer, element로는 22q12.1이랑 20p11.21-1, 
	- 3p22-1.1 locus는 tmumor type에 따라 특이한 패턴 보임. 
- Fig5

- 이 locus-TRT burden across tumor type, loci clustered에서 차이 관찰
	- (RTR burdern, cancor type 따른 loci 연관)

- RT와 RNA 관계를 보는 도중, 위에서 말한 Xp22.2-2가 낮은 expression에 비해 많은 TRT를 만들고 있음.
- 22q12.1은 반대로 expression은 높으나 조금의 TRT.
- 우리의 detectability issue일수 있기에, 이 두 loci 중 하나라도 TRT를 가진 sample들에서 다시 확인
	- 22q12.1은 상ㄷ애적으로 많은 RNA, Xp22.2-2만 있는 sample에서도.
	- 몇 tumor는 양쪽 locus allele을 가지나 RT할 수 없도록 ㅏ지고 있어 ratio에 영향을 줄 수 있었기에, functional allele를 가진 sample로 추려냄
		- 그래도 TRT per RNA ratio가 Xp22.2-2에서 높았음.
		- = L1의 RT activity 차이 있음

- Xp22.2-2라는 예외를 제외하면, RNA와 RT 차이가 크게 나는 곳은 TRT.
	- 가장 큰 클러스터를 제하고 RT, RNA
>[! NOTE] 누가 한 문장에 쉼표를 6개씩 넣어요 무슨 래퍼신가
>Xp22.2-2라는 예외를 제하면, RNA와 RT간 간극이 컸던, 확인한 TRT 값에 기반한 경우, 몇 하위 locus 클러스터에 속하는, tumor type에 관계 없이 TRT가 없지만 RNA 발현에 패턴을 가지고 있던, 하지만 가장 큰 클러스터에서 예외를 가진, 양쪽 RNA와 RT activity가 가장 낮은.
- :lost:
## Quantifying the efficiency differences between L1 loci
- locus efficiency를 계측하기 위해, regression model. (locus TRT vs locus RNA)
	- attr로 tumor type, p53 mutation을 covariatef로 2개 이상의 tumor에서 TRT가 관찰된 48 loci로 
	- effiency : low expression 하더라도 RT는 많이 함.
	- tissue-specefic 위해서 해당 정보도 포함
	- p53은 L1 RT regulator로 de-biasising 위해서

- lung squamous cell tumor에서 22q12.1 locus와 xXp22.2-2 locus 차이가 보임.
	- 비교 위해서 resmample한 background와 서로 비교해서 확인
	- 해보니 loci간 variance가 크게 차이남.
	- = RT efficiency가 L1 Loci에 따라 차이 있음
	- 이전 in-vitro activity와 비교해보니 correlation 있음.
- Fig6

- reference loci의 TRT로 locus-specefic 분석을 했지만, 전체 RT call의 2.5%밖에 안 되고 non-transduction bearing RT로 인한 결과를 반영하지 않음
	- 그래서 locus-level RNA vs total RT per sample로 regression(lkinear)
	- = TRT가 아니더라도, 모든 loci의 RT activity와 RNA 연결해 보

## p53 independently regulates L1 RNA and L1 RT burden

## Extension to germline predispositions and other cancer driver genes

# Discussion

# Methods

# Re
- RNA expression, expression, level <> RT, insertion, count, burden
- 되게 비문 느낌이라, locus-level, loci, aggregates, cluster들이 뭘 가리키는지 잘 봐야 함
- TRT에 되게 집중을 많이 하는데, TRT가 RT에 비해 더 특징적인게 있나?
- L1 expression 간 간섭(cis-trans 하게 regulate or activate) + 찾은 loci의 부정확성으로 좀 어려운 분석이 맞음;;
	- 이 부분에 대해서, casuality inference할게 있나

- 특이하게 
# Takeaway


- p53 regulates transcription and RT both, but *TP53* variant … / rel w/ `*IDH1, ATRX*`




# Supplementary
## Supplementary Informations
### Supplementary Methods
#### L1 retrotransposition detection from short-r3ead whole genome sequencing data

- Retrotransposition : quant 하면 어디서 왔는지 알 수 있음. 문제는 L1 seq들이 다들 비슷비슷함 + insertion시 다양한 mode(transduction 등) 존재
	- 정확한 L1 source나 위치 찾기 힘듬
- TotalReCall은 2개 signal을 중심으로 봄
	- TE breakpoint를 spanning하는 chimeric read(soft clipped)
	- Paired-end이나 서로 다른 곳(L1 + Genome)으로 map되는 discordent pair

- clipped read를 primary로,(더 specefic해서) clipped read는 breakpoint를 support
- Class I Transposable Element : RNA intermediate(Retrotransposition)
	- Class I LTR Transposon : likke retrovirus
		- synth dsDNA from RNA including LTRs, (L:ong Terminal Repeat) then integrate
		- Always integrate completely
			- (Sometime, internal provirus part may removed by homologous recombination > Leave solo LTR)
	- Class I non-LTR Transposon : L1, SINE, SVA
		- Directly TPRT
			- May terminate prematurely, partially integrated
				- like 5’ truncation : from twin priming 
				- 3’ transduction : RTase overruns polyA signal at 3’
- Class II : DNA intermediate

- TotalReCall tries detect 5’ truncation / L1 inversion.
	- more rare and complex struct will left for long-read seq

- TotalReCall : canonical retrotransposition + inversion-containing retrotransposition
	- canonical / single-stranded retrotransposition : single segment of L1 >> Genome
		- can be detected by two clipped reads
- 이후 sort by length(dec), cluster by cd-hit-like way
	- cd-hit-like : 각 시퀀스를 이미 존재하는 representative에 align. match하면 assign cluster. 아니면 new representative
- 이렇게 하면 right clip(3’ @ ref) / left clip(5’ @ ref)들이 clustering, 
	- 그중 가장 긴 seq가 cluster의 representative of breakpoint
	- 만약 5’ 방향으로 맞추면, canonical retrotranspostiion으로 생각. (polyT 존재)
		- 3’ transduction 여부와는 무관
		- 그럼 다른 breakpoint는 L1의 positive 방향으로 mapping
		- 해당 coordiante는 transposon length infer에 사용(w/o 3’ transduction)
	- Insertion-containing L1 retrotransposition은 clipped read가 polyT 를 가지고 있으면 (3’ transduction 존재와 무관)서 + 다른 breakpoint가 L1의 negative 방향으로 align되면.
		- supplementary alignment와 hardclipped part를 써서 sensitivity 올림

- Discordant read pair와, breakpoint 인근의 read도 confidence 올리기 위해 사용
	- clipped seq / discordant read는 L1 consensus에 LAST aligner로 align.
		- L1 consensus 는 세 DFAM sequence(DF0000225 226 316)의 3’, 5’ end와 ORF2 subdomain을 합쳐서 만듦
		- DFAM의 ambiguous character는 L1base의 intact l1 seq에서 가져와 채움

- somatic L1 retrotransposone(like tumor-specefic insertion) 확인을 위해 모든 call을 case / control에서 비교.
- 추가로 low complexity / alignment artifact(high entropy region)들에 대해 filter 사용
	- 한 위치에 너무 많은 clipped read가 있으면서 여러 시퀀스가 존재하거나
	- genomic sequence가 clipped와 matching되는 low complexity region 등

- 이후 500개 정도의 스크린샷을 리뷰(TCGA-LUSC에서 L1 call)
- 이걸로 naive bayesian classifier(scikit-learn)를 train.
	- 사용한 변수
		- bitscore for local alignment of clipped seq at L1 5’
		- bitscore for local alignment of clipped seq at 3’ to polyA-L1 3’
		- length of unaligned end of 5’ of clipped seq’s 5’
		- length of unaligned end of 3’ of clipped seq’s 5’
		- number of cllipped reads with matching seq at 5’ end breakpoint
		- low complexity filter flag
		- not properly mappd as pair, mate map to L1 / 를 5’, 3’ end, clipped with matching 여부
	- 해서 short read로는 어려운 3’ transduction 확인 가능
		- 너무 짧은 3’ transduction 에선 fail

#### Non-reference LINE-1 insertions present in both case and control samples
