## Overview
* [Prerequisites](#prerequities)
* [Introduction](#introduction)
* [Basic Structure of Alarm Configuration File](#basic-structure-of-alarm-configuration-file)
* [Configuring Multiple Different Alarm Services](#configuring-multiple-different-alarm-services)
* [Configuring Multiple of the Same Alarm Services](#configuring-multiple-of-the-same-alarm-services)
* [Editing or Adding Alarms](#editing-or-adding-alarms)
* [Customizing Alerts](#customizing-alerts)
* [Example `alarms.json`](#example-alarmsjson)
* [Enabling/Disabling Pokemon](#enabling-disabling-pokemon)

## Prerequisites
This guide assumes:

1. You have a working RocketMap installation
2. You are familiar with [JSON formatting](http://www.w3schools.com/json/default.asp)

## Introduction

In PokeAlarm, an "Alarm" is defined as a supported message service that sends a notification. Slack, Twitter, Telegram, each are considered message service.

PokeAlarm allows you to add as many different supported message services as you'd like (e.g., Slack, Twitter, Telegram), and as many of each message service that you would like (e.g., 3 Slack channels, 10 Twitter feeds, 5 Telegrams.)

You add alarms to the PokeAlarm alarm configuration file, which is `alarms.json` by default.

`alarms.json` is where:

1. Alarms are enable/disabled
2. Notification messages for each alarm are customized

The alarm configuration file follows the JSON format and has 4 sections:

1. `alarms`
2. `pokestops`
3. `gyms`
4. `pokemon`


## Editing or Adding Alarms
To add, edit, or remove alarms, edit the `alarms.json` file. If you haven't created an `alarms.json` file before, you can make a copy of `alarms.json.example` and rename it to `alarms.json`. PokeAlarm uses `alarms.json` by default.

Alarms are represented as a list of JSON Objects, inside an array labeled alarms. Each alarm should be surrounded by curly brackets, and the space in between fields should have a comma. Your default `alarms.json` looks like this:
```json
[
  {
    "active": "False",
    "type": "boxcar",
    "user_credentials": "YOUR_API_KEY"
  },
  {
    "active": "False",
    "type": "discord",
    "api_key": "YOUR_WEBHOOK_URL"
  },
  {
    "active": "False",
    "type": "facebook_page",
    "page_access_token": "YOUR_PAGE_ACCESS_TOKEN"
  },
  {
    "active": "False",
    "type": "pushbullet",
    "api_key": "YOUR_API_KEY"
  },
  {
    "active": "False",
    "type": "pushover",
    "app_token": "YOUR_APP_TOKEN",
    "user_key": "YOUR_USER_KEY"
  },
  {
    "active": "False",
    "type": "slack",
    "api_key": "YOUR_API_KEY"
  },
  {
    "active": "False",
    "type": "telegram",
    "bot_token": "YOUR_BOT_TOKEN",
    "chat_id": "YOUR_CHAT_ID"
  },
  {
    "active": "False",
    "type": "twilio",
    "account_sid": "YOUR_API_KEY",
    "auth_token": "YOUR_AUTH_TOKEN",
    "from_number": "YOUR_FROM_NUM",
    "to_number": "YOUR_TO_NUM"
  },
  {
    "active": "False",
    "type": "twitter",
    "access_token": "YOUR_ACCESS_TOKEN",
    "access_secret": "YOUR_ACCESS_SECRET",
    "consumer_key": "YOUR_CONSUMER_KEY",
    "consumer_secret": "YOUR_CONSUMER_SECRET"
  }
]
```

Each alarm requires some sort of API key or URL so that PokeAlarm can gain permissions to post.  Visit the wiki page of the service you are setting up to make sure you have the proper config.

Each alarm setting runs independent of the other alarms, so changes to one alarm do not affect the others (even if they are of the same type).

If is perfectly valid to have any combination of services, including repeats. 

## Customizing Alerts

Most alarms have customizable fields for each alert that allow you to insert your own message. This allows your to override the standard message and provide your own. You may customize as few or as many fields as you want - any field not present in your config will reset to default.

In order to customize an Alert, you must specify what type of alert you want to config: Either `pokemon`, `pokestop`, or `gym`. Each of these has different defaults available. The following is a config where a portion of the Alert has been updated:

```json
{
  "type":"slack",
  "active":"True",
  "api_key":"YOUR_API_KEY_HERE",
	"pokemon":{
		"channel":"Pokemon",
		"username":"<pkmn>",
		"title":"A GIANT <pkmn> jumped out of the grass!",
		"body": "Available until <24h_time> (<time_left>)."
	},
	"pokestop":{
		"channel":"Pokestop",
		"title":"Someone  has placed a lure on a Pokestop!",
		"body":"Better hurry! The lure only has <time_left> remaining!"
	}
}
```
For more information about Dynamic Text Substitutions (the `<text>`), please see the Dynamic Text Substitution wiki. 

For what service has what fields, please check the specific wiki page for that service.

## Example `alarms.json`

Below is a working alarm configuration for discord and slack:

```json
[
  {
    "active": "True",
    "type":"discord",
    "api_key":"DISCORD_WEBHOOK_URL_FOR_FALLBACK",
    "startup_message":"False",
    "startup_list":"False",
    "pokemon":{
      "api_key":"DISCORD_WEBHOOK_URL_FOR_POKEMON_CHANNEL",
      "username":"<pkmn>",
      "icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/<pkmn_id>.png",
      "title": "[<neighborhood>] <pkmn> (<iv>% <atk>/<def>/<sta>, <move_1> / <move_2>) at <address> <postal>",
      "url": "<gmaps>",
      "body": "Available until <24h_time> (<time_left> remaining)"
    },
    "pokestop":{
      "username":"Pokestop",
      "api_key":"DISCORD_WEBHOOK_URL_FOR_POKESTOP_CHANNEL",
      "icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/pokestop.png",
      "title": "[<neighborhood>] <address> <postal>",
      "url": "<gmaps>",
      "body": "expires at <24h_time> (<time_left>)."
    },
    "gym":{
      "api_key":"DISCORD_WEBHOOK_URL_FOR_GYM_CHANNEL",
      "username":"Pokemon Gym",
      "icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/gym_<team_id>.png",
      "title": "[<neighborhood>] <address> <postal>",
      "url": "<gmaps>",
      "body": "A team <old_team> gym has fallen to <new_team>."
    }
  },
  {
    "active": "True",
    "type": "slack",
    "api_key": "YOUR_SLACK_API_KEY",
    "startup_message": "False",
    "startup_list": "False",
    "pokemon": {
      "channel": "pokemon",
      "username": "Pokemon",
      "icon_url": "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/<pkmn_id>.png",
      "title": "*<pkmn>* (<iv>% <atk>/<def>/<sta>) in <neighborhood> at <address> <postal>",
      "url": "<gmaps>",
      "body": "Available until <24h_time> (<time_left>)\n*Moves:* <move_1> / <move2_>",
      "map": {
        "enabled": "True",
        "width": "330",
        "height": "250",
        "maptype": "roadmap",
        "zoom": "17"
      }
    },
    "pokestop": {
      "channel": "pokestops",
      "username": "Pokestop",
      "icon_url": "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/pokestop.png",
      "title": "[<neighborhood>] <address> <postal>",
      "url": "<gmaps>",
      "body": "expires at <24h_time> (<time_left>).",
      "map": {
        "enabled": "False",
        "width": "330",
        "height": "250",
        "maptype": "roadmap",
        "zoom": "15"
      }
    },
    "gym": {
      "channel": "gyms",
      "username": "Gym",
      "icon_url": "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/gym_<team_id>.png",
      "title": "[<neighborhood>] <address> <postal>",
      "url": "<gmaps>",
      "body": "A team <old_team> gym has fallen to <new_team>.",
      "map": {
        "enabled": "True",
        "width": "330",
        "height": "250",
        "maptype": "terrain",
        "zoom": "13"
      }
    }
  }
]
```

Note both have `"active":"true"` set, meaning both alarms are enabled.  Setting either to "false" will disable the specific alarm.. This allows you to have alarms set up and ready to go, but only enabled when you want them.

Visit the wiki article on [Filters](Filters) to limit pokemon notifications by distance, %IV, and moves with the `filters.json` file.
