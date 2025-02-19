# PostgreSQL Client Docker Image

한국 시간대가 설정된 PostgreSQL 클라이언트 Docker 이미지입니다.

## 이미지 빌드 방법
docker build -t postgresql-client-kr .

..또는 Dockerfile 이름이 다르다면..

docker build -t postgresql-client-kr -f Dockerfile_postgresql_client .

## 실행 방법

### 1. 기본 실행
docker run --rm postgresql-client-kr psql -h [호스트] -p [포트] -U [사용자] -d [데이터베이스]

### 2. VACUUM 실행 예시
docker run --rm postgresql-client-kr vacuumdb -h [호스트] -p [포트] -U [사용자] -d [데이터베이스] --analyze --verbose

### 3. 환경변수 설정 예시
docker run --rm \
-e PGHOST=[호스트] \
-e PGPORT=[포트] \
-e PGUSER=[사용자] \
-e PGPASSWORD=[비밀번호] \
postgresql-client-kr psql -d [데이터베이스]

## 주요 옵션 설명

- `--rm`: 컨테이너 실행 후 자동 삭제
- `-e`: 환경변수 설정
- `-h`: 데이터베이스 호스트
- `-p`: 포트
- `-U`: 사용자
- `-d`: 데이터베이스 이름
- `--analyze`: 통계 정보 업데이트
- `--verbose`: 상세 출력


## Dockerfile
```
FROM litmuschaos/postgresql-client

# 타임존 설정
ENV TZ=Asia/Seoul

# 타임존 데이터 설치 및 설정
RUN apk add --no-cache tzdata && \
    cp /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    apk del tzdata

# 환경변수 설정으로 로케일 지정
ENV LANG=ko_KR.UTF-8
```
