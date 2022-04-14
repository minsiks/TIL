# 4/11 Java Day6

## Day5 복습 

### Ws03 복습

- `System.out.println(Arrays.toString());` : 배열값을 나타낸다.

- ```java
  public static void main(String[] args) {
  		//int 형 배열 사이즈인 6인 배열을 만들고 1~6 까지의 값을 입력한다
  				//단, 중복되지 않게 입력 한다. 값을 출력한다
  		int ar[] = new int[6];
  		Random rd = new Random();
  		for (int i = 0; i < ar.length; i++) {
  			ar[i] = rd.nextInt(6)+1;
  			for (int j = 0; j < i; j++) {//j값과 i 값을 비교하여 
  				if (ar[i]==ar[j]) { // 같다면 i값이 돌아가라
  					i--;
  				}
  			}
  		}
  		System.out.println(Arrays.toString(ar));
  	}
  ```

### Ws04 복습

- 코딩이전에 논리부터 세우자

## Day 06

> *팀 짜기
>
> 6조 : 김민식,문설연,정세연,홍지훈

- team workshop 문제
- 2개를 만든다.

//for 2중배열 

padlet.com

[6조 (padlet.com)](https://padlet.com/seolyeonmoon/Bookmarks)

시나리오 및 요구사항 정의 후 구현하시오

제출 시 시나리오 및 요구사항을 정리한 후 소스를 올리시오

1. Number Guess Game
   - 조건 : 0~99

2. Lotte Game
