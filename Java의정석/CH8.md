# ✔️ 예외처리 (exception handling)
## ▶️ 프로그램 오류
- 컴파일 에러: 컴파일 시에 발생하는 에러
- 런타임 에러: 실행 시에 발생하는 에러
    - 에러(error) - 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
        - ex. 메모리부족, 스택오버플로우
    - 예외(exception) - 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
- 논리적 에러: 실행은 되지만, 의도와 다르게 동작하는 것



## ▶️ 예외 클래스의 계층구조
- 자바에서는 런타임 오류(에러 & 예외)를 클래스로 정의하였다.
  
![image](https://user-images.githubusercontent.com/69254943/160418756-fdcf03c9-aeb7-488b-8b2c-03d1cfe68909.png)
- 모든 클래스의 최고 조상은 Object 클래스이므로, Exception과 Error 클래스는 Object의 자손들이다.

<img width="522" alt="image" src="https://user-images.githubusercontent.com/69254943/160420807-8ea35147-3e52-4631-97d2-c54e9eafbee7.png">   

- 예외(Exception) 클래스들은 크게 2가지 그룹으로 나눠질 수 있다.
- A: Exception클래스와 그 자손들
    - 프로그램 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외들
- B: RuntimeException클래스와 그 자손들
    - 주로 프로그래머의 실수에 의해 발생될 수 있는 예외들



## ▶️ 예외처리하기 - try-catch문
- 예외처리(exception handling)
    - 정의: 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것
    - 목적: 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

### #️⃣ 예외의 발생과 catch 블록
- 예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.

### #️⃣ printStackTrace()와 getMessage()
- 해당 메서드들은 java.lang.Throwable 클래스에 정의되어 있다.
- printStackTrace()
    - 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메세지를 화면에 출력한다.
    
- getMessage()
    - 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.



## ▶️ 예외 발생시키기
```java
//1. 연산자 new를 이용해 발생시키려는 예외 클래스의 객체 생성
Exception e = new Exception("Exception 발생"); 
//2. 키워드 throw를 이용해 예외 발생시킴
throw e;

//한 줄로 표현
throw new Exception();
```

### #️⃣ Checked vs. Unchecked Exception
- Checked Exception 
  - Exception클래스들(Exception클래스와 그 자손들)이 발생할 가능성이 있는 문장들에 대해 예외처리를 해주지 않으면 컴파일 에러가 발생한다.
  
- Unchecked Exception
  - 예외처리를 하지 않아도 컴파일 에러가 발생하지 않는다.
  - RuntimeException클래스와 그 자손들에 해당하는 예외는 프로그래머에 의해 실수로 발생하는 것들이기 때문에 예외처리를 강제하지 않는다.
  



## ▶️ 메서드에 예외 선언하기
```java
void method() throws Exception1, Exception2, Exception3 {
  ...
}
```
- 메서드에 예외를 선언하려면, 메서드의 선언부에 키워드 throws를 사용해 메서드 내에서 발생할 수 있는 예외를 적어준다.
- 일반적으로 RuntimeException클래스들(unchecked exception)은 적지 않고, 반드시 처리해주어야 하는 예외들(checked exception)만 선언한다.
- 예외를 메서드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라, 자신을 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것이다.
  - 어느 한 곳에서는 반드시 try-catch문으로 예외처리를 해주어야 한다.
  
### #️⃣ 메서드 내 예외처리 방식
- 방식1. 예외가 발생한 메서드 내에서 try-catch문으로 처리
  - 예외가 발생한 메서드를 호출한 메서드에서는 예외가 발생했다는 사실조차 모른다.
 
- 방식2. throws를 사용해 예외를 전달한 뒤 호출한 메서드 내에서 try-catch문으로 처리
  - 호출한 메서드로 예외를 넘겨주어 해당 메서드를 호출하는 라인에서 예외가 발생한 것으로 간주하고 이에 대한 처리를 수행한다.
  - 넘겨받아야 할 값이 있는 경우, 호출한 메서드 내에서 처리가 되도록 한다.



## ▶️ finally 블럭
```java
try {
  //에외 발생 가능성이 있는 문장들
} catch (Exception e) {
  //예외처리를 위한 문장들
} finally {
  //예외 발생여부에 관계없이 항상 수행되어야하는 문장들
  //try-catch문의 맨 마지막에 위치함      
}
```
- try블럭이나 catch블럭에서 return문이 실행되는 경우에도 finally 블럭의 문장들이 먼저 실행된 후 return문이 실행된다.



## ▶️ 자동 자원 반환 - try-with-resources문
- JDK1.7부터 try-with-resources문이라는 try-catch문의 변형이 새로 추가되었다.
- 주로 입출력(I/O)과 관련된 클래스를 사용할 때 유용하다.
  - 입출력에 사용되는 클래스 중에는 사용한 후 꼭 닫아줘야 사용했던 자원이 반환되기 때문이다.
  

### #️⃣ 기존 try-catch문을 사용해 자원 반환
```java
try {
  fis = new FileInputStream("test.txt");  
} catch (IOException ie) {
  ie.printStackTrace();
} finally {
  try { 
    if(fis != null)
      fis.close();
  } catch (IOException ie) {
    ie.printStackTrace();  
  }   
}
```
- close()가 예외를 발생시킬 수 있기 때문에 try-catch문으로 예외처리를 추가로 해줘야 한다.
- finally블럭에서 예외가 발생하면, 앞선 try블럭에서 발생한 예외를 무시된다.


### #️⃣ try-with-resources문을 사용해 자원 반환
```java
try (FileInputStream fis = new FileInputStream("test.txt");) { 
  ...
} catch (EOFException e) {  
  ...  
} catch (IOException ie) {
  ie.printStackTrace();  
}
```
- try-with-resources문의 ()안에 객체를 생성하는 문장을 넣으면, 이 객체는 따로 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동적으로 close()가 호출된다.
- 위와같이 자동으로 객체의 close()가 호출될 수 있으려면, 해당 클래스가 AutoCloseable이라는 인터페이스를 구현한 것이어야 한다.
  ```java
  public interface AutoCloseable {
    void close() throws Exception;
  }
  ```
- close()에서 예외가 발생하더라도, 앞서 발생한 예외에 대한 내용이 먼저 출력되고, close()에서 발생한 예외는 'suppressed'라는 의미의 머리말과 함께 출력된다.



## ▶️ 사용자정의 예외 만들기
- 필요에 따라 예외처리의 여부를 선택할 수 있는 unchecked 예외인 RuntimeException을 상속받아 작성하는 쪽으로 선호된다.
- checked 예외는 반드시 예외처리를 해주어야 하기 때문에 try-catch문을 넣어 코드가 복잡해질 수 있다.



## ▶️ 연결된 예외(chained exception)
한 예외가 다른 예외를 발생시키도록 원인 예외로 등록해서 다시 예외를 발생시킬 수 있다.
### 이유1. 여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위함
  ```java
  Throwable initCause(Throwable cause)  //지정한 예외를 원인 예외로 등록
  Throwable getCause()  //원인 예외를 반환
  ```

*기존 - 원인 예외를 등록하지 않고, 상위 Exception으로 처리하는 경우

  ```java
  //발생 가능한 예외가 SpaceException, MemoryException 2가지 일 때
  //InstallException은 SpaceException, MemoryException의 조상 클래스여야 함 (상속관계 필수)
  try { 
    startInstall(); //SpaceException 발생
  } catch (InstallException e) {
    e.printStackTrace();
  }
  ```
  - 위의 2가지 예외 중 실제로 발생한 예외가 무엇인지 알 수 없고, 상속관계로 설정해야 한다.

*개선 - 원인 예외로 등록
```java
try {
  startInstall(); //SpaceException 발생
} catch (SpaceException e) {
  InstallException ie = new InstallException("설치중 예외 발생");  //예외생성
  ie.initCause(e);  //원인예외 지정
  throw ie;      
} catch (MemoryException me) {
  ...  
}
```
- 원인 예외를 포함시키게 하면, 두 예외는 상속관계가 아니어도 상관없게 된다.
  

### 이유2. checked예외를 unchecked예외로 바꿀 수 있도록 하기 위함
  - unchecked예외로 바꾸면 예외처리를 선택적으로 할 수 있다.
  ```java
  static void startInstall throws SpaceException { 
    if(!enoughSpace())
      throw new SpaceException("설치 공간 부족");
    if(!enoughMemory())
      throw new RuntimeException(new MemoryException("메모리 공간 부족")); //생성자를 사용해 원인 예외 등록
  }
  ```