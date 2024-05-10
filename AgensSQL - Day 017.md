# 24-05-10 AgensSQL Day 17

## AgensSQL 

### AgesnSQL 24-05-10 오전 교육

```
Query Explain
> Explain : 계획만
> cost : 좌측 -> 첫번째 나오는 row 비용 / 우측 -> 마지막 나오는 row 비용
> Explain Analyze : 실제 실행후 결과
>> loop : 몇번의 결과를 looping 하는지 확인필요
> 쿼리 성능 주의
>> sorting, grouping
>> Inline SUbquery, Function을 셀렉트 문에서 사용시 과하게 데이터 조회
>> On 절에 Between 사용시 loop 반복

```



