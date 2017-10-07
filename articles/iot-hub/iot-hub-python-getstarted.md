---
title: "aaaGet Azure IoT Hub (Python) használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toosend eszköz-felhő üzenetek tooAzure IoT Hub Python IoT SDK-k használatával. Szimulált eszköz és a szolgáltatás alkalmazások tooregister létrehozása az eszköz, üzenetküldés és üzenetek olvasni az IoT-központ."
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: dkshir
ms.custom: na
ms.openlocfilehash: aa23e792fb144202e121274723bcfaeae0c04723
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-python"></a>Csatlakoztassa a szimulált eszköz tooyour IoT hubot pythonos környezetekben
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Ez az oktatóanyag végén hello hogy két Python-alkalmazások:

* **CreateDeviceIdentity.py**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect a szimulált eszköz alkalmazás.
* **SimulatedDevice.py**, IoT-központ tooyour köti össze a korábban létrehozott hello eszközidentitás, és rendszeres időközönként telemetriai adatokat küld üzenet hello MQTT protokoll használatával.

> [!NOTE]
> hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.
> 
> 

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* [Python 2.x vagy 3.x][lnk-python-download]. Győződjön meg arról, hogy toouse hello 32 bites vagy 64 bites telepítési a telepítő szükség szerint. Amikor a rendszer kéri, hello telepítése során, győződjön meg arról, hogy tooadd Python tooyour platform-specifikus környezeti változó. Python használata 2.x, szükség lehet a túl[telepítené vagy frissítené a *pip*, hello Python-csomag felügyeleti rendszer][lnk-install-pip].
* Ha Windows operációs rendszer, majd használja [Visual C++ terjeszthető változatát tartalmazó csomagot] [ lnk-visual-c-redist] tooallow hello használata natív Python DLL-eket.
* [Node.js 4.0 vagy újabb][lnk-node-download]. Győződjön meg arról, hogy toouse hello 32 bites vagy 64 bites telepítési a telepítő szükség szerint. Ez a szükséges tooinstall hello [IoT Hub Explorer eszköz][lnk-iot-hub-explorer].
* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].

> [!NOTE]
> Hello *pip* a csomagok `azure-iothub-service-client` és `azure-iothub-device-client` jelenleg csak a Windows operációs rendszer elérhető. A Linux vagy Mac OS, tekintse meg az toohello Linux és Mac OS-specifikus szakaszokat hello [a fejlesztési környezet előkészítése a Python] [ lnk-python-devbox] utáni.
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Ezzel létrehozta az IoT Hubot. Hello rest oktatóanyag hello IoT-központ állomásnév és az IoT-központ kapcsolati karakterlánc hello használja.

> [!NOTE]
> Könnyen is létrehozhat az IoT hub a parancssorban, hello Python segítségével, vagy a Node.js-alapú Azure parancssori felület. hello cikk [létrehoz egy IoT-központot hello Azure CLI 2.0 használatával] [ lnk-azure-cli-hub] Gyorsműveletek toodo így hello jeleníti meg. 
> 

## <a name="create-a-device-identity"></a>Eszközidentitás létrehozása
Ez a szakasz hello lépéseket toocreate egy Python-Konzolalkalmazás, az IoT hub hello identitásjegyzékhez egy eszközidentitás létrehozó. Egy eszköz csak kapcsolódhatnak tooIoT Hub-e azt egy bejegyzést a hello identitásjegyzékhez. További információkért lásd: hello **Identitásjegyzékhez** hello szakasza [IoT Hub fejlesztői útmutató][lnk-devguide-identity]. A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub.

1. Nyisson meg egy parancssort, és telepítse a hello **Azure IoT Hub szolgáltatás SDK for Python**. Zárja be a parancssort hello hello SDK telepítése után.

    ```
    pip install azure-iothub-service-client
    ```

2. Hozzon létre egy Python-fájlt **CreateDeviceIdentity.py** néven. Nyissa meg a [egy Python szerkesztő/IDE az Ön által választott][lnk-python-ide-list], például az alapértelmezett hello [ÜRESJÁRATBAN][lnk-idle].

3. Adja hozzá a hello tooimport szükséges hello kódmodulok hello SDK szolgáltatástól a következő:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. Adja hozzá a következő kódot, cseréje hello helyőrzője hello `[IoTHub Connection String]` hello IoT hub hello előző szakaszban létrehozott hello kapcsolati karakterlánccal. Egyetlen nevét használják hello `DEVICE_ID`.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. Adja hozzá hello függvény tooprint következő hello eszköz adatok egy részét.

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. Adja hozzá a következő függvény toocreate hello eszköz azonosítása hello beállításjegyzék-kezelő használatával hello. 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. Végül adja hozzá az alábbiak szerint hello fő funkciója, és mentse hello fájlt.

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using hello Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. A hello parancssorból futtassa a hello **CreateDeviceIdentity.py** az alábbiak szerint:

    ```python
    python CreateDeviceIdentity.py
    ```
6. Hello szimulált eszköz létrehozva első kell megjelennie. Jegyezze fel a hello **deviceId** és hello **primaryKey** eszköz. Ezek az értékek később szüksége tooIoT eszközként központ kapcsolódó alkalmazásban létrehozásakor.

    ![Eszköz létrehozása sikeres][1]

> [!NOTE]
> az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja. Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, és egy engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol. Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon. További információkért lásd: hello [IoT Hub fejlesztői útmutató][lnk-devguide-identity].
> 
> 


## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ez a szakasz hello lépéseket toocreate egy Python-Konzolalkalmazás, szimulálja egy eszközt, és elküldi az eszköz a felhőbe küldött üzeneteket tooyour IoT-központ.

1. Nyisson meg egy új parancssort, és telepítse az Azure IoT Hub eszköz SDK hello a Python az alábbiak szerint. Zárja be a hello parancssort hello telepítése után.

    ```
    pip install azure-iothub-device-client
    ```
2. Hozzon létre egy **SimulatedDevice.py** nevű fájlt. Nyissa meg a fájlt egy szabadon választott Python-szerkesztőben/IDE-ben (például az IDLE környezetben).

3. Adja hozzá a következő tooimport szükséges hello kódmodulok SDK hello eszközről hello.

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. Adja hozzá a hello alábbi kód, és cserélje le a helyőrző hello `[IoTHub Device Connection String]` az eszközhöz hello kapcsolati karakterlánccal. hello eszköz kapcsolati karakterlánca általában hello formátumban `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`. Használjon hello **deviceId** és **primaryKey** hello előző szakasz tooreplace hello létrehozott hello eszköz `<deviceId>` és `<primaryKey>` kulcsattribútumokkal. A `<hostName>` nevet cserélje le az IoT Hub gazdagépnevére. Ez általában `<IoT hub name>.azure-devices.net`.

    ```python
    # String containing Hostname, Device Id & Device Key in hello format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. Adja hozzá a következő kód toodefine egy küldési megerősítő visszahívás hello. 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. Adja hozzá a következő kód tooinitialize hello eszközügyfél hello.

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set hello time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. Adja hozzá a következő hello tooformat funkciót, és egy üzenetet küldeni a szimulált eszköz tooyour IoT-központ.

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C tooexit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission tooIoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. Végül adja hozzá a hello fő funkciója. 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using hello Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. Mentse és zárja be a hello **SimulatedDevice.py** fájlt. Meg vannak most már készen áll a toorun ezt az alkalmazást.

> [!NOTE]
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a>Üzenet fogadása a szimulált eszközről
tooreceive telemetriai üzeneteket az eszközről, szükség toouse egy [Event Hubs][lnk-event-hubs-overview]-kompatibilis végpont által hello IoT-központot, amelyben a köszönőüzenetei eszközről a felhőbe. Olvasási hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] hogyan tooprocess érkező üzenetek Event Hubs az IoT hub Event Hub-kompatibilis végpont kapcsolatos útmutató. Az Event Hubs nem támogatja az telemetriai a Python, így hozhat létre egy [Node.js](iot-hub-node-node-getstarted.md#D2C_node) vagy egy [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) az IoT-központ konzol Event Hubs-alapú eszköz-felhő app tooread köszönőüzenetei. Ez az oktatóanyag bemutatja, hogyan használhatja a hello [IoT Hub Explorer eszköz] [ lnk-iot-hub-explorer] tooread ezek eszközre küldött üzenetek.

1. Nyisson meg egy parancssort, és az IoT Hub Explorer hello telepítse. 

    ```
    npm install -g iothub-explorer
    ```

2. Futtassa a parancsot a hello parancssor a következő hello, toobegin figyelési hello eszközről a felhőbe üzeneteket az eszközről. Az IoT hub kapcsolódási karakterlánc használata után hello helyőrző `--login`.

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. Nyisson meg egy új parancssort, és keresse meg a hello tartalmazó toohello könyvtár **SimulatedDevice.py** fájlt.

4. Futtassa a hello **SimulatedDevice.py** fájlt, amely rendszeresen küld a telemetriai adatok tooyour IoT-központot. 
   
    ```
    python SimulatedDevice.py
    ```
5. Az IoT Hub Explorer hello fut az előző szakasz hello hello parancssori eszköz köszönőüzenetei láthatja. 

    ![Eszközről felhőbe irányuló Python-üzenetek][2]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás. Az eszköz identitás tooenable hello szimulált eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta. Ön megfigyelt köszönőüzenetei hello IoT Hub Explorer eszköz hello segítségével hello IoT-központ fogadja. 

Python SDK tooexplore hello Azure IoT Hub használati részletesen, látogasson el [a központ Git-tárház][lnk-python-github]. tooreview hello üzenetkezelési képességeket, hello Azure IoT Hub szolgáltatás SDK for Python, letöltése és futtatása [iothub_messaging_sample.py][lnk-messaging-sample]. Eszköz ügyféloldali szimuláció hello Azure IoT Hub eszköz SDK-t használ a Python, töltse le és futtassa a hello [iothub_client_sample.py][lnk-client-sample].

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

* [Kapcsolódás az eszközhöz][lnk-connect-device]
* [Eszközfelügyelet – első lépések][lnk-device-management]
* [Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]

toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
