## Задача 3. Конвертер

### Описание
Напишем программу перевода из одной системы счисления в другую. Из 16 и 2 в 10 и обратно.

### Функционал программы
1. Ввод числа;
2. Перевод из одной системы в другую;
3. Вывод результата.

### Пример
```
Из какой системы в какую переводим?
1. 16 -> 10
2. 2 -> 10
3. 10 -> 16
4. 10 -> 2
Выберите и нажмите ввод:
2 <enter>
Введите число для перевода:
00000001
Результат:
1
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
2. Нужно вывести варианты перевода значений, с помощью `System.out.println` и прочитать значение c помощью `scanner`
```java
    System.out.println("Из какой системы в какую переводим?"
      + "\n1. 16 -> 10" 
      + "\n2. 2 -> 10"
      + "\n3. 10 -> 16"
      + "\n4. 10 -> 2"
      + "\nВыберите и нажмите ввод или введите 0 для завершения программы:");
    int choise = scanner.nextInt();
``` 
3. Добавим условие выхода из цикла
```java
    if (choise == 0) {
        System.out.println("Программа завершена");
        break;
    }
``` 
4. Добавим вне метода `main` еще один метод конвертирования введеных значений
```java
    static String convertValue(String inputValue, int choise) {
        String result = "";
        switch(choise) {
          case 1: {
            result = Integer.valueOf(inputValue, 16).toString();
            break;
          }
          case 2: {
            result = Integer.valueOf(inputValue, 2).toString();
            break;
          }
          case 3: {
            int digitalValue = Integer.valueOf(inputValue);
            result = Integer.toHexString(digitalValue);
            break;
          }
          case 4: {
            int digitalValue = Integer.valueOf(inputValue);
            result = Integer.toBinaryString(digitalValue);
            break;
          }
      }
      return result;
    }
```
5. Прочитаем значение для перевода в методе `main` и выведем результат
```java
    System.out.println("Введите значение для перевода и нажмите enter");
    String inputValue = scanner.next();

    String result = convertValue(inputValue, choise);
    System.out.println("Результат: " + result);
``` 
6. Повторно запросим данные.
