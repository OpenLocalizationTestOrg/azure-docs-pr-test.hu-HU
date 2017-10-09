---
title: "Azure Automation - PowerShell munkafolyamat aaaStarting és leállításával virtuális gépek |} Microsoft Docs"
description: "Grafikus verzióját, beleértve a runbookok toostart, majd szüntesse meg a klasszikus virtuális gépek Azure Automation-forgatókönyv."
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
redirect_document_id: False
ms.openlocfilehash: 273631c7fc5ddb989b3bbdc82b470ac3af6ee482
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

Ez a hello ebben a forgatókönyvben PowerShell-munkafolyamati forgatókönyv verzióját. Akkor is elérhető használatával [grafikus forgatókönyvek](automation-solution-startstopvm-graphical.md).

## <a name="getting-hello-scenario"></a>Első hello forgatókönyv
Ebben a forgatókönyvben két PowerShell-munkafolyamati forgatókönyvek tölthet le a következő hivatkozások hello áll.  Lásd: hello [grafikus verzió](automation-solution-startstopvm-graphical.md) az ebben a forgatókönyvben hivatkozások toohello grafikus forgatókönyvekhez.

| Forgatókönyv | Hivatkozás | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| Start-AzureVMs |[Az Azure klasszikus virtuális gépek elindítása](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |PowerShell-munkafolyamat |Indítja el az összes klasszikus virtuális gép egy Azure subscriptionor az összes virtuális gép egy adott szolgáltatáshoz nevű. |
| STOP-AzureVMs |[Az Azure klasszikus virtuális gépek leállítása](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |PowerShell-munkafolyamat |Automation-fiók összes virtuális gép vagy egy adott szolgáltatáshoz névvel rendelkező összes virtuális gép leáll. |

## <a name="installing-and-configuring-hello-scenario"></a>Telepítése és konfigurálása hello forgatókönyv
### <a name="1-install-hello-runbooks"></a>1. Runbookokat hello telepítése
Runbookokat hello a letöltés után importálhatja azokat a hello eljárással [olyan Runbookot importálna](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-hello-description-and-requirements"></a>2. Felülvizsgálati hello leírása és követelményei
runbookokat hello megjegyzésként súgószöveg, amely tartalmazza a szükséges eszközök és leírását tartalmazza.  Is megkapható hello ugyanazokat az információkat a cikkből.

### <a name="3-configure-assets"></a>3. Eszközök konfigurálása
runbookokat hello eszközöket, létre kell hoznia és rendelt megfelelő értékeket a következő hello igényelnek.

| Eszköz típusa | Eszköz neve | Leírás |
|:--- |:--- |:--- |:--- |
| Hitelesítő adat |AzureCredential |Egy Azure-előfizetés hello hatóság toostart és állítsa le a virtuális gépek rendelkező fiók hitelesítő adatait tartalmazza.  Másik lehetőségként megadhat egy másik hitelesítőadat-eszköz hello **Credential** hello paramétere **Add-AzureAccount** tevékenység. |
| Változó |AzureSubscriptionId |Hello előfizetés-azonosító az Azure-előfizetés tartalmazza. |

## <a name="using-hello-scenario"></a>Hello forgatókönyv használata
### <a name="parameters"></a>Paraméterek
hello runbookokat hello a következő paraméterekkel rendelkezik.  Adjon meg értéket minden kötelező paraméterhez, és opcionálisan szükséges más paramétereket a követelményeitől függően.

| Paraméter | Típus | Kötelező | Leírás |
|:--- |:--- |:--- |:--- |
| Szolgáltatásnév |Karakterlánc |Nem |Ha egy érték áll rendelkezésre, majd az összes virtuális gép szolgáltatás nevű elindításakor vagy leállt.  Ha nincs érték megadva, majd minden hello Azure-előfizetéssel klasszikus virtuális gépek elindításakor vagy leállt. |
| AzureSubscriptionIdAssetName |Karakterlánc |Nem |Hello hello nevét tartalmazza [változóeszköz](#installing-and-configuring-the-scenario) , amely tartalmazza az Azure-előfizetéshez hello előfizetés-azonosítója.  Ha nem adja meg egy értéket *AzureSubscriptionId* szolgál. |
| AzureCredentialAssetName |Karakterlánc |Nem |Hello hello nevét tartalmazza [hitelesítőadat-eszköz](#installing-and-configuring-the-scenario) tartalmazó hello runbook toouse hello hitelesítő adatait.  Ha nem adja meg egy értéket *AzureCredential* szolgál. |

### <a name="starting-hello-runbooks"></a>Runbookokat hello indítása
Hello módszerek bármelyikét használhatja [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) toostart runbookokat hello a ebben a forgatókönyvben egyik sem.

a következő Példaparancsok hello használja a Windows PowerShell toorun **StartAzureVMs** toostart hello szolgáltatás névvel rendelkező összes virtuális gép *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Kimenet
runbookokat hello fog [kimeneti üzenet](automation-runbook-output-and-messages.md) minden virtuális gép, amely azt jelzi-e hello indítási vagy leállítási utasítás sikeresen el lett küldve.  Kereshet az hello kimeneti toodetermine hello eredmények minden runbook egy adott karakterláncot.  a következő táblázat hello hello lehetséges kimeneti karakterláncok listáját.

| Forgatókönyv | Az állapot | Üzenet |
|:--- |:--- |:--- |
| Start-AzureVMs |Virtuális gép már fut. |MyVM már fut. |
| Start-AzureVMs |Kérelem küldése sikeres volt a virtuális gép indítása |MyVM el lett indítva. |
| Start-AzureVMs |Nem sikerült a virtuális gép indítási kérésre |Nem sikerült MyVM toostart |
| STOP-AzureVMs |Virtuális gép már le van állítva. |MyVM már le van állítva. |
| STOP-AzureVMs |Kérelem küldése sikeres volt a virtuális gép leállítása |MyVM le lett állítva. |
| STOP-AzureVMs |A virtuális gép leállítási kérelem sikertelen volt |Nem sikerült MyVM toostop |

Például következő kódrészlet egy runbookból hello kísérletek toostart hello szolgáltatás névvel rendelkező összes virtuális gép *MyServiceName*.  Ha bármelyik hello indítsa el a kérelmek sikertelenek, majd hiba műveleteket lehessen állítani.

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action tootake in case of success.
        }
        else {
            # Action tootake in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Részletes információkat
Az alábbiakban látható az ebben a forgatókönyvben hello runbookok részletes információkat.  Ezen információk tooeither runbookokat hello vagy ezekből a saját automatizálási esetekben tartalomkészítéshez csak toolearn testreszabása.

### <a name="parameters"></a>Paraméterek
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

hello munkafolyamat való első hello értékek beolvasása hello [bemeneti paraméterek](#using-the-scenario).  Ha hello eszköz neve nincs megadva alapértelmezett neveket használnak.

### <a name="output"></a>Kimenet
    # Returns strings with status messages
    [OutputType([String])]

A sor deklarálja, hogy hello kimeneti hello runbook lesz-e egy karakterláncot.  Ez nincs szükség, de hello runbook használatakor az ajánlott eljárás egy [gyermekrunbook](automation-child-runbooks.md) , hogy megtudják, hogy a szülő runbook hello kimeneti tooexpect írja be.

### <a name="authentication"></a>Authentication
    # Connect tooAzure and select hello subscription toowork against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

hello következő sorok hello beállítása [hitelesítő adatok](automation-credentials.md) és az Azure-előfizetéssel, amely jelzi a hello rest hello runbook.
Először használjuk **Get-AutomationPSCredential** tooget hello eszköz, amely az Azure-előfizetés hello toostart, majd szüntesse meg a virtuális gépek hello hitelesítő adatok elérésével tárolja. **Adja hozzá AzureAccount** az eszköz tooset hello megadott hitelesítő adatokat használja majd.  hello kimeneti tooa típusú változó van hozzárendelve, így azt nem tartalmazza a runbook-kimenet hello.  

hello változóeszköz hello előfizetés azonosítója majd beolvasni a **Get-AutomationVariable** és a beállított hello előfizetés **válasszon-AzureSubscription**.

### <a name="get-vms"></a>Virtuális gépek beolvasása
    # If there is a specific cloud service, then get all VMs in hello service,
    # otherwise get all VMs in hello subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

**Get-AzureVM** van használt tooretrieve hello virtuális gépek hello runbook működni fog-e.  Ha értéket megadva a hello **szolgáltatásnév** bemeneti a rendszer beolvassa a változót, akkor csak hello virtuális gépek, a szolgáltatás neve.  Ha **szolgáltatásnév** üres, akkor a rendszer beolvassa az összes virtuális gép.

### <a name="startstop-virtual-machines-and-send-output"></a>Virtuális gépek indítása/leállítása és kimenetként
    # Start each of hello stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # hello VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # hello VM needs toobe started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # hello VM failed toostart, so send notice
                Write-Output ($VM.InstanceName + " failed toostart")
            }
            else
            {
                # hello VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

hello következő sorok lépésenként minden virtuális géphez.  Először hello **PowerState** hello, a virtuális gép bejelölt toosee is, ha már fut vagy leállt, attól függően, hogy hello runbook.  Ha már hello cél állapotban, majd egy üzenetet kap toooutput, és hello runbook akkor ér véget.  Ha nem, majd **Start-AzureVM** vagy **Stop-AzureVM** használt tooattempt toostart vagy stop hello virtuális gép hello kérelem tárolt tooa változó hello eredménnyel.  Egy üzenetet küldi el a rendszer toooutput megadó e hello kérelem toostart vagy leállítása sikeresen el lett küldve.

## <a name="next-steps"></a>Következő lépések
* toolearn több gyermekrunbookot, használatával kapcsolatban lásd: [gyermek az Azure Automation runbookjai](automation-child-runbooks.md)
* További részletek toolearn a runbook végrehajtása és a naplózás toohelp állapotüzenetei hibaelhárítás című témakörben [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)

