# Задача 2: Дженерик класс с несколькими параметрами

## Описание
У дженерик класса может быть несколько параметров. 

Расширьте созданный в первой задача типизированный класс `Box`. Помимо параметра `T` добавьте параметр `K`:
```java
class Box<K,T>{ 

}
```

Теперь класс `Box` будет обладать двумя полями:
```java
private K key;
private T obj;
```

Их значения могут быть получены через конструктор класса. Конструктор класса будет выглядеть следующим образом:
```java
public Box(K key, T obj){
    this.key = key;
    this.obj = obj;
}
```

Для работы с переменными класса добавьте методы `get` и `set`. Метод `toString()` будет выводить значения и типы объектов класса `Box`:
```java
@Override
public String toString() {
    return "Box{" +
            "key=" + key +
            "; keyType=" + key.getClass().getName() +
            ", obj=" + obj +
            "; objType=" + obj.getClass().getName() +
            '}';
}
```

## Реализация
Создадим несколько экземпляров класса `Box`, используя при этом различные типы:
```java
public class Main {
    public static void main(String a[]) {
        // параметризируем класс типом String для ключа и значения
        Box<String, String> sample1 = new Box<>("имя", "Нетология");
        System.out.println(sample1);
        // параметризируем класс типом Integer для ключа и Boolean для значения
        Box<Integer, Boolean> sample2 = new Box<>(1, Boolean.TRUE);
        System.out.println(sample2);
    }
}
```

В результате выполнения программы в консоле увидим следующие строки:
```
Box{key=имя; keyType=java.lang.String, obj=Нетология; objType=java.lang.String}
Box{key=1; keyType=java.lang.Integer, obj=true; objType=java.lang.Boolean}
```
