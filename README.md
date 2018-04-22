# home-assistant-config

My personal configuration for open-source home automation platform [Home Assistant](https://home-assistant.io).

Sensitive information has been purportedly removed (i.e. `secrets.yaml`), but take a look at the [file structure](#file-structure) to understand what you may or may not need.

[![build status][travis-image]][travis-url]

## Usage

```
❯ git clone git@github.com:ruimarinho/home-assistant-config.git
❯ mv home-assistant-config/secrets.sample.yaml home-assistant-config/secrets.yaml
```

Replace all sample secrets by your own keys. Naturally, it assumes you will have exactly the same devices as me, but the goal of this repository is to share ideas.

![basement](https://user-images.githubusercontent.com/288709/39090005-03e3e222-45cb-11e8-9100-e6a721bbf492.png)

## Features

- Automatically turning on and off the hall and garden lights before and after sunrise and sunset, respectively.
- Listing the weather forecast.
- Monitoring of pollution levels in the neighborhood.
- Monitoring the fridge power consumption.
- Notifications when the front door is opened.
- Security monitoring via UniFi cameras.

## File Structure

### Included

All files are logically separated into `groups` (merged together under the `group` key) and `includes`. The exception is `automations.yaml` at the root level as it is required for the Automation UI Editor to work properly.

- `automations.yaml`
- `configuration.yaml`
- `groups/`
- `includes/`

### Excluded

The following list of files are present on the working dir of Home Assistant but have been excluded from this configuration repository as they can either be recreated on-the-fly when the instance restarts or they can be reconstructed with minor effort.

- `.HA_VERSION`: the current Home Assistant version.
- `.ios.conf`: contains the most recent state of all registered iOS devices. Deleting this file will not disable the devices and the file will be recreated the next time a new device is connected or an existing one reconnects.
- `.uuid`: instance unique ID for analytics purposes.
- `home-assistant_v2.db`: historical data storage (SQLite).
- `home-assistant.log`: applications logs.
- `known_devices.yaml`: a list of devices known including vendor names, MAC addresses and tracking settings.

### Z-Wave

If you're integrating with Z-Wave devices, you will likely have the following files on your working dir as well:

- `options.xml`
- `OZW_Log.txt`
- `pyozw.sqlite`
- `zwcfg_<HomeId>.xml`
- `zwscene.xml`

None of these files are critical, but there are some caveats if you lose them. The file `zwcfg_<HomeId>.xml` contains custom metadata assigned to nodes (like names, locations and return routes) that will be lost if the file is removed. The rest of the node information will be reconstructed back on restart and there is no impact on the Z-Wave network itself (device inclusion will not be affected in any way). Certain automations based on the node's names (the mentioned metadata) may be affected.

The `zwscene.xml` contains the scenes composition, but it's unlikely you're using them when paired with Home Assistant.

The `options.xml` will be recreated by Home Assistant as long as the Network Key is set on the `zwave.yaml` file.

Lastly, `OZW_Log.txt` and `pyozw.sqlite` are generated on demand and contain only volatile or cached data.

## License

MIT

[travis-image]: https://img.shields.io/travis/ruimarinho/home-assistant-config.svg?style=flat-square
[travis-url]: https://travis-ci.org/ruimarinho/home-assistant-config
