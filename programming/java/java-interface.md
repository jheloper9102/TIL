# 자바 인터페이스

자바의 인터페이스는 객체의 사용 방법을 정의한 타입. 인터페이스는 객체의 교환성을 높여주기 때문에 다형성을 구현하는 매우 중요한 역할을 함. 특히 자바 8의 람다식은 함수적 인터페이스의 구현 객체를 생성하기 때문에 인터페이스의 중요성이 더욱 커짐.

인터페이스는 개발 코드와 객체가 서로 통신하는 접점 역할을 함. 개발 코드가 인터페이스의 메서드를 호출하면 인터페이스는 객체의 메서드를 호출시킴. 그렇기 때문에 개발 코드는 객체의 내부 구조를 알 필요가 없고 인터페이스의 메서드만 알고 있으면 됨.

직접 객체의 메서드를 호출하지 않고 중간에 인터페이스를 두는 이유는 개발 코드를 수정하지 않고, 사용하는 객체를 변경할 수 있도록 하기 위해서임. 인터페이스는 하나의 객체가 아니라 여러 객체들과 사용이 가능하므로 어떤 객체를 사용하느냐에 따라서 실행 내용과 리턴 값이 다를 수 있음. 따라서 개발 코드 측면에서는 코드 변경 없이 실행 내용과 리턴 값을 다양화할 수 있음.



## 인터페이스 선언

```java
[public] interface 인터페이스명 { ... }
```

클래스는 필드, 생성자, 메서드를 구성 멤버로 가지지만 인터페이스는 상수와 메서드만을 구성 멤버로 가짐. 인터페이스는 객체로 생성할 수 없기 때문에 생성자를 가질 수 없음. 자바 7 이전까지는 인터페이스의 메서드는 실행 블럭이 없는 추상 메서드로만 선언이 가능했으나 자바 8부터는 디폴트 메서드와 정적 메서드도 선언 가능함.

```java
interface 인터페이스명 {
    // 상수
    int A = 10;
    // 추상 메서드
    int absMethod(int a, int b);
    // 디폴트 메서드
    default int defMethod(int a, int b) { ... }
    // 정적 메서드
    static int staMethod(int a) { ... }
}
```

### 상수 필드

인터페이스에 선언된 필드는 모두 public static final의 특성을 가지며 생략하더라도 자동으로 컴파일 과정에서 붙게 됨.

### 추상 메서드

인터페이스에 선언된 추상 메서드는 모두 public abstract의 특성을 갖기 때문에 생략하더라도 자동적으로 컴파일 과정에서 붙게 됨.

### 디폴트 메서드

디폴트 메서드는 인터페이스에 선언되지만 구현 객체가 가지고 있는 인스턴스 메서드라고 생갹해야 함. 자바 8에서 디폴트 메서드를 허용한 이유는 기존 인터페이스를 확장해서 새로운 기능을 추가하기 위해서임.

디폴트 메서드는 public 특성을 갖기 때문에 생략하더라도 자동적으로 컴파일 과정에서 붙게 됨.

### 정적 메서드

인터페이스의 정적 메서드는 디폴트 메서드와는 달리 객체가 없어도 인터페이스만으로 호출이 가능함.

정적 메서드는 public 특성을 갖기 때문에 생략하더라도 자동적으로 컴파일 과정에서 붙게 됨.



## 인터페이스 구현

객체는 인터페이스에 정의된 추상 메서드와 동일한 시그너처를 가진 실체 메서드를 가지고 있어야 함. 이러한 객체를 인터페이스의 구현 객체라고 하고, 구현 객체를 생성하는 클래스를 구현 클래스라고 함.

### 구현 클래스

구현 클래스는 보통의 클래스와 동일하나, 인터페이스 타입으로 사용할 수 있음을 알려주기 위해 클래스 선언부에 `implements` 키워드를 추가하고 인터페이스명을 명시해야 함.

```java
public class 구현클래스명 implements 인터페이스명 { 
    // 인터페이스에 선언된 추상 메서드의 실체 메서드 선언.
}
```

구현 클래스에서 인터페이스의 추상 메서드들에 대한 실체 메서드 작성 시 주의할 점은 인터페이스의 모든 메서드는 기본적으로 public 접근 제한을 갖기 때문에 public보다 더 낮은 접근 제한으로 작성할 수 없다는 점. 만약 인터페이스에 선언된 추상 메서드에 대응하는 실체 메서드를 구현 클래스가 작성하지 않으면 구현 클래스는 자동적으로 추상 클래스가 되기 때문에 클래스 선언부에 `abstract` 키워드를 추가해야 함.

### 익명 구현 객체

구현 클래스를 만들어 사용하는 것이 일반적이며 클래스를 재사용할 수 있기에 편리하지만, 일회성의 구현 객체를 위해 클래스를 선언하는 것은 비효율적. 자바는 소스 파일을 만들지 않고 클래스를 선언하지 않아도 구현 객체를 만들 수 있는 방법을 제공하는데, 그것이 **익명 구현 객체**.

익명 구현 객체 작성 시 주의할 점은 하나의 실행문이므로 끝에 세미콜론을 반드시 붙여야 한다는 점.

```java
인터페이스 변수 = new 인터페이스() {
    // 인터페이스에 선언된 추상 메서드의 실체 메서드 선언
};
```

인터페이스에 선언된 모든 추상 메서드들의 실체 메서드를 작성해야 하며, 추가적으로 필드와 메서드를 선언할 수 있지만 익명 객체 안에서만 사용할 수 있고 인터페이스 변수로 접근할 수 없음.

### 다중 인터페이스 구현 클래스

객체는 다수의 인터페이스 타입으로 사용할 수 있음. 따라서 클래스는 여러 인터페이스를 구현할 수 있으며, 구현 클래스는 모든 인터페이스의 추상 메서드에 대해 실체 메서드를 작성해야 함. 만약 하나라도 추상 메서드에 대한 실체 메서드를 작성하지 않으면 추상 클래스로 선언해야 함.



## 인터페이스 사용

인터페이스로 구현 객체를 사용하려면 인터페이스 변수를 선언하고 구현 객체를 대입해야 함.

### 추상 메서드 사용

구현 객체가 인터페이스 타입에 대입되면 인터페이스에 선언된 추상 메서드를 호출할 수 있음. 인터페이스의 추상 메서드를 선언하면 대입된 구현 객체의 실체 메서드가 호출됨.

### 디폴트 메서드 사용

디폴트 메서드는 인터페이스에 선언되나 인터페이스에서 바로 사용할 수 없음. 디폴트 메서드는 추상 메서드가 아닌 인스턴스 메서드이므로 구현 객체가 있어야 사용할 수 있음.

디폴트 메서드는 인터페이스의 모든 구현 객체가 가지고 있는 기본 메서드라고 생각하면 됨. 그러나 어떤 구현 객체는 디폴트 메서드의 내용이 맞지 않아 수정이 필요할 수도 있는데, 구현 클래스 작성 시 디폴트 메서드를 재정의해서 자신에게 맞게 수정할 수 있음.

### 정적 메서드 사용

인터페이스의 정적 메서드는 인터페이스에서 바로 호출이 가능함.



## 타입 변환과 다형성

프로그램 개발 시 인터페이스를 사용해서 메서드를 호출하도록 코딩한다면, 구현 객체를 교체하는 것은 매우 손쉽고 빠르게 할 수 있음. 소스 코드는 변함이 없지만 구현 객체를 교체함으로써 프로그램의 실행 결과가 다양해짐. 이것이 인터페이스의 다형성.

### 자동 타입 변환

구현 객체가 인터페이스 타입으로 변환되는 것은 자동 타입 변환에 해당됨.

### 강제 타입 변환

구현 객체가 인터페이스 타입으로 자동 변환하면 인터페이스에 선언된 메서드만 사용 가능하다는 제약 사항이 따름. 하지만 경우에 따라서 구현 클래스에 선언된 필드와 메서드를 사용해야 할 경우도 발생하는데, 이때 강제 타입 변환을 해서 다시 구현 클래스 타입으로 변환 후 구현 클래스의 필드와 메서드를 사용할 수 있음.

### 객체 타입 확인(instanceof)

구현 객체가 어떤 인터페이스 타입으로 변환되었는지 확인할 때, 상속에서 `instanceof` 연산자를 사용한 것처럼 인터페이스에서도 사용할 수 있음.



## 인터페이스 상속

인터페이스도 다른 인터페이스를 상속할 수 있음. 인터페이스는 클래스와 달리 다중 상속을 허용함. `extends` 키워드 뒤에 상속할 인터페이스들을 나열하면 됨.

```java
public interface 하위인터페이스 extends 상위인터페이스1, 상위인터페이스2 { ... }
```

하위 인터페이스를 구현하는 클래스는 하위 인터페이스의 메서드뿐 아니라 상위 인터페이스의 모든 추상 메서드에 대한 실체 메서드를 가지고 있어야 함. 그렇기 때문에 구현 클래스로부터 객체를 생성하고 나서 하위 및 상위 인터페이스 타입으로 변환이 가능함.



## 디폴트 메서드와 인터페이스 확장

### 디폴트 메서드의 필요성

인터페이스에서 디폴트 메서드를 허용한 이유는 기존 인터페이스를 확장해서 새로운 기능을 추가하기 위해서임. 기존 인터페이스의 이름과 추상 메서드의 변경 없이 디폴트 메서드만 추가할 수 있기 때문에 이전에 개발한 구현 클래스를 그대로 사용하면서 새롭게 개발하는 클래스는 디폴트 메서드를 활용할 수 있음.

### 디폴트 메서드가 있는 인터페이스 상속

부모 인터페이스에 디폴트 메서드가 있는 경우 자식 인터페이스는 다음과 같은 방법으로 활용할 수 있음.

- 디폴트 메서드를 단순히 상속만 받음.
- 디폴트 메서드를 재정의해서 실행 내용을 변경함.
- 디폴트 메서드를 추상 메서드로 재선언함.