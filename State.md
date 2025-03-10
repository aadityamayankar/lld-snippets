# State Pattern

State pattern is a behavioral design pattern that allows an object to change its behavior when its internal state changes. The pattern encapsulates states into separate classes and delegates the state-specific behavior to these classes.

```java
public interface State {
    void action();

    void observe();
}

public class PeacefulState implements State {
    @Override
    public void action() {
        System.out.println("The character is walking");
    }

    @Override
    public void observe() {
        System.out.println("The character is calm");
    }
}

public class AggressiveState implements State {
    @Override
    public void action() {
        System.out.println("The character is attacking");
    }

    @Override
    public void observe() {
        System.out.println("The character is angry");
    }
}
```

```java
public class Character {
    private State state;

    public Character() {
        this.state = new PeacefulState();
    }

    public void attack() {
        state = new AggressiveState();
    }

    public void setState(State state) {
        this.state = state;
    }

    public void action() {
        state.action();
    }

    public void observe() {
        state.observe();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Character character = new Character();
        character.action();
        character.observe();

        character.attack();
        character.action();
        character.observe();
    }
}
```

