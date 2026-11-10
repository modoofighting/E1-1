# 바인드 마운트 초기 시도 기록

이 기록은 과제 루트 `e1-1`에서 잘못된 호스트 폴더를 연결했던 초기 시도다. 최종 성공 증거와 구분해 보관한다.

## 초기 시도와 확인된 원인

실행 위치는 `/mnt/c/all in one/e1-1`이었다. 따라서 `$(pwd)/site`는 `e1-1/site`를 가리켰고, 과제 웹 파일이 있는 `project/site`와 다른 위치였다.

```bash
docker run -d -p 8082:80 -v "$(pwd)/site:/usr/share/nginx/html:ro" --name bind-test my-web:1.0
curl http://localhost:8082
```

첫 응답은 `403 Forbidden`이었다. 호스트의 `e1-1/site`에 NGINX가 시작 페이지로 읽을 `index.html`이 없었던 것이 확인된 원인이다. 해당 폴더의 파일에 임시 HTML을 기록하자 응답은 바뀌었지만, 과제 대상인 `project/site/index.html`을 연결한 결과는 아니었다.

초기 파일에는 UTF-8 문자셋 선언과 올바른 HTML 구조도 없어서 브라우저 한글이 깨졌다. 호스트 파일에서 `<meta charset="utf-8">`를 선언하고 HTML을 바로잡아야 한다.

## 최종 실습 경로

올바른 실습은 `project` 폴더에서 `$(pwd)/site`를 연결해 진행한다. 최종 명령·호스트 파일 변경 전후 응답·브라우저 화면은 [바인드 마운트 검증 기록](./19-bind-mount-verification.md)에 정리했다.