# 23-2-10 Change SpringBoot From Spring- Day 05

### Admin 상점관리 spring->springboot 고도화

### 통계

1. 기존 JSP 차트 데이터 출력 X 

   원인 : ajax로 받아온 데이터를 입력받아 결과 데이터를 model로 return값을 전달해주려고 코딩한듯 하지만 data.data가 null로 응답하지 X

   해결 : 컨트롤러의 return값으로 ajax의 success의 data로 전달 

### 내 계정

1. 마찬가지로 .ajax가 중복되어서 데이터가 나타나 중복
2. 팝업창 jsp에서 안뜸 (parent().blahblah)
   - jsp 무시 thymeleaf에서 사용가능하도록 완료
3. jsp:useBean 오늘날짜
   - 타임리프 문법으로 대체

> 해결못한 문제 : 
>
> 1. 엑셀 다운로드 : JSP에서도 기능 X  
