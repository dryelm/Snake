## Баги

1. Есть странная логика что змейка может идти назад, например движемся вправо, нажимаем левую стрелку и игра заканичивается (если змейка достаточно длинная). В обычных змейках такого не встречал.

2. На экране закрытия не откатил каретку назад из-за чего Game Over пишется по середине. Можешь сказать что это фича :)

## Хотелки

1. Добавить управление на WASD

2. Хочется чтобы программа останавливалась сама, а не приходилось это делать в эмуляторе.  

## Качество кода

### Main

1. Два цикла while true?

2. `let key = Memory.peek(24576);` почему не Keyboard.keyPressed()?

3. `if (key = 130)` и т.п. Вижу что есть комменты что значит каждая кнопка, но предлагаю за отсутствием енамов выделить отдельный класс, который бы хранил нужное значение char. Не придется дописывать комменты. Например 

```Plain Text
class KeyboardValues{
  function char getLeftArrowValue(){
    return 130;
  }
  
  // и т.п.
}
```

1. `do board.Draw();` кажется стоит это унести в отдельный класс для рисования. Single-responsibility не выполняется. (Вообще я подумал про какую-то ничью сначала, так как ожидал что это класс с логикой игры, а не отображения)

2. В целом унес бы логику из Main.

### Board 

1. Методы должны быть с маленкькой буквы. [Это Java-подобный язык](https://google.github.io/styleguide/javaguide.html#s5.2.3-method-names). 

2. Как и все поля

3. Отступы на первой строчке new, Left, Right, Up, Down поехали, может где то еще поехали.

4. Немного придерусь, Почему не по порядку числа идут?)

```Plain Text
    method void Left(){
      let currentDirection = 3;
          return;
      }

    method void Right(){
      let currentDirection = 1;
          return;    
      }

    method void Up(){
      let currentDirection = 0;
          return;   
      }

    method void Down(){
      let currentDirection = 2;
          return;   
      }
```

1. В отдельный метод (класса Snake например)

```Plain Text
        let head = snake.getHead();
        if (currentDirection = 0){
            let newElement = QueueElement.new(head.getX(), head.getY() - 1, head);
        }

        if (currentDirection = 1){
            let newElement = QueueElement.new(head.getX() + 1 , head.getY() , head);
        }

        if (currentDirection = 2){
            let newElement = QueueElement.new(head.getX(), head.getY() + 1, head);
        }

        if (currentDirection = 3){
            let newElement = QueueElement.new(head.getX() - 1, head.getY(), head);
        }

```

1. Зачем тебе переменная tookCherry? Можно проще 

```Plain Text
        if(IsIntersect(CherryX, CherryY)){
            let tookCherry = true;
        }

        if(~(tookCherry)){
            do snake.Dequeue();
        }

        // почему не так
        if(~(IsIntersect(CherryX, CherryY))){
        }
```

1. Не знаю как в данный момент пофиксить, но кажется, что каждый раз перерисовывать границы игры не оптимально. Тоже поместить в отдельный метод.

2. Магические числа в `DrawCell`

3. `EnsureAllowed`- название неполное. Лучше `CheckIfPlayerLose` и т.п. И вынести логику по отображению проигрышного экрана в другой класс.

4. `if((x < 0) | (x > 39) | (y < 0) | (y > 24))` - 24 и 39 - магические переменные. Назови их как нибудь, сделай полем, инициализируй в конструкторе.

5. В целом непонятно почему board знает как устроена змейка и что под капотом там queue. Лучше вынести всю логику про змейку в отдельный класс Snake например. 

### Queue

1. В твоей реализации очереди операция Dequeue выполняется за O(n). Может добавить указатель на конец очереди и в элементах хранить предыдущий элемент в очереди?

2. Методы должны быть с маленкькой буквы. [Это Java-подобный язык](https://google.github.io/styleguide/javaguide.html#s5.2.3-method-names). 

3. Как и все поля

### QueueElement

1. Поля должны быть с маленкькой буквы. [Это Java-подобный язык](https://google.github.io/styleguide/javaguide.html#s5.2.3-method-names). 

### Random

1. `let x = y - ((y / m) * m);` Это взятие отстатка от деления. Можно вынести в класс MathOps или что нибудь такое.

2. Какие то непонятные названия переменных, может можно дать им осмысленные названия.



