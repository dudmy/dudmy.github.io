---
layout: post
title: "AWS에 워드프레스 설치하기 3 (웹서버 설치)"
categories: web
comments: true
---

지금까지 AWS에 EC2 Ubuntu를 설치하고 RDS MySQL까지 설치했다. 워드프레스 설치를 위한 기본적인 인스턴스 생성은 마친 것이다. 이제 워드프레스가 돌아갈 웹서버를 구축 할 차례이다.

본 포스팅에서는 웹서버 구축에 가장 많이 사용되는 조합인 APM (Apache + PHP + MySQL)을 설치한다. 마지막으로 Wordpress를 설치한 후 마무리한다.


### PuTTY로 Ubuntu 접속하기

우선 Ubuntu에 접속하는 방법부터 알아본다. 자세한 설명은 [PuTTY를 사용하여 Windows에서 Linux 인스턴스에 연결](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/putty.html) 참고.

1.　[PuTTY Download Page](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)에서 putty.exe 파일, puttygen.exe 파일 다운

![awi-1]({{ site.url }}/assets/post/20150706/awi-1.png)

2.　puttygen.exe 실행→ Load 클릭 후 xxx.pem 파일 선택

EC2 인스턴스 생성 시 다운로드 한 private key file (*.pem)이 필요하다.

![awi-2]({{ site.url }}/assets/post/20150706/awi-2.png)

3.　Save private key 클릭

xxx.pem 파일이 ppk 파일로 저장된다. ppk 파일은 PuTTY에 사용하기에 올바른 형식으로 되어있어, PuTTY의 SSH 클라이언트를 사용하여 EC2 인스턴스에 연결할 수 있다.

![awi-3]({{ site.url }}/assets/post/20150706/awi-3.png)

4.　putty.exe 클릭

![awi-4]({{ site.url }}/assets/post/20150706/awi-4.png)

5.　Host Name에 EC2 인스턴스의 Public IP (ex. 12.34.56.789) 입력

Public IP는 AWS 관리자 콘솔→ EC2 → Instances에서 인스턴스 선택 후  Connect를 클릭하면 4번에 주소가 나온다.

![awi-5]({{ site.url }}/assets/post/20150706/awi-5.png)

![awi-6]({{ site.url }}/assets/post/20150706/awi-6.png)

6.　카테고리에서 Connection - SSH - Auth 선택→ Browse 클릭→ xxx.ppk 파일 선택→ Open 클릭

![awi-7]({{ site.url }}/assets/post/20150706/awi-7.png)

7.　ubuntu 입력하면 접속 완료

![awi-8]({{ site.url }}/assets/post/20150706/awi-8.png)


### Ubuntu에 APM 웹서버 설치

1.　설치 하기 전 저장소에서 설치된 패키지와 인덱스 정보 업데이트

```
sudo apt-get update
```

2.　Apache, PHP, PHP와 MySQL 연동을 설치 (워드프레스2 기준)

```
sudo apt-get install apache2 php5 php5-mysql
```

3.　아파치 홈 디렉토리인 html 로 이동 후 해당 위치에 워드프레스 설치

[Amazon Linux를 통한 WordPress 블로그 호스팅](http://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/hosting-wordpress.html) 참고.

```
cd /var/www/html
```

4.　워드프레스 설치 & 압축 해제

```
// 영문 버전
sudo wget http://wordpress.org/latest.tar.gz  

sudo tar -zxvf latest.tar.gz  

// 한글 버전
sudo wget https://ko.wordpress.org/wordpress-4.2.2-ko_KR.tar.gz  

sudo tar -zxvf wordpress-4.2.2-ko_KR.tar.gz  
```

5.　아파치 서버 재시작

```
sudo service apache2 restart
```

6.　브라우저에서 Public IP/wordpress (ex. 12.34.56.789/wordpress)로 접속하면 아래와 같은 화면이 등장 → Let's go! 클릭

![awi-9]({{ site.url }}/assets/post/20150706/awi-9.png)

7.　각 항목 입력 후 전송 클릭

데이터베이스 이름, 사용자 이름, 비밀번호에 RDS 인스턴스 생성 시 만든 Database Name, Master Username, Master Password를 입력한다. 데이터베이스 호스트는 AWS 관리자 콘솔 → RDS → Instances에서 인스턴스 선택 후 해당 Endpoint를 입력한다.

![awi-10]({{ site.url }}/assets/post/20150706/awi-10.png)

![awi-11]({{ site.url }}/assets/post/20150706/awi-11.png)

8.　PuTTY로 파일 생성 후 설치 실행하기 클릭  

* wordpress 디렉토리 안에 wp-config.php 파일 생성

```
sudo vi /var/www/html/wordpress/wp-config.php
```

* 다음의 텍스트 복사 후 wp-config.php 파일에 붙여넣기 (오른쪽 마우스 클릭)

![awi-12]({{ site.url }}/assets/post/20150706/awi-12.png)

* 키보드 Esc 누른 후 :wq! 입력 (w: 저장, q!: 종료 → 저장 후 종료)

9.　워드프레스 설치 완료

![awi-13]({{ site.url }}/assets/post/20150706/awi-13.png)
