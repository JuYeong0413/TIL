# DataFrame의 column 이름 설정하기
## 전체 설정
```r
df <- read.csv('some_file.csv', header = FALSE)
names(df) <- c('new', 'column', 'names')
```
## 개별 설정
```r
df <- read.csv('some_file.csv', header = FALSE)
names(df)[1] <- "new header name"
```
