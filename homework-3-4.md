# Команды

## Задание 1

### Описание задачи

Создать общую группу команд "Печать" и два варианта печати "Сразу на принтер" и "Предварительный просмотр"

### Требование к результату

Добавить в конфигурацию из задания 3.1 общую команду печати для объектов "Заказ покупателя" и "Заказ поставщику". Команды должны находиться в общем подменю "Печать".
При вызове команды "Сразу на принтер" должно выводиться сообщение "Печать объекта <представление объекта> В разработке. Здесь будет команда вывода на принтер".
При вызове команды "Предварительный просмотр" должно выводиться сообщение "Печать объекта <представление объекта> В разработке. Здесь будет команда предварительного просмотра".

### Процесс выполнения

1. Возьмите конфигурацию из задания 3.1
2. В ветке "Общие/Общие команды" добавьте группу команд "Печать" категории "Командная панель формы".
3. Добавьте 2 общие команды "ПечатьНаПринтер" и "ПечатьСПредпросмотром" и поместите их в группу команд "Печать"
4. В качестве типа параметра обеих команды укажите документы "Заказ покупателя" и "Заказ поставщику". Не забудьте установить флажок "Составной тип" в окне выбора типа, иначе можно будет выбрать только один документ
5. В качестве значения поля "Режим использования параметра" в свойствах команды укажите "Одиночный"
6. В каждой команде напишите код процедуры ОбработкаКоманды так, чтобы она выводила требуемое сообщение. Обратите внимание, объект, который мы печатаем будет передан в процедуру ОбработкаКоманды в параметре "ПараметрКоманды"


## Задание 2

### Описание задачи

Создать команду, открывающую формы объектов с заданными параметрами.

### Требование к результату

Группа команд "Быстрый доступ" отображающая список справочника "Сотрудники" с фильтрацией по полю "Уволен". Команда "Уволенные" открывает список сотрудников с фильтрацией по полю "Уволен = Истина". Команда "Работающие" - список сотрудников с фильтрацией по полю "Уволен = Ложь". Повторный вызов команд должен использовать механизм уникальности форм. Если открыта форма "Уволенные" команда "Работающие" должна все равно открывать вторую форму списка сотрудников с нужной фильтрацией. И наоборот, при открытых "Работающих" список "Уволенных" также должен открываться. Однако, повторный вызов одной и той же команды не должен открывать 2 одинаковых отфильтрованных формы.

### Процесс выполнения

1. Создайте новую подсистему "Кадровый учет"
2. Создайте новый справочник "Сотрудники" и включите его в подсистему "Кадровый учет"
3. Добавьте в справочник "Сотрудники" реквизит "Уволен" с типом Булево
4. Добавьте общую группу команд "Быстрый доступ" и включите ее в подсистему "Кадровый учет". Категория группы команд - Панель навигации
5. Добавьте общую команду "Уволенные сотрудники", включите ее в группу команд "Быстрый доступ" и в подсистему "Кадровый учет"
6. Добавьте общую команду "Работающие сотрудники", включите ее в группу команд "Быстрый доступ" и в подсистему "Кадровый учет"
7. Изучите документацию метода ОткрытьФорму, пользуясь синтакс-помощником и презентацией к лекции 3.3
8. В каждой команде в процедуре ОбработкаКоманды воспользуйтесь методом ОткрытьФорму, используя имя формы "Справочник.Сотрудники.ФормаСписка"
9. В качестве параметров открываемой формы воспользуйтесь системным параметром формы "Отбор" в который передайте структуру со свойством "Уволен" и значением соответствующего фильтра. У вас должна получится Структура `ПараметрыФормы` со свойством "Отбор", значением отбора является структура со свойством "Уволен", т.е. Структура-в-Структуре.
10. В качестве ключа уникальности формы используйте значение текущего статуса сотрудника - уволен или нет.
11. В форме списка справочника Сотрудники добавьте над ДинамическимСписком новое `ПолеНадписи` и реквизит формы "ИмяТекущегоФильтра". Полю надписи снимите флажок "Видимость" в палитре свойств.
12. В обработчике "ПриСозданииНаСервере" формы списка проверьте значение свойства Параметры.Отбор. Если там находится структура со свойством "Актуален" - установите видимость поля надписи `ИмяТекущегоФильтра` в Истина, а в качестве текста надписи укажите "Уволенные" или "Работающие", в зависимости от текущего значения параметра `Параметры.Отбор.Уволен`
13. Если же в параметрах формы не задан отбор (нет свойства "Уволен" в параметре Параметры.Отбор), то поле надписи отображаться не должно.
14. В случае возникновения сложностей воспользуйтесь отладчиком, установив точку останова в процедуре ПриСозданииНаСервере формы списка сотрудников и изучите содержимое структуры Параметры с помощью клавиши Shift+F9