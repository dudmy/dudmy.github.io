---
layout: post
title: "졸업 프로젝트 회의록"
categories:
  - Project
comments: true
---

### 1차 회의 - 2015.03.24
중간 발표 이후 첫 회의에 대한 내용입니다.

**애플리케이션 추가할 기능**

* GPS & 지도
* 지역 리스트
* 메뉴 리스트
* 결제 (쿠폰, 주문내역)
* 이벤트
* 비콘 DB 추가 -> 카페 DB에 비콘 ID 필드 추가  

**웹 페이지 추가할 기능**

* 로그인.jsp
* main.jsp
* 통계.jsp (jquery 공부)
* java beans 설계

**3월 30일까지의 계획 설계**

* 24~25: 메뉴 리스트, GPS&DB, 로그인.jsp
* 26~27: 메뉴&결제, 지역 리스트&비콘 분석, 통계 기능 추가
* 28~30: etc

---
　

### 2차 회의 - 2015.03.27
프로젝트의 수정 사항 및 진행 상태에 대한 회의 내용입니다.

**데이터베이스**

* 이벤트 DB 누락: 이벤트에 관한 정보를 담을 수 있는 테이블 생성
* 애플리케이션 및 웹 페이지 틀 작성을 위한 간단한 레코드 미리 삽입
* 프로젝트를 진행하면서 추가적으로 수정해야 할 부분이 있어 보임

**애플리케이션**

* 뷰 페이저: 뷰를 넘길 경우 애플리케이션이 멈추던 기존의 오류 수정
* 지역 리스트: 몇 가지 지역을 정하여 기본 틀 완성
* 간단한 틀만 만들고 후에 배치를 하는 것은 비효율적이라고 판단 -> 기본적인 디자인을 미리 구성하고 서버, 데이터베이스를 제작하며 개발

**웹 페이지**

* Java Beans, Mgr 작성 중 몇 가지 빠진 DB를 발견 -> 테이블 추가

**기타**

* 다른 졸프 팀과의 기능, 흐름 부분에서유사성을 발견 -> 그들과의 차별점을 고안해야 함
* 현재 구상해 놓은 부분은 최대한 빠른 시일내에 개발하고 추가부분 구상
* Navi를 포기하지 않고 ViewPager를 구현하기 위해 많은 시간이 소요됨 -> 구현에 성공하긴 했지만 일정이 빡빡한 만큼 버려야할 상황에는 가차없이 버리기

---
　

### 3차 회의 - 2015.03.30
프로젝트 2차 발표에 대한 회의입니다.

**시연 방법 논의**

* 아래 모든 사항을 PPT에 첨부
* 어플리케이션 실행 이미지
* 데이터베이스와 서버 구축 이미지
* 웹 페이지는 시연에서 제외 → Java Beans와 Mgr 코드 이미지

**진행 방향**

* 3/30~3/31: 기능 추가
* 4/1: 어플리케이션에 데이터베이스와 서버 연동
* 4/2: 발표 자료 작성 및 준비
* 4/3: 프로젝트 2차 발표

**기타**

* 3/31 졸업 프로젝트를 위한 첫 밤샘!

---
　

### 4차 회의 - 2015.04.06
앱과 웹의 진행방향을 구체적으로 수립하였습니다. 4월말까지 앱+비콘은 디자인 제외한 모든 기능, 웹은 약간의 디자인과 모든 기능을 완성하는 것이 목표입니다.

**애플리케이션 진행방향**

* GPS를 이용한 근처 카페 리스트 찾기
* 지역 리스트 별 카페 찾기
* 쿠폰, 주문 및 결제 과정 창
* 비콘을 이용한 이벤트 수신 및 발신
* 사용자 옵션 기능 (즐겨찾는 카페.)
* 현재까지 구현된 부분에서의 '사용 중지' 오류 수정 -> 리스트 크기에 대한 문제

**웹 페이지 진행방향**

* Ajax이용한 실시간 업데이트 구현
* Jquery통계 구현 -> 통계 Mgr부터 작성
* 위의 두 사항만 구현되면 그외는 쉽게 구현 가능 -> DB만 건드리면 되는 부분

**기타**

* 애플리케이션 완성 후 결제 모듈부분 추가적으로 고려
* 중간고사 등의 개인적인 일정 관리하며 졸업 프로젝트 진행
* 지도교수님께서 주신 자료를 통해 다른 앱과의 차별성 -> 완성 후 생각

**전체적인 일정표**

![gm]({{ site.url }}/public/post/20150324/gm.jpg)

---
　

### 5차 회의 - 2015.05.22
졸업 프로젝트 발표 전, 지도 교수님과의 마지막 회의입니다. 추가 기능이 아닌 시나리오를 기반으로 해야할 작업들을 정리했습니다.

**애플리케이션**

* 이벤트: 주문내역을 통해 사용자에게 해당 이벤트, 쿠폰 발생 (ex. 라떼러버)
* 비콘 거리에 따른 서비스 기능
* 네트워크 작업이나 무거운 작업들은 UI Main Thread 에서 돌리면 않됨

> ERROR: The application may be doing too much work on its main thread

**웹 페이지**

* 비콘으로 감지되는 사용자에 대한 정보 받기
* 구글 차트 & 이벤트 실시간으로 X -> 등록 시 새로 고침으로 변경
* 주문 내역만 Ajax 사용 -> 제작 완료 시 onclick (draw)
* 지점과 본점의 경계 허물기

**기타**

* 톰캣 <-> 노드 포트 충돌: 두 서버 모두 80포트 사용
* 80포트 이외에 열려있는 학교 포트 알아보기
* 시연 시나리오 구상