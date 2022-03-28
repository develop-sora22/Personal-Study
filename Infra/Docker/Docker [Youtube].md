#  Docker

[TOC]

## Docker의 정의

### 도커란?

어플리케이션을 패키징할 수 있는 툴

container라고 불리는 하나의 작은 소프트웨어 유닛 안에 `application`, `system Tools[환경설정]`, `Dependencies` 등을 하나로 묶어서 다른 서버, 다른 PC에도 쉽게 배포하고 안정적으로 구동할 수 있도록 도와주는 툴



우리 어플리케이션을 구동하는데 꽤 많은 것들이 필요해졌다.

그렇기 때문에 우리의 source파일만 서버에 배포하는 것은  우리 어플리케이션을 구동하는데 문제점이 있습니다.

(NodeJS, npm, 외부 라이브러리를 사용한다면 여러 Dependencies와 환경설정하는 환경변수 등 이런 모든 것들을 다 설정해줘야 한다. )

내 PC에서는 잘 되었는데  왜 서버에서만 안되는가? => 서버와 개발 PC의 NodeJS의 버전이 달라서



#### 도커 container

- 우리 어플리케이션 (ex: **app.js**)
- 우리 어플리케이션을 정상적으로 동작하기 위한 NodeJS, 환경설정 npm, 그리고 여러 라이브러리들의 dependencies, 그리고 어플리케이션에 필요한 다양한 리소스 들이 포함될 수 있다.
- 
- 





[도커 한방에 정리](https://www.youtube.com/watch?v=LXJhA3VWXFA&t=134s)
