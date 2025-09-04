---
layout: single
title: 추상 클래스와 인터페이스
categories: SDLC
tag: [SDLC]
use_math: true
toc: true
author_profile: true
sidebar:
  nav: docs
---

# 추상 클래스와 인터페이스의 의의 : 일관된 기능을 구현하도록 강제하여 코드의 안정성과 재사용성을 높임

## 1. 추상클래스
- 미완성된 메소드를 포함한 클래스. 
- 추상 메소드를 반드시 구현해야함. 
- 상속(Extend)으로 구현. 
- 공통 기본 기능은 일반 메소드로 구현, 하위 클래스마다 다르게 구현될 기능은 추상 메서드로 구현

예시 :

```java
public abstract class Animal {
    // 공통 속성
    String name;
    int age;

    // 생성자
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 모든 동물에 적용되는 공통 메서드
    public void breathing() {
        System.out.println("숨을 쉽니다.");
    }

    public void eating() {
        System.out.println("먹이를 먹습니다.");
    }

    // 동물마다 다른 행동 (추상 메서드)
    public abstract void bark();
}

// 추상 클래스를 상속받는 하위 클래스
class Dog extends Animal {
    public Dog(String name, int age) {
        super(name, age);
    }

    // 추상 메서드 반드시 구현
    @Override
    public void bark() {
        System.out.println("멍멍!");
    }
}

class Cat extends Animal {
    public Cat(String name, int age) {
        super(name, age);
    }

    // 추상 메서드 반드시 구현
    @Override
    public void bark() {
        System.out.println("야옹!");
    }
}
```

## 2. 인터페이스
- 메서드의 선언만 있는 클래스
- 내부에 있는 메소드를 반드시 구현해야함. 
- implements로 구현.
- 어떤 기능을 수행할 수 있는지에 대한 규약


예시 :

```java
public interface Flyable {

    // 상수선언(변경불가능)
    public static final int value = 100;

    // 추상 메서드만 선언
    void fly();

    void fall();
}

// 인터페이스를 구현하는 클래스들
class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("새가 날개를 퍼덕이며 하늘을 납니다.");
    }

    @Override
    public void fall() {
        System.out.println("새가 날개를 퍼덕이며 하늘을 떨어집니다.");
    }
}

class Airplane implements Flyable {
    @Override
    
    public void fly() {
        System.out.println("비행기가 엔진을 이용해 하늘을 납니다.");
    }

    public void fall() {
        System.out.println("비행기가 " + value + "m 떨어집니다.");
    }
}

}
```