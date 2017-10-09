---
title: "AAA \"Azure Batch-készlet esemény létrehozása |} Microsoft dokumentumok\""
description: "Útmutató a Batch-készlet esemény létrehozása."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: ad31251969af553baa21e8c533d8ea95d3eeaf91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pool-create-event"></a>Készlet esemény létrehozása

 Ez az esemény kibocsátott készlet létrehozása után. hello tartalom hello napló megmutatják hello készlet kapcsolatos általános információkat. Vegye figyelembe, hogy hello cél hello készlet mérete nagyobb, mint 0 számítási csomópontok, ha egy átméretezési start eseménye követi ezt az eseményt követően azonnal.

 hello következő példa bemutatja egy alkalmazáskészlet hello törzsét hello CloudServiceConfiguration tulajdonság használatával létrehozott készlet esemény létrehozása.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Elem|Típus|Megjegyzések|
|-------------|----------|-----------|
|id|Karakterlánc|hello készlet hello azonosítója.|
|displayName|Karakterlánc|hello hello készlet nevét jeleníti meg.|
|vmSize|Karakterlánc|hello mérete hello virtuális gépek hello készletben. A készlet összes virtuális gépnek hello azonos méretűnek. <br/><br/> További információ elérhető méretek a virtuális gépek Felhőszolgáltatásai számára (készletek cloudServiceConfiguration létre), készletek: [Felhőszolgáltatások mérete](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Kötegelt támogatja kivételével az összes felhőalapú szolgáltatások Virtuálisgép-méretek `ExtraSmall`.<br/><br/> További információ a rendelkezésre álló virtuális gép szolgáltatással hello virtuális gépek piactér (virtualMachineConfiguration létrehozott készleteket) a képek méretei: [virtuális gépek méretei](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) vagy [a méretének Virtuális gépek](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). A Batch a `STANDARD_A0`, illetve a Premium Storage típusú méretek (`STANDARD_GS`, `STANDARD_DS` és `STANDARD_DSV2` sorozat) kivételével az összes Azure virtuálisgép-méretet támogatja.|
|[cloudServiceConfiguration](#bk_csconf)|Összetett típus|hello cloud service hello készlet konfigurációjában.|
|[virtualMachineConfiguration](#bk_vmconf)|Összetett típus|hello virtuálisgép-konfiguráció hello készlet.|
|[Készlet](#bk_netconf)|Összetett típus|hello hello készlet hálózati konfigurációjában.|
|resizeTimeout|Time|a számítási csomópontok toohello címkészlet hello utolsó átméretezési művelet hello készlet a megadott foglalási hello időkorlátját.  (hello kezdeti létrehozásakor hello készlet számát, a átméretezési méretezése.)|
|targetDedicated|Int32|számítási csomópontok hello készlet igényelt hello száma.|
|enableAutoScale|logikai érték|Meghatározza, hogy hello mérete automatikusan igazodni adott idő alatt.|
|enableInterNodeCommunication|logikai érték|Meghatározza, hogy hello készlet be van állítva a közvetlen kommunikációt a csomópontok között.|
|isAutoPool|logikai érték|Speficies e hello készlet használatával egy feladat AutoPool mechanizmus jött létre.|
|maxTasksPerNode|Int32|hello hello készlet egyes számítási csomóponton egyidejűleg futtatható feladatok maximális számát.|
|vmFillType|Karakterlánc|Határozza meg, hogyan osztja el a Batch szolgáltatás hello feladatok hello készlet számítási csomópontjai között. Érvényes értékek van elosztva, vagy a csomag.|

###  <a name="bk_csconf"></a>cloudServiceConfiguration

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|Család|Karakterlánc|hello Azure vendég operációs rendszer termékcsalád toobe hello készletben hello virtuális gépeken telepített.<br /><br /> Lehetséges értékek:<br /><br /> **2** – operációs rendszer családja 2, egyenértékű tooWindows Server 2008 R2 SP1.<br /><br /> **3** – operációs rendszer családja 3, egyenértékű tooWindows Server 2012-ben.<br /><br /> **4** – operációs rendszer családja 4, egyenértékű tooWindows Server 2012 R2-ben.<br /><br /> További információkért lásd: [Azure vendég operációs rendszereinek kiadásait](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Karakterlánc|hello Azure vendég operációs rendszer verziója toobe hello készletben hello virtuális gépeken telepített.<br /><br /> hello alapértelmezett értéke  **\***  hello hello legújabb operációs rendszer verziója határozza meg, amely a megadott termékcsalád.<br /><br /> Más engedélyezett értékeket, lásd: [Azure vendég operációs rendszereinek kiadásait](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a>virtualMachineConfiguration

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Összetett típus|Megadja az hello platform vagy Piactéri lemezkép toouse információkat.|
|nodeAgentSKUId|Karakterlánc|hello SKU hello kötegelt csomópont ügynök hello számítási csomópont lett kiépítve.|
|[windowsConfiguration](#bk_winconf)|Összetett típus|Határozza meg a hello virtuális gép Windows operációs rendszer beállításait. Ez a tulajdonság nem lehet megadva, ha hello imageReference egy Linux operációsrendszer-lemezképek hivatkozik.|

###  <a name="bk_imgref"></a>imageReference

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|Közzétevő|Karakterlánc|hello publisher hello kép.|
|az ajánlat|Karakterlánc|hello ajánlat hello kép.|
|Termékváltozat|Karakterlánc|hello hello lemezkép Termékváltozat.|
|Verzió|Karakterlánc|hello lemezkép hello verziója.|

###  <a name="bk_winconf"></a>windowsConfiguration

|Elem neve|Típus|Megjegyzések|
|------------------|----------|-----------|
|enableAutomaticUpdates|Logikai érték|Azt jelzi, hogy engedélyezve van-e virtuális gép hello az automatikus frissítések. Ha ez a tulajdonság nincs megadva, a hello alapértelmezett érték: igaz.|

###  <a name="bk_netconf"></a>Készlet

|Elem neve|Típus|Megjegyzések|
|------------------|--------------|----------|
|subnetId|Karakterlánc|Megadja, mely hello készlet számítási csomópontok létrejönnek hello alhálózati hello erőforrás-azonosítója.|
