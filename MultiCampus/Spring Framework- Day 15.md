# 6/21 Spring Framework Day 15

### [mybatis Logger]

### 1. maven library 추가

```xml
<!-- log4j2 -->   
<dependency>
	<groupId>org.bgee.log4jdbc-log4j2</groupId>
	<artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
	<version>1.16</version>
</dependency>
```

### 2. src/main/resources 폴더 아래 logback.xml 생성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

	<appender name="STDOUT"
		class="ch.qos.logback.core.ConsoleAppender">
		<encoder>
			<pattern>%d{yyyyMMdd HH:mm:ss.SSS} [%thread] %-3level %logger{5}-%msg %n</pattern>
		</encoder>
	</appender>
	
	<logger name="jdbc" level="OFF" />
	<logger name="jdbc.sqlonly" level="DEBUG" />
	<logger name="jdbc.sqltiming" level="DEBUG" />
	<logger name="jdbc.audit" level="OFF" />
	<logger name="jdbc.resultset" level="DEBUG" />
	<logger name="jdbc.resultsettable" level="DEBUG" />
	<logger name="jdbc.connection" level="OFF" />

	
	<root level="DEBUG">
		<appender-ref ref="STDOUT" />
	</root>

</configuration>
```

### *서비스를 할시에는 root level을 INFO로 바꾸어 준다.

### *그외 상황에서는 DEBUG를 해준다.

