## Задача 3. Калькулятор long

### Описание
Напишем программу для операций над очень большими числами `от -9223372036854775808 до 9223372036854775807`

### Функционал программы
1. Ввод двух значений и присваивание их переменным типа long;
2. Ввод операции:
    - abs;
    - div;
    - div_round (деление с округлением);
    - pow.
3. Вывод результата;
4. Запросить повторный ввод чисел.

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
1. Создадим в методе `main` объект типа Scanner для чтения данных
```java
    Scanner scanner = new Scanner(System.in);
```
2. Добавим цикл для возможности повторного ввода значений в программу
```java
    while (true) {
        ...
    }
```
3. В тело цикла добавим запрос и чтение значений, в случае если пользователь ввел вместо первого значения `end` выйдем
из цикла и завершим работу программы
```java
    System.out.println("Введите первое число (для выхода введите end)");
    String input = scanner.next();
    if ("end".equals(input)) {
        break;
    }
    long value1 = Long.parseLong(input);
    System.out.println("Введите второе число");
    long value2 = scanner.nextLong();
```
4. Добавим ввод операции над числами и совершение операции:
```java
     System.out.println("Выберите операцию abs, div, div_round, pow");
     String operation = scanner.next();
     switch (operation) {
         case "abs":
             System.out.printf("value1 abs = %s %n", Math.abs(value1));
             System.out.printf("value2 abs = %s %n", Math.abs(value2));
             break;
         case "div":
             System.out.printf("div = %s %n", (double) value1 / value2);
             break;
         case "div_round":
             System.out.printf("round div = %s %n", Math.round((double) value1 / value2));
             break;
         case "pow":
             System.out.printf("pow = %s %n", Math.pow(value1, value2));
             break;
     }
```
5. В конце программы добавим визуальное разделение каждой итерации цикла программы
```java
    System.out.println("-----------------------------------------------");
```
