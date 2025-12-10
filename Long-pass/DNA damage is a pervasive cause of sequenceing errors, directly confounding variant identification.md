[Chen, L., Liu, P., Evans Jr, T. C., & Ettwiller, L. M. (2017). DNA damage is a pervasive cause of sequencing errors, directly confounding variant identification. _Science_, _355_(6326), 752-756.](https://www.science.org/doi/full/10.1126/science.aai8690)

[Suppliment](https://www.science.org/doi/suppl/10.1126/science.aai8690/suppl_file/chen-sm.pdf)

# Paper
## Intro
1.  Intro
	- 기존 NGS 기반 분석은 point mutation 등으로 cancer 분석과 같은 형태. (Allelic differences)
		- 이 경우 low allelic freq를 가진 mutation은 deep seq 등 특별한 분석 필요
	- Sequencing 과정 중 error는 PCR miscalling으로 주로 생각됨
		- 특별한 sample(CTC, formalin-fixed paraffin-embed:FFPE) 에서 DNA err 있다고 생각
## Research
- .  Research
	- 그래서 Paired end(R1/R2)에서 variant detection
	- 7,8-dihydro-8-oxoguanine(8-oxo-dG)는 amplification에서 G-to-T transversion 있음
		- 8-oxo-dG는 sonification-oxidation으로 인해 생길 수 있음
		- 이렇게 R1 read에 G-to-T가 생기면 amp 과정에서 R2에는 C-to-A complement varint가 보일 것.
	- GIV(Global Imbalance Value) : 전체 Genome에서 양 strand imbalancing value
		- 1 : OK
		- 1.5 : Variant type 중 1/3은 errorneous = DAMAGED
		- 1000 Genome project / TCGA(The Cancer Genome Atlas)에 해보니까 문제 있음
	- 결국 low frequency variant(somatic variant, germline varint...)등이 이런 damage로 오염되었을 수 있음
	- 직접 lib prep- sequencing, sequencing depth 높게 repair enzyme과 함께 돌려봄
		- low-frequency modification이 사라짐
		- 실제로 R1에 G>T, R2에 C>A가 나옴.
	- >2M read에서 정확함
	- = False positive이 많을 수 있음
2. Verify, Conclusion
	- TCGA dataset에서 subtition 종류(G to T, C to A)가 가장 많음을 확인
	- 이후 False Positive를 확인해 보니 50% 이상, age와 연관됨
	- Varscan 한 variant들도 차이가 많이 남.
	- 결국 Rigid somatic variant를 찾으려면 
		- Sequencing coverage 늘리기
		- stringent variant frequent threshold 설정
		- postprocess - filtering @ high-confidience variants
	- 
### Suppliment
- Data Analysis
	- Paired-end read는 [Trimgalore]((https://github.com/FelixKrueger/TrimGalore)로 trimming
	- 이후 hg19 mapping w/ BWA-MEM(k=1)
	- local realignment : GenomeAnalysisTKLite (GATK, IndelRealigner)
	- R1/R2 read 분리
		-  P5 primer / P7(3’) primer단 묶어서, 5’단부터 읽으면 R1, 3’단부터 읽으면 R2
	- 각 group에 대해 Mpileup
- [Main Code](https://github.com/Ettwiller/Damage-estimator)
	- [estimate_damage_location.pl](https://github.com/Ettwiller/Damage-estimator/blob/master/estimate_damage_location.pl)
	- Check code section below
		- location : 각 paired read의 순서에 따라 값 정렬
		- context : 아무래도 mpileup 행간 context를 봐서 정보를 추가하는 것으로 보임
- GIV(Global Imbalance Value)
	- 8-oxo-dG에 대해 생각해 보면, oxidative damage가 G에. 
		- 근데 polymerase가 G-to-T misread
		- 그러면 반대 R2 read에서는 C-to-A misread 발생 
	- $GIV_{G\_T} = \cfrac{\cfrac{C1v + C2v}{C1 + C2}}{\cfrac{C1v_{RC}+C2v_{RC}}{C1_{RC} + C2_{RC}}} = \cfrac{\cfrac{\text{R1 G>T + R2 C>A}}{\text{R1ref G + R2ref C}}}{\cfrac{\text{R1 C>A + R2 G>T}}{\text{R1ref C + R2ref G}}}$
		- C1v : G-to-T varint in R1
		- C1 : G in R1
		- C2v : C-to-A variant in R2
		- C2 : C in R2
		- C1v_RC : C-to-A in R1
		- C1_RC : C in R1
		- C2v_RC : G-to-T in R2
		- C2_RC : G in R2
		- Prob. RC means Rev.Compl
	- 결국 G to T misread + SNP가 모두 존재할 수 있는데, oxidation(misread)는 strand에 편향되어(양쪽 다는 불가능) 나타날 것이지만 SNP는 양쪽 모두에 보일 것
	- Update @ [[2025-03-05]] 
	- $GIV_{G\_T} = \cfrac{\cfrac{F1v + F2v}{F1 + F2} + \cfrac{R1v_{RC} + R2v_{RC}}{R1_{RC} + R2{RC}}}{\cfrac{F1v_{RC}+F2v_{RC}}{F1_{RC} + F2_{RC}} + \cfrac{R1v + R2v}{R1 + R2}} = \cfrac{\cfrac{\text{FR1 G>T + FR2 C>A}}{\text{FR1ref G + FR2ref C}} + \cfrac{\text{RR1 C>A + RR2 G>T}}{\text{RR1ref C + RR2ref G}} }{\cfrac{\text{FR1 C>A + FR2 G>T}}{\text{FR1ref C + FR2ref G}} + \cfrac{\text{RR1 G>T + RR2 C>A}}{\text{RR1ref G + RR2ref C}}}$
		- Reasoning : Reverse strand의 oxoG의 경우, 반대 양상으로 떨어짐. = Reciprocal. 아니면 결과값이 saturation됨.
			- Reference fragment는 pool되서 떨어짐
			- 이후 P5-P3 어댑터는 방향에 맞게 붙음(5>5’)
			- 이후 쪼개지고, 1차 library 제작됨
			- 이후 PCR Amp
			- 그 중 한 streand 일부가 amp 이후 sequenced
	- $GIV_{G\_T} = \cfrac{\cfrac{F1v + F2v + R1v_{RC} + R2v_{RC}}{F1 + F2 + R1_{RC} + R2{RC}}}{\cfrac{F1v_{RC}+F2v_{RC}+R1v + R2v}{F1_{RC} + F2_{RC} + R1 + R2}} = \cfrac{\cfrac{\text{FR1 G-T + FR2 C-A + RR1 C-A + RR2 G-T}}{\text{FR1ref G + FR2ref C + RR1ref C + RR2ref G}} }{\cfrac{\text{FR1 C-A + FR2 G-T + RR1 G-T + RR2 C-A}}{\text{FR1ref C + FR2ref G + RR1ref G + RR2ref C}}}$ 
		- 확률이니까 합친게 맞음 ㅇㅇ 위 수식은 무시
		- 근데 Reverse strand에서의 C>A라고 해도 BAM 상에선 Reference 위정렬(이미 RC)되어
	- GIV_{G\_T} = \cfrac{\cfrac{F1v + F2v + R1v_{RC} + R2v_{RC}}{F1 + F2 + R1_{RC} + R2{RC}}}{\cfrac{F1v_{RC}+F2v_{RC}+R1v + R2v}{F1_{RC} + F2_{RC} + R1 + R2}} = \cfrac{\cfrac{\text{FR1 G-T + FR2 C-A + RR1 G-T + RR2 C-A}}{\text{FR1ref G + FR2ref C + RR1ref G + RR2ref C}} }{\cfrac{\text{FR1 C-A + FR2 G-T + RR1 C-A + RR2 G-T}}{\text{FR1ref C + FR2ref G + RR1ref C + RR2ref G}}}
	- **GIV is RATIO between misreading base / SNP**
		- [[GIV Explanation]]
		- 다만 mpileup 파일 내에서의 reverse matching(lowercase / comma)에 관한 처리가 코드 상에 없음
		- 
- Code Analysis
```perl
my $QUALITY_CUTOFF = 30;
my $COV_MIN = 1;
my $COV_MAX = 100;
my $SOFT_MASKED=1;

my %RC;
$RC{'A_C'} = 'T_G';
$RC{'A_A'} = 'T_T';
$RC{'A_T'} = 'T_A';
$RC{'A_G'} = 'T_C';

$RC{'T_C'} = 'A_G';
$RC{'T_A'} = 'A_T';
$RC{'T_T'} = 'A_A';
$RC{'T_G'} = 'A_C';

$RC{'C_C'} = 'G_G';
$RC{'C_A'} = 'G_T';
$RC{'C_T'} = 'G_A';
$RC{'C_G'} = 'G_C';

$RC{'G_C'} = 'C_G';
$RC{'G_A'} = 'C_T';
$RC{'G_T'} = 'C_A';
$RC{'G_G'} = 'C_C';

$RC{'A_-'} = 'T_-';
$RC{'T_-'} = 'A_-';
$RC{'C_-'} = 'G_-';
$RC{'G_-'} = 'C_-';

$RC{'A_+'} = 'T_+';
$RC{'T_+'} = 'A_+';
$RC{'C_+'} = 'G_+';
$RC{'G_+'} = 'C_+';

#start the main program =============================
my $relative_count_R1 = get_relative_count($file_R1);
my $relative_count_R2 = get_relative_count($file_R2);
#====================================================

#open the output file ====================================
open(OUT, ">$out") or die "can't save result into $out\n";
#=========================================================  


#go over all the mutation possibilities :
foreach my $type (keys %RC) # is all kind of mutation types

{
    my $ref = $$relative_count_R1{$type}; # and get {pos}{rel/abs} dict on mut type
    foreach my $position (sort {$a<=>$b} keys %$ref) #numerically sort key(pos)
    {
		my $value1 = $$relative_count_R1{$type}{$position}{"relative"};
		my $value2 = $$relative_count_R2{$type}{$position}{"relative"};
		my $abs1 = $$relative_count_R1{$type}{$position}{"absolute"};
		my $abs2 = $$relative_count_R2{$type}{$position}{"absolute"};
	if ($$relative_count_R1{$type}{$position}{"relative"} && $$relative_count_R2{$type}{$position}{"relative"}) # rel value are valid?
		{
		   
	
		    print OUT "$generic\t$type\tR1\t$value1\t$abs1\t$position\n";
		    print OUT "$generic\t$type\tR2\t$value2\t$abs2\t$position\n";
		    #get posiotion based mutation data
		}
    }
}
close OUT;

sub clean_bases{
    #remove unwanted sign (insertion / deletion (but retain the + and - and remove the first and last base information)
    my ($pattern, $n)=@_; #for example : ,$,,
    #remove the ^ and the next character that correspond to the first base of the read. 
	$pattern =~ s/\^.//g;
    #remove the dollars sign from the last base of the read. 
	$pattern =~ s/\$//g;
	
	if ($pattern =~/\+(\d+)/ || $pattern=~/\-(\d+)/)
    {

	while ($pattern=~/\+(\d+)/)
	{
	    my $size = $1;
	    my $string_to_remove;
	    for(my $i=0; $i<$size; $i++)
	    {
		$string_to_remove = $string_to_remove.".";
	    }
	    $string_to_remove = '.\+\d+'.$string_to_remove;
	    
	    
	    $pattern =~ s/$string_to_remove/\+/;
	    
	}
	while ($pattern=~/\-(\d+)/)
	{
	    my $size = $1;
	    my $string_to_remove;
	    for(my $i=0; $i<$size; $i++)
	    {
		$string_to_remove = $string_to_remove.".";
	    }
	    $string_to_remove = '.\-\d+'.$string_to_remove;
	    
	    
	    $pattern =~ s/$string_to_remove/\-/;
	}

	#Find +N or -N pattern and make "."*N pattern at 
    }
    


    return $pattern;
}

sub get_relative_count {
    my ($file)=@_; ##First argument transferred to this subroutine = Filename
    while (<FILE>){
	s/\r|\n//g; ## REGEX in perl. carrage return and newline will be removed
	my ($chr,$loc,$ref, $number, $dbases,$q1, $q2, $pos) = split /\t/;
	# chr : Chromatin name : ch49
	# loc : 1-based position in chromatin : 145
	# ref : Reference base at this position : G
	# number : number of reads on this position : 35
	# dbases : Readed bases w/ mismatcㅊh info : ....,T..*.>>>$
		# + : inserted
		# - : deleted in other end
		# $ : EOL
	# Base quality : phred score of base on each read
	# Alignment quality
	# Comma-separated positions : position of base on each read

	my $bases = clean_bases($dbases, $number); ## Removes deletion, insertion, $ at end of sequence.

	my @tmp = split //, $bases;
	my @poss = split/,/, $pos;
	my $length_mutation = @tmp;

	if ($number == $length_mutation && $number >= $COV_MIN && $number <= $COV_MAX && $ref =~/[ACTG]/)
	    #$number == $length_mutation : making sure that the position array and base array are in agreement. 
	    # $number > 0  : position with at least one read
	    #$number < 10  : at 2 M reads across the human genome we are not expecting tp have a high coverage. #!!! be careful of other species.
	    #ref : making sure the position is not soft masked
	{
	    my @base=split (//,$bases);
	    my @qualities =split(//, $q1);
	    #loop all bases and qualities in read alignment result(filtered)
	    for(my $i=0;$i<@base;$i++){
		#given a character $q, the corresponding Phred quality can be calculated with:                                       
                #$Q = ord($q) - 33;  
		my $quality_score = ord($qualities[$i]) - 33;
		
		if ($quality_score > $QUALITY_CUTOFF)
		{
			# If quality is good(>cutoff) : add 
				#{type}{position(rp)}{ref base _ changed base}
				#{nt}{position(rp)}{ref base}
			# But this base matches(.)
				#{type}{rp}{ref _ ref}
				#{nt}{rp}{ref} added
			# It is 3-times nested hash with 3 keys(type, rp, ...)
			
		    my $ch=$base[$i];
		    my $rp = $poss[$i];
		    if($ch=~/[ATCG]/){
			my $type = $ref."_".$ch;
			$result{"type"}{$rp}{$type}++;
			$result{"nt"}{$rp}{$ref}++;
			
		    }
		    elsif($ch=~/[\+\-]/ && $rp > 5){ #this is to remove the artefact of alignment algorithm that add indels (?) at the begining. Should be delt differently. 
                        my $type = $ref."_".$ch;
                        $result{"type"}{$rp}{$type}++;
                        $result{"nt"}{$rp}{$ref}++;

                    }

		    elsif($ch eq "."){
			my $type = $ref."_".$ref;
			$result{"type"}{$rp}{$type}++;
			$result{"nt"}{$rp}{$ref}++;
			
		    }
		}
		
	    }#end the loop
	}
    }
    close FILE;
    my $positions = $result{"type"};
    foreach my $pos (keys %$positions) # it is rp
    {
		my $mutations = $$positions{$pos}; # it is type
		foreach my $type (keys %$mutations)
		{
		    my $total_type = $result{"type"}{$pos}{$type};
		    $type =~/(.)\_(.)/; ## -> $1 and $2
		    my $total = $result{"nt"}{$pos}{$1};
		    
		    my $value = $total_type/$total;
		    
		    $final{$type}{$pos}{"relative"}=$value;
		    $final{$type}{$pos}{"absolute"}=$total_type;
		}
    }
	 # relative : 특정 pos, 특정 type mutation에 대해서 
    return(\%final);
}


sub reverse_complement {
        my $dna = shift;

	# reverse the DNA sequence
        my $revcomp = reverse($dna);

	# complement the reversed DNA sequence
        $revcomp =~ tr/ACGTacgt/TGCATGCA/;
        return $revcomp;
}


```
- explaination
	- [[Mpileup]] 데이터의 column은 아래와 같음
		- Chromosome name
		- 1-based position on chromosome : chromosome 상에서의 위치(숫자)
		- Reference base at this position : 위 위치에서의 reference가 나타내는 base
		- Number of reads covering this position : 해당 위치를 덮는 reads 개수
		- Read bases : 각 read가 해당 위치에 align이 어떻게 되었는지.
		- Base qualities : 각 read에서 base quality(PHRED)
		- Alignment qualities
		- Comma-seperated 1-base position : 각 read에서 해당 base의 위치(숫자)
	- estimate_damage.pl의 구성
		- 일단 같은 곳의 split_mapped_reads.pl로 R1/R2 read 구분
		- R1, R2의 mpileup 파일을 읽고, 각 행에 대해서 이하 과정 수행
			- sub get_relative_count
				- 각 Line만 따서 가져옴
					- Chr loc ref number dbase q1 q2 pos
				- Preprocess(sub clean_bases), dbase
					- Read bases(alignment 정보) 를 보고, 해당 ^…$ 구조에서 정보만 빼냄
					- 추가로 필요하지 않은 정보(insertion/deletion, +,- 정보를 .으로 모두 대체)을 제거
						- ex ) ...`^+`.+`3GGT`.`$`
						- 변환시 ….+.
				- 이후 read bases > tmp list(base list)
				- comma-position > poss list
				- soft_masked == 0 : ref = uc(ref)
				- QC
					- 일단 해당 위치에 적절한 reference base가 존재해야 함
					- read coverage가 0-10. (Point mutation에 대해  high coverage 기대 안함)
					- read 수와 뒤의 base 정보 수가 같음
				- 
				- Calculation($result)
					- 각 행의 ref base($ref)
					- quality가 일정 이상인 read base에 대해
					- 각 read에서 읽은 base가($ch) 변화했다면(ATCG or +-)
						- ref_ch 값 ++, 그리고 원본 base 수 ++(ref)
					- 변화하지 않았다면(.)
						- ref_ref(무변화) 값 ++, 원본 base 수 ++(ref)
					- 위에서 만든 $result 해시맵
					- ref_ch(type) 은 total_type 값으로, 해당 ref base 수는 total 값으로 반환
			- Calculation +
				- R1과 R2에서 서로 상응하는 변화(G_T면 C_A)에 대해서 total과 total_type값 얻기
				- 각 total_type이랑 total값 합치고 value로
				- value와 그 반대의 value를 나눔
				- 결국 위 식과 동일 ㅇㅇ
				- location 정보가 포함된 분석 파일이 따로 존재(read position 따라서.)
				-  
### Note
- 애초에 Human genome에 oxo-dG가 많았을 경우? (DNA repair enzyme cocktail이 mod를 지운 경우)
- C-to-A도 oxidation?
- Verification이 하필 repair cocktail… Amp 없이 reading하는건?
- [[Terms#MMR|MMR]]같이 아예 수정되면 GIV 낮아질 수 있음
귀하의 몸무게를 늘립니다

### [Comment](https://www.science.org/doi/full/10.1126/science.aas9824)

Also check [[Discovery and characterization of artifactual mutations in deep coverage targeted capture sequencing data due to oxidative DNA damage during sample preparation]], Costello _et al_

기존에 TCGA에서 본인 dataset이 oxidative damage X. 그리고 machine error, oxidation, crosslinking(Formalin-Fixed) 등을 이미 연구했었고, 관련한 pipeline과 library preparation method가 정립됨

1. Oxidative damage level 계산 과정에 있어 GIV는 기존의 oxoQ(??)와 동일. 
	1. 다만 해당 implemetation은 forward strand에 align된 것만 썼었고, low-quality base filtering에 의한 biasing이 존재.
	2. (그리고 phred처럼 낮은 base를 거르는 목적으로 디자인, oxoQ 30>). 
	3. 기존 GIV_GT가 oxoQ<30의 damaged sample에서 aggrement 부족. (Q>30, x100 cover 제외) 
	4. 그래서 더 낮은 Q>20 read들과 100x 이상 read 포함하니까 GIV_GT와 oxoQ가 concord.
2. 8-oxo-G와의 연관된 context가 부족. 
	1. 저자들이 nucleotide-context specificty가 부족하다 했는데, 이는 위에서 말했듯 높은 Q의 read만 사용했기 때문 = damaged read를 제거한 데이터임
	2. Fig1. lego plot에서 보다시피 Q>30에선 damaged base profile이 잘 안보임/distorted (원래 clear peak @ CCG>CAG)
	3. 따라서 8oxoG의 sequence context가 존재함. 저자들은 filter가 잘못 걸림.
3. 8-oxo-G damage level의 interpretation
	1. 저자는 73%의 TCGA run이 damage(GIV>2)를 보인다고 함
	2. = GIV>2는 8-oxo-G가 non-8-oxo-G보다 2배 더 있다는 뜻(including background G>T Error)
		1. 그런데 sample들을 기존의 oxoQ filter로 나눠서 oxoG/non-oxoG mode로 봤을 때 GIV<5에서 dominance 없음
		2. 그리고 giv=2라는건 정작 5-10%정도의 전체 base error 증가율만을 포함함
		3. 이는 intersample variaty보다 작음
		4. 어쩌면 False-positive call이 늘었다면, 기존 알고리즘들이 일반적인 Q의 read를 써서 그럴지도 모름.
	3. GIV>5(oxoQ<35급) 일때만 error가 다른 경우와 비교 가능해짐
		1. (Mutect와 비교했을 때)
4. FPR(False Positive Rate) evalualtion()
5. TCGA prepartion들의 최근 데이터도 문제가 있다(GT imbalnce)고 했지만…
	1. 이미 costello 논문 이후, 2012에서 관련한 제안이 이미 된 이후에 제작됨.

### Respone
- 우리 지표는 이후에 ICGC(IUnternational Cancer Genome consortium)에서도 쓰임

1. Damage 있으면 Variant caller도 문제가 발생함.(Mutation-calling pipeline)
	1. GIV 기준으로 나눈 group(<2/>2) GC>TA mutation 비율이 증가
	2. VCF(calling된) 에서 전체 mutation과 PASS된거 비교했을 때 GIV 높을 때 GC>TA 높은 추세가 남음
2. 
```shell
OUTPUT
/Volumes/Promise_Pegasus/AGO_WORKSPACE/AGO_MCGL_DATA/Jung/o8dG/WGS/GIV/ver4/wg/w18/GIV_TE_shear.damage

DIRECTORIEE
/Volumes/Promise_Pegasus/AGO_WORKSPACE/AGO_MCGL_DATA/NGS_data/in-house/HiSeq_X_Ten_BerryGenomics_20190527_retry/bam_files/

Split
samtools view -b -f 64/128 -b $bam > $bam

MPILEUP
for bam in bam/*dedup.R1.bam; do out=$(echo ${bam} | cut -d'-' -f2); samtools mpileup -O -s -q 20 -Q 38 -d 1000000 -f ~/AGO_WORKSPACE/AGO_MCGL_DATA/JYP/Reference_genome/rn6.fa $bam > GIV/ver4/wg/${out}/file1.mpileup; done;

PERL (uses softmask)
for out in w0 w5 w18; do perl ../programs/Damage-estimator/estimate_damage.pl --mpileup1 GIV/ver4/wg/${out}/file1.mpileup --mpileup2 GIV/ver4/wg/${out}/file2.mpileup --id TE_shear --qualityscore 38 --max_coverage_limit 1000000 --soft_masked 0 > GIV/ver4/wg/${out}/GIV_TE_shear.damage; done;
```
