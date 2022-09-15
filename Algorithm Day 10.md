# 9/7 Algorithm Day 09

## 프로그래머스

### 와이즈넛 대비

#### TCP / UDP

#### 직사각형 좌표 구하기

- 스플릿

### 자연수 변환

```java
public class Solution {
    public int solution(int n) {
        int answer = 0;
        String s = Integer.toString(n); //int n을 String으로 변환
        
        for(int i=0; i<s.length(); i++){
            answer += Integer.parseInt(s.substring(i, i+1));
        }
        return answer;
    }
}
```

- String.substring(i,i+1); 스트링 짜르기

### 핸드폰 번호 가리기

```java
class Solution {
  public String solution(String phone_number) {
      String answer = "";
      for(int i = 0; i < phone_number.length(); i++){
         if(i < phone_number.length()-4){
             answer += "*";
         }
         else{
             answer += phone_number.charAt(i);
         }
      }
      return answer;
  }
}
```

- .charAt(n) = n에 있는 글자 뽑아옴
