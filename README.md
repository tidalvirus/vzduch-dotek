# Vzduch Dotek

An API to interface with AirTouch 3 AC controller.

## Why Vzduch Dotek

Vzduch Dotek translates to `Air Touch` in Czech (naming is always hard). Now as to `why` do this? Because I want local control of my AC unit. I run [Home Assistant](https://github.com/home-assistant) and while AirTouch 3 has IFTTT support, IFTTT is not ideal. IFTTT is still in the cloud and on top of that there is no feedback from IFTTT if the action succeeded or failed. So when doing any automatiions in Home Assistant it only works `most of the time`. I have created a `Home Assistant` custom component that wraps this api.

## Settings

The `appsettings.json` file contains a section for the AirTouch3 controller. You will need to provide the `localhost` and `localPort` of your AirTouch 3 unit`

```json
  "airTouch": {
    "localHost": "192.168.80.21",
    "localPort": 8899
  }
```

## Running

I used `vscode` to put this together. The default port the api runs on is `5353`. Once you pull down the repo, update the `appsettings.json` file you should be able to hit F5 and then navigate to `http://localhost:5353/api/aircons` and get the current status of your AC.

I have also include a `DockerFile` which runs the api. Note: The `DockerFile` relies on a compiled version of the api (Linux). I used this to create the artefacts required: `dotnet publish -c Release --self-contained -r linux-x64`

The build has been tested on Windows 10 and Linux (Ubuntu).

### Running and Building Within Docker

Clone this repo, cd into the source directoryi `cd vzduch-dotek`, edit appsettings.json, and run `docker build -f Dockerfile -t vzduch-dotek  .`

Once built, you should be able to run it as any other docker container, including putting into a docker-compose file. An example to get you started:

```
---
version: "2.1"
services:
  vzduch-dotek:
    container_name: vzduch-dotek
    image: vzduch-dotek
    restart: unless-stopped
    ports:
      - 5353:5353
```

### Examples

* Get current status: `http://localhost:5353/api/aircons`
* Switch aircon On: `http://localhost:5353/api/aircons/0/switch/1`
* Toggle aircon: `http://localhost:5353/api/aircons/0/toggle`
* Switch zone Off: `http://localhost:5353/api/aircons/0/zones/0/switch/0`

## Limitations

There are many. This code has been tested against one (yes just one) AirTouch 3 unit interfacing with a Daikin System reverse cycle ducted aircondition unit. It has not been tested with any other systems or combinations of. Please bear that in mind. Other limitations:
* AirTouch 3 supports controlling upto two AC units. `Vzduch Dotek` currently only supports controlling one AC unit.  
* Setting Date / Time is currently not supported
* Setting passwords not supported

## Todo

I want to add some level of security. It is running locally but some sort of auth would not go astray. I also plan to add tests and finish working on setting the date and time. Plus more.
