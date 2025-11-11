---
Share: true
---

Share: true

| **Unit**       | **Имя**                                        | **Описание**                                                                   |
| -------------- | ---------------------------------------------- | ------------------------------------------------------------------------------ |
| Ключевое слово | [Packed](../P/Packed.md)                       | Уплотняет сложные типы данных в минимальный объём памяти                       |
| Тип            | [PAnsiChar](../P/PAnsiChar.md)                 | Указатель на значение AnsiChar                                                 |
| Тип            | [PAnsiString](../P/PAnsiString.md)             | Указатель на значение AnsiString                                               |
| Функция        | [ParamCount](../P/ParamCount.md)               | Выдает число параметров переданной текущей программе                           |
| Функция        | [ParamStr](../P/ParamStr.md)                   | Возвращается один из параметров используемых для запуска текущей программы     |
| Тип            | [PChar](../P/PChar.md)                         | Указатель на значение символа (Char)                                           |
| Тип            | [PCurrency](../P/PCurrency.md)                 | Указатель на значение Валюты (Currency)                                        |
| Тип            | [PDateTime](../P/PDateTime.md)                 | Указатель на значение TDateTime                                                |
| Тип            | [PExtended](../P/PExtended.md)                 | Указатель на значение Extended с плавающей запятой                             |
| Функция        | [Pi](../P/Pi.md)                               | Математическая константа                                                       |
| Тип            | [PInt64](../P/PInt64.md)                       | Указатель на значение Int64                                                    |
| Функция        | [Point](../P/Point.md)                         | Генерирует значение TPoint из значений X и Y                                   |
| Тип            | [Pointer](../P/Pointer.md)                     | Определяет общее использование указателя на любые данные, основанные на памяти |
| Функция        | [PointsEqual](../P/PointsEqual.md)             | Сравнивает два значения TPoint на предмет равенства                            |
| Функция        | [Pos](../P/Pos.md)                             | Находит позицию одной строки в другой                                          |
| Функция        | [Pred](../P/Pred.md)                           | Уменьшает порядковую переменную                                                |
| Функция        | [Printer](../P/Printer.md)                     | Возвращает ссылку к глобальному объекту Printer                                |
| Деректива      | [Private](../P/Private.md)                     | Начинает частный (Private) раздел данных и методов в классе                    |
| Ключевое слово | [Procedure](../P/Procedure.md)                 | Определяет подпрограмму, которая не возвращает значение                        |
| Процедура      | [ProcessPath](../P/ProcessPath.md)             | Разделяет строку диск/путь/имя файла на ее составляющие части                  |
| Ключевое слово | [Program](../P/Program.md)                     | Определяет начало приложения                                                   |
| Функция        | [PromptForFileName](../P/PromptForFileName.md) | Показывает диалог, позволяющий пользователю выбрать файл                       |
| Ключевое слово | [Property](../P/Property.md)                   | Определяет управляемый доступ к полям класса                                   |
| Деректива      | [Protected](../P/Protected.md)                 | Начинает раздел класса частных данных доступных подклассам                     |
| Тип            | [PShortString](../P/PShortString.md)           | Указатель на значение ShortString                                              |
| Тип            | [PString](../P/PString.md)                     | Указатель на String значение                                                   |
| Функция        | [PtInRect](../P/PtInRect.md)                   | Проверяет, находится ли **точка** в пределах **прямоугольника**                |
| Деректива      | [Public](../P/Public.md)                       | Начинает внешне доступный раздел класса                                        |
|                |                                                |                                                                                |
|                |                                                |                                                                                |








```base
views:
  - type: table
    name: Table
    filters:
      and:
        - and:
            - file.folder.endsWith("P")
            - and:
                - file.folder.contains("Functions & Procedures")
        - and:
            - file.name != "_blanck"
    order:
      - file.name
      - Unit
      - Description
    columnSize:
      note.Unit: 124
    rowHeight: medium

```