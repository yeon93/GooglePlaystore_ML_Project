# 구글 플레이스토어 파헤치기

## 프로젝트 목표
+ 사용자들이 새로운 애플리케이션을 다운로드할 때 크게 고려하는 요소 중의 하나가 바로 평점
+ 평점이 높을수록 구글 플레이스토어에서 노출될 확률도 올라감  
=>높은 평점을 받는 애플리케이션들은 어떤 특징이 있을지 알아보고, 애플리케이션의 평점을 예측해봄으로써  
애플리케이션 제작 시 어떤 요소들을 고려해야할지에 대한 인사이트 얻기




## 진행 과정
+ EDA를 통해 데이터를 살펴보고, 평점에 영향을 줄만 한 특성 선택
+ 머신러닝에 사용할 수 있도록 데이터 전처리
+ scikit-learn의 다양한 회귀모델들을 적용 및 비교
+ 하이퍼 파라미터 튜닝을 통해 모델 성능 향상
+ 모델의 성능 및 인사이트 도출/공유



## Data
+ [Kaggle의 Google-playstore 데이터](https://www.kaggle.com/datasets/gauthamp10/google-playstore-apps)
+ 원본 데이터셋 (2312944, 24)  
  => 특성 선택 :['Category', 'Rating', 'Rating Count', 'Installs', 'Price', 'Size', 'Last Updated', 'Content Rating']
+ target : 'Rating'(회귀문제로 접근)
+ 전처리 
  + drop_duplicates, dropna
  + 'Rating'       평점이 0인 데이터 drop
  + 'Category'     48class => 45class 
  + 'Size'         dtype object => float, 단위 통일
  + 'Installs'     22class => 7 class
  + 'Last Updated' dtype object(Feb 26, 2020) => int(786) 현재까지 며칠이 흘렀는지 계산



## Model
+ 회귀문제이므로, RMSE와 R2 점수 확인
+ Baseline model 
  + 훈련데이터셋의 'Rating' 데이터 사용(y_train)
  + y_train의 모든 데이터에 대해 y_train의 평균을 예측값으로 반환하는 모델을 baseline으로 선택  
    => base model RMSE : 0.6895426039410417
    
+ Cross Validation 점수 비교  
![image](https://user-images.githubusercontent.com/88722429/164978928-bc2f2e52-2f7a-413a-bc5a-a397d71d4ba4.png)
  + RandomForest, LGBM, XGB 세 모델의 성능이 유사하게 좋아보임
  + 학습에서 성능확인까지 걸린 시간이 각각 약 50분, 30초, 5분  
    => LGBM 모델을 선택
    
    
    
## LGBM 하이퍼파라미터 튜닝
+ Best params : {'normalize': True, 'n_estimators': 50, 'max_depth': 30, 'learning_rate': 0.1}
+ Best RMSE : 0.6313311130844016



## 최종 모델 성능 평가
+ best model test RMSE: 0.6215052573153973
+ best model test R2: 0.15971898762037473