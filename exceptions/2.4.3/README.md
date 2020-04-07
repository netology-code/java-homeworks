## Задача 3. Проверка корректности url веб-сайта

### Описание
Напишем программу для проверки коррекности введенного пользователем url, для того что бы определить что url корректный, нужно проверить
что:
    1. Начинается с указания схемы (протокола) http или https
    2. В строке указанного url нет пробелов
Для сулчая когда строка содержит пробел нужно создать и выбросить checked исключение NotValidUrlException.    
Для случая когда схема не соотвествует http или https, нужно выбросить checked исключение UnsupportedSchemaException, унаследованное от базового исключения NotValidUrlException.

### Функционал программы
1. Создание Scanner для чтения url из консоли, в блоке try/catch with resources;
2. Создание checked исключения NotValidUrlException;
3. Создание checked исключения UnsupportedSchemaException унаследованного от NotValidUrlException;
4. В блоке catch обрабатывать любые исключения и в случае попадания в блок выводить сообщение "Введенный url некорректен"; 
5. Если ошибок не возникло вывести сообщение "Введенный url корректен".

### Процесс реализации
1. Создадим проект, с базовым исключением NotValidUrlException, так это checked исключение оно должно быть унаследовано от класса Exception,
для того чтобы можно было передавать свое сообщение, переопределим конструктор с аргументом message класса родителя: 
```java
public class NotValidUrlException extends Exception {
    public NotValidUrlException(String message) {
        super(message);
    }
}
```
2. Добавим еще одно исключение UnsupportedSchemaException, так как по условию задачи оно является наследником только что созданного класса NotValidUrlException
в котором описан только один конструктор с сообщением, нам нужно добавить его в созданный класс:
```java
public class UnsupportedSchemaException extends NotValidUrlException {
    public UnsupportedSchemaException(String message) {
        super(message);
    }
}
```
3. Создадим класс Main cо статичным main для запуска программы, в этом методе откроем Scanner для чтения данных из консоли, 
воспользуемся блоком try with resources чтобы освободить ресурсы в случае ошибки и закрыть Scanner. Переменная url будет использоваться
для хранения введеннго значения для дальнейшей проверки. В блоке catch мы будем ловить все пользовательские исключения, которые могу возникнуть при работе нашей программы:
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(System.in)) {
            String url = scanner.nextLine();
        } catch (Exception e) {
            System.out.print("Введенный url некорректен");
        }
    }
}
```
4. Добавим класс UrlUtils в который добавим метод для проверки введенного url - validate, этот метод проверяет два условия (соответствие схемы http и https, наличие пробела)
```java
public class UrlUtils {
    public static void validate(String value) throws NotValidUrlException {
        boolean isValidSchema = value.startsWith("http://") || value.startsWith("https://");
        if (!isValidSchema) {
            throw new UnsupportedSchemaException(value);
        }
        if (value.contains(" ")) {
            throw new NotValidUrlException("Url содержит пробелы: "  + value);
        }
    }
}
```
5. Добавим созданный метод проверки урл в метод main:
```java
public static void main(String[] args) {
    try (Scanner scanner = new Scanner(System.in)) {
        String url = scanner.nextLine();
        UrlUtils.validate(url);
        System.out.print("Введенный url корректен");
    } catch (Exception e) {
        System.out.print("Введенный url некорректен");
    }
}
```
6. Готово, программа с использованием checked исключений создана.
