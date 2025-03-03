# Задание к занятию "Универсальные коллекции"
_Примерное время выполнения: 60 минут_

Все задачи обязательны к выполнению. Пожалуйста, присылайте на проверку все задачи сразу.

Любые вопросы по решению задач задавайте в чате учебной группы.

## Цель задания

1. Закрепить на практике работу с универсальными коллекциями

Решение задания научит вас использовать массивы, структуры и соответствия для написания алгоритмов. Этот навык является одним из базовых для дальнейшего решения домашних заданий и промышленной разработки.

## Чеклист готовности к домашнему заданию

- [ ] Установлена учебная платформа версии 8.3.20 или больше.
- [ ] Развернута информационная база с конфигурацией **УправлениеИТФирмой**, полученная по итогу выполнения [домашнего задания к занятию Циклы](/homework-2-6.md).

## Инструкция к заданию

1. Решите описанные задачи в конфигураторе.
2. Протестируйте решение в пользовательском режиме.
3. Отправьте на проверку в личном кабинете Нетологии внешнюю обработку **ПодсчетЧислаУникальныхСлов** с решением задачи 1 и файл выгрузки информационной базы (.dt) с конфигурацией, содержащей решение по задаче 2. Файлы прикрепите в раздел "решение" в практическом задании.

## Задача 1 "Подсчет числа уникальных слов"

### Описание задачи
Вы хотите реализовать обработку, которая сможет посчитывать число уникальных слов в тексте, введенном пользователем.

### Требования к результату
Внешняя обработка с реквизитом и многострочным полем ввода для текста и кнопкой "Подсчитать", по нажатию на которую определяется и выводится пользователю число уникальных слов в тексте без учета регистра. Иначе говоря, слово, встречающееся в тексте несколько раз, учитывается в итоговом результате лишь однажды.

_Например: для текста "Привет привет ПрИвЕт Нетология" результат должен быть 2_

### Процесс выполнения
1. Создайте внешнюю обработку с именем, например, **ПодсчетЧислаУникальныхСлов**.
2. Добавьте в нее реквизит типа **Строка** - например, **Текст** - и перетащите его на форму, сделав многострочным полем ввода.
3. Добавьте команду **Подсчитать** и перетащите ее кнопкой на форму.
4. В обработчике команды:
* Создайте **Соответствие** для хранения уникальных слов.

<details>
    <summary>Подсказка</summary>

Пример решения задачи по поиску дублирующих чисел:

```
МассивЧисел = Новый Массив;  
МассивЧисел.Добавить(0);  
МассивЧисел.Добавить(1);  
МассивЧисел.Добавить(2);  
МассивЧисел.Добавить(2);  
МассивЧисел.Добавить(0);  
Дубликаты = Новый Соответствие;
Для Каждого Число Из МассивЧисел Цикл  
   Повтор = Дубликаты.Получить(Число);  
   Если Повтор = Неопределенно Тогда  
    Дубликаты.Вставить(Число, 1);  
 Иначе 
  НовоеЗначение = Повтор + 1;  
  Дубликаты.Вставить(Число, НовоеЗначение);  
 КонецЕсли;  
КонецЦикла;

Для Каждого ЭлементСоответствия Из Дубликаты Цикл  
Сообщить (СтрШаблон(“Значение %1 встретилось %2 раза”,  ЭлементСоответствия.Ключ, ЭлементСоответствия.Значение));  
КонецЦикла;  
```

</details>

* Разделите текст на слова вызовом **СтрРазделить()**.
* Обойдите в цикле все слова.
* Вставляйте в соответствие слово, приведя его к верхнему или нижнему регистру.
* Выведите результат - число элементов соответствия - вызовом **Сообщить()** или **ПоказатьПредупреждение()**, формируя строку с помощью **СтрШаблон()**.

## Задача 2 "Поздравления"

### Описание задачи
Вы хотите, чтобы программа при запуске поздравляла пользователя с праздником, если он приходится на сегодня или на завтра.

### Требования к результату
Выгрузка информационной базы (.dt), при начале работы системы выводящая поздравление с сегодняшней или завтрашней праздничной датой, находя ее в заранее подготовленном массиве структур. Приветствие должно содержать название праздника и собираться функцией СтрШаблон().

### Процесс выполнения
1. Используйте конфигурацию из прошлого задания.
2. Реализуйте алгоритм оповещения о праздниках в обработчике **ПриНачалеРаботыСистемы** в модуле приложения.
3. Объявите переменную-массив праздничных дат.
4. Добавьте в него несколько праздничных дат в виде структур со свойствами **День**, **Месяц**, **Название**.
5. Определите, не является ли текущая дата праздничной или предпраздничной, обходя в цикле этот заранее подготовленный массив структур, и покажите соответствующее поздравление.

<details>
    <summary>Подсказка</summary>

Пример задачи, которая уведомляет сотрудника о планируемых работах на оборудовании.

```
ДатыПрофилактик = Новый Массив;  
ДатыПрофилактик.Добавить(Новый Структура("День, Название", 2,  "Профилактика оборудования ПЭВМ"));  
ДатыПрофилактик.Добавить(Новый Структура("День, Название", 3,  "Профилактика оборудования ИБП"));  
ДатыПрофилактик.Добавить(Новый Структура("День, Название", 4,  "Профилактика серверного оборудования"));
Для Каждого Элемент Из ДатыПрофилактик Цикл  
	Если День(ТекущаяДата()) = Элемент.День 	Тогда  
		Сообщить(СтрШаблон("Сегодня - %1",Элемент.Название));  
	КонецЕсли;	КонецЦикла;  
```

</details>

## Критерии оценки

Задание считается выполненным при соблюдении следующих условий:
1. Решение включает внешнюю обработку **ПодсчетЧислаУникальныхСлов** и  выгрузку в формате dt с конфигурацией **УправлениеИТФирмой**;
2. Обработка **ПодсчетЧислаУникальныхСлов** позволяет выявить уникальные слова в тексте, веденным пользователем;
3. При запуске программы выводится поздравление, если на сегодня или на завтра приходится праздник.

## Подсказка:

Чтобы вам было проще понять, что в итоге должно получиться, мы подготовили подсказки: анимационные изображения в формате gif или картинки. Чтобы их увидеть, кликните по [ссылке](https://github.com/netology-code/1c-homeworks/blob/vy-new-format/Examples/homework-2-7-example.md)
