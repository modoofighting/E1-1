# Ubuntu 이미지 다운로드 DNS 오류 확인

확인일: 2026-11-10<br>
실행 위치: Windows PowerShell에서 Ubuntu WSL 및 Docker Engine 점검

## 처음 발생한 오류

`docker run -it ubuntu bash` 실행 시 `registry-1.docker.io`를 찾지 못해 Ubuntu 이미지 다운로드가 중단됐다.

```text
lookup registry-1.docker.io: no such host
Docker Desktop has no HTTPS proxy
```

첫 문구는 그 시점에 Docker Engine이 Docker Hub 주소를 DNS로 조회하지 못했음을 보여준다. 두 번째 문구는 HTTPS 프록시 없이 직접 연결을 시도했다는 뜻이며, 이것만으로 프록시 설정이 원인이라고 단정할 수는 없다.

## 확인 및 재시도

Windows에서는 `registry-1.docker.io` DNS 조회가 응답했고 HTTPS 443 연결 확인은 `True`였다. Ubuntu WSL에서도 Docker Hub 주소와 `google.com`의 DNS 조회가 응답했다.

```text
nameserver 10.255.255.254
```

Ubuntu WSL의 DNS 설정도 기록했다. 이후 Docker Engine을 통해 Ubuntu 이미지를 다시 내려받았다.

```text
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

이미지를 사용한 컨테이너 시작도 확인했다.

```text
container=running
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
```

## 결론 및 해결 방법

현재는 DNS 조회, 이미지 다운로드, Ubuntu 컨테이너 실행이 모두 성공한다. 따라서 최초 오류는 지속 중인 설정 오류로 재현되지 않았고, 발생 당시의 일시적인 DNS 또는 네트워크 문제로 보인다. 최초 실패의 더 깊은 원인은 현재 자료만으로 확정할 수 없다. Docker 설정을 임의로 바꾸지 않고 이미지 다운로드를 재시도해 해결을 확인했다.

같은 오류가 다시 발생하면 먼저 Docker Desktop이 실행 중인지와 Windows 네트워크 연결을 확인한 다음 Ubuntu에서 `docker pull ubuntu:latest`를 재시도한다. 회사·학교망 또는 VPN처럼 프록시를 요구하는 네트워크라면 Docker Desktop의 **Settings > Resources > Proxies**에서 해당 네트워크의 프록시를 설정한다. 일반 직접 연결을 사용하는 현재 환경에서는 프록시 설정을 추가할 필요가 없다.

Docker Desktop 문서는 **No proxy**를 프록시 없이 직접 연결하는 모드로 설명하며, 이미지 다운로드는 Containers proxy 설정을 사용한다고 안내한다. [Docker Desktop 설정 문서](https://docs.docker.com/desktop/settings-and-maintenance/settings/)
