---
layout: single
title: DB 회복기법
categories: 데이터베이스
tag: [데이터베이스]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 1. 로그 기반 회복기법

- 즉시갱신 회복기법
  - 트랜잭션 실행 중 변경내역을 DB에 즉시 반영(Commit 된 것은 Redo, Commit 안된 것은 Undo)
- 지연갱신 회복기법
  - 트랜잭션이 부분완료(Partially Commited)될 때까지(Commit 단계만 남겨둘 때까지) DB반영을 지연(Commit 된 것만 Redo)
- 체크포인트 회복기법
  - 채크포인트 이후부터 즉시갱신/지연갱신 기법을 사용하는 것