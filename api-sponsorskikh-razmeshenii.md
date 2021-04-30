---
description: >-
  В документе описываются принципы и способы получения контента спонсорского
  контента.
---

# API получения спонсорских размещений

## Термины

**Рекламная площадка**: магазин, предоставляющий страницы своего web или мобильного приложения для размещения рекламных объявлений. Обладает идентификатором `partnerId`. Можно получить в личном кабинете или у аккаунт менеджера.

**Рекламодатель**: лицо/организация, которая заинтересована в том, чтобы показывать на рекламных площадках свое содержимое, чтобы достигать маркетинговые цели.

**Содержимое для показа \(content\)**: <a id="acceptcontent" name="acceptcontent"></a> объект произвольного состава, содержащий необходимые данные для показа пользователю рекламной площадки. Запрашивается рекламной площадкой у API спонсорских размещений. Показывается рекламной площадкой с целью получить плату от рекламодателя за просмотр этого сдержимого пользователем площадки.

При запросе необходимо использовать параметр строки запроса `acceptContent` для указания типа содержимого, который рекламная площадка готова разместить.

Значения `acceptContent`:

* `string`
* `productIds`

Если рекламная площадка готова размещать более одного варианта содержимого, их можно указать через запятую, например: `acceptContent=string,productIds`

**Рекламное место \(placement\)**: область страницы web приложения или экрана мобильного приложения рекламной площадки, предназначенная для размещения там платного содержимого. Бывает разных типов в зависимости от вида места области размещения.

* **any placement  -** место размещения рекламы, которое может находится в любом разделе веб сайта или мобильного приложения
* **product placement** - место размещения в контексте карточки товара
* **category placement** - место размещения в контексте карточки категории
* **search placement** - место размещения в контексте поисковой выдачи
* **product group placement** - место размещения в контексте группы товаров, например это может быть корзина

**Товар \(product\)**: позиция товарного каталога рекламной площадки. Обладает идентификатором `productId` типа 64 разрядное целое со знаком.

**Категория \(category\)**: группа товаров, содержащая произвольный \(существенный для рекламной площадки\) признак. Обладает идентификатором `categoryId` типа 64 разрядное целое со знаком. Предоставляется рекламной площадкой.

## Base URL

`https://visitors-sp.retailrocket.net/v1/`

## Resources

### Impression for AnyPlacement

#### Path
`partners/{partnerId}/anyPlacements/{placementId}/impressions`

#### Http Method
`GET`

#### Параметры пути запроса
|Имя поля           |Тип|Описание|
|-------------------|---|--------|
|`partnerId`|string|[Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina)|
|`placementId`|string|Идентификатор места размещения, выдается сотрудником Retail Rocket|

#### Параметры строки запроса

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`sessionExternalId`|Да |string      |[Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)|
|`acceptContent`    |Да |string      |Типы содержимого, которые площадка готова разместить, через запятую|
|`apiKey`           |Да |string      |[Ключ авторизации](obshie-principy-integracii-s-retail-rocket#avtorizaciya)        |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

### Impression for ProductPlacement
#### Path
`partners/{partnerId}/productPlacements/{placementId}/impressions`

#### Http Method
`GET`

#### Параметры пути запроса
|Имя поля           |Тип|Описание|
|-------------------|---|--------|
|`partnerId`|string|[Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina)|
|`placementId`|string|Идентификатор места размещения, выдается сотрудником Retail Rocket|

#### Параметры строки запроса

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`sessionExternalId`|Да |string      |[Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)|
|`acceptContent`    |Да |string      |Типы содержимого, которые площадка готова разместить, через запятую|
|`apiKey`           |Да |string      |[Ключ авторизации](obshie-principy-integracii-s-retail-rocket#avtorizaciya)        |
|`productId`        |Да |integer     |Идентификатор товара, в контексте которого находится пользователь|
|`stockId`          |Нет|string      |Идентификатор склада, в контексте которого находится пользователь|


#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

### Impression for ProductGroupPlacement
#### Path
`partners/{partnerId}/productGroupsPlacements/{placementId}/impressions`

#### Http Method
`GET`

#### Параметры пути запроса
|Имя поля           |Тип|Описание|
|-------------------|---|--------|
|`partnerId`|string|[Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina)|
|`placementId`|string|Идентификатор места размещения, выдается сотрудником Retail Rocket|

#### Параметры строки запроса

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`sessionExternalId`|Да |string      |[Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)|
|`acceptContent`    |Да |string      |Типы содержимого, которые площадка готова разместить, через запятую|
|`apiKey`           |Да |string      |[Ключ авторизации](obshie-principy-integracii-s-retail-rocket#avtorizaciya)        |
|`productIds`       |Да |string      |Идентификаторы товаров, которые вхояд в некоторую группу (например корзину), перечисленные через запятую|
|`stockId`          |Нет|string      |Идентификатор склада, в контексте которого находится пользователь|

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

### Impression for CategoryPlacement

#### Path
`partners/{partnerId}/categoryPlacements/{placementId}/impressions`

#### Http Method
`GET`

#### Параметры пути запроса
|Имя поля           |Тип|Описание|
|-------------------|---|--------|
|`partnerId`|string|[Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina)|
|`placementId`|string|Идентификатор места размещения, выдается сотрудником Retail Rocket|

#### Параметры строки запроса

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`sessionExternalId`|Да |string      |[Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)|
|`acceptContent`    |Да |string      |Типы содержимого, которые площадка готова разместить, через запятую|
|`apiKey`           |Да |string      |[Ключ авторизации](obshie-principy-integracii-s-retail-rocket#avtorizaciya)        |
|`categoryId`       |Да |integer     |Идентификатор категории, в контексте которой находится пользователь|

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

### Impression for SearchPlacement


#### Path
`partners/{partnerId}/searchPlacements/{placementId}/impressions`

#### Http Method
`GET`

#### Параметры пути запроса
|Имя поля           |Тип|Описание|
|-------------------|---|--------|
|`partnerId`|string|[Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina)|
|`placementId`|string|Идентификатор места размещения, выдается сотрудником Retail Rocket|

#### Параметры строки запроса

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`sessionExternalId`|Да |string      |[Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei)|
|`acceptContent`    |Да |string      |Типы содержимого, которые площадка готова разместить, через запятую|
|`apiKey`           |Да |string      |[Ключ авторизации](obshie-principy-integracii-s-retail-rocket#avtorizaciya)        |
|`searchQuery`      |Да |string      |Поисковый запрос, который ввел пользователь|


#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

## Responses

### Impression

Содержит данные для показа спонсорского содержимого.

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`id`|Да|string|Идентификатор запрошенного показа|
|`content`|Да|StringImpressionContent or ProductIdsImpressionContent|Содержимое для показа|

### StringImpressionContent

Строковое содержимое для показа

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`id`|Да|string|Идентификатор содержимого|
|`string`|Да|string|Произвольный текст|

### ProductIdsImpressionContent

Cодержимое для показа товарной полки

|Имя поля           |Обязательное|Тип|Описание|
|-------------------|---|------------|--------|
|`id`|Да|string|Идентификатор содержимого|
|`productIds`|Да|number array|Массив идентификаторов товаров для отображения|

### Примеры

Объект типа `Impression` с полем `content` типа строка.

```text
{
    "id": "impression identifier",
    "content": {
        "id": "content identifier",
        "string": "any string"
    }
}
```

Объект типа Impression с полем `content` типа товарная полка.

```text
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
| :--- | :--- | :--- |
| 200 | OK | Запрос принят |
| 204 | NoContent | По запросу ничего не найдено |
| 400 | BadRequest | Ошибка в запросе, необходимо проверить правильность построения запроса |
| 401 | Unauthorized | Ошибка авторизации, необходимо проверить корректность `partnerId`, `apiKey` |
| 403 | Forbidden | Доступ запрещен |
| 404 | NotFound | Запрошенный ресурс не найден, необходимо проверить правильность URL |
| 429 | TooManyRequests | Превышение лимита запросов |

## Время ответа

Ожидаемое время ответа: 100 мс для всех ресурсов \(может отличаться из-за особенностей сетевой инфраструктуры\).

## Ограничения

По умолчанию действует ограничение в 50 запросов/секунду для каждой рекламной площадки. При необходимости возможно изменить \(с помощью аккаунт менеджера\).

