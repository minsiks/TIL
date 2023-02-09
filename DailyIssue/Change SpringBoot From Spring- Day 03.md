# 23-2-8 Change SpringBoot From Spring- Day 04

### Admin 상점관리 spring->springboot 고도화

### DetailView

1. JSP ListAction에서 ajax가 tiles로 분리되지 않고 전체화면으로 구성되는 오류

   - submit코드 변경 (기존코드)

   ```javascript
   function detailView(store_id) {
   		$("#store_id").val(store_id);
   		$("#pageType").val("storeList");
   		$("#myForm").attr("action", "detailView.ajax");
   		$("#myForm").submit();
   	}
   ```

   - 변경코드

   ```javascript
   function detailView(store_id) {
   		$("#store_id").val(store_id);
   		$("#pageType").val("storeList");
   		var param = $("#myForm").serialize();
   		$.post('detailView.ajax', param, function(data) {
   			$('#content').html(data);
   		});
   	}
   ```

2. 변경한 코드로 thymeleaf, html에 적용후 에러

   - html에서는 기존코드 유지하니 문제없이 작동

3. onclick 타임리프 문법 에러

   - storeInfo가 추출되지 않아 생긴 에러
   - 이유 : th:onclick="|detailView(${store_id})|" -> th:onclick="detailView([[${store_id}]])" 로 변경 필요

4. JSP에서도 운영상태 변경 안됨

   - 이유 : Controller에서 Service 까지 잘작동 하지만 Model, "result"값으로 전달 안됨

   - 직접적으로 String으로 "succ"을 전송하여 data로 Validation

   - $.ajax에서 `var result = data.result`에서 `var result = data`로 변경

     - dataType : "json" 삭제

   - 변경전

     ```java
     @RequestMapping(value = "statUpdateAction", method = RequestMethod.POST)
     	public void statUpdateAction(Model model, StoreInfoMgmt storeInfoMgmt) {
     		model.addAttribute("result", storeService.statUpdateAction(storeInfoMgmt));
     	}
     ```

   - 변경후

     ```java
     @RequestMapping(value ={"statUpdateAction","statUpdateAction.json"}, method = RequestMethod.POST)
     	public @ResponseBody String statUpdateAction(Model model, StoreInfoMgmt storeInfoMgmt) {
     		
     		model.addAttribute("result", storeService.statUpdateAction(storeInfoMgmt));
     		return storeService.statUpdateAction(storeInfoMgmt);
         }
     ```

5. daum.Postcode jsp 인식문제

   - 이유 : ajax로 updatePage를 .html로 가져오는 문제

   - 해당 daumPost 답변 참고

     - 저희 서비스의 경우에는 postcode.v2.js를 로딩 한 후에, 실제 core스크립트를 한번 더 로딩합니다.
       해당 오류는 현재 이 core스크립트가 로딩이 안되서 발생한 문제이며,

       아주 기본적인 자바스크립트를 사용 (script 태그로 파일 추가하고, 그 아래에서 실행하는 방식)으로 할때는 문제가 되진 않지만, 만약 리엑트나 앵귤러나 이와 비슷한, 동적으로 DOM을 생성및 삭제하는 라이브러리들을 이용하실때에는 타이밍 문제가 발생합니다. 그런 경우엔 저희 가이드 페이지에 있는 동적로딩 방법을 이용해 보시면 해결 되실거에요

   - 해당 javascript문을 list.jsp로 미리 끌고와서 해결 (jsp(스프링)만 해당, html은 해당 X )

### 신규/탈퇴회원 통계

1. 기존 JSP 차트 데이터 출력 X 
2. 

> 해결못한 문제 : 
>
> 1. 엑셀 다운로드 : JSP에서도 기능 X  
