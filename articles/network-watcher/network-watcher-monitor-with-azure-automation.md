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
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a><span data-ttu-id="29d41-103">VPN-átjárók a hálózati figyelőt hibaelhárításban figyelése</span><span class="sxs-lookup"><span data-stu-id="29d41-103">Monitor VPN gateways with Network Watcher troubleshooting</span></span>

<span data-ttu-id="29d41-104">Kritikus tooprovide megbízható szolgáltatások toocustomers való mélyebben elemezheti a hálózati teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="29d41-104">Gaining deep insights on your network performance is critical tooprovide reliable services toocustomers.</span></span> <span data-ttu-id="29d41-105">Ezért kritikus toodetect hálózati kimaradás feltételek gyorsan és javítási műveletek toomitigate hello kimaradás feltétel igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="29d41-105">It is therefore critical toodetect network outage conditions quickly and take corrective action toomitigate hello outage condition.</span></span> <span data-ttu-id="29d41-106">Azure Automation tooimplement lehetővé teszi, és a feladat futtatása programozott módon runbookok keresztül.</span><span class="sxs-lookup"><span data-stu-id="29d41-106">Azure Automation enables you tooimplement and run a task in a programmatic fashion through runbooks.</span></span> <span data-ttu-id="29d41-107">Egy tökéletes receptet folyamatos és proaktív hálózati figyelés és riasztás végrehajtásához Azure Automation használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="29d41-107">Using Azure Automation creates a perfect recipe for performing continuous and proactive network monitoring and alerting.</span></span>

## <a name="scenario"></a><span data-ttu-id="29d41-108">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="29d41-108">Scenario</span></span>

<span data-ttu-id="29d41-109">a következő kép hello hello forgatókönyv egy többrétegű alkalmazást, a helyi kapcsolat létrejött, a VPN-átjáró és alagút használatával.</span><span class="sxs-lookup"><span data-stu-id="29d41-109">hello scenario in hello following image is a multi-tiered application, with on premises connectivity established using a VPN Gateway and tunnel.</span></span> <span data-ttu-id="29d41-110">Biztosítsa a hello VPN-átjáró működik-e és fut kritikus toohello alkalmazások teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="29d41-110">Ensuring hello VPN Gateway is up and running is critical toohello applications performance.</span></span>

<span data-ttu-id="29d41-111">Egy runbook egy parancsfájl toocheck kapcsolati állapotához tartozó hello VPN-alagút, kapcsolat bújtatási állapot hello erőforrás hibakeresési API toocheck használatával hozza létre.</span><span class="sxs-lookup"><span data-stu-id="29d41-111">A runbook is created with a script toocheck for connection status of hello VPN tunnel, using hello Resource Troubleshooting API toocheck for connection tunnel status.</span></span> <span data-ttu-id="29d41-112">Ha hello állapota nem kifogástalan állapotú, egy e-mailek eseményindító tooadministrators küldött.</span><span class="sxs-lookup"><span data-stu-id="29d41-112">If hello status is not healthy, an email trigger is sent tooadministrators.</span></span>

![Példaforgatókönyv][scenario]

<span data-ttu-id="29d41-114">Ez a forgatókönyv tartalma:</span><span class="sxs-lookup"><span data-stu-id="29d41-114">This scenario will:</span></span>

- <span data-ttu-id="29d41-115">Hozzon létre egy runbook hívó hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag tootroubleshoot kapcsolat állapota</span><span class="sxs-lookup"><span data-stu-id="29d41-115">Create a runbook calling hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet tootroubleshoot connection status</span></span>
- <span data-ttu-id="29d41-116">Egy ütemezés toohello runbook hivatkozás</span><span class="sxs-lookup"><span data-stu-id="29d41-116">Link a schedule toohello runbook</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="29d41-117">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="29d41-117">Before you begin</span></span>

<span data-ttu-id="29d41-118">Ez a forgatókönyv megkezdése előtt rendelkeznie kell a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="29d41-118">Before you start this scenario, you must have hello following pre-requisites:</span></span>

- <span data-ttu-id="29d41-119">Egy Azure automation-fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="29d41-119">An Azure automation account in Azure.</span></span> <span data-ttu-id="29d41-120">Győződjön meg arról, hogy hello automation-fiók hello legújabb modul van hello AzureRM.Network modult is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="29d41-120">Ensure that hello automation account has hello latest modules and also has hello AzureRM.Network module.</span></span> <span data-ttu-id="29d41-121">hello AzureRM.Network modul esetén érhető el, a hello modul galériában tooadd kell azt tooyour automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="29d41-121">hello AzureRM.Network module is available in hello module gallery if you need tooadd it tooyour automation account.</span></span>
- <span data-ttu-id="29d41-122">A hitelesítő adatok készletét konfigurálása az Azure Automationben kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="29d41-122">You must have a set of credentials configure in Azure Automation.</span></span> <span data-ttu-id="29d41-123">További tudnivalókért olvassa el [Azure Automation szolgáltatásbeli biztonsági](../automation/automation-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="29d41-123">Learn more at [Azure Automation security](../automation/automation-security-overview.md)</span></span>
- <span data-ttu-id="29d41-124">Egy érvényes SMTP-kiszolgáló (Office 365, a helyszíni e-mail vagy egy másik) és az Azure Automationben hitelesítő adatokhoz</span><span class="sxs-lookup"><span data-stu-id="29d41-124">A valid SMTP server (Office 365, your on-premises email or another) and credentials defined in Azure Automation</span></span>
- <span data-ttu-id="29d41-125">A konfigurált virtuális hálózati átjáró az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="29d41-125">A configured Virtual Network Gateway in Azure.</span></span>
- <span data-ttu-id="29d41-126">Egy meglévő tárfiók egy meglévő tároló toostore hello naplózza.</span><span class="sxs-lookup"><span data-stu-id="29d41-126">An existing storage account with an existing container toostore hello logs in.</span></span>

> [!NOTE]
> <span data-ttu-id="29d41-127">kép megelőző hello kitaláltak hello infrastruktúra illusztrációs célokat szolgálnak, és még nem jöttek létre ebben a cikkben szereplő hello leírásával.</span><span class="sxs-lookup"><span data-stu-id="29d41-127">hello infrastructure depicted in hello preceding image is for illustration purposes and are not created with hello steps contained in this article.</span></span>

### <a name="create-hello-runbook"></a><span data-ttu-id="29d41-128">Hello runbook létrehozása</span><span class="sxs-lookup"><span data-stu-id="29d41-128">Create hello runbook</span></span>

<span data-ttu-id="29d41-129">hello első lépés tooconfiguring hello példája toocreate hello runbook.</span><span class="sxs-lookup"><span data-stu-id="29d41-129">hello first step tooconfiguring hello example is toocreate hello runbook.</span></span> <span data-ttu-id="29d41-130">A példa egy futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="29d41-130">This example uses a run-as account.</span></span> <span data-ttu-id="29d41-131">Keresse fel a futtató fiókokkal kapcsolatos toolearn [Runbookok hitelesítéséhez az Azure-beli futtató fiók](../automation/automation-sec-configure-azure-runas-account.md)</span><span class="sxs-lookup"><span data-stu-id="29d41-131">toolearn about run-as accounts, visit [Authenticate Runbooks with Azure Run As account](../automation/automation-sec-configure-azure-runas-account.md)</span></span>

### <a name="step-1"></a><span data-ttu-id="29d41-132">1. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-132">Step 1</span></span>

<span data-ttu-id="29d41-133">Keresse meg a hello Automation tooAzure [Azure-portálon](https://portal.azure.com) kattintson **Runbookok**</span><span class="sxs-lookup"><span data-stu-id="29d41-133">Navigate tooAzure Automation in hello [Azure portal](https://portal.azure.com) and click **Runbooks**</span></span>

![Automation-fiók – áttekintés][1]

### <a name="step-2"></a><span data-ttu-id="29d41-135">2. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-135">Step 2</span></span>

<span data-ttu-id="29d41-136">Kattintson a **hozzáadása egy runbook** toostart hello létrehozási folyamata hello runbook.</span><span class="sxs-lookup"><span data-stu-id="29d41-136">Click **Add a runbook** toostart hello creation process of hello runbook.</span></span>

![runbookok panel][2]

### <a name="step-3"></a><span data-ttu-id="29d41-138">3. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-138">Step 3</span></span>

<span data-ttu-id="29d41-139">A **Gyorslétrehozás**, kattintson a **hozzon létre egy új runbookot** toocreate hello runbook.</span><span class="sxs-lookup"><span data-stu-id="29d41-139">Under **Quick Create**, click **Create a new runbook** toocreate hello runbook.</span></span>

![egy runbook panel hozzáadása][3]

### <a name="step-4"></a><span data-ttu-id="29d41-141">4. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-141">Step 4</span></span>

<span data-ttu-id="29d41-142">Ebben a lépésben, amelyben tudatjuk a felhasználókkal hello a runbook nevét, hello példában nevezik **Get-VPNGatewayStatus**.</span><span class="sxs-lookup"><span data-stu-id="29d41-142">In this step, we give hello runbook a name, in hello example it is called **Get-VPNGatewayStatus**.</span></span> <span data-ttu-id="29d41-143">Ez fontos toogive hello runbook könnyen megjegyezhető nevet, és adjon neki egy nevet, amely PowerShell szabványos elnevezési szabályai a következő ajánlott.</span><span class="sxs-lookup"><span data-stu-id="29d41-143">It is important toogive hello runbook a descriptive name, and recommended giving it a name that follows standard PowerShell naming standards.</span></span> <span data-ttu-id="29d41-144">Ehhez a példához hello runbook típusa **PowerShell**, hello is grafikus PowerShell-munkafolyamati, és a grafikus PowerShell-munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="29d41-144">hello runbook type for this example is **PowerShell**, hello other options are Graphical, PowerShell workflow, and Graphical PowerShell workflow.</span></span>

![runbook panel][4]

### <a name="step-5"></a><span data-ttu-id="29d41-146">5. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-146">Step 5</span></span>

<span data-ttu-id="29d41-147">Ebben a lépésben hello runbook jön létre, a következő példakód hello biztosít összes hello hello például szükséges kódot.</span><span class="sxs-lookup"><span data-stu-id="29d41-147">In this step hello runbook is created, hello following code example provides all hello code needed for hello example.</span></span> <span data-ttu-id="29d41-148">hello kódot tartalmazó elemeket hello \<érték\> kell toobe helyett hello értékeket az előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="29d41-148">hello items in hello code that contain \<value\> need toobe replaced with hello values from your subscription.</span></span>

<span data-ttu-id="29d41-149">Kattintson kód használata hello következő **mentése**</span><span class="sxs-lookup"><span data-stu-id="29d41-149">Use hello following code as click **Save**</span></span>

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

### <a name="step-6"></a><span data-ttu-id="29d41-150">6. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-150">Step 6</span></span>

<span data-ttu-id="29d41-151">Miután hello runbook mentettük, ütemezés szerint kell tooit tooautomate hello start hello runbook csatolva.</span><span class="sxs-lookup"><span data-stu-id="29d41-151">Once hello runbook is saved, a schedule must be linked tooit tooautomate hello start of hello runbook.</span></span> <span data-ttu-id="29d41-152">toostart hello folyamat, kattintson a **ütemezés**.</span><span class="sxs-lookup"><span data-stu-id="29d41-152">toostart hello process, click **Schedule**.</span></span>

![6. lépés][6]

## <a name="link-a-schedule-toohello-runbook"></a><span data-ttu-id="29d41-154">Egy ütemezés toohello runbook hivatkozás</span><span class="sxs-lookup"><span data-stu-id="29d41-154">Link a schedule toohello runbook</span></span>

<span data-ttu-id="29d41-155">Létre kell hozni egy új ütemezést.</span><span class="sxs-lookup"><span data-stu-id="29d41-155">A new schedule must be created.</span></span> <span data-ttu-id="29d41-156">Kattintson a **ütemezés tooyour runbook hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="29d41-156">Click **Link a schedule tooyour runbook**.</span></span>

![7. lépés][7]

### <a name="step-1"></a><span data-ttu-id="29d41-158">1. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-158">Step 1</span></span>

<span data-ttu-id="29d41-159">A hello **ütemezés** paneljén kattintson **új ütemezés létrehozása**</span><span class="sxs-lookup"><span data-stu-id="29d41-159">On hello **Schedule** blade, click **Create a new schedule**</span></span>

![8. lépés][8]

### <a name="step-2"></a><span data-ttu-id="29d41-161">2. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-161">Step 2</span></span>

<span data-ttu-id="29d41-162">A hello **új ütemezés** panel kitöltése során hello ütemezési információkat.</span><span class="sxs-lookup"><span data-stu-id="29d41-162">On hello **New Schedule** blade fill out hello schedule information.</span></span> <span data-ttu-id="29d41-163">hello beállítható értékei a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="29d41-163">hello values that can be set are in hello following list:</span></span>

- <span data-ttu-id="29d41-164">**Név** -hello ütemezés hello rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="29d41-164">**Name** - hello friendly name of hello schedule.</span></span>
- <span data-ttu-id="29d41-165">**Leírás** -hello ütemezés leírását.</span><span class="sxs-lookup"><span data-stu-id="29d41-165">**Description** - A description of hello schedule.</span></span>
- <span data-ttu-id="29d41-166">**Elindul** -e érték dátum, idő és időzóna hello idő hello ütemezés eseményindítók alkotó kombinációja.</span><span class="sxs-lookup"><span data-stu-id="29d41-166">**Starts** - This value is a combination of date, time, and time zone that make up hello time hello schedule triggers.</span></span>
- <span data-ttu-id="29d41-167">**Ismétlődés** -Ez az érték szabja meg hello ütemezés ismétlési.</span><span class="sxs-lookup"><span data-stu-id="29d41-167">**Recurrence** - This value determines hello schedules repetition.</span></span>  <span data-ttu-id="29d41-168">Érvényes értékek a következők **egyszer** vagy **ismétlődő**.</span><span class="sxs-lookup"><span data-stu-id="29d41-168">Valid values are **Once** or **Recurring**.</span></span>
- <span data-ttu-id="29d41-169">**Ismétlődik minden** -hello ismétlési időköz hello ütemterv az óra, nap, hét és hónap.</span><span class="sxs-lookup"><span data-stu-id="29d41-169">**Recur every** - hello recurrence interval of hello schedule in hours, days, weeks, or months.</span></span>
- <span data-ttu-id="29d41-170">**Beállíthatja a lejárati idejét** -hello az érték szabja meg, ha hello ütemezés lejárati vagy sem.</span><span class="sxs-lookup"><span data-stu-id="29d41-170">**Set Expiration** - hello value determines if hello schedule should expire or not.</span></span> <span data-ttu-id="29d41-171">Túl beállítható**Igen** vagy **nem**.</span><span class="sxs-lookup"><span data-stu-id="29d41-171">Can be set too**Yes** or **No**.</span></span> <span data-ttu-id="29d41-172">Érvényes dátumot és időpontot megadva Igen választása toobe.</span><span class="sxs-lookup"><span data-stu-id="29d41-172">A valid date and time are toobe provided if yes is chosen.</span></span>

> [!NOTE]
> <span data-ttu-id="29d41-173">Ha egy runbook futtatása minden órában gyakrabban kell toohave, egyszerre több ütemezés léteznie kell más időközönként (Ez azt jelenti, 15, 30, hello óra 45 perc)</span><span class="sxs-lookup"><span data-stu-id="29d41-173">If you need toohave a runbook run more often than every hour, multiple schedules must be created at different intervals (that is, 15, 30, 45 minutes after hello hour)</span></span>

![9. lépés][9]

### <a name="step-3"></a><span data-ttu-id="29d41-175">3. lépés</span><span class="sxs-lookup"><span data-stu-id="29d41-175">Step 3</span></span>

<span data-ttu-id="29d41-176">Kattintson a Mentés toosave hello ütemezés toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="29d41-176">Click Save toosave hello schedule toohello runbook.</span></span>

![10. lépés][10]

## <a name="next-steps"></a><span data-ttu-id="29d41-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29d41-178">Next steps</span></span>

<span data-ttu-id="29d41-179">Most, hogy megismerhesse hogyan toointegrate hálózati figyelőt hibaelhárítása az Azure Automation szolgáltatásban, megtudhatja, hogyan tootrigger csomag rögzíti a virtuális gép riasztások ellátogatva [hozzon létre egy riasztási kiváltott csomagrögzítéssel Azure hálózati figyelőt](network-watcher-alert-triggered-packet-capture.md).</span><span class="sxs-lookup"><span data-stu-id="29d41-179">Now that you have an understanding on how toointegrate Network Watcher troubleshooting with Azure Automation, learn how tootrigger packet captures on VM alerts by visiting [Create an alert triggered packet capture with Azure Network Watcher](network-watcher-alert-triggered-packet-capture.md).</span></span>

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
