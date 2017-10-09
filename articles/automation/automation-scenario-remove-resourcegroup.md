---
title: "Az erőforráscsoportok aaaAutomate eltávolítása |} Microsoft Docs"
description: "Egy Azure Automation-forgatókönyv többek között a runbookok tooremove PowerShell munkafolyamat-verziójának minden erőforráscsoportokat az előfizetésben."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: 
ms.assetid: b848e345-fd5d-4b9d-bc57-3fe41d2ddb5c
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2016
ms.author: magoedte
ms.openlocfilehash: d7ff8064842385d57b0eebdf7b263150c958255f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Azure Automation-forgatókönyv – erőforráscsoportok eltávolításának automatizálása
Számos ügyfél hoz létre több erőforráscsoportot. Vannak, amelyeket éles alkalmazások felügyeletéhez, és vannak, amelyeket fejlesztési, tesztelési és átmeneti környezetként használnak. Az alábbi források hello telepítés automatizálásáról egyetlen művelet, de tudja toodecommission hello gombra kattintással erőforráscsoport folyamatban egy másik. Az Azure Automation használatával leegyszerűsítheti ezt a gyakori felügyeleti feladatot. Bizonyulhat hasznosnak, ha Azure-előfizetés, amely rendelkezik a költségkeret maximumát olyan tag ajánlat MSDN vagy a Microsoft Partner hálózati Cloud Essentials program hello keresztül dolgozik.

Ez a forgatókönyv egy PowerShell-forgatókönyv alapul, és tervezett tooremove egy vagy több erőforrás csoportokba az előfizetésből. hello alapértelmezett hello runbook lehet tootest a folytatás előtt. Ez biztosítja, hogy nem véletlenül hello erőforráscsoport előtt törölnie készen toocomplete Ön ezt az eljárást.   

## <a name="getting-hello-scenario"></a>Első hello forgatókönyv
Ebben a forgatókönyvben áll egy PowerShell-forgatókönyv, amely letölthető hello [PowerShell-galériában](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Közvetlenül a hello is importálhat [forgatókönyvek](automation-runbook-gallery.md) a hello Azure-portálon.<br><br>

| Forgatókönyv | Leírás |
| --- | --- |
| Remove-ResourceGroup |Egy vagy több Azure erőforráscsoport-sablonok és a kapcsolódó erőforrások eltávolítása hello előfizetésből. |

<br>
a következő bemeneti paraméterek hello Ez a forgatókönyv meghatározása:

| Paraméter | Leírás |
| --- | --- |
| NameFilter (kötelező) |Adja meg egy nevet szűrő toolimit hello erőforráscsoportok, melyet törlése. Vesszővel tagolt listaként több értéket is megadhat.<br>hello szűrő nem kis-és nagybetűket, és bármely erőforráscsoport hello karakterláncot tartalmazó karakterláncnak. |
| PreviewMode (választható) |Hello runbook toosee mely csoportok törlése akkor történik meg, de nincs művelet végrehajtása.<br>hello alapértelmezett érték a **igaz** toohelp elkerülése érdekében egy véletlen törlés vagy további erőforráscsoportok átadott toohello runbook. |

## <a name="install-and-configure-this-scenario"></a>A forgatókönyv telepítése és konfigurálása
### <a name="prerequisites"></a>Előfeltételek
Ez a forgatókönyv a hello használatával hitelesít [Azure-beli futtató fiók](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-hello-runbooks"></a>Telepítse és runbookokat hello közzététele
Hello runbook letöltését követően importálhatja azt a hello eljárás használatával [runbook eljárások importálása](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation). Hello runbook közzététele, miután sikeresen importálta az Automation-fiók be.

## <a name="using-hello-runbook"></a>Hello runbook használatával
hello lépések végigvezeti a runbook és megismerni működésének súgó hello végrehajtását. Csak tesztelni fogja hello runbook ebben a példában nem tényleges a hello erőforráscsoport törlése.  

1. Hello Azure-portálon, nyissa meg az Automation-fiók, és kattintson a **Runbookok**.
2. Jelölje be hello **Remove-ResourceGroup** runbook kattintson **Start**.
3. Hello runbook indításakor hello **runbook meghívása** panel nyílik meg, és konfigurálhatja hello paramétereit. Írja be a hello nevét az erőforráscsoportok tesztelési használnia, és hatására nem árt, ha véletlenül törli az előfizetésben.<br> ![A Remove-ResouceGroup paraméterei](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

   > [!NOTE]
   > Győződjön meg arról, hogy **Previewmode** értéke túl**igaz** hello törlése tooavoid kiválasztott erőforrás-csoportok.  **Megjegyzés:** , hogy ez a forgatókönyv nem távolítja el, amely tartalmazza ezt a runbookot futtató hello Automation-fiók hello erőforráscsoportot.  
   >
   >
4. Miután konfigurálta az összes hello paraméterértékeket, kattintson a **OK**, és hello runbook a végrehajtási, várósorba kerülnek.  

hello tooview hello részleteit **Remove-ResourceGroup** hello Azure portálon, válassza a runbook-feladat **feladatok** hello runbookban. hello feladat összegzése hello bemeneti paraméterek megjelenítése és hello kimeneti adatfolyam továbbá toogeneral információ hello feladat és összes kivétel történt.<br> ![A Remove-ResourceGroup forgatókönyv-feladat állapota](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

Hello **feladat összegzése** hello kimeneti, figyelmeztető és adatfolyamok üzeneteit tartalmazza. Válassza ki **kimeneti** tooview részletes eredményeinek hello a runbook végrehajtása.<br> ![A Remove-ResourceGroup forgatókönyv kimeneti eredményei](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Következő lépések
* a saját runbook létrehozásához tooget lásd: [létrehozása vagy importálása az Azure Automationben runbook](automation-creating-importing-runbook.md).
* a PowerShell-munkafolyamati forgatókönyvek használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md).
