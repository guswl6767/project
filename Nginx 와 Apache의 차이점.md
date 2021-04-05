# Nginx 와 Apache의 차이점

### Nginx

> Apache의 동시 접속자수가 1만명이 넘어갈 때의 문제점(C10K Problem)을 해결하기 위해 만들어진 비동기 `Event-Driven ` 구조의 웹서버 소프트웨어

> 사용이 매우 심플하고 규모가 작은 서비스이면서 정적 데이터 처리가 많은 서비스에 적합.

#### Event Driven 방식:

요청이 들어오면 어떤 동작을 해야하는지만 알려주고 다른 요청을 처리하는 방식

프로세스를 fork 하거나 쓰레드를 사용하는 아파치와는 달리 CPU와 관계없이 모든 IO들을 전부 Event Listener로 미루기 때문에 흐름이 끊기지 않고 응답이 빠르게 진행되어 1개의 프로세스로 더 빠른 작업이 가능하게 될 수  있다.

### Apache

> 스레드/ 프로세스 기반 구조, 클라이언트 요청 하나당 스레드 하나가 처리하는 구조.
>
> MPM (Multi Processing Module) 아키텍쳐를 기반으로 클라이언트 요청 처리 



1) Prefork MPM 방식: 1개의 프로세스가 1개의 쓰레드를 개별로 처리

![img](https://blog.kakaocdn.net/dn/c3vjnp/btqBW1bQTB7/uwQArg6B27Nwv9JHm8K9r1/img.gif)



2) Worker MPM 방식: 1개의 프로세스가 여러개의 쓰레드로 처리 가능

![img](https://blog.kakaocdn.net/dn/brutQx/btqBXFl3PMo/eCZ8klssXtsJjg4OpDezj0/img.gif)



### Apahce의 한계점

> 클라이언트 접속마다 Process 혹은 Thread를 생성하는 구조라서 1만명 이상이 동시접속 요청이 들어온다면 CPU와 메모리 사용이 증가하고 대용량 요청에서 한계를 보인다.

> Netflix, Wordpress, 네이버, Github, 카카오 등 많은 곳에서 Apache에서 Nginx로 갈아타고 서버 대수를 감소시키면서 운영되고 있습니다.