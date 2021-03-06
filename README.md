# 토픽모델링과 감성분석을 이용한 화장품 리뷰 분석

## 1. 과제 개요
### 1.1 과제 선정 배경 및 필요성 
상품의 온라인 리뷰는 보통 종합적인 상품 만족도를 나타내는 별점이나 줄글 형식의 텍스트, 혹은 두 가지가 복합적으로 구성된 형태로 되어있다. 별점은 상품에 대한 종합적인 평가를 0~5점 사이의 지수로 나타낼 뿐 제품의 세부적인 사항에 대한 사용자의 평가를 알기에는 어렵고, 텍스트는 구체적인 내용을 알 수 있지만 한 눈에 파악하기 어렵다. 특히 화장품은 의류나 숙박 등의 제품, 서비스와는 달리 직접 써보기 전까지는 눈으로 사진과 상품 설명을 보는 것만으로 나에게 맞는 제품인지 파악하기 어렵고 잘못된 선택에 대한 리스크가 크다. 이러한 이유로 화장품 리뷰에 대한 개선의 필요성을 느껴 이번 과제를 진행했다.

### 1.2 과제 주요 내용
1.[데이터 수집 및 전처리](#21-데이터-수집-및-전처리)
- 데이터 크롤링
- 전처리
- 피부타입 재분류  

2.[토픽모델링 및 토픽별 키워드 추출](#22-토픽모델링-및-토픽별-키워드-추출) 
- LDA
- Word2Vec
- 문장 분리  

3.[감성분석](#23-감성분석)  
- 감성사전 구축
- 준지도 학습
- 분류 모델  

4.[시각화](#24-시각화)

## 2. 과제 수행방법
### 2.1. 데이터 수집 및 전처리  
##### 데이터 크롤링
re, selenium, beautifulsoup, request 모듈 및 라이브러리를 이용하여 '올리브영' 웹사이트의 텍스트 리뷰를 크롤링했다.  
이 때 제품 군은 '크림'으로 한정했으며 '인기순'으로 정렬 시 표시되는 351개의 제품에 대한 한국어 리뷰 총 90422개를 크롤링했다.
##### 전처리
re 모듈과 정규표현식을 사용해 raw data 내에 존재하는 이모티콘, 특수문자, 외국어, 숫자 등의 불용어를 제거했다.  
한국어 자연어 처리 패키지인 konlpy내의 형태소 분석기 중 하나인 komoran을 사용해 정제된 데이터에 대해 형태소 분석을 실시했다.  
명사의 n-gram 분포를 확인하여 빈번하게 등장하는 연속 단어는 분리하지 않고 하나의 명사로 취급하여 재토큰화 했다.

### 2.2. 토픽모델링 및 토픽별 키워드 추출
##### LDA 
tfidf 벡터값을 인풋으로 사용하여 리뷰 데이터 내에 존재하는 토픽(주제)들을 추출했다.  
이 중 분석가의 판단에 따라 유사한 의미를 내포하는 두 종류의 토픽은 하나로 합치는 작업을 거쳤다.   
##### Word2Vec 
리뷰 내에서 하나의 문장이 어떤 토픽으로 분류되는지 파악하기 위해 각 토픽을 대표하는 키워드들을 파악해야 했다.  
Word2Vec 모델의 most_similar() 함수를 이용해 토픽별 대표 키워드를 추출했다.  
##### 문장 분리
리뷰 단위의 데이터셋을 kss 문장 분리 패키지와 자체적으로 구축한 품사 패턴을 이용해 문장 단위로 분리하는 작업을 거쳤다.
LDA와 Word2Vec을 통해 도출한 각 토픽의 키워드를 이용해서 토픽별로 문장을 분류했다.

### 2.3. 감성분석
##### 감성사전 구축
KNU 감성사전에 LR모델의 coefficient값을 통해 도출한 도메인 특화된 긍부정어를 추가했다.
##### 준지도 학습
200개의 직접 labeling한 문장을 초기 입력값으로 한 준지도 학습 모델(SVM)을 구축했다.
##### 분류 모델
scikit-learn의 LogisticRegression, MultinomialNB, rbf kernel SVM, SCDClassifier 분류모델의 성능을 비교했다.  
독립변수X : 리뷰에 등장한 각 단어들의 tf-idf값  
종속변수y : 사용자가 리뷰에 부여한 1~5점의 별점 중 1, 2, 3점을 부정인 class 0으로, 5점을 긍정인 class 1로 매핑한 값
#### 최종 감성분석
문장단위 감성분석을 위해 다음 3가지 기법을 테스트 해보고 가장 성능이 좋은 분류모델-나이브베이지안 모델을 이용하여 최종 감성분석을 실시했다.

### 2.4. 시각화
![image01](https://user-images.githubusercontent.com/49268298/175094119-da348e6b-2526-4d2e-ac67-ddef2ed05e69.png)
- plotly를 사용하여 시각화했다.
- 특정 토픽에 대한 긍정적으로 평가한 비율과 리뷰 문장 중에서 특정 토픽에 대한 평가 비율을 제공한다.
- 사용자가 검색한 제품들을 동시에 확인하고 비교할 수 있다.
