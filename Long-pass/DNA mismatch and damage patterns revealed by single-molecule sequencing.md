https://www.nature.com/articles/s41586-024-07532-8

! Mutation Burden : non-genetic mutation per megabase 느낌으로 받으면 됨.
# Abstract
- 현재 DNA sequencing으로는 single-strand에서 일어나는 damage/mismatch 기반 mutation 탐지가 힘듦
- Develop `HiDEF-seq`
	- to see single-molecule fieldity, either both strand
	- Cytosine deamination
- 다양한 sample에서 테스트, single-strand mismatch와 damaging signature, 그리고 그것들이 double-strand mutation signature과 대응됨을 봄
	- mismatch repair랑 pol proofread 모두 못하는 tumor는 더 큰 ss-mismatch
- Single-strand damage signature : APOBEC3A
# Main
- mosaic mutation은 항상 일어나고 있음 ㅇㅇ (당연히 다세포 생물이니까…)
	- 그리고 dsDNA에서의 한/두쪽의 mismatch나 damage로부터 시작됨.
	- 이런 변이가 repair 전에 replication되면, permanant dsDNA mosaic mutation.
	- 기존에는 dsDNA mutation을 보지만, ssDNA상에서의 event를 보진 못함.

- 앞에서 말했듯 dsDNA mosaic = result from ssDNA mismatch, damage, repair, replication.
- 그리고 dsDNA의 signature는 ssDNA event를 반영하지 않음.
- 결국 모든 mutational process를 이해하려면 ssDNA event도 봐야 함.

# HiDEF-seq
- 이런 dsDNA mosaic mutation을 보려면 $10^{-9}$급 에러를 찾아야 함.
- 그리고 ssDNA에서의 mismatch, damage도 이보다 비슷하거나 더 큰 fieldty가 필요함
- HiDEF-seq는 아래 방법으로 fieldty를 늘림:
	- Strand sequencing pass 횟수를 늘림. 1.7kb strand가 32pass정도
		- 이건 다른 single-molecule seq보다 더 높은 pass, high qulaity
	- Eliminating in-vitro artifacs durning prep
		- ssDNA nick ligation + Nanoseq A-tailing or w/o A-tailing for post-mortem samples
	- Computational pipeline, avoid analytic artifacts
- sequencing은 PacBio sinagle-molecule long-read.

- Fig1
	- a
		- Nick ligation - Fragmentization -  A-tailing -  hairpin ligation and sequencing
		- = Can verify sequencing error, dsDNA mutataion & ssDNA mutation
	- b
		- Pacbio 기존 HiFi보다 제시한 HiDEF의 depth가 더 깊음
	- C
		- 나이별 sperm sample에서 보이는 dsDNA mutation
		- HiDEF vs NanoSeq
	- d
		- Human tissue에서의 HiDEF, dsDNA mutation
	- e, f
		- Nanoseq vs HiDEF, dsDNA and ssDNA mutation calling
	- g
		- Nanoseq vs Hidef, ssDNA mutation by call type

- Sperm은 가장 낮은 dsDNA mutation, 여기서 mosaic을 HiDEF로 찾음
- 그리고 기존 de-novo mutation/NanoSeq profiling과 합치
- 나이에 따라 증가하는 mutation signature 보여줌

- +20 pass에서 +5로 완화해 계산하니 dsDNA mutation 일치.
	- = PacBio의 high fidelity
	- > $1/3\times10^{13}$ bp error @ 5 depth
		- 계산 방법은 Method…
- dsDNA mutation analysis에서는
	- depth 5가 충분히 괜찮고, QC해도 많이 안 버려져서 사용
	- Mosaic mutation 보기 위해서도 더 많은 dna 필요
	- HiDEF-seq은 인간 DNA의 40%를 커버하는 제한효소 사용
		- > mosaic 보기 충분한
		- 다만 긴 read를 쓰면 pass가 감소
- ssDNA mutation analysis에서는 더 작은 read + 높은 Pass threshold로 계산

- ssDNA calling의 mismatch는 실제 base가 다른 것 뿐만 아니라 damaged base의 mispair를 포함할 수 있음 (polymerase에서 변화)
	- 오히려 damage까지 포함하니까 좋을지도
- ssDNA mutation은 dsDNA처럼 다른 strand로 correction 불가, 정확한 call을 알 수 없음
- 그래서 필터를 조정해서 stable하게 > Methods / Ext.fig 3
	- param : minimum passes per strand  + consensus sequence accuracy threshold
- HiDEF-seq / Nanoseq 비교 
	- dsDNA burden / pattern 유사
	- ssDNA는 HiDEF이 18배 적고, C>T만 봐도 4배 적음. Nanoseq이 말한대로 오류가 많음
- = HiDEF-seq achieves high fildity for SBSs

# Cancer predisposition syndromes
- 이전에는 ssDNA mismatch를 sequencing 못함
- calling 정확성 보기 위해서 inherited cancer predisposition syndrom을 가진 환자 혈액 샘플 프로파일링
	- 8명의 환자, 17개의 sample. fibroblast lymphoblastoid lines
	- including defects in NER, NMR, proofread, BER
	- → confirming HiDEF-seq ssDNA fildity

- control sample들에 비해 ssDNA call이 더 많이 보임.
- 2.6fold increase in _POLE_ polymerase proofreading-assoicated polyposis syndrome(PPAP)
	- G>T, G>A, A>C ssDNA calls
- 1.6fold increase in congenital mismatch repair deficiency syndrome(CMMRD)
	- small alteration on ssDNA calls comp.to PPAP
- big Purine ssDNA calls elevated(G/A>others)

- Fig 2
	- a
		- ssDNA calls on cancer predisposition samples
			- Burdens are corrected > Method’
				- corrected for trinucleotide context oppertinuties?
				- detection sensitivity
	- b
		- ssDNA calls(Pyrimidine/Purine) plotted
			- also corrected for trinucleotide context oppertunities
	- c, d
		- Spectra of ssDNA calles, and POLE PPAP sample for representation
	- e f
		- signature comparison between ssDNA and dsDNA.
		- projected

- ssDNA mismatch pattern spectra.(96) in POLE PPAP
	- shows pattern of AGA>ATA and AAA>ACA peaks and some small peaks
	- dsDNA 패턴과 매우 유사.
	- = ssDNA mismatch(polymerase epsilon nucleotide misincorporation)이 dsDNA mutation으로 이어진다고 생각됨
		- de novo SBSE + SBSF sample dsDNA 패턴과도 유사.
	- SBS10ss로 명명, 기존 SBS10c COSMIC과도 유사
		- 나머지 시그니처는 ssDNA cytosine demaination 시그니처인 SBS30ss\*로
- CRMMD는 ssDNA call이 적어서 못함

- PPAP의 가장 큰 피크(AGA>ATA - TCT>TAT), (AAA>ACA - TTT>TGT)에서 complement간 call의 assymetry가 있음
	- = C:dT >> G:dA 
	- 이런 asymmetry는 기존 yeast/human tumor 연구에서도 보고됨
		- polymerase epsilon exonuclease domain에서 leading-lagging strand synthesis 과정에서 template/complement에서 어느 mutation context가 더 replicated되는지 간접적으로 확인
		- 다만 이런 데이터는 replication timing 데이터에 의존해, 완벽히 replication measure할 수 없음. 
		- = ssDNA mismatch call을 우리는 직접 볼수 있고, 비대칭성에 직접/관적
	- 기존 in-vitro polymerase gap-filling assay와도 같은 결과
		- 하지만 위 assay 방법은 full context의 DNA replication을 보지 않음
	- 우리도 위의 replication timing analysis를 POLE PPAP sample에 해봤고 
		- AGA>ATA dsDMA mutation, AGA>ATA ssDNA mismatch on leading direction 확인
- = 우리 결과는 direct, in-vivo, ssDNA mismatch를 봄

# Hypermutating tumors
- ssDNA mismatch(durning replication) 과 mismatch repair 과정의 상호 작용을 보기 위해서 hypermutating brain tumor with CMMRD
- 3번 샘플(이 가장 높은 ssDNA C>T, SBS30ss\* = cytosine demaination dmg signature) 사용. 아마 C>T가 ex-vivo에서 온 것으로 추정하기(suppl table 2 3)
- 추가로 두 샘플은 biallelic germline PSM2 mutation & somatic POLE domain mutation
	- mismatch repair나 pol.proofread mutation 중 하나만 있던 sample과ㅡ 비교해서 distinct pattern이 있었음
	- 이 dsDNA mutation spectra가 보고되어 있는 mismatch repair와 pol.proofreading mutation 모두를 가진 tumor/cell line과 유사
	- 하나는 Medulloblastoma(p.S459F) / 다른건 Glioblastoma(p.E277G)
- 대부분의 dsDMA mutation은 COSMIC SBS14와 유사
- ssDNA call도 dsDNA와 유사했지만, C>T call은 SBS30ss

- Fig 3

- 위 샘플의 ssDNA signature는 polymerase proofread만 mutation이 있던 sample과 차이가 보임 (proofread OK? No? sample)
	- ssDNA AG>AT call 증가, G>A, A>G, T>C 증가
	- = 기존 연구에서는 pol.proofread deficiency로 인한 특정 Mismatch에 대한  Repair가 더 효과적으로 된다=고 함
		- 이 샘플도 proofread만 deficiency
		- 그럼 proofread를 주로 안하는 mismatch에 대해 MMR이, 더 잘되는 건가
- 특히 ssDNA C>T(from cytosine demaination)이 크게 증가.
	- 이건 proofread-deficient pol epsilon이 잘못 중합해서 만들어지는 C>T/G>A dsDNA mutation, 그리고 이건 G:dT보다 C:dA로 발생
- 이 ssDNA signature @ tumor sample 을 SBS14ss로 명명, 
	- 이 중 가운데가 pyrimidine인 context에서는 COSMIC dsDNA SBS14와 유사
	- 이 SBS14ss는 위의 두 tumor sample에서 모두 관찰됨
- Post-nortem brain / spinal cord sample도 봄. 환자는 MSH2, MSH6 CMMRD였고, 뇌에 POLE mutation으로 인해 사망
	- 여기서는 기존 연구와 같이 SBS1 dsDNA mutation뿐만 아니라, ssDNA C>T @ CG dinucleotide도 많이 보임
	- == HiDEF-seq이 SBS1의 전구체(ssDNA C>T)도 확인

- Fig 4 : ssDNA damage signatures of sperm and heat-treated DNA
	- a
		- non-cancer blood sample의 ssDNA call
		- 이후에 SBS30과 유사성 비교함 (Central pyrimidine context)
	- b
		- heat-treated DNA의 dsDNA mutation / ssDNA call 
	- c
		- sperm / heat-treated blood의 ssDNA call
	- d

# Patterns of cytosine deamination damage
-  Cytosine deamination → Uracil, Uracil glycol, 5-hydrouracil, 7-hydroxyhydantoin 
- 발생하면 dsDNA c>T가 보일거고, HiDEF-seq가 C→G event를 볼 수 있을듯
- Damaged Cytosine은 Thymine으로 읽힘(by seq.pol)

- 그래서 ssDNA C>T call을 그냥 blood DNA(w/o cancer predisposition)
	- blood는 potential post-morten damage 적게 빠른 처리가 가능
	- RT에서 DNA 추출(heat X), $2\times10^{-8}$ ssDNA C>T per base
	- 그리고 이 C>T가 전체 call중 71%
	- = 분명 in-vivo + ex-vivo process로 발생했을 텐데, 백혈구에서 세포당 <250 C>U가 존재할 것으로 추정
- 그리고 우리 HiDEF-seq의 1/50M base sensitivity는 mass spectrography(다만 서열 맥락 볼 수 없음)급이고, 낮은 background를 줌
- 특히 위의 blood ssDNA call을 central pyrimidine context로 보면 COSMIC SBS30과 유사
	- SBS30은 cytosine oxidative demaination damage repair by DNA glycosylase와 연결
- G>T ssDNA call은 8-oxoguanine으로 많을 것으로 예상했지만 blood sample에서 매우 적었음.
	- 아마 sequence polymerase가 잘 dC를 합친 듯

- 높은 sensitivity 기반으로 heat의 효과를 보려고 함.
- Heat는 Cytosine demaination artifact source로 알려짐
- 그래서 purified blood DNA를 heat incubation(56deg, 72deg, 3h) 하고 비교
	- dsDNA mutation에는 영향을 주지 않앙ㅆ지만
	- HiDEF-seq로 ssDNA call 증가를 확인 C>T
	- 그리고 heat temp와 time에 비례
- = HiDEF-seq 시에는 37도 안넘게 주의

- healthy tissue와 cell line에서는 보이지 않은 C>T ssDNA call이 sperm에서 보임
	- 아마 cytosine deamination이 in-vivo에서 더 일어나거나, freezing, purification, extraction 등의 exvivo 문제일 거임
	- 그래서 non-sperm sample로 비교
		- 전자 : non sperm sample을 sperm과 같은 방식으로 처리
			- C>T increase 없음
		- 후자 : 다른 sperm sample을 filter chip으로 정자 운동으로 인한 분리를 모방
			- simillar C>T burden.
			- 기존 sample은 density gradient c.fuge로 분리
	- = sperm이 liquefing/purification 전에 demaniation stress를 받음
		- 그러면 fertilizing에서 문제가 될 것이고, egg의 DNA repair을 받을 것
		- 추가로 sperm의 ssDNA C>T는 transcription level/strand bias 없었음

- 모든 sperm sample + heat-treated DNA sample이 비슷한 ssDNA C>T 보여줌
	- 그리고 이는 dsDNA COSMIC SBS30과 유사
	- ssDNA signature은 SBS30ss\*로 명명
	- SBS30은 NTHL1과 UNG loss-of-function mutation, formalin fixation과 연관
		- NTHL, UNG는 DNA glycosylase로 base excision repair, oxidized pyrimidine
			- 그리고 Uracil-species repair도 포함됨어 있음
- 결국 Heat-treating은 ssDNA damage 유발하고, 이건 in-vivo SBS30과 유사.
- 그러니 nucleotide-context bias가 cytosine deamination에 있으며 oxidized intermediate가 영향을 줬을 것이다.

- ssDNA C>T 결과를 보면서 single-molecule sequencer의 kinetic data도 봄. = dureation of nucleotide incorporation(Pulse width PD) + time between nucleotide incorporation(Interpulse duration IPD)
- PD와 IPD로 damaged base<>canonical base의 차이나는 kinetic signature를 볼 수 있음
- heat-terated DNA와 sperm은 dsDNA C>T와는 다른 kinetic signature를 보임
	- 4f, method
- = ssDNA c>T가 deamination으로 나온 uracil이고, c>T를 유발한다고 볼 수 있음
- 나중에 DNA glycosylase랑 endonuclease VIII로 검증
	- SBS30ss 패턴과 C>T call 없어짐

- 다른 buffer에서 heating도 확인해 봄
- Water/Tris buffer w/o salt에서는 high-salt buffer에 비해 66fold cytosin damage 더 있음
- low salt는 DNA duplex stability가 고온에서 낮아짐.
- 그러니 아마 SBS30ss의 cytosine deamination은 single-strand transient state에서 발생했을 것

# Patterns of APOBEC3A-induced damage
- HiDEF-seq의 cytosine deamination detection w/ single-molecule fileity로 APOBEC3A의 damage signature 보기.
	- APOBEC3A는 최ㅏ근 알려진 cytosine deamination contributer.
- APOBEC3A를 primary human fibroblast에서 발현시키고 ssDNA signature 찍음
	- called SBS2ss
	- 그리고 이건 APOBEC3A associated COSMIC dsDNA signature인 SBS2와 유사
	- 신기하게, SBS2ss는 TCN context가 아닌 ssDNA c>T call의 낮은 peak가 추가적으로 나타남.
	- 그리고 ssDNA C>A C>Gcall이 없음
- = COSMIC SBS13 signature w/ APOBEC3A가 base excision 후의 abasic site의 error-prone translesion synthesis로 인한 것임을 시사

# Profiling the mitochondrial genome
- 기존 연구는 노화에서 20-40fold 정도 높은 dsDNA mutatiion이 mt-genome에 nuclear보다 더 보인다고 함
- 그런데 왜 mt-gene이 더 변이하는지는 모름
	- 아마 oxidative damage로 추정
	- 최근에는 replication으로도 추정
	- 특히, A>G, C>T dsDNA mutation이 mt-heavy(G+T rich) strand에서 보이며 mt-heavy region에서 멀어질수록 감소
- 이런 발견으로 관련한 hyothesis
	- 1) strand-displacement replication이 heavy strand를 더 오래 ssDNA로 노출시키고, deamination에 취약해짐
	- 2) polymerase incoporation of canonical nt에서의 assymmetries
	- 3) DNA repair의 asymmetries
	- 특히, DNA repair이 nuclei에 비해 mt에서 덜 효율적이라고 가정하고,
	- HiDEF-seq가 mt의 ssDNA mismatch를 감지할 수 있다는 가정 하에,
	- 2)3)은 mt에서 ssDNA burden이 높아야 함.
		- 왜냐면 HiDEF-seq는 CMMRD,POLE PPAP sample에서도 더 확률이 낮은 mutation을 발견함
	- 반대로 1)은 mt와 nuclear간 ssDNA burden 차이가 없을 것으로 예상
		- 왜냐면 복제 과정 중이기 때문에, ssDNA mutation→비로 복제로 dsDNA로 변형됨
	- 이렇게 가정하고 실험

-Fig 5

- liver/kidney sample, 왜냐ㅑ면 더 mtDNA가 많음. 5개 sample에서 mt DNA yield
- HiDEF-seq,  liver/kidney에서 mt-dsDNA mutation rate는 60fold 더 nuclear genome에 비해 보임
	- Liver-Kidney 합해도 45fold diff
- 그리고 assymetric pattern도 찾음
	- A>G, C>T dsDNA mutation on Heavy strand
	- A>G는 SBS30ss와 유사

- dsDNA muatation은 차이가 큰 데 비해, ssDNA는 1.5 fold만 차이(MT-nuclear)
	- ssDNA MT call이 적긴 했지만, dsDNA spectrum과 유사하게 보였음
- = mt gene mutates primary replication, 아마 DNA damage on heavy strand @ single-stranded state or cytosine deamination on light strand


# Discussion
- dsDNA mutation = past mutation
- ssDNA mismatch/damage = DNA damage and repair status
	- if ssDNA transfered into dsDNA, it losts original data
- =HiDEF-seq is good to see the DNA damaging process

- profiled SBS10,14,30,2ss
- SBS10ss / SBS14ss : errornous replication

- quite expensive (4.6x)

- CMMRD/PPAP mutation : high reate of ssDNA call
	- 하지만 calling이 바뀌는 형태는 못 봄.(@ BER, ER 문제 환자)
- 아마 있지 않을까?

- Diverse methods damages DNA, 하지만 low-resolution pattern
- HiDEF-seq은 Cytosine deamination signature SBS30ss를 봄 (even if 건강한 조직)
- <>SBS1?

# Methods

## WET
### Sample sources

### Sperm purification

### Cerebral cortex neuronal nuclei purification

### Extraction and isolation of mitochondria

### Cell culture for direct profiling

### Lentivirus experiments

#### Lentivirus plasmid design and synthesis

#### Lentivirus packaging

#### Lentivirus transduction

### DNA extraction

..
### Illumina germline and tumour library preparation and sequencing

### Pacific Biosciences germline library preparation and sequencing

### Heat damage of DNA

### NanoSeq library preparation and sequencing

### HiDEF-seq library preparation and sequencing

#### Choice of restriction enzymes for DNA fragmentation

#### HiDEF-seq library preparation

#### Sequences and preparation of blunt adapters used for HiDEF-seq without A-tailing

#### Modified HiDEF-seq library preparation trials to remove ssDNA T>A artifacts

#### HiDEF-seq library preparation with uracil DNA glycosylase/endonuclease VIII treatment

#### HiDEF-seq library preparation with multi-glycosylase/endonuclease treatment

#### HiDEF-seq large fragment library preparation

#### HiDEF-seq library preparation with random fragmentation

#### HiDEF-seq library sequencing

## DRY
### Gernline and turmor sequencing data processing

#### Illumina germline and turmour sequencing data processing
- CHM13 reference, BWA-MEM, markduplicate(picard)
- Caller
	- GATK HaplotypeCaller w/ parameters
	- Deepvariant w/ parameters
#### Pacific Biosciences germline sequencing data processing
- retreve consensus sequence by pbcss
- filter HiFi reads
	- ‘rq’ ≥ 0.99(consensus accuracy)
- Align on CHM13 w/ pbmm2
- Call w/ DeepVariant, model type pacbio
### HiDEF-seq primary data processing
- bash + awk : process raw subreads
	- drop cx tag != 3. (adaptor missing)
	- generate consensus by pbccs
	- demultiplex with lima
	- drop unpaired reads(one fwd, one rev consensus ZMW)
	- align fwd / rev to CHM13, w/ pbmm2
		- CHM13 only contains 1-22 and X, MT. not Y
	- reciprocal overlap 90%, non-supplementary alignment
	- sort
	- add tags
- R
	- load
	- Call annotation which ref is ‘N’
	- Annotate indel positions
	- Annotate call if present in VCF files(corresponding germline annodataion)
	- save indel positions
	- transform dataset to ss/dsDNA calls
	- save
- 
### HiDEF-seq call filtering
- multiple filters..
	- optimized using low-mutation samples(infants, sperm samples) to remove False positives
		- frequent false-positives @  low-quality regions of genome, low-quality aligmnet/read

- optimized parameters(thersholds) using sperm samples, knowwn low mutation burden
- 3 key filters(ds / ss)
	- minimum predicted consensus accuracy : 0.99 / 0.999
	- minimum number of passes per strand : 5 to 20
	- minimum fraction of subreads detecting the mutation : 0.5 to 0.8
-  burden calculation →? ?

- additional filter w/ REAPR, 
	- 원래 genome assembly 상에서 error 를 찾는 용도지만, 여기선 illumina에서 variant call을 평가하는데 사용.
	- Illumina align to CHM13 w/ SMALT
	- sort / markduplicate
	- PEAPR perfectfrombam, metric used.

- Mutation analysis filter:
#### Filters based on DNA molecule quality and alignment metrics
- Retain DNA molecules with this criteria
	- ccs consensus accuracy ≥ 0.99 in both(rq tag), ≥Q30(0.999 RQ) for ssDNA
	- minimum 5(ds), 20(ss) sequencing passes for each strands
	- both fwd,rev mapping quality ≥ 60
	- Max. difference ssDNA calls are 5. 
		- = remove low-quality molecules
	- average number of indels relative to human reference genome in fwd/rev ≤ 20
	- average number of soft-clipped bases ≤ 30
#### Filters based on germline seqsuencing variant calls
- Filter calls that also identified in any germline VCF w/ read depth ≥ 3, QUAL(allele qulaity) ≥ 3, genotype quality(GQ) ≥ 3, variant allele fraction ≥ 0.05
- Filter DNA molecules with >8 dsDNA after germlime filtering. 
#### Filter based on genomic regions
- Either illumina/pacbio
	- ANY Overlap with segmental duplication regions, from repeatmasker 
	- Except MT mutation.
- On Illumina
	- Over lap with telomere
	- region의 fwd/rev consensus alignment의 30% 이상이 mappability < 0.4 인 경우(Umap)
	- 전체 genome을 20bp bin으로 나눠, orphan read 비율이 전체 aligment 중 20% 이상이 orphande read 비율이 15%이상 인 지역
		- REAPR, orphan_cov, orphan_cov_r 사용
- removing portion of this region:
	- genome sequence is N
	- Either illumina/pacbio
		- satellite sequence
		- gnomAD, SNV with Pas flag and population allele frequency > 0.1%(lifted to hg38)
			- germline 변이나, mosaic event 제거
	- Illumina-only
		- 100-mer mappability : …
		- fraction of peared, REAPR
		- orpahn…
#### Base Quality Filter
- ds/ssDNA : <93 filtered

#### Filter based on location within the read
- ≤10 bases from end of alignment.
	- ssDNA call에서는 call을 가진 read 에 대해서
	- dsDNA call 에서는 모든 rwd/rev alignment에 대해.

#### Filter based on location near germline indels
- Indel 길이의 2배 혹은 최소 15bp 떨어진 영역의 call은 제거.(artifact)
- germline seq 데이터(GATK/DeepVariant) 혹은 pacbio (DeepVariant indel call) 에서
	- GATK indel은 read depth ≥ 5, QUAL > 10, Genotype quality ≥ 5, Variant Allele fraction ≥ 0.2
	- Deepvariant indel은 depth ≥ 3, QUAL ≥ 3, genotype quality ≥ , Variant allele raction ≥ 0.1 
	- 만 사용
#### Filter based on location near consensus sequence indels
- consensus sequence 에서 indel 인근 제거
	- 위와 같이 2배 / 15bp 인근
	- ss에서는 해당 방향 strand만, ds는 양쪽

#### Filters based on germline sequencing read depth and variant allele fraction
- Germline sequencing data의 coverage가 <15인 영역의 call 제거
- Variant allele fraction > .05, 혹은 read depth > 3인 germline sequencing data에서 발견된 call 제거. 아마 germline일 거라고 생각하고, 혹은 극초기 somatic mutation 일 것.

#### Filters based on fraction of subreads (passes) detecting the call and fraction of subreads overlapping the cell
- subread의 비율이 <50%(ds), <60%(ss)인 call 제
	- ds에서는 fwd/rev 따로
	- ss는 해당 방향 strand
- …

### HiDEF-seq calculation of call burdens
- 필터 후, 하나의 read에서 하나의 call이 나오도록
	- = ssDNA call per ssDNA strand / dsDNA call per dsDNA strand
	- 만 사용해서 burden calculation
- 
### Signature analys
 - sigfit v2.2
	 - 입력 1 : trinucleotide context raw mutation count
	 - 입력 2(opportunities) : 각 context에 대해, `전체 mutation 중 해당 context 비율 / 전체 reference genome 상에서 해당 context 비율`
		 - burden analysis(여기 아님) ref.genome은 CHM13 v1.0 사용했으나, 여긴(signature analysis) GRCh37 씀
		 - 그래야 COSMIC 등이랑 비교 가능
 - 이후 signature간 분리/추출 위해서 `plot_gof`를 쓰려고 했는데, COSMIC SBS1이 de-novo extraction 과정에서 분리가 안되서 아예 `fit_extract_signature`로 SBS1과 다른 de-novo signature 분리.
	 - 이렇게 얻은 de-novo signature는 COSMIC SBS v3.2 카탈로그랑 비교(cosine simillarity)
	 - 더 정확한 표현 위해서 피팅한 SBS signature랑 추출한 시그니쳐를 다시 mutation count로 다시 피팅함(?) `fit_signature`. 이 과정에서 context 확률은 유지
 - SBS5(ubiquitous clock-wise signature)는 SBS40, 혹은 SBS3과 유사함. 이와 같은 signature에 대해 reduce함.

- ssDNA 시그니쳐(192개)는 sigfit에서 192-context 분석이 가능하고, strand 단위 분석 가능
	- 여기서는 central-pyrimidine vs central-purine context 비교할 때 사용
		- sigfit의 `build_catalogues`를 써서 두 context 합치는걸 막음
	- 그걸로 transcribed / untranscribed(전사 여부 strand)에서 ssDNA call 볼 수 있ㅇ므
- 그 후 ssDNA 시그니쳐를 같은 방법을 ㅗ뽑음(trinucleoticde context oppertunity correcting)
	- 비교 시에는 ssDNA를 96-central pyrimidine context로 투영 후 dsDNA 시그니처와 비교(cos similiarity)
		- = 딱 reverse complement sequence로 합침

- cosine similarity로부터 significance 추정(quantification) 하기 위해서 시뮬레이션 함
	- = 96 random element / 192-random element vector(마치 임의의 랜덤 시그니쳐) 생성(n=10k) 
	- 이렇게 생성된 벡터들 간 cosine similiarity를 cutoff로 검증함(0.8-0.9 cutoff 에서 similiairty % 확인)
	- 그리고 similiarity 값에 따라서 명칭을 좀 달아줌. weak / moderate/ strong(0.8 0.85 0.9 기준) 96일때랑 192일때랑 다르게 씀;;; (192-context에선 moderate부터)

# Notes
- 두 샘플(CMMRD, POLE PPAP) 설명:
	- Cancer predisposition syndrome(특정 cancer가 유전적으로 발현)
		- 중에서
		- CMMRD(Consistutional msimatch repair deficiency) : repairing gene에 문제가 있음. 여러 종류(PMS2, MLH1…) 등
		- PPAP(Polymerase proofreading-assoicated polyposis) at POLE(DNA pol epsilon)
- Deamination repair(BER) 과정
	- 여기서는 C>(U)>T 과정으로 Pyrimidine인 C의 deamination to uarcil
	- 원인은 역시 다양(Ox stress, UV, readiation, Redox, sodium bisulfate : deaminase)
	- Repair mechanic
		- Uracil DNA N-glycosylase(UDG), 다양한 Family 존재하는데 인간/E.coli는 UDG I
			- 다만 5-fU, 5-hmU, 5-mU등에 작용 안하고, Uracil에만 매우 특이적
			- ssU, dsU 모두 절단
		- TDG(Human Tymine DNA Glycosylase)는 UDG II family로 dna U:G Mismatch에 작용
		- sMUG(Type 3 UDG) : U:G, U:A 쌍에서 작용 ~~smug~~
		- 위의 Glycosylase 작용으로 Abasic site(AP) 만들고, APE1로 cleavage
- Trinucleotide correction?

- A-tailing 여부(see add.fig 5)
	- HiDEF-seq에서 A-tailing w/ dATP, Klenow fragment 시 : Nick/Damaged base가 Pyrophosphorolysis 되면 빈 공간을 A로 채움. = ssDNA A mismatch artifact at nick
	- HiDEF-seq에서 w/o A-tailing, use ddBTP with Klenow fragment : nick에 ddBTP가 들어가고 Non-circulaized molecule은 이후 degradation됨


# Presentation
BGs

DNA mutation and mosaics accumulates. But current method only can see the dsDNA events

Target : ssDNA damage detection, single-molecule fildity. Then see the  ssDNA deamination damage signature

Current limit and why?

Short-read sequencing tech or Long-read sequencing (Illumina and Pacbio/Oxford)

PCR amplification & current SNP calling,

Short read에서는 구분 불가(polymerase proofread) <> long-read에서는 errornous

ssDNA damages 특징 : temporary, and in-vivo fixing

HiDEF- seq overview (Hairpin Duplex Enhanced Fieldity Sequencing), based on HiFi-seq(High-filedity long-read sequencing, Pacbio)

  Hifi-seq makes high-accuracy consensus from subreads

  Hidef-seq : more sequencing pass / Eliminate in-vivo artifacs + comp.pipeline

Pipeline

  Exp

  Hpy166II restriction.enzyme (40% coverage of total genome) > 1-4kb dna fragment > nick-conn/A-tail(xcept post-morten) > seq

  Hpy166II : 1-4kb read 제일 많고, 37도 활성, DNA blunt ending. 20min

  small fragment removal : AMPure PB bead 75%

  Nick ligation : rCutSmart buffer + M beta-nicotinamide adenie dinucleotide NAD+  + E.Coli DNA ligase

  Remove ends : Klenow fragment

  Hairpin : SMRTball kit, nuclease for remove non-hairpins, repurify ampure bead

  기존 HiFi-seq에서는 Extract -> Prep(SMRTbell), Prep 과정에 있는 A-tailing 없에고

  seq

  Post

  remove non-adaptor reads

  Pbcss, (SMRT-link) : consensus

  pbcss, Q20>, inf.read subrfead

  Lima > demultiplex

  remove unmatching consensus

  pbmm align

  CHM13 / pbmm2 to align

  90% duplexed filter

  Deepvariant + GATK variant calling

  Calling filter

  optmiuzation w/ young , sperm samples

  1 : minimum predicted consensus accuracy 

  2: Minum number of passes per strand

  3: minimum frac tion of subreades detecting the mutation

  I


# Note
```
Overvew of papaer

(1-2-3th seq, 3th : ont & biopac)

backgrounds : Hi-Fi / SBS signature(NMF / LDA)
	Hi-Fi : 3rd long read, ZMW + pol, light emission
	SBS : PCAWG data(4645WGS), extract 67 sigs, (by SignatureAnalyzer, using Bayseian Model(Automatic Relevance Determination))


Why Hi-DEF?
To see what? : Deamination
Different of hi-def <> Hi-Fi

Method section : wet : read length opt(and restriction enzyme selection), heat regulation(deamination, later), A-tailing signatures
	
then pbccs, SMRTlink > remove adaptor and demultiplex, a
pbmm2 align, variant call deepvariant gatk

Filtering section : ds/ss 차이, basic qcs, call filtering optimization
Comparing fidelity of Hidef, by Hi-Fi and Nanoseq, calculation of its error quality

	Call filters : quality filter, sequence ends(10), indel near(2x indel size or 15), remove inheritances, < Tested with iterative optimization, on sperm/child?(blood) 

QC with pass number, rel w/ consensus

Results

Cancer predisposition : POLE PPAP(SBS10ss, call asymmetry), CMMRD(SBS14,ss) increase C>T

Cytosine deamination damage : blood sample , SBS30ss + kinetic signatures, APOBEC3A

MT gene profile : 



```