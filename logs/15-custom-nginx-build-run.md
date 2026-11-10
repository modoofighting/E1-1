# my-web 이미지 빌드 및 컨테이너 확인

확인일: 2026-11-10<br>
실행 위치: Ubuntu Bash, `project` 폴더

## 커스텀 이미지 빌드

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1/project$ <span style="color: #0969da;">docker build -t my-web:1.0 .</span>
[+] Building 1.3s (7/7) FINISHED                                     docker:default
 => [internal] load build definition from Dockerfile                           0.0s
 => => transferring dockerfile: 172B                                           0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                0.1s
 => [internal] load .dockerignore                                              0.0s
 => => transferring context: 2B                                                0.0s
 => [internal] load build context                                              0.1s
 => => transferring context: 303B                                              0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:713fc2ebfd92e8217db45147  0.8s
 => => resolve docker.io/library/nginx:alpine@sha256:713fc2ebfd92e8217db45147  0.0s
 => => extracting sha256:881fc932939efef04ca120ad44bbd2c0917851eb247aa4995428  0.1s
 => => extracting sha256:1abafee971b93c1ae05f03c35c6fb6e3376e75e6f16b590092a8  0.1s
 => => extracting sha256:c7dfd64eeb02740cc41e9f32ad7f75781882d4c167b77427aa96  0.0s
 => => extracting sha256:e011f124249bf1c4be27b3311c3b40cb90dd7737381af1c62b50  0.0s
 => => extracting sha256:a746bd1dc828f61bf43b775dcfecb9ded8a3d3b41789915fe135  0.0s
 => => extracting sha256:b9c6ee6a30f8811636d7273be348906cf0e4e9725378dd54b7d4  0.0s
 => => extracting sha256:69d81d5522572d5b7abcc8f594ecbc4cf9bea72402675a419e8d  0.0s
 => => extracting sha256:da90db506dd5f2d604467c4dc38442534f74c9e76c1982df0c26  0.3s
 => [2/2] COPY site/ /usr/share/nginx/html/                                    0.1s
 => exporting to image                                                         0.2s
 => => exporting layers                                                        0.1s
 => => exporting manifest sha256:96abc6e31e0ce4839467baf053c2bbfb6a56afab19d8  0.0s
 => => exporting config sha256:000a935176b1d13819086fd315967c97ecd1bc34bc6558  0.0s
 => => exporting attestation manifest sha256:329899d712af7ffd04e9d073dd4b68e1  0.0s
 => => exporting manifest list sha256:015613cb612f550817894dd1696940391169734  0.0s
 => => naming to docker.io/library/my-web:1.0                                  0.0s
 => => unpacking to docker.io/library/my-web:1.0                               0.0s</code></pre>

빌드가 `7/7 FINISHED`로 끝났고 `my-web:1.0` 이미지가 만들어졌다. Dockerfile의 `COPY site/ ...` 단계도 완료됐다.

## 두 웹 컨테이너 시작

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1/project$ <span style="color: #0969da;">docker run -d -p 8080:80 --name my-web-8080 my-web:1.0</span>
cf736d6f4fe58cf8dd657760723ce1821a7770f665c307fbeff86a78f3898f8d</code></pre>

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1/project$ <span style="color: #0969da;">docker run -d -p 8081:80 --name my-web-8081 my-web:1.0</span>
5249359579c8e2f1b7e5012b9f171729d43b303bf9fe52c560f576ac6391efa6</code></pre>

## 웹 응답 확인

`curl`로 두 포트에 요청해 `index.html`이 제공되는지 확인했다. 두 응답 모두 아래 HTML을 반환했다.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1/project$ <span style="color: #0969da;">curl http://localhost:8080</span>
&lt;!doctype html&gt;
&lt;html lang=&quot;ko&quot;&gt;
&lt;head&gt;
  &lt;meta charset=&quot;utf-8&quot;&gt;
  &lt;title&gt;My Custom NGINX&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;h1&gt;My Custom NGINX&lt;/h1&gt;
  &lt;p&gt;정적 웹 페이지가 정상적으로 표시됩니다.&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>

`http://localhost:8081`에서도 같은 HTML 응답을 확인했다. 브라우저 화면은 [8080 포트](./13-nginx-8080.png), [8081 포트](./14-nginx-8081.png)에서 확인할 수 있다.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/all in one/e1-1/project$ <span style="color: #0969da;">curl http://localhost:8081</span>
&lt;!doctype html&gt;
&lt;html lang=&quot;ko&quot;&gt;
&lt;head&gt;
  &lt;meta charset=&quot;utf-8&quot;&gt;
  &lt;title&gt;My Custom NGINX&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;h1&gt;My Custom NGINX&lt;/h1&gt;
  &lt;p&gt;정적 웹 페이지가 정상적으로 표시됩니다.&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
