# Abstract Factory Pattern

Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

```java
public interface Castle {
    String getDescription();
}

public interface King {
    String getDescription();
}

public interface Army {
    String getDescription();
}
```

```java
public class ElfCastle implements Castle {
    static final String DESCRIPTION = "This is the Elven castle!";
    public String getDescription() {
        return DESCRIPTION;
    }
}

public class OrcCastle implements Castle {
    static final String DESCRIPTION = "This is the Orc castle!";
    public String getDescription() {
        return DESCRIPTION;
    }
}
```

```java
public class ElfKing implements King {
    static final String DESCRIPTION = "This is the Elven king!";
    public String getDescription() {
        return DESCRIPTION;
    }
}

public class OrcKing implements King {
    static final String DESCRIPTION = "This is the Orc king!";
    public String getDescription() {
        return DESCRIPTION;
    }
}
```

```java
public class ElfArmy implements Army {
    static final String DESCRIPTION = "This is the Elven army!";
    public String getDescription() {
        return DESCRIPTION;
    }
}

public class OrcArmy implements Army {
    static final String DESCRIPTION = "This is the Orc army!";
    public String getDescription() {
        return DESCRIPTION;
    }
}
```

```java
public interface KingdomFactory {
    Castle createCastle();
    King createKing();
    Army createArmy();
}
```

```java
public class ElfKingdomFactory implements KingdomFactory {
    public Castle createCastle() {
        return new ElfCastle();
    }

    public King createKing() {
        return new ElfKing();
    }

    public Army createArmy() {
        return new ElfArmy();
    }
}

public class OrcKingdomFactory implements KingdomFactory {
    public Castle createCastle() {
        return new OrcCastle();
    }

    public King createKing() {
        return new OrcKing();
    }

    public Army createArmy() {
        return new OrcArmy();
    }
}
```

```java
public class App {
    private King king;
    private Castle castle;
    private Army army;

    public App(KingdomFactory factory) {
        king = factory.createKing();
        castle = factory.createCastle();
        army = factory.createArmy();
    }

    public void createKingdom() {
        System.out.println(king.getDescription());
        System.out.println(castle.getDescription());
        System.out.println(army.getDescription());
    }

    public static void main(String[] args) {
        App app = new App(new ElfKingdomFactory());
        app.createKingdom();
    }

}
```