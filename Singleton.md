# Singleton Pattern

Singleton pattern is a creational design pattern that ensures a class has only one instance and provides a global point of access to that instance. This pattern is used when we need to ensure that a class has only one instance and provide a global point of access to it.

```java
public class Singleton {
    private static Singleton instance = null;
    private Singleton() {}
    public static synchronized Singleton getInstance() {
        if(instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

```java
class DoubleCheckedSingleton {
    private static DoubleCheckedSingleton instance = null;
    private DoubleCheckedSingleton() {}
    public static DoubleCheckedSingleton getInstance() {
        if(instance == null) {
            synchronized(DoubleCheckedSingleton.class) {
                if(instance == null) {
                    instance = new DoubleCheckedSingleton();
                }
            }
        }
        return instance;
    }
}
```

```java
class EagerSingleton {
    private static final EagerSingleton instance = new EagerSingleton();
    private EagerSingleton() {}
    public static EagerSingleton getInstance() {
        return instance;
    }
}
```