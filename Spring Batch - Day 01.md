# 22/5/24 Spring Batch Day01

## 스프링 배치

### Webmagazine에 적용

- @EnableBatchProcessing
  - Spring부트에서 Spring Batch 기능을 활성화하기 위한 어노테이션
  - 선언을 해야 배치를 실행하기 위한 내부 Bean 들이 초기화
- JobBuilderFactory, StepBuilderFactory
  - Job과 Step 객체를 생성하기 위한 FactoryBean 등을 생성자를 통해 Injection
- Job, Step Beans
  - Job, Step Bean을 선언, .get 메서드 안의 문자열은 각 빈의 이름
- Tasklet vs Chunk
  - 실제 처리 로직이 들어가 있는 빈
  - Chunk : 세가지 클래스 빈 ItemReader, ItemProcessor, ItemWriter 를 구현하고 이미 스프링 배치에서 작성해 놓은 클래스 들이 있어 그 것을 이용
  - Tasklet : 인터페이스만 구현, 개발자가 직접 작성
