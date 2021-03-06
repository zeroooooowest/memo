# Outside-In TDD

## 정의

- 일반적인 TDD를 Inside-Out(bottom up, chicago school, classic approach)이라고 함
- Outside-In는 Top Down으로 진행하고 London School TDD라고도 부르고, Mockist Approach라고도 함
- Outside-In TDD는 최상위 레이어(view)에서 시작해서 최하위 레이어(DB)로 향해 가면서 TDD를 진행한다.
- 프로그래밍을 할 때 종종 어디서 시작해야 할 지 모르는 경우가 있다.
  - TDD를 한다면 시작이 되는 단위 테스트 작성이 좋은 시작점이됨
  - 하지만 아무 것도 없는 상태라면 어떤 단위 테스트를 작성할지 결정하는 것은 쉽지 않음
  - 이럴 경우 Inside-Out TDD보다는 Outside-In TDD가 효과적임
- Outside-In TDD는 Inside-Out TDD의 절차를 따르지만 한가지 예외가 있음
  > “테스트가 실패할 때 테스트를 성공시키기 위해 반드시 프로덕션 코드를 작성해야만 하는 것은 아니다. 다른 하위 레벨에서 새로운 기능을 구현해야 할 수도 있다”
- 테스트가 하위 레벨 구현을 가이드하면 하위 레벨 구현을 위한 새로운 Inside-Out TDD의 Red-Green-Blue 사이클을 시작
- 이 하위 레벨에서 TDD 싸이클을 아래와 같은 상황이 발생할 때까지 지속
  - 원하는 행위를 구현하기 위해 또 다른 하위 레이어로 이동하여 Inside-Out TDD 수행
  - 현재 레이어에서의 작업이 완료
- 인수테스트(Acceptance Test) 작성으로 시작하고 Acceptance Test가 실패하면 이를 성공시키기 위해 Inside-Out TDD를 수행함
- 아래 그림은 Outside-In TDD 방식으로 어플리케이션을 개발하는 과정을 보여준다. 이 그림은 [Growing Object-Oriented Software Guided by Tests](https://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627)에서 발췌했음
![](https://camo.githubusercontent.com/c9bebf338115b153147422c8044dbea07a7cc1fd/68747470733a2f2f6170692e6d6f6e6f736e61702e636f6d2f7270632f66696c652f646f776e6c6f61643f69643d5a74646a5a6f506d43513045496b5977566e3577534a6e6f686d45394d63)

## Outside-In TDD의 절차
1. 어떤 기능(Feature)의 인수테스트로 시작
2. 인수테스트를 실행해서 합당한 이유로 실패하면 어떤 클래스 A가 제공하는 기능이 필요해짐
  - 이런 경우 인수테스트를 잠시 중지하고, Figure 5.1이 내부 루프가 시작되어 Unit Test가 시작됨
  - 이게 TDD의 **Double Loop**(Outer Loop: Acceptance Test, Inner Loop: Unit Test)
3. 클래스 A의 기능에 대한 단위테스트(Unit Test) 작성을 통한 Inside-Out TDD 실행
4. 단위테스트가 성공하면 인수테스트를 실행시켜서 다음 오류 해소를 위해 또 다른 어떤 클래스 B의 어떤 기능 F가 필요한지 파악
5. 클래스 B의 기능 F 구현을 위해 3부터 반복

## 인수테스트의 역할
1. 새로운 기능 구현을 할때 어디서 부터 시작할 지 알게 해줌
2. 지금 테스트(인수, 단위)가 성공하면 다음에 무엇을 할지 알려줌
3. 어떤 기능의 구현이 완료되었는지 확인
	- 전체 기능들에 대한 인수테스트 집합을 만들었다면 전체 프로젝트에 대한 공정율도 알 수 있다.


## 두 방식의 차이점

| | Inside-Out | Outside-In |
|:---|:---|:---|
| 정의 & 방식 | 개발자가 한번에 하나에만 집중할 수 있도록 해줌 | 일부 엔터티들이 초기엔 하드코딩될 수도 있지만 처음부터 시스템을 정의할 수 있는 경로를 제공  |
| | 한번에 하나의 엔터티에 집중하는 것은 팀원들이 병렬로 개발할 수 있도록 해줌  | 상위 레벨의 인수테스트(High Level Acceptance Test)를 사용하므로 엔터티들의 내부 구현보다는 엔터티들의 상호작용에 집중하게 됨 |
| | 테스트 더블이 필요치 않음. 엔터티들이 사전에 식별되므로 실제 구현체를 사용할 수 있음  | 상호작용이 필요한 엔터티들이 발견될 때 마다 테스트 더블로 치환되어 해당 엔터티들에 대한 상세가 미뤄질 수 있음  |
| |  | 개발자들은 새로운 단위 테스트를 통해서 테스트 더블에 대한 구현물을 제공할 때까지 루프를 수행  |
| 이슈 | 개별 엔터티들은 통합되어 함께 동작할 때까지는 가치가 없을 수 있음  | 설계의 상세 구현이 테스트에 존재. 설계 변경은 대개 테스트의 변경을 수반함. 이런 상황이 위험을 추가하거나 구현에 자신감을 줄 수 있음 |
| | 개발 후반부에 연동을 하는 것은 리스크가 클 수 있음 | 개발자가 사전에 테스트 더블을 활용하여 어떻게 상호작용을 테스트할지 알아야 함  |
| 초기 | 초기에는 시스템 설계에 대한 완전한 이해는 필요치 않음. 시작할 때는 한개의 엔터티만 식별되면 됨  | 초기부터 시스템 전반의 완전한 흐름에 집중함으로써 시스템의 서로 다른 부분들이 어떻게 상호작용하는지에 대한 지식이 요구됨  |
| 적합성 | 기능의 수행 후 상태 값 검증을 통한 정합성 검증에 유리한 알고리즘 구현에 적합  | 협력 객체들의 상호작용 검증(인자, 호출 여부 등)을 통한 정합성 검증이 유리한 비즈니스 어플리케이션에 적합(탑다운으로 점진적으로 협력 객체의 인터페이스를 찾아나가는 방법: [Mock Roles, not Objects](http://jmock.org/oopsla2004.pdf) - Iterative Interface Discovery) |
| | 개발자들이 프로그래밍 언어 초보인 경우 좋은 시작점임. 개발자는 한번에 하나의 엔터티에만 집중하면 됨. 개발을 진행해 나가면서 언어, 테스트 프레임워크 등에 대한 지식이 축적됨 | 개발자들은 전체 시스템을 만들면서 시작하고 리팩토링 기회가 발생할 때 작은 컴포넌트들로 분해한다. 과정은 보다 탐색적일 수 있고, 목표에 대한 일반적인 아이디어가 있지만 구현의 세부사항이 명확치 않을 때 이상적임 |
| | 기 구축된 시스템에 추가 기능을 구현하는 경우, 꽤 상세한 설계가 있는 경우 Inside-Out | 신규 기능 셋 추가, 무엇부터 시작해야 할지 모르는 경우 Outside-In |


# 참고 자료
- [Outside-In TDD Part I](https://www.youtube.com/watch?v=XHnuMjah6ps&t=9s)
- [Outside-In Test-Driven Development](https://www.codecademy.com/articles/tdd-outside-in)
- [TDD - From the Inside Out or the Outside In?](https://8thlight.com/blog/georgina-mcfadyen/2016/06/27/inside-out-tdd-vs-outside-in.html)