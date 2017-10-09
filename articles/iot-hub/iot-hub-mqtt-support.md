---
title: "Azure IoT Hub MQTT támogatási aaaUnderstand |} Microsoft Docs"
description: "Fejlesztői útmutató - tooan IoT Hub eszköz felé néző végpont használatával csatlakozó eszközök hello MQTT protokoll támogatása. Hello Azure IoT eszközoldali SDK-k beépített MQTT támogatásával kapcsolatos információkat tartalmazza."
services: iot-hub
documentationcenter: .net
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e461687963138987acdf1f4e0e34c453744ea191
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="communicate-with-your-iot-hub-using-hello-mqtt-protocol"></a>Az IoT hub hello MQTT protokoll használatával folytatott kommunikációhoz

Az IoT-központ lehetővé teszi, hogy a eszközök toocommunicate hello IoT Hub eszköz végpontokon hello segítségével [MQTT v3.1.1] [ lnk-mqtt-org] protokoll-as port 8883 vagy MQTT v3.1.1 WebSocket protokoll 443-as porton keresztül. Az IoT-központ szükséges összes eszköz kommunikációs toobe a TLS/SSL használatával biztonságossá (emiatt az IoT-központ nem támogatja a nem biztonságos kapcsolatok 1883 porton keresztül).

## <a name="connecting-tooiot-hub"></a>TooIoT központi csatlakozás

Eszközök bármelyikével hello MQTT protokoll tooconnect tooan IoT-központ hello hello tárak segítségével [Azure IoT SDK-k] [ lnk-device-sdks] vagy közvetlenül hello MQTT protokoll használatával.

## <a name="using-hello-device-sdks"></a>Hello eszköz SDK-k használata

[Eszközoldali SDK-k] [ lnk-device-sdks] érhetők el, hogy támogatási hello MQTT protokoll Java, Node.js, C, C# és Python. hello eszköz SDK-k használata hello szabványos IoT-központ kapcsolati karakterlánc tooestablish kapcsolat tooan IoT-központot. toouse hello MQTT protokoll hello ügyfél protokoll paramétert kell beállítani túl**MQTT**. Alapértelmezés szerint hello eszköz SDK-k kapcsolódni az IoT-központ tooan hello **CleanSession** jelző beállítva túl**0** és **QoS 1** hello IoT hubbal üzenet exchange-hez.

Ha egy eszköz csatlakoztatott tooan IoT-központot, hello eszköz SDK-k biztosítanak, amelyek lehetővé teszik az eszköz toosend köszönőüzenetei tooand az IoT-központ érkező üzenetek fogadására módszerek.

hello következő tábla tartalmaz hivatkozásokat toocode minták egyes nyelv támogatott, és adja meg a hello paraméter toouse tooestablish egy kapcsolat tooIoT Hub hello MQTT protokoll használatával.

| Nyelv | Protokoll paraméter |
| --- | --- |
| [NODE.js][lnk-sample-node] |Azure-iot-eszközök – mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-toomqtt"></a>Egy eszköz alkalmazásának AMQP tooMQTT áttelepítése

Hello használata [eszköz SDK-k][lnk-device-sdks], AMQP használatával való váltás tooMQTT módosítani kell hello protokoll paraméter hello ügyfél inicializálása ahogy korábban is hangsúlyoztuk.

Annak során, győződjön meg arról, hogy toocheck hello a következő elemek:

* AMQP hibáit a sok feltételek MQTT hello kapcsolat leállítása közben. A kivételkezelő logikát is eredményeképpen bizonyos változásokat igényelhet.
* MQTT nem támogatja a hello *elutasítása* műveletek fogadásakor [felhő-eszközre küldött üzenetek][lnk-messaging]. Ha a háttér-tooreceive hello eszközalkalmazás válaszára kell, érdemes lehet [módszerek közvetlen][lnk-methods].

## <a name="using-hello-mqtt-protocol-directly"></a>Hello MQTT protokoll segítségével közvetlenül
Ha egy eszköz nem használható hello eszköz SDK-k, továbbra is kapcsolódhatnak toohello nyilvános eszköz végpontok hello MQTT protokollal a port 8883. A hello **CONNECT** csomag hello eszközt kell használnia a következő értékek hello:

* A hello **ClientId** mezőben használja a hello **deviceId**.
* A hello **felhasználónév** mezőben `{iothubhostname}/{device_id}/api-version=2016-11-14`, ahol {iothubhostname} van hello hello IoT hub teljes CName.

    Például hello neve az IoT hub akkor **contoso.azure-devices.net** , és ha az eszköz nevére hello **MyDevice01**, teljes hello **felhasználónév** mezőt kell tartalmaznia. `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.
* A hello **jelszó** mezőben egy SAS-jogkivonatot használja. SAS-jogkivonat neve hello hello formátuma megegyezik hello hello HTTP és a AMQP protokollok esetében:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    További információ a SAS-tokenje toogenerate, a hello eszköz című szakaszában talál [IoT-központ használatával biztonsági jogkivonatokat][lnk-sas-tokens].

    Vizsgálatakor is használhatja hello [eszköz explorer] [ lnk-device-explorer] eszköz tooquickly a SAS-token másolja és illessze be a saját kód létrehozásához:

  1. Nyissa meg toohello **felügyeleti** lapján **eszköz Explorer**.
  2. Kattintson a **SAS-Token** (jobb felső).
  3. A **SASTokenForm**, jelölje ki az eszköz hello **DeviceID** legördülő listán. Állítsa be a **TTL**.
  4. Kattintson a **Generate** toocreate a jogkivonatot.

     hello generált SAS-jogkivonat van ez a struktúra: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

     a token toouse, hello részét hello **jelszó** mező tooconnect MQTT használatával van: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

MQTT csatlakozni, és válassza le a csomagok, IoT-központ kibocsát egy eseménnyel hello **Operations figyelés** csatorna további információkkal, melyek segíthetnek tootroubleshoot kapcsolódási problémák.

### <a name="sending-device-to-cloud-messages"></a>Eszköz-felhő üzenetek küldése

Sikeres csatlakozás után a egy eszköz küldhet üzeneteket tooIoT központ használatával `devices/{device_id}/messages/events/` vagy `devices/{device_id}/messages/events/{property_bag}` , egy **témakör**. Hello `{property_bag}` elem lehetővé teszi, hogy az eszköz toosend köszönőüzenetei további tulajdonságokkal url-kódolású formátumban. Példa:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Ez `{property_bag}` elem által használt hello ugyanazon kódolás hello HTTP protokoll a lekérdezési karakterláncok esetében.
>
>

hello eszköz alkalmazást is használhatja `devices/{device_id}/messages/events/{property_bag}` hello, **lesz a témakör neve** toodefine *üzenetek fog* toobe telemetriai üzenetként továbbítja.

- Az IoT-központ nem támogatja a QoS 2 üzeneteket. Ha egy eszköz alkalmazás tesz közzé egy üzenetet, amelyben **QoS 2**, IoT-központ hello hálózati kapcsolat bezárása után.
- Az IoT-központ nem maradnak megőrzése üzeneteket. Ha egy eszköz küldi hello üzenet **megőrzése** jelző too1 beállítva, az IoT-központ hozzáadja hello **x-opt-megőrzése** alkalmazás tulajdonság toohello üzenet. Ebben az esetben helyett tárolásakor hello tartsa meg az üzenetet, IoT-központ toohello háttér app továbbítja azokat.
- Az IoT-központ csak eszközönként egy aktív MQTT kapcsolatot támogat. Egy új MQTT kapcsolathoz nevében hello ugyanazon Eszközazonosítót hatására az IoT-központ toodrop hello meglévő kapcsolat.

További információkért lásd: [fejlesztői útmutató üzenetküldési][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Felhő-eszközre küldött üzenetek fogadása

az IoT-központ tooreceive üzenetek, egy eszközt kell előfizetés `devices/{device_id}/messages/devicebound/#` , egy **témakör szűrő**. több szintű helyettesítő hello  **#**  hello a témakör szűrővel csak tooallow hello tooreceive további eszköztulajdonságok hello témakör neve. Az IoT-központ nem teszi lehetővé a hello hello használata  **#**  vagy **?** helyettesítő karakterek alárendelt témakörök szűréséhez. Mivel az IoT-központ nem egy általános célú pub-sub üzenetkezelési broker, csak a támogatott hello részletes ismertetését lásd a témakör nevét és a témakör szűrők.

hello eszköz nem kap minden üzenet IoT-központot, amíg nem tooits eszközspecifikus végpont hello által képviselt sikeresen kér le `devices/{device_id}/messages/devicebound/#` témakör szűrő. Sikeres előfizetés létrejötte után hello eszköz elindítja a tooit elküldött hello előfizetés hello idő után csak felhőalapú eszközre üzenetek fogadására. Ha hello eszköz csatlakozik az **CleanSession** jelző beállítva túl**0**, különböző munkamenetek között hello előfizetés megőrződjenek. A következő csatlakozásakor ebben az esetben hello **CleanSession 0** hello eszköz kapja, függőben lévő üzenetek elküldött tooit közben le lett választva. Ha hello eszköz által használt **CleanSession** jelző beállítva túl**1** azonban azt nem bármely üzenetek fogadása az IoT-központ mindaddig, amíg az eszköz-végpont tooits feliratkozva.

Az IoT-központ lekéri az üzeneteket a hello **témakör** `devices/{device_id}/messages/devicebound/`, vagy `devices/{device_id}/messages/devicebound/{property_bag}` bármely üzenettulajdonságok esetén. `{property_bag}`url-kódolású kulcs/érték párok az üzenet tulajdonságait tartalmazza. Csak alkalmazáshoz és a felhasználó állítható be rendszer tulajdonságai (például **messageId** vagy **correlationId**) hello tulajdonságcsomag szerepelnek. Rendszer tulajdonságnevek rendelkezik hello előtag  **$** , alkalmazástulajdonságok hello eredeti tulajdonságnév használata nincs előtagja.

Ha egy eszköz alkalmazásának előfizet tooa témakör **QoS 2**, IoT-központ biztosít maximális QoS szintjének 1 hello **SUBACK** csomagot. Ezt követően az IoT-központ üzenetek toohello eszközt QoS 1 nyújt.

### <a name="retrieving-a-device-twins-properties"></a>Egy eszköz iker tulajdonságainak beolvasása

Először egy eszköz túl előfizet`$iothub/twin/res/#`, tooreceive hello művelet válaszokat. Ezután elküldi egy üres üzenet tootopic `$iothub/twin/GET/?$rid={request id}`, megadott értékkel **kérelemazonosító**. hello szolgáltatás majd egy iker eszközadatok hello témakör tartalmazó válaszüzenetet küld vissza `$iothub/twin/res/{status}/?$rid={request id}`, használatával hello azonos  **Kérelemazonosító** hello kérés.

Kérelemazonosító tetszőleges érvényes érték, egy üzenet tulajdonságérték lehet megfelelően [IoT Hub fejlesztői útmutató üzenetküldési][lnk-messaging], és állapota egy egész számként van hitelesítve.
hello adott válasz törzsének hello eszköz iker hello tulajdonságok szakasza tartalmazza:

hello törzs hello identitás bejegyzés korlátozott toohello "Tulajdonságok" tag, például:

        {
            "properties": {
                "desired": {
                    "telemetrySendFrequency": "5m",
                    "$version": 12
                },
                "reported": {
                    "telemetrySendFrequency": "5m",
                    "batteryLevel": 55,
                    "$version": 123
                }
            }
        }

hello lehetséges állapota kódok a következők:

|status | Leírás |
| ----- | ----------- |
| 200 | Sikeres |
| 429 | Túl sok kérelmek (halmozódni), mint / [IoT Hub-szabályozás][lnk-quotas] |
| 5** | Kiszolgáló hibák |

További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Eszköz iker jelentett tulajdonságainak frissítése

hello következő feladatütemezési ismerteti, hogy egy eszköz frissíti hello jelentett hello eszköz iker az IoT Hub-tulajdonságokat:

1. Egy eszköz először elő kell fizetnie toohello `$iothub/twin/res/#` IoT-központ témakör tooreceive hello művelet válaszát.

1. Egy eszköz, amely tartalmazza a hello eszköz iker frissítés toohello üzenet küldése `$iothub/twin/PATCH/properties/reported/?$rid={request id}` témakör. Ez az üzenet tartalmaz egy **kérelemazonosító** érték.

1. hello szolgáltatást, majd elküldi a válaszüzenetet, amely tartalmazza az új ETag érték hello hello jelentett tulajdonsággyűjteményében témakör `$iothub/twin/res/{status}/?$rid={request id}`. A válaszüzenet által használt azonos hello **kérelemazonosító** hello kérés.

hello kérelem üzenettörzs egy JSON-dokumentumában, amely új értékek biztosít a jelentett tulajdonságokat (más tulajdonság vagy metaadatai nem módosíthatók) tartalmazza.
Minden tagjának hello JSON-dokumentum frissíti, vagy adja hozzá a megfelelő tag hello hello eszköz iker dokumentumban. Egy tag beállítása túl`null`, törléseket hello objektumot tartalmazó hello tagja. Példa:

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

hello lehetséges állapota kódok a következők:

|status | Leírás |
| ----- | ----------- |
| 200 | Sikeres |
| 400 | Hibás kérés. Nem megfelelően formázott JSON-ban |
| 429 | Túl sok kérelmek (halmozódni), mint / [IoT Hub-szabályozás][lnk-quotas] |
| 5** | Kiszolgáló hibák |

További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>A fogadó kívánt tulajdonságokkal vonatkozó frissítési értesítések

Ha egy eszköz csatlakozik, IoT-központ küld-e az értesítések toohello témakör `$iothub/twin/PATCH/properties/desired/?$version={new version}`, tartalmazó hello hello megoldás háttérrendszerének által végzett hello frissítés tartalmát. Példa:

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

Tulajdonság frissítései, mint `null` érték azt jelenti, hogy a hello JSON objektum tag törlése folyamatban van.


> [!IMPORTANT] 
> Az IoT-központ állít elő változási értesítéseket csak akkor, ha a csatlakoztatott eszközök is. Győződjön meg arról, hogy tooimplement hello [eszköz újracsatlakozás folyamat] [ lnk-devguide-twin-reconnection] tookeep hello szükséges tulajdonságok szinkronizálja az IoT Hub és hello eszköz alkalmazás között.

További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].

### <a name="respond-tooa-direct-method"></a>Válaszoljon tooa közvetlen módszer

Először egy eszköznek toosubscribe túl`$iothub/methods/POST/#`. Az IoT-központ küld metódus kérelmek toohello témakör `$iothub/methods/POST/{method name}/?$rid={request id}`, vagy egy érvényes JSON-adatokat, vagy egy üres szövegtörzzsel.

toorespond, hello eszköz üzenetet küld a egy érvényes JSON vagy üres szövegtörzzsel toohello témakör `$iothub/methods/res/{status}/?$rid={request id}`, ahol **kérelemazonosító** rendelkezik egy hello kérelemüzenetet, a hello toomatch és **állapot** toobe rendelkezik egy egész számot .

További információkért lásd: [közvetlen módszer fejlesztői útmutató][lnk-methods].

### <a name="additional-considerations"></a>Néhány fontos megjegyzés

Végső szempont, mint ha toocustomize hello MQTT protokoll viselkedését hello felhő oldalon tekintse át hello [Azure IoT protokoll-átjáró] [ lnk-azure-protocol-gateway] , amely lehetővé teszi egy nagy teljesítményű toodeploy egyéni protokoll-átjáró, amely közvetlenül az IoT-központ felületek. hello Azure IoT protokoll-átjáró lehetővé teszi, hogy toocustomize hello eszköz protokoll tooaccommodate brownfield MQTT rendszerbe állításához vagy egyéb egyéni protokollok. Ezt a módszert igényel, azonban, hogy futtatja, és egy egyéni protokoll-átjáró működik.

## <a name="next-steps"></a>Következő lépések

toolearn hello MQTT protokoll, bővebben lásd: hello [MQTT dokumentáció][lnk-mqtt-docs].

További információ az IoT-központ telepítésének tervezése toolearn lásd:

* [Az IoT-eszközök katalógus Azure Certified][lnk-devices]
* [Támogatja a további protokollok][lnk-protocols]
* [Az Event Hubs összehasonlítása][lnk-compare]
* [Méretezés, a magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási][lnk-scaling]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
