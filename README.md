Docker
=============

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

## Docker ps

## Docker run

## Docker exec

## Docker logs

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
