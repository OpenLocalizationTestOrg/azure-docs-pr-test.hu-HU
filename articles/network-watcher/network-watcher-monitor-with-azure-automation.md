---
title: "aaaMonitor VPN-átjárók Azure hálózati figyelőt hibaelhárítás |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan diagnosztizálhatja a helyi kapcsolat az Azure Automation- és hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a607d0c862ea1be63c687717f0c5dc137db58a43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>VPN-átjárók a hálózati figyelőt hibaelhárításban figyelése

Kritikus tooprovide megbízható szolgáltatások toocustomers való mélyebben elemezheti a hálózati teljesítményre. Ezért kritikus toodetect hálózati kimaradás feltételek gyorsan és javítási műveletek toomitigate hello kimaradás feltétel igénybe vehet. Azure Automation tooimplement lehetővé teszi, és a feladat futtatása programozott módon runbookok keresztül. Egy tökéletes receptet folyamatos és proaktív hálózati figyelés és riasztás végrehajtásához Azure Automation használatával hoz létre.

## <a name="scenario"></a>Forgatókönyv

a következő kép hello hello forgatókönyv egy többrétegű alkalmazást, a helyi kapcsolat létrejött, a VPN-átjáró és alagút használatával. Biztosítsa a hello VPN-átjáró működik-e és fut kritikus toohello alkalmazások teljesítményét.

Egy runbook egy parancsfájl toocheck kapcsolati állapotához tartozó hello VPN-alagút, kapcsolat bújtatási állapot hello erőforrás hibakeresési API toocheck használatával hozza létre. Ha hello állapota nem kifogástalan állapotú, egy e-mailek eseményindító tooadministrators küldött.

![Példaforgatókönyv][scenario]

Ez a forgatókönyv tartalma:

- Hozzon létre egy runbook hívó hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag tootroubleshoot kapcsolat állapota
- Egy ütemezés toohello runbook hivatkozás

## <a name="before-you-begin"></a>Előkészületek

Ez a forgatókönyv megkezdése előtt rendelkeznie kell a következő előfeltételek hello:

- Egy Azure automation-fiók az Azure-ban. Győződjön meg arról, hogy hello automation-fiók hello legújabb modul van hello AzureRM.Network modult is tartalmaz. hello AzureRM.Network modul esetén érhető el, a hello modul galériában tooadd kell azt tooyour automation-fiók.
- A hitelesítő adatok készletét konfigurálása az Azure Automationben kell rendelkeznie. További tudnivalókért olvassa el [Azure Automation szolgáltatásbeli biztonsági](../automation/automation-security-overview.md)
- Egy érvényes SMTP-kiszolgáló (Office 365, a helyszíni e-mail vagy egy másik) és az Azure Automationben hitelesítő adatokhoz
- A konfigurált virtuális hálózati átjáró az Azure-ban.
- Egy meglévő tárfiók egy meglévő tároló toostore hello naplózza.

> [!NOTE]
> kép megelőző hello kitaláltak hello infrastruktúra illusztrációs célokat szolgálnak, és még nem jöttek létre ebben a cikkben szereplő hello leírásával.

### <a name="create-hello-runbook"></a>Hello runbook létrehozása

hello első lépés tooconfiguring hello példája toocreate hello runbook. A példa egy futtató fiókot. Keresse fel a futtató fiókokkal kapcsolatos toolearn [Runbookok hitelesítéséhez az Azure-beli futtató fiók](../automation/automation-sec-configure-azure-runas-account.md)

### <a name="step-1"></a>1. lépés

Keresse meg a hello Automation tooAzure [Azure-portálon](https://portal.azure.com) kattintson **Runbookok**

![Automation-fiók – áttekintés][1]

### <a name="step-2"></a>2. lépés

Kattintson a **hozzáadása egy runbook** toostart hello létrehozási folyamata hello runbook.

![runbookok panel][2]

### <a name="step-3"></a>3. lépés

A **Gyorslétrehozás**, kattintson a **hozzon létre egy új runbookot** toocreate hello runbook.

![egy runbook panel hozzáadása][3]

### <a name="step-4"></a>4. lépés

Ebben a lépésben, amelyben tudatjuk a felhasználókkal hello a runbook nevét, hello példában nevezik **Get-VPNGatewayStatus**. Ez fontos toogive hello runbook könnyen megjegyezhető nevet, és adjon neki egy nevet, amely PowerShell szabványos elnevezési szabályai a következő ajánlott. Ehhez a példához hello runbook típusa **PowerShell**, hello is grafikus PowerShell-munkafolyamati, és a grafikus PowerShell-munkafolyamat.

![runbook panel][4]

### <a name="step-5"></a>5. lépés

Ebben a lépésben hello runbook jön létre, a következő példakód hello biztosít összes hello hello például szükséges kódot. hello kódot tartalmazó elemeket hello \<érték\> kell toobe helyett hello értékeket az előfizetésből.

Kattintson kód használata hello következő **mentése**

```PowerShell
# Set these variables toohello proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<tooemail address>"
$smtpServer = "<smtp.office365.com>"
$smtpPort = 587
$runAsConnectionName = "<AzureRunAsConnection>"
$subscriptionId = "<subscription id>"
$region = "<Azure region>"
$vpnConnectionName = "<vpn connection name>"
$vpnConnectionResourceGroup = "<resource group name>"
$storageAccountName = "<storage account name>"
$storageAccountResourceGroup = "<resource group name>"
$storageAccountContainer = "<container name>"

# Get credentials for Office 365 account
$cred = Get-AutomationPSCredential -Name $o365AutomationCredential

# Get hello connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in tooAzure..."
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context tooa specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView hello logs at $($storagePath) toolearn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -too$toEmail `
        -Subject $subject `
        -Body $body `
        -UseSsl `
        -Port $smtpPort `
        -SmtpServer $smtpServer `
        -From $fromEmail `
        -BodyAsHtml `
        -Credential $cred
    }
else
    {
    Write-Output ("Connection Status is: $($result.code)")
    }
```

### <a name="step-6"></a>6. lépés

Miután hello runbook mentettük, ütemezés szerint kell tooit tooautomate hello start hello runbook csatolva. toostart hello folyamat, kattintson a **ütemezés**.

![6. lépés][6]

## <a name="link-a-schedule-toohello-runbook"></a>Egy ütemezés toohello runbook hivatkozás

Létre kell hozni egy új ütemezést. Kattintson a **ütemezés tooyour runbook hivatkozás**.

![7. lépés][7]

### <a name="step-1"></a>1. lépés

A hello **ütemezés** paneljén kattintson **új ütemezés létrehozása**

![8. lépés][8]

### <a name="step-2"></a>2. lépés

A hello **új ütemezés** panel kitöltése során hello ütemezési információkat. hello beállítható értékei a következő lista hello:

- **Név** -hello ütemezés hello rövid nevét.
- **Leírás** -hello ütemezés leírását.
- **Elindul** -e érték dátum, idő és időzóna hello idő hello ütemezés eseményindítók alkotó kombinációja.
- **Ismétlődés** -Ez az érték szabja meg hello ütemezés ismétlési.  Érvényes értékek a következők **egyszer** vagy **ismétlődő**.
- **Ismétlődik minden** -hello ismétlési időköz hello ütemterv az óra, nap, hét és hónap.
- **Beállíthatja a lejárati idejét** -hello az érték szabja meg, ha hello ütemezés lejárati vagy sem. Túl beállítható**Igen** vagy **nem**. Érvényes dátumot és időpontot megadva Igen választása toobe.

> [!NOTE]
> Ha egy runbook futtatása minden órában gyakrabban kell toohave, egyszerre több ütemezés léteznie kell más időközönként (Ez azt jelenti, 15, 30, hello óra 45 perc)

![9. lépés][9]

### <a name="step-3"></a>3. lépés

Kattintson a Mentés toosave hello ütemezés toohello runbook.

![10. lépés][10]

## <a name="next-steps"></a>Következő lépések

Most, hogy megismerhesse hogyan toointegrate hálózati figyelőt hibaelhárítása az Azure Automation szolgáltatásban, megtudhatja, hogyan tootrigger csomag rögzíti a virtuális gép riasztások ellátogatva [hozzon létre egy riasztási kiváltott csomagrögzítéssel Azure hálózati figyelőt](network-watcher-alert-triggered-packet-capture.md).

<!-- images -->
[scenario]: ./media/network-watcher-monitor-with-azure-automation/scenario.png
[1]: ./media/network-watcher-monitor-with-azure-automation/figure1.png
[2]: ./media/network-watcher-monitor-with-azure-automation/figure2.png
[3]: ./media/network-watcher-monitor-with-azure-automation/figure3.png
[4]: ./media/network-watcher-monitor-with-azure-automation/figure4.png
[5]: ./media/network-watcher-monitor-with-azure-automation/figure5.png
[6]: ./media/network-watcher-monitor-with-azure-automation/figure6.png
[7]: ./media/network-watcher-monitor-with-azure-automation/figure7.png
[8]: ./media/network-watcher-monitor-with-azure-automation/figure8.png
[9]: ./media/network-watcher-monitor-with-azure-automation/figure9.png
[10]: ./media/network-watcher-monitor-with-azure-automation/figure10.png
