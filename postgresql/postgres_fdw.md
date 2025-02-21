# PostgreSQL Foreign Data Wrapper (postgres_fdw)

postgres_fdw는 외부 PostgreSQL 데이터베이스의 테이블을 로컬 테이블처럼 접근할 수 있게 해주는 Foreign Data Wrapper입니다.

## 1. 사용 목적
- 외부 PostgreSQL DB의 테이블을 로컬 테이블처럼 투명하게 사용
- 쿼리 최적화 가능
- 트랜잭션 통합 관리
- 여러 데이터베이스의 데이터를 효율적으로 통합

## 2. 설치 및 기본 설정

### 2.1. 확장 설치
```sql
CREATE EXTENSION postgres_fdw;
```

### 2.2. 외부 서버 정의
```sql
CREATE SERVER foreign_server
FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (
    host 'remote_host',
    port '5432',
    dbname 'remote_db'
);
```

### 2.3. 사용자 매핑 생성
```sql
CREATE USER MAPPING FOR local_user
SERVER foreign_server
OPTIONS (
    user 'remote_user',
    password 'remote_password'
);
```

## 3. 외부 테이블 생성 및 사용

### 3.1. 외부 테이블 생성
```sql
CREATE FOREIGN TABLE foreign_table (
    id integer,
    name text,
    created_at timestamp
)
SERVER foreign_server
OPTIONS (
    schema_name 'public',
    table_name 'remote_table'
);
```

### 3.2. 외부 테이블 사용
```sql
-- 일반 테이블처럼 사용 가능
SELECT * FROM foreign_table;

-- JOIN 사용 예시
SELECT ft.*, lt.*
FROM foreign_table ft
JOIN local_table lt ON ft.id = lt.foreign_id;
```

## 4. 관리 명령어

### 4.1. 외부 테이블 정보 확인
```sql
-- 외부 서버 목록 조회
SELECT * FROM pg_foreign_server;

-- 사용자 매핑 확인
SELECT * FROM pg_user_mappings;

-- 외부 테이블 목록
SELECT * FROM pg_foreign_table;
```

### 4.2. 외부 테이블 삭제
```sql
DROP FOREIGN TABLE foreign_table;
DROP SERVER foreign_server CASCADE;
```

## 5. dblink와의 주요 차이점

### 5.1. 구조적 차이
- **postgres_fdw**
  - 외부 테이블을 로컬 테이블처럼 직접 매핑
  - 쿼리 플래너가 최적화 수행 가능
  - 영구적인 테이블 매핑 구조

- **dblink**
  - 세션 기반의 임시 연결
  - 독립적인 쿼리 실행
  - 동적 쿼리에 적합

### 5.2. 성능 차이
- **postgres_fdw**
  - 쿼리 최적화 가능
  - 더 나은 성능 제공
  - 효율적인 데이터 전송

- **dblink**
  - 각 쿼리가 독립적으로 실행
  - 최적화가 제한적
  - 추가적인 연결 오버헤드

### 5.3. 사용 케이스
- **postgres_fdw 사용 시기**
  - 지속적인 외부 데이터 접근이 필요할 때
  - 쿼리 성능이 중요할 때
  - 트랜잭션 통합이 필요할 때

- **dblink 사용 시기**
  - 임시적인 연결이 필요할 때
  - 동적 쿼리 실행이 필요할 때
  - 독립적인 트랜잭션이 필요할 때

## 6. 네트워크 설정

### 6.1. 방화벽/보안그룹 설정
- 원격 PostgreSQL 서버의 5432 포트(또는 사용자 지정 포트)를 허용해야 함
- 로컬 서버 → 원격 서버로의 아웃바운드 트래픽 허용
- 원격 서버 → 로컬 서버로의 인바운드 트래픽 허용

### 6.2. PostgreSQL 설정
1. postgresql.conf 파일 수정
```conf
listen_addresses = '*'  # 또는 특정 IP 주소
```

2. pg_hba.conf 파일에 원격 접속 허용 설정 추가
```conf
# IPv4 connections
host    all    all    로컬_서버_IP/32    md5
```

### 6.3. 연결 테스트
```sql
-- postgres_fdw의 경우
SELECT * FROM pg_foreign_server;

-- dblink의 경우
SELECT dblink_connect('connection_test',
    'host=remote_host port=5432 dbname=remote_db user=remote_user password=remote_password'
);
```

### 6.4. 문제 해결
- 연결 실패 시 확인사항:
  1. 방화벽/보안그룹 설정
  2. PostgreSQL 리스닝 주소 설정
  3. pg_hba.conf 설정
  4. 네트워크 연결성 (telnet 또는 nc 명령어로 테스트)
