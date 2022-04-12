# 4/11 Java Day5

#### ※ 이중 For문

```java
public static void main(String[] args) {
		outter:
		for (int i = 2; i < 10; i++) {
			if(i%2 ==1) {
				continue;
			}
			System.out.println(i+"단 시작 ------------");
			for (int j = 1; j < 10; j++) {
				if(i*j == 28) {
					break outter;
				}
				System.out.printf("%d x %d = %d \n",i,j,(i*j));
			}
		}
	}
```

- 구구단을 이용한 간단한 이중 For문 코딩

- break를 for문 안에서만 걸지않고 전체 코딩에서 걸으려면 break <이름>; 을 넣고 상단에 <이름>:을 넣으면 전체 통제된다.

  > *\*참고* 
  >
  > `				System.out.printf("%d x %d = %d \t",i,j,(i*j));`
  >
  > -  %d = 정수,%f : 소수점, %c :문자형식, %s :문자열 형식
  > - \n : 줄바꿈 , \t : 탭

-------------------------

## ch.5 참조 타입 (reference type)

### i )JVM 나누는 영역

|                  Method                  |        STACK         |      HEAP      |
| :--------------------------------------: | :------------------: | :------------: |
|                   CODE                   |  s1:#0000000001010   |      abc       |
|                                          |  s2:#0000000001010   |       ''       |
| (new string  ("abc")) *새로 HEAP만들어짐 |  s3:#0000000001010   |      abc       |
| (new string  ("abc")) *새로 HEAP만들어짐 |  s4:#0000000001010   |      abc       |
|                                          |       s5:null        |                |
|                                          |   c:#0000000010010   |                |
|            (*배열을 만들 때)             | sr:#0000000000000000 | null,null,null |
|                                          |                      |                |

### ii) null(널)

- 참조 타입 (reference type)의 초기 값 (ex. int의 초기값은 0, double의 초기값은 0.0)
- ==, != 연산가능

### iii) null pointer exception

- 아무런 주소가 들어가있지 않은 null에 접근할 시 발생하는 에러



### iv) String 타입 

- 문자열을 저장 하는 타입
- 문자열 리터럴 동일하다면 String 객체 공유
- new 연산자를 이용한 String 객체 생성
  - 힙 역역에 새로운 String 객체 생성
  - String 객체를 생성한 후 번지 리턴

### v)배열 타입

- 하나의 변수안에 여러개의 데이터들을 뭉탱이로 관리할 수 있는 기능

  - ```java
    타입[] 변수;                 타입 변수[];
    
    ```

  - 타입[] 변수;

  - `int ar [] = new int[4];    , int [] ar = new int[4]; //둘다가능`

  - 범위를 설정해 주어야 함

  - `		System.out.println(Arrays.toString(ar));`: 배열값을 나타내준다.

  - ex.

    ```java
    public static void main(String[] args) {
    		String sr [] = new String[3];
    		sr[0] = "ab";
    		sr[1] = "cd";
    		sr[2] = "ef";		
    
    		System.out.println(Arrays.toString(sr));
    	}
    ```

    

- 2차원 배열
  - `int ar \[][] = new int\[3][]
  
    - 행부터 생성
  
    - ```java
      	public static void main(String[] args) {
              
              // int ar[][] = new int[3][3]; 밑의 코딩과 동일
      		int ar [][] = new int[3][];
      		ar[0] = new int[3];
      		ar[1] = new int[3];
      		ar[2] = new int[3];
      		
      		ar[1][2] = 100;
      		ar[0][1] = 200;
      		
      	}
      ```
  
    - | 0    | 0,0  | 0,1  | 0,2  |
      | ---- | :--: | :--: | :--: |
      | 1    | 1,0  | 1,1  | 1,2  |
      | 2    | 2,0  | 2,1  | 2,2  |

설치

eclipse.org

-> download

-> download package

-> Windows x86_64

