### Зарплата

Все добавляемые объекты включаем в новую подсистему **Зарплата**, выведенную в командный интерфейс, и включаем в состав функциональной опции **ВестиРасчетЗарплаты**.

1. Добавить регистр сведений **Календарь**:
  * Измерение - День (Дата)
  * Ресурс - Рабочий (Число)
  
2. Добавить план видов расчета **Начисления**:
  * Без кода и с наименованием разумной длины (например, 100)
  * На закладке "Расчет" укажите:
    * Использование периода действие
    * Зависимость от базы по периоду действия по тому же плану видов расчета
  * Создайте предопределенные виды расчета:
    * **Больничный**, **Отпуск**, **ФиксированнаяПремия**
    * **ОплатаПоОкладу** с вытесняющими видами расчета **Больничный** и **Отпуск**
    * **ПремияПроцентом** с базовым и ведущим видом расчета **ОплатаПоОкладу**

3. Добавить регистр расчета **Зарплата**:
  * С учетом по плану видов расчета **Начисления**
  * Использующий период действия и базовый период
  * Использующий в качестве графика регистр **Календарь**, со значением графика **Рабочий**, а датой графика - **День**
  * С периодичностью "Месяц"
  * С измерением **Сотрудник** (СправочникСсылка.Сотрудники) и ресурсом **Сумма** (ОпределяемыйТип.Сумма).

4. Добавить документ **НачислениеСписком** для начисления фиксированных сумм по списку сотрудников:
  * С обязательным реквизитом **Начисление** (ВидРасчетаСсылка.Начисления)
  * С обязательными реквизитами **ПериодДействияНачало** и **ПериодДействияКонец** (Дата и время)
  * С табличной частью **Сотрудники** с реквизитами **Сотрудник** (СправочникСсылка.Сотрудники) и **Сумма** (ОпределяемыйТип.Сумма).
  * Создать для него понятную форму документа.
  * На закладке "Движения" сделать его регистратором для регистра расчета **Зарплата** и для регистра бухгалтерии **Управленческий**
  * В обработке проведения для каждой строки формировать движения:
    * По регистру расчета **Зарплата**, не забыв заполнить вид расчета (по реквизиту **Начисление**), период регистрации (дата документа) и период действия (согласно реквизитам **ПериодДействияНачало** и **ПериодДействияКонец**). Период действия понадобится в дальнейшем для вытеснения оплаты по окладу больничными и отпусками, введенными этим документом.
    * По регистру бухгалтерии **Управленческий** в Дт счета **Расходы** и в Кт счета **РасчетыССотрудниками** с заполнением субконто **Сотрудник** на сумму начисления.
    
5. Добавить документ **НачислениеОплатыПоОкладу** для начисления оплаты по окладу с учетом фактически отработанного времени:
  * С обязательным реквизитом **ЗаМесяц** типа Дата с форматом "MM.yyyy", чтобы пользователь видел только месяц.
  * С табличной частью **Сотрудники** с единственным реквизитом **Сотрудник** (СправочникСсылка.Сотрудники).
  * Создать для него понятную форму документа.
  * На закладке "Движения" сделать его регистратором для регистра расчета **Зарплата** и для регистра бухгалтерии **Управленческий**
  * В обработке проведения:
    * Запросом выбрать оклады (реквизит **Оклад** из справочника **Сотрудники**) по сотрудникам из табличной части. Проще всего передать в запрос массив сотрудников, выгруженный из табличной части методом **ВыгрузитьКолонку**.
    * Записать движения, пока без сумм, по регистру расчета **Зарплата**. При этом:
      * В качестве периода регистрации указать дату документа, а в качестве периода действия - начало и конец месяца даты **ЗаМесяц**.
      * В качестве вида расчета указать предопределенный ПланыВидовРасчета.Начисления.ОплатаПоОкладу.
    * Записав движения, обойти их повторно и для каждой записи рассчитать сумму оплаты по окладу. При этом:
      * Оклад определить, найдя сотрудника в ранее выбранных данных.
      * Факт и норму определить вызовом метода **ПолучитьДанныеГрафика**, а сумму - умножением оклада на частное деления факта на норму: 

```bsl
	ТаблицаНорма = Движение.ПолучитьДанныеГрафика(
	ВидПериодаРегистраРасчета.ПериодДействия);
	Норма = ТаблицаНорма[0].Рабочий;
	ТаблицаФакт = Движение.ПолучитьДанныеГрафика(
		ВидПериодаРегистраРасчета.ФактическийПериодДействия);
	Факт = ТаблицаФакт[0].Рабочий;
	Движение.Сумма = ?(Норма = 0, 0, Оклад * Факт / Норма);

 ```
 
   * Определив сумму, сформировать парное движение на такую же сумму по регистру бухгалтерии **Управленческий** в Дт счета **Расходы** и в Кт счета **РасчетыССотрудникми**, заполнив субконто **Сотрудник**.
   * Создав документ, заполните календарь и убедитесь в том, что документ:
     * В отсутствие вытесняющих начислений начисляет за месяц полную сумму оклада.
     * После ввода оклада или больничного документом **НачислениеСписком** уменьшает начисления по окладу пропорционально дням невыхода.
   

6. Добавить документ **НачислениеПремииПроцентом** для начисления премии процентом от оклада:
  * С обязательным числовым реквизитом **Процент**
  * С обязательным реквизитом **ЗаМесяц** типа Дата с форматом "MM.yyyy", чтобы пользователь видел только месяц.
  * С табличной частью **Сотрудники** с единственным реквизитом **Сотрудник** (СправочникСсылка.Сотрудники).
  * Создать для него понятную форму документа.
  * На закладке "Движения" сделать его регистратором для регистра расчета **Зарплата** и для регистра бухгалтерии **Управленческий**
  * В обработке проведения:
    * Записать движения, пока без сумм, по регистру расчета **Зарплата**. При этом:
      * В качестве периода регистрации указать дату документа, а в качестве периода действия и базового периода действия - начало и конец месяца даты **ЗаМесяц**.
      * В качестве вида расчета указать предопределенный ПланыВидовРасчета.Начисления.ПремияПроцентом.
    * Записав движения, получить базу запросом:
    
```bsl
	Запрос = Новый Запрос(
		"ВЫБРАТЬ
		|	ЗарплатаБазаЗарплата.Сотрудник КАК Сотрудник,
		|	ЗарплатаБазаЗарплата.СуммаБаза КАК СуммаБаза
		|ИЗ
		|	РегистрРасчета.Зарплата.БазаЗарплата(
		|			&Измерения,
		|			&Измерения,
		|			,
		|			Регистратор = &Регистратор
		|				И ВидРасчета = &ВидРасчета) КАК ЗарплатаБазаЗарплата");
	Запрос.УстановитьПараметр("Регистратор", Ссылка);
	Запрос.УстановитьПараметр("ВидРасчета", ПланыВидовРасчета.Начисления.ПремияПроцентом);
	Измерения = Новый Массив;
	Измерения.Добавить("Сотрудник");
	Запрос.УстановитьПараметр("Измерения", Измерения);
	База = Запрос.Выполнить().Выгрузить();
```

   * Обойти движения повторно и для каждой записи рассчитать сумму премии процентом. При этом:
     * Базу определить, найдя сотрудника в ранее выбранной таблице **База**.
     * Сумму определить, умножив базу на процент премии и поделив на 100. 

```bsl
	Для Каждого Движение Из Движения.Зарплата Цикл
		СтрокаБазы = База.Найти(Движение.Сотрудник);
		Если СтрокаБазы = Неопределено Тогда
			Продолжить;
		КонецЕсли;
		Движение.Сумма = СтрокаБазы.СуммаБаза * Процент / 100;
	КонецЦикла;
 ```
 
  * Определив сумму, сформировать парное движение на такую же сумму по регистру бухгалтерии **Управленческий** в Дт счета **Расходы** и в Кт счета **РасчетыССотрудниками**, заполнив субконто **Сотрудник**.
  * Создав документ, проверьте себя, начислив премию процентом от оклада и выплатив все начисления документом **СписаниеДенежныхСредств**. В оборотно-сальдовой ведомости кредитовые обороты счета **РасчетыССотрудниками** должны полностью закрыться дебетовыми, и остаток стать нулевым.