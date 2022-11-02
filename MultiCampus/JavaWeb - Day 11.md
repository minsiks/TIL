# 5/25 JavaWeb Day11

## KAKAO MAP 

> [깃헙 프로젝트 정리 예](https://github.com/rcm2000/BigdataProject02)
>
> 다른 사람 프로젝트 보고 느낀점
>
> - 고화질의 이미지가 전체 화면을 덮을때의 폭력성이 있다.
>
> 아작스로 가져온다라는 말의 의미란?
>
> 제이스로 내려온다라는 말의 의미란?

### 라이브러리

- 라이브러리를 추가하려면 키값 옆에 &libraries=services를 추가해야한다.

### 마커에 마우스 오버 아웃 이벤트 등록

- aj09.html

```html
<meta charset="UTF-8">
<style>
	#map{
		width:500px;
		height:400px;
	}
</style>
<script>
$(document).ready(function(){
	var mapContainer = document.getElementById('map'), // 지도를 표시할 div 
    mapOption = { 
        center: new kakao.maps.LatLng(33.450701, 126.570667), // 지도의 중심좌표
        level: 3 // 지도의 확대 레벨
    };

	var map = new kakao.maps.Map(mapContainer, mapOption); // 지도를 생성합니다

	// 마커를 표시할 위치입니다 
	var position =  new kakao.maps.LatLng(33.450701, 126.570667);

	// 마커를 생성합니다
	var marker = new kakao.maps.Marker({
	  position: position
	});

	// 마커를 지도에 표시합니다.
	marker.setMap(map);
	
	// 마커에 커서가 오버됐을 때 마커 위에 표시할 인포윈도우를 생성합니다
	var iwContent = '<div style="padding:5px;">Hello World!</div><h3><a href="">GO</a></h3><img width="50px" src="img/bass.jpg">'; // 인포윈도우에 표출될 내용으로 HTML 문자열이나 document element가 가능합니다

	// 인포윈도우를 생성합니다
	var infowindow = new kakao.maps.InfoWindow({
	    content : iwContent
	});
	
	// 마커에 마우스오버 이벤트를 등록합니다
	kakao.maps.event.addListener(marker, 'mouseover', function() {
	  // 마커에 마우스오버 이벤트가 발생하면 인포윈도우를 마커위에 표시합니다
	    infowindow.open(map, marker);
	});

	// 마커에 마우스아웃 이벤트를 등록합니다
	kakao.maps.event.addListener(marker, 'mouseout', function() {
	    // 마커에 마우스아웃 이벤트가 발생하면 인포윈도우를 제거합니다
	    infowindow.close();
	});
	// 마커에 마우스아웃 이벤트를 등록합니다
	kakao.maps.event.addListener(marker, 'click', function() {
	    // 마커에 마우스아웃 이벤트가 발생하면 인포윈도우를 제거합니다
	    location.href="aj06";
	});
});
</script>
<h1>AJ09</h1>

<div id ="map"></div>
```

### 여러개의 마크 표시하기

```html
<meta charset="UTF-8">
<style>
	#map{
		width:500px;
		height:400px;
	}
</style>
<script>
var map = null;

function displaymarker(pos){
	$(pos).each(function(index,item){
		var marker = new kakao.maps.Marker({
			map:map,
			position: new kakao.maps.LatLng(item.lat,item.lng)
		});	
		var infowindow = new kakao.maps.InfoWindow({
			content : item.content
		});
		kakao.maps.event.addListener(marker, 'mouseover', function() {
			    infowindow.open(map, marker);
		});
		kakao.maps.event.addListener(marker, 'mouseout', function() {
		    infowindow.close();
		});
		kakao.maps.event.addListener(marker, 'click', function() {
		    // 마커에 마우스아웃 이벤트가 발생하면 인포윈도우를 제거합니다
		    location.href=item.target;
		});
	});
};

function getmarkers(loc){
	var pos = null;
	pos = [
	    {
	        content: '<div>카카오1</div>', 
	        lat: 37.50361692365908,
	        lng: 126.93957178013711,
	        target: 'js01'
	    },
	    {
	    	content: '<div>카카오2</div>', 
	        lat: 37.51291692365908,
	        lng: 126.99287178013711,
	        target: 'js02'
	    },
	    {
	    	content: '<div>카카오3</div>', 
	        lat: 37.52671692365908,
	        lng: 126.94787178013711,
	        target: 'js03'
	    },
	    {
	    	content: '<div>카카오4</div>', 
	        lat: 37.59741692365908,
	        lng: 126.90637178013711,
	        target: 'js04'
	    }
	];
	displaymarker(pos);
};
function gomap(loc){
	var latlng = null;
	if(loc == 's'){
		latlng = new kakao.maps.LatLng(37.55041692365908, 126.91037178013711)
		getmarkers('s');
	}else if(loc == 'b'){
		latlng = new kakao.maps.LatLng(35.17642453774257, 129.16669784099807)
		getmarkers('b');
	}else if(loc == 'g'){
		latlng = new kakao.maps.LatLng(35.16173425533525, 126.88758871719189)
		getmarkers('g');
	}
	map.panTo(latlng);
};
function displaymap(){
	var mapContainer = document.getElementById('map'); // 지도를 표시할 div 
    var mapOption = { 
        center: new kakao.maps.LatLng(37.5032909, 127.0498321), // 지도의 중심좌표
        level: 8 // 지도의 확대 레벨
    };
	map = new kakao.maps.Map(mapContainer, mapOption); // 지도를 생성합니다 
	
};

//#서울 37.55041692365908, 126.91037178013711
//#부산 35.17642453774257, 129.16669784099807
//#광주 35.16173425533525, 126.88758871719189

$(document).ready(function(){
	displaymap();
	
	$('#s').click(function(){
		gomap('s');
	});
	$('#b').click(function(){
		gomap('b');
	});
	$('#g').click(function(){
		gomap('g');
	});
	
});
</script>

<h1>AJ 10</h1>
<button id = "s">Seoul</button>
<button id = "b">Busan</button>
<button id = "g">Gwangju</button>
<div id ="map"></div>
```

## OPEN API

> https://www.data.go.kr/
>
> 인증키
>
> - %2FilFcPTUlcQRDwwxCuRocfcCX22AI%2FyLxewflf4Yap04LRnuTm%2Bl5H14%2B1s%2B9NUxBKMkv7G1b3TqCWW4R3ncQg%3D%3D