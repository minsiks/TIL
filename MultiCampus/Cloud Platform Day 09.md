# 7/13 Cloud Platform Day 09

> [사용 가이드 - HOME (ncloud-docs.com)](https://guide.ncloud-docs.com/docs/ko/home)

## WebSocket

- 네트워크가 브라우저와 서버가 양방향 통신
- 예) 대표적으로 다음의 증권사이트 (클릭이나 별다른 이벤트 없이 실시간 계속 값이 바뀜)
- WebSocket - HTML5 지원 (But 잘 돌아가지 않는 경우가 많음)
- Stopm - 웹소켓을 좀 더 편하게 핸들링을 할 수 있는 라이브러리 (사용)

### SpringBoot websocket

- Pom.xml 수정 - 최초 spring starter project 생성 시 추가해도 됨

```xml
<!-- WebSocket -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-websocket</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>webjars-locator-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>sockjs-client</artifactId>
			<version>1.0.2</version>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>stomp-websocket</artifactId>
			<version>2.3.3</version>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap</artifactId>
			<version>3.3.7</version>
		</dependency>
```

- 라이브러리 설치
- StomWebSocketConfig - WebSocket 과련 Config 등록

``` java
package com.ncp;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@EnableWebSocketMessageBroker
@Configuration
public class StomWebSocketConfig implements WebSocketMessageBrokerConfigurer {

	@Override
	public void registerStompEndpoints(StompEndpointRegistry registry) {
		registry.addEndpoint("/ws").setAllowedOrigins("http://localhost:8080").withSockJS();
	}//ws는 서버의 경로 (이름음 바꿔도 된다.)

	/* 어플리케이션 내부에서 사용할 path를 지정할 수 있음 */
	@Override
	public void configureMessageBroker(MessageBrokerRegistry registry) {
		registry.enableSimpleBroker("/send");
	}//send는 
}
```

- Msg VO 구현

```java
package com.ncp.vo;


import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@AllArgsConstructor
@NoArgsConstructor
@ToString
@Getter
@Setter
public class Msg {
	private String sendid; //보내는사람
	private String receiveid; // 받는사람
	private String content1;
	private String content2;
}
```

- Conroller 구현

```java
package com.ncp.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.handler.annotation.SendTo;
import org.springframework.messaging.simp.SimpMessageHeaderAccessor;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.web.util.HtmlUtils;

import com.ncp.vo.Msg;



@Controller
public class MsgController {
	
	@Autowired
	SimpMessagingTemplate template;
	
	@MessageMapping("/receiveall") // 모두에게 전송
	public void receiveall(Msg msg, SimpMessageHeaderAccessor headerAccessor) {
		System.out.println(msg);
		template.convertAndSend("/send",msg);
	}
	@MessageMapping("/receiveme") // 나에게만 전송 ex)Chatbot
	public void receiveme(Msg msg, SimpMessageHeaderAccessor headerAccessor) {
		String id = msg.getSendid();
		msg.setContent2("TR Message");
		template.convertAndSend("/send/"+id,msg);
	}
	@MessageMapping("/receiveto") // 특정 Id에게 전송
	public void receiveto(Msg msg, SimpMessageHeaderAccessor headerAccessor) {
		String id = msg.getSendid();
		String target = msg.getReceiveid();
		template.convertAndSend("/send/to/"+target,msg);
	}
}
```

- HTML 구현

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script
	src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="/webjars/sockjs-client/sockjs.min.js"></script>
<script src="/webjars/stomp-websocket/stomp.min.js"></script>
<style>
	#all{
		width:400px;
		height:200px;
		overflow: auto;
		border:2px solid red;
	}
	#me{
		width:400px;
		height:200px;
		overflow: auto;
		border:2px solid blue;
	}
	#to{
		width:400px;
		height:200px;
		overflow: auto;
		border:2px solid green;
	}
</style>

<script th:inline="javascript">
	var stompClient = null;
	var id = [[${session.loginid}]];
	// 서버소켓에 연결
	function connect() {
		var socket = new SockJS('/ws');
		stompClient = Stomp.over(socket);

		stompClient.connect({}, function(frame) { 
			setConnected(true);
			console.log('Connected: ' + frame);
			stompClient.subscribe('/send', function(msg) { 
				$("#all").append(
						"<h4>" + JSON.parse(msg.body).sendid +":"+
						JSON.parse(msg.body).content1
								+ "</h4>");
			});
			stompClient.subscribe('/send/'+id, function(msg) { 
				$("#me").append(
						"<h4>" + JSON.parse(msg.body).sendid +":"+
						JSON.parse(msg.body).content1+":"+
						JSON.parse(msg.body).content2
								+ "</h4>");
			});
			stompClient.subscribe('/send/to/'+id, function(msg) { 
				$("#to").append(
						"<h4>" + JSON.parse(msg.body).sendid +":"+
						JSON.parse(msg.body).content1
								+ "</h4>");
			});
		});
	}

	// 서버소켓에 연결끊기
	function disconnect() {
		if (stompClient !== null) {
			stompClient.disconnect();
		}
		setConnected(false);
		console.log("Disconnected");
	}

	// connect&disconnect버턴 활성화/비활성화
	function setConnected(connected) {
		if (connected) {
			$("#status").text("Connected");
		} else {
			$("#status").text("Disconnected");
		}
		
	}

	// 서버에 메세지 요청 메서드
	function sendAll() {
		var msg = JSON.stringify({
			'sendid' : id,
			'content1' : $("#alltext").val()
		});
		stompClient.send("/receiveall", {}, msg);
	}
	function sendMe() {
		var msg = JSON.stringify({
			'sendid' : id,
			'content1' : $('#metext').val()
		});
		stompClient.send("/receiveme", {}, msg);
	}
	function sendTo() {
		var msg = JSON.stringify({
			'sendid' : id,
			'receiveid' : $('#target').val(),
			'content1' : $('#totext').val()
		});
		stompClient.send('/receiveto', {}, msg);
	}
	$(document).ready(function() {
		$("#connect").click(function() {
			connect();
		});
		$("#disconnect").click(function() {
			disconnect();
		});
		$("#sendall").click(function() {
			sendAll();
		});
		$("#sendme").click(function() {
			sendMe();
		});
		$("#sendto").click(function() {
			sendTo();
		});
	});
</script>
</head>
<body>
	<H1 th:text="${session.loginid}">ID</H1>
	<H1 id="status">Status</H1>
	<button id="connect">Connect</button>
	<button id="disconnect">Disconnect</button>
	
	<h3>All</h3>
	<input type="text" id="alltext"><button id="sendall">Send</button>
	<div id="all"></div>
	
	<h3>Me</h3>
	<input type="text" id="metext"><button id="sendme">Send</button>
	<div id="me"></div>
	
	<h3>To</h3>
	<input type="text" id="target">
	<input type="text" id="totext"><button id="sendto">Send</button>
	<div id="to"></div>

</body>
</html>
```

>  jsc안에 tymeleaf 사용가능 =[[${session.id}]]

## Scheduler

- 작업을 주기적으로 수행

### Springboot scheduler

-  XXXApplication.java 에 @EnableScheduling 추가

```java
@EnableScheduling
@SpringBootApplication
public class Day051Application {
	public static void main(String[] args) {
		SpringApplication.run(Day051Application.class, args);
	}

}
```

- Configuration 생성

```java
package com.multi;

import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.SchedulingConfigurer;
import org.springframework.scheduling.concurrent.ThreadPoolTaskScheduler;
import org.springframework.scheduling.config.ScheduledTaskRegistrar;

@Configuration
public class ScheduleConfig implements SchedulingConfigurer

{
	private final int POOL_SIZE = 6;

	@Override
	public void configureTasks(ScheduledTaskRegistrar registry) {
		ThreadPoolTaskScheduler threadPoolTaskScheduler = new ThreadPoolTaskScheduler();

		threadPoolTaskScheduler.setPoolSize(POOL_SIZE);
		threadPoolTaskScheduler.setThreadNamePrefix("CommB-Scheduled-task-");
		threadPoolTaskScheduler.initialize();

		registry.setTaskScheduler(threadPoolTaskScheduler);
	}

}
```

- Scheduler

```java
package com.multi.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.messaging.simp.SimpMessageSendingOperations;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

import com.multi.vo.Msg;

@Component
public class Scheduler {
	@Autowired
	private SimpMessageSendingOperations messagingTemplate;

	
    @Scheduled(cron = "*/15 * * * * *")
    public void cronJobDailyUpdate() {
    	
    	System.out.println("----------- Scheduler ------------");
    	Msg msg = new Msg();
    	msg.setContent("1");
    	msg.setContent1("2");
    	msg.setContent2("3");
    	msg.setContent3("4");
    	messagingTemplate.convertAndSend("/send", msg);
    }

    @Scheduled(cron = "1 0 0 1,8,15,22 * *")
    public void cronJobWeeklyUpdate(){

    }


}
        초 분 시 일 월 요일
cron = "*  *  *  *  *  *"   

0 0 * * * * : 매 시 0분 0초에 작업
*/15 * * * * * : 매 15초마다 작업
0 0 0 1,8,17,26 * * : 매달 1, 8, 17, 26일 자정에 작업
0 0 10-20 * * * : 매일 10시 ~ 20시 한시간 간격으로 작업
0 0 9-18 * * 1-5 : 월 ~ 금(평일) 9 ~ 18시 매 정각에 작업
0 0 */3 4 * * : 매달 4일에 3시간 간격으로 작업
```

## Chatbot과 연동

- chatbot.java

```java
package com.ncp.restapi;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Base64;
import java.util.Date;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.springframework.stereotype.Controller;

@Controller
public class Chatbot {
	private String secretKey = "cUVMQ3FGYnN0T29PSXF1Y0lHTHpMbkhueU1xa2VFV3I=";
	private String apiUrl = "https://vhigwthcip.apigw.ntruss.com/custom/v1/7270/e6e867dc37ff53f690c12361a2666873c49be4342c23898dc77bfad79e1e1488";
	
	public String getMessage(String txt) throws IOException {
		URL url = new URL(apiUrl);
		String chatMessage = txt;
		String message =  getReqMessage(chatMessage);
		String encodeBase64String = makeSignature(message, secretKey);
		System.out.println(message);
		System.out.println(encodeBase64String);
		HttpURLConnection con = (HttpURLConnection)url.openConnection();
		con.setRequestMethod("POST");
		con.setRequestProperty("Content-Type", "application/json;UTF-8");
		con.setRequestProperty("X-NCP-CHATBOT_SIGNATURE", encodeBase64String);

		con.setDoOutput(true);
		DataOutputStream wr = new DataOutputStream(con.getOutputStream());

		wr.write(message.getBytes("UTF-8"));
		wr.flush();
		wr.close();
		int responseCode = con.getResponseCode();
		System.out.println("responseCode:"+responseCode);

		BufferedReader br;

		if(responseCode==200) { // 정상 호출

			BufferedReader in = new BufferedReader(
					new InputStreamReader(
							con.getInputStream()));
			String decodedString;
			String jsonString = "";
			while ((decodedString = in.readLine()) != null) {
				jsonString = decodedString;
			}
			//chatbotMessage = decodedString;
			
			JSONParser jsonparser = new JSONParser();
			try {

				JSONObject json = (JSONObject)jsonparser.parse(jsonString);
				JSONArray bubblesArray = (JSONArray)json.get("bubbles");
				JSONObject bubbles = (JSONObject)bubblesArray.get(0);
				JSONObject data = (JSONObject)bubbles.get("data");
				String description = "";
				description = (String)data.get("description");
				chatMessage = description;
			} catch (Exception e) {
				System.out.println("error");
				e.printStackTrace();
			}

			in.close();

		} else {  // 에러 발생
			System.out.println("Error");

			chatMessage = con.getResponseMessage();
		}
		System.out.println("REsult:"+chatMessage);
		return chatMessage;
	}	
	
	
	
	public String getReqMessage(String voiceMessage) {

		String requestBody = "";

		try {

			JSONObject obj = new JSONObject();

			long timestamp = new Date().getTime();

			System.out.println("##"+timestamp);

			obj.put("version", "v2");
			obj.put("userId", "U47b00b58c90f8e47428af8b7bddc1231heo2");
			obj.put("timestamp", timestamp);

			JSONObject bubbles_obj = new JSONObject();

			bubbles_obj.put("type", "text");

			JSONObject data_obj = new JSONObject();
			data_obj.put("description", voiceMessage);

			bubbles_obj.put("type", "text");
			bubbles_obj.put("data", data_obj);

			JSONArray bubbles_array = new JSONArray();
			bubbles_array.add(bubbles_obj);

			obj.put("bubbles", bubbles_array);
			obj.put("event", "send");

			requestBody = obj.toString();

		} catch (Exception e){
			System.out.println("## Exception : " + e);
		}

		return requestBody;

	}
	public String makeSignature(String message, String secretKey) {

		 String encodeBase64String = "";

	        try {
	            byte[] secrete_key_bytes = secretKey.getBytes("UTF-8");

	            SecretKeySpec signingKey = new SecretKeySpec(secrete_key_bytes, "HmacSHA256");
	            Mac mac = Mac.getInstance("HmacSHA256");
	            mac.init(signingKey);

	            byte[] rawHmac = mac.doFinal(message.getBytes("UTF-8"));
	            encodeBase64String = Base64.getEncoder().encodeToString(rawHmac);

	            return encodeBase64String;

	        } catch (Exception e){
	            System.out.println(e);
	        }

	        return encodeBase64String;

	}
}
```

- MsgController.java

```java
package com.ncp.controller;

import java.io.IOException;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.simp.SimpMessageHeaderAccessor;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Controller;

import com.ncp.restapi.Chatbot;
import com.ncp.vo.Msg;



@Controller
public class MsgController {
	
	@Autowired
	SimpMessagingTemplate template;
	
	@Autowired
	Chatbot chatbot;
	
	@MessageMapping("/receiveall") // 모두에게 전송
	public void receiveall(Msg msg, SimpMessageHeaderAccessor headerAccessor) {
		System.out.println(msg);
		template.convertAndSend("/sends",msg);
	}
	@MessageMapping("/receiveme") // 나에게만 전송 ex)Chatbot
	public void receiveme(Msg msg, SimpMessageHeaderAccessor headerAccessor) {
		String id = msg.getSendid();
		String result = "";
		try {
			result = chatbot.getMessage(msg.getContent1());
		} catch (IOException e) {
			e.printStackTrace();
		}
		msg.setContent2(result);
		template.convertAndSend("/sends/"+id,msg);
	}
	@MessageMapping("/receiveto") // 특정 Id에게 전송
	public void receiveto(Msg msg, SimpMessageHeaderAccessor headerAccessor) {
		String id = msg.getSendid();
		String target = msg.getReceiveid();
		template.convertAndSend("/sends/to/"+target,msg);
	}
}
```

