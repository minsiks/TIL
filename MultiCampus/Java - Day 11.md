# 4/21 Java Day11

## Ch08 인터페이스

### 다중인터페이스

- ![다중인터페이스](C:\Users\bangs\OneDrive\Pictures\다중인터페이스.png)

## Ch09 중첩 클래스와 중첩 인터페이스

### awt

- Anonymous Class

```java
package awt;

import java.awt.Frame;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class MyFrame {
	Frame f;
	
	public MyFrame() {
		f = new Frame("My Frame");
	}
	public void setView() {
		f.setSize(300, 200);
		f.setVisible(true);
		f.addWindowListener(new WindowAdapter() {
//중첩 클래스
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
			
			
		});
	}
}
```

```java
package awt;

public class App {

	public static void main(String[] args) {
		MyFrame m = new MyFrame();
		m.setView();

	}

}

```

## Ch10 예외처리

```java
package p422;

import java.util.Scanner;

public class Ex2 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("Input number");
		String num = sc.next();
		int n = 0;
		int result = 0;		
		try {
			n = Integer.parseInt(num);
			result = 100 / n;
			System.out.println(result);
		}catch(ArithmeticException e) {
			System.out.println("분모가 0 입니다.");
		}catch(NumberFormatException e) {
			System.out.println("숫자가 아닙니다.");
		}finally {
			sc.close();
			System.out.println("end");
		}

	}
}

```

### 예외 클래스

- `e.printStackTrace();` : 자세한 예외 상황 프린트
- `System.out.println(e);`: 간단하게 예외 내용 프린트

#### ~오류의 종류

- 에러 (Error)
  - 정상 실행 상태로 돌아갈 수 없음
  - 에러가 발생되면 프로그램 종료
  - 하드웨어의 잘못된 동작 또는 고장으로 인한 오류
- 예외(Exception)
  - 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인한 오류
  - 예외가 발생되면 프로그램 종료
  - 예외 처리 추가하면 정상 실행 상태로 돌아갈 수 있음

### 예외 처리 코드(try-catch-finally)

### 예외 떠넘기기

-> `throw new Exception` -> Exception 상속 새로운 클래스 생성->

```java
package bank;

public class MinusException extends Exception {
	public MinusException() {
		
	}
	public MinusException(String msg) {
		super(msg);
	}
}
```

```java
package bank;

public class BankApp {

	public static void main(String[] args) {
		Account acc =new Account("1111", 10000);
		
		try {
			acc.deposit(-100);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		
		try {
			acc.withdraw(-50);
		} catch (MinusException | OverdrawnException e) {
			System.out.println(e.getMessage());
		}
		
		System.out.println(acc);
	}

}
```

> VO : Value Object
>
> CRUD : Create / Read / Update / Delete
>
> DAO : Data Access Object