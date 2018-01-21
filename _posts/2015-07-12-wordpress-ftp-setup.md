---
layout: post
title: "워드프레스 FTP 및 SFTP 설정하기"
categories: etc
comments: true
---

### FTP 인증 생략하기

1.　wp-config.php 파일의 마지막에 추가  

```php
if (is_admin()) {
    add_filter('filesystem_method', create_function('$a', 'return "direct";'));
    define('FS_CHMOD_DIR', 0751);  
}
```

2.　워드프레스 폴더의 소유자 변경  

```
sudo chown -R www-data:www-data /var/www/html/wordpress
```

3.　워드프레스폴더에 권한 부여  

```
sudo chmod -R 755 /var/www/html/wordpress
```


### SFTP로 EC2 인스턴스 접속

AWS에 파일을 전송하기 위하여 파일 전송 프로토콜인 SFTP를 이용한다. SFTP(Secure File Transfer Protocol)는 일반 FTP에 보안성을 강화한 것으로 데이터 전송을 암호화하기 때문에 보안상의 문제를 사전에 방지할 수 있다.

21번 포트를 사용하는 FTP와는 달리 SSH와 같은 포트인 22번을 사용한다. 즉, SSH로 EC2 인스턴스에 접속하는 것과 같이 SFTP 또한 *.ppk 파일을 이용해 접속한다.

1.　FTP 클라이언트 프로그램다운로드 - [FileZilla Client Download](https://filezilla-project.org/download.php?type=client)

2.　FileZilla 실행 → Edit - Settings 클릭

![wfs-1]({{ site.url }}/assets/post/20150712/wfs-1.png)

3.　Connection - SFTP → Add keyfile 클릭 → *.ppk 파일 추가 → OK 클릭

![wfs-2]({{ site.url }}/assets/post/20150712/wfs-2.png)

4.　Site Manager 클릭 → 각 항목을 입력한 후 Connect 클릭

Host에는 DNS 또는 IP 주소 입력한다. Protocol은 SFTP, Logon Type은 Normal로 선택하고, User에는 EC2 인스턴스에 접속할 때 쓰는 username을 입력한다. 본 포스팅에서는 Ubuntu에 접속하므로 ubuntu를 입력했다. 그 외에 Port, Password는 빈칸으로 놔두면 된다.

![wfs-3]({{ site.url }}/assets/post/20150712/wfs-3.png)

5.　EC2 인스턴스 접속 완료

![wfs-4]({{ site.url }}/assets/post/20150712/wfs-4.png)
