---
layout: post
title: Architechture - MVC, MVP, MVVM ?
subtitle: A heading to clean code


---

<br>

오늘은 Architechture Patterns을 공부한 내용을 정리해보려고 한다.

- <a herf="#1" style="color:black;">Architechture Pattern을 공부한 이유</a>
- <a herf="#2" style="color:black;">MVC, MVP, MVVM</a>
- <a herf="#3" style="color:black;">마무리</a>

<br>

<h3 id="1">Architechture Pattern을 공부한 이유  : be a cooool code ✨ </h3>

나는 올해 초까지만 하더라도, 개발 구조에 대한 것을 알지 못했다.

MVC 패턴을 쓰고 있다 하더라도 왜 쓰는지, 어떤 원리로 동작하는지 이해하지 않은 채 사용하고 있었다. Architechture Pattern의 중요함은 익히 들어 알고 있었으나 당장 급하지 않다고 생각했다.

그러던 어느날 github에서 나의 프로젝트와 비슷한 기능을 구현한 앱의 코드를 구경한 적이 있다. 

MVVM패턴을 사용한 코드였는데 너무 놀라웠다. 기능 구현에만 초점이 맞춰져 있는 코드와 다르게 SOLID 에 훨씬 더 가까웠고, 훨씬 깨끗했다. 그 코드가 멋져 보여서, 디자인 패턴을 공부를 더는 늦춰서는 안된다고 생각했다.

<br>

<h3 id="2">MVC, MVP, MVVM</h3>

- MVC (Model, View, Controller)

 : controller가 중심으로 view와 model은 데이터 변경 및 유저 액션 등의 event를 controller에게 알리고, controller는 view와 model을 갱신해 준다. 다만 Swift에서는 View와 Controller가 결합 되어있는 ViewController로 형태가 변형되어 유닛테스트를 진행할때 어려움을 준다고 한다. 

예를 들어 뉴스(Model), 신문(View), 신문사(Controller) 가 있다고 하자.

신문사는 뉴스와 신문을 가지고 있다가, 뉴스 (새로운 기삿거리)가 발생하면 신문사가 해당 뉴스 데이터를 갱신한다. 또,  신문에 대해 구독자(User)가 항의를 하는 등 어떤 액션을 취하면 신문사가 신문을 업데이트 해준다. 한마디로 Model과 View가 어떠한 이벤트가 발생했음을 알려주면(notify) Controller는 이를 인지하고 Model과 View를 갱신한다.

<br>

- MVP (Model, Passive View, Presenter)

 : MVC 와 비슷하게 Presenter가 중심으로 Passive View 와 Model을 갱신하는 구조이다. 이것만 보면 MVC 패턴과 비슷해보이지만 완전히 다른 방식으로 동작한다. MVP방식은 테스트에 용이하나, iOS에선 코드가 길어질 수 있다고 한다. 비교해서 이해해보자

첫째, Controller와 Presenter : 둘의 역할이 같이 보인다. 하지만 Presenter는 UIKit와 독립적인 관계(서로 관여를 하지 않는다.)이다. Controller는 View를 소유하고 직접적으로 갱신하지만 (Swift는 View와 Controller가 아예 결합되어 있음), Presenter는 갱신하라고 알려주기만 할뿐 UI를 직접 갱신하지 않는다. 그래서 이때, delegate (위임자)를 사용하여 view를 갱신한다. View == ViewController라고 생각하면 될 것 같다.

둘째, View & Model : MVC에서는 View를 갱신할때 Model에 직접적으로 접근한다. 즉 View가 model을 알고있다. 하지만 MVP의 View는 Model을 알지 못하고 Presenter가 Model 정보를 가지고 있다가 View에 뿌려주기만 한다.

셋째, View의 소유권 : MVC에서는 Controller가 View를 소유하고 제어한다. 하지만 MVP에서는 View가 Presenter를 소유하고 있다가 이벤트가 발생하면 Presenter에게 알려준다.

넷째, Passive View : MVP에서 View는 말 그대로 수동적인 View이다. View의 역할은 그저 Presenter에게 이벤트가 발생하면 알림을 보내고 보여주는 껍데기이다. 

<br>

- MVVM (Model, View, View Model)

 : 똑바로 해도 MVVM 꺼꾸로 해도 MVVM인 MVVM은 MVC 패턴을 변형한 것이다. View를 추상화 하여 재사용성을 높이고 테스트하기 쉽도록 구현하는 패턴이다. MVC와 마찬가지의 역할을 수행하는 Model과 View가 있다. 그리고 ViewModel 이란 것이 있다. MVVM의 핵심인 ViewModel을 구체적으로 보자면,

<b style="font-size: 13.5pt">View Model?</b>   View의 모형이라는 뜻의 view model은 추상화 된 뷰이다. 뷰모델은 추상화를 위해 ViewState(뷰 상태), ValueConverters(값 변환기), Commands(명령)를 가진다. 그리고 이렇게 추상화 된 View와 물리적인 View를 동기화 해줄 수단으로 DataBinding을 사용한다. 데이터바인딩은 ViewModel이 변경될 경우 View를 변경해주고, 반대로 View가 변경될 경우 ViewModel의 상태가 변경되는 동기화의 역할을 한다.  아래는 <a href="https://justhackem.wordpress.com/2017/03/05/mvvm-architectural-pattern/">이규원님의 블로그</a>에서 MVVM을 사용할 때 지켜야하는 행동강령(?)이라고 생각되는 부분을 스크랩하였다.

첫째, 추상화는 구체화에 대한 지식을 가지지 않기 때문에 뷰모델은 뷰에 대해 알지 못한다. (재사용성을 위해)

둘째,  MVVM 패턴의에핵심 가치는 "응용프로그램은 복잡한 컨트롤러 논리 없이 단순한 구조를 유지할 수 있다." 이다.

셋째, 모델(Model), 뷰모델(ViewModel), 뷰(View)를 물리적으로 구분된 프로젝트에 분산시켜라.

넷째, 프로젝트 순환 참조를 금지하라.

다섯째, 뷰모델 프로젝트가 모델 프로젝트를 참조하게 하라.

여섯째, 뷰 프로젝트가 뷰모델 프로젝트를 참조하게 하라.

일곱째, 뷰모델 프로젝트가 UI 프레임워크를 참조하지 않게 하라.

여덟째, [의존성 역전 원리(Dependency Inversion Principle)](https://justhackem.wordpress.com/2016/05/13/dependency-inversion-terms/#dip)를 필요한 곳에 필요한 만큼만 절제해 사용하라. 이 때 반드시 [인터페이스 분리 원리(Interface Segregation Principle)](https://en.wikipedia.org/wiki/Interface_segregation_principle)를 함께 고려하라

<br>

<h3>마무리</h3>

이번에 디자인 패턴을 공부를 해본것을 정리해 보았다.

예제 코드도 많이 보고, 관련 글도 많이 읽어보고, 나의 나름대로 예제코드를 작성해보았지만 아직은 윤곽만 대충 잡은 느낌이다. 디자인 패턴을 계기로 나의 부족함을 많이 느끼게 된 것 같다. 

개발을 할때, 나름 코드의 이유를 생각하고 짠다고 생각했었는데, 거대한 장벽에 맞닿은 느낌이 왔다. 이후엔 좀 더 디자인 패턴을 이해한 이후에 next로 데이터 바인딩을 공부할 생각이다. 

<br><br>

읽어보면 좋을 참고자료 : <a href="https://academy.realm.io/kr/posts/doios-natasha-murashev-protocol-oriented-mvvm/">[프로토콜 지향 MVVM을 소개합니다.] </a>

<br>

출처 및 참조

- <a href="https://justhackem.wordpress.com/2017/03/06/structuring-projects-for-mvvm-application/">[프로그래머 이규원의 블로그 : MVVM 응용프로그램을 위한 프로젝트 구조화] </a>
- <a href=" https://justhackem.wordpress.com/2017/03/05/mvvm-architectural-pattern/"> [프로그래머 이규원의 블로그 : MVVM 아키텍처 패턴] </a>

<br>
