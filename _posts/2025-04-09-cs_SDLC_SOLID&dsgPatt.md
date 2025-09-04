---
layout: single
title: SDLC와 디자인패턴
categories: SDLC
tag: [SDLC]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# SOLID와 디자인 패턴

## 객체 지향 다섯 가지의 설계 원칙 : SOLID
- *S*RP(단일 책임 원칙) : 각 클래스는 하나의 기능(책임)
- *O*CP(개방-폐쇄 원칙) : 확장에는 열려있고, 수정에는 닫혀 있어야함
- *L*SP(리스코프 치환 원칙) : 자식 클래스는 부모 클래스의 기능을 손상시키지 않으면서 작동
- *I*SP(인터페이스 분리 원칙) : 인터페이스를 잘게 분리해서 사용해야함 
- *D*IP(의존성 역전 원칙) : 고수준의 클래스 타입이 아니라 저수준의 클래스 타입 으로 선언 

## 디자인 패턴
### 1. 생성패턴

#### (1) 싱글톤 패턴
클래스의 인스턴스, 즉 객체를 하나만 만들어 사용하는 패턴

#### (2) 팩토리 메소드 패턴
오버라이드된 메소드가 객체를 반환하는 패턴

#### (3) 추상 팩토리 패턴
서로 관련이 있는 객체들을 통째로 묶어서 팩토리 클래스로 만들고, 이를 조건에 따라 생성하도록 다시 팩토리를 만들어서 객체를 생성하는 패턴
#### (4) 빌더 패턴
복잡한 객체를 단계적으로 만들 수 있게 해주는 패턴 ex. .builder().name().age()

#### (5) 프로토타입 패턴
기존 객체의 복사를 통해 새로운 객체를 생성하는 패턴

### 2. 구조패턴
#### (1) 어댑터 패턴
호출 당하는 쪽의 메소드를 호출하는 쪽의 코드에 대응하도록 중간에 변환기를 통해 호출하는 패턴

#### (2) 브리지 패턴
추상화와 구현을 분리하여 두 가지를 독립적으로 확장할 수 있는 패턴

#### (3) 컴포지트 패턴
개별 객체와 복합 객체를 동일하게 다루어, 트리 구조의 객체를 구성

#### (4) 데코레이터 패턴
메소드 호출의 반환값에 변화를 주기 위해 중간에 장식자를 두는 패턴

#### (5) 퍼사드 패턴
서브시스템을 더 쉽게 사용할 수 있도록 단순한 인터페이스를 제공하는 패턴

#### (6) 플라이웨이트 패턴
공유 가능한 객체를 통해 메모리 사용을 최적화하는 패턴

#### (7) 프록시 패턴
제어 흐름을 조정하기 위한 목적으로 중간에 대리자를 두는 패턴

### 3. 행위 패턴

#### (1) 옵저버 패턴
어떤 객체의 상태가 변할 때 그와 연관된 객체들에게 메시지를 보내는 패턴

#### (2) 전략 패턴
클라이언트가 전략을 생성해 전략을 실행할 컨텍스트에 주입하는 패턴

#### (3) 커맨드 패턴
실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴

#### (4) 상태 패턴
상태에 따라 행동을 다르게 하기 위해 객체의 상태를 클래스로 분리하는 패턴

#### (5) 책임 연쇄 패턴
여러 객체가 순차적으로 요청을 처리할 수 있도록 연결해주는 패턴

#### (6) 방문자 패턴
객체의 구조는 변경하지 않고, 방문자 인터페이스를 통해  객체에 새로운 동작(기능)을 추가할 수 있게 해주는 패턴입니다.

#### (7) 인터프리터 패턴
수학 표현식, 논리식 등 언어 형식으로 된 데이터를 처리할 때 사용하는 패턴

#### (8) 메멘토 패턴
객체의 이전 상태를 저장했다가 필요할 때 복원할 수 있게 해주는 패턴

#### (9) 중재자 패턴
객체 간의 직접적인 통신을 피하고, 중재자(Mediator)라는 중앙 집중형 객체를 통해 서로 소통하게 만드는 패턴

#### (10) 템플릿 메소드 패턴
상위 클래스의 견본 메소드에서 하위 클래스가 오버라이딩한 메소드를 호출하는 패턴

#### (11) 이터레이터 패턴
컬렉션의 내부 구조를 노출하지 않고, 그 안에 들어 있는 요소들을 하나씩 순차적으로 접근할 수 있게 해주는 디자인 패턴

<table style="border: 2px;">
  <tr>
    <td style="border: 1px solid black; text-align:center;" colspan = 3> 생성 패턴 </td>
    <td style="border: 1px solid black; text-align:center;" colspan = 3> 구조 패턴 </td>
    <td style="border: 1px solid black; text-align:center;" colspan = 3> 행위 패턴 </td>
  </tr>
  <tr>
    <td style="border: 1px solid black;"> 싱글톤 패턴 </td>
    <td style="border: 1px solid black;"> 팩토리 메소드 패턴 </td>
    <td style="border: 1px solid black;"> 추상 팩토리 패턴 </td>
    <td style="border: 1px solid black;"> 어댑터 패턴 </td>
    <td style="border: 1px solid black;"> 브리지 패턴 </td>
    <td style="border: 1px solid black;"> 컴포지트 패턴 </td>
    <td style="border: 1px solid black;"> 옵저버 패턴 </td>
    <td style="border: 1px solid black;"> 전략 패턴 </td>
    <td style="border: 1px solid black;"> 커맨드 패턴 </td>
  </tr>
  <tr>
    <td style="border: 1px solid black;"> 빌더 패턴 </td>
    <td style="border: 1px solid black;"> 프로토타입 패턴 </td>
    <td style="border: 1px solid black;"> </td>
    <td style="border: 1px solid black;"> 데코레이터 패턴 </td>
    <td style="border: 1px solid black;"> 퍼사드 패턴 </td>
    <td style="border: 1px solid black;"> 플라이웨이트 패턴 </td>
    <td style="border: 1px solid black;"> 상태 패턴 </td>
    <td style="border: 1px solid black;"> 책임 연쇄 패턴 </td>
    <td style="border: 1px solid black;"> 방문자 패턴 </td>
  </tr>
  <tr>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;"> 프록시 패턴 </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;"> 인터프리터 패턴 </td>
    <td style="border: 1px solid black;"> 메멘토 패턴 </td>
    <td style="border: 1px solid black;"> 중재자 패턴 </td>
  </tr>
  <tr>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;">  </td>
    <td style="border: 1px solid black;"> 템플릿 메소드 패턴 </td>
    <td style="border: 1px solid black;"> 이터레이터 패턴 </td>
    <td style="border: 1px solid black;">  </td>
  </tr>
</table>