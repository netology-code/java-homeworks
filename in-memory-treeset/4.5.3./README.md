## Задача 3.  Реккомендация товаров

### Описание
Напишем программу для вывода наиболее релевантного товара.

### Функционал программы
1. Созадние списка/множества товаров с полями (String name и BigDecimal cost);
2. Создание карты (map) для расстановки релевантности каждого из товаров (String name, double relevant);
3. Вывод топ 3 наиболее релевантных продукта.

### Пример
```
Рекомендуем Вам посмотреть так же следующие товары:
1. Наименование: lg tv, цена: 50000
2. Наименование: samsung tv, цена: 55000
3. Наименование: apple iphone, цена: 85000
```

### Реализация
1. Создадим класс продукт - `Product` как написано в условии программы, со следующими полями:
  - String name;
  - BigDecimal cost;
2. Переопределим метод `toString` - чтобы был читаемый вывод информации о продукте, так же добавим всем полям класса 
`Product` метод `get` чтобы можно было получать значения и добавим конструтор для присвоения значейни обоим полям во 
время создания объекта
```
    public Product(String name, BigDecimal cost) {
        this.name = name;
        this.cost = cost;
    }

    @Override
    public String toString() {
        return String.format("Наименование: %s, цена: %s", name, cost);
    }
```
3. Создадим класс `Main` - в нем будем создавать объекты класса продукт и вызывать остальные методы в основном
методе `main`
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
4. Для каждого продукта добавим индекс релевантности
```
        Map<String, Double> relevantMap = new HashMap<>();
        relevantMap.put(product1.getName(), 2D);
        relevantMap.put(product2.getName(), 0.1D);
        relevantMap.put(product3.getName(), 0.2D);
        relevantMap.put(product4.getName(), 3D);
        relevantMap.put(product5.getName(), 1D);
``` 
5. Создадим коллекцию `TreeSet` для автоматической сортировки объектов при вставке, для сортировки будем
использоват в класс `ProductComparator`, который так же нужно будет создать и передать в конструтор `TreeSet products`
```
public class ProductComparator implements Comparator<Product> {

    Map<String, Double> relevantMap;

    public ProductComparator(Map<String, Double> relevantMap) {
        this.relevantMap = relevantMap;
    }

    @Override
    public int compare(Product o1, Product o2) {
        double product1 = relevantMap.get(o1.getName());
        double product2 = relevantMap.get(o2.getName());
        if (product1 > product2) {
            return 1;
        } else if (product1 == product2) {
            return 0;
        }
        return -1;
    }
}
```
Создание коллекции `TreeSet products` в методе `main`
```
Set<Product> products = new TreeSet<>(new ProductComparator(relevantMap).reversed());
```  
6. Заполним коллекцию `Set<Product> products` ранее созданными `product1, product2, ...` (в методе main)
```
        products.add(product1);
        products.add(product2);
        products.add(product3);
        products.add(product4);
        products.add(product5);
```
7. Остался последний шаг, это вывести топ 3 наиболее релевантных товара. Для этого напишем статичный метод
`public static void printTop3(Set<Product> products)` - он будет принимать на вход множество товаров и печатать первые
три из списка в порядке их сортировки.
```
    public static void printTop3(Set<Product> products) {
        Product[] productsArray = products.toArray(new Product[0]);
        System.out.println("Рекомендуем Вам посмотреть так же следующие товары:");
        for (int i = 0; i < 3; i++) {
            System.out.printf("%d. %s\n", i+1, productsArray[i]);
        }
    }
```
8. Вызовем в методе `main` метод `printTop3` и передадим в качестве аргумента коллекцию `Set<Product> products`
```
        printTop3(products);
```
9. Завершим работу программы.