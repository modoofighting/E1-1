# Docker 연결 오류 직접 진단 및 수정

확인일: 2026-11-10

## 확인한 실행 환경

| 구분 | Ubuntu Docker CLI | Windows Docker CLI |
| --- | --- | --- |
| Client OS | linux | windows |
| 현재 context | default | desktop-linux |
| 연결 주소 | unix:///var/run/docker.sock | npipe:////./pipe/dockerDesktopLinuxEngine |
| 이미지 목록 | 정상 출력, 이미지 0개 | 정상 출력, 이미지 0개 |

Ubuntu의 `/usr/bin/docker`는 `/mnt/wsl/docker-desktop/cli-tools/usr/bin/docker`를 가리키는 링크이며 실제 파일은 Linux ELF 실행 파일이다. 사용자 그룹은 `docker`(1001), 소켓 그룹도 1001이다. Docker Desktop 상태는 `running`으로 확인했다.

## 오류 재현

Windows Docker CLI에 Unix 소켓을 지정했을 때 사용자가 받은 마지막 오류와 같은 메시지가 발생했다.

<pre><code class="powershell"><span style="color: #0969da;">& 'C:\Users\UserK\AppData\Local\Programs\DockerDesktop\resources\bin\docker.exe' --host=unix:///var/run/docker.sock images</span>
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?</code></pre>

사용자가 공유한 `default → npipe:////./pipe/docker_engine`, `desktop-linux *` 목록도 Windows CLI의 목록과 일치했다. 반면 Ubuntu CLI에서는 `default * → unix:///var/run/docker.sock`이다. 실행 환경과 연결 주소의 혼용 가능성을 뒷받침하는 결과지만, 과거 사용자 명령의 실행 프로세스가 모두 확인된 것은 아니다. Windows CLI의 Unix 소켓 실패는 재현했으며, 최초 `permission denied`와 named pipe 실패의 근본 원인은 확정하지 못했다. Windows에서 `desktop-linux` context와 named pipe를 사용하는 것은 정상적인 구성이다.

## 수정

이 프로젝트의 `.vscode/settings.json`에 `Ubuntu (WSL)` 프로필을 추가하고 새 터미널의 기본값으로 지정했다. 이 설정은 실행 환경을 명확히 선택하기 위한 조치이며 Docker 엔진이나 소켓을 수리한 것은 아니다. PowerShell에서 `wsl -d Ubuntu`로 진입하는 방법과 프로필 메뉴에서 Ubuntu를 여는 방법은 모두 정상이다. VS Code 자체의 실행 방식은 바뀌지 않으며 Windows 작업 시 PowerShell 프로필을 선택할 수 있다. Windows context는 Windows용 정상 설정이므로 수정하지 않았다.

## 수정 후 직접 확인

Ubuntu에서 다음 명령을 실행했다.

<pre><code class="bash"><span style="color: #0969da;">docker version --format 'Client={{.Client.Os}} Server={{.Server.Os}}'</span>
Client=linux Server=linux
<span style="color: #0969da;">docker info --format 'Engine={{.Name}} Version={{.ServerVersion}} Images={{.Images}} Containers={{.Containers}}'</span>
Engine=docker-desktop Version=29.9.3 Images=0 Containers=0
<span style="color: #0969da;">docker context show</span>
default
<span style="color: #0969da;">docker images</span>
IMAGE   ID             DISK USAGE   CONTENT SIZE   EXTRA</code></pre>

새 Ubuntu 대화형 셸에서도 context와 이미지 목록 조회가 성공했다. 이미지와 컨테이너는 생성하지 않았다.
