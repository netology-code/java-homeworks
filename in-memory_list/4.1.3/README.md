# Задание к занятию 4.1 Коллекции List
## Задача 3. Email рассылка

### Описание
Напишем программу для массовой рассылки email сообщений.

### Функционал программы
1. Создание общего шаблона сообщения
2. Создание списка пользователей для рассылки, у каждого пользователя должны быть следующие поля:
    - Имя
    - Фамилия
    - email
3. Создание списка писем для рассылки с текстом из шаблона и адерсом назначения    
4. Вывод на экран списка писем для отправки

### Пример
```
mail to: ivan777@yandex.ru
Здравствуйте, Иван у нас для вас специальное предложение!

mail to: tatiana1@yandex.ru
Здравствуйте, Татьяна у нас для вас специальное предложение!

mail to: olga123@yandex.ru
Здравствуйте, Ольга у нас для вас специальное предложение!
```

### Процесс реализации
1. В методе main создадим переменную `messageTemplate` в которой сохраним общий для всех отправляемых
сообщений шаблон
```
    String textTemplate = "Здравствуйте, %s у нас для вас специальное предложение!";
```
2. Создадим класс `UserProfile` в котором создадим поля для хранения информации о пользователе
с полями: 
    - String surname (фамилия)
    - String name (имя)
    - String email
3. В методе main создадим несколько объектов из класса `UserProfile` и заполним данными
```
        UserProfile userProfile1 = new UserProfile("Иван", "ivan777@yandex.ru");
        UserProfile userProfile2 = new UserProfile("Татьяна", "tatiana1@yandex.ru");
        UserProfile userProfile3 = new UserProfile("Ольга", "olga123@yandex.ru");
```
4. Добавим только что созданные объекты в список `userProfiles`
```        
        List<UserProfile> userProfiles = new ArrayList<>();
        userProfiles.add(userProfile1);
        userProfiles.add(userProfile2);
        userProfiles.add(userProfile3);
```
5. Создадим класс почтовой рассылки `Mail` с полями:
    - String email (адрес назначения)
    - String message (сообщение составленное из шаблона)
6. Для удобства чтения данных из консоли пользователем, переопределим метод `toString` у класса `Mail`
```
    @Override
    public String toString() {
        return String.format("mail to: %s\n%s", email, message);
    }
```    
7. Создадим пустой список для будущей интернет рассылки `emailList`
```
    List<Mail> emailList = new ArrayList<>();
```
8. С помощью цикла `foreach` заполним созданный на передыдщем шаге список `emailList`, это список
будет хранить созданные на основе списка профилей пользователей `userProfiles` сообщения, нам
нужно взять из каждого профия имя пользователя и подставить его в шаблон
```
   for (UserProfile userProfile: userProfiles) {
       Mail email = new Mail(userProfile.getEmail(), String.format(textTemplate, userProfile.getName()));
       emailList.add(email);
   }    
```
9. Воспользуемся альтернативным способом перебора списков, с помощью итератора выведем все сообщения
на экран
```
   Iterator<Mail> emailListIterator = emailList.iterator();
   while (emailListIterator.hasNext()) {
       Mail email = emailListIterator.next();
       System.out.printf("%s\n\n", email);
   }
```
10. Завершим работу программы.