# GitHub CLI 인증 확인

Windows PowerShell에서 `gh auth status --hostname github.com`을 실행해 확인했다. 토큰 값은 보안상 기록하지 않는다.

```text
github.com
  ✓ Logged in to github.com account modoofighting (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: (값은 기록하지 않음)
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'

  ✓ Logged in to github.com account art-ruby (keyring)
  - Active account: false
  - Git operations protocol: https
  - Token: (값은 기록하지 않음)
  - Token scopes: 'delete_repo', 'gist', 'read:org', 'repo', 'workflow'
```

활성 계정은 `modoofighting`이다. `art-ruby`는 로그인 상태지만 현재 비활성 계정이다.