# API получения контента спонсорских размещений

## Общие рекомендации по интеграции

При интеграции API на рекламную площадку, необходимо помнить, что обращение к внешнему API всегда может закончиться неудачно.
Причин для этого множество - нестабильность сетевой инфраструктуры, блокировки Роскомнадзора, ошибки на стороне сервера или просто человеческий фактор.
Поэтому мы рекомендуем учитывать возможность получения нестандартного ответа, и прорабатывать это с точки зрения внешнего вида веб-сайта или мобильного приложения.
Это особенно важно для главной страницы - если промо материал занимает большую часть экрана, то, возможно, следует предусмотреть заполнение этого пространства содержимым по умолчанию.

Обязательно нужно устанавливать какой-то разумный таймаут на все внешние запросы, чтобы сохранить отзывчивость интерфейса.

Так же, стоит не забывать, что получение пустого промо материала (ответ 204)  - это вполне стандартная и частая ситуация, которая возникает, если для запрашиваемого сегмента пользователей не запущено ни одной кампании.

Стоит предусмотреть возможность того, что результат запроса может иметь неожиданный формат. Кампании могут предоставлять разные варианты содержимого, представленные в различной форме. Это могут быть баннеры, товарные полки, брендированные товарные полки, и т.д. Для кажого варианта содержимого будет своя логика отрисовки и учета факта просмотра.
Варианты этого содержимого всегда ограничиваются параметром `acceptContent`, что гарантирует предсказумость результата запроса, однако общей практикой является предусмотреть и обработать неизвестный формат содержимого.

## Термины

### **Рекламная площадка**

Магазин, предоставляющий страницы своего web или мобильного приложения для размещения рекламных объявлений. Обладает идентификатором `partnerId`, который можно получить в личном кабинете или у аккаунт-менеджера.

**Рекламодатель**: лицо/организация, которая заинтересована в том, чтобы показывать на рекламных площадках свое содержимое, чтобы достигать маркетинговые цели.

### **Содержимое для показа**

Объект, содержащий необходимые данные для показа пользователю рекламной площадки. Запрашивается рекламной площадкой у API спонсорских размещений. Показывается рекламной площадкой с целью получить плату от рекламодателя за просмотр этого содержимого пользователем площадки.

При запросе необходимо использовать параметр строки запроса `acceptContent` для указания типа содержимого, который рекламная площадка готова разместить.

Значения `acceptContent`:

* `string`
* `productIds`
* `banners`
* `sharedBanners`

Если рекламная площадка готова размещать более одного варианта содержимого, их можно указать через запятую, например: `acceptContent=string,productIds`

### **Рекламное место**
****
Область страницы web-приложения или экрана мобильного приложения рекламной площадки, предназначенная для размещения там платного содержимого. Бывает разных типов в зависимости от вида, места, области размещения:

* **any placement  –** место размещения рекламы, которое может находиться в любом разделе веб-сайта или мобильного приложения;
* **product placement** – место размещения в контексте карточки товара;
* **category placement** – место размещения в контексте категории;
* **search placement** – место размещения в контексте поисковой выдачи;
* **product group placement** – место размещения в контексте группы товаров, например, это может быть корзина или страница успешного завершения заказа.

### **Товар**

Позиция товарного каталога рекламной площадки. Обладает идентификатором `productId` типа  «64-разрядное целое со знаком».

### **Категория**

Группа товаров, содержащая произвольный (существенный для рекламной площадки) признак. Обладает идентификатором `categoryId` типа  «64-разрядное целое со знаком». Предоставляется рекламной площадкой.

## Base URL <a href="#acceptcontent" id="acceptcontent"></a>

`https://visitors-sp.retailrocket.net/v1/`

## Resources

### Спонсорский контент для товарных страниц

#### Path

`partners/{partnerId}/productPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля      | Тип    | Описание                                                                                                                                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partnerId`   | string | [Идентификатор интернет-магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения. Выдается сотрудником Retail Rocket                                                                                                                                                  |

#### Параметры строки запроса

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                                                                          |
| ------------------- | ------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent`     | Да           | string  | Типы содержимого, которые площадка готова разместить. Через запятую                                                                                                                               |
| `apiKey`            | Да           | string  | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya)                 |
| `productId`         | Да           | integer | [Идентификатор товара, в контексте которого находится пользователь](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                             |
| `stockId`           | Нет          | string  | [Идентификатор склада, в контексте которого находится пользователь](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                             |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/productPlacements/a2837ec9-b000-46d7-9272-f64df080da51/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&productId=93845&stockId=berlin13'
```

### Спонсорский контент для страниц групповых товаров

#### Path

`partners/{partnerId}/productGroupPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля      | Тип    | Описание                                                                                                                                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partnerId`   | string | [Идентификатор интернет-магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения. Выдается сотрудником Retail Rocket                                                                                                                                                  |

#### Параметры строки запроса

| Имя поля            | Обязательное | Тип          | Описание                                                                                                                                                                                          |
| ------------------- | ------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent`     | Да           | string       | Типы содержимого, которые площадка готова разместить. Через запятую                                                                                                                               |
| `apiKey`            | Да           | string       | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya)                 |
| `productIds`        | Да           | number array | [Список идентификаторов товаров](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare)                                         |
| `stockId`           | Нет          | string       | [Идентификатор склада, в контексте которого находится пользователь](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                             |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/productGroupPlacements/545f2f85-dcbe-4d2d-8260-6ecdf6c8c415/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&productIds=93845,93846,93847&stockId=berlin13'
```

### Спонсорский контент для страниц товарных категорий

{% hint style="warning" %}
В зависимости от того, как выполнена интеграция [товарной базы](../#tovarnaya-baza), необходимо передавать определенный тип идентификатора категории в запросе спонсорского контента для страницы товарной категории.

[Интеграции через YML-файл](api-sponsorskikh-razmeshenii.md#pri-integracii-cherez-yml-fail)

[Интеграции через Product API](api-sponsorskikh-razmeshenii.md#pri-integracii-cherez-product-api)
{% endhint %}

#### При интеграции через YML-файл

В YML файле доступно дерево категорий с целочисленными идентификаторами категории, поэтому и товары имеют целочисленные идентификаторы категорий, что позволяет использовать такие идентификаторы для запроса спонсорского контента для страниц товарных категорий.

#### Path

`partners/{partnerId}/categoryPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля      | Тип    | Описание                                                                                                                                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partnerId`   | string | [Идентификатор интернет-магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения. Выдается сотрудником Retail Rocket                                                                                                                                                  |

#### Параметры строки запроса

| Имя поля            | Обязательное | Тип     | Описание                                                                                                                                                                                          |
| ------------------- | ------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string  | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent`     | Да           | string  | Типы содержимого, которые площадка готова разместить. Через запятую                                                                                                                               |
| `apiKey`            | Да           | string  | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya)                 |
| `categoryId`        | Да           | integer | [Идентификатор категории, в контексте которой находится пользователь](../#derevo-tovarnykh-kategorii)                                                                                             |
| `stockId`           | Нет          | string  | [Идентификатор склада, в контексте которого находится пользователь](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                             |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/categoryPlacements/b17d8910-d0c5-4fd7-97d0-66b314f797f2/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&categoryId=65'
```

#### При интеграции через Product API

При использовании Product API категория передается как `categoryPath,` она должна быть передана в таком же виде и при запросе спонсорского контента для страницы товарной категории.

#### Path

`partners/{partnerId}/categoryPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля      | Тип    | Описание                                                                                                                                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partnerId`   | string | [Идентификатор интернет-магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения. Выдается сотрудником Retail Rocket                                                                                                                                                  |

#### Параметры строки запроса

| Имя поля            | Обязательное | Тип    | Описание                                                                                                                                                                                          |
| ------------------- | ------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent`     | Да           | string | Типы содержимого, которые площадка готова разместить. Через запятую                                                                                                                               |
| `apiKey`            | Да           | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya)                 |
| `categoryPath`      | Да           | string | [Путь категории товара](../#derevo-tovarnykh-kategorii)                                                                                                                                           |
| `stockId`           | Нет          | string | [Идентификатор склада, в контексте которого находится пользователь](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                             |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/categoryPlacements/b17d8910-d0c5-4fd7-97d0-66b314f797f2/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&categoryPath=Electronics%2FTV'
```

### Спонсорский контент для страницы поиска

#### Path

`partners/{partnerId}/searchPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля      | Тип    | Описание                                                                                                                                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partnerId`   | string | [Идентификатор интернет-магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения. Выдается сотрудником Retail Rocket                                                                                                                                                  |

#### Параметры строки запроса

| Имя поля            | Обязательное | Тип    | Описание                                                                                                                                                                                          |
| ------------------- | ------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent`     | Да           | string | Типы содержимого, которые площадка готова разместить. Через запятую                                                                                                                               |
| `apiKey`            | Да           | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya)                 |
| `searchQuery`       | Да           | string | Поисковый запрос, который ввел пользователь                                                                                                                                                       |
| `stockId`           | Нет          | string | [Идентификатор склада, в контексте которого находится пользователь](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                             |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/searchPlacements/d34fa7a5-26fe-4cd4-af3f-a52362d90c80/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string&searchQuery=Red%20Apples'
```

### Спонсорский контент для прочих страниц

#### Path

`partners/{partnerId}/anyPlacements/{placementId}/impressions`

#### Http Method

`GET`

#### Параметры пути запроса

| Имя поля      | Тип    | Описание                                                                                                                                                                                                            |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partnerId`   | string | [Идентификатор интернет-магазина](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#identifikator-internet-magazina) |
| `placementId` | string | Идентификатор места размещения. Выдается сотрудником Retail Rocket                                                                                                                                                  |

#### Параметры строки запроса

| Имя поля            | Обязательное | Тип    | Описание                                                                                                                                                                                          |
| ------------------- | ------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#upravlenie-sessiei) |
| `acceptContent`     | Да           | string | Типы содержимого, которые площадка готова разместить, через запятую                                                                                                                               |
| `apiKey`            | Да           | string | [Ключ авторизации](https://github.com/RetailRocket/Documentation/tree/117692a06b3c513e32856a45dee367feafada3cb/obshie-principy-integracii-s-retail-rocket/README.md#avtorizaciya)                 |
| `stockId`           | Нет          | string | [Идентификатор склада, в контексте которого находится пользователь](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                             |

#### **Тип ответа**

* [Impression](api-sponsorskikh-razmeshenii.md#impression)

#### Пример вызова

```bash
curl 'https://visitors-sp.retailrocket.net/v1/partners/608423a9b126ac6ab3f8f0a5/anyPlacements/2e1f1f39-bad1-46a4-9488-c075dd95dc9b/impressions?apiKey=608423a104249fa8e9952323&sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&acceptContent=productIds,string'
```

## Responses

### Impression

Содержит данные для показа спонсорского содержимого. Может принимать разные формы, в зависимости от значения переданного в запросе `acceptContent`.

- `StringImpression` соответствует `acceptContent=string`
- `ProductIdsImpression` соответствует `acceptContent=productIds`
- `BannersImpression` соответствует `acceptContent=banners`
- `SharedBannersImpression` соответствует `acceptContent=sharedBanners`

### StringImpression

| Имя поля      | Обязательное | Тип                     | Описание                                                 |
| ------------- | ------------ | ----------------------- | -------------------------------------------------------- |
| `id`          | Да           | string                  | Идентификатор запрошенного показа                        |
| `contentType` | Нет          | string                  | Тип содержимого. Всегда равен `string`                   |
| `content`     | Да           | StringImpressionContent | [Содержимое для показа](api-sponsorskikh-razmeshenii.md) |

### StringImpressionContent

Строковое содержимое для показа

| Имя поля | Обязательное | Тип    | Описание                  |
| -------- | ------------ | ------ | ------------------------- |
| `id`     | Да           | string | Идентификатор содержимого |
| `string` | Да           | string | Произвольный текст        |

### Примеры

Объект типа `Impression` с полем `content` типа «строка».

```javascript
{
    "id": "impression identifier",
    "contentType": "string",
    "content": {
        "id": "content identifier",
        "string": "any string"
    }
}
```

### ProductIdsImpression

| Имя поля      | Обязательное | Тип                         | Описание                                                 |
| ------------- | ------------ | --------------------------- | -------------------------------------------------------- |
| `id`          | Да           | string                      | Идентификатор запрошенного показа                        |
| `contentType` | Нет          | string                      | Тип содержимого. Всегда равен `productIds`               |
| `content`     | Да           | ProductIdsImpressionContent | [Содержимое для показа](api-sponsorskikh-razmeshenii.md) |

### ProductIdsImpressionContent

Cодержимое для показа товарной полки

| Имя поля     | Обязательное | Тип          | Описание                                       |
| ------------ | ------------ | ------------ | ---------------------------------------------- |
| `id`         | Да           | string       | Идентификатор содержимого                      |
| `productIds` | Да           | number array | Массив идентификаторов товаров для отображения |

### Примеры

Объект типа Impression с полем `content` типа «товарная полка».

```javascript
{
    "id": "impression identifier",
    "contentType": "productIds",
    "content": {
        "id": "content identifier",
        "productIds": [123, 321]
    }
}
```

### BannersImpression

| Имя поля      | Обязательное | Тип                      | Описание                                                 |
| ------------- | ------------ | ------------------------ | -------------------------------------------------------- |
| `id`          | Да           | string                   | Идентификатор запрошенного показа                        |
| `contentType` | Нет          | string                   | Тип содержимого. Всегда равен `banners`                  |
| `content`     | Да           | BannersImpressionContent | [Содержимое для показа](api-sponsorskikh-razmeshenii.md) |

### BannersImpressionContent

Cодержимое для показа баннеров

| Имя поля  | Обязательное | Тип          | Описание                        |
| --------- | ------------ | ------------ | ------------------------------- |
| `id`      | Да           | string       | Идентификатор содержимого       |
| `banners` | Да           | Banner array | Массив баннеров для отображения |

### Banner

Информация о баннере

| Имя поля     | Обязательное | Тип    | Описание                             |
| ------------ | ------------ | ------ | ------------------------------------ |
| `targetUrl`  | Нет          | string | URL для перехода при клике на баннер |
| `pictureUrl` | Да           | string | URL изображения                      |

### Примеры

Объект типа Impression с полем `content` типа «баннеры».

```javascript
{
    "id": "impression identifier",
    "contentType": "banners",
    "content": {
        "id": "content identifier",
        "banners": [
            {
                "targetUrl" : "http://host/path",
                "pictureUrl": "http://host/path/image.png"
            },
            {
                "targetUrl" : "http://host/path",
                "pictureUrl": "http://host/path/image.png"
            },
        ]
    }
}
```

### SharedBannersImpression

Содержимое для показа общих баннеров. Отличается от BannersImpression тем, что тут могут присутствовать баннеры разных рекламодателей.

| Имя поля      | Обязательное | Тип                           | Описание                                                 |
| ------------- | ------------ | ----------------------------- | -------------------------------------------------------- |
| `id`          | Да           | string                        | Идентификатор запрошенного показа                        |
| `contentType` | Нет          | string                        | Тип содержимого. Всегда равен `sharedBanners`            |
| `content`     | Да           | BannerImpressionContent array | [Содержимое для показа](api-sponsorskikh-razmeshenii.md) |

### BannerImpressionContent

Cодержимое для показа баннера

| Имя поля | Обязательное | Тип    | Описание                  |
| -------- | ------------ | ------ | ------------------------- |
| `id`     | Да           | string | Идентификатор содержимого |
| `banner` | Да           | Banner | Баннер для отображения    |

### Примеры

Объект типа Impression с полем `content` типа «общие баннеры».

```javascript
{
    "id": "impression identifier",
    "contentType": "sharedBanners",
    "content": [
        {
            "id": "content identifier",
            "banner": {
                "targetUrl" : "http://host/path",
                "pictureUrl": "http://host/path/image.png"
            }
        },
        {
            "id": "content identifier",
            "banner": {
                "targetUrl" : "http://host/path",
                "pictureUrl": "http://host/path/image.png"
            }
        }
    ]
}
```

## Status codes

| Код ответа | Название        | Описание                                                                    |
| ---------- | --------------- | --------------------------------------------------------------------------- |
| 200        | OK              | Запрос принят                                                               |
| 204        | NoContent       | По запросу ничего не найдено                                                |
| 400        | BadRequest      | Ошибка в запросе, необходимо проверить правильность построения запроса      |
| 401        | Unauthorized    | Ошибка авторизации, необходимо проверить корректность `partnerId`, `apiKey` |
| 403        | Forbidden       | Доступ запрещен                                                             |
| 404        | NotFound        | Запрошенный ресурс не найден, необходимо проверить правильность URL         |
| 429        | TooManyRequests | Превышение лимита запросов                                                  |

## Время ответа

Ожидаемое время ответа – 100 мс для всех ресурсов (может отличаться из-за особенностей сетевой инфраструктуры).

## Ограничения

По умолчанию действует ограничение в 50 запросов/секунду для каждой рекламной площадки. При необходимости возможно изменить (с помощью аккаунт-менеджера).
