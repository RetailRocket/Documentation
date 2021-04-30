---
description: >-
  В документе описываются принцыпы и способы получения контента спонсорского
  контента.
---
# RetailRocket Sponsored Products API Contract

Руководство содержит описание http ресурсов, необходимых для использования партнером продукта Sponsored Products.

<!-- TOC -->

- [Термины](#%D1%82%D0%B5%D1%80%D0%BC%D0%B8%D0%BD%D1%8B)
- [Авторизация](#%D0%B0%D0%B2%D1%82%D0%BE%D1%80%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F)
- [Управление сессией](#%D1%83%D0%BF%D1%80%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D1%81%D0%B5%D1%81%D1%81%D0%B8%D0%B5%D0%B9)
- [Base URL](#base-url)
- [Resources](#resources)
- [Response](#response)
- [Status codes](#status-codes)
- [Время ответа](#%D0%B2%D1%80%D0%B5%D0%BC%D1%8F-%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D0%B0)
- [Ограничения](#%D0%BE%D0%B3%D1%80%D0%B0%D0%BD%D0%B8%D1%87%D0%B5%D0%BD%D0%B8%D1%8F)

<!-- /TOC -->

## Термины

**Рекламная площадка**: магазин, предоставляющий страницы своего web или мобильного приложения для размещения рекламных объявлений. Обладает идентификатором `partnerId` типа ObjectId. Можно получить в личном кабинете или у аккаунт менеджера.

**Рекламодатель**: лицо/организация, которая заинтересована в том, чтобы показывать на рекламных площадках свое содержимое, чтобы достигать маркетинговые цели.

**Содержимое для показа (content)**: объект произвольного состава, содержащий необходимые данные для показа пользователю рекламной площадки.
Запрашивается рекламной площадкой у API Sponsored Products.
Размещается рекламной площадкой с целью получить плату от рекламодателя за показ пользователю рекламной площадки.

При запросе необходимо использовать параметр строки запроса `acceptContent` для указания типа содержимого, который рекламная площадка готова разместить.

Значения `acceptContent`:
- `string`
- `productIds`

**Рекламное место (placement)**: область страницы web приложения или экрана мобильного приложения рекламной площадки, предназначенная для продажи рекламодателю с помощью продукта Sponsored Products. Бывает разных типов в зависимости от вида места области размещения. Обладает идентификатором `placementId`. Присваивается исходя из перечня рекламных мест, предоставленных рекламной площадкой.

**Товар (product)**: позиция товарного каталога рекламной площадки. Обладает идентификатором `productId` типа 64 разрядное целое со знаком.

**Категория (category)**: группа товаров, содержащая произвольный (существенный для рекламной площадки) признак. Обладает идентификатором `categoryId` типа 64 разрядное целое со знаком. Предоставляется рекламной площадкой.

## Авторизация

- Для авторизации необходимо во все запросы включать параметр `apiKey={apiKey}` типа строка длиной до 64 символов.
- `apiKey` может измениться, это нужно учитывать при разработки интеграции, и хранить его в БД на сервере.
- `apiKey` можно получить у аккаунт-менеджера.
- В случае неуспешной авторизации (неверный `apiKey`) вернется статус `401`.

## Управление сессией

**Сессия**: набор данных, хранящий состояние пользователя рекламной площадки.
Обладает идентификатором:
- `session` типа [ObjectId](https://docs.mongodb.com/manual/reference/bson-types/#objectid). Предоставляется системой интеграции RetailRocket для web приложений.
- `sessionExternalId` типа строка длиной до 64 символов. Предоставляется рекламной площадкой.

## Base URL
`https://visitors-sp.retailrocket.net/v1/partners/{partnerId}`

## Resources

### Impression for AnyPlacement
`GET /anyPlacements/{placementId}/impressions`

#### Параметры строки запроса
- `session` или `sessionExternalId`
- `acceptContent`
- `apiKey`
#### Тип ответа
`impression`

### Impression for ProductPlacement
`GET /productPlacements/{placementId}/impressions`

#### Параметры строки запроса
- `session` или `sessionExternalId`
- `productId`
- `acceptContent`
- `apiKey`
- `stockId` (необязательный параметр)
#### Тип ответа
`impression`

### Impression for ProductGroupPlacement
`GET /productGroupsPlacements/{placementId}/impressions`

#### Параметры строки запроса
- `session` или `sessionExternalId`
- `productIds`(строка, состоящая из `productId` через запятую)
- `acceptContent`
- `apiKey`
- `stockId` (необязательный параметр)
#### Тип ответа
`impression`

### Impression for CategoryPlacement
`GET /categoryPlacements/{placementId}/impressions`

#### Параметры строки запроса
- `session` или `sessionExternalId`
- `categoryId`
- `acceptContent`
- `apiKey`
#### Тип ответа
`impression`

### Impression for SearchPlacement
`GET /searchPlacements/{placementId}/impressions`

#### Параметры строки запроса
- `session` или `sessionExternalId`
- `searchQuery`
- `acceptContent`
- `apiKey`
#### Тип ответа
`impression`

## Response

### Impression
Объект типа Impression с полем `content` типа строка.

```
{
    "id": "impression identifier",
    "content": {
        "id": "content identifier",
        "string": "any string"
    }
}

```
Объект типа Impression с полем `content` типа товарная полка.

```
{
    "id": "impression identifier",
    "content": {
        "id": "content identifier",
        "productIds": [<product id>, <product id>]
    }
}

```

## Status codes

| Код ответа | Название | Описание |
| ----------- | ----------- | ----------- |
| 200 | OK | Запрос принят |
| 204 | NoContent | По запросу ничего не найдено |
| 400 | BadRequest | Ошибка в запросе, необходимо проверить правильность построения запроса |
| 401 | Unauthorized | Ошибка авторизации, необходимо проверить корректность `partnerId`, `apiKey` |
| 403 | Forbidden | Доступ запрещен |
| 404 | NotFound | Запрошенный ресурс не найден, необходимо проверить правильность URL |
| 429 | TooManyRequests | Превышение лимита запросов |

## Время ответа

Ожидаемое время ответа: 100 мс для всех ресурсов (может отличаться из-за особенностей сетевой инфраструктуры).

## Ограничения

По умолчанию действует ограничение в 50 запросов/секунду для каждой рекламной площадки. При необходимости возможно изменить (с помощью аккаунт менеджера).