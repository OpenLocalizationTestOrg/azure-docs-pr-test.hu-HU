---
title: "Az Azure Automationben Azure modulok frissítése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan most már frissítheti az Azure Automationben alapértelmezés szerint biztosított közös Azure PowerShell-modulok."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: f5e7c66cfd26bd6927d48ffd8bc0f82e9a3e2d13
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Az Azure Automationben Azure PowerShell-modulok frissítése

A leggyakoribb Azure PowerShell-modulok minden Automation-fiókban alapértelmezés szerint biztosítottak.  Az Azure-csapat rendszeresen, frissíti az Azure modulok, így lehetővé teszi a modulok a fiók frissítését, ha új verziói érhetők el a portálról az általunk az Automation-fiókban.  

Modulok rendszeresen frissülnek a csoport által, mert negatívan befolyásolhatja a változás, például egy paraméter átnevezése vagy teljesen elavulttá parancsmag típusától függően a runbookok a belefoglalt parancsmagok módosítások alakulhat ki. A runbookok és a folyamatok automatizálásához azok érintő elkerüléséhez erősen ajánlott tesztelése, és a folytatás előtt ellenőrzi.  Ha erre a célra készült dedikált Automation-fiók nem rendelkezik, fontolja meg, hogy a teszteléssel számos különböző alkalmazási helyzetek és kombinációinak a runbookok fejlesztése során továbbá frissítése iteratív változásokra hozzon létre egyet a PowerShell-modulok.  Után az eredményeket a rendszer érvényesíti, és telepítette a szükséges módosításokat, folytassa a runbookokat, amely szükséges a módosítási áttelepítésének koordinációs, és hajtsa végre a frissítést, éles környezetben alább leírtak.     

## <a name="updating-azure-modules"></a>Az Azure modulok frissítése

1. Az Automation-fiók modulok lapján, a rendszer felajánlja a nevű **frissítés Azure modulok**. Mindig engedélyezve van.<br><br> ![Azure-modulok beállítás modulok oldalon frissítése](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Kattintson a **frissítés Azure modulok** és, amely arra kéri, ha folytatni kívánja megerősítési értesítés jelenik meg.<br><br> ![Azure-modulok értesítési frissítése](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Kattintson a **Igen** és a modul frissítési folyamat megkezdése. A frissítési folyamat veszi körülbelül 15 – 20 perc frissítése a következő modult:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Ha a modulok már naprakészek legyenek, majd a folyamat befejezése néhány másodpercen belül. A frissítési folyamat befejezéséről értesítést fog kapni.<br><br> ![Azure-modulok frissítési állapotának frissítése](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Azure Automation szolgáltatásbeli használja a legújabb modulok az Automation-fiók egy új ütemezett feladat futtatásakor.    

Használatakor a Azure PowerShell-modulok parancsmagjait a runbookok Azure erőforrások kezeléséhez, majd szeretne végrehajtani a frissítési folyamat minden hónap vagy így ahhoz, hogy biztosítsa, hogy rendelkezik-e a legújabb modulok.

## <a name="next-steps"></a>Következő lépések

* Integrációs modulok és az egyéni modulok további Automation integrálása más rendszerek, a szolgáltatások vagy a megoldások létrehozásával kapcsolatos további információkért lásd: [integrációs modulok](automation-integration-modules.md).

* Érdemes lehet forrás vezérlő integrációs [GitHub vállalati](automation-scenario-source-control-integration-with-github-ent.md) vagy [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) központi kezelésére, és szabályozhatja a automatizálási runbook és konfigurációs portfóliót kiadásaiban.  