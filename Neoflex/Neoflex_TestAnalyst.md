# Задание 1. Сбор требований
Напишите какие уточняющие вопросы зададите заказчику по поступившей задаче:
«Сделать страницу в существующем web-приложении для отображения количества услуг и
продуктов Банка, получаемых сотрудниками».
Будет здорово, если укажете к пониманию функциональных или каких нефункциональных
требований относится вопрос.

# Задание 2. Бизнес-процесс
Опишите возможный бизнес-процесс обучения в Учебном Центре (от подачи заявки до
завершения обучения).
Используйте любой удобный инструмент: Camunda, PlantUML, Visio, draw.io и пр.

### Вариант 1
> BPMN в draw.io
![](https://github.com/vnukov-vv/My_Test_Tasks/blob/main/Neoflex/Обучение_bpmn.drawio.svg)

### Вариант 2
> PlantUML
```

```

# Задание 3. Тестирование web-приложения
Ссылка на web-приложение: https://angular-test-silk-five.vercel.app/login
Составьте чек-лист с проверками web-приложения (шаблон чек-листа определяете
самостоятельно). Приложение содержит баги, их оформлять необязательно.

# Задание 4. Написание SQL-запросов

## Есть БД с 3 таблицами:

![image](https://github.com/user-attachments/assets/57829cf5-4fe7-48c3-960c-b6ab95868c8d)

```dbml

Table FIO_person {
  ID int [pk, note:'id сотрудника']
  FIO varchar [note:'ФИО сотрудника'] 
}

Table Salary {
  Salary_ID int [pk, note:'id записи по ЗП']
  Person_ID int [ref: > FIO_person.ID, note:'внешний ключ к таблице FIO_person']
  Value decimal [note:'ЗП сотрудника']
}

Table JobPosition {
  Position_ID int [pk, note:'id записи по должности'] 
  Person_ID int [ref: > FIO_person.ID, note: 'внешний ключ к таблице FIO_person'] 
  NamePosition varchar [note: 'название должности']
  Duration int [note: 'количество лет в должности']
}
```
См. на [dbdiagram.io](https://dbdiagram.io/d/neoflex2-66e5c65a6dde7f41491e28c8)

## Написать SQL-запросы (предоставить SQL-запросы в текстовом виде):

1. Получить список всех сотрудников по имени Иван

**PostgreSQL**:
```sql
SELECT *
FROM FIO_person
WHERE split_part(FIO, ' ', 2) = 'Иван'
;
/* Функция `split_part` разделяет строку `FIO` (в нашем случае - по пробелам) и возвращает второй элемент, который соответствует имени сотрудника. */
```

**Oracle**:
```sql
SELECT *
FROM FIO_person

/* Вариант 1 - через поиск позиции пробела */
WHERE SUBSTR(
    FIO,
    INSTR(FIO, ' ', 1, 1) + 1,
    INSTR(FIO, ' ', 1, 2) - INSTR(FIO, ' ', 1, 1) - 1
) = 'Иван';

/* Вариант 2 - через регулярные выражения */
WHERE REGEXP_SUBSTR(FIO, '[^ ]+', 1, 2) = 'Иван';

/* `[^ ]+` - соответствует последовательности из одного или более символов кроме пробела, т.е. целого "слова" */
```

2. Получить список всех ФИО сотрудников в должности «Разработчик» и зарплатой, больше
10 000 рублей
   
```sql
SELECT f.FIO
FROM FIO_person f
JOIN Salary s ON f.ID = s.Person_ID
JOIN JobPosition j ON f.ID = j.Person_ID
WHERE j.NamePosition = 'Разработчик' AND s.Value > 10000
;
```
