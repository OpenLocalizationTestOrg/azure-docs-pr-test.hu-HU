---
title: "aaaCreating runbook vagy importálása az Azure Automationben"
description: "Ez a cikk ismerteti, hogyan toocreate egy új runbookot, az Azure Automation vagy importálása fájlból."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 24414362-b690-4474-8ca7-df18e30fc31d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: d45f44cf15fbcacdd0de2977668502c2e1671063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Létrehozása vagy egy Azure Automation forgatókönyv importálása
Hozzáadhat egy runbook tooAzure automatizálás által [újat hoz létre](#creating-a-new-runbook) vagy egy létező runbookot importál egy fájlból, vagy a hello [forgatókönyvek](automation-runbook-gallery.md). Ez a cikk bemutatja, létrehozása és runbookok importálása fájlból.  Hello részleteinek fér hozzá a közösségi runbookok és a modulok mindegyikének kaphat [az Azure Automation forgatókönyv- és gyűjtemények](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Új runbook létrehozása
Az Azure Automationben hello Azure egyikének használatával hozhat létre egy új runbookot portálok vagy a Windows PowerShell. Hello runbook létrehozása után szerkesztheti témakörben található információk alapján [tanulási PowerShell munkafolyamat](automation-powershell-workflow.md) és [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).

### <a name="toocreate-a-new-azure-automation-runbook-with-hello-azure-classic-portal"></a>toocreate egy új Azure Automation-runbook hello klasszikus Azure portálon
Csak dolgozhat [PowerShell munkafolyamat-forgatókönyvekről](automation-runbook-types.md#powershell-workflow-runbooks) a hello Azure-portálon.

1. Hello klasszikus Azure portálon kattintson, **új**, **alkalmazásszolgáltatások**, **Automation**, **Runbook**, **Gyorslétrehozás**.
2. Adja meg a hello szükséges információkat, és kattintson **létrehozása**. hello runbook nevének betűvel kell kezdődnie, és rendelkezhetnek, betűket, számokat, aláhúzásjeleket és kötőjeleket.
3. Ha most tooedit hello runbookot, majd kattintson **Runbook szerkesztése**. Ellenkező esetben kattintson a **OK**.
4. Az új runbook megjelenik a hello **Runbookok** fülre.

### <a name="toocreate-a-new-azure-automation-runbook-with-hello-azure-portal"></a>toocreate egy új Azure Automation-runbook a hello Azure-portálon
1. Hello Azure-portálon nyissa meg az Automation-fiók.
2. Hello Hub, válassza ki **Runbookok** runbookokat hello listája tooopen.
3. Kattintson a hello **hozzáadása egy runbook** gombra, majd **hozzon létre egy új runbookot**.
4. Adjon meg egy **neve** hello runbookhoz és válassza ki a [típus](automation-runbook-types.md). hello runbook nevének betűvel kell kezdődnie, és rendelkezhetnek, betűket, számokat, aláhúzásjeleket és kötőjeleket.
5. Kattintson a **létrehozása** toocreate hello runbookot és a nyitott hello szerkesztő.

### <a name="toocreate-a-new-azure-automation-runbook-with-windows-powershell"></a>a Windows PowerShell használatával új Azure Automation-runbook toocreate
Használhatja a hello [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) parancsmag toocreate egy üres [PowerShell-munkafolyamati forgatókönyv](automation-runbook-types.md#powershell-workflow-runbooks). Megadhatja a hello **neve** paraméter toocreate egy üres runbookot, hogy később módosíthatja, vagy megadhatja a hello **elérési** paraméter tooimport egy runbook fájl. Hello **típus** paraméter szerepel toospecify hello négy runbook típusok egyikét kell is.

hello következő minta parancsok megjelenítése hogyan toocreate egy új üres runbookot.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Runbook Azure Automation-fájlból való importálása
Létrehozhat egy új runbookot az Azure Automation PowerShell-parancsfájl vagy PowerShell-munkafolyamati (.ps1 kiterjesztéssel), vagy egy exportált grafikus forgatókönyvnek (.graphrunbook) importálásával.  Meg kell adnia a hello [típusú forgatókönyvet](automation-runbook-types.md) , figyelembe véve a következő szempontokat figyelembe hello hello import alapján jön létre.

* Egy .graphrunbook fájl csak a olyan új importálhatók [grafikus forgatókönyvnek](automation-runbook-types.md#graphical-runbooks), és csak grafikus forgatókönyvekhez hozhatók létre .graphrunbook fájlból.
* Egy PowerShell-munkafolyamat tartalmazó .ps1 fájl csak olyan importálhatók egy [PowerShell-munkafolyamati forgatókönyv](automation-runbook-types.md#powershell-workflow-runbooks).  Ha hello fájl több PowerShell-munkafolyamatok tartalmazza, majd hello importálása sikertelen lesz. Minden munkafolyamat tooits saját fájl mentése és importálása egyes külön-külön kell.
* Egy .ps1 fájlt, amely nem tartalmaz egy munkafolyamat vagy importálhatók egy [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) vagy egy [PowerShell-munkafolyamati forgatókönyv](automation-runbook-types.md#powershell-workflow-runbooks).  Ha való importálást egy PowerShell-munkafolyamati forgatókönyv, akkor azt lesz konvertált tooa munkafolyamat, és megjegyzéseket szerepelni fog a hello runbook elvégzett módosítások hello megadása.

### <a name="tooimport-a-runbook-from-a-file-with-hello-azure-classic-portal"></a>egy runbook hello klasszikus Azure portálon fájlból tooimport
Következő eljárás tooimport parancsfájl az Azure Automation hello is használhatja.  Vegye figyelembe, hogy csak importálhatja egy .ps1 fájlt egy PowerShell-munkafolyamati forgatókönyv használata ezen a portálon.  Egyéb hello Azure-portálon kell használnia.

1. Hello Azure felügyeleti portálon, válassza ki a **Automation** , és válassza az Automation-fiók.
2. Kattintson az **Importálás** gombra.
3. Kattintson a **fájl** , és keresse meg a hello parancsfájl fájl tooimport.
4. Ha most tooedit hello runbookot, majd kattintson **Runbook szerkesztése**. Ellenkező esetben kattintson az OK gombra.
5. hello új runbook megjelenik a hello **Runbookok** hello Automation-fiók lapján.
6. Meg kell [hello runbook közzététele](#publishing-a-runbook) előtt is futtathatja.

### <a name="tooimport-a-runbook-from-a-file-with-hello-azure-portal"></a>az Azure-portálon hello fájlból egy runbook tooimport
Következő eljárás tooimport parancsfájl az Azure Automation hello is használhatja.  

> [!NOTE]
> Vegye figyelembe, hogy csak importálhatja egy .ps1 fájlt egy PowerShell-munkafolyamati forgatókönyv hello portál használatával.
> 
> 

1. Hello Azure-portálon nyissa meg az Automation-fiók.
2. Hello Hub, válassza ki **Runbookok** runbookokat hello listája tooopen.
3. Kattintson a hello **hozzáadása egy runbook** gombra, majd **importálási**.
4. Kattintson a **Runbook fájl** tooselect hello fájl tooimport
5. Ha hello **neve** mező engedélyezve van, akkor el kell-e hello beállítás toochange azt.  hello runbook nevének betűvel kell kezdődnie, és rendelkezhetnek, betűket, számokat, aláhúzásjeleket és kötőjeleket.
6. Hello [runbooktípusba](automation-runbook-types.md) lesz automatikusan kiválasztva, de hello típusát hello vonatkozó korlátozások figyelembe vétele után módosíthatja. 
7. hello új runbook megjelenik a runbookokat hello Automation-fiók a hello listájában.
8. Meg kell [hello runbook közzététele](#publishing-a-runbook) előtt is futtathatja.

> [!NOTE]
> Egy grafikus forgatókönyvnek vagy egy grafikus PowerShell-munkafolyamati forgatókönyv importálása után hello beállítás tooconvert toohello más típusú Ha kívánta. Tootextual nem lehet konvertálni.
> 
> 

### <a name="tooimport-a-runbook-from-a-script-file-with-windows-powershell"></a>a Windows PowerShell parancsfájl runbookok tooimport
Használhatja a hello [Import-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) parancsmag tooimport vázlatként PowerShell-munkafolyamati forgatókönyv egy parancsfájlt. Ha hello runbook már létezik, hello importálása sikertelen lesz, ha nem használ hello *-Force* paraméter. 

hello a következő mintaparancsok bemutatják, hogyan tooimport parancsfájl forrásfájlt egy runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Runbook közzététele
Létrehozásakor, vagy egy új forgatókönyv importálása, közzé kell tennie, mielőtt futtatná azt.  Minden runbook Automation rendelkezik, egy Piszkozat és egy közzétett verziója. Csak a hello közzétett verzió elérhető toobe futtatni, és csak a hello piszkozat verzió szerkeszthető. hello közzétett verzió nem bármely módosítások toohello vázlatként megjelölt verziót. Amikor hello vázlatként megjelölt verziót elérhetővé kell tenni, majd közzététel amely hello közzétett verzió felülírja hello vázlatként megjelölt verziót.

## <a name="toopublish-a-runbook-using-hello-azure-classic-portal"></a>a runbook hello a klasszikus Azure portál használatával toopublish
1. Nyissa meg a hello runbookot hello a klasszikus Azure portálon.
2. Hello hello képernyő felső részén kattintson **Szerző**.
3. Hello a hello képernyő aljára, kattintson a **közzététel** , majd **Igen** toohello megerősítés kérésekor.

## <a name="toopublish-a-runbook-using-hello-azure-portal"></a>a runbook hello Azure-portál használatával toopublish
1. Nyissa meg a hello runbook hello Azure-portálon.
2. Kattintson a hello **szerkesztése** gombra.
3. Kattintson a hello **közzététel** gombra, majd **Igen** toohello megerősítés kérésekor.

## <a name="toopublish-a-runbook-using-windows-powershell"></a>toopublish egy runbook Windows PowerShell használatával
Használhatja a hello [Publish-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) runbookkal a Windows PowerShell parancsmag toopublish. hello következő minta parancsok megjelenítése hogyan toopublish a minta-runbookhoz.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Következő lépések
* toolearn kapcsolatos hogyan révén kihasználhatja a hello Runbook és a PowerShell modul gyűjteménye, lásd: [az Azure Automation forgatókönyv- és minták](automation-runbook-gallery.md)
* További információ a szöveges szerkesztőt, PowerShell és a PowerShell-munkafolyamati forgatókönyvek Szerkesztés toolearn lásd [szöveges az Azure Automation runbookjai szerkesztése](automation-edit-textual-runbook.md)
* toolearn grafikus forgatókönyvnek szerzői, kapcsolatos további információkért lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)

