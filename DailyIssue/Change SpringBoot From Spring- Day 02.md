# 23-2-3 Change SpringBoot From Spring- Day 02

## @Slf4j

- Java 로깅을 위한 프레임워크
- SLF4J는 Facade(퍼사드) 디자인 패턴에 따라 만들어진 클래스
- SLF4J가 실제 사용하게 될 구현 클래스 (위에서 말한 로깅 프레임워크) 사용자가 선정
- Facade 디자인 패턴을 사용하기 덕분에, 다양한 구현체를 하나의 통일된 방식으로 사용 가능
- SLF4J 로깅에 대한 추상 레이어를 제공하는 inteface의 모음

#### 콘솔에 로그 안떴던 이유

- 로그가 길어 유지 X
- 콘솔 창 - 우클릭 - Preferences - Limit console output 해제

### Thymeleaf 문법 재정리

```html
<!DOCTYPE html>
<!-- 네임스페이스 추가 태그 -->
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>Thymeleaf</h2>
	- JSP를 대체하기 위한 Spring Boot의 Templates Engine으로 View를 담당한다. <br>
	- 특징은 html 포멧을 많이 벗어나지 않고, 태그의 옵션처럼 보일수 있게 구성<br>
	- 기능 적으로는 JSP와 거의 유사하다. (사실상 JSP 3.0이라고 생각한다.)<br>
	
	<h3>0. 설정하는 방법</h3>
	- Spring Boot 프로젝트 셋팅시 thymeleaf 의존성을 추가한다.(수동으로도 가능) <br>
	- html 태그 옵션에 xmlns 태그(네임스페이스 태그)를 추가한다. <br> 
	ex) &gt; html xmlns:th="http://www.thymeleaf.org" &lt;
	<!-- <xmlns:th="http://www.thymeleaf.org"> -->
	<hr><br>
	
	<h3>1. HTML 화면 출력하는 문법 정리</h3>
	
	<b>1) Java 문자열 출력하기 </b><br>
	 - HTML 태그 옵션값의 th:text="${}"로 java 텍스트를 출력할수 있다. <br>
	 일반 표기법1 : <span th:text="${str}"></span><br> 
	 일반 표기법2 : <span th:text=${str}></span><br> 
	 잘못된 표기법 : ${str} ※안되는 문법!! EL표기를 지원하지 않음<br> 
	 인라인 표기법 : [[${str}]]<br>
	 인라인 표기법으로 js 추가하기
	 <script type="text/javascript">
		// alert('[[${str}]]');	 	
	 </script>
	 <hr>
	 <br>
	 
	 <b>2) 문자열 결합하기 (Java + Java, HTML + Java) </b><br>
	 - 출력할 문자열의 결합이 필요한 경우 결합 가능하다. <br>
	 결합1 : <span th:text="'안녕하세요? ' + ${name} +'님이 로그인 하였습니다.'"></span><br>
	 결합2 : <span th:text="|안녕하세요? ${name}님이 로그인 하였습니다.|"></span><br>
	 결합3 : <span th:with="start='안녕하세요? ', end='님이 로그인 하였습니다.'" th:text="${start} + ${name} + ${end}"></span><br>
	 <hr><br>
	 
	 <b>3) 주소값 처리하기 = @{}</b><br>
	 - context path를 대신처리하는 표기법 <br>
	 a태그 일반 : <a th:href="@{/request}"> a태그 </a><br>
	 a태그 + 파라메터 : <a th:href="@{/request(value1=${str})}"> a태그 </a><br>
	 a태그 + 파라메터's : <a th:href="@{/request(value1=${str}, value2=${name})}"> a태그 </a><br>
	 리소스, img 가져오기 <br>
	 <img width="100px" th:src="@{/images/logo-spring.png}"> 
	 <script type="text/javascript" th:src="@{/js/jquery-3.6.0.min.js}"></script>
	 <hr><br>
	 
	 
	 <b>4) 숫자 처리하기</b><br>
	 <div th:with="x=1000000, y=123456.789, z=0.05">
	 	정수형 : <span th:text="${#numbers.formatInteger(x, 3, 'COMMA')}"></span><br>
	 	부동소수점 : <span th:text="${#numbers.formatDecimal(x, 3, 'COMMA', 2, 'POINT')}"></span><br>
	 	퍼센트 : <span th:text="${#numbers.formatPercent(z, 2, 2)}"></span><br>
	 	원화 : <span th:text="${#numbers.formatCurrency(x)}"></span><br>
	 </div>
	 <hr><br>

	 <b>5) 날짜 처리하기</b><br>
	 - 현재 날짜 출력 : <span th:text="${#dates.createNow()}" ></span><br>
	 - java 날짜 출력 : <span th:text="${date}" ></span><br>
	 - 포멧(기본) : <span th:text="${#dates.format(date)}"></span><br>
	 - 포멧(변경) : <span th:text="${#dates.format(date,'yyyy/MM/dd (E) hh:mm:ss')}"></span><br>
	 - 포멧(변경) : <span th:text="${#dates.format(date,'yyyy/MM/dd (E) a HH:mm:ss')}"></span><br>
	 - 년 : <span th:text="${#dates.year(date)}"></span><br>
	 - 월 : <span th:text="${#dates.month(date)}"></span><br>
	 - 일 : <span th:text="${#dates.day(date)}"></span><br>
	<hr><br>
	
	<b>6) 문자열 처리하기</b><br>
	- 대문자변환 : <span th:text="${#strings.toUpperCase(str)}"></span><br>
	- 빈문자확인 : <span th:text="${#strings.isEmpty(str)}"></span><br>
	- 길이 : <span th:text="${#strings.length(str)}"></span><br>
	- indexOf : <span th:text="${#strings.indexOf(str,'Boot')}"></span><br>
	<hr><br>

	<b>7-1) 객체 접근하기 (.으로)</b><br>
	- id : <span th:text="${member.id}"></span><br>
	- name : <span th:text="${member.name}"></span><br>
	- age : <span th:text="${member.age}"></span><br>
	<hr><br>

	<b>7-2) 객체 접근하기 ([]으로, 맵도 가능한 방법)</b><br>
	- id : <span th:text="${member['id']}"></span><br>
	- name : <span th:text="${member['name']}"></span><br>
	- age : <span th:text="${member['age']}"></span><br>
	<hr><br>

	<b>7-2) 객체 접근하기 (*로 접근하는 방법)</b><br>
	<div th:object="${member}">
		- id : <span th:text="*{id}"></span><br>
		- name : <span th:text="*{name}"></span><br>
		- age : <span th:text="*{age}"></span><br>
	</div>
	<hr><br>
	
	<b>8) list 접근하기 </b><br>
	- index 0 : <span th:text="${list[0]}"></span><br>
	- index 1 : <span th:text="${list[1]}"></span><br>
	- index 2 : <span th:text="${list[2]}"></span><br>
<!-- 	- index 100 : <span th:text="${list[100]}"></span><br> -->
	- 인덱스 범위 외를 접근하면 에러 발생<br>
	<hr><br>

	<b>9) Map 접근하기</b><br>
	- 없는 값 : <span th:text="${map['noValue']}"></span><br>
	- id : <span th:text="${map['id']}"></span><br>
	- name : <span th:text="${map['name']}"></span><br>
	<hr><br>


	<h3>2. 조건문 </h3>
	<b>1) 논리 연산자 <br></b>
	5 > 10 : <span th:text="5 > 10"></span><br>
	5 &lt; 10 : <span th:text="5 < 10"></span><br>
	5 >= 10 : <span th:text="5 >= 10"></span><br>
	5 &lt;= 10 : <span th:text="5 <= 10"></span><br>
	5 == 10 : <span th:text="5 == 10"></span><br>
	5 != 10 : <span th:text="5 != 10"></span><br>
	홍길동 == 홍길동 : <span th:text="${name} == 홍길동"></span><br>
	홍길동 != 홍길동 : <span th:text="${name} != 홍길동"></span><br>
	<hr><br>
	
	<b>2) 삼항연산자 <br></b>
	<div th:text="${name} == '홍길동' ? '홍길동입니다.' : '홍길동이 아닙니다.'"></div>
	<hr><br>
	
	<b>3) if문</b><br>
	<div th:if="${name} == '홍길동'">
		<p>홍길동 입니다.</p>
	</div>
	<!-- th:unless는 else을 대체할수 있다. -->
	<div th:unless="${name} != '홍길동'">
		<p>홍길동 입니다.</p>
	</div>
	<hr><br>
	
	<b>4) if문 + block문<br></b>
	block문 : 태그 없이 사용할수 있는 빈태그<br>
	<th:block th:if="${name} == '홍길동'"> 
		<p>홍길동입니다!</p> 
	</th:block>
	
	<b>5) switch문<br></b><br>
	<th:block th:switch="${name}"> 
 		<span th:case="홍길동">홍길동입니다.</span> 
  		<span th:case="최길동">최길동입니다.</span> 
	</th:block>
	<hr><br>
	
	
	<h3>3. 반복문</h3>
	<b>1) 단순 반복문</b><br>
	<div th:each="str : ${list}"> 
		<div th:text="${str}"></div>
	</div>
	<!-- th블록으로 처리하는 방법 : 태그가 남지 않게 반복시킬때 -->	
	<th:block th:each="str : ${list}"> 
		<th:block th:text="${str}"></th:block>
	</th:block>
	<hr><br>
	
	<b>2) 객체를 포함한 반복문</b><br>
	<div th:each="member : ${memberList}">
		id : <th:block th:text="${member.id}"></th:block>
		이름 : <th:block th:text="${member.name}"></th:block>
		나이 : [[${member.age}]]
	</div>
	<hr><br>
		
	<b>3) status 활용하기 <br></b>
	<div th:each="member, status : ${memberList}" th:object="${member}"> 
		index :  [[${status.index}]]<br> 
		count :  [[${status.count}]]<br>
		size :  [[${status.size}]]<br> 
		current :  [[${status.current}]]<br> 
		even :  [[${status.even}]]<br> 
		odd-> [[${status.odd}]]<br> 
		first-> [[${status.first}]]<br>  
		last-> [[${status.last}]]<br>  
		[[*{id}]] : [[*{name}]]<br> 
		<br> 
	</div>
	<hr><br>


	<h3>4. Layout</h3>
	- header, footer를 include 할경우 사용 가능하다.
	<!--
	layout.html
	 
	<div th:fragment="div1">
		<div id="div2">
			fragment div1안에 div2 입니다.
		</div>
	</div>

	<div id="div3">
		<div id="div4">
			div태그의 div3 안에 div4 입니다.
		</div>
	</div>
	 -->
	
	<div th:replace="thymeleaf/layout :: div1"></div>
	<div th:include="thymeleaf/layout :: #div3"></div>
	<hr><br>
	
	
	<h3>4. 입력용 Form 기능</h3>
	<b>1) 일반 Form으로 전송하기<br></b>
	<form th:action="@{/join}" method="post">
		id : <input type="text" name="id"> <br>
		name :<input type="text" name="name"><br>
		age :<input type="number" name="age"><br>
		<input type="submit" value="전송">
	</form>
	<br>
	
	<b>2) Update Form 작성하기<br></b>
	<form th:action="@{/join}" th:object="${member}" method="post">
		id :<input type="text" name="id" th:field="*{id}"><br>
		name :<input type="text" name="name" th:field="*{name}"><br>
		age :<input type="text" name="age" th:field="*{age}"><br>
		<input type="submit" value="전송">
	</form>
	<br>
	
	
	
	<br><br><br><br><br><br><br><br><br>
</body>
</html>
```

- th:text
  - HTML 태그 속성에서 th:text를 사용하거나, HTML 태그 속성이 아닌 밖에서 이용하려면 [[..]]를 사용하면 된다.
- th:utext
  - 태그를 반영하는 text를 출력하려면 th:utext를 사용 HTML 태그 속성이 아닌 밖에서 이용하려면 [(...)]를 사용

