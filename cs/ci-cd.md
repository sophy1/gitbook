# CI/CD



* 컨테이너 서비스는 복잡하고 어려운 환경 구성 없이 필요한 라이브러리나 구성 요소를 패키지화하여 간단한 동작만으로도 앱 구동이 가능하게 한다.&#x20;
  * 기존에 하이퍼바이저 기반 가상화에 비해 경량화됨
  * 개발자는 컨테이너 기반의 앱 개발하고, devops에서는 운영 및 image 배포를 담당

> * 컨테이너 image 작성도 작성할 줄 알아야.. redii 컨테이너 이미지 저장소 PATH 오타 내지 말자
> * 인프라 정보를 확인하여, branch-overlay 설정을 보고 branch를 생성하고 단위 개발환경에 앱을 배포하기 위해서 해당 branch에 development branch 와 동기화한다.
> * 운영계에 배포하면, 해당 형상은 최대한 유지한다. 하지만 hotfix patch 작업은 늘 있지 -\_-

* 컨테이너를 구동하고 관리하는 역할은 컨테이너 엔진( Docker, CRI-O, Containered)
* 컨테이너 엔진 위에 컨테이너가 수없이 증가함에 따라, 오케스트레이션 툴을 사용하여 관리
* 컨테이너 오케스트레이션 툴 (k8s, Docker Sarm, Apache mesos..)
  * 자동화된 컨테이너의 배포, 관리, 확장 네트워킹 기능 제공
  * k8s는 수 많은 컨테이너를 논리적인 단위로 그룹핑, 통합 관리할 수 있는 기능 제공
  * k8s namespace 에 대한 배포 정의를 위해 폴더 구조와 manifest파일이 생성되면, 거기에 나는 application.yaml 파일로 설정 관리 및 유지 보수

#### 파이프라인 동작 과정

1. \[개발] branch에 GitHub 코드 커밋
2. Jenkins에 등록한 job 시작
   1. base image에서 Git clone
   2. 소스코드 build, test => \[개발자] build/test fail이면 PR 페이지에 등록되거나 메일이 옴..&#x20;
   3. 생성된 image 를 이미지 저장소에 push
3. 해당 k8s namespace의 배포 정의서(groovy 파일)에 따라서 등록된 image를 k8s에 배포
   1. argoCD에서 sync 버튼을 누르거나 auto-sync면 pod이 새로 생성되면서 배포
   2. \[개발자] pod의 상태와 log를 확인한다.&#x20;

