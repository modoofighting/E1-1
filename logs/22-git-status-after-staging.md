# Git 연결 및 제출 파일 등록 기록

Windows PowerShell · 작업 폴더: `C:\all in one\e1-1`

```powershell
# 브랜치와 파일 등록 상태 확인
git status -sb
```

```text
## main...origin/main
M  .gitignore
MM README.md
A  docs/container-log-commands.md
A  docs/root-admin-account.md
A  logs/01-wsl-online.md
A  logs/02-ubuntu-ls-la.md
A  logs/03-terminal-practice.md
A  logs/04-ubuntu-permission.md
A  logs/05-docker-install.png
A  logs/05-docker-install.txt
A  logs/06-docker-wsl-integration-pending.png
A  logs/06-docker-wsl-integration.png
A  logs/07-docker-basic-check.md
A  logs/08-docker-repair.md
A  logs/09-docker-ubuntu-pull-dns.md
A  logs/10-docker-attach-exec.png
A  logs/11-nginx-base-image.md
A  logs/12-index-html.png
A  logs/13-nginx-8080.png
A  logs/14-nginx-8081.png
A  logs/15-custom-nginx-build-run.md
A  logs/16-bind-mount-8082.png
A  logs/16-bind-mount-result.md
A  logs/17-bind-mount-browser-before-refresh.png
A  logs/18-bind-mount-curl-before-after.png
A  logs/19-bind-mount-verification.md
A  logs/20-bind-mount-browser-after-refresh.png
A  logs/21-volume-pull-credentials-error.md
A  project/Dockerfile
A  project/site/index.html
```

```powershell
# GitHub 원격 주소 확인
git remote -v
```

```text
origin	https://github.com/modoofighting/E1-1.git (fetch)
origin	https://github.com/modoofighting/E1-1.git (push)
```

`main...origin/main`은 원격 브랜치를 추적한다는 뜻이다. 첫 번째 칸의 `M`은 수정 파일, `A`는 새 파일을 커밋 준비 영역에 등록했다는 뜻이다. `??`는 미등록 파일이며 오류가 아니다.

제출 파일 등록 명령:

```powershell
git add .gitignore README.md docs logs project
```

개인 설정·복구 폴더와 참고용 README는 `.gitignore`로 제외하고 로컬에 보존했다. 커밋·업로드는 아직 진행하지 않았다.

`LF will be replaced by CRLF`는 확인된 `core.autocrlf=true` 설정에 따른 줄바꿈 변환 안내다. 등록 실패나 한글 인코딩 오류가 아니다.

## 사용자가 공유한 최신 확인 결과

```powershell
PS C:\all in one\e1-1> git status -sb
## main...origin/main
M  .gitignore
MM README.md
A  docs/container-log-commands.md
A  docs/root-admin-account.md
A  logs/01-wsl-online.md
A  logs/02-ubuntu-ls-la.md
A  logs/03-terminal-practice.md
AM logs/04-ubuntu-permission.md
A  logs/05-docker-install.png
A  logs/05-docker-install.txt
A  logs/06-docker-wsl-integration-pending.png
A  logs/06-docker-wsl-integration.png
A  logs/07-docker-basic-check.md
A  logs/08-docker-repair.md
A  logs/09-docker-ubuntu-pull-dns.md
A  logs/10-docker-attach-exec.png
A  logs/11-nginx-base-image.md
A  logs/12-index-html.png
A  logs/13-nginx-8080.png
A  logs/14-nginx-8081.png
A  logs/15-custom-nginx-build-run.md
A  logs/16-bind-mount-8082.png
A  logs/16-bind-mount-result.md
A  logs/17-bind-mount-browser-before-refresh.png
A  logs/18-bind-mount-curl-before-after.png
A  logs/19-bind-mount-verification.md
A  logs/20-bind-mount-browser-after-refresh.png
A  logs/21-volume-pull-credentials-error.md
A  logs/22-git-status-after-staging.md
A  project/Dockerfile
A  project/site/index.html

PS C:\all in one\e1-1> git branch --show-current
main
PS C:\all in one\e1-1> git remote -v
origin  https://github.com/modoofighting/E1-1.git (fetch)
origin  https://github.com/modoofighting/E1-1.git (push)
```

`MM README.md`는 README에 스테이징된 변경과 아직 스테이징되지 않은 추가 수정이 모두 있다는 표시다. `AM logs/04-ubuntu-permission.md`도 같은 방식으로, 새 파일이 스테이징된 뒤 추가 수정이 생겼다는 뜻이다. 나머지 `M`과 `A` 항목은 스테이징된 수정·추가 파일이다. 현재 브랜치는 `main`이고 `origin/main`을 추적하며, 원격 주소는 `modoofighting/E1-1`이다.

## 최신 Git 상태

```powershell
PS C:\all in one\e1-1> git status -sb
## main...origin/main
M  .gitignore
MM README.md
A  docs/container-log-commands.md
A  docs/root-admin-account.md
A  logs/01-wsl-online.md
A  logs/02-ubuntu-ls-la.md
A  logs/03-terminal-practice.md
AM logs/04-ubuntu-permission.md
A  logs/05-docker-install.png
A  logs/05-docker-install.txt
A  logs/06-docker-wsl-integration-pending.png
A  logs/06-docker-wsl-integration.png
A  logs/07-docker-basic-check.md
A  logs/08-docker-repair.md
A  logs/09-docker-ubuntu-pull-dns.md
A  logs/10-docker-attach-exec.png
A  logs/11-nginx-base-image.md
A  logs/12-index-html.png
A  logs/13-nginx-8080.png
A  logs/14-nginx-8081.png
A  logs/15-custom-nginx-build-run.md
A  logs/16-bind-mount-8082.png
A  logs/16-bind-mount-result.md
A  logs/17-bind-mount-browser-before-refresh.png
A  logs/18-bind-mount-curl-before-after.png
A  logs/19-bind-mount-verification.md
A  logs/20-bind-mount-browser-after-refresh.png
A  logs/21-volume-pull-credentials-error.md
AM logs/22-git-status-after-staging.md
A  project/Dockerfile
A  project/site/index.html
?? logs/23-github-cli-auth.md
?? logs/24-git-author-email.md
```

`??`는 Git이 아직 추적하지 않는 파일이다. 인증·작성자 이메일 로그는 이 상태 확인 이후 생성되어 아직 추가되지 않은 것으로 표시됐다. `MM`은 README에 스테이징된 변경과 미스테이징 변경이 함께 있음을, `AM`은 추가된 로그에 스테이징 뒤의 수정이 더 있음을 뜻한다.
