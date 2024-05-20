# 24-05-20 AgensSQL Day 21

## AgensSQL 

### AgesnSQL 24-05-20 오전 교육

```
HA (High Availability)
> 오라클은 active, active 
>> Primary DB 와 Standby 테이블이 존재하지 않고 두 디비 모두 활성상태
> postgresql은 wal파일을 복제하여 Replication
> failover이 HA의 핵심 (Primary가 장애 시 Standby로 전환)
> 주로 postgre는 상용 솔루션으로 HA를 구성
> drdb를 주로 사용 오라클의 rac와 비교
>> drbd : HA를 위한 솔루션
> 리플리케이션의 부하가 주로 문제 (리플리케이션을 활용한 ha는 사용하지 않음)

```



