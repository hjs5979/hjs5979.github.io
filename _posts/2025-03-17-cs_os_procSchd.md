---
layout: single
title: 프로세스 스케줄링
categories: OS
tag: [OS]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 프로세스 스케줄링

프로세스가 생성되어 실행될 때 필요한 시스템의 여러 자원을 해당 프로세스에게 할당하는 작업

<br>

## 선점형 스케줄링

하나의 프로세스가 CPU를 차지하고 있을 때, 우선순위가 높은 다른 프로세스가 현재 프로세스를 중단시키고 CPU를 점유하는 스케줄링 방식

<br>

### SRT(Shorted Remaining Time)

현재 실행 중인 프로세스의 남은 시간과 준비상태 큐에 새로 도착한 프로세스의 실행 시간을 비교하여 가장 짧은 실행 시간을 요구하는 프로세스에 CPU를 할당하는 기법

<br>

### Round Robin (RR)

준비상태 큐에 먼저 들어온 프로세스가 먼저 CPU를 할당받지만 각 프로세스는 시간할당량 동안만 실행한 후 실행이 완료된 후 실행이 완료되지 않으면 다음 프로세스에게 CPU를 넘겨주고 준비상태 큐의 가장 뒤로 배치

<br>

### 다단계 큐

- 프로세스를 우선순위에 따라 시스템 프로세스, 대화형 프로세스, 편집 프로세스, 일괄 처리 프로세스, 학생 프로세스로 나눈다.
- 각 그룹은 독자적인 스케줄링 기법을 사용한다.
- 프로세스가 특정 그룹의 준비상태 큐에 들어갈 경우 다른 준비상태 큐로 이동할 수 없다.
- 하위 그룹의 준비상태 큐에 있는 프로세스를 실행하는 도중에 상위 단계 준비상태 큐에 프로세스가 들어오면 상위 단계 프로세스가 CPU를 선점한다.

<br>

### 다단계 피드백 큐

- 다단계 큐와 유사하지만, 최하위 그룹만 FCFS 기법 사용, 타 그룹은 RR 기법 사용
- 시간할당량을 모두 사용했지만, 프로세스가 종료되지 않은 경우, 프로세스는 하위 그룹으로 이동됨
- 하위 그룹으로 갈수록 시간할당량은 커짐
- 최하위 그룹에서는 기아 상태를 방지하기 위해 에이징 기법을 사용하여 너무 오래기다리면 상위 그룹으로 이동하는 방법을 사용하기도 함.

<br>

## 비선점형 스케줄링

### FCFS (First Come, First Serve)

먼저 들어온 프로세스가 먼저 수행

<br>

### SJF (Shortest Job First)

가장 짧은 실행시간을 갖는 프로세스가 종료 시 까지 선점

<br>

### HRN (Highest Response Ratio)

대기 중인 프로세스 중 현재 Response Ratio가 가장 높은 것이 프로세스 종료 시 까지 선점

Response Ratio = (대기시간 + 서비스시간) / 서비스시간

<table style="border: 2px;">
  <tr>
    <td style="border: 1px solid black; text-align:center;" colspan = 4> 선택 정렬 </td>
    <td style="border: 1px solid black; text-align:center;" colspan = 3> 삽입 정렬 </td>
  </tr>
  <tr>
    <td style="border: 1px solid black;"> SRT(Shorted Remaining Time) </td>
    <td style="border: 1px solid black;"> Round Robin (RR) </td>
    <td style="border: 1px solid black;"> 다단계 큐 </td>
    <td style="border: 1px solid black;"> 다단계 피드백 큐 </td>
    <td style="border: 1px solid black;"> FCFS (First Come, First Serve) </td>
    <td style="border: 1px solid black;"> SJF (Shortest Job First) </td>
    <td style="border: 1px solid black;"> HRN (Highest Response Ratio) </td>
  </tr>
</table>