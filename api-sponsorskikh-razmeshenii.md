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

## Base URL <a id="acceptcontent"></a>

`https://visitors-sp.retailrocket.net/v1/`

## Resources

### Спонсорский контент для товарных страниц/экранов

#### Path

`partners/{partnerId}/productPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля | Тип | Описание |
| :--- | :--- | :--- |
| `partnerId` | string | [Идентификатор интернет магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения, выдается сотрудником Retail Rocket |

#### Параметры строки запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent` | Да | string | Типы содержимого, которые площадка готова разместить, через запятую |
| `apiKey` | Да | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya) |
| `productId` | Да | integer | Идентификатор товара, в контексте которого находится пользователь |
| `stockId` | Нет | string | Идентификатор склада, в контексте которого находится пользователь |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/productPlacements/a2837ec9-b000-46d7-9272-f64df080da51/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&productId=93845&stockId=berlin13'
```

### Спонсорский контент для страниц/экранов груповых товаров

#### Path

`partners/{partnerId}/productGroupsPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля | Тип | Описание |
| :--- | :--- | :--- |
| `partnerId` | string | [Идентификатор интернет магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения, выдается сотрудником Retail Rocket |

#### Параметры строки запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent` | Да | string | Типы содержимого, которые площадка готова разместить, через запятую |
| `apiKey` | Да | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya) |
| `productIds` | Да | string | Идентификаторы товаров, которые вхояд в некоторую группу \(например корзину\), перечисленные через запятую |
| `stockId` | Нет | string | Идентификатор склада, в контексте которого находится пользователь |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/productGroupsPlacements/545f2f85-dcbe-4d2d-8260-6ecdf6c8c415/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&productIds=93845,93846,93847&stockId=berlin13'
```

### Спонсорский контент для страниц/экранов товарных категорий

#### Path

`partners/{partnerId}/categoryPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля | Тип | Описание |
| :--- | :--- | :--- |
| `partnerId` | string | [Идентификатор интернет магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения, выдается сотрудником Retail Rocket |

#### Параметры строки запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent` | Да | string | Типы содержимого, которые площадка готова разместить, через запятую |
| `apiKey` | Да | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya) |
| `categoryId` | Да | integer | Идентификатор категории, в контексте которой находится пользователь |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/categoryPlacements/b17d8910-d0c5-4fd7-97d0-66b314f797f2/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&categoryId=65'
```

### Спонсорский контент для страницы/экрана поиска

#### Path

`partners/{partnerId}/searchPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля | Тип | Описание |
| :--- | :--- | :--- |
| `partnerId` | string | [Идентификатор интернет магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения, выдается сотрудником Retail Rocket |

#### Параметры строки запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent` | Да | string | Типы содержимого, которые площадка готова разместить, через запятую |
| `apiKey` | Да | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya) |
| `searchQuery` | Да | string | Поисковый запрос, который ввел пользователь |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/searchPlacements/d34fa7a5-26fe-4cd4-af3f-a52362d90c80/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&searchQuery=Red%20Apples'
```

## Responses

### Impression

Содержит данные для показа спонсорского содержимого.

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `id` | Да | string | Идентификатор запрошенного показа |
| `content` | Да | StringImpressionContent or ProductIdsImpressionContent | Содержимое для показа |

### StringImpressionContent

Строковое содержимое для показа

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `id` | Да | string | Идентификатор содержимого |
| `string` | Да | string | Произвольный текст |

### ProductIdsImpressionContent

Cодержимое для показа товарной полки

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `id` | Да | string | Идентификатор содержимого |
| `productIds` | Да | number array | Массив идентификаторов товаров для отображения |

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

### Спонсорский контент для прочих страниц/экранов

#### Path

`partners/{partnerId}/anyPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля | Тип | Описание |
| :--- | :--- | :--- |
| `partnerId` | string | [Идентификатор интернет магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения, выдается сотрудником Retail Rocket |

#### Параметры строки запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent` | Да | string | Типы содержимого, которые площадка готова разместить, через запятую |
| `apiKey` | Да | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya) |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/anyPlacements/2e1f1f39-bad1-46a4-9488-c075dd95dc9b/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string'
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

