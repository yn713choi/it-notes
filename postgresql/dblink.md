

dblink 사용법을 정리해서 파일에 작성하겠습니다.

```markdown:it-notes/postgresql/dblink.md
# PostgreSQL dblink 사용법

dblink는 PostgreSQL에서 다른 데이터베이스에 접근하여 쿼리를 실행할 수 있게 해주는 기능입니다.

## 1. dblink 설치
```sql
CREATE EXTENSION dblink;
```

## 2. dblink 연결 및 사용

### 2.1. 일회성 연결 사용
```sql
SELECT * FROM dblink(
    'host=127.0.0.1 port=5432 dbname=db_name user=user_name password=1234',
    'SELECT id, name FROM table_name'
) AS t1(id int, name text);
```

### 2.2. 연결 생성 후 재사용
```sql
-- 연결 생성
SELECT dblink_connect('conn_name', 
    'host=127.0.0.1 port=5432 dbname=db_name user=user_name password=1234'
);

-- 연결 사용
SELECT * FROM dblink(
    'conn_name',
    'SELECT id, name FROM table_name'
) AS t1(id int, name text);

-- 연결 종료
SELECT dblink_disconnect('conn_name');
```

## 3. 연결 관리

### 3.1. 연결 목록 확인
```sql
SELECT dblink_get_connections();
```

### 3.2. 연결 상태 확인
```sql
SELECT dblink_is_busy('conn_name');
```

## 4. 주의사항
- dblink 연결은 세션에 종속적입니다
- 세션이 종료되면 모든 연결이 자동으로 종료됩니다
- 사용하지 않는 연결은 명시적으로 종료하는 것이 좋습니다
- 연결 문자열에 비밀번호가 포함되므로 보안에 주의해야 합니다

## 5. 활용 예시

### 5.1. SELECT 쿼리
```sql
SELECT *
FROM dblink(
    'conn_name',
    'SELECT id, name, age FROM users WHERE age > 20'
) AS t1(id int, name text, age int);
```

### 5.2. INSERT/UPDATE/DELETE 쿼리
```sql
SELECT dblink_exec(
    'conn_name',
    'UPDATE users SET name = ''John'' WHERE id = 1'
);
```

참고: https://ytubelog.tistory.com/22
```
