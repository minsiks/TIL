# 7/6 Cloud Platform Day 06

> [사용 가이드 - HOME (ncloud-docs.com)](https://guide.ncloud-docs.com/docs/ko/home)

## NCP

- Client ID
  - xip09ymf3b
- Client Secret
  - 5D8Q8uZffMeO4tqndjAaSOIscimr6QC9YOxRygiL

## PAPAGO

- 스트링값 JSON으로 바꾸는 법

``` 
JSONParser parser = new JSONParser();
Object obj = parser.parse(json);	
```

- NaverAPI.java

```java
package com.ncp.restapi;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;

import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;
import org.springframework.stereotype.Component;

@Component
public class NaverAPI {
	String clientId = "xip09ymf3b";// 애플리케이션 클라이언트 아이디값";
	String clientSecret = "5D8Q8uZffMeO4tqndjAaSOIscimr6QC9YOxRygiL";// 애플리케이션 클라이언트 시크릿값";

	public Object papago(String txt) {
		String result = null;
		try {
			String text = URLEncoder.encode(txt, "UTF-8");
			String apiURL = "https://naveropenapi.apigw.ntruss.com/nmt/v1/translation";
			URL url = new URL(apiURL);
			HttpURLConnection con = (HttpURLConnection) url.openConnection();
			con.setRequestMethod("POST");
			con.setRequestProperty("X-NCP-APIGW-API-KEY-ID", clientId);
			con.setRequestProperty("X-NCP-APIGW-API-KEY", clientSecret);
			// post request
			String postParams = "source=ko&target=en&text=" + text;
			con.setDoOutput(true);
			DataOutputStream wr = new DataOutputStream(con.getOutputStream());
			wr.writeBytes(postParams);
			wr.flush();
			wr.close();
			int responseCode = con.getResponseCode();
			BufferedReader br;
			if (responseCode == 200) { // 정상 호출
				br = new BufferedReader(new InputStreamReader(con.getInputStream()));
			} else { // 오류 발생
				br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
			}
			String inputLine;
			StringBuffer response = new StringBuffer();
			while ((inputLine = br.readLine()) != null) {
				response.append(inputLine);
			}
			br.close();
			result = response.toString();
			System.out.println(response.toString());
		} catch (Exception e) {
			System.out.println(e);
		}
		JSONParser parser = new JSONParser();
		Object obj = null;
		try {
			obj = parser.parse(result);
		} catch (ParseException e) {
			e.printStackTrace();
		}
		return obj;
	}
}

```

- AJAXController.java

```JAVA
@Autowired
	NaverAPI naverapi;
	
	@RequestMapping("papagotr")
	public Object papagotr(String txt) throws Exception {
		Object result = naverapi.papago(txt);
		return result;
	}
```

- papago.html

```java
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=df87c9522b5396d8eec3a79c7cacc39a"></script>
<script>


$(document).ready(function(){
	$('#bt').click(function(){
		var param = $('#param').val();
		$.ajax({
			url:'/papagotr',
			data:{'txt':param},
			success:function(data){
				alert(data);
			}
		});
	});
});
</script>
</head>
<body>
	<h1>PAPAgo Page</h1>
	<input type="text" id="param">
	<button id="bt">Click</button>
</body>
</html>
```

- 결과 텍스트 뽑아내기

  `var trdata = data.message.result.translatedText;`
