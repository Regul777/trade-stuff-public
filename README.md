# My personal crypto trading stuff

#### Why trust ourselves our money? The machines will end up ruling anyway!  
![](https://media1.giphy.com/media/50OuNGJcwIVXcuJURc/giphy.gif?cid=ecf05e47zl6yrmjhxbsj5kvl9208r3o0tpk5hdozk7aqe4e8&rid=giphy.gif)

### You have to have docker. So if you don't have docker, do get your docker and then come back here.

## Build
#### Go ahead and bake the cake. This step can be ignored if you're using images in the docker-compose.yml file.
```bash
$ docker-compose build
```

## Run
#### I just opened my eyes to crypto, so I'm still learning about all the stuff.
#### The BBRSI strategy is already "kinda" optimized, but I'm currently working in a huge improvement in its parameters.
#### The SMA strategy is not optimized at all, and the Hyperopt is not ready. But it's working...

* This is not configured for dry run. By running the steps bellow, you're already trading.

```bash
# BBRSI
$ docker-compose start freqtrade_bbrsi
# SMA
$ docker-compose start freqtrade_sma
```

## Download data
#### This is pretty straight forward.
#### You can pretty much download any chunk of time from your exchange for further machine learning and becktesting.
#### That's it!
```bash
# BBRSI
$ docker-compose run --rm freqtrade_bbrsi download-data --pairs-file ./user_data/data/binance/pairs_bbrsi.json --exchange binance -t 5m --days 90
# SMA
$ docker-compose run --rm freqtrade_sma download-data --pairs-file ./user_data/data/binance/pairs_sma.json --exchange binance -t 5m --days 90
```

## Backtesting
#### The bot travels to past with information from the future and applies its knowledge to try to beat its own last record. Amazing!
#### In truth, it just tests the strategy against the data you just downloaded from the above command...
#### So it's still amazing, because you can improve your gorgeous strategy and test it with past data without even need to waste money.
```bash
# BBRSI
$ docker-compose run --rm freqtrade_bbrsi backtesting --config ./user_data/config_bbrsi.json --strategy BBRSIOptimizedStrategy --datadir user_data/data/binance --export trades --max-open-trades=10 --stake-amount 26 -i 5m
# SMA
$ docker-compose run --rm freqtrade_sma backtesting --config ./user_data/config_sma.json --strategy SMAOffsetStrategy --datadir user_data/data/binance --export trades --max-open-trades=10 --stake-amount 26 -i 5m
```

## Train
#### Not gonna give any explanation about this today...
```bash
# BBRSI
$ docker-compose run --rm freqtrade_bbrsi hyperopt --config ./user_data/config_bbrsi.json --hyperopt BBRSIHyperopt --hyperopt-loss SortinoHyperOptLoss --strategy BBRSIOptimizedStrategy -i 5m --epochs 200 --stake-amount unlimited --min-trades 500 --max-open-trades 10
# SMA
$ docker-compose run --rm freqtrade_sma hyperopt --config ./user_data/config_sma.json --hyperopt SMAHyperopt --hyperopt-loss SortinoHyperOptLoss --strategy SMAOffsetOptimizedStrategy -i 5m --epochs 150 --stake-amount 26.6
```

## Plot
#### This is not done, don't even try
```
# BBRSI
$ docker-compose run --rm freqtrade_bbrsi hyperopt --config ./user_data/config_bbrsi.json --hyperopt BBRSIHyperopt --hyperopt-loss SortinoHyperOptLoss --strategy BBRSIOptimizedStrategy -i 5m --epochs 200 --stake-amount unlimited --min-trades 500 --max-open-trades 10
# SMA
$ docker-compose run --rm freqtrade_sma hyperopt --config ./user_data/config_sma.json --hyperopt SMAHyperopt --hyperopt-loss SortinoHyperOptLoss --strategy SMAOffsetOptimizedStrategy -i 5m --epochs 150 --stake-amount 26.6
```
