# 7/5 Cloud Platform Day 05

> [사용 가이드 - HOME (ncloud-docs.com)](https://guide.ncloud-docs.com/docs/ko/home)

## NCP 환경 이미지

![070501](C:\Users\user\Desktop\김민식\MultiCam_Web\Images\7.5\070501.png)

## 카카오 API 이용하기

- REST API 키
  - 1aebbeeff8d563e40c048cba393713fc
- KaKaoTests.java를 test/java/com/ncp 에 넣기

```java
package com.ncp;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.Charset;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class KaKaoTests {
	
	@Test
	void contextLoads() throws Exception {
		String address = "https://dapi.kakao.com/v2/local/search/keyword.JSON?" +
                "query=" + "맛집"//query
                + "&category_group_code=" + "FD6"
                + "&x=" + "37.5606326"
                + "&y=" + "126.9433486"
                + "&radius=" + "100";
		String apiKey = "1aebbeeff8d563e40c048cba393713fc";	//발급받은 restapi key
		
		URL url = new URL(address);  			//접속할 url 설정
		HttpURLConnection conn;					//httpURLConnection 객체
		conn = (HttpURLConnection) url.openConnection();	//접속할 url과 네트워크 커넥션을 연다.
		conn.setRequestMethod("GET");             
		//conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");	//Property 설정//리퀘스트방식은 GET
		conn.setRequestProperty("Authorization", "KakaoAK " + apiKey);	//Property 설정
		

		int responseCode = conn.getResponseCode();		//responseCode를 받아옴.
	
		InputStream inputStream = conn.getInputStream();	//데이터를 받아오기 위한 inputStream
		BufferedReader br;		//inputStream으로 들어오는 데이터를 읽기 위한 reader
		String json = null;
		Charset charset = Charset.forName("UTF-8");
		if(responseCode == 200) {
			br = new BufferedReader(new InputStreamReader(inputStream,charset));
			json = br.readLine();
			br.close();
		}
		else {
			System.out.println(" ERROR !!! ");
		}
	
		inputStream.close();
		conn.disconnect();
		System.out.println(json);
	
		}

}
```

### kakao.html

- 아작스로 데이터 전송

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
$(document).ready(function(){
	$('#bt').click(function(){
		var param = $('#param').val();
		$.ajax({
			url:'/kakaolocal',
			data:{'keyword':param},
			success:function(data){
				alert(data);
			}
		});
	});
});
</script>
</head>
<body>
	<h1>KaKao Page</h1>
	<input type="text" id="param">
	<button id="bt">Click</button>
</body>
</html>
```

### AJAXController.java

```java
package com.ncp.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.ncp.restapi.KakaoAPI;

@RestController
public class AJAXController {
	
	@Autowired
	KakaoAPI kakaoapi;
	
	@RequestMapping("kakaolocal")
	public Object kakaolocal(String keyword) throws Exception {
		System.out.println(keyword);
		String result = kakaoapi.kakaolocalapi(keyword);
		return result;
	}
}
```

### KakaoAPI.java

- AJAX로 

```java
package com.ncp.restapi;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.Charset;

import org.springframework.stereotype.Component;

@Component
public class KakaoAPI {

	public String kakaolocalapi(String keyword) throws Exception {
		String address = "https://dapi.kakao.com/v2/local/search/keyword.JSON";
		
        String param = "query=" + keyword
                //+ "&category_group_code=" + "FD6"
                + "&x=" + "37.5606326"
                + "&y=" + "126.9433486"
                + "&radius=" + "1000";
                
        String apiKey = "1aebbeeff8d563e40c048cba393713fc";	//발급받은 restapi key
		
		
		
		URL url = new URL(address);  			//접속할 url 설정
		HttpURLConnection conn;					//httpURLConnection 객체
		conn = (HttpURLConnection) url.openConnection();	//접속할 url과 네트워크 커넥션을 연다.
		conn.setRequestMethod("POST");             
		conn.setDoOutput(true);
        conn.setUseCaches(false);
		conn.setRequestProperty("Authorization", "KakaoAK " + apiKey);	//Property 설정

		OutputStreamWriter ds = new OutputStreamWriter(conn.getOutputStream());
		ds.write(param);
		ds.flush();
		ds.close();
		
		
		int responseCode = conn.getResponseCode();		//responseCode를 받아옴.
	
		InputStream inputStream = conn.getInputStream();	//데이터를 받아오기 위한 inputStream
		BufferedReader br;		//inputStream으로 들어오는 데이터를 읽기 위한 reader
		String json = null;
		Charset charset = Charset.forName("UTF-8");
		if(responseCode == 200) {
			br = new BufferedReader(new InputStreamReader(inputStream,charset));
			json = br.readLine();
			br.close();
		}
		else {
			System.out.println(" ERROR !!! ");
		}
	
		inputStream.close();
		conn.disconnect();
		return json;
	}
	
}
```

