---
title: zero-inflated vs. hurdle model
author: Ju
tags: 
    - blog 
    - count 
    - statistics
category: blog
date: 2022-01-04
---

# zero-inflated vs. hurdle model

[[zero-inflated model]]和[[hurdle model]]都是在處理count data有大量的0的時候常用的方法。

這兩種方法的結果也長得很像，都有一個針對多有可能為0的zero part，以及一個針對大於0的數字估計的count part。在文獻上兩種方法出來的結果適配度也很常是差不多的，有時候前者好一點，有時候後者好一點。

兩者主要的區分在於對0的概念上。zero-inflated model的假設中0可能有兩種分配的來源（zero part以及count part兩個分配），而hurdle model則假設0只來自zero part的那個binary分配。有的人是這麼說的，zero-inflated model的0可能是真的0（這事件本來就不會發生）以及取樣的0（有可能會發生，但是剛好取樣到0）。

因此，在選擇要使用兩一種時，如果適配結果都差不多時，就要傷點腦筋想一下，資料的0到底比較符合哪一種情況了。