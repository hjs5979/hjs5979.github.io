---
layout: single
title: C언어
categories: 프로그래밍
tag: [프로그래밍]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 자료형

<table style="border: 2px;">
  <thead>
    <tr>
      <td style="border: 1px solid black;"> 자료형 </td>
      <td style="border: 1px solid black;"> 의미 </td>
      <td style="border: 1px solid black;"> 출력서식 </td>
      <td style="border: 1px solid black;"> 용도 </td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black;"> int </td>
      <td style="border: 1px solid black;"> 정수 </td>
      <td style="border: 1px solid black;"> %d</td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> unsigned int </td>
      <td style="border: 1px solid black;"> 양의 정수 </td>
      <td style="border: 1px solid black;"> %u </td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> long </td>
      <td style="border: 1px solid black;"> 큰 정수 </td>
      <td style="border: 1px solid black;"> %ld </td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> unsigned long </td>
      <td style="border: 1px solid black;"> 큰 양의 정수 </td>
      <td style="border: 1px solid black;"> %lu </td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> long long </td>
      <td style="border: 1px solid black;"> 매우 큰 정수 </td>
      <td style="border: 1px solid black;"> %lld </td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> float </td>
      <td style="border: 1px solid black;"> 실수(소수점 6자리) </td>
      <td style="border: 1px solid black;"> %f </td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> double </td>
      <td style="border: 1px solid black;"> 실수(소수점 15자리) </td>
      <td style="border: 1px solid black;"> %lf </td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> char </td>
      <td style="border: 1px solid black;"> 문자 </td>
      <td style="border: 1px solid black;"> %c </td>
      <td style="border: 1px solid black;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid black;"> char[] </td>
      <td style="border: 1px solid black;"> 문자열 </td>
      <td style="border: 1px solid black;"> %s </td>
      <td style="border: 1px solid black;"></td>
    </tr>
  </tbody>
</table>

# 문자

```c
char ch = 'A';

printf("문자1: %c\n", ch);
```
```bash
문자1: A
```
```c
char ch2 = 65;

printf("문자2: %c\n", ch2);  
```
```bash
문자2: B
```

# 문자열

```c
char str1[] = "ABC";

printf("문자열1 : %s\n", str1);
```
```bash
문자열1 : ABC
```
```c
printf("문자열2 : ");

for (int i = 0; str1[i] != '\0'; i++) {
    printf("%c ", str1[i]);
}

printf("\n");
```
```bash
문자열2 : A B C
```
```c
// 문자열 길이
printf("문자열 길이 : %lu\n", strlen(str1));
```
```bash
문자열 길이 : 3
```
```c
// 문자열 복사
char src[] = "ABC";
char dest[10];

strcpy(dest, src);

printf("복사된 문자열: %s\n", dest);
```
```bash
복사된 문자열: ABC
```
```c
// 문자열 합치기
char str2[] = "ABC";
char str3[] = "DEF";

strcat(str2, str3);

printf("합쳐진 문자열 : %s\n", str2);
```
```bash
합쳐진 문자열 : ABCDEF
```
```c
// 문자열에서 문자 인덱스 찾기
char str4[] = "ABC";
char *ptr = strchr(str4, 'B');

if (ptr != NULL)
    // 찾는 문자의 주소값에서 문자의 시작 주소값을 빼서 인덱스를 구함
    printf("B는 %ld에 있음\n", ptr - str4);
else
    printf("문자를 찾을 수 없음\n");
```
```bash
B는 1에 있음
```
```c
// 문자열에서 문자 포함 여부
char str5[] = "ABC";
char *found = strstr(str5, "B");

if (found != NULL)
    printf("B의 주소값 이후의 문자열 : %s\n", found);
else
    printf("못 찾음\n");
```
```bash
B의 주소값 이후의 문자열 : BC
```
