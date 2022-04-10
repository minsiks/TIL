# 4/6 Java Day3

```java
String : 큰따옴표 사용 ("")문자열을 담고 출력 가능 (reference Type)
.toLowerCase() : 소문자로 변환
.equals() : 같은 값을 구할때 사용 (ex.s1. equals(s2))
== : 주소 비교, equals와 비슷하지만 다름
```



## JAVA / JDK / JVM 구조

★ CODE / STACK / HEAP

\-----------------------------        -----------------------------         -----------------------------
l         CODE                l         l         STACK               l         l            HEAP             l
l                                l         l                                l         l                                l
l         실행한               l         l          지역변수           l         l                                l
l                                l         l          매개 변수          l         l                                l
l                                l         l          리턴 값             l         l                                l
l                                l         l                                l         l                                l
l                                l         l                                l         l                                l
l                                l         l                                l         l                                l

\----------------------------         -----------------------------         --------------------



## 조건문과 반복문



- 조건문 (if, switch문)
- 반복문 (for문, while문, do-while(예전에 쓰던거))
- break문, continue문



> ```
> - Ctrl + Space : 자동완성
> - System.out.printf(%d, %s, %f) 정수, 문자열, 실수 (Ex. System.out.printf("*"%"\n",num);)
> *\n은 다음줄로 가는것
> - ||:or
> - &&, & : and
> - integer.parselnt(num); : 이게뭐였더라
> - n1.charAt(0) : 이것도 뭐지
> - %.nf : 소수점 n자리까지 반올림
> ```



> > [Eclips 단축키 참고 링크](https://iamfreeman.tistory.com/entry/Eclipse-%EB%8B%A8%EC%B6%95%ED%82%A4-%EB%AA%A8%EC%9D%8C-%EC%9D%B4%ED%81%B4%EB%A6%BD%EC%8A%A4-Effective-Eclipse-Shortcut-Keys)