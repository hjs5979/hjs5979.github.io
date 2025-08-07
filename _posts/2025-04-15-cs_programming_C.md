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
```c
// 문자열 => int
char str6[] = "123";
int num = atoi(str6);
printf("문자열 => int : %d\n", num);
```
```bash
문자열 => int : 123
```
```c
// 문자열 => long
char str7[] = "123";
long num2 = atol(str7);
printf("문자열 => long : %ld\n", num2);
```
```bash
문자열 => long : 123
```
```c
// 문자열 => float
char str8[] = "123.123";
float num3 = atof(str8);
printf("문자열 => float : %f\n", num3);
```
```bash
문자열 => float : 123.123001
```
```c
// 문자열 => double
char str9[] = "123.123";
char *end; // 숫자 외 문자열을 저장할 장소
double num4 = strtod(str9, &end);
printf("문자열 => double : %lf\n", num4);
```
```bash
문자열 => double : 123.123000
```


# 숫자


```c
// int => 문자열
int num = 123;
char str[10];

sprintf(str, "%d", num);
printf("int => 문자열: %s\n", str);
```
```bash
int => 문자열: 123
```
```c
//int 절댓값
float num5 = 1.5;
int num6 = abs(num5);
printf("int 절댓값 : %d\n", num6);
```
```bash
int 절댓값 : 123
```
```c
//double 절댓값
float num7 = 1.5;
double num8 = abs(num7);
printf("double 절댓값 : %lf\n", num8);
```
```bash
double 절댓값 : 1.000000
```
```c
//제곱
int x = 2;
int y = 3;
int num9 = pow(2,3);
printf("제곱 : %d\n", num8);
```
```bash
제곱 : 8
```
```c
// 올림
float num10 = 1.86;
int num11 = ceil(num9);
printf("올림 : %d\n", num11);
```
```bash
올림 : 2
```
```c
// 내림
float num12 = 1.86;
int num13 = floor(num12);
printf("내림 : %d\n", num13);
```
```bash
내림 : 1
```