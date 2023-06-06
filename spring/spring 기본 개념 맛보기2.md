-   IOC Container

IOC Container는 bean(스프링이 관리하는 객체)를 생성하고, bean끼리 의존성을 엮고, 제공한다.

[##_Image|kage@bHXoaX/btsiPiaZKrh/YtptJaaSYj3DJHWRhunWZ0/img.png|CDM|1.3|{"originWidth":860,"originHeight":205,"style":"alignCenter"}_##]

이런식으로 왼쪽에 초록색 빈이 있는것을 스프링에서 빈으로 처리한다. 

이런경우가 아닌걸 bean으로 만들고싶을땐 @Component, @Bean 애노테이션을 사용하면 된다.

-   PSA

PSA는 Portable Service Abstraction으로, 직독직해하자면...가벼운 서비스 추상화라고 생각하면 될 것같다. 

Spring web MVC등이 PSA에 해당된다. 

\* 앞에서 소개한 IOC, DI, PSA가 Spring의 3대 핵심요소이다. 

-   Proxy

프록시에 대해서는 아래 블로그가 해설이 아주 잘 되어있어서 도움을 받았다.

[https://yejun-the-developer.tistory.com/5](https://yejun-the-developer.tistory.com/5)



프록시는 클라이언트가 요구하는 타켓을 대신하여, 클라이언트가 요구하는것을 받아주는 역할을 한다.

이때 사용되는 프록시는

**타깃 구현 클래스가 상속받은 동일한 타킷 인터페이스를 상속받아야한다.** 

프록시는 사용목적에 따라 두가지 패턴으로 나뉜다.


데코패턴:

타깃에 부가적인 기능을 부여하는 패턴.

하나 이상의 프록시를 사용할 수 있다.


프록시패턴 :

타깃에 대한 접근권한을 제어하는 목적으로 사용하는 패턴.

프록시 패턴을 아래와 같은 특징을 가진다.

1) 타깃에 대한 접근권한을 제어

2) 캐싱 가능

3) 객체생성을 효율적으로 함( 최소한으로, 최대한 미룸)