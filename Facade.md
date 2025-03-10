# Facade Pattern

Facade pattern is a structural design pattern that provides a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
*Note: Facade pattern is more like a helper for client applications, it doesn't hide subsystem interfaces from the client. Whether to use Facade or not is completely dependent on client code.*

```java
public abstract class MineWorker {
    public void goToSleep() {
        System.out.println("Go to sleep");
    }

    public void wakeUp() {
        System.out.println("Wake up");
    }

    public void goToMine() {
        System.out.println("Go to mine");
    }

    public void goHome() {
        System.out.println("Go home");
    }

    private void action(Action action) {
        switch (action) {
            case GO_TO_SLEEP:
                goToSleep();
                break;
            case WAKE_UP:
                wakeUp();
                break;
            case GO_TO_MINE:
                goToMine();
                break;
            case GO_HOME:
                goHome();
                break;
            default:
                System.out.println("Undefined action");
                break;
        }
    }

    public void action(Action... actions) {
        for (Action action : actions) {
            action(action);
        }
    }

    public abstract void work();

    public enum Action {
        GO_TO_SLEEP, WAKE_UP, GO_TO_MINE, GO_HOME
    }
}
```

```java
public class TunnelMiner extends MineWorker {
    @Override
    public void work() {
        System.out.println("Miner is working");
    }
}

public class GoldDigger extends MineWorker {
    @Override
    public void work() {
        System.out.println("Gold digger is working");
    }
}
```

```java
public class MinerFacade {
    private final MineWorker tunnelMiner = new TunnelMiner();
    private final MineWorker goldDigger = new GoldDigger();

    public void mineGold() {
        tunnelMiner.action(Action.GO_TO_MINE, Action.WAKE_UP, Action.GO_HOME);
        goldDigger.action(Action.GO_TO_MINE, Action.WAKE_UP, Action.GO_HOME);
    }

    public void mineCoal() {
        tunnelMiner.action(Action.GO_TO_MINE, Action.WAKE_UP, Action.GO_HOME);
    }
}
```

```java
public class FacadeExample {
    public static void main(String[] args) {
        MinerFacade minerFacade = new MinerFacade();
        minerFacade.mineGold();
        minerFacade.mineCoal();
    }
}
```