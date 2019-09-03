# Задание к занятию 4.2. Коллекции. Очередь.
## Задача 3. Обработка транзакций.

### Описание
Напишем программу для перевода денег с одного счета на другой, обязательное условие что транзакции
должны выполняться в порядке добавления (создания). 

### Функционал программы
1. Создать очередь для обработки транзакций в порядке добавления
2. Вывести на экран ошибку если обработка транзакции была отклонена или закончилась с ошибкой
3. Вывод начального и конечного (после проведения транзакций) состояния счетов

### Пример
```
Начальное состояние счетов
Счет номер: 77777777777777, владелец: Mike Ross, сумма: 10000
Счет номер: 11111111111111, владелец: Bill Short, сумма: 5000000
Счет номер: 33333333333333, владелец: John Vot, сумма: 1000
Ошибка обработки транзакции: со счета: 77777777777777, на счет: 11111111111111, сумма: 1000

Конечное состояние счетов
Счет номер: 77777777777777, владелец: Mike Ross, сумма: 0
Счет номер: 11111111111111, владелец: Bill Short, сумма: 5011000
Счет номер: 33333333333333, владелец: John Vot, сумма: 0
```  

### Процесс реализации
1. Создадим класс `Account` с полями:
  - String number (номер)
  - String owner (владелец)
  - BigDecimal amount (сумма на счете)
2. Переопределим метод `toString` у класса `Account` и создадим конструктор
```
    public Account(String number, String owner, BigDecimal amount) {
        this.number = number;
        this.owner = owner;
        this.amount = amount;
    }

    @Override
    public String toString() {
        return String.format("Счет номер: %s, владелец: %s, сумма: %s", number, owner, amount);
    }
```  
3. Создадим класс `Transaction` с полями:
  - Account accountFrom (с какого счета осуществляется перевод)
  - Account accountTo (на какой счет осуществляется перевод)
  - BigDecimal amount (сумма перевода)
4. У класса `Transaction` так же переопределим метод `toString` и создадим дополнительный конструктор
со всеми аргументами
```
    public Transaction(Account accountFrom, Account accountTo, BigDecimal amount) {
        this.accountFrom = accountFrom;
        this.accountTo = accountTo;
        this.amount = amount;
    }

    @Override
    public String toString() {
        return String.format("со счета: %s, на счет: %s, сумма: %s", accountFrom.getNumber(), accountTo.getNumber(), amount);
    }
```
5. Дополнительно нам потребуется метод для выполнения транзакции, создадим его в классе `Transaction` и назовем
`make()` - этот метод будет выполнять транзакцию, переводить деньги с одного счета на другой и в случае если
это не возможно, выводить ошибку выполнения транзакции
```
    public boolean make() {
        accountFrom.setAmount(accountFrom.getAmount().subtract(amount));
        if (accountFrom.getAmount().compareTo(BigDecimal.ZERO) < 0) {
            accountFrom.setAmount(accountFrom.getAmount().add(amount));
            return false;
        }
        accountTo.setAmount(accountTo.getAmount().add(amount));
        return true;
    }
```  
6. В методе `main` (можно создать дополнительный класс `Main` или использовать любой из имеющихся)
создадим очередь для обработки транзакции 
```
    Deque<Transaction> transactions = new ArrayDeque<>();
```
7. Созадим счета с данными (номер, владелец, сумма на счете)
```
    Account account1 = new Account("77777777777777", "Mike Ross", BigDecimal.valueOf(10_000));
    Account account2 = new Account("11111111111111", "Bill Short", BigDecimal.valueOf(5_000_000));
    Account account3 = new Account("33333333333333", "John Vot", BigDecimal.valueOf(1000));
```
8. Создадим транзакции перевода с одного счета на другой (будем переводить все деньги на счет к Bill Short - account2)
```
    Transaction transaction1 = new Transaction(account1, account2, BigDecimal.valueOf(9_000));
    Transaction transaction2 = new Transaction(account3, account2, BigDecimal.valueOf(1_000));
    Transaction transaction3 = new Transaction(account1, account2, BigDecimal.valueOf(1_000));
    Transaction transaction4 = new Transaction(account1, account2, BigDecimal.valueOf(1_000));
```
9. Созданные транзации добавим в очередь выполнения (добавим их в порядке их нумерации)
```
    transactions.add(transaction1);
    transactions.add(transaction2);
    transactions.add(transaction3);
    transactions.add(transaction4);
``` 
10. Выведем начальное состояние счетов
```
    System.out.println("Начальное состояние счетов");
    System.out.println(account1);
    System.out.println(account2);
    System.out.println(account3);
```
11. Созаздим цикл в котором выполним все наши транзакции в очереди в порядке добавления в нее,
для выполнения каждой из них, нужно извлечь транзакцию из очереди и вызвать на ней метод `make()`,
можно создать еще одну очередь куда добавлять все неудачные транзакции (сейчас мы этого делать не будем)
```
    while (true) {
        Transaction t = transactions.poll();
        if (t == null) {
            break;
        }
        boolean transactionStatus = t.make();
        if (!transactionStatus) {
            System.out.printf("Ошибка обработки транзакции: %s\n\n", t.toString());
        }
    }
```
12. Выведем конечно состояние счетов
```
    System.out.println("Конечное состояние счетов");
    System.out.println(account1);
    System.out.println(account2);
    System.out.println(account3);
```
13. Завершим работу программы.