# workspace.json 파일 Git 제외하기

## 1. .gitignore 파일에 추가
```bash
# 프로젝트 루트의 .gitignore 파일에 추가
workspace.json

# 특정 경로의 workspace.json만 제외하고 싶은 경우
path/to/workspace.json
```

## 2. 이미 Git에 추적된 경우 제거 방법
이미 Git에 추적되고 있는 workspace.json 파일이라면:

```bash
# Git 캐시에서 파일 제거 (파일은 유지)
git rm --cached workspace.json

# 여러 경로의 workspace.json 파일을 한번에 제거
git rm --cached **/workspace.json

# 변경사항 커밋
git commit -m "Remove workspace.json from git tracking"
```

## 3. workspace.json 템플릿 관리 (선택사항)
팀원들과 공유가 필요한 경우:

```bash
# workspace.json.example 파일 생성하여 공유
cp workspace.json workspace.json.example
```

그리고 .gitignore에:
```bash
workspace.json
!workspace.json.example
```

이렇게 하면 workspace.json은 Git에서 제외되고, 필요한 경우 예제 파일을 통해 팀원들과 설정을 공유할 수 있습니다.
