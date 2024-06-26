---
id: 8
title: http, https. 대칭키, 비대칭키.
description: http 프로토콜로 통신을 하긴 하는데, 어떤 원리로 이루어지는거지? 속 까지 알아봐야 하지 않겠어?
date: '2024-02-02'
category: CS
---

# \[CS\] https. 대칭키와 비대칭키.

## http

http는 하이퍼 텍스트 전성 프로토콜로, 인터넷상에서 커뮤니케이션에 활용되는 규약이다. 주로 웹 브라우저와 서버 간에 데이터를 주고받을 때 사용된다. http는 주로 html 문서, 이미지, stylesheet, script 등을 전송하는 데 사용된다.

<br/>

클라이언트(일반적으로 브라우저)와 서버는 개별적인 메세지 교환에 의해 통신한다. 클라이언트에서 보내는 메시지를 request라고 부르고, 서버에서 반환하는 메시지를 response라고 부른다.

<br/>

브라우저에서 보내는 이러한 요청을 보자.

```
http://blabla
```

앞에 http가 붙는다. 이는 http 형식으로 요청을 보낸다고 알려주는 것이다.

<br/>

request와 response는 직접적으로 연결된 것이 아니라 사이에 중개자 역할을 해주는 여러 개체들이 존재한다. 그중 하나가 바로 proxy라는 것이다.

### proxy

proxy는 client와 server 간의 통신을 중개하고 중간에서 요청과 응답을 가로채는 역할을 한다. proxy는 이전에 서버에서 받은 response를 저장하고 동일한 요청이 들어온 경우, 저장된 응답을 제공하는 캐싱 기능을 제공한다. 또한, 여러 서버에 대한 요청을 분산시키는 로드 밸런싱, 특정 콘텐츠에 대한 접근을 제한하는 필터링, ip 주소를 숨기거나 정보를 조작하여 익명성 유지, 보안 위험 탐지와 차단 등의 다양한 역할을 한다.

<br/>

proxy는 client와 server 사이에서 통신을 중개하기 때문에 이런 다양한 작업이 가능하며, 그만큼 없어서는 안될 존재이다.

## https

https의 s는 secure의 약자이다. 즉, 기존의 http보다 안전하다는 뜻이다. http로 인터넷 요청을 하면 request 정보들이 텍스트로 보내지기 때문에 유출의 위험이 있다. https는 request 정보들을 암호화해서 보내기 때문에 http보다 안전하며, 검증된 사이트만 https 사용이 허가되기 때문에 사이트의 안정성 또한 검증 가능하다.

<br/>

이러한 https는 어떻게 구현될까?

<br/>

https의 암호화를 구현하기 위한 방법으로는 대표적으로 두 가지 방법이 있다. 바로 대칭키와 비대칭키(공개키)이다.

<br/>

대칭키부터 알아보도록하자.

## 대칭키

대칭키는 키 값을 어떠한 알고리즘에 넣고 암호문을 해석하여 결괏값을 찾아내는 방식이다. 키값이 없다면 암호문을 해독하는 것이 불가능하다. client와 server에 동일한 키가 있다면 암호화, 복호화를 통해 정보 전달이 가능하다.

<br/>

그림으로 표현한 모습은 아래와 같다.

<br/>

12345라는 데이터가 특정 키로 암호화되어 서버로 전달된다. 전달되는 도중에 어떠한 제삼자가 가로채더라도 해독이 불가능하다. server로 도착한 암호화된 데이터는 서버 측에서 키를 사용하여 복호화된다. 이렇게 안전하게 데이터를 전송할 수 있다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbqWFRX%2FbtsEli8X8eY%2F7ygw0Pt9a40EDfXJKAEtNk%2Fimg.png'/>

그런데 이 방법은 완벽하지 않다. 물론 방법 자체는 괜찮지만, key가 유출된다면?? 큰 사고이다. 또한, 초기에 서로 같은 key값을 어떻게 전달할지가 의문이다.

<br/>

따라서 비대칭키라는 방식이 개발되었다.

## 비대칭키

비대칭키에서는 두 개의 키가 있다. A키로 암호화를 하면 B키로만 복호화가 가능하다. 이 두 키들 중 하나를 공개하고 남은 하나는 보관한다. client에서 공개된 A키가 유출되더라도 B키로만 복호화가 가능하기 때문에 문제가 없다.

<br/>

그림으로 표현한 모습은 아래와 같다.

<br/>

12345라는 데이터를 빨간 키로 암호화하여 server로 보내면 파란 키로 복호화하여 데이터를 확인한다. server에서 client로 데이터를 보낼 때에도 마찬가지다. 54321이라는 데이터를 빨간 키로 암호화하여 client로 보내면 파란 키로만 복호화할 수 있다.

<img src='https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F05XZV%2FbtsEkAowRLh%2FgVYZVsHNnHc3e0GrYQqzBK%2Fimg.png'/>

이러한 비대칭키에서는 client가 server를 바로 신뢰할 수 없기 때문에 handshake를 통해 인증하는 과정이 필요하다. TCP handshake 또는, 3 way handshake로 불리는 것 과는 다른 개념이다.

<br/>

https에서 handshake 과정은 아래와 같다.

<br/>

1\. client에서 랜덤 한 데이터를 생성하여 server에 보낸다. server도 무작위의 데이터를 인증서와 함께 client에 대한 답변으로 보낸다.

<br/>

2\. client는 브라우저에 내장된 CA의 인증으로 인증서를 검사한다. 이후 pre-master secret을 생성하여 server에게 보낸다.

<br/>

3\. 비대칭키로 암호화와 복호화를 하는 것은 server에 부담이 되기 때문에 비대칭키와 대칭키를 혼합하여 사용한다. client는 처음 handshake에서 생성된 무작위의 데이터를 사용하여 세션키를 만들고, 이후의 통신은 이 세션 키를 사용하여 대칭키 암호화로 이루어진다.

## 마치며

오늘은 이렇게 간략하게나마 https의 원리에 대해 알아보았다.
