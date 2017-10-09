---
title: "aaaRemote előre konfigurált figyelés megoldás forgatókönyv |} Microsoft Docs"
description: "Hello Azure IoT előre konfigurált megoldás távoli megfigyelési és az architektúra leírását."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 57a336bd94938c2b9ee5d3456ea8e45446cf3d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>A távoli figyelési előre konfigurált megoldás bemutatója

az IoT Suite távoli megfigyelési hello [előre konfigurált megoldás] [ lnk-preconfigured-solutions] egy-végpontok megvalósítása figyeli a távoli helyeken futó több gépek megoldás. hello megoldás kulcsfontosságú Azure szolgáltatások tooprovide hello üzleti helyzethez általános végrehajtásának egyesíti. Használhat hello megoldás kiindulási pontként a saját megvalósítási és [testreszabása] [ lnk-customize] azt toomeet saját speciális üzleti igényeinek.

Ez a cikk bemutatja, hogyan néhány fontosabb elemei hello hello távoli figyelési megoldást tooenable toounderstand, hogyan működik. Ezeknek az ismereteknek a birtokában:

* Problémamegoldás hello megoldásban.
* Tervezze meg hogyan toocustomize toohello megoldás toomeet a saját konkrét követelmények. 
* Kialakíthatja saját, Azure-szolgáltatásokat használó IoT-megoldását.

## <a name="logical-architecture"></a>Logikai architektúra

a következő diagram hello hello logikai összetevőinek előre konfigurált hello megoldás ismerteti:

![Logikai architektúra](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Szimulált eszközök

A megoldás előre konfigurált hello hello szimulált eszköz egy hűtőeszköz (például épület légkondicionálók vagy létesítmény vezeték nélkül kezelési egység) jelöli. Előre konfigurált hello megoldás telepítésekor automatikusan is kiépítése futó négy szimulált eszköz egy [Azure webjobs-feladat][lnk-webjobs]. Szimulált hello eszközök megkönnyítik a akkor tooexplore hello viselkedését hello megoldás nélkül hello kell toodeploy fizikai eszközöket. toodeploy tényleges fizikai eszközön, lásd: hello [csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello] [ lnk-connect-rm] oktatóanyag.

### <a name="device-to-cloud-messages"></a>Az eszközről a felhőbe irányuló üzenetek

Minden egyes szimulált eszköz küldhet a következő üzenet típusok tooIoT Hub hello:

| Üzenet | Leírás |
| --- | --- |
| Indítás |Hello eszköz indításakor elküldi egy **eszközinformáció** adatokat saját magáról toohello háttér tartalmazó üzenetet. Ezen adatok tartalmazzák a hello eszközazonosító és hello parancsok listáját, és módszerek hello eszköz támogatja. |
| Jelenlét |Egy eszköz rendszeresen küld egy **jelenléte** tooreport üzenet, hogy hello eszköz is értelemben érzékelő hello jelenlétét. |
| Telemetria |Egy eszköz rendszeresen küld egy **telemetriai** üzenet szimulált értékeinek hello hőmérséklet és a páratartalom hello eszközről összegyűjtött jelentéseket a szimulált érzékelők. |

> [!NOTE]
> hello megoldás tárolja egy Cosmos DB adatbázisban, és nem hello eszköz iker hello eszköz által támogatott parancsok hello listáját.

### <a name="properties-and-device-twins"></a>Tulajdonságok és ikereszközök

hello szimulált eszközök elküldik a következő eszköz tulajdonságok toohello hello [kettős] [ lnk-device-twins] hello IoT-központ, a *tulajdonságok jelentett*. hello eszköz küld jelentett tulajdonságok indításkor és a válasz tooa **eszköz állapotváltoztatási** parancs vagy a metódus.

| Tulajdonság | Cél |
| --- | --- |
| Config.TelemetryInterval | Gyakoriság (másodperc) hello eszköz küld telemetriai adat |
| Config.TemperatureMeanValue | Hello átlagos értéket a szimulált hello hőmérséklet telemetriai adat |
| Device.DeviceID |Azonosító van megadva, vagy egy eszköz hello megoldásban létrehozásakor hozzárendelt |
| Device.DeviceState | Hello eszköz által jelzett állapotát |
| Device.CreatedTime |Hello megoldás idő hello eszköz sikeresen létrehozva |
| Device.StartupTime |Idő hello eszköz indítása |
| Device.LastDesiredPropertyChange |hello verziószámát hello utolsó kívánt tulajdonság módosítása |
| Device.Location.Latitude |A földrajzi hosszúság hello eszköz helye |
| Device.Location.Longitude |Hello eszköz hosszúság helye |
| System.Manufacturer |Eszközgyártó |
| System.ModelNumber |Hello eszköz modell száma |
| System.SerialNumber |Hello eszköz sorozatszáma |
| System.FirmwareVersion |Belső vezérlőprogram hello eszköz jelenlegi verziója |
| System.Platform |Hello eszköz platform architektúrája |
| System.Processor |Processzor futó hello eszköz |
| System.InstalledRAM |Hello eszközre telepített, a RAM mennyisége |

hello szimulátor magok ezeket a tulajdonságokat, a szimulált eszköz minta értékekkel. Minden alkalommal, amikor hello szimulátor inicializálja a szimulált eszköz, hello eszköz jelentett tulajdonságként hello előre definiált metaadatok tooIoT központi jelentés. Jelentett tulajdonságok hello eszköz csak akkor lehet frissíteni. toochange jelentett tulajdonságot, állítsa a kívánt tulajdonság megoldás portálon. Feladata hello hello eszköz:

1. Rendszeres időközönként lekérdezni kívánt tulajdonságokkal hello IoT-központot.
2. Frissítse a konfigurálás szükséges hello tulajdonság értéke.
3. Hello új érték hátsó toohello hub küldése jelentett tulajdonságainál.

Hello megoldás irányítópultról használható *szükséges tulajdonságok* tooset tulajdonságok hello segítségével egy eszközön [eszköz iker][lnk-device-twins]. Általában egy eszköz olvassa be az kívánt tulajdonság értéke a jelentett tulajdonságként módosíthatja a belső állapotot és a jelentés hello hello hub tooupdate.

> [!NOTE]
> hello szimulált eszköz kód csak akkor használja, hello **Desired.Config.TemperatureMeanValue** és **Desired.Config.TelemetryInterval** kívánt tulajdonságokkal tooupdate hello jelentett küldött vissza tulajdonságai tooIoT központ. Az összes többi kívánt tulajdonság változáskérések hello szimulált eszköz figyelmen kívül hagyja.

### <a name="methods"></a>Metódusok

hello szimulált eszközöket képes kezelni a következő módszerek hello ([módszerek közvetlen][lnk-direct-methods]) hello megoldás portálon keresztül hello IoT-központ meghívott:

| Módszer | Leírás |
| --- | --- |
| InitiateFirmwareUpdate |Arra utasítja a hello eszköz tooperform vezérlőprogram-frissítés |
| Újraindítás |Arra utasítja a hello eszköz tooreboot |
| FactoryReset |Arra utasítja a gyári hello eszköz tooperform |

Egyes metódusok használata jelentett tulajdonságok tooreport folyamatban van. Például hello **InitiateFirmwareUpdate** metódus aszinkron módon hello eszközön futó hello frissítés szimulálja. hello metódus azonnal visszaadja hello eszközön, miközben hello aszinkron feladat vissza toosend állapotának frissítése tovább toohello megoldás Irányítópult segítségével jelentett tulajdonságok.

### <a name="commands"></a>Parancsok

Szimulált hello eszközök kezelni tud a következő parancsokat (felhő eszközre üzenetek) hello megoldás portálon keresztül hello IoT-központ által küldött hello:

| Parancs | Leírás |
| --- | --- |
| PingDevice |Elküldi a *ping* toohello eszköz toocheck életben |
| StartTelemetry |Telemetriai adatok küldését indítása hello eszköz |
| StopTelemetry |Leállítja a telemetriai adatok küldését a faxeszközön hello |
| ChangeSetPointTemp |Módosítások hello pont beállítása értéket körül mely hello véletlenszerű adatokat |
| DiagnosticTelemetry |Eseményindítók hello eszköz szimulátor toosend egy további telemetriai értékét (externalTemp) |
| ChangeDeviceState |Hello eszköz egy kiterjesztett tulajdonság módosítása és hello eszköz tájékoztató üzenet küldi hello eszközről |

> [!NOTE]
> Ezen parancsok (felhőből egy eszközre irányuló üzenetek) összehasonlításáért lásd a [felhőből egy eszközre irányuló kommunikáció útmutatóját][lnk-c2d-guidance].

## <a name="iot-hub"></a>IoT Hub

Hello [IoT-központ] [ lnk-iothub] ingests hello eszközökről hello felhőbe küldött adatokat, és lehetővé teszi az elérhető toohello Azure Stream Analytics (ASA) feladatok. Minden egyes adatfolyam ASA feladat külön IoT-központ fogyasztói csoportot tooread hello adatfolyam lévő üzenetek az eszközök használja.

az IoT-központ hello megoldásban is hello:

- Egy tároló hello azonosítók és a hitelesítési kulcsokat tooconnect toohello portal engedélyezett összes hello eszközök identitásjegyzékhez kezeli. Engedélyezi, és tiltsa le az eszközöknek az üdvözlő identitásjegyzékhez.
- Küld parancsokat tooyour eszközök hello megoldás portal nevében.
- Módszerek az eszközök nevében hello megoldás portal hív meg.
- Az összes regisztrált eszközhöz biztosítja a megfelelő ikereszközt. Egy eszköz a két eszköz által jelentett hello tulajdonságértékek tárolja. Egy eszköz iker hello megoldás-portálon, hello eszköz tooretrieve következő csatlakozáskor beállított kívánt tulajdonságok tárolja.
- Ütemezések feladatok több eszközre tooset tulajdonságait, vagy több eszközön metódusok.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

A távoli felügyeleti megoldás, hello [Azure Stream Analytics] [ lnk-asa] (ASA) kiszállítja feldolgozást vagy tárolási hello IoT hub tooother háttér-összetevők által fogadott üzenetek küldési eszköz. Különböző ASA feladatok köszönőüzenetei hello tartalma alapján adott funkció végrehajtására.

**1. feladat: Eszközinformáció** eszköz információs üzenetet hello bejövő üzenet adatfolyam-szűrők és tooan Eseményközpont végpont küldése. Egy eszköz eszköz információs üzenetet küld, indításkor és a válasz tooa **SendDeviceInfo** parancsot. Ez a feladat használja a következő lekérdezés definíciója tooidentify hello **eszközinformáció** üzenetek:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Ez a feladat elküldi a kimeneti tooan Event Hubs további feldolgozásra.

A **2. feladat: szabályok** kiértékeli a bejövő hőmérséklettel és páratartalommal kapcsolatos telemetriaértékeket az eszközönkénti küszöbértékekhez képest. Hello szabályok szerkesztő hello megoldás portálon elérhető küszöbértékek vannak beállítva. A Blob-tárhelyek időbélyegző szerint rendezve tárolják az eszköz/érték párokat, amelyeket a rendszer **referenciaadatokként** olvas be a Stream Analyticsbe. hello feladat hello beállított küszöbértéket hello eszköz szemben nem üres értéket összehasonlítja. Ha az érték meghaladja a hello ">" feltétel, hello feladat kimenete egy **riasztás** eseményt, amely azt jelzi, hogy hello küszöbérték túllépése és hello eszközök, az érték és az időbélyegző értékei biztosít. Ez a feladat a következő lekérdezés definíciója tooidentify telemetriai üzeneteket, amelyek elindítják a riasztót olyan esetben hello használja:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

hello feladat elküldi a kimeneti tooan Event Hubs további feldolgozásra, és menti minden riasztás tooblob tárolási részleteit a ahol hello megoldás portal hello riasztási információkat olvashatja.

**3. feladat: Telemetriai** hello bejövő eszköz telemetriai adatfolyam két módon működik. hello elküldi az összes telemetriai üzenet blobtárolóból hello eszközök toopersistent a hosszú távú tároláshoz. hello második kiszámítja átlagos, minimális és maximális nedvességtartalma keresztül egy 5 perces csúszóablak értéket, majd elküldi a tooblob tárolására. hello megoldás portal hello telemetriai adatokat olvas a blob storage toopopulate hello diagramokat. Ez a feladat a következő lekérdezés definíciója hello használja:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Event Hubs

Hello **eszközinformáció** és **szabályok** ASA feladatok kimeneti az adatok tooEvent hubok tooreliably előre a toohello **Eseményfeldolgozóhoz** hello webjobs-feladat futtatása.

## <a name="azure-storage"></a>Azure Storage

hello megoldás hello eszközökről származó összes hello nyers, összesített és telemetriai adatok használ az Azure blob storage toopersist hello megoldásban. hello portal hello telemetriai adatokat olvas a blob storage toopopulate hello diagramokat. toodisplay riasztások hello megoldás portal hello adatokat olvas blob-tároló, hogy a rekordok telemetriai túllépte értékek hello konfigurálásakor küszöbértékeket. hello megoldás a blob storage toorecord hello küszöbértékek hello megoldás portál beállítása is használ.

## <a name="webjobs"></a>WebJobs

Ezenkívül toohosting hello eszköz szimulátoros oktatás módszereiről, hello WebJobs hello megoldásban is állomás hello **Eseményfeldolgozóhoz** egy Azure webjobs-feladat, amely kezeli a parancs válaszok futtatása. A parancs válasz üzenetek tooupdate hello parancs eszközelőzmények (hello Cosmos DB adatbázisban tárolt) használja.

## <a name="cosmos-db"></a>Cosmos DB

hello megoldás Cosmos DB adatbázis toostore információ a hello eszközök csatlakoztatott toohello megoldást használ. Ezen információk közé tartozik a hello előzmények hello megoldás portálról toodevices küldött parancsokat és hello megoldás portal metódusokra.

## <a name="solution-portal"></a>Megoldásportál

hello megoldás portál egy webalkalmazást, előre konfigurált hello megoldás részeként. hello kulcs hello megoldás portálon oldalai hello irányítópult és hello eszközök listáját.

### <a name="dashboard"></a>Irányítópult

Ezen a lapon hello web app alkalmazásban használja a Power bi javascript vezérlők (lásd: [Power bi-látványelemek tárház](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetriai adatokat hello eszközökről. hello megoldás hello ASA telemetriai feladat toowrite hello telemetriai adatok tooblob tárolást használ.

### <a name="device-list"></a>Eszközlista

Ezen a lapon a hello megoldás portál a következőket teheti:

* Új eszközöket építhet ki. Ez a művelet beállítja a hello az eszköz egyedi azonosítója, és hello hitelesítési kulcsot hoz létre. Hello eszköz tooboth hello IoT Hub identitásjegyzékhez kapcsolatos információkat és hello Megoldásfüggő Cosmos DB adatbázis ír.
* Felügyelheti az eszköztulajdonságokat. A művelet segítségével megtekintheti a meglévő tulajdonságokat, és új tulajdonságokat hozhat létre.
* Parancsok tooa eszköz küldése.
* Egy eszköz hello parancs előzményeinek megtekintése.
* Engedélyezheti és letilthatja a különböző eszközöket.

## <a name="next-steps"></a>Következő lépések

hello TechNet következő blogbejegyzésekben adjon meg több részletet távoli felügyeleti előkonfigurált megoldás hello:

* [Az IoT Suite - alatt hello technikai részletek - távoli figyelése](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [IIoT Suite – Távoli figyelés – Élő és szimulált eszközök hozzáadása](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:

* [Csatlakozzon a távoli felügyeleti előkonfigurált megoldás eszköz toohello][lnk-connect-rm]
* [Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
