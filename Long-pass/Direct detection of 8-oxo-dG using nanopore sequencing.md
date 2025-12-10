https://www.nature.com/articles/s41467-025-60391-3

# Abstract
- DNA는 지속적인 oxidative damage를 받고 있고, 8-oxo7,8-dihydro-2’-deoxyguanosine(8-oxo-dG) 가 mutagenesis, epigenetics와 gene regulation에 영향 주고 있음
- 기존 detection 방법으론 indirect한데, nanopore sequencing은 직접 확인이 가능
	- 다만 traning data가 없어 detection mech. 없음
- 그래서 synthetic oligo로 긴 8-oxo-dG context-variable DNA 분자를 만드는 방법을 만들었음. >>> deep learning / nanopore에서 쓰자
- 특히, 우리가 사용한 학습 방법은 o8G의 희소성에 대해 특이적으로 검출 가능하게 했음
	- 아마 학습 잘 시켰다는 의미인듯(데이터셋 크기 차이 등)
- tissue culture + oxidative damage에서도 해봤는데, uneven genomic o8G distribution과 C>A context patterm, local 5mC depletion을 확인함
- 이 m5C + o8G single-molecule resolution detection은 조와요!
- 이런 방법이라면 rare dna modification을 synth.DNA + nanopore로 찾을 수 있을 것
# Introduction

