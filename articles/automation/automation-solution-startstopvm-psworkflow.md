---
title: "Indítása és leállítása a virtuális gépek az Azure Automation - PowerShell munkafolyamat |} Microsoft Docs"
description: "Többek között a runbookok elindítására és leállítására klasszikus virtuális gépek Azure Automation-forgatókönyv grafikus verzióját."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d380bd43-d45d-45af-a5b2-78e7f66263c3
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: FALSE
ms.openlocfilehash: 95a7b02b0d11bf18c398daea48d16e0ead30b543
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

Ez az ebben a forgatókönyvben PowerShell-munkafolyamati forgatókönyv verziója. Akkor is elérhető használatával [grafikus forgatókönyvek](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>A forgatókönyv beszerzése
Ebben a forgatókönyvben két PowerShell-munkafolyamati forgatókönyvek tölthet le a következő hivatkozások áll.  Tekintse meg a [grafikus verzió](automation-solution-startstopvm-graphical.md) az ebben a forgatókönyvben a grafikus forgatókönyvek mutató hivatkozásokat tartalmaz.

| Forgatókönyv | Hivatkozás | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| Start-AzureVMs |[Az Azure klasszikus virtuális gépek elindítása](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |PowerShell-munkafolyamat |Indítja el az összes klasszikus virtuális gép egy Azure subscriptionor az összes virtuális gép egy adott szolgáltatáshoz nevű. |
| STOP-AzureVMs |[Az Azure klasszikus virtuális gépek leállítása](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |PowerShell-munkafolyamat |Automation-fiók összes virtuális gép vagy egy adott szolgáltatáshoz névvel rendelkező összes virtuális gép leáll. |

## <a name="installing-and-configuring-the-scenario"></a>Telepítése és konfigurálása a forgatókönyv
### <a name="1-install-the-runbooks"></a>1. A runbookok telepítése
A runbookok a letöltés után importálhatja azokat a [olyan Runbookot importálna](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. Tekintse át a leírása és követelményei
A runbookok megjegyzésként súgószöveg, amely tartalmazza a szükséges eszközök és leírását tartalmazza.  A cikkből is megkapható ugyanazokat az információkat.

### <a name="3-configure-assets"></a>3. Eszközök konfigurálása
A runbookok a következő eszközök, létre kell hoznia és megfelelő értékekkel feltöltéséhez szükséges.

| Eszköz típusa | Eszköz neve | Leírás |
|:--- |:--- |:--- |:--- |
| Hitelesítő adat |AzureCredential |Egy olyan fiók, amely rendelkezik a szolgáltató indítása és leállítása a virtuális gépek az Azure-előfizetés hitelesítő adatokat tartalmaz.  Azt is megteheti, megadhat egy másik, a hitelesítőadat-eszköz a **hitelesítő adatok** paramétere a **Add-AzureAccount** tevékenység. |
| Változó |AzureSubscriptionId |Az előfizetés-azonosító az Azure-előfizetés tartalmazza. |

## <a name="using-the-scenario"></a>A forgatókönyv használata
### <a name="parameters"></a>Paraméterek
A runbookok egyes az alábbi paraméterekkel rendelkezik.  Adjon meg értéket minden kötelező paraméterhez, és opcionálisan szükséges más paramétereket a követelményeitől függően.

| Paraméter | Típus | Kötelező | Leírás |
|:--- |:--- |:--- |:--- |
| Szolgáltatásnév |Karakterlánc |Nem |Ha egy érték áll rendelkezésre, majd az összes virtuális gép szolgáltatás nevű elindításakor vagy leállt.  Ha nincs érték megadva, majd az Azure-előfizetés összes klasszikus virtuális gépek elindításakor vagy leállt. |
| AzureSubscriptionIdAssetName |Karakterlánc |Nem |A neve tartalmazza a [változóeszköz](#installing-and-configuring-the-scenario) , amely tartalmazza az előfizetés-azonosítója az Azure-előfizetéshez.  Ha nem adja meg egy értéket *AzureSubscriptionId* szolgál. |
| AzureCredentialAssetName |Karakterlánc |Nem |A neve tartalmazza a [hitelesítőadat-eszköz](#installing-and-configuring-the-scenario) , amely tartalmazza a runbook használandó hitelesítő adatait.  Ha nem adja meg egy értéket *AzureCredential* szolgál. |

### <a name="starting-the-runbooks"></a>A runbookok elindítása
A módszerek bármelyikét használhatja [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) ebben a forgatókönyvben a runbookok indítása.

Az alábbi Példaparancsok Windows PowerShell használatával futtassa **StartAzureVMs** az összes virtuális gép elindítása a szolgáltatás neve *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Kimenet
A runbookok fog [kimeneti üzenet](automation-runbook-output-and-messages.md) minden egyes virtuális géphez, vagy nem sikerült elküldeni a indítási vagy leállítási utasítás jelző.  Kereshet egy adott karakterláncot a kimenetben minden runbook eredmény meghatározására.  A lehetséges kimeneti karakterláncok a következő táblázatban láthatók.

| Forgatókönyv | Az állapot | Üzenet |
|:--- |:--- |:--- |
| Start-AzureVMs |Virtuális gép már fut. |MyVM már fut. |
| Start-AzureVMs |Kérelem küldése sikeres volt a virtuális gép indítása |MyVM el lett indítva. |
| Start-AzureVMs |Nem sikerült a virtuális gép indítási kérésre |Nem sikerült elindítani a MyVM |
| STOP-AzureVMs |Virtuális gép már le van állítva. |MyVM már le van állítva. |
| STOP-AzureVMs |Kérelem küldése sikeres volt a virtuális gép leállítása |MyVM le lett állítva. |
| STOP-AzureVMs |A virtuális gép leállítási kérelem sikertelen volt |Nem sikerült leállítani a MyVM |

Például a következő kódrészletet egy runbookból megkísérli elindítani az összes virtuális gép szolgáltatásnévvel *MyServiceName*.  Ha vannak ilyenek a start kérelmek sikertelenek, majd hiba műveleteket lehessen állítani.

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Részletes információkat
Az alábbiakban látható az ebben a forgatókönyvben a runbookok részletes információkat.  Ez az információ segítségével testre szabhatja a runbookokat, vagy csak a saját automatizálási esetekben tartalomkészítéshez azokat a további.

### <a name="parameters"></a>Paraméterek
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

A munkafolyamat által az értékek első elindul a [bemeneti paraméterek](#using-the-scenario).  Ha az eszköz neve nincs megadva alapértelmezett neveket használnak.

### <a name="output"></a>Kimenet
    # Returns strings with status messages
    [OutputType([String])]

A sor deklarálja, hogy a runbook kimenete lesz-e egy karakterláncot.  Ez nincs szükség, de a runbook használatakor az ajánlott eljárás egy [gyermekrunbook](automation-child-runbooks.md) , hogy a szülő runbook fogja tudni a várt kimeneti típus.

### <a name="authentication"></a>Authentication
    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

A következő sor beállítása a [hitelesítő adatok](automation-credentials.md) és jelzi a runbook a többi Azure-előfizetéshez.
Először használjuk **Get-AutomationPSCredential** , amely tárolja a hitelesítő adatok indítása és leállítása a virtuális gépek Azure-előfizetést a hozzáférést az eszköz eléréséhez. **Adja hozzá AzureAccount** Ez az eszköz használatával állítsa be a hitelesítő adatokat.  A kimeneti van rendelve egy üres változó, így azt nem található meg a runbook-kimenet.  

Az azonosító visszakeresése az előfizetéshez a változóeszköz **Get-AutomationVariable** és állítható be az előfizetés **válasszon-AzureSubscription**.

### <a name="get-vms"></a>Virtuális gépek beolvasása
    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

**Get-AzureVM** segítségével kérhető le a virtuális gépeket, a runbook működni fog-e.  Ha az érték megtalálható a **szolgáltatásnév** bemeneti változót, akkor a rendszer beolvassa a szolgáltatás neve csak a virtuális gépek.  Ha **szolgáltatásnév** üres, akkor a rendszer beolvassa az összes virtuális gép.

### <a name="startstop-virtual-machines-and-send-output"></a>Virtuális gépek indítása/leállítása és kimenetként
    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

A következő sorokban minden egyes virtuális gép lépéseit.  Első a **PowerState** a virtuális gép be van jelölve meg, ha már fut vagy leállt, attól függően, hogy a runbook.  Ha már van a cél állapotban, majd egy üzenettel történő megjelenítéshez és a runbook véget ér.  Ha nem, majd **Start-AzureVM** vagy **Stop-AzureVM** próbálja újból elindítani vagy leállítani a virtuális gépet, a kérelem egy változóhoz tárolt eredményével szolgál.  Egy üzenet majd kimeneti megadása, hogy indítása vagy leállítása a kérelem sikeresen elküldve.

## <a name="next-steps"></a>Következő lépések
* Gyermek runbookok használatával kapcsolatos további információkért lásd: [gyermek az Azure Automation runbookjai](automation-child-runbooks.md)
* További információt a kimeneti üzenetek során a runbook végrehajtása és a hibaelhárítás elősegítése érdekében naplózása, lásd: [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)

