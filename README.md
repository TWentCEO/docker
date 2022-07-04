Docker
=============

* 참조링크 : <https://helloailab.notion.site/Docker-1-bb3017bde7284a3ea3b0e06ec2639d79>

# 1. Docker의 정의

도커란? 어플리케이션을 패키징하는 것

* 컨테이너안에 Appliocation(코드), System Tools(환경), Dependencies를 하나로 묶어 다른 서버나 컴퓨터에 배포하여 안정성을 갖추어 실행할 수 있게 해주는 플랫폼
* 예를들어 어떠한 라이브러리의 버전이 맞지않아 실행할 수 없게 되는 상황을 해결해주기 위한 기술
# 2. Docker Architectuer

![Docker Architectuer](https://user-images.githubusercontent.com/62507896/177143029-13b0acbe-5d00-45ab-944f-2d3a8496420c.png)

출처 : https://docs.docker.com/get-started/overview/

## VM과 Container의 차이점

![Container_VS_VM](https://user-images.githubusercontent.com/62507896/177144702-e0b8747f-9e81-46b4-9405-e043cecce67e.png)

출처 : https://tech.weperson.com/wedev/devops/docker/#docker%E1%84%85%E1%85%A1%E1%86%AB

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


-----------------------------
-----------------------------
-----------------------------
Kubernetes
===========================

# 1. Kubernetes 개념

쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식성이 있고, 확장가능한 오픈소스 플랫폼이다.

## Kubernetes 생성과정

![kubernetes_story](https://user-images.githubusercontent.com/62507896/177145786-1653f75f-d731-4fa0-872b-f7c8f323d07c.png)

1. 전통적인 배포 시대 : 초기 조직은 애플리케이션을 물리 서버를 통해 실행했다. 많은 애플리케이션이 사용되면 성능도 떨어지고 서버를 유지하기위해 많은 비용이 들었다.
2.  가상화된 배포 시대 : 단일 물리 서버의 CPU에 여러 Virtual Machine을 실행할 수 있게 한다. VM간에 애플리케이션을 격리하고 정보를 다른 애플리케이션에서 자유롭게 엑세스 할 수 없으므로, 일정 수준 보안성을 제공할 수 있다.
3.  컨테이너 개발 시대 : VM과 유사하지만 격리 속성을 완화하여 애플리케이션간에 운영체제를 공유한다. 그렇기 때문에 VM보다 장점이 많다.


## Kubernetes의 기능

* 서비스 디스커버리와 로드 밸런싱
  
  * DNS 이름을 사용하거나 자체 IP주소를 사용하여 컨테이너를 노출할 수 있다. 컨테이너에 대한 트래픽이 많으면, 네트워크 트래픽을 로드밸런싱하고 배포하여 배포가 안정적으로 이루어짐.

* 스토리지 오케스트레이션

    * 로컬 저장소, 공용 클라우드 공급자 등과 같이 원하는 저장소 시스템을 자동으로 탑재 할 수 있음.

* 자동화된 롤아웃과 롤뱃

    * 배포된 컨테이너의 원하는 상태를 서술할 수 있으며 현재 상태를 원하는 상태로 설정한 속도에 따라 변경할 수 있음.

* 자동화된 빈 패킹(bin packing)

    * 컨테이너화된 작업을 실행하는데 사용할 수 있는 쿠버네티스 클러스터 노드를 제공함.
    * 각 컨테이너가 필요로 하는 CPU와 메모리(RAM)를 쿠버네티스에게 지시함 
    * 쿠버네티스는 컨테이너를 노드에 맞추어서 리소스를 가장 잘 사용할 수 있도록 해줌.


* 자동화된 복구(self-healing)

    * 실패한 컨테이너를 다시 시작하고, 컨테이너를 교체하며, '사용자 정의 상태 검사'에 응답하지 않는 컨테이너를 죽이고, 서비스 준비가 끝날 떄까지 그러한 과정을 클라이언트에 보여주지 않음.

* 시크릿과 구성 관리

    * 쿠버네티스를 사용하면 암호, OAuth 토큰 및 SSH키와 같은 중요한 정보를 저장하고 관리 할 수 있음.
    * 컨테이너 이미지를 재구성하지 않고 스택 구성에 시크릿을 노출하지 않고도 시크릿 및 애플리케이션 구성을 배포 및 업데이트 할 수 있음.


# 2. Kubenetes 활용
# 1. Prerequisite

- References
    - minikube
        - [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)
    - kubectl
        - [https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/](https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/)
- 최소 사양
    - **CPU : 2 이상**
        - 원활한 실습을 위해서는 6 이상을 추천
    - **Memory : 2GB 이상**
        - 원활한 실습을 위해서는 12 GB 이상을 추천
    - **Disk : 20 GB 이상**
        - 원활한 실습을 위해서는 100 GB 이상을 추천
    - 가상화 tool : **Docker**, Hyperkit, Hyper-V, ...

### VM 스펙 업그레이드 필요

- CPU : multicore    
    - VM 생성 이후 **demo 용 머신 우클릭 → 설정 → 시스템 → 프로세서 → cpu 3개 이상으로 변경**
- Disk: 40 GB 이상
    - **VM 생성 단계**에서 Disk 크기 조절 후 **재생성** 필요

---

# 2. Let's Install Minikube

- minikube 의 최신 버전 (v1.22.0) 바이너리를 다운받고, 실행할 수 있도록 변경합니다.
    - 이하의 모든 커맨드는 amd 기반의 CPU 를 기준으로 합니다.
    - arm 기반의 CPU 는 공식 문서를 확인해주시기 바랍니다.

```bash
curl -LO https://storage.googleapis.com/minikube/releases/v1.22.0/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

- 정상 다운로드 확인

```bash
minikube --help
```

- 터미널에 다음과 같은 메시지가 한글 or 영어로 출력된다면 정상적으로 설치된 것입니다.
    
    ```bash
    minikube는 개발 워크플로우에 최적화된 로컬 쿠버네티스를 제공하고 관리합니다.
    
    Basic Commands:
      start          로컬 쿠버네티스 클러스터를 시작합니다
      status         로컬 쿠버네티스 클러스터의 상태를 가져옵니다
      stop           실행 중인 로컬 쿠버네티스 클러스터를 중지합니다
      delete         로컬 쿠버네티스 클러스터를 삭제합니다
    ```
    
- minikube version 을 확인합니다.
    
    ```bash
    minikube version
    ```
    

---

# 3. Let's Install Kubectl

- kubectl 은 kubernetes cluster (server) 에 요청을 간편하게 보내기 위해서 널리 사용되는 client 툴입니다.
- kubectl 은 v1.22.1 로 다운로드 받겠습니다.

```bash
curl -LO https://dl.k8s.io/release/v1.22.1/bin/linux/amd64/kubectl
```

- kubectl 바이너리를 사용할 수 있도록 권한과 위치를 변경합니다.

```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

- 정상적으로 설치되었는지 확인합니다.

```bash
kubectl --help
```

- 터미널에 다음과 같은 메시지가 한글 or 영어로 출력된다면 정상적으로 설치된 것입니다.
    
    ```bash
    kubectl controls the Kubernetes cluster manager.
    
     Find more information at:
    https://kubernetes.io/docs/reference/kubectl/overview/
    
    Basic Commands (Beginner):
      create        Create a resource from a file or from stdin
      expose        Take a replication controller, service, deployment or pod and
    expose it as a new Kubernetes service
      run           Run a particular image on the cluster
      set           Set specific features on objects
    ```
    
- kubectl version 을 확인합니다.

```bash
kubectl version
```

- 터미널에 다음과 같은 메시지가 한글 or 영어로 출력된다면 정상적으로 설치된 것입니다.
    
    ```bash
    Client Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.1", GitCommit:"632ed300f2c34f6d6d15ca4cef3d3c7073412212", GitTreeState:"clean", BuildDate:"2021-08-19T15:45:37Z", GoVersion:"go1.16.7", Compiler:"gc", Platform:"linux/amd64"}
    The connection to the server localhost:8080 was refused - did you specify the right host or port?
    ```
    
    - `The connection to the server localhost:8080 was refused - did you specify the right host or port?` 메시지는 에러를 의미하는 것이 맞습니다.
    - 하지만 `kubectl version` 은 client 의 버전과 kubernetes server 의 버전을 모두 출력하는 명령어이며, 현재 저희는 kubernetes server 를 생성하지 않았기 때문에 client 의 버전만 정상적으로 출력됩니다.

---

# 4. Minikube 시작하기

### minikube start

- minikube 를 docker driver 를 기반으로 하여 시작합니다.
    
    ```bash
    minikube start --driver=docker
    ```
    
- 다음과 같은 화면이 출력되며, 필요한 docker image 들을 다운받게 되고, 다운로드가 완료되면 이를 기반으로 minikube 를 구동합니다.
    
![minikube start1](https://user-images.githubusercontent.com/62507896/177148116-fde58744-9e9e-4d94-ab66-122e8f626864.png)
    
- 정상적으로 `minikube start` 가 완료되면 다음과 같은 메시지가 출력됩니다.

![minikube start2](https://user-images.githubusercontent.com/62507896/177148150-f33b1b0e-2ed1-4019-98b9-c09f5ab33e4a.png)


### minikube status

- 정상적으로 생성되었는지 minikube 의 상태를 확인해봅니다.

```bash
minikube status
```

- 터미널에 다음과 같은 메시지가 출력되어야 합니다.

```bash
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### kubectl get pod -n kube-system

- kubectl 을 사용하여 minikube 내부의 default pod 들이 정상적으로 생성되었는지 확인해봅니다.

```bash
kubectl get pod -n kube-system
```

- 터미널에 다음과 같은 메시지가 출력되어야 합니다.

```bash
NAME                               READY   STATUS    RESTARTS   AGE
coredns-558bd4d5db-bwkjv           1/1     Running   0          3m40s
etcd-minikube                      1/1     Running   0          3m46s
kube-apiserver-minikube            1/1     Running   0          3m46s
kube-controller-manager-minikube   1/1     Running   0          3m53s
kube-proxy-ppgbx                   1/1     Running   0          3m40s
kube-scheduler-minikube            1/1     Running   0          3m46s
storage-provisioner                1/1     Running   1          3m51s
```

---

# 5. Minikube 삭제하기

### minikube delete

- 다음 명령어로 간단하게 삭제할 수 있습니다.

```bash
minikube delete
```

- 터미널에 다음과 같은 메시지가 출력되어야 합니다.

```bash
🔥  docker 의 "minikube" 를 삭제하는 중 ...
🔥  Deleting container "minikube" ...
🔥  /home/kjy/.minikube/machines/minikube 제거 중 ...
💀  "minikube" 클러스터 관련 정보가 모두 삭제되었습니다
```

# 1. Pod 이란?

- Pod(파드)는 쿠버네티스에서 생성하고 관리할 수 있는 배포 가능한 **가장 작은 컴퓨팅 단위**입니다.
    - [https://kubernetes.io/ko/docs/concepts/workloads/pods/](https://kubernetes.io/ko/docs/concepts/workloads/pods/)
- 쿠버네티스는 Pod 단위로 스케줄링, 로드밸런싱, 스케일링 등의 관리 작업을 수행합니다.
    - 쿠버네티스에 어떤 애플리케이션을 배포하고 싶다면 최소 Pod 으로 구성해야 한다는 의미입니다.
- 조금 어렵다면 Pod 은 Container 를 감싼 개념이라고 생각할 수 있습니다.
    - 하나의 Pod 은 한 개의 Container 혹은 여러 개의 Container 로 이루어져있을 수 있습니다.
    - Pod 내부의 여러 Container 는 자원을 공유합니다.
- Pod 의 자세한 구조는 생략하겠습니다.
    - 다만 Pod 은 Stateless 한 특징을 지니고 있으며, 언제든지 삭제될 수 있는 자원이라는 점을 꼭 기억해주시기 바랍니다.

---

# 2. Pod 생성

- 간단한 Pod 의 예시입니다.

```yaml
apiVersion: v1 # kubernetes resource 의 API Version
kind: Pod # kubernetes resource name
metadata: # 메타데이터 : name, namespace, labels, annotations 등을 포함
  name: counter
spec: # 메인 파트 : resource 의 desired state 를 명시
  containers:
  - name: count # container 의 이름
    image: busybox # container 의 image
    args: [/bin/sh, -c, 'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done'] # 해당 image 의 entrypoint 의 args 로 입력하고 싶은 부분
```

- 위의 스펙대로 Pod 을 하나 생성해보겠습니다.

```bash
vi pod.yaml
# 위의 내용을 복사 후 붙여넣습니다.

kubectl apply -f pod.yaml 
```

- `kubectl apply -f <yaml-file-path>` 를 수행하면, `<yaml-file-path>` 에 해당하는 kubernetes resource 를 생성 또는 변경 할 수 있습니다.
    - kubernetes resource 의 desired state 를 기록해놓기 위해 항상 YAML 파일을 저장하고, 버전 관리하는 것을 권장합니다.
    - `kubectl run` 명령어로 YAML 파일 생성 없이 pod 을 생성할 수도 있지만, 이는 kubernetes 에서 권장하는 방식이 아니므로 생략하겠습니다.
- 생성한 Pod 의 상태를 확인합니다.

```bash
kubectl get pod
# ContainerCreating

kubectl get pod
# 시간이 지난 후 Running 으로 변하는 것을 확인할 수 있습니다.
```

---

# 3. Pod 조회

- 방금 current namespace 의 Pod 목록을 조회하는 명령을 수행하였습니다.
    - 조회 결과는 Desired state 가 아닌, **Current State** 를 출력합니다.

```bash
kubectl get pod
```

- **namespace** 란 ?
    - namespace 는 kubernetes 에서 리소스를 격리하는 가상의(논리적인) 단위
    - `kubectl config view --minify | grep namespace:` 로 current namespace 가 어떤 namespace 로 설정되었는지 확인할 수 있습니다.
        - 따로 설정하지 않았다면 `default` namespace 가 기본으로 설정되어 있을 것입니다.

- 특정 namespace 혹은 모든 namespace 의 pod 을 조회할 수도 있습니다.

```bash
kubectl get pod -n kube-system
# kube-system namespace 의 pod 을 조회합니다.

kubectl get pod -A
# 모든 namespace 의 pod 을 조회합니다.
```

- pod 하나를 조회하는 명령어는 다음과 같습니다.
    - `<pod-name>` 에 해당하는 pod 을 조회합니다.

```bash
kubectl get pod <pod-name>
```

- pod 하나를 조금 더 자세히 조회하는 명령어는 다음과 같습니다.
    - `<pod-name>` 에 해당하는 pod 을 자세히 조회합니다.

```bash
kubectl describe pod <pod-name>
```

- 기타 유용한 명령을 소개드리겠습니다.

```bash
kubectl get pod -o wide
# pod 목록을 보다 자세히 출력합니다.

kubectl get pod <pod-name> -o yaml
# <pod-name> 을 yaml 형식으로 출력합니다.

kubectl get pod -w
# kubectl get pod 의 결과를 계속 보여주며, 변화가 있을 때만 업데이트됩니다.
```

---

# 4. Pod 로그

- pod 의 로그를 확인하는 명령어는 다음과 같습니다.

```bash
kubectl logs <pod-name>

kubectl logs <pod-name> -f
# <pod-name> 의 로그를 계속 보여줍니다.
```

- pod 안에 여러 개의 container 가 있는 경우에는 다음과 같습니다.

```bash
kubectl logs <pod-name> -c <container-name>

kubectl logs <pod-name> -c <container-name> -f
```

---

# 5. Pod 내부 접속

- pod 내부에 접속하는 명령어는 다음과 같습니다.

```bash
kubectl exec -it <pod-name> -- <명령어>
```

- pod 안에 여러 개의 container 가 있는 경우에는 다음과 같습니다.

```bash
kubectl exec -it <pod-name> -c <container-name> -- <명령어>
```

- docker exec 과 비슷한 명령임을 확인할 수 있습니다.

---

# 6. Pod 삭제

- pod 을 삭제하는 명령어는 다음과 같습니다.

```bash
kubectl delete pod <pod-name>
```

- 혹은 다음과 같이 리소스를 생성할 때, 사용한 YAML 파일을 사용해서 삭제할 수도 있습니다.

```bash
kubectl delete -f <YAML-파일-경로>
```

- 위 명령어는 꼭 pod 이 아니더라도 모든 kubernetes resource 에 적용할 수 있습니다.

# 1. Deployment 란?

- Deployment(디플로이먼트)는 Pod와 Replicaset에 대한 **관리**를 제공하는 단위입니다.
    - [https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/](https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/)
- **관리**라는 의미는 Self-healing, Scaling, Rollout(무중단 업데이트) 과 같은 기능을 포함합니다.
- 조금 어렵다면 Deployment 는 Pod을 감싼 개념이라고 생각할 수 있습니다.
    - Pod 을 Deployment 로 배포함으로써 여러 개로 복제된 Pod, 여러 버전의 Pod 을 안전하게 관리할 수 있습니다.
- Deployment 의 자세한 구조는 생략하겠습니다.

---

# 2. Deployment 생성

- 간단한 Deployment 의 예시입니다.

```yaml
apiVersion: apps/v1 # kubernetes resource 의 API Version
kind: Deployment # kubernetes resource name
metadata: # 메타데이터 : name, namespace, labels, annotations 등을 포함
  name: nginx-deployment
  labels:
    app: nginx
spec: # 메인 파트 : resource 의 desired state 를 명시
  replicas: 3 # 동일한 template 의 pod 을 3 개 복제본으로 생성합니다.
  selector:
    matchLabels:
      app: nginx
  template: # Pod 의 template 을 의미합니다.
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx # container 의 이름
        image: nginx:1.14.2 # container 의 image
        ports:
        - containerPort: 80 # container 의 내부 Port
```

- 위의 스펙대로 Deployment 를 하나 생성해보겠습니다.

```bash
vi deployment.yaml
# 위의 내용을 복사 후 붙여넣습니다.

kubectl apply -f deployment.yaml
```

---

# 3. Deployment 조회

- 생성한 Deployment 의 상태를 확인합니다.

```bash
kubectl get deployment
# 다음과 같은 메시지가 출력됩니다.
# NAME               READY   UP-TO-DATE   AVAILABLE   AGE
# nginx-deployment   0/3     3            0           10s

kubectl get deployment,pod
```

- 시간이 지난 후, deployment 와 함께 3 개의 pod 이 생성된 것을 확인할 수 있습니다.

```bash
kubectl describe pod <pod-name>
```

- pod 의 정보를 자세히 조회하면 `Controlled By` 로부터 Deployment 에 의해 생성되고 관리되고 있는 것을 확인할 수 있습니다.

---

# 4. Deployment Auto-healing

- pod 하나를 삭제해보겠습니다.

```bash
kubectl delete pod <pod-name>
```

- 기존 pod 이 삭제되고, 동일한 pod 이 새로 하나 생성된 것을 확인할 수 있습니다.

```bash
kubectl get pod
```

---

# 5. Deployment Scaling

- replica 개수를 늘려보겠습니다.

```bash
kubectl scale deployment/nginx-deployment --replicas=5

kubectl get deployment

kubectl get pod
```

- replica 개수를 줄여보겠습니다.

```bash
kubectl scale deployment/nginx-deployment --replicas=1

kubectl get deployment

kubectl get pod
```

---

# 5. Deployment 삭제

- deployment 를 삭제합니다.

```bash
kubectl delete deployment <deployment-name>

kubectl get deployment

kubectl get pod
```

- Deployment 의 Control 을 받던 pod 역시 모두 삭제된 것을 확인할 수 있습니다.
- 혹은 `-f` 옵션으로 YAML 파일을 사용해서 삭제할 수도 있습니다.

```bash
kubectl delete -f <YAML-파일-경로>
```

# 1. Service 란?

- Service 는 쿠버네티스에 배포한 애플리케이션(Pod)을 외부에서 접근하기 쉽게 추상화한 리소스입니다.
    - [https://kubernetes.io/ko/docs/concepts/services-networking/service/](https://kubernetes.io/ko/docs/concepts/services-networking/service/)
- Pod 은 IP 를 할당받고 생성되지만, 언제든지 죽었다가 다시 살아날 수 있으며, 그 과정에서 IP 는 항상 재할당받기에 고정된 IP 로 원하는 Pod 에 접근할 수는 없습니다.
- 따라서 클러스터 외부 혹은 내부에서 Pod 에 접근할 때는, Pod 의 IP 가 아닌 Service 를 통해서 접근하는 방식을 거칩니다.
- Service 는 고정된 IP 를 가지며, Service 는 하나 혹은 여러 개의 Pod 과 매칭됩니다.
- 따라서 클라이언트가 Service 의 주소로 접근하면, 실제로는 Service 에 매칭된 Pod 에 접속할 수 있게 됩니다.

---

# 2. Service 생성

- 우선 지난 시간에 생성한 Deployment 를 다시 생성합니다.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

- `kubectl apply -f deployment.yaml`

- 생성된 Pod 의 IP 를 확인하고 접속을 시도해봅니다.

```bash
kubectl get pod -o wide
# Pod 의 IP 를 확인합니다.

curl -X GET <POD-IP> -vvv
ping <POD-IP>
# 통신 불가능
```

- 할당된 <POD-IP> 는 클러스터 내부에서만 접근할 수 있는 IP 이기 때문에 외부에서는 Pod 에 접속할 수 없습니다.

- minikube 내부로 접속하면 통신이 되는지 확인해보겠습니다.

```bash
minikube ssh
# minikube 내부로 접속합니다.

curl -X GET <POD-IP> -vvv
ping <POD-IP>
# 통신 가능
```

- 그럼 이제 위의 Deployment 를 매칭시킨 Service 를 생성해보겠습니다.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  type: NodePort # Service 의 Type 을 명시하는 부분입니다. 자세한 설명은 추후 말씀드리겠습니다.
  ports:
  - port: 80
    protocol: TCP
  selector: # 아래 label 을 가진 Pod 을 매핑하는 부분입니다.
    app: nginx 
```

- Service 를 생성합니다.

```bash
vi service.yaml
# 파일을 열어 위의 내용을 복사 붙여넣기 합니다.

kubectl apply -f service.yaml

kubectl get service
# PORT 80:<PORT> 숫자 확인

curl -X GET $(minikube ip):<PORT>
# 이렇게 서비스를 통해서 클러스터 외부에서도 정상적으로 pod 에 접속할 수 있는 것을 확인합니다.
```

* ***Service 의 Type 이란?***
    - **NodePort** 라는 type 을 사용했기 때문에, minikube 라는 kubernetes cluster 내부에 배포된 서비스에 클러스터 외부에서 접근할 수 있었습니다.
        - 접근하는 IP 는 pod 이 떠있는 노드(머신)의 IP 를 사용하고, Port 는 할당받은 Port 를 사용합니다.
    - **LoadBalancer** 라는 type 을 사용해도, 마찬가지로 클러스터 외부에서 접근할 수 있지만, LoadBalancer 를 사용하기 위해서는 LoadBalancing 역할을 하는 모듈이 추가적으로 필요합니다.
    - **ClusterIP** 라는 type 은 고정된 IP, PORT 를 제공하지만, 클러스터 내부에서만 접근할 수 있는 대역의 주소가 할당됩니다.
    - **실무**에서는 주로 kubernetes cluster 에 MetalLB 와 같은 LoadBalancing 역할을 하는 모듈을 설치한 후, **LoadBalancer** type 으로 서비스를 expose 하는 방식을 사용합니다.
        - NodePort 는 Pod 이 어떤 Node 에 스케줄링될 지 모르는 상황에서, Pod 이 할당된 후 해당 Node 의 IP 를 알아야 한다는 단점이 존재합니다.
