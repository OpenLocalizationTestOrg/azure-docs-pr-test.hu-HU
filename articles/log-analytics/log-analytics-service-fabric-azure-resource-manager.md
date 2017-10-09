---
title: "Service Fabric-alkalmazások Naplóelemzési használatával aaaAssess hello Azure-portálon |} Microsoft Docs"
description: "Naplóelemzési hello Azure portál tooassess hello kockázat és a Service Fabric-alkalmazások állapotát a hello Service Fabric megoldás is használhat micro-szolgáltatások, csomópontokat és fürtöket."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: nini
ms.openlocfilehash: 891c7f6e5ed511ac18599bdc280ab3dc09700fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-hello-azure-portal"></a>Mérje fel a Service Fabric-alkalmazások és micro-szolgáltatások hello Azure-portálon

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![A Service Fabric szimbólum](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

Ez a cikk ismerteti, hogyan toouse hello Naplóelemzési toohelp megoldás a Service Fabric azonosítása és elhárítása a Service Fabric-fürt között.

hello Service Fabric megoldás Azure diagnosztikai adatokat a Service Fabric virtuális gépek által az adatok gyűjtése az Azure ÜVEGVATTA táblát használ. A Naplóelemzési ezután beolvassa a Service Fabric keretrendszer eseményeihez, beleértve a **megbízható szolgáltatás események**, **szereplő események**, **működési eseményeit**, és **egyéni ETW-események**. A hello megoldás irányítópultja, esetén képes tooview jelentős problémák és kapcsolódó eseményeket a Service Fabric-környezetben.

tooconnect szüksége tooget hello megoldás használatába, a Service Fabric fürt tooa Naplóelemzési munkaterületet. Az alábbiakban a három forgatókönyvek tooconsider:

1. Ha nem telepítette a Service Fabric-fürt, lépésekkel hello a ***központi telepítése a Service Fabric-fürt csatlakoztatott tooa Naplóelemzési munkaterület*** toodeploy új fürt, és hozzá beállítva tooreport tooLog elemzés.
2. Ha toocollect teljesítményszámlálók a gazdagépek toouse a más OMS megoldások, például biztonsági meg a Service Fabric-fürt, kövesse hello ***tartalmazó csatlakozott a Service Fabric-fürt tooa Naplóelemzési munkaterület Virtuálisgép-bővítmény telepítése telepítve.***
3. Ha már telepítette a Service Fabric-fürt, és szeretné, hogy tooconnect azt tooLog elemzés, kövesse hello ***hozzáadása egy meglévő storage-fiók tooLog elemzés.***

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace"></a>A Service Fabric-fürt csatlakoztatott tooa Naplóelemzési munkaterület központi telepítése.
Ez a sablon hello a következő:

1. Az Azure Service Fabric fürt már csatlakozott tooa Naplóelemzési munkaterület telepíti. Lehetősége van hello beállítás toocreate új munkaterület hello sablont, vagy egy már meglévő Naplóelemzési munkaterület bemeneti hello neve telepítése közben.
2. Hello diagnosztikai tárolási fiók toohello Naplóelemzési munkaterület hozzáadja.
3. Lehetővé teszi, hogy a Service Fabric megoldás hello Naplóelemzési munkaterületet.

[![TooAzure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

Miután hello gombot, a rendszer hello Azure megnyílik az Ön tooedit paraméterekkel. Lehet, hogy toocreate egy új erőforráscsoportot Ha, adjon meg egy új nevet a Naplóelemzési munkaterület:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Fogadja el a hello jogi feltételeket, majd kattintson a **létrehozása** toostart hello központi telepítés. Ha hello telepítés befejeződött, inkább hello új munkaterületet, és a fürt létrehozása, és hello WADServiceFabric * hozzáadott esemény, WADWindowsEventLogs és WADETWEvent táblák:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace-with-vm-extension-installed"></a>Telepítse a Service Fabric-fürt csatlakoztatott tooa Naplóelemzési munkaterület telepített Virtuálisgép-bővítménnyel.

Ez a sablon hello a következő:

1. Az Azure Service Fabric fürt már csatlakozott tooa Naplóelemzési munkaterület telepíti. Hozzon létre egy új munkaterületet, vagy használjon egy meglévőt.
2. Hello diagnosztikai tárolási fiókok toohello Naplóelemzési munkaterület hozzáadja.
3. Lehetővé teszi, hogy a Service Fabric megoldás hello hello Naplóelemzési munkaterület.
4. Állítsa be a Service Fabric-fürt minden egyes virtuálisgép-méretezési hello MMA ügynök bővítményt telepít. Hello MMA ügynök van telepítve készül, amelyek képesek tooview teljesítménymutatók a csomópontokat.

[![TooAzure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

Következő hello bemeneti hello szükséges paraméterek a fenti lépéseket, és a központi telepítés indítsa. Még egyszer kell megjelennie hello új munkaterületet, fürt, vagy az összes létrehozott ÜVEGVATTA táblák:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Teljesítményadatok

Teljesítményadatok tooview a csomópontjáról:


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- Indítsa el a Naplóelemzési munkaterület hello hello Azure-portálon.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Nyissa meg tooSettings hello bal oldali ablaktáblán, és válassza ki az adatok >> Windows teljesítményszámlálók >> "Add hello kijelölt teljesítményszámlálók": ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- A keresési napló azokat a csomópontokat kapcsolatos alapvető metrikákat a lekérdezések toodelve következő hello használata:

    a. Hasonlítsa össze a processzorkihasználtság átlagosan hello hello csomópontok problémát tapasztal elmúlt egy óra toosee minden a csomópontra, és milyen időközönként csomópont kellett egy csúcs:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Megtekintheti a rendelkezésre álló memória hasonló vonaldiagramok lekérdezéshez minden egyes csomóponton:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    az összes a csomópont hello pontos átlagos értéket megjelenítő a rendelkezésre álló memória (MB) az egyes csomópontok listáját tooview használja ezt a lekérdezést:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. Hello esetében, hogy a kívánt toodrill le egy adott csomópont hello óránkénti átlag, megvizsgálásával minimális, maximális és 75-PERCENTILIS CPU-használat, Ön tudja toodo ez által a lekérdezés (a név felülírandó számítógép mező) használata:

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

További információt szeretne teljesítmény metrikákat a Log Analyticshez hello [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-toolog-analytics"></a>Egy meglévő storage-fiók tooLog Analytics hozzáadása

Ez a sablon egyszerűen hozzáadja a meglévő tárolási fiókok tooa új vagy meglévő Naplóelemzési munkaterületet.

[![TooAzure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Csoport kiválasztása egy erőforrást, ha egy már meglévő Naplóelemzési munkaterület dolgozik, válassza ki "Használata meglévő" és a keresési hello hello Naplóelemzési munkaterületet tartalmazó erőforráscsoport. Hozzon létre egy új egy egyébként.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

Ez a sablon telepítése után fog tudni toosee hello storage-fiók csatlakoztatva tooyour Naplóelemzési munkaterület. Ebben a példában egy további tárolási fiók toohello Exchange munkaterület fent létrehozott hozzáadott.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>A Service Fabric-események megtekintése

Miután hello központi telepítések és a Service Fabric megoldás hello engedélyezve van a munkaterületen, válassza ki a hello **Service Fabric** hello Log Analytics portál toolaunch hello Service Fabric dashboard csempére. hello irányítópult szerepel a következő táblázat hello hello oszlopa. Egyes oszlopok hello felső 10 események száma a hozzá, hogy az oszlop feltételek hello megadott időtartomány sorolja fel. Futtathatja a napló keresési kattintva hello teljes lista biztosító **láthatja az összes** hello jobb alsó oszlopok, vagy hello oszlop fejlécére kattint.

| **A Service Fabric-esemény** | **Leírás** |
| --- | --- |
| Jelentős problémák |Megjeleníti a problémák, például RunAsyncFailures RunAsynCancellations és csomópont időszakosan megszakadó. |
| A működési események |Például az alkalmazásfrissítés és központi telepítések figyelmet a jelentősebb működési eseményeit. |
| Megbízható eseményei |Fontos megbízható szolgáltatás események ilyen Runasyncinvocations. |
| Szereplő események |Fontos szereplő eseményeit a micro-szolgáltatások, például egy aktormetódus, szereplő aktiválások és deactivations, és így tovább által okozott kivételeket. |
| Alkalmazás-események |Minden egyéni ETW által előállított eseményeket az alkalmazások. |

![A Service Fabric irányítópult](./media/log-analytics-service-fabric/sf3.png)

![A Service Fabric irányítópult](./media/log-analytics-service-fabric/sf4.png)

hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan Service Fabric részleteit.

| Platform | Közvetlen ügynök | Operations Manager-ügynök | Azure Storage | Az Operations Manager szükséges? | Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 perc |

> [!NOTE]
> Ezek az események a Service Fabric megoldás hello hello hatókör kattintva módosíthat **adatok alapján az elmúlt 7 napban** hello irányítópult hello tetején. Elmúlt hét napban, egy nap vagy hat órán belül hello generált események akkor is megjelenhet. Vagy választhat **egyéni** toospecify egyéni dátumtartományt.
>
>

## <a name="next-steps"></a>Következő lépések

* Használjon [napló megkeresi a Naplóelemzési](log-analytics-log-searches.md) tooview részletes Service Fabric eseményadatok.
