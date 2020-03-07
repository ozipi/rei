# rei
QCK Prism Cloth Javascript interface

# Description
## what
Rei is a javascript interface between the [Gamesense Engine](https://github.com/SteelSeries/gamesense-sdk/) and any other app through a server listening for notification messages.

## why
To control [QCK Prism Cloth XL](https://steelseries.com/gaming-mousepads/qck-prism-series) to receive external visual notifications from other apps via plugins.

## research
### GameSense notifications flow
The GameSense engine receives `POST` calls on the server port ([references on GameSense server and port](https://github.com/SteelSeries/gamesense-sdk/blob/master/doc/api/sending-game-events.md)).


- execute steelseries engine and check the port`
    ```
    cat /Library/Application\ Support/SteelSeries\ Engine\ 3/coreProps.json
    ```

Let's say we are going to bind a new game called `JOBSTATUS` with a `HEALTH` event to send `HEALTH` notifications to the `Prism Cloth`

- Bind Game event

    `POST 127.0.0.1:49494/bind_game_event`
    ```
    {
        "game": "JOBSTATUS",
        "event": "HEALTH",
        "min_value": 0,
        "max_value": 100,
        "handlers": [
            {
            "device-type": "indicator",
            "zone": "one",
            "color": {
                "red": 255,
                "green": 247,
                "blue": 0
            },
            "mode": "color"
            }
        ]
    }
    ```

- And then we are ready to send Game Events to the `JOBSTATUS` game
   
    `POST 127.0.0.1:49494/game_event`
    ```
    {
        "game": "JOBSTATUS",
        "event": "HEALTH",
        "data": {
            "value": 75
        }
    }
    ```

    ![qck-yellow](docs/qck-yellow-low.gif)

- Remember to send a heatbeat (color event lasts 15s adn then returns to the previous color loop it was before the event)

    `POST 127.0.0.1:49494/game_heartbeat`
    ```
    {
        "game": "JOBSTATUS"
    }
    ```

## Project plan

### Mindmap
![rei-mindmap](docs/rei-mindmap.jpeg)

## Roadmap
- [x] Create Repo and project description
- [ ] create server and initial notification system
- [ ] create plugins structure
- [ ] create plugins
- [ ] research linux 

## References
### Gamesense Engine SDK
https://github.com/SteelSeries/gamesense-sdk/blob/master/doc/api/sending-game-events.md

### future research
#### Linux Research (no drivers support)
- https://purplepalmdash.github.io/2016/02/03/hacking-steelseries-engine-3-usb-mouse-under-linux/

- https://github.com/antage/ssv2leds/

#### Unrestricting Gamesense
- https://techblog.steelseries.com/2016/04/06/gamesense-arduino.html


Ozipi