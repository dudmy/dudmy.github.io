---
layout: post
title: "자바의 접근지정자와 캡슐화"
categories:
  - Java
comments: true
---

접근지정자(Access Modifier)는 클래스 내의 변수나 메소드의 접근을 제어하는 역할을 의미한다. 우선 접근제어자를 사용하는 이유를 알아본다.

> 캡슐화(Encapsulation)란?
> 
> 객체의 속성(data fields)과 행위(methods)를 하나로 묶고, 구현 내용 일부를 외부에 감추는 개념이다. 객체의 외부에서 내부 정보를 직접 접근하거나 조작할 수 없도록 정보 은닉(Information Hiding) 한다. 이는 정보 보호의 목적에서 만들어진 개념이다. - 위키피디아 - 

즉, 접근지정자는 OOP(Object-Oriented Programming)의 특징인 캡슐화를 위하여 사용된다. 외부에 감추는 방법으로 언어적 측면에서 접근지정자를 두어 감추는 정도를 기술하여 구현하는 것이다. 자바에는 4개의 접근지정자(private, default, protected, public)가 있다.

### public

변수나 메소드가 public으로 설정된 경우 모든 클래스에서 접근할 수 있다.

### protected

변수나 메소드가 protected로 설정된 경우 같은 패키지 내의 클래스 & 해당 클래스를 상속받은 클래스(외부 패키지 포함)에서 접근할 수 있다.

### default

접근지정자를 별도로 설정하지 않으면 해당 패키지 내에서만 접근할 수 있다. (외부 패키지에서는 접근 불가)

### private

변수나 메소드가 private으로 설정된 경우 해당 클래스 내에서만 접근할 수 있다.
