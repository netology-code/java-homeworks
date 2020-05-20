## Задача 4. Каталог патентов

### Описание
Напишем программу создания каталога патентов по годам, патенты описаны во внешнем файле.

### Функционал программы
1. Построчное чтение и разбор данных из внешнего файла `patents.csv`;
2. Вывести список патентов с детальной информацией, сгруппированных по годам. Для этого используйте TreeMap.

### Пример
```
Каталог патентов
Год: 1981
		номер: 567567556, компания: apple, автор: С. Джобс, дата: 1981, описание: Персональный копьютер
Год: 2003
		номер: 765565464, компания: apple, автор: С. Джобс, дата: 2003, описание: Музыкальный плеер ipod
Год: 2008
		номер: 456456456, компания: apple, автор: С. Джобс, дата: 2008, описание: Корпус моноблок, для компьютера
		номер: 42543534534, компания: google, автор: С. Брин, дата: 2008, описание: Оснащение пользовательского интерфейса расширением поисковых запросов
		номер: 653452242, компания: google, автор: С. Брин, дата: 2008, описание: Способ сегментации текста
Год: 2016
		номер: 5353534533, компания: amazon, автор: М. Корддрайу, дата: 2016, описание: Управление главными вычислительными устройствами
Год: 2017
		номер: 2423423, компания: yandex, автор: И. Кураленок, дата: 2017, описание: Система и способ уточнения результатов поиска
Год: 2018
		номер: 234242342424, компания: yandex, автор: Ю. Зеленков, дата: 2018, описание: Способ и система автоматического создания тезауруса
		номер: 53453234, компания: amazon, автор: Г. Рот, дата: 2018, описание: Формирование ключа в зависимости от параметра
Год: 2019
		номер: 5365453242, компания: yandex, автор: В. Шарандо, дата: 2019, описание: Устройство для доставки товара
		номер: 65635353, компания: amazon, автор: Т. Сигнетти, дата: 2019, описание: Многофункциональная идентификация виртуального вычислительного узла
```

### Реализация
1. Посмотрим файл `patents.csv`, в нем содержатся данные о патентах. В начале файла есть заголовок, который
описывает данные в этом файле `номер;компания;автор;дата;описание;`. Все поля разделены `;` друг от друга.

Создадим класс `Patent` для описания такой структуры данных тем же количеством полей, что и в файле:

```
    - String number;
    - String company;
    - String author;
    - int year;
    - String description;
```

Создадим конструктор со всеми параметрами, чтобы заполнить все поля при создании объекта:

```java
    public Patent(String number, String company, String author, int year, String description) {
        this.number = number;
        this.company = company;
        this.author = author;
        this.year = year;
        this.description = description;
    }
```

Переопределим метод `toString` для простоты вывода данных в консоль:

```java
    @Override
    public String toString() {
        return String.format("номер: %s, компания: %s, автор: %s, дата: %s, описание: %s", number, company, author,
                year, description);
    }
```

2. Создадим класс `Main` для чтения в нем файла и создания метода `public static void main(String[] args)`:

```java
    class Main {

    }
  ``` 

3. Для чтения данных из файла создадим метод `parsePatentsData(String filename)`. Этот метод будет принимать на вход
имя файла, в нашем случае имя файла `patents.csv`. Возвращать этот метод будет карту `Map`, где в качестве ключа значений
будет использоваться год, а в качестве значений — множество из патентов, опубликованных в этом году `(Set<Patent>)`.

```java
    private static Map<Integer, Set<Patent>> parsePatentsData(String filename) throws FileNotFoundException {
        File file = new File(filename);
        Scanner scanner = new Scanner(file);

        Map<Integer, Set<Patent>> patentsMap = new TreeMap<>();
        scanner.nextLine();
        while (scanner.hasNext()) {
            String[] data = scanner.nextLine().split(";");
            Patent patent = new Patent(data[0], data[1], data[2], Integer.parseInt(data[3]), data[4]);
            if (patentsMap.get(patent.getYear()) != null) {
                Set<Patent> patentsSet = patentsMap.get(patent.getYear());
                patentsSet.add(patent);
            } else {
                Set<Patent> patentsSet = new HashSet<>();
                patentsSet.add(patent);
                patentsMap.put(patent.getYear(), patentsSet);
            }
        }
	scanner.close();
        return patentsMap;
    }
``` 

4. Напишем метод для вывода прочитанных значений из файла:

```java
    private static void printPatents(Map<Integer, Set<Patent>> patentsMap) {
        System.out.println("Каталог патентов");
        for (Entry<Integer, Set<Patent>> entry : patentsMap.entrySet()) {
            System.out.printf("Год: %d\n", entry.getKey());
            for (Patent patent: entry.getValue()) {
                System.out.printf("\t\t%s\n", patent.toString());
            }
        }
    }
``` 

5. В методе `main` вызовем методы для чтения из файла и вывод патентов на экран:

```java
    public static void main(String[] args) throws FileNotFoundException {
        String filename = "src/patents.csv";
        Map<Integer, Set<Patent>> patentsMap = parsePatentsData(filename);
        printPatents(patentsMap);
    }
```

6. Завершим работу программы.
