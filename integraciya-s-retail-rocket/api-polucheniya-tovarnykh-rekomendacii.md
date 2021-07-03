# API получения товарных-рекомендаций

## **Общие концепции**

Для получение товарных рекомендаций от системы Retail Rocket необходимо обратиться к ресурсу соответствующему типу рекомендаций. 

API получения товарных-рекомендаций ****придерживается [общих принципов интеграционных API](obshie-principy-integracii-s-retail-rocket.md). 

{% hint style="info" %}
**Товарные параметры**

Параметр `stockId` требуется для того чтобы рекомендации содержали только товары в наличии на конкретном складе\(регионе\). Значение параметра должно, как и всех товарных параметров, соответствовать значениям переданным в Retail Rocket с товарной базой.
{% endhint %}

{% hint style="info" %}
**Лимит частоты обращение к ресурсам**

По умолчанию установлен лимит в 100 обращений в секунду на магазин\(`partnerId`\) в целом. Для изменения лимита необходимо обратиться к вашему аккаунт-менеджеру или через форму подвержки в личном кабинете.
{% endhint %}

{% hint style="warning" %}
**Управление кеширование**

Вместе с возвращаемыми данными передается http-заголовок `Cache-Control,` которому необходимо следовать при кеширование полученных данных
{% endhint %}

### **Base URL**

`https://externalapi.retailrocket.net/api/3.0/`

### HTTP-метод доступа

Все ресурсы API доступны только по `GET` методу

### HTTP-стутатусы обращени

Успешность обращение к ресурсу можно оценить по полученному HTTP-статусу

* 200 OK - запрос принят
* 400 BadRequest - ошибка в запросе, необходимо проверить правильность построения запроса
* 401 Unauthorized - ошибка аутентификации, проверьте корректность пары `partnerId`, `apiKey`
* 403 Forbidden - доступ запрещен
* 404 NotFound - запрошенный ресурс не найден, необходимо проверить правильность URL
* 429 TooManyRequest - слишком много запросов и/или лимит на количество запросов в интервал времени превышен

### Возвращаемое значение

В случае успешного обращанеия к любому ресурсу, HTTP-статус будет 200 Ok, а в теле запроса будет вовзращенна строка в содержащая JSON-объект.

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

#### HTTP-метод

## Типы рекомендаций

### **Популярные товары для главного экрана**

Рассчитанны на основание поведения пользователя во всем интернет мгазине без привязки к конетретным категориями, товарам, поисковым фаразам т.д. 

Представляет из себя актуальные бестселлеры магазина.

#### **Path**

**`partnerRecommendations/popular`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazinahttps://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/partnerRecommendations/popular/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017'
```

#### Возвращаемое значение

Описано по ссылке

### **Популярные товары для экрана** товарная категория

Пердназначен для показа пользователю самых популярных товаров в рамках категорийной группы. Учитываются товары и пользовательское поведение в рамках конкретной категории и всех ее подкатегориях.

**`categoryRecommendations/popular`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :---: | :--- |
| `apiKey` | Да | string | [Ключ авторизации](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazinahttps://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `categoryIds` | Да | number array | [Идентификатор товарной категории](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/categoryRecommendations/popular/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&categoryIds=123,234,254'
```

#### Возвращаемое значение

Описано по ссылке

### **Поисковые рекомендации**

Предназначен для показа пользователю товаров наиболее подходящих под его поисковый запрос. Рекомендации расчитываются на основе поведения других пользователей с подобным поисковым запросом и не учитывают смысл поисковых фраз.

Поисковые рекомендации следует применять там где известен поисковый запрос пользователя, обычно это поисковый экран интернет магазина. Рекомендации показывают свою эффективность и как дополнение к поисковой выдаче и в случае отсутствия поисковой выдачи.

#### **Path**

**`searchRecommendations/search`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazinahttps://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `searchPhrase` | Да | string | Поисковый запрос пользователя |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/searchRecommendations/search/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&searchPhrase=skirts'
```

### **Похожие товары**

Предназначен для показа товаров наболее похожишь к просмотреваемому пользоватлем товара или товаров. Рекомендации расчитываются на основе поведения других пользователей и учитывают парамтеры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### **Path**

**`productRecommendations/alternatives`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazinahttps://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `productIds` | Да | string | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/alternatives/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=93845,93846,93847'
```

### **Сопутствующие товары**

Предназначен для показа комплементарных товаров к товару или товарам. Рекомендации расчитываются на основание поведения других пользователей и учитывают параметры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### **Path**

**`productRecommendations/related`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazinahttps://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `productIds` | Да | string | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/relatedByInterestedProperties/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=93845,93846,93847'
```

### **Сопутствующие товары, персонализированные с учетом интереса пользователя к свойствам товара**

Предназначен для показа комплементарных товаров к товару или товарам ****которые дополнительно ранжированны в зависимости от интереса пользователей к конкретным свойствам товара\(размер, цвет\). Рекомендации расчитываются на основание поведения других пользователей и учитывают параметры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### **Path**

**`productRecommendations/relatedByInterestedProperties`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#identifikator-internet-magazinahttps://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `productIds` | Да | string | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) |

### **Персональные рекомендации**

Алгоритм персональных рекомендаций возвращает наиболее интересные товары для пользователя

#### **Path**

**`partnerRecommendations/personalComposite`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/partnerRecommendations/personalComposite/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017'
```

### **Аксессуары**

Предназначен для показа товаров из аксессуарных категорий которые которые преобретаются совместно с товаром или товарами к которым запрашиваются рекомендации. Рекомендации расчитываются на основание поведения других пользователей и учитывают параметры товаров и настройки бизнес-правил в личном кабинете Retail Rocket.

#### Path

**`productRecommendations/accessories`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](https://mobileapidocs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `productIds` | Да | string | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) |

#### Пример вызова

```bash
curl 'https://externalapi.retailrocket.net/api/3.0/productRecommendations/accessories/?sessionExternalId=3beb9714-82e9-4c08-938d-1391f5d86f2b&stockId=moscow&partnerId=69908d02c7d513ce40de715a&apiKey=5b333f5297a528b0184b6017&productIds=93845,93846,93847'
```

### Популярные товары к интересующим пользователя категориям

Retail Rocket в реальном времени анализирует интересы пользователя и строит связи «пользователь» – «категория». Это позволяет демонстрировать хиты продаж только из категорий, которые наиболее интересны данному пользователю в долгосрочной перспективе.

#### Path

**`partnerRecommendations/popularInInterestedCategories`**

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |
| `stockId` | Нет | string | [Идентификатор склада/региона](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) |
| `productIds` | Да | string | [Список идентификаторов товара записанных через запятую](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) |

