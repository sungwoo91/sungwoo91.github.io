Google StudyJam 에서 제공하는 Google Cloud 강좌를 듣고 정리한 글입니다.
이번 코스는 coursera 에서 강의를 들을 수 있고 총 4파트로 구분된다.


# Introduction to Containers and Docker
* Container

컨테이너 개념에 대해서는 [구글 클라우드 문서](https://cloud.google.com/containers/?authuser=0&hl=ko)에서 쉽게 설명 하고 있습니다.

*궁금한점 : 컨테이너 기술에서는 어플리케이션과 디펜던시만 컨테이너로 배포하고 커널과 컨테이너런타임은 공유? 하는건가
공유 한다면 어떻게 같은 컨테이너를 공유하는 커널에서 같이 실행 할 수 있지?
공유 안한다면 계속 같은 커널을 만들어내는건가?*
> 커널은 도커 엔진에서 제공하는 커널을 공유 하는데, 리눅스 os 는 베이스 이미지를 토대로 다시 띄운다.
예를 들어 ubuntu 위에서 nginx를 띄울 때 리눅스 커널은 공유하고 ubuntu 가 커널을 제외하고 갖는 라이브러리나 특정 파일만을 이미지로 받아오게 된다.

* Docker

도커의 개념은 [도커 공식 홈페이지](https://www.docker.com/why-docker)를 참고하면 된다.

간단하게 컨테이너 기술을 쉽게 사용 할 수 있게 해주는 기술이라고 생각하면 될 듯하다.

# Kubernetes Basics
이런 컨테이너들을 관리해주는 기술이 쿠버네티스다.

* Clusters : 쿠버네티스에서 관리하는 대상들의 묶음이 클러스터이다. master와 node로 이루어져 있고 master에서 node들을 관리하고 
node에서는 kubelet을 통해 pod들의 상태를 관리하고 장애 발생시 재시작 해준다.
* Pods : 노드안에서 실행되는 컨테이너들의 집합이다. 쿠버네티스에서 관리하는 최소 단위이고, 네트워킹과 스토리지를 공유한다. pod에 대한 정보를 yaml 파일로 작성하여 마스터에서 실행하면 
쿠버네티스가 알아서 노드에 pod을 실행 시켜 준다.
* Deployments : yaml 파일에 deployment로 어떤 pod을 얼마나 실행 시킬지를 설정해 놓으면 마스터 관리하면서 pod상태를 유지시켜 준다.
* Services : pod들에 고정 IP를 할당하는 역할을 한다. 서로다른 Pods 이나 Services 끼리 통신을 할수 있게 된다.
* Labels : pod에 이름을 지어주어 관리하기 쉽게 해준다. 같은 이름끼리 골라서 실행하거나 관리 할 수 있다.
* Selectors : Selector가 같은 이름인 pod들을 골라준다.
* Volumes : Pods안에 있는 컨테이너들이 공유하는 디스크 공간이다.

# Deploying to Kubernetes
쿠버네티스의 핵심인 자동으로 클러스터에 pod을 배포해주고 관리하는 기술들을 다룬다.
Delployments yaml 파일에 ReplicaSet 이란 항목에서 pod의 개수와 app의 종류를 지정 할 수 있다.
또 설정만으로 pod의 개수를 늘리거나 줄일 수 있다.

Rolling Update란 개념은 이미 클러스터에서 동작중인 컨테이너의 버전을 올릴 때 설정만으로 자동으로 업데이트 되는 걸 말한다.
이 과정은 두개의 ReplicaSet을 만들면 자동으로 이전 버전의 pod을 하나씩 새로운 버전으로 올려주는 걸 말한다.
하나 띄우고 이전 버전 하나 내리는 식으로..!
이렇게 되면 무중단배포가 가능하다.

Canary and Blue-Green Deployments 두 가지 방법이 있는데 
Canary는 위에서 말한거처럼 하나 띄우고 하나 내리고 하는 식으로 업데이트를 하는 거다. 
그래서 로드밸런서를 이용해서 두 가지 버전을 동시에 이용 하게 해줘야 한다.
Blue-Green은 새로운 버전을 다 띄워놓고 한번에 업데이트 하는 식이다.

# Creating a Continuous Delivery Pipeline
쿠버네티스에 배포 자동화 하는 것은 Jenkins나 Spinnaker로 할 수 있다.
기존에 Jenkins를 이용하여 소스 배포 자동화를 할 때는 Github같은 곳에 연동을 시켜 놨었는데, 클라우드 상에 컨테이너를 배포 할 때는
Docker Hub나 구글 컨테이너 레지스트리를 연동 시켜 놓으면 이미지가 바뀔 때 마다 자동으로 배포 하게 할 수 있다.
