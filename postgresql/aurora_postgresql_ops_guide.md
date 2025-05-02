
# 📊 Aurora PostgreSQL 운영 업무 정리

## 📡 모니터링 항목

| 카테고리 | 항목 | 설명 | 도구/경로 | 알림 여부 |
|:------------|:------------|:------------|:------------|:------------|
| 성능 | CPU, 메모리, 디스크 IOPS | 자원 사용률 및 부하 상태 확인 | CloudWatch | O |
| 성능 | DB Connections 수 | 최대 연결수 근접 여부 | CloudWatch | O |
| 성능 | Replication Lag | 리드 리플리카 지연 시간 | CloudWatch / RDS 이벤트 | O |
| 쿼리 | Slow Query | 느린 쿼리 발생 여부 | Performance Insights / pg_stat_statements | O |
| 쿼리 | Lock 대기 | Lock 발생 및 대기 상태 | pg_locks / pg_stat_activity | O |
| 백업 | 자동 스냅샷 상태 | 정상 스냅샷 생성 여부 | RDS 스냅샷 | O |
| 장애 | Failover 이벤트 | 자동 장애 조치 발생 여부 | RDS 이벤트 | O |
| 스토리지 | 스토리지 사용량 | 사용량 확인 및 임계치 설정 | CloudWatch | O |
| 기타 | 테이블 bloat | autovacuum 동작 상태 및 bloat 확인 | 쿼리 | X |

## 📅 정기 업무

| 주기 | 업무 | 설명 | 도구/경로 |
|:------------|:------------|:------------|:------------|
| 일일 | 장애 로그 확인 | 에러 및 장애 로그 점검 | RDS 로그 |
| 일일 | Slow Query 점검 | 전일 기준 느린 쿼리 조회 | pg_stat_statements |
| 일일 | Replication 상태 확인 | 리플리카 지연 시간 및 정상 동작 여부 | CloudWatch |
| 주간 | autovacuum 동작 상태 확인 | vacuum, analyze 진행 현황 | pg_stat_user_tables |
| 주간 | 테이블/인덱스 bloat 점검 | bloat 발생 테이블 확인 | 쿼리 |
| 주간 | 주요 쿼리 튜닝 | 쿼리 실행 계획 검토 및 개선 | Performance Insights |
| 주간 | 커넥션 수, 사용량 점검 | 최대 연결수 및 커넥션 상태 점검 | CloudWatch |
| 월간 | 리소스 사용량 리포트 | 자원 사용 트렌드 리포트 | CloudWatch |
| 월간 | DB parameter 검토 | 파라미터 변경 여부 검토 | RDS Parameter Group |
| 월간 | 백업 정책 검토 | 스냅샷 보존 기간 및 복구 테스트 여부 | RDS Snapshot |

## 🚨 변동사항 대응

| 상황 | 대응 업무 | 설명 | 도구/경로 |
|:------------|:------------|:------------|:------------|
| 신규 테이블/인덱스 생성 | 권한, storage, vacuum 정책 확인 | 권한/설계 점검 | DDL 리뷰 |
| 스키마 변경 | 영향도 분석 및 적용 | 트랜잭션 영향 검토 | 쿼리 / 서비스 영향도 |
| 성능 장애 발생 | 쿼리 튜닝, 인덱스 추가 | 원인 분석 후 조치 | Performance Insights |
| 리소스 과부하 | 커넥션 조정, 쿼리 킬 | max connections, 세션 킬 | CloudWatch / pg_stat_activity |
| 리플리카 장애 | 리플리카 재배포 | 장애 원인 분석 후 재생성 | RDS 콘솔 |
| parameter 변경 요청 | 영향도 분석 및 변경 승인 | 테스트 및 적용 | Parameter Group |
