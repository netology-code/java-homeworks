## Задача 3.  Рекомендация товаров

### Описание
Напишем программу для вывода наиболее релевантного товара.

### Функционал программы
1. Создание списка/множества товаров с полями (String name и BigDecimal cost);
2. Создание карты (map) для расстановки релевантности каждого из товаров (String name, double relevant);
3. Вывод топ 3 наиболее релевантных продукта.

### Пример

```
Рекомендуем вам посмотреть так же следующие товары:
1. Наименование: lg tv, цена: 50000
2. Наименование: samsung tv, цена: 55000
3. Наименование: apple iphone, цена: 85000
```

### Реализация

1. Создадим класс продукт - `Product`, как написано в условии программы, со следующими полями:
  - String name;
  - BigDecimal cost;

2. Переопределим метод `toString`, чтобы был читаемый вывод информации о продукте. Также добавим всем полям класса 
`Product` метод `get`, чтобы можно было получать значения. Добавим конструтор для присвоения значений обоим полям во 
время создания объекта. А так же сгенерируем методы `equals` и `hashCode`, для использования класса в качестве ключа в `HashMap`:

```
    public Product(String name, BigDecimal cost) {
        this.name = name;
        this.cost = cost;
    }

    @Override
    public String toString() {
        return String.format("Наименование: %s, цена: %s", name, cost);
    }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Product product = (Product) o;
        return name.equals(product.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
```

3. Создадим класс `Main`: в нем будем создавать объекты класса продукт и вызывать остальные методы в основном
методе `main`:

```
public class Main {

    public static void main(String[] args) {

        Product product1 = new Product("samsung tv", BigDecimal.valueOf(55_000));
        Product product2 = new Product("radio sony", BigDecimal.valueOf(10_000));
        Product product3 = new Product("apple ipad", BigDecimal.valueOf(22_000));
        Product product4 = new Product("lg tv", BigDecimal.valueOf(50_000));
        Product product5 = new Product("apple iphone", BigDecimal.valueOf(85_000));
    }
}    
```
4. Для каждого продукта добавим индекс релевантности:

```
        Map<Product, Double> relevantMap = new HashMap<>();
        relevantMap.put(product1, 2.0);
        relevantMap.put(product2, 0.1);
        relevantMap.put(product3, 0.2);
        relevantMap.put(product4, 3.0);
        relevantMap.put(product5, 1.0);
``` 

5. Создадим коллекцию `TreeSet` для автоматической сортировки объектов при вставке, для сортировки будем
использовать класс `ProductComparator`, который также нужно будет создать и передать в конструктор `TreeSet products`:

```
public class ProductComparator implements Comparator<Product> {

    Map<String, Double> relevantMap;

    public ProductComparator(Map<String, Double> relevantMap) {
        this.relevantMap = relevantMap;
    }

    @Override
    public int compare(Product o1, Product o2) {
        double rel1 = relevantMap.get(o1);
        double rel2 = relevantMap.get(o2);
        return Double.compare(rel1, rel2);
    }
}
```

Создание коллекции `TreeSet products` в методе `main`:

```
// reversed() – чтобы изменить направление сортировки, от болле релевантного к менее
ProductComparator comparator = new ProductComparator(relevantMap).reversed();
TreeSet<Product> products = new TreeSet<>(comparator);
```  

6. Заполним коллекцию `TreeSet<Product> products` ранее созданными `product1, product2, ...` (в методе main):

```
        products.add(product1);
        products.add(product2);
        products.add(product3);
        products.add(product4);
        products.add(product5);
```

7. Остался последний шаг — вывести топ 3 наиболее релевантных товара. Для этого напишем статичный метод
`public static void printTop3(TreeSet<Product> products)`, он будет принимать на вход множество товаров и печатать первые
три из списка в порядке их сортировки:

```
    public static void printTop3(TreeSet<Product> products) {
        Iterator<Product> iterator = products.iterator();
        System.out.println("Рекомендуем вам посмотреть также следующие товары:");
        for (int i = 0; i < 3; i++) {
            System.out.printf("%d. %s\n", i+1, iterator.next());
        }
    }
```

8. Вызовем в методе `main` метод `printTop3` и передадим в качестве аргумента коллекцию `TreeSet<Product> products`:

```
        printTop3(products);
```

9. Завершим работу программы.
