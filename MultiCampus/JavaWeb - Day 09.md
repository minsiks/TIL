# 5/20 JavaWeb Day9

## AJAX

### 1. JASON을 활용한 AJAX

#### 데이터 랜덤하게 제이슨으로 받아 비동기화로 내보내기

> [Oven - HTML5-Powered Web/App Prototyping Tool (ovenapp.io)](https://ovenapp.io/)
>
> [데이터 분석 사이트](https://www.highcharts.com/)

- aj05.html

```html
<meta charset="UTF-8">
<style>
	#result{
		border: 2px solid red;
	}
</style>
<script>
function display(data){
	var str = '';
	$(data).each(function(index,item){
		str += '<h3>';
		str += item.id+' '+item.name+' '+item.age;
		str += '</h3>';
		
	});
	$('#result').html(str);
};

function getdata(){
	$.ajax({
		url:'getdata',
		success:function(data){
			// [{},{},....]
			// JSON
			display(data);
		}
	});
};

$(document).ready(function(){
	getdata();
	setInterval(() => {
		getdata();		
	}, 3000);	

});
</script>




<h1>AJ 05</h1>
<button>GET DATA</button>
<div id="result"></div>
```

- AJAXController.java

```java
	@RequestMapping("/getdata")
	public Object getdata() {
		//[{},{},{},....] JSON
		JSONArray ja = new JSONArray();
		for(int i=0; i<6; i++) {
			JSONObject jo = new JSONObject();
			jo.put("id", "id0"+i);
			jo.put("name", "james"+i);
			Random r = new Random();
			int a = r.nextInt(50)+1;
			jo.put("age", a);
			ja.add(jo);
		}
		return ja;
	}
```

### 수행 평가 

> 5월 24일 오후에 발표

1. 주제 선정
   - 구인구직 사이트

2. 화면 설계 - https://ovenapp.io
3. 테마 서치
4. 개발 환경 구축
5. 시스템 개발