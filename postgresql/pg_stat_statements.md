PostgreSQL 13 버전부터는 실제로 컬럼명이 다릅니다. 수정된 버전을 제시해드리겠습니다:

````markdown:obsidian/postgresql-tip.md
// ... existing code ...

### 1. 가장 많은 시간을 소요한 쿼리 확인
```sql
SELECT 
    round(total_exec_time::numeric, 2) as total_time,
    calls,
    round(mean_exec_time::numeric, 2) as mean,
    round((100 * total_exec_time / sum(total_exec_time::numeric) over ())::numeric, 2) as percentage,
    substring(query, 1, 200)
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 10;
```

### 2. 가장 많이 실행된 쿼리 확인
```sql
SELECT 
    calls,
    round(total_exec_time::numeric, 2) as total_time,
    round(mean_exec_time::numeric, 2) as mean,
    round((100 * calls / sum(calls) over ())::numeric, 2) as percentage,
    substring(query, 1, 200)
FROM pg_stat_statements
ORDER BY calls DESC
LIMIT 10;
```

### 3. 평균 실행 시간이 긴 쿼리 확인
```sql
SELECT 
    round(mean_exec_time::numeric, 2) as mean,
    calls,
    round(total_exec_time::numeric, 2) as total_time,
    substring(query, 1, 200)
FROM pg_stat_statements
WHERE calls > 100  -- 의미있는 샘플 수를 위해 최소 호출 횟수 설정
ORDER BY mean_exec_time DESC
LIMIT 10;
```

## 주요 컬럼 설명

- `total_exec_time`: 쿼리 실행에 소요된 총 시간 (밀리초)
- `calls`: 쿼리가 실행된 총 횟수
- `mean_exec_time`: 평균 실행 시간 (total_exec_time / calls)
// ... existing code ...
````

PostgreSQL 13 이상 버전에서는 다음과 같은 주요 변경사항이 있었습니다:
- `total_time` → `total_exec_time`
- `mean_time` → `mean_exec_time`

이렇게 컬럼명이 변경되었으며, 더 명확한 의미를 전달하도록 개선되었습니다. 또한 실행 계획 시간(`plan_time`)과 실행 시간(`exec_time`)이 분리되어 더 상세한 성능 분석이 가능해졌습니다.



rds에서 스냅샷으로 복구한 경우에는 pg_stat_statements 데이터도 기존 데이터를 유지하나요?

RDS에서 스냅샷 복구와 pg_stat_statements의 관계에 대해 설명드리겠습니다:

## RDS 스냅샷 복구와 pg_stat_statements

### 동작 방식
- pg_stat_statements의 통계 데이터는 메모리에 저장되며 휘발성
- 스냅샷 복구 시 통계 데이터는 초기화됨
- 새로운 인스턴스에서 처음부터 통계 수집 시작

### 복구 후 확인사항
```sql
-- 통계 수집이 정상적으로 시작되었는지 확인
SELECT count(*) from pg_stat_statements;

-- 확장 모듈이 활성화되어 있는지 확인
SELECT * FROM pg_extension WHERE extname = 'pg_stat_statements';
```

### 주의사항
1. 복구 직후
- 통계 데이터가 없으므로 성능 분석에 필요한 데이터 축적 시간 필요
- 최소 24시간 이상의 데이터 수집 권장

2. 모니터링 시스템
- 복구 후 통계가 초기화되므로 모니터링 시스템의 경고 임계값 조정 필요
- 이전 통계와 비교 분석 시 이 점을 고려해야 함
// ... existing code ...
`````

pg_stat_statements는 메모리 기반 통계이므로, RDS 스냅샷으로 복구하더라도 이전 통계 데이터는 유지되지 않습니다. 새로운 인스턴스에서는 처음부터 통계를 수집하게 됩니다.
