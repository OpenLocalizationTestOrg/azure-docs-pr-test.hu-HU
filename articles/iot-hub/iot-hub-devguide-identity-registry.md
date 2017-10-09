---
title: "aaaUnderstand hello Azure IoT Hub identitásjegyzékhez |} Microsoft Docs"
description: "Fejlesztői útmutató - hello IoT Hub identitásjegyzékhez leírása, hogyan toouse azt toomanage az eszközök. Tömeges hello importálásának és exportálásának eszköz identitások kapcsolatos információkat tartalmazza."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0706eccd-e84c-4ae7-bbd4-2b1a22241147
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c9fe3730a4608e28c47807ecb42e13e73f6a2e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-identity-registry-in-your-iot-hub"></a>Az IoT hub a hello identitásjegyzékhez ismertetése

Minden IoT-központ rendelkezik egy identitásjegyzékhez tooconnect toohello IoT-központ engedélyezett hello eszközökkel kapcsolatos információkat tárolja. Mielőtt egy eszköz csatlakozna tooan IoT-központot, az eszköznek a hello IoT hub identitásjegyzékhez bejegyzése kell. Egy eszköz hello IoT-központ hello identitásjegyzékhez tárolt hitelesítő adatok alapján is kell hitelesíteni.

hello identitás beállításjegyzékben tárolt hello Eszközazonosító a kis-és nagybetűket.

Magas szinten hello identitásjegyzékhez gyűjteménye REST-kompatibilis eszköz identitás-erőforrások. Amikor a hello identitásjegyzékhez adjon hozzá egy bejegyzést, IoT-központ hoz létre eszközönkénti erőforrások, például az üzenetsoroktól felhő eszközre üzeneteket tartalmazó hello várólista.

### <a name="when-toouse"></a>Ha toouse

Hello identitásjegyzékhez használja, ha szeretné:

* Az IoT-központ tooyour csatlakozó eszközök beállításához.
* Eszköz hozzáférési tooyour hub eszköz felé néző végpontok szabályozzák.

> [!NOTE]
> hello identitásjegyzékhez nem minden alkalmazás-specifikus metaadatot tartalmaz.

## <a name="identity-registry-operations"></a>Identitásjegyzék műveletei

az IoT-központ identitásjegyzékhez hello mutatja meg a következő műveletek hello:

* Hozzon létre az eszközidentitás
* Frissítés az eszközidentitás
* Az eszközazonosító-azonosító szerint beolvasása
* Az eszközazonosító törlése
* Másolatot too1000 identitások felsorolása
* A blob storage összes identitások tooAzure exportálása
* Az Azure blob storage identitások importálása

Ezeket a műveleteket egyidejű hozzáférések optimista, használhatja a [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> csak úgy tooretrieve hello összes identitását egy IoT-központot identitásjegyzékhez toouse hello [exportálása] [ lnk-export] funkciót.

Az IoT-központ identitásjegyzékhez:

* Nem minden alkalmazás metaadatot tartalmaz.
* Hello segítségével szótár, például elérhető **deviceId** hello kulcsként.
* Nem támogatja a kifejező lekérdezések.

Az IoT-megoldás jellemzően rendelkezik, amely tartalmazza az alkalmazás-specifikus metaadatok külön Megoldásfüggő tárolhatják. Például hello megoldás-specifikus tárolási intelligens épület-megoldásban rögzíti hello hely hőmérséklet-érzékelő van telepítve.

> [!IMPORTANT]
> Csak akkor használnak hello identitásjegyzékhez Eszközkezelés és a üzembe helyezési műveletek. Magas teljesítmény műveletek futási időben nem függ a hello identitásjegyzékhez műveletet hajt végre. Például hello kapcsolat állapota az eszköz ellenőrzése előtt parancsot nincs támogatott mintát. Győződjön meg arról, hogy toocheck hello [díjszabás szabályozás] [ lnk-quotas] hello identitásjegyzékhez és hello [eszköz szívverés] [ lnk-guidance-heartbeat] mintát.

## <a name="disable-devices"></a>Eszközök letiltása

Eszközök letilthatja hello frissítésével **állapot** hello identitásjegyzékhez az identitás tulajdonság. Általában akkor használják ezt a tulajdonságot két esetben:

* A létesítési vezénylési során. További információkért lásd: [eszköz kiépítése][lnk-guidance-provisioning].
* Ha bármilyen okból fontolja meg egy eszköz biztonsága sérül, vagy jogosulatlan vált.

## <a name="import-and-export-device-identities"></a>Importálás és exportálás eszköz identitások

Exportálhatja eszköz identitások tömeges az IoT-központ identitás beállításjegyzékből hello aszinkron műveletek használatával [IoT-központ erőforrás-szolgáltató végpont][lnk-endpoints]. Export vannak hosszan futó feladatok egy ügyfél által megadott blob tároló toosave eszköz identitásadatok használó hello identitás beállításkulcs olvasásához.

Aszinkron műveletek hello segítségével importálhatja a tömeges tooan IoT hub identitásjegyzékhez, az eszköz identitások [IoT-központ erőforrás-szolgáltató végpont][lnk-endpoints]. Importálja a hosszan futó használó feladatokat adatok a egy ügyfél által megadott blob tároló toowrite eszköz identitásadatok hello identitás beállításjegyzékbe.

* Hello importálásának és exportálásának API-k kapcsolatos részletes információkért lásd: [IoT-központ erőforrás-szolgáltató REST API-k][lnk-resource-provider-apis].
* bővebben a futó importálni és exportálni a feladatokat, lásd: toolearn [IoT Hub eszköz identitások kezelését tömeges][lnk-bulk-identity].

## <a name="device-provisioning"></a>Eszköz kiépítése

egy adott IoT-megoldás tárolja hello eszközadatok hello konkrét követelmények, hogy a megoldás függ. De legalább egy megoldást kell tárolnia, eszköz identitások és a hitelesítési kulcsokat. Azure IoT Hub tartalmaz egy, az egyes eszközök, például az azonosítók, a hitelesítési kulcsokat, és a állapotkódok értékeket tárolhat identitásjegyzékhez. A megoldás használhatja például a table storage, a blob-tároló vagy a Cosmos DB toostore más Azure-szolgáltatások további adatokat.

*Eszköz kiépítése* hello folyamat hello kezdeti eszköz adattárolókhoz toohello hozzáadása a megoldásban. Új eszköz tooconnect tooyour központ tooenable, hozzá kell adnia egy eszköz azonosítója és a kulcsok toohello IoT-központ identitásjegyzékhez. Hello kiépítési folyamat részeként tooinitialize eszközre vonatkozó adatok más megoldás üzletekben szüksége lehet.

## <a name="device-heartbeat"></a>Eszköz szívverés

hello IoT Hub identitásjegyzékhez tartalmaz egy nevű mező **connectionState**. Csak a hello használata **connectionState** mező fejlesztési és hibakeresés során. Az IoT-megoldások nem kell lekérdezni hello mező futási időben. Például nem lekérdezni a hello **connectionState** mező toocheck, ha egy eszköz csatlakozik, a felhő-eszköz vagy az SMS küldése előtt.

Ha az IoT-megoldásból tooknow van szükség, ha egy eszköz csatlakozik, meg kell valósítania hello *szívverés mintát*.

Hello szívverés mintában hello eszköz eszközről a felhőbe üzeneteket küld legalább egyszer minden rögzített időn (például óránként legalább egyszer). Ezért még akkor is, ha egy eszköz nem rendelkezik semmilyen adatok toosend, továbbra is küldené egy üres eszközről a felhőbe (általában egy tulajdonsággal rendelkező üzenet azt állapítja meg, a szívverés). Hello szolgáltatás oldalon hello megoldás hello utolsó szívverés kapott az egyes eszközök a térkép tart fenn. Hello megoldás hello eszközről hello várt időn belül nem kap szívverésüzenet, ha azt feltételezi, hogy nincs-e hello eszköz hibás.

Egy összetettebb megvalósítási tartalmazhatnak hello adatait [figyelési műveletek] [ lnk-devguide-opmon] tooconnect próbált vagy kommunikációhoz tooidentify eszközök, de sikertelen. Amikor hello szívverés mintát valósítja meg, győződjön meg arról, hogy toocheck [IoT Hub kvóták és szabályozások][lnk-quotas].

> [!NOTE]
> Ha az IoT-megoldás használ hello kapcsolat állapot kizárólag toodetermine e toosend felhő-eszközre küldött üzenetek és üzenetek nem szórási toolarge beállítása az eszközök, érdemes lehet egyszerűbb hello *rövid lejárati idejének* mintát. Ez a minta azonos vezethet, mint a kapcsolat állapota eszközbeállításjegyzékben hello szívverés minta, miközben sokkal hatékonyabb használatával karbantartása hello éri el. Üzenet a nyugtázás kér le, ha az IoT-központ értesíti a felhasználót arról, hogy mely eszközök képes tooreceive üzeneteket, és nem.

## <a name="device-lifecycle-notifications"></a>Eszköz életciklus-értesítést

Az IoT-központ az IoT-megoldásból értesítheti, ha egy eszközidentitás létrehozása vagy törlése úgy, hogy az eszköz életciklus-értesítést küld. toodo Igen, az IoT-megoldásból toocreate útvonal és kell tooset hello adatforrás értéke túl*DeviceLifecycleEvents*. Alapértelmezés szerint nincs életciklus-értesítést kapnak, ez azt jelenti, hogy, hogy nincs ilyen útvonal létezik-e előre. hello üzenet tulajdonságait és a szövegtörzs tartalmaz.

Tulajdonságok: Rendszer üzenettulajdonságok fűzve előtagként hello `'$'` szimbólum.

| Név | Érték |
| --- | --- |
$content-típus | application/json |
$iothub-enqueuedtime |  Ha hello értesítés küldése idő |
$iothub-üzenet-forrás | deviceLifecycleEvents |
$content-kódolás | az UTF-8 |
opType | **createDeviceIdentity** vagy **deleteDeviceIdentity** |
hubName | Az IoT-központ nevét |
deviceId | Hello eszköz azonosítója |
operationTimestamp | A művelet ISO8601 időbélyegzője |
IOT hubbal-üzenet-séma | deviceLifecycleNotification |

Törzs: Ebben a szakaszban JSON formátumú, és az eszközidentitást létrehozott hello hello iker jelöli. Például:

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "etag":"AAAAAAAAAAE=",
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        }
    }
}
```

## <a name="reference-topics"></a>Referencia-témaköreit:

hello következő referencia-témakörökre nyújtanak további információt a hello identitásjegyzékhez.

## <a name="device-identity-properties"></a>Eszköztulajdonságok identitás

Eszköz identitások JSON-dokumentumokként jelölik hello következő tulajdonságai:

| Tulajdonság | Beállítások | Leírás |
| --- | --- | --- |
| deviceId |szükség esetén a frissítések csak olvasható |ASCII 7 bites alfanumerikus karakterekből álló (felfelé too128 karakter) a kis-és nagybetűket karakterlánc + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId |szükség esetén csak olvasható |IoT hub által létrehozott, a kis-és nagybetűket karakterláncra mentése too128 karakter. Az értéket nem használt toodistinguish eszközök hello azonos **deviceId**, törölt és újból létrehozza. |
| ETag |szükség esetén csak olvasható |Egy karakterlánc, amely a hello eszközidentitás, gyenge ETag értékre megfelelően [RFC7232][lnk-rfc7232]. |
| hitelesítés |Nem kötelező |A hitelesítési adatokat és biztonsági anyagokat tartalmazó összetett objektum. |
| auth.symkey |Nem kötelező |Egy elsődleges és másodlagos kulcsot, amely egy összetett objektum base64 formátumban tárolja. |
| status |Szükséges |Az access mutató. Lehet **engedélyezve** vagy **letiltott**. Ha **engedélyezve**, hello eszköz tooconnect engedélyezett. Ha **letiltott**, az eszköz minden eszköz felé néző végpont nem érhető el. |
| statusReason |Nem kötelező |A 128 karakter hosszú karakterlánc-tárolóinak hello ezért hello eszköz identitása állapot. Minden UTF-8 karakterek használhatók. |
| statusUpdateTime |csak olvasható |A historikus mutató, hello dátum-és hello utolsó frissítésének idejét. |
| connectionState |csak olvasható |A kapcsolat állapotát jelző mező: vagy **csatlakoztatva** vagy **Disconnected**. Ez a mező képviseli hello hello eszköz kapcsolati állapotát az IoT Hub nézetben. **Fontos**: Ez a mező csak fejlesztési/hibakeresési célokra lehetne felhasználni. hello kapcsolat állapota csak az eszközök MQTT vagy AMQP frissül. Is (MQTT ping-üzenetek, illetve AMQP ping-üzenetek) protokoll szintű ping-üzenetek alapul, és csak 5 perces késéssel veheti fel. Ezen okok miatt lehet téves, például a csatlakoztatott eszközöket jelentett, de, amely nem kapcsolódik. |
| connectionStateUpdatedTime |csak olvasható |A historikus mutató, hello dátum és az utolsó idő hello kapcsolat állapotát megjelenítő frissítve lett. |
| lastActivityTime |csak olvasható |A historikus mutató, hello dátum és az utolsó idő hello eszköz csatlakozik, fogadott vagy elküldött egy üzenetet. |

> [!NOTE]
> Kapcsolat állapota csak jelenthet hello hello kapcsolat hello állapotának az IoT Hub nézetben. Frissítések toothis állapotát később, attól függően, hogy hálózati körülmények és konfigurációkat.

## <a name="additional-reference-material"></a>További referenciaanyag

Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:

* [IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.
* [Sávszélesség-szabályozási és kvóták] [ lnk-quotas] hello kvóták ismerteti, és a szabályozás viselkedéseket, amelyek az IoT-központ szolgáltatás toohello alkalmazni.
* [Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.
* [Az IoT-központ lekérdezési nyelv] [ lnk-query] hello lekérdezési nyelv használhatja tooretrieve IoT Hub-ből származó adataival a eszköz twins és a feladatokat ismerteti.
* [Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.

## <a name="next-steps"></a>Következő lépések

Most megtanulta, hogyan toouse hello IoT Hub identitásjegyzékhez, esetleg érdekli hello IoT Hub fejlesztői útmutató témakörei a következő:

* [Központi hozzáférési tooIoT szabályozása][lnk-devguide-security]
* [Az eszköz twins toosynchronize állapotát és konfigurációkat használják][lnk-devguide-device-twins]
* [Az eszközön közvetlen metódus][lnk-devguide-directmethods]
* [Több eszközön feladatok ütemezése][lnk-devguide-jobs]

Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ az oktatóanyag következő hello iránt érdeklődik:

* [Ismerkedés az Azure IoT Hub][lnk-getstarted-tutorial]

<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
