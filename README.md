# freqtrade

[![Build Status](https://travis-ci.org/gcarq/freqtrade.svg?branch=develop)](https://travis-ci.org/gcarq/freqtrade)

Simple High frequency trading bot for crypto currencies.
Currently supported exchanges: bittrex, poloniex (partly implemented)

This software is for educational purposes only.
Don't risk money which you are afraid to lose.

#### Telegram Set-Up
The command interface is accessible via Telegram (not required).
To set it up do:
1. Register a new bot on https://telegram.me/BotFather and read the `token`
2. Create a new Telegram group chat and invite the bot to the chat
3. Send the message `/my_id @my_bot` to the bot (replace `@my_bot` with your actual bot name)
4. Go to following url: `https://api.telegram.org/botXXX:YYYY/getUpdates` (replace XXX:YYYY with your bot token)
5. Look for `"chat":{"id":-zzzzzzzzzz,` -- `-zzzzzzzzzz` is your chat id (with the negative sign).
6. Enter the telegram `token` and your `chat_id` in `config.json`

#### Telegram RPC commands:
* /start: Starts the trader
* /stop: Stops the trader
* /status: Lists all open trades
* /profit: Lists cumulative profit from all finished trades
* /forcesell <trade_id>: Instantly sells the given trade (Ignoring `minimum_roi`).
* /performance: Show performance of each finished trade grouped by pair

#### Persistance
Currently, persistence is achieved through sqlite -- no set up required


#### Config
`minimal_roi` is a JSON object where the key is a duration
in minutes and the value is the minimum ROI in percent.
See the example below:
```
"minimal_roi": {
    "2880": 0.005, # Sell after 48 hours if there is at least 0.5% profit
    "1440": 0.01,  # Sell after 24 hours if there is at least 1% profit
    "720":  0.02,  # Sell after 12 hours if there is at least 2% profit
    "360":  0.02,  # Sell after 6 hours if there is at least 2% profit
    "0":    0.025  # Sell immediately if there is at least 2.5% profit
},
```

`stoploss` is loss in percentage that should trigger a sale. 
For example value `-0.10` will cause immediate sell if the
profit dips below -10% for a given trade. This parameter is optional.

`initial_state` is an optional field that defines the initial application state.
Possible values are `running` or `stopped`. (default=`running`)
If the value is `stopped` the bot has to be started with `/start` first.

The other values should be self-explanatory,
if not feel free to raise a github issue.

#### Prerequisites
* python3.6
* sqlite
* [TA-lib](https://github.com/mrjbq7/ta-lib#dependencies) binaries

#### Install
```
$ cd freqtrade/
# copy example config. Dont forget to insert your api keys
$ cp config.json.example config.json
$ python -m venv .env
$ source .env/bin/activate
$ pip install -r requirements.txt
$ ./main.py
```

#### Execute tests

```
$ python -m unittest
```

#### Docker
```
$ cd freqtrade
$ docker build -t freqtrade .
$ docker run --rm -it freqtrade
```
