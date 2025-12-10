https://genomebiology.biomedcentral.com/articles/10.1186/s13059-022-02690-2

# Abstract
- TF의 binding site를 찾는 일은 중요하고, 이를 알아야 genomic region evaluation 
- 다만 많은 TF는 sequence-specefic하지 않고, motif들이 잘 설명되지 않음
- V.Chip-seq은 각 TF를 cell type에 맞게 binding site를 추측함
	- 이는 cell type + expression profile간 연관성을 tf binding site와 chromatin accessibility와 함께 학습시켜서 추정
	- 이 방법은 단순 시퀀스만 가지고 binding 을 추정하는 것보다 좋았고, 36TFs에 대해 작용(MCC>0.3)
# Background
- TF들은 transcription regulation site들에 binding하면서 expression 조절
- 그리고 시퀀스나 TF 숫자의 변화는 disorder, disease, cancer 등의 주 원인


- TF는 접근 가능한 크로마틴에 약한 covalent interaction(아미노산-핵산간)으로 결합
- DNA의 primary struct(=sequence), secondary struct(=shape), tertiary struct(=conformation) 모두 TF binding에 영향
- 많은 TF는 DNA에 간접적으로 binding.
	- 이런 경우, in-vitro 데이터 기반 모델의 in-vivo에서 잘 작동하지 않게 되ㅏㅁ
	- 그래서 우린 context-dependent한 모델을 구상

- ChIP-seq은 sample에서 TF의 존재를 mapping 가능
	- 이를 위해 ChIP-seq은 1M-100M cell과 적절한 Ab가 필요함
	- 하지만 저정도 양은 clinical sample에선 안 나올수도 있고, 
	- 그래서 대부분의 disease system에서 TB binding을 기계적으로 알아내긴 힘듬
- [[Terms#ATAC-seq|ATAC-seq]]은 반대로 100-1k cells를 요구하지만 chromatin accessibility는 TF binding을 직접 결정하진 않음.
	- 그래서 TF sequence preference, genomic conservation, 그 외 feature로 TF binding을 추정

- 우린 TF binding을 더 정확한 툴로 추측하면 더 많은 context에서 tf binding의 역할을 알아낼 수 있을 것.

- 기존 방법으로 hierarchial mixture models, HMM 등이 chromatin accessibility 데이터로부터 TF footprint를 얻었슴
	- 이런 방법은 sequence motif score로 TF footproint를 다른 TF에 연결시킴(?)
	- 하지만 Sequence motif score는 높은 FP, FN
	- TF의 cooperative binding과 sequence specificay의 분산은 정확히 TF binding을 추측하기 힘들게 함

 - 여러 연구들은 서로 다른 benchmarking을 시도
	 - 몇몇 경우는 TF sequence motif에 맞는 genomic region만을 pred.
		 - 이런 경우 TF seq motif 이 아닌 곳의 ChIP-seq peak를 제외함으로써 FN peak underestimate + overestimate pred accuracy
	 - 최근 ENCODE-DREAM in-vivo binding site pred challenge가 TF binding prediction에 가이드라인 제공
		 - 이들은 FN 예측을 보이는 auROC(area under the receiver operating characteristic curve)의 하부 영역 평가, FP 예측을 평가하기 위해 auPR(area under the precision-=recall curve) 모두를 제시하길 권장
		 - auROC, auPR 모두 모델 성능을 scoring classifier로 생각하고 계산함
			 - 이 경우, 예측을 계층화하는 threshold 값에 의존
		 - 우린 추가로 MCC(Matthews correlation coeff)로도 평가.
## Virtual ChIP-seq
- 