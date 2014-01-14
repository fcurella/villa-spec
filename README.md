# Villa

__Status__ _Draft_

A REST API to describe smart devices in your home (or farm, or whatever you like).

## Examples

`curl -x GET http://192.168.100.1/`

```
HTTP/1.1 200 OK
Content-Type: application/json
Connection: close

{
    "name": "home",
    "url": "http://192.168.100.1/",
    "protocol": "villa-v0.0.1",
    "devices": [
        {
            "name": "Thermostat",
            "url": "http://192.168.100.1/thermostat/",
        },
        {
            "name": "Front Porch",
            "url": "http://192.168.100.1/frontporch/",
        },
        {
            "name": "Thermometer",
            "url": "http://192.168.100.1/thermostat/thermometer/",
        }
    ]
}

```

`curl -x GET http://192.168.100.1/thermostat/`

```
HTTP/1.1 200 OK
Content-Type: application/json
Connection: close

{
    "name": "Thermostat",
    "description": "",
    "url": "http://192.168.100.1/thermostat/",
    "protocol": "villa-v0.0.1",
    "input": [
        {
            "name": "Furnace Status",
            "description": "Turns the furnace on or off."
            "url": "http://192.168.100.1/thermostat/furnace/status/",
            "type": "bool"
        },
        {
            "name": "AC Status",
            "description": "Turns the AC on or off."
            "url": "http://192.168.100.1/thermostat/ac/status/",
            "type": "bool"
        },
        {
            "name": "Mode",
            "description": "Set the thermostat mode. Possible values are 'HOT', 'COLD', or 'OFF'",
            "url": "http://192.168.100.1/thermostat/furnace/mode/",
            "type": "char"
        }
        {
            "name": "Set HOT Temperature",
            "description": "Set a temperature for the furnace in HOT mode"
            "url": "http://192.168.100.1/thermostat/furnace/hot_temp/",
            "type": "float",
            "unit": "F"
        },
        {
            "name": "Set COLD Temperature",
            "description": "Set a temperature for the furnace in COLD mode"
            "url": "http://192.168.100.1/thermostat/furnace/cold_temp/",
            "type": "float",
            "unit": "F"
        }
    ],
    "output": [
        {
            "name": "Temperature",
            "url": "http://192.168.100.1/thermostat/thermometer/temperature/",
            "type": "float",
            "unit": "F"
        },
        {
            "name": "Mode",
            "description": "Get the thermostat mode. Possible values are 'HOT', 'COLD', or 'OFF'",
            "url": "http://192.168.100.1/thermostat/furnace/mode/",
            "type": "char"
        }
        {
            "name": "Furnace Status",
            "url": "http://192.168.100.1/thermostat/furnace/status/",
            "type": "bool"
        },
        {
            "name": "AC Status",
            "url": "http://192.168.100.1/thermostat/ac/status/",
            "type": "bool"
        },
    ],
    "devices": [
        {
            "name": "Thermometer",
            "url": "http://192.168.100.1/thermostat/thermometer/",
        },
        {
            "name": "Furnace",
            "url": "http://192.168.100.1/thermostat/furnace/",
        }
        {
            "name": "Air conditioning",
            "url": "http://192.168.100.1/thermostat/ac/",
        }
    ]
}

```

`curl -x GET http://192.168.100.1/thermostat/thermometer/temperature/`

```
HTTP/1.1 200 OK
Content-Type: application/json
Connection: close

{
    "name": "Temperature",
    "url": "http://192.168.100.1/thermostat/thermometer/temperature/",
    "type": "float",
    "unit": "F",
    "value": 69.0
}

```

## API Cheatsheet

### Devices

Mandatory fields:

* `name`
* `url`
* `protocol` (required only for the top device)

Optional fields:

* `description` Defaults to `''`
* `input` Defaults to `[]`
* `output` Defaults to `[]`
* `devices` Defaults to `[]`
* `protocol` Defaults to the closest ancestor's `protocol` value.

### Inputs and Outputs 

Mandatory fields:

* `name`
* `url`
* `type` supported types are `str`, `float`, `int`, `bool` and `error`

Optional fields:

*  unit

### Values

Just like outputs, but with an additional, required `value`.

Mandatory fields:

* `name`
* `url`
* `type` supported types are `str`, `float`, `int`, `bool` and `error`
* `value`

Optional fields:

*  `unit` Defaults to `null`

## Specification

`Villa` is a REST API to describe smart devices in your home (or farm, or whatever you like).

Its output is a JSON representation of a tree of devices. Any device can expose inputs, outputs, or other devices.

The root of the tree is itself a device. A device is identified by its `url`. A device must have a `name` and a `url`.

A device must specify with version of the villa protocol is using. This is specified in the `protocol` key.

A device may expose other devices. Children devices inherit the protocol from their parent or they may switch to a different one by specifying their own `protocol` key.

Children devices must be represented only by their `name`, `url`, and optionally their `description`.

Any device may expose devices that are children of other, unrelated devices. For example, a `home` device contains a `thermostat`, which in turns contains a `thermometer`. The `home` device may choose to expose the `thermometer` device so it can be queried without interrogating `thermostat`.

Devices may expose inputs and outputs. A device may choose to expose another, unrelated device's inputs and outputs as theirs.

Outputs' URLs must be requested by using the `GET` HTTP verb. Inputs' URLs must be requested using the `PUT` HTTP verb. And input and output may share the same URL (eg. for reading and setting a lightbulb status), in which case both `GET` and `PUT` are allowed verbs the URL.

## TODO

* moar examples
* Error handling
* POST and DELETE methods?
