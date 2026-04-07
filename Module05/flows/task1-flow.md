[
    {
        "id": "97acc56f9d88ad0e",
        "type": "mqtt in",
        "z": "f9e0322178022cc3",
        "name": "",
        "topic": "first-node/button1",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "2d048788753b0ebb",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 280,
        "y": 160,
        "wires": [
            [
                "3f628ca46c1a4beb"
            ]
        ]
    },
    {
        "id": "3f628ca46c1a4beb",
        "type": "debug",
        "z": "f9e0322178022cc3",
        "name": "debug 1",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 560,
        "y": 160,
        "wires": []
    },
    {
        "id": "74493788e5a68e19",
        "type": "mqtt in",
        "z": "f9e0322178022cc3",
        "name": "",
        "topic": "first-node/button1",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "2d048788753b0ebb",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 280,
        "y": 240,
        "wires": [
            [
                "f89ed045106074f3"
            ]
        ]
    },
    {
        "id": "6cac6d6da21717db",
        "type": "debug",
        "z": "f9e0322178022cc3",
        "name": "debug 2",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 800,
        "y": 280,
        "wires": []
    },
    {
        "id": "f89ed045106074f3",
        "type": "change",
        "z": "f9e0322178022cc3",
        "name": "",
        "rules": [
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "pressed",
                "fromt": "str",
                "to": "on",
                "tot": "str"
            },
            {
                "t": "change",
                "p": "payload",
                "pt": "msg",
                "from": "released",
                "fromt": "str",
                "to": "off",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 540,
        "y": 240,
        "wires": [
            [
                "1993812fb5c692ac",
                "6cac6d6da21717db"
            ]
        ]
    },
    {
        "id": "1993812fb5c692ac",
        "type": "mqtt out",
        "z": "f9e0322178022cc3",
        "name": "",
        "topic": "first-node/blue_led/set",
        "qos": "",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "2d048788753b0ebb",
        "x": 840,
        "y": 240,
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