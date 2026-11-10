# 바인드 마운트 확인 기록

## 실행 위치

```text
userk@Win11-01:/mnt/c/all in one/e1-1$
```

현재 위치는 과제 루트 `e1-1`이다. 아래 명령의 `$(pwd)/site`는 `e1-1/site`를 가리킨다. README에서 의도한 `project/site`와 다른 경로다.

## 실행 명령 및 확인 내용

```bash
docker run -d -p 8082:80 -v "$(pwd)/site:/usr/share/nginx/html:ro" --name bind-test my-web:1.0
```

컨테이너가 생성되어 실행됐다.

```text
bbcb4cfa44045788fd41e857247c54fdbb8f74a65923fb823ae2ceb89330569e
```

```bash
curl http://localhost:8082
```

첫 요청은 다음 응답을 반환했다.

```html
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.31.8</center>
</body>
</html>
```

호스트 쪽 `e1-1/site`에 웹 시작 파일 `index.html`이 없어서 NGINX가 디렉터리 내용을 보여주지 못하고 `403 Forbidden`을 반환한 것으로 판단된다.

호스트 파일을 수정한 뒤 다시 요청하자 새 파일 내용이 응답에 나타났다. 호스트 파일 변경이 마운트를 통해 컨테이너에 전달되는 점은 확인했다. 다만 연결한 경로가 `project/site`가 아닌 `e1-1/site`였으므로 과제용 정적 페이지 반영을 확인한 결과로 보기는 어렵다.

브라우저 화면에서는 한글이 깨져 보였다. 호스트 파일에 `<meta charset="utf-8">`가 없어 브라우저가 UTF-8 문자를 올바르게 해석하지 못한 것이 원인이다. 파일의 HTML 닫는 태그도 올바르지 않았다. 화면 캡처는 수정 전 상태다: [8082 화면 캡처](./16-bind-mount-8082.png)

## 수정

현재 컨테이너는 호스트의 `e1-1/site`를 읽기 전용으로 연결하고 있다. 컨테이너 안 파일을 고치는 대신 호스트 파일 [site/index.html](../site/index.html)을 올바른 HTML 구조와 `<meta charset="utf-8">`를 포함하도록 수정했다. 브라우저에서 `localhost:8082`를 새로고침해 수정된 한글 표시를 확인한다.

## 다음 확인 전 정리

의도한 경로를 확인하려면 `project` 폴더에서 실행해 `project/site`를 마운트해야 한다. 현재 `bind-test`가 8082 포트를 사용 중이므로 같은 포트로 다시 실행하기 전에는 기존 컨테이너 상태를 확인해야 한다. 이 기록에서는 컨테이너를 변경하거나 삭제하지 않았다.
