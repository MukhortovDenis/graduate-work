# GoPostCSS

## Содержание
* [Что такое AST](#ast)
* [AST на языке CSS](#AST-and-CSS)
* [Реализация AST в GoPostCSS](#ast-and-gopostcss)
* [Установка](#installing)
  * [Установка GoPostCSS](#installing-gopostcss)
  * [Добавление плагинов](#plugins)
* [Разработка](#development)
  * [Как работает GoPostCSS](#how-it-works)
  * [Создание плагина](#writing-new-plugin)

## AST

Для начала определение AST из [Wikipedia](https://ru.wikipedia.org/wiki/Абстрактное_синтаксическое_дерево).

`Дерево абстрактного синтаксиса (ДАС) — в информатике конечное помеченное ориентированное дерево, в котором внутренние вершины сопоставлены (помечены) с операторами языка программирования, а листья — с соответствующими операндами. Таким образом, листья являются пустыми операторами и представляют только переменные и константы.`
    
Нужно понимать, что данная формулировка не является панацеей в понимании AST. Многое зависит от устройства самого языка, в одном случае AST может быть подобным, если речь идет ООП языках, то в другом будет разным, если это язык таблицы стилей и язык того же ООП. В первую очередь, AST - это концепция, по которой можно раскладывать разные сущности.

Абстрактное синтаксическое дерево представляет код в ином виде, более структурированном и иерархическим. Подобный разбор сущностей дает разные преимущества, в контексте gopostcss - позволяет автоматически добавлять, заменять и удалять куски кода без собственного участия, тем самым ускоряя выполнение задачи и уменьшая возможность допустить ошибку. AST может быть разной сложности, одно может фиксировать наличие или отсутствие пробелом и пустых линий, другие же фокусируются только на конкретных свойствах и действиях.

В качестве конкретного примера советую рассмотреть [AST-Explorer](https://astexplorer.net). Там можно увидеть, как работают разные парсеры AST на разных языках.

## AST and CSS

CSS - это таблицы стилей, в которых определяются свойства. Его подход к AST отличается в значительной степени от других языков. Классическое AST в CSS является набором правил, в которых содержаться описанные в коде свойства. Рассмотрим на стандартном теге `а`:
```
a {
  text-decoration: none;
}
```

Так как AST таблиц стилей является набором правил, то одним из этих правил будет тег `a`, а свойством является `text-decoration: none;`. 

Описать в AST подобное можно в разной степени подробности. Стандартный парсер для `postcss` не только вычленяет информацию о селекторе, но и четко указывает, как он был написан изначально, сколько пробелов от начала строки у свойств, есть ли пустые строки, на какой строке и на каком по счету символе располагается тот или иной символ. Подобный парсер не является быстродействующим, зато предоставляет широкий функционал для других разработчиков. Четкое описание правила позволяет с помощью плагинов легко менять не только суть кода, но и его оформление, что особенно важно в тех местах, где нужен строго определенный синтаксис.

## AST and GoPostCSS

GoPostCSS на данный момент поддерживает синтаксис [stylelint](https://stylelint.io), строго определенный синтаксис ускоряет обработку CSS и дает возможность разработчикам простоту написания плагинов. К примеру, допустимый синтаксис:
```
a {
  color: #fff;
}
```
Сама AST представлена в виде обьекта, содержащего токены. Подход с токенами обусловлен более быстрой обработкой документов, в отличие от другого способа, поточной обработки.

Каждый токен является сущностью, это может быть селектор, импорт, комментарий и т.д. В токене в зависимости от его вида, могут находиться различные правила(в случае, если это селектор). Каждое правило содержит свой массив свойств. Таким образом, разработчику плагина достаточно заменить правила и свойства на свои в AST, чтобы новый CSS документ уже обладал ими.

### Installing
-----------------------

#### Installing GoPostCSS

#### Plugins

### Development
-----------------------

#### How it works

#### Writing new plugin
