## Задача 3. Крестики нолики

### Описание
Давайте напишем игру Крестики-нолики, для двух игроков.

### Функционал программы

1. Создайте поле 3х3;

2. После каждого хода в консоли выведите поле с прошедшими ходами;

3. Определите победителя. Выигрывает тот, кто первый составит три крестика или три нолика подряд.

# Пример

```
Игра "Крестики нолики"
Ход игрока 1, введите координаты Х (через пробел).
0 0
X   
   
   
Ход игрока 2, введите координаты O (через пробел).
1 1
X   
 O  
   
Ход игрока 1, введите координаты Х (через пробел).
2 0
X   
 O  
X   

Ход игрока 2, введите координаты O (через пробел).
0 2
X  O 
 O  
X   

Ход игрока 1, введите координаты Х (через пробел).
1 0
X  O 
X O  
X   

Выиграл X
Игра окончена!
``` 

### Пример реализации

1.  Создадим поле 3х3 для координат `X` и `O`, для хранения символа будем использовать тип `char`.

```
    char[][] motions = new char[3][3];
```

2. Создадим объект Scanner для ввода значений игроками.

```
    Scanner scanner = new Scanner(System.in);
```

3. Создадим цикл от 0 до 9 (так поле имеет всего 9 вариантов размещения значений).

```
    for (int i = 0; i < 9; i++) {
        System.out.println("Ход игрока 1, введите коодинаты Х (через пробел)");
        int x1 = scanner.nextInt();
        int y1 = scanner.nextInt();
        motions[x1][y1] = 'X';
```

4. После каждого ввода выведем поле, для это создадим отдельный метод.

```
   public static void printField(char[][] motions) {
        for (int i = 0; i < motions.length; i++) {
            for (int j = 0; j < motions[i].length; j++) {
                System.out.printf("%s ", motions[i][j]);
            }
            System.out.println();
        }
    }
```

5. После каждого ввода значений проверяем, выиграл игрок или нет. Для этого создадим отдельный метод для проверки
всех выигрышных вариантов (всего их 8):

```
    public static boolean checkStatus(char[][] motions) {
        //lines
        if (motions[0][0] != 0 && motions[0][0] == motions[0][1] && motions[0][1] == motions[0][2]) {
            System.out.printf("Выиграл %s\n", motions[0][1]);
            return true;
        }
        if (motions[1][0] != 0 && motions[1][0] == motions[1][1] && motions[1][1] == motions[1][2]) {
            System.out.printf("Выиграл %s\n", motions[1][0]);
            return true;
        }
        if (motions[2][0] != 0 && motions[2][0] == motions[2][1] && motions[2][1] == motions[2][2]) {
            System.out.printf("Выиграл %s\n", motions[2][0]);
            return true;
        }
        //rows
        if (motions[0][0] != 0 && motions[0][0] == motions[1][0] && motions[1][0] == motions[2][0]) {
            System.out.printf("Выиграл %s\n", motions[0][0]);
            return true;
        }
        if (motions[0][1] != 0 && motions[0][1] == motions[1][1] && motions[1][1] == motions[2][1]) {
            System.out.printf("Выиграл %s\n", motions[1][0]);
            return true;
        }
        if (motions[0][2] != 0 && motions[0][2] == motions[1][2] && motions[1][2] == motions[2][2]) {
            System.out.printf("Выиграл %s\n", motions[2][0]);
            return true;
        }
        //diagonals
        if (motions[0][0] != 0 && motions[0][0] == motions[1][1] && motions[1][1] == motions[2][2]) {
            System.out.printf("Выиграл %s\n", motions[0][0]);
            return true;
        }
        if (motions[0][2] != 0 && motions[0][2] == motions[1][1] && motions[1][1] == motions[2][0]) {
            System.out.printf("Выиграл %s\n", motions[0][0]);
            return true;
        }
        return false;
    }
```

6. Добавим ввод значений и проверку статуса для второго игрока:

```
    System.out.println("Ход игрока 2, введите коодинаты O (через пробел)");
    int x2 = scanner.nextInt();
    int y2 = scanner.nextInt();
    motions[x2][y2] = 'O';
    printField(motions);
    if (checkStatus(motions)) {
        break;
    }
```

7. Если игрок выиграл, выходим из цикла ввода данных и выведем сообщение о завершении игры:

```
    if (checkStatus(motions)) {
        break;
    }
    ...
    System.out.println("Игра закончена");
```

8. Выйдем из программы. 
