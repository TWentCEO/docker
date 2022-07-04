Docker
=============

* 참조링크 : <https://helloailab.notion.site/Docker-1-bb3017bde7284a3ea3b0e06ec2639d79>

# 1. Docker의 정의

도커란? 어플리케이션을 패키징하는 것

* 컨테이너안에 Appliocation(코드), System Tools(환경), Dependencies를 하나로 묶어 다른 서버나 컴퓨터에 배포하여 안정성을 갖추어 실행할 수 있게 해주는 플랫폼
* 예를들어 어떠한 라이브러리의 버전이 맞지않아 실행할 수 없게 되는 상황을 해결해주기 위한 기술


## VM과 Container의 차이점

VM(Virtual Machine)은 무거운 운영체제를 포함하고 있기 떄문에 많은 자원을 차지함. 

Container는 VM의 경량화된 버전이라고 할 수 있다. HOST OS에서 Container Engine(Ex. Docker)만 설치하면 여러개의 어플리케이션을 고립된 환경에서 구동할 수 있게해줌. 즉 운영체제를 포함하지 않아 자원의 낭비가 없음.

컨테이너의 장점

* 애플리케이션 레벨 고립
* VM보다 빠른 setup과 자원 경량화
* 마이크레이션, 백업, 전송이 쉬움 (VM보다 크기가 작기 때문)
* 하드웨어와의 빠른 커뮤니케이션은 성능에 따라 효과적일 수 있다.
* 애플리케이션 배치와 유지보수를 향상
* 애플리케이션 전달 시간 감소 
기본적인 명령어
-------------
## Docker pull
* docker image repository 부터 Docker image 를 가져오는 커맨드입니다.
```shell
$ docker pull --help
```
#### ex)
```shell
$ docker pull ubuntu:18.04
```
* 참고사항
  * 추후 docker.io 나 public 한 docker hub 와 같은 repository 대신에, 특정 private 한 repository 에서 docker image 를 가져와야 하는 경우:
     * docker login 을 통해서 특정 repository 를 바라보도록 한 뒤, docker pull 을 수행하는 형태로 사용합니다.
   
## Docker images
* 로컬에 존재하는 docker image 리스트 출력
```
$ docker images
```
## Docker ps
* 현재 실행중인 도커 컨테이너 출력
```
$ docker ps
```
## Docker run
* 도커 컨테이너를 실행시키는 커맨드
```
$ docker run -it --name test1 ubuntu:18.04 /bin/bash
```
 * -it : -i 옵션 + -t 옵션
   * container를 실행시킴과 동시에 interactive한 terminal로 접속시켜주는 옵션
 * --name : 컨테이너 id 대신 이름 지정
 * /bin/bash : 컨테이너를 실행시킴과 동시에 실행할 커맨드로 bash 터미널을 사용
## Docker exec
* Docker 컨테이너 내부에서 명령을 내리거나, 내부로 접속하는 커맨드
```
$ docker exec -it test1 /bin/bash
```

## Docker logs
* 도커 컨테이너의 log를 확인하는 커맨드
```
$ docker logs test1
$ docker logs test1 -f
```
 * -f 옵션 : 실시간으로 watch 하며 출력 
## Docker stop
* 실행 중인 도커 컨테이너를 중단시키는 커맨드
```shell
$ docker stop
```
#### ex)
```shell
$ docker stop test1
```
## Docker rm
* 도커 컨테이너를 삭제하는 커맨드
```shell
$ docker rm --help
```
#### ex)
```shell
$ docker rm test1
$ docker rm test2
```
## Docker rmi
* 도커 이미지를 삭제하는 커맨드
```
$ docker rmi --help
```
#### ex)
```shell
$ docker images 
$ docker rmi busybox
```

* * * 

표준형 도커 만들기
------------------
* Dockerfile 파일 생성하고 편집기 이동
```
vi Dockerfile
```
* vi 편집기에서 명령어 입력
```
FROM ubuntu:18.04

RUN apt-get update

CMD ["echo","Hello Docker"]
```
* cat 명령어를 이용하여 저장한 파일 보기
```
cat Dockerfile
```

# 도커 빌드
```
$ docker build -t 이미지 이름:버전 경로
```

# 도커 이미지 찾기
```
docker images | grep my
```
* grep 옵션 : 앞에 명령어의 결과물 중에서 grep 뒤에 문자열이 있는 것을 찾아줌

도커 레지스트리
----------------
```
$ docker run -d -p 5000:5000 --name registry registry
```
* pull rate limit 오류 시
```
$ docker login
```

```
$ docker push [OPTION] NAME:[TAG]
```
*레지스트리에 생성한 이미지를 업로드
