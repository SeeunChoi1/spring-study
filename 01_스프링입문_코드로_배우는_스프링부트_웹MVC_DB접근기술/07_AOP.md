# AOP

- 공통 관심사항 (cross-cutting concern)
- 핵심 비즈니스와 공통 관심사항이 섞이면 유지보수가 힘들다
- 수정이 필요하면 다 찾아서 수정해줘야한다

```java
package hello.hellospring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))") // 타게팅, 어디에 AOP 적용할거야?
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("START : " + joinPoint.toString());
        try{
            return joinPoint.proceed();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}
```

- 가짜 spring bean을 올림 (프록시)
- 대상으로하는 컴포넌트 injection받을 때 확인해볼 수 있음

    ```java
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
        System.out.println("memberService = " + memberService.getClass());
    }
    ```

    - DI : 뭔지 모르겟고 난 그냥 받아서 쓸게
    - 자바 코드를 앞뒤로 붙여주고 그런 기술도 있음...