---
title: "felhőalapú szolgáltatás memóriafoglalási hiba aaaTroubleshooting |} Microsoft Docs"
description: "Hozzárendelési hibák elhárítása a Cloud Services telepítése során az Azure szolgáltatásban"
services: azure-service-management, cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: dfd5cc4663ccc6ed1b27ca9df579182737363b0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Hozzárendelési hibák elhárítása a Cloud Services telepítése során az Azure szolgáltatásban
## <a name="summary"></a>Összefoglalás
Ha példányok tooa felhőalapú szolgáltatás telepítéséhez, vagy adja hozzá az új webes vagy feldolgozói szerepkör példányok, a Microsoft Azure számítási erőforrásokat foglal le. Alkalmanként kaphat a hibák, ezen műveletek végrehajtásakor, még mielőtt hello Azure előfizetési korlátozásait. Ez a cikk ismerteti a hello okok egyes hello közös pufferallokációs hibák, és lehetséges javítási javasol. hello információkat is lehetnek hasznosak, ha azt tervezi, hogy a szolgáltatások hello telepítését.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Háttér – foglalási működése
az Azure adatközpontjaiban hello kiszolgálók particionáltak csoportokba. Egy új felhőalapú szolgáltatás memóriafoglalási kérelem több fürt kísérlet történik. Ha hello első példány telepített tooa felhőalapú szolgáltatást (az átmeneti vagy üzemi), a felhőalapú szolgáltatás lekérdezi a rögzített tooa fürt. Bármely további központi telepítéseket hello felhőszolgáltatás fordul elő hello ugyanaz a fürt. Ebben a cikkben kifejezés toothis "rögzített tooa fürtként". Alább 1 diagram azt ábrázolja, hello kis-és egy normál foglalási, amely több fürtökben; kísérlet történik 2. ábrán látható, hello kis-és memóriakiosztást meg rögzített tooCluster 2 mert, ahol van hello a meglévő felhőalapú szolgáltatás CS_1 üzemelteti.

![Foglalási diagramja](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Miért memóriafoglalási hiba történik
Ha egy memóriafoglalási kérelem rögzített tooa fürt, magasabb esély van a toofind szabad erőforrást sikertelenek lesznek, mivel hello elérhető erőforráskészlet korlátozott tooa fürt. Továbbá ha a memóriafoglalási kérelem rögzített tooa fürt, de hello típus a kért erőforrás nem érhető el, hogy a fürt, a kérelem sikertelen lesz akkor is, ha hello fürt szabad erőforrás. 3. ábra alatt hello esetet, ahol egy rögzített foglalás sikertelen lesz, mivel hello csak jelölt fürtnek nincs szabad erőforrást mutatja be. 4. ábrán látható, ahol egy rögzített foglalás sikertelen lesz, mivel nem támogatja a hello csak jelölt fürt hello eset hello a kért Virtuálisgép-méretet, annak ellenére, hogy a fürt hello ingyenes erőforrással rendelkezik.

![Rögzített memóriafoglalási hiba](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>A felhőszolgáltatások észlelt lefoglalási hibák elhárítása
### <a name="error-message"></a>Hibaüzenet
A következő hibaüzenet hello jelenhetnek meg:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable toosatisfy constraints in request. hello requested new service deployment is bound tooan Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains hello new deployment toospecific Azure resources. Please retry later or try reducing hello VM size or number of role instances. Alternatively, if possible, remove hello aforementioned constraints or try deploying tooa different region."

### <a name="common-issues"></a>Gyakori problémák
Az alábbiakban hello közös foglalási forgatókönyvek, amelyek egy foglalási kérelem toobe rögzített tooa egyetlen fürthöz.

* Tárolóhely - tooStaging telepítésekor egy felhőalapú szolgáltatás a központi telepítés van vagy tárolóhelye, majd hello teljes felhőszolgáltatás rögzített tooa adott fürt.  Ez azt jelenti, hogy ha egy telepítés már létezik a hello éles webalkalmazásra, majd egy új átmeneti központi telepítés csak lehet hozzárendelni hello azonos fürtöt hello éles tárolóhelyre. Ha hello fürt közelít kapacitásának határához, hello kérelem sikertelen lehet.
* Skálázás - hozzáadása új példányok tooan létező felhőalapú szolgáltatást kell rendelnie a hello azonos fürt.  Rövid skálázás kérelmek általában osztják, de nem minden esetben. Ha hello fürt közelít kapacitásának határához, hello kérelem sikertelen lehet.
* Affinitáscsoport - egy új központi telepítési tooan üres felhőszolgáltatás foglalhatók hello fabric az adott régióban, a fürt által kivéve, ha hello felhőszolgáltatás rögzített tooan affinitáscsoport. Központi telepítések toohello ugyanabban az affinitáscsoportban kísérli meg a rendszer a hello ugyanabban a fürtben. Ha hello fürt közelít kapacitásának határához, hello kérelem sikertelen lehet.
* Affinitáscsoport vNet - régebbi virtuális hálózatok volt a feltételekhez tooaffinity csoportok régiókat helyett, és ezek a virtuális hálózatok felhőszolgáltatások rögzített toohello affinitáscsoport fürt. Központi telepítések toothis fajta virtuális hálózatot rögzítve hello fürtön kísérli meg a rendszer. Ha hello fürt közelít kapacitásának határához, hello kérelem sikertelen lehet.

## <a name="solutions"></a>Megoldások
1. Helyezze üzembe újra tooa új felhőszolgáltatás - Ez a megoldás valószínűleg toobe legtöbb sikeres megegyezik lehetővé teszi a hello platform toochoose összes fürtök az adott régióban.

   * Hello munkaterhelés tooa új felhőalapú szolgáltatás üzembe helyezése  
   * Hello CNAME vagy A rekord toopoint forgalom toohello új felhőalapú szolgáltatás frissítése
   * Nulla forgalom toohello régi webhely állapotra vált, amennyiben törölheti hello régi felhőalapú szolgáltatás. Ez a megoldás kell fel Önnek leállítása nélkül.
2. Üzemi és átmeneti üzembe helyezési ponti törölni – Ez a megoldás is megőrzi a meglévő DNS-nevével, de állásidő tooyour alkalmazást, akkor.

   * Hello üzemi és átmeneti üzembe helyezési ponti egy meglévő felhőalapú szolgáltatás, így a hello felhőalapú szolgáltatás nem üres, törölje, majd
   * Hozzon létre egy új központi telepítési hello létező felhőalapú szolgáltatást. Minden fürtön hello régióban tooallocation újbóli kísérlet. Győződjön meg arról hello felhőalapú szolgáltatás nem kapcsolt tooan affinitáscsoport.
3. Fenntartott IP - Ez a megoldás megtartja a meglévő IP-cím, de állásidő tooyour alkalmazás okoz.  

   * Hozzon létre egy foglalt IP-cím a meglévő telepítéshez Powershell használatával

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Hajtsa végre a promptjai, így új foglalt IP-cím a hello szolgáltatás szolgáltatáskonfigurációs SÉMA hello meg arról, hogy toospecify #2.
4. Távolítsa el az új központi telepítéseknél - affinitáscsoport Affinitáscsoportok már nem támogatottak. Új felhőalapú szolgáltatás toodeploy #1 utasításai. Győződjön meg róla, felhőalapú szolgáltatás nem olyan affinitáscsoportban van.
5. Átalakítás tooa regionális virtuális hálózat – lásd: [hogyan toomigrate az Affinitáscsoportok tooa regionális virtuális hálózatot (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
