https://www.nature.com/articles/nrg3030
# Abstract
- TE는 다른 genomic location으로 움직이는 특이한 능력이 있고, NGS 통해서 TE와 Host간 영향(Integration sites, Activation patterns…) 볼 수 있음
	- 다른 mechanisms(sRNA-RNAi / Epigenitic marks)과도 interact
- 최근에는 TE의 이동이 host genome에 diversity를 늘려 긍정적인 영향도, 반대로 cancer 형성에도 영향을 준다는 점이 알려짐
- =정리하려고 함

# Main
- 시작은 Barbara McClintock이 옥수수(maize)에서 TE가 Chromosomal integration
- 이젠 대부분의 생물에 DNA-RNA intermediate를 통해 진화 과정동안 증폭되어 있음
- 이런 TE는 host에 의해 산발적으로 선택되어 남지만…
	- 사실 움직이는 genomic parasite와 비슷함
- 이제는 DNA seq tech와 데이터를 통해 TE를 organism에 따라 content, mobility 등을 볼 수 있음

- Multicellular eukaryote에서는 TE는 gamate나 early-development stage에서 움직여 다음 generation으로 넘어감
- 현재(2011) 65개의 *de novo* TE Insertion으로 인한 disease case가 보고되어 있고 질병의 1/1000 정도 케이스를 차지
- 최근의 cell cultured based experiment + new tech 로 active TE가 기존에 알려진 것보다 더 많이 인간에 있다고 밝힘
	- Early development stage에서 Mammalian TE integration이 더 자주 발생
	- Neurogenesis와 특정 cancer에서, 특정 somatic cell에서 TE activity 가 더 높음
- 아직 빙산의 일각이지만, TE mediated event와 inter/intra structual variation in genome을 알아내야 함

- TE mobility는 host fitness에 큰 영향을 줌
- 동시에,(Paradoxically) 잘못된 TE insertion은 Host, TE 생존에 영향을 줌
- 그래서 많은 TE는 targeting mechnasim을 통해 genomic ‘save havens’에 insertion됨
- 물론 host도 restriction mechanism(like methylation, sRNA, cytidine deaminase) 등으로 TE를 억제

- TE가 특정 env. conditiaon에 활성화한다는 증거가 늘어나고 있음.
	- like stress → induce TE transcription, integration, redirect target sites(alt)
- 이건 Barbara McClintock의 환경이 생존을 위해 genetic diversity를 유도한다는 가설과 맞음

- 여기서는 TE types와 mobility modes, 그리고 Te integration mechanism, host defending mechanism, 그리고 최근 연구인 TE mobility의 영향과 target selection, active TE in humans 등을 이야기


## The Diversity and abundance of transposons
### Mobilization mechanisms
- TE는 다양한 방법을 통해 mobilize. 
- 많은 DNA transposon은 non-replicative, `Cut and paste` mechanism을 씀
	- transposase가 near TE - inverted terminal repeats를 찾아 TE를 자르고 다른 genomic location에 붙임

- Retrotransposon은 RNA intermediate의 Reverse transcription을 통해 mobilize.
- 그런데 여기서도 mechanism 차이에 따라 나뉨
- Long terminal repeat(LTR) transposon
	- 은 element-encoded enzyme(Integrase)를 통해 mobility.
- Non-LTR transposon
	- 은 endunuclease와 RTase로 genome에 priming
	- 이 단백질은 non-autonomous SINE(Short interspersed elements)의 mobility에도 영향
Fig1
### Transposon activity across species
- *Escherichia coli*의 Tn5, Tn7, *Drosophila melanogaster*의 P elements, *Caenorhabditis elegans*의 Tc1가 DNA TE로 알려져 있고, 박테리아나 simpler eukaryote에서도 발견됨
- 대부분 mammel에서 DNA TE가 관측되고 evolution에 영향을 주었을 것
- 하지만 최근에는 `hAT elements`랑 `helitrons`같은 DNA TE가 박쥐 종에서 발현됨도 알려짐
- =Seq tech이 더 잘 확인 가능

- LTR Retrotransposon은 eukaryotes에서 더 abundant
- *D. melanogaster* 는 20개의 서로 다른 familiy의 LTR Retrotransposon을, 약 ~1%정도 genome에 가짐
- 근데 maize는 약 400 family의 LTR Retrotransposon이 ~75% genome에 가짐
- mouse의 경우에는 몇개의 활성화된 LTR Retrotransposon family를 가짐
	- 이런 retrotransposon(LTR/Non-LTR)으로 인해 non-autonomous derivate가 10-12%의 sporadic mutation을 차지할 거라고 생각됨
- 반대로 human에는 매우 적은 LTR retrotransposon activity
	- 이 중에서 일부 retrovirus의 polymorphism은 비교해서 최근에 retrotranposed 된 것으로 추정됨

- Non-LTR Retrotransposon은 eukaryote에 널리 퍼져 있고 mammalian genome에서 특히 prolific.
- LINE-1(Long interspersed element 1)과 non-autonomous SINE이 mobilize되며 30%의 human genome을 차지. (ex: *Alu* Element, *SVA* Element: SINE-R-VNTR-Alu)
- 최근 연구에서는 L1의 dimorphism(presence, basence)과 non-allelic recombination between L1 and *Alu* 가 inter-individual structual variation의 일부를 차지하고 이게 human genome에 영향 줌

## The diverse patterns of integration sites
- TE는 다양한 integration behavior를 보이고
- 일부는 gene-dense region을 선호, 다른건 heterochromatin, telomers, ribosomal DNA 영역을 선호하기도 함
- 여기서는 TE integration의 예시를 통해 DNA에서 target site를 보기로 함

### Integration into gene-rich regions
- 많은 TE는 gene-rich region에 integrate, 그리고 ORF disruption을 방지하는 mechanism을 사용함
- 극단적인 예시는 *E.coli*의 Tn7 DNA TE. 
	- Tn7은 sequence-specific DNA binding protein인 TnsD를 코딩하고, 이게 Tn7이 특정 genomic position인 *attnTn7*로 integration을 mediate
	- 이렇게 genomic damage를 회피
	- 두번째 targeting protein인 TnsE는 conjugation, dsDNA break, DNA replication structure, *E.coli*간 transferring되는 plasmid 등으로 target을 바꿈
- 비교되는 *D.melanogaster*의 P element는 transcript start site 위 500bp에 integration하면서 ORF disruption을 방지
	- 근데 이 P element target이 어떻게 되는지는 설명이 더 필요

- 특정 non-LTR retrotrnasposon은 특정 target site를 가지는 endonuclease를 코딩
- 예시로 곤충의 R1, R2 element는 28S rDNA locus의 특정 sequence specefic endonuclease를 코딩하고 target-site-primed reverse transcription(TPRT)를 시도함
	- R1읜 APE(DNA repair endonuclease)와 유사한
	- R2는 type IIS restriction endonuclease와 유사하게 코딩


- LTR Retrotransposon은 host에 damage를 적게 주면서 gene-rich region에 integration하는 방법을 씀
- *Saccharomyces cerevisiae*