---
layout: single
title: C++언어
categories: 프로그래밍
tag: [프로그래밍]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 대표 함수 및 라이브러리

- iostream
  - std::cout << : 화면에 데이터를 출력할 때 사용합니다.
  - std::cin >> : 사용자로부터 데이터를 입력받을 때 사용합니다.
  - std::endl << : 출력 스트림에 개행 문자를 삽입하고 버퍼를 비우는 역할을 합니다. '\n'과 비슷하지만 버퍼를 비우는 기능이 추가되어 있습니다.
- string
  - std::string : 문자열을 저장하는 클래스입니다.
  - length() : 문자열의 길이를 반환합니다.
  - find() : 특정 문자열이나 문자를 찾아 인덱스를 반환합니다.
  - substr() : 문자열의 일부를 추출합니다.
- vector
  - push_back() : 벡터의 끝에 요소를 추가합니다.
  - pop_back() : 벡터의 끝 요소를 제거합니다.
  - size() : 벡터에 들어있는 요소의 개수를 반환합니다.
  - at() : 특정 인덱스의 요소를 안전하게 접근합니다.
- algorithm
  - sort() : 컨테이너의 요소를 정렬합니다.
  - find() : 컨테이너에서 특정 요소를 찾습니다.
  - min(), max() : 두 값 중 최솟값, 최댓값을 찾습니다.
  - swap() : 두 변수의 값을 교환합니다.

  # 문자

```c++
std::string str = "ABCD";
std::cout << "문자열 길이: " << str.length() << std::endl;
std::cout << "C의 위치: " << str.find("C") << std::endl;
std::cout << "부분 문자열: " << str.substr(2, 3) << std::endl;
```
```bash
문자열 길이: 4
C의 위치: 2
문자열 첫번째 문자: C
부분 문자열: CD
```
```c++
//문자열 => int
std::string str2 = "123";
int number = std::stoi(str2);
std::cout << "문자열 => int: " << number << std::endl;
```
```bash
문자열 => int: 123
```
```c++
//문자열 => long
std::string str2 = "1234";
long number = std::stol(str2);
std::cout << "문자열 => long: " << number << std::endl;
```
```bash
문자열 => long: 1234
```

# 숫자

```c++
// 숫자형
// C와 동일

int number = 12345;
double pi = 3.14159;

std::string str_number = std::to_string(number);
std::string str_pi = std::to_string(pi);

std::cout << "정수 변환: " << str_number << std::endl;
std::cout << "실수 변환: " << str_pi << std::endl;
```
```bash
정수 변환: 12345
실수 변환: 3.141590
```
```c++
// 숫자배열
int numbers[3] = {10, 20, 30}
```

# stringstream

```c++
// stringstream : 공백을 구분자로 여러 타입 추출 가능

std::string sentence = "I am 25 years old.";
std::stringstream ss(sentence);

std::string word1, word2;
int age;
std::string word3, word4;

ss >> word1 >> word2 >> age >> word3 >> word4;

std::cout << "단어1: " << word1 << std::endl; // I
std::cout << "단어2: " << word2 << std::endl; // am
std::cout << "나이: " << age << std::endl;  // 25
```
```bash
단어1: I
단어2: am
나이: 25
```