# http와 https의 차이

> **부제: SSL의 동작원리**

#### 💡 HTTPS란? Hypertext Transfer Protocol Secure
- 웹 통신 프로토콜
- HTTPS는 HTTP의 기능에 `TLS(Transport Layer Security)` 혹은 `SSL(Secure Sockets Layer)`을 사용하여 암호화된 버전
- **HTTP**: `80` port
- **HTTPS**: `443` port

## SSL 인증서

#### 💡 SSL 인증서란?
* SSL 프로토콜에 사용되는 인증서
* HTTPS 프로토콜을 사용하기 위해서는 인증기관(CA)로부터 SSL 인증서를 발급받아야 한다.




💡 **CA란? - Certificate Authority**

위 인증서를 발급해주는 제 3자 인증기관

- **인증서 발급 기관(CA) 정보**
    
    - 코모도 : [www.comodossl.co.kr](http://www.comodossl.co.kr)
    - 시만텍 : [www.nortonstore.kr](http://www.nortonstore.kr)
    - 고 대디 : [kr.godaddy.com](http://kr.godaddy.com)
    - 글로벌사인 : [www.globalsign.com](http://www.globalsign.com)
    - DigiCert : [www.digicert.com](http://www.digicert.com)
    - StartCom :
    - Entrust : [www.entrust.com](http://www.entrust.com)
    - 버라이즌 : [www.verizon.com](http://www.verizon.com)
    - Trustwave : [www.trustwave.com](http://www.trustwave.com)
    - 세콤 : secom
    - Unizeto :
    - Buypass : [www.buypass.com](http://www.buypass.com)
    - Let's Encrypt : [letsencrypt.org](http://letsencrypt.org)
    - yessign : [www.yessign.or.kr](http://www.yessign.or.kr)
    
    대표적인 SSL인증기관(CA)이고 이밖에도 알려지지 않은 인증 기관들이 있다.
    
    대부분 유료 이며 이중에 Let`s Encrypt는 무료 이고 3개월 단위 갱신을 해야한다.
    
    이중에서 yessign은 금융인증센터에서 운영중인 한국 SSL인증기관(CA) 이다.
    

## SSL 인증서 발급 과정 및 원리



1. 웹 사이트는 자신의 domain을 기준으로 사이트 정보를 인증기관(`CA`)에 제공한다.
2. 인증기관(`CA`)는 검증을 거친 후 사이트 정보와 공개키를 인증기관의 개인키로 암호화 해서 인증서를 제작한다.
3. 웹 사이트에서는 발급 받은 `인증서`를 `웹서버`에 탑재한다(웹서버)
4. 인증기관은 웹브라우저에게 자신의 `공개키`를, 인터넷 사이트에게는 암호화한 `인증서`를 제공한다(웹서버)
5. Client에서 웹브라우저를 통해서 웹서버에 접속을 요청한다
6. Client 웹브라우저에 인증서를 보낸다
7. Client에서는 인증기관(CA)의 공개키와 웹사이트의(Site정보+Site공개키)를 가지고 대칭키를 암호화 한다.
8. 암호화 된 정보를 웹서버에 보낸다
9. 웹서버에서는 Site에서 가지고 있는 비밀키로 복호화 후 대칭키를 획득한다.(여기까지가 SSL해드쉐이크 완료 단계)
10. HTTS Session을 통해서 (대칭키 활용) 데이터를 주고 받음

**→ Client에서 웹브라우저의 종료시 해당 과정은 파기 된다(4~10과정)**

## SSL HandShake 과정

> handshake: 악수

서버와 클라이언트가 통신을 연결할 때 서로 악수(협상)를 통해 이런저런 정보를 주고받으며, 신뢰할 수 있는 서버인지 통신할 때 암호화는 이렇게 하자~ 하고 검증하고 정하는 과정.

### **SSL Handshake 목표 2가지 (중요)**

- 서버와 클라이언트가 주고받을 데이터의 _**암호화 알고리즘을 결정**_
- 서버와 클라이언트가 주고받을 데이터의 암호화를 위한 동일한 대칭키(데이터를 암호화하는 키)를 얻는다 ([대칭키 알고리즘 wiki](https://en.wikipedia.org/wiki/Symmetric-key_algorithm))

아래에서 노란색 플로우가 SSL Handshake 과정이다.

![https://blog.kakaocdn.net/dn/blusyN/btraoOJE4fj/TBe4rGKq1fbTVONcSHine0/img.png](https://blog.kakaocdn.net/dn/blusyN/btraoOJE4fj/TBe4rGKq1fbTVONcSHine0/img.png)

SYN, SYN ACK, ACK → TCP Layer의 3-way handshake

HTTPS가 TCP 기반의 프로토콜이기 때문에 협상에 앞서 연결을 생성하는 과정

Handshake 과정

1. 암호화 알고리즘(Cipher Suite) 결정
2. 데이터를 암호화할 대칭키(비밀키) 전달

- Cipher Suite 구성

![[Pasted image 20231118102011.png]]
### 참고

[https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/](https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/)

[https://www.lesstif.com/ws/ssl-43843966.html](https://www.lesstif.com/ws/ssl-43843966.html)