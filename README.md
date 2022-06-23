# 토픽모델링과 감성분석을 이용한 화장품 리뷰 분석
본 프로젝트에서는 화장품의 리뷰 텍스트를 이용해 더 가시적이고 직관적인 형태의 리뷰 시스템을 개발하고자 했다.

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
- 준지도 학습 : 200개의 직접 labeling한 문장을 초기 입력값으로 한 준지도 학습 모델 구축
- 분류 모델 : SVM, Logistic Regression, SGD, NB 분류모델의 성능 비교

### 4. 시각화(Visualization)
![image01](https://user-images.githubusercontent.com/49268298/175094119-da348e6b-2526-4d2e-ac67-ddef2ed05e69.png)
- plotly를 이용한 시각화
- 특정 토픽에 대한 긍정적으로 평가한 비율과 리뷰 문장 중에서 특정 토픽에 대한 평가 비율 제공
- 사용자가 검색한 제품들을 동시에 확인 및 비교 가능
