# 5/24 JavaWeb Day10

## KAKAO MAP

### 카카오 앱키 

> https://apis.map.kakao.com/

- JavaScript 키
  - df87c9522b5396d8eec3a79c7cacc39a

- aj08.html

```html
<meta charset="UTF-8">	
<style>
	#map{
		width:500px;
		height:400px;
		border:2px solid red;
	}
</style>
<script>
$(document).ready(function(){
	var mapContainer = document.getElementById('map'); // 지도를 표시할 div 
    var mapOption = { 
        center: new kakao.maps.LatLng(33.450701, 126.570667), // 지도의 중심좌표
        level: 3 // 지도의 확대 레벨
    };

	var map = new kakao.maps.Map(mapContainer, mapOption); // 지도를 생성합니다
	
	var mapTypeControl = new kakao.maps.MapTypeControl();

	// 지도에 컨트롤을 추가해야 지도위에 표시됩니다
	// kakao.maps.ControlPosition은 컨트롤이 표시될 위치를 정의하는데 TOPRIGHT는 오른쪽 위를 의미합니다
	map.addControl(mapTypeControl, kakao.maps.ControlPosition.TOPRIGHT);

	// 지도 확대 축소를 제어할 수 있는  줌 컨트롤을 생성합니다
	var zoomControl = new kakao.maps.ZoomControl();
	map.addControl(zoomControl, kakao.maps.ControlPosition.RIGHT);

	var markerPosition  = new kakao.maps.LatLng(35.162613, 129.160888); 

	// 마커를 생성합니다
	var marker = new kakao.maps.Marker({
	    position: markerPosition
	});

	// 마커가 지도 위에 표시되도록 설정합니다
	marker.setMap(map);
	
	var iwContent = '<div style="padding:5px;">구구닷 <br><a href="https://map.kakao.com/link/map/Hello World!,33.450701,126.570667" style="color:blue" target="_blank">큰지도보기</a> <a href="https://map.kakao.com/link/to/Hello World!,33.450701,126.570667" style="color:blue" target="_blank">길찾기</a></div>', // 인포윈도우에 표출될 내용으로 HTML 문자열이나 document element가 가능합니다
    iwPosition = new kakao.maps.LatLng(35.162613, 129.160888); //인포윈도우 표시 위치입니다

	// 인포윈도우를 생성합니다
	var infowindow = new kakao.maps.InfoWindow({
	    position : iwPosition, 
	    content : iwContent 
	});
	  
	// 마커 위에 인포윈도우를 표시합니다. 두번째 파라미터인 marker를 넣어주지 않으면 지도 위에 표시됩니다
	infowindow.open(map, marker); 
	
	$('#go1').click(function(){
		    var moveLatLon = new kakao.maps.LatLng(35.162613, 129.160888);
		    
		    map.setCenter(moveLatLon);
		
	});
	$('#go2').click(function(){
			var moveLatLon = new kakao.maps.LatLng(33.450580, 126.574942);
		    map.panTo(moveLatLon);           
    
	});
});
</script>
<h1>AJ 08</h1>
<button id="go1">GO1</button>
<button id="go2">GO2</button>
<div id="map"></div>

```

>
>
>수행평가 프로젝트 중 안되는점
>
>- 탭창 토글
>- slide down
>- 테이블 수직 맞춤 안됨
>- 앵커 크기