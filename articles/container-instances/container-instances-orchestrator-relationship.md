---
title: "aaaAzure tároló példányok és tároló Vezénylési"
description: "Azure-tároló példányok és tároló orchestrators együttműködését ismertetése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 69a39edc6f14d885c1ac300990ed1399002ccfee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure-példányokon tároló és a tároló orchestrators

Kis méret és alkalmazás tájolás, miatt tárolók alkalmas mikroszolgáltatási-alapú architektúra és gyors kézbesítési környezetben. hello feladat automatizálása és kezelése a tárolók és azok működésmódját nagy számú nevezik *vezénylési*. Népszerű tároló orchestrators például Kubernetes, a DC/OS és a Docker Swarm, amelyek mindegyike hello elérhető [Azure Tárolószolgáltatás](https://docs.microsoft.com/azure/container-service/).

Azure tároló példányok néhány alapvető képességek vezénylési platformokról ütemezés hello biztosít, de nem terjed ki, hogy ezek a platformok adja meg, és ténylegesen lehet őket a kiegészítő hello nagyobb értékű szolgáltatások. Ez a cikk ismerteti a hello hatókör Azure tároló példányok kezeli, és hogyan teljes tároló orchestrators lehet használni.

## <a name="traditional-orchestration"></a>Hagyományos vezénylési

vezénylési hello szabványos definícióját a következő feladatok hello tartalmazza:

- **Ütemezés**: egy tároló lemezképet és az erőforrás kérelmet kap, található megfelelő gép mely toorun hello tárolójához.
- **Affinitás/Anti-affinity**: Adja meg, hogy tárolók készlete kell közelben egymástól (teljesítmény) vagy fusson elég távolságra (a rendelkezésre állás érdekében).
- **Állapotfigyelés**: nézze meg a tároló hibák, és automatikusan ütemezze újra őket.
- **Feladatátvételi**: nyomon követjük, hogy mi minden számítógépen fut, és ütemezze újra a sikertelen gépek toohealthy csomópontjáról tárolók.
- **Skálázás**: hozzáadása vagy eltávolítása a tároló példányok toomatch igény szerint manuálisan vagy automatikusan.
- **Hálózati**: Adjon meg egy átmeneti területre hálózatot a tárolók toocommunicate összehangolása a különböző több gazdagép gépnek.
- **A szolgáltatásészlelés**: tárolók toolocate egymással automatikusan engedélyezni akár gazdagépeken között, és módosítsa az IP-címeket.
- **Alkalmazásfrissítések koordinált**: tároló frissítések tooavoid alkalmazás állásidő kezeléséhez, és engedélyezze a visszaállítás, ha valamilyen hiba.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Vezénylési Azure tároló osztályt: réteges megközelítésének

Azure-példányokon tároló lehetővé teszi, hogy a réteges megközelítésének tooorchestration összes hello ütemezés megadása, és felügyeleti képességek szükséges toorun egyetlen tárolót, miközben lehetővé teszi az orchestrator a platformok toomanage több tároló feladatok utasítást.

Összes alapjául szolgáló infrastruktúra tároló példányok hello Azure kezeli, mert az orchestrator platform nem kell egy megfelelő gazdagépet a mely toorun egyetlen tárolót keresése a saját magát tooconcern. hello felhő hello rugalmassága biztosítja, hogy egy mindig elérhető legyen. Ehelyett hello orchestrator hello feladatok, amelyek megkönnyítik a több tároló-architektúrák, beleértve a méretezés és koordinált frissítéseinek hello fejlesztésének összpontosíthat.



## <a name="potential-scenarios"></a>A lehetséges forgatókönyvek

Azure-tároló példányaival orchestrator integrációs pedig továbbra is születő tervezzük, hogy néhány különböző környezetekben előfordulhat, hogy megjelenni:

### <a name="orchestration-of-container-instances-exclusively"></a>Vezénylési tároló példányok kizárólag

Mivel gyorsan start, illetve kiszámlázni hello által a második, kizárólag a alapú környezet Azure tároló példányok kínál hello leggyorsabb módon tooget elindult és a munkaterhelések magas változó toodeal.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Tároló-példányok és a virtuális gépek a tárolók

Hosszú futású stabil, tárolók dedikált virtuális gépek fürtben futó munkaterhelések jellemzően lesz olcsóbbak futó hello azonos tárolók tároló osztályt. Azonban a tároló példányok kiválóan alkalmas gyorsan kibontásával, és a teljes kapacitásának toodeal való használata során váratlan vagy rövid élettartamú igényeiben jelentkező szerződő kínálnak. Ahelyett, hogy a fürtben lévő virtuális gépek száma hello kiterjesztése, akkor telepíti ezeket a gépeket alakzatot további tárolókat, hello orchestrator is egyszerűen ütemezése hello tároló példányok használatával további tárolókat, és törölje őket, ha azok nem már szükséges.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Példa: Kubernetes Azure tároló példányok összekötő

Hogyan tároló vezénylési platformok integrálhatják az Azure tároló osztályt toodemonstrate, azt elindította épület egy [Kubernetes minta összekötő][aci-connector-k8s]. 

összekötő hello a Kubernetes utánozza hello [kubelet] [ kubelet-doc] korlátlan kapacitás csomópontként regisztrálása és hello létrehozása terjesztéséhez [három munkaállomás-csoporttal] [ pod-doc] tároló csoportokként Azure tároló példányát. 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

Összekötők a többi orchestrators épülhet, hasonlóképpen platform primitívek toocombine hello power hello orchestrator API hello sebességű és egyszerű Azure tároló példányát tárolók kezelése a integrálva.

> [!WARNING]
> hello ACI összekötő Kubernetes érték *kísérleti* és éles környezetben nem használható.

## <a name="next-steps"></a>Következő lépések

Az első tároló létrehozása Azure-tároló osztályt hello segítségével [– első lépések útmutató](container-instances-quickstart.md).

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/