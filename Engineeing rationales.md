# Spring boot 사용 결정 이유

인터넷에서 가장 널리 사용되는 standard한 stateless 프로토콜인 HTTP를 사용해 작업하기로 했습니다. 그러다 보니 서버 프로그래밍에서 HTTP 서버를 만들기 위해 가장 많이 사용되는 자바 프레임워크인 스프링 웹 + 부트를 사용하기로 했습니다.

# Spring security 사용 결정 이유

쉽고 빠르게 authentication과 authorization을 구현하기 위해 사용했습니다.

# QueryDsl 사용 결정 이유

검색 및 필터링 기능은 일반적인 쿼리문 작성 방식으로 구현하기 까다로운 부분이 있습니다. 이에 따라, 동적 쿼리문을 작성하기 위해 QueryDsl을 사용했습니다.

# JPA 사용 결정 이유

Jpa를 사용하면 도메인 클래스가 Jpa 관련 annotation 및 추가 작업에 영향을 받습니다. 

하지만, 그럼에도 불구하고 Jpa를 사용하기로 선택한 것은 아키택처적 결정이었는데요. 저희 팀은 Data Access Layer를 만들어 Data access 책임을 어플리케이션의 다른 부분에서 분리하고자 했습니다. 

이때, Jpa를 사용하지 않고 스스로 Data access layer를 만들 경우 다음과 같은 장/단점이 있습니다.

- 장점

  1. 도메인 클래스가 어떤 프레임워크의 영향도 받지 않는다. 이에 따라, 도메인 클래스를 어떤 제약도 없이 설계가 가능하다.

- 단점

  1. RDB를 쓰거나 NoSql 계열 DB, 혹은 파일 시스템을 사용한다면, 각 DB의 패러다임과 Java의 OOP 패러다임에 격차가 발생하여 그 격차를 줄이는 작업을 직접 해야한다. 예를 들어, RDB는 reference key를 통한 join을 기반으로 엔터디 사이의 관계를 정의하지만, OOP는 reference를 통해 오브젝트 사이의 관계를 정의한다. 이 패러다임의 격차를 좁혀야한다. 그렇지 않으면 DB의 패러다임으로 인해 도메인 클래스가 영향받을 가능성이 크다.
  2. Persistence layer implementation을 직접 구현해야한다.

  

반면, Jpa를 사용한다면 다음과 같은 장/단점이 있습니다.

- 장점
  1. Jpa를 사용할 줄 아는 사람이 있었기 때문에, 낮은 Learning curve로 바로 작업에 들어갈 수 있다.
  2. ORM을 통해 RDB와 OOP 사이의 패러다임 차이를 상당부분 극복 가능하다
  3. Persistence layer Implementation을 직접 구현하지 않아도 된다.

- 단점
  1. 도메인 클래스가 Jpa의 제약사항과 요구사항으로 인해 더럽혀진다. 예를 들어, Jpa 엔터티는 final class가 될 수 없으며, public, 혹은 protected인 No argument constructor를 무조건 생성해야한다. 또한, RDB 관계사항이 entity에 @Annotation을 통해 드러나게 된다.

물론 도메인 클래스가 특정 프레임워크에 종속되면 추후 persistence 프레임워크가 바뀔 때 도메인 클래스 및 레포지토리 인터페이스를 전부 수정해야 한다는 치명적인 단점이 있습니다. 하지만 ORM이 RDB와 OOP 사이의 간격을 줄여주기 때문에 RDB를 신경쓰지 않고 OOP 설계에 집중할 수 있도록 만들어주고, persistence에 대한 구현 부담을 줄여준다는 점에서 Jpa를 채택하게 되었습니다.

# GRASP Principle

## Information expert & Protected Variations

Encapsulation을 가능한 사용하였습니다. 도메인 모델에서 추출한 클래스들이 자신이 아는 요소에 대해선 자신이 스스로 처리할 수 있게 설계했습니다. 예를 들어, `User`클래스는 자신에게 주어진 권한이 어떤것인지 알고있기 때문에, 권한에 관련된 내용은 스스로 처리합니다. 또한 `Issue.Status`클래스는 스스로 어떤 Status에서 어떤 Status로 넘어갈 수 있는지 알고있어서 현재 상태에서 특정 상태로 넘어갈 수 있는지 판단할 수 있습니다. 이를 통해 특정 operation에 변화를 주려할 때, 그 operation에 대한 정보가 있는 클래스로 가서 내부 구현만 바꿔주면 encapsulation이 되어있기 때문에 다른 클래스들은 영향을 받지 않습니다.

## Creator

Creator는 Issue 생성과 Comment생성에 사용되었습니다. 

Issue의 경우, `Issue`를 composite하는 `Project`에서 생성됩니다. `Comment`의 경우, 유저가 입력해준 코멘트를 받는 `CommentService`에서 생성됩니다.

## Controller

Controller는 HTTP 메시지를 받는 ~Controller 클래스들 (org/kkycp/server/controller) 에서 사용되었습니다. 해당 클래스들은 요청을 받을 url과 데이터 구조를 정의합니다. 그렇게 외부 요청 데이터를 받으면, 그 데이터들을 정제해서 ~Service 클래스들로 넘깁니다.

## Indirection & Pure fabrication

Indirection과 Pure fabrication은 ~Service 클래스들 (org/kkycp/server/services) 에서 사용되었습니다. Service 클래스들은 Domain과 직접적으로 관련돼있지는 않지만, Domain layer와 Infrastructure layer를 연결합니다. 요청이 Service 클래스로 전달되면, Service클래스는 권한 확인을 Security layer에 요청한 뒤, Persistence layer에서 Domain 오브젝트를 가져와 Domain 오브젝트가 작업을 수행하게 합니다. 그 후, Domain 오브젝트를 다시 Persistence layer에 저장하는 식으로 Infra와 Domain을 연결합니다.

## Low coupling  & High cohesion

Low coupling  & High cohesion은 Controller, Service, Security, Persistence, Domain으로 layer를 나누어 달성하였습니다. 각 layer의 역할이 명확히 나누어져 있기 때문에, 어플리케이션에 수정사항이 생겼을 때 어플리케이션의 여러 부분이 영향받는 것이 아니라 각 layer만 영향을 받았으며, 여러 부분에 걸쳐 수정을 가하지 않아도 되게 되었습니다.

## Polymorphism

한 행동에 대해 다른 클래스들을 할당해야 할 정도로 요구사항이 복잡하지 않았기 때문에 도메인 클래스에서 사용할 일은 없었지만, 프레임워크를 사용하면서 많이 사용했습니다. 예를 들어, Spring Security 설정파일  (org/kkycp/server/auth/SecurityConfig.java)에서는 authentication implementation으로 basicLogin (for http basic authentication) 방식을 formLogin (cookie based form login) 방식으로 바꾸는 것만으로 authentication 방식을 전환할 수 있습니다. 

# Solid

## Single responsibility principle 

 `org/kkycp/server/auth/jpa/AuthUserDetails.java`와 `org/kkycp/server/domain/User.java`는 모두 개념적으로는 시스템 유저를 나타냅니다. 하지만 AuthUserDetails는 Spring Security에서 인증 시 사용되는 정보를 담고있고, User는 도메인 정책적인 권한에 대한 정보를 담고있습니다. 두 클래스는 이에 따라 다른 변할 이유를 가지고 있기 때문에 비록 같은 개념을 나타내지만 SRP에 따라 분리되었습니다.

## Open-closed principle

유저의 권한을 확인하는 클래스 (org/kkycp/server/auth/authorizaiton/PrivilegeChecker.java) 에서 Strategy 패턴을 이용하였습니다. PrivilegeChecker#doBasicAuthWith() 에서는 BooleanSupplier 인터페이스를 인자로 받습니다. 이 메소드는 기본적인 권한 처리를 한 후, BooleanSupplier 인자를 통해 받은 커스텀 권한 확인 구현으로 추가로 권한을 확인합니다. 이를 이용하면 새로운 권한 확인 절차를 추가하고 싶을 때, PrivilegeChecker#doBasicAuthWith() 와 함께 커스템 권한 확인 구현을 추가하면 다른 코드를 건드리지 않고 새로운 권한 확인 절차를 추가할 수 있습니다.

## Liskov substitution principle

같은 타입 즉, 같은 인터페이스를 공유하는 도메인 객체가 없었기 때문에 상속은 사용하지 않았습니다. 코드 재사용은 모두 composition을 통해 이루어집니다.

## Interface segregation principle

Jpa를 사용함에 있어, 각 repository 클래스들이 필요한 최소의 jpa 인터페이스만 상속하게 하여, 불필요한 기능 (ex. pagination or sorting)에 대한 인터페이스를 각 repository가 상속하지 않도록 함.

## Dependency inversion principle

`org/kkycp/server/repo/issue/IssueStatisticsRepo.java`와 `org/kkycp/server/repo/issue/IssueSearchRepo.java` 인터페이스를 만들고, 어플리케이션의 Service 객체들이 이 인터페이스들에 의존하게 만들었다. 따라서 Repository를 사용하는 Service와 인터페이스의 구현체들이 모두 추상화에 의존하도록 만들었다.

