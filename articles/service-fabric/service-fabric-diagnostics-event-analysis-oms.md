---
title: "Service Fabric esemény elemzésekről az OMS aaaAzure |} Microsoft Docs"
description: "Információ megjelenítése és eseményeket OMS figyelési és az Azure Service Fabric-fürtök diagnosztika elemzése."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 526519293e70982c95e31241465b87f190096f74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Esemény elemzése és az OMS képi megjelenítés

Az Operations Management Suite (OMS) hello felhőben üzemeltetett szolgáltatások és szolgáltatások figyelése és az alkalmazások diagnosztika segítenek gyűjteménye. tooget részletes áttekintése OMS, és mi kínál, olvassa el [Mi az az OMS?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-hello-oms-workspace"></a>Naplófájl elemzés és hello OMS-munkaterület

Naplóelemzési kezelt erőforrások, például egy Azure storage tábla vagy az ügynök gyűjti az adatokat, és egy központi tárházban megőrzi azt. az elemzés, a riasztás és a képi megjelenítés, vagy a használt további exportáló majd lehet az hello adatokat. A Naplóelemzési támogatja az eseményeket, teljesítményadatokat vagy egyéb egyéni adatokat.

Ha OMS van konfigurálva, hogy hozzáférési tooa specifikus *OMS-munkaterület*, ahol lekérdezése vagy az irányítópultokon ábrázolt a adatokat.

Naplóelemzési fogadja a adatokat, miután a OMS rendelkezik, több *megoldások* , amelyek testreszabott tooseveral forgatókönyvek előre csomagolt megoldások toomonitor bejövő adatok. Ezek közé tartozik egy *Service Fabric Analytics* megoldás és a *tárolók* megoldást, amely két legfontosabb ők hello toodiagnostics és figyelése a Service Fabric-fürtök használata esetén. Léteznek számos egyéb is, amelyek érdemes felfedezése, és az OMS egyéni megoldások, amelyek olvashat további tudnivalókat hello létrehozását is lehetővé teszi [Itt](../operations-management-suite/operations-management-suite-solutions.md). Egyes toouse válassza ki, a fürt lesz konfigurálva hello megoldások azonos OMS-munkaterület Naplóelemzési mellett. Egyéni irányítópultok és a képi megjelenítés adatainak munkaterületek lehetővé, és azt szeretné, hogy toocollect, módosítások toohello adatok feldolgozásához, és elemezze.

## <a name="setting-up-an-oms-workspace-with-hello-service-fabric-solution"></a>A Service Fabric megoldás hello az OMS-munkaterület beállítása

Javasoljuk, hogy a Service Fabric megoldás hello szerepeljenek az OMS-munkaterület, mivel hasznos irányítópult biztosít, amely mutatja hello hello platform és az alkalmazás szintje és hello képes tooquery Service Fabric különböző bejövő naplócsatornák adott naplózza. Íme egy viszonylag egyszerű Service Fabric megoldás néz, hello fürtön telepítve egyetlen alkalmazással:

![OMS ú megoldás](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Két módon tooprovision van, és konfigurálja az OMS-munkaterület, a Resource Manager-sablon vagy közvetlenül az Azure piactérről. Hello volt akkor használja, ha telepít egy fürtöt, és ez utóbbi, ha már rendelkezik egy fürt üzembe helyezéséhez diagnosztika hello engedélyezve van.

### <a name="deploying-oms-using-a-resource-management-template"></a>Erőforrás-kezelés sablon használatával OMS telepítése

Ez akkor történik meg hello fürt létrehozása szakaszban,-üzembe helyezése a Resource Manager-sablon, hello sablon egy fürtöt is létrehozhat egy új OMS-munkaterület, amikor hello Service Fabric megoldás tooit hozzáadása, és konfigurálja hello megfelelő tárolási tooread adatait táblák.

>[!NOTE]
>A toowork diagnosztika rendelkezik ahhoz, hogy az Azure storage-táblákat tooexist hello OMS engedélyezve toobe / Log Analyticshez tooread adatait a.

[Itt](https://azure.microsoft.com/resources/templates/service-fabric-oms/) egy minta sablon használ, és módosítsa megfelelően követelmény, amely a fenti műveleteket hajt végre. Hello esetében, hogy azt szeretné, hogy több lehetőség, néhány további sablonok, amelyek különböző beállítások attól függően, hogy hol hello folyamat is, hogyan kell beállítani az OMS-munkaterület - helyen találhatók [Service Fabric és OMS sablonok](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Központi telepítés OMS segítségével az Azure piactéren keresztül

Miután telepítette a fürt egy OMS-munkaterület tooadd tetszés szerint tooAzure piactér lépjen, és keresse meg *"Service Fabric Analytics"*. Csak akkor kell egy erőforrást, amely mutatja be, hello "Figyelési + felügyelet" kategóriába, alább látható módon:

![A piactéren OMS ú elemzés](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

Kattintson a **létrehozása** kérni fogja az OMS-munkaterület. Kattintson a **munkaterület kiválasztása** , majd **hozzon létre egy új munkaterületet**. Töltse ki a szükséges hello bejegyzések – a hello itt egyetlen követelménye hello Service Fabric-fürt és hello OMS-munkaterület kell hello előfizetés hello azonos. Ha a bejegyzések érvényesítése az OMS-munkaterület telepíti, néhány perc múlva. A Befejezés után üzembe helyezése közben hello Service Fabric megoldás panel hello létrehozása továbbra is nyitva marad. Győződjön meg arról, hogy az azonos munkaterület-jön létre hello *OMS-munkaterület* és találati **létrehozása** hello alján tooadd hello Service Fabric megoldás toohello munkaterületen.

## <a name="using-hello-oms-agent"></a>Hello OMS-ügynököt használatával

Ajánlott toouse EventFlow és ÜVEGVATTA összesítési megoldások, mivel lehetővé teszik a több moduláris megközelítés toodiagnostics és figyelését. Például ha azt szeretné, toochange EventFlow a kimeneteinek, igényel nincs változás tooyour tényleges instrumentation, csak egy egyszerű módosítása tooyour konfigurációs fájlt. Ha azonban tooinvest OMS használata mellett dönt, és a rendszer használná esemény elemzése toocontinue (nincs toobe hello csak platformot használ, de ahelyett, hogy, hogy az informatikai hello platformok közül legalább egy), javasoljuk, hogy hello beállításatanulmányoznia[</C0>OMS-ügynököt<spanclass="notranslate">](../log-analytics/log-analytics-windows-agents.md).</span> Is használjon hello OMS-ügynököt tárolók tooyour fürt, telepítésekor, az alábbiak ismertetik.

hello folyamat mindezt alkalmazása viszonylag könnyű, mivel csak rendelkezik tooadd hello ügynök egy virtuálisgép-méretezési állíthat be, bővítmény tooyour Resource Manager-sablon, győződjön meg arról, hogy megkapja-e telepítve minden egyes csomópontján. Rendszert futtató fürtöket, amelyek a Service Fabric megoldás (lásd fent) hello hello OMS-munkaterület telepíti, és hello ügynök tooyour csomópontok minta Resource Manager sablon található [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) vagy [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

hello előnyei ez hello következők tartoznak:

* Bővebb információ a hello teljesítmény-számlálókból és a metrikák oldalon
* Könnyen tooconfigure hello az összegyűjtött adatokat a fürt és módosítások tooit ellenőrizze az alkalmazások üzembe helyezésével vagy hello fürthöz, mert a módosítások toohello beállításainak hello ügynök hello OMS-munkaterület között elvégezhető, és csak fog hello ügynök alaphelyzetbe állítása automatikusan. tooconfigure hello OMS ügynök toopick egyes teljesítményszámlálókat, nyissa meg toohello munkaterület mentése **Kezdőlap > Beállítások > adatok > Windows-teljesítményszámlálók** , válassza ki a hello adatokat gyűjtött toosee szeretné használni
* Adatok gyorsabb, mint az OMS alatt észlelnie előtt tárolt toobe rendelkező mutatja / Log Analyticshez
* Sokkal egyszerűbb, tárolók figyelése azért, mert azt átvételéhez docker-naplók (stdout, stderror) és a statisztikák (teljesítménymutatók tároló-és csomópont)

hello Itt a fő szempont, hogy, mert az ügynök, annak telepítése mellett az alkalmazások, a fürtön, hogy az alkalmazások hello fürtön néhány gyakorolt minimális hatás mellett toohello teljesítményének történjen.

## <a name="monitoring-containers"></a>Tárolók figyelése

Tárolók tooa Service Fabric-fürt telepítésekor ajánlott, hogy hello fürt beállítása a hello OMS-ügynököt, és adott hello tárolók megoldás tooyour OMS munkaterület tooenable figyelése és diagnosztikai bővült. Milyen hello tárolók megoldás a következőképpen néz munkaterület a következő:

![Alapszintű OMS irányítópult](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

hello ügynök lehetővé teszi, hogy több tároló-specifikus naplók lekérhetők az OMS Szolgáltatáshoz, vagy toovisualized teljesítménymutatók hello gyűjteménye. hello napló típusok tartoznak a következők:

* ContainerInventory: tároló helye, a nevét, és a képeket információkat jeleníti meg.
* ContainerImageInventory: információ a központilag telepített lemezképek, többek között az azonosítók vagy mérete
* ContainerLog: más bejegyzések, adott hibanaplókat és a docker-naplók (stdout, stb.)
* ContainerServiceLog: docker démon parancsok futtatása
* Teljesítmény: teljesítményszámlálókat, beleértve a tároló processzor, memória, a hálózati forgalom, lemez i/o, és egyéni metrikák hello a gépek üzemeltetéséhez.

Ez a cikk ismerteti a hello lépéseket szükséges tooset fel a fürt tárolófigyelő. További információk az OMS Szolgáltatáshoz tartozó tárolók megoldás, toolearn tekintse meg a [dokumentáció](../log-analytics/log-analytics-containers.md).

tooset mentése hello tároló megoldás a munkaterületen ellenőrizze, hogy a fent említett hello lépéseket követve a fürtcsomópontokon telepített hello OMS-ügynököt. Ha hello fürt készen áll, telepítsen egy tároló tooit. Vegye figyelembe, hogy első alkalommal a tároló lemezkép hello rendelkeznek telepített tooa fürt, néhány perc toodownload hello kép attól függően, hogy annak méretét vesz igénybe.

A Azure piactérről, keresse meg a *tárolók* és tárolók erőforrás létrehozása (a hello figyelés + felügyeleti kategória).

![Tárolók megoldás hozzáadása](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

Hello létrehozását lépést, a kérelmek az OMS-munkaterület. Válassza ki a hello valamelyik fenti hello telepítési hoztak létre. Ezt a lépést hozzáadja az OMS-munkaterület belül tárolók megoldást, és automatikusan felismeri hello sablon által telepített hello OMS-ügynököt. hello ügynök megkezdődik az adatgyűjtés hello tárolók folyamatokon hello fürtöt és kevesebb mint 15 percet, vagy Igen, megtekintheti az hello megoldás könnyű fel adatokkal, ahogy a fenti hello irányítópult hello képe.


## <a name="next-steps"></a>Következő lépések

A következő eszközök és beállítások a munkaterület tooyour kell toocustomize OMS hello ismerheti meg:

* A helyi fürthöz OMS kínál, amelyek lehetnek használt toosend adatok tooOMS átjáró (http-továbbítás Proxy). További tájékoztatást talál, amely a [számítógépek kapcsolódásához nélkül Internet access tooOMS használatával hello OMS-átjáró](../log-analytics/log-analytics-oms-gateway.md)
* Konfigurálja az OMS tooset mentése [riasztás automatikus](../log-analytics/log-analytics-alerts.md) tooaid észlelésére és diagnosztika
* Hello az beszerzése familiarized [naplófájl keresési és lekérdezése](../log-analytics/log-analytics-log-searches.md) szolgáltatásai által kínált Naplóelemzési
