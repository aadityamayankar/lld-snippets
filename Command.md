# Command Pattern

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