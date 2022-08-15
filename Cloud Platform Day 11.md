# 8/15 Cloud Platform Day 11

## I) 사용자 페이지

### 1. 영화 목록 페이지

 ![mlist](Images/mlist.png)

![mlist2](Images/mlist2.png)

![mlist1](Images/mlist1.png)![modaltotal](Images/modaltotal.png)

- 상영 영화 데이터 연동, 화면에 노출
- AJAX로 각 예약 스케쥴 날짜 선택 시 날짜별 표시 (해당 영화의 날짜별 상영관, 스케줄 등 비동기식 정보 가져와서 노출) 
- 각 전체, 개봉일 오래된 순, 별점 순, 리뷰많은 순, 상영중인 영화 기준으로 정렬 기능 구현
- 페이징 기능 구현 (최초의 페이지에선 이전 버튼을 표시하지 않음, 정렬마다도 페이징 가능)

- 리뷰 별점 데이터 연동해서 CSS활용 별점 퍼센트로 채우기 기능 구현
- 각 영화당 리뷰, 동영상, 포스터 이미지 아이콘 클릭시 모달로 정보 표시 기능 구현

### 2. 영화 상세 페이지

#### 2-1 영화 상세 정보 (정보, 차트)

![detail](Images/detail.png)

![detail1](Images/detail1.png)

- 해당 영화에 대한 정보 및 리뷰와 사진 및 동영상 등 상세정보 표시 기능 구현
- 차트 기능 구현 (연령별, 성별별 차트 기능 구현)
- 슬라이드를 이용 영화 포스터를 노출, 드래그시 이동 가능
- 각 슬라이드 마다 클릭시 이벤트 다르게 기능 구현

#### 2-2 첫번째 슬라이드

![slide1](Images/slide1.png)

- 첫번째 슬라이드를 클릭할 때 발생하는 이벤트
- 데이터 속 영화 제목과 유튜브를 연동하여 유튜브에서 해당 영화 제목으로 검색 한다.

#### 2-3 두번째 슬라이드

![slide2](Images/slide2.png)

- 각 영화들 마다 티저 영상이나, 예고편을 보여준다.
- 모달이 닫히면 영상도 자동으로 정지된다.

#### 2-4 그외 슬라이드

![slide3](Images/slide3.png)

- 포스터 이미지와 그 외의 해당 영화의 관련된 이미지 정보들을 노출
- 슬라이드, carousel 기능으로 화살표 클릭시 돌아가면서 이미지 노출

#### 2-5 리뷰 표시 및 리뷰 남기기 기능![detail2](Images/detail2.png)

![detail3](Images/detail3.png)

- 리뷰 표시 (n일전, 날짜 및 요일, 이름(아이디), 리뷰 내용, 별점 개수)
- 로그인 비활성화시 리뷰 남기기란 표시 X (리뷰 작성 불가)
- 로그인 활성화시 별점 체크 후 리뷰 작성 가능 (남은 글자 수 표시)

#### 2-6 구글맵 연동 및 토글 기능 추가 (영화관 위치 표시)

![detail4](Images/detail4.png)

- 구글맵 연동 및 영화관 위치 표시
- 영화 상세 페이지 안에서 토글로 구글맵을 열고 닫을 수 있는 기능 구현

### 3. Contact 페이지

#### 3-1 연락처 표시 및 문의사항 접수(폼으로 이메일 전송)

![contact1](Images/contact1.png)

![contact2](Images/contact2.png)

![contact3](Images/contact3.png)

- 컨택트 (사용자 문의 및 편의 기능 위주) 페이지 기능 구현
- 정적 HTML form태그에서 메일 보내기 : Google Apps Mail 
- 해당 폼에 문의 내용과 이름, 이메일을 기입하여 전송 버튼을 누를시, 구글 스프레드시트를 통하여 직접적으로 설정된 이메일로 정보들이 전송
- 설정된 이메일에서 전송된 정보 확인 가능
- 성공시 성공메시지 Fade In 표시 기능 구현

#### 3-2 구글맵 연동

![contact4](Images/contact4.png)

![contact5](Images/contact5.png)

![contact6](Images/contact6.png)

- 컨택트 페이지 하단에 구글맵 연동하여 표시
- 해당 영화관위치가 마커 아이콘으로 표시 (BOUNCE 애니메이션 효과 추가)
- 마커 아이콘 클릭시 맵이 축소되며 상세 위치정보 표시 이벤트 추가
- 해당 a태그를 클릭하면 직접적인 구글 맵으로 이동하여 정보 확인 가능

### 4.  닮은꼴 찾기 이벤트 페이지

#### 4-1 유명배우 사진,영상 팝업으로 표시 기능 구현

![cfr](Images/cfr.png)

![cfr1](Images/cfr1.png)

![cfr2](Images/cfr2.png)

- 페이지가 심심하지 않기위해 유명 배우 사진을 나열
- 해당 이미지를 클릭할시 사진 아이콘은 해당 배우의 사진이, 영상 아이콘은 해당 배우 관련 유튜브 영상을 볼 수 있는 기능 구현

#### 4-2 Naver AI CLOVA를 활용 얼굴 인식 및 닮은꼴 찾기 기능 구현

![cfr3](Images/cfr3.png)

![cfr4](Images/cfr4.png)

![cfr5](Images/cfr5.png)

![cfr6](Images/cfr6.png)

- Naver CLOVA Face Recognition API(이하 CFR API) 활용 기능 구현
- 입력한 이미지 데이터의 얼굴 인식 결과를 json 형태로 반환하는 서비스를 제공
- 사진파일 입력과 동시에 화면에 표시
- 반환된 json 형태의 데이터를 정제하여 표시된 이미지 하단에 정보 표시
- a링크 클릭시 해당 관련인물 구글 이미지 검색 페이지로 이동
- 얼굴이 아닌 사진, 너무 많은 얼굴이 많은 사진 등 입력된 사진 데이터가 불안정하다면 '사진을 한번 더 확인해 주세요'라는 문구 'Fade In'으로 노출

### 5. ChatBot

![chatbot](Images/chatbot.png)

![chatbot1](Images/chatbot1.png)

![chatbot2](Images/chatbot2.png)

- Naver CLOVA ChatBot을 활용 고객 편의 기능 구현
- 헤더에 챗봇문의 버튼 추가, 버튼 클릭시 모달로 챗봇 이벤트 발생(클릭시 웹소켓 커넥트)
- 모달 발생시 자동으로 메세지 발생, 질문 유도
- 질문 입력시 챗봇에게 받은 데이터를 화면에서 출력
- 챗봇 대화 빌드 작성 및 공통메시지(실패메시지) 작성
- 모달 닫힐 시 웹소켓 디스커넥트 이벤트 발생

### 6. 사용자 화면 UI 1차 변경 및 정리, 구성

![UI2](Images/UI2.png)

![UI3](Images/UI3.png)

- 기존 프로젝트의 테마 UI

![UI](Images/UI.png)

![UI4](Images/UI4.png)

- 1차적으로 포스터 위치, 크기, 전체 주제에 맞는 컬러 선정, 폰트 선정

- 화면을 효과적으로 구성하기 위해 그라데이션, div 크기 재설정, 이미지 및 영상 추가 등 시도

- 테마의 CSS 변경 (버튼 컬러, 폰트 컬러 및 크기,  백그라운드 이미지 재설정 등)

  

## 2. 관리자 사이트

### 1. 로그인 페이지

![alogin](Images/alogin.png)

![alogin1](Images/alogin1.png)

- 관리자사이트 로그인 기능 구현
- DB에 관리자사이트 회원관련 데이터와 비교하여 로그인 가능
- 관리자사이트는 회원 정보가 담긴 Session값이 없으면 모든페이지 로그인 페이지로 Return
- 잘못된 로그인 정보 입력시 간단 확인 메시지 출력으로 유효성 검사

### 2. 회원가입 페이지

![aregister1-1](Images/aregister1-1.png)

- 관리자 페이지의 보안 강화를 위해 간단한 인증 절차 진행
- master에게 부여받은 코드를 전달받아 입력 해야 회원가입 가능
- 코드를 잘못 입력하면 '코드가 잘못되었습니다'라는 메시지 출력

![aregister1-2](Images/aregister1-2.png)

- 마스터인 회원정보에서만 코드보기 버튼 표시
- 마스터가 코드 확인후 전달 (코드는 주기적으로 변함)
- 코드 확인후 정확한 코드와 아이디 비밀번호 입력 시 회원가입 성공

### 3. 고객 관리 페이지

#### 3-1 고객 등록 페이지

![acust5](Images/acust5.png)

![acust6](Images/acust6.png)

- 고객 정보 입력, 등록
- 날짜는 숫자로 입력 ('-' 자동 생성, 날짜 범위 벗어나면 버튼 클릭시 alert로 다시 확인 메시지 출력)
- 아이디, 비밀번호 유효성 검사 (Ajax 데이터 보내서 데이터 유효성 검사), 간단하게 alert로 출력
- 성공시 성공페이지 출력

#### 3-2 고객 목록 페이지

![acust1](Images/acust1.png)

![acust2](Images/acust2.png)

![acust3](Images/acust3.png)

![acust4](Images/acust4.png)

- 고객 정보 나열 및 각 고객별 쿠폰목록 정보, 수정 삭제 기능 구현, 페이징 기능 구현
- 검색 기능 구현 (고객 관련 아무 정보나 서치 가능)
- 쿠폰함 클릭시 해당 아이디의 쿠폰 정보 나열, 삭제 가능
- 수정 삭제 버튼 클릭시 수정 및 삭제 가능

### 4. 영화 관리 페이지

#### 4-1 영화 등록

![addmoive](Images/addmoive.png)

![addmoive3](Images/addmoive3-166053313953751.png)

- 영화 정보 입력, 등록
- :zap: 영화 포스터 이미지 파일의 데이터는 관리자, 사용자 페이지에 모두 전송
- 즉시 사용자 페이지에도 적용 
- 날짜는 숫자로 입력 ('-' 자동 생성, 날짜 범위 벗어나면 버튼 클릭시 alert로 다시 확인 메시지 출력)
- 성공시 성공페이지 출력

#### 4-2 영화 목록 페이지

![moivelist](Images/moivelist.png)

![moivelist1](Images/moivelist1.png)

![moivelist2](Images/moivelist2.png)

![moivelist3](Images/moivelist3.png)

- 영화 정보 나열 및 각 영화별 리뷰 정보, 수정 삭제 기능 구현, 페이징 기능 구현
- 검색 기능 구현 (영화 관련 아무 정보나 서치 가능)
- 리뷰 관리 클릭시 해당 영화의 리뷰 목록 확인, 삭제 가능
- 수정 삭제 버튼 클릭시 수정 및 삭제 가능

### 5. 장르 관리 페이지

#### 5-1 장르 추가

![addgenre](Images/addgenre.png)

- 장르 정보 입력, 등록
- 상위 장르는 데이터베이스의 상위 장르들을 나열 그 중 SELECT만 가능하도록 구현

#### 5-2 장르 목록

![genrelist](Images/genrelist.png)

![genrelist1](Images/genrelist1.png)

- 장르 정보 나열 및 각 장르별 수정 삭제 기능 구현
- 수정 삭제 버튼 클릭시 수정 및 삭제 가능

### 6. 쿠폰 관리 페이지

#### 6-1 쿠폰 추가

![addcoupon](Images/addcoupon.png)

- 쿠폰 정보 입력, 등록
- 날짜는 숫자로 입력 ('-' 자동 생성, 날짜 범위 벗어나면 버튼 클릭시 alert로 다시 확인 메시지 출력)

#### 6-2 쿠폰 목록

![couponlist](Images/couponlist.png)

![couponlist1](Images/couponlist1.png)

- 쿠폰 정보 나열 및 각 쿠폰별 수정 삭제 기능 구현
- 수정 삭제 버튼 클릭시 수정 및 삭제 가능

### 7. 예매내역 조회 페이지

![reservation](Images/reservation.png)

![reservation1](Images/reservation1.png)

- 예매된 전체 내역을 조회하는 페이지를 구현
- 예매 상세 버튼을 클릭하면 해당 예약건의 상세 정보가 출력(좌석, 상영 회차 등)
