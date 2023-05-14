# week5. WIL

# 1. **객체지향 패러다임과 객체란?**

**객체 지향 패러다임** : 소프트웨어 개발 방법론 중 하나로, 현실 세계의 개념과 개체들을 소프트웨어로 모델링하는 접근 방식이다. 이 패러다임은 문제를 해결하기 위해 독립적인 개체(객체)들을 정의하고, 이들 개체들 간의 상호작용을 통해 시스템을 구성하는 것에 초점을 둔다.

**객체** : 데이터와 그 데이터를 처리하는 메서드(함수)를 포함하는 소프트웨어 개체를 의미합니다. 개체는 현실 세계의 개념이나 사물을 모델링하여 만들어진 것으로, 각각의 개체는 고유한 상태를 가지며 다른 개체와 상호작용할 수 있다.

객체 지향 패러다임에서 중요한 개념들로는 클래스, 객체, 상속, 다형성, 캡슐화가 있다.

# 2. **객체 지향의 4 대 특성**

- **캡슐화** : 객체의 상태(데이터)와 행위(메서드)를 하나로 묶고, 외부에서 직접 접근하지 못하도록 정보 은닉을 통해 객체의 내부를 보호하는 것을 의미한다. 캡슐화를 통해 객체는 자신의 내부 동작 방식을 숨기고, 외부에는 필요한 기능만을 제공함으로써 코드의 가독성과 유지보수성을 향상시킬 수 있다.
- **상속** : 클래스 간에 관련성과 계층 구조 형성을 위해 사용되는 개념으로, 하위클래스는 상위 클래스의 속성과 메서드를 상속받아 사용할 수 있다. 재사용성과 유연성을 높이기 위해 사용한다.
- **다형성** : 같은 이름의 메서드를 다양한 방식으로 동작하도록 하는 개념으로, 오버로딩 / 오버라이딩을 통해 구현된다. 서로 다른 클래스의 객체들이 동일한 메서드를 호출할 때 각자의 클래스 내에서 정의된 대로 메서드를 동작시킬 수 있으며, 코드의 가독성과 유지보수를 위해 사용한다.
- **추상화** : 복잡한 시스템을 단순화(은닉)하고 핵심적인 개념과 기능만을 표현하는 것. Java에서는 클래스와 인터페이스를 통해 추상화를 구현할 수 있다. 추상화를 통해 개념적인 모델링을 하고, 필요한 세부 사항을 무시함으로써 코드의 가독성과 이해도를 높이고, 유지보수를 용이하게 할 수 있다.

# 3. **오버라이딩 vs 오버로딩**

- **오버로딩** : 같은 이름의 메서드를 여러 개 정의하는 것. 메서드의 이름은 동일하지만 매개변수의 타입, 개수, 순서가 다르게 정의된다. 즉, 동일한 기능을 수행하지만 다양한 매개변수를 처리할 수 있는 여러 버전의 메서드를 제공할 수 있어, 호출 시 인자에 따라 적절한 오버로딩된 메서드가 선택되어 실행됩니다.
- **오버라이딩** : 상속 관계에서 상위 클래스의 메서드를 하위 클래스에서 재정의하는 것. 즉, 상속 받은 메서드를 하위 클래스에서 동일한 이름으로 재정의하여 하위 클래스에 맞게 동작하도록 할 수 있다. 이를 통해 하위 클래스는 상위 클래스의 메서드를 재정의하거나 확장하여 자신의 동작을 구현할 수 있다.

# 4. **call by value? call by reference?**

- **call by value** : 함수 호출 시 인자로 전달되는 값의 복사본이 함수에 전달되는 방식. 함수 내에서 매개변수의 값이 변경되어도 호출한 측에는 영향을 주지 않아, 호출된 함수 내에서 매개변수의 값이 변경되어도 원본 변수에는 영향을 주지 않는다.
- **call by reference** : 함수 호출 시 인자로 전달되는 변수의 참조(주소)가 함수에 전달되는 방식. 함수 내에서 매개변수의 값이 변경되면 호출한 측의 변수에도 영향을 주기 때문에 호출된 함수 내에서 매개변수의 값이 변경되면 원본 변수도 변경된다.