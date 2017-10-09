---
title: "Azure Automation-fiók a Naplóelemzési aaaUnlink |} Microsoft Docs"
description: "A cikkben megtudhatja, hogyan toounlink az Azure Automation szolgáltatásbeli fiók az OMS-munkaterület áttekintését."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 02/07/2017
ms.author: magoedte
ms.openlocfilehash: 3ec0745673f6a872538d33023a7a1d2bb1d18ec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toounlink-your-automation-account-from-a-log-analytics-workspace"></a>Hogyan toounlink az automatizálási fiók egy Naplóelemzési munkaterületet

Azure Automation szolgáltatásbeli Naplóelemzési toonot csak támogatja az előrejelzéses figyeléssel a runbook-feladatok az Automation-fiók összes integrálható, de is szükség a következő megoldások Naplóelemzési függő hello importálásakor:

* [Frissítéskezelés](../operations-management-suite/oms-solution-update-management.md)
* [Változáskövetés](../log-analytics/log-analytics-change-tracking.md)
* [Indítása/leállítása virtuális gépek munkaidőn kívüli során](automation-solution-vm-management.md)
 
Ha úgy dönt, hogy már nem kívánja toointegrate az Automation-fiók a Naplóelemzési, megszüntetheti a fiók közvetlenül a hello Azure-portálon.  Mielőtt továbblép, akkor először a korábban említett tooremove hello megoldások, ellenkező esetben ez a folyamat nem lesz lehetséges a folytatás.  Tekintse át a hello témakör hello adott megoldás toounderstand hello lépéseit importálta tooremove azt.  

Ezek a megoldások eltávolítása után végezheti el a következő lépéseket toounlink hello az Automation-fiók.

## <a name="unlink-workspace"></a>Munkaterület választható

1. Hello Azure-portálon, nyissa meg az Automation-fiók, és hello Automation-fiók panelen hello-fiók panelen válassza ki a **munkaterület választható**.<br><br> ![Munkaterület beállítás leválasztása](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Hello szétkapcsolás munkaterület paneljén kattintson **worksapce leválasztása**.<br><br> ![Panel munkaterület választható](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  A kérdés ellenőrzése tooproceed kívánja fog kapni.<br><br>
3. Azure Automation próbál toounlink hello fiók Naplóelemzési munkaterületet, amíg előrehaladásának hello alatt **értesítések** hello menüből.

Ha hello frissítés felügyeleti megoldás, opcionálisan érdemes lehet tooremove hello hello megoldás eltávolítása után már nem szükséges elemeket a következő.

* Ütemezés frissítésére.  Minden egyes lesz melyek nevei egyeznek hello központi telepítést hozott létre)

* Hibrid feldolgozó csoportja hello megoldás.  Minden egyes lesznek elnevezve hasonlóképpen túl machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Ha munkaidőn kívüli megoldás során használt hello indítása/leállítása virtuális gépeket, opcionálisan érdemes lehet tooremove hello hello megoldás eltávolítása után már nem szükséges elemeket a következő.

* Elindítása és leállítása a virtuális gép runbook ütemezése 
* Virtuális gép runbookok elindítása és leállítása
* Változók   

## <a name="next-steps"></a>Következő lépések

tooreconfigure az Automation-fiók toointegrate az OMS szolgáltatáshoz, lásd: [feladat állapotát és a feladat adatfolyam továbbítása Automation tooLog szolgáltatás (OMS)](automation-manage-send-joblogs-log-analytics.md). 
