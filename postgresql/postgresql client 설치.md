# PostgreSQL 윈도우 설치 및 환경변수 설정

## 1. PostgreSQL 설치

### 1.1. 다운로드
- PostgreSQL 공식 웹사이트 (https://www.postgresql.org/download/windows/) 방문
- Windows 버전에 맞는 installer 다운로드
- EnterpriseDB 설치 관리자 선택 (추천)

### 1.2. 설치 과정
1. 다운로드한 설치 파일 실행
2. 설치 옵션 선택:
   - PostgreSQL Server
   - pgAdmin 4 (GUI 관리 도구)
   - Command Line Tools
   - Stack Builder
3. 설치 디렉토리 선택 (기본: `C:\Program Files\PostgreSQL\{version}`)
4. 데이터 디렉토리 선택
5. 데이터베이스 superuser(postgres) 비밀번호 설정
6. 포트 번호 설정 (기본: 5432)
7. 설치 진행

## 2. 환경변수 설정

### 2.1. 자동 설정 방법
- PostgreSQL 설치 시 "Command Line Tools" 옵션을 선택했다면 자동으로 환경변수가 설정됨

### 2.2. 수동 설정 방법
1. 시스템 속성 열기
   - Windows 키 + R → sysdm.cpl 입력
   - 또는 제어판 → 시스템 → 고급 시스템 설정

2. 환경 변수 설정
   ```
   Path 변수에 추가할 경로:
   C:\Program Files\PostgreSQL\{version}\bin
   C:\Program Files\PostgreSQL\{version}\lib
   ```

3. 구체적인 단계:
   - '환경 변수' 버튼 클릭
   - 시스템 변수에서 'Path' 선택
   - '편집' 버튼 클릭
   - '새로 만들기' 클릭하여 위의 경로들 추가
   - 확인 버튼으로 저장

## 3. 설치 확인

### 3.1. 명령 프롬프트에서 확인
```cmd
# PostgreSQL 버전 확인
psql --version

# 데이터베이스 접속
psql -U postgres
```

### 3.2. pgAdmin 4 실행
- 시작 메뉴에서 pgAdmin 4 실행
- 처음 실행 시 마스터 비밀번호 설정
- 서버 연결 시 설치할 때 설정한 superuser 비밀번호 사용

## 4. 주의사항
- 설치 시 입력한 superuser(postgres) 비밀번호 기억하기
- 방화벽에서 5432 포트 확인 (필요한 경우 열기)
- pgAdmin 4는 웹 기반으로 동작하므로 브라우저가 자동으로 실행됨
- 환경변수 설정 후 명령 프롬프트 재시작 필요

## 5. 문제 해결
- 'psql is not recognized' 오류
  - 환경변수가 제대로 설정되었는지 확인
  - Path 변수에 bin 디렉토리 경로가 제대로 추가되었는지 확인
  - 명령 프롬프트 재시작

- 연결 오류
  - PostgreSQL 서비스가 실행 중인지 확인
  - 서비스 앱에서 'postgresql-x64-{version}' 서비스 상태 확인
  - 필요한 경우 서비스 재시작


