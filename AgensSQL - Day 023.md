# 24-05-22 AgensSQL Day 22

## AgensSQL 

### AgesnSQL 24-05-20 오전 교육

```
Lock
> ShareLock
>> 주로 조회시 락 , 업데이트는 허용
> ExclusiveLock
>> 배타적 lock 모든 접근 거부
> DeadLock
>> Lock이 걸린 상황에서 다른세션에서 추가적으로 Lock을 잡는 경우
pgAdmin
? JDBC VS ODBC
```

### Lock

- 같은 데이터를 동시에 접근하는 상황에서, 데이터의 무결성과 일관성을 지키기 위해 Lock을 사용

