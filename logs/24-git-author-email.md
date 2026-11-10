# Git 작성자 이메일 설정 기록

Windows PowerShell · 저장소: `C:\all in one\e1-1`

이메일 주소를 설정 키처럼 조회해 오류가 발생했다.

```powershell
PS> git config --local --get skylandspace01@gmail.com
error: invalid key: skylandspace01@gmail.com
```

`--get` 뒤에는 이메일 주소가 아니라 설정 키를 입력해야 한다. 올바른 키는 `user.email`이다. 사용자가 입력한 주소를 이 저장소에 설정하고 조회해 확인했다.

```powershell
git -C "C:\all in one\e1-1" config --local user.email "skylandspace01@gmail.com"
git -C "C:\all in one\e1-1" config --local --get user.name
modoofighting
git -C "C:\all in one\e1-1" config --local --get user.email
skylandspace01@gmail.com
```

`--local` 설정은 이 저장소에만 적용된다. 이름과 이메일 설정을 확인했으며, 커밋과 업로드는 별도 단계다.