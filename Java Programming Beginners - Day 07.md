# 22-12-21 Java Programming Beginners - Day 07

## Inflearn 자바 프로그래밍 입문

> 박은종

## 자바 입출력

### 스트림

- 네트웍에서 자료의 흐름이 물과 같다는 의미에서 유래
- 다양한 입출력 장치에 독립적으로 일관성 있는 입출력을 제공하는 방식

### 스트림의 구분

- 대상 기준
  - 입력 스트림 / 출력 스트림

- 자료의 종류
  - 바이트 스트림 / 문자 스트림
- 기능
  - 기반 스트림/ 보조 스트림

### 입력 스트림과 출력 스트림

> 입출력을 같이 하지 않음

- 입력 스트림 : 대상으로 부터 자료를 읽어 들이는 스트림

- 출력 스트림 : 대상으로 자료를 출력하는 스트림

### 기반 스트림과 보조 스트림

- 기반 스트림 : 대상에 직접 자료를 읽고 쓰는 기능의 스트림
- 보조 스트림 : 혼자는 작동 불가

### Buffered 스트림

- 내부적으로 8192 바이트 배열을 가지고 읽거나 쓰기 기능을 제공하여 속도가 빨라짐

### 직렬화 (serialization)

- 인스턴스의 상태를 그대로 저장 하거나(serialization) 다시 복원하는 (deserialization) 방식
- 파일에 쓰거나 네트웍으로 전송
- ObjectInputStream과 ObjectOutputStream 사용
  - ObjectInputStream(InputStream in)
  - ObjectOutputStream(OutputStream out)

```java
package stream.serialization;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

class Person implements Serializable {
	/**
	 * 
	 */
	private static final long serialVersionUID = -8979042624550852984L;
	String name;
	transient String title;
	
	
	public Person() {}
	public Person(String name, String title) {
		this.name =name;
		this.title = title;
	}
	
	public String toString() {
		return name+","+title;
	}
}

public class SerializationTest {

	public static void main(String[] args) throws ClassNotFoundException {

		Person personLee = new Person("Lee","Manager");
		try( FileOutputStream fos = new FileOutputStream("serial.dat");
				ObjectOutputStream oos = new ObjectOutputStream(fos)){
			
			oos.writeObject(personLee);
		}catch(IOException e) {
			System.out.println(e);
		}
		
		try( FileInputStream fis = new FileInputStream("serial.dat");
				ObjectInputStream ois = new ObjectInputStream(fis)){
			
			Object obj = ois.readObject();
			if(obj instanceof Person) {
				Person p = (Person)obj;
				System.out.println(obj);
			}
		}catch(IOException e) {
			System.out.println(e);
		}
	}

}
```

### 그 외 입출력 클래스

- File 클래스
  - 입출력 기능은 없고 파일의 속성, 경로, 이름 등을 알 수 있음
- RandomAccessFile 클래스
  - 입출력 클래스 중 유일하게 파일 입출력을 동시에 할 수 있는 클래스
