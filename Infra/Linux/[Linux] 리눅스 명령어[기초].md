# 리눅스 명령어[기초]

[TOC]

## wget

> wget [option] [url] :  인터넷에서 url로 파일을 현재 디렉토리에 다운로드할 수 있게 해주는 명령어

[option]

- -P [directory] : 다른 디렉토리에 저장하고 싶을때, 원하는 디렉토리를 쓰면 된다.

  -q : 다운로드 진행을 터미널에 출력되지 않게 함

  -0 [새로운파일명] : 파일을 '새로운파일명'으로 다운로드한다. 

  -i : 동시에 여러 파일 다운로드: 원하는 파일의 url들을 txt파일로 저장(각 url은 엔터로 구분)

  ``` bash
  $ wget -i downloadFiles.txt
  ```

**[출처]** [Ubuntu 20.04.4\] 우분투에서 wget으로 zoom 설치하기](https://blog.naver.com/kongbeen28/222687357576)

<br/>

<br/>

## zip

> zip 은 여러 파일을 묶고 압축할 수 있는 유틸리티로 tar 와는 달리 아카이빙과 압축을 같이 할 수 있습니다.

### 하위 디렉토리 압축

- 하위 디렉토리를 포함하는 압축 옵션인 *-r* 을 사용해서 *compressed.zip* 파일에 */path/to/dir* 내용을 압축합니다.

  ```bash
    zip -r compressed.zip /path/to/dir
  ```

### 여러 소스 압축

- dir1, dir2, file3 세 개의 소스를 압축합니다.

  ```bash
    zip -r compressed.zip /path/to/dir1 /path/to/dir2 /path/to/file3
  ```

### zip 에 내용 추가

- 이미 존재하는 zip 파일에 새로운 파일 추가합니다.

  ```bash
    zip compressed.zip path/to/file
  ```

### 특정 폴더 제외

- 특정 폴더를 제외하려면 -x 옵션을 사용하며, 아래 예제는 *.git* 폴더를 빼고 압축하는 예제입니다.

  ```bash
    zip -9 -r compressed.zip /path/to/dir -x *.git'
  ```

<br/>

## unzip

> unzip 은 zip 으로 압축된 파일을 푸는 명령어입니다.

### 압축내 목록 보기

- 압축을 해제(❌)하지 않고 압축 파일내의 목록만 출력합니다.

  ```bash
    unzip -l compressed.zip
  ```

### 압축 해제

- 현재 폴더에 압축 해제합니다.

  ```bash
    unzip compressed.zip
  ```

### 특정 폴더에 해제

- 압축이 풀릴 대상을 지정하는 -d 옵션을 사용하여, 원하는 폴더에 압축을 해제할 수 있습니다.

  ```bash
    unzip compressed.zip -d /path/to/put
  ```

### 여러 파일 압축 해제

- 여러 압축 파일을 해제할 경우 bash 의 for 함수를 이용해서 간단하게 처리할 수 있습니다.

  ```bash
    for i in *.zip; do unzip $i -d /path/to/put;done
  ```

- *unzip* 은 file globing 을 제대로 지원하지 않아서 아래와 같이 사용할 수 없습니다.

  ```bash
    unzip *.zip  -d /path/to/put
  ```

## 주요 옵션

- **zip**
  - r : 디렉토리까지 압축
  - 1 : 빠른 압축(압축률 ⬇)
  - 9 : 높은 압축률 (속도 ⬇)
  - e : zip 파일에 암호 설정
  - x : 압축시 파일 제외
- **unzip**
  - d : 지정한 디렉토리에 압축 해제
  - l : 압축 파일내 목록 보기

**[출처]** [[Linux] zip/unzip 압축 및 압출해제](https://wooono.tistory.com/283)

<br/>

<br/>

## ln

> link 명령어, 한 파일을 다른 파일 이름으로도 사용하고자 할 때 사용하는 명령어
> 링크된 파일 중 한 파일을 수정하면 다른 파일들도 수정된다.
>
> ln -s 는 심볼릭 링크로, 링크로 생성된 파일에 내용이 존재하지 않고 원본 파일명이 바뀌면 사용하지 못한다.

``` bash
$ ln -s [원본 파일명] [대상 파일명]
```

**[출처]** [리눅스 명령어 ln](https://pooboo.tistory.com/114)

<br/>

<br/>



