https://www.nature.com/articles/s41596-022-00692-9
ATAC-seq

# Abstract
- ATAC-seq(Assay for transposase-accessible chromatin using sequencing) 은 chromatin landscape 보기 좋음
	- 그리고 꽤 적은 수의 cell을 요구하면서, TF, epigenitic mark에 관한 priori knoledge도 요구하지 않음
- 여기선 updated protocol인 Omni-ATAC을 소개. (여러 cell / tissue에 applicable)
- 전체 workflow는 5-step 구성으로, 
	- sample preparation
	- transposition
	- library preparation
	- sequencing and analysis
- 여기선 전체 디테일과 추천 분석법도 소개.
- 12 sample에 대한 ATAC-seq은 약 10h에 가능!
- 하위 분석은 pipeline되어 있음!

# Introduction
- cell state에 따른 mapping alteration은 bio system 이해에 중요한 관점
- 그리고 cell state는 development, differentiation, disease 등으로 변하고 expression으로 조절됨
- 최근에는 gene regulatory program은 TF activity가 조절하고, interpret + alter chromatin 한다는 점을 앎
- chromatin의 epigenetic state는 다양하게 조절
	- DNA, histone의 chemical modification
	- 혹은 더 high-dimensional chromatin struct mdynamics
- 우린 chromatin이 다양한 상태로 존재 가능함을 알고, 여러 epigenetic modification + gene regularitary pattern이 연관
- 이 스펙트럼의 양 끝은 : 
	- enhancer, promotor, insulator와 같은 DNA-binding regularoty element
	- chromatin의 inactive region(silenced, poised)가 gene expression mech.에 영향
- 따라서 chromatin state를 알아내는 건 gene expression pattern에 가해지는 옇양을 볼 수 있음(shedding)

- epigenome는 다양한 방법으로 seq 가능
	- specific method는 특정 modification을 잡고 ChIP-seq.
	- 최근에는 antibody-based DNA-protein interaction을 보는 기법의 sensitivity가 늘어 ChIC(Chromatin Immunoclevage) 기법인 Cut&Run, Cut&TAG가 나옴
		- nuclease나 Tn5 transposase를 targeting AB에 protein-A로 붙임
		- 이러면 protein binding site의 resolution이 오름
			- = IP down steop이 없어지고, material도 덜 먹음
		- 다양한 파생 : ChIPmentation, CoBATCH, ChIL-seq, scChIC-seq…

- 몇 경우엔 더 broad한 landscape을 얻는게 중요할 수 있음. (phenomenon만 알려진 상황 ; 특정 epigenetic change는 불명)
- 그래서 전체 landscape를 보려는 strat.도 존재 : 보통 모든 TF binding site/TF identities
	- DNase-seq, FAIRE-seq, MNase-seq, NOMe-seq…
	- 처음은 DNase-seq로, DNase digestion으로 chromatin state seq
		- = hypersensitive site를 unbaised.
		- 일종의 TF footprinting의 golden-standard
	- MNase-seq는 비슷하게 MNase(endo-exonuclease > DNA)
		- nucleosome으로 보호된 곳도 못 잘라서, nucleosome occupancey를 볼 때 

- 다만 DNase-seq나 MNase-seq는 복잡하고, 오래 걸리고, 많은 cell과 재료가 필요
- chromatin의 agnostic profiling을 위해 ATAC-seq
	- 잘 조정된 hyperactive Tn5 transposase를 sequencing adaptor와 preload
		- = accessible chromatin 찾기
	- ATAC-seq은 2개의 발견에 기반
		- 1 : Transposase는 이전에 tagmentation library에 사용되었는데, Tn5 transposase가 sequencing adaptor와 preload되어 계속 genome을 fragmentation + tagging해 high-throughput prep
		- 2 : in-vivo Tn5가 효과적으로 nucleosome-free 영역에 insertion
	- 이렇게 ATAC-seq은 genome-wide regulatory map을 DNase-seq와 비슷하게 얻을 수 있음. 하지만 더 빠르고 간단하게!
	- <50k cell require, short time(10h), …
	- 여기선 말했듯 업데이트된 Omni-ATAC 프로토콜

## Application of ATAC-seq
- ATAC-seq는 쉽고 scalable하게 TF가 bound한 위치를 assay
- in-vitro transposition으로 adaptor를 chromatin에 넣고(insertion, Tn5 dimer가 cut-and-paste). 
- 동시에 DNA가 fragmentization, 쉽게 amplification 가능
	- 즉, sequenceable DNA fragment는 두번의 transposase insertion event에서 나옴
- 다만 정확한 Tn5 transposition site는 완전히 밝혀지지 않음
	- 보통 TF가 DNA에 붙을 때, Nucleosome-free-region을 만들고 이게 Tn5 transposition을 늘릴 것이라고 생각
	- 이건 Suppl 1로
- Fig1

- 여러 profiling으로, Tn5-accessible chromatin이 promotor/TSS(Transcription start site) 인근임을 밝혔고 intergenic region 중에선 enhancer, insulator, silencer 등과 연관됨을 보임
- 이런 Tn5 접근성 패턴/위치를 보아 특히 말단 부분에서 cell type/cell state specefic하게 나타남
- = ATAC-seq이 어떻게 cell 이 expression 조절하는지 잘 보여줌
- procewssing & Alignment 후에 얻는 Tn5 retrotransposition enrichment는 genomic region에서 어디가 Tn5-accessible chromatin인지 볼 수 ㅣㅇㅆ음
	- = 이걸 보통 ATAC-seq peak라고 말함
	- 이 peak region에서 chromatin accessibility signal은 다른 영역과 비교 가능
	- 예로, 특정 putative gene target에 해당하는 peak를 orthogonal chromatin conformation capture dataset이나 genomic distance 등으로 얻어 link 가능
	- 종종 이미 잘 알려진 promotor와 gene의 경우, ATAC-seq peak가 expression activity로도 해석 가능
	- RNA-seq이 직접 expression quant. 는 가능하지만, ATAC-seq는 condition 등에 따른 mechanism을 보여줄 수 있음

- 일반적으로 ATAC-seq는 novel inhancer나 regulatory region을 특정 context, cell type에서 알아내는데 사용됨
	- 예로, gene A의 5kb upstream의 TSS를 cell type X에서는 ATAC-seq peak가 나타나나 cell type Y에서는 보이지 않을 때, cell-type specefic enhancer가 gene A 발현을 cell X에서 조절하고 있다고 추정 가능
	- 이런 방법은 stimuli를 가하기 전후로 비교하거나, development timepoint, 여러 cell type 등에서 비교 가능.
- 반대로 regulatory element의 activity를 여러 disesase array 에서 찾앗다면 ATAC-seq으로 gene regulatory landscape를 큰 cohort에서 분석 가능
- 혹은 ATAC-seq을 통해 GWAS(Genome-wide association studies)의 disease-associated genetic variant를 mapping하는 데 쓸 수 있음
	- 역사적으로 GWASs는 SNP(Single nucleotide polymorphism)을 찾았으나,(보통 noncoding에 있음) 직접 functional impact를 보이긴 어려웠음
	- ATAC-seq으로 Interested regulatory element를 disease-relevant cell type에서 확인해 GWAS SNP와 비교함으로써 특정 SNP가 질병과 연관되었는지 가설 세울수 있다.

- ATAC-seq peak는 TF motif sequence의 존재를 위해 annotation되거나, enrichment test를 통해 여러 driver에 따른 chromatin accessibility 차이를 볼 수 있음.
- 이런 motif-based 분석은 다른 cell type나 현재 state(disesase state, development stage..)에 따른 trajectoryㅇ에서의 differentiation이나, in-vitro cell과 in-vivo equivalents 비교 등에 사용 가능
- 위의 cell type X / Y의 toy example을 가져오면, cell type X-specefic ATAC-seq peak @ 5kb upstream of gene A는 TF B에 bound, 하지만 cell type Y에선 발현하지 ㅇ낳음. 이런 cell type-difference in TF expression은 TF B motif의 다른 peak subset에 대해 X-Y간 enrichment 차가 나올 것.
- 이런 분석법은 cell state에 따른 TF usage를 볼 수 있음.
	- 예로, small-cell lung cancer의 metastasis중 inflammatory stimuli 여부에 따라/reprogramming fibroblast into neuron에서.
	- ATAC-seq은 nucleosome 위치나, TF binding에 따른 chromatin reugulation의 insight를 줌.
- 종합하면, ATAC-seq는 gene regulatory change를 밝히고 왜 cell이 특정 gene을 발현하는지와, 어떻게 gene expression change가 유도되었는지를 봄

## Comparison with other chromatin profiling methods
- 다양한 기법을 통해 DNA regulatory element를 확인할 수 있음.
- Table 1에 DNA regulator element mapping에 쓰이는 여러 기법을 정리함 : ATAC-seq, DNase-seq, MNase-seq, ChIP-seq, targeted CUT&TAG
	- 어떤 정보를 얻고 싶은지(1), 그리고 어떤 물질이 있는지(2) 에 따라 구분.
	- 보통 epigenomic profiling은 gene regulatory change를 볼 때 좋음
	- 뭐가 변하고 있는지 보고 싶다면, RNA-seq으로 시작하는건?
- Table1

- …
## Comparison with previous ATAC-seq methods
- Omni-ATAC은 mDNA mapping을 줄이고, SNR을 여러 cell line이나 frozen cell에서 높인 protocol.
	- 이는 cell lysis 과정과, nucli isolation, transposition 과정을 최적화해서 이룸
- …

## Experimental Design
### Breif overview
- ATAC-seq은 다섯 단계로 나눌 수 있음
	- Input material preparation
	- Transposition
	- Library preparation
	- Sequencing
	- Data analysis
- 프로토콜에서는 transposition과 preparation 단계에 focus를 둠.
	- + 어떻게 시작 물질을 얻고, 어떻게 만든 library 를 sequencing 할 지.
- 그리고 여러 pipeline과 software tool을 보여줄 예정. : Data analysis section에서 overview 가능
- Fig 2는 Wet experiment의 overview 가능
- Fig 2
	- 