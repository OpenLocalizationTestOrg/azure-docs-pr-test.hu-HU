---
title: "aaaAzure IoT előre konfigurált megoldások |} Microsoft Docs"
description: "Hello Azure IoT leírása előre konfigurált megoldások és hivatkozások tooadditional erőforrásokkal architektúráját."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: bd059d08ab458fdb0b6f49b3ac469db930dab09e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-hello-azure-iot-suite-preconfigured-solutions"></a>Mik azok a hello Azure IoT Suite előre konfigurált megoldások?

hello Azure IoT Suite előre konfigurált megoldások olyan IoT-megoldás közös minták, hogy az előfizetés használatával tooAzure telepíthet implementációja. Használhat előre konfigurált hello megoldások:

* A saját IoT-megoldásai kezdőpontjaként.
* az IoT-megoldás kialakításának és fejlesztési közös mintájával kapcsolatos toolearn.

Minden előkonfigurált megoldás, hogy a használt eszközök toogenerate telemetriai szimulált a teljes, végpontok közötti megvalósítása.

Hello teljes forrás kód toocustomize letöltheti és kiterjesztése hello megoldás toomeet az adott IoT igények szerint.

> [!NOTE]
> toodeploy hello az előre konfigurált megoldások, látogasson el [Microsoft Azure IoT Suite][lnk-azureiotsuite]. hello cikk [Ismerkedés a hello előre konfigurált IoT-megoldások] [ lnk-getstarted-preconfigured] hogyan toodeploy és futtatása egy hello megoldások további információt nyújt.

hello következő táblázatban hogyan hello megoldások leképezése toospecific IoT-funkciókat:

| Megoldás | Adatfeldolgozás | Eszközidentitás | Eszközfelügyelet | Parancs és vezérlés | Szabályok és műveletek | Prediktív elemzés |
| --- | --- | --- | --- | --- | --- | --- |
| [Távoli figyelés][lnk-getstarted-preconfigured] |Igen |Igen |Igen |Igen |Igen |- |
| [Prediktív karbantartás][lnk-predictive-maintenance] |Igen |Igen |- |Igen |Igen |Igen |
| [Csatlakoztatott gyár][lnk-getstarted-factory] |Igen |Igen |Igen |Igen |Igen |- |

* *Adatfeldolgozást*: érkező méretezési toohello felhő adatainak.
* *Az eszközazonosító*: az eszköz egyedi identitást kezeljen és szabályozhatja a hozzáférést toohello megoldás.
* *Eszközfelügyelet*: Eszköz metaadatainak kezelése és olyan műveletek elvégzése, mint az eszközök újraindítása és a belső vezérlőprogramok frissítése.
* *A parancs és a vezérlő*: toocause hello eszköz tootake Action, küldjön üzeneteket tooa eszköz hello felhőből.
* *Szabályok és a műveletek*: adatokon adott eszközről a felhőbe, hello megoldás háttérrendszerének tooact szabályokat használ.
* *A prediktív elemzés*: hello megoldás háttérrendszerének elemzi az eszközről a felhőbe adatok toopredict, amikor bizonyos műveleteket kell végrehajtani. Repülőgép motor telemetriai toodetermine például elemzése, amikor motor karbantartásának.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>A távoli figyelési előre konfigurált megoldás áttekintése

Azt választotta, toodiscuss hello ebben a cikkben távoli figyelési előkonfigurált megoldás, mert azt szemlélteti, hogy számos gyakori látványelemek, más megoldások megosztás hello.

hello következő diagram azt ábrázolja hello fő elemei a távoli felügyeleti megoldás hello. hello következő szakaszok további információval szolgálnak ezeket az elemeket.

![Távoli figyelési előre konfigurált megoldás architektúrája][img-remote-monitoring-arch]

## <a name="devices"></a>Eszközök

Távoli felügyeleti előkonfigurált megoldás hello telepítésekor négy szimulált tartoznak előtti ponton aktívvá vált hűtőeszköz szimulálása hello-megoldásban. Ezek a szimulált eszközök beépített hőmérséklet- és páratartalom-modellel rendelkeznek, amelyek telemetriai adatokat küldenek. Ezek a szimulált eszközök a következők érdekében vannak megadva:

- Hello végpontok közötti adatáramlás hello megoldás a bemutatására.
- A telemetriák kényelmesen használható forrását nyújtják.
- Adjon meg egy cél módszerek vagy parancsok egy háttér-fejlesztők megoldással hello kiindulási pontként egy egyéni végrehajtásához.

Szimulált hello eszközök hello megoldásban válaszolhassanak toohello felhő eszközre kommunikációt a következő:

- *Módszerek ([módszerek közvetlen][lnk-direct-methods])*: Ha a csatlakoztatott eszközön várt toorespond azonnal kétirányú kommunikációs módszer.
- *Parancsok (felhő eszközre üzenetek)*: Ha egy eszköz lekéri a tartós várólista hello parancs egyirányú kommunikációs módszer.

Ezen különböző megközelítések összehasonlításáért lásd a [felhőből egy eszközre irányuló kommunikáció útmutatóját][lnk-c2d-guidance].

Ha egy eszköz először csatlakozik tooIoT Hub előre konfigurált hello megoldásban, toohello hub eszköz információk üzenet küld. Ez az üzenet hello eszköz válaszolhassanak hello módszerek enumerálása. Szimulált eszközök távoli felügyeleti előkonfigurált megoldás hello, ezeket a módszereket támogatja:

* *Belső vezérlőprogram frissítése kezdeményezése*: Ez a módszer kezdeményezi a hello eszköz tooperform vezérlőprogram-frissítés aszinkron feladat. hello aszinkron feladat jelentett tulajdonságok toodeliver állapot frissítések toohello megoldás irányítópultja használja.
* *Újraindítás*: a metódus hello szimulált eszköz tooreboot hatására.
* *FactoryReset*: Ez a módszer a gyári beállításokra történő visszaállításának hello szimulált eszköz váltja ki.

Ha egy eszköz először csatlakozik tooIoT Hub előre konfigurált hello megoldásban, toohello hub eszköz információk üzenet küld. Ez az üzenet hello eszköz válaszolhassanak hello parancsok enumerálása. A távoli felügyeleti előkonfigurált megoldás hello szimulált eszköz támogatja ezeket a parancsokat:

* *Ping eszközt*: hello eszköz válaszol nyugtázást toothis parancsot. Ez a parancs esetén hasznos ellenőrzése hello eszköz továbbra is az aktív és figyelő.
* *Indítsa el a telemetriai adatok*: arra utasítja a hello eszköz toostart telemetriai adatok küldését.
* *Állítsa le a telemetriai adatok*: arra utasítja a hello eszköz toostop telemetriai adatok küldését.
* *Módosítsa a pont beállítása hőmérséklet*: vezérlők hello szimulált hőmérséklet telemetriai értékek hello eszköz küld. Ez a parancs a háttérlogika teszteléséhez hasznos.
* *Diagnosztikai Telemetriai*: szabályozza, hogy hello eszköz küldje el külső hőmérséklet hello telemetriai adatokat.
* *Eszköz állapotváltoztatási*: hello eszköz állapota metaadatok tulajdonság, amely eszközökről készült jelentések hello beállítása. Ez a parancs a háttérlogika teszteléséhez hasznos.

Hozzáadhat további szimulált eszköz toohello megoldás, amely a kibocsátás hello azonos telemetriai és válaszoljon toohello azonos módszerek és parancsok.

Ezenkívül tooresponding toocommands és módszerek, hello megoldás az [eszköz twins][lnk-device-twin]. Eszközök eszköz twins tooreport tulajdonság értékek toohello megoldás háttérrendszerének használnak. hello megoldás irányítópultja eszköz twins tooset szükséges toonew tulajdonság értékeket használja, az eszközökön. Hello belső vezérlőprogram frissítési folyamat szimulált hello eszközökről készült jelentések során például hello hello állapotának frissítése jelentett tulajdonságok használatával.

## <a name="iot-hub"></a>IoT Hub

Előre konfigurált megoldásban hello IoT Hub-példány megfelel-e toohello *Felhőátjáróhoz* egy általános [IoT-megoldásarchitektúra][lnk-what-is-azure-iot].

Az IoT-központ telemetriai kap egy végpontot hello eszközökön. Az IoT-központ is fenntartja, ahol minden eszköz tudja lekérni a hello parancsok tooit küldött eszközspecifikus végpontok.

hello IoT-központ hello Szolgáltatásoldali telemetriai olvassa el a végpont keresztül elérhetővé kapott hello telemetriai adatokat.

hello eszköz kezelési lehetőségek az IoT-központ lehetővé teszi, hogy Ön toomanage hello megoldás portal és ütemezés feladatokból műveleteket, mint az eszköz tulajdonságok:

- Eszközök újraindítása
- Eszközállapotok módosítása
- Belső vezérlőprogram frissítése

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

hello előkonfigurált megoldás az három [Azure Stream Analytics] [ lnk-asa] (ASA) feladatok toofilter hello telemetriai adatfolyam hello eszközökről:

* *Deviceinfo információja feladat* -kimenetek adatok tooan eseményközpont, amely eszköz regisztrációs-specifikus üzenetek toohello megoldás eszköz beállításjegyzékébe. Ez az eszközjegyzék egy Azure Cosmos DB-adatbázis. Ezek az üzenetek küldése történik, amikor először csatlakozik egy eszközt, vagy a válasz tooa **módosította az eszköz állapotát,** parancsot.
* *Telemetria feladat* – összes nyers telemetriaadatok tooAzure blob-tároló cold tárolási küld, és kiszámítja az hello megoldás irányítópult megjelenítő telemetriai összesítések.
* *Szabályok feladat* - szűrők hello telemetriai adatfolyam értékek, amelyek mérete meghaladja a bármely szabály küszöbértékek és kimenetek hello adatok tooan eseményközpontot. A szabály akkor következik be, amikor hello megoldás portál irányítópult-nézet hello riasztás tábla egy új sor jeleníti meg ezt az eseményt. Ezek a szabályok is elindítható hello definiált hello beállításai alapján a művelet **szabályok** és **műveletek** nézetek hello megoldás portálon.

Előre konfigurált megoldásban hello ASA feladatok toohello részét képező **IoT-megoldás háttérrendszerének** egy általános [IoT-megoldásarchitektúra][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Eseményfeldolgozó

Előre konfigurált megoldásban hello eseményfeldolgozóhoz hello részét képezi **IoT-megoldás háttérrendszerének** egy általános [IoT-megoldásarchitektúra][lnk-what-is-azure-iot].

Hello **deviceinfo információja** és **szabályok** ASA feladatok küldése a kimeneti tooEvent hubok kézbesítésre tooother háttér-szolgáltatásaihoz. hello megoldás használ egy [EventProcessorHost] [ lnk-event-processor] példány fut egy [webjobs-feladat][lnk-web-job], ezek eseményközpontokból származó tooread köszönőüzenetei. Hello **EventProcessorHost** használja:
- Hello **deviceinfo információja** adatok tooupdate hello eszközadatok hello Cosmos DB adatbázisban.
- Hello **szabályok** tooinvoke hello Logic app és a frissítés hello Adatriasztások meg hello megoldás portálon.

## <a name="device-identity-registry-device-twin-and-cosmos-db"></a>Eszközidentitás-jegyzék, ikereszköz és Cosmos DB

Minden IoT Hub tartalmaz eszközkulcsokat tároló [eszközidentitás-jegyzéket][lnk-identity-registry]. Az IoT-központ használja ezt az információt eszközök hitelesítését – az eszköz regisztrálva kell lennie, és rendelkezik érvényes kulccsal, mielőtt az csatlakozna toohello központ.

A [eszköz iker] [ lnk-device-twin] hello IoT-központ által kezelt JSON-dokumentum. Az eszközök ikereszköze a következőket tartalmazza:

- Hello eszköz toohello központ által küldött jelentésben szereplő tulajdonságok. Ezek a Tulajdonságok hello megoldás portálon tekintheti meg.
- Kívánt tulajdonságokkal, amelyet az toosend toohello eszköz. A tulajdonságok beállításához hello megoldás portálon.
- Csak a hello eszköz iker és hello eszközön nem létező címkék. A címkék toofilter listák eszközök hello megoldás portálon is használhatja.

A megoldás az eszköz twins toomanage eszköz metaadatait. hello megoldás is használ egy Cosmos DB adatbázis toostore további megoldás-specifikus eszköz adatok, például minden eszköz és hello parancselőzmények által támogatott hello parancsok.

hello megoldás is kell ne hello információk hello eszközidentitási hello tartalmát hello Cosmos DB adatbázis szinkronizálva. Hello **EventProcessorHost** használ hello adatait **deviceinfo információja** stream analytics-feladat toomanage hello szinkronizálás.

## <a name="solution-portal"></a>Megoldásportál

![megoldásportál][img-dashboard]

hello megoldás portál, a webes felhasználói felület, amely telepített toohello felhő előre konfigurált hello megoldás részeként. Ezzel a következőket teheti:

* Telemetria és riasztások előzményeinek megtekintése egy irányítópulton.
* Új eszközök kiépítése.
* Eszközök kezelése és figyelése.
* Parancsok toospecific eszközök küldése.
* Metódusok meghívása adott eszközökön.
* Szabályok és műveletek kezelése.
* Ütemezett feladatok toorun egy vagy több eszközön.

Előre konfigurált megoldásban hello megoldás portal hello részét képezi **IoT-megoldás háttérrendszerének** és hello része **feldolgozásra és üzleti kapcsolati** a tipikus hello [IoT-megoldás architektúra][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Következő lépések

További információ az IoT-megoldásarchitektúrákról: [Microsoft Azure IoT-szolgáltatások: referenciaarchitektúra][lnk-refarch].

Milyen előkonfigurált megoldás az eddig, elkezdheti hello üzembe helyezésével *távoli megfigyelési* előre konfigurált megoldás: [Ismerkedés az előre konfigurált hello megoldások] [ lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-getstarted-factory]: iot-suite-connected-factory-overview.md
