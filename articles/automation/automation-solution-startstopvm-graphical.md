---
title: "aaaStarting és a virtuális gépek leállítása - diagramot |} Microsoft Docs"
description: "Azure Automation-forgatókönyv többek között a runbookok toostart, majd szüntesse meg a klasszikus virtuális gépek PowerShell munkafolyamat-verzió."
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
redirect_document_id: False
ms.openlocfilehash: 5add8d8cf35ea2e89a570744755ac7db0a6feb07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automation forgatókönyv - elindítása és leállítása a virtuális gépek
Az Azure Automation-forgatókönyv runbookok toostart, majd szüntesse meg a klasszikus virtuális gépeket tartalmaz.  Ebben a forgatókönyvben hello következő használható:  

* A saját környezetben runbookokat hello a módosítás nélkül használható.
* Módosítsa a hello runbookok tooperform testreszabott funkcióit.  
* Runbookokat hello hívható meg egy másik runbookból egy teljes megoldás részeként.
* Szerzői alapfogalmak oktatóanyagok toolearn runbook runbookokat hello használják.

> [!div class="op_single_selector"]
> * [Grafikus](automation-solution-startstopvm-graphical.md)
> * [PowerShell-munkafolyamat](automation-solution-startstopvm-psworkflow.md)
>
>

Ez a forgatókönyv hello grafikus forgatókönyvnek verziója telepítve. Akkor is elérhető használatával [PowerShell munkafolyamat-forgatókönyvekről](automation-solution-startstopvm-psworkflow.md).

## <a name="getting-hello-scenario"></a>Első hello forgatókönyv
Ebben a forgatókönyvben két áll, amely letölthető két grafikus forgatókönyvek hello hivatkozásokat követve.  Lásd: hello [PowerShell munkafolyamat-verzió](automation-solution-startstopvm-psworkflow.md) az ebben a forgatókönyvben PowerShell-munkafolyamati forgatókönyvek hivatkozások toohello.

| Forgatókönyv | Hivatkozás | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| StartAzureClassicVM |[Indítsa el az Azure klasszikus virtuális gép grafikus forgatókönyv](https://gallery.technet.microsoft.com/scriptcenter/Start-Azure-Classic-VM-c6067b3d) |Grafikus |Minden klasszikus virtuális gépek Azure-előfizetés vagy az összes virtuális gép egy adott szolgáltatáshoz nevű indítja el. |
| StopAzureClassicVM |[Grafikus forgatókönyv Azure klasszikus virtuális gép leállítása](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-Classic-VM-397819bd) |Grafikus |Automation-fiók összes virtuális gép vagy egy adott szolgáltatáshoz névvel rendelkező összes virtuális gép leáll. |

## <a name="installing-and-configuring-hello-scenario"></a>Telepítése és konfigurálása hello forgatókönyv
### <a name="1-install-hello-runbooks"></a>1. Runbookokat hello telepítése
Runbookokat hello a letöltés után importálhatja azokat a hello eljárással [grafikus forgatókönyvnek eljárások](automation-graphical-authoring-intro.md#graphical-runbook-procedures).

### <a name="2-review-hello-description-and-requirements"></a>2. Felülvizsgálati hello leírása és követelményei
runbookokat hello tartalmaznak nevű tevékenység **Readme** , amely tartalmazza a szükséges eszközök és leírását.  Hello kiválasztásával megtekintheti ezeket az információkat **Readme** tevékenység és hello **munkafolyamat-parancsfájl** paraméter.  Is megkapható hello ugyanazokat az információkat a cikkből.

### <a name="3-configure-assets"></a>3. Eszközök konfigurálása
runbookokat hello eszközöket, létre kell hoznia és rendelt megfelelő értékeket a következő hello igényelnek.  hello nevek alapértelmezett.  Is használhatja eszközök eltérő nevű Ha megadja ezeket a neveket a hello [bemeneti paraméterek](#using-the-runbooks) hello runbook indításakor.

| Eszköz típusa | Alapértelmezett neve | Leírás |
|:--- |:--- |:--- |:--- |
| [Hitelesítő adatok](automation-credentials.md) |AzureCredential |Egy Azure-előfizetés hello hatóság toostart és állítsa le a virtuális gépek rendelkező fiók hitelesítő adatait tartalmazza. |
| [Változó](automation-variables.md) |AzureSubscriptionId |Hello előfizetés-azonosító az Azure-előfizetés tartalmazza. |

## <a name="using-hello-scenario"></a>Hello forgatókönyv használata
### <a name="parameters"></a>Paraméterek
hello minden runbook alapvetően hello következő [bemeneti paraméterek](automation-starting-a-runbook.md#runbook-parameters).  Adjon meg értéket minden kötelező paraméterhez, és opcionálisan szükséges más paramétereket a követelményeitől függően.

| Paraméter | Típus | Kötelező | Leírás |
|:--- |:--- |:--- |:--- |
| Szolgáltatásnév |Karakterlánc |Nem |Ha egy érték áll rendelkezésre, majd az összes virtuális gép szolgáltatás nevű elindításakor vagy leállt.  Ha nincs érték megadva, majd minden hello Azure-előfizetéssel klasszikus virtuális gépek elindításakor vagy leállt. |
| AzureSubscriptionIdAssetName |Karakterlánc |Nem |Hello hello nevét tartalmazza [változóeszköz](#installing-and-configuring-the-scenario) , amely tartalmazza az Azure-előfizetéshez hello előfizetés-azonosítója.  Ha nem adja meg egy értéket *AzureSubscriptionId* szolgál. |
| AzureCredentialAssetName |Karakterlánc |Nem |Hello hello nevét tartalmazza [hitelesítőadat-eszköz](#installing-and-configuring-the-scenario) tartalmazó hello runbook toouse hello hitelesítő adatait.  Ha nem adja meg egy értéket *AzureCredential* szolgál. |

### <a name="starting-hello-runbooks"></a>Runbookokat hello indítása
Hello módszerek bármelyikét használhatja [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) toostart vagy runbookokat hello a ebben a cikkben.

a következő Példaparancsok hello használja a Windows PowerShell toorun **StartAzureClassicVM** toostart hello szolgáltatás névvel rendelkező összes virtuális gép *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "StartAzureClassicVM" –Parameters $params

### <a name="output"></a>Kimenet
runbookokat hello fog [kimeneti üzenet](automation-runbook-output-and-messages.md) minden virtuális gép, amely azt jelzi-e hello indítási vagy leállítási utasítás sikeresen el lett küldve.  Kereshet az hello kimeneti toodetermine hello eredmények minden runbook egy adott karakterláncot.  a következő táblázat hello hello lehetséges kimeneti karakterláncok listáját.

| Forgatókönyv | Az állapot | Üzenet |
|:--- |:--- |:--- |
| StartAzureClassicVM |Virtuális gép már fut. |MyVM már fut. |
| StartAzureClassicVM |Kérelem küldése sikeres volt a virtuális gép indítása |MyVM el lett indítva. |
| StartAzureClassicVM |Nem sikerült a virtuális gép indítási kérésre |Nem sikerült MyVM toostart |
| StopAzureClassicVM |Virtuális gép már fut. |MyVM már le van állítva. |
| StopAzureClassicVM |Kérelem küldése sikeres volt a virtuális gép indítása |MyVM el lett indítva. |
| StopAzureClassicVM |Nem sikerült a virtuális gép indítási kérésre |Nem sikerült MyVM toostart |

Az alábbiakban látható egy képet hello segítségével **StartAzureClassicVM** , egy [gyermekrunbook](automation-child-runbooks.md) egy minta grafikus runbook.  A következő táblázat hello hello feltételes hivatkozások használja.

| Hivatkozás | Feltételek |
|:--- |:--- |
| Sikeres kapcsolat |[StartAzureClassicVM] $ActivityOutput – például a "\* elindult" |
| Hiba történt hivatkozás |[StartAzureClassicVM] $ActivityOutput-notlike "\* elindult" |

![Gyermek runbook – példa](media/automation-solution-startstopvm/graphical-childrunbook-example.png)

## <a name="detailed-breakdown"></a>Részletes információkat
Az alábbiakban látható az ebben a forgatókönyvben hello runbookok részletes információkat.  Ezen információk tooeither runbookokat hello vagy ezekből a saját automatizálási esetekben tartalomkészítéshez csak toolearn testreszabása.

### <a name="authentication"></a>Authentication
![Authentication](media/automation-solution-startstopvm/graphical-authentication.png)

hello runbook tevékenységek tooset hello kezdődik [hitelesítő adatok](automation-credentials.md) és az Azure-előfizetéssel, amely jelzi a hello rest hello runbook.

első két tevékenység hello **előfizetés-azonosító beszerzése** és **első Azure hitelesítő**, hello beolvasása [eszközök](#installing-the-runbook) hello következő két tevékenység által használt.  Adott tevékenységek közvetlenül hello eszközök kell megadni, de szükségük hello eszköz nevét.  Mivel azt engedélyezi a felhasználó toospecify hello ezeket a neveket a hello [bemeneti paraméterek](#using-the-runbooks), ezeket a tevékenységeket tooretrieve hello eszközöket kell egy bemeneti paraméter által megadott névvel.

**Adja hozzá AzureAccount** beállítása hello hello rest hello runbook használt hitelesítő adatait.  hello hitelesítőadat-eszköz, amely lekérdezi a **első Azure hitelesítő** hozzáférés toostart és állítsa le a virtuális gépek hello Azure-előfizetéssel kell rendelkeznie.  hello használt előfizetés szerint ki van választva **válasszon-AzureSubscription** hello előfizetés-azonosítót használó a **előfizetés-azonosító beszerzése**.

### <a name="get-virtual-machines"></a>Virtuális gépeinek beolvasása
![Virtuális gépek beolvasása](media/automation-solution-startstopvm/graphical-getvms.png)

hello runbook kell a virtuális gépek azt fogja dolgozni a toodetermine, és hogy azok vannak már elindított vagy leállított (attól függően, hogy hello runbook).   Hello virtuális gépek két tevékenység egyik kér le.  **Virtuális gépek beolvasása a szolgáltatás** fog futni, ha hello *szolgáltatásnév* hello runbook bemeneti paraméter értéke.  **Minden virtuális gép első** fog futni, ha hello *szolgáltatásnév* hello runbook bemeneti paraméter nem tartalmaz értéket.  A működési elvet minden tevékenység megelőző hello feltételes hivatkozások végzi el.

Mindkét tevékenységek használata hello **Get-AzureVM** parancsmag.  **Minden virtuális gép első** használ hello **ListAllVMs** paraméter beállítása tooreturn összes virtuális gépet.  **Beolvasása a virtuális gépek szolgáltatás** használ hello **GetVMByServiceAndVMName** paraméter beállítása és hello biztosít **szolgáltatásnév** hello bemeneti paramétere **szolgáltatásnév**paraméter.  

### <a name="merge-vms"></a>Virtuális gépek egyesítése
![Virtuális gépek egyesítése](media/automation-solution-startstopvm/graphical-mergevms.png)

Hello **egyesítése virtuális gépek** tevékenysége túl bemeneti szükséges tooprovide**Start-AzureVM** amely kell hello és virtuális gép van toostart hello szolgáltatás nevét.  Hogy bemeneti származhat vagy **minden virtuális gép első** vagy **virtuális gépeinek beolvasása szolgáltatás**, de **Start-AzureVM** csak adhatja meg a bemeneti egy tevékenység.   

hello forgatókönyv, toocreate **egyesítése virtuális gépek** amely fut hello **Write-Output** parancsmag.  Hello **InputObject** parancsmagot paramétere, amely egyesíti az előző két hello tevékenységek hello bemeneti PowerShell-kifejezést.  Ezek a tevékenységek csak az egyik, csak a várt kimeneti egy készletét fog futni.  **Start-AzureVM** kimenet használhatja a bemeneti paraméterek.

### <a name="startstop-virtual-machines"></a>Virtuális gépek indítása és leállítása
![Indítsa el a virtuális gépek](media/automation-solution-startstopvm/graphical-startvm.png) ![Állítsa le a virtuális gépek](media/automation-solution-startstopvm/graphical-stopvm.png)

Hello runbooktól függő hello következő tevékenységek toostart kísérletet, vagy állítsa le a hello powershellel **Start-AzureVM** vagy **Stop-AzureVM**.  Mivel hello tevékenység egy folyamatkapcsolódása előzi meg, akkor fog futni egyszer minden objektum által visszaadott **egyesítése virtuális gépek**.  hello kapcsolat feltételes, hogy hello tevékenység csak fog futni, ha hello *RunningState* hello a virtuális gép van *leállítva* a **Start-AzureVM** és  *Lépések* a **Stop-AzureVM**. Ha ez a feltétel nem teljesül, majd **értesítés már el van indítva** vagy **értesítés már le van állítva** toosend futtathatók, egy üzenet használatával **Write-Output**.

### <a name="send-output"></a>Kimenetként
![Kezdő virtuális gépek értesítése](media/automation-solution-startstopvm/graphical-notifystart.png) ![Állítsa le virtuális gépek értesítése](media/automation-solution-startstopvm/graphical-notifystop.png)

utolsó lépésként hello hello runbook toosend kimeneti, hogy a start hello, vagy az egyes virtuális gépek leállítási kérelem küldése sikeres volt. Nincs külön **Write-Output** tevékenységet minden egyes, és a rendszer meghatározza, mely egy toorun feltételes hivatkozásokkal.  **Értesíti a virtuális gép indítása** vagy **VM leállítása értesítés** akkor fut, ha *OperationStatus* van *sikeres*.  Ha *OperationStatus* bármely olyan értéket, majd az **nem sikerült értesíteni tooStart** vagy **nem sikerült értesíteni tooStop** futtatása.

## <a name="next-steps"></a>Következő lépések
* [Grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)
* [Azure Automation runbookjai gyermek](automation-child-runbooks.md)
* [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)
