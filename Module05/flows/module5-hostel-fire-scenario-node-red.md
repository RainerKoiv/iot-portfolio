```
[
    {
        "id": "46779913630be1ca",
        "type": "tab",
        "label": "Flow 1",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "0d897d5a911276fd",
        "type": "mqtt in",
        "z": "46779913630be1ca",
        "name": "",
        "topic": "co2/gas/CO2ppm",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "2d048788753b0ebb",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 80,
        "y": 180,
        "wires": [
            [
                "0034df02902c9e1d"
            ]
        ]
    },
    {
        "id": "15139a5f26c7cb34",
        "type": "mqtt in",
        "z": "46779913630be1ca",
        "name": "",
        "topic": "co2/gas/temp",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "2d048788753b0ebb",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 70,
        "y": 240,
        "wires": [
            [
                "63de5a2251a4c78c"
            ]
        ]
    },
    {
        "id": "2b5f0b9df5a8d451",
        "type": "mqtt in",
        "z": "46779913630be1ca",
        "name": "",
        "topic": "distance/distance",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "2d048788753b0ebb",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 600,
        "y": 420,
        "wires": [
            [
                "8c473d93341b5030"
            ]
        ]
    },
    {
        "id": "51abf08ec33a0abe",
        "type": "mqtt out",
        "z": "46779913630be1ca",
        "name": "",
        "topic": "servo_node/door/set",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "2d048788753b0ebb",
        "x": 1440,
        "y": 320,
        "wires": []
    },
    {
        "id": "0abd18b366bc708d",
        "type": "change",
        "z": "46779913630be1ca",
        "name": "90",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "90",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1070,
        "y": 420,
        "wires": [
            [
                "d71a18e2fc7dca9d",
                "07e529917eeebfee"
            ]
        ]
    },
    {
        "id": "change1",
        "type": "change",
        "z": "46779913630be1ca",
        "name": "tag sensor1",
        "rules": [
            {
                "t": "set",
                "p": "topic",
                "pt": "msg",
                "to": "sensor1",
                "tot": "str"
            }
        ],
        "x": 390,
        "y": 180,
        "wires": [
            [
                "joiner"
            ]
        ]
    },
    {
        "id": "change2",
        "type": "change",
        "z": "46779913630be1ca",
        "name": "tag sensor2",
        "rules": [
            {
                "t": "set",
                "p": "topic",
                "pt": "msg",
                "to": "sensor2",
                "tot": "str"
            }
        ],
        "x": 390,
        "y": 240,
        "wires": [
            [
                "joiner"
            ]
        ]
    },
    {
        "id": "joiner",
        "type": "function",
        "z": "46779913630be1ca",
        "name": "combine both",
        "func": "// store values\nlet s1 = flow.get(\"s1\");\nlet s2 = flow.get(\"s2\");\n\nif (msg.topic === \"sensor1\") {\n    s1 = msg.payload;\n    flow.set(\"s1\", s1);\n}\n\nif (msg.topic === \"sensor2\") {\n    s2 = msg.payload;\n    flow.set(\"s2\", s2);\n}\n\nif (s1 !== undefined && s2 !== undefined) {\n    let out = {\n        payload: {\n            sensor1: s1,\n            sensor2: s2\n        }\n    };\n\n    flow.set(\"s1\", undefined);\n    flow.set(\"s2\", undefined);\n\n    return out;\n}\n\nreturn null;",
        "outputs": 1,
        "timeout": "",
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 570,
        "y": 210,
        "wires": [
            [
                "f2d548b31007dbca",
                "f51770145dbde16a",
                "ee666db54a2e4556"
            ]
        ]
    },
    {
        "id": "f2d548b31007dbca",
        "type": "switch",
        "z": "46779913630be1ca",
        "name": "",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "nnull"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 1,
        "x": 750,
        "y": 300,
        "wires": [
            [
                "f3d00762612997f7",
                "0f125356ba456eeb"
            ]
        ]
    },
    {
        "id": "f51770145dbde16a",
        "type": "trigger",
        "z": "46779913630be1ca",
        "name": "10s no message watchdog",
        "op1": "",
        "op2": "No message received for 10 seconds",
        "op1type": "nul",
        "op2type": "str",
        "duration": "10",
        "extend": true,
        "overrideDelay": false,
        "units": "s",
        "reset": "",
        "bytopic": "all",
        "topic": "topic",
        "outputs": 1,
        "x": 840,
        "y": 360,
        "wires": [
            [
                "0abd18b366bc708d",
                "b9cc1012a3d4a66b",
                "2376568b2a0a04dc"
            ]
        ]
    },
    {
        "id": "cce70885556c7ab4",
        "type": "mqtt out",
        "z": "46779913630be1ca",
        "name": "",
        "topic": "bzz/bzz/set",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "2d048788753b0ebb",
        "x": 1310,
        "y": 220,
        "wires": []
    },
    {
        "id": "b9cc1012a3d4a66b",
        "type": "change",
        "z": "46779913630be1ca",
        "name": "0",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "0",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1090,
        "y": 220,
        "wires": [
            [
                "cce70885556c7ab4"
            ]
        ]
    },
    {
        "id": "63de5a2251a4c78c",
        "type": "switch",
        "z": "46779913630be1ca",
        "name": "",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "gt",
                "v": "26",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 1,
        "x": 230,
        "y": 240,
        "wires": [
            [
                "change2"
            ]
        ]
    },
    {
        "id": "0034df02902c9e1d",
        "type": "switch",
        "z": "46779913630be1ca",
        "name": "",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "gt",
                "v": "1600",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 1,
        "x": 230,
        "y": 180,
        "wires": [
            [
                "change1"
            ]
        ]
    },
    {
        "id": "935d55b324151e2c",
        "type": "inject",
        "z": "46779913630be1ca",
        "name": "",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "0",
        "payloadType": "num",
        "x": 1090,
        "y": 140,
        "wires": [
            [
                "cce70885556c7ab4"
            ]
        ]
    },
    {
        "id": "2338b2ff8bb541ce",
        "type": "inject",
        "z": "46779913630be1ca",
        "name": "",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "5",
        "payloadType": "num",
        "x": 1090,
        "y": 100,
        "wires": [
            [
                "cce70885556c7ab4"
            ]
        ]
    },
    {
        "id": "72e0de2aa83420fc",
        "type": "inject",
        "z": "46779913630be1ca",
        "name": "",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "0",
        "payloadType": "num",
        "x": 1110,
        "y": 560,
        "wires": [
            [
                "51abf08ec33a0abe"
            ]
        ]
    },
    {
        "id": "4138c3919d14faf5",
        "type": "inject",
        "z": "46779913630be1ca",
        "name": "",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "90",
        "payloadType": "num",
        "x": 1110,
        "y": 520,
        "wires": [
            [
                "51abf08ec33a0abe"
            ]
        ]
    },
    {
        "id": "f3d00762612997f7",
        "type": "change",
        "z": "46779913630be1ca",
        "name": "5",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "5",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 950,
        "y": 280,
        "wires": [
            [
                "cce70885556c7ab4"
            ]
        ]
    },
    {
        "id": "0f125356ba456eeb",
        "type": "change",
        "z": "46779913630be1ca",
        "name": "-90",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "-90",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 950,
        "y": 320,
        "wires": [
            [
                "d71a18e2fc7dca9d",
                "07e529917eeebfee"
            ]
        ]
    },
    {
        "id": "ee666db54a2e4556",
        "type": "debug",
        "z": "46779913630be1ca",
        "name": "start",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 750,
        "y": 120,
        "wires": []
    },
    {
        "id": "78a31aabc8d6a236",
        "type": "debug",
        "z": "46779913630be1ca",
        "name": "distance",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 1000,
        "y": 460,
        "wires": []
    },
    {
        "id": "2376568b2a0a04dc",
        "type": "debug",
        "z": "46779913630be1ca",
        "name": "watchdog",
        "active": false,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 1080,
        "y": 360,
        "wires": []
    },
    {
        "id": "8c473d93341b5030",
        "type": "rbe",
        "z": "46779913630be1ca",
        "name": "",
        "func": "rbe",
        "gap": "",
        "start": "",
        "inout": "out",
        "septopics": true,
        "property": "payload",
        "topi": "topic",
        "x": 810,
        "y": 420,
        "wires": [
            [
                "0abd18b366bc708d",
                "78a31aabc8d6a236"
            ]
        ]
    },
    {
        "id": "d71a18e2fc7dca9d",
        "type": "rbe",
        "z": "46779913630be1ca",
        "name": "",
        "func": "rbe",
        "gap": "",
        "start": "",
        "inout": "out",
        "septopics": false,
        "property": "payload",
        "topi": "topic",
        "x": 1250,
        "y": 320,
        "wires": [
            [
                "51abf08ec33a0abe"
            ]
        ]
    },
    {
        "id": "07e529917eeebfee",
        "type": "debug",
        "z": "46779913630be1ca",
        "name": "debug 6",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 1280,
        "y": 380,
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