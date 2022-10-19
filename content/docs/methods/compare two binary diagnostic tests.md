---
title: compare two binary diagnostic tests
author: Ju
tags: 
  - diagnostic
  - statistic
  - Rpackage
categories: document
date: 2022-10-18
summary:
ShowToc: true
TocOpen: true
mathjax: true
cover:
  image: "https://juyujeng.github.io/researchlife/docs/methods/images/compbdt.svg"
  alt: "diagnost result table"

---

# 如何比較兩個診斷方式的準確性？

## 幾個可以比較的指標

sensitivity (*Se*) = 病人有病且可以被診斷出有病的機率 

specificity (*Sp*) = 病人沒病且可以被診斷沒病的機率 

positive predicted value (*PPV*) = 被診斷為有病而實際上真的有病的機率 

negative predicted value (*NPV*) = 被診斷為沒病而實際上真的沒病的機率 

## 可以使用的方法

| 統計方法     | 比較的指標               |
| ------------ | ------------------------ |
| McNemar test | sensitivity、specificity |
| GEE          | PPV、NPV                 |
| weighted generalized score             | PPV、NPV |

## 如何計算

GEE 的計算很難，而且在使用上的結果比較不直觀。雖然說Leisenring等人（2000）的概念令我感到驚豔（原來還可以這樣子用哦！）。他們以疾病的狀態為依變項做logistic GEE。二分的診斷結果X、二分的診斷工具Z為兩個預測變項。就變成了 $$logit(D) = \beta_1 Z + \beta_2 X + \beta_3 ZX$$
X = 1時，Z的係數是否顯著代表了兩個診斷工具的PPV是否有顯著差異；而X = 0時Z的係數則是NPV的差異。 所以要比較NPV時就看$\beta_1$有沒有顯著，但是在比較PPV時則是要看$\beta_1 + \beta_3$，這個檢定就比較難做。

weighted generalized score是近期比較多人使用的。而這個指標在計算上也是在不同的文獻當中有許多的改進。幸好最近有人將這些方法寫成了R的套件，有和`DTComPair`和`compbdt`兩個。

不過最近（2022/10/18）我發現`DTComPair`在新版的R已經不支援了，可能要找舊版的R去安裝，或是希望作者可以有更新。目前可能暫時就剩下`compbdt`可以使用。

### compbdt的使用

`compbdt`其實不是一個完整的套件，只有一個函式，可以在原始論文的[補充資料](https://static-content.springer.com/esm/art%3A10.1186%2Fs12874-020-00988-y/MediaObjects/12874_2020_988_MOESM1_ESM.txt)當中獲得。

使用時需要輸入兩種診斷工具的診斷結果（見下表）

```r
compbdt(s11 = s11, s10 = s10, s01 = s01, s00 = s00,
        r11 = r11, r10 = r10, r01 = r01, r00 = r00,
        alpha = .05)
```

![兩個診斷工具的診斷結果代號表（取自Roldán-Nofuentes, 2020）](../images/compbdt.svg)

這個函式的寫法會直接把分析的結果輸出到畫面上，並不會回傳任何資料。如果想要讓該函式回傳需要的統計值的話，需要自行修改作者提供的程式碼

例如我要輸出兩個診斷的PPV和NPV以及這兩個指標的比較統計值，我便在最後加上這些程式碼

```r
return(list(PPV1 = PPV1, # PPV for test 1
            PPV2 = PPV2, # PPV for test 2
            NPV1 = NPV1, # NPV for test 1
            NPV2 = NPV2, # NPV for test 2
            wald.s = Q1, # wald test statistics
            wald.p = globalpvalue3[1,1], # wald test p value
            wgst.ppv = T1, # PPV weighted generalized statistic
            wgsp.ppv = pvalue7, # PPV statistic p value
            wgst.npv = T2, # NPV weighted generalized statistic
            wgsp.npv = pvalue8)) # NPV statistic p value  
```

## references

Kosinski, A. S. (2013). A weighted generalized score statistic for comparison of predictive values of diagnostic tests. Statistics in Medicine, 32(6), 964–977. https://doi.org/10.1002/sim.5587

Leisenring, W., Alono, T., & Pepe, M. S. (2000). Comparisons of Predictive Values of Binary Medical Diagnostic Tests for Paired Designs. Biometrics, 56(2), 345–351. https://doi.org/10.1111/j.0006-341X.2000.00345.x

Roldán Nofuentes, J. A., Luna del Castillo, J. de D., & Montero Alonso, M. Á. (2012). Global hypothesis test to simultaneously compare the predictive values of two binary diagnostic tests. Computational Statistics & Data Analysis, 56(5), 1161–1173. https://doi.org/10.1016/j.csda.2011.06.003

Roldán-Nofuentes, J. A. (2020). Compbdt: An R program to compare two binary diagnostic tests subject to a paired design. BMC Medical Research Methodology, 20(1), 143. https://doi.org/10.1186/s12874-020-00988-y
