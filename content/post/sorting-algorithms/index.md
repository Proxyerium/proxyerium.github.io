---
title: "排序算法"
date: 2023-10-24T12:23:05+08:00
slug: sorting-algorithm
math: true

categories:
    - notes
tags: 
    - algorithm
---

****
## 冒泡排序 Bubble-sort
最简单的算法之一。比较两个数据，然后根据需求交换位置，循环至没有任何数据被交换，即完成排序。

### 复杂度
> 时间复杂度：$O(n^2)$ \
> 空间复杂度：$O(n)$

### C实现
```c
/*
flag: immediately terminate sorting if equal 0
*/
for(int i=0; i<len-1; i++){
        for(int ii=0;ii<len-1-i; ii++){
            if(a[ii]>a[ii+1]){
                temp = a[ii];
                a[ii] = a[ii+1];
                a[ii+1] = temp;
                flag += 1;
            }
        }
        if(flag==0)
            break;
        else
            step += flag;
            flag = 0;
}
```