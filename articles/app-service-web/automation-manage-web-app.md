---
title: "Azure Web App használatával Azure Automation kezelése |} Microsoft Docs"
description: "Hogyan az Azure Automation szolgáltatás segítségével kezelheti az Azure Web Apps megismerése."
services: app-service\web, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: c960a44f-f941-401d-afba-a4bc0f0394eb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte
ms.openlocfilehash: a96332346747114620ee6ebd2a2d0c4417d4a213
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="managing-azure-web-app-using-azure-automation"></a>Azure Web App használatával Azure Automation kezelése
Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható az egyszerűbb kezelés Azure webalkalmazás számára.

## <a name="what-is-azure-automation"></a>Mi az Azure Automation?
[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén. Használja az Azure Automation, manuális, ismétlődik, hosszan futó és hibalehetőséget feladatok automatizálhatók megbízhatóságát, hatékonyságát és a szervezet hamarabb növelése érdekében.

Azure Automation szolgáltatásbeli biztosít egy magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi az igényeinek. Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.

Csökkentheti a működési munkaterhelés és szabadítson fel informatikai és DevOps alkalmazottak munka, amely üzleti értéket hozzáadja a felhőbeli felügyeleti feladatok automatikusan Azure Automation által futtatandó mozgatásával összpontosíthat.

## <a name="how-can-azure-automation-help-manage-azure-web-app"></a>Hogyan segíthet az Azure Automation kezelése az Azure-webalkalmazás?
Webes alkalmazás által biztosított PowerShell-parancsmagok használatával kezelhető az Azure Automationben a [Azure PowerShell-modulok](/powershell/azureps-cmdlets-docs). Is [a webes alkalmazás PowerShell-parancsmagjainak telepítése az Azure Automationben](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), hogy a webalkalmazás felügyeleti feladatokat a szolgáltatáson belül végezheti el. Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások különböző Azure-szolgáltatások és a 3. fél rendszerek összetett feladatok automatizálása a parancsmagjaival is párosítható.

Íme néhány példa a alkalmazásszolgáltatások automatizálással kezelése:

* [Webalkalmazások kezelésére szolgáló parancsfájlok](https://azure.microsoft.com/documentation/scripts/)

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte az Azure Automation, és hogyan használható Azure Web Apps kezeléséhez alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Automation.

* Tekintse meg az Azure Automation szolgáltatásbeli [használatába bevezető oktatóanyagot](../automation/automation-first-runbook-graphical.md)

