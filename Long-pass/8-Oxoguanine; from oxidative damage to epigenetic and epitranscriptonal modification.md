[Hahm, J. Y., Park, J., Jang, E. S., & Chi, S. W. (2022). 8-Oxoguanine: from oxidative damage to epigenetic and epitranscriptional modification. _Experimental & molecular medicine_, _54_(10), 1626-1642.](https://www.nature.com/articles/s12276-022-00822-z)

### Paper
#### Abstract
- ROS(Reactive Oxygen Species) oxidizes, cause diversity(phenotypes)
	- (Hydroxyl radical, H$_2$O$_2$…)
- In nucleic acids, Guanine is most vulnerable(8-oxo-dG)
	- It induces G_T(C_A) mutations
	- in RNA(o$^8$G) effects qualtiy, translation and decay
	- Also work as epigenetic mod.

#### Introduction
- ROS(radical, h2o2)등은 metabolism + env stress, homeostatic regulation 에서 발생
	- 원래는 ROS concen.은 antioxidant path로 조절
	- Oxidizes lipid, protein, nucleic acids = oxidative damages
- 그 중 8-oxo-dG, o$^8$G(DNA/RNA)가 존재.
	- 8-oxo-dG
		- 는 ROS marker로도
		- G-to-T [[terms: transversion]] (=C-to-A) 
		- Repair with BER(Base excision repair) w/ OGG1(8-oxoguanine DNA glycosylase)
		- Check Fig.1
	- o$^8$G
		- pair with A
		- effects RNA structure, translation error, activity
		- alter RNA-RNA interactions
- = epigenetic/epitranscriptional effect,  caused by oxidative damage

#### 8-Oxoguanine in DNA
##### 8-Oxo-dG
- DNA damaging freqently~70000 bases(by UV, alkylation, carcinogen, ROS…)
- = triggers DNA damage response and repair
- 100+ oxidative DNA adducts

- G가 가장 낮은 redox potential = most oxidize, @ C8 oxygen
	- 주변 sequence 따라 영향
- 2+ oxidative lesion : OCDLs(Oxidative clustered DNA lesions)

- 8-oxo-dg는 syn conformation 의 adenine과 pair
	- oxidation으로 인해 8-oxo-dGTP상태일 때에도 adenine에 결합 가능
- 이 결과 T-G / A-C mutation
	- = polymerase-induced cytotoxicity

##### 8-Oxo-dG repair pathways
- BER은 작은 base 오류에 대해 수정
	- DNA glycosylases로 base 제거
	- 남은 AP(Apurinic, Apyrimidinic) site를 endonuclease로 처리해서 SSB(Single-strand break)
	- Single nucleotide filling(short-patch BER) 혹은 2-10 nucleotide synthesize(long-patch BER)로 처리
- 다양한 glycosylase가 BER에서 작용 (oxidation, alkylation, deamination target)
	- OGG1이 8-oxoguanine
- monofunctional DNA glycosylase는 Separate AP endonuclease(APE1, APE2)와 함께 BER의 SSB(Glycosylase → Endonuclease). 아니면 bifunctional endonuclease

- OGG1은 (C-G*) 8-oxo-dG 제거 / bifunctional DNA glycosylase.(cleave 3’ of AP site)
	- → producing 3’-dRP and 5’-P via $\beta$-elimination
	- APE1 hydrolysis @ 5’ end of AP
	- > AP site backbone cleveage 
	- but OGG1 has low efficiency of AP lyase
		- → produce diff.types of DNA ends
			- AP endonuclease : 3’OH, 5’dRP
			- AP lyase : 3’dRP 5’P
	- SSB detect → PARP1, 2 : poly(ADP-ribose) (PAR) polymerase, PARylation
	- then recurit downstream repair(XRCC1...)

- pol $\beta$ : dRP lyase activity
	- Short-patch BER
		- pol$\beta$ excises 5’-dRP  insert nucleotide
		- DNA ligase III ligation
		- XRCC1 complex
	- Long-patch BER (freq.used for OCDL)
		- pol$\beta$ insert first nucleotide
		- elongate with DNA polymerases(pol $\delta$, $\epsilon$)
			- PCNA(Proliferating cell nuclear antigen) helps pol$\delta$ to synth w/ sliding clamp, FEN1
			- RFC(Replication factor C) facilitates PCNA loading
			- RPA(Replication factor A) stablizes synthsized DNA for pol.
		- “Flap” structure resolve with flap endonuclease (FEN1) : remove displaced oligonucleotide
		- seal with DNA ligase I

- MUTYHs(MutY DNA glycosylases) repaires 8-Oxo-dG by removing A at A-G* pair
	- remove adenine / paired with 8-oxo-dg
	- monofunctional = opposite of 8-oxo-dg excised by APE1
	- replacing as C with pol $\lambda$ complex with PCNA, RPA by FEN1
	- Ligation with ligase I
- NEIL1(Nei-like DNA glycosylase 1) removes 8-oxo-dG
	- but main function in oxidized pyrimidine / ring-opened purine.(fapyG, fapyA)
	- ~homologous to bacterical fapy-DNA glycosylase(Fpg)

- Other repair mechaninsm
	- Fundamentally remove 8-oxo-dGTP from **nucleotide pool**
		- MTH1(MutT homolog 1) hydrolyze 8-oxo-dGTP
	- TC-NER(Transcription coupled nucleotide excision repair) also remove 8-oxo-dG in transribed strand
	- Mismatch(G*-A) after replication can be removed by MMR(Mismatch repair)
		- w/ MMR proteins(MSH2, MSH6)
	- MPG(N-methylpurine DNA glycosylase), RPS3(40S ribosomal protein S3) can cleave 8-oxo-dG
	- Main : OGG1

##### 8-Oxo-dG-induced mutation and genome instability in cancer
- 8-oxo-dG involved with ROS-related diseases
	- ex) aging, neurodegeneration, cancer
	- G* induce G>T(C>A) impairment
		- proven by proto-oncogene HRas
	- but mutageneic G* make different helix structure, repair rapidly

- Cancer-related studies
	- OGG1-KO mice : high 8-oxo-dG, G>T mutation, tumor develop
	- MUYTH KO, MTH1 KO aslo produce higher tumor, G>T mutation
	- KBrO3 treat : KO mices also attacked
	- double KO : oncogene mut.

- Defection in 8-oxo-dg repair shown in cancer patients
	- OGG1 locus deleted in cancer serverally
	- G>T mutation(8-oxo-dg) widespread in cancer
		- Known as major somatic mutation
		- Can find in Single base subtitutions(SBS) db(COSMIC)

- Genome stability <> CD / Tumorgenesis
	- OGG1, MUTYH, APE1, NEIL1
	- but also high BER activity impairs genome.
		- High APE1 activity is known in cancer
	- Unrepaired 8-oxo-dG hinder the control of gene topology(=Genome destabilize)
		- TOP1(Topoisomerase I) affinity increase by 8-oxo-dG
			- cause abnormal cleavage

##### 8-Oxo-dG induced transcriptonal mutations in diseases
- 8-oxo-dg also effects mutation in transcription
	- RNA pol high fieldity, cause C>A transversion
	-  = Transcriptional mutation(TM)
	- 
