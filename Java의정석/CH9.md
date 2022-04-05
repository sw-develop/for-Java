# ✔️ java.lang 패키지
## ▶️ Object 클래스
### #️⃣ clone()
- 자신을 복제하여 새로운 인스턴스를 생성
- Object 클래스에 정의된 clone()은 단순히 인스턴스변수의 값만 복사하기 때문에 참조 타입의 인스턴스변수가 있는 클래스는 완전한 인스턴스 복제가 이뤄지지 않는다.
    - 배열의 경우, 복제된 인스턴스도 같은 배열의 주소를 갖기 때문에 복제된 인스턴스의 작업이 원래의 인스턴스에 영향을 미친다.
    
```java
class Point implements Cloneable {
  
  ...
  
  public Object clone() {
    Object obj = null;
    try {
      obj = super.clone();
    } catch (CloneNotSupportedException e) {}
    return obj;
  }
}
```
- clone() 사용을 위한 조건
    - 복제할 클래스가 Cloneable 인터페이스를 구현해야 한다.
    - clone()을 오버라이딩하여 접근제어자를 protected에서 public으로 변경해야 한다.
        
### #️⃣ 공변 반환타입
- JDK1.5부터 '공변 반환타입(covariant return type)'이 추가되어, 오버라이딩할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용한다.
```java
class Point implements Cloneable {
  
  ...
  
  public Point clone() {
    Object obj = null;
    try {
      obj = super.clone();
    } catch (CloneNotSupportedException e) {}
    return (Point) obj;
  }
}
```
- 조상의 타입이 아닌, 실제로 반환되는 자손 객체의 타입으로 반환할 수 있어서 번거로운 형변환이 줄어든다.

### #️⃣ 얕은 복사와 깊은 복사
- 얕은 복사
    - clone()은 단순히 객체에 저장된 값을 그대로 복제할 뿐, 객체가 참조하고 있는 객체까지는 복제하지 않는다.
    - 원본을 변경하면 복사본도 영향을 받는다.
    
- 깊은 복사
    - 원본이 참조하고 있는 객체까지 복제한다.
    - 원본과 복사본이 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.

## ▶️ String 클래스
### #️⃣ StringBuffer클래스와 StringBuilder 클래스
- String클래스는 인스턴스 생성 시 지정된 문자열을 변경할 없지만, StringBuffer클래스는 가능하다.
- 내부적으로 문자열 편집을 위한 buffer를 가지고 있으며, StringBuffer인스턴스를 생성할 때 그 크기를 지정할 수 있다.
- StringBuffer는 멀티쓰레드에 안전(thread safe)하도록 동기화되어 있다.
    - 멀티쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능을 떨어뜨린다.

- StringBuffer에서 쓰레드의 동기화만 뺀 것이 StringBuilder이다.
    - 다만, StringBuffer도 충분히 성능이 좋기 때문에 성능향상이 반드시 필요한 경우를 제외하고는 굳이 변경할 필요는 없다.

