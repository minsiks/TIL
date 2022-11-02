# 6/16 Spring Framework Day 13

## SPRINGBOOT

### 1. 다국어 설정

[[springboot 다국어 처리\] : 네이버 카페 (naver.com)](https://cafe.naver.com/2022webservice)

1. 언어펙 위치 설정 Configuration

```java
package com.multi;

import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.context.MessageSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.support.ResourceBundleMessageSource;

@Configuration
public class WebConfig implements ApplicationContextAware {

    ApplicationContext context;

    @Bean
    public MessageSource messageSource() {
        ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
        messageSource.setBasenames("messages/message");
        messageSource.setDefaultEncoding("UTF-8");
        return messageSource;
    }

    @Override
    public void setApplicationContext(ApplicationContext context) {
        this.context = context;
    }
}
```

​	1.1 src/main/resources 디렉토리에 messges 디렉토리 생성 

​	1.2 언어별 파일 생성 

​	messge_en_US.properties

​	messge_kr_KR.properties

​	messge_zn_CN.properties

​	messge.properties : default 언어, 사용언어가 없을 때 적용

```java
home.welcome= \uD658\uC601\uD569\uB2C8\uB2E4. {0} {1}
tel=010-9999-9898
```

2. 사용자가 사용하는 브라우져의 사용 언어 확인

```java
package com.multi;

import java.util.Locale;

import org.springframework.context.annotation.Configuration;
import org.springframework.context.i18n.LocaleContext;
import org.springframework.context.i18n.SimpleLocaleContext;
import org.springframework.web.server.ServerWebExchange;
import org.springframework.web.server.i18n.LocaleContextResolver;

@Configuration
public class LocaleResolve implements LocaleContextResolver {

    @Override
    public LocaleContext resolveLocaleContext(ServerWebExchange exchange) {
        //String language = exchange.getRequest().getHeaders().getFirst("Accept-Language");
        String language = exchange.getRequest().getQueryParams().getFirst("lang");
        Locale targetLocale = Locale.getDefault();
        if (language != null && !language.isEmpty()) {
            targetLocale = Locale.forLanguageTag(language);
        }
        return new SimpleLocaleContext(targetLocale);
    }

    @Override
    public void setLocaleContext(ServerWebExchange exchange, LocaleContext localeContext) {
        throw new UnsupportedOperationException("Not Supported");
    }

}
```

3. 언어팩 적용

```java
package com.multi;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.MessageSource;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.i18n.LocaleContext;
import org.springframework.web.server.ServerWebExchange;

@Configuration
public class MessageByLocale {

    @Autowired
    private MessageSource messageSource;

    @Autowired
    private LocaleResolve localeResolver;

    public String getMessage(String code, ServerWebExchange exchange) {
        LocaleContext localeContext = localeResolver.resolveLocaleContext(exchange);
        return messageSource.getMessage(code, null, localeContext.getLocale());
    }
}
```

4. 화면에 적용

```java
<h1 th:text="#{home.welcome('hi','kim')}">환영 합니다.</h1>
<p th:text="#{tel}">Lorem consequat.</p>
```



