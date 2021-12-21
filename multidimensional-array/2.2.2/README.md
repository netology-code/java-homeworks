# Домашнее задание к занятию "Массивы многомерные"
## Задача 2. Дописываем крестики-нолики

### Описание
Возьмём за основу игру крестики-нолики с вебинара и допишем метод проверки победы одного из игроков, переписав его на циклы. Так он будет работать при любом значении `SIZE`.

### Образ результата
1. Вы дописываете метод `isWin` так, что он работает для любого значения `SIZE`
2. Удаляете содержимое метода `main`, заполняете его кодом, запуск которого демонстрирует правильную работу вашего метода `isWin` на разных примерах (пример вывода программы дан ниже); 
3. На проверку отправляете реплит, в котором есть статическое поле `SIZE` со значением 5 (остальные статические поля оставляете как есть), метод `isWin` и метод `main`, в котором вы демонстрируете работу этого метода на разных видах полей; остальные методы и код можно удалить.
4. Общаться с пользователем (спрашивать у него что-то, делать ходы и тп) не нужно

### Пример вывода при запуске main
```
ДЕМОНСТРАЦИЯ

O O O - -
X X X X X
X O X O X
O O - O X
O - O X X
ПОБЕДИЛИ КРЕСТИКИ

X O - - -
- X O - -
X - X O -
O O - X -
O - - X X
ПОБЕДИЛИ КРЕСТИКИ

O O O O O
X X X - -
X - X X X
O - - - X
O - - - -
ПОБЕДИЛИ НОЛИКИ

X O X O X
O X O X O
- - X - -
- - - - -
- - - - -
НИКТО НЕ ПОБЕДИЛ
```

### Код игры в крестики-нолики
```java
import java.util.Scanner;

public class Main {
    public static final int SIZE = 3;
    public static final char EMPTY = '-';
    public static final char CROSS = 'X';
    public static final char ZERO = 'O';

    public static void main(String[] args) {
        char[][] field = new char[SIZE][SIZE];
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                field[i][j] = EMPTY;
            }
        }

        Scanner scanner = new Scanner(System.in);

        boolean isCrossTurn = true;

        while (true) {
            printField(field);
            System.out.println("Ходят " + (isCrossTurn ? "крестики" : "нолики") + "!");
            String input = scanner.nextLine(); // "2 3"
            String[] parts = input.split(" "); // ["2" , "3"]
            int r = Integer.parseInt(parts[0]) - 1; // 2-1 = 1
            int c = Integer.parseInt(parts[1]) - 1; // 3-1 = 2

            if (field[r][c] != EMPTY) {
                System.out.println("Сюда ходить нельзя");
                continue;
            }

            field[r][c] = isCrossTurn ? CROSS : ZERO;
            if (isWin(field, isCrossTurn ? CROSS : ZERO)) {
                printField(field);
                System.out.println("Победили " + (isCrossTurn ? "крестики" : "нолики"));
                break;
            } else {
                if (isCrossTurn) {
                    isCrossTurn = false;
                } else {
                    isCrossTurn = true;
                }
                //isCrossTurn = !isCrossTurn;
            }
        }

        System.out.println("Игра закончена!");
    }

    // !!ВНИМАНИЕ!!
    // Работает только для 3x3
    // Этот метод вам и надо переписать
    public static boolean isWin(char[][] field, char player) {
        if (field[0][0] == player && field[0][1] == player && field[0][2] == player)
            return true;
        if (field[1][0] == player && field[1][1] == player && field[1][2] == player)
            return true;
        if (field[2][0] == player && field[2][1] == player && field[2][2] == player)
            return true;

        if (field[0][0] == player && field[1][0] == player && field[2][0] == player)
            return true;
        if (field[0][1] == player && field[1][1] == player && field[2][1] == player)
            return true;
        if (field[0][2] == player && field[1][2] == player && field[2][2] == player)
            return true;

        if (field[0][0] == player && field[1][1] == player && field[2][2] == player)
            return true;
        if (field[2][0] == player && field[1][1] == player && field[0][2] == player)
            return true;

        return false;
    }

    public static void printField(char[][] field) {
        for (char[] row : field) {
            for (char cell : row) {
                System.out.print(cell + " ");
            }
            System.out.println();
        }
    }
}
``` 

### Советы по реализации
1. Для начала всмотритесь в этот метод и найдите повторяющиеся закономерности. Если вы плохо себе представляете какие ячейки тут проверяются, нарисуйте поле на листочке и посмотрите на клетки поля каждой строки кода на рисунке.
2. Рассмотрим на примере проверки заполненности строк. Мы видим, что первые три условные оператора повторятся с точностью до первого индекса (номер строки), поэтому мы можем сделать цикл:
```java
for (int row = 0; row < SIZE; row++) { // обратите внимание, что мы поставили SIZE, а не 3, чтобы работало с любым значением
    if (field[row][0] == player && field[row][1] == player && field[row][2] == player)
        return true;
}
```
3. Того что мы сделали недостаточно, тк если в строке будет больше трёх клеток, то наш `if` всё равно проверит лишь первые три. Для этого и условный оператор надо переписать на вложенный цикл. Идея его такова: заведём переменную для подсчёта количества `player` в строке, изначально она будет 0. Пробежимся по всем клеткам строки, для каждой клетки, если там `player`, увеличим эту переменную на 1. После цикла по клеткам строки, сравним эту переменную с `SIZE`. Если она не будет равна `SIZE`, значит не во всех клетках был `player`; если будет равна, значит `player` победил и возвращаем `true`.
4. На проверку предоставляем только метод `isWin` с демонстрацией его работоспособности в `main`.
