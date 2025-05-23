![](https://raw.githubusercontent.com/sllavon/crypto-pay-api-sdk/3e83818c975a47f4ca61209b478f2508224058db/media/header.svg)
# crypto-pay-api-sdk
Описание Telegram Бота, Интегрированного с Crypto Bot
Telegram бот, интегрированный с Crypto Bot, предназначен для упрощения операций с криптовалютами, таких как создание счетов, прием платежей и перевод средств. Ниже описаны основные модели работы такого бота.

Модели Работы Бота
Прием Платежей

Описание: Бот позволяет создавать счета для приема платежей в различных криптовалютах, таких как BTC, TON, USDT, и т.д.

Реализация: Используйте метод createInvoice класса Crypto для создания нового счета. Этот метод принимает параметры, такие как валюта (asset), сумма (amount), и необязательные параметры для описания счета (description), скрытого сообщения (hidden_message), и т.д.

Перевод Средств

Описание: Бот может переводить криптовалюту между пользователями Telegram.

Реализация: Используйте метод transfer класса Crypto для отправки криптовалюты пользователю. Этот метод требует идентификатора пользователя Telegram (user_id), валюты (asset), суммы (amount), и уникального идентификатора (spend_id).

Получение Баланса

Описание: Бот может отображать текущий баланс криптовалютных активов.

Реализация: Используйте метод getBalance класса Crypto для получения баланса всех поддерживаемых активов.

Получение Курсы Валют

Описание: Бот может предоставлять актуальные курсы обмена криптовалют.

Реализация: Используйте метод getExchangeRates класса Crypto для получения текущих курсов обмена поддерживаемых валют.

Создание Счетов

Описание: Бот позволяет создавать одноразовые или многоразовые счета для сбора средств.

Реализация: Используйте метод createInvoice для создания счетов. Для многоразовых счетов необходимо реализовать логику обновления статуса счета после каждой оплаты.

Пример Кодовой Реализации python
```python
from requests import get, post

class CryptoBot:
    def __init__(self, token):
        self.crypto = Crypto(token)

    def start(self):
        print("Бот запущен. Выберите действие:")
        print("1. Создать счет")
        print("2. Перевести средства")
        print("3. Проверить баланс")
        action = input("Введите номер действия: ")
        
        if action == "1":
            self.create_invoice()
        elif action == "2":
            self.transfer_coins()
        elif action == "3":
            self.check_balance()

    def create_invoice(self):
        asset = input("Введите валюту (например, BTC): ")
        amount = input("Введите сумму: ")
        description = input("Введите описание счета (необязательно): ")
        
        params = {}
        if description:
            params['description'] = description
        
        invoice = self.crypto.createInvoice(asset, amount, params)
        print("Счет создан:", invoice)

    def transfer_coins(self):
        user_id = int(input("Введите ID пользователя Telegram: "))
        asset = input("Введите валюту (например, BTC): ")
        amount = input("Введите сумму: ")
        spend_id = input("Введите уникальный идентификатор: ")
        
        transfer = self.crypto.transfer(user_id, asset, amount, spend_id)
        print("Перевод выполнен:", transfer)

    def check_balance(self):
        balance = self.crypto.getBalance()
        print("Текущий баланс:", balance)

# Запуск бота
if __name__ == "__main__":
    token = "Ваш_API_токен"
    bot = CryptoBot(token)
    bot.start()
```

## [SDK for working with Crypto Bot](https://t.me/CryptoBot)

[![Downloads](https://static.pepy.tech/badge/crypto-pay-api-sdk)](https://pepy.tech/project/crypto-pay-api-sdk) [![TON](https://camo.githubusercontent.com/862a7c69bd3b8a405bdd94557b8e6d5a90702f363058e59fd8dadda3adb60a97/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539322538452d544f4e2d677265656e)](https://ton.org) [![license - MIT](https://camo.githubusercontent.com/63691059c8dda9856bd568ef8bb0b326677b863d8b1fc9237cc096b6fd18a205/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f737461732d70726f6b6f706965762f697079776964676574735f746f67676c655f627574746f6e73)](https://github.com/sllavon/crypto-pay-api-sdk/blob/main/LICENSE) 

# Installation
```sh
pip install crypto-pay-api-sdk
```

# Explanation

First, you need to create your application and get an API token.
Open [@CryptoBot](http://t.me/CryptoBot?start=pay) or [@CryptoTestnetBot](http://t.me/CryptoTestnetBot?start=pay) (for testnet), end a command `/pay` to create a new app and get API Token.
| Net  | Bot | Hostname |
| ------------- | ------------- | ------------- |
| mainnet  | [@CryptoBot](http://t.me/CryptoBot?start=pay)  | pay.crypt.bot |
| testnet  | [@CryptoTestnetBot](http://t.me/CryptoTestnetBot?start=pay)  | testnet-pay.crypt.bot |
>All queries to the Crypto Pay API must be sent over HTTPS



# Examples
**Support for all methods [official API](https://help.crypt.bot/crypto-pay-api)**
```python
from crypto_pay_api_sdk import cryptopay

Crypto = cryptopay.Crypto("TOKEN", testnet = True) #default testnet = False

print(Crypto.getMe())
print(Crypto.createInvoice("TON", "0.4", params={"description": "Test Invoice",
                                                 "expires_in": 300
                                                 }))
```

## Methods

**API**

* [getMe](#getMe)
* [createInvoice](#createInvoice)
* [transfer](#transfer)
* [getInvoices](#getInvoices)
* [getBalance](#getBalance)
* [getExchangeRates](#getExchangeRates)
* [getCurrencies](#getCurrencies)

### getMe

A simple method for testing your app's authentication token. Requires no parameters. Returns basic information about the app.

```python
Crypto.getMe()
```

### createInvoice

Use this method to create a new invoice. Returns object of created invoice.

* **asset** (string)
Currency code. Supported assets: `BTC`, `TON`, `ETH` (only testnet), `USDT`, `USDC`, `BUSD`.
* **amount** (string)
Amount of the invoice in float. For example: `125.50`
* **description** (string)
*Optional*. Description of the invoice. Up to 1024 symbols.
* **hidden_message** (string)
*Optional*. The message will show when the user pays your invoice.
* **paid_btn_name** (string) default - `callback`
*Optional*. Paid button name. This button will be shown when your invoice was paid. Supported names:

  * `viewItem` - View Item
  * `openChannel` - Open Channel
  * `openBot` - Open Bot
  * `callback` - Return

* **paid_btn_url** (string)
*Optional but requried when you use paid_btn_name*. Paid button URL. You can set any payment success link (for example link on your bot). Start with https or http.
* **payload** (string, up to 4kb)
*Optional*. Some data. User ID, payment id, or any data you want to attach to the invoice.
* **allow_comments** (boolean)
*Optional*. Allow adding comments when paying an invoice. Default is true.
* **allow_anonymous** (boolean)
*Optional*. Allow pay invoice as anonymous. Default is true.
* **expires_in** (number)
*Optional*. You can set the expiration date of the invoice in seconds. Use this period: 1-2678400 seconds.

```python
Crypto.createInvoice("BTC", 1,
                    params = {
                        description: 'kitten',
                        paid_btn_name: viewItem,
                        paid_btn_url: 'http://placekitten.com/150'
                    })
```

### transfer

Use this method to send coins from your app to the user. Returns object of completed transfer.

* **user_id** (number)
Telegram User ID. The user needs to have an account in our bot (send /start if no).
* **asset** (string)
Currency code. Supported assets: `BTC`, `TON`, `ETH` (only testnet), `USDT`, `USDC`, `BUSD`.
* **amount** (string)
Amount of the transfer in float. For example: `125.50`
* **spend_id** (string)
It is used to make your request idempotent. It's guaranteed that only one of the transfers with the same spend_id will be accepted by Crypto Pay API. This parameter is useful when the transfer should be retried (i.e. request timeout/connection reset/500 HTTP status/etc). You can use a withdrawal id or something. Up to 64 symbols.
* **comment** (string)
*Optional*. The comment of the invoice. The comment will show in the notification about the transfer. Up to 1024 symbols.

```python
Crypto.transfer(121011054, 'ETH',
                0.1, 'ZG9uYXRl',
                params = {
                    comment: 'donate'
                })
```

### getInvoices

Use this method to get invoices of your app. On success, the returns array of invoices.

* **asset** (string)
*Optional*. Currency code. Supported assets: `BTC`, `TON`, `ETH` (only testnet), `USDT`, `USDC`, `BUSD`. Default: all assets.
* **invoice_ids** (string)
*Optional*. Invoice IDs separated by comma.
* **status** (string)
*Optional*. Status of invoices. Available statusses: active or paid. Default: all statusses.
* **offset** (number)
*Optional*. Offset needed to return a specific subset of  invoices. Default 0.
* **count** (number)
*Optional*. Number of invoices to return. Default 100, max 1000.

```python
Crypto.getInvoices(params = {'asset': "TON", 'count': 1})
```

### getBalance

Use this method to get balance of your app. Returns array of assets.

```python
Crypto.getBalance()
```

### getExchangeRates

Use this method to get exchange rates of supported currencies. Returns array of currencies.

```python
Crypto.getExchangeRates()
```

### getCurrencies

Use this method to supported currencies. Returns array of currencies.

```python
Crypto.getCurrencies()
```

## License
MIT
