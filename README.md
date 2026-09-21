## 🏗 아키텍처
![제목 없는 다이어그램 drawio](https://github.com/weare4potato/eCommerce/assets/131866367/b6669871-fdc7-4296-a57d-70367f5ed60e)

<br>

## 🍀 주요 기술
- Language : Java 17
- Server : AWS EC2, RDS, S3, CloudFront
- CI/CD : Github Actions, Docker
- Monotoring : Actuator, Prometheus, Grafana, CloudWatch
- Database: MySQL, Redis
- Framework : Spring Boot, Spring Data JPA, Gradle, Swagger
- OpenAPI : Toss Payments

<br>

## 🗣️ 기술적 의사결정
- Redis 
  - 인 메모리 기반의 빠른 데이터 접근과 서버 확장 시 공유 가능한 저장소로 활용할 수 있어 채택
  - 캐싱을 통한 조회 성능 개선 및 분산 환경에서의 동시성 제어에 활용
- Redisson
  - Redis 기반의 분산 락을 구현하여 동시성 제어에 활용
  - Lock 획득 대기 시간과 점유 시간을 설정할 수 있어 동시 요청을 제어하기 용이
  - Pub/Sub 기반의 Lock 대기 및 해제를 지원하여 불필요한 반복 요청을 줄일 수 있음
- Github Actions
  - 테스트 및 배포 과정을 자동화하여 반복 작업을 최소화
  - GitHub 기반의 CI/CD 환경을 구성하여 개발 및 배포 효율 향상
- CDN
  - 정적 리소스를 사용자와 가까운 서버에서 제공하여 응답 속도를 개선
  - Origin 서버에 대한 반복 요청과 트래픽 부담을 줄이기 위해 CloudFront를 활용
- Docker
  - 개발, 테스트 및 배포 환경의 일관성을 확보
  - 애플리케이션 실행 환경을 표준화하여 배포 편의성을 향상

<br>

## 🛠 트러블슈팅
- 카테고리별 상품 조회 성능 최적화
  - 복합 Index 적용으로 조회 성능 개선(3020ms -> 106ms, 약 96.5% 개선)
 
- 상품 검색 성능 개선
  - LIKE 검색으로 인한 Full Table Scan 발생
  - Full Text Index를 통해 검색 성능 개선(3990ms -> 138ms, 약 96.5% 개선)

- 장바구니 조회 성능 개선
  - Redis Cache 적용으로 조회 성능 개선(4460ms -> 199ms, 약 95.5% 개선) 

- CORS
  - 프론트엔드 연동 과정에서 PreFlight 요청 처리 문제 발생
  - Preflight 요청이 OPTIONS 메서드인지, access-control-request 가 포함되었는지 확인한 후 interceptor를 통과하도록 설정하여 해결

- 트랜잭션 범위로 인한 DeadLock
  - 주문 생성 과정에서 트랜잭션 범위가 분리되어 DeadLock 발생
  - 트랜잭션 범위를 조정하여 해결

- 주문 동시성 문제
  - 동일 상품의 재고에 대한 동시 접근으로 데이터 정합성 문제 발생
  - Redisson 기반 분산 락을 적용하여 동시성 제어
