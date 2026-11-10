# 컨테이너 생성 후 점검 명령

Ubuntu Bash에서 실행한다. 기본 점검 당시에는 이미지와 컨테이너가 각각 0개였으므로, 컨테이너 생성 후 아래 명령으로 상태를 확인한다.

## 컨테이너 이름 확인

<pre style="background: #f6f8fa; color: #1f2328; padding: 12px; white-space: pre-wrap; overflow-wrap: anywhere;"><code style="color: inherit;"><span style="color: #0969da;">docker ps -a</span></code></pre>

`NAMES` 열에서 이름을 확인한다. 아래 명령의 `<컨테이너명>` 전체를 실제 이름으로 바꾼다.

## 컨테이너 출력 로그 조회

<pre style="background: #f6f8fa; color: #1f2328; padding: 12px; white-space: pre-wrap; overflow-wrap: anywhere;"><code style="color: inherit;"><span style="color: #0969da;">docker logs &lt;컨테이너명&gt;</span></code></pre>

컨테이너 프로그램이 표준 출력과 표준 오류로 남긴 로그를 확인한다.

## 실행 중 자원 사용량 확인

<pre style="background: #f6f8fa; color: #1f2328; padding: 12px; white-space: pre-wrap; overflow-wrap: anywhere;"><code style="color: inherit;"><span style="color: #0969da;">docker stats --no-stream</span></code></pre>

실행 중인 컨테이너의 CPU·메모리 등 사용량을 한 번 출력한다.
