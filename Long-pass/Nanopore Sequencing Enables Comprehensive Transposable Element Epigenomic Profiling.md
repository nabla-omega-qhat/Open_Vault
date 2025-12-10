https://www.cell.com/molecular-cell/fulltext/S1097-2765(20)30731-0
# Summary
- TE → Genome evolution but also pathogenesis
	- CpG methylation이 TE 활성을 조절
	- 하지만 locus-specefic TE methylation landscape는 사실 알기 힘듬
	- Nanopore로 확인해 보겠습니다.

- LINE-1 demethylation이 cancer에서 봄
	- 다른 TE와는 다르게
- SVA(SINE-VNTR-Alu) retretroposon 내의 tandem repeat-associated CpG island도 광범위하게 methylated
	- SINE - VNTR(Variable Number Tandem Repeat) - Alu
	- Shape : CCCTCT(n) - Alu-like - VNTR - SINE - poly-A
- insertion된 tumor-specefic LINE-1의 완전한 시퀀스와, hallmark도 같이 제시해서 TE mobilization 볼 수 있게.

# Introduction

- TE는 genomic architecture에 퍼져 있고, 영향도 줌
- ONT  long-read T2T 등으로 high-copy TE들이 refactoring 중
- 오래된 TE는 구분 가능한 diversity가 진화적으로 있어 구분 가능
	- ex) SVA-A의 경우 13.6M years old, SVA-F는 3.2M
- 하지만 최근의 TE insertion은 알 수 없음. 특히 Short-read로는

- 인간 염색체는 80-100개의 mobile LINE-1을 가지고, L1Hs(Human-specefic)로 표기
	- L1Hs는 6kbp 길이
	- 단백질로
		- *cis*, *trans* retrotranspose를 위한 *Alu*
		- composite SVA(SINE-VNTR-Alu, 2kbp) 를 encode
- Reference Genome에는 TE copy들이 포함되어 있지만, 대부분의 polymorphic Te는 non-reference에 포함됨
- **L1Hs-mediated germline insertional mutagenesis**는 질병 원인이면서 tumorgenesis에  관여

- TE regulation에는 다양한 host factor가 관여, 그 중 하나가 CpG methylation
	- 많은 CpG가 TE 안에 존재하고, CpG methylation이 young TE의 mobility를 억제
	- old TE는 histone marks 혹은 그 외 pathway로 억제됨

- L1Hs 5’ UTR 끝에 CpG island가 존재하고, 여기의 demethylation이 retrotransposition의 선행 조건(prerequisite)
	- 하지만 이 L1Hs internal CpG methylation profile은 모름
- SVA methylation과 transcriptional regulation간 관계는 잘 밝혀지지 않았음
	- SVA promotor structure가 모호함
	- CpG island가 VNTRs(internal variable number tandeom repeats) region에 있음
- 반대로 Young *Alu* sibfamily는 CpG Rich, Short-read로도 접근 가능
	- 다만 copy number가 높아서 분석이 힘듬
	- locus-specific genome wide bisulfide seq로 볼 수 있겠지만, 아직 thorughput과 resolution 문제가 있음
- 결국 Methylation landscape of young TEs는 어떻게, 왜 cancer와 관여하는지 불명함 → ONT로 볼 예정
## Design
- WGS, Short-read data에서 TE insertion 보는 툴은 있음
	- 보통 genomic coordinate + adj. seq relevant to retrotransposition
	- LINE-1 Pacbio는 있는데, general TE / ONT에서는 없음…
- TLDR
	- long read data에서 non-reference TE insertion annotation
	- can resolve entire TE insertion
		- 5’ inversion, TSDs(Target site duplication), 3’ Poly A tracts, transdutions…
			- 5’ inv : https://www.nature.com/articles/s41586-023-06046-z/figures/4
			- 
	- Read 수가 적어서(Long-read) computing time이 빠름

# Results
## Genome-wide Methylation Profiles of Young TE subfamilies in Human Tissues
- ONT PromethION 으로 5 human sample, 15x depth
	- Hippocampus, Liver, Heart(Germ layers) from Individual without post-mortem pathology(CTRL-5413)
	- paired tumor/non-tumor(T/NT) liver tussue from hepoatocellulr carcinoma(HCC33)
- *en masse* TE subfamily 분석 중 tumor-specefic L1Hs demethylation in patient sample(HCC33)
	- 다른 young TE나 Genome에 비해 더 많이 demethylation
	- CTRL과 비교하면 Hippocampus < Heart < Liver로 demethylation
	- older LINE-01 subfamily에서 더 나타나며, younger *Alu*에 비교해서도 methlyation이 떨어짐
- 반대로 SVA는 크게 차이가 나지 않음
- Immobile human endogenous retrivirus K(HERV-K)의 Long terminal repeat(LTR5_Hs) flanking region에서도 methylation 상대적으로 떨어짐(Ctr, HCC 모두)
- Genome-wide methylation은 CTRL normal liver와 비교해 HCC non-tumor liver에서 약간 떨어짐
- 기존에 못 보던 full-length Composite methylation profile에서는 L1Hs 5’ UTR CpG island가 모든 sample에서 나옴
	- =flanking SINE-R이나 Alu-like보다 young SVA CpG-rich VNTR이 더 methylation

- Fig 1
	- A : Vis
	- B : CpG methylation in HCC33(demethylated) <> Non-tumor()
		- WG : whole genome
		- L1Hs : >5.9kbp
		- Alu : >280bp
		- SVA : 1kbp
		- HERV-K flanking long terminal repeats(LTR5_Hs) > 900bp
	- C : CpG methylation in CTRL, diff TEs
	- D : Met profile for ref L1Hs intronic to TTC28
		- most-upper : purple circle is L1Hs loc at TTC28
		- Upper : Refernce space → CpG space, blue is utr5 red is body of L1Hs
		- low : mCpG methylation fraction
	- E

- Fig 2
	- 각 Hcc33(tumor/nontumor liver), CTRL5413(hippocam,heart,liver)의 L1Hs, AluYa5, SVA_F domain의 methylation 
	- Consensus가 가장 위체, 그 아래 black bar가 CpG position, Orange bar가 CpG island
- 
## Locus-, Element- and Hyplotype-Specific Young Reference TE Methylation States
- 대부분 TE는 구조적으로 methylated.
- 그런데 normal tissue의 methylation pattern을 관측함
	- 예로, Intronic TTC28에 있는 L1Hs은 cancer에서 mobile하다고 알려져 있고, CTR liver에서 hypomethylated되어 있었음
	- Chr 13에서 5’이 잘린 L1Hs는 neurodevelopment 과정에서 somatic retotranspostion을 일으킨다고 알려져 있는데 이 또한 CTRL에서 demethylated
	- ZNF683 antisense intron에 존재하는 L1Hs는 CTRL 심장 조직에서 demethlated, 알려진대로 5’UTR-promoted alternative ZNF683 transcript를 transcription.
	- Chr1 L1Hs도 germline / cancer에서 mobile하다고 알려져 있고, 노화된 fibroblast에서 발현한다고 알려져 있음. 그리고 CTRL의 심장/간 조직에서 demethylated. 

- HCC33 tumor랑 normal liver간에서도 methylation trend가 보임
	- SVA methylation은 넓게 보면 HCC33에서 변화가 없어 보이지만 각각은 차이가 있긴 함(Intergenic SVA_F on chrX이 demethylated)
	- 보통 L1Hs subfamily는 tumor-sepecefic(HCC33)에서 demethylation되는 것으로 보이나 hypermethylated(PGAP1 intronic L1Hs)도 있음
- 4.7% L1Hs, 2.6% AluY, 1.8%$ SVA copy 가 hcc33에서 hypermethylated, 반대로 hypo도 1.9, 3.0, 4.1%
- 이렇게 element-specefic하지 않게는 전체적으로 demethylation @ cancer epigenome


- Fig3
	- A : Diffrerential metylation of ind.TEs in tumor<>nontumor liver
		- X: TE 중 10+ CpG, 20+ met calls. Tumor/Nontumor mCpG
		- Y: 해당 TE에서 spaning하는 flank의 T/NT mCpG
		- T/NT>4면 filled, 
		- Outlier는 red/orange(Z score >2, >1)
	- B
		- chr X 의 interenic SVA_F의 met.profile. 
		- reduced met rel/ non-tumor and hcc33
	- C
		- L1Hs insertion in intron, of PGAP1 gene and methylation profile about surroundings
		- demethylated proior regin will represent promotor
	- D
		- Intergenic L1Hs demethylation in HCC33.
	- 

- Mammalian TE는 parent의 methylation state에 영향을 받음
- 그래서 imprinted gene(gene이 온 parent에 따라 발현 변화하는 gene) 에서 hyplotype-specefic diff. methylated region 찾음(backend phased methylation profile)
	- ex) GNAS, PEG3
	- 그리고 18개 TE copy가 element, haplotype specefic methyaltion patttern 보임
	- chr 7의 L1Hs는 cancer/germline 모두 비슷한 expression을 보이지만
	- 반대로 L1PA2는 hyplotype-diff methylation과 copy number diff 보임
- = ONT로 hyplotype-specefic TE regulation도 봄

- Fig4
	- A : Haplotype specefic diff. in CTRL hippocampus, GNAS gene : Read-backed phasing
		- Diffrently Methylated Regions(DMRs) identified
		- 1 upper : location of identified DMR
		- 2 Rel. between genome space and CpG space
		- Fraction of methylated CpGs between haplotype
	- B
		- 
## Long-read Detection of Polymorphic and Somatic TE insertions with TLDR
- 기존에 short-read로 만든 TEBreak와 비슷한 sensitivity를 가지도록 TLDR을 만듦
- 그러기 위해 먼저 TE inerstion이 보고된 Known non-reference(KNR)의 High-confidence set을 사용
- 같은 sample을 45x illumina - TEBreak하고 TLDR과 비교해도 KNR insertion sensitivity 비슷함.
- 특히, non-reference insertion은 TLDR이 더 잘 찾을수 있었으며 insertion detection에 보통 한개 read만 필요했음(TEBreak은 5’-3’ junction coverage가 필요)
 -

- Fig 5 : vis. explanation

? : 5’ or 3’ transduction? Polymorphic (TE)
- TLDR은 TE insertion에 관련한 여러 sequence feature를 얻어냄
	- 2798개의 filter-passed known/unknown nonreference insertion을 CTRL-5413과 HCC33에서 찾음
	-  median(TSD)는 14bp, 기존 연구 결과와 유사
	- 저 중 1073개가 intrargenic insertion
	- Alu는 UTR/Coding seq에 들어간 exonic event가 15번 있었고
	- L1Hs는 그 fraction이 가장 적음
	- consensus length는 consistent w/ family consensus seq, 더 긴 경우는 tail이 길어진 경우
		- 왜냐면 TLDR은 insertion/transduction 찾아냄(?)
		- L1Hs/SVA 계열에서 온 걸로 추정된 5, 3’ transduction은 31-2000bp
		- L1Hs 와 SVA는 5’, 3’ transduction이 동반된 경우가 있었고 5’ transduction이 상대적으로 드뭄
		- 이걸 PCR/capillary seq로 확인했음
	- 결국 23.6%의 L1Hs은 5’ inverted, 16.8%는 full-length, 
		- 기존 연구에 비해 TLDR이 spanning read를 필요로 한다는 조건에 의해 더 확실(반대로 under-ascertainments) 할 것으로 보임
	- SVA에서의 Internal polymorphism(ex)VNTR)이나 hexamer length 변화도 볼 수 있었음
		- 145개의 polymorphic SVA VNTR expansion
		- 크기는 37-4000bp, 
		- 그리고 3개의 polymorphic AluY insertion withIN VNTR도 찾음
- = TLDR은 LINE-1 mediated retrotransposition도 잘 찾고 non-=reference TE insertion도 전체적으로 *in toto*로 찾음

- Fig 6
	- A : TE family들의 composition.
	- B : TE 평균 TSD 길이
	- C-E : L1Hs, Alu, SVA의 size distribution

- PCR로 확인된 tumor-specefic L1Hs insertion(HCC33 short read에서 찾음)도 TLDR이 찾았음
- 그 외에도 TSD같은 insertion feature나, EFHD1 내의 internal inversion breakpoint도 봤음
- 추가적인 Tumor-specefic TE Insertion은 못 찾음
## Genome-wide Resolution of Non-reference TE Methylation
- 각 reference TE를 위해 TLDR은 non-reference TE insertion에 대해 element-specefic methylation profile을 만들 수 있음
	- ex) Non-reference L1.2(ORF1)는 LINE-1 mobility나 pathogenesis에 연관 있다고 알려지는데 15% lesss methylated in CTRL liver and heart than hippocampus
	- Non-reference L1Hs copy들은 cultulred cell에서 더 efficient하다고 하고 대부분의 mobile L1Hs는 reference에 없음

- Fig7
	- A. Example met. profile for non-ref L1Hs insertion at upstream of GFOD1
		- Liver sample의 특정 구간은 low-confidience(<20 met call with 30 cpg window)
	- B : Fraction of met.CpGs in CTRL as in 1C
	- C : ref-tE met Hcc33
	- 

- 일반적으로 노화되면서 TE의 somatic methylation이 떨어지지만, 그 균일성과 초기의 methalytion으로부터의 시간과 같은 부분은 모호
- TLDR로 CTRL-5413과 HCC33 양쪽에서 ref에 비해 덜 methylation된 non-reference TE insertion을 찾음
- 분석한 TE 중에서는 L1Hs가 ref와 non-ref TE의 평균 methylation 차이에서 가장 크고 확실한 변화
- non-reference variant가 더 최근의 TE라면, germline에서 온 이후 천천히 methylation 증가 후 감소하는 것으로 추정


- 
# Discussion
- Long-read ONT가 somatic/polymorphic한 TE insertion을 더 reliable하게 찾음
	- end-to-end resolution으로 볼 수 있는 insertion
	- PCR amplification artifact도 없다!
- TLDR로 retrotransposition의 hallmark feature도 찾음.(long transductions, internal rearrangements)
	- = TLDR이 somatic TE insertion을 잘 찾을것이고, methylation inference도 가능

- TLDR은 any non-ref TE insertion을 위해 적어도 하나의 complete span이 필요.
	- = insertion 전체 seq를 얻음
		- = TE로 인한 structual variation(seq polymorphism, SVA VNTR espansions, non-allelic homologous recombition, L1Hs 5’ inversion, transductions…) 확ㅇ닌 가능
		- 이런 insertion-spanning requirement는 잠재적으로 더 짧은 데이터셋에서 sensitivity를 떨어트릴 수 있음
		- 특히 Long-read는 error rate가 높기에, TSD, 3’ PolyA tracts, insertion feature들은 여러 회수의 sequencing으로 error correction.
	- 그리고 빠름

- long read nanopore는 cancer TE methylation을 보기 좋음
- 기존에 오래된 TE에 대해 NGS한 것과 비슷하게, young Alu, SVA의 demethylation은 HCC liver tumor보다 적거나 같음.
- 반대로 L1Hs는 dynamic하고 genome-wide change 보임
- 여러 TE methylation disjoint between surrounding을 봄
	- differential met. discordant @ HCC33 tumor은 6.6%의 L1Hs copy
- 각 케이스에 대해서도 봤고, 메틸화가 감소하고 있지만 TE met가 증가하는 경우를 HCC33에서 봄
	- = 기존의 확률적인 tumor-기반 demethylation과는 다르게 일부 TE만 더 쉽게 잃고/얻는 것 같음(특히 L1Hs)
- = somatic TE insertion in cancer는 L1Hs가 많았고, Alu와 SVA가 적음

- ONT로 met과 함께, 기존에 보기 힘든 SVA VNTR같은 것도 봄
- SVA insertion과 structual variant들은 genetic disease의 근원
- 그리고 SVA로 인한 strong VNTR methylation이 퍼지면 주변 gene에도 영향
	- 여기서는 SVA VNTR + L1Hs 5’ UTR의 경우에서 위치에 따른 met 변화가 큼
	- 이렇게 최근에 삽입된 TE의 CpG island methylation으로 인한 “Sloping shore"”(경사 해안?)은 TE termini에서의 met. assay misleading도 가능
		- Sloping shore : SB system으로 봐서, 2-3kb 이내 CpG island간 영향 주고받음
	- locus-specefic bisulfide seq는 정확히 terminal TE methylation을 볼수 있겠지만 ONT가 더 빠르고 많이 봄
- 게다가 Longread로 haplotype/genotype도 볼 수 있음.(allele-specefic TE methylation)




# Methods

## Sequencing data generation
- Phenol-chloroform DNA exrtaction

- Protinease, RNase A

- ONT PromethION, Guppy, Minimap, 
- met call with nanopolish


- short read
	- Haplotyping, NovaSeq 6000, BWA-MEM, dupcall Picard tools, Variant call with GATK haplotype caller, SnpSift
	- Backend-pahsing with whatshap
## Reference insertions
- Ref te location : RepeatMasker, UCSC genome browser, 
	- SVA are broken into multiple adjacants, merge em(+1000bp)_
	- Line1 if 5900bp+
	- Alu if 280+bp
- Gu
## Finding non-reference TE insertions from short reads

## Finding non-reference TE insertions from long reads
[TLDR](https://github.com/adamewing/TLDR) for analyze non-reference TE content from long reads

- This study focused on ONT reads, but TLDR can use any **Accurately-aligned** reads that **enough to span TE insertions**
	- approx 30x long-read genomes < 1h w/ 32 core
- Operates in two phase : *Clustering* and *Insertion resolution*
- REquired inputs : *Aligned, Indexed BAM*, *Indexed reference genome fasta*, *set of reference TE in fasta*(Human reference is included.)
- Recommended options : `-e ref.fa -n nonref.bed.gz --color-consensus -c chroms.txt -o all_extend_extend consensus 20000 -detail_out` ???
	- chorms.txt to limit insertion cals only in canonical chromosomes

- *Clustering phase*
	- seeded by identify insertions(like long-indels) embedded in long reads : Insertion이 있는 구역 = Seed
		- embedded insertion bound : 200 - 10000bp
	- Seed insertion 끝에 breakpoint가 있으면 추가적으로 junction 주변의 ambiguity(200bp) 허용
	- Read가 길기 때문에, 하나의 read가 multiple cluster 형성 가능
	- 특히, 긴 read는 chromosome 길이까지 읽기 때문에 one-chromosome 단위로 계산 가능
	- Short read mapping도 같이 봐서 정보량 늘릴 수 있으
- *Processing phase*
	- Cluster 단위로 병렬화. 
	- Flank size(500bp)에 따라 seed site 양 끝을 trimming하기 시작
	- 각 클러스터마다 reference TE에 align하기 시작(exonrate w/ affine:local)
	- 최대 3개의 non-overlapping alignment가 80% 이상의 TE를 커버하면 internal arrangement(3-5 반전)를 고려해 OK
	- alignment들은 group으로 할당, non-overlapping sets of alignment(read coverage가 안 겹치는 거라고 보기보단 겹치는 위치의 alignment 방향이 같은 group이라고 생각)를 가지는, best align ref TE 기반의 group.
		- 여기의 best align ref TE가 identity 결정
	- 각 cluster $C$에 대해, reference TE에 mapping이 되어야만 함. 이런 클러스터들은 $C_{useable}$로 칭함
	- 이후 minimum read count(3 total, 1 fully contain insertion) 조건을 넘어야 함
		- Supporting reads는 TE에 align된 후, Z score를 계산해 $|Z|>2$만 유지
	- 혹은 Reference TE에 50% 이하만 assign되면 clujster는 reject
- *Consensus generation*
	- 각 Cluster $C_{useable}$에 대해 consensus sequence를 MAFFT(MSA)로 만듦
	- 각 MSA column에 대해 gap이 있으면 다른 2개의 base로 메꿔지면 그렇게 함
	- 이후
		- 각 consensus seq가 만들어지면, 다시 minimap2와 pileup을 해서 계산. 각 pileup column에 대해 consensus seq base가 3개 이상의 non-consensus base보다 적게(50%) voting되면, 4변환
		- 각 pileup depth가 cluster에서 supporting하는 read의 10%보다 작으면, 바뀌지 않음
- TE insertion breakpoint identification($b_1$, $b_2$, $b_1 \le b_2$)
	- [[Terms#GMM]] (Gaussian Mixture Models)의 1 or 2 means로 fitting
		- TSD의 존재 여부에 따라, insertion breakpoint가 1개 혹은 2개
	- 이후 AIC(Akaike information criterion)으로 best-fit model 결정
	- 이를 통해 insertion region $b_1-F$와 $b_2+F$를 estimate
	- 위치(breakpoint)와 base refine을 위해 consensus가 minimap2로 insertion location align되고,
	- 이 alignment들은 @de(gap-compressed per-base divergence)가 0.12 이하일때 사용됨
	- 이후 inserted/soft clipped/matched base들의 profile을 합쳐 reference base의 coverage mask를 만ㄷ름
	- maksed consensus에 대해 longest inserted segment 먼저 reference TE에 exonerate로 map. 
		- subfamily designation은 여기부터 시작
	- 만약 1> unmapped segment가 있으면, 합치고 ref에 insert된 걸로 취급(preseuming and intervene)
	- 이렇게 refin된 brakpoint는 initial TSD 결정과 TSD extending에 영향
- Detailed output을 원하면(특히 methylation status of non-ref TE insertion을 보기 위해) 



# Toolkit
https://github.com/adamewing/TLDR < Long read based / Mapping insertion 기반
https://github.com/akiomiyao/tif < Short read based / TSD(Traget site duplication) 기반
## Params
`-b` : bam files
`-e` : Reference elements, formatted like `>Superfamily:Subfamily`

## Outputs

- Consensus
	- Uppercase : ref-genome
	- Lowercase : insertion
	- TSD : Red
	- TE : Blue
	- Otherinsertion(Transduction) : Yello
