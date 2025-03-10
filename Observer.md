# Observer Pattern

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