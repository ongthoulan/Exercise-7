[
    {
        "id": "e8ccb1256f906813",
        "type": "tab",
        "label": "Exercise 7",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "76c1f35faef6fda8",
        "type": "tab",
        "label": "Flow 3",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "1c6107c96f94fb48",
        "type": "mqtt-broker",
        "name": "",
        "broker": "host.docker.internal",
        "port": 1883,
        "clientid": "",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": "5",
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
    },
    {
        "id": "9b54e9a184f6977c",
        "type": "mongodb4-client",
        "name": "MongoDB Docker",
        "protocol": "mongodb",
        "hostname": "",
        "port": "",
        "dbName": "smart_campus",
        "appName": "",
        "authSource": "",
        "authMechanism": "DEFAULT",
        "tls": false,
        "tlsCAFile": "",
        "tlsCertificateKeyFile": "",
        "tlsInsecure": false,
        "connectTimeoutMS": "30000",
        "socketTimeoutMS": "0",
        "minPoolSize": "0",
        "maxPoolSize": "100",
        "maxIdleTimeMS": "0",
        "uri": "mongodb://host.docker.internal:27017/smart_campus?directConnection=true",
        "advanced": "{}",
        "uriTabActive": "tab-uri-advanced"
    },
    {
        "id": "189ba0897b95beb2",
        "type": "mqtt-broker",
        "name": "host.docker.internal ",
        "broker": "host.docker.internal",
        "port": 1883,
        "clientid": "",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": "5",
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
    },
    {
        "id": "7fad470e198766e8",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{\"device_id\":\"GATEWAY_01\",\"battery\":85,\"status\":\"active\",\"sensors_attached\":[\"temperature\",\"humidity\",\"soil_moisture\"],\"active_warnings\":[{\"code\":102,\"severity\":\"low\"},{\"code\":505,\"severity\":\"critical\"}],\"transmission_count\":100}",
        "payloadType": "json",
        "x": 150,
        "y": 180,
        "wires": [
            [
                "4a6ee68eb7db9740"
            ]
        ]
    },
    {
        "id": "4a6ee68eb7db9740",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "insertOne",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 420,
        "y": 180,
        "wires": [
            []
        ]
    },
    {
        "id": "9c8f995bb3e79f81",
        "type": "function",
        "z": "e8ccb1256f906813",
        "name": "$eq, $ne, $gt, $gte, $lt, $lte",
        "func": "msg.payload = {\n    \"battery\": { \"$gte\": 20, \"$lt\": 90 },\n    \"status\": { \"$ne\": \"maintenance\" }\n};\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 320,
        "y": 260,
        "wires": [
            [
                "31bfd1991cc6797a"
            ]
        ]
    },
    {
        "id": "19b7df27155081db",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{  \"device_id\": \"GATEWAY_01\",  \"battery\": 85,  \"status\": \"active\",  \"sensors_attached\": [\"temperature\", \"humidity\", \"soil_moisture\"],  \"active_warnings\": [{\"code\": 102, \"severity\": \"low\"}, {\"code\": 505, \"severity\": \"critical\"}],  \"transmission_count\": 100 }",
        "payloadType": "json",
        "x": 90,
        "y": 260,
        "wires": [
            [
                "9c8f995bb3e79f81"
            ]
        ]
    },
    {
        "id": "31bfd1991cc6797a",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "find",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 530,
        "y": 260,
        "wires": [
            [
                "e9a953d60db9c361"
            ]
        ]
    },
    {
        "id": "e9a953d60db9c361",
        "type": "debug",
        "z": "e8ccb1256f906813",
        "name": "debug 1",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 700,
        "y": 260,
        "wires": []
    },
    {
        "id": "7afd7c06ac8d5cb9",
        "type": "function",
        "z": "e8ccb1256f906813",
        "name": "$or & $and",
        "func": "msg.payload = {\n    \"$or\": [\n        { \"battery\": { \"$lte\": 15 } },\n        { \"sensors_attached\": { \"$in\": [\"soil_moisture\", \"co2\"] } }\n    ],\n    \"$and\": [\n        { \"status\": \"active\" }\n    ]\n};\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 270,
        "y": 360,
        "wires": [
            [
                "6ba8184ead28861a"
            ]
        ]
    },
    {
        "id": "2c37b192de5b6ae1",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{  \"device_id\": \"GATEWAY_01\",  \"battery\": 85,  \"status\": \"active\",  \"sensors_attached\": [\"temperature\", \"humidity\", \"soil_moisture\"],  \"active_warnings\": [{\"code\": 102, \"severity\": \"low\"}, {\"code\": 505, \"severity\": \"critical\"}],  \"transmission_count\": 100 }",
        "payloadType": "json",
        "x": 110,
        "y": 360,
        "wires": [
            [
                "7afd7c06ac8d5cb9"
            ]
        ]
    },
    {
        "id": "6ba8184ead28861a",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "find",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 430,
        "y": 360,
        "wires": [
            [
                "89f1b55c3c048398"
            ]
        ]
    },
    {
        "id": "89f1b55c3c048398",
        "type": "debug",
        "z": "e8ccb1256f906813",
        "name": "debug 2",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 600,
        "y": 360,
        "wires": []
    },
    {
        "id": "2afe435415c0e443",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "updateOne",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 450,
        "y": 460,
        "wires": [
            [
                "0c8c233bdaec2a2e"
            ]
        ]
    },
    {
        "id": "d122df60e437bf2e",
        "type": "function",
        "z": "e8ccb1256f906813",
        "name": "$set & $inc",
        "func": "var filter = { \"device_id\": \"GATEWAY_01\" };\nvar update = {\n    \"$set\": { \"battery\": 82 },\n    \"$inc\": { \"transmission_count\": 1 }\n};\nmsg.payload = [filter, update];\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 290,
        "y": 460,
        "wires": [
            [
                "2afe435415c0e443"
            ]
        ]
    },
    {
        "id": "73cb8b7991ce0db5",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{  \"device_id\": \"GATEWAY_01\",  \"battery\": 85,  \"status\": \"active\",  \"sensors_attached\": [\"temperature\", \"humidity\", \"soil_moisture\"],  \"active_warnings\": [{\"code\": 102, \"severity\": \"low\"}, {\"code\": 505, \"severity\": \"critical\"}],  \"transmission_count\": 100 }",
        "payloadType": "json",
        "x": 110,
        "y": 460,
        "wires": [
            [
                "d122df60e437bf2e"
            ]
        ]
    },
    {
        "id": "0c8c233bdaec2a2e",
        "type": "debug",
        "z": "e8ccb1256f906813",
        "name": "debug 3",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 620,
        "y": 460,
        "wires": []
    },
    {
        "id": "d025575c99751e23",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "updateOne",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 470,
        "y": 560,
        "wires": [
            [
                "5f61fcaecd4ceb89"
            ]
        ]
    },
    {
        "id": "1d79edd9a2f47152",
        "type": "function",
        "z": "e8ccb1256f906813",
        "name": "$push",
        "func": "var filter = { \"device_id\": \"GATEWAY_01\" };\nvar update = {\n\"$push\": { \"sensors_attached\": \"light_level\" },\n\n// -1 removes the first item, 1 removes the last item\n};\nmsg.payload = [filter, update];\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 270,
        "y": 560,
        "wires": [
            [
                "d025575c99751e23"
            ]
        ]
    },
    {
        "id": "95a83bb87ed5c77b",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{  \"device_id\": \"GATEWAY_01\",  \"battery\": 85,  \"status\": \"active\",  \"sensors_attached\": [\"temperature\", \"humidity\", \"soil_moisture\"],  \"active_warnings\": [{\"code\": 102, \"severity\": \"low\"}, {\"code\": 505, \"severity\": \"critical\"}],  \"transmission_count\": 100 }",
        "payloadType": "json",
        "x": 110,
        "y": 560,
        "wires": [
            [
                "1d79edd9a2f47152"
            ]
        ]
    },
    {
        "id": "5f61fcaecd4ceb89",
        "type": "debug",
        "z": "e8ccb1256f906813",
        "name": "debug 4",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 660,
        "y": 560,
        "wires": []
    },
    {
        "id": "0acbfe78e9d84d46",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "updateOne",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 490,
        "y": 640,
        "wires": [
            [
                "f78ab74c4add8ac4"
            ]
        ]
    },
    {
        "id": "b720d736629e8acd",
        "type": "function",
        "z": "e8ccb1256f906813",
        "name": "$pop",
        "func": "var filter = { \"device_id\": \"GATEWAY_01\" };\nvar update = {\n\"$pop\": { \"sensors_attached\": 1 }\n\n// -1 removes the first item, 1 removes the last item\n};\nmsg.payload = [filter, update];\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 270,
        "y": 640,
        "wires": [
            [
                "0acbfe78e9d84d46"
            ]
        ]
    },
    {
        "id": "febaca9c3c919ca5",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{  \"device_id\": \"GATEWAY_01\",  \"battery\": 85,  \"status\": \"active\",  \"sensors_attached\": [\"temperature\", \"humidity\", \"soil_moisture\"],  \"active_warnings\": [{\"code\": 102, \"severity\": \"low\"}, {\"code\": 505, \"severity\": \"critical\"}],  \"transmission_count\": 100 }",
        "payloadType": "json",
        "x": 110,
        "y": 640,
        "wires": [
            [
                "b720d736629e8acd"
            ]
        ]
    },
    {
        "id": "f78ab74c4add8ac4",
        "type": "debug",
        "z": "e8ccb1256f906813",
        "name": "debug 5",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 700,
        "y": 640,
        "wires": []
    },
    {
        "id": "09756707811b5248",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "updateOne",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 490,
        "y": 720,
        "wires": [
            [
                "e35d13969427d7c3"
            ]
        ]
    },
    {
        "id": "d4c640afb13268a0",
        "type": "function",
        "z": "e8ccb1256f906813",
        "name": "$pull & $addToSet",
        "func": "var filter = { \"device_id\": \"GATEWAY_01\" };\nvar update = {\n    \"$addToSet\": { \"sensors_attached\": \"light_level\" },\n    // Will not duplicate if it already exists\n    \"$pull\": { \"active_warnings\": { \"code\": 102 } }\n    // Removes the specific object from the array\n};\nmsg.payload = [filter, update];\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 290,
        "y": 720,
        "wires": [
            [
                "09756707811b5248"
            ]
        ]
    },
    {
        "id": "bbadeec18ba4ffbf",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{  \"device_id\": \"GATEWAY_01\",  \"battery\": 85,  \"status\": \"active\",  \"sensors_attached\": [\"temperature\", \"humidity\", \"soil_moisture\"],  \"active_warnings\": [{\"code\": 102, \"severity\": \"low\"}, {\"code\": 505, \"severity\": \"critical\"}],  \"transmission_count\": 100 }",
        "payloadType": "json",
        "x": 110,
        "y": 720,
        "wires": [
            [
                "d4c640afb13268a0"
            ]
        ]
    },
    {
        "id": "e35d13969427d7c3",
        "type": "debug",
        "z": "e8ccb1256f906813",
        "name": "debug 6",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 720,
        "y": 720,
        "wires": []
    },
    {
        "id": "283a3ff14cca8e2e",
        "type": "mongodb4",
        "z": "e8ccb1256f906813",
        "clientNode": "9b54e9a184f6977c",
        "mode": "collection",
        "collection": "sensor_data",
        "operation": "find",
        "output": "toArray",
        "maxTimeMS": "0",
        "handleDocId": false,
        "name": "",
        "x": 470,
        "y": 800,
        "wires": [
            [
                "118e42dc29912050"
            ]
        ]
    },
    {
        "id": "0498a871c5a140da",
        "type": "function",
        "z": "e8ccb1256f906813",
        "name": "$elemMatch",
        "func": "msg.payload = {\n    \"active_warnings\": {\n        \"$elemMatch\": {\n            \"code\": { \"$gt\": 500 },\n            \"severity\": \"critical\"\n        }\n    }\n};\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 270,
        "y": 800,
        "wires": [
            [
                "283a3ff14cca8e2e"
            ]
        ]
    },
    {
        "id": "91943444ad2adf13",
        "type": "inject",
        "z": "e8ccb1256f906813",
        "name": "step 1",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": "0.5",
        "topic": "",
        "payload": "{  \"device_id\": \"GATEWAY_01\",  \"battery\": 85,  \"status\": \"active\",  \"sensors_attached\": [\"temperature\", \"humidity\", \"soil_moisture\"],  \"active_warnings\": [{\"code\": 102, \"severity\": \"low\"}, {\"code\": 505, \"severity\": \"critical\"}],  \"transmission_count\": 100 }",
        "payloadType": "json",
        "x": 90,
        "y": 800,
        "wires": [
            [
                "0498a871c5a140da"
            ]
        ]
    },
    {
        "id": "118e42dc29912050",
        "type": "debug",
        "z": "e8ccb1256f906813",
        "name": "debug 7",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "true",
        "targetType": "full",
        "statusVal": "",
        "statusType": "auto",
        "x": 680,
        "y": 800,
        "wires": []
    },
    {
        "id": "41393bd2685007f7",
        "type": "global-config",
        "env": [],
        "modules": {
            "node-red-contrib-mongodb4": "3.4.0"
        }
    }
]
