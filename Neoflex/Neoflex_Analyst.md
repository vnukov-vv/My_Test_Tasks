# Задание 1. Функциональные и нефункциональные требования
> Опишите функциональные и нефункциональные требования для формы «Хочу учиться» на сайте Учебного Центра Neoflex https://edu.neoflex.ru/ (форма открывается по кнопке «Хочу учиться»).

## ФТ
- Я, как **Пользователь**, хочу **Оставить Заявку**, чтобы получить **Уведомление** о новом наборе на интересующую меня **Программу** обучения по указанным контактным данным.
- **Форма** должна предлагать выбор из актуальных **Программ** обучения.
- **Форма** должна валидировать введенные **Пользователем** контактные данные на соответствие форматам.
- **Форма** должна уведомить **Пользователя** об успешном завершении процесса **Оставить Заявку**

### НФТ
- В **Форме** должно быть указано несколько каналов связи с **Пользователем**
- **Форма** должна отвечать требованиям Федерального закона от 27.07.2006 г. № 152-ФЗ «О персональных данных»
- Процесс заполнения **Формы** **Пользователем** не должен занимать более 1 минуты.
- От начала до конца заполнения **Формы** должно быть не более 3-х кликов (состояний): Инициация, Подтверждение действия, Завершение процесса.

# Задание 2. Бизнес-процесс
> Опишите возможный бизнес-процесс обучения в Учебном Центре (от подачи заявки до завершения обучения).

![](https://github.com/vnukov-vv/My_Test_Tasks/blob/main/Neoflex/Обучение_bpmn.drawio.svg)


# Задание 3. Декомпозиция задачи на подзадачи
> Выпишите задачи для реализации формы «Набор закрыт, но ты можешь оставить заявку» на сайте Учебного Центра Neoflex https://edu.neoflex.ru/ (форма открывается по кнопке «Хочу учиться» → «Отправить»). <br>
> Уточнение: Предположите реализацию формы в web-приложении на микросервисной архитектуре<br>
> Подсказка: Используйте требования, полученные в первом задании<br>
> Пример: Описать форму «Набор закрыт, но ты можешь оставить заявку. Успешная отправка заявки»<br>

- Определить и описать состав и форматы обязательных полей **Формы**
- Описать иные обязательные атрибуты **Формы** для реализации НФТ
- Описать **Форму** для успешного состояния (данные введены корректно, заявка успешно отправлена)
- Описать **Форму** для неуспешных состояний (данные введены некорректно или не полностью, заявка не отправлена)
- Спроектировать архитектуру приложения (фронт, бэк)
- Спроектировать модель базы данных приложения
- Спроектировать и описать API
- Определить критерии приёмки
- Написать тест-кейсы

# Задание 4. Проектирование БД и написание SQL-запросов

## 4.1 Постройте модель данных
> РСУБД (реляционной системы управления базами данных) для хранения информации о процессе обучения в Учебном Центре. Для этого опишите сущности (таблицы), атрибуты (наборы полей), типы данных, обязательность заполнения, первичные и внешние ключи, а также связи между сущностями.<br>
Подсказка: Таблицы, которые может включать база данных: «Студенты», «Лекторы», «Дисциплины», «Расписание» и т.д. <br>
Допущения:
> - Один студент может посещать несколько дисциплин
> - Лектор может вести несколько дисциплин


![DBML](https://github.com/vnukov-vv/My_Test_Tasks/blob/main/Neoflex/Neoflex_dbml.svg)

```dbml
//«Факультет»
Table Departments {
  dept_id smallint [primary key]
  department text [not null]
}

//«Учебные модули»
Table Modules {
  mod_id integer [primary key]
  dept_id smallint [ref: > Departments.dept_id]
  module text [not null]
}
//«Дисциплины»
Table Subjects {
  subj_id integer [primary key]
  mod_id integer [ref: > Modules.mod_id]
  subject text [not null]
}

// «Лекторы»
Table Lecturers {
  lect_id integer [primary key]
  name text [not null]
  surname text [null]
  lastname text [not null]
  birth_date date [not null]
  email text [null]
  phone char [null]
}

// «Дисциплины по лекторам»
Table Lecturer_Subjects {
  lect_id integer [ref: > Lecturers.lect_id]
  subj_id integer [ref: > Subjects.subj_id]
  PRIMARY KEY (lect_id, subj_id)
}

// «Группы»
Table Squads {
  squad_id integer [primary key]
  dept_id smallint [ref: > Departments.dept_id]
  squad text [not null]
}

// «Студенты»
Table Students {
  stud_id integer [primary key]
  squad_id integer [ref: > Squads.squad_id]
  name text [not null]
  surname text [null]
  lastname text [not null]
  birth_date date [not null]
  email text [null]
  phone char [null]
}

// «Типы лекций» (онлайн/оффлайн)
Table Types {
  type_id smallint [primary key]
  type text [not null]
}

// «Расписание»
Table Shedules {
  unit_id integer [primary key, increment]
  study_day date [not null]
  subj_id integer [ref: > Subjects.subj_id]
  squad_id integer [ref: > Squads.squad_id]
  type_id smallint [ref: > Types.type_id]
  link text [null]
}

```


## 4.2 Ответьте на вопросы:
> 1. В каком виде правильно хранить информацию о соотношении дисциплин, студентов и лекторов?

В табличном. Реляционная база данных
> 2. Приведение к какой нормальной форме должно использоваться при хранении информации о соотношении дисциплин, студентов и лекторов?

3-я нормальная форма. Все неключевые поля, содержимое которых может относиться к нескольким записям таблицы выносятся в отдельные таблицы (нет транзитивной зависимости).

## 4.3 Напишите SQL-запросы к спроектированной схеме БД, которые выведут:
> 1. Перечень дисциплин, к которым прикреплены более 5 студентов
> Набор возвращаемых полей: Наименование дисциплины, Количество студентов, прикрепленных к дисциплине

```sql
SELECT
    s.subject AS "Наименование дисциплины",
    COUNT(DISTINCT st.stud_id) AS "Количество студентов"
FROM
    Shedules sh
JOIN Subjects s ON sh.subj_id = s.subj_id
JOIN Students st ON sh.squad_id = st.squad_id  -- Используйте правильные имена полей
WHERE 1=1
/* чтобы выбрать обучающиеся в настоящее время группы, необходимо модифицировать БД добавив в таблицу Squads атрибут "активности" '0','1'
тогда можно сделать JOIN squads и добавить условие
AND sq.active = 1 */ 
GROUP BY
    s.subject
HAVING
    COUNT(DISTINCT st.stud_id) > 5
;
```

> 2. Средний возраст лекторов по дисциплине «Системный анализ» на текущий день
> Набор возвращаемых полей: цифра

```sql
SELECT 
    AVG(EXTRACT(YEAR FROM AGE(CURRENT_DATE, birth_date))) AS average_age
FROM Lecturers l
JOIN Lecturer_Subjects ls ON l.lect_id = ls.lect_id
JOIN Subjects s ON ls.subj_id = s.subj_id
WHERE s.subject = 'Системный анализ'
;
```

> 4. Список студентов, у которых не указан мобильный телефон
> Набор возвращаемых полей: Фамилия, Имя, Отчество выведены в поле с наименованием «ФИО»
> Подсказки:
> Мобильный телефон может быть не указан, когда поле «мобильный телефон» является необязательным
> Используйте конкатенацию полей «Фамилия», «Имя», «Отчество»

```sql
SELECT 
    CONCAT(lastname, ' ', name, ' ', surname) AS "ФИО"
FROM 
    Students
WHERE 
    phone IS NULL
ORDER BY 
    "ФИО"
;
```


> 5. Рейтинг популярности дисциплин среди студентов таким образом, что первое место будет занимать дисциплина с наибольшим количеством прикрепленных студентов
> Набор возвращаемых полей: Номер места в рейтинге, Наименование дисциплины, Количество студентов на дисциплине
> Подсказки:
> По желанию можно воспользоваться оконной функцией row_number() для вывода номера дисциплины в рейтинге
> Группировку можно производить по наименованию дисциплины

```sql
SELECT
    row_number() OVER (ORDER BY COUNT(DISTINCT st.stud_id) DESC) AS "Номер места в рейтинге",
    s.subject AS "Наименование дисциплины",
    COUNT(DISTINCT st.stud_id) AS "Количество студентов"
FROM
    Shedules sh
JOIN Subjects s ON sh.subj_id = s.subj_id
JOIN Squads sq ON sh.squad_id = sq.squad_id
JOIN Students st ON sq.squad_id = st.squad_id
WHERE 1=1
GROUP BY s.subject
ORDER BY "Номер места в рейтинге"
;


```

> 6. Список занятий по «Системный анализ», проведенных в формате «Онлайн» за текущий месяц (занятия, проводимые в текущий день должны попасть в выборку)
> Набор возвращаемых полей: Название дисциплины, Дата проведения занятия, Название формата
> Подсказка: Не забудьте исключить занятия, которые еще только будут проведены в текущем месяц

```sql
SELECT 
    s.subject AS "Название дисциплины",
    sh.study_day AS "Дата проведения занятия",
    t.type AS "Название формата"
FROM
    Shedules sh
JOIN
    Subjects s ON sh.subj_id = s.subj_id
JOIN
    Types t ON sh.type_id = t.type_id
WHERE
    s.subject = 'Системный анализ'
    AND t.type = '1' -- 0 - оффлайн (по умолчанию); 1 - онлайн
    AND sh.study_day >= DATE_TRUNC('month', CURRENT_DATE)
    AND sh.study_day <= CURRENT_DATE
ORDER BY
    sh.study_day
;

```