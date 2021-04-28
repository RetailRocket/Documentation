# Внедрение Retail Rocket в моб. приложение

## **Трекинг действий пользователей в мобильном приложении**

**Цель документаr**  
Описание метода интеграции сервисов Retail Rocket в мобильное приложение интернет-магазина

**Владелец документа**  
Александр Сацюк

## **Общие концепции**

**Управление сессией**

* Для управления сессией используется параметр sessionExternalId. Для API он передается в качестве query параметра, для трекинга – в теле запроса.
* Это произвольная строка длиной до 50 символов.
* sessionExternalId может быть любым, но уникальным для каждой установки мобильного приложения.
* Не рекомендуется использовать любые персональные данные пользователя \(например, email\). Вместо этого лучше сгенерировать случайный guid при первом запуске приложения и использовать его во всех последующих запросах. Рекомендуем использовать для этого идентификатор пользователя для разработчика \(идентификатор установки приложения\).
* sessionExternalId  не должен изменяться при авторизации или смене аккаунта внутри одной и той же установки приложения.

### **Авторизация**

* Для авторизации необходимо включать query параметр apiKey={apiKey} во все запросы
* apiKey может измениться, это нужно учитывать при разработке интеграции и хранить его в БД на сервере
* apiKey можно получить у аккаунт-менеджера
* В случае неуспешной авторизации \(неверный apiKey\) вернется статус 401

### **Base URL**

{% embed url="https://apptracking.retailrocket.net/1.0/" %}

### \*\*\*\*

{% api-method method="post" host="" path="" %}
{% api-method-summary %}
Метод отправки событий трекинга
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Запрос принят
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
ошибка в запросе, необходимо проверить правильность построения запроса
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
ошибка аутентификации, проверьте корректность пары partnerId, ApiKey
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
доступ запрещен 
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
запрошенный ресурс не найден, необходимо проверить правильность URL
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### **Примечания**

Параметры "productId", "categoryId", "stockId" должны совпадать с ID, передаваемыми в мета-данных о товаре \(в YML или через JS-трекинг на сайте\)

## **Подробное описание методов трекинга поведения пользователя в мобильном приложении**

### view

Событие просмотра карточки товара. Вызывается, при каждом просмотре карточки товара пользователем.

**Path**

/view

**Query**

* apiKey
* partnerId

**Body**

```markup
{
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required    
	"productId": <product id>, // int64, required    
	"stockId": "<stock identifier>", // string, up to 50 characters, optional
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

### **groupView**

Событие просмотра карточки группового товара. Вызывается, при каждом просмотре карточки товара пользователем.

Групповые товары следует использовать, если на сайте представлены несколько предложений, которые являются цвето-размерными вариациями одного товара. Подобные торговые предложения \(то есть разные размеры одной разновидности товара\разные фасовки и весовки\разные цвета\) необходимо объединить с помощью groupId.

groupId должен быть одинаковым для всех товаров, которые необходимо объединить в группу. Если у товара нет разновидностей, то можно передавать groupId равный id товара.

Функционал групповых товаров позволяет исключить из выдачи товары из одной группы \(например вещи разных размеров или цветов\). В любой выдаче всегда будет находиться только один \(самый популярный\) товар из группы.

Справка по groupId от Яндекса: 

{% embed url="https://yandex.ru/support/partnermarket/guides/clothes.html\#group-id" %}

#### Path

/groupView

#### **Query**

* apiKey
* partnerId

#### **Body**

```markup
{
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required
	"groupId": <group id>, // int64, required
	"productIds": [<product id>, <product id>], // int64[], required
	"stockId": "<stock identifier>", // string, up to 50 characters, optional
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

**Где:**

**`groupId`** - идентификатор группы товаров

**`productIds`** - все идентификаторов товаров объединенных общим groupId

### **addToBasket**

Событие добавления товара в корзину. Вызывается, при каждом добавлении в корзину товара пользователем.

#### **Path**

/addToBasket

#### **Query**

* apiKey
* partnerId

#### **Body**

```markup
{
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required
	"productId": <product id>, // int64, required
	"stockId": "<stock identifier>", // string, up to 50 characters, optional
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

### **order**

Событие покупки одного товара. Должен вызываться для каждой позиции в заказе

**Path**

/order

**Query**

* apiKey
* partnerId

**Body**

```markup
{
 "sessionExternalId": "<session external id>", // string, up to 50 characters, required
 "productId": <product id>, // int64, required
 "stockId": "<stock identifier>", // string, up to 50 characters, optional
"timestamp": "<iso8601 DateTime>", // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
 "quantity": <quantity>, // number, required
 "price": <price>, // number, required
 "transaction": "<transaction id>", // string, up to 50 characters, required
}
```

### **categoryView**

Событие просмотра раздела категории. Обычно это список товаров в категории \(листинг предложений\)

#### **Path**

/categoryView

#### **Query**

* apiKey
* partnerId

#### **Body**

```markup
{
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required
	"categoryId": <category id>, // int64, required
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

### **search**

Событие поиска товаров

**Path**

/search

**Query**

* apiKey
* partnerId

**Body**

```markup
{
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required
	"searchPhrase": "<search query>", // string, up to 50 characters, required
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

### **setContact**

Привязка каналов коммуникации к текущей сессии. Нужно для отправки триггерных писем или производства персонализированных маркетинговых кампаний. Как правило - это авторизация/регистрация пользователя в приложении.  
  
**Важно:**

1. Передавайте email адрес только тех пользователей, которые явно выразили согласие на получение сообщений электронной почты, в том числе рекламного характера.
2. С целью увеличения кол-ва отправок триггерных писем необходимо отправлять setContact с данными, по api, каждый раз, когда пользователь заходит в приложение, если известен email этого пользователя.
3. Передавайте Retail Rocket мобильные номера только тех абонентов, которые:
   1.  Дали явное предварительное согласие на получение информации рекламного характера, выраженное через совершение ими действий, которые позволяют однозначно их идентифицировать;
   2. Явно выразили свое согласие на обработку персональных данных, которое также выражено через совершение ими действий, которые позволяют однозначно их идентифицировать;. 
   3. Согласия, указанные в пп.2.2.-2.3. должны быть представлены в формах, которые могут быть предъявлены Retail Rocket в качестве доказательства наличия данных согласий по запросу Retail Rocket.

**Path**

/setContact

**Query**

* apiKey
* partnerId

**Body**

```markup
{
	"contactExternalId": "<contact id>", // string, up to 50 characters, required
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required
	"email": "<email address>", // optional
"phone": "<phone number>", // E.164, optional
"stockId": "<stock identifier>", // string, up to 50 characters, optional
"customData": "<dictionary>", // dictionary with string keys, optional
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz" 
}
```

**Где:**

`contactExternalId` - уникальный идентификатор контакта, по которому пользователь может быть распознан в учетной системе клиента. Служит для обмена информацией о контактах пользователя между платформой Retail Rocket и источниками данных клиента.

## **Пакетная загрузка событий**

Эндпоинт предназначен для получения последовательности событий одного пользователя.

**Path**

/visitorEvents

**Method**

POST

**Query**

* apiKey
* partnerId

**Body**

```markup
[
    {
        "view": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "productId": <product id>, // int64, required
            "stockId": "<stock identifier>",              // string, up to 50 characters, optional,
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "groupView": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "groupId": <group id>, // int64, required
            "productIds": [<product id>, <product id>], // int64[], required
            "stockId": "<stock identifier>",              // string, up to 50 characters, optional
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "addToBasket": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "productId": <product id>, // int64, required
            "stockId": "<stock identifier>",              // string, up to 50 characters, optional
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "order": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "productId": <product id>, // int64, required
            "stockId": "<stock identifier>", // string, up to 50 characters, optional
            "quantity": <quantity>, // number, requred
            "price": <price>, // number, required
            "transaction": "<transaction id>",            // string, up to 50 characters, required
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "categoryView": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "categoryId": <category id>,                  // int64, required
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "search": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "searchPhrase": "<search query>",             // string, up to 50 characters, required
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "setContact": {
            "contactExternalId": "<contact id>", // string, up to 50 characters, required
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "email": "email address", // email address, optional
            "phone": "phone number, E.164",               // , optional
            "timestamp": "<iso8601 DateTime>"              // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "recomTap": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "productId": <product id>, // int64, required - recommended product Id
            "stockId": "<stock identifier>", // string, up to 50 characters, optional
            "recomBlockId": "<recommendation block id>",  // string, up to 50 characters, required - any recommendation block identifier
            "timestamp": "<iso8601 DateTime>"              // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "recomAddToBasket": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "productId": <product id>, // int64, required - recommended product Id
            "stockId": "<stock identifier>", // string, up to 50 characters, optional
            "recomBlockId": "<recommendation block id>",  // string, up to 50 characters, required - any recommendation block identifier
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    },
    {
        "recomBlockViewed": {
            "sessionExternalId": "<session external id>", // string, up to 50 characters, required
            "recomBlockId": "<recommendation block id>",  // string, up to 50 characters, required - any recommendation block identifier
            "timestamp": "<iso8601 DateTime>"             // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ"
        }
    }
]
```

## **Передача данных о взаимодействии с рекомендациями**

Для оценки эффективности рекомендаций, а также для учета продаж через товарные рекомендации, необходимо передавать соответствующие события взаимодействия с рекомендациями.

### recomTap

Tap по рекомендации – это, фактически, переход в карточку товара, рекомендованную в виджете. Все блоки в мобильном приложении, относящиеся к рекомендуемому товару \(товар, который был получен из рекомендаций\) и указывающие на карточку товара \(название товара, в фотографии/изображении товара, в цена и т.д.\), должны содержать обработчик tap и вызывать событие recomTap

**Path**

/recomTap

**Query**

* apiKey
* partnerId

**Body**

```markup
{
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required    
	"productId": <product id>, // int64, required - recommended product Id    
	"stockId": "<stock identifier>", // string, up to 50 characters, optional    
	 "recomBlockId": "<recommendation block id>", // string, up to 50 characters, required - any recommendation block identifier
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

### **recomAddToBasket**

Если в виджете товарных рекомендаций используется кнопка добавления товара в корзину, то при каждом добавлении товара в корзину непосредственно из виджета рекомендаций \(без перехода в карточку товара\) необходимо вызывать обработчик добавления товара в корзину из блока с рекомендациями.

**Path**

/recomAddToBasket

**Query**

* apiKey
* partnerId

**Body**

```markup
{
	"sessionExternalId": "<session external id>", // string, up to 50 characters, required
	"productId": <product id>, // int64, required - recommended product Id
	"stockId": "<stock identifier>", // string, up to 50 characters, optional
	"recomBlockId": "<recommendation block id>", // string, up to 50 characters, required - any recommendation block identifier
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

### **recomBlockViewed**

Событие просмотра блока с рекомендациями в приложении. Событие отправляется в тот момент, когда блок появился на экране

**Path**

/recomBlockViewed

**Query**

* apiKey
* partnerId

**Body**

```markup
{
    "sessionExternalId": "<session external id>", // string, up to 50 characters, required
    "recomBlockId": "<recommendation block id>", // string, up to 50 characters, required - any recommendation block identifier 
"timestamp": "<iso8601 DateTime>" // date, iso 8601 - "yyyy-MM-ddTHH:mm:ssZ", "yyyy-MM-ddTHH:mm:sszzz"
}
```

  


