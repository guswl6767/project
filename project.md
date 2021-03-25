# apache 와 Nginx 
## **apache**
MPM 방식으로 HTTP 요청을 처리합니다.

> MPM:  Multi-Process Module 은 크게 두 가지 방식이 있습니다.
> * PreFork 방식
>  * Worker 방식

### 1. PreFork 방식
   ![PreFork MPM (다중 프로세스)](https://taetaetae.github.io/images/apache-vs-nginx/prefork.gif)
* Client 요청에 대해 apache 자식 프로세스를 생성하여 처리합니다.
* 요청이 많을 경우 Process 를 생성하여 처리합니다. 이 방식은 Apache 설치시 default 로 설정되어 있습니다.
* 하나의 자식프로세스당 하나의 스레드 를 갖습니다. (최대 1024개)
* 스레드간 메모리 공유를 하지 않습니다. 이 방식은 독립적이기에 안정적인 반면, 메모리 소모가 크다는 단점이 있습니다.

### 2. Worker MPM 방식
   ![Worker MPM (멀티 프로세스-스레드)](https://taetaetae.github.io/images/apache-vs-nginx/worker.gif)
* Prefork 보다 메모리 사용량이 적고 동시접속자가 많은 사이트에 적합합니다. 각 프로세스의 스레드를 생성해 처리하는 구조입니다.
* 스레드 간의 메모리 공유가 가능합니다.
* 프로세스 당 최대 64개의 스레드처리가 가능하며, 각 스레드는 하나의 연결만을 부여받습니다.
* 참고로 WAS로 tomcat을 연동하는 경우라면 mod_jk, mod_proxy, mod_proxy_ajp 방식을 Apache 자체적으로 지원해주기 때문에 다양하고 효율적으로 tomcat을 연동할수 있습니다.

____
## **Nginx**
프로그램의 흐름이 이벤트에 의해 결정이 되는 Event Driven 방식의 웹 서버입니다.

### 1.Event-Driven 처리 기반 구조
![Event-Driven 처리 기반구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnsMYW%2FbtqxGaQTl6O%2FzQwB8vDmOaadPSRnzHIYRk%2Fimg.png)
- Event-Driven 방식이란 요청에 대한 각 상태를 정해서 Event가 발생할 때마다 event를 처리하는 것을 말합니다.
- Event-Driven 처리 기반 구조는 여러 개의 커넥션을 모두 Event-Handler를 통해 비 동식 방식으로 처리해 먼저 처리되는 것부터 로직이 진행하도록 합니다. 이러한 기법의 주 사용 목적은 대화형 프로그램을 만드는 데 사용하는데 PCP 처리와 유사합니다.


### 2. Nginx 처리 과정
   
![Nginx 처리과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtfUgp%2FbtqxGjGN1cV%2Fzyjd1YvteWksq9wg5KKf10%2Fimg.png)
* NginX는 스레드를 많이 사용하지 않기 때문에context Switching비용이 적고CPU소모도 낮습니다.

* 적은 수의 스레드로 효율적으로 일 처리하며, 스레드당 할당되는 메모리도 적게 사용하는 구조 입니다. 
*   Event-Driven 처리 방식을 사용하다 보니 프로세스를 Fork 하거나 스레드를 사용하는 아파치와 달리 CPU와 관계없이 모든 IO들을 전부 Event Listener로 미루기 때문에 흐름이 끊기지 않고 응답이 빠르게 진행이 되어 1개의 프로세스로 더 빠른 작업이 가능하게 될 수 있습니다.

이 때문에 메모리 측면에서 NginX가 System Resource를 적게 처리한다는 장점이 있습니다.

____

## **Apache와 Nginx 비교**

![Apach와 Nginx 비교](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlFdAx%2FbtqxEP7wfLv%2F7zSjsmR1KQ6iq0j7DCAqOK%2Fimg.png)



## **Apache 선택이유**
### 1. 정적 컨텐츠 처리 
* 전통적인 파일기반 방식의 정적 컨텐츠
### 2. 동적 컨텐츠 처리
* 서버 내에서 처리하며
기본적으로 유연성과 범용성을 갖추는 방식으로 서버 자체내에서 동적 컨텐츠 처리가 가능합니다.
(**Nginx 동적 컨텐츠처리 가능하나 다소 복잡**)
### 3. OS 지원에 대한 범용성
* 리눅스, BSD, UNIX , Window로 지원범위가 다양하고 특히 Window까지 지원이 가능하여 일관성 있는 웹 서비스 아키텍쳐를 구현 할 수 있습니다.
(**Nginx는 Apache 만큼 완벽히 지원하지 않음**)

### 4. 분산/중앙집중식 구성 방식
* .htaccess 를 통해 디렉토리별로 추가 구성을 할 수 있습니다. 단일 기반 뿐만 아니라 분산형 구축이 가능하므로 대용량 서버 아키텍쳐에서 자원만 충분하다면 여러 웹 서비스를 구현 할 수 있습니다.
  (**Nginx는 Apache 처럼 .htaccess를 지원하지 않는다. 따라서 추가 구성을 할 수 없는 단점이 있음**)

### 5. 모듈 및 확장성/보안
* 60개 이상의 다양한 기능과 모듈을 지원하며, 필요에 따라 활성화 또는 비활성 시킬 수 있다.동적 모듈을 통해 웹 서버의 사용자 지정도 가능하게 할 수 있는 등 다양한 디자인과 확장이 가능합니다.
* 또한 보안을 위해 다양한 Web기반 DDoS  방어에 대한 기술을 제공합니다.

![구글 트렌드](https://taetaetae.github.io/images/apache-vs-nginx/google-trand.png)