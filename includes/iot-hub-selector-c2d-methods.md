> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-direct-methods.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-direct-methods.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-direct-methods.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-direct-methods.md)

Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációs eszközök millióira és a megoldás közötti háttér. Előző oktatóanyagok ([Ismerkedés az IoT-központ] és [IoT Hub-felhő eszközre üzenetek]) hello alapvető eszköz-felhő és a felhő eszközre üzenetkezelési funkcióit az IoT-központ bemutatására. Az IoT-központ is lehetővé teszi az hello képességét tooinvoke nem tartós módszerek hello felhőből eszközökön. Közvetlen módszerek határoz meg a kérelem-válasz interakció egy hasonló eszköz tooan HTTP hívható meg abban, hogy sikeres legyen, vagy közvetlenül (felhasználó által meghatározott időtúllépési) után sikertelen a toolet hello felhasználói tudja hello hívás hello állapotát. [Az eszközön közvetlen metódus] [ lnk-devguide-methods] részletesebben közvetlen módszereket írja le, és ha toouse közvetlen módszerek helyett a felhő-eszközre küldött üzenetek vagy a kívánt tulajdonságokkal útmutatást nyújt.

Ez az oktatóanyag a következőket mutatja be:

* Az Azure portál toocreate az IoT-központ hello használata, és az IoT hub hozzon létre egy eszközidentitás.
* Létrehoz egy szimulált eszköz alkalmazást, amely hello felhő meghívható közvetlen metódus.
* Hozzon létre egy konzolalkalmazást, amely közvetlen metódus meghívja az IoT hub keresztül hello szimulált eszköz alkalmazásban.

> [!NOTE]
> Most közvetlen módszereket csak a támogatott eszközök tooIoT központ használatával kapcsolódó levelezőprogramokkal hello MQTT protokoll. Tekintse meg a toohello [MQTT támogatási] [ lnk-devguide-mqtt] a cikk útmutatást tooconvert meglévő eszköz alkalmazás toouse MQTT.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[IoT Hub-felhő eszközre üzenetek]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[Ismerkedés az IoT-központ]: ../articles/iot-hub/iot-hub-node-node-getstarted.md