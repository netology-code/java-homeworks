## Задача 2. Иерархия "Жанры книг"

### Описание
Продолжим рассматривать предметную область онлайн-библиотеки. 
В нашей библиотеке есть множество жанров книг. Часть из них можно объединить в группы и переиспользовать их функционал вместо того, чтобы писать его заново.

Реализуйте иерархию жанров книг с помощью наследования. В ней должны быть представлены три группы жанров: 

→ по объему текста (повесть/роман/рассказ); 

→ по форме текста (проза/стихи); 

→ по содержанию (фантастика/детектив/проф.литература).

Диаграмма жанров указана здесь: 
![](https://i.imgur.com/Ui1FqtU.jpg)

Необходимо с помощью наследования реализовать программу, в которой будет один базовый класс `Genre`, три наследника базового класса 
(`GenreByContent`, `GenreByForm`, `GenreByNumberOfPages`), в которых будут определены атрибуты каждой группы жанров, 
и на каждый класс группы жанров — по несколько классов, в которых будет определено конкретное название жанра.

Данный функционал пригодится в случае массовой фильтрации книг по какому-то искомому статусу (в нашем случае по жанру).

Также необходимо будет описать класс `Book` с базовым набором полей, состоящим из `title` и списка жанров, и класс `BookService` с методом `filterBookByGenre`, в котором будет осуществляться фильтрация книг.

Общая идея следующая: у каждой книги может быть множество жанров разных групп одновременно, мы хотим научиться отбирать книги по нужному
нам жанру.

### Процесс реализации
1. Создайте Enum класс `GenreEnum` с 8 возможными жанрами в нашей программе.
```
public enum GenreEnum {
    STORY,
    NOVEL,
    NARRATIVE,
    PROSE,
    VERSE,
    FANTASTIC,
    DETECTIVE,
    PROFESSIONAL
}
```
2. Создайте класс `Genre` с `protected` полем `attribute` типа `String`, конструктором, принимающим один аргумент, и двумя методами. 
```
    public String getAttributeOfGenre() {
            return attribute;
    }
    
    public String getGenreName() {
            return "Жанр не указан";
    }
```
3. Создайте трёх наследников класса `Genre`. 
    - `GenreByContent` - класс с конструктором, вызывающий конструктор предка `Genre`. В нем обязательно необходимо переопределить метод `equals` класса `Object` - это необходимо, что бы в дальнейшем корректно сравнивать жанры.
```
public class GenreByContent extends Genre {
    public GenreByContent() {
            super("Жанр по контенту книги"); //присвоим уникальный атрибут одному из трех базовых жанров
    }

    @Override
    public boolean equals(Object object) {
        if (this == object) return true;
        if (object == null || getClass() != object.getClass()) return false;

        GenreByContent genreByContent = (GenreByContent) object;

        return attribute != null ? attribute.equals(genreByContent.attribute) : false;
    }
}
```
Сделайте самостоятельно оставшиеся два класса (так же наследуемых от Genre):
    
    - class GenreByForm extends Genre, со значением поля attribute - Жанр по форме текста.
    - class GenreByNumberOfPages extends Genre со значением поля attribute - Жанр по количеству страниц.

4. Создайте наследников каждого из классов групп жанров, а именно для каждого из базовых классов, созданных на предыдущем шаге: `GenreByContent`, `GenreByForm`, `GenreByNumberOfPages` нужно создать классы наследники. 

Для `GenreByContent` - `FantasticGenre`, `DetectiveGenre`, `ProfessionalGenre`.

Для `GenreByForm` - `ProseGenre`, `VerseGenre`.

Для `GenreByNumberOfPages` - `StoryGenre`,`NovelGenre`,`NarrativeGenre`.

В каждом классе наследнике нужно переопределить метод `String getGenreName()` и, используя ранее созданное перечисление `enum GenreEnum`, возвращать нужное значение из перечисления в этом методе (для того чтобы значение `enum` возвращал строку - имя нашего жанра, нужно вызвать метод `name()`, как указано в примере: `GenreEnum.DETECTIVE.name()`).

Пример создания жанра детектив:
```
public class DetectiveGenre extends GenreByContent {

    @Override
    public String getGenreName() {
        return GenreEnum.DETECTIVE.name();
    }
}
```
Пример создания жанра фантастика: 
```
public class FantasticGenre extends GenreByContent {

    @Override
    public String getGenreName() {
        return GenreEnum.FANTASTIC.name();
    }
}
```

Создайте остальных наследников согласно диаграмме (`ProseGenre`, `VerseGenre`, `StoryGenre`,`NovelGenre`,`NarrativeGenre`), используя в качестве базовых классов `GenreByNumberOfPages` и `GenreByForm`.

На этом этапе создание жанров завершено.

5. Создадим класс книги - `Book`, который будет использовать любые наши реализации жанров (`ProseGenre`, `VerseGenre`, `StoryGenre`,`NovelGenre`,`NarrativeGenre`, `DetectiveGenre`, `FantasticGenre`, `ProfessionalGenre`), для этого в классе `Book` нужно создать поле с типом массив `Genre[] genres`.

```
public class Book {
    private String title;
    private Genre[] genres;

    public Book(String title, Genre[] genres) {
        this.title = title;
        this.genres = genres;
    }

    //for creating new Book
    public Book(String title) {
        this.title = title;
    }

    public Genre[] getGenres() {
        return genres;
    }

    public String getTitle() {
        return title;
    }

    public String toString(){
        return this.title;
    }
}
```

6. Создайте сервис `BookService`, в котором можно будет отфильтровать книги с помощью метода `filterBookByGenre`. Этот класс больше ничего не будет делать, кроме того, что выводить на экран какая из книг соответствует фильтру, а какая нет.

```
public class BookService {

    //bookList - список книг, genre - жанр для фильтрации (поиска соответствия)
    public void filterBookByGenre(Book[] bookList, Genre genre) {
        for (Book book : bookList) { //перебираем все книги по очереди
            for (Genre genreFromBook : book.getGenres()) { //у каждой книги перебираем всю коллекцию жанров, которыми она обладает
              if (genreFromBook.getAttributeOfGenre().equals(genre.getAttributeOfGenre())) { //если базовый тип соотвествует базовому типу жанра, который мы передали в качестве аргумента методу, то переходим к следующей проверке
                if (genreFromBook.equals(genre)) { //если жанр соответствует искомому, значит книга подходит по жанру
                    System.out.println("Книга - " + book.getTitle() + " подходит под данный фильтр: жанр - " + genre.getGenreName());
                    break;
                } else { //книга не прошла фильтр
                    System.out.println("Книга - " + book.getTitle() + " не прошла фильтр: жанр - " + genre.getGenreName() + ", критерий- " + 
                    genre.getAttributeOfGenre() + ", так как имеет жанр " +
                    genreFromBook.getGenreName());
                }
              }  
            }
        }
    }
}
```

7. В классе `Main.java` создайте объект/объекты класса `Book`, используя конструктор, и убедитесь, что функция фильтрации была реализована верно. Например:

```
   public static void main(String[] args) {
        //Создадим первую книгу с тремя жанрами
        Book book1 = new Book("Властелин колец", new Genre[] {new StoryGenre(), new ProseGenre(), new FantasticGenre())};
        //Создадим вторую книгу с двумя жанрами
        Book book2 = new Book("Шерлок Холмс", new Genre[] {new NovelGenre(), new DetectiveGenre())};

        //Создадим объект `BookService` - для фильтрации
        BookService bookService = new BookService();
        
        //Вызовем метод фильтрации, куда передадим список книг и жанр фильтрации в качестве аргументов
        bookService.filterBookByGenre(new Book[]{book1, book2}, new StoryGenre());
        bookService.filterBookByGenre(new Book[]{book1, book2}, new DetectiveGenre());
        bookService.filterBookByGenre(new Book[]{book1, book2}, new NarrativeGenre());
        bookService.filterBookByGenre(new Book[]{book1, book2}, new VerseGenre());
   }
```
8. Протестируем работу программы.
