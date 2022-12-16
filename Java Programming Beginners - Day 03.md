# 22-12-15 Java Programming Beginners - Day 03

## Inflearn 자바 프로그래밍 입문

> 박은종

## 클래스와 객체

### static 응용 : singleton 패턴

> 자바는 글로벌 변수 X => static변수 사용

```java
package singleton;

public class Company {

	private static Company instance = new Company();
	
	private Company() {}
	
	public static Company getInstance() {
		if(instance == null)
			instance = new Company();
		return instance;
	}
}
```

- 하나의 인스턴스만 생성하여 사용하는 디자인 패턴

## 배열과 ArrayList

### 배열(Array)

- 동일한 자료형의 변수를 한꺼번에 순차적으로 관리
- 배열은 고정길이(Fixed Length)로 선언
- 연속된 자료구조

### 배열 선언

- 자료형[] 배열이름 = new 자료형[개수];
  - int[] arr = new int[10];
- 자료형 배열이름[] = NEW 자료형[개수];
  - int arr[] = new int[10];

- [] : 인덱스 혹은 첨자 연산자, 배열의 위치를 지정하여 자료를 가져옴
- 모든 배열의 시작은 0부터 시작

### 배열의 길이와 유효한 요소 값

- 배열의 길이의 속성 : length;
- 배열의 유요한 요소의 길이 : size;

## 객체 배열 사용하기

### 객체 배열

- 참조 자료형을 선언하는 객체 배열

```java
package arrau;

public class book {

	private String bookName;
	private String author;
	
	public book() {}
	public book(String bookName, String author) {
		this.bookName = bookName;
		this.author = author;
	}
	public String getBookName() {
		return bookName;
	}
	public void setBookName(String bookName) {
		this.bookName = bookName;
	}
	public String getAuthor() {
		return author;
	}
	public void setAuthor(String author) {
		this.author = author;
	}
	
	public void showBookInfo() {
		System.out.println(bookName + "," +author);
	}
}
```

```java
package arrau;

public class BookArray {

	public static void main(String[] args) {
		
		book[] library = new book[5];
		
		library[0] = new book("태백산맥1","조정래");
		library[1] = new book("태백산맥2","조정래");
		library[2] = new book("태백산맥3","조정래");
		library[3] = new book("태백산맥4","조정래");
		library[4] = new book("태백산맥5","조정래");
		
		for (int i = 0; i < library.length; i++) {
			System.out.println(library[i]);
		}
		
		for (int i = 0; i < library.length; i++) {
			library[i].showBookInfo();
		}
	}

}
```

### 배열 복사 하기

- System.arraycopy(복사할 배열 이름, 복사할 첫 위치, 대상 배열, 붙여 넣을 첫 위치, 복사할 요소 개수)
- 객체 배열 복사하기
  - 얕은 복사
    - System.arraycopy(복사할 배열 이름, 복사할 첫 위치, 대상 배열, 붙여 넣을 첫 위치, 복사할 요소 개수)
    - 주소만 복사하여 어느 쪽에서라도 값이 변경되면 같이 변경된다.
  - 깊은 복사
    - setter, getter를 이용하여 복사
    - 어느 한쪽이 값이 변경되더라도 분리

```java
package arrau;

public class ObjectCopy {

	public static void main(String[] args) {

		book[] bookArray1 = new book[3];
		book[] bookArray2 = new book[3];
		
		bookArray1[0] = new book("태백산맥1", "조정래");
		bookArray1[1] = new book("태백산맥2", "조정래");
		bookArray1[2] = new book("태백산맥3", "조정래");
		
		bookArray2[0] = new book();
		bookArray2[1] = new book();
		bookArray2[2] = new book();
		
		
//		System.arraycopy(bookArray1, 0, bookArray2, 0, 3);
		
		for (int i = 0; i < bookArray2.length; i++) {
			bookArray2[i].setAuthor(bookArray1[i].getAuthor());
			bookArray2[i].setBookName(bookArray1[i].getBookName());			
		}
		
		bookArray1[0].setBookName("나목");
		bookArray1[0].setAuthor("박완서");
		
		for (int i = 0; i < bookArray1.length; i++) {
			bookArray1[i].showBookInfo();
		}
		System.out.println("=============");
		for (int i = 0; i < bookArray2.length; i++) {
			bookArray2[i].showBookInfo();
		}
		
	}

}

```

### 향상된 for 문 

- foreach문

```java
String[] strArr = {"java","Android","C"};
		
		for (String string : strArr) {
			System.out.println(string);
		}
```

## 다차원 배열

### 다차원 배열

- 2차원 이상의 배열
- 지도,게임 등 평면이나 공간을 구현 할 때 많이 사용
- 이차열 배열의 선언과 구조
  - `int[][] arr = new int[2][3];`
  - `자료형[][] 배열이름 = new int[행개수][열개수];`

```java
int[][] arr =new int[][] {{1,2,3},{4,5,6}};
		
		for (int i = 0; i < arr.length; i++) {
			for (int j = 0; j < arr[i].length; j++) {
				System.out.println(arr[i][j]);
			}
```

## ArrayList 클래스

### ArrayList 클래스

- 객체 배열을 편리하게 관리
- 길이를 정하지 않아도 되며 중간값이 삭제 되는것에대한 유연함 보유

#### - ArrayList 메서드

- .add(); / .get(index); / .size(); 등등 

```java
package arrau;

import java.util.ArrayList;

public class ArrayListTest {

	public static void main(String[] args) {
		
		ArrayList<String> list = new ArrayList<String>();
		list.add("aaa");
		list.add("bbb");
		list.add("ccc");
		
		for (int i = 0; i < list.size(); i++) {
			System.out.println(list.get(i));
		}
	}

}
```

