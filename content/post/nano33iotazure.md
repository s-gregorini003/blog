---
title: "Send Large JSON Data to IoT Hub Using MQTT"
subtitle: "How to Connect Arduino Nano 33 IoT and Azure IoT Hub"
date: 2020-08-24T09:31:28+01:00
draft: false 
---

This article is to demonstrate how to program an Arduino Nano 33 IoT board to communicate with Microsoft Azure. With a quick Google search, you would find out that there's already a library with all the functions required to share messages between Azure and Arduino boards. It is called  [AzureIoTHub](https://github.com/Azure/azure-iot-arduino) and is the IoT Hub's official Arduino library, published by Azure as a port of the [Microsoft Azure IoT device SDK for C](https://github.com/Azure/azure-iot-sdk-c)). The problem is that it doesn't actually support any Arduino board. Looking at the `README.md` file, the hardware currently supported is:

- ESP8266 based boards with [esp8266/arduino](https://github.com/esp8266/arduino)

  - SparkFun [Thing](https://www.sparkfun.com/products/13711)

  - Adafruit [Feather Huzzah](https://www.adafruit.com/products/2821)
  
- ESP32 based boards with [espressif/arduino-esp32](https://github.com/espressif/arduino-esp32)
  
  - Adafruit [HUZZAH32](https://www.adafruit.com/product/3405)
  
So, until the next compatibility update, the only way to send telemetry from a Nano 33 IoT to Azure IoT Hub is through third-party libraries. 

## Prerequisites

To code my workaround, I used the following libraries:

- [ArduinoJson](https://arduinojson.org) - To encapsulate my data in a JSON message, one of the formats accepted by IoT Hub;

- [WiFiNINA](https://github.com/arduino-libraries/WiFiNINA) - To connect the board to the WiFi;

- [ArduinoMqttClient](https://github.com/arduino-libraries/ArduinoMqttClient) - Client that allows to send and receive MQTT messages;

- [ArduinoBearSSL](https://github.com/arduino-libraries/ArduinoBearSSL) - Port of [BearSSL](https://bearssl.org/) to Arduino, to implement the SSL/TLS protocol;

- [ArduinoECCX08](https://github.com/arduino-libraries/ArduinoECCX08) - Library for the Atmel/Microchip ECC508 and ECC608 crypto chips, used for authentication with IoT Hub with a SelfSigned X.509 certificate;

All libraries must be installed on your system before compiling the code (instructions on how to install Arduino libraries can be found [here](https://www.arduino.cc/en/guide/libraries)).

## Implementation

Unfortunately, it's not that easy. Turns out that in the first version of my code, I could successfully send short messages over `Serial` and `MQTT`. But, when I tried to send a JSON text bigger than 256 Bytes (i.e. longer than 256 characters), it arrived at the Hub truncated.

{{< figure src="/img/nano33iotazure/monitor-events-NOT-WORKING.png" caption="Screenshot of the Azure Cloud Shell when receiving a truncated message." >}}

A similar issue is documented [here](https://github.com/firedog1024/mkr1000-iotc), using a different library for the MQTT client. In that case, the advised solution is to change the value `ENTER VALUE TO CHANGE` from `256` to `2048`, but that implies the following things:

- you'd have to edit the library source code, so your program is not reproducible on other systems;
- you still set a fixed value, which, even if bigger, it's not scalable.

{{< figure src="/img/nano33iotazure/monitor-events-NOT-WORKING.png" caption="Azure Cloud Shell when the hub is receiving the correct message." >}}

Instead, the solution explained here doesn't require any modification of the libraries' source code, since all the changes are made inside the sketch. Concretely, in the ``publishMessage()`` function, we declare the char array named ``payload``, which acts as a temporary buffer to store the JSON document. Then, using ``serializeJson()``, the document is serialized to the buffer and the function returns the number of bytes written, which is stored in ``payloadSize``.

```c++
char payload[1024]; // length of the char buffer that contains the JSON file, concretely the number of characters included in one message
size_t payloadSize = serializeJson(doc, payload);
```

 Now that we have the message and its size, we can send it via MQTT protocol to the IoT Hub. Through the overloaded function ``beginMessage()`` we pass the topic and the message size. Then, using the ``Print`` class implemented in ArduinoMqttClient, the JSON document is passed directly in the message body. Finally, the ``endMessage()`` function publishes the document to the specified topic.
  
```c++
// send message, the Print interface can be used to set the message contents
mqttClient.beginMessage("devices/" + deviceId + "/messages/events/", static_cast<unsigned long>(payloadSize));
mqttClient.print(payload);
mqttClient.endMessage();
```

The complete function, as found in the example code, is the following:

```c++
void publishMessage() {
  Serial.println("Publishing message");
  
  const int capacity = JSON_ARRAY_SIZE(10) + 10*JSON_OBJECT_SIZE(2)+ JSON_OBJECT_SIZE(3) + 280;     // Calculation of the JSON doc size, as explained in the documentation

/*  
Here is where you should write the body of your message. In this example, the JSON doc is purposely longer than 256 characters, to highlight the issue.
*/

  StaticJsonDocument<capacity> doc;
  doc["topic"] = "messageTopic";  
  doc["deviceId"] = deviceId;
  JsonArray data = doc.createNestedArray("data");
  JsonObject data_0 = data.createNestedObject();
  data_0["label"] = "Ankara";
  data_0["state"] = true;
  JsonObject data_1 = data.createNestedObject();
  data_1["label"] = "Beirut";
  data_1["state"] = true;
  JsonObject data_2 = data.createNestedObject();
  data_2["label"] = "Cincinnati";
  data_2["state"] = false;
  JsonObject data_3 = data.createNestedObject();
  data_3["label"] = "Detroit";
  data_3["state"] = false;
  JsonObject data_4 = data.createNestedObject();
  data_4["label"] = "Eindhoven";
  data_4["state"] = true;
  JsonObject data_5 = data.createNestedObject();
  data_5["label"] = "Fresno";
  data_5["state"] = false;
  JsonObject data_6 = data.createNestedObject();
  data_6["label"] = "Genoa";
  data_6["state"] = false;
  JsonObject data_7 = data.createNestedObject();
  data_7["label"] = "Huddersfield";
  data_7["state"] = true;
  JsonObject data_8 = data.createNestedObject();
  data_8["label"] = "Istanbul";
  data_8["state"] = true;
  JsonObject data_9 = data.createNestedObject();
  data_9["label"] = "Jakarta";
  data_9["state"] = false;
  

//   DEBUG - serialize the document in the serial monitor
//   serializeJson(doc, Serial);
//   Serial.println(" ");
  
  char payload[1024]; // length of the char buffer that contains the JSON file, concretely the number of characters included in one message
  size_t payloadSize = serializeJson(doc, payload);

//   DEBUG - write the size of the serialized document
//   Serial.print("json size:");
//   Serial.println(payloadSize);
  
// send message, the Print interface can be used to set the message contents
  mqttClient.beginMessage("devices/" + deviceId + "/messages/events/", static_cast<unsigned long>(payloadSize));
  mqttClient.print(payload);
  mqttClient.endMessage();
/*  
To replicate the issue, uncomment the following 3 lines and comment the 3 above. This way you'll only be able to send MQTT messages smaller than 256 Bytes.
*/
//  mqttClient.beginMessage("devices/" + deviceId + "/messages/events/");
//  serializeJson(doc, mqttClient);
//  mqttClient.endMessage();
}
```

The function above creates a JSON document as follows. Obviously, it can be changed as needed, this is an example to highlight the issue, since the text is longer than 256 characters.

```json
{
  "topic": "messageTopic",
  "deviceId": "deviceId",
  "data": [
    {
     "label": "Ankara",
     "state": true
    },
    {
     "label": "Beirut",
     "state": true
    },
    {
     "label": "Cincinnati",
     "state": false
    },
    {
     "label": "Detroit",
     "state": false
    },
    {
     "label": "Eindhoven",
     "state": true
    },
    {
     "label": "Fresno",
     "state": false
    },
    {
     "label": "Genoa",
     "state": false
    },
    {
     "label": "Huddersfield",
     "state": true
    },
    {
     "label": "Istanbul",
     "state": true
    },
    {
     "label": "Jakarta",
     "state": false
    }
  ]
}
```


---

I hope that this post was helpful. I uploaded an example of the code to send large JSON data from an Arduino Nano 33 IoT to Azure IoT Hub to a [GitHub repository](https://github.com/s-gregorini003/azure-iot-arduino-nano-33-iot.git). Follow the instructions in the ``README.md`` to configure it properly. Feel free to use it, modify it and extend it as you like. Thanks for reading!