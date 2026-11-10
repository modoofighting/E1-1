# Ubuntu 이미지 내려받기 중 인증 도구 오류

## 직접 확인 및 해결

Ubuntu에서 직접 점검한 현재 상태:

```text
credsStore=desktop.exe
helper=/usr/bin/docker-credential-desktop.exe
helper_returncode=0
engine_returncode=0
Server Version: 29.9.3
```

인증값은 출력하지 않고 helper의 성공 여부만 확인했다. 설정을 변경하지 않고 다음 명령을 재시도했다.

```bash
docker pull ubuntu:24.04
```

```text
Digest: sha256:fe81560990fae86dc4743202b2cb4d3df98d0bcf949173cecef15ce1127ef852
Status: Downloaded newer image for ubuntu:24.04
docker.io/library/ubuntu:24.04
```

동일 이름의 기존 컨테이너가 없음을 확인한 뒤 처음 실패했던 명령을 실행했다.

```bash
docker run -d --name vol-test --mount source=mydata,target=/data ubuntu:24.04 sleep infinity
```

```text
8bcd711dccbe27f4c2dbe8c2607ddf030278352ce774cf7ba5a0186e66b9d816
```

실행 상태와 연결한 볼륨을 조회했다.

```bash
docker inspect vol-test --format '{{.State.Status}} {{range .Mounts}}{{.Type}} {{.Name}} {{.Destination}}{{end}}'
```

```text
running volume mydata /data
```

이미지 다운로드와 볼륨 연결 컨테이너 생성이 완료됐다. 최초 인증 helper 실패는 재현되지 않아 구체적 원인을 확정할 수 없다. 파일 저장과 컨테이너 삭제 후 보존 확인은 아직 수행하지 않았다.

## 실행 명령

```bash
docker run -d --name vol-test --mount source=mydata,target=/data ubuntu:24.04 sleep infinity
```

## 실제 출력

```text
Unable to find image 'ubuntu:24.04' locally
docker: error getting credentials - err: exit status 1, out: ``

Run 'docker run --help' for more information
```

## 원인 판단

`ubuntu:24.04` 이미지가 로컬에 없어서 Docker가 레지스트리에서 가져오려 했다. 이 과정에서 Docker CLI가 인증 정보를 제공하는 외부 credential helper를 호출했지만, 도구가 오류 코드 `1`로 종료했고 메시지도 반환하지 않았다. 따라서 이 오류는 볼륨 연결 옵션에서 난 것이 아니다.

현재 출력만으로는 어떤 helper가 설정되어 있는지, helper가 없거나 실행에 실패했는지까지 확정할 수 없다. 다음 진단 명령으로 설정된 helper 이름과 실행 파일 경로를 확인한다. `config.json` 전체에는 민감한 설정이 있을 수 있으므로 파일 전체를 공유하지 않는다.

## 다음 진단 명령

```bash
grep -E '"credsStore"|"credHelpers"' ~/.docker/config.json 2>/dev/null || echo '도우미 설정 항목 없음'
command -v docker-credential-desktop.exe || true
command -v docker-credential-desktop || true
command -v docker-credential-wincred || true
```

Docker 문서에 따르면 `credsStore` 또는 `credHelpers`가 설정되어 있으면 CLI는 해당 외부 helper 프로그램을 통해 인증 정보를 읽으며, helper는 CLI가 실행되는 환경의 `PATH`에서 찾을 수 있어야 한다. [Docker 로그인 및 credential helper 설명](https://docs.docker.com/reference/cli/docker/login/)
