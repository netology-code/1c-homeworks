# Задание к занятию "Пользователи и отладка"

## Задача 1 "Создание ролей и пользователей"

### Описание задачи
Нужно создать роли **ПолныеПрава**, **БазовыеПрава**, **ДобавлениеИзменениеКонтрагентов** и **ДобавлениеИзменениеСотрудников** и назначить их четырем пользователям.

### Требования к результату
Результат - выгрузка (.dt) информационной базы, в которой есть роли **ПолныеПрава** и **БазовыеПрава**, а также:
- пользователь **Администратор** с полными правами;
- пользователь с базовыми правами;
- пользователь с базовыми правами и правом на редактирование контрагентов;
- пользователь с базовыми правами и правом на редактирование сотрудников;

### Процесс выполнения

1. Используйте конфигурацию **УправлениеИТФирмой**.
2. Создайте в ней роль **ПолныеПрава**, включив в ней все права и предписав установку прав для новых объектов. Проще всего назначать права не отдельным объектам метаданных, с сразу их классам ("Справочники", "Документы") и т.д. Сделайте эту роль основной для конфигурации.
3. Создайте в ней роль **БазовыеПрава**, включив в ней права на просмотр и чтение всех данных конфигурации, а также право на запуск тонкого клиента и веб-клиента.

<details>
    <summary>Подсказка</summary>

- **БазовыеПрава** – это как зритель в зрительном зале. Можно смотреть, читать, но влиять на процесс, изменять нельзя. Поэтому права нужны на все, но только на Чтение и Просмотр. Также на запуск клиента, так как мы это делаем через него. 

- Остальные роли добавляют разрешения на определенные действия. Поэтому добавляем в них только эти действия.

</details>

4. Создайте роли **ДобавлениеИзменениеСотрудников** и **ДобавлениеИзменениеКонтрагентов**, дающие права на интерактивное добавление и редактирование контрагентов и сотрудников.
5. Создайте пользователей ИБ:
- **Администратор** с ролью **ПолныеПрава**;
- произвольного пользователя с ролью **БазовыеПрава**; 
- пользователя с ролями **БазовыеПрава** и **ДобавлениеИзменениеКонтрагентов**; 
- пользователя с ролями **БазовыеПрава** и **ДобавлениеИзменениеСотрудников**. 
6. Обновите конфигурацию БД и запустите конфигурацию в режиме Предприятия подо всеми четырьмя пользователями. Убедитесь, что администратор может редактировать, что угодно; пользователь с базовыми правами может только просматривать данные; а два пользователя с ролью "ДобавлениеИзменение..." могут редактировать данные одного вида, и только просматривать все остальные.
7. Выгрузите ИБ в .dt как результат выполнения задания.

## Задача 2 "Поиск ошибки с помощью точки останова"

### Описание задачи
Нужно найти в учебной демобазе и исправить специально привнесенную ошибку с помощью остановки по ошибке.

### Требования к результату
Результат - файл .CF конфигурации, в котором исправлена ошибка, возникавшая при попытке записать блок.

### Процесс выполнения
1. Создайте пустую ИБ и загрузите в нее [демо-базу](https://github.com/netology-code/1c-homeworks/blob/master/1C_developer_demo_updated.dt).
3. Запустите ее в режиме Предприятия с отладкой и воспроизведите ошибку, открыв любой блок из списка и попытавшись записать его.
4. Найдите строку с ошибкой с помощью функции "Остановка по ошибке" и исправьте ее.

<details>
    <summary>Подсказка</summary>

- Так как процедура вызывается в другом месте, полностью ее удалять нельзя. Удалять следует только содержимое процедуры. Удалите только строку в процедуре, а не саму процедуру.

 ![](https://u.netology.ru/backend/uploads/lms/attachments/files/data/54593/%D0%BF%D0%BE%D0%B4%D1%81%D0%BA%D0%B0%D0%B7%D0%BA%D0%B0_6.png)
   
</details>
    
4. Сохраните конфигурацию как результат выполнения задания.

## Задача 3 "Поиск ошибки с помощью журнала регистрации"

### Описание задачи
Нужно найти в учебной демобазе и исправить специально привнесенную ошибку с помощью журнала регистрации.

### Требования к результату
Результат - файл .CF конфигурации, в котором исправлена ошибка, возникавшая при работе фонового задания.

### Процесс выполнения
1. Создайте пустую ИБ и загрузите в нее [демо-базу](https://github.com/netology-code/1c-homeworks/blob/master/1C_developer_demo_updated.dt).
2. Запустите ее в режиме Предприятия с отладкой и найдите в журнале регистрации ошибки работы фонового задания.
3. Найдите в конфигурации строку с этой ошибкой и исправьте ее.

<details>
    <summary>Подсказка</summary>

- Так как процедура вызывается в другом месте, полностью ее удалять нельзя. Удалять следует только содержимое процедуры. То есть удалите только строку в процедуре, а не саму процедуру.
    
    </details>

4. Убедитесь, снова запустив конфигурацию в режиме "Предприятия", что ошибка перестала воспроизводиться.
5. Сохраните конфигурацию как результат выполнения задания.

# Критерии оценки

## Зачет
1. Форма предоставленных файлов соответсвует требованиям задач, предоставлено 3 файла (один по итогу выполнения каждой задачи)
2. В конфигурации **УправлениеИТФирмой** созданы необходимые роли, в ролях корректно установлены права, созданы пользователи для каждого набора ролей для тестирования. Роли для создания и изменения не имют прав, кроме создания и изменения соответствующих объектов (в т.ч. на запуск клиента, просмотр подсистемы и т.д.)
3. В информационной базе созданы 4 пользователя для тестирования: 
    - администратор с логином "Администратор" и ролью Полные права; 
    - пользователь с произвольным логином и ролью Базовые права; 
    - пользователь с произвольным логином и ролями Базовые права и Добавление изменение сотрудников; 
    - пользователь с произвольным логином и ролями Базовые права и Добавление изменение контрагентов.
4. В конфигурации **Курс 1С-разработчик** в результате выполнения задачи 2 найдена и исправлена ошибка при записи элемента справочника Блоки
5. В конфигурации **Курс 1С-разработчик** в результате выполнения задачи 3 найдена и исправлена ошибка в работе фонового задания

## На доработку
1. Предоставлены результаты решения не всех задач
2. В конфигурации **УправлениеИТФирмой** не созданы необходимые роли, некорректно установлены права или не созданы тестовые пользователи
3. В конфигурации **Курс 1С-разработчик** не исправлены специально привнесенные ошибки
