# 6/3 Spring Framework Day 05

## SPRINGBOOT

> html5 Templete 변경 window - Properties- web - html files -Editor - Templates -html5 eddit
>
> - 팀 리프 설정
>
> <html xmlns:th="http://www.thymeleaf.org">
>
> - jquery 설정
>
> <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>

### 1. ajax를 이용한 차트 생성 및 데이터 연동

> [Highcharts | Highcharts.com](https://www.highcharts.com/demo)

- AJAXController.java

```java
package com.multi.controller;

import java.util.List;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.multi.biz.ProductBiz;
import com.multi.vo.ProductVO;

@RestController
public class AJAXController {
	
	@Autowired
	ProductBiz pbiz;
	
	@RequestMapping("/chartimpl")
	public Object chartimpl() {
		 //{'cate':['p1','p2','p3','p4','p5'],'data':[10000,30000,20000,50000,60000]}
		JSONObject jo = new JSONObject();
		JSONArray ja1 = new JSONArray();
		JSONArray ja2 = new JSONArray();
		List<ProductVO> list =null;
		
		try {
			list = pbiz.get();
		} catch (Exception e) {
			e.printStackTrace();
		}
		for (ProductVO p : list) {
			ja1.add(p.getName());
			ja2.add(p.getPrice());
		}
		jo.put("cate", ja1);
		jo.put("data", ja2);
		return jo;
	}
}
```

- chart.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="https://code.highcharts.com/highcharts.js"></script>
<script src="https://code.highcharts.com/modules/exporting.js"></script>
<script src="https://code.highcharts.com/modules/export-data.js"></script>
<script src="https://code.highcharts.com/modules/accessibility.js"></script>
<style>
	#container{
		width:500px;
		height:400px;
		border:2px solid red;
	}
</style>
<script>
function display(gdata){
	Highcharts.chart('container', {
	    chart: {
	        type: 'line'
	    },
	    title: {
	        text: 'My Shop'
	    },
	    subtitle: {
	        text: 'Pants Shop'
	    },
	    xAxis: {
	        categories: gdata.cate
	    },
	    yAxis: {
	        title: {
	            text: 'Price'   
	    }
	    },
	    plotOptions: {
	        line: {
	            dataLabels: {
	                enabled: true
	            },
	            enableMouseTracking: false
	        }
	    },
	    series: [{
	        name: 'Pants',
	        data: gdata.data
	    }]
	});
};

function getData(){
	//var gdata = {'cate':['p1','p2','p3','p4','p5'],'data':[10000,30000,20000,50000,60000]};
	//display(gdata);
	$.ajax({
		url:'chartimpl',
		success:function(data){
			display(data);
		}
	});
};

$(document).ready(function(){
	getData();
});
</script>

</head>
<body>
	<h1>Chart Page</h1>
	<div id="container"></div>
</body>
</html>
```

> 다시 프로젝트 생성 처음부터 환경설정 복기
>
> 1. spring boot project 생성
>    1. Springboot dev
>    2. spring web
>    3. thymeleaf
>    4. Mybatis
>    5. Mysql Driver
>    6. lombok
> 2. application.properties 수정
>    1. port 정보
>    2. Mysql 정보
>    3. Mybatis 정보
> 3. pom.xml 에 library 추가
>    1. servlet, tomcat, json
> 4. package setting

### 2. 디렉토리 정리

### 3. 인클루드

### 4. 부트스트랩 사용

> [Tutorial: Using Thymeleaf](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#setting-attribute-values)