---
layout: post
title: Java Puzzles
categories: [Java]
tags: 编程语言
---

## `+=` 实质
```java
int x = 4;
long y = 3;
x = x + y;//报错
x = (int)(x+y);
x += y;// 相当于 x = (int)(x+y);
```
## `++i` and `i++`
```java
public class JavaP {
    public static void main(String[] args) {
        int i = 0;
        i++;
        int a = 0;
        int b = a++;// b = 0, a = 1
        int c = ++a;//c = 2, a = 2
        System.out.println("i: "+i);//1    
        i = i++;
        i = i++;
        i = i++;
        i = i++;
        System.out.println("i: "+i);//1
        for (int j = 0; j < 10; j++) {
            System.out.print(j+" ");//0 1 2 3 4 5 6 7 8 9 
        }
        System.out.println();
        for (int j = 0; j < 10; ++j) {
            System.out.print(j+" ");//0 1 2 3 4 5 6 7 8 9 
        }

    }

```
* `i++` 和 `++i` 单独使用没有区别，但是赋值时就有区别了。
* 当 `int i = i++` 时，i 先赋值后加一。
* 当 `int i = ++i` 时，i 先加一后赋值。
