# 6/7 Spring Framework Day 06

## SPRINGBOOT

### 1. session 사용

- 정보를 받아서 가지고 있는 상황을 유지

- ex) login 후 id를 유지
- HttpSession session로 불러온다.

- "$session.~"로 정보를 빼낼 수 있다.
- controller.java

``` java
@RequestMapping("/loginimpl")
	public String loginimpl(Model m,String id, String pwd, HttpSession session) {
		String next ="";
		CustVO cust = null;
		try {
			cust = cbiz.get(id);
			if(cust != null) {
				if(cust.getPwd().equals(pwd)) {
					session.setAttribute("logincust", cust);
					m.addAttribute("logincust", cust);
					next = "loginok";
				}else {
					throw new Exception();
				}
			}else {
				throw new Exception();
			}
		} catch (Exception e) {
			next="loginfail";
		}
		m.addAttribute("center", next);
		return "main";
	}
```

- logouttop.html

```html
<meta charset="UTF-8">

<li><a href="/logout">
<span th:text="${session.logincust.name}"></span>
<span class="glyphicon glyphicon-log-out"></span> Logout</a></li>
```

### 2. 오토 인크리먼트로 입력된아이디 값 찾아서 표시하기

- 마지막 아이디값을 찾는 SQL문

```sql
SELECT last_insert_id() AS cnt;
```

- mybatis - mapper.xml에 함수 추가

```xml
<select id="selectcnt" resultType="int">
		SELECT last_insert_id() AS cnt
	</select>
```

- mapper - Mapper.java에 함수 추가

```java
public int selectcnt() throws Exception;
```

- biz- Biz.java 에 기능 추가

```java
public int getcnt() throws Exception{
		return dao.selectcnt();
	}
```

- controller - Controller.java에 기능 추가

```java
@RequestMapping("/registerimpl")
	public ModelAndView registerimpl(ModelAndView mv, ProductVO obj) {
			mv.setViewName("main");
			mv.addObject("left", "product/left");
			try {
				biz.register(obj);
				int cnt = biz.getcnt();
				mv.addObject("center", "product/registerok");
				mv.addObject("cnt", cnt);
			} catch (Exception e) {
				mv.addObject("center", "product/registerfail");
			}
		return mv;
	}
```

- 해당 registerok.html에 팀리프 형식 추가

```html
<p th:text="${cnt}+'번째 제품이 입력 되었습니다.'"></p>
```

### 3. Thymeleaf에서의 if-else 절

- th:if / th:unless

  :question:unless에도 똑같은 조건을 풀어 써줘야 한다.

```html
<ul class="nav navbar-nav navbar-right">
    <li th:if="${session.logincust == null}"><a href="/login">
        <span class="glyphicon glyphicon-log-in">
        </span> Login</a></li>
    <li th:unless="${session.logincust == null}"><a href="/logout">
        <span th:text="${session.logincust.name}"></span>
        <span class="glyphicon glyphicon-log-out"></span> Logout</a></li>
</ul>
```

### 4. 다국어 처리

- WebConfig.java / MessageByLocale.java / LocaleResolve.java 파일 com.multi패키지에 복사

- resources > messages 폴더 생성

- 메세지 폴더에 각 이름으로 된 파일설정

  - message_ko_KR.properties
    - {}로 argument를 추가 할 수 있다.

  ```properties
  home.welcome= \uD658\uC601\uD569\uB2C8\uB2E4.{0} {1}
  tel=010-9999-9898
  ```

  - message_en_US.properties
  - message_zn_CN.properties 

- center.html

```html
<h1 th:text="#{home.welcome('hi','kim')}">환영 합니다.</h1>
<p th:text="#{tel}">Lorem consequat.</p>
```

#### :notebook_with_decorative_cover: day WS. shoppingdb CRUD 기능 구현 및 테스트

- xml문에서 if문을 써 여러가지 상황 가정 만들기

```xml
<insert id="insert" parameterType="CateVO">
		<if test="pid != 0">
		INSERT INTO cate VALUES (#{id},#{name},#{pid})
		</if>
		<if test="pid == 0">
		INSERT INTO cate VALUES (#{id},#{name},null)
		</if>
	</insert>
```

- JOIN을 이용하여 다양한 결과 값 얻기