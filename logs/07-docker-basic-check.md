# Docker 기본 점검 기록

기록일: 2026-11-09
실행 위치: Ubuntu Bash

## Docker CLI 버전 확인

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">docker --version</span>
Docker version 29.9.3, build 5457199</code></pre>

Docker CLI 버전은 확인됨.

## Docker 엔진 연결 확인

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">docker info</span></code></pre>

Client 정보는 출력됐으나 Server 정보 요청은 소켓 권한 오류로 실패함.

```text
Client:
 Version:    29.9.3
 Context:    default
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.153.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-agent
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-ai
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     /usr/local/lib/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.7.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-compose
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-debug
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.99.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-desktop
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.10
    Path:     /usr/local/lib/docker/cli-plugins/docker-dhi
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     /usr/local/lib/docker/cli-plugins/docker-extension
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     /usr/local/lib/docker/cli-plugins/docker-init
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.3
    Path:     /usr/local/lib/docker/cli-plugins/docker-mcp
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     /usr/local/lib/docker/cli-plugins/docker-model
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.36
    Path:     /usr/local/lib/docker/cli-plugins/docker-offload
  pass: Docker Pass Secrets Manager (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     /usr/local/lib/docker/cli-plugins/docker-pass
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-sandbox
  scout: Docker Scout (Docker Inc.)
    Version:  v1.28.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-scout

Server:
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
```

현재 확인 내용: Ubuntu Bash에서 Docker CLI는 실행되지만 `/var/run/docker.sock`에 접근하지 못해 Docker Engine 연결은 확인되지 않음.

## Docker 소켓 및 사용자 그룹 확인

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">id</span>
uid=1000(userk) gid=1000(userk) groups=1000(userk),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users),1001(docker)
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">ls -l /var/run/docker.sock</span>
srw-rw---- 1 root docker 0 Nov  9 22:48 /var/run/docker.sock
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">command -v docker</span></code></pre>

`id` 출력에 `docker` 그룹이 있고 소켓도 `root:docker` 소유 및 그룹 읽기·쓰기 권한으로 표시됨. `command -v docker` 출력은 공유 기록에 없었으나, 아래 진단에서 실행 경로를 확인함.

## Docker 명령 경로와 연결 대상 확인

진단 기록일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">type -a docker</span>
docker is /usr/bin/docker
docker is /bin/docker
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">getent group docker</span>
docker:x:1001:userk
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">stat -c '%u:%g %A %n' /var/run/docker.sock</span>
0:1001 srw-rw---- /var/run/docker.sock
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker context ls</span>
NAME            DESCRIPTION                               DOCKER ENDPOINT                             ERROR
default *       Current DOCKER_HOST based configuration   unix:///var/run/docker.sock
desktop-linux   Docker Desktop                            npipe:////./pipe/dockerDesktopLinuxEngine</code></pre>

현재 `default` 연결 대상은 `unix:///var/run/docker.sock`이며 `desktop-linux`는 Windows named pipe 주소로 표시됨. 사용자 그룹과 소켓 권한은 서로 맞아 보이나 `docker info`는 여전히 접근 거부됨. 원인은 아직 확정되지 않음.

## Docker 연결 환경 변수 확인

진단 기록일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">printf 'DOCKER_HOST=%s\n' "${DOCKER_HOST-&lt;unset&gt;}"</span>
DOCKER_HOST=&lt;unset&gt;
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">printf 'DOCKER_CONTEXT=%s\n' "${DOCKER_CONTEXT-&lt;unset&gt;}"</span>
DOCKER_CONTEXT=&lt;unset&gt;
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">printf 'DOCKER_CONFIG=%s\n' "${DOCKER_CONFIG-&lt;unset&gt;}"</span>
DOCKER_CONFIG=&lt;unset&gt;</code></pre>

세 환경변수는 모두 설정되지 않음. `docker context ls`의 `Current DOCKER_HOST based configuration` 표시는 환경변수가 설정됐다는 증거가 아님.

## Docker Desktop 엔진 설정 확인

확인일: 2026-11-10

Docker Desktop `Settings > General`의 `Choose Virtual Machine Manager (VMM)` 값이 `WSL 2`이며, 화면에 `No unsaved changes`가 표시됨. WSL 2 엔진 설정은 저장된 상태.

## Docker Engine 연결 재확인

확인일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker info</span></code></pre>

Client와 Server 정보가 모두 출력됨. Docker Engine 연결이 확인됨.

```text
Client:
 Version:    29.9.3
 Context:    default
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.153.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-agent
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-ai
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     /usr/local/lib/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.7.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-compose
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-debug
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.99.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-desktop
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.10
    Path:     /usr/local/lib/docker/cli-plugins/docker-dhi
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     /usr/local/lib/docker/cli-plugins/docker-extension
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     /usr/local/lib/docker/cli-plugins/docker-init
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.3
    Path:     /usr/local/lib/docker/cli-plugins/docker-mcp
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     /usr/local/lib/docker/cli-plugins/docker-model
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.36
    Path:     /usr/local/lib/docker/cli-plugins/docker-offload
  pass: Docker Pass Secrets Manager (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     /usr/local/lib/docker/cli-plugins/docker-pass
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-sandbox
  scout: Docker Scout (Docker Inc.)
    Version:  v1.28.0
    Path:     /usr/local/lib/docker/cli-plugins/docker-scout

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 29.9.3
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
 Kernel Version: 6.18.33.2-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 22
 Total Memory: 15.37GiB
 Name: docker-desktop
 ID: 69c5c1d3-fd6b-465b-8912-85cc5074414d
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=unix:///var/run/docker-cli.sock
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables
```

`Images: 0`, 컨테이너 수 `0`으로 확인됨. 이 결과만으로 이미지를 직접 만들어야 하는 것은 아니며, 이미지 목록은 `docker images`로 확인할 수 있음.

## 초기 권한 오류 원인 검토

- 확인된 사실: 첫 `docker info`는 `/var/run/docker.sock`에서 `permission denied`를 반환했으나, 설정 변경 없이 다시 실행한 `docker info`는 성공함. 사용자 그룹과 소켓 그룹 ID는 모두 1001이고 연결 환경변수는 설정되지 않음.
- 원인: 최초 오류의 근본 원인은 확인되지 않음.
- 추정: 첫 확인 당시 WSL 연동 소켓이 준비 중이었거나 일시적으로 접근 상태가 달랐을 가능성. 재실행 성공에 근거한 추측이며 확정 사실은 아님.

## `docker images` 연결 오류

확인일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker images</span>
failed to connect to the docker API at npipe:////./pipe/dockerDesktopLinuxEngine; check if the path is correct and if the daemon is running: open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.</code></pre>

이 명령은 이미지 목록을 받지 못함. 오류에 나온 연결 대상은 `npipe:////./pipe/dockerDesktopLinuxEngine`이며, 직전 성공한 `docker info`의 `default` Unix 소켓 연결과 다름. 현재 활성 context를 다시 확인해야 하며, 이미지가 0개라는 뜻은 아님.

## 현재 context 확인

확인일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker context show</span>
desktop-linux
userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker context ls</span>
NAME              DESCRIPTION                               DOCKER ENDPOINT
default           Current DOCKER_HOST based configuration   npipe:////./pipe/docker_engine
desktop-linux *   Docker Desktop</code></pre>

`desktop-linux`가 현재 context로 확인됨. `docker images` 오류에서 이 context의 연결 주소는 `npipe:////./pipe/dockerDesktopLinuxEngine`으로 나타남. 이전 `docker info` 성공 때 선택됐던 `default` Unix 소켓 연결과 달라, 현재 명령은 WSL에서 찾을 수 없는 Windows named pipe를 사용하고 있음.

## 명령 입력 오류와 올바른 형식

확인일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker host</span>
docker: unknown command: docker host

Run 'docker --help' for more information</code></pre>

`docker host`는 유효한 하위 명령이 아님. Docker 옵션 `--host`를 사용하려면 아래처럼 하이픈 두 개를 포함해 전체 명령을 입력해야 함.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash"><span style="color: #0969da;">docker --host=unix:///var/run/docker.sock images</span></code></pre>

## WSL Unix 소켓 연결 실패

확인일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker --host=unix:///var/run/docker.sock images</span>
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?</code></pre>

`--host`로 지정한 Unix 소켓에서도 엔진에 연결하지 못함. `desktop-linux` context의 named pipe 문제와는 별도로, 현재 WSL Unix 소켓 연결 상태도 확인이 필요함.

## Docker `--host` 인수 누락

확인일: 2026-11-10

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker --host</span>
flag needs an argument: --host

Usage:  docker [OPTIONS] COMMAND [ARG...]

Run 'docker --help' for more information</code></pre>

`docker --host`만으로는 연결 주소가 전달되지 않아 인수 누락 오류가 발생. 주소를 단독으로 입력하지 말고 다음 전체 명령을 한 줄로 실행해야 함.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash"><span style="color: #0969da;">docker --host=unix:///var/run/docker.sock images</span></code></pre>

## 직접 진단 후 정정

2026-11-10 직접 비교 결과, Ubuntu CLI는 `default → unix:///var/run/docker.sock`로 엔진 연결과 이미지 조회에 성공했다. Windows CLI는 `desktop-linux → npipe:////./pipe/dockerDesktopLinuxEngine`를 사용했고, 여기에 Unix 소켓을 지정하면 마지막 오류가 동일하게 재현됐다. 앞선 “WSL context가 바뀌었다” 및 “Unix 소켓 실패가 엔진 중지를 뜻한다”는 설명은 확정되지 않은 해석이다. 최종 확인과 프로젝트 터미널 수정 내용은 [직접 진단 및 수정 기록](./08-docker-repair.md)에 정리했다.
