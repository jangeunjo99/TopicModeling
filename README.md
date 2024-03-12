# 토픽모델링을 통한 Nature-Based Solutions(NbS) 연구 동향 분석

<br/>
<br/>
<br/>

## 목차
0. 프로젝트 성과
1. 담당 업무 및 나의 기여도
2. 프로젝트 개요  
   2.1 프로젝트 개요  
   2.2 프로젝트 소개 
3. 실험 준비 
   3.1 데이터 정의  
   3.2 데이터 전처리  
4. 토픽 모델링
5. 토픽 모델링 사후 분석
   5.1 토픽별 Hot/Cold 분석
   5.2 네트워크 분석
     5.2.1 전처리
     5.2.2 네트워크 구성
     5.2.3 네트워크 군집화 
   
   
<br/>
<br/>
<br/>

# 0. 프로젝트 성과 
> Atmosphere 2022년 기준 IF 2.9
1. <code style="color : red">**[한국기후변화학회]** 자연기반해법 연구동향 분석을 통한 제언 (**최우수포스터논문상**)</code> 
2. <code style="color : red">**[Atmosphere]** Analysis of Nature-Based Solutions Research Trends and Integrated Means of Implementation in Climate Change</code> 

<br/>
<br/>
<br/>

# 1. 담당 업무 및 나의 기여도 
0. 나의 기여도 : **100%**
1. 분석 요청을 받은 토픽 모델링 방법 외에 추가적인 분석 방법들을 탐색
   - 연구 동향 관련된 논문들을 조사하고, 여러 분석 기법들을 검토
   - 참고 논문 참고
2. 토픽 모델링의 결과를 사후 분석하는 새로운 방법론 제안
   - Hot/Cold 분석 도입하여, 연구 동향이 시간에 따라 어떻게 변화하는지를 통계적으로 검증
   - 네트워크 분석과 중심성 분석을 통해 각 토픽 간의 연관성을 추가적으로 조사하고 결과 도출

<br/>
<br/>
<br/>

# 2. 프로젝트 개요
## 2.1 프로젝트 개요

|||
|:---:|--|
|**과제명**|토픽모델링을 통한 Nature-Based Solutions(NbS) 연구 동향 분석|
|**지원 기관**|산림청|
|**프로젝트 기간**| 2022.06 - 2022.12 |
|**연구 목표**| - 자연 기반 해결책(Nature-Based Solutions, NbS)에 대한 연구 동향 분석 <br/> - 다양한 분야에서의 NbS 관련 활동과의 상호작용 조사|

<br/>
<br/>
<br/>

## 2.2 프로젝트 소개
- Nature based Solutions (NbS) 연구가 시간에 따라 어떻게 변화하고 있는지, 그리고 다양한 분야에서 NbS가 어떻게 적용되고 있는지를 체계적으로 분석
- 이를 위해, 관련 문헌 검토와 토픽 모델링, 네트워크 분석을 통해 NbS 연구의 주요 주제와 그 연관성 탐색
- 본 연구는 특히 수자원, 산림, 도시와 같은 주요 분야에서의 NbS 연구 동향에 초점을 맞추며, 산림 분야에서 NbS가 기후 변화 대응 및 관련 사회 문제 해결에 기여할 수 있는 통합적 방안 제시
  

<br/>
<br/>
<br/>

# 3. 실험 준비  


## 3.1 데이터 정의    
- 2016년~2021년 논문 중 NbS 관련 키워드 들어간 논문 데이터 수집 (산림청에서 제공) 
- Abstract + Index Keywords 대상 분석 진행
- 논문 데이터 출처 : Scopus

<br/>
<br/>
<br/>

## 3.2 데이터 전처리

 
|순서|과정|비고|
|:---:|:---:|:---:|
|1|한글, 숫자, 특수문자 등 제거||
|2|대문자 ▶ 소문자||
|3|토큰화 및 품사 태깅|(terrestrial, JJ), (wine, NN), (face, VBP),(a, DT)|
|4|품사 형태 변경|(terrestrial, a), (wine,n), (face, v) <br/> * 표제어 추출 input 형식에 맞는 품사 형태로 변경|
|5|표제어(Lemmaization)|(producers, n) → (producer, n) ,  (growing, v) → (grow, v)|
|6|명사 추출|품사 ‘n’인 단어들만 추출 |
|7|한 글자 제거||
|8|불용어 제거 -nltk 제공|I my me mine all should a ... |
|9|불용어 제거 - 산림청 요청|'nature', 'based', 'solutions', 'nbs', 'studing', 'studies', 'manage', 'model', 'use' 등|
|10|불용어 제거 - 분석가 판단|'approach','author','analysis','benefit' 등|
|11|TF-IDF 임계값 이하 단어 제거| 전체 논문의 95%에서 등장하는 단어 제거|  

- **[문제 발견]** 10번까지의 전처리 후 토픽 모델링 결과, 논문 자체의 특성으로 많이 등장하는 단어들(예. show, across, may 등)이 각 토픽의 상위 키워드가 되는 문제점 발견
- **[해결 방안]** TF-IDF 개념을 이용하여 전체 논문의 95% 이상 등장하는 단어는 대표 키워드로써 의미가 없다고 판단되어 삭제  


<br/>
<br/>
<br/>

# 4. 토픽 모델링 결과  
- 최적의 토픽 수를 결정하기 위해, 각 토픽 수 (3~20) 에 대한 Coherence 측정 (10회 평균)
- Coherence가 가장 높은 19개로 토픽 개수 설정
- 각 토픽별 대표 키워드 20개 선정 후, 산림청에 전달
- 대표 키워드를 통해 각 토픽의 주제 선정
  
<br/>

|Topic|주제(영문)|주제(한글)|
|:---:|:---:|:---:|
|1|Coastal Restoration|해양, 해안 복원|
|2|Forest Carbon Sequestration|산림 탄소 격리|
|3|Urban Ecosystem|도시 생태계|
|4|Fresh Water Ecosystem|깨끗한 수질|
|5|Urban  Microclimate management|도시 미기후 관리|
|6|Wastewater Management|폐수 관리|
|7|Ecosystem-based Adaptation (EbA)|생태계 기반 적용|
|8|Biodiversity Conservation|생물다양성 보전|
|9|Sustainable  Agriculture|지속가능한 농업|
|10|Water Management|물관리|
|11|Soil Carbon Sequestration|토양 탄소 격리|
|12|Water Disaster Management|물 관련 재난 관리|
|13|Green  Therapy|그린 테라피|
|14|Forest Landscape Restoration (FLR)|산림경관복원|
|15|Water Pollution Remediation|수질오염 처리|
|16|Natural Pollination|자연 수분|
|17|Land Degradation Neutrality (LDN)|토지 황폐화 중립|
|18|Urban Greening|도시 녹지조성|
|19|Water System|수자원 시스템|

정리하면, 
|Topics|주제|
|:---:|:---:|
|4,6,10,12,15,19|수자원 및 물관리|
|2,11,14,17|산림|
|3,5,18|도시|

<br/>

# 5. 토픽 모델링 사후 분석
<br/>

## 5.1 토픽별 Hot/Cold 분석  
- 토픽 모델링 결과로 얻은 19개 토픽 각각에 대해 X: 연도, Y: 연도별 토픽 점유율(%)에 대해 시계열 회귀 분석(tslm) 실시
- tslm 결과, 5% 유의수준 하에서 결과가 유의하게 나타났을 때, 그때의 회귀계수 부호가 (+)이면 상향 주제(Hot Topic), (-)이면 하향 주제(Cold Topic)로 해석
- 또한, 잔차 분석(Durbin_Watson)을 통해 회귀 모형의 적합성을 검토
  
<br/>
<br/>


## 5.2 네트워크 분석 

### 5.2.1 전처리  
- 각 토픽의 세부주제를 나타내기 위해 각각의 토픽을 대표하는 10개의 단어 추출
- 각 토픽별 키워드 10개 대상으로 TF-IDF 구함
  - 한 토픽에만 출현한 단어의 TF-IDF의 값은 크다.
  - 모든 토픽에 공통적으로 등장할수록 단어의 TF-IDF의 값은 작다.
  - TF-IDF를 통해 각 토픽 통틀어 해당 토픽을 대표할 수 있는 키워드들을 TF-DIF 수치화 할 수 있다.
  - 즉, TF-IDF 기반 토픽X단어 행렬을 생성
- 이를 기반으로 각 토픽별 코사인 유사도를 계산하여 토픽 간의 유사도를 산출한다.
  - i토픽과 j토픽의 유사도가 높다는 것은, 대표 키워드가 유사하다는 것을 의미

<br/>
<br/>
<br/>

### 5.2.2 네트워크 구성  
a. Node : 각 토픽 (node1~node19) 

b. Edge : 각 토픽 간 코사인 유사도
  - 유사도 값이 높을수록 가까이 연결
  - 토픽 간 유사도가 0.1 이상 & 1.0 미만인 edge만 추출

c. b에서 구성된 edge를 통해 각 토픽별로 연결 중심성 계산   
  - 연결된 노드가 많을수록 연결 중심성 값은 커짐.
    
d. Node_size : 연결 중심성이 큰 노드일수록 노드 사이즈 크다.  

<br/>
<br/>

### 5.2.3 네트워크 군집화  
- Network Community Detection : 네트워크에서 다른 집단 보다 좀 더 밀집하게 묶여있는, 다시 말해 모듈성(Modularity)이 높은 집단을 Community라고 부른다. Community detection 알고리즘은 전체 집단에 어떤 커뮤니티가 있고, 또 이러한 커뮤니티는 어떤 특성을 가지고 있는지를 분석하는 방법
<img src="/Image/nbs_result.png" title="네트워크 군집화 결과"></img><br/>  

