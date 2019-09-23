## Задача 4. Калькулятор long

### Описание
Напишем программу для операций над очень большими числами `от -9223372036854775808 до 9223372036854775807`,
но добавим удаление лишних пробелов при вводе и возможность ввода только операций из списка: abs, div, div_round, pow.

### Функционал программы
1. Ввод двух значений и присваивание их переменным типа long;
2. Ввод операций:
    - abs;
    - div;
    - div_round (деление с округлением);
    - pow.
3. Вывод результата;
4. Запрос повторного ввода чисел.

### Пример
```
Введите первое число (для выхода введите end)
1
Введите второе число
2
Выберите операцию abs, div, div_round, pow
div
div = 0.5 
-----------------------------------------------
Введите первое число (для выхода введите end)
3
Введите второе число
4
Выберите операцию abs, div, div_round, pow
pow
pow = 81.0 
-----------------------------------------------
Введите первое число (для выхода введите end)
end
```

### Процесс реализации

1. Создадим в методе `main` объект типа Scanner для чтения данных:

```java
    Scanner scanner = new Scanner(System.in);
```

2. Добавим цикл для возможности повторного ввода значений в программу:

```java
    while (true) {
        ...
    }
```

3. В тело цикла добавим запрос и чтение значений. В случае, если пользователь ввел вместо первого значения `end`, выйдем
из цикла и завершим работу программы:

```java
    System.out.println("Введите первое число (для выхода введите end)");
    String input = scanner.next().trim();
    if ("end".equals(input)) {
        break;
    }
    long value1 = Long.parseLong(input);
    System.out.println("Введите второе число");
    long value2 = Long.parseLong(scanner.next().trim());
```

4. Создадим перечисление возможных операций:

```java
    public enum Operation {
        DIV,
        ABS,
        DIV_ROUND,
        POW
    }
```

5. Добавим метод вычисления операций:

```java
    private static void calculate(long value1, long value2, Operation operation) {
        switch (operation) {
            case ABS:
                System.out.printf("value1 abs = %s %n", Math.abs(value1));
                System.out.printf("value2 abs = %s %n", Math.abs(value2));
                break;
            case DIV:
                System.out.printf("div = %s %n", (double) value1 / value2);
                break;
            case DIV_ROUND:
                System.out.printf("round div = %s %n", Math.round((double) value1 / value2));
                break;
            case POW:
                System.out.printf("pow = %s %n", Math.pow(value1, value2));
                break;
        }
    }
```

6. Добавим чтение операции:

```java
     System.out.println("Выберите операцию abs, div, div_round, pow");
     Operation operation = Operation.valueOf(scanner.next().trim());
```

7. Вызовем метод вычисления операции:

```java
     calculate(value1, value2, operation);
```

8. В конце программы добавим визуальное разделение каждой итерации цикла программы:

```java
    System.out.println("-----------------------------------------------");
```
