# 구조체(Struct)와 클래스(Class)
예전부터 계속해서 나오는 질문이고 면접 질문 중에서도 가장 간단한 질문이라고 할 수 있다.
하지만 매번 답변을 할때마다 `어...구조체는 값타입이고 클래스는 참조타입입니다.` 라는 답변밖에 할 수 없었다.
면접관 분께서 다른 차이점은 없나요? 라고 질문해주셨는데 더이상 답하지 못했다. 두가지의 차이점에 대한 추가적인 점과 키워드들이 머릿속에 담겨있지 않은 느낌이였다. 그래서 이번기회에 확실하게 알아보려고한다.

## 클래스와 구조체의 공통점
먼저 두가지의 차이점을 알아보기 전에 공통점에 대해서 먼저 생각해보자. 공통점은 다음과 같이 쉽게 정리할 수 있다.
- 값을 저장할 프로퍼티를 선언할 수 있다.
- 동작을 정의하는 메서드를 선언할 수 잇다.
- 내부의 값에 `.`을 이용하여 접근할 수 있다.
- 익스텐션(extension)을 사용해서 기능을 추가할 수 있다.
- 프로토콜(protocol)을 정의하여 기능을 재정의할 수 있다.

## 차이점
### 클래스
- 참조타입이다.
- ARC로 메모리를 관리한다.
- 같은 클래스 인스턴스를 여러개의 변수에 할당한 뒤 값을 변경시키면 할당한 모든 변수에 영향을 줍니다.
- 상속이 가능하다.
- 타입 캐스팅을 통해 **런타임에 클래스 인스턴스의 타입을 확인할 수 있다.**
- deinit을 사용해서 클래스 인스턴스의 메모리 할당을 해제할 수 있다.
### 구조체
- 값타입이다.
- 구조체 변수를 새로할당할 때마다 새로운 구조체가 할당된다.
- 같은 구조체를 여러개의 변수에 할당 한 뒤 값을 변경시키더라도 다른 변수에 영향을 주지 않는다.

## 스택과 힙
스택에 저장되는 데이터들은 컴파일단계에서 언제 생성되고 해제되는지 알 수 있는 데이터들이 저장됩니다. 스레드들이 각각 독립적인 스택공간을 가지고 있기 때문에 상호배제를 위한 자원이 필요하지 않습니다. 즉 스레드로부터 안전하다는 말이 됩니다. 이러한 특징 때문에 스택에 있는 것을 사용하는 것이 힙을 사용하는 것보다 빠르다고 할 수 있습니다.<br>

힙에는 컴파일 단계에서 생성과 해제를 알 수 없는 참조타입의 객체가 할당됩니다. 메모리의 할당과 해제가 하나의 명령어로 처리되지 않기 때문에 스택보다 관리하기 어렵습니다. 스택에서는 push, pop명령어 만으로 할당과 해제가 이루어졌지만 힙은 참조에 대한 계산도 해주어야하므로 스택보다 복잡한 것입니다.

그리고 힙은 스레드끼리 공유하는 메모리 공간이기 때문에 스레드로부터 안전하지 않습니다. 이를 관리해주기 위한 lock과 같은 자원이 필요해지기 때문에 이러한 점들이 곧 오버헤드로 이어지게 되는 것입니다.

### 그럼 클래스는 무조건 힙에, 구조체는 무조건 스택에 저장되나요?
하지만 코딩을 하다보면 클래스안에 구조체 프로퍼티가 있고, 구조체 안에 클래스 프로퍼티가 있을 수 있습니다. 두가지 경우로 나누면 다음과 같습니다.
- 값 타입을 포함하는 참조 타입
    - 간단하게 말하면 클래스안에 구조체 프로퍼티가 있는 것입니다.
    - 이 경우 참조 타입이 할당해제 되기 전에 값타입도 할당해제되지 않도록 하기 위해서 값 타입도 힙에 저장합니다.
    - 클래스 말고도 클로저 내부에서 사용하는 값 타입도 위의 경우에 해당합니다.
- 참조 타입을 포함하는 값 타입
    - 구조체 안에 클래스 프로퍼티가 존재하는 경우를 말합니다.
    - 이 경우에는 값타입, 구조체가 힙에 할당되지는 않지만 내부에 참조타입이 있기 때문에 참조 카운트를 처리해주어야 합니다.

## 값 타입의 Copy-on-assignment, Copy-on-write
값 타입을 다른 변수에 할당하면 복사를 하게 됩니다. 즉 새로운 메모리 공간에 같은 값을 복사하게 되는데 이것을 `copy-on-assignment`라고 합니다. 

`copy-on-write`는 다른 변수에 할당하면 일단은 메모리를 할당하지 않고 같은 곳을 보고있다가, 해당 값을 변경할 때 실제로 메모리에 값을 복사한 후 값을 변경하게 됩니다. 메모리를 최적화해주기 위한 기법이고 스위프트에서는 기본적으로 `Int`, `Double`과 같은 기본적인 자료형과 `Array`, `Set`, `Dictionary`같은 Collection 에서 사용되고 있습니다. 참조타입을 포함하고 있는 값타입은 이러한 메모리 최적화를 할 수 없습니다.

결론은,
- 단순한 데이터를 보관하기 위해서는 구조체를 사용하는 것이 낫습니다.
- 메모리의 스택은 크기가 크지 않기 때문에 작은 값을 갖는 데이터를 처리할 때 구조체를 사용합니다.
- Objective-C의 상호운용성을 원할 때 클래스를 사용합니다.

확실히 면접에서 스위프트에서 사용할 때의 차이점뿐만 아니라 위에서 말한 스택과 힙에 대한 CS적인 내용들도 함께 말한다면 메리트가 있을 것 같습니다.