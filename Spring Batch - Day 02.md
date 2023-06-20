# 23/6/9 Spring Batch Day02

## 스프링 배치

### 배치 애플리케이션

- 개발자가 정의한 작업을 한번에 일괄 처리하는 애플리케이션

- 배치를 적용한 애플리케이션
  - 매출 데이터를 이용한 일매출 집계
  - 매우 큰 데이터를 활용한 보험급여 결정
  - 트랜잭션 방식으로 포맷, 유효성 확인 및 처리가 필요한 내부 및 외부 시스템에서 수신한 정보를 기록 시스템으로 통합 등등..
- 웹 크롤러
- 메일 구독 서비스
- 배치 애플리케이션의 조건
  - 대용량 데이터
  - 자동화
  - 견고성
  - 신뢰성
  - 성능
