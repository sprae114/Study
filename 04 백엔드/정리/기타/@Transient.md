[JPA에서 @Transient 애노테이션이 존재하는 이유](https://gmoon92.github.io/jpa/2019/09/29/what-is-the-transient-annotation-used-for-in-jpa.html)

## @Transient
어노테이션이 붙은 필드는 직렬화 과정에서 무시됩니다.

직렬화란, 객체의 상태를 바이트 스트림으로 변환하여 파일에 저장하거나 네트워크를 통해 전송할 수 있도록 하는 것을 말합니다. 그러나 모든 필드가 직렬화에 적합하지 않을 수 있습니다. 예를 들어, 메모리 사용량이 큰 필드나 네트워크 전송에 적합하지 않은 데이터 등을 가진 필드가 해당될 수 있습니다.


## 예시

```java
import java.io.*;

class User implements Serializable {
    private String name;
    
    @Transient
    private String password;

    public User(String name, String password) {
        this.name = name;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}


public class Main {
  public static void main(String[] args) throws IOException, ClassNotFoundException {
      User user = new User("John", "1234");

      // serialize the object
      FileOutputStream fileOutputStream = new FileOutputStream("user.ser");
      ObjectOutputStream objectOutputStream = new ObjectOutputStream(fileOutputStream);
      objectOutputStream.writeObject(user);

      // close resources
      objectOutputStream.close();
      fileOutputStream.close();

      
      // deserialize the object
      FileInputStream fileInputStream = new FileInputStream("user.ser");
      ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);
      
       User deserializedUser= (User)objectInputStream.readObject();

       // close resources
       objectInputStream.close();
       fileInputStream.close();

       System.out.println(deserializedUser.toString());
  }
}
```

위의 코드는 `name`과 `password`라는 두 개의 필드를 가진 `User` 클래스를 생성하고, 이 객체를 파일에 저장한 후 다시 읽어들이는 과정을 보여줍니다. 여기서 `password` 필드에는 `@Transient` 어노테이션이 붙어 있으므로 이 필드는 직렬화 대상에서 제외됩니다.


```javascript
User{name='John', password='null'}
```
직렬화 과정에서 `password`필드가 제외되었으므로, 역직렬화(디시리얼라이즈)된 객체에서 해당 값은 null이 됩니다.