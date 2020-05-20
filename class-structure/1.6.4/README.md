## Задача 4. Кинорежиссер

### Описание
Напишем программу кратко описывающую фильмографию режиссеров.
Для этого нам нужно будеть создать свои классы описывающие фильмы (Film) и их режиссеров (Director), а потом связать эти классы между собой.

### Функционал программы
1. Создать класс Film;
2. Создать класс Director;
3. Добавить свои конструкторы для каждого из классов и связать их между собой.

### Пример реализации
1. Создадим класс Film c полями с полями (воспользуемся новым типом данных LocalDate - тип даты без времени):
```java
    LocalDate release;  // дата выпуска
    String title;       // название фильма
    String[] countries; // страны
```
2. Добавим конструктор для этого класса с возможностью заполнения всех полей объекта при его создании.
```java
    public Film(LocalDate release, String title, String[] countries) {
        this.release = release;
        this.title = title;
        this.countries = countries;
    }
```
3. Создадим еще один класс Director (режиссер) с полями
```java
    LocalDate birthday; // дата рождения 
    String name;        // имя
    String surname;     // фамилия
    Film [] films;      // массив срежессированных фильмов
```
4. Создадим конструктор для класса Director с возможностью заполнить все поля
```java
    public Director(LocalDate birthday, String name, String surname, Film[] films) {
        this.birthday = birthday;
        this.name = name;
        this.surname = surname;
        this.films = films;
    }
```
5. Добавим класс Main для запуска нашей программы, создадим в методе main несколько объектов типа Film (фильмов Гая Ричи), а так же информацию
о режиссере - объект типа Director (информация о режиссере Гай Ричи). Для разбора дать воспользуемся методом класса `LocalDate.parse`
```java
        Film film1 = new Film(LocalDate.parse("1998-10-10"), "Карты, деньги, два ствола", new String[]{"Великобритания"});
        Film film2 = new Film(LocalDate.parse("2008-01-02"), "Рок-н-рольщик", new String[]{"США", "Великобритания"});

        Director director = new Director(LocalDate.parse("1968-09-10"), "Гай", "Ричи", new Film[]{film1, film2});
```
6. Выведем созданные объекты в консоль и убедимся в работе программы, как видите мы связали объекты классов Film и Director
```java
        System.out.printf("Режиссер: %s %s, дата рождения: %s", director.name, director.surname, director.birthday);
        System.out.println("   Фильмы:");
        for (Film film: director.films) {
            System.out.printf("   %s %s%n", film.title, film.release);
            for (String county: film.countries) {
                System.out.printf("      %s%n", county);
            }
        }
```
