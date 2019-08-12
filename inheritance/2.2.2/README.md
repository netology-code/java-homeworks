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

Данный функционал пригодится в случае массовой фильтрации книг по какому-то искомому статусу.

Также необходимо будет описать класс `Book` с базовым набором полей, состоящим из 'title' и списка жанров, и класс `BookService`, в котором будет осуществляться фильтрация книг.

### Дополнительная информация
В задаче рекомендуется использование класса коллекции List<>, более подробный обзор которого будет в следующих лекциях. Назначение этого класса — хранить пронумерованный список объектов одного типа или его производных, указанного в угловых скобках (например List<String> list – объявление списка строк). В пункте "процесс реализации" дан пример работы со списком, его создания и наполнения элементами и перебора всех элементов списка.

### Процесс реализации
1. Создайте Enum класс `GenreEnum` с 8 возможными жанрами в нашей программе.
```
public enum GenreEnum {
    STORY,NOVEL,NARRATIVE,PROSE,VERSE,FANTASTIC,DETECTIVE,PROFESSIONAL
}
```
2. Создайте класс `Genre` с protected полем `attribute` типа String, конструктором, принимающим один аргумент, и двумя методами. 
```
    public String getAttributeOfGenre() {
            return attribute;
    }
    
    public String getGenreName() {
            return "Some genre name";
    }
```
3. Создайте трёх наследников класса Genre. 
Например: `GenreByContent` - класс с конструктором, вызывающий конструктор предка. В нем обязательно необходимо переопределить метод `equals` класса `Object`.
```
public class GenreByContent extends Genre {
    public GenreByContent() {
            super("Content of the text");
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
Сделайте самостоятельно оставшиеся два класса `GenreByForm` и `GenreByNumberOfPages` с собственным значением поля 'attribute', присущим данной группе жанров.

4. Создайте наследников каждого из классов групп жанров. В них необходимо переопределить только метод `getGenreName`, конструктор предка будет вызван по умолчанию.
Например:
```
public class DetectiveGenre extends GenreByContent {

    @Override
    public String getGenreName() {
        return GenreEnum.DETECTIVE.name();
    }
}
```
и 
```
public class FantasticGenre extends GenreByContent {

    @Override
    public String getGenreName() {
        return GenreEnum.FANTASTIC.name();
    }
}
```

Создайте остальных наследников согласно диаграмме.

5. Добавьте в класс Book атрибут — список жанров (genres).

```
import java.util.List;

public class Book {
    private String title;
    private List<Genre> genres;

    public Book(String title, List<Genre> genres) {
        this.title = title;
        this.genres = genres;
    }

    //for creating new Book
    public Book(String title) {
        this.title = title;
    }

    public List<Genre> getGenres() {
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

6. Создайте сервис `BookService`, в котором можно будет отфильтровать книги.

```
import java.util.ArrayList;
import java.util.List;

public class BookService {
    private List<Book> bookList;

    public void setBookList(List<Book> bookList) {
        this.bookList = bookList;
    }

    public void filterBookByGenre(Genre genre) {
        for (Book book : bookList) {
            for (Genre genreFromBook : book.getGenres()) {
              if (genreFromBook.getAttributeOfGenre().equals(genre.getAttributeOfGenre())) {
                if (genreFromBook.equals(genre)) {
                    System.out.println("Книга - " + book.getTitle() + " подходит под данный фильтр: жанр - " + genre.getGenreName());
                    break;
                } else {
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

7. В классе Main.java создайте объект класса Book, используя конструктор, и убедитесь, что функция фильтрации была реализована верно. Например:

```
   import java.util.Arrays;
   import java.util.List;
   .....
   BookService bookService = new BookService();
   Book book = new Book("Lord of the Rings", Arrays.asList(new StoryGenre(), new ProseGenre(), new FantasticGenre()));
   bookService.setBookList(Arrays.asList(book));
   
   bookService.filterBookByGenre(new StoryGenre());
   bookService.filterBookByGenre(new DetectiveGenre());
   
   bookService.filterBookByGenre(new NarrativeGenre());
   bookService.filterBookByGenre(new VerseGenre());
```
