# 5/13 JavaWeb Day4

## Ch.08 반응형 웹

### 1. 반응형 웹 소개

- 웹 페이지 하나로도 데스크톱, 태블릿PC, 스마트폰에 맞게 디자인이 자동으로 반응해서 변경되는 웹 페이지를 의미

- 메타 정보 추가

```jsp
<meta name="viewport" content="user-scalable=no,initial-scale=1,maximum-scale=1">
```

- 반응형 웹  실습

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<meta name="viewport" content="user-scalable=no,initial-scale=1,maximum-scale=1">
<title>Insert title here</title>
<style>
	/* 스마트 폰 */
	@media screen and (max-width: 767px){
		body { background-color: red;}
	}
	
	/* 태블릿PC 세로*/
	@media screen and (min-width: 768px) and (max-width: 959px){
		body { background-color: green;}
	}
	/* 데스크톱 */
	@media screen and (min-width: 960px){
		body { background-color: blue;}
	}
</style>
</head>
<body>
	<h1>Media Page</h1>
	<p>13일 국민의힘 경기도당 등에 따르면 A씨는 과천 내 한 선거구에서 지난 7일 당내 과천시의원 후보로 최종 확정됐다.
 
CBS노컷뉴스가 입수한 신천지의 '2005년도 총회보고서'를 보면 A씨의 성명이 이듬해인 2006년 신천지 '부녀회' 소속 간부인 문화부장(총 3명) 명단에 사진과 함께 올라 있다.
 
신천지 교인들의 인적사항을 담은 또 다른 교적부에서도 A씨로 보이는 인물사진은 물론, 그와 동일한 한자 이름 등이 확인됐다.
 
신상정보의 최초 입력 시점은 2007년 11월 15일, 최근 변경 일자는 2011년 9월 26일이며, 신분번호와 고유번호 등이 명시돼 있다.</p>
</body>
</html>
```

> 자주 복사하면 생기는 문제
>
> 1. porm.xml - artifactId와 name을 변경
>
> 2. 프로젝트의 properties로 가서 Web Project Settings에서 context root 변경

> * Maven 이란?
>
> - thymeleaf 란?

### jsp를 이용한 화면전환

- main.jsp

```JSP
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
<link rel="stylesheet" href="css/main.css">
<style>

</style>
</head>
<body>
<header>
	<h1><a href="/">HTML5 & CSS3.0</a></h1>
</header>
<nav>
	<ul>
		<li><a href="html5">HTML5</a></li>
		<li><a href="css3">CSS3</a></li>
		<li><a href="js">JavaScript</a></li>
		<li><a href="media">Media</a></li>
	</ul>
</nav>
<section>
	<jsp:include page="${center }.jsp"></jsp:include>
</section>
<footer>2022.05.12</footer>
</body>
</html>
```

- MainController.java

```JAVA
package com.shop.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class MainController {

	@RequestMapping("/")
	public ModelAndView main(ModelAndView mv) {
		mv.addObject("center", "center");
		mv.setViewName("main");
		return mv;
	}
	@RequestMapping("/media")
	public ModelAndView media(ModelAndView mv) {
		mv.setViewName("media");
		return mv;
	}

	@RequestMapping("/html5")
	public ModelAndView html5(ModelAndView mv) {
		mv.addObject("center", "html5");
		mv.setViewName("main");
		return mv;
	}
	@RequestMapping("/js")
	public ModelAndView js(ModelAndView mv) {
		mv.addObject("center", "js");
		mv.setViewName("main");
		return mv;
	}
	@RequestMapping("/css3")
	public ModelAndView css3(ModelAndView mv) {
		mv.addObject("center", "css3");
		mv.setViewName("main");
		return mv;
	}
}
```

- 이후 각 .jsp파일 생성

### thymeleaf를 이용한 화면전환

> jsp를 안쓴다!
>
> .html 파일을 src/main/resources 아래 templates에 저장한다.

- 팀 링크를 쓸 수 있게 만드는 코드

`<html xmlns:th="http://www.thymeleaf.org">`

- html파일 안에 섹션 코드

```html
<section th:include="${center} ? ${center} : maincenter"></section>
```

> [Color Names — HTML Color Codes](https://htmlcolorcodes.com/color-names/)

- main.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" href="css/main.css">
</head>
<body>
	<header>
		<h1><a href="/">HTML5 & CSS3.0</a></h1>
	</header>
	<nav>
		<ul>
			<li><a href="html5">HTML5</a></li>
			<li><a href="css3">CSS3</a></li>
			<li><a href="js">JavaScript</a></li>
			<li><a href="media">Media</a></li>
		</ul>
	</nav>
	<section th:include="${center}"></section>
	
	<footer>2022.05.13</footer>
</body>
</html>
```

- MainController.java

```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" href="css/main.css">
</head>
<body>
	<header>
		<h1><a href="/">HTML5 & CSS3.0</a></h1>
	</header>
	<nav>
		<ul>
			<li><a href="html5">HTML5</a></li>
			<li><a href="css3">CSS3</a></li>
			<li><a href="js">JavaScript</a></li>
			<li><a href="media">Media</a></li>
		</ul>
	</nav>
	<section th:include="${center}"></section>
	
	<footer>2022.05.13</footer>
</body>
</html>
```



### bootstrap 활용

>  -- bootstrap site --
>
> [**https://startbootstrap.com/**](https://startbootstrap.com/)
>
> [**https://templated.co/**](https://templated.co/)
>
> [**https://bootstrapmade.com**](https://bootstrapmade.com/)
>
> [**https://uicookies.com/free-bootstrap-portfolio-templates/**](https://uicookies.com/free-bootstrap-portfolio-templates/)
>
> [**https://themewagon.com/41-free-high-quality-responsive-personal-portfolio-website-html5-bootstrap-templates-2018/**](https://themewagon.com/41-free-high-quality-responsive-personal-portfolio-website-html5-bootstrap-templates-2018/)

