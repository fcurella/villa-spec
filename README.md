# Villa

__Status__ _Draft_  
__Version__ `villa-v0.0.1`  
__License__ _BSD-3-clause_  

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
    "protocols": [
        "villa-v0.0.1"
    ],
    "devices": [
        {
            "name": "Thermostat",
            "url": "http://192.168.100.1/thermostat/"
        },
        {
            "name": "Front Porch",
            "url": "http://192.168.100.1/frontporch/"
        },
        {
            "name": "Thermometer",
            "url": "http://192.168.100.1/thermostat/thermometer/"
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
    "protocols": [
        "villa-v0.0.1"
    ],
    "actions": [
        {
            "name": "Furnace Status",
            "description": "Turns the furnace on or off."
            "url": "http://192.168.100.1/thermostat/furnace/status/",
            "type": "bool",
        },
        {
            "name": "AC Status",
            "description": "Turns the AC on or off."
            "url": "http://192.168.100.1/thermostat/ac/status/",
            "type": "bool",
        },
        {
            "name": "Mode",
            "description": "Set the thermostat mode. Possible values are 'HOT', 'COLD', or 'OFF'",
            "type": "str"
        }
        {
            "name": "Set HOT Temperature",
            "description": "Set a temperature for the furnace in HOT mode"
            "url": "http://192.168.100.1/thermostat/hot_temp/",
            "type": "float",
            "unit": "F",
        },
        {
            "name": "Set COLD Temperature",
            "description": "Set a temperature for the furnace in COLD mode"
            "url": "http://192.168.100.1/thermostat/cold_temp/",
            "type": "float",
            "unit": "F",
        }
    ],
    "properties": [
        {
            "name": "Temperature",
            "url": "http://192.168.100.1/thermostat/thermometer/temperature/",
            "type": "float",
            "unit": "F",
            "value": 59.0
        },
        {
            "name": "Mode",
            "description": "Get the thermostat mode. Possible values are 'HOT', 'COLD', or 'OFF'",
            "url": "http://192.168.100.1/thermostat/furnace/mode/",
            "type": "str",
            "value": "HOT"
        }
        {
            "name": "Furnace Status",
            "url": "http://192.168.100.1/thermostat/furnace/status/",
            "type": "bool",
            "value": True
        },
        {
            "name": "AC Status",
            "url": "http://192.168.100.1/thermostat/ac/status/",
            "type": "bool",
            "value": false
        },
        {
            "name": "Get HOT Temperature",
            "description": "Get the temperature set for the furnace"
            "url": "http://192.168.100.1/thermostat/hot_temp/",
            "type": "float",
            "unit": "F",
            "value": 69.0
        },
        {
            "name": "Get COLD Temperature",
            "description": "Get the temperature set for the AC"
            "url": "http://192.168.100.1/thermostat/cold_temp/",
            "type": "float",
            "unit": "F",
            "value": 79.0
        }
    ],
    "devices": [
        {
            "name": "Thermometer",
            "url": "http://192.168.100.1/thermostat/thermometer/"
        },
        {
            "name": "Furnace",
            "url": "http://192.168.100.1/thermostat/furnace/"
        }
        {
            "name": "Air conditioning",
            "url": "http://192.168.100.1/thermostat/ac/"
        }
    ]
}

```

`curl -x GET http://192.168.100.1/thermostat/hot_temp/`

```
HTTP/1.1 200 OK
Content-Type: application/json
Connection: close

{
    "protocols": [
        "villa-v0.0.1"
    ],
    "name": "Temperature",
    "url": "http://192.168.100.1/thermostat/hot_temp/",
    "type": "float",
    "unit": "F",
    "value": 65.0
}

```

`curl -x PUT http://192.168.100.1/thermostat/hot_temp/ -H 'Content-Type: application/json' -H 'X-Villa-Version: v0.0.1' --data '{"name": "Temperature", "type": "float", "unit": "F", "value": 69.0}'`

```
HTTP/1.1 200 OK
Content-Type: application/json
Connection: close

{
    "protocols": [
        "villa-v0.0.1"
    ],
    "name": "Temperature",
    "url": "http://192.168.100.1/thermostat/hot_temp/",
    "type": "float",
    "unit": "F",
    "value": 69.0
}

```

`curl -x PATCH http://192.168.100.1/thermostat/hot_temp/ -H 'Content-Type: application/json' -H 'X-Villa-Version: v0.0.1' --data '{"value": 70.0}'`

```
HTTP/1.1 200 OK
Content-Type: application/json
Connection: close

{
    "protocols": [
        "villa-v0.0.1"
    ],
    "name": "Temperature",
    "url": "http://192.168.100.1/thermostat/hot_temp/",
    "type": "float",
    "unit": "F",
    "value": 70.0
}

```

## API Cheatsheet

### Common fields

Mandatory fields:

* `name`
* `url`
* `protocols` (only if object is the top-level object)

Optional fields:

* `description` Defaults to `''`
* `protocols` Defaults to the closest ancestor's `protocols` value.

### Devices

Common fields, plus:

* Optional fields:
    * `actions` Defaults to `[]`
    * `properties` Defaults to `[]`
    * `devices` Defaults to `[]`
    * `protocols` Defaults to the closest ancestor's `protocol` value.

### Actions 

Common fields, plus:

* Mandatory fields:
    * `type` supported types are `"str"`, `"float"`, `"int"`, `"bool"` and `"error"`
* Optional fields:
    * `unit`

### Properties

Actions' fields, plus:

* Mandatory fields:
    * `value`

## Specification

`Villa` is a REST API to describe and operate smart devices in your home (or farm, or whatever you like).

Its output is a JSON representation of a tree of devices. Any device can expose inputs, outputs, or other devices.

The root of the tree is itself a device. A device is identified by its `url`. A device must have a `name` and a `url`.

A device must specify with version of the villa protocol is using. This is specified in the `protocol` key.

A device may expose other devices. Children devices inherit the protocol from their parent or they may switch to a different one by specifying their own `protocol` key.

Children devices must be represented only by their `name`, `url`, and optionally their `description`.

Any device may expose devices that are children of other, unrelated devices. For example, suppose a `home` device contains a `thermostat`, which in turns contains a `thermometer`. The `home` device may choose to expose the `thermometer` device so it can be queried without interrogating `thermostat`.

Devices may expose actions and properties (collectively called IO Objects). A device may choose to expose any other, unrelated device's actions and properties as theirs.

Properties' URLs must be requested by using the `GET` HTTP verb. Actions' URLs must be requested using the `PUT` or the `PATCH` HTTP verbs. An Action and a Property may share the same URL (eg. for reading and setting a lightbulb status), in which case `GET`, `PUT` and `PATCH` are all allowed verbs for the URL.

### Protocol versioning

Every object must specifies which protocols can understand and speak.

The top object in every response must contain the `protocols` key. Its value must be an array of protocol version names. Children objects inherits their parent's protocols by default, or they may override it by providing their own `protocols` value.

The current version name is `villa-v0.0.1`.

## TODO
* maybe add an 'options' or `enum` type
* think more about versioning
* moar examples
* Error handling
* POST and DELETE methods?
