---
description: >-
  В документе описываются принципы и способы получения контента спонсорского
  контента.
---

# API получения спонсорских размещений

## Термины

**Рекламная площадка**: магазин, предоставляющий страницы своего web или мобильного приложения для размещения рекламных объявлений. Обладает идентификатором `partnerId`. Можно получить в личном кабинете или у аккаунт менеджера.

**Рекламодатель**: лицо/организация, которая заинтересована в том, чтобы показывать на рекламных площадках свое содержимое, чтобы достигать маркетинговые цели.

**Содержимое для показа \(content\)**: объект произвольного состава, содержащий необходимые данные для показа пользователю рекламной площадки. Запрашивается рекламной площадкой у API спонсорских размещений. Показывается рекламной площадкой с целью получить плату от рекламодателя за просмотр этого сдержимого пользователем площадки.

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

`https://visitors-sp.retailrocket.net/v1/partners/{partnerId}`

## Resources

### Impression for AnyPlacement

`GET /anyPlacements/{placementId}/impressions`

#### Параметры строки запроса

* `session` или `sessionExternalId`
* `acceptContent`
* `apiKey`

#### **Тип ответа**

#### \*\*\*\*[impression](api-sponsorskikh-razmeshenii.md#impression)

### Impression for ProductPlacement

`GET /productPlacements/{placementId}/impressions`

#### Параметры строки запроса

* `session` или `sessionExternalId`
* `productId`
* `acceptContent`
* `apiKey`
* `stockId` \(необязательный параметр\)

  **Тип ответа**

  `impression`

### Impression for ProductGroupPlacement

`GET /productGroupsPlacements/{placementId}/impressions`

#### Параметры строки запроса

* `session` или `sessionExternalId`
* `productIds`\(строка, состоящая из `productId` через запятую\)
* `acceptContent`
* `apiKey`
* `stockId` \(необязательный параметр\)

  **Тип ответа**

  `impression`

### Impression for CategoryPlacement

`GET /categoryPlacements/{placementId}/impressions`

#### Параметры строки запроса

* `session` или `sessionExternalId`
* `categoryId`
* `acceptContent`
* `apiKey`

#### **Тип ответа**

`impression`

### Impression for SearchPlacement

`GET /searchPlacements/{placementId}/impressions`

#### Параметры строки запроса

* `session` или `sessionExternalId`
* `searchQuery`
* `acceptContent`
* `apiKey`

#### **Тип ответа**

`impression`

## Response

### Impression

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

