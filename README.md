# Monologue

제가 IT공부를 하면서 가장 좋아하는 취미 중 하나인 블로그 관리입니다.
배운 내용들을 정리하여 올리고 있습니다.

## Preview my Post of Java

## cloneable

  자바의 복제방법에는 얕은 복제(Shallow copying), 깊은 복제(Deep copying)가 있습니다.

  자바의 Cloneable 인터페이스는 인스턴스가 복제 가능하도록 하기 위해서 구현해야 하는 인터페이스입니다.
  최상위 클래스인 Object 클래스는 객체의 클로닝을 위한 `Object.clone()` 메소드를 제공하고 있습니다.
  clone()을 사용해 객체 복사를 하여 객체 생성을 할 경우 객체의 생성자와 메소드를 실행하지 않고 객체 상태값들을 그대로 복사하게 됩니다. __원본 객체에는 아무런 영향이 없이 복제가 가능합니다.__ .

  > protected Object clone() throws CloneNotSupportedException

  clone()은 Cloneable인터페이스의 추상 메소드이며 Cloneable 인터페이스를 구현한 클래스만
  clone()메소드를 `public으로 오버라이딩` 하여 사용할 수 있습니다.

  ```java
  // Cloneable 인터페이스를 구현, clone()메소드를 오버라이딩
  // 객체가 힙영역에 가지고 있는 값이 복사되지만 main에서 형변환을 해줘야한다
  @Override
  public Object clone() throws CloneNotSupportedException {
    return super.clone();
  }
  ```

  clone()메소드가 지원되지 않을 수도 있기 때문에 CloneNotSupportedException 예외처리가 필요합니다.
  clone()은 `깊은 복제(Deep copy)`를 이용한 방법으로서 주소값이아닌 힙영역에 저장된 객체의 값을 복사하여
  각자에게 영향을끼치지 않습니다.
