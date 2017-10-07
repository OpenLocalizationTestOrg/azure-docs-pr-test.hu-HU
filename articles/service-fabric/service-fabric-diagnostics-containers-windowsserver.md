---
title: "aaaAzure Service Fabric tárolók figyelése és diagnosztikai |} Microsoft Docs"
description: "Megtudhatja, hogyan toomonitor és diagnosztizálhatja az OMS Szolgáltatáshoz tartozó tárolók megoldással a Microsoft Azure Service Fabric összehangolva tárolók."
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
ms.date: 05/10/2017
ms.author: dekapur
ms.openlocfilehash: cd79111cf78b9d76a60d489bb9953587aa06186d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-windows-server-containers-with-oms"></a>Windows Server OMS tárolók figyelése

## <a name="oms-containers-solution"></a>OMS-tárolók megoldás

hello Operations Management Suite (OMS) csapat tárolók megoldást a diagnosztika és a tárolók figyelésének tett közzé. A Service Fabric megoldás mellett ez a megoldás egy remek eszköz toomonitor üzemelő tárolópéldányokat a Service Fabric összehangolva. Íme egy egyszerű példa milyen hello irányítópult hello megoldás a következőhöz hasonló:

![Alapszintű OMS irányítópult](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Is gyűjti a különböző naplófájlokat, amelyek lekérhetők az hello OMS Naplóelemzési eszközt, és bármilyen metrikák vagy eseményeit éppen használt toovisualize lehet. hello napló típusok tartoznak a következők:

1. ContainerInventory: tároló helye, a nevét, és a képeket információkat jeleníti meg.
2. ContainerImageInventory: információ a központilag telepített lemezképek, többek között az azonosítók vagy mérete
3. ContainerLog: más bejegyzések, adott hibanaplókat és a docker-naplók (stdout, stb.)
4. ContainerServiceLog: docker démon parancsok futtatása
5. Teljesítmény: teljesítményszámlálókat, beleértve a tároló processzor, memória, a hálózati forgalom, lemez i/o, és egyéni metrikák hello a gépek üzemeltetéséhez.

Ez a cikk ismerteti a hello lépéseket szükséges tooset fel a fürt tárolófigyelő. További információk az OMS Szolgáltatáshoz tartozó tárolók megoldás, toolearn tekintse meg a [dokumentáció](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. A Service Fabric-fürt beállítása

Hozzon létre egy fürtöt hello Azure Resource Manager sablon található [Itt](https://github.com/dkkapur/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Sample). Győződjön meg arról, hogy tooadd egyedi OMS-munkaterület nevét. Ez a sablon is alapértelmezés szerint a toodeploying hello preview a fürt létrehozása a Service Fabric (v255.255), ami azt jelenti, hogy éles környezetben nem használható, és nem lehet frissíteni a tooa különböző Service Fabric-verzió. Ha úgy dönt, toouse a sablon hosszú távú vagy éles használatához módosítsa hello verziószáma tooa stabil verzióját.

Ha hello fürt készen áll, győződjön meg arról, hogy telepítette a hello megfelelő tanúsítványt, és győződjön meg arról, hogy képes tooconnect toohello fürt.

Győződjön meg arról, hogy az erőforráscsoport helyesen van beállítva által címsor toohello [Azure-portálon](https://portal.azure.com/) és hello telepítési keresése. hello erőforráscsoport tartalmazza az összes hello szolgáltatást a háló típusú erőforrások, és Naplóelemzési megoldást, valamint a Service Fabric megoldás is rendelkezik.

A meglévő Service Fabric-fürt módosítását:
* Győződjön meg arról, hogy engedélyezve van-e a Diagnostics (Ha nem, engedélyezze azt [hello virtuálisgép-méretezési csoport frissítése](/rest/api/virtualmachinescalesets/create-or-update-a-set))
* Adja hozzá az OMS-munkaterület hozzon létre egy "A Service Fabric Analytics" megoldás a hello Azure piactéren
* Az erőforráscsoport, amely a fürt hello van hello hello adatforrását hello Service Fabric megoldás toopick hello (által létrehozott ÜVEGVATTA) megfelelő Azure tárolási táblákból származó adatok szerkesztése
* Adja hozzá a hello ügynököt egy [bővítmény toohello virtuálisgép-méretezési csoport](/powershell/module/azurerm.compute/add-azurermvmssextension) PowerShell vagy a frissítési hello virtuális gép méretezési (a fenti ugyanazon kapcsolathoz, toomodify hello Resource Manager-sablon)

## <a name="2-deploy-a-container"></a>2. A tároló üzembe

Miután hello fürt készen áll, és meggyőződött róla, hogy hozzá tud férni, telepítsen egy tároló tooit. Hello sablon által beállított választása toouse hello előzetes verzióját, is megismerheti a Service Fabric új docker compose funkciót. Vegye figyelembe, hogy első alkalommal a tároló lemezkép hello rendelkeznek telepített tooa fürt, néhány perc toodownload hello kép attól függően, hogy annak méretét vesz igénybe.

## <a name="3-add-hello-containers-solution"></a>3. Hello tárolók megoldás hozzáadása

A hello Azure-portálon, és hozzon létre egy tároló-erőforrás (alatt hello figyelés + felügyeleti kategória) Azure piactéren keresztül. 

![Tárolók megoldás hozzáadása](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

Hello létrehozását lépést, a kérelmek az OMS-munkaterület. Válassza ki a hello valamelyik fenti hello telepítési hoztak létre. Ezt a lépést hozzáadja az OMS-munkaterület belül tárolók megoldást, és automatikusan felismeri hello sablon által telepített hello OMS-ügynököt. hello ügynök elindul, hello tárolók folyamatok hello fürt az adatgyűjtést, és körülbelül 10 – 15 perc, a könnyű fel adatokkal, ahogy a fenti hello irányítópult hello képe hello megoldás megjelennie.

## <a name="4-next-steps"></a>4. Következő lépések

OMS kínál a hello munkaterület toomake különböző eszközök hasznos, ha az Ön. A következő beállítások toocustomize hello megoldás tooyour igényeinek hello ismerheti meg:
- Hello az beszerzése familiarized [naplófájl keresési és lekérdezése](../log-analytics/log-analytics-log-searches.md) szolgáltatásai által kínált Naplóelemzési
- Hello OMS-ügynök toopick egyes teljesítményszámlálókat be konfigurálása (toohello munkaterület otthoni Ugrás > Beállítások > adatok > Windows-teljesítményszámlálók)
- Konfigurálja az OMS tooset mentése [riasztás automatikus](../log-analytics/log-analytics-alerts.md) szabályok tooaid észlelésére és diagnosztika
