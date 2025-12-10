HISAT2, STAR, TopHat2 등 비교
https://www.nature.com/articles/nmeth.4106
# Abstract

# Main
- RNA-seq로 나온 결과는 reference genome/transcriptome에 align.
- ref.genome 없이도 가능하지만, alignment-guided하지 않으면 성능이 떨어짐
- 대부분 align 과정에서 splicing 고려하며 설계해 RNAseq specefic.
- BWA랑 Bowtie는 DNA alignment에서 쓰이지만 intron-sized gap을 신경 쓰게 해서 RNA에도 가능

- RNA Polymorphism, Sequencing Error, low-complexity seq, intron-gap, intron signal, incomplete annotation, alternative splciing, pathological splicing 모두가 aliignment를 방해
- RNA-seq aligner는…
	- Splice junction 넘으며 align
	- Paired-end read handling
	- strand-specefic data handling
	- RUN efficient.
	- 해야 함
- annotation이 완벽하지 않기 때문에, unannotated splice junction에서도 매핑할 수 있어야 함
- 총 14개 aligner 비교


- 사용한 시뮤레이션 데이터는 2011/2013년에 사용된 benchmarking study 에 쓰인 데이터.
- 다만 base, read, junction level에서 평가하고 파라미터는 default.
- 메모리 사용량과 시간, uncanonical junction, untrimmed adaptor effect, indel performance, multimapping등도 평가

- human ref로 해도 일부 gene은  mapping이 어렵고, different strain/species에서 mapping하면 매우 어려움
- 그러니 aligner는 high-complex region도 잘 핸들링 해야 ㅏㅎㅁ

# Results
- 데이터는 malaria parasite(*Plasmodium falciparum*) 와 human에서 가져왔고, Complexity level(T1-T2-T3)로 나눔
- 이후 3번 시뮬레이션해 18개의 데이터셋 만듦 →Methods
- *P. falciparum*은 사람과 함께 자주 분석되면서 차이가 명확해서 사용
	- 80% AT rich
- 각 데이터셋마다 10M 100-base read pair 생성

- 데이터셋(T1-T3)의 경우 polymorphism rate + Error rate를 T1이 낮게, T3가 높게 설정했음

# Base and read level
- base / read / junction metric
	- base metric은 제일 정확해야 함
- recall : fraction of all bases that aligned coorectly 
	- 잘 정렬된 base의 비율
		- 모든 base에 대해서, 잘 align된 base의 비율
	- [[Terms#Sensitivity]]
- precision : fraction of all aligned base that aligned correctly
	- [[Terms#Precision]]
	- 모든 aligned base에 대해서, 잘 align된 base의 비율

[[Terms#Confusion matrix]]
- Pricision은 T3 complexity에서도 잘 유지
- Recall은 많이 차이남
- = 일단 align하면 base도 잘 맞추려고 함

