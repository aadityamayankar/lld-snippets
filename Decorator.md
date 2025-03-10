# Decorator Pattern

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