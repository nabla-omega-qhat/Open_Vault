https://academic.oup.com/nar/article/41/6/e67/2902364?login=true
https://doi.org/10.1093/nar/gks1443
Costello, Maura, et al. "Discovery and characterization of artifactual mutations in deep coverage targeted capture sequencing data due to oxidative DNA damage during sample preparation." _Nucleic acids research_ 41.6 (2013): e67-e67.

- Allelic Frequency : frequency in population
- Allelic Fraction : alt reads / total reads

# Abstract
- Various NGS tech, decreasing cost, and error rates in extraction-prepration step is well-defined
- but not downstream integrity is not evaluated
- discover C>A/G>T transversion at low allelic fractions, and not related with non-biologic mechanism
- find source : acoustic shearing, generate 8-oxoG

# Introduction
- Recent tech.development = low cost, deep-coverage = detect somatic mutation in lower fraction :)

- well characterized sequencer, enzyme….
- but how effect the fildity of downstream mutent calling?
	- such as DNA extraction method, storage condition…

- discover artifactual mutation, in NGS process
- outline experimental results, find source of artificial source, like oxidation
- suggest process to reduce opportunities for oxidation

# Materials and methods

## Illumina sequencing library preparation and agilent SureSelet targeted capture process

- Illumina library prepartion, follows these modifications:
	- inital DNA shearing
	- paired-send adaptor ligation, with palindromic 8 base index
	- gain 150-bp/500-bp fragment, by ultrasonicator
	- hybridization and capture with Agilent SureSelect

## Illumina cluster amplification, sequencing and data processing
- Quantify with qPCR, normlaized to 2nM and denature with  .1 NaOH\
- Cluster amplification (Sequencing-by-synthesis) w/ flowcell
+
## Sample preparation and sequencing on the Ion Torrent Personal Genome Machine(PGM)
- ligation with PGM paired-end adaptor
- Ion sphere templating, PGM sequencing by protocol
- adaptor trimmed before align

## Sequence analysis and mutation calling
- Align with hg19 / bwa
- quality score recalibration (GAT)
- Sequencing metric by picard tools
- QC with SNP, mass spectrometry
+
## Enzyme-linked immunosorbent assay for 8-oxog
- ELISA with monoclonal AB for 8-oxog

## Bead-based buffer exchange and antioxidant evaluation
- Buffer exchange before shearing, use SPRI(Solid Phase Reversible Immobilization) magnetic cleanup
- elute with antioxidant(EDTA, DFAM(deforoxamine mesylate), BHT) buffer or Tris-HCL

# Results
## Discovery and initial characterization of a low allelec fraction artifact
- 221 tumor(melanoma) <> Normal tissue
- observe high number of variant at allelc fraction <20%
	- not the expected C>T transition (by UV damage as report)
	- many C>A/G>T variants, with CCG>CAG context
	- and also observed in normal
	- have strand orientation.
		- G>T always in first read, illumina HiSeq
		- C>A always in second read, illumina HiSeq
		- ohter deep-coverage exome data on other cancer also shown in low allelic fraction
		- = artifical variant, not from nature
- Fig1
	- CCG>CAG mutation shown in low AF(allelec frequency)
	- lego plot
		- color : base modification
		- square locations : preciding and traliing bases pairs
- Fig2
	- CCG>CAG shown in both sample(Tumor/Normal)

- make metric using context, starnd specificity, orientation
	- At high-quality(Q>20) C/G base, classify as 
		- 1) Reference basecall
		- 2) Alternative basecall(A/T) & consistent with artifact characteristics
		- 3) Alternative basecall(A/T) & inconsistent with artifact characteristics
		- 4) Other bases
	- $\text{ArtQ} = -10\text{log}_{10}(\cfrac{\text{Consistent Errors} - \text{Inconsistent Errors}}{\text{All observations}})$
		- 라고 가정하기. (기기와 transversion, 그룹 내 correl 없다는 가정)
		- = $-10log_{10}(\cfrac{\text{G>T / C>A calls on read 1/2} - \text{Other all calls on C/G site}}{\text{All G/C reference location}})$
		- 일종의 Scaled phred error to G>T/C>A transversions, 특정 artifact, 제시한 artifact에 관한 값.
		- ArtQ(Artifact Q) > 30 : <1/1000 base is artifact error
		- 30 as threshold
## Determining the origin of the artifact
- 위 ArtQ  값을 human targeted capturs에 대해 모두 계산, <30 library도 있음
- Calculate ArtQ on all sumane WGS, targeted capture sequences
- artifactual C>A/G>T transversion shown… WGS << targeted capture
	- Prob. effect from SureSelected targeted enrichment process?
	- also artifactual C>A/G>T transversion이 특정 동일 시기의 targeted capture sample에서 안보임
- Sequencing 370 pre-exome enrichment libraries and post-exome enrichment.
	- Both, ArtQ score are highly correlated
	- = Base change were already induced
	- Lab protocols are streamlined, almost same in WGS/Targeted except acoustic shearing(150bp/500bp:WGS)
- Fig 4
	- Pre-Post targeted capture, correl ArtQ
- Then see 150bp shearing introduce C>A/G>T?
	- Sample from low-ArtQ sample and 150/500bp shering
	- = Lower artQ at 150bp shearing = high base changes
- Fig 5
## Interaction of shering protocol with incoming genomic sample quality

- But saw upper(Fig3) less than a half of sample are less affected. and there were processed on 96-well together
- Same dna sample but different library shows simillar ArtQ
- = 150 searhing is not sufficient to cause. Some Inherit proterty exits

- On each disease project, rate of artifact varied without pattern
- separate pre-extracted DNA samples with ‘colleciton sites’, origin of collaborators/institutions
	- These separate labs are not using same protocols
- Observed artifact’s prevalence, by collection site
- = Upstream method also affects
- Fig6


## Confirmation of DNA oxidation mediated mutation during DNA shearing
- Determining molecular basis of artifact.
- C>A/G>T transversions are mostly occur in CCG>CAG context → Oxidation of DNA is problem?
	- G→8-oxoG.
	- Also CCG/GGC motif has highest oxidation potential
	- Various possible sources : long-term storage, heat, metals, sonication
	- @ Sonification process, shearing tube temp increase 10→30 C(dispite submerged in 10C bath) and not increase much in 500bp protocol
	- = Power of sonication + HEAT => oxidization

- To confirm, 8-oxoG ELISA on melanoma sample, 6(highly-affected, artq<20), 6(rel.unaffected, artq>30)
	- split into 3 eq aliquots and 150-shear/500-shear/unsher
	- then ELISA
	- = Elevated levels of 8-oxoG, were present only in high-affected sample
	- not significant increase in 500-shear.
- Difference between oxoG leves between affected-unaffected samples?
	- Using SPRI bead, purify
	- = significant decrease, contaminant contributes oxidation
- Fig 7

- Conclusion the shearing-induced oxidation hyphothesis
	- Source buffer contaminant + High heat and energy via sonication = Generate high-oxidative environment
	- = generation of 8-oxoG, passed through NGS/PCR step
		- with owing to Hoogsteen base pairing(8-oxoG=A)
		- amp durning PCR enrichment

## Development of methods to reduce DNA oxidation durning shearing

- Experiment with antioxidant before shearing
- EDTA, DFAM, BHT on Tris-HCL buffer
- = Protection by oxidation, EDTA/DFAM > BHT
- Fig 8
- = All DNA samples are buffer exchanged to TE buffer'
- Metal-catalysed oxidation is main mechanism

## Development of a filter-based method for removing oxidation artifacts from sequencing data durning mutation calling
- artificial C>A/G>T transversion disrupts mutation calling
	- Reprocess with ArtQ also preferred, but cannot use when data cannot be reprocessed
	- → Post-filtering method
		- screening oxidation induced artifacts
		- make confidence at low allelic fraction
		- also work with other, like C>A in lung adenocarcinomas and smoking

다시 정리
- 기존 artifactual transversion을 ArtQ로 다시 reprocess할 필요가 있음
- 하지만 이미 영향을 받은 데이터셋은 post-filter가 필요
- 그리고 이런 artifact는 low allelec fraction 가짐
- 여기서는 somatic mutation은 작은 allele fraction에서 보이지만, artifact는 low fraction에서만 보임을 이용

- Mutation calling은 MuTect, using Tumor_lod(Tumor_limit of detection)
- FoxoG(Fraction of alternate allele reads in the oxoG artifact configuration)
	- R1G>T와 R2C>A allele의 비율로 하려고 했는데
		- $FoxoG = \cfrac{\text{R1 G>T \& R2 C>A} - \text{All others}}{\Sigma alt.alleles}$(Naive)
		- https://archive.broadinstitute.org/cancer/cga/dtoxog
	- nonC>A/G>T인 경우에도 위해서 R1 G>CT, R2C>AG로.
		- $OXI_Q=-10log_{10}(\cfrac{\text{C,G>A on oxidated sample} - \text{C,G>A on refrence sample}}{\text{C,G>C}})$
		- 
		- 
		- 
		- $OxoQ_G = -10log_{10}(\cfrac{\text{G>A}}{\text{G>A + G>C}} - \cfrac{\text{C>A}}{\text{C>A+C>C}} )$
		- 
		- G reference site에 대해서 C>A/G>T가 예상치와 비교한 결과
		- https://github.com/broadinstitute/picard/blob/master/src/main/java/picard/analysis/CollectOxoGMetrics.java
			- 
FoxoG <> OxoG/C(piccard), OxiQ <> OxoQ(D-ToxoG)  at MuTect2
https://bio-info.tistory.com/39

- Base quality does not reflect that base is true mutataion
- We uses MuTect as mutation caller, adding post-filter here
	- Remove CCG>CAG Oxidation
	- G>T on read 1, C>A on read 2
	- filter (https://broadinstitute.github.io/picard/picard-metric-definitions.html#CollectOxoGMetrics.CpcgMetrics) : 
		- one mettric called FoxoG(Fraction oxoG)
			- $FoxoG = \cfrac{R1 G>T}{R2 C>A}$ (Naive)
				- $FoxoG = Grer/Crer$ x
				- $\text{FoxoG} = \cfrac{\text{G>W}}{\text{G>D}}/\cfrac{\text{C>W}}{\text{C>H}}$? x
		
			- G>T가 R1/ G>T가 R1이면서 C>A가 R2에 있는 경우 / 전체 G>T variant
			- 반대로 C>A에 대해서는 C>A R2/
		- oxoQ? (At Piccard, not this paper)
			- $\text{Oxidation\_Error\_Rate} = \cfrac{\text{max} ( \Delta _ {[Control, Oxidated]} (\text{C>A} + \text{G>T}) }{\Sigma _{Ref} (G \lor C)}$
			- $\text{C\_Reference\_Oxidation\_Error\_Rate} = \text{C\_rer} - \text{G\_rer}$
			- $C_{rer} = \cfrac{\text{C>A,T}}{\text{C>C/A/T}}$
			- then make it as picard-like (-10log10)
			- $OxoQ_C = -10log_{10}(\cfrac{\text{C \rangle A,T}}{\text{C>C,A,T}} - \cfrac{\text{G>A,T}}{\text{G>G,A,T}})$
			- $OxoQ_C = -10log_{10}(\cfrac{\text{C}\rangle\text{W}}{\text{C}\rangle\text{H}} - \cfrac{\text{G}\rangle\text{W}}{\text{G}\rangle\text{D}})$
			- 
			- 
			- also works on G
			- see CollectOxoGMetrics (Picard)
			- https://github.com/broadinstitute/picard/blob/master/src/main/java/picard/analysis/CollectOxoGMetrics.java
		- also use MuTect ‘Tumor_lod’ score
			- = Tumor limit of detection
			- log odds observedd in tumor/reference
			- $LOD+r = log_{10}\cfrac{P(\text{Observed data in tumor|site is mutated})}{P(\text{Observed data in tumor|site is reference})}$
			- $LOD_N = log_{10}\cfrac{P(\text{Observed data in normal | site is reference})}{P(\text{observed data in normal | site is mutated})}$
			- MuTect detects variant in tumor sample by Bayesian classifier
			- 
		- Then build linear inequality : as filter
			- $\text{tumor\_lod} > -10 + (100/3) \text{FoxoG}$
		- context different filtering
			- 1) Full : CCG>CAG
			- 2) Partial : NCG>NAG or CCN>CAN
			- 3) No : VCH > VAH (IUPAC code)
		- fig9
-

# Discussion

- 알려지지 않은 DNA damaging mechanism
	- 하위 데이터 분석에 영향을 줄 수 있음
	- 특히 점점 low freq variant를 lowest allelec fraction에서 찾으려고 함
	- 이런 low allelic fraction에서 artifact는 중요함
	- 특히 high-powered acoustic shearing에 주의
- 또한
	- random stochastic damaging이 증폭되고 있는 것 같음. 시간에 따라 점점 늘어났음
	- exome 양의 문제로 생각되어서 3ug>100ng 로 감소시킴
- 이런 upstream effect에 따라 extraction/storing tech가 SNP calling에 영향ㅇ
	- all incoming buffer change해서 reducing oxidation하는 중
- 









http://gatkforums.broadinstitute.org/gatk/discussion/8771