# [실습] Docker

[TOC]

> 도커를 이용하면 빠르고 쉽게 웹 어플리케이션 실행 환경을 만들 수 있습니다.



## 1. 설치하기(ubuntu)

​	윈도우와 맥은 [도커 홈페이지](https://www.docker.com/products/docker-desktop)에서 할 수 있습니다.

​	아래 명령어를 순차적으로 실행합니다. 우분투에서는 기본적으로 모든 명령어 앞에 sudo를 붙여줘야 실행됩니다.

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
```

<br/>

## 2. Docker 파일 만들기

​	도커 파일을 만들 프로젝트 폴더에서 확장자가 없는 `Dockerfile` 파일을 만들고 아래 내용을 입력 후 저장합니다. 

```bash
#./Dockerfile
#기반이 될 이미지
FROM python:3

# 작업디렉토리(default)설정
WORKDIR /usr/src/app

## Install packages
# 현재 패키지 설치 정보를 도커 이미지에 복사
COPY requirements.txt ./

#설치정보를 읽어 들여서 패키지를 설치
RUN pip install -r requirements.txt

## Copy all src files
#현재경로에 존재하는 모든 소스파일을 이미지에 복사
COPY . .

## Run the application on the port 8080
#8000번 포트를 외부에 개방하도록 설정
EXPOSE 8000

#gunicorn을 사용해서 서버를 실행
#example 에는 장고의 경우 프로젝트 이름을 넣어줍니다.
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "example.wsgi:application"]
```

​	해당 폴더에는 `pip freeze > requirements.txt` 명령어를 통해 만들어진 `requirements.txt` 파일이 있어야 하며 `gunicorn` 모듈이 포함돼 있어야 합니다. 그리고 db호스트가 로컬로 되어 있다면 db 서버 호스트로 정보를 변경합니다.

<br/>

## 3. 이미지 빌드하기

​	앞서 만든 도커 파일을 이용해 도커 이미지를 빌드합니다.

```bash
docker build -t '도커허브에 가입한 계정명'/'이미지명(프로젝트명 권장)':'버전' .
ex) docker build -t whybein/wefish:0.1.0 .
```

 <br/>



## 4. 이미지 실행하기

 	만든 이미지를 실행하고 httpie 또는 postman으로 연결이 잘 되었는지 테스트 해봅니다.

```bash
docker run --name '컨테이너 명' -d'데몬으로 실행하기 위한 옵션' -p '호스트 포트':'컨테이너 포트' '이미지명'
ex) docker run --name wefish01 -d -p 8000:8000 whybein/wefish:0.1
```

 <br/>



## 5. 실행한 컨테이너 보기

```bash
> docker ps -a
CONTAINER ID        IMAGE                COMMAND                  CREATED              STATUS              PORTS                    NAMES
9efd11a3a730        whybein/wefish:0.1   "gunicorn --bind 0.0…"   About a minute ago   Up About a minute   0.0.0.0:8000->8000/tcp   wefish01
```

 <br/>



## 6. 실행한 컨테이너 종료하기

```bash
docker stop wefish01
```

 <br/>



## 7. 도커 로그인 하기

```bash
docker login
```

 <br/>



## 8. 도커 저장소에 이미지 푸쉬하기

```bash
docker push whybein/wefish:0.1
```

 <br/>



## 9. 컨테이너 삭제하기

​	실행중이면 삭제가 되지 않습니다. 종료 시킨 후 삭제할 수 있습니다.

```bash
docker rm wefish01
```

 



## 10. (다른 서버에서) 이미지 가져오기

​	이제 다른 서버에서 만들어놓은 도커 이미지를 사용할 수 있습니다.
​	사용하고자 하는 서버에서 도커를 설치하고 로그인 한 후 아래 명령어를 실행하고 이미지를 실행하면 됩니다.

```bash
docker pull whybein/wefish:0.1
```

 



## 11. Volume

파일들을 컨테이너로 복사하지않고 참조하도록 설정하는것
docker는 기본적으로 컨테이너를 삭제하면 데이터가 삭제되므로 데이터를 보존하고 싶을 때 혹은 여러 컨테이너간에 데이터를 공유해서 사용하고 싶을때 적용하면됨
Volume을 사용하는 방식은 두가지가 있음 ( 알고보니 Volume이 두가지방식은 아님 )

### 11.1 Volume방식

호스트의 특정 공간에 공유할 폴더를 생성하는 방식임

#### 11.1.1 Volume생성

```
docker volume create <volume-name>`
`docker volume create db-redis
```

#### 11.1.2 Volume조회

```
docker volume ls
```

#### 11.1.3 특정 Volume상세조회

```
docker volume inspect <volume-name>`
`docker volume inspect db-redis
```

- `Mountpoint`에 적힌 경로가 현재 volume값이 저장된 경로가 적혀있음
  ( ubuntu 20.04 에서 실행한 결과 `/var/lib/docker/volumnes/db-redis/_data` )

#### 11.1.4 Volume사용

`docker run`의 다른 옵션 입력하듯이 입력

- `docker run <option> -v <volume-name>:<container-route> <image-name>`
- `docker run --name my-redis -d -p 8080:8080 -v my-redis:/data redis`
  위 처럼 사용하면 my-redis이라는 폴더에 존재하는 파일들이 컨테이너의 `/data`에 공유됨
  컨테이너를 몇개를 사용하든 동일한 파일이 공유되며 하나가 수정되면 전체가 바뀜
- `-v usr/app/node_modules`처럼 작성시에는 현재 컨테이너환경에서 `node_modules`를 찾으라는 의미

#### 11.1.5 Volume제거

- 특정삭제: `docker volume rm <volume-name>`
- 전체삭제: `docker volume prune`

### 11.2 bind-mount방식

특정 volume을 생성하지않고 호스트의 특정 경로의 폴더를 공유하는것

#### 11.2.1 사용

- `docker run <option> -v <host-route>:<container-route> <image-name>`
- `docker run --name my-node -d -p 8080:8080 -v $(pwd):/app/src node:10`
  \- ubuntu 20.04 terminal 기준 현재경로는 `$(pwd)`
  \- window 10 cmd 기준 현재경로는 `%cd%`

<br/>

<br/>



## 참고자료

[Docker-설치부터 배포까지](https://velog.io/@swhybein/Docker-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

[Docker Volume & Bind-mount](https://velog.io/@1-blue/docker-volume-%EC%82%AC%EC%9A%A9%EB%B2%95)
