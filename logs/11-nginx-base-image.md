# NGINX Alpine 베이스 이미지 확인

확인일: 2026-11-10<br>
실행 위치: Ubuntu Bash (WSL)

## 이미지 내려받기

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker pull nginx:alpine</span>
alpine: Pulling from library/nginx
881fc932939e: Pulling fs layer
69d81d552257: Pulling fs layer
1abafee971b9: Pulling fs layer
da90db506dd5: Pulling fs layer
b9c6ee6a30f8: Pulling fs layer
c7dfd64eeb02: Pulling fs layer
e011f124249b: Pulling fs layer
a746bd1dc828: Pulling fs layer
69d81d552257: Download complete
881fc932939e: Download complete
1abafee971b9: Download complete
c7dfd64eeb02: Download complete
a746bd1dc828: Download complete
b9c6ee6a30f8: Download complete
e011f124249b: Download complete
881fc932939e: Pull complete
8801fee91458: Download complete
da90db506dd5: Download complete
1abafee971b9: Pull complete
c7dfd64eeb02: Pull complete
e011f124249b: Pull complete
69d81d552257: Pull complete
a746bd1dc828: Pull complete
b9c6ee6a30f8: Pull complete
4c900eca9c64: Download complete
da90db506dd5: Pull complete
Digest: sha256:713fc2ebfd92e8217db451477182ee91944b123d2b1ee9fd5a8c6e91ca6c3a11
Status: Downloaded newer image for nginx:alpine
docker.io/library/nginx:alpine</code></pre>

## 이미지 확인

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash"># 로컬 nginx:alpine 이미지의 상세 정보 확인
<span style="color: #0969da;">docker image inspect nginx:alpine --format 'ID={{.Id}} OS={{.Os}} Architecture={{.Architecture}} Size={{.Size}} bytes Created={{.Created}}'</span>
ID=sha256:713fc2ebfd92e8217db451477182ee91944b123d2b1ee9fd5a8c6e91ca6c3a11 OS=linux Architecture=amd64 Size=93626761 bytes Created=2026-11-05T19:55:10.705521378Z</code></pre>

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash"># 내려받은 이미지의 목록과 크기 확인
<span style="color: #0969da;">docker images nginx:alpine</span>
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
nginx:alpine   713fc2ebfd92       94.5MB         27.3MB</code></pre>

NGINX 공식 이미지는 NGINX Docker Maintainers가 관리한다. 공식 이미지 설명에 따르면 `nginx:<version>-alpine` 변형은 Alpine Linux 기반이며, 작은 이미지 크기가 중요한 경우에 사용할 수 있다. Alpine 기반 이미지에는 Bash나 Git 같은 추가 도구가 기본으로 포함되지 않을 수 있다. [NGINX 공식 이미지](https://hub.docker.com/_/nginx)

## Ubuntu Bash에서 다시 확인

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1$ <span style="color: #0969da;">docker pull nginx:alpine</span>
alpine: Pulling from library/nginx
Digest: sha256:713fc2ebfd92e8217db451477182ee91944b123d2b1ee9fd5a8c6e91ca6c3a11
Status: Image is up to date for nginx:alpine
docker.io/library/nginx:alpine</code></pre>

이미지가 이미 최신 상태여서 다시 내려받을 필요가 없음을 확인했다.
