# Low Level Design Patterns

### Creational Patterns
1. [Singleton](#singleton)
2. [Builder](#builder)
3. [Factory Method](#factory-method)
4. [Abstract Factory](#abstract-factory)
5. [Prototype](#prototype)

### Structural Patterns
1. [Adapter](#adapter)
2. [Bridge](#bridge)
3. [Decorator](#decorator)
4. [Facade](#facade)
5. [Proxy](#proxy)

### Behavioral Patterns
1. [Chain of Responsibility](#chain-of-responsibility)
2. [Command](#command)
3. [Iterator](#iterator)
4. [Observer](#observer)
5. [State](#state)
6. [Strategy](#strategy)

---

##### Singleton

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

---

#### Builder

Builder pattern is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

```java
public class Hero {
    private final String name;
    private final String profession;
    private final String hairType;
    private final String hairColor;
    private final String armor;
    private final String weapon;

    private Hero(Builder builder) {
        this.name = builder.name;
        this.profession = builder.profession;
        this.hairType = builder.hairType;
        this.hairColor = builder.hairColor;
        this.armor = builder.armor;
        this.weapon = builder.weapon;
    }

    public static class Builder {
        private final String name;
        private final String profession;
        private String hairType;
        private String hairColor;
        private String armor;
        private String weapon;

        public Builder(String name, String profession) {
            if (name == null || profession == null) {
                throw new IllegalArgumentException("name and profession can not be null");
            }
            this.name = name;
            this.profession = profession;
        }

        public Builder withHairType(String hairType) {
            this.hairType = hairType;
            return this;
        }

        public Builder withHairColor(String hairColor) {
            this.hairColor = hairColor;
            return this;
        }

        public Builder withArmor(String armor) {
            this.armor = armor;
            return this;
        }

        public Builder withWeapon(String weapon) {
            this.weapon = weapon;
            return this;
        }

        public Hero build() {
            return new Hero(this);
        }
    }
}
```

```java
public class BuilderExample {
    public static void main(String[] args) {
        Hero mage = new Hero.Builder("Riobard", "Mage")
            .withHairColor("Pink")
            .withWeapon("Staff")
            .build();
        Hero warrior = new Hero.Builder("Amberjill", "Warrior")
            .withHairColor("Green")
            .withArmor("Plate")
            .withWeapon("Sword")
            .build();
    }
}
```

---

#### Factory

Factory pattern is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. This pattern is most commonly used when a class can't anticipate the class of objects it must create.

```java
public class Hero {
    private String profession;

    public void setProfession(String profession) {
        this.profession = profession;
    }

    @Override
    public String toString() {
        return "Hero [profession=" + profession + "]";
    }
}
```

```java
public class HeroFactory {
    public static Hero createWarrior() {
        Hero hero = new Hero();
        hero.setProfession("warrior");
        return hero;
    }

    public static Hero createMage() {
        Hero hero = new Hero();
        hero.setProfession("mage");
        return hero;
    }

    public static Hero createThief() {
        Hero hero = new Hero();
        hero.setProfession("thief");
        return hero;
    }
}
```

```java
public class FactoryExample {
    public static void main(String[] args) {
        Hero warrior = HeroFactory.createWarrior();
        System.out.println(warrior);

        Hero mage = HeroFactory.createMage();
        System.out.println(mage);

        Hero thief = HeroFactory.createThief();
        System.out.println(thief);
    }
}
```

---

#### Abstract Factory

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

---

#### Prototype

Prototype pattern is a creational design pattern that allows cloning objects, even complex ones, without coupling to their specific classes. This pattern is used when creating an instance of a class is either expensive or complicated. The prototype pattern is used to create a new object by clone an existing object, known as the prototype.

```java
abstract class Shape implements Cloneable {
    private String id;
    protected String type;

    abstract void draw();

    public String getType() {
        return type;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public Object clone() {
        Object clone = null;
        try {
            clone = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }
}
```

```java
class Rectangle extends Shape {
    public Rectangle() {
        type = "Rectangle";
    }

    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
    }
}

class Square extends Shape {
    public Square() {
        type = "Square";
    }

    @Override
    public void draw() {
        System.out.println("Inside Square::draw() method.");
    }
}

class Circle extends Shape {
    public Circle() {
        type = "Circle";
    }

    @Override
    public void draw() {
        System.out.println("Inside Circle::draw() method.");
    }
}
```

```java
class ShapeCache {
    private static Hashtable<String, Shape> shapeMap = new Hashtable<String, Shape>();

    public static Shape getShape(String shapeId) {
        Shape cachedShape = shapeMap.get(shapeId);
        return (Shape) cachedShape.clone();
    }

    public static void loadCache() {
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(), circle);

        Square square = new Square();
        square.setId("2");
        shapeMap.put(square.getId(), square);

        Rectangle rectangle = new Rectangle();
        rectangle.setId("3");
        shapeMap.put(rectangle.getId(), rectangle);
    }
}
```

```java
public class PrototypePattern {
    public static void main(String[] args) {
        ShapeCache.loadCache();

        Shape clonedShape = (Shape) ShapeCache.getShape("1");
        System.out.println("Shape : " + clonedShape.getType());

        Shape clonedShape2 = (Shape) ShapeCache.getShape("2");
        System.out.println("Shape : " + clonedShape2.getType());

        Shape clonedShape3 = (Shape) ShapeCache.getShape("3");
        System.out.println("Shape : " + clonedShape3.getType());
    }
}
```

---

#### Adapter

Adapter pattern is a structural design pattern that allows objects with incompatible interfaces to collaborate. It is used to convert the interface of a class into another interface that a client expects. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

```java
interface MediaPlayer {
    void play(String audioType, String fileName);
}

interface AdvancedMediaPlayer {
    void playVlc(String fileName);
    void playMp4(String fileName);
}
```

```java
class VlcPlayer implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        System.out.println("Playing vlc file. Name: " + fileName);
    }

    @Override
    public void playMp4(String fileName) {
        // do nothing
    }
}

class Mp4Player implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        // do nothing
    }

    @Override
    public void playMp4(String fileName) {
        System.out.println("Playing mp4 file. Name: " + fileName);
    }
}
```

```java
class MediaAdapter implements MediaPlayer {
    AdvancedMediaPlayer advancedMusicPlayer;

    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMusicPlayer = new VlcPlayer();
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMusicPlayer = new Mp4Player();
        }
    }

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMusicPlayer.playVlc(fileName);
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMusicPlayer.playMp4(fileName);
        }
    }
}
```

```java
class AudioPlayer implements MediaPlayer {
    MediaAdapter mediaAdapter;

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing mp3 file. Name: " + fileName);
        } else if (audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        } else {
            System.out.println("Invalid media. " + audioType + " format not supported");
        }
    }
}

public class AdapterPatternDemo {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();

        audioPlayer.play("mp3", "beyond the horizon.mp3");
        audioPlayer.play("mp4", "alone.mp4");
        audioPlayer.play("vlc", "far far away.vlc");
        audioPlayer.play("avi", "mind me.avi");
    }
}
```

---

#### Bridge

Bridge pattern is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.
*Note: Similar to Strategy & Adapter Patterns, but with a different intent.*

```java
interface DrawAPI {
    void drawCircle(int radius, int x, int y);
}

class RedCircle implements DrawAPI {
    @Override
    public void drawCircle(int radius, int x, int y) {
        System.out.println("Drawing Circle[ color: red, radius: " + radius + ", x: " + x + ", " + y + "]");
    }
}

class GreenCircle implements DrawAPI {
    @Override
    public void drawCircle(int radius, int x, int y) {
        System.out.println("Drawing Circle[ color: green, radius: " + radius + ", x: " + x + ", " + y + "]");
    }
}
```

```java
abstract class Shape {
    protected DrawAPI drawAPI;

    protected Shape(DrawAPI drawAPI) {
        this.drawAPI = drawAPI;
    }

    public abstract void draw();
}

class Circle extends Shape {
    private int x, y, radius;

    public Circle(int x, int y, int radius, DrawAPI drawAPI) {
        super(drawAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    @Override
    public void draw() {
        drawAPI.drawCircle(radius, x, y);
    }
}
```

```java
public class BridgePatternDemo {
    public static void main(String[] args) {
        Shape redCircle = new Circle(100, 100, 10, new RedCircle());
        Shape greenCircle = new Circle(100, 100, 10, new GreenCircle());

        redCircle.draw();
        greenCircle.draw();
    }
}
```

---

#### Decorator

Decorator pattern is a structural design pattern that allows a user to add new functionality to an existing object without altering its structure. It achieves this by creating a new decorator class that wraps the original class.

```java
public interface Car {
    void assemble();
}

public class BasicCar implements Car {
    @Override
    public void assemble() {
        System.out.print("Basic Car.");
    }
}
```

```java
public class CarDecorator implements Car {
    protected Car car;
    
    public CarDecorator(Car car) {
        this.car = car;
    }
    
    @Override
    public void assemble() {
        this.car.assemble();
    }
}

public class SportsCar extends CarDecorator {
    public SportsCar(Car car) {
        super(car);
    }
    
    @Override
    public void assemble() {
        super.assemble();
        System.out.print(" Adding features of Sports Car.");
    }
}

public class LuxuryCar extends CarDecorator {
    public LuxuryCar(Car car) {
        super(car);
    }
    
    @Override
    public void assemble() {
        super.assemble();
        System.out.print(" Adding features of Luxury Car.");
    }
}
```

```java
public class DecoratorPattern {
    public static void main(String[] args) {
        Car sportsCar = new SportsCar(new BasicCar());
        sportsCar.assemble();
        System.out.println("\n*****");
        
        Car sportsLuxuryCar = new SportsCar(new LuxuryCar(new BasicCar()));
        sportsLuxuryCar.assemble();
    }
}
```

---

#### Facade

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

---

#### Proxy

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

---

#### Chain Of Responsibility

Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it along the chain.

```java
public abstract class Logger {
    public static int INFO = 1;
    public static int DEBUG = 2;
    public static int ERROR = 3;

    protected int level;

    protected Logger nextLogger;

    public void setNextLogger(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }

    public void logMessage(int level, String message) {
        if (this.level <= level) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
    }

    abstract protected void write(String message);
}
```

```java
public class ConsoleLogger extends Logger {
    public ConsoleLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("Standard Console::Logger: " + message);
    }
}

public class ErrorLogger extends Logger {
    public ErrorLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("Error Console::Logger: " + message);
    }
}

public class FileLogger extends Logger {
    public FileLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("File::Logger: " + message);
    }
}
```

```java
public class ChainOfResponsibilityPatternDemo {
    private static Logger getChainOfLoggers() {
        Logger errorLogger = new ErrorLogger(Logger.ERROR);
        Logger fileLogger = new FileLogger(Logger.DEBUG);
        Logger consoleLogger = new ConsoleLogger(Logger.INFO);

        errorLogger.setNextLogger(fileLogger);
        fileLogger.setNextLogger(consoleLogger);

        return errorLogger;
    }

    public static void main(String[] args) {
        Logger loggerChain = getChainOfLoggers();

        loggerChain.logMessage(Logger.INFO, "This is an information.");
        loggerChain.logMessage(Logger.DEBUG, "This is a debug level information.");
        loggerChain.logMessage(Logger.ERROR, "This is an error information.");
    }
}
```

---

#### Command

Command pattern is a behavioral design pattern that is used to encapsulate all information needed to perform an action or trigger an event at a later time. It supports undoable operations and can be used to implement logging and transactional systems.

Example bank account transaction:

```java
public interface Command {
    void execute();
    void undo();
}

public class DepositCommand implements Command {
    private final Account account;
    private final int amount;

    public DepositCommand(Account account, int amount) {
        this.account = account;
        this.amount = amount;
    }

    @Override
    public void execute() {
        account.deposit(amount);
    }

    @Override
    public void undo() {
        account.withdraw(amount);
    }
}
```

```java
public class Account {
    private int balance;

    public void deposit(int amount) {
        balance += amount;
    }

    public void withdraw(int amount) {
        balance -= amount;
    }
}
```

```java
public class Bank {
    private final List<Command> commands = new ArrayList<>();
    private int current;

    public void deposit(Account account, int amount) {
        Command command = new DepositCommand(account, amount);
        command.execute();
        commands.subList(current, commands.size()).clear();
        commands.add(command);
        current++;
    }

    public void undo() {
        if (current > 0) {
            Command command = commands.get(--current);
            command.undo();
        }
    }

    public void redo() {
        if (current < commands.size()) {
            Command command = commands.get(current++);
            command.execute();
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Account account = new Account();
        Bank bank = new Bank();
        bank.deposit(account, 100);
        bank.deposit(account, 50);
        bank.undo();
        bank.redo();
    }
}
```

---

#### Iterator

Iterator is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

```java
public class Item {
    private String name;
    private int price;

    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
}
```

```java
public interface Iterator<T> {
    boolean hasNext();
    T next();
}

public class ItemCollection {
    private List<Item> items = new ArrayList<>();

    public void addItem(Item item) {
        items.add(item);
    }

    public Iterator<Item> iterator() {
        return new ItemIterator();
    }

    private class ItemIterator implements Iterator<Item> {
        private int index = 0;

        @Override
        public boolean hasNext() {
            return index < items.size();
        }

        @Override
        public Item next() {
            return items.get(index++);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        ItemCollection itemCollection = new ItemCollection();
        itemCollection.addItem(new Item("Item 1", 10));
        itemCollection.addItem(new Item("Item 2", 20));
        itemCollection.addItem(new Item("Item 3", 30));

        Iterator<Item> iterator = itemCollection.iterator();
        while (iterator.hasNext()) {
            Item item = iterator.next();
            System.out.println(item.getName() + ": " + item.getPrice());
        }
    }
}
```

---

#### Observer

Observer pattern is a behavioral design pattern that defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

```java
public interface WheatherObserver {
    void update(int temperature);
}

public class PhoneDisplay implements WheatherObserver {
    @Override
    public void update(int temperature) {
        System.out.println("Phone Display: " + temperature);
    }
}

public class LaptopDisplay implements WheatherObserver {
    @Override
    public void update(int temperature) {
        System.out.println("Laptop Display: " + temperature);
    }
}
```

```java
public class WheatherStation {
    private int temperature;
    private List<WheatherObserver> observers = new ArrayList<>();

    public void addObserver(WheatherObserver observer) {
        observers.add(observer);
    }

    public void removeObserver(WheatherObserver observer) {
        observers.remove(observer);
    }

    public void setTemperature(int temperature) {
        this.temperature = temperature;
        notifyObservers();
    }

    private void notifyObservers() {
        for (WheatherObserver observer : observers) {
            observer.update(temperature);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        WheatherStation station = new WheatherStation();
        PhoneDisplay phoneDisplay = new PhoneDisplay();
        LaptopDisplay laptopDisplay = new LaptopDisplay();

        station.addObserver(phoneDisplay);
        station.addObserver(laptopDisplay);

        station.setTemperature(20);
        station.setTemperature(25);
    }
}
```

---

#### State

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

---

#### Strategy

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


----

# Thread safe Collections

#### Lock free syncronized  variables

* AtomicInteger
* AtomicLong
* AtomicBoolean
* AtomicReference<>

Supported methods:

* get()
* set()
* compareAndSet()
* getAndIncrement()
* getAndDecrement()
* getAndAdd()

Note: Collections provide synchronized wrappers by default. These block the entire collection for operations because of which it's generally not recommended. 
Examples:
* Collection<Integer> syncCollection = Collections.synchronizedCollection(new ArrayList<>());
* List<Integer> syncList = Collections.synchronizedList(new ArrayList<>());
* Map<Integer, String> syncMap = Collections.synchronizedMap(new HashMap<>());
* Set<Integer> syncSet = Collections.synchronizedSet(new HashSet<>());
... etc

#### Dynamic Array

* Vector<>: This is legacy and every operation requires locking/unlocking so too much overhead
* ConcurrentLinkedQueue<>: Preferred for high concurrency scenarios. Lock free internally. **Iterating has high overhead**
* CopyOnWriteArrayList<>: Preferred when there are more reads than writes. Creates a new copy on write and replaces it with CAS. Better for iteration.

#### Stack / Queue

* Stack<>: Extends the Vector<> class so it's generally not used because of the same reasons.
* LinkedBlockingQueue<>: Blocks writes if underlying queue is full and reads are blocked if queue is empty. Internal operations are atomic. Generally preferred for producer / consumer situations.
* ConcurrentLinkedDeque<>: Lock free internally

#### Priority Queue
 
* PriorityBlockingQueue<>: Similar to BlockingQueue

#### Hash Map / Hash Set

* ConcurrentHashMap<>: Almost always the recommended implementation because it allows lock free reads and locked writes. Internal implementation has lock stripping and other optimizations.

#### Tree Map / Tree Set

* ConcurrentSkipListMap<>: Almost always the recommended implementation.

#### Linked Hash Map / Set

* No direct implementation 
