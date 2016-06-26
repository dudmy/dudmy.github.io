---
layout: post
title: "AWS에 워드프레스 설치하기 1 (EC2 생성)"
categories:
  - Web
comments: true
---

워드프레스의 웹 서버로 클라우드 컴퓨팅 서비스인 아마존 웹 서비스(AWS)를 사용한다. AWS는 가상의 하드웨어 자원을 사용자에게 제공하고, 사용자는 그 위에 OS와 소프트웨어를 설치하여 클라우드 서비스를 이용한다.

우선 가상 컴퓨팅 환경을 제공하는 EC2(Elastic Compute Cloud) 인스턴스를 만든다. 컴퓨터 한대를 빌리는 것이라고 생각하면 된다. 자세한 설명은 [아마존 EC2 설명서](http://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html) 참고.  
　

### 아마존에 EC2 인스턴스 만들기 (Ubuntu)

1.　아마존 웹 서비스 가입: [Amazon Web Services](http://aws.amazon.com/ko/)

2.　관리자 콘솔에서 EC2 클릭

![awi-1]({{ site.url }}/public/post/20150630/awi-1.png)  

3.　우측 상단의 계정 이름 옆에 지역을 Tokyo로 선택  
아마존이 운영하고 있는 데이터 센터로 한국에서 가장 가까운 지역을 선택한다.

![awi-2]({{ site.url }}/public/post/20150630/awi-2.png)

4.　Launch Instance 클릭  

![awi-3]({{ site.url }}/public/post/20150630/awi-3.png)

5.　Free tier only 체크 후 Ubuntu(우분투) 선택  
AMI는 미리 만들어져 있는 가상 머신 이미지이다. 그중 Ubuntu가 OS인 무료 가상 머신 이미지를 설치한다. Free tier에 대한 정보는 [AWS 프리 티어](http://aws.amazon.com/ko/free/) 참고.

![awi-4]({{ site.url }}/public/post/20150630/awi-4.png)

6.　Free tier 버전인 micro 선택 후 Review and Launch 클릭  
Configure Instance Details를 클릭하면 인스턴스의 설정을 원하는 대로 할 수 있다. 여기서는 기본 인스턴스 설정으로 놔두고 넘어간다.

![awi-5]({{ site.url }}/public/post/20150630/awi-5.png)

7.　Edit security groups 클릭

![awi-6]({{ site.url }}/public/post/20150630/awi-6.png)

8.　Create a new security group 선택 후 이름과 설명 작성 → Add Rule 클릭 후 Type HTTP 선택 & Source Anywhere 선택 → Review and Launch 클릭
웹 서버 용으로 사용할 가상 머신이므로 HTTP 포트를 열어둔다. 언제 어디서나 접속하기 위해 IP는 Anywhere로 설정한다. SSH는 인스턴스 접속용이다.

![awi-7]({{ site.url }}/public/post/20150630/awi-7.png)

9.　Launch 클릭

![awi-8]({{ site.url }}/public/post/20150630/awi-8.png)

10.　Create a new key pair 선택 후 key 이름 작성 → Download Key Pair 클릭→ Launch Instances 클릭
인스턴스 접속을 위한 Key를 생성하고 다운로드한다.

> pem 파일을 통해서만 EC2 인스턴스에 접속할 수 있다. 분실할 경우 인스턴스를 삭제하고 다시 만들어야 한다. 또한 pem 파일이 노출될 경우 다른 사용자가 접속할 수 있으므로 보안에 신경 쓰며 잘 보관해야 한다.

![awi-9]({{ site.url }}/public/post/20150630/awi-9.png)

11.　View Instances 클릭

![awi-10]({{ site.url }}/public/post/20150630/awi-10.png)

12.　EC2 인스턴스 생성 완료

![awi-11]({{ site.url }}/public/post/20150630/awi-11.png)
