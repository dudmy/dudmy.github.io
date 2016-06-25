---
layout: post
title: "AWS에 워드프레스 설치하기 2 (RDS 생성)"
categories:
  - Web
comments: true
---

워드프레스의 데이터베이스로 RDS(Relational Database Service)를 사용한다. Amazon RDS는 클라우드에서 관계형 데이터베이스를 더욱 간편하게 설정, 관리 및 확장할 수 있는 서비스이다. 자세한 설명은 [아마존 RDS 설명서](http://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/Welcome.html) 참고.

아래 과정의 세부적인 내용은 AWS Documentation에서 제공하는 [MySQL DB 인스턴스 생성 설명서](http://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html)에 나와있다.  
　

### 아마존에 RDS 인스턴스 만들기 (MySQL)

1.　관리자 콘솔에서 EC2 클릭

우선 RDS에 사용될 Security Group을 먼저 만든다.

![awi-1]({{ site.url }}/assets/images/post/20150704/awi-1.png)

2.　좌측에서 Security Groups을 선택 후 Create Security Group 클릭

![awi-2]({{ site.url }}/assets/images/post/20150704/awi-2.png)

3.　그룹의 이름과 설명을 작성→ Add Rule 클릭 후 Type MYSQL 선택 & Source Anywhere 선택→ Create 클릭

데이터베이스용으로 사용할 Security Group이므로 MYSQL 포트를 열어둔다. 언제 어디서나 접속하기 위해 IP는 Anywhere로 설정한다.

![awi-3]({{ site.url }}/assets/images/post/20150704/awi-3.png)

4.　관리자 콘솔에서 RDS 클릭

RDS를 위한 사전 작업은 끝났고, 본격적으로 인스턴스를 생성한다.

![awi-4]({{ site.url }}/assets/images/post/20150704/awi-4.png)

5.　Get Started Now 클릭

![awi-5]({{ site.url }}/assets/images/post/20150704/awi-5.png)

6.　여러 Database Engine 중에서 MySQL 선택

![awi-6]({{ site.url }}/assets/images/post/20150704/awi-6.png)

7.　No 선택 후 Next Stop 클릭

RDS 인스턴스를 매월 750시간 무료로 사용할 수 있는 Free Tier로 설치한다. 자세한 정보는 [Amazon RDS 프리 티어](http://aws.amazon.com/ko/rds/free/) 참고.

![awi-7]({{ site.url }}/assets/images/post/20150704/awi-7.png)

8.　DB 인스턴스 이름, Master Username & Password 입력 후 Next Step 클릭

Master Username과 Password는 MySQL에서 root계정과 같은 역할이다. DB 인스턴스에 로그인 하기 위해 사용되고, 추후 워드프레스 설치 시 필요하므로 잊지 않도록 한다.

![awi-8]({{ site.url }}/assets/images/post/20150704/awi-8.png)

9.　앞서 만든 Security Group 선택→ 데이터베이스 이름 입력

Database Name을 제공하지 않으면 만들고 있는 DB 인스턴스에서 Amazon RDS가 데이터베이스를 자동으로 만들지 않는다.

![awi-9]({{ site.url }}/assets/images/post/20150704/awi-9.png)

10.　Launch DB Instance 클릭

11.　View Your DB Instances 클릭

![awi-10]({{ site.url }}/assets/images/post/20150704/awi-10.png)

12.　RDS 인스턴스 생성 완료

![awi-11]({{ site.url }}/assets/images/post/20150704/awi-11.png)
