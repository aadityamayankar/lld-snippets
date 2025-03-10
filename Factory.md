# Factory Pattern

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