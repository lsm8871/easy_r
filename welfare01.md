제목
================
이선민
July 30, 2020

# 한국인의 삶의 이해 ‘복지패널데이터’

## 1\. 한국복지패널데이터 분석 준비하기

### 데이터 분석 준비하기

#### 1\. 데이터 준비하기

#### 2\. 패키지 설치 및 로드하기

``` r
install.packages("foreign")  # foreign 패키지 설치
library(foreign)             # SPSS 파일 로드
library(dplyr)               # 전처리
library(ggplot2)             # 시각화
library(readxl)              # 엑셀 파일 불러오기
```

#### 3\. 데이터 불러오기

``` r
raw_welfare<-read.spss(file="Koweps_hpc10_2015_beta1.sav",
                       to.data.frame=T)
welfare<-raw_welfare
```

#### 4\. 데이터 검토하기

데이터의 구조와특징파악

``` r
head(welfare)
tail(welfare)
View(welfare)
dim(welfare)
str(welfare)
summary(welfare)
```

#### 5\. 변수명 바꾸기

``` r
welfare<-rename(welfare,
                sex=h10_g3,
                birth=h10_g4,
                marriage=h10_g10,
                religion=h10_g11,
                income=p1002_8aq1,
                code_job=h10_eco9,
                code_region=h10_reg7)
```

  - 1단계. 변수 검토 및 전처리
  - 2단계. 변수 간 관계 분석 ![](img/09_01.png)
