FinalizerDaemon
---
```
FinalizerDaemon是用来gc的daemon thread
实现了object的finalize（）的类在创建时会新建一个FinalizerReference，这个对象是强引用类型  

封装了override finalize（）的对象，叫原对象。  

原对象没有被其他对象引用时（FinalizeReference除外），执行GC不会马上被清除掉，
而是放入一个静态链表中（ReferenceQueue），有一个守护线程专门去维护这个链表。
轮到该线程执行时就弹出里面的对象，执行它们的finalize（），对应的FinalizerReference对象在下次执行GC时就会被清理掉。
守护线程优先级比较低，运行的时间比较少。如果较短时间内创建较多的原对象，就会因为守护线程来不及弹出原对象而使FinalizerReference和原对象都得不到回收。
无论怎样调用GC都没有用的，因为只要原对象没有被守护线程弹出执行其finalize（）方法，FinalizerReference对象就不会被GC回收。

```

动态代理
--
```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.util.Date;

public class LogHandler implements InvocationHandler {
    Object target;  // 被代理的对象，实际的方法执行者

    public LogHandler(Object target) {
        this.target = target;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        before();
        Object result = method.invoke(target, args);  // 调用 target 的 method 方法
        after();
        return result;  // 返回方法的执行结果
    }
    // 调用invoke方法之前执行
    private void before() {
        System.out.println(String.format("log start time [%s] ", new Date()));
    }
    // 调用invoke方法之后执行
    private void after() {
        System.out.println(String.format("log end time [%s] ", new Date()));
    }
}
```
参考链接
---
https://blog.csdn.net/longlong2015/article/details/79487520
