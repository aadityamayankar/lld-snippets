# Iterator Pattern

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