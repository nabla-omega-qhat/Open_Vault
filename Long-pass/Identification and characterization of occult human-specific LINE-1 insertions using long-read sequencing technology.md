# Introduction
- 인간 45%는 TE. > DNA TE / LTR TE/ LINE / SINE
	- 그 중 LINE의 L1이 17%
		- 그리고 80-100개의 active full-length human-specefic L1s(L1Hs).
- L1s의 Retrotransposition element는 6kb, 5’UTR(promotor)-ORF1,2-UTR3’-polyA
	- Insertion TPRT로
		- >Hallmarks, 5’ truncated 
		- flanking TSDs
		- Insertion @ ORF2p endonuclease consensus(TTTTT/AA)
		- L1-mediated 3’ transductions
- L1 insertion > muta/germline event / acts as driver in certain  cancers
	- Somatic insertion은 Neuron progenitor에서도 발생
	- Short-read로는 힘듦
- Long-read면 되는데, L1 hallmark 안 쓴다. 
- PALMER 만들었다. 
	- PacBio 쓰고, NA12828 benchmark genome에서 돌림.
	- 그 외에도 illumina WGS, 3’ targeted L1 capture로도 검증
	- 이후 single0-cell WGA. 

# Materials and methods
## Resolving germline non-reference L1Hs insertions from PacBio data
- PALMER
	- 먼저 repbase에서 알려진 L1들을 long-read에서 mask
	- 이후, Hot L1으로 알려진 L1.3에 대조해서 찾음
		- putative insertion seq가 있으면, candidate
		- up 50bp upstream(5’), 3.5kb downstream(3.5kb)에서 TSD motif, 5’ transduction and polyA 찾음
Fig1
- 50x coverage의 NA12878 pacbio에서.
	- ambiguous read 제거, FLAG로 suppl/sec/dupl-align/low-mapq 제거


- FP 줄이려고 filter
	- pacbio subread(?) req for L1Hs insertions
		- = mean $\mu$ - std $\sigma$ of pacbio read coverage on insertion sites
	- ≥25 bp seq identical to L1.3
	- ≥20 bp polyA seq
	- bounding with ≥6 bp TSDs

- Sequncing depth 영향 보기 위해 downsample해봄
	- 8 6 4 20% > 4 3 2 10x coverage
-
- accuracy 올리기 위해(주로 base accuracy 낮은 read)
	- Error correction은 CANU. 이후 local realign은 BLASR로 다시 정렬함.
	- 이렇게 하면 locally aligned error-corrected reads 얻고, 이걸로 다시 찾음
- pipeline > github
- ref seq랑 error-corrected read를 BLASTn을 써서 recurrence plot으로 그리고 확인함

## Validation from sequence data from BLAST database and clone data
- BLASTn으로 NCBI nr/nt db에 L1Hs seq가 있는지 확인
- NA12878에서 찾은 non-ref L1Hs 평가를 위해서 해당 lib의 end-seq clone pair을 찾아봄
	- ?? 

## Short-read WGS data of NA12878 and L1Hs 5’ genomic DNA/L1 junction sequence k-mer analysis
- 사용한 데이터
	- 50x coverage WGS of NA12878, NA12891, NA12892(CEPH)
	- 30x coverage linked-read NA12878(10x genomics)
	- hs37d5 ref

- 