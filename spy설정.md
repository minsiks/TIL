# spy 설정 


#################################################################
# P6Spy 3.9.1 spy.properties
#################################################################

# 어떤 모듈을 쓸지 (보통 기본 모듈로 충분해서 module.log는 굳이 안 씀)
# modulelist=com.p6spy.engine.spy.P6SpyFactory

# 실제 JDBC Driver (Tibero)
driverlist=com.tmax.tibero.jdbc.TbDriver

# URL이 jdbc:p6spy: 로 시작하는 경우 true 권장
useprefix=true

# 드라이버 선점 방지 (유지)
deregisterdrivers=true

# 로그 출력 방식 (파일)
appender=com.p6spy.engine.spy.appender.StdoutLogger
logfile=C:/NSIS_Studio/logs/miw/miw_spysql.log
append=true
autoflush=true

# 로그 포맷
logMessageFormat=com.p6spy.engine.spy.appender.CustomLineFormat
customLogMessageFormat=%(executionTime) ms | %(sql)
# 로그 레벨(INFO/DEBUG/ERROR)
loglevel=DEBUG

# 카테고리 필터(원하면 조정)
# 가능한 값: error, info, batch, debug, statement, commit, rollback, result, resultset
excludecategories=debug,result,resultset,batch,info

# 느린 쿼리만 보고 싶으면 ms 설정 (예: 500)
# executionthreshold=500

# 바인된 쿼리 출력
includeBindings=true