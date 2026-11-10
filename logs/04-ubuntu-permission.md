# Ubuntu Bash 권한 실습

기록일: 2026-11-09 (KST)

## 파일 권한 확인

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">touch permission-file.txt</span>
userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">ls -l permission-file.txt</span></code></pre>

```text
-rwxrwxrwx 1 userk userk 0 Nov  9 21:20 permission-file.txt
```

<span style="color: #8250df;">확인</span>: 파일을 만든 직후 권한이 `rwxrwxrwx`(777)로 표시됐다. 소유자·그룹·기타 사용자 모두에게 읽기·쓰기·실행 권한이 표시된 상태다.

| 표시 | 의미 |
|------|------|
| <span style="color: #8250df;">`-`</span> | 일반 파일을 뜻한다. 디렉터리라면 `d`로 표시된다. |
| <span style="color: #8250df;">`rwx` `rwx` `rwx`</span> | 소유자·그룹·기타 사용자 순서다. 세 범위 모두 읽기(`r`), 쓰기(`w`), 실행(`x`) 권한이 표시되어 전체 권한은 `777`이다. |
| <span style="color: #8250df;">`1`</span> | 파일을 가리키는 하드 링크 수다. |
| <span style="color: #8250df;">`userk userk`</span> | 소유자와 그룹 이름이다. |
| <span style="color: #8250df;">`0`</span> | 파일 크기이며, 단위는 바이트다. |
| <span style="color: #8250df;">`Nov 9 21:20`</span> | 파일의 마지막 수정 시각이다. |
| <span style="color: #8250df;">`permission-file.txt`</span> | 파일 이름이다. |

따라서 이 출력은 크기가 0바이트인 일반 파일이며, 표시상 세 사용자 범위 모두 `rwx` 권한을 가진다는 뜻이다.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">chmod 600 permission-file.txt</span>
userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">ls -l permission-file.txt</span></code></pre>

```text
-rwxrwxrwx 1 userk userk 0 Nov  9 21:20 permission-file.txt
```

<span style="color: #8250df;">확인</span>: `600`을 지정했지만 표시 권한은 `rwxrwxrwx`(777)로 그대로다. 요청한 `600`의 권한 표시로 바뀌지 않았다.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">chmod 644 permission-file.txt</span>
userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">ls -l permission-file.txt</span></code></pre>

```text
-rwxrwxrwx 1 userk userk 0 Nov  9 21:20 permission-file.txt
```

<span style="color: #8250df;">확인</span>: `644`를 지정한 뒤에도 표시 권한은 `rwxrwxrwx`(777)로 그대로다. 요청한 `644`의 권한 표시로 바뀌지 않았다.

같은 확인 명령을 한 번 더 실행했으며, 출력은 동일했다.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">ls -l permission-file.txt</span></code></pre>

```text
-rwxrwxrwx 1 userk userk 0 Nov  9 21:20 permission-file.txt
```

<span style="color: #8250df;">확인</span>: 추가로 다시 확인해도 같은 표시여서 변경된 권한이 보이지 않는다.

## 디렉터리 권한 확인

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">mkdir permission-dir</span>
userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">ls -ld permission-dir</span></code></pre>

```text
drwxrwxrwx 1 userk userk 4096 Nov  9 21:30 permission-dir
```

<span style="color: #8250df;">확인</span>: 첫 글자 `d`는 디렉터리, 세 묶음의 `rwx`는 소유자·그룹·기타 사용자 모두에게 읽기·쓰기·진입 권한이 표시됨을 뜻한다. `userk userk`는 소유자와 그룹, `4096`은 표시된 크기(바이트), `Nov 9 21:30`은 수정 시각, `permission-dir`은 디렉터리 이름이다.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">chmod 700 permission-dir</span>
userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">ls -ld permission-dir</span></code></pre>

```text
drwxrwxrwx 1 userk userk 4096 Nov  9 21:30 permission-dir
```

<span style="color: #8250df;">확인</span>: `700`을 지정한 뒤에도 `drwxrwxrwx`로 표시되어 소유자만 접근하도록 바뀐 상태가 확인되지 않았다.

## 확인 내용

파일과 디렉터리 모두 `chmod` 실행 뒤에도 `ls` 표시가 `rwxrwxrwx`로 같았다. 작업 위치 `/mnt/c/Users/UserK`는 Windows C: 드라이브를 WSL에서 접근한 경로다. DrvFS의 기본 설정은 Linux 권한 메타데이터가 꺼진 상태이며, 이때 Windows 권한을 Linux 권한 표시로 변환한다. 현재 mount 설정은 별도로 확인하지 않았지만, 이번 출력은 메타데이터를 사용하지 않을 때의 표시와 일치한다. 따라서 이번 실습에서는 `600`, `644`, `700` 권한 변경을 확인하지 못했다.

Linux 파일 시스템에서의 권한 동작은 Ubuntu 홈 디렉터리에서 확인한다. [Microsoft Learn: WSL 파일 권한](https://learn.microsoft.com/en-us/windows/wsl/file-permissions)

## Ubuntu 홈 디렉터리에서 권한 변경 재확인

기록일: 2026-11-10 (KST) · 작업 위치: `/home/userk/workspace/practice`

처음 파일을 만들 때 사용한 이름은 `permission.file.txt`였다. 조회 명령에서 `permission-file.txt`로 이름을 다르게 입력해 파일을 찾지 못했다. 이후 `chmod`에도 `permission.ile.txt`를 입력하여 같은 이유로 실패했다. 파일 이름의 점(`.`)과 하이픈(`-`), 철자를 정확히 맞춰야 한다.

```bash
userk@Win11-01:~/workspace/practice$ ls -l permission.file.txt
-rw-r--r-- 1 userk userk 0 Nov 10 13:09 permission.file.txt
userk@Win11-01:~/workspace/practice$ chmod 600 permission.ile.txt
chmod: cannot access 'permission.ile.txt': No such file or directory
userk@Win11-01:~/workspace/practice$ ls -l permission.file.txt
-rw-r--r-- 1 userk userk 0 Nov 10 13:09 permission.file.txt
```

이후 실제 파일명에 맞춰 실행했다.

```bash
userk@Win11-01:~/workspace/practice$ chmod 600 permission.file.txt
userk@Win11-01:~/workspace/practice$ ls -l permission.file.txt
-rw------- 1 userk userk 0 Nov 10 13:09 permission.file.txt
```

`-rw-------`는 일반 파일(`-`)이며 소유자에게 읽기(`r`)·쓰기(`w`) 권한만 있고, 그룹과 기타 사용자에게는 권한이 없다는 뜻이다. Ubuntu 홈 경로에서 `chmod 600`이 적용된 것을 확인했다.

### 파일 권한을 644로 변경

```bash
userk@Win11-01:~/workspace/practice$ chmod 644 permission.file.txt
userk@Win11-01:~/workspace/practice$ ls -l permission.file.txt
-rw-r--r-- 1 userk userk 0 Nov 10 13:09 permission.file.txt
```

`-rw-r--r--`는 소유자가 읽기·쓰기 가능하고 그룹과 기타 사용자는 읽기만 가능한 상태다. `644` 권한 변경을 확인했다.

### 디렉터리 권한 700 확인

```bash
userk@Win11-01:~/workspace/practice$ mkdir permission-dir
mkdir: permission-dir: File exists
userk@Win11-01:~/workspace/practice$ ls -ld permission-dir
drwx------ 2 userk userk 4096 Nov 10 13:22 permission-dir
userk@Win11-01:~/workspace/practice$ chmod 700 permission-dir
userk@Win11-01:~/workspace/practice$ ls -ld permission-dir
drwx------ 2 userk userk 4096 Nov 10 13:22 permission-dir
```

`mkdir`는 `permission-dir`가 이미 있어 새로 만들지 못했다. 최초 조회에서 이미 `drwx------`였고, `chmod 700` 뒤에도 같은 표시였다. 따라서 기존 디렉터리가 700 권한인 것을 확인했다.
