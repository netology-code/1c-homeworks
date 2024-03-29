# Задание к занятию «Запись и чтение регистров»

## Задача 1. «Установка цен»

### Описание задачи
Создать обработку для пакетного изменения цен номенклатуры.

### Требования к результату
Выгрузка информационной базы (.dt) с конфигурацией из предыдущих заданий с обработкой УстановкаЦен, которая содержит:
* таблицу значений и таблицу формы Номенклатура (с единственной колонкой Номенклатура);
* переключатель с тремя вариантами установки цен:
  * фиксированная цена с полем ввода цены рядом,
  * процент от текущей с полем ввода процента рядом,
  * удаление цен;
* кнопку и команду Установить, по которой обрабатываются цены номенклатуры из списка. Цены устанавливаются текущей датой. Изменение цены на процент должно исходить от последней цены и позволять как увеличить, так и уменьшить цену. Удаление должно удалять цены за любой период. Установка цен, фиксированной и процентом, должно оставлять историю нетронутой.
  
### Процесс выполнения

1. Создать обработку УстановкаЦен и создать её единственную и основную форму.
2. Добавить реквизит ВариантУстановки (Число) и перетащить его на форму, сделав полем переключателя. В список выбора добавить элементы:
  * 0 — Фиксированная цена;
  * 1 — Процент от текущей;
  * 2 — Удаление цен.
3. Добавить реквизиты ФиксированнаяЦена и ПроцентОтТекущей (Число). Перетащить их на форму как поля ввода.
4. Объединить поле переключателя и поля ввода в группы так, чтобы поля ввода были справа от соответствующих вариантов. Возможно, придётся изменить интервалы между элементами.
5. Реализовать обработчик события ПриИзменении переключателя ВариантУстановки так, чтобы доступным оставалось лишь поле ввода, соответствующее выбранному варианту. Сделать ПроцентОтТекущей недоступным по умолчанию, чтобы это соответствовало варианту по умолчанию (0).
6. Добавить таблицу значений Номенклатура с единственной колонкой Номенклатура и перетащить её на форму таблицей формы. Скрыть заголовок колонки, бессмысленный в этом случае.
7. Добавить команду и кнопку Установить, а в обработчике команды, вызвав серверную процедуру:
  * получить элементы справочника Номенклатура как выбранные непосредственно, так и находящиеся в выбранных пользователем группах. Далее, в зависимости от варианта:
    * Фиксированная цена — для каждого элемента создать запись в регистре Цены на текущую дату;
    * Процент от текущей — для каждого элемента прочитать записи и, найдя последнюю, изменить цену на указанный процент и создать запись на текущую дату;
    * Удаление цен — для каждого элемента записать пустой набор записей с отбором по нему. 
