05-09 13:07:48.673 [DEBUG] o.s.j.support.SQLErrorCodesFactory.getErrorCodes : SQL error codes for 'MySQL' found

05-09 13:07:48.673 [DEBUG] o.s.j.support.SQLErrorCodesFactory.registerDatabase : Caching SQL error codes for DataSource [com.zaxxer.hikari.HikariDataSource@62f6185a]: database product name is 'MySQL'

05-09 13:07:48.680 [DEBUG] o.s.j.s.SQLErrorCodeSQLExceptionTranslator.logTranslation : Translating SQLException with SQL state '40001', error code '1205', message [Lock wait timeout exceeded; try restarting transaction] for task [

\### Error updating database.  Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

\### The error may exist in file [C:\nComzSts\nshop2ndjqgrid\target\classes\com\ncomz\nshop\dao\admin\product\ProductMapper.xml]

\### The error may involve com.ncomz.nshop.dao.admin.product.ProductMapper.updateProduct-Inline

\### The error occurred while setting parameters

\### SQL: UPDATE T_PRODUCT SET /* ProductMapper.xml.updateProduct */    prod_name = ?    , prod_price = ?    , prod_detail = ?    , prod_delivery_info = ?    , prod_refund_info = ?   WHERE prod_id = ?

\### Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

]

org.springframework.dao.CannotAcquireLockException: 

\### Error updating database.  Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

\### The error may exist in file [C:\nComzSts\nshop2ndjqgrid\target\classes\com\ncomz\nshop\dao\admin\product\ProductMapper.xml]

\### The error may involve com.ncomz.nshop.dao.admin.product.ProductMapper.updateProduct-Inline

\### The error occurred while setting parameters

\### SQL: UPDATE T_PRODUCT SET /* ProductMapper.xml.updateProduct */    prod_name = ?    , prod_price = ?    , prod_detail = ?    , prod_delivery_info = ?    , prod_refund_info = ?   WHERE prod_id = ?

\### Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

; Lock wait timeout exceeded; try restarting transaction; nested exception is com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

​	at org.springframework.jdbc.support.SQLErrorCodeSQLExceptionTranslator.doTranslate(SQLErrorCodeSQLExceptionTranslator.java:267)

​	at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:70)

​	at org.mybatis.spring.MyBatisExceptionTranslator.translateExceptionIfPossible(MyBatisExceptionTranslator.java:91)

​	at org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:441)

​	at jdk.proxy2/jdk.proxy2.$Proxy67.update(Unknown Source)

​	at org.mybatis.spring.SqlSessionTemplate.update(SqlSessionTemplate.java:288)

​	at org.apache.ibatis.binding.MapperMethod.execute(MapperMethod.java:67)

​	at org.apache.ibatis.binding.MapperProxy$PlainMethodInvoker.invoke(MapperProxy.java:152)

​	at org.apache.ibatis.binding.MapperProxy.invoke(MapperProxy.java:85)

​	at jdk.proxy2/jdk.proxy2.$Proxy88.updateProduct(Unknown Source)

​	at com.ncomz.nshop.service.admin.product.ProductService.modifyAction(ProductService.java:226)

​	at com.ncomz.nshop.service.admin.product.ProductService$$FastClassBySpringCGLIB$$2cc2bbbb.invoke(<generated>)

​	at org.springframework.cglib.proxy.MethodProxy.invoke(MethodProxy.java:218)

​	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.invokeJoinpoint(CglibAopProxy.java:793)

​	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:163)

​	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:763)

​	at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:123)

​	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:388)

​	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:119)

​	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)

​	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:763)

​	at org.springframework.aop.framework.CglibAopProxy$DynamicAdvisedInterceptor.intercept(CglibAopProxy.java:708)

​	at com.ncomz.nshop.service.admin.product.ProductService$$EnhancerBySpringCGLIB$$94412428.modifyAction(<generated>)

​	at com.ncomz.nshop.controller.admin.product.ProductController.modifyAction(ProductController.java:215)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)

​	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)

​	at java.base/java.lang.reflect.Method.invoke(Method.java:568)

​	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205)

​	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:150)

​	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117)

​	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895)

​	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:808)

​	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)

​	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1071)

​	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:964)

​	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)

​	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:909)

​	at javax.servlet.http.HttpServlet.service(HttpServlet.java:696)

​	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)

​	at javax.servlet.http.HttpServlet.service(HttpServlet.java:779)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:227)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at com.ncomz.nshop.filter.RequestUrlRewritingFilter.doFilter(RequestUrlRewritingFilter.java:120)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)

​	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)

​	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)

​	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:177)

​	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97)

​	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541)

​	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:135)

​	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)

​	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78)

​	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:360)

​	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:399)

​	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)

​	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:891)

​	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1784)

​	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)

​	at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191)

​	at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)

​	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)

​	at java.base/java.lang.Thread.run(Thread.java:833)

Caused by: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

​	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:123)

​	at com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:122)

​	at com.mysql.cj.jdbc.ClientPreparedStatement.executeInternal(ClientPreparedStatement.java:916)

​	at com.mysql.cj.jdbc.ClientPreparedStatement.execute(ClientPreparedStatement.java:354)

​	at net.sf.log4jdbc.sql.jdbcapi.PreparedStatementSpy.execute(PreparedStatementSpy.java:443)

​	at com.zaxxer.hikari.pool.ProxyPreparedStatement.execute(ProxyPreparedStatement.java:44)

​	at com.zaxxer.hikari.pool.HikariProxyPreparedStatement.execute(HikariProxyPreparedStatement.java)

​	at jdk.internal.reflect.GeneratedMethodAccessor57.invoke(Unknown Source)

​	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)

​	at java.base/java.lang.reflect.Method.invoke(Method.java:568)

​	at org.apache.ibatis.logging.jdbc.PreparedStatementLogger.invoke(PreparedStatementLogger.java:59)

​	at jdk.proxy3/jdk.proxy3.$Proxy116.execute(Unknown Source)

​	at org.apache.ibatis.executor.statement.PreparedStatementHandler.update(PreparedStatementHandler.java:47)

​	at org.apache.ibatis.executor.statement.RoutingStatementHandler.update(RoutingStatementHandler.java:74)

​	at org.apache.ibatis.executor.SimpleExecutor.doUpdate(SimpleExecutor.java:50)

​	at org.apache.ibatis.executor.BaseExecutor.update(BaseExecutor.java:117)

​	at org.apache.ibatis.executor.CachingExecutor.update(CachingExecutor.java:76)

​	at org.apache.ibatis.session.defaults.DefaultSqlSession.update(DefaultSqlSession.java:197)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)

​	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)

​	at java.base/java.lang.reflect.Method.invoke(Method.java:568)

​	at org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:427)

​	... 73 more

05-09 13:07:48.685 [DEBUG] o.s.j.d.DataSourceTransactionManager.commit : Transactional code has requested rollback

05-09 13:07:48.687 [DEBUG] o.s.j.d.DataSourceTransactionManager.processRollback : Initiating transaction rollback

05-09 13:07:48.688 [DEBUG] o.s.j.d.DataSourceTransactionManager.doRollback : Rolling back JDBC transaction on Connection [HikariProxyConnection@1863233535 wrapping net.sf.log4jdbc.sql.jdbcapi.ConnectionSpy@ebec66d]

05-09 13:07:48.697 [DEBUG] o.s.j.d.DataSourceTransactionManager.doCleanupAfterCompletion : Releasing JDBC Connection [HikariProxyConnection@1863233535 wrapping net.sf.log4jdbc.sql.jdbcapi.ConnectionSpy@ebec66d] after transaction

05-09 13:07:48.708 [DEBUG] o.s.w.s.m.m.a.RequestResponseBodyMethodProcessor.writeWithMessageConverters : Using 'text/plain', given [*/*] and supported [text/plain, */*, text/plain, */*, application/json, application/*+json, application/json, application/*+json]

05-09 13:07:48.709 [DEBUG] o.s.w.s.m.m.a.RequestResponseBodyMethodProcessor.traceDebug : Writing ["<EOL><EOL>### Error updating database.  Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackExceptio (truncated)..."]

05-09 13:07:48.713 [DEBUG] o.s.web.servlet.DispatcherServlet.logResult : Completed 200 OK

05-09 13:08:11.978 [ERROR] jdbc.sqltiming.exceptionOccured :  com.zaxxer.hikari.pool.ProxyPreparedStatement.execute(ProxyPreparedStatement.java:44)

10. UPDATE T_PRODUCT SET /* ProductMapper.xml.updateProduct */

​			prod_name = '티맥스 레이저캐디'

​			, prod_price = '5000'

​			, prod_detail = '<p><img src="/common/file/downloadImage/572" width="750" height="3128" /></p>'

​			, prod_delivery_info = '<p>zz</p>'

​			, prod_refund_info = '<p>zz</p>'

​		WHERE prod_id = '24'

 {FAILED after 53019 msec}

com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

​	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:123)

​	at com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:122)

​	at com.mysql.cj.jdbc.ClientPreparedStatement.executeInternal(ClientPreparedStatement.java:916)

​	at com.mysql.cj.jdbc.ClientPreparedStatement.execute(ClientPreparedStatement.java:354)

​	at net.sf.log4jdbc.sql.jdbcapi.PreparedStatementSpy.execute(PreparedStatementSpy.java:443)

​	at com.zaxxer.hikari.pool.ProxyPreparedStatement.execute(ProxyPreparedStatement.java:44)

​	at com.zaxxer.hikari.pool.HikariProxyPreparedStatement.execute(HikariProxyPreparedStatement.java)

​	at jdk.internal.reflect.GeneratedMethodAccessor57.invoke(Unknown Source)

​	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)

​	at java.base/java.lang.reflect.Method.invoke(Method.java:568)

​	at org.apache.ibatis.logging.jdbc.PreparedStatementLogger.invoke(PreparedStatementLogger.java:59)

​	at jdk.proxy3/jdk.proxy3.$Proxy116.execute(Unknown Source)

​	at org.apache.ibatis.executor.statement.PreparedStatementHandler.update(PreparedStatementHandler.java:47)

​	at org.apache.ibatis.executor.statement.RoutingStatementHandler.update(RoutingStatementHandler.java:74)

​	at org.apache.ibatis.executor.SimpleExecutor.doUpdate(SimpleExecutor.java:50)

​	at org.apache.ibatis.executor.BaseExecutor.update(BaseExecutor.java:117)

​	at org.apache.ibatis.executor.CachingExecutor.update(CachingExecutor.java:76)

​	at org.apache.ibatis.session.defaults.DefaultSqlSession.update(DefaultSqlSession.java:197)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)

​	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)

​	at java.base/java.lang.reflect.Method.invoke(Method.java:568)

​	at org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:427)

​	at jdk.proxy2/jdk.proxy2.$Proxy67.update(Unknown Source)

​	at org.mybatis.spring.SqlSessionTemplate.update(SqlSessionTemplate.java:288)

​	at org.apache.ibatis.binding.MapperMethod.execute(MapperMethod.java:67)

​	at org.apache.ibatis.binding.MapperProxy$PlainMethodInvoker.invoke(MapperProxy.java:152)

​	at org.apache.ibatis.binding.MapperProxy.invoke(MapperProxy.java:85)

​	at jdk.proxy2/jdk.proxy2.$Proxy88.updateProduct(Unknown Source)

​	at com.ncomz.nshop.service.admin.product.ProductService.modifyAction(ProductService.java:226)

​	at com.ncomz.nshop.service.admin.product.ProductService$$FastClassBySpringCGLIB$$2cc2bbbb.invoke(<generated>)

​	at org.springframework.cglib.proxy.MethodProxy.invoke(MethodProxy.java:218)

​	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.invokeJoinpoint(CglibAopProxy.java:793)

​	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:163)

​	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:763)

​	at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:123)

​	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:388)

​	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:119)

​	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)

​	at org.springframework.aop.framework.CglibAopProxy$CglibMethodInvocation.proceed(CglibAopProxy.java:763)

​	at org.springframework.aop.framework.CglibAopProxy$DynamicAdvisedInterceptor.intercept(CglibAopProxy.java:708)

​	at com.ncomz.nshop.service.admin.product.ProductService$$EnhancerBySpringCGLIB$$94412428.modifyAction(<generated>)

​	at com.ncomz.nshop.controller.admin.product.ProductController.modifyAction(ProductController.java:215)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)

​	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)

​	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)

​	at java.base/java.lang.reflect.Method.invoke(Method.java:568)

​	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205)

​	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:150)

​	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117)

​	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895)

​	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:808)

​	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)

​	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1071)

​	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:964)

​	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)

​	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:909)

​	at javax.servlet.http.HttpServlet.service(HttpServlet.java:696)

​	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)

​	at javax.servlet.http.HttpServlet.service(HttpServlet.java:779)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:227)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at com.ncomz.nshop.filter.RequestUrlRewritingFilter.doFilter(RequestUrlRewritingFilter.java:120)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)

​	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)

​	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)

​	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)

org.springframework.dao.CannotAcquireLockException: 

\### Error updating database.  Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

\### The error may exist in file [C:\nComzSts\nshop2ndjqgrid\target\classes\com\ncomz\nshop\dao\admin\product\ProductMapper.xml]

\### The error may involve com.ncomz.nshop.dao.admin.product.ProductMapper.updateProduct-Inline

\### The error occurred while setting parameters

\### SQL: UPDATE T_PRODUCT SET /* ProductMapper.xml.updateProduct */    prod_name = ?    , prod_price = ?    , prod_detail = ?    , prod_delivery_info = ?    , prod_refund_info = ?   WHERE prod_id = ?

\### Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

; Lock wait timeout exceeded; try restarting transaction; nested exception is com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

​	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)

​	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)

​	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:177)

​	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97)

​	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541)

​	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:135)

​	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)

​	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78)

​	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:360)

​	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:399)

​	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)

​	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:891)

​	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1784)

​	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)

​	at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191)

​	at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)

​	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)

​	at java.base/java.lang.Thread.run(Thread.java:833)

05-09 13:08:15.460 [DEBUG] o.s.j.s.SQLErrorCodeSQLExceptionTranslator.logTranslation : Translating SQLException with SQL state '40001', error code '1205', message [Lock wait timeout exceeded; try restarting transaction] for task [

\### Error updating database.  Cause: com.mysql.cj.jdbc.exceptions.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction

\### The error may exist in file [C:\nComzSts\nshop2ndjqgrid\target\classes\com\ncomz\nshop\dao\admin\product\ProductMapper.xml]

\### The error may involve com.ncomz.nshop.dao.admin.product.ProductMap
