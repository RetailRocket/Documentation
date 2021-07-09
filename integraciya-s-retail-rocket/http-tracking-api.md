---
description: >-
  В документе описываются принцыпы и способы передачи в систему Retail Rocket
  пользовательского поведения на интернет магазине. По средствам протокола HTTP.
---

# API трекинг пользовательского поведения

Данные о пользовательском поведении необходимы для работы многих продуктов Retail Rocket: товарных рекомендаций, автоматизированных коммуникационных кампаний, для отслеживания метрик эффективности коммуникаций и т.д.

## **Общие концепции**

Для передачи пользовательского поведения в систему Retail Rocket необходимо вызывать соответствующие действию методы трекинг API для всех значимых действий пользователя \(просмотр товара, добавление в корзину и т.д.\). Ниже описаны методы для каждого такого действия с их параметрами, возможными кодами ответов и примерами вызова.

## **Base URL**

`https://apptracking.retailrocket.net/1.0/`

## Resources

Трекинг API придерживается [общих принципов интеграционных API](obshie-principy-integracii-s-retail-rocket.md).

### Просмотр карточки товара

Должен быть вызван при каждом просмотре посетителем карточки товара.

#### Path

**`view`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`ViewEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/view?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Просмотр карточки группового товара

Должен быть вызван при каждом просмотре посетителем карточки товара, на которой представлен групповой товар.

#### Path

**`groupView`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`GroupViewEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `groupId` | Да | integer | [Идентификатор товарной группы](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `productIds` | Да | number array | [Список идентификаторов товаров](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/groupView?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"groupId\": 654321,
         \"productIds\": [123456, 234567, 345678],
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Добавление товара в корзину

Должен быть вызван при каждом добавлении посетителем товара в корзину.

#### Path

**`addToBasket`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`AddToBasketEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/addToBasket?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Просмотр страницы товарной категории

Должен быть вызван при просмотре посетителем страницы товарной категории.

#### Path

**`categoryView`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`CategoryViewEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `categoryId` | Да | integer | [Идентификатор категории товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/categoryView?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"categoryId\": 123456,
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Заказ товара

​Должен быть вызван для каждой товарной позиции в заказе.

#### Path

**`order`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`OrderEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `price` | Да | number | Цена с учетом скидок за **единицу товара** |
| `quantity` | Да | number | Кол-во единиц товара в заказе |
| `transaction` | Да | string | Идентификатор покупки |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/order?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"quantity\": 2,
         \"price\": 234,
         \"transaction\": \"135243\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Поисковый запрос

Должен быть вызван при вводе посетителем поисковой фразы.

#### Path

**`search`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`SearchEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `searchPhrase` | Да | string | Поисковая фраза, которую ввел пользователь |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит пользователь](obshie-principy-integracii-s-retail-rocket.md#podderzhka-regionalnosti-sklad-region) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/search?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"searchPhrase\": \"подгузник для новорожденных\",
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Установка канала коммуникации

Привязка каналов коммуникации используется для доставки персонализированного контента пользователю. В данном примере рассматривается Email канал. Метод нужно вызывать при регистрации, успешной авторизации, оформлении заказа и других местах интернет-магазина, когда пользователь может оставить email адрес для получения сообщений.

{% hint style="info" %}
**`contactExternalId` -**  уникальный идентификатор контакта, по которому пользователь может быть распознан в учетной системе клиента. Служит для обмена информацией о контактах пользователя между платформой Retail Rocket и источниками данных клиента.
{% endhint %}

#### Требования к contactExternalId

* Должен состоять только из цифр и букв;
* Не превышать 50 символов;

#### Path

**`setContact`**

#### HTTP-метод

`POST`

#### HTTP-заголовки

`Content-type: application/json`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### Тело запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `contactExternalId` | Да | string | [Уникальный идентификатор контакта](http-tracking-api.md#ustanovka-sessii) |
| `email` | Нет | integer | Адрес почты пользователя |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит пользователь](obshie-principy-integracii-s-retail-rocket.md#podderzhka-regionalnosti-sklad-region) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |
| `phone` | Нет | string | Номер телефона пользователя |
| `customData` | Нет | string | Пользовательские параметры |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/setContact?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"contactExternalId\": \"2342dfgf253454c645346wer3\",
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\",
         \"email\": \"example@email.com\"
      }
   "
```

## Передача данных о взаимодействии с рекомендациями

Для оценки эффективности рекомендаций, а также для учета продаж через товарные рекомендации, необходимо передавать соответствующие события взаимодействия с рекомендациями.

{% hint style="info" %}
Идентификатор блока рекомендаций `recomBlockId` **-** любая строка до 50 символов, используется для идентификации каждого блока рекомендаций
{% endhint %}

### Просмотр блока рекомендаций

Должен быть вызван при каждом показе блока рекомендаций пользователю приложения

#### Path

**`recomBlockViewed`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `recomBlockId` | Да | string | [Идентификатор блока рекомендаций](http-tracking-api.md#peredacha-dannykh-o-vzaimodeistvii-s-rekomendaciyami) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/recomBlockViewed?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"recomBlockId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\", 
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Клик по блоку рекомендаций

Нажатие по рекомендации – это, фактически, переход в карточку товара, рекомендованную в блоке. Элементы, такие как название товара, изображения товара, цена и другие, относящиеся к рекомендуемому товару из рекомендаций, должны содержать обработчик tap и вызывать событие recomTap.

#### Path

**`recomTap`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |
| `recomBlockId` | Да | string | [Идентификатор блока рекомендаций](http-tracking-api.md#peredacha-dannykh-o-vzaimodeistvii-s-rekomendaciyami) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/recomTap?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\",
         \"recomBlockId\": \"5994d90bc7d0114fa0a26122\"
      }
   "
```

### Добавление в корзину из блока рекомендаций

Если в блоке товарных рекомендаций используется кнопка добавления товара в корзину, то при каждом добавлении товара в корзину из блока рекомендаций \(без перехода в карточку товара\) необходимо вызывать обработчик добавления товара из блока в корзину.

#### **Path**

**`recomAddToBasket`**

#### HTTP-метод

`POST`

#### HTTP-заголовки

`Content-type: application/json`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### Тело запроса

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада, к которому принадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |
| `recomBlockId` | Да | string | [Идентификатор блока рекомендаций](http-tracking-api.md#peredacha-dannykh-o-vzaimodeistvii-s-rekomendaciyami) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/recomAddToBasket?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\",
         \"recomBlockId\": \"5994d90bc7d0114fa0a26122\"
      }
   "
```

## Передача данных о взаимодействии со спонсорским контентом

### Просмотр спонсорского контента

Должен быть вызван при каждом показе посетителю спонсорского контента.

#### Path

**`impressionContentViewed`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`ImpressionContentViewedEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `impressionContentId` | Да | string | [Идентификатор спонсорского контента](instrukciya-po-integracii-retail-rocket-sponsorskoe-razmeshenie.md#integraciya-sistem) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/impressionContentViewed?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Клик по спонсорскому контенту

Должен быть вызван при каждом клике посетителем в спонсорский контент.

#### Path

**impressionContentClicked**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`ImpressionContentViewedEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `impressionContentId` | Да | string | [Идентификатор спонсорского контента](instrukciya-po-integracii-retail-rocket-sponsorskoe-razmeshenie.md#integraciya-sistem) |
| `timestamp` | Да | string | [Метка времени вызова](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/impressionContentClicked?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

## Пакетная загрузка пользовательского поведения

API предоставляет возможность пакетной загрузки пользовательского поведения. В теле вызова метода передается список пользовательских событий.

#### Path

**`visitorEvents`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет-магазина](obshie-principy-integracii-s-retail-rocket.md#identifikator-internet-magazina) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается список пользовательских событий любого из следующих типов:

**`ViewEventEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `view` | Да | `ViewEvent` | Тип подробно описан в разделе «Просмотр карточки товара» как тип параметра тела запроса |

**`GroupEventViewEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `groupView` | Да | `GroupViewEvent` | Тип подробно описан в разделе «Просмотр карточки группового товара» как тип параметра тела запроса |

**`AddToBasketEventEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `addToBasket` | Да | `addToBasketEvent` | Тип подробно описан в разделе «Добавление товара в корзину» как тип параметра тела запроса |

**`OrderEventEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `order` | Да | `OrderEvent` | Тип подробно описан в разделе «Заказ товара» как тип параметра тела запроса |

**`CategoryViewEventEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `categoryView` | Да | `CategoryViewEvent` | Тип подробно описан в разделе «Просмотр страницы категории товара» как тип параметра тела запроса |

**`SearchViewEventEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `search` | Да | `SearchViewEvent` | Тип подробно описан в разделе «Просмотр страницы категории товара» как тип параметра тела запроса |

**`ImpressionContentViewedEventEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `impressionContentViewed` | Да | `ImpressionContentViewedEvent` | Тип подробно описан в разделе «Просмотр спонсорского контента» как тип параметра тела запроса |

**`ImpressionContentClickedEventEnvelope`**

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `impressionContentClicked` | Да | `ImpressionContentClickedEvent` | Тип подробно описан в разделе «Клик по спонсорскому контенту» как тип параметра тела запроса |

**Пример вызова**

{% tabs %}
{% tab title="Bash" %}
```bash
curl \
   -X POST "https://apptracking.retailrocket.net/1.0/visitorEvents?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5" \
   -H "Content-type: application/json" \
   --data "
    [
        {
            \"view\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 123456,
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"groupView\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"groupId\": 654321,
                \"productIds\": [123456, 234567, 345678],
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"addToBasket\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 234567,
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"order\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 234567,
                \"stockId\": \"NewYork\",
                \"quantity\": 3,
                \"price\": 1321.43,
                \"transaction\": \"135243\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"categoryView\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"categoryId\": 123456,                 
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"search\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"searchPhrase\": \"подгузник для новорожденных\",            
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"recomBlockViewed\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"recomBlockId\": \"34453dffg34534te565\",            
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"recomTap\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"recomBlockId\": \"34453dffg34534te55\",  
                \"productId\" : 123,          
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"recomAddToBasket\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"recomBlockId\": \"34453dffg34534te54\",      
                \"productId\" : 123,        
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"impressionContentViewed\": {
              \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
              \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
              \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"impressionContentClicked\": {
              \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
              \"impressionContentId\": \"568da4f6-7952-49b1-afd3-9ac44d6e78c7\",
              \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        }
    ]
"
```
{% endtab %}
{% endtabs %}

## Коды ответов

| Код ответа | Описание |
| :--- | :--- |
| 200 | Запрос принят |
| 400 | Ошибка в запросе, необходимо проверить правильность построения запроса |
| 401 | Ошибка аутентификации, проверьте корректность пары `apiKey` |
| 403 | Доступ запрещен |
| 404 | Запрошенный ресурс не найден, необходимо проверить правильность URL |

## Время ответа

Ожидаемое время ответа – 100 мс для всех ресурсов \(может отличаться из-за особенностей сетевой инфраструктуры\).

## Ограничения

По умолчанию действует ограничение в 50 запросов/секунду для площадки. При необходимости возможно изменить \(с помощью аккаунт-менеджера\).

