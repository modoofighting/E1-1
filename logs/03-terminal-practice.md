# Ubuntu Bash 기본 조작

기록일: 2026-11-09 (KST)

## 실습 폴더와 파일 생성

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">mkdir -p ~/workspace/practice</span>
userk@Win11-01:/mnt/c/Users/UserK$ <span style="color: #0969da;">cd ~/workspace/practice</span>
userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">pwd</span></code></pre>

```text
/home/userk/workspace/practice
```

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">touch hello.txt</span>
userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">exho "Hello. Dec Workstation!" &gt;hello.txt</span></code></pre>

```text
Command 'exho' not found, did you mean:
  command 'echo' from deb coreutils-from-gnu (0.0.0~ubuntu25)
  command 'echo' from deb coreutils-from-uutils (0.0.0~ubuntu25)
Try: sudo apt install <deb name>
```

<span style="color: #0969da;"><code>exho</code></span>는 오타라 파일에 쓰지 못했다. 이어서 <span style="color: #0969da;"><code>echo</code></span>를 실행했다.

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">echo "Hello. Dev Worksttion!" &gt;hello.txt</span>
userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">cat hello.txt</span></code></pre>

```text
Hello. Dev Worksttion!
```

## 복사·이동·삭제

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">cp hello.txt hello_backup.txt</span>
userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">mv hello_backup.txt renamed.txt</span>
userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">ls -la</span></code></pre>

```text
total 16
drwxr-xr-x 2 userk userk 4096 Nov  9 17:57 .
drwxr-xr-x 3 userk userk 4096 Nov  9 17:53 ..
-rw-r--r-- 1 userk userk   23 Nov  9 17:56 hello.txt
-rw-r--r-- 1 userk userk   23 Nov  9 17:56 renamed.txt
```

<pre style="border: 1px solid #d0d7de; border-radius: 6px; padding: 12px; background: #f6f8fa;"><code class="bash">userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">rm renamed.txt</span>
userk@Win11-01:~/workspace/practice$ <span style="color: #0969da;">ls -la</span></code></pre>

```text
total 12
drwxr-xr-x 2 userk userk 4096 Nov  9 17:57 .
drwxr-xr-x 3 userk userk 4096 Nov  9 17:53 ..
-rw-r--r-- 1 userk userk   23 Nov  9 17:56 hello.txt
```
