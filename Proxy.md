# Proxy Pattern

Proxy pattern is a structural pattern that provides a surrogate or placeholder for another object to control access to it.
*Note: Proxy pattern is similar to facade pattern, but with a different intent. Facade pattern is used to provide a simplified interface to a larger body of code.*

```java
public interface WizardTower {
    void enter(Wizard wizard);
}

public class IvoryTower implements WizardTower {
    public void enter(Wizard wizard) {
        System.out.println(wizard + " enters the tower.");
    }
}

public class Wizard {
    private final String name;

    public Wizard(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return name;
    }
}
```

```java
public class WizardTowerProxy implements WizardTower {
    private static final int NUM_WIZARDS_ALLOWED = 3;
    private int numWizards;
    private final WizardTower tower;

    public WizardTowerProxy(WizardTower tower) {
        this.tower = tower;
    }

    @Override
    public void enter(Wizard wizard) {
        if (numWizards < NUM_WIZARDS_ALLOWED) {
            tower.enter(wizard);
            numWizards++;
        } else {
            System.out.println(wizard + " is not allowed to enter!");
        }
    }
}

public class App {
    public static void main(String[] args) {
        WizardTowerProxy proxy = new WizardTowerProxy(new IvoryTower());
        proxy.enter(new Wizard("Red wizard"));
        proxy.enter(new Wizard("White wizard"));
        proxy.enter(new Wizard("Black wizard"));
        proxy.enter(new Wizard("Green wizard"));
    }
}
```