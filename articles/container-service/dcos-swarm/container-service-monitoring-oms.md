---
title: "aaaMonitor Azure DC/OS-fürtről - műveletek kezelése |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS fürtben, a Microsoft Operations Management Suite figyelése."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 0ebfa3ba3cef8f0205b15731b0e91f5b304bc8fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a>Egy Azure tároló szolgáltatás DC/OS fürtben, az Operations Management Suite figyelése

A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében. Tároló egy olyan megoldás, az OMS szolgáltatáshoz, így a nézet hello tároló szoftverleltár, a teljesítmény és a naplók egyetlen helyen. Naplózási, tárolók hibaelhárításáról hello naplók megtekintése központi helyen, és zajos fel felesleges tároló egy gazdagépen található.

![](media/container-service-monitoring-oms/image1.png)

A tároló megoldásról további információkért tekintse meg az toothe [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-hello-dcos-universe"></a>A DC/OS universe hello OMS beállítása


Ez a cikk feltételezi, hogy a DC/OS beállítását, és egyszerű webes tároló alkalmazások hello fürtön telepített.

### <a name="pre-requisite"></a>Előfeltétel
- [A Microsoft Azure-előfizetés](https://azure.microsoft.com/free/) -szabad szerezni ez.  
- Microsoft OMS-munkaterület beállítása - lásd a "3. lépés" alatt
- [DC/OS parancssori felület](https://dcos.io/docs/1.8/usage/cli/install/) telepítve.

1. Hello DC/OS-irányítópultot kattintson a Universe, és keressen a "OMS" alább látható módon.

![](media/container-service-monitoring-oms/image2.png)

2. Kattintson az **Install** (Telepítés) gombra. Látni fogja a pop mentése hello OMS fájlverzió-információkat, és egy **csomagtelepítés** vagy **speciális telepítési** gombra. Amikor rákattint **speciális telepítési**, amely részletes útmutatást toohello **OMS konfigurációs tulajdonságok** lap.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Itt, meg kell adnia tooenter hello `wsid` (hello OMS-munkaterület azonosítója) és `wskey` (hello OMS elsődleges kulcsát hello munkaterület azonosítója). mindkét tooget `wsid` és `wskey` OMS fiók kell toocreate <https://mms.microsoft.com>. Kövesse az hello lépéseket toocreate fiókkal. Miután létrehozása hello fiókja, szüksége tooobtain a `wsid` és `wskey` kattintva **beállítások**, majd **csatlakoztatott források**, majd **Linux-kiszolgálókon** lent látható módon.

 ![](media/container-service-monitoring-oms/image5.png)

4. Hello szám, OMS-példányok kiválasztása, hogy szeretné, hogy és hello áttekintése és telepítése gombra. Általában érdemes OMS példányok egyenlő toohello számát a virtuális gép van az ügynök fürt toohave hello száma. Linux OMS-ügynököt telepíti minden egyes virtuális gépen, hogy szeretnének toocollect figyelés és naplózás adatokat egyes tárolóként.

## <a name="setting-up-a-simple-oms-dashboard"></a>Egy egyszerű OMS irányítópult beállítása

Hello virtuális gépeken Linux hello OMS-ügynök telepítése után a következő lépésre tooset hello OMS irányítópult mentése. Nincsenek két módon toodo ez: OMS-portálon vagy az Azure portálon.

### <a name="oms-portal"></a>OMS-portálon 

Jelentkezzen be az OMS-portálon toohello (<https://mms.microsoft.com>), és toohello **megoldás gyűjteménye**.

![](media/container-service-monitoring-oms/image6.png)

Miután belépett hello **megoldás gyűjtemény**, jelölje be **tárolók**.

![](media/container-service-monitoring-oms/image7.png)

Hello tároló megoldás kijelölt hello OMS áttekintése irányítópult-oldalon csempét hello jelenik meg. Miután egy indexelt okozhatnak hello lévő adatokhoz, megjelenik a megoldás nézet csempék adatokkal feltöltve hello csempe.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure Portal 

Bejelentkezési tooAzure portálon, a <https://portal.microsoft.com/>. Nyissa meg a **piactér**, jelölje be **figyelés + felügyeleti** kattintson **tekintse meg az összes**. Írja be `containers` keresésben. Látni fogja a "tárolók" hello keresési eredmények között. Válassza ki **tárolók** kattintson **létrehozása**.

![](media/container-service-monitoring-oms/image9.png)

Miután rákattintott **létrehozása**, akkor kérni fogja a munkaterületen. A munkaterület kiválasztása, vagy ha nem rendelkezik ilyennel, hozzon létre egy új munkaterületet.

![](media/container-service-monitoring-oms/image10.PNG)

A munkaterület kijelölése után kattintson **létrehozása**.

![](media/container-service-monitoring-oms/image11.png)

A hello OMS tároló megoldás kapcsolatos további információkért tekintse meg az toothe [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).

### <a name="how-tooscale-oms-agent-with-acs-dcos"></a>Hogyan tooscale OMS-ügynököt a DC/OS ACS 

Abban az esetben telepítve toohave OMS-ügynököt hello tényleges csomópontok száma kevés van szüksége, vagy adja hozzá a további VM skálázás be VMSS, ehhez hello skálázással `msoms` szolgáltatás.

Nyissa meg tooMarathon vagy hello DC/OS felhasználói felületének (szolgáltatások) lapján, és növelheti a csomópontok száma.

![](media/container-service-monitoring-oms/image12.PNG)

Ezzel telepít tooother csomópontokat, amelyeknek hello OMS-ügynök még nincs telepítve.

## <a name="uninstall-ms-oms"></a>MS OMS eltávolítása

MS OMS toouninstall adja meg a következő parancs hello:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Ossza meg velünk!!!
Mi működik? Mi az a hiányzó? Milyen hiba van szüksége a toobe a hasznos meg? Ossza meg velünk <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Következő lépések

 Most, hogy meg van adva OMS toomonitor a tárolók[tekintse meg a tároló irányítópult](../../log-analytics/log-analytics-containers.md).
