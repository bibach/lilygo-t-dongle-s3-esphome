# lilygo-t-dongle-s3-esphome
Lilygo T-Dongle-S3 running ESPHome

Based on the work of [landonr](https://github.com/landonr/lilygo-tdisplays3-esphome).

## Installation
You will first need to do a manual installation by putting the t-dongle-s3.yaml file into your esphome folder then using the modern format in ESPHome to get a local copy of the firmware and finally use https://web.esphome.io/ to install over USB.

### Method
Download a copy of this code and place the t-dongle-s3 folder in your esphome folder. Also place the t-dongle-s3.yaml and secrets.yaml into the esphome folder, ensuring to change the secrets.yaml details to your own credentials.

From your ESPHome dashboard, create a local copy of the t-dongle-s3 firmware by clicking the three dots > Install > Manual Download > Modern Format

To set the board into flash mode, hold the button on the bottom while plugging the dongle to your computer.

Navigate to https://web.esphome.io/ in a compatible browser (Chrome, Edge etc.) and click "Connect". Select the corresponding COM Port and click connect again. 

When the board has connected, click "install" and select the firmware that you have created, this will erase the board and write the new firmware.

When the firmware has been written to the board you will need to unplug and reconnect the dongle.

## Expanding on this for use in Home Assistant
You can use the standard tft_espi drawing functions (not the typical esphome "display:" section / drawing components). (Thanks  @jamesarm97)

To diplay a string of text from a text sensor add the sensor to your t-dongle-s3.yaml (make sure to edit the entity to match your own setup):

```
text_sensor:
  - platform: homeassistant
    id: kitchenmusictrack
    entity_id: media_player.kitchen_display
    attribute: media_title
  - platform: homeassistant
    id: kitchenmusicartist
    entity_id: media_player.kitchen_display
    attribute: media_artist
```

And amend your t-dongle-s3.yaml file to call these in the lambda script:
```
interval:
  - interval: 1s
    then:
    - lambda: |-
        displayControllerComponent->clear();
        displayControllerComponent->tft.setFreeFont(FMB12);
        displayControllerComponent->tft.drawString(id(kitchenmusicartist).state.c_str(),10,105);
        displayControllerComponent->tft.drawString(id(kitchenmusictrack).state.c_str(),10,125);
```
Save the t-dongle-s3.yaml file, create another Modern Format firmware, put the board into flash mode upload, the code and finally unplug and re-connect the dongle. Assuming you have used the correct credentials, you can now power the T-Dongle and as long as it is in Wifi range of your router, will display the (in the case of this example) Artist and Title of the song playing on the media player.
