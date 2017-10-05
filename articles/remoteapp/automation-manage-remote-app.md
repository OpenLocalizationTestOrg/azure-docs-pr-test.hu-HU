---
title: "Azure RemoteApp használata az Azure Automation kezelése |} Microsoft Docs"
description: "További tudnivalók hogyan az Azure Automation szolgáltatás Azure RemoteApp kezelésére használható."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 589f01ef-f9c1-4e7b-a040-1b46862d3544
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: magoedte;csand
ms.openlocfilehash: 59ac11f153c0bd74a1106400dbbf5790968b657c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-remoteapp-using-azure-automation"></a>Azure RemoteApp használata az Azure Automation kezelése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható az Azure RemoteApp felügyeleti leegyszerűsítése érdekében.

## <a name="what-is-azure-automation"></a>Mi az Azure Automation?
[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén. Azure Automation használ, manuális, gyakran ismétlődő, hosszan futó és hibalehetőséget feladatok automatizálhatók megbízhatóságát, hatékonyságát és a szervezet hamarabb növelése érdekében.

Azure Automation szolgáltatásbeli biztosít egy magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi az igényeinek. Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.

Csökkentheti a működési munkaterhelés és szabadítson fel informatikai és DevOps alkalmazottak munka, amely üzleti értéket hozzáadja a felhőbeli felügyeleti feladatok automatikusan Azure Automation által futtatandó mozgatásával összpontosíthat.

## <a name="how-can-azure-automation-help-manage-azure-remoteapp"></a>Hogyan segíthet az Azure Automation kezelése az Azure RemoteApp?
RemoteApp által biztosított PowerShell-parancsmagok használatával kezelhető az Azure Automationben a [Azure PowerShell eszközök](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automation a érhető el a RemoteApp PowerShell-parancsmagok alapesetben rendelkezik, így a RemoteApp felügyeleti feladatokat a szolgáltatáson belül végezheti el. Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások, Azure-szolgáltatások és a 3. fél rendszerek között összetett feladatok automatizálása a parancsmagjaival is párosítható.

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte az Azure Automation, és hogyan használható az Azure RemoteApp kezeléséhez alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Automation.

* Tekintse meg az Azure Automation szolgáltatásbeli [használatába bevezető oktatóanyagot](../automation/automation-first-runbook-graphical.md)

