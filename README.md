# ObjectSize
通过 Agent 代理，获取整个 Object 占用的大小
## 导入 maven 依赖
```xml
<dependency>
    <groupId>org.vnjohn</groupId>
    <artifactId>object-size-agent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```
## 主函数测试代码

```java
import org.vnjohn.agent.ObjectSizeAgent;

/**
 * @author vnjohn
 * @since 2023/6/26
 */
public class SizeOfAnObject {
  public static void main(String[] args) {
    System.out.println(ObjectSizeAgent.sizeOf(new Object()));
    System.out.println(ObjectSizeAgent.sizeOf(new int[]{}));
    System.out.println(ObjectSizeAgent.sizeOf(new OrdinaryObject()));
  }

  // 一个 Object 占多少个字节
  // Oops = ordinary object pointers  普通对象指针
  private static class OrdinaryObject {
    // 8 _markword
    // 4 _class pointer
    int id;         // 4
    String name;    // 4
    int age;        // 4

    byte b1;        // 1
    byte b2;        // 1

    Object o;       // 4
    byte b3;        // 1
  }
}
```
## 可修改如下启动参数
可选参数 

1、+ 代表压缩普通对象,- 代表不进行压缩,按道理来说引用类型都是占用 8 字节的
  -XX:-UseCompressedOops

2、+ 代表 classPointer 只占用 4 字节进行压缩,- 代表不进行压缩仍为 8 字节
  -XX:+UseCompressedClassPointers

3、启动测试 Main 方法时，必须指定的参数（找到 maven 打好包后的 jar 包所在路径）：
```java
-javaagent:/Users/vnjohn/repository/org/vnjohn/object-size-agent/1.0.0-SNAPSHOT/object-size-agent-1.0.0-SNAPSHOT.jar
```
4、第一、二个参数可以自行进行添加或更改，进行测试
