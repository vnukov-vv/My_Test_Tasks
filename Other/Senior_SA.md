# Тестовое задание для системного аналитика

## Сбор требований/Анализ
 -  Поступает задача на системный анализ - интеграция нового, запасного провайдера кредитной истории клиентов.
 -  В BRD (Business requirements document) присутствует информация о провайдере, ссылки на api, ключи интеграции.
 -  Основное требование заказчика - подключить провайдера к системе, выполнять запрос за основным провайдером, запросы к новому провайдеру свести к минимуму, обеспечить запись 100% ответов запасного провайдера в системе, учесть возможность отключения интеграции без поставки в прод.

## Ограничения
 -  Архитектура приложения не позволяет записывать ответы в базу данных мастер системы, необходимо использовать брокер сообщений для хранения, настроить коннектор до архивной базы данных.
 -  Запросы к запасному провайдеру не должны влиять на ход работы, то есть каким либо образом замедлять работу системы.

## Задача
 - Последовательно описать этапы проработки требований с учетом, что заказчик не силён в тонкостях системы, проще говоря верхнеуровневые функциональные и нефункциональные.
    + Какие вопросы задать? Что дополнить от себя?
 - Последовательно описать все вопросы, которые приходят на ум, при условии, что система малознакома.

## Проектирование/ТЗ
### Задача
 - Необходимо разработать функционал создания аннуитетного кредита в системе на REST-архитектуре.
 - Расписать методы post/get/patch.
 - Расписать колоночную структуру всех сущностей, ERD.
 - Расписать sequence diagram вызова каждого из методов.(Front-end <-> Back-end <-> Database)
 - ТЗ задание оформить в виде неразрывной постановки в структуре:
    * Краткое описание.
    * Диаграммы.
    * Структура БД.
    * Описание рест методов.
> Паттерн ТЗ не важен, оцениваться будет полнота.

### Вводные по задаче:
 - В тело `post` передается `uuid` клиента, сумма займа, тип займа, срок.
 - В рамках одного клиента может быть только один активный договор.
 - Срок кредита не должен превышать 12 месяцев.
 - Рассчитываются периоды платежей по аннуитетной формуле, создаются записи в бд с суммами займа, сам договор.
 - В `get` передается `uuid` договора.
     * Ответ содержит текущее состояние договора.
 - В тело `patch` передается сторнирующая транзакция, и период платежа
     * Метод вносит сторнирующую транзакцию и пересчитывает периоды
     * В ответе возвращается массив периодов с учетом сторнирующийтранзакции.
 - Упростим структуру таблиц до 3х:
    * Таблица договоров.
    * Таблица транзакций.
    * Таблица периодов.

## Инструменты
 - ERD и табличная структура - https://dbdiagram.io/
 - Sequence diagram - https://sequencediagram.org/
 - post/get/patch/update - POSTMAN экспорт коллекции в JSON
