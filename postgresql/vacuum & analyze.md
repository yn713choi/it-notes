vacuumdb -h 192.168.1.100 -p 5432 -U postgres -v -f -z -t public.table_name dbname

vacuumdb -h 192.168.1.100 -p 5432 -U postgres -v -d dbname --analyze --jobs=4

vacuum (verbose, analyze, parallel 4);

# VACUUM 명령어 병렬 처리 옵션

1. 명령줄 유틸리티:
   - vacuumdb --jobs=4
   - 여러 테이블 동시 처리 가능

2. SQL 명령어:
   - VACUUM (PARALLEL 4)
   - jobs 옵션 없음
   - parallel 옵션으로 단일 테이블 내부 병렬 처리

# 여러 테이블 동시 VACUUM 처리 방법

1. vacuumdb 유틸리티 사용 (권장)
   ```bash
   vacuumdb --jobs=4 -d database_name
   ```

2. 별도 세션으로 처리
   - 각각 다른 세션에서 VACUUM 실행
   - 수동으로 여러 세션 열어서 처리

3. 배치 스크립트 작성
   ```bash
   psql -c "VACUUM table1;" &
   psql -c "VACUUM table2;" &
   ```

주의: SQL의 VACUUM 명령어 자체는 단일 테이블만 처리 가능

# VACUUM 실행 시 타임아웃 방지

1. statement_timeout 설정 변경
   ```sql
   -- 세션에서 타임아웃 해제
   SET statement_timeout = 0;
   ```

2. vacuumdb 옵션 사용
   ```bash
   # --echo 옵션으로 실행되는 명령 확인 가능
   vacuumdb --jobs=4 -d database_name --statement-timeout=0 --echo
   ```

3. 환경변수 설정
   ```bash
   export PGOPTIONS='-c statement_timeout=0'
   vacuumdb --jobs=4 -d database_name
   ```

주의사항:
- idle_in_transaction_session_timeout
- lock_timeout
- statement_timeout
위 세 가지 타임아웃 설정 모두 고려 필요

#postgresql
