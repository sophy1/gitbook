# Docker

### Command

1. docker run: 컨테이너 이미지로 컨테이너 실행
   * $ docker run --name my-container -p 8080:8080 -d my-container-image
   * `--name <name>`: 도커 컨테이너 이름을 설정
   * `-d`: 백그라운드로 실행
   * `-p <local port>:<container port>`
     * 특정 포트로 연결
     * 로컬 머신의 8080 포트를 컨테이너 내부의 8080 포트와 매핑
     * `http://localhost:8080` 으로 애플리케이션에 접근 가능
2. docker exec
   * 컨테이너에 (bash) command 명령어를 전달하여 실행(excute)
   * $ docker exec my\_container echo "hello world!"
     * my\_container 컨테이너에 echo 명령어 실행
   * $ docker exec -it my\_container bash
     * my\_container 컨테이너의 bash 접속
     * `i` 옵션을 빼면 명령어를 입력할 수 없고, `t` 옵션을 빼면 명령어 프롬포트가 화면에 표시되지 않는다.
     * `-i` : 표준 입력(STDIN) 을 오픈 상태로 유지. 셸에 명령어를 입력하기 위해 필요.
     * `-t` : 의사 (PSEUDO) 터미널 (TTY) 을 할당
   * $ docker images
     * docker 이미지 목록 조회
   * $ docker rmi my-docker-image
     * docker 이미지 삭제

### Dockerfile

### docker-compose.yml

### Ref

[https://docs.docker.com/engine/reference/commandline/docker/](https://docs.docker.com/engine/reference/commandline/docker/)

