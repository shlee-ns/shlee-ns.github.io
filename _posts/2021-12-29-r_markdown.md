---
title:  "R 마크다운"
excerpt: 
toc: true
toc_sticky: true

categories:
  - R
tags:
  - Blog
---

### 1. 옵션을 문서 전체에 적용
``` r
{r setup, include=FALSE}
knitr::opts_chunk$set(echo = FALSE, message=FALSE, warning=FALSE, comment=NA)
```

### 2. 옵션
``` r
# 코드 숨기기
echo=FALSE

# 에러 출력 여부
error=TRUE

# 메시지
message=FALSE

# 경고 메시지
warning=TRUE

# 결과에 ## 없에기
comment=NA

# code만 남기고 실행 시키지 않기
eval=FALSE

# plot 사이즈 정하기
fig.width=7
fig.height=7

```

### 3. 단축키

chunk생성: command + opt + I  

knit하기: command + shift + k 

