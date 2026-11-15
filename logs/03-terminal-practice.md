# Ubuntu Bash 기본 조작

실습 경로: `/home/skylandspace010191/workspace/practice`

## 1. 실습 폴더 생성과 이동

```bash
# 중간 경로를 포함해 실습 폴더를 만든다.
mkdir -p ~/workspace/practice

# 실습 폴더로 이동한다.
cd ~/workspace/practice

# 현재 경로를 확인한다.
pwd
```

실행 결과:

```text
/home/skylandspace010191/workspace/practice
```

## 2. 파일 생성과 내용 확인

```bash
# 빈 파일을 만든다.
touch hello.txt

# 파일의 권한과 크기를 확인한다.
ls -la hello.txt
```

실행 결과:

```text
-rw-r--r-- 1 skylandspace010191 skylandspace010191 0 Nov 15 15:58 hello.txt
```

```bash
# 파일에 문장을 쓴다.
echo "hello, Dev workstation!" > hello.txt

# 파일의 내용을 확인한다.
cat hello.txt
```

실행 결과:

```text
hello, Dev workstation!
```

## 3. 파일 복사와 이름 변경

```bash
# 원본을 복사해 백업 파일을 만든다.
cp hello.txt hello_backup.txt

# 백업 파일의 이름을 변경한다.
mv hello_backup.txt renamed.txt

# 파일 목록을 확인한다.
ls -la
```

실행 결과:

```text
total 16
drwxr-xr-x 2 skylandspace010191 skylandspace010191 4096 Nov 15 16:07 .
drwxr-xr-x 3 skylandspace010191 skylandspace010191 4096 Nov 15 15:57 ..
-rw-r--r-- 1 skylandspace010191 skylandspace010191   24 Nov 15 15:59 hello.txt
-rw-r--r-- 1 skylandspace010191 skylandspace010191   24 Nov 15 16:00 renamed.txt
```

원본인 `hello.txt`와 이름을 변경한 `renamed.txt`를 확인했다.

## 4. 파일 삭제와 확인

```bash
# 이름을 변경한 파일을 삭제한다.
rm renamed.txt

# 삭제가 반영됐는지 확인한다.
ls -la
```

실행 결과:

```text
total 12
drwxr-xr-x 2 skylandspace010191 skylandspace010191 4096 Nov 15 16:08 .
drwxr-xr-x 3 skylandspace010191 skylandspace010191 4096 Nov 15 15:57 ..
-rw-r--r-- 1 skylandspace010191 skylandspace010191   24 Nov 15 15:59 hello.txt
```

`renamed.txt`가 삭제되고 원본 `hello.txt`가 남아 있는 것을 확인했다.
