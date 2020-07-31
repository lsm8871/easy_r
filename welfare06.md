어떤 직업이 월급을 가장 많이 받을까
================
이선민
July 31, 2020

## 6\. 직업별 월급 차이

### 분석 절차

직업, 월급 변수 검토 및 전처리

변수간 관계 분석

### 직업 변수 검토 및 전처리하기

#### 1\. 변수 검토하기

``` r
class(welfare$code_job)
table(welfare$code_job)
```

code\_job변수는 직업코드를 의미.

#### 2\. 전처리

직업분류코드 목록을 이용해 직업의 명칭으로 된 변수를 만든다.

먼저 직업분류코드 목록 데이터 프레임을 만든다.

``` r
library(readxl)
list_job<- read_excel("Koweps_Codebook.xlsx", col_names=T, sheet=2)

head(list_job)

dim(list_job)

welfare<- left_join(welfare, list_job, id="code_job")
```

    ## Joining, by = "code_job"

``` r
welfare %>% 
  filter(!is.na(code_job)) %>% 
           select(code_job, job) %>% 
           head(10)
```

### 직업별 월급 차이 분석하기

#### 1\. 직업별 월급 평균표 만들기

``` r
job_income<- welfare %>% 
  filter(!is.na(job)& !is.na(income)) %>% 
  group_by(job) %>% 
  summarise(mean_income=mean(income))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
head(job_income)
```

#### 2\. 상위 10개 추출

``` r
top10<- job_income %>% 
  arrange(desc(mean_income)) %>% 
  head(10)

top10
```

#### 3\. 그래프 만들기

``` r
ggplot(data = top10, aes(x=reorder(job, mean_income),y= mean_income))+
  geom_col()+
  coord_flip()
```

![](welfare06_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

출력괸 표와 그래프를 보면 ‘금속 재료 공학 기술자 및 시험원’이 평균 845만 원으로 가장 많은 월급을 받고 다음으로
’의료진료전문가’, ‘의회의원 고위공무원 및 공공단체임원’,’보험 및 금융 관리자’의 월급이 많다.

#### 4\. 하위 10위 추출

``` r
bottom10<- job_income %>% 
  arrange(mean_income) %>% 
  head(10)

bottom10
```

#### 5\. 그래프 만들기

``` r
ggplot(data=bottom10, aes(x=reorder(job,-mean_income), 
                          y=mean_income))+ geom_col()+ coord_flip()+ ylim(0,850)
```

![](welfare06_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

출력된 표와 그래프를 보면 ‘가사 및 육아 도우미’의 월급이 평균 80만 원으로 가장 적고 그 뒤로 ’임업 관련 종사자’ ,‘기타
서비스 관련 단순 종사원’, ’청소원 및 환경 미화원’의 월급이 적다.
