# How to use

## Simple usage example

Calculate CSV provided transactions fees.

```shell
php cli.php ./tests/fixtures/transactions.csv
```

# Classes

### App:class

Responsible for CLI application

Main *Singleton* instance and container for whole application.

**Example**

```php
use Paysera\CommissionTask\App;

App::getInstance()->run();
```

### TransactionsRepository:class

Can be replaced with production repository that implementing `TransactionRepositoryInterface::class`

**Example**

```php
use Paysera\CommissionTask\entities\Transaction;
use Paysera\CommissionTask\repositories\TransactionsRepository;

$repository = new TransactionsRepository();
$repository->addTransaction(new Transaction());

var_dump($repository->getTransactions());
```

### RatesService::class Service

Service to provide amount currency exchange functionality.

> `RatesService::convert` method required from `RatesServiceInterface::class`

Supported providers

1. ExchangeRatesApi (`ExchangeRatesApi::class`)
2. ...
3. ...

**Example**

```php
use Paysera\CommissionTask\services\currency\providers\ExchangeRatesApi;
use Paysera\CommissionTask\services\currency\RatesService;

$provider = new ExchangeRatesApi('api_key',true);
$service = new RatesService($provider);

echo $service->convert(100,'USD','EUR');
```

### FeeCalculator::class Service

Constructor required `$transactionsRepository` to use user previous transactions on calculation.

**Example**

```php
use Paysera\CommissionTask\repositories\TransactionsRepository;
use Paysera\CommissionTask\services\currency\RatesService;
use Paysera\CommissionTask\services\FeeCalculatorService;

/**
 * @var TransactionsRepository $transactionsRepository
 * @var RatesService $rates_service
 * */
$calculator = new FeeCalculatorService($transactionsRepository, $rates_service);

foreach($transactionsRepository->getTransactions() as $transaction){
    $fee = $calculator->calculateFee($transaction);
    
    echo $fee.PHP_EOL;
}

```

### AmountFormatter:class

Helper class with static method format. Intended for formatting `amount` float values.

**Example**

```php

echo \Paysera\CommissionTask\helpers\AmountFormatter::format(1891.151);

```

# Tests

Command to run PHPUnit tests

```bash
composer run-script phpunit
```