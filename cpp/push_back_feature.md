# Как написать push_back

Этим, начиная с основ, мы занимались вот 
[тут](https://github.com/dasfex/ProgrammingNotes/blob/master/cpp/topics/new_delete.md) 
, 
[тут](https://github.com/dasfex/ProgrammingNotes/blob/master/cpp/topics/allocators.md)
и
[тут](https://github.com/dasfex/ProgrammingNotes/blob/master/cpp/topics/move_semantics.md).

Уточним ещё один момент для идеальной реализации.

Что делает ```push_back``` на данный момент?
Если требуется, реаллоцирует память, перекладывает элементы 
в новое место и конструирует новый объект.

Так вот это не совсем верно.
Настоящий ```push_back``` делает вот так:
он сначала кладёт новый элемент на его место, а только потом 
перекладывает остальные элементы, если требуется.

Зачем это нужно?

Что будет, если написать ```v.push_back(v.back())```?
Если так окажется, что при текущем добавлении нужно изменять capacity,
после всех перекладываний элементов в новое место ```v.back()```
окажется битой ссылкой на место в старом массиве.
Потому надо сначала сконструировать то, что попросили, а потом переложить.
