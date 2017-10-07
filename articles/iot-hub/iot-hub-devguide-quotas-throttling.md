---
title: "aaaUnderstand Azure IoT Hub kvóták és sávszélesség-szabályozási |} Microsoft Docs"
description: "Fejlesztői útmutató - hello kvóták tooIoT Hub és hello vonatkozó leírása szabályozási viselkedés várható."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 023fa29bfbfb1de35708d6d121a1c56b50adfed9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referencia - IoT-központ kvóták és sávszélesség-szabályozás

## <a name="quotas-and-throttling"></a>Kvóták és szabályozás
Minden Azure-előfizetéssel, legfeljebb 10 IoT-központok, és legfeljebb 1 szabad hub rendelkezhet.

Minden egyes IoT-központot egy adott SKU egység bizonyos számú ki van építve (további információkért lásd: [Azure IoT Hub árképzési][lnk-pricing]). hello SKU és egységek száma határozza meg, hello maximális napi beállított kvótát küldhet üzeneteket.

hello SKU határozza meg a sávszélesség-szabályozás korlátozza az összes művelet kikényszeríti az IoT-központ hello is.

## <a name="operation-throttles"></a>A művelet szabályozások
A művelet szabályozások hello percben tartományok lesznek alkalmazva, és sebessége korlátozások is tooavoid visszaélés készült. Az IoT-központ megpróbál tooavoid visszaadó hibák, amikor csak lehetséges, de kivételek ad vissza, ha túl sokáig hello késleltetési sérül kezdődik.

a következő táblázat azt mutatja be hello hello szabályozások lépnek érvénybe. Értékek tooan egyedi hub hivatkozik.

| Szabályozás | Szabad és S1 hubok | S2 hubok | S3 hubok | 
| -------- | ------- | ------- | ------- |
| Identitás kapcsolatos műveletek (létrehozása, beolvasása, listázása, frissítése és törlése) | 1.67/sec/Unit (100/perc/egység) | 1.67/sec/Unit (100/perc/egység) | 83.33/sec/Unit (5000/perc/egység) |
| Eszközkapcsolatok | Legfeljebb 100 másodpercenkénti vagy 12/mp/egység <br/> Például két S1 egység: 2\*12 = 24 vagy mp, de legalább 100/mp a egységek között. Kilenc S1 egység, az informatikai részleg 108/mp (9\*12) között a egységeket. | 120/mp/egység | 6000/mp/egység |
| Az eszközről a felhőbe irányuló küldések | Legfeljebb 100 másodpercenkénti vagy 12/mp/egység <br/> Például két S1 egység: 2\*12 = 24 vagy mp, de legalább 100/mp a egységek között. Kilenc S1 egység, az informatikai részleg 108/mp (9\*12) között a egységeket. | 120/mp/egység | 6000/mp/egység |
| Küldések a felhőből az eszközökre | 1.67/sec/Unit (100/perc/egység) | 1.67/sec/Unit (100/perc/egység) | 83.33/sec/Unit (5000/perc/egység) |
| Fogadások a felhőből az eszközökön <br/> (csak ha eszköz HTTP Protokollt használ)| 16.67/sec/Unit (1000/perc/egység) | 16.67/sec/Unit (1000/perc/egység) | 833.33/sec/Unit (50000/perc/egység) |
| Fájl feltöltése | értesítések/mp/egység 1.67 fájl feltöltése (100/perc/egység) | értesítések/mp/egység 1.67 fájl feltöltése (100/perc/egység) | értesítések/mp/egység 83.33 fájl feltöltése (5000/perc/egység) |
| Közvetlen metódusok | 20/mp/egység | 60 másodperc/egységenként | 3000/mp/egység | 
| Ikereszköz-olvasások | 10/mp | Legfeljebb 10 másodpercenként vagy 1/mp/egység | 50/mp/egység |
| Ikereszköz-frissítések | 10/mp | Legfeljebb 10 másodpercenként vagy 1/mp/egység | 50/mp/egység |
| Feladatműveletek <br/> (létrehozás, frissítés, listázás, törlés) | 1.67/sec/Unit (100/perc/egység) | 1.67/sec/Unit (100/perc/egység) | 83.33/sec/Unit (5000/perc/egység) |
| Feladatok eszközönkénti műveleti teljesítménye | 10/mp | Legfeljebb 10 másodpercenként vagy 1/mp/egység | 50/mp/egység |

Fontos, hogy hello tooclarify *eszközcsatlakozás* késleltetési irányító hello sebesség, amellyel új eszköz kapcsolatok hozhatók létre és az IoT-központ. Hello *eszközcsatlakozás* késleltetési nem szabályozza a hello egyidejűleg csatlakoztatott eszközök maximális számát. hello adatátviteli egység az IoT-központ hello kiépített hello száma függ.

Például ha vásárol S1 egyetlen egységben, kap egy 100-kapcsolatok a késleltetési. Ezért tooconnect 100 000 eszközt, hogy másodpercet vesz igénybe legalább 1000 (körülbelül 16 perc). Rendelkezik a identitásjegyzékhez-ben regisztrált eszközök annyi egyidejűleg csatlakoztatott eszközök azonban akkor is.

Az IoT-központot egy részletes ismertető viselkedését, szabályozási hello ismertető blogbejegyzésben talál [IoT Hub-szabályozás és][lnk-throttle-blog].

> [!NOTE]
> Lehetséges tooincrease kvóták egy adott időpontban vagy a sávszélesség-szabályozási korlátok kiosztott egység az IoT-központ hello számának növelésével.
> 
> [!IMPORTANT]
> Identitás kapcsolatos műveletek az eszközkezelés és a üzembe helyezési forgatókönyvek futásidejű használatra lettek tervezve. Keresztül támogatja a olvasása vagy frissítése nagyszámú eszköz identitások [importálni és exportálni a feladatokat][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Egyéb korlátozások

Az IoT-központ érvényesíti a más működési korlátai:

| Művelet | Korlát |
| --------- | ----- |
| Fájl feltöltése URI-azonosítók | SAS URI-azonosítók 10000 lehet ki egy tárfiók egy időben. <br/> Eszközönként egyszerre 10 SAS URI lehet használatban. |
| Feladatok | Feladat előzményei őrződnek meg too30 nap mentése <br/> Egyidejű feladatok maximális értéke 1 (szabad és S1, (az S2) 5, 10 (S3). |
| További végpontok | Termékváltozat fizetős hubok 10 további végpontokat is rendelkezhet. Szabad SKU hubok előfordulhat, hogy egy további végpontot. |
| Üzenet útválasztási szabályokat | Termékváltozat fizetős hubok 100 útválasztási szabályokat is rendelkezhet. Előfordulhat, hogy az ingyenes SKU hubok öt útválasztási szabályokat. |
| Üzenetküldési eszközről a felhőbe | Maximális üzenet mérete 256 KB |
| Felhő eszközre üzenetkezelés | Maximális méret 64 KB |
| Felhő eszközre üzenetkezelés | Függőben levő üzenetek a szállítási maximális érték 50 |

> [!NOTE]
> Jelenleg a maximális szám hello tooa egyetlen IoT-központ eszközökön is elérheti az 500 000 értéket. Ha azt szeretné, tooincrease ezt a határt, forduljon a [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Késés
Az IoT-központ nagy hangsúlyt fektet tooprovide kis késleltetésű minden műveletnél. Azonban toonetwork feltételek és egyéb előre nem látható tényezők miatt azt a maximális késés nem garantálja. A megoldás tervezésekor a következőket:

* Ne bármely hello maximális késleltetés feltételezéseket minden IoT Hub művelet elvégzése.
* Az IoT hub hello Azure-régiót legközelebbi tooyour eszközökön használhatók.
* Érdemes lehet Azure IoT peremhálózati tooperform késésérzékeny műveletek hello eszközön vagy az átjáró Bezárás toohello eszközön.

Több IoT Hub-egységek befolyásolja a sávszélesség-szabályozás korábban leírt, de nem nyújtanak semmilyen további késés előnyöket vagy garanciát.
Ha nem várt növekszik művelet késés jelenik meg, forduljon a [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Következő lépések
Az IoT Hub fejlesztői útmutató egyéb témaköröket tartalmazza:

* [IoT-központok végpontjai][lnk-devguide-endpoints]
* [Az IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás][lnk-devguide-query]
* [Az IoT Hub MQTT támogatása][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
