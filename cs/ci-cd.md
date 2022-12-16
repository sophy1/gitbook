# CI/CD

### 등장 배경

* 여러 사람이 개발할 때 코드의 안정성, 동일한 실행 환경 구성은 굉장히 중요하다.
* 하지만, Git repository 1개를 사용하더라도 코드 공유 및 개발시 발생할 수 있는 문제점은 쉽게+많이 발생한다. 예를 들어, 환경 변수 변경, 코드 수정, Git confict 등. &#x20;
* 실수를 방지하기 위해 TC 작성하지만 매 PR 리뷰 때마다 각 리뷰어들이 일일히 빌드하고, TC를 돌려보며 리뷰하면 생산성이 저하된다.
* 리뷰어들이 직접 확인하지 않고, TC pass을 확실히 보장해주는 수단이 필요하여 CI/CD 등장하게 된다.
* 특히 MSA 구조에서는 app module(repository)도 수십 개가 있고 (물론 branch도 n개) 서로 dependency가 있기 때문에, 효율적인 CI/CD 구성은 필수불가결하다.

### CI/CD

* CI (Continuous Integration)
  * "지속적 통합" 으로 여러 개발자가 하나의 프로젝트를 같이 개발할 때 발생하는 불일치를 최소화 해주는 개념
  * 소스코드 변경 사항 발생시, 자동으로 빌드 및 테스트 트리거되어, 잘못된 코드가 공유되는 걸 방지
  * 자동 빌드 및 테스트를 진행하여 여러 개발자들이 공유하는 코드의 신뢰성을 높임
* CD (Continuous Deployment)
  * "지속적 배포" 으로 프로젝트의 변경 사항을 가상 환경에 자동으로 배포
  * CD 를 구성해두면 변경 사항을 배포할 때 사용하는 파이프라인을 공유하고 자동화
  * 배포 파이프라인을 자동화하여 누구나 동일한 플로우로 배포 할 수 있게 한다

### Docker Container

* 컨테이너 서비스는 복잡하고 어려운 환경 구성 없이 필요한 라이브러리나 구성 요소를 패키지화하여 간단한 동작만으로도 앱 구동이 가능하게 한다.&#x20;
  * 기존에 하이퍼바이저 기반 가상화에 비해 경량화됨
  * 개발자는 컨테이너 기반의 앱 개발하고, devops에서는 운영 및 image 배포를 담당

> * 컨테이너 image 생성을 위한 Dockerfile 작성도 작성할 줄 알아야.. redii 컨테이너 이미지 저장소 PATH 오타 내지 말자
> * 인프라 정보를 확인하여, branch-overlay 설정을 보고 branch를 생성하고 단위 개발환경에 앱을 배포하기 위해서 해당 branch에 development branch 와 동기화한다.
> * 운영계에 배포하면, 해당 형상은 최대한 유지한다. 하지만 hotfix patch 작업은 자주 발생하지.. hotfix branch는 development branch에도 꼭 merge할 것

* 컨테이너를 구동하고 관리하는 역할은 컨테이너 엔진(Docker, CRI-O, Containered)
* 컨테이너 엔진 위에 컨테이너가 수없이 증가함에 따라, 오케스트레이션 툴을 사용하여 관리
* 컨테이너 오케스트레이션 툴 (k8s, Docker Swarm, Apache mesos..)
  * 자동화된 컨테이너의 배포, 관리, 확장 네트워킹 기능 제공
  * k8s는 수 많은 컨테이너를 논리적인 단위로 그룹핑, 통합 관리할 수 있는 기능 제공
  * k8s namespace 에 대한 배포 정의를 위해 폴더 구조와 manifest파일이 생성되면, 거기에 나는 application.yaml 파일로 설정 관리 및 유지 보수

#### 파이프라인 동작 과정

> 단위 개발 환경에 커밋 전에 k8s 설정이 올바른지, DDL/DML 쿼리 실행했는지 확인

1. \[개발자] branch에 GitHub 코드 커밋
2. Jenkins에 등록한 job 시작 (CI shellscript 실행)
   1. base image에서 Git clone
   2. 소스코드 build, test => \[개발자] build/test fail이면 PR 페이지에 등록되거나 메일이 옴..&#x20;
   3. 생성된 image 를 이미지 저장소에 push
3. 해당 k8s namespace의 배포 정의서(groovy 파일)에 따라서 등록된 image를 k8s에 배포
   1. argoCD에서 sync 버튼을 누르거나 auto-sync면 pod이 새로 생성되면서 배포
   2. \[개발자] pod의 상태와 log를 확인한다.&#x20;



### Ref

[https://www.redhat.com/ko/topics/devops/what-is-ci-cd](https://www.redhat.com/ko/topics/devops/what-is-ci-cd)
