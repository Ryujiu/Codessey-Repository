# 🚀 Docker 개발 환경 구축 및 웹 서버 배포 과제

1. 프로젝트 개요
미션 목표: Docker를 활용한 개발 환경 구축, 리눅스 명령어 숙달, Dockerfile을 이용한 커스텀 웹 서버 배포 및 데이터 영속성 검증.
핵심 수행 내용: CLI 기초, 권한 관리, Docker 운영, Dockerfile 빌드, 볼륨 마운트, Git/GitHub 연동.

2. 실행 환경
항목	내용
OS	Windows 11 (WSL2)
Shell/Terminal 	PowerShell / Ubuntu Bash
Docker 버전	[Docker version29.9.2, build a82e528]
Git 버전 회사 PC	[git version 2.56.0.windows.4] / 집 PC [git version 2.47.1.windows.1]

3. 수행 항목 체크리스트
터미널 기본 명령어 실습
파일 및 디렉토리 권한 변경 실습
Docker 설치 및 기본 점검
Docker 이미지 및 컨테이너 운영 명령 수행
Dockerfile 기반 커스텀 이미지 제작 및 배포
포트 매핑 및 브라우저 접속 확인
Docker 볼륨을 이용한 데이터 영속성 검증
GitHub 저장소 연동 및 푸시

4. 터미널 조작 및 권한 실습 로그
4.1. 기본 명령어 수행

## 현재 위치 확인
`pwd`

```
PS C:\Users\knuser\Desktop\Workspace> pwd

Path                             
----                             
C:\Users\knuser\Desktop\Workspace
```

## 목록 확인 
`ls`

```
PS C:\Users\knuser\Desktop\Workspace> ls


    디렉터리: C:\Users\knuser\Desktop\Workspace


Mode                 LastWriteTime         Length Name                                                                                                                                                                   
----                 -------------         ------ ----                                                                                                                                                                   
-a----      2026-11-05   오후 2:30           4604 readme.md   
```

## 숨김파일 포함한 목록 확인
`ls -al`
9.트러블슈팅 Case2로 인해 Ryu_linux 컨테이너 생성 후 명령어 실습 재개

```
root@6efc1a9039b7:/home/Workspace# ls -al
total 12
drwxrwxrwx 1 root root  512 Nov  5 05:30 .
drwxr-xr-x 1 root root 4096 Nov  5 06:48 ..
-rwxrwxrwx 1 root root 4604 Nov  5 05:30 readme.md
root@6efc1a9039b7:/home/Workspace# 
```

## 디렉토리 생성 및 이동
`mkdir workspace` & `cd workspace`
testfolder 디렉토리 생성 및 이동

```
root@6efc1a9039b7:/home/Workspace# mkdir testfolder
root@6efc1a9039b7:/home/Workspace# cd testfolder
```

## 빈 파일 생성, 내용 입력 및 내용 확인
`touch`
`echo`
`cat`

```
root@6efc1a9039b7:/home/Workspace/testfolder# touch memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# echo "Hello?" > memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# cat memo.txt
Hello?
```

## 파일 복사 및 이름 변경
`cp` 
`mv`

```
root@6efc1a9039b7:/home/Workspace/testfolder# cp memo.txt memo_copy.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls
memo.txt  memo_copy.txt
root@6efc1a9039b7:/home/Workspace/testfolder# mv memo_copy.txt Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls
Hello.txt  memo.txt
```

## 삭제
`rm`
```
root@6efc1a9039b7:/home/Workspace/testfolder# rm memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls
Hello.txt
```

4.2. 권한(Permission) 실습
대상: note.txt (파일), test_dir (디렉토리)
변경 전/후 비교:
## 파일 권한 변경 (644 -> 755)
`chmod`
```
root@6efc1a9039b7:/home/Workspace/testfolder# ls -l Hello.txt
-rw-r--r-- 1 root root 7 Nov  5 07:02 Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# chmod 755 Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls -l Hello.txt
-rwxr-xr-x 1 root root 7 Nov  5 07:02 Hello.txt
```

## 디렉토리 권한 변경 (755 -> 700)
`chmod`
```
root@6efc1a9039b7:/home/Workspace/testfolder# mkdir permission test 
root@6efc1a9039b7:/home/Workspace/testfolder# ls
Hello.txt  permission  test
root@6efc1a9039b7:/home/Workspace/testfolder# rm -r test
root@6efc1a9039b7:/home/Workspace/testfolder# ls -ld permission
drwxr-xr-x 1 root root 512 Nov  5 07:09 permission
root@6efc1a9039b7:/home/Workspace/testfolder# chmod 700 permission
root@6efc1a9039b7:/home/Workspace/testfolder# ls -ld permission
drwx------ 1 root root 512 Nov  5 07:09 permission
```

5. Docker 운영 및 컨테이너 실습
5.1. Docker 기본 점검
`docker version`
```
PS C:\Users\knuser\Desktop\Workspace> docker --version
Docker version 29.9.2, build a82e528
```

`docker info`
```
PS C:\Users\knuser\Desktop\Workspace> docker info
Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.152.0
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v5.6.1
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.98.0
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-desktop.exe
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.9
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-dhi.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-extension.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-init.exe
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.2
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-mcp.exe
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-model.exe
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.35
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-offload.exe
  pass: Docker Pass Secrets Manager Plugin (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-pass.exe
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-sandbox.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.27.0
    Path:     C:\Users\knuser\.docker\cli-plugins\docker-scout.exe

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 1
 Server Version: 29.9.2
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: local
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Discovered Devices:
  cdi: docker.com/gpu=webgpu
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 nvidia runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b7a25261e029d68978bbf290df0a62fc819b3c51
 runc version: v1.5.2-0-g29dd3dc2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.18.54.1-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 4
 Total Memory: 9.655GiB
 Name: docker-desktop
 ID: eb0fa6f0-4934-41f4-813a-10211133eff5
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables
 ```
 
`docker images` 이미지 다운로드/목록 확인
  ```
 PS C:\Users\knuser\Desktop\Workspace> docker images
                                                                                                                                                                                                     i Info →   U  In Use
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
ubuntu:22.04   076e7bf82319        119MB         31.7MB    U  
```
이전 터미널 조작, 권한 실습에 사용했던 ubuntu:22.04 이미지 확인

`docker ps -a` 컨테이너 목록 확인
```
PS C:\Users\knuser\Desktop\Workspace> docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS                       PORTS     NAMES
6efc1a9039b7   ubuntu:22.04   "/bin/bash"   44 minutes ago   Exited (127) 5 minutes ago             ryu_linux
```
터미널 조작, 권한 실습에 사용한 ryu_linux 컨테이너 확인

`docker logs` 로그 확인
```
PS C:\Users\knuser\Desktop\Workspace> docker logs ryu_linux
root@6efc1a9039b7:/# ls -al
total 56
drwxr-xr-x   1 root root 4096 Nov  5 06:48 .
drwxr-xr-x   1 root root 4096 Nov  5 06:48 ..
-rwxr-xr-x   1 root root    0 Nov  5 06:48 .dockerenv
lrwxrwxrwx   1 root root    7 Oct 20 17:07 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Apr 18  2022 boot
drwxr-xr-x   5 root root  360 Nov  5 06:48 dev
drwxr-xr-x   1 root root 4096 Nov  5 06:48 etc
drwxr-xr-x   1 root root 4096 Nov  5 06:48 home
lrwxrwxrwx   1 root root    7 Oct 20 17:07 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Oct 20 17:07 lib32 -> usr/lib32
lrwxrwxrwx   1 root root    9 Oct 20 17:07 lib64 -> usr/lib64
lrwxrwxrwx   1 root root   10 Oct 20 17:07 libx32 -> usr/libx32
drwxr-xr-x   2 root root 4096 Oct 20 17:07 media
drwxr-xr-x   2 root root 4096 Oct 20 17:07 mnt
drwxr-xr-x   2 root root 4096 Oct 20 17:07 opt
dr-xr-xr-x 147 root root    0 Nov  5 06:48 proc
drwx------   2 root root 4096 Oct 20 17:15 root
drwxr-xr-x   5 root root 4096 Oct 20 17:15 run
lrwxrwxrwx   1 root root    8 Oct 20 17:07 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Oct 20 17:07 srv
dr-xr-xr-x  12 root root    0 Nov  5 06:48 sys
drwxrwxrwt   2 root root 4096 Oct 20 17:15 tmp
drwxr-xr-x  14 root root 4096 Oct 20 17:07 usr
drwxr-xr-x  11 root root 4096 Oct 20 17:15 var
root@6efc1a9039b7:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@6efc1a9039b7:/# cd home
root@6efc1a9039b7:/home# ls -al
total 8
drwxr-xr-x 1 root root 4096 Nov  5 06:48 .
drwxr-xr-x 1 root root 4096 Nov  5 06:48 ..
drwxrwxrwx 1 root root  512 Nov  5 05:30 Workspace
root@6efc1a9039b7:/home# cd workspace
bash: cd: workspace: No such file or directory
root@6efc1a9039b7:/home# cd Workspace
root@6efc1a9039b7:/home/Workspace# ls
readme.md
root@6efc1a9039b7:/home/Workspace# ls -al
total 12
drwxrwxrwx 1 root root  512 Nov  5 05:30 .
drwxr-xr-x 1 root root 4096 Nov  5 06:48 ..
-rwxrwxrwx 1 root root 4604 Nov  5 05:30 readme.md
root@6efc1a9039b7:/home/Workspace# mkdir testfolder
root@6efc1a9039b7:/home/Workspace# cd testfolder
root@6efc1a9039b7:/home/Workspace/testfolder# touch memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# cat memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# echo "Hello?" > memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# cat memo.txt
Hello?
root@6efc1a9039b7:/home/Workspace/testfolder# cp memo.txt memo_copy.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls
memo.txt  memo_copy.txt
root@6efc1a9039b7:/home/Workspace/testfolder# mv memo_copy.txt Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls
Hello.txt  memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# rm memo.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls
Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls -l Hello.txt
-rw-r--r-- 1 root root 7 Nov  5 07:02 Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# chmod Hello.txt 755
chmod: invalid mode: 'Hello.txt'
Try 'chmod --help' for more information.
root@6efc1a9039b7:/home/Workspace/testfolder# chmod 755 Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# ls -l Hello.txt
-rwxr-xr-x 1 root root 7 Nov  5 07:02 Hello.txt
root@6efc1a9039b7:/home/Workspace/testfolder# mkdir permission test 
root@6efc1a9039b7:/home/Workspace/testfolder# rm test
rm: cannot remove 'test': Is a directory
root@6efc1a9039b7:/home/Workspace/testfolder# rm -r test
root@6efc1a9039b7:/home/Workspace/testfolder# ls -l permission
total 0
root@6efc1a9039b7:/home/Workspace/testfolder# ls -al permission
total 0
drwxr-xr-x 1 root root 512 Nov  5 07:09 .
drwxr-xr-x 1 root root 512 Nov  5 07:12 ..
root@6efc1a9039b7:/home/Workspace/testfolder# -ls ls permission
bash: -ls: command not found
root@6efc1a9039b7:/home/Workspace/testfolder# ls -ld permission
drwxr-xr-x 1 root root 512 Nov  5 07:09 permission
root@6efc1a9039b7:/home/Workspace/testfolder# mkdir test
root@6efc1a9039b7:/home/Workspace/testfolder# ls 
Hello.txt  permission  test
root@6efc1a9039b7:/home/Workspace/testfolder# ls -l
total 0
-rwxr-xr-x 1 root root   7 Nov  5 07:02 Hello.txt
drwxr-xr-x 1 root root 512 Nov  5 07:09 permission
drwxr-xr-x 1 root root 512 Nov  5 07:15 test
root@6efc1a9039b7:/home/Workspace/testfolder# chmod 700 permission
root@6efc1a9039b7:/home/Workspace/testfolder# ls -ld permission
drwx------ 1 root root 512 Nov  5 07:09 permission
root@6efc1a9039b7:/home/Workspace/testfolder# docker --version
bash: docker: command not found
root@6efc1a9039b7:/home/Workspace/testfolder# exit
exit
```
이전 실습에서 사용한 명령어 로그들 확인 완료

`docker stats` 리소스 확인
```
PS C:\Users\knuser\Desktop\Workspace> docker stats ryu_linux

CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
6efc1a9039b7   ryu_linux   0.00%     0B / 0B             0.00%     0B / 0B   0B / 0B     0
```
현재 도커에서 퇴장한 상태이기에 멈춰있는 것 확인

## (출력 결과 중 핵심 내용 일부 복사/붙여넣기)
5.2. 컨테이너 실행 실습
hello-world 실행: docker run hello-world (성공 로그 확인)
```
PS C:\Users\knuser\Desktop\Workspace> docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

ubuntu 컨테이너 내에서 ls, echo 등 간단 명령 수행
`docker run -it --name my-ubuntu ubuntu:latest /bin/bash`
```
PS C:\Users\knuser\Desktop\Workspace> docker run -it --name ubuntu-container ubuntu:latest /bin/bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
f3f25b84a7d7: Pull complete 
edafa32f5d9e: Pull complete 
21594acc2d8b: Download complete 
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest
```

`ls`
```
root@5d197c6486c5:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

`echo hi` - 입력값이 바로출력
```
root@5d197c6486c5:/# echo hi
hi
```

`echo hello > hello.txt` - 입력값이 입력된 파일 생성
```
root@5d197c6486c5:/# echo hello >  hello.txt
root@5d197c6486c5:/# ls
bin  boot  dev  etc  hello.txt  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@5d197c6486c5:/# cat hello.txt
hello
```

## 내부에서 ls, echo "hello" 수행
attach vs exec 차이점: ctrl + p

`docker run -it -d --name test ubuntu:22.04`
로 exit하여도 컨테이너 멈추지 않도록 컨테이너 생성

`exec`로 해당컨테이너 접속 후 hello 파일 생성 exit 후 컨테이너 up 상태인 것을 확인
```
PS C:\Users\USER\OneDrive\Desktop\workspace\Codessey-Repository> docker exec -it test /bin/bash
root@bccdf62cbad6:/# touch hello
root@bccdf62cbad6:/# ls
bin  boot  dev  etc  hello  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@bccdf62cbad6:/# exit
exit

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug test
    Learn more at https://docs.docker.com/go/debug-cli/
PS C:\Users\USER\OneDrive\Desktop\workspace\Codessey-Repository> docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
bccdf62cbad6   ubuntu:22.04   "/bin/bash"   3 minutes ago   Up 3 minutes             test
```

`attach`로 컨테이너 접속 후 exit로 나갈시 컨테이너 사라진 것 확인
```
PS C:\Users\USER\OneDrive\Desktop\workspace\Codessey-Repository> docker attach test
root@bccdf62cbad6:/# ls
bin  boot  dev  etc  hello  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@bccdf62cbad6:/# exit
exit
PS C:\Users\USER\OneDrive\Desktop\workspace\Codessey-Repository> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

attach: 실행 중인 컨테이너의 표준 입력/출력에 직접 연결 (종료 시 컨테이너도 멈출 수 있음).
exec: 실행 중인 컨테이너에 새로운 프로세스를 실행 (작업 후 나와도 컨테이너는 계속 실행됨).

6. 커스텀 Dockerfile 제작 및 웹 서버 배포
6.1. 커스텀 설정 내용
베이스 이미지: ubuntu:22.04
커스텀 목적: 실습 목적에 ubuntu가 적합하다 판단하여 ubuntu 기반에서 nginx 인스톨
환경변수 설정: 관리자 메일 환경변수로 설정
커스텀 목적: 환경변수 실습

6.2. 웹서버 소스코드 및 Dockerfile 코드

웹서버 소스코드
index.html
```
<h1>Gyeongnam Campus Docker Study</h1>
<p>Admin: MY_EMAIL</p>
```

dockerfile
```
FROM ubuntu:22.04

RUN apt-get update && apt-get install -y nginx curl

ENV CAMPUS_NAME="Gyeongnam"

COPY index.html /var/www/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```


6.3. 빌드 및 실행 결과
## 빌드 명령
docker build -t my-web-server .
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker build -t my-web-server .
[+] Building 26.9s (9/9) FINISHED                                                                                                                                            docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                         0.0s
 => => transferring dockerfile: 315B                                                                                                                                                         0.0s
 => [internal] load metadata for docker.io/library/ubuntu:22.04                                                                                                                              2.5s
 => [internal] load .dockerignore                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                              0.0s
 => [1/4] FROM docker.io/library/ubuntu:22.04@sha256:076e7bf82319619f28dd0aa9c02ab34b055ad37100395b4f49a2ffba04fb5e11                                                                        2.5s
 => => resolve docker.io/library/ubuntu:22.04@sha256:076e7bf82319619f28dd0aa9c02ab34b055ad37100395b4f49a2ffba04fb5e11                                                                        0.0s
 => => sha256:0626d3cc316c9fea3d4891e2ec19b8f371fe22ffb915622f6072a2ee2ca9017f 29.75MB / 29.75MB                                                                                             2.1s
 => => extracting sha256:0626d3cc316c9fea3d4891e2ec19b8f371fe22ffb915622f6072a2ee2ca9017f                                                                                                    0.3s
 => [internal] load build context                                                                                                                                                            0.1s
 => => transferring context: 169B                                                                                                                                                            0.0s
 => [2/4] RUN apt-get update && apt-get install -y nginx curl                                                                                                                               17.8s
 => [3/4] COPY index.html /var/www/html/index.html                                                                                                                                           0.1s
 => [4/4] RUN sed -i "s/MY_EMAIL/fbwldn1004@naver.com/g" /var/www/html/index.html                                                                                                            0.2s 
 => exporting to image                                                                                                                                                                       3.7s 
 => => exporting layers                                                                                                                                                                      2.7s 
 => => exporting manifest sha256:1109a86f1d28018411764a1218e3b0c57e083200283f7a3e77be3aef4ec32cce                                                                                            0.0s 
 => => exporting config sha256:82a46f8eec46a6704bedffbe9ca70137ecb0f786a1b86799d610773206ee1780                                                                                              0.0s 
 => => exporting attestation manifest sha256:b0a1a719bd979a261de911a99f46b19360dd0471ecbcc0ad8b7037f25bdf288c                                                                                0.0s
 => => exporting manifest list sha256:2a92c94b65fb1d727dcf2fb7e6be187307719410985c313d65d81ef4060dea15                                                                                       0.0s
 => => naming to docker.io/library/my-web-server:latest                                                                                                                                      0.0s
 => => unpacking to docker.io/library/my-web-server:latest                     
 ```

## 실행 명령 (포트 8080 매핑)
docker run -d -p 8080:80 --name my-web-container my-web-server
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker run -d -p 8080:80 --name my-web-container my-web-server
1ff570bf89f2a35e149c463e3d3e29b1ad671c8eb2c9f4e0e6df79ae4b7239dc
```
접속 확인: localhost:8080 curl 결과 및 접속 스크린샷 첨부
```
StatusCode        : 200
StatusDescription : OK
Content           : <h1>Gyeongnam Campus Docker Study</h1>
                    <p>Admin: envtest@email.com</p>
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 71
                    Content-Type: text/html
                    Date: Sat, 07 Nov 2026 04:36:10 GMT
                    ETag: "6aeeaa24-47"
                    Last-Modified: Sat, 07 Nov 2026 04...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 71], [Content-Type, text/html]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : System.__ComObject
RawContentLength  : 71
```
<img width="645" height="229" alt="스크린샷 2026-11-07 134257" src="https://github.com/user-attachments/assets/0fef1af5-98dc-498f-93ee-a24fb030dbb6" />

7. Docker 볼륨 영속성 검증


## 볼륨 생성 
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker volume create my-vol      
my-vol
```

## 연결 및 데이터 생성:
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker run -d --name volume-test-container -v my-vol:/data alpine sleep 1000
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
881fc932939e: Pull complete 
86ff264fe568: Download complete 
c35a7d82b8fa: Download complete 
Digest: sha256:3f4d6b32e26703c6082839234f43769d5046b3ac7c910033911d1e71d8cd630a
Status: Downloaded newer image for alpine:latest
b12d253e7bb4b4e71e45463ecf9d86794c49d2ce7e7d2aeada08fb7dd43a9f6f
```

## 컨테이너 내부의 /data 경로에 파일 생성
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker exec volume-test-container sh -c "echo 'Hello, Docker Volume!' > /data/hello.txt"
```

## 생성된 데이터 확인
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker exec volume-test-container cat /data/hello.txt
Hello, Docker Volume!
```

## 컨테이너 삭제
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker rm -f volume-test-container
volume-test-container
```

## 새 컨테이너 생성, 볼륨 재연결 및 데이터 확인
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository\docker-custom-test> docker run --rm -v my-vol:/data alpine cat /data/hello.txt
Hello, Docker Volume!
```

## 결과: "Hello, Docker Volume!" 출력됨 (데이터 유지 확인)

8. Git 및 GitHub 연동
Git 설정 확인: `git config --list`
```
PS C:\Users\fbwldn10042731\Desktop\workspace\Codessey-Repository> git config --list
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=schannel
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.name=***
user.email=***@naver.com
init.defaultbranch=main
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=https://github.com/Ryujiu/Codessey-Repository
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
## (user.name, user.email 확인 - 개인정보는 ***로 마스킹)
연동 증거: [VSCode와 GitHub 연동 화면 또는 GitHub 저장소 메인 스크린샷]

9. 트러블슈팅 (Troubleshooting)
Case 1: Docker Desktop 설치 시 가상화 오류
문제: "Virtual Machine Platform" 기능 미활성화 에러 발생.
원인: 윈도우 WSL2 기능이 꺼져 있어 Docker 엔진이 구동되지 않음.
해결: 관리자 권한 파워쉘에서 wsl --install 실행 후 재부팅하여 해결.
Case 2: 도커파일 활용한 웹파일 환경변수 미작동 오류
문제: ENV 및 sed 활용하여 관리자 이메일을 환경변수로 적용하려했으나 실 구동시 환경변수 작동하지 않음
원인: 도커파일은 UTF-8, 인덱스파일은 UTF-16 LE로 되어있어 인덱스파일의 MY_EMAIL을 제대로 인식하지 못함
해결: index.html 파일을 UTF-8로 다시 인코딩하여 저장하고 다시 빌드 및 실행

10. 보안 및 개인정보 보호
본 문서에 포함된 모든 로그와 스크린샷에서 개인 이메일, 토큰, 비밀번호 등은 마스킹 처리되었음을 확인합니다.