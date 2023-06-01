# 22/5/22 SpringBoot & JPA Day01

## 실정! 스프링 부트와 JPA 활용1 - 웹 어플리케이션 개발

> 인프런 김영한

### 환경 설정

- Gradle
- 스프링 부트 3.1.0
- Java 17
- Dependencies
  - Spring Web
  - Thymeleaf
  - Spring Data JPA
  - H2 Database
  - Lombok

### 의존 관계 라이브러리

- Srping web

  - Tomcat - 스프링부트 내장 서

- Thymeleaf

- jpa

  - Srping jdbc
    - HikariCP

  - hibernate
  - spring-data-jpa
  - logging
    - logback
    - slf4j

핵심 : 스프링 MVC, 스프링 ORM, JPA, 하이버네이트, 스프링 데이터 JPA

기타 : H2 데이터베이스 클라이언트, 커넥션 풀 : HikariCP, WEB(thymeleaf), 로깅

### application.yml 설정

- application.properties 삭제 후 application.yml 생성

```yaml
spring:
  datasource:
    url: jdbc:h2:tcp://localhost/~/jpashop;
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
#        show_sql: true
        format_sql: true

logging:
  level:
    org.hibernate.SQL: debug
    org.hibernate.orm.jdbc.bind: trace <- sql쿼리 안에파라미터 값들 확인
```

- build.gradle 설정

```gradle
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.0.0'
	id 'io.spring.dependency-management' version '1.1.0'
}

group = 'jpbook'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'
targetCompatibility = '17'

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-devtools'

	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.8.1' // sql 쿼리 파라미터 알려주는 외부 라이브러리 

	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
	useJUnitPlatform()
}

```

- 스프링부트 3.0 이상부터는 resources 밑에 META-INF.spring폴더 생성 후 `org.springframework.boot.autoconfigure.AutoConfiguration.imports` 파일 생성 

  - 내용:  `com.github.gavlyukovskiy.boot.jdbc.decorator.DataSourceDecoratorAutoConfiguration`

- resources 폴더 밑에 spy.properties 파일 생성

  - 내용 : `appender=com.p6spy.engine.spy.appender.Slf4JLogger`

- 추가 사항 (줄 바꿈 등 편하게 하기 위함)

  - java-jpabook.jpashop에 P6SpySqlFormatter 클래스 추가

  - ```java
    package jpbook.jpashop;
    
    import com.p6spy.engine.logging.Category;
    import com.p6spy.engine.spy.P6SpyOptions;
    import com.p6spy.engine.spy.appender.MessageFormattingStrategy;
    import jakarta.annotation.PostConstruct;
    import org.hibernate.engine.jdbc.internal.FormatStyle;
    import org.springframework.context.annotation.Configuration;
    
    import java.util.Locale;
    
    @Configuration
    public class P6SpySqlFormatter implements MessageFormattingStrategy {
    
        @PostConstruct
        public void setLogMessageFormat() {
            P6SpyOptions.getActiveInstance().setLogMessageFormat(this.getClass().getName());
        }
    
        @Override
        public String formatMessage(int connectionId, String now, long elapsed, String category, String prepared, String sql, String url) {
            sql = formatSql(category, sql);
            return String.format("[%s] | %d ms | %s", category, elapsed, formatSql(category, sql));
        }
    
        private String formatSql(String category, String sql) {
            if (sql != null && !sql.trim().isEmpty() && Category.STATEMENT.getName().equals(category)) {
                String trimmedSQL = sql.trim().toLowerCase(Locale.ROOT);
                if (trimmedSQL.startsWith("create") || trimmedSQL.startsWith("alter") || trimmedSQL.startsWith("comment")) {
                    sql = FormatStyle.DDL.getFormatter().format(sql);
                } else {
                    sql = FormatStyle.BASIC.getFormatter().format(sql);
                }
                return sql;
            }
            return sql;
        }
    }
    ```
