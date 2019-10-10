# CryptAPI's PHP Library
Official PHP library of CryptAPI

## Requirements:

```
PHP >= 5.5
PHP Curl
```



## Install


```
git clone https://github.com/cryptapi/php-cryptapi
```

[on GitHub](https://github.com/cryptapi/php-cryptapi)

## Usage

### Generating a new address

```php
<?php
require ('cryptapi.php');

$ca = new Cryptapi($coin, $my_address, $callback_url, $parameters, $pending);
$payment_address = $ca->get_address();
```

Where:

``$coin`` is the coin you wish to use, can be one of: ``['btc', 'eth', 'bch', 'ltc', 'iota', 'xmr']``  

``$my_address`` is your own crypto address, where your funds will be sent to  

``$callback_url`` is the URL that will be called upon payment

``$parameters`` is any parameter you wish to send to identify the payment, such as ``['order_id' => 1234]``

``$pending`` if you want to be notified of pending transactions.

``$payment_address`` is the newly generated address, that you will show your users


### Getting notified when the user pays

The URL you provided earlier will be called when a user pays, for easier processing of the request we've added the ``process_callback`` helper

```php
<?php
require ('cryptapi.php');

$payment_data = Cryptapi::process_callback($_GET, $convert);
```

Where:

`$convert` is a boolean to whether to convert to the main coin denomination.

&nbsp;

The `$payment_data` will be an array with the following keys:

`address_in` - the address generated by our service, where the funds were received

`address_out` - your address, where funds were sent

`txid_in` - the received TXID

`txid_out` - the sent TXID or null, in the case of a pending TX

`confirmations` - number of confirmations, or 0 in case of pending TX

`value` - the value that your customer paid

`value_forwarded` - the value we forwarded to you, after our fee

`coin` - the coin the payment was made in, one of: ``['btc', 'eth', 'bch', 'ltc', 'iota', 'xmr']``

`pending` - whether the transaction is pending, if `false` means it's confirmed

plus, any values set on `$params` when requesting the address, like the order ID.

&nbsp;

From here you just need to check if the value matches your order's value.


### Checking the logs of a request

```php
<?php
require ('cryptapi.php');

$ca = new Cryptapi($coin, $my_address, $callback_url, $parameters);
$data = $ca->check_logs();
```

Same parameters as before, the `$data` returned can be checked here: https://cryptapi.io/docs/#/Bitcoin/btclogs


## Help

Need help?  
Contact us @ https://cryptapi.io/contact/