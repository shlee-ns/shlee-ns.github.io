---
title:  "metafor 패키지를 이용한 메타분석"
excerpt: R의 metafor 패키지를 이용해 메타 분석을 실습했다.
toc: true
toc_sticky: true

categories:
  - R
---

```R
library(metafor)
library(tidyverse)
```

## 1. 연속형 변수

### 1. 데이터 로딩
교사들의 관심이 학생들의 IQ 수준에 영향을 미치는 정도를 연구한 19개의 연구이다.   
이 연구들에 대한 메타 분석을 할 것이다.


```R
df1 <- dat.raudenbush1985
```


```R
# yi: 효과 크기
# vi: 효과 크기에 대한 추정치의 분산 (표준 오차 아님)
# sqrt(vi) = standard error

df1 <- as_tibble(df1)
df1
```


<table>
<thead><tr><th scope=col>study</th><th scope=col>author</th><th scope=col>year</th><th scope=col>weeks</th><th scope=col>setting</th><th scope=col>tester</th><th scope=col>n1i</th><th scope=col>n2i</th><th scope=col>yi</th><th scope=col>vi</th></tr></thead>
<tbody>
	<tr><td> 1                  </td><td>Rosenthal et al.    </td><td>1974                </td><td> 2                  </td><td>group               </td><td>aware               </td><td> 77                 </td><td>339                 </td><td> 0.03               </td><td>0.0156              </td></tr>
	<tr><td> 2                  </td><td>Conn et al.         </td><td>1968                </td><td>21                  </td><td>group               </td><td>aware               </td><td> 60                 </td><td>198                 </td><td> 0.12               </td><td>0.0216              </td></tr>
	<tr><td> 3                                                              </td><td>Jose &amp; Cody       </td><td>1971                                                            </td><td>19                                                              </td><td>group                                                           </td><td>aware                                                           </td><td> 72                                                             </td><td> 72                                                             </td><td>-0.14                                                           </td><td>0.0279                                                          </td></tr>
	<tr><td> 4                                                              </td><td>Pellegrini &amp; Hicks </td><td>1972                                                            </td><td> 0                                                              </td><td>group                                                           </td><td>aware                                                           </td><td> 11                                                             </td><td> 22                                                             </td><td> 1.18                                                           </td><td>0.1391                                                          </td></tr>
	<tr><td> 5                                                              </td><td>Pellegrini &amp; Hicks </td><td>1972                                                            </td><td> 0                                                              </td><td>group                                                           </td><td>blind                                                           </td><td> 11                                                             </td><td> 22                                                             </td><td> 0.26                                                           </td><td>0.1362                                                          </td></tr>
	<tr><td> 6                                                              </td><td>Evans &amp; Rosenthal </td><td>1969                                                            </td><td> 3                                                              </td><td>group                                                           </td><td>aware                                                           </td><td>129                                                             </td><td>348                                                             </td><td>-0.06                                                           </td><td>0.0106                                                          </td></tr>
	<tr><td> 7                  </td><td>Fielder et al.      </td><td>1971                </td><td>17                  </td><td>group               </td><td>blind               </td><td>110                 </td><td>636                 </td><td>-0.02               </td><td>0.0106              </td></tr>
	<tr><td> 8                  </td><td>Claiborn            </td><td>1969                </td><td>24                  </td><td>group               </td><td>aware               </td><td> 26                 </td><td> 99                 </td><td>-0.32               </td><td>0.0484              </td></tr>
	<tr><td> 9                  </td><td>Kester              </td><td>1969                </td><td> 0                  </td><td>group               </td><td>aware               </td><td> 75                 </td><td> 74                 </td><td> 0.27               </td><td>0.0269              </td></tr>
	<tr><td>10                  </td><td>Maxwell             </td><td>1970                </td><td> 1                  </td><td>indiv               </td><td>blind               </td><td> 32                 </td><td> 32                 </td><td> 0.80               </td><td>0.0630              </td></tr>
	<tr><td>11                  </td><td>Carter              </td><td>1970                </td><td> 0                  </td><td>group               </td><td>blind               </td><td> 22                 </td><td> 22                 </td><td> 0.54               </td><td>0.0912              </td></tr>
	<tr><td>12                  </td><td>Flowers             </td><td>1966                </td><td> 0                  </td><td>group               </td><td>blind               </td><td> 43                 </td><td> 38                 </td><td> 0.18               </td><td>0.0497              </td></tr>
	<tr><td>13                  </td><td>Keshock             </td><td>1970                </td><td> 1                  </td><td>indiv               </td><td>blind               </td><td> 24                 </td><td> 24                 </td><td>-0.02               </td><td>0.0835              </td></tr>
	<tr><td>14                  </td><td>Henrikson           </td><td>1970                </td><td> 2                  </td><td>indiv               </td><td>blind               </td><td> 19                 </td><td> 32                 </td><td> 0.23               </td><td>0.0841              </td></tr>
	<tr><td>15                  </td><td>Fine                </td><td>1972                </td><td>17                  </td><td>group               </td><td>aware               </td><td> 80                 </td><td> 79                 </td><td>-0.18               </td><td>0.0253              </td></tr>
	<tr><td>16                  </td><td>Grieger             </td><td>1970                </td><td> 5                  </td><td>group               </td><td>blind               </td><td> 72                 </td><td> 72                 </td><td>-0.06               </td><td>0.0279              </td></tr>
	<tr><td>17                      </td><td>Rosenthal &amp; Jacobson</td><td>1968                    </td><td> 1                      </td><td>group                   </td><td>aware                   </td><td> 65                     </td><td>255                     </td><td> 0.30                   </td><td>0.0193                  </td></tr>
	<tr><td>18                                                              </td><td><span style=white-space:pre-wrap>Fleming &amp; Anttonen  </span></td><td>1971                                                            </td><td> 2                                                              </td><td>group                                                           </td><td>blind                                                           </td><td>233                                                             </td><td>224                                                             </td><td> 0.07                                                           </td><td>0.0088                                                          </td></tr>
	<tr><td>19                  </td><td>Ginsburg            </td><td>1970                </td><td> 7                  </td><td>group               </td><td>aware               </td><td> 65                 </td><td> 67                 </td><td>-0.07               </td><td>0.0303              </td></tr>
</tbody>
</table>



### 2. 고정 효과 모형


```R
# 고정 효과 모형은 method = "FE"로 지정

res.FE <- rma(yi, vi, data = df1, digits = 3, method = "FE", slab = paste(author, year, sep = ", "))
res.FE
```


    
    Fixed-Effects Model (k = 19)
    
    Test for Heterogeneity:
    Q(df = 18) = 35.830, p-val = 0.007
    
    Model Results:
    
    estimate     se   zval   pval   ci.lb  ci.ub 
       0.060  0.036  1.655  0.098  -0.011  0.132  . 
    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1



### 3. 임의 효과 모형

•Q값: 관찰된 전체 분산값으로 실제 분산과 표집 오차를 더한 값이다. 이 값을 근거로 동질성 검증을 시행하는데, p값이 0.10보다 작은 경우 연구간의 이질성이 있다고 판단한다.  

•T^2(tau squared): 실제 서로 다른 모집단 효과 크기에 의 한 분산으로 연구 간 분산(실제 분산)을 의미한다. 평균 효과 크기의 범위를 구하기 위해 사용된다.  

•I^2(I squared): 총분산에 대한 실제 연구 간 분산의 비율 이다. 25% 이하이면 이질성이 작은 것으로, 50%이면 중간, 75% 이상이면 매우 큰 것으로 해석한다.  

주로 동질성 검증 p값이 0.1 이하이고 I2가 50% 이상이면 이질성이 상당하다고 판단하다. 



```R
# Random/mixed-effects models이 rma에서 기본값이다. method = "REML"가 default
# method 값인 REML, ML, DL, PM 등은 집단간의 변동인 τ^2을 추정하는 방법들을 의미한다
# 결과에 출력되는 tau^2 -> 집단 간의 변동을 의미한다. 
# 따라서 모집단이 다르거나, 치료 효과가 연구마다 다르다고 가정하는 임의 효과 모형에만 나온다.
# 전체가 동일한 모집단이라고 가정하는 고정 효과 모형에서는 출력 되지 않으며

res.RE <- rma(yi, vi, data = df1, digits = 3, slab = paste(author, year, sep = ", "))
res.RE
```


    
    Random-Effects Model (k = 19; tau^2 estimator: REML)
    
    tau^2 (estimated amount of total heterogeneity): 0.019 (SE = 0.016)
    tau (square root of estimated tau^2 value):      0.137
    I^2 (total heterogeneity / total variability):   41.86%
    H^2 (total variability / sampling variability):  1.72
    
    Test for Heterogeneity:
    Q(df = 18) = 35.830, p-val = 0.007
    
    Model Results:
    
    estimate     se   zval   pval   ci.lb  ci.ub 
       0.084  0.052  1.621  0.105  -0.018  0.185    
    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1



### 4. 숲 그림 (Forest plot)


```R
fig <- function(width, heigth){
     options(repr.plot.width = width, repr.plot.height = heigth)
}

fig(6, 6)

# 효과 크기의 95% 신뢰 구간 = yi - 1.96*sqrt(vi), yi + 1.96*sqrt(vi)
# 고정 효과 모형에 대한 forest plot

forest(res.FE, showweights=T, order="prec")
```


<img src="output_10_0.png" height="600" width="600">    
    


### 5. 깔때기 그림 (Funnel plot)


```R
fig(6, 6)

# 점 하나는 스터디 하나를 의미
# 점의 x 값: 효과 크기
# 점의 y 값: standard error

# 삼각형의 변: 통합 추정치 - 1.96 * standard error
# 삼각형은 통합 standard error

funnel(res.FE)
```


<img src="output_12_0.png" height="600" width="600">      
    


## 2. 범주형 변수

폐결핵(tuberculosis)의 예방에 대한 BCG 백신의 효과를 평가 하기 위해  
백신 접종을 받은 군(treated)과 대조군(control) 그룹에서 폐결핵 발생 여부를 조사하여  
2X2 교차표를 작성함

||Tb(+)|Tb(-)|
|---|---|---|
|Treated|tpos|tneg|
|Control|cpos|cneg|

### 1. 데이터 로딩


```R
df2 = as_tibble(dat.bcg)
df2
```


<table>
<thead><tr><th scope=col>trial</th><th scope=col>author</th><th scope=col>year</th><th scope=col>tpos</th><th scope=col>tneg</th><th scope=col>cpos</th><th scope=col>cneg</th><th scope=col>ablat</th><th scope=col>alloc</th></tr></thead>
<tbody>
	<tr><td> 1                  </td><td>Aronson             </td><td>1948                </td><td>  4                 </td><td>  119               </td><td> 11                 </td><td>  128               </td><td>44                  </td><td>random              </td></tr>
	<tr><td> 2                                                              </td><td>Ferguson &amp; Simes  </td><td>1949                                                            </td><td>6             </td><td>300                 </td><td> 29                                                             </td><td> 274       </td><td>55                                                              </td><td>random            </td></tr>
	<tr><td> 3                  </td><td>Rosenthal et al     </td><td>1960                </td><td>  3                 </td><td>  228               </td><td> 11                 </td><td>  209               </td><td>42                  </td><td>random              </td></tr>
	<tr><td> 4                                                              </td><td>Hart &amp; Sutherland  </td><td>1977                                                            </td><td> 62                                                             </td><td>13536                                                           </td><td>248                                                             </td><td>12619                                                           </td><td>52                                                              </td><td>random              </td></tr>
	<tr><td> 5                  </td><td>Frimodt-Moller et al</td><td>1973                </td><td> 33                 </td><td> 5036               </td><td> 47                 </td><td> 5761               </td><td>13                  </td><td>alternate           </td></tr>
	<tr><td> 6                                                              </td><td>Stein &amp; Aronson   </td><td>1953                                                            </td><td>180                                                             </td><td> 1361                                                           </td><td>372                                                             </td><td> 1079                                                           </td><td>44                                                              </td><td>alternate                                                       </td></tr>
	<tr><td> 7                  </td><td>Vandiviere et al    </td><td>1973                </td><td>  8                 </td><td> 2537               </td><td> 10                 </td><td>  619               </td><td>19                  </td><td>random              </td></tr>
	<tr><td> 8                  </td><td>TPT Madras          </td><td>1980                </td><td>505                 </td><td>87886               </td><td>499                 </td><td>87892               </td><td>13                  </td><td>random              </td></tr>
	<tr><td> 9                                                              </td><td>Coetzee &amp; Berjak   </td><td>1968                                                            </td><td> 29                                                             </td><td> 7470                                                           </td><td> 45                                                             </td><td> 7232                                                           </td><td>27                                                              </td><td>random       </td></tr>
	<tr><td>10                  </td><td>Rosenthal et al     </td><td>1961                </td><td> 17                 </td><td> 1699               </td><td> 65                 </td><td> 1600               </td><td>42                  </td><td>systematic          </td></tr>
	<tr><td>11                  </td><td>Comstock et al      </td><td>1974                </td><td>186                 </td><td>50448               </td><td>141                 </td><td>27197               </td><td>18                  </td><td>systematic          </td></tr>
	<tr><td>12                                                              </td><td>>Comstock &amp; Webster</td><td>1969                                                            </td><td>  5                </td><td> 2493                                                           </td><td> 3                 </td><td> 2338                                                           </td><td>33                                                              </td><td>systematic                                                      </td></tr>
	<tr><td>13                  </td><td>Comstock et al      </td><td>1976                </td><td> 27                 </td><td>16886               </td><td> 29                 </td><td>17825               </td><td>33                  </td><td>systematic          </td></tr>
</tbody>
</table>



### 2. 효과 크기와 분산 계산


```R
# ai = tpos
# bi = tneg
# ci = cpos
# di = cneg

# 함수 escal을 이용하여 각 스터디의 효과의 크기와 분산을 계산 
# 아래 R 문장은 각 연구에 대하여 로그 상대위험(measure = "RR")와 그 분산을 계산하는 명령이다.

dat1 <- escalc(measure = "RR", ai = tpos, bi = tneg, ci = cpos, di = cneg, data = df2, 
    append = T)
dat1


```


<table>
<thead><tr><th scope=col>trial</th><th scope=col>author</th><th scope=col>year</th><th scope=col>tpos</th><th scope=col>tneg</th><th scope=col>cpos</th><th scope=col>cneg</th><th scope=col>ablat</th><th scope=col>alloc</th><th scope=col>yi</th><th scope=col>vi</th></tr></thead>
<tbody>
	<tr><td> 1                  </td><td>Aronson             </td><td>1948                </td><td>  4                 </td><td>  119               </td><td> 11                 </td><td>  128               </td><td>44                  </td><td>random              </td><td>-0.88931133         </td><td>0.325584765         </td></tr>
	<tr><td> 2                                                              </td><td><span style=white-space:pre-wrap>Ferguson &amp; Simes    </span></td><td>1949                                                            </td><td><span style=white-space:pre-wrap>  6</span>                     </td><td><span style=white-space:pre-wrap>  300</span>                   </td><td> 29                                                             </td><td><span style=white-space:pre-wrap>  274</span>                   </td><td>55                                                              </td><td><span style=white-space:pre-wrap>random    </span>              </td><td>-1.58538866                                                     </td><td>0.194581121                                                     </td></tr>
	<tr><td> 3                  </td><td>Rosenthal et al     </td><td>1960                </td><td>  3                 </td><td>  228               </td><td> 11                 </td><td>  209               </td><td>42                  </td><td>random              </td><td>-1.34807315         </td><td>0.415367965         </td></tr>
	<tr><td> 4                                                              </td><td><span style=white-space:pre-wrap>Hart &amp; Sutherland   </span></td><td>1977                                                            </td><td> 62                                                             </td><td>13536                                                           </td><td>248                                                             </td><td>12619                                                           </td><td>52                                                              </td><td><span style=white-space:pre-wrap>random    </span>              </td><td>-1.44155119                                                     </td><td>0.020010032                                                     </td></tr>
	<tr><td> 5                  </td><td>Frimodt-Moller et al</td><td>1973                </td><td> 33                 </td><td> 5036               </td><td> 47                 </td><td> 5761               </td><td>13                  </td><td>alternate           </td><td>-0.21754732         </td><td>0.051210172         </td></tr>
	<tr><td> 6                                                              </td><td><span style=white-space:pre-wrap>Stein &amp; Aronson     </span></td><td>1953                                                            </td><td>180                                                             </td><td> 1361                                                           </td><td>372                                                             </td><td> 1079                                                           </td><td>44                                                              </td><td>alternate                                                       </td><td>-0.78611559                                                     </td><td>0.006905618                                                     </td></tr>
	<tr><td> 7                  </td><td>Vandiviere et al    </td><td>1973                </td><td>  8                 </td><td> 2537               </td><td> 10                 </td><td>  619               </td><td>19                  </td><td>random              </td><td>-1.62089822         </td><td>0.223017248         </td></tr>
	<tr><td> 8                  </td><td>TPT Madras          </td><td>1980                </td><td>505                 </td><td>87886               </td><td>499                 </td><td>87892               </td><td>13                  </td><td>random              </td><td> 0.01195233         </td><td>0.003961579         </td></tr>
	<tr><td> 9                                                              </td><td><span style=white-space:pre-wrap>Coetzee &amp; Berjak    </span></td><td>1968                                                            </td><td> 29                                                             </td><td> 7470                                                           </td><td> 45                                                             </td><td> 7232                                                           </td><td>27                                                              </td><td><span style=white-space:pre-wrap>random    </span>              </td><td>-0.46941765                                                     </td><td>0.056434210                                                     </td></tr>
	<tr><td>10                  </td><td>Rosenthal et al     </td><td>1961                </td><td> 17                 </td><td> 1699               </td><td> 65                 </td><td> 1600               </td><td>42                  </td><td>systematic          </td><td>-1.37134480         </td><td>0.073024794         </td></tr>
	<tr><td>11                  </td><td>Comstock et al      </td><td>1974                </td><td>186                 </td><td>50448               </td><td>141                 </td><td>27197               </td><td>18                  </td><td>systematic          </td><td>-0.33935883         </td><td>0.012412214         </td></tr>
	<tr><td>12                                                              </td><td>Comstock &amp; Webster </td><td>1969                                                            </td><td> 5    </td><td> 2493                                                           </td><td> 3  </td><td> 2338                                                           </td><td>33                                                              </td><td>systematic                                                      </td><td> 0.44591340                                                     </td><td>0.532505845                                                     </td></tr>
	<tr><td>13                  </td><td>Comstock et al      </td><td>1976                </td><td> 27                 </td><td>16886               </td><td> 29                 </td><td>17825               </td><td>33                  </td><td>systematic          </td><td>-0.01731395         </td><td>0.071404660         </td></tr>
</tbody>
</table>



### 3. 임의 효과 모형


```R
res1.FE <- rma(yi, vi, data = dat1, method = "FE", digits = 3, slab = paste(author, year, sep = ", "))
res1.FE
```


    
    Fixed-Effects Model (k = 13)
    
    Test for Heterogeneity:
    Q(df = 12) = 152.233, p-val < .001
    
    Model Results:
    
    estimate     se     zval   pval   ci.lb   ci.ub 
      -0.430  0.040  -10.625  <.001  -0.510  -0.351  *** 
    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1



### 4. 고정 효과 모형


```R
res1.RE <- rma(yi, vi, data = dat1, method = "REML", digits = 3, slab = paste(author, year, sep = ", "))
res1.RE
```


    
    Random-Effects Model (k = 13; tau^2 estimator: REML)
    
    tau^2 (estimated amount of total heterogeneity): 0.313 (SE = 0.166)
    tau (square root of estimated tau^2 value):      0.560
    I^2 (total heterogeneity / total variability):   92.22%
    H^2 (total variability / sampling variability):  12.86
    
    Test for Heterogeneity:
    Q(df = 12) = 152.233, p-val < .001
    
    Model Results:
    
    estimate     se    zval   pval   ci.lb   ci.ub 
      -0.715  0.180  -3.974  <.001  -1.067  -0.362  *** 
    
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1



### 5. 숲 그림 (Forest plot)


```R
forest(res1.RE, showweights=T)
```


<img src="output_24_0.png" height="600" width="600">      
    


### 6. 깔때기 그림 (Funnel plot)


```R
funnel(res1.RE)
```


<img src="output_26_0.png" height="600" width="600">  




```R
citation(package='metafor')
```


    
    To cite the metafor package in publications, please use:
    
      Viechtbauer, W. (2010). Conducting meta-analyses in R with the
      metafor package. Journal of Statistical Software, 36(3), 1-48. URL:
      http://www.jstatsoft.org/v36/i03/
    
    A BibTeX entry for LaTeX users is
    
      @Article{,
        title = {Conducting meta-analyses in {R} with the {metafor} package},
        author = {Wolfgang Viechtbauer},
        journal = {Journal of Statistical Software},
        year = {2010},
        volume = {36},
        number = {3},
        pages = {1--48},
        url = {http://www.jstatsoft.org/v36/i03/},
      }

