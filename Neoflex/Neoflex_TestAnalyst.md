# Задание 1. Сбор требований
>Напишите какие уточняющие вопросы зададите заказчику по поступившей задаче:
>«Сделать страницу в существующем web-приложении для отображения количества услуг и продуктов Банка, получаемых сотрудниками».
>
><sup>*</sup> Будет здорово, если укажете к пониманию функциональных или каких нефункциональных
>требований относится вопрос.

|п.|Вопросы|ФТ/НФТ|Для чего|
|-|-|-|-|
|0.| Контекст: Что за приложение? Кто, как и для чего использует? Бизнес-цель создания страницы?|-|С4(Context), UC, US|
|1.| Какое целевое действие для перехода на страницу? Как и кем будут использоваться выведенные результаты?|ФТ|С4(Context). UseCase, UserStory, UserStoryMap|
|2.| Где брать данные, которые необходимо отобразить? В каком виде (формате) эти данные можно получить? Есть ли ограничение на доступ к (получаемым) данным? В каком виде (формате) их необходимо отобразить?|ФТ+НФТ|С4(Context,Container), Интеграции, UX, Безопасность|
|3.| Какова структура вывода предоставляемых данных? Есть ли гайдлайны по дизайну страницы? Необходима ли адаптация для мобильных устройств?|НФТ|UX/UI|
|4.| На странице будет только просмотр данных или взаимодействие с ними? Если да, то какое (фильтры, детализация, агрегация, выгрузка)?|ФТ + НФТ|UseCase + UX/UI|
|5.| Предполагаемый объем данных (количество продуктов/услуг, количество сотрудников)? Предполагаемая периодичность использования (рилтайм, день/месяц/год).|НФТ|Нагрузка, UX/UI|
|7.| У кого должен быть доступ к странице? Кто определяет права доступа? Есть ли необходимость логирования? |НФТ|Безопасность|
|8.| Какова срочность задачи?|-|Планирование, приоритизация|

# Задание 2. Бизнес-процесс
>Опишите возможный бизнес-процесс обучения в Учебном Центре (от подачи заявки до
>завершения обучения).
>Используйте любой удобный инструмент: Camunda, PlantUML, Visio, draw.io и пр.

### Вариант 1
> BPMN в draw.io
 
![](https://github.com/vnukov-vv/My_Test_Tasks/blob/main/Neoflex/Обучение_bpmn.drawio.svg)

### Вариант 2
> Activity в PlantUML

![](https://www.plantuml.com/plantuml/svg/pLNDJXDX5DtFKzoDIPk80Gd6b4H9tFi4R4QPmeGw9UqGT4SfFnAmg24nCGgYSJ_yZ4oddVOLxlSAFeddC3ELaHfPkg0pytttd7lFkVVxc6lwTiDVVL6phbuig-6iEBOlewT8HT-RFJtLWLaN7Pfr7UWvVWFjQz_ioRo7XLFJDZiQQgAHEPWbI8-nVA41bWFHIxmSw9d66gMxNn1yZk2MsI2NsIQGN0d3XzhJ03kRJ8eyeJnrwar_TU4LqLxTTwJXBYxv5T4JKEmXF2QEO01kKdT8VMGsTCWb519XSr_oR0EpW-ysoQbsYwLh0jvGc9sQYMIDuBXzfTLwOnaFihv5VAmXu46-8_QYxNhuA6J5RoW82ob08-P6lG6jrN0iz6iMopY2wJQOha4SRo6lvPdzV7dDRYpa3JhN0Q6v9QQLYel5j20jr0jqjcDsyTOtRUfDm-FKhMnArfRScYCb_GRG4DMyfep7PN6RMFnCrMK4JppIGwgtAayuCgoQ2OhwhZTtGfJQjhZ7yK9yp2YXq7GKsNrXJMBUQ9obNwVBKfgUPFSYJShp7hdUZJ86K9SEIepWDjqccVQbD3MJW_uNi_GeFrxZFC9kmXdKhYOmeXXArV1NfkzH5ULZWBUnUpRBkZ_Gzj58tv-OfYS1nmSEYoPKXB-UwTobOLCFAfEmz9EUw4VYyUGbC4Y7ZppAu7rDUDfJhbDQGBuelplWELQIiPEQNK95VOXEBn_M5BDB1D1hdhLgpNbfMMvLIhocQ3dSI4O6K3u1T1agjuefk_9pzxiydBnJ5d_9yNxxxzIQ3ZanCm7uUa9YOTTv-sKdZJqUVr1D9oMUkHtn3Mj8NQYKXF-ez9ZAyVS3qQD6m96kA5jmYTSpuQjm7jaSYk5p72M-3h5ZOV9vxq5pZoRclkRtdsdz_IJZqd6hdD-Jx4X6PXCxCHC3J-2V0njTZK1znPe3cF-KVW40)

<details><summary> <i> см. код (развернуть)</i> </summary>

```PlantUML
@startuml
start

repeat 
repeat 
  :Выбор программы \nобучения;
  :Подача заявки;
  :Регистрация \nна платформе Neostudy;
  note right: Ссылка на регистрацию \nОтправить на почту;
  :Входные мероприятия \nна платформе Neostudy;
  fork
    :Анкета;
  fork again
    :Входное тестирование;
  fork again
    :Тестовое задание;
  end fork
  backward:Подготовиться \nк следующему потоку;
  repeat while (Успешно?) is (Нет)
  ->Да;

:Cобеседование;
note right: - Технические вопросы от эксперта (30 мин)\n - Беседа с рекрутером (15 мин)
  backward:Подготовиться \nк следующему потоку;
  repeat while (Успешно?) is (Нет)
  ->Да;

:Обучение;
note right: Онлайн обучение - до 3.5 месяцев; \nЗанятия 2-3 раза в неделю, с 16:00 МСК; \nСамостоятельная работа и проектное задание;

start
note right: Отбор в команду Neoflex

if (Выполнение домашних заданий ≥ 80%) then (Нет)
elseif (Балл по итоговому тестированию ≥ 85%) then (Нет)
elseif (Балл по проектному заданию ≥ 80%) then (Нет)
elseif (Положительный отзыв от куратора) then (Нет)
else (Да)
:Приглашение \nна собеседование;
endif

:Сертификат \nоб обучении;

stop
@enduml
```
</details>


# Задание 3. Тестирование web-приложения
>Ссылка на web-приложение: https://angular-test-silk-five.vercel.app/login
>Составьте чек-лист с проверками web-приложения (шаблон чек-листа определяете самостоятельно). Приложение содержит баги, их оформлять необязательно.

# Задание 4. Написание SQL-запросов

## Таблицы БД:

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

## SQL-запросы:

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
