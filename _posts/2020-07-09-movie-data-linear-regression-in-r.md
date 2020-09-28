---
title: R 영화 관객수 예측 회귀분석
excerpt: R로 영화 관객수를 예측해보자!! 회귀분석 사용
categories:
  - R
tags:
  - R
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2020-07-09T13:00-18:00
permalink: movie_data_linear_regression_in_R.html
---
# 영화 관객수 예측하기 with 선형회귀분석 in R

> 이번 포스트는 "데이콘(DACON)" 영화 데이터를 가지고 선형회귀분석을 통해 영화 관객수를 예측해보는 것이다. 
>
> 환경
>
> *  운영체제: windows10
> * 사용언어: R



[TOC]

## 1. 개요

"[데이콘(DACON) 영화 관객수 예측 모델 개발](https://www.dacon.io/competitions/open/235536/overview/)"은 데이터 사이언스와 머신러닝을 처음 접하거나 연습용으로 해보기 좋은 프로젝트이다. 그래서 데이콘에서 제공하는 [영화 데이터](https://www.dacon.io/competitions/open/235536/data/)를 사용한다.

영화 데이터는 다음과 같다.

> * movies_train.csv: 2010년대 한국에서 개봉한 한국영화 600개에 대한 감독, 이름, 상영등급, 관객수 등의 정보가 담긴 데이터
> * movies_test.csv: 관객수를 제외하고 movies_train과 동일
> * submission.csv: 제출 파일 형식을 제공한다.
>
> | column name    | description                     | type      | example           |
> | -------------- | ------------------------------- | --------- | ----------------- |
> | title          | 영화 제목                       | Character | "국제시장"        |
> | distributor    | 배급사                          | Character | "CJ 엔터테인먼트" |
> | genre          | 장르                            | Character | "드라마"          |
> | release_time   | 개봉일                          | Date      | "2014-12-17"      |
> | time           | 상영시간(분)                    | Integer   | 126               |
> | screening_rat  | 상영등급                        | Character | "12세 관람가"     |
> | director       | 감독이름                        | Character | "윤제균"          |
> | dir_prev_bfnum | 해당 감독 전작 영화 평균 관객수 | Numeric   | **NA**            |
> | dir_prev_num   | 해당 감독 전작 영화 수          | Integer   | 0                 |
> | num_staff      | 스탭 수                         | Integer   | 869               |
> | num_actor      | 주연배우 수                     | Integer   | 4                 |
> | box_off_num    | 관객 수                         | Integer   | 14262766          |

## 2. 회귀 분석

> *“회귀 분석은 관찰된 연속형 변수들에 대해 두 변수 사이의 모형을 구한 뒤 적합도를 측정해 내는 분석 방법이다.” – 위키백과* 
> $$
> y = w_0 + w_1x_1 + w_2x_2 + \cdots + w_nx_n
> $$

여기서 y는 종속변수로서 관객수를 의미한다. 그리고 나머지 변수를 독립변수로 하고 검증을 통해 불필요한 변수를 제거한다. 

```R
# movies_train.csv, movies_test.csv를 읽어온다.
data_path <- "c:/r/data"
movies_train <- read.csv(file.path(data_path, "movies_train.csv"), header=TRUE)
movies_test <- read.csv(file.path(data_path, "movies_test.csv"), header=TRUE
```

### 1) 데이터 정제

회귀분석을 하기 위해 다음과 같이 데이터를 정제한다.

1. 영화 데이터 중 **dir_prev_bfnum**의 **NA(결측값)**를 숫자 0으로 변환한다.

2. 장르, 영화 등급을 **one_hot** 인코딩을 해준다.

3. 날짜 데이터를 년(year)과 월(month)로 수정한다.

4. 배급사의 경우, 각 영화 배급사마다 3사분위 이상의 값을 1로 주고 미만이면 0을 준다. 

   ~~내가 쓰고도 무슨 말인지 모르겠다.~~

5. 중복 및 불필요한 컬럼을 삭제한다.

```R
# 패키지 설치
install.packages("mltools")
install.packages("data.table")
install.packages("lubridate")
install.packages("dplyr")
library(mltools) # one_hot 함수
library(data.table) # as.data.table 함수
library(lubridate) # year, month 함수
library(dplyr) # select, %>% 등 사용

# dir_prev_bfnum의 NA 변환
movies_train$dir_prev_bfnum[is.na(movies_train$dir_prev_bfnum)] <- 0
movies_test$dir_prev_bfnum[is.na(movies_test$dir_prev_bfnum)] <- 0

# one-hot encoding
genre_onehot <- one_hot(as.data.table(movies_train$genre)) # 장르 ONEHOT
screening_onehot <- one_hot(as.data.table(movies_train$screening_rat)) # 상영 등급 ONEHOT

# 날짜 데이터 year, month변경
movies_train$year <- year(movies_train$release_time)
movies_train$month <- month(movies_train$release_time)

movies_train <- movies_train %>% select(-release_time, -genre, -distributor, -screening_rat, -title, -director)

#배급사는 3사분위 이상 값 1로 만들고 나머지 0
distributor_value <- movies_train %>%
	group_by(distributor) %>%
	summarize(median=median(box_off_num))

# 정렬
distributor_value_sort <- distributor_value %>% arrange(desc(median))

# 관객수 중앙값이 전체 관객수의 3분위수를 넘으면 1, 아니면 0이다.
distributor_value <- ifelse(distributor_value_sort$median >= 1295249, 1, 0); distributor_value

# 합치기
movies_train_cbind <- cbind(movies_train, genre_onehot, screeing_onehot)

# V로 시작하는 데이터 열을 factor로 변환한다.
train[, substr(colnames(train), start = 1, stop = 1)] <- lapply(train[,substr(colnames(train), start = 1, stop = 1) == 'V'], factor)

movies_train_cbind$year <- factor(movies_train_cbind$year)
movies_train_cbind$month <- factor(movies_train_cbind$month)
```

### 2) 회귀 분석

```R
# 회귀 분석
model <- lm(box_off_num ~ ., data = train)
summary(model)

# Call:
#lm(formula = box_off_num ~ ., data = movies_train_cbind)
#
#Residuals:
#     Min       1Q   Median       3Q      Max 
#-2750691  -664660   -97016   344548  9800393 
#
#Coefficients: (2 not defined because of singularities)
#                        Estimate Std. Error t value Pr(>|t|)    
#(Intercept)           -1.951e+06  5.435e+05  -3.590 0.000360 ***
#time                   1.476e+04  4.536e+03   3.254 0.001207 ** 
#dir_prev_bfnum         1.294e-01  5.216e-02   2.481 0.013398 *  
#dir_prev_num          -6.902e+04  6.144e+04  -1.123 0.261766    
#num_staff              4.775e+03  5.551e+02   8.602  < 2e-16 ***
#num_actor              3.924e+04  2.579e+04   1.521 0.128771    
#year2011               4.340e+05  2.351e+05   1.846 0.065384 .  
#year2012               8.096e+05  2.313e+05   3.500 0.000501 ***
#year2013               1.033e+06  2.245e+05   4.601 5.19e-06 ***
#year2014               4.763e+05  2.085e+05   2.284 0.022713 *  
#year2015               3.072e+05  2.125e+05   1.446 0.148785    
#month2                -6.858e+05  3.574e+05  -1.919 0.055524 .  
#month3                -7.680e+05  3.198e+05  -2.402 0.016647 *  
#month4                -6.071e+05  3.221e+05  -1.885 0.059974 .  
#month5                -4.837e+05  3.062e+05  -1.580 0.114765    
#month6                -3.720e+05  3.630e+05  -1.025 0.305921    
#month7                 1.539e+05  3.216e+05   0.478 0.632500    
#month8                -3.339e+05  3.184e+05  -1.049 0.294713    
#month9                -1.004e+05  3.059e+05  -0.328 0.742785    
#month10               -6.137e+05  3.049e+05  -2.013 0.044620 *  
#month11               -6.080e+05  2.943e+05  -2.066 0.039253 *  
#month12               -1.975e+04  3.161e+05  -0.062 0.950213    
#V1_SF1                 7.828e+05  4.690e+05   1.669 0.095666 .  
#V1_공포1              -1.975e+05  3.134e+05  -0.630 0.528904    
#V1_느와르1             7.092e+05  3.645e+05   1.946 0.052157 .  
#V1_다큐멘터리1         7.340e+04  2.947e+05   0.249 0.803400    
#V1_드라마1            -2.188e+05  2.308e+05  -0.948 0.343619    
#`V1_멜로/로맨스`1      6.665e+03  2.722e+05   0.024 0.980477    
#V1_뮤지컬1            -1.342e+05  7.233e+05  -0.186 0.852861    
#V1_미스터리1          -1.473e+05  4.214e+05  -0.350 0.726771    
#V1_서스펜스1          -6.336e+05  1.071e+06  -0.592 0.554222    
#V1_애니메이션1         6.960e+04  4.181e+05   0.166 0.867852    
#V1_액션1               2.482e+05  3.608e+05   0.688 0.491758    
#V1_코미디1                    NA         NA      NA       NA    
#`V1_12세 관람가`1      2.229e+05  1.991e+05   1.120 0.263316    
#`V1_15세 관람가`1      3.401e+05  1.611e+05   2.111 0.035179 *  
#`V1_전체 관람가`1      3.405e+05  2.452e+05   1.388 0.165602    
#`V1_청소년 관람불가`1         NA         NA      NA       NA    
#---
#Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
#
#Residual standard error: 1456000 on 564 degrees of freedom
#Multiple R-squared:  0.4023,	Adjusted R-squared:  0.3652 
#F-statistic: 10.85 on 35 and 564 DF,  p-value: < 2.2e-16
```

## 3. 결과

생성된 회귀 모델로 그림을 그려봤다.

![회귀분석 모델 그림](/assets/images/회귀분석모델그림.png)

> 위 각 그래프는 선형성, 정규성, 등분산성, 극단값을 확인하는 그래프이다.
> Residuals vs Fitted : 선형성 만족하지 않음
> Normal Q-Q : 정규성 만족하지 않음
> Scale-Location: 등분산성 만족하지 않음
> Residuals vs Leverage : 극단값을 가진다.(위에 부분)

한 마디로 좋은 회귀가 아니라는 것이다. 이를 해결하기 위해 선형 회귀가 아닌 비선형 회귀를 해볼 수 있다. 또한 데이터의 량이 많지 않다 고작 600개다. [KOBIS(영화관입장권통합전산망)](http://www.kobis.or.kr/kobis/business/main/main.do)에서 다양한 데이터를 볼 수 있다. 이 데이터로 다시 선형 회귀분석을 해보는 것도 시험해보아야 한다.



감사합니다.

