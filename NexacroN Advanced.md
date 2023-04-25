# 2023.04.13 넥사크로N 심화 교육

## 넥사크로N 컴포넌트 활용

### Method

- getColumn : 컬럼 정보를 호출
  - `this.Dataset1.getColumn(nRow, "FULL_NAME");`

- FindRow  : 단일조건 데이터 로우의 인덱스값을 가져옴
  - `this.Dataset1.findRow("EMPL_ID", "KR120");`

- findRowExpr : 여러개의 조건으로 데이터 로우의 인덱스 값을 가져옴
  - `this.Dataset1.findRowExpr("DEPT_CODE == 'K10' && SALARY <= 5000");`
- extractRows : 조건에 맞는 여러개의 인덱스를 어레이 형태로 호출
  - `this.Dataset1.extractRows("DEPT_CODE=='K10'");`
- getCaseAvg : 조건에 맞는 평균
  - `this.Dataset1.getCaseAvg("GENDER=='W'", "SALARY",0, -1, false);`

- set_keystring : 정렬 또는 그룹핑
  - `this.Dataset1.set_keystring("S:-HIRE_DATE");`
    - S : 정렬
      - -내림차순
    - G: 그룹핑

- filter : 조건에 맞는 데이터
  - this.Dataset1.filter("GENDER == 'M' && MARRIED == '0'");
  - 필터 해제 : this.Dataset1.filter("");
- getRowCountNF() : 원본데이터개수 추출

> NF : Not Filter 필터 없는 데이터를 해당

- insertRow() : 원하는 위치에 데이터 삽입

  - `this.Dataset4.insertRow(0);`

- .deleteMultiRows(arrRow);

  - ```javascript
    var arrRow = [3,4,5]; 
     	this.Dataset4.deleteMultiRows(arrRow);
    ```

- getDeletedRowCount() : 지워진 데이터 개수
- getDeletedColumn() : 지워진 데이터의 컬럼값 호출
  - `var sDelData = this.Dataset4.getDeletedColumn(0, "FULL_NAME");`

- copyData() : 데이터셋을 copy

- assign() : 데이터셋을 수정,삭제,상태 등 정보까지 copy

- copyRow : row단위 copy

  - ```javascript
    var nFromRow = this.Dataset4.findRow("EMPL_ID", "KR210");
    var nToRow   = this.Dataset5.addRow();
    var sCol = "EMPL_ID=EMPL_ID, FULL_NAME=FULL_NAME";	
    this.Dataset5.copyRow(nToRow, this.Dataset4, nFromRow, sCol);
    ```

### Grid

- Dataset내용을 격자모양로 표현
- Grid1.getCellProperty() : 셀의 속성값을 추출
  - `this.Grid1.getCellProperty("body", i, "text");`

- insertContentsCol() : Grid에 컬럼 추가
  - `this.Grid2.insertContentsCol("body", 0);`

- setCellProperty() : 셀의 속성을 추가 또는 변경
  - `this.Grid2.setCellProperty("body", 0, "text", "bind:COL_CHK");`

- getCurFormatString() : Gird의 Format(모양)을 스트링으로 추출

  - ```javascript
    var sCurFormat = this.Grid3.getCurFormatString();
    this.Grid3_1.set_formats("<Formats>" + sCurFormat + "</Formats>");
    	
    var sOrgFormat = this.Grid3.getCurFormatString(true);
    this.Grid3_2.set_formats("<Formats>" + sOrgFormat + "</Formats>");
    ```

  
