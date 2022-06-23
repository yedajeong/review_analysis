# 토픽모델링과 감성분석을 이용한 화장품 리뷰 분석

## 1. 과제 개요
### 1.1 과제 선정 배경 및 필요성 
 소비자들의 구매 행위에 있어 가장 선행되는 것은 상품에 대한 정보 수집이다. 정보 수집의 대표적인 방법은 이전에 구매한 소비자들의 평가나 리뷰를 확인해 보는 것이다. 리뷰는 보통 종합적인 상품 만족도를 나타내는 별점이나 줄글 형식의 텍스트, 혹은 두 가지가 복합적으로 구성된 형태를 흔히 볼 수 있다. 하지만 별점 리뷰는 상품에 대한 종합적인 평가를 0~5점 사이의 지수로 나타낼 뿐 제품의 세부적인 사항에 대한 사용자의 평가를 알아내기 어렵기 때문에 보다 구체적인 내용은 리뷰의 텍스트 내용을 확인해야 한다. 하지만 텍스트 형태의 리뷰는 한 눈에 파악하기 어려우며 시간의 투자가 필요하다. 이런 리뷰의 불편함은 특히 ‘화장품’이라는 도메인에서 강하게 작용한다. 화장품은 의류나 숙박 등의 제품, 서비스와는 달리 직접 써보기 전까지는 눈으로 사진과 상품 설명을 보는 것만으로 나에게 맞는 제품인지 파악하기 어렵다. 이러한 이유로 화장품 리뷰에 대한 개선의 필요성을 느꼈다.
### 1.2 과제 주요 내용
[1. 전처리](1. 전처리(Preprocessing))

### 1.3 최종 결과물 목표

## 2. 과제 수행방법
### 1. 전처리(Preprocessing)  
- 데이터 크롤링 : 올리브영의 '크림' 제품군 리뷰 크롤링  
- 전처리 : 데이터 정제, 형태소 분석 및 토큰화, 합성명사 처리  
- 피부타입 재분류 : NoneType 데이터 중 일부를 리뷰 내용을 통해 건성, 지성, 복합성으로 재분류
  
### 2. 토픽모델링(LDA & Word2Vec & Spliting sentences)
- LDA : 리뷰에서 평가 요소(토픽) 도출  
- Word2Vec : LDA로 도출한 토픽의 키워드 추출  
- 문장 분리 : 키워드를 이용한 문장 분리 및 토픽 별 분류

### 3. 감성분석(Sentiment Analysis)
- 감성사전 구축 : KNU 감성사전에 LR모델의 coefficient값을 통해 도출한 도메인 특화된 긍부정어 추가
- 준지도 학습 : 200개의 직접 labeling한 문장을 초기 입력값으로 한 준지도 학습 모델(SVM) 구축
- 분류 모델 : SVM, Logistic Regression, SGD, NB 분류모델의 성능 비교

### 4. 시각화(Visualization)
![image01](https://user-images.githubusercontent.com/49268298/175094119-da348e6b-2526-4d2e-ac67-ddef2ed05e69.png)
- plotly를 이용한 시각화
- 특정 토픽에 대한 긍정적으로 평가한 비율과 리뷰 문장 중에서 특정 토픽에 대한 평가 비율 제공
- 사용자가 검색한 제품들을 동시에 확인 및 비교 가능
