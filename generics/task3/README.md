# Задача 3: Наследование дженерик классов (со звездочкой *)

## Описание
В данной задаче требуется реализовать коробку, в которую можно будет положить только вкусные фрукты и нельзя будет положить полезные овощи.

Начните с того, что создайте обобщающий класс фрутов:
```java
class Fruit {
    public void printClass() {
        System.out.println("I am super class Fruit");
    }
}
```

Далее создайте несколько подтипов фруктов, которые будут унаследованы от общего предка `Fruit`:
```java
class Apple extends Fruit {
    @Override
    public void printClass() {
        System.out.println("I am sub class Apple");
    }
}

class Banana extends Fruit {
    @Override
    public void printClass() {
        System.out.println("I am sub class Banana");
    }
}
```

Создайте один полезный овощ:
```java
class Cabbage {
    public void printClass() {
        System.out.println("I am Cabbage");
    }
}
```

Теперь создайте дженерик коробку, в которую можно будет положить только фрукты. Реализуется это путем указания, от какого класса может быть наследован параметр `T`:
```java
class Box<K, T extends Fruit> {
    // Реализация класса коробки из предыдущего задания
}
```

## Реализация
Создайте несколько фруктов и поместите в созданные специально для них коробки:
```java
public class Main {
    public static void main(String a[]) {
        Box<String, Banana> bananaBox = new Box<>("banana", new Banana());
        bananaBox.getObj().printClass();

        Box<String, Apple> appleBox = new Box<>("apple", new Apple());
        appleBox.getObj().printClass();

        Box<String, Fruit> fruitBox = new Box<>("fruit", new Fruit());
        fruitBox.getObj().printClass();

        Box<String, Cabbage> cabbageBox = new Box<>("cabbage", new Cabbage());
        cabbageBox.getObj().printClass();
    }
}
```

Обратите внимание, что в коде выше есть ошибка. Найдите ошибку и с помощью комментария объясните, по какой причине она возникла.

Исправьте ошибку. По аналогии с созданной коробкой для фруктов, создайте вторую коробку `VegetableBox`, которая бы хранила в себе только полезные овощи `Vegetable`, и положите в нее полезную капусту.

В случае успешного выполнения задания, Вы увидите в консоле следующие строки:
```
I am sub class Banana
I am sub class Apple
I am super class Fruit
I am Cabbage
```
