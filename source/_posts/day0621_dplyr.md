---
title: "21JUN2022_ dplyr"
date: '2022-06-21'
---



p.98
- 분석 프로세스

- dplyr      데이터 전처리를 위한 도구
- data table 데이터 전처리를 위한 도구


다루는 데이터 10GB 이내 -> dplyr 사용 가능
다루는 데이터 50GB 이상 -> data.table
--> 처리속도 차이

- 배움의 측면
dplyr -> 매우 쉬움
data.tabel -> 어려움

- 라이브러리 불러오기
#install.packages("dplyr")


```r
library(dplyr)
```

```
## 
## 다음의 패키지를 부착합니다: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
mpgl <- read.csv("data/mpg1.csv", stringsAsFactors = F)
```


- 파이프 연산자 %>% -> ctrl + shift + M

```r
data2 <- mpgl %>%
  select(drv, cty, hwy) %>%
  filter(drv == "f") 
```

#Select : 컬럼명 추출
#Filter : 행 추 출(조건식)
#Group by :
#summarize() : 


-위와 같은 내용이지만 코드를 추가하기 번거롭다

```r
data3 <- select(mpgl, drv, cty, hwy)
data3 <- filter(data3, drv == "f")
#data3 <- v()
```

-변수이름 바꾸기

```r
mpgl_newname1 <- mpgl %>%
  rename(transmission = trans, drive_method = drv, city = cty, highway = hwy)

mpgl_newname2 <- rename(mpgl,transmission = trans, drive_method = drv, city = cty, highway = hwy)

str(mpgl_newname1) #str(mpgl_newname2)의 결과도 같음
```

```
## 'data.frame':	234 obs. of  5 variables:
##  $ manufacturer: chr  "audi" "audi" "audi" "audi" ...
##  $ transmission: chr  "auto" "manual" "manual" "auto" ...
##  $ drive_method: chr  "f" "f" "f" "f" ...
##  $ city        : int  18 21 20 21 16 18 18 18 16 20 ...
##  $ highway     : int  29 29 31 30 26 26 27 26 25 28 ...
```

-빈도분석

```r
count(mpgl, trans)
```

```
##    trans   n
## 1   auto 157
## 2 manual  77
```

```r
class(count(mpgl, trans)) #데이터 유형은 데이터프레임
```

```
## [1] "data.frame"
```

```r
table(mpgl$trans)
```

```
## 
##   auto manual 
##    157     77
```

```r
class(table(mpgl$trans)) #데이터 유형은 테이블
```

```
## [1] "table"
```

-데이터세트에서 일부 열을 추출하기
-필요한 열을 추출하기

```r
mnpgl_1 <- mpgl %>% select(manufacturer, trans, cty)

mpgl_2 <- select(mpgl, manufacturer, trans, cty)

str(mnpgl_1) #str(mnpgl_2)의 결과도 같음
```

```
## 'data.frame':	234 obs. of  3 variables:
##  $ manufacturer: chr  "audi" "audi" "audi" "audi" ...
##  $ trans       : chr  "auto" "manual" "manual" "auto" ...
##  $ cty         : int  18 21 20 21 16 18 18 18 16 20 ...
```

-불필요한 열 빼고 필요한 열만 남기기

```r
mpgl_type1 <- mpgl %>% select(-cty, -hwy)
mpgl_type2 <- mpgl %>% select(-c(cty, hwy))
```

-데이터세트에서 행 추출하기
-행의 일부 추출

```r
mpgl %>% slice(1, 4, 5)
```

```
##   manufacturer trans drv cty hwy
## 1         audi  auto   f  18  29
## 2         audi  auto   f  21  30
## 3         audi  auto   f  16  26
```

```r
slice(mpgl, 1, 4, 5)
```

```
##   manufacturer trans drv cty hwy
## 1         audi  auto   f  18  29
## 2         audi  auto   f  21  30
## 3         audi  auto   f  16  26
```

-조건에 맞는 행 추출하기
-비교값이 같은 데이터 추출

```r
mpgl_hd1 <- mpgl %>% filter(manufacturer == "hyundai")
mpgl_hd2 <- filter(mpgl, manufacturer == "hyundai")

str(mpgl_hd1)
```

```
## 'data.frame':	14 obs. of  5 variables:
##  $ manufacturer: chr  "hyundai" "hyundai" "hyundai" "hyundai" ...
##  $ trans       : chr  "auto" "manual" "auto" "manual" ...
##  $ drv         : chr  "f" "f" "f" "f" ...
##  $ cty         : int  18 18 21 21 18 18 19 19 19 20 ...
##  $ hwy         : int  26 27 30 31 26 26 28 26 29 28 ...
```

```r
max(mpgl$cty)
```

```
## [1] 35
```

```r
mpgl %>% filter(cty == 35)
```

```
##   manufacturer  trans drv cty hwy
## 1   volkswagen manual   f  35  44
```

```r
filter(mpgl, cty == 35)
```

```
##   manufacturer  trans drv cty hwy
## 1   volkswagen manual   f  35  44
```

```r
mpgl %>% filter(cty == max(cty))
```

```
##   manufacturer  trans drv cty hwy
## 1   volkswagen manual   f  35  44
```

-비교값이 다른 데이터 추출 108p

```r
mpg1_no_hd1 <- mpgl %>% filter(manufacturer != "hyundai")
mpgl_no_hd2 <- filter(mpgl, manufacturer != "hyundai")
```

-비교값이 크거나 작은 데이터 추출

```r
test <- mean(mpgl$cty)
mpgl_low1 <- mpgl %>% filter(cty < 16.85897)
mpgl_low2 <- filter(mpgl, cty < test)
mpgl_low3 <- mpgl %>%  filter(cty < mean(cty))

str(mpgl_low1)
```

```
## 'data.frame':	116 obs. of  5 variables:
##  $ manufacturer: chr  "audi" "audi" "audi" "audi" ...
##  $ trans       : chr  "auto" "auto" "auto" "manual" ...
##  $ drv         : chr  "f" "4" "4" "4" ...
##  $ cty         : int  16 16 15 15 15 16 14 11 14 13 ...
##  $ hwy         : int  26 25 25 25 24 23 20 15 20 17 ...
```

-비교값이 작거나 같은 데이터 추출

```r
quantile(mpgl$cty)
```

```
##   0%  25%  50%  75% 100% 
##    9   14   17   19   35
```

```r
mpgl_low4 <- mpgl %>% filter(cty <= 14)
mpgl_low5 <- filter(mpgl, cty <= 14)

str(mpgl_low4)
```

```
## 'data.frame':	73 obs. of  5 variables:
##  $ manufacturer: chr  "chevrolet" "chevrolet" "chevrolet" "chevrolet" ...
##  $ trans       : chr  "auto" "auto" "auto" "auto" ...
##  $ drv         : chr  "r" "r" "r" "r" ...
##  $ cty         : int  14 11 14 13 12 14 11 11 14 11 ...
##  $ hwy         : int  20 15 20 17 17 19 14 15 17 17 ...
```

-비교값이 큰 데이터 추출 110p

```r
median(mpgl$cty) #중앙값 구하기
```

```
## [1] 17
```

```r
mpgl_high1 <- mpgl %>% filter(cty > 17)
mpgl_high2 <- filter(mpgl, cty > 17)
mpgl_high3 <- mpgl %>% filter(cty > median(cty))
```

-비교값이 크거나 같은 데이터 추출

```r
quantile(mpgl$cty)
```

```
##   0%  25%  50%  75% 100% 
##    9   14   17   19   35
```

```r
mpgl_high4 <- mpgl %>% filter(cty >= 19)
mpgl_high5 <- filter(mpgl, cty >= 19)

str(mpgl_high4)
```

```
## 'data.frame':	76 obs. of  5 variables:
##  $ manufacturer: chr  "audi" "audi" "audi" "audi" ...
##  $ trans       : chr  "manual" "manual" "auto" "manual" ...
##  $ drv         : chr  "f" "f" "f" "4" ...
##  $ cty         : int  21 20 21 20 19 19 22 28 24 25 ...
##  $ hwy         : int  29 31 30 28 27 27 30 33 32 32 ...
```

-복수의 조건을 충족하는 데이터 추출 113p
-조건이 2개일 때

```r
mean(mpgl$cty)
```

```
## [1] 16.85897
```

```r
mpgl_hd3 <- mpgl %>% filter(manufacturer == "hyumdai" & cty >= 16.85897)
mpgl_hd4 <- filter(mpgl, manufacturer == "hyundai" & cty >= 16.85897)
mpgl_hd5 <- mpgl %>% filter(manufacturer == "hyundai" & cty >= mean(cty))
```

-조건이 3개일 때

```r
mpgl_hd6 <- mpgl %>%
  filter(manufacturer == "hyundai" & 
           trans == "auto" &
           cty >= 17)

str(mpgl_hd6)
```

```
## 'data.frame':	7 obs. of  5 variables:
##  $ manufacturer: chr  "hyundai" "hyundai" "hyundai" "hyundai" ...
##  $ trans       : chr  "auto" "auto" "auto" "auto" ...
##  $ drv         : chr  "f" "f" "f" "f" ...
##  $ cty         : int  18 21 18 19 19 20 17
##  $ hwy         : int  26 30 26 28 26 27 24
```

-복수 조건 중 하나라도 충족하는 데이터 추출

```r
mpgl_j1 <- mpgl %>% 
            filter(manufacturer == "honda"|
                     manufacturer == "nissan"|
                     manufacturer == "toyota")
str(mpgl_j1)
```

```
## 'data.frame':	56 obs. of  5 variables:
##  $ manufacturer: chr  "honda" "honda" "honda" "honda" ...
##  $ trans       : chr  "manual" "auto" "manual" "manual" ...
##  $ drv         : chr  "f" "f" "f" "f" ...
##  $ cty         : int  28 24 25 23 24 26 25 24 21 21 ...
##  $ hwy         : int  33 32 32 29 32 34 36 36 29 29 ...
```

-열을 추출한 후에 조건에 맞는 행 추출 116p

```r
mean(mpgl$cty)
```

```
## [1] 16.85897
```

```r
mpgl_hd9 <- mpgl %>% 
  select(manufacturer, cty) %>% 
  filter(manufacturer == "hyundai" & cty >= mean(mpgl$cty))
str(mpgl_hd9)
```

```
## 'data.frame':	13 obs. of  2 variables:
##  $ manufacturer: chr  "hyundai" "hyundai" "hyundai" "hyundai" ...
##  $ cty         : int  18 18 21 21 18 18 19 19 19 20 ...
```

-데이터 미리보기

```r
glimpse(iris)
```

```
## Rows: 150
## Columns: 5
## $ Sepal.Length <dbl> 5.1, 4.9, 4.7, 4.6, 5.0, 5.4, 4.6, 5.0, 4.4, 4.9, 5.4, 4.…
## $ Sepal.Width  <dbl> 3.5, 3.0, 3.2, 3.1, 3.6, 3.9, 3.4, 3.4, 2.9, 3.1, 3.7, 3.…
## $ Petal.Length <dbl> 1.4, 1.4, 1.3, 1.5, 1.4, 1.7, 1.4, 1.5, 1.4, 1.5, 1.5, 1.…
## $ Petal.Width  <dbl> 0.2, 0.2, 0.2, 0.2, 0.2, 0.4, 0.3, 0.2, 0.2, 0.1, 0.2, 0.…
## $ Species      <fct> setosa, setosa, setosa, setosa, setosa, setosa, setosa, s…
```

-파생변수 만들기 119p mutate()

```r
iris %>% 
  # Species, setosa versicolor
  filter(Species != "virginica") %>% 
  select(Sepal.Length, Sepal.Width) %>%
  filter(Sepal.Length > 5.0) %>% 
  mutate(total = Sepal.Length + Sepal.Width) -> data2

iris %>% 
  # Species, setosa versicolor
  filter(Species != "virginica"& Sepal.Length > 5.0) %>% 
  select(Sepal.Length, Sepal.Width) %>%
  mutate(total = Sepal.Length + Sepal.Width) -> data3
```

p.121 집단별 통계량
p.122

```r
mpgl %>%
  group_by(trans) %>% 
  summarise(avg     = mean(cty)    #평균
            , total = sum(cty)     #총합
            , med   = median(cty)
            , count = n()) #중간값
```

```
## # A tibble: 2 × 5
##   trans    avg total   med count
##   <chr>  <dbl> <int> <int> <int>
## 1 auto    16.0  2507    16   157
## 2 manual  18.7  1438    18    77
```

#test

```r
mpgl %>%
  summarise(m = mean(cty)) -> test1

mean(mpgl$cty)
```

```
## [1] 16.85897
```

```r
mpgl %>% 
  summarise(mean(mpgl$cty)) -> test2

str(test1)
```

```
## 'data.frame':	1 obs. of  1 variable:
##  $ m: num 16.9
```

```r
str(test2)
```

```
## 'data.frame':	1 obs. of  1 variable:
##  $ mean(mpgl$cty): num 16.9
```

-집단별 빈도와 비율 구하기

```r
mpgl %>% 
  group_by(trans, drv) %>% #2개 변수로 집단 분류
  summarise(n = n(),       #집단별 빈도 구하기
            m = mean(cty)) #집단별 평균 구하기
```

```
## `summarise()` has grouped output by 'trans'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 6 × 4
## # Groups:   trans [2]
##   trans  drv       n     m
##   <chr>  <chr> <int> <dbl>
## 1 auto   4        75  13.9
## 2 auto   f        65  19.1
## 3 auto   r        17  13.3
## 4 manual 4        28  15.6
## 5 manual f        41  21.3
## 6 manual r         8  15.8
```

```r
?summarise
```

```
## httpd 도움말 서버를 시작합니다 ... 완료
```

```r
?count
?n
```

-분류한 집단별 빈도와 비율 구하기 128p
-group_by()+summarise(n())+mutate()

```r
mpgl %>%
  group_by(trans) %>%
  summarise(n = n()) %>% 
    mutate(total = sum(n),
           pct = n/total*100)
```

```
## # A tibble: 2 × 4
##   trans      n total   pct
##   <chr>  <int> <int> <dbl>
## 1 auto     157   234  67.1
## 2 manual    77   234  32.9
```
  
-mutate() + ifelse()

```r
a <- c(1, 3, 4, 6, 9)
ifelse(a %% 2==0, "짝수", "홀수")
```

```
## [1] "홀수" "홀수" "짝수" "짝수" "홀수"
```

-복수의 조건으로 새 변수 만들기

```r
quantile(mpgl$hwy)
```

```
##   0%  25%  50%  75% 100% 
##   12   18   24   27   44
```

```r
mpgl <- mpgl %>% 
  mutate(hwy_class = ifelse(hwy >= 27, "best",
                            ifelse(hwy >= 24, "good",
                                   ifelse(hwy >= 18, "normal", "bad"))))
table(mpgl$hwy_class)
```

```
## 
##    bad   best   good normal 
##     55     69     60     50
```
