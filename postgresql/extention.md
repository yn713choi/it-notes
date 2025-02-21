

PostgreSQL extension 설치 관련 명령어를 정리해드리겠습니다:

```markdown:it-notes/postgresql/extention.md
# PostgreSQL Extension 관리

## 1. Extension 조회
```sql
-- 설치 가능한 extension 목록 조회
SELECT * FROM pg_available_extensions;

-- 현재 설치된 extension 목록 조회
SELECT * FROM pg_extension;

-- 특정 데이터베이스의 설치된 extension 조회
\dx
```

## 2. Extension 설치
```sql
-- 기본 설치 구문
CREATE EXTENSION IF NOT EXISTS extension_name;

-- 특정 스키마에 설치
CREATE EXTENSION IF NOT EXISTS extension_name SCHEMA schema_name;

-- 자주 사용되는 extension 예시
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";      -- UUID 생성
CREATE EXTENSION IF NOT EXISTS "pg_stat_statements";  -- 쿼리 통계
CREATE EXTENSION IF NOT EXISTS "postgis";        -- 공간 데이터
CREATE EXTENSION IF NOT EXISTS "pg_trgm";        -- 문자열 유사도
CREATE EXTENSION IF NOT EXISTS "hstore";         -- key-value 저장
```

## 3. Extension 제거
```sql
-- extension 제거
DROP EXTENSION IF EXISTS extension_name;

-- CASCADE 옵션으로 의존성 있는 객체도 함께 제거
DROP EXTENSION IF EXISTS extension_name CASCADE;
```

## 4. Extension 업데이트
```sql
-- extension 버전 업데이트
ALTER EXTENSION extension_name UPDATE;

-- 특정 버전으로 업데이트
ALTER EXTENSION extension_name UPDATE TO 'new_version';
```

## 주의사항
- Extension 설치는 데이터베이스 단위로 이루어짐
- 일부 extension은 superuser 권한 필요
- RDS의 경우 사용 가능한 extension이 제한될 수 있음
- Extension 설치 전 호환성 확인 필요
```

이 명령어들로 PostgreSQL의 extension을 관리할 수 있습니다. RDS나 Aurora를 사용하는 경우에는 일부 extension이 제한될 수 있으니 AWS 문서를 참고하시는 것이 좋습니다.
