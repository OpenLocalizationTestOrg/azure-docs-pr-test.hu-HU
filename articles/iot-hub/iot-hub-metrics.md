---
title: "aaaUse metrikák toomonitor Azure IoT Hub |} Microsoft Docs"
description: "Hogyan toouse Azure IoT Hub metrikák tooassess és a figyelő hello az IoT-központok általános állapotát."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a47108fd-f994-4105-b21d-5b8f697b699c
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7d045013fb0229f488e72c93a6f668048b9d5c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-iot-hub-metrics"></a>Az IoT-központ metrikák ismertetése
IoT Hub mérőszámok segítenek hello Azure IoT-erőforrások állapotát hello jobb adatait az Azure-előfizetése. Az IoT-központ metrikák engedélyezése meg tooassess hello hello IoT-központ szolgáltatás és hello eszközök általános állapotát tooit csatlakoztatva. Felhasználók számára is elérhető statisztika fontosak, mert azok segíti a jelenít meg az IoT hub és súgó kiváltó okának kapcsolatos problémákkal rendelkező anélkül, hogy az Azure támogatási toocontact.

Alapértelmezés szerint engedélyezve vannak a metrikákat. Hello Azure-portálon az IoT-központ metrikák tekintheti meg.

## <a name="how-tooview-iot-hub-metrics"></a>Hogyan tooview IoT-központ metrikák
1. Létrehoz egy IoT-központot. Hogyan találhat útmutatót az IoT-központ a hello toocreate [Ismerkedés] [ lnk-get-started] útmutató.
2. Az IoT hub hello panel megnyitásához. Itt kattintson **metrikák**.
   
    ![][1]
3. Hello metrikák paneljéről hello metrikák megtekintheti az IoT hub és a metrikákat, az egyéni nézeteket hozhat létre. Dönthet úgy toosend a metrikák adatok tooan Event Hubs-végpontot, vagy egy Azure Storage-fiók kattintva **diagnosztikai beállítások**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-toouse-them"></a>Az IoT-központ metrikákat, és hogyan toouse őket
Az IoT-központ több mérőszámait jeleníti meg a központi és hello hello állapotának áttekintése teljes száma toogive csatlakoztatott eszközök. Több metrikák toopaint hello IoT-központ hello állapotának áttekintő képet adatait kombinálhatja. hello a következő táblázat ismerteti, nyomon követi az egyes IoT-központ hello metrikákat, és hogyan mindegyik metrikát vonatkozik toohello általános állapotának hello IoT-központot.

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetria üzenet küldési kísérlet|Darabszám|Összes|Elküldött tooyour IoT-központ eszközről a felhőbe telemetriai üzenetek megkísérelt toobe száma|
|d2c.telemetry.ingress.SUCCESS|Küldött telemetriai üzenetek|Darabszám|Összes|Tooyour IoT-központ sikeresen elküldve az eszközről a felhőbe telemetriai üzenetek száma|
|c2d.commands.egress.Complete.SUCCESS|A parancsok befejeződött|Darabszám|Összes|Hello eszköz által végrehajtott felhő eszközre parancsok száma|
|c2d.commands.egress.Abandon.SUCCESS|Elhagyott parancsok|Darabszám|Összes|Hello eszköz által félbe hagyott felhő eszközre parancsok száma|
|c2d.commands.egress.Reject.SUCCESS|Elutasított parancsok|Darabszám|Összes|Hello eszköz által elutasított felhő eszközre parancsok száma|
|devices.totalDevices|Eszközök teljes száma|Darabszám|Összes|A regisztrált eszközök száma tooyour IoT-központ|
|devices.connectedDevices.allProtocol|Csatlakoztatott eszközök|Darabszám|Összes|A csatlakoztatott eszközök száma tooyour IoT-központ|
|d2c.telemetry.egress.SUCCESS|Telemetria kézbesített üzenetek|Darabszám|Összes|Ennyiszer üzenetek sikeresen készültek tooendpoints (összesen)|
|d2c.telemetry.egress.dropped|Az eldobott üzenetek|Darabszám|Összes|Az üzenetek száma dobva, mert nem felelt meg egyetlen útvonalak és hello tartalék útvonal le lett tiltva.|
|d2c.telemetry.egress.Orphaned|Árva üzenetek|Darabszám|Összes|hello bármely útvonalakat, beleértve a hello tartalék útvonal nem megfelelő üzenetek száma|
|d2c.telemetry.egress.Invalid|Érvénytelen üzenet|Darabszám|Összes|hello üzenetek száma nem érkezik miatt hello végponttal tooincompatibility|
|d2c.telemetry.egress.fallback|Tartalék feltételnek megfelelő üzenetek|Darabszám|Összes|Tartalék végpont toohello üzenetek száma|
|d2c.endpoints.egress.eventHubs|Üzenetek kézbesítése történik tooEvent központok végpontjai|Darabszám|Összes|Üzenetek volt sikeresen írásbeli tooEvent Hub végpontok száma|
|d2c.endpoints.latency.eventHubs|Az Event Hubs végpontok üzenet késés|ideje ezredmásodpercben|Átlagos|hello átlagos közötti késés üzenet érkező toohello IoT-központ és az üzenet belépő az Event Hubs végpont, ezredmásodpercben|
|d2c.endpoints.egress.serviceBusQueues|Üzenetek kézbesítése történik tooService Bus-üzenetsorba végpontok|Darabszám|Összes|Üzenetek volt sikeresen írásbeli tooService Bus-üzenetsorba végpontok száma|
|d2c.endpoints.latency.serviceBusQueues|A Service Bus-üzenetsorba végpontokhoz üzenet késés|ideje ezredmásodpercben|Átlagos|hello átlagos közötti késés üzenet érkező toohello IoT-központ és az üzenet belépő be egy Service Bus-üzenetsorba végpont ezredmásodpercben|
|d2c.endpoints.egress.serviceBusTopics|Üzenetek kézbesítése történik tooService Bus-témakörbe végpontok|Darabszám|Összes|Üzenetek volt sikeresen írásbeli tooService Bus-témakörbe végpontok száma|
|d2c.endpoints.latency.serviceBusTopics|A Service Bus-témakörbe végpontokhoz üzenet késés|ideje ezredmásodpercben|Átlagos|hello átlagos közötti késés üzenet érkező toohello IoT-központ és az üzenet belépő be egy Service Bus-témakörbe végpont ezredmásodpercben|
|d2c.endpoints.egress.builtIn.events|Üzenetek kézbesítése történik toohello beépített végpont (üzenet/esemény)|Darabszám|Összes|Ennyiszer üzenetek volt sikeresen írásbeli toohello beépített végpont (üzenet/esemény)|
|d2c.endpoints.latency.builtIn.events|Üzenet késés hello beépített végpont (üzenet/esemény)|ideje ezredmásodpercben|Átlagos|hello átlagos várakozási ideje üzenet érkező toohello IoT-központ és az üzenet érkező között történő hello beépített végpont (üzenet/esemény), ezredmásodpercben |
|d2c.Twin.Read.SUCCESS|Sikeres iker olvassa be az eszközökön|Darabszám|Összes|olvassa be az összes sikeres eszköz által kezdeményezett iker hello száma.|
|d2c.Twin.Read.failure|Nem sikerült a kettős olvasások eszközökről|Darabszám|Összes|hello teljes számát a két eszköz által kezdeményezett olvasás sikertelen volt.|
|d2c.Twin.Read.size|A két válasz mérete olvassa be az eszközökön|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének minden sikeres eszköz által kezdeményezett iker olvasási műveletek.|
|d2c.Twin.Update.SUCCESS|Sikeres iker frissítések eszközökről|Darabszám|Összes|a két eszköz által kezdeményezett összes sikeres frissítések hello száma.|
|d2c.Twin.Update.failure|Nem sikerült a két frissítések eszközökről|Darabszám|Összes|hello teljes számát a két eszköz által kezdeményezett frissítések nem sikerült.|
|d2c.Twin.Update.size|A két frissítések eszközökről mérete|Bájtok|Átlagos|hello átlagos, minimális és maximális méretét az összes sikeres eszköz által kezdeményezett iker frissítések.|
|c2d.methods.SUCCESS|Sikeres közvetlen módszer meghívásához|Darabszám|Összes|az összes sikeres közvetlen metódushívások hello száma.|
|c2d.methods.failure|Nem sikerült a közvetlen módszer meghívásához|Darabszám|Összes|hello teljes számát a közvetlen metódushívások nem sikerült.|
|c2d.methods.requestSize|A közvetlen módszer meghívásához kérelem mérete|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének minden sikeres közvetlen metódusú kérelmeket.|
|c2d.methods.responseSize|Válasz mérete közvetlen módszer meghívásához|Bájtok|Átlagos|hello átlagos, a min és a max az összes sikeres közvetlen módszer válasz.|
|c2d.Twin.Read.SUCCESS|Sikeres iker háttérből olvassa be|Darabszám|Összes|olvassa be az összes sikeres back-end által kezdeményezett iker hello száma.|
|c2d.Twin.Read.failure|Sikertelen a két háttérből olvassa be|Darabszám|Összes|hello teljes számát a két back-end által kezdeményezett olvasások nem sikerült.|
|c2d.Twin.Read.size|A válasz méretének iker olvasások háttérből|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének minden sikeres back-end által kezdeményezett iker olvasási műveletek.|
|c2d.Twin.Update.SUCCESS|Sikeres iker frissítések háttérből|Darabszám|Összes|az összes sikeres back-end által kezdeményezett iker frissítések hello száma.|
|c2d.Twin.Update.failure|Sikertelen a két frissítések háttérből|Darabszám|Összes|hello teljes számát a két back-end által kezdeményezett frissítések nem sikerült.|
|c2d.Twin.Update.size|A két frissítések háttérből mérete|Bájtok|Átlagos|hello átlagos, minimális és maximális méretét az összes sikeres back-end által kezdeményezett iker frissítések.|
|twinQueries.success|Sikeres iker lekérdezések|Darabszám|Összes|az összes sikeres iker lekérdezés hello száma.|
|twinQueries.failure|Sikertelen a két lekérdezések|Darabszám|Összes|összes sikertelen iker lekérdezések hello száma.|
|twinQueries.resultSize|A két lekérdezések eredmény méretének|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének hello eredmény méretének minden sikeres iker lekérdezések.|
|jobs.createTwinUpdateJob.success|A sikeres és a két frissítési feladatok|Darabszám|Összes|a két frissítési feladatok minden sikeres létrehozása hello száma.|
|jobs.createTwinUpdateJob.failure|Sikertelen és a két frissítési feladatok|Darabszám|Összes|a két frissítési feladatok során létrehozására tett sikertelen kísérlet hello száma.|
|jobs.createDirectMethodJob.success|A metódus meghívása feladatok sikeres létrehozása|Darabszám|Összes|közvetlen metódus meghívása feladatok minden sikeres létrehozása hello száma.|
|jobs.createDirectMethodJob.failure|A metódus meghívása feladatok sikertelen számát|Darabszám|Összes|közvetlen metódus meghívása feladatok során létrehozására tett sikertelen kísérlet hello száma.|
|jobs.listJobs.success|Sikeres hívás toolist feladatok|Darabszám|Összes|hello száma az összes sikeres hívás toolist feladat.|
|jobs.listJobs.failure|Sikertelen hívások toolist feladatok|Darabszám|Összes|összes sikertelen hívás toolist feladat hello száma.|
|jobs.cancelJob.success|Megszakított feladatok sikeres|Darabszám|Összes|az összes sikeres hello száma meghívja toocancel egy feladatot.|
|jobs.cancelJob.failure|Sikertelen feladat sikertelen|Darabszám|Összes|összes sikertelen hívás toocancel egy feladat hello száma.|
|jobs.queryJobs.success|Feladat sikeres lekérdezések|Darabszám|Összes|hello száma az összes sikeres hívás tooquery feladat.|
|jobs.queryJobs.failure|Sikertelen feladat-lekérdezések|Darabszám|Összes|összes sikertelen hívás tooquery feladat hello száma.|
|jobs.Completed|Befejezett feladatokhoz|Darabszám|Összes|az összes befejezett feladatokhoz hello számát.|
|jobs.Failed|A sikertelen feladatok|Darabszám|Összes|összes sikertelen feladatok száma hello.|

## <a name="next-steps"></a>Következő lépések
Most, hogy az IoT-központ metrikák áttekintést láthatta, kövesse a hivatkozást toolearn Azure IoT Hub kezelésével kapcsolatos további:

* [Figyelési műveletek][lnk-monitor]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
