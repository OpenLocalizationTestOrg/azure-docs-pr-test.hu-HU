---
title: "Azure IoT Hub-fájl feltöltése aaaUnderstand |} Microsoft Docs"
description: "Fejlesztői útmutató – az eszköz tooan Azure tárolóra fájlok feltöltése az IoT-központ toomanage használata hello fájl feltöltése szolgáltatása."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a0427925-3e40-4fcd-96c1-2a31d1ddc14b
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: d44f9303ead4fa282dc0baf70887af1b8a03293d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-with-iot-hub"></a>Az IoT hubbal fájlok feltöltése

Hello részletezett [IoT-központok végpontjai] [ lnk-endpoints] a cikkben egy eszköz is kezdeményezhető a fájlfeltöltés úgy, hogy egy eszköz felé néző végpont keresztül értesítést küld (**/devices/ {deviceId} /fájlok**). Amikor egy eszköz értesíti, hogy befejeződött-e a feltöltés IoT Hub, az IoT-központ keresztül hello fájl feltöltése értesítő üzenetet küld a **/messages/servicebound/filenotifications** szolgáltatás felé néző végpont.

Helyett kapcsolatoknak a üzeneteket maga az IoT-központ keresztül, az IoT-központ helyette eleget kell egy kézbesítő tooan társított Azure Storage-fiók. Egy eszköz kér, a tárolási jogkivonatot adott toohello fájl hello eszköz IoT-központ által tooupload. hello eszköz hello SAS URI tooupload hello fájl toostorage használja, és hello feltöltés befejezésekor hello eszköz befejezési tooIoT Hub, értesítést küld. Az IoT-központ hello fájl feltöltése befejeződött, és hozzáadja a fájl feltöltése értesítési üzenet toohello szolgáltatás irányuló fájl értesítési végpont ellenőrzi.

Mielőtt feltölti a fájl tooIoT Hub az eszközről, konfigurálnia kell a központ által [társítása egy Azure Storage] [ lnk-associate-storage] tooit fiók.

Az eszköz lehet majd [feltöltés inicializálása] [ lnk-initialize] , majd [értesíteni az IoT-központ] [ lnk-notify] hello feltöltés befejezését. Ha szükséges, ha egy eszköz értesíti az IoT-központ adott hello feltöltése befejeződött, hello szolgáltatást hozhat létre egy [értesítési üzenet][lnk-service-notification].

### <a name="when-toouse"></a>Ha toouse

Fájl feltöltése toosend médiafájlok és nagy telemetriai kötegek töltötte fel időnként csatlakoztatott eszközök vagy tömörített toosave sávszélesség használata.

Tekintse meg a túl[eszközről a felhőbe kommunikációs útmutatást] [ lnk-d2c-guidance] Ha bizonytalan jelentett tulajdonságok, az eszköz a felhőbe küldött üzeneteket vagy a fájl feltöltése használata között.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Egy Azure Storage-fiók társítása az IoT-központ

toouse hello fájl feltöltése funkciót, először egy Azure Storage-fiók toohello IoT-központ rendelnie. Ez a feladat vagy hello hajthatja végre [Azure-portálon][lnk-management-portal], akár programozott módon hello [IoT-központ erőforrás-szolgáltató REST API-k] [ lnk-resource-provider-apis]. Amennyiben az Azure Storage-fiók társított az IoT Hub, hello szolgáltatás esetén ad vissza egy SAS URI tooa eszköz hello eszköz kéréssel fájl feltöltése.

> [!NOTE]
> Hello [Azure IoT SDK-k] [ lnk-sdks] automatikusan hello SAS URI-t, hello fájl feltöltése, és a feltöltött az IoT-központ értesítés leíró lekérése során.


## <a name="initialize-a-file-upload"></a>A fájlfeltöltés inicializálása
Az IoT-központ rendelkezik végpont kifejezetten eszközök toorequest tárolási tooupload egy fájlt egy SAS URI-Azonosítóját. tooinitiate hello fájl feltöltési folyamat hello eszköz egy POST kérést küld túl`{iot hub}.azure-devices.net/devices/{deviceId}/files` a hello a következő JSON-törzsére:

```json
{
    "blobName": "{name of hello file for which a SAS URI will be generated}"
}
```

Az IoT-központ ad vissza adatokat, hogy hello eszközök tooupload hello fájlt használja a következő hello:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Elavult: a GET fájlfeltöltés inicializálása

> [!NOTE]
> Ez a szakasz ismerteti, hogyan elavult funkciók tooreceive az IoT-központ SAS URI. A fentiekben ismertetett hello POST metódussal használható.

Az IoT-központ két REST végpontok toosupport fájl feltöltéséhez, egy tooget hello SAS URI-t a tároláshoz, és más toonotify hello IoT-központ, a feltöltött hello rendelkezik. hello eszköz kezdeményezi hello fájl feltöltési folyamat egy GET toohello IoT-központot, elküldésével `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. az IoT-központ hello adja vissza:

* Egy adott toohello fájl toobe feltöltött SAS URI.
* Amikor befejeződött a feltöltés hello használt korrelációs azonosító toobe.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Az IoT-központ a befejezett fájlfeltöltés értesítése

hello eszköz felelős feltöltése hello fájl toostorage hello Azure Storage SDK-k használatával. Hello feltöltés befejeződése után hello eszköz egy POST kérést küld túl`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` a hello a következő JSON-törzsére:

```json
{
    "correlationId": "{correlation ID received from hello initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

hello értékének `isSuccess` van egy logikai jelző, hogy hello fájl feltöltése sikeresen befejeződött. Az állapotkód: hello `statusCode` hello hello fájl toostorage hello feltöltésének állapotát, és hello `statusDescription` toohello megfelel `statusCode`.

## <a name="reference-topics"></a>Referencia-témaköreit:

hello következő referencia-témakörökre nyújtanak további információt az eszközről fájlok feltöltése.

## <a name="file-upload-notifications"></a>Fájl feltöltése értesítések

Nem kötelező amikor egy eszköz értesíti az IoT-központ, hogy befejeződött-e a feltöltés, IoT-központ hozhat létre egy értesítési üzenetet, amely tartalmazza a hello fájl hello nevét és a tárolási helyét.

A [végpontok][lnk-endpoints], IoT-központ biztosítja a fájl feltöltése értesítések keresztül a szolgáltatás felé néző végpont (**/messages/servicebound/fileuploadnotifications**) üzeneteihez. hello szemantikáját kap, a fájl feltöltése értesítések hello ugyanaz, mint a felhő-eszközre küldött üzenetek és hello azonos [üzenet életciklus][lnk-lifecycle]. Minden egyes hello fájl feltöltése értesítési végpont lekért üzenet egy JSON rekordot hello következő tulajdonságai:

| Tulajdonság | Leírás |
| --- | --- |
| EnqueuedTimeUtc |Hello értesítés létrehozásának jelző időbélyegző. |
| Eszközazonosító |**DeviceId** hello eszköz, amely hello fájl feltöltése. |
| BlobUri |URI-je hello feltöltött fájl. |
| Blobnév |Hello neve feltöltött fájl. |
| LastUpdatedTime |Hello fájl utolsó frissítésekor jelző időbélyegző. |
| BlobSizeInBytes |Hello mérete feltöltött fájl. |

**Példa**. Ez a példa bemutatja, hogy egy fájl hello törzsében feltöltése értesítési üzenetet.

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Fájl feltöltése értesítési konfigurációs beállítások

Minden egyes IoT-központ elérhetővé teszi az alábbi konfigurációs beállítások, a fájl feltöltése értesítések hello:

| Tulajdonság | Leírás | Tartomány- és alapértelmezett |
| --- | --- | --- |
| **enableFileUploadNotifications** |Meghatározza, hogy fájl feltöltése értesítések írt toohello fájl értesítések végpont. |Logikai érték. Alapértelmezett: igaz. |
| **fileNotifications.ttlAsIso8601** |Alapértelmezett élettartam-fájl feltöltése értesítéseket. |Másolatot too48H ISO_8601 időköz (legalább 1 perc). Alapértelmezett: 1 óra. |
| **fileNotifications.lockDuration** |Hello fájl feltöltése értesítések várólista zárolási időtartama. |5 too300 másodpercben (legalább 5 másodperces). Alapértelmezett: 60 másodperc. |
| **fileNotifications.maxDeliveryCount** |Hello fájl maximális száma töltse fel az értesítési várólista. |1 too100. Alapértelmezett: 100. |

## <a name="additional-reference-material"></a>További referenciaanyag

Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:

* [IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.
* [Sávszélesség-szabályozási és kvóták] [ lnk-quotas] hello kvóták ismerteti, és a szabályozás viselkedéseket, amelyek az IoT-központ szolgáltatás toohello alkalmazni.
* [Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.
* [Az IoT-központ lekérdezési nyelv] [ lnk-query] hello lekérdezési nyelv használhatja tooretrieve IoT Hub-ből származó adataival a eszköz twins és a feladatokat ismerteti.
* [Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.

## <a name="next-steps"></a>Következő lépések

Most megtanulta, hogyan tooupload eszközök IoT-központ a fájlokat, esetleg érdekli hello IoT Hub fejlesztői útmutató témakörei a következő:

* [Az IoT Hub eszköz identitásainak kezelése][lnk-devguide-identities]
* [Központi hozzáférési tooIoT szabályozása][lnk-devguide-security]
* [Az eszköz twins toosynchronize állapotát és konfigurációkat használják][lnk-devguide-device-twins]
* [Az eszközön közvetlen metódus][lnk-devguide-directmethods]
* [Több eszközön feladatok ütemezése][lnk-devguide-jobs]

Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ az oktatóanyag következő hello iránt érdeklődik:

* [Hogyan eszközök toohello tooupload fájlok cloud Az IoT hubbal][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
