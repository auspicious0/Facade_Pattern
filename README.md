# Java 파사드 패턴 (Facade Pattern)

# 목차
   
   1. [파사드 패턴이란?](#파사드-패턴이란)
      
   2. [파사드 패턴의 효과](#파사드-패턴의-효과)
      
   3. [예제 코드](#예제-코드)
      
   4. [실행 결과](#실행-결과)
      
   5. [참고](#참고)


## 파사드 패턴이란?

파사드 패턴은 서브 시스템을 보다 쉽게 쓸 수 있도록 높은 수준의 인터페이스를 정의하는 작업으로, 이미 수많은 API 서비스와 라이브러리, 패키지에서 파사드 패턴을 응용 중이다.

파사드 패턴은 강력한 결합 구조를 해결하기 위해 코드의 의존성을 줄이고 느슨한 결합으로 구조를 변경한다.

파사드 패턴은 새로운 인터페이스 계층을 추가하며 시스템 간 의존성을 해결한다. 인터페이스 계층은 메인 시스템과 서브 시스템의 연결 관계를 대신 처리한다. 인터페이스는 한 개 일 수 있고 여러 개 일 수도 있으며, 함수 형태로 제공될 수도 있다. 객체의 내부 구조를 상세히 알 필요가 없이 시스템의 연결성과 종속성을 최소화하는 것이 파사드 패턴의 핵심이다.

## 파사드 패턴의 효과

1. 서브 시스템 보호: 서브 시스템의 구성 요소를 직접 호출하지 않으므로 잘못된 사용을 방지한다. 내부 구조와 외부 사용을 구분하므로 추후 서브 시스템 업그레이드에 자유롭다.

2. 확장성: 코드 변경 시 상위 시스템에는 파사드를 이용하므로 서브 시스템이 변경되어도 큰 변화를 느낄 수 없다. 확장성을 고려하면서 서브 시스템 기능을 유지할 수 있도록 완충하는 역할을 한다.

3. 결합도 감소: 서브 시스템이 복잡하고 종속성이 강할 때는 파사드 패턴을 이용한다면 복잡한 종속적 결합도를 낮추고 독립적인 코드 유지 가능하다.

4. 계층화: 서브 시스템이 계층화된 구조를 갖더라도 파사드는 계층 단계별로 접근하여 행위를 호출할 수 있다.

5. 이식성: 여러 작업을 하나의 묶음으로 처리하여 복잡한 클래스를 단순화한다. 서브 시스템을 공통적으로 사용할 수 있도록 이식성을 향상한다.

6. 공개 인터페이스: 외부에 공개되는 기능과 공개되지 않는 기능을 구분할 수 있다. 인터페이스를 제공함과 동시에 서브 시스템의 기능을 캡슐화하여 일시적으로 특정 기능을 감출 수 있다.

## 예제 코드

```java
public class SayHi {
    private boolean hi = false;

    public void sayHi() {
        hi = true;
        System.out.println("안녕하세요");
    }

    public void sayBye() {
        hi = false;
        System.out.println("안녕히계세요");
    }

    public boolean inOffice() {
        return hi;
    }
}

public class Lunch {
    private boolean hi = false;

    public void sayHi() {
        hi = true;
        System.out.println("식사 맛있게 하십쇼");
    }

    public void sayBye() {
        hi = false;
        System.out.println("맛있게 드셨습니까");
    }

    public boolean inOffice() {
        return hi;
    }
}

public class Vacation {
    private boolean hi = false;

    public void sayHi() {
        hi = true;
        System.out.println("편히 쉬셨습니까");
    }

    public void sayBye() {
        hi = false;
        System.out.println("편히 쉬십쇼");
    }

    public boolean inOffice() {
        return hi;
    }
}

public class The_Office {
    private SayHi sayhi;
    private Lunch lunch;
    private Vacation vacation;

    public The_Office(SayHi sayhi, Lunch lunch, Vacation vacation) {
        this.sayhi = sayhi;
        this.lunch = lunch;
        this.vacation = vacation;
    }

    public void OfficeIn() {
        System.out.println("\n회사에 왔으니 인사를 해보자.");
        if (!sayhi.inOffice()) {
            sayhi.sayHi();
        }
        if (!lunch.inOffice()) {
            lunch.sayHi();
        }
        if (!vacation.inOffice()) {
            vacation.sayHi();
        }
    }

    public void OffcieOut() {
        System.out.println("\n얼른 집에가서 푹 쉬자.");
        if (sayhi.inOffice()) {
            sayhi.sayBye();
        }
        if (lunch.inOffice()) {
            lunch.sayBye();
        }
        if (vacation.inOffice()) {
            vacation.sayBye();
        }
    }
}

public class TestPattern {
    public static void main(String[] args) {
        SayHi sayhi = new SayHi();
        Lunch lunch = new Lunch();
        Vacation vacation = new Vacation();
        
        // 파사드를 적용하지 않는다면
        
        System.out.println("===========출근 파사드 적용전===========\n");
        sayhi.sayHi();
        lunch.sayHi();
        vacation.sayHi();
        
        System.out.println("\n===========퇴근 파사드 적용전===========\n");
        sayhi.sayBye();
        lunch.sayBye();
        vacation.sayBye();
        
        System.out.println("\n===========출근 파사드 적용===========\n");
        The_Office office = new The_Office(sayhi, lunch, vacation);
        office.OfficeIn();
        
        System.out.println("\n===========퇴근 파사드 적용후===========\n");
        office.OffcieOut();
    }
}
```

## 실행 결과

![image](https://github.com/auspicious0/Facade_Pattern/assets/108572025/5b0fe9cd-47b0-4fd1-92d7-b38d304fa905)


## 참고

출처: https://hirlawldo.tistory.com/172 [노바의 개발도서관:티스토리]

출처: https://break-over.tistory.com/47 [만들어가는 세상:티스토리]


