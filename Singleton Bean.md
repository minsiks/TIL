Singleton Bean 완전 정리
1. Singleton이란?
디자인 패턴으로, 애플리케이션 전체에서 인스턴스가 딱 하나만 존재하도록 보장하는 패턴입니다.

2. Spring에서의 Singleton Bean
Spring IoC 컨테이너는 기본적으로 모든 Bean을 Singleton으로 관리합니다.
java@Service
public class MyService {
    // 이 클래스의 인스턴스는 Spring 컨테이너에 딱 하나만 존재
}
java@Autowired
MyService serviceA;

@Autowired
MyService serviceB;

// serviceA == serviceB → true (같은 인스턴스)

3. Bean Scope 종류 (비교)
Scope설명singleton컨테이너당 인스턴스 1개 (기본값)prototype요청할 때마다 새 인스턴스 생성requestHTTP 요청마다 1개 (Web)sessionHTTP 세션마다 1개 (Web)applicationServletContext당 1개 (Web)
java@Scope("prototype")  // singleton이 아닌 경우 명시
@Component
public class MyPrototypeBean { }

4. Singleton Bean의 생명주기
컨테이너 시작
    ↓
Bean 인스턴스 생성 (딱 1번)
    ↓
의존성 주입 (DI)
    ↓
초기화 콜백 (@PostConstruct)
    ↓
사용 (애플리케이션 동작 내내)
    ↓
소멸 콜백 (@PreDestroy)
    ↓
컨테이너 종료
java@Service
public class MyService {

    @PostConstruct
    public void init() {
        System.out.println("Bean 초기화!");  // 딱 1번 실행
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Bean 소멸!");  // 컨테이너 종료 시 실행
    }
}

5. ⚠️ 가장 중요한 주의사항 — 상태(State) 문제
Singleton Bean은 멀티스레드 환경에서 여러 요청이 같은 인스턴스를 공유합니다.
🚨 위험한 코드
java@Service
public class OrderService {

    private int count = 0;  // ← 위험! 공유 상태

    public void order() {
        count++;  // 여러 스레드가 동시에 접근 → 데이터 꼬임
    }
}
✅ 올바른 코드
java@Service
public class OrderService {

    // 상태를 갖지 않음 (Stateless)
    public void order(int count) {  // 상태는 파라미터로 받기
        // count를 여기서 처리
    }
}
핵심 원칙: Singleton Bean은 반드시 Stateless(무상태)로 설계해야 합니다.

6. Singleton Bean에 Prototype Bean 주입 시 문제
java@Service  // singleton
public class MyService {

    @Autowired
    private MyPrototypeBean prototypeBean;  // 🚨 문제 발생!
    
    // prototypeBean이 singleton처럼 동작해버림
    // (MyService가 생성될 때 딱 1번만 주입되기 때문)
}
✅ 해결방법 — ApplicationContext 직접 사용
java@Service
public class MyService implements ApplicationContextAware {

    private ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext ctx) {
        this.context = ctx;
    }

    public void doSomething() {
        MyPrototypeBean bean = context.getBean(MyPrototypeBean.class);
        // 매번 새 인스턴스를 가져옴
    }
}
✅ 해결방법 2 — ObjectProvider (더 세련된 방법)
java@Service
public class MyService {

    @Autowired
    private ObjectProvider<MyPrototypeBean> prototypeBeanProvider;

    public void doSomething() {
        MyPrototypeBean bean = prototypeBeanProvider.getObject();
        // 매번 새 인스턴스
    }
}

7. 순수 Java Singleton vs Spring Singleton 차이
순수 Java SingletonSpring Singleton범위JVM 전체에서 1개Spring 컨테이너당 1개구현직접 getInstance() 구현컨테이너가 자동 관리테스트어려움쉬움 (DI로 교체 가능)멀티 컨테이너무조건 1개컨테이너마다 1개

8. 실무 요약


Spring Bean의 기본 Scope는 Singleton
컨테이너 시작 시 생성, 종료 시 소멸
반드시 Stateless로 설계
인스턴스 변수에 상태 저장 ❌ → 지역변수, 파라미터 활용 ✅
Singleton에 Prototype 주입 시 ObjectProvider 사용