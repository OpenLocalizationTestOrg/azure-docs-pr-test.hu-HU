---
title: "A Service Fabric-fürt biztonsági: ügyfél szerepkörök |} Microsoft Docs"
description: "Ez a cikk ismerteti a hello két ügyfél szerepköröket, illetve a megadott toohello szerepkörök hello engedélyeket."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: coreysa
editor: 
ms.assetid: 7bc808d9-3609-46a1-ac12-b4f53bff98dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 4a4a9f93e91ea816005b730bebbcb317f8bab255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>A Service Fabric ügyfelek szerepköralapú hozzáférés-vezérlés
Az Azure Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa Service Fabric-fürt: rendszergazdai és felhasználói. Hozzáférés-vezérlés lehetővé teszi, hogy hello rendszergazda toolimit hozzáférés toocertain fürt fürtműveletekben hello fürt biztonságosabbá tétele a felhasználók különböző csoportok számára.  

**A rendszergazdák** (beleértve az olvasási/írási képességek) teljes körű hozzáférési toomanagement képességekkel rendelkeznek. Alapértelmezés szerint **felhasználók** csak van olvasási hozzáférési toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.

Hello két ügyfél szerepkörök (a rendszergazda és az ügyfél) a fürt létrehozása hello időpontjában adja meg az egyes különálló tanúsítványok megadásával. Lásd: [Service Fabric-fürt biztonsági](service-fabric-cluster-security.md) biztonságos Service Fabric-fürt beállításával kapcsolatos részleteket.

## <a name="default-access-control-settings"></a>Alapértelmezett hozzáférés-vezérlési beállításokkal
hello rendszergazdai hozzáférés-vezérlő típusa rendelkezik teljes körű hozzáférési tooall hello FabricClient API-k. Minden olvasási és írási végrehajtható, a hello Service Fabric-fürt, beleértve a következő műveletek hello:

### <a name="application-and-service-operations"></a>Alkalmazás és a szolgáltatási műveletek
* **CreateService**: a szolgáltatás létrehozása                             
* **CreateServiceFromTemplate**: a szolgáltatás létrehozása sablonból                             
* **UpdateService**: frissítheti a szolgáltatást                             
* **DeleteService**: szolgáltatás törlése                             
* **ProvisionApplicationType**: alkalmazás típusa kiépítése                             
* **CreateApplication**: alkalmazás létrehozása                               
* **Deleteapplication függvényhez**: alkalmazás törlése                             
* **UpgradeApplication**: indítása és alkalmazásfrissítések megszakítása                             
* **UnprovisionApplicationType**: alkalmazás típus leépítése                             
* **MoveNextUpgradeDomain**: explicit frissítési tartománnyal alkalmazásfrissítések folytatása                             
* **ReportUpgradeHealth**: hello aktuális frissítés állapota az alkalmazásfrissítések folytatása                             
* **ReportHealth**: reporting állapota                             
* **PredeployPackageToNode**: telepítés előtti API                            
* **CodePackageControl**: kód csomagok újraindítása                             
* **RecoverPartition**: egy partíció helyreállítása                             
* **RecoverPartitions**: partíciók helyreállítása                             
* **RecoverServicePartitions**: szolgáltatáspartíciók helyreállítása                             
* **RecoverSystemPartitions**: rendszer szolgáltatáspartíciók helyreállítása                             

### <a name="cluster-operations"></a>Fürt működését
* **ProvisionFabric**: MSI-fájl és/vagy a fürt manifest kiépítése                             
* **UpgradeFabric**: fürt frissítések indítása                             
* **UnprovisionFabric**: MSI-fájl és/vagy a fürt manifest kiépítés megszüntetésére                         
* **MoveNextFabricUpgradeDomain**: explicit frissítési tartománnyal fürt frissítések folytatása                             
* **ReportFabricUpgradeHealth**: fürt frissítések hello jelenlegi frissítési folyamat folytatása                             
* **StartInfrastructureTask**: infrastruktúra feladatok indítása                             
* **FinishInfrastructureTask**: infrastruktúrakezelési feladatok befejezése                             
* **InvokeInfrastructureCommand**: infrastruktúra feladat parancsok                              
* **ActivateNode**: egy csomópont aktiválása                             
* **DeactivateNode**: a csomópont inaktiválása                             
* **DeactivateNodesBatch**: több csomópont inaktiválása                             
* **RemoveNodeDeactivations**: több csomóponton visszaállítási inaktiválása                             
* **GetNodeDeactivationStatus**: az inaktiválást állapotának ellenőrzése                             
* **NodeStateRemoved**: csomópont állapota reporting eltávolítása                             
* **ReportFault**: reporting hiba                             
* **FileContent**: image store ügyfél fájlátvitel (külső toocluster)                             
* **FileDownload**: image store ügyfél fájl letöltése kezdeményezés (külső toocluster)                             
* **InternalList**: image store ügyfél fájl list művelet (belső)                             
* **Törlés**: image store ügyfél delete művelet                              
* **Töltse fel**: image store ügyfél feltöltési művelete                             
* **NodeControl**: indítása, leállítása és újraindítása csomópontok                             
* **MoveReplicaControl**: egy csomópont tooanother replikák áthelyezése                             

### <a name="miscellaneous-operations"></a>Különböző műveletekhez
* **Ping**: ügyfél pingelésre.                             
* **Lekérdezés**: engedélyezett összes lekérdezés
* **NameExists**: elnevezési URI meglétének ellenőrzése                             

hello felhasználói hozzáférés vezérlőtípus, alapértelmezés szerint ki, korlátozott toohello a következő műveleteket: 

* **EnumerateSubnames**: elnevezési URI felsorolása                             
* **EnumerateProperties**: elnevezési tulajdonság felsorolása                             
* **PropertyReadBatch**: elnevezési tulajdonság olvasási műveletek                             
* **GetServiceDescription**: szolgáltatás hosszú-lekérdezési értesítéseket és olvasási szolgáltatás leírása                             
* **ResolveService**: csatlakoztatásra-alapú szolgáltatás feloldása                             
* **ResolveNameOwner**: feloldó elnevezési URI tulajdonosa                             
* **ResolvePartition**: rendszerszolgáltatások feloldása                             
* **ServiceNotifications**: eseményalapú szolgáltatáshoz értesítést                             
* **GetUpgradeStatus**: lekérdezési alkalmazás frissítési állapot                             
* **GetFabricUpgradeStatus**: a fürt frissítési állapotának lekérdezése                             
* **InvokeInfrastructureQuery**: infrastruktúrakezelési feladatok lekérdezése                             
* **Lista**: image store ügyfél fájl list művelet                             
* **ResetPartitionLoad**: betöltése a failover Unit visszaállítása                             
* **ToggleVerboseServicePlacementHealthReporting**: való átváltással részletes elhelyezési állapotát szolgáltatásjelentési                             

hello rendszergazdai hozzáférés-vezérlés hozzáférési toohello megelőző műveletek is rendelkezik.

## <a name="changing-default-settings-for-client-roles"></a>Ügyfél-szerepkörök alapértelmezett beállításainak módosítása
Hello fürt jegyzékfájl, akkor is rendszergazdai képességek toohello ügyfél szükség esetén adja meg. Hello alapértelmezett módosíthatja is toohello **Hálóbeállításokat** során lehetőséget [fürtlétrehozás](service-fabric-cluster-creation-via-portal.md), és a beállítások megelőző hello hello **neve**, **admin**, **felhasználói**, és **érték** mezőket.

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-fürt biztonsági](service-fabric-cluster-security.md)

[A Service Fabric-fürt létrehozása](service-fabric-cluster-creation-via-portal.md)

