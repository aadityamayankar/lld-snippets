# Builder Pattern

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