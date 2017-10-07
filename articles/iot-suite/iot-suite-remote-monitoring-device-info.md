---
title: "távoli felügyeleti megoldás hello aaaDevice információk metaadatok |} Microsoft Docs"
description: "Hello Azure IoT előre konfigurált megoldás távoli megfigyelési és az architektúra leírását."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 8387b98b8b2ae4934b0c900bc4df37dc17337c60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="device-information-metadata-in-hello-remote-monitoring-preconfigured-solution"></a>Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás hello

hello Azure IoT Suite távoli felügyeleti előkonfigurált megoldás egy eszköz metaadatait kezelésére szolgáló módszert mutatja be. Ez a cikk ismerteti a hello megközelítés ehhez a megoldáshoz szükséges tooenable meg toounderstand:

* Milyen metaadatok hello megoldás tárolja.
* Hello megoldás hogyan kezeli a hello eszköz metaadatait.

## <a name="context"></a>Környezet

hello távoli megfigyelési előre konfigurált megoldás által használt [Azure IoT Hub] [ lnk-iot-hub] tooenable az eszközök toosend adatok toohello felhő. hello megoldás három különböző helyeken eszközökkel kapcsolatos információkat tárolja:

| Hely | Tárolt információ | Megvalósítás |
| -------- | ------------------ | -------------- |
| Identitásjegyzékhez | Eszközazonosító, a hitelesítési kulcsokat, engedélyezett állapota | A beépített tooIoT Hub |
| Eszköz twins | Metaadatok: jelentett tulajdonságai, a kívánt tulajdonságokat, a címkék | A beépített tooIoT Hub |
| Cosmos DB | A parancs és metódus előzményei | Egyéni megoldás |

Az IoT-központ tartalmazza a [eszközidentitási] [ lnk-identity-registry] toomanage tooan IoT hub és a hozzáférési [eszköz twins] [ lnk-device-twin] toomanage eszköz metaadatait . Is van a távoli felügyeleti megoldás-specifikus *eszközbeállításjegyzékben* , amely tárolja a parancs és metódus előzményeit. hello távoli figyelési megoldást használ egy [Cosmos DB] [ lnk-docdb] adatbázis tooimplement parancs és metódus előzményekhez szükséges egyéni tárolót.

> [!NOTE]
> távoli felügyeleti előkonfigurált megoldás hello tartja hello eszközidentitási hello információk szinkronban hello Cosmos DB adatbázisban. Mindkettő használja a azonos eszköz azonosítója toouniquely azonosítása hello minden eszköz csatlakoztatva tooyour IoT-központot.

## <a name="device-metadata"></a>Eszköz metaadatait

Az IoT-központ tart fenn a [eszköz iker] [ lnk-device-twin] az egyes szimulált és a fizikai eszközök távoli felügyeleti megoldás tooa csatlakoztatva. hello megoldás az eszköz twins toomanage hello társított metaadatokat eszközök. Egy eszköz iker IoT-központ által kezelt JSON-dokumentum, és hello megoldás eszköz twins hello IoT Hub API toointeract használ.

Egy eszköz iker metaadatok három típusú tárolja:

- *Tulajdonságok jelentett* tooan IoT hub eszköz által küldött. Hello távoli felügyeleti megoldás, a szimulált eszköz küldött jelentésben szereplő tulajdonságok indításkor és a válasz túl**módosította az eszköz állapotát,** parancsok és módszerek. Jelentett tulajdonságok megtekintheti a hello **eszközlista** és **eszközadatok** hello megoldás portálon. Jelentett tulajdonságok csak olvashatók.
- *Szükségeskonfiguráció-tulajdonságok* lekért hello IoT hub-eszközök által. Hello felelőssége hello eszköz toomake bármely szükséges konfigurációs változásokat hello eszközön. Feladata is hello hello eszköz tooreport hello módosítás vissza toohello központ jelentett tulajdonságainál. Beállíthatja a kívánt tulajdonság értéke hello megoldás portálon keresztül.
- *Címkék* hello eszköz iker csak szerepelnek, és egy eszköz nem lesznek szinkronizálva. Állítsa be az értékeket hello megoldás portálon, és az eszközök listájának hello szűrésekor használja őket. hello megoldás is használ egy címke tooidentify hello ikon toodisplay hello megoldás portálon egy eszközhöz.

Példa jelentett, a szimulált hello eszközökről tulajdonságai közé tartozik a gyártó, a modell számának, a szélességi és hosszúsági. Szimulált eszköz is visszatérési hello támogatott módszerek listája jelentett tulajdonságainál.

> [!NOTE]
> hello szimulált eszköz kód csak akkor használja, hello **Desired.Config.TemperatureMeanValue** és **Desired.Config.TelemetryInterval** kívánt tulajdonságokkal tooupdate hello jelentett küldött vissza tulajdonságai tooIoT központ. Az összes többi kívánt tulajdonság változáskérések figyelmen kívül lesznek hagyva.

Egy eszköz információk metaadatok JSON-dokumentum hello eszköz beállításjegyzék Cosmos DB adatbázisban tárolt struktúra a következő hello rendelkezik:

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> Eszközadatok tartalmazhat metaadatok toodescribe hello telemetriai hello eszköz küld tooIoT Hub is. hello távoli figyelési megoldást használja a telemetriai adatok metaadatok toocustomize hello irányítópult megjelenítésének [dinamikus telemetriai][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Életciklus

Ha először eszköz hello megoldás portálon, hello megoldás hoz létre egy Cosmos DB adatbázis toostore parancs hello bejegyzést és metódus előzményei. Ezen a ponton hello megoldás is létrehoz egy tételt hello eszköz hello eszközidentitási, amely hello kulcsok hello eszköz által használt tooauthenticate IoT-központ állít elő. Egy eszköz iker is létrehoz.

Ha egy eszköz először csatlakozik toohello megoldás, és küldi el jelentett tulajdonságai egy eszköz tájékoztató üzenetet. hello jelentett tulajdonságértékek automatikusan megtörténik az eszköz a két hello. hello jelentett tulajdonságai közé tartozik a hello gyártó modellszámát, sorozatszám és támogatott módszerek listája. eszköz információk üdvözlőüzenetére hello listán szereplő hello parancsok hello eszköz támogatja, beleértve bármilyen parancs paraméterei kapcsolatos információkat tartalmazza. Amikor hello megoldás ezt az üzenetet kap, frissíti hello eszközadatokat hello Cosmos DB adatbázisban.

### <a name="view-and-edit-device-information-in-hello-solution-portal"></a>Hello megoldás portálon eszköz adatainak megtekintése és szerkesztése

hello hello megoldás portálon eszközök listáját jeleníti meg eszköztulajdonságok alábbi oszlopokként alapértelmezés szerint hello: **állapot**, **DeviceId**, **gyártó**, **Típusszámát**, **sorozatszám**, **belső vezérlőprogram**, **Platform**, **processzor**, és  **RAM telepített**. Testre szabhatja hello oszlopok kattintva **oszlop szerkesztő**. Eszköztulajdonságok hello **szélesség** és **hosszúság** hello helyre a Bing térképek hello meghajtó hello irányítópulton.

![Oszlop-szerkesztőt az eszközök listája][img-device-list]

A hello **eszközadatok** ablaktáblán hello megoldás portálon, szerkesztheti a kívánt tulajdonságokkal és címkék (csak olvasható tulajdonságok jelentette).

![Eszköz részletező ablaktábláján][img-device-edit]

A megoldás hello megoldás portál tooremove egy eszközt használhatja. Egy eszköz eltávolításakor hello megoldás identitásjegyzékhez eltávolítja a hello eszköz bejegyzést, és majd törli az eszköz a két hello. hello megoldás is eltávolítja az adatokat kapcsolódó toohello eszköz hello Cosmos DB adatbázisból. Eszköz eltávolítása előtt le kell tiltani.

![Eszköz eltávolítása][img-device-remove]

## <a name="device-information-message-processing"></a>Eszköz információk üzenet feldolgozása

Eszköz információs üzenetet küldött egy eszköz különbözőek a telemetria-üzeneteket. Eszköz információk üzenetekben eszköz válaszolhassanak hello parancsok, és a parancselőzményekben. Maga az IoT-központ nincsenek saját ismeretei az eszköz információk üzenet tárolt hello metaadatok és a folyamatok hello hello üzenet ugyanúgy bármely eszközről a felhőbe üzenetet feldolgozza. A távoli felügyeleti megoldás, hello egy [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) feladat köszönőüzenetei olvassa be az IoT-központot. Hello **deviceinfo információja** stream analytics-feladat tartalmazó üzenetek szűrők **"Objektumtípus": "Deviceinfo információja"** , majd továbbítja azokat toohello **EventProcessorHost** állomás a webes projekt futtató példányhoz. A logikai hello **EventProcessorHost** példány hello adott eszköz és a frissítés hello rekord hello azonosító toofind hello Cosmos DB rekordokat használ.

> [!NOTE]
> Egy eszköz információs üzenet jelenik meg egy szabványos eszközről a felhőbe üzenet. hello megoldás különböztet eszköz információs üzenetet és telemetriai üzenetek ASA lekérdezések használatával.

## <a name="next-steps"></a>Következő lépések

Miután befejezte a tanulási, hogyan szabhatja testre előre konfigurált hello megoldások, ismerje meg a néhány hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Előre konfigurált prediktív karbantartási megoldás áttekintése][lnk-predictive-overview]
* [Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]
* [A hello IoT biztonsági szabad][lnk-security-groundup]

<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
