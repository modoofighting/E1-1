# Ubuntu Bash 권한 실습

실습 경로: `/home/skylandspace010191/workspace/practice`

## 1. 파일 생성과 권한 확인

```bash
# 권한 실습에 사용할 빈 파일을 만든다.
touch permission-file.txt

# 파일의 현재 권한을 확인한다.
ls -l permission-file.txt
```

실행 결과:

```text
-rw-r--r-- 1 skylandspace010191 skylandspace010191 0 Nov 15 16:11 permission-file.txt
```

파일 생성 직후 권한은 `644`로 표시됐다. 소유자는 읽고 쓸 수 있고, 그룹과 기타 사용자는 읽을 수 있다.

## 2. 파일 권한을 600으로 변경

```bash
# 소유자만 파일을 읽고 쓸 수 있도록 변경한다.
chmod 600 permission-file.txt

# 변경한 권한을 확인한다.
ls -l permission-file.txt
```

실행 결과:

```text
-rw------- 1 skylandspace010191 skylandspace010191 0 Nov 15 16:11 permission-file.txt
```

`600`은 소유자에게 읽기·쓰기 권한을 부여하고, 그룹과 기타 사용자에게는 권한을 부여하지 않는다.

## 3. 파일 권한을 644로 변경

```bash
# 소유자는 읽고 쓸 수 있고, 나머지 사용자는 읽을 수 있도록 변경한다.
chmod 644 permission-file.txt

# 변경한 권한을 확인한다.
ls -l permission-file.txt
```

실행 결과:

```text
-rw-r--r-- 1 skylandspace010191 skylandspace010191 0 Nov 15 16:11 permission-file.txt
```

`644`는 소유자에게 읽기·쓰기 권한을, 그룹과 기타 사용자에게 읽기 권한을 부여한다.

## 4. 디렉터리 생성과 권한 확인

```bash
# 디렉터리 권한 실습용 폴더를 만든다.
mkdir permission-dir

# 디렉터리 자체의 권한을 확인한다.
ls -ld permission-dir
```

실행 결과:

```text
drwxr-xr-x 2 skylandspace010191 skylandspace010191 4096 Nov 15 16:13 permission-dir
```

첫 글자 `d`는 디렉터리를 뜻한다. `ls -ld`의 `-d`는 폴더 내부 목록이 아닌 디렉터리 자체의 정보를 표시한다.

## 5. 디렉터리 권한을 700으로 변경

```bash
# 소유자만 디렉터리에 접근하고 내용을 수정할 수 있도록 변경한다.
chmod 700 permission-dir

# 변경한 디렉터리 권한을 확인한다.
ls -ld permission-dir
```

실행 결과:

```text
drwx------ 2 skylandspace010191 skylandspace010191 4096 Nov 15 16:13 permission-dir
```

`700`은 소유자에게 읽기·쓰기·접근 권한을 부여하고, 그룹과 기타 사용자에게는 권한을 부여하지 않는다.
