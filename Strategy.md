# Strategy Pattern

Strategy pattern is a behavioral design pattern that enables selecting an algorithm at runtime. It defines a family of algorithms, encapsulates each algorithm, and makes the algorithms interchangeable within that family.

```java
public interface AttackStrategy {
    void attack();
}

public class MeleeStrategy implements AttackStrategy {
    @Override
    public void attack() {
        System.out.println("Attacking with a sword");
    }
}

public class RangedStrategy implements AttackStrategy {
    @Override
    public void attack() {
        System.out.println("Attacking with a bow");
    }
}
```

```java
public class Character {
    private AttackStrategy attackStrategy;

    public Character(AttackStrategy attackStrategy) {
        this.attackStrategy = attackStrategy;
    }

    public void attack() {
        attackStrategy.attack();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Character character = new Character(new MeleeStrategy());
        character.attack();

        character = new Character(new RangedStrategy());
        character.attack();
    }
}
```