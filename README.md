# laravel-gestpay
Gestpay - Banca Sella payment libraries for Laravel 5
The easiest way to allow your customers to pay with their credit card their purchase on your website using Gestay - Banca Sella
**The documentation will be improved in the coming days**

## Liability limitations
We are not and will not be responsible for any errors or problems caused by these files. **Please read Gestpay's official documentation carefully before using this package**.

## Installation

You can install the package via composer:
```sh
composer require biscolab/laravel-gestpay
```
The service **provider** must be registered in `config/app.php`:
```php
'providers' => [
    ...
    Biscolab\Gestpay\GestpayServiceProvider::class,
];
```
You can use the facade for shorter code. Add "Gestpay" to your aliases:
```php
'aliases' => [
    ...
    'Gestpay' => Biscolab\Gestpay\Facades\Gestpay::class,
];
```
Create `config/gestpay.php` configuration file using:
```su
php artisan vendor:publish --provider="Biscolab\Gestpay\GestpayServiceProvider"
```

## Configuration

### Laravel configuration
Open `config/gestpay.php` configuration file and set `shopLogin` and `uicCode`:
```php
return [
    'shopLogin'      => 'YOUR_SHOP_LOGIN',
    'uicCode'        => 'CURRENCY_CODE',
    'test'           => 'true' // supported: v2|invisible 
];
```
- **shopLogin** is the code that is assigned to your account
- **uicCode** is already set to 242 (Euro). You can find the complete list of currency codes [here](http://api.gestpay.it/#currency-codes)
- **test** if true it indicates that you are using your test account. More info at [Using Gestpay payment page ](http://docs.gestpay.it/pay/using-banca-sella-payment-page.html)

For more information about **shopLogin** and **uicCode** please visit [Gestpay - Creating your custom payment page](http://docs.gestpay.it/pay/creating-your-custom-payment-page.html)

### Gestpay configuration
Login to your **Gestpay BackOffice** account and set:
- IP Address (your server IP, you can add more than one)
- Response Address 
    -  URL for positive response (e.g. https://[yourdomain]/gestpay_callback/ok) 
    -  URL for negative response (e.g. https://[yourdomain]/gestpay_callback/ko)

## How to use
### Ok, and now let's pay!
As always, paying is the easiest thing
```php
gestpay()->pay($amount, $shopTransactionId);
```
That's all! 
- $amount: is the amount you have to pay
- $shopTransactionId: is the unique identifier you have assigned to the transaction
 
I was joking, that's not all! Now you have to handle the callback.
Based on the gestpay configuration, you now have to create the routes. For example, you can create a controller that handles callbacks through the method "**gestpayCallback**"
```php
    // e.g.
    Route::get('/gestpay_callback/{status}', ['uses' => 'GestpayController@gestpayCallback']);
```