```
[
    {
        "id": "f9e0322178022cc3",
        "type": "tab",
        "label": "Flow 1",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "50438bffd7fb3357",
        "type": "mqtt in",
        "z": "f9e0322178022cc3",
        "name": "",
        "topic": "rfid-reader/reader/uid",
        "qos": "2",
        "datatype": "utf8",
        "broker": "2d048788753b0ebb",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 280,
        "y": 460,
        "wires": [
            [
                "193208168c944a15",
                "17a2d8f03cdb36b2"
            ]
        ]
    },
    {
        "id": "affe8bdf2e7f3c94",
        "type": "function",
        "z": "f9e0322178022cc3",
        "name": "function 2",
        "func": "let uid = msg.payload;\n\n// DEFINE YOUR CARDS HERE\nlet allowed = [\n    \"A35DCE09deny\",   // card 1\n    \"03279297\"    // blue keychain\n];\n\nif (allowed.includes(uid)) {\n    msg.payload = \"granted\"\n} else {\n    msg.payload = \"denied\"\n}\n\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 600,
        "y": 460,
        "wires": [
            [
                "da6f39b5fbd3f042",
                "30c09d4a29d52442",
                "3ba841a437c56aeb",
                "599bae2a644afc44",
                "e0b3013fa163f858"
            ]
        ]
    },
    {
        "id": "5013a7cb3c87ba8d",
        "type": "mqtt out",
        "z": "f9e0322178022cc3",
        "name": "",
        "topic": "buzzer/buzzer/frequency/set",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "2d048788753b0ebb",
        "x": 1120,
        "y": 620,
        "wires": []
    },
    {
        "id": "30c09d4a29d52442",
        "type": "change",
        "z": "f9e0322178022cc3",
        "name": "",
        "rules": [
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "granted",
                "fromt": "str",
                "to": "1000",
                "tot": "num"
            },
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "denied",
                "fromt": "str",
                "to": "512",
                "tot": "num"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 840,
        "y": 620,
        "wires": [
            [
                "5013a7cb3c87ba8d",
                "5db59a359f7816fe"
            ]
        ]
    },
    {
        "id": "832cf7e486a0fb70",
        "type": "mqtt out",
        "z": "f9e0322178022cc3",
        "name": "",
        "topic": "buzzer/buzzer/set",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "2d048788753b0ebb",
        "x": 1490,
        "y": 560,
        "wires": []
    },
    {
        "id": "da6f39b5fbd3f042",
        "type": "change",
        "z": "f9e0322178022cc3",
        "name": "",
        "rules": [
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "granted",
                "fromt": "str",
                "to": "10",
                "tot": "str"
            },
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "denied",
                "fromt": "str",
                "to": "5",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 840,
        "y": 560,
        "wires": [
            [
                "832cf7e486a0fb70",
                "ab50d1f25827c201",
                "beaad8e9846cdf86"
            ]
        ]
    },
    {
        "id": "ab50d1f25827c201",
        "type": "debug",
        "z": "f9e0322178022cc3",
        "name": "debug 8",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 1460,
        "y": 480,
        "wires": []
    },
    {
        "id": "4f7e91e03f56b625",
        "type": "change",
        "z": "f9e0322178022cc3",
        "name": "",
        "rules": [
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "10",
                "fromt": "str",
                "to": "0",
                "tot": "str"
            },
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "5",
                "fromt": "str",
                "to": "0",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1240,
        "y": 480,
        "wires": [
            [
                "ab50d1f25827c201",
                "832cf7e486a0fb70"
            ]
        ]
    },
    {
        "id": "193208168c944a15",
        "type": "switch",
        "z": "f9e0322178022cc3",
        "name": "",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "neq",
                "v": "none",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 1,
        "x": 470,
        "y": 460,
        "wires": [
            [
                "affe8bdf2e7f3c94",
                "17a2d8f03cdb36b2"
            ]
        ]
    },
    {
        "id": "3ba841a437c56aeb",
        "type": "debug",
        "z": "f9e0322178022cc3",
        "name": "debug 7",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 820,
        "y": 700,
        "wires": []
    },
    {
        "id": "5db59a359f7816fe",
        "type": "debug",
        "z": "f9e0322178022cc3",
        "name": "debug 3",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 1060,
        "y": 700,
        "wires": []
    },
    {
        "id": "599bae2a644afc44",
        "type": "mqtt out",
        "z": "f9e0322178022cc3",
        "name": "",
        "topic": "display/oled",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "2d048788753b0ebb",
        "x": 1390,
        "y": 340,
        "wires": []
    },
    {
        "id": "beaad8e9846cdf86",
        "type": "delay",
        "z": "f9e0322178022cc3",
        "name": "",
        "pauseType": "delay",
        "timeout": "1",
        "timeoutUnits": "seconds",
        "rate": "1",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": false,
        "allowrate": false,
        "outputs": 1,
        "x": 1060,
        "y": 480,
        "wires": [
            [
                "4f7e91e03f56b625"
            ]
        ]
    },
    {
        "id": "e0b3013fa163f858",
        "type": "delay",
        "z": "f9e0322178022cc3",
        "name": "",
        "pauseType": "delay",
        "timeout": "1",
        "timeoutUnits": "seconds",
        "rate": "1",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": false,
        "allowrate": false,
        "outputs": 1,
        "x": 920,
        "y": 240,
        "wires": [
            [
                "2a2083b78c6264af"
            ]
        ]
    },
    {
        "id": "2a2083b78c6264af",
        "type": "change",
        "z": "f9e0322178022cc3",
        "name": "",
        "rules": [
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "granted",
                "fromt": "str",
                "to": "&&clear",
                "tot": "str"
            },
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "denied",
                "fromt": "str",
                "to": "&&clear",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1120,
        "y": 240,
        "wires": [
            [
                "599bae2a644afc44",
                "44d30fdb804f9ab9"
            ]
        ]
    },
    {
        "id": "44d30fdb804f9ab9",
        "type": "debug",
        "z": "f9e0322178022cc3",
        "name": "debug 4",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 1320,
        "y": 240,
        "wires": []
    },
    {
        "id": "17a2d8f03cdb36b2",
        "type": "debug",
        "z": "f9e0322178022cc3",
        "name": "debug 5",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 600,
        "y": 360,
        "wires": []
    },
    {
        "id": "2d048788753b0ebb",
        "type": "mqtt-broker",
        "name": "",
        "broker": "192.168.14.1",
        "port": 1883,
        "clientid": "",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": 4,
        "keepalive": 60,
        "cleansession": true,
        "autoUnsubscribe": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "false",
        "birthPayload": "",
        "birthMsg": {},
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "false",
        "closePayload": "",
        "closeMsg": {},
        "willTopic": "",
        "willQos": "0",
        "willRetain": "false",
        "willPayload": "",
        "willMsg": {},
        "userProps": "",
        "sessionExpiry": ""
    }
]
```