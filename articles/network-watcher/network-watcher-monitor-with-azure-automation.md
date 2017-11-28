---
title: "VPN-átjárók Azure hálózati figyelőt hibaelhárítás figyelése |} Microsoft Docs"
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
ms.openlocfilehash: 55ec52dd0d94a0347cc67a8785b89611da955111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a><span data-ttu-id="2d3fe-103">VPN-átjárók a hálózati figyelőt hibaelhárításban figyelése</span><span class="sxs-lookup"><span data-stu-id="2d3fe-103">Monitor VPN gateways with Network Watcher troubleshooting</span></span>

<span data-ttu-id="2d3fe-104">Való mélyebben elemezheti a hálózat teljesítménye szempontjából fontos a megbízható szolgáltatásokat nyújtsanak az ügyfeleinek.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-104">Gaining deep insights on your network performance is critical to provide reliable services to customers.</span></span> <span data-ttu-id="2d3fe-105">Ezért fontos hálózati kimaradás körülmények gyorsan észlelését és csökkenthető a leállás feltétel intézkedéseket.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-105">It is therefore critical to detect network outage conditions quickly and take corrective action to mitigate the outage condition.</span></span> <span data-ttu-id="2d3fe-106">Azure Automation szolgáltatásbeli bevezetése, valamint a feladat futtatása programozott módon runbookok keresztül teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-106">Azure Automation enables you to implement and run a task in a programmatic fashion through runbooks.</span></span> <span data-ttu-id="2d3fe-107">Egy tökéletes receptet folyamatos és proaktív hálózati figyelés és riasztás végrehajtásához Azure Automation használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-107">Using Azure Automation creates a perfect recipe for performing continuous and proactive network monitoring and alerting.</span></span>

## <a name="scenario"></a><span data-ttu-id="2d3fe-108">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2d3fe-108">Scenario</span></span>

<span data-ttu-id="2d3fe-109">Az alábbi ábrán a forgatókönyv egy többrétegű alkalmazást, a helyi kapcsolat létrejött, a VPN-átjáró és alagút használatával.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-109">The scenario in the following image is a multi-tiered application, with on premises connectivity established using a VPN Gateway and tunnel.</span></span> <span data-ttu-id="2d3fe-110">Annak biztosítása, a VPN-átjáró működik-e és fut-létfontosságú alkalmazások teljesítménye.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-110">Ensuring the VPN Gateway is up and running is critical to the applications performance.</span></span>

<span data-ttu-id="2d3fe-111">Egy runbook egy parancsfájlt, amely a VPN-alagúton, az erőforrás hibaelhárítási API-val való kapcsolat bújtatási állapotának ellenőrzése kapcsolat állapotának ellenőrzése hozza létre.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-111">A runbook is created with a script to check for connection status of the VPN tunnel, using the Resource Troubleshooting API to check for connection tunnel status.</span></span> <span data-ttu-id="2d3fe-112">Ha az állapot nem kifogástalan, egy e-mailek eseményindító rendszergazdák érkezik.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-112">If the status is not healthy, an email trigger is sent to administrators.</span></span>

![Példaforgatókönyv][scenario]

<span data-ttu-id="2d3fe-114">Ez a forgatókönyv tartalma:</span><span class="sxs-lookup"><span data-stu-id="2d3fe-114">This scenario will:</span></span>

- <span data-ttu-id="2d3fe-115">Hozzon létre egy runbook meghívása a `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmaggal hibaelhárításához kapcsolat állapota</span><span class="sxs-lookup"><span data-stu-id="2d3fe-115">Create a runbook calling the `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet to troubleshoot connection status</span></span>
- <span data-ttu-id="2d3fe-116">Ütemezés kapcsolása a forgatókönyvhöz</span><span class="sxs-lookup"><span data-stu-id="2d3fe-116">Link a schedule to the runbook</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2d3fe-117">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2d3fe-117">Before you begin</span></span>

<span data-ttu-id="2d3fe-118">Ez a forgatókönyv megkezdése előtt rendelkeznie kell a következő előfeltételeknek:</span><span class="sxs-lookup"><span data-stu-id="2d3fe-118">Before you start this scenario, you must have the following pre-requisites:</span></span>

- <span data-ttu-id="2d3fe-119">Egy Azure automation-fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-119">An Azure automation account in Azure.</span></span> <span data-ttu-id="2d3fe-120">Győződjön meg arról, hogy az automation-fiók a legújabb modul van, és a AzureRM.Network modul is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-120">Ensure that the automation account has the latest modules and also has the AzureRM.Network module.</span></span> <span data-ttu-id="2d3fe-121">A AzureRM.Network modul esetén érhető el, a modul gyűjteményben kell adja hozzá az automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-121">The AzureRM.Network module is available in the module gallery if you need to add it to your automation account.</span></span>
- <span data-ttu-id="2d3fe-122">A hitelesítő adatok készletét konfigurálása az Azure Automationben kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-122">You must have a set of credentials configure in Azure Automation.</span></span> <span data-ttu-id="2d3fe-123">További tudnivalókért olvassa el [Azure Automation szolgáltatásbeli biztonsági](../automation/automation-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2d3fe-123">Learn more at [Azure Automation security](../automation/automation-security-overview.md)</span></span>
- <span data-ttu-id="2d3fe-124">Egy érvényes SMTP-kiszolgáló (Office 365, a helyszíni e-mail vagy egy másik) és az Azure Automationben hitelesítő adatokhoz</span><span class="sxs-lookup"><span data-stu-id="2d3fe-124">A valid SMTP server (Office 365, your on-premises email or another) and credentials defined in Azure Automation</span></span>
- <span data-ttu-id="2d3fe-125">A konfigurált virtuális hálózati átjáró az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-125">A configured Virtual Network Gateway in Azure.</span></span>
- <span data-ttu-id="2d3fe-126">A naplók tárolásához egy létező tárolóval meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-126">An existing storage account with an existing container to store the logs in.</span></span>

> [!NOTE]
> <span data-ttu-id="2d3fe-127">Az infrastruktúra az előző ábrán kitaláltak illusztrációs célokat szolgálnak, és még nem jöttek létre ebben a cikkben található lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-127">The infrastructure depicted in the preceding image is for illustration purposes and are not created with the steps contained in this article.</span></span>

### <a name="create-the-runbook"></a><span data-ttu-id="2d3fe-128">Hozza létre a runbookot</span><span class="sxs-lookup"><span data-stu-id="2d3fe-128">Create the runbook</span></span>

<span data-ttu-id="2d3fe-129">Az első lépés a példa konfigurálására, hogy hozza létre a runbookot.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-129">The first step to configuring the example is to create the runbook.</span></span> <span data-ttu-id="2d3fe-130">A példa egy futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-130">This example uses a run-as account.</span></span> <span data-ttu-id="2d3fe-131">A futtató fiókokkal kapcsolatos további tudnivalókért keresse fel a [Runbookok hitelesítéséhez az Azure-beli futtató fiók](../automation/automation-sec-configure-azure-runas-account.md)</span><span class="sxs-lookup"><span data-stu-id="2d3fe-131">To learn about run-as accounts, visit [Authenticate Runbooks with Azure Run As account](../automation/automation-sec-configure-azure-runas-account.md)</span></span>

### <a name="step-1"></a><span data-ttu-id="2d3fe-132">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-132">Step 1</span></span>

<span data-ttu-id="2d3fe-133">Keresse meg az Azure Automation a [Azure-portálon](https://portal.azure.com) kattintson **Runbookok**</span><span class="sxs-lookup"><span data-stu-id="2d3fe-133">Navigate to Azure Automation in the [Azure portal](https://portal.azure.com) and click **Runbooks**</span></span>

![Automation-fiók – áttekintés][1]

### <a name="step-2"></a><span data-ttu-id="2d3fe-135">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-135">Step 2</span></span>

<span data-ttu-id="2d3fe-136">Kattintson a **hozzáadása egy runbook** a runbook létrehozásának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-136">Click **Add a runbook** to start the creation process of the runbook.</span></span>

![runbookok panel][2]

### <a name="step-3"></a><span data-ttu-id="2d3fe-138">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-138">Step 3</span></span>

<span data-ttu-id="2d3fe-139">A **Gyorslétrehozás**, kattintson a **hozzon létre egy új runbookot** hozza létre a runbookot.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-139">Under **Quick Create**, click **Create a new runbook** to create the runbook.</span></span>

![egy runbook panel hozzáadása][3]

### <a name="step-4"></a><span data-ttu-id="2d3fe-141">4. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-141">Step 4</span></span>

<span data-ttu-id="2d3fe-142">Ebben a lépésben, amelyben tudatjuk a felhasználókkal a runbook nevét, a példában szereplő nevezik **Get-VPNGatewayStatus**.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-142">In this step, we give the runbook a name, in the example it is called **Get-VPNGatewayStatus**.</span></span> <span data-ttu-id="2d3fe-143">Ez nem lényeges, hogy a runbook egy leíró nevet, és adjon neki egy nevet, amely PowerShell szabványos elnevezési szabályai a következő ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-143">It is important to give the runbook a descriptive name, and recommended giving it a name that follows standard PowerShell naming standards.</span></span> <span data-ttu-id="2d3fe-144">A runbook típusa ehhez a példához **PowerShell**, a többi beállítást a rendszer grafikus PowerShell-munkafolyamati és grafikus PowerShell-munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-144">The runbook type for this example is **PowerShell**, the other options are Graphical, PowerShell workflow, and Graphical PowerShell workflow.</span></span>

![runbook panel][4]

### <a name="step-5"></a><span data-ttu-id="2d3fe-146">5. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-146">Step 5</span></span>

<span data-ttu-id="2d3fe-147">Ebben a lépésben a runbook jön létre az alábbi példakód biztosít a példában a szükséges összes kódot.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-147">In this step the runbook is created, the following code example provides all the code needed for the example.</span></span> <span data-ttu-id="2d3fe-148">Az elemek, amelyek tartalmazzák a kódban \<érték\> kell cserélni az értékeket az előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-148">The items in the code that contain \<value\> need to be replaced with the values from your subscription.</span></span>

<span data-ttu-id="2d3fe-149">Kattintson az alábbi kód használata **mentése**</span><span class="sxs-lookup"><span data-stu-id="2d3fe-149">Use the following code as click **Save**</span></span>

```PowerShell
# Set these variables to the proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<to email address>"
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

# Get the connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in to Azure..."
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context to a specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView the logs at $($storagePath) to learn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -To $toEmail `
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

### <a name="step-6"></a><span data-ttu-id="2d3fe-150">6. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-150">Step 6</span></span>

<span data-ttu-id="2d3fe-151">A forgatókönyv mentése, után ütemezés össze kell kapcsolni, hogy automatizálja a runbook elindítása.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-151">Once the runbook is saved, a schedule must be linked to it to automate the start of the runbook.</span></span> <span data-ttu-id="2d3fe-152">A folyamat elindításához kattintson **ütemezés**.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-152">To start the process, click **Schedule**.</span></span>

![6. lépés][6]

## <a name="link-a-schedule-to-the-runbook"></a><span data-ttu-id="2d3fe-154">Ütemezés kapcsolása a forgatókönyvhöz</span><span class="sxs-lookup"><span data-stu-id="2d3fe-154">Link a schedule to the runbook</span></span>

<span data-ttu-id="2d3fe-155">Létre kell hozni egy új ütemezést.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-155">A new schedule must be created.</span></span> <span data-ttu-id="2d3fe-156">Kattintson a **ütemezés kapcsolása a forgatókönyvhöz**.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-156">Click **Link a schedule to your runbook**.</span></span>

![7. lépés][7]

### <a name="step-1"></a><span data-ttu-id="2d3fe-158">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-158">Step 1</span></span>

<span data-ttu-id="2d3fe-159">Az a **ütemezés** paneljén kattintson **új ütemezés létrehozása**</span><span class="sxs-lookup"><span data-stu-id="2d3fe-159">On the **Schedule** blade, click **Create a new schedule**</span></span>

![8. lépés][8]

### <a name="step-2"></a><span data-ttu-id="2d3fe-161">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-161">Step 2</span></span>

<span data-ttu-id="2d3fe-162">Az a **új ütemezés** panel kitöltése során az ütemezési információkat.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-162">On the **New Schedule** blade fill out the schedule information.</span></span> <span data-ttu-id="2d3fe-163">A beállítható értékei a következők:</span><span class="sxs-lookup"><span data-stu-id="2d3fe-163">The values that can be set are in the following list:</span></span>

- <span data-ttu-id="2d3fe-164">**Név** -ütemezés rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-164">**Name** - The friendly name of the schedule.</span></span>
- <span data-ttu-id="2d3fe-165">**Leírás** -ütemezés leírását.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-165">**Description** - A description of the schedule.</span></span>
- <span data-ttu-id="2d3fe-166">**Elindul** -e érték dátum, idő és időzóna alkotó az idő az ütemezés eseményindítók kombinációja.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-166">**Starts** - This value is a combination of date, time, and time zone that make up the time the schedule triggers.</span></span>
- <span data-ttu-id="2d3fe-167">**Ismétlődés** -Ez az érték határozza meg az ütemezés ismétlési.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-167">**Recurrence** - This value determines the schedules repetition.</span></span>  <span data-ttu-id="2d3fe-168">Érvényes értékek a következők **egyszer** vagy **ismétlődő**.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-168">Valid values are **Once** or **Recurring**.</span></span>
- <span data-ttu-id="2d3fe-169">**Ismétlődik minden** -az ismétlődési ütemezés az óra, nap, hét és hónap.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-169">**Recur every** - The recurrence interval of the schedule in hours, days, weeks, or months.</span></span>
- <span data-ttu-id="2d3fe-170">**Beállíthatja a lejárati idejét** -az érték szabja meg, ha az ütemezés lejárati vagy nem.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-170">**Set Expiration** - The value determines if the schedule should expire or not.</span></span> <span data-ttu-id="2d3fe-171">Megadható **Igen** vagy **nem**.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-171">Can be set to **Yes** or **No**.</span></span> <span data-ttu-id="2d3fe-172">Érvényes dátumot és időpontot meg kell adni, ha az Igen választása vannak.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-172">A valid date and time are to be provided if yes is chosen.</span></span>

> [!NOTE]
> <span data-ttu-id="2d3fe-173">Egy runbook minden órában gyakoribb futtatásához rendelkeznie kell, ha egyszerre több ütemezés léteznie kell más időközönként (Ez azt jelenti, 15, 30, az óra 45 perc)</span><span class="sxs-lookup"><span data-stu-id="2d3fe-173">If you need to have a runbook run more often than every hour, multiple schedules must be created at different intervals (that is, 15, 30, 45 minutes after the hour)</span></span>

![9. lépés][9]

### <a name="step-3"></a><span data-ttu-id="2d3fe-175">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2d3fe-175">Step 3</span></span>

<span data-ttu-id="2d3fe-176">Kattintson a Mentés menteni az ütemezést a runbookhoz.</span><span class="sxs-lookup"><span data-stu-id="2d3fe-176">Click Save to save the schedule to the runbook.</span></span>

![10. lépés][10]

## <a name="next-steps"></a><span data-ttu-id="2d3fe-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d3fe-178">Next steps</span></span>

<span data-ttu-id="2d3fe-179">Most, hogy megismerhesse a integrálása az Azure Automation szolgáltatásban hibaelhárítási hálózati figyelőt, megtudhatja, hogyan csomag rögzíti a virtuális gép riasztásokat kiváltó ellátogatva [hozzon létre egy riasztási kiváltott csomagrögzítéssel Azure hálózati figyelőt](network-watcher-alert-triggered-packet-capture.md).</span><span class="sxs-lookup"><span data-stu-id="2d3fe-179">Now that you have an understanding on how to integrate Network Watcher troubleshooting with Azure Automation, learn how to trigger packet captures on VM alerts by visiting [Create an alert triggered packet capture with Azure Network Watcher](network-watcher-alert-triggered-packet-capture.md).</span></span>

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
