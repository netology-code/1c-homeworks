# Требования к результату

## Технические требования

* Программный код всех модулей должен быть оформлен в соответствии со стандартами разработки на платформе 1С:Предприятие [по ссылке](https://its.1c.ru/db/v8std#content:456:hdoc).

* Недопустимо выполнять запросы в цикле, в том числе неявные при обращении через точку к реквизиту ссылки. То есть обращение `Строка.Номенклатура.ТипНоменклатуры` недопустимо.

* При работе с регистрами расчёта необходимо получать данные для расчёта через виртуальные таблицы. Недопустимо использовать объектную модель для обращения к регистрам расчёта.

* Должны быть введены тестовые данны для полноценной проверки выполнения задания. Созданы все документы, описанные в задании, введены необходимые элементы справочников, записи в регистрах и т.п.

## Функциональные требования

Выгрузка информационной базы (файл с расширением dt), включающей демоданные и конфигурацию с именем «УправлениеИТФирмой» из диплома блока Б, дополненную:

Подсистемой **Учет**, а в ней:

* Регистром бухгалтерии **Управленческий**, который:
  * содержит ресурсы Количество и Сумма определяемых типов,
  * поддерживает корреспонденции.
   
* Планом видов характеристик **ВидыСубконто**, который:
  * включает значения типов СправочникСсылка.Номенклатура, СправочникСсылка.Контрагенты, СправочникСсылка.Сотрудники;
  * содержит предопределённые элементы Номенклатура, Контрагенты, Сотрудники.

* Планом счетов **Управленческий**, который:
  * используется для учета в регистре бухгалтерии **Управленческий**,
  * в качестве видов субконто использует ПВХ **ВидыСубконто**,
  * содержит предопределенные счета:
    * активный 41 **Товары** (виды субконто: Номенклатура, учёт по количеству);
    * активный 50 **ДенежныеСредства**;
    * активно-пассивный 60 **РасчётыСПоставщиками** (виды субконто: Контрагенты);
    * активно-пассивный 62 **РасчётыСПокупателями** (виды субконто: Контрагенты);
    * активно-пассивный 70 **РасчётыССотрудниками** (виды субконто: Сотрудники);
    * активно-пассивный 90 **ПрибылиИУбытки** с субсчётами:
      * пассивный 90.1 **Выручка**,
      * активный 90.2 **Расходы**.
      
* Отчетом **ОборотноСальдоваяВедомость**, который:
  * построен на СКД,
  * выводит отстатки и обороты по счетам за выбранный период.

* Документы, созданные в рамках диплома Б, должны быть доработаны для формирования проводок по регистру бухгалтерии **Управленческий**:
  * документ **ПоступлениеТоваровИУслуг**:
    * для товаров — Дт **Товары** с заполнением субконто Номенклатура — Кт **РасчётыСПоставщиками** с заполнением субконто Контрагенты на сумму закупки;
    * для услуг — Дт **Расходы** — Кт **РасчётыСПоставщиками** с заполнением субконто Контрагенты на сумму закупки.
  * Документ **РеализацияТоваровИУслуг**:
    * для всех строк — Дт **РасчётыСПокупателями** с заполнением субконто Контрагенты — Кт **Выручка** на сумму продажи;
    * для товаров — Дт **Расходы** — Кт **Товары** с заполнением субконто Номенклатура на сумму себестоимости списанного товара. Себестоимость должна рассчитываться по данным регистра бухгалтерии, а не по данным регистра накопления **Товары**. Недопустимо списание товара в минус.
  * Документ **ПоступлениеДенежныхСредств** (после расширения типа реквизита **Плательщик** типом **СправочникСсылка.Сотрудники**):
    * для контрагентов — Дт **ДенежныеСредства** — Кт **РасчётыСПокупателями** с заполнением субконто **Контрагенты** на сумму платежа;
    * для сотрудников — Дт **ДенежныеСредства** - Кт **РасчётыССотрудниками** с заполнением субконто **Сотрудники** на сумму платежа.
  * Документом **СписаниеДенежныхСредств** (после расширения типа реквизита **Получатель** типом **СправочникСсылка.Сотрудники**):
    * для контрагентов — Дт **РасчётыСПоставщиками** с заполнением субконто **Контрагенты** — Кт **ДенежныеСредства** на сумму платежа;
    * для сотрудников — Дт **РасчётыССотрудниками** с заполнением субконто **Сотрудники** — Кт **ДенежныеСредства** на сумму платежа.

Подсистемой **Зарплата**, а в ней:

* Регистром сведений **Календарь**, который:
  * содержит измерение **День** (Дата) и ресурс **Рабочий** (Число).

* Регистром расчёта **Зарплата**, который:
  * содержит измерение **Сотрудник** и ресурс **Сумма** определяемого типа,
  * поддерживает период действия и период регистрации,
  * привязан к регистру **Календарь** как к календарю.
   
* Планом видов расчёта **Начисления**, который:
  * используется для учёта в регистре расчёта **Зарплата**,
  * использует период действия и зависит по базе по периоду действия от самого себя,
  * содержит предопределенные виды расчётов:
    * Больничный, Отпуск, ФиксированнаяПремия;
    * ОплатаПоОкладу, вытесняемый видами расчёта Больничный и Отпуск;
    * ПремияПроцентом, зависящая по базе от вида расчёта ОплатаПоОкладу.
      
* Документом **НачислениеСписком**, который:
  * содержит реквизит **Начисление** (ПланВидовРасчётаСсылка.Начисления) и начало и конец периода действия;
  * содержит табличную часть **Сотрудники** с сотрудниками и суммами;
  * при проведении формирует движения:
    * по регистру расчёта **Зарплата** с указанием сотрудника, вида расчёта, периодов и суммы;
    * по регистру бухгалтерии **Управленческий** в Дт счета **Расходы** с Кт счета **РасчетыССотрудниками** с заполнением субконто **Сотрудники** на ту же сумму.

* Документом **НачислениеОплатыПоОкладу**, который:
  * содержит реквизит **ЗаМесяц** (Дата), определяющий месяц периода действия;
  * содержит табличную часть **Сотрудники** с сотрудниками;
  * при проведении формирует движения:
    * по регистру расчёта **Зарплата** с указанием сотрудника, вида расчёта, периодов и суммы. Сумма рассчитывается по данным графиков как оклад, умноженный на частное от деления фактически отработанного времени (с учётом вытеснения больничным и окладом) на норму времени.
    * по регистру бухгалтерии **Управленческий** в Дт счета **Расходы** с Кт счета **РасчетыССотрудниками** с заполнением субконто **Сотрудники** на ту же сумму.    
    
* Документом **НачислениеПремииПроцентом**, который:
  * содержит реквизит **ЗаМесяц** (Дата), определяющий месяц периода действия, и **Процент** (Число), определяющий процент премии;
  * содержит табличную часть **Сотрудники** с сотрудниками;
  * при проведении формирует движения:
    * по регистру расчёта **Зарплата** с указанием сотрудника, вида расчёта, периодов и суммы. Сумма рассчитывается по данным базы (оплаты по окладу за базовый период) умножением базы на процент;
    * по регистру бухгалтерии **Управленческий** в Дт счета **Расходы** с Кт счета **РасчетыССотрудниками** с заполнением субконто **Сотрудники** на ту же сумму.

Подсистемой **Взаимодействие**, а в ней:

* справочником **Роли** с наименованием разумной длины;
* регистром сведений **ИсполнителиРолей**, который:
  * содержит измерения **Роль** (СправочникСсылка.Роли) и **Исполнитель** (СправочникСсылка.Сотрудники);
  * используется для адресации задач.

* Задачей **Задача**, которая:
  * содержит реквизиты адресации **Исполнитель** (основной, СправочникСсылка.Сотрудники) и **Роль** (СправочникСсылка.Роли), заполняемые по данным процесса;
  * в качестве текущего исполнителя использует значение параметра сеанса **ТекущийСотрудник**;
  * содержит реквизит **ПодробноеОписание** (строка неограниченной длины), заполняемый по данным процесса;
  * содержит форму задачи с ее прикладными реквизитами, недоступными для редактирования;
  * содержит форму **ЗадачиМне**, которая:
    * содержит задачи, адресованные текущему сотруднику по данным виртуальной таблицы **Задача.Задача.ЗадачиПоИсполнителю** с учётом ролевой адресации;
    * выведена на рабочую область начальной страницы.

* Процессом **Поручение**, который:
  * использует задачу **Задача**;
  * содержит реквизиты, достаточные для заполнения создаваемых задач, и реквизит **Предмет** (ОпределяемыйТип.ПредметПроцесса);
  * имеет простую схему, состоящую из точки старта, точки действия и точки завершения;
  * при создании задач заполняет их наименование, подробное описание, исполнителя и роль по собственным данным.
