# 4/23 Algorithm coding _ Review

## Team 2 

### q1

- 최대값과 최소값

- ```java
  package team2;
  
  import java.util.Random;
  
  public class q1 {
  
  	public static void main(String[] args) {
  		int i[] = new int[5];
  		
  		Random r = new Random();
  		
  		int max = 0;
  		int min = 99;
  		
  		for (int j : i) {
  			j = r.nextInt(99)+1;
  			System.out.println(j);
  			if(max < j) {
  				max = j;			
  			}
  			if(min> j) {
  				min = j;
  			}
  			
  		}
  
  		System.out.println("max=" + max);	
  		System.out.println("min=" + min);	
  	}
  
  }
  ```

  

### q2

- 배열 내 중복되지 않는 값을 넣고, 들어가지 않은 값의 합을 구해라

- 

  ```java
  package team2;
  
  import java.util.Arrays;
  import java.util.Random;
  
  public class q2 {
  
  	public static void main(String[] args) {
  		int i[] = new int[6];
  		Random r = new Random();
  		int result = 0;
  		
  		for (int j = 0; j < i.length; j++) {
  			i[j] = r.nextInt(9)+1;
  			for (int k = 0; k < j; k++) {
  				if(i[j]==i[k]) {
  					j--;
  				}
  				
  			}
  			
  		}
  		
  		for (int j = 0; j < i.length; j++) {
  			result += i[j];
  		}
  		System.out.println(Arrays.toString(i));
  		System.out.println("들어가지않은 값의 합:"+((9+8+7+6+5+4+3+2+1)-result));
  	}
  
  }
  
  ```

  