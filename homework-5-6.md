# Задание к занятию «Расчёт себестоимости и последовательности документов»

## Задача 1. Реализовать расчёт себестоимости товаров в документе «Реализация»

### Описание задачи

Необходимо реализовать суммовой учёт товаров на складе.
Обеспечить, чтобы при реализации товаров, списание происходило корректно — количество, указанное в документе, сумма, рассчитанная по средней себестоимости.

### Требования к результату

Выгрузка информационной базы (.dt) с реализованным функционалом суммового учёта остатков товаров.

### Процесс выполнения

Добавить в регистр накопления «Товары» ресурс «Сумма». Тип — определяемый тип «Сумма».

Добавьте поле «Сумма» в отчёт «Остатки товаров».

В документ проведения прихода товара добавить заполнение сумм значениями, указанными в табличной части.

В документ проведения реализации товаров добавьте заполнение поля «Сумма».

*(Попробуйте сначала воспроизвести ошибочную ситуацию, когда количество списано в ноль, а остаток по сумме отрицательный).*

Реализуйте списание суммы по средней себестоимости товаров.

Убедитесь, что суммы списываются правильно.

## Задача 2. Реализовать последовательность документов для пересчёта документов «Поступление» и «Реализация товаров»

### Описание задачи

Для восстановления сбитого учёта остатков товаров нужно иметь возможность восстановить последовательность учёта.

### Требования к результату

Выгрузка информационной базы (.dt), включающая решение задачи 1, а также добавляющая на форму журнала документов функционал для восстановления последовательности.

### Процесс выполнения

На форму списка журнала документов добавить кнопку для восстановления границы последовательности.

### Подсказка: [результат выполнения домашнего задания](Examples/homework-5-6-example.md).
