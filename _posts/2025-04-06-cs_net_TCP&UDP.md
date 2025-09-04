---
layout: single
title: TCP와 UDP
categories: 네트워크
tag: [네트워크]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# TCP

패킷을 추적 및 관리하는 프로토콜

## 특징

- 연결지향 방식
- 3-way handshaking과정을 통해 연결을 설정하고 4-way handshaking을 통해 해제한다.
- 흐름 제어 및 혼잡 제어(방식에 대해서 다른 문서에서 설명)
- 높은 신뢰성
- UDP보다 속도는 느림
- 전이중 방식
- 점대점 방식
- 헤더 20 Byte ~ 60 Byte
- ChekcSum 필드를 통해 데이터와 세그먼트 오류 검사.

# UDP

데이터그램 단위로 처리하는 프로토콜

## 특징

- 비연결형 방식
- 신호절차 없음
- CheckSum 필드를 통해 최소한의 오류만 검출
- 신뢰성이 낮음
- 속도가 빠름
