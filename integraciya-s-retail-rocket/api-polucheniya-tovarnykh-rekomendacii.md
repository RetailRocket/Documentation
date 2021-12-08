# API получения товарных рекомендаций

## **Общие концепции**

Для получение товарных рекомендаций от системы Retail Rocket необходимо обратиться к ресурсу, соответствующему типу рекомендаций.&#x20;

API получения товарных рекомендаций **** придерживается [общих принципов интеграционных API](obshie-principy-integracii-s-retail-rocket.md).&#x20;

{% hint style="info" %}
**Товарные параметры**

Параметр `stockId` требуется для того, чтобы рекомендации содержали только товары в наличии на конкретном складе (регионе). Значение параметра, как и все товарные параметры, должно соответствовать значениям, переданным в Retail Rocket с товарной базой.
{% endhint %}

{% hint style="info" %}
**Лимит частоты обращения к ресурсам**

По умолчанию установлен лимит в 100 обращений в секунду на магазин (`partnerId`) в целом. Для изменения лимита необходимо обратиться к вашему аккаунт-менеджеру или через форму поддержки в личном кабинете.
{% endhint %}

{% hint style="warning" %}
**Управление кешированием**

Вместе с возвращаемыми данными передается http-заголовок `Cache-Control,` которому необходимо следовать при кеширование полученных данных.
{% endhint %}

### **Base URL**

`https://externalapi.retailrocket.net/api/3.0/`

### HTTP-метод доступа

Все ресурсы API доступны только по `GET` методу

### HTTP-статусы обращения

Успешность обращение к ресурсу можно оценить по полученному HTTP-статусу:

* **200 OK** - запрос принят;
* **400 BadRequest** - ошибка в запросе, необходимо проверить правильность построения запроса;
* **401 Unauthorized** - ошибка аутентификации, проверьте корректность пары `partnerId`, `apiKey;`
* **403 Forbidden** - доступ запрещен;
* **404 NotFound** - запрошенный ресурс не найден, необходимо проверить правильность URL;
* **429 TooManyRequest** - слишком много запросов и/или лимит на количество запросов в интервал времени превышен;

### Возвращаемое значение

В случае успешного обращения к любому ресурсу, HTTP-статус будет 200 Ok, а в теле запроса будет возвращена строка, содержащая JSON-объект.

{% tabs %}
{% tab title="Пример возвращаемых данных" %}
```javascript
{
    "recommendations":
    [
        {
            "productId": [53242, 41231]
        }
    ]
}
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    "$schema": "https://json-schema.org/draft/2019-09/schema",
    "type": "object",
    "properties": {
        "recommendations": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "productId": {
                        "type": "array",
                        "items": {
                            "type": "integer"
                        }
                    }
                }
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

Если рекомендаций по этим параметром нет, то вернётся пустой массив `recommendations`

## Типы рекомендаций

### **Популярные товары для главного экрана**

Рассчитаны на основании поведения пользователя во всем интернет-магазине, без привязки к конкретным категориям, товарам, поисковым фразам т.д.&#x20;

Представляет из себя актуальные бестселлеры магазина.

#### **Path**

**`partnerRecommendations/popular`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип    | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/partnerRecommendations/popular/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017'
```

### **Популярные товары для экрана** товарной категории

Предназначен для показа пользователю самых популярных товаров в рамках категорийной группы. Учитываются товары и пользовательское поведение в рамках конкретной категории и всех ее подкатегорий.

{% hint style="warning" %}
В зависимости от того, как выполнена интеграция передачи [товарной базы](obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza), необходимо вызывать разные методы для получения товарных рекомендаций для экрана товарной категории.

[Интеграции через YML-файл](api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-yml-fail)

[Интеграции через Product API](api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-product-api)&#x20;
{% endhint %}

#### При интеграции через YML-файл

В YML файле доступно дерево категорий с целочисленными идентификаторами категории, поэтому и товары имеют целочисленные идентификаторы категорий, что позволяет использовать такие идентификаторы для запроса на получение товарных рекомендаций категории.

#### Path

**`categoryRecommendations/popular`**

#### Параметры строки запроса

| Имя параметра       | Обязательное |      Тип     | Описание                                                                                                                                                                   |
| ------------------- | ------------ | :----------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           |    string    | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           |    string    | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           |    string    | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `categoryIds`       | Да           | number array | [Список идентификаторов товарных категорий, записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                    |
| `stockId`           | Нет          |    string    | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/popular/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&categoryIds=123,234,254'
```

#### При интеграции через Product API

При использовании product API категория передается как `categoryPath` также она должна быть передана и в метод для получения товарных рекомендаций.

#### Path

**`categoryRecommendations/popularByCategoryPath`**

#### Параметры строки запроса

| Имя параметра       | Обязательное |      Тип     | Описание                                                                                                                                                                   |
| ------------------- | ------------ | :----------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           |    string    | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           |    string    | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           |    string    | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `categoryPaths`     | Да           | string array | [Список путей товарных категорий, записанных как несколько параметров через &](obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza)                               |
| `stockId`           | Нет          |    string    | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/popularByCategoryPath/?sessionExternalId=73018148&categoryPaths=katinwinkel&categoryPaths=accu-gereedschap%2Faccu&apiKey=5b333f5697a528b0184b6017&partnerId=59908d02c7d013ce40de715a&session=5bb7b6201323790001561024'
```

### **Популярные товары со скидкой для экрана** товарной категории

Предназначен для показа пользователю самых популярных товаров в рамках категорийной группы с учетом скидки. Учитываются товары со скидкой и пользовательское поведение в рамках конкретной категории и всех ее подкатегорий.

{% hint style="warning" %}
В зависимости от того, как выполнена интеграция передачи [товарной базы](obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza), необходимо вызывать разные методы для получения товарных рекомендаций для экрана товарной категории.

[Интеграции через YML-файл](api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-yml-fail-1)

[Интеграции через Product API ](api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-product-api-1)
{% endhint %}

#### При интеграции через YML-файл

В YML файле доступно дерево категорий с целочисленными идентификаторами категории, поэтому и товары имеют целочисленные идентификаторы категорий, что позволяет использовать такие идентификаторы для запроса на получение товарных рекомендаций категории.

#### Path

**`categoryRecommendations/saleByPopular`**

#### Параметры строки запроса

| Имя параметра       | Обязательное |      Тип     | Описание                                                                                                                                                                   |
| ------------------- | ------------ | :----------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           |    string    | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           |    string    | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           |    string    | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `categoryIds`       | Да           | number array | [Список идентификаторов товарных категорий, записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                    |
| `stockId`           | Нет          |    string    | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/saleByPopular/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&categoryIds=123,234,254'
```

#### При интеграции через Product API

При использовании product API категория передается как `categoryPath` также она должна быть передана и в метод для получения товарных рекомендаций.

#### Path

**`categoryRecommendations/saleByPopularByCategoryPath`**

#### Параметры строки запроса

| Имя параметра       | Обязательное |      Тип     | Описание                                                                                                                                                                   |
| ------------------- | ------------ | :----------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           |    string    | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           |    string    | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           |    string    | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `categoryPaths`     | Да           | string array | [Список путей товарных категорий, записанных как несколько параметров через &](obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza)                               |
| `stockId`           | Нет          |    string    | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/saleByPopularByCategoryPath/?sessionExternalId=73018148&categoryPaths=katinwinkel&categoryPaths=accu-gereedschap%2Faccu&apiKey=5b333f5697a528b0184b6017&partnerId=59908d02c7d013ce40de715a&session=5bb7b6201323790001561024'
```

### **Поисковые рекомендации**

Предназначен для показа пользователю товаров, наиболее подходящих под его поисковый запрос. Рекомендации рассчитываются на основе поведения других пользователей с подобным поисковым запросом и не учитывают смысл поисковых фраз.

Поисковые рекомендации следует применять там, где известен поисковый запрос пользователя. Обычно это поисковый экран интернет-магазина. Рекомендации показывают свою эффективность, как и в роли дополнения к поисковой выдаче, так и в случае отсутствия поисковой выдачи.

#### **Path**

**`searchRecommendations/search`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип    | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `searchPhrase`      | Да           | string | Поисковый запрос пользователя                                                                                                                                              |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/searchRecommendations/search/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&searchPhrase=skirts'
```

### **Похожие товары**

Предназначен для показа товаров, наиболее похожих на просматриваемый пользователем товар. Рекомендации рассчитываются на основе поведения других пользователей и учитывают параметры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### **Path**

**`productRecommendations/alternatives`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `productIds`        | Да           | number array | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                 |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/alternatives/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=93845,93846,93847'
```

### **Сопутствующие товары**

Предназначен для показа комплементарных товаров к товару или товарам. Рекомендации рассчитываются на основание поведения других пользователей и учитывают параметры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### **Path**

**`productRecommendations/related`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `productIds`        | Да           | number array | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                 |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/relatedByInterestedProperties/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=93845,93846,93847'
```

### **Сопутствующие товары, персонализированные с учетом интереса пользователя к свойствам товара**

Предназначен для показа комплементарных товаров к товару или товарам, **** которые дополнительно ранжированны в зависимости от интереса пользователей к конкретным свойствам товара (размер, цвет). Рекомендации рассчитываются на основании поведения других пользователей и учитывают параметры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### **Path**

**`productRecommendations/relatedByInterestedProperties`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `productIds`        | Да           | number array | [Список идентификаторов товара, записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/relatedByInterestedProperties/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=93845,93846,93847'
```

### **Персональные рекомендации**

Алгоритм персональных рекомендаций возвращает наиболее интересные товары для пользователя.

#### **Path**

**`partnerRecommendations/personalComposite`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип    | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/partnerRecommendations/personalComposite/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017'
```

### **Ранее купленные пользователем товары**

Алгоритм возвращает ранее заказанные пользователем товары.

#### **Path**

**`partnerRecommendations/personalOrdered`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип    | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/partnerRecommendations/personalOrdered/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017''
```

### **Аксессуары**

Предназначен для показа товаров из аксессуарных категорий, которые приобретаются совместно с товаром или товарами, к которым запрашиваются рекомендации. Рекомендации рассчитываются на основании поведения других пользователей и учитывают параметры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### Path

**`productRecommendations/accessories`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `productIds`        | Да           | number array | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                 |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/accessories/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=93845,93846,93847'
```

### Популярные товары к интересующим пользователя категориям

Retail Rocket в реальном времени анализирует интересы пользователя и строит связи «пользователь» – «категория». Это позволяет демонстрировать хиты продаж только из категорий, которые наиболее интересны данному пользователю в долгосрочной перспективе.

#### Path

**`partnerRecommendations/popularInInterestedCategories`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип    | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/partnerRecommendations/popularInInterestedCategories/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017'
```

### Просмотренные товары

Алгоритм возвращает просмотренные пользователем товары.

#### Path

**`partnerRecommendations/personalViewed`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип    | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/partnerRecommendations/personalViewed/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017'
```

### Сопутствующие товары из категорий, отличных от категории текущего товара

Алгоритм возвращает сопутствующие товарные позиции, которые с максимальной вероятностью могут быть куплены вместе с товаром, к которому запрошены рекомендации. Из рекомендаций сопутствующих товаров удаляются товары, принадлежащие той же категории, что и текущий товар.

#### Path

**`productRecommendations/relatedWithoutCurrentCategory`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `productIds`        | Да           | number array | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                 |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/relatedWithoutCurrentCategory/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=1254,2345,5484'
```

### Сопутствующие товары с учетом бизнес-правил

Алгоритм возвращает сопутствующие товарные позиции, которые с максимальной вероятностью могут быть куплены вместе с товаром, к которому запрошены рекомендации. При этом данный алгоритм учитывает бизнес-правила и позволяет:

* Продвинуть товары из непопулярных категорий (например, новая категория товаров в ассортименте);
* Реализовать “экспертные рекомендации” при продаже технически сложных товаров, когда покупатель может не знать что именно нужно докупить учесть пожелания партнера к подбору сопутствующих товаров;

#### Path

**`productRecommendations/relatedWithRules`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `productIds`        | Да           | number array | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                                 |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/relatedWithRules/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=1254,2345,5484'
```

### Новинки в категории

Алгоритм возвращает новинки в категории.

{% hint style="warning" %}
В зависимости от того, как выполнена интеграция передачи [товарной базы](obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza), необходимо вызывать разные методы для получения товарных рекомендаций для экрана товарной категории.

[Интеграции через YML-файл](api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-yml-fail-2)

[Интеграции через Product API ](api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-product-api-2)
{% endhint %}

#### При интеграции через YML-файл

В YML файле доступно дерево категорий с целочисленными идентификаторами категории, поэтому и товары имеют целочисленные идентификаторы категорий, что позволяет использовать такие идентификаторы для запроса на получение товарных рекомендаций категории.

#### Path

**`categoryRecommendations/latest`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `categoryIds`       | Да           | number array | [Список идентификаторов товарных категорий, записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare)                                    |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/latest/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&categoryIds=1254,2345,5484'
```

#### При интеграции через Product API

При использовании product API категория передается как `categoryPath` также она должна быть передана и в метод для получения товарных рекомендаций.

#### Path

**`categoryRecommendations/latestByCategoryPath`**

#### Параметры строки запроса

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                   |
| ------------------- | ------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                                                             |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                                                           |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                                                             |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region) |
| `categoryPaths`     | Да           | string array | [Список путей товарных категорий, записанных как несколько параметров через &](obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza)                               |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/latestByCategoryPath/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&categoryPaths=accu-gereedschap%2Faccu'
```

### Новинки со скидками

Алгоритм возвращает новинки по категориям со скидками.

{% hint style="warning" %}
В зависимости от того, как выполнена интеграция передачи [товарной базы](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza), необходимо вызывать разные методы для получения товарных рекомендаций для экрана товарной категории.

[Интеграции через YML-файл](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-yml-fail-2)

[Интеграции через Product API ](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/api-polucheniya-tovarnykh-rekomendacii.md#pri-integracii-cherez-product-api-2)
{% endhint %}

**При интеграции через YML-файл**

В YML файле доступно дерево категорий с целочисленными идентификаторами категории, поэтому и товары имеют целочисленные идентификаторы категорий, что позволяет использовать такие идентификаторы для запроса на получение товарных рекомендаций категории.

**Path**

**`categoryRecommendations/saleByLatest`**

**Параметры строки запроса**

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                                                           |
| ------------------- | ------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `apiKey`            | Да           | string       | [Ключ авторизации](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                          |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                        |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                          |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region)                                         |
| `categoryIds`       | Да           | number array | [Список идентификаторов товарных категорий, записанных через запятую](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) |

**Пример вызова**

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/saleByLatest/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&categoryIds=1254,2345,5484'
```

**При интеграции через Product API**

При использовании product API категория передается как `categoryPath` также она должна быть передана и в метод для получения товарных рекомендаций.

**Path**

**`categoryRecommendations/saleByLatestByCategoryPath`**

**Параметры строки запроса**

| Имя параметра       | Обязательное | Тип          | Описание                                                                                                                                                                                                                |
| ------------------- | ------------ | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apiKey`            | Да           | string       | [Ключ авторизации](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#avtorizaciya)                                                               |
| `partnerId`         | Да           | string       | [Идентификатор интернет-магазина](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina)                             |
| `sessionExternalId` | Да           | string       | [Идентификатор пользователя](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei)                                               |
| `stockId`           | Нет          | string       | [Идентификатор склада/региона](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#podderzhka-regionalnosti-sklad-region)                                              |
| `categoryPaths`     | Да           | string array | [Список путей товарных категорий, записанных как несколько параметров через &](https://app.gitbook.com/s/-MYtMBMe1CFjdXfHKdxi/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket.md#tovarnaya-baza) |

**Пример вызова**

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/saleByLatestByCategoryPath/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&categoryPaths=accu-gereedschap%2Faccu'
```
