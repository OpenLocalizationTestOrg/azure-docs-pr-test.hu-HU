---
title: "Indítása és leállítása a virtuális gépek - diagram |} Microsoft Docs"
description: "Azure Automation-forgatókönyv többek között a runbookok elindítására és leállítására klasszikus virtuális gépek PowerShell munkafolyamat-verzió."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 62d93170-ca3d-42ef-ac1d-6d50b61ac87d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: FALSE
ms.openlocfilehash: 338d5712239356e13cbf480d9655ca3ca499701d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automation forgatókönyv - elindítása és leállítása a virtuális gépek
Az Azure Automation-forgatókönyv runbookok elindítása és leállítása a klasszikus virtuális gépeket tartalmaz.  Ebben a forgatókönyvben a következő használható:  

* A saját környezetben használható a runbookok módosítás nélkül.
* Módosítsa a runbookok testreszabott funkcióinak végrehajtásához.  
* Hívja a runbookok másik runbookból egy teljes megoldás részeként.
* A runbookok használják oktatóanyagok további runbook fogalmak szerzői.

> [!div class="op_single_selector"]
> * [Grafikus](automation-solution-startstopvm-graphical.md)
> * [PowerShell-munkafolyamat](automation-solution-startstopvm-psworkflow.md)
>
>

Ez az a forgatókönyv grafikus forgatókönyv verzióját. Akkor is elérhető használatával [PowerShell munkafolyamat-forgatókönyvekről](automation-solution-startstopvm-psworkflow.md).

## <a name="getting-the-scenario"></a>A forgatókönyv beszerzése
Ebben a forgatókönyvben két áll, amely letölthető a következő hivatkozások két grafikus forgatókönyvek.  Tekintse meg a [PowerShell munkafolyamat-verzió](automation-solution-startstopvm-psworkflow.md) az ebben a forgatókönyvben a PowerShell-munkafolyamati forgatókönyvek mutató hivatkozásokat tartalmaz.

| Forgatókönyv | Hivatkozás | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| StartAzureClassicVM |[Indítsa el az Azure klasszikus virtuális gép grafikus forgatókönyv](https://gallery.technet.microsoft.com/scriptcenter/Start-Azure-Classic-VM-c6067b3d) |Grafikus |Minden klasszikus virtuális gépek Azure-előfizetés vagy az összes virtuális gép egy adott szolgáltatáshoz nevű indítja el. |
| StopAzureClassicVM |[Grafikus forgatókönyv Azure klasszikus virtuális gép leállítása](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-Classic-VM-397819bd) |Grafikus |Automation-fiók összes virtuális gép vagy egy adott szolgáltatáshoz névvel rendelkező összes virtuális gép leáll. |

## <a name="installing-and-configuring-the-scenario"></a>Telepítése és konfigurálása a forgatókönyv
### <a name="1-install-the-runbooks"></a>1. A runbookok telepítése
A runbookok a letöltés után importálhatja azokat a [grafikus forgatókönyvnek eljárások](automation-graphical-authoring-intro.md#graphical-runbook-procedures).

### <a name="2-review-the-description-and-requirements"></a>2. Tekintse át a leírása és követelményei
A runbookok tartalmaznak nevű tevékenység **Readme** , amely tartalmazza a szükséges eszközök és leírását.  Ezek az információk kiválasztásával megtekintheti a **Readme** tevékenység, majd a **munkafolyamat-parancsfájl** paraméter.  A cikkből is megkapható ugyanazokat az információkat.

### <a name="3-configure-assets"></a>3. Eszközök konfigurálása
A runbookok a következő eszközök, létre kell hoznia és megfelelő értékekkel feltöltéséhez szükséges.  A nevek alapértelmezett.  Eszközök is eltérő nevű használja, ha megadja ezeket a neveket a [bemeneti paraméterek](#using-the-runbooks) a runbook indításakor.

| Eszköz típusa | Alapértelmezett neve | Leírás |
|:--- |:--- |:--- |:--- |
| [Hitelesítő adatok](automation-credentials.md) |AzureCredential |Egy olyan fiók, amely rendelkezik a szolgáltató indítása és leállítása a virtuális gépek az Azure-előfizetés hitelesítő adatokat tartalmaz. |
| [Változó](automation-variables.md) |AzureSubscriptionId |Az előfizetés-azonosító az Azure-előfizetés tartalmazza. |

## <a name="using-the-scenario"></a>A forgatókönyv használata
### <a name="parameters"></a>Paraméterek
A runbookok egyes rendelkezik a következő [bemeneti paraméterek](automation-starting-a-runbook.md#runbook-parameters).  Adjon meg értéket minden kötelező paraméterhez, és opcionálisan szükséges más paramétereket a követelményeitől függően.

| Paraméter | Típus | Kötelező | Leírás |
|:--- |:--- |:--- |:--- |
| Szolgáltatásnév |Karakterlánc |Nem |Ha egy érték áll rendelkezésre, majd az összes virtuális gép szolgáltatás nevű elindításakor vagy leállt.  Ha nincs érték megadva, majd az Azure-előfizetés összes klasszikus virtuális gépek elindításakor vagy leállt. |
| AzureSubscriptionIdAssetName |Karakterlánc |Nem |A neve tartalmazza a [változóeszköz](#installing-and-configuring-the-scenario) , amely tartalmazza az előfizetés-azonosítója az Azure-előfizetéshez.  Ha nem adja meg egy értéket *AzureSubscriptionId* szolgál. |
| AzureCredentialAssetName |Karakterlánc |Nem |A neve tartalmazza a [hitelesítőadat-eszköz](#installing-and-configuring-the-scenario) , amely tartalmazza a runbook használandó hitelesítő adatait.  Ha nem adja meg egy értéket *AzureCredential* szolgál. |

### <a name="starting-the-runbooks"></a>A runbookok elindítása
A módszerek bármelyikét használhatja [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) ebben a cikkben a runbookok indítása.

Az alábbi Példaparancsok Windows PowerShell használatával futtassa **StartAzureClassicVM** az összes virtuális gép elindítása a szolgáltatás neve *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "StartAzureClassicVM" –Parameters $params

### <a name="output"></a>Kimenet
A runbookok fog [kimeneti üzenet](automation-runbook-output-and-messages.md) minden egyes virtuális géphez, vagy nem sikerült elküldeni a indítási vagy leállítási utasítás jelző.  Kereshet egy adott karakterláncot a kimenetben minden runbook eredmény meghatározására.  A lehetséges kimeneti karakterláncok a következő táblázatban láthatók.

| Forgatókönyv | Az állapot | Üzenet |
|:--- |:--- |:--- |
| StartAzureClassicVM |Virtuális gép már fut. |MyVM már fut. |
| StartAzureClassicVM |Kérelem küldése sikeres volt a virtuális gép indítása |MyVM el lett indítva. |
| StartAzureClassicVM |Nem sikerült a virtuális gép indítási kérésre |Nem sikerült elindítani a MyVM |
| StopAzureClassicVM |Virtuális gép már fut. |MyVM már le van állítva. |
| StopAzureClassicVM |Kérelem küldése sikeres volt a virtuális gép indítása |MyVM el lett indítva. |
| StopAzureClassicVM |Nem sikerült a virtuális gép indítási kérésre |Nem sikerült elindítani a MyVM |

Az alábbiakban látható a lemezkép használatával a **StartAzureClassicVM** , egy [gyermekrunbook](automation-child-runbooks.md) egy minta grafikus runbook.  A feltételes hivatkozások használja az alábbi táblázatban.

| Hivatkozás | Feltételek |
|:--- |:--- |
| Sikeres kapcsolat |[StartAzureClassicVM] $ActivityOutput – például a "\* elindult" |
| Hiba történt hivatkozás |[StartAzureClassicVM] $ActivityOutput-notlike "\* elindult" |

![Gyermek runbook – példa](media/automation-solution-startstopvm/graphical-childrunbook-example.png)

## <a name="detailed-breakdown"></a>Részletes információkat
Az alábbiakban látható az ebben a forgatókönyvben a runbookok részletes információkat.  Ez az információ segítségével testre szabhatja a runbookokat, vagy csak a saját automatizálási esetekben tartalomkészítéshez azokat a további.

### <a name="authentication"></a>Authentication
![Authentication](media/automation-solution-startstopvm/graphical-authentication.png)

A runbook tevékenységek beállítása elindítja a [hitelesítő adatok](automation-credentials.md) és az Azure-előfizetéssel, amely jelzi a runbook a többi.

Az első két tevékenység **előfizetés-azonosító beszerzése** és **első Azure hitelesítő**, beolvasni a [eszközök](#installing-the-runbook) a következő két tevékenység által használt.  Ezek a tevékenységek közvetlenül kell megadni az eszközök, de szükségük van az eszköz nevét.  Mivel ezeket a neveket adni a felhasználó engedélyezi azt a [bemeneti paraméterek](#using-the-runbooks), igazolnia kell, hogy ezek a tevékenységek az eszközök beolvasni egy bemeneti paraméter által megadott névvel.

**Adja hozzá AzureAccount** a runbook a többi használandó hitelesítő adatok beállítása.  A hitelesítőadat-eszköz, amely lekérdezi a **első Azure hitelesítő** indítása és leállítása a virtuális gépek az Azure-előfizetésben hozzáféréssel kell rendelkeznie.  Az előfizetés használt szerint ki van választva **válasszon-AzureSubscription** az előfizetés-azonosítót használó a **előfizetés-azonosító beszerzése**.

### <a name="get-virtual-machines"></a>Virtuális gépeinek beolvasása
![Virtuális gépek beolvasása](media/automation-solution-startstopvm/graphical-getvms.png)

A runbook meg kell állapítania, a rendszer használata virtuális gépek, és hogy azok vannak már elindított vagy leállított (attól függően, hogy a runbook).   Két tevékenység egyik kér le a virtuális gépeket.  **Virtuális gépek beolvasása a szolgáltatás** fog futni, ha a *szolgáltatásnév* bemeneti paraméter a runbook értéket tartalmaz.  **Minden virtuális gép első** fog futni, ha a *szolgáltatásnév* bemeneti paraméter a runbook nem tartalmaz értéket.  Ez a módszer az egyes tevékenységek megelőző feltételes hivatkozások végzi el.

Mindkét tevékenységek használata a **Get-AzureVM** parancsmag.  **Minden virtuális gép első** használja a **ListAllVMs** paraméter állítsa vissza az összes virtuális gépet.  **Beolvasása a virtuális gépek szolgáltatás** használja a **GetVMByServiceAndVMName** paramétert állítsa be, és biztosítja a **szolgáltatásnév** bemeneti paramétere a **szolgáltatásnév** a paraméter.  

### <a name="merge-vms"></a>Virtuális gépek egyesítése
![Virtuális gépek egyesítése](media/automation-solution-startstopvm/graphical-mergevms.png)

A **egyesítése virtuális gépek** tevékenysége kell megadnia a bemeneti **Start-AzureVM** amely van szüksége, és a szolgáltatás nevét elindítani a virtuális gép van.  Hogy bemeneti származhat vagy **minden virtuális gép első** vagy **virtuális gépeinek beolvasása szolgáltatás**, de **Start-AzureVM** csak adhatja meg a bemeneti egy tevékenység.   

A forgatókönyvben, ha a **egyesítése virtuális gépek** mely fut a **Write-Output** parancsmag.  A **InputObject** parancsmagot paramétere, hogy az előző két tevékenység bevitelt PowerShell-kifejezést.  Ezek a tevékenységek csak az egyik, csak a várt kimeneti egy készletét fog futni.  **Start-AzureVM** kimenet használhatja a bemeneti paraméterek.

### <a name="startstop-virtual-machines"></a>Virtuális gépek indítása és leállítása
![Indítsa el a virtuális gépek](media/automation-solution-startstopvm/graphical-startvm.png) ![Állítsa le a virtuális gépek](media/automation-solution-startstopvm/graphical-stopvm.png)

Attól függően, hogy a runbook a következő tevékenységek megpróbálja indítása vagy leállítása a powershellel **Start-AzureVM** vagy **Stop-AzureVM**.  Mivel a tevékenység egy folyamatkapcsolódása előzi meg, akkor fog futni egyszer minden objektum által visszaadott **egyesítése virtuális gépek**.  A hivatkozás nem feltételes, hogy a tevékenység csak fog futni, ha a *RunningState* a virtuális gép *leállítva* a **Start-AzureVM** és *elindítva*  a **Stop-AzureVM**. Ha ez a feltétel nem teljesül, majd **értesítés már el van indítva** vagy **értesítés már le van állítva** Futtatás való üzenetküldéshez használ **Write-Output**.

### <a name="send-output"></a>Kimenetként
![Kezdő virtuális gépek értesítése](media/automation-solution-startstopvm/graphical-notifystart.png) ![Állítsa le virtuális gépek értesítése](media/automation-solution-startstopvm/graphical-notifystop.png)

Az utolsó lépés a runbook-hoz kimenetként, hogy az egyes virtuális gépek indítási vagy leállítási kérelem küldése sikeres volt. Nincs külön **Write-Output** tevékenységet minden egyes, és azt határozza meg, mely egy feltételes hivatkozásokkal futtatásához.  **Értesíti a virtuális gép indítása** vagy **VM leállítása értesítés** akkor fut, ha *OperationStatus* van *sikeres*.  Ha *OperationStatus* bármely olyan értéket, majd az **értesítés nem tudta indítsa el a** vagy **értesítés leállítása nem sikerült** futtatása.

## <a name="next-steps"></a>Következő lépések
* [Grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)
* [Azure Automation runbookjai gyermek](automation-child-runbooks.md)
* [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)
