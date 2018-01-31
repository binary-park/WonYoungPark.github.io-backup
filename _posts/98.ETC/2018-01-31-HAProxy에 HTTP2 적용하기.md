---
layout: post
title: "HAProxy에 HTTP/2 적용하기"
subtitle: "ETC"
categories: document
tags: etc
published: false
comments: true
---



현재 HTTP/2 스펙을 지원하는 브라우저들의 **HTTP/2 프로토콜 이용 조건으로 TLS을 전제**하고 있기때문에 SSL 인증서가 필요합니다. 공인인증기관(CA)로 부터 인증을 받는것이 제일 좋겠지만, 개인 혹은 테스트 용도로 사용하는 것이라면 비용이 발생하므로 부담이 될 수 있습니다.

다행스럽게도 인증서를 만드는 방법이 몇가지 있습니다.

- 자가 서명 인증서(Self Signed Certificate)
  - 인증 서명 요청(CSR) 명시적 생성 -> 인증서에 self-sign -> 인증서 발급
  - key와 부가정보들을 입력하여 직접 self-sign하여 인증서 발급
- 더미 인증서(Dumy Certificate)
- 인증 서명 요청(CSR)을 만들어 공인 인증기관(CA)에 요청해서 발급 : **유료**



위의 방법 모두 공개키 기반(PKI)이므로 개인키(Private key)와 공개키(Public key)를 만들어야 합니다.



### 디지털 인증서

디지털 인증서는 공인 인증 기관(CA)에서 발급하며, 제한된 시간동안에만 사용할 수 있습니다. 만료일이 지나면 인증서를 교체해야 합니다. 인정서는 키 교환, 서버 인증, 클라이언트 인증에 사용됩니다.



인증서에는 인증서 소유자의 신분과 인증서 기구에 대한 정보가 포함되어 있어야 한다.

- 소유자 이름
- 소유자 퍼블릭 키
- 디지털 인증서가 발행된 날짜
- 인증서의 종료 날짜
- 발행 기구 이름 (CA의 기구명)
- 발행 기구의 디지털 서명



>  HAProxy의 HTTP/2는 ALPN을 통해서만 지원됩니다.



#### 인증서 포맷 변환

HAProxy와 호환되도록 인증서와 키를 .pem 형식으로 변환합니다.

```bash
> cat /etc/ssl/dummy.crt /etc/ssl/dummy.key > /etc/ssl/dummy.pem
```



### OpenSSL

#### 다운로드

```bash
> wget https://www.openssl.org/source/openssl-1.0.2n.tar.gz ~/openssl-1.0.2n.tar.gz
```

#### 압축 해제

```bash
> tar xzvf ~/openssl-1.0.2n.tar.gz -C ~/
```

#### 옵션 설정

```bash
> ./config --prefix=/usr --openssldir=/etc/ssl --libdir=lib no-shared zlib-dynamic
```

#### 컴파일 & 설치

```bash
> make && make install
```



#### 개인키 생성

```bash
# 암호 없는 개인키 생성 방법
# openssl genrsa -out <키이름> <비트 수>
> openssl genrsa -out cert.key 2048

# 암호 부여한 개인키 생성 방법
# openssl genrsa -<암호화알고리즘> -out <키이름> <비트 수>
> openssl genrsa -aes256 -out cert.key 2048
```



#### 공개키 생성

공개키는 개인키와 한 쌍을 이루기 때문에, 공개키를 생성할 때 개인키가 사용됩니다.

```bash
# openssl rsa -in <개인키 파일> -pubout -out <공개키 파일>
> openssl rsa -in cert.key -pubout -out cert_pub.key
```



#### 인증서명요청(CSR) 생성

```bash
# openssl req -new -key <개인키> -out <CSR 파일>
> openssl req -new -key cert.key -out cert.csr
```



### Slef-Sigend 인증서 생성

#### 인증서 생성

```bash
# 인증서명요청(CSR)을 삽입하여 인증서를 생성하는 방법
# openssl x509 -req -days <유효날수> -in <인증사인요청파일> -signkey <개인키> -out <인증서 파일명>
> openssl x509 -req -days 3650 -in cert.csr -signkey cert.key -out cert.crt

# 인증서명요청(CSR)을 삽입하지 않고, 인증서를 만드는 방법(암묵적으로 CSR이 생성됨)
# openssl req -new -x509 -days <유효날수> -key <개인키> -out <인증서파일명>
> openssl req -new -x509 -days 3650 -key cert.key -out cert.crt
```



### 인증서 정보 보기

```bash
> openssl x509 -text -in cert.crt -noout
```





### Reference

- [HTTP/2 에서 TLS 지원은 필수 사항일까?](http://ondemand.tistory.com/224)
- [[홈서버 구축기] SSL 인증서 만들기 (연습)](https://blog.hangadac.com/2017/07/31/%ED%99%88%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95%EA%B8%B0-ssl-%EC%9D%B8%EC%A6%9D%EC%84%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0-%EC%97%B0%EC%8A%B5/)
- [OpenSSL 로 ROOT CA 생성 및 SSL 인증서 발급](https://www.lesstif.com/pages/viewpage.action?pageId=6979614)
- [openssl 로 자가 서명 인증서 만들기](http://ohgyun.com/429)
- [The complete guide to HTTP/2 with HAProxy and Nginx](http://m12.io/blog/http-2-with-haproxy-and-nginx-guide)
- [HAProxy in 2018 - A load balancer with HTTP/2 and dynamic reconfig.](https://certsimple.com/blog/haproxy-http2-dynamic-load-balancing-ssl)