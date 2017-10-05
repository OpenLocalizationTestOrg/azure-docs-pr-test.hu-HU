---
title: "Ehhez a proaktív hálózatfigyelési riasztások és az Azure Functions csomagrögzítéssel használható |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan hozzon létre egy riasztási kiváltott csomagrögzítéssel Azure hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b813172fc1fc1cc683f463f05370c95bfec10f8d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a><span data-ttu-id="f01ca-103">Proaktív hálózatfigyelési riasztások és az Azure Functions csomagrögzítéssel használata</span><span class="sxs-lookup"><span data-stu-id="f01ca-103">Use packet capture for proactive network monitoring with alerts and Azure Functions</span></span>

<span data-ttu-id="f01ca-104">Hálózati figyelő csomagrögzítéssel hoz létre a rögzítési munkamenet forgalom mindkét virtuális gépek nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="f01ca-104">Network Watcher packet capture creates capture sessions to track traffic in and out of virtual machines.</span></span> <span data-ttu-id="f01ca-105">A rögzítési fájl lehet egy szűrő, amely csak a figyelni kívánt forgalom nyomon követéséhez van definiálva.</span><span class="sxs-lookup"><span data-stu-id="f01ca-105">The capture file can have a filter that is defined to track only the traffic that you want to monitor.</span></span> <span data-ttu-id="f01ca-106">Ezeket az adatokat a tárolási blob vagy helyileg, a vendég gépen majd tárolja.</span><span class="sxs-lookup"><span data-stu-id="f01ca-106">This data is then stored in a storage blob or locally on the guest machine.</span></span>

<span data-ttu-id="f01ca-107">Ez a funkció más automatizálási esetekben, például az Azure Functions távolról is indítható.</span><span class="sxs-lookup"><span data-stu-id="f01ca-107">This capability can be started remotely from other automation scenarios such as Azure Functions.</span></span> <span data-ttu-id="f01ca-108">Csomagrögzítéssel lehetővé teszi a funkció futtatásához a meghatározott hálózati rendellenességeket alapján proaktív rögzíti.</span><span class="sxs-lookup"><span data-stu-id="f01ca-108">Packet capture gives you the capability to run proactive captures based on defined network anomalies.</span></span> <span data-ttu-id="f01ca-109">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, első hálózati behatolások, a hibakeresési ügyfél-kiszolgáló közötti kommunikációt, és további információkat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="f01ca-109">Other uses include gathering network statistics, getting information about network intrusions, debugging client-server communications, and more.</span></span>

<span data-ttu-id="f01ca-110">Az Azure rendszerbe telepítendő erőforrásokat 24/7 fut.</span><span class="sxs-lookup"><span data-stu-id="f01ca-110">Resources that are deployed in Azure run 24/7.</span></span> <span data-ttu-id="f01ca-111">Alkalmazottak nem aktív figyelését az összes erőforrás 24/7 állapotát.</span><span class="sxs-lookup"><span data-stu-id="f01ca-111">You and your staff cannot actively monitor the status of all resources 24/7.</span></span> <span data-ttu-id="f01ca-112">Például mi történik, ha hiba fordul elő, hajnali 2 Órakor?</span><span class="sxs-lookup"><span data-stu-id="f01ca-112">For example, what happens if an issue occurs at 2 AM?</span></span>

<span data-ttu-id="f01ca-113">Használatával hálózati figyelőt, riasztások, és az Azure-ökoszisztémán belül funkciókat proaktív reagálhat az adatok és eszközök a hálózati problémák megoldására.</span><span class="sxs-lookup"><span data-stu-id="f01ca-113">By using Network Watcher, alerting, and functions from within the Azure ecosystem, you can proactively respond with the data and tools to solve problems in your network.</span></span>

![Forgatókönyv][scenario]

## <a name="prerequisites"></a><span data-ttu-id="f01ca-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f01ca-115">Prerequisites</span></span>

* <span data-ttu-id="f01ca-116">A legújabb [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f01ca-116">The latest version of [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span></span>
* <span data-ttu-id="f01ca-117">Hálózati figyelőt meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="f01ca-117">An existing instance of Network Watcher.</span></span> <span data-ttu-id="f01ca-118">Ha még nem rendelkezik egy, [hálózati figyelőt példányt létrehozni](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="f01ca-118">If you don't already have one, [create an instance of Network Watcher](network-watcher-create.md).</span></span>
* <span data-ttu-id="f01ca-119">Egy meglévő virtuális gép hálózati figyelőt az ugyanabban a régióban a [Windows bővítmény](../virtual-machines/windows/extensions-nwa.md) vagy [Linux virtuálisgép-bővítmény](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="f01ca-119">An existing virtual machine in the same region as Network Watcher with the [Windows extension](../virtual-machines/windows/extensions-nwa.md) or [Linux virtual machine extension](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="f01ca-120">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="f01ca-120">Scenario</span></span>

<span data-ttu-id="f01ca-121">Ebben a példában a szokásosnál több TCP-szegmens küld, és szeretne riasztást kapni.</span><span class="sxs-lookup"><span data-stu-id="f01ca-121">In this example, your VM is sending more TCP segments than usual, and you want to be alerted.</span></span> <span data-ttu-id="f01ca-122">TCP-szegmens az itt példaként szolgálnak, de a figyelmeztetési állapotot használhatja.</span><span class="sxs-lookup"><span data-stu-id="f01ca-122">TCP segments are used as an example here, but you can use any alert condition.</span></span>

<span data-ttu-id="f01ca-123">Amikor a rendszer megkérdezi, szeretné tudni, miért kommunikációs növekedett csomagszintű adatok fogadására.</span><span class="sxs-lookup"><span data-stu-id="f01ca-123">When you are alerted, you want to receive packet-level data to understand why communication has increased.</span></span> <span data-ttu-id="f01ca-124">Ezután térjen vissza a virtuális gép rendszeres kommunikációt lépéseket is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f01ca-124">Then you can take steps to return the virtual machine to regular communication.</span></span>

<span data-ttu-id="f01ca-125">Ez a forgatókönyv feltételezi, hogy rendelkezik-e hálózati figyelőt, és egy erőforráscsoport, érvényes virtuális gépet a meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="f01ca-125">This scenario assumes that you have an existing instance of Network Watcher and a resource group with a valid virtual machine.</span></span>

<span data-ttu-id="f01ca-126">Az alábbi lista a részben áttekintjük a munkafolyamatot, amely akkor történik meg:</span><span class="sxs-lookup"><span data-stu-id="f01ca-126">The following list is an overview of the workflow that takes place:</span></span>

1. <span data-ttu-id="f01ca-127">Figyelmeztetés jelenik meg a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="f01ca-127">An alert is triggered on your VM.</span></span>
1. <span data-ttu-id="f01ca-128">A riasztás meghívja az Azure-függvény olyan webhook keresztül.</span><span class="sxs-lookup"><span data-stu-id="f01ca-128">The alert calls your Azure function via a webhook.</span></span>
1. <span data-ttu-id="f01ca-129">Az Azure-függvény dolgozza fel a riasztást, és a hálózati figyelőt csomag rögzítési munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="f01ca-129">Your Azure function processes the alert and starts a Network Watcher packet capture session.</span></span>
1. <span data-ttu-id="f01ca-130">A csomagrögzítéssel a virtuális gép fut, és összegyűjti a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="f01ca-130">The packet capture runs on the VM and collects traffic.</span></span>
1. <span data-ttu-id="f01ca-131">A csomag rögzítésfájl áttekintésre és diagnosztikai tárfiók van feltöltve.</span><span class="sxs-lookup"><span data-stu-id="f01ca-131">The packet capture file is uploaded to a storage account for review and diagnosis.</span></span>

<span data-ttu-id="f01ca-132">Ez a folyamat automatizálására azt hozzon létre és riasztást csatlakozni a virtuális Gépen indul el, ha az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="f01ca-132">To automate this process, we create and connect an alert on our VM to trigger when the incident occurs.</span></span> <span data-ttu-id="f01ca-133">Azt is hozzon létre egy függvényt hálózati figyelőt belülre hívni.</span><span class="sxs-lookup"><span data-stu-id="f01ca-133">We also create a function to call into Network Watcher.</span></span>

<span data-ttu-id="f01ca-134">Ebben a forgatókönyvben a következőket teszi:</span><span class="sxs-lookup"><span data-stu-id="f01ca-134">This scenario does the following:</span></span>

* <span data-ttu-id="f01ca-135">Egy Azure függvény, amely elindítja a csomagrögzítéssel hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f01ca-135">Creates an Azure function that starts a packet capture.</span></span>
* <span data-ttu-id="f01ca-136">Egy riasztási szabályt hoz létre a virtuális gépen, és konfigurálja a riasztási szabály Azure függvény.</span><span class="sxs-lookup"><span data-stu-id="f01ca-136">Creates an alert rule on a virtual machine and configures the alert rule to call the Azure function.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="f01ca-137">Egy Azure-függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="f01ca-137">Create an Azure function</span></span>

<span data-ttu-id="f01ca-138">Az első lépés, ha a riasztás feldolgozni, és hozzon létre egy csomagrögzítéssel egy Azure függvényben.</span><span class="sxs-lookup"><span data-stu-id="f01ca-138">The first step is to create an Azure function to process the alert and create a packet capture.</span></span>

1. <span data-ttu-id="f01ca-139">Az a [Azure-portálon](https://portal.azure.com), jelölje be **új** > **számítási** > **függvény App**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-139">In the [Azure portal](https://portal.azure.com), select **New** > **Compute** > **Function App**.</span></span>

    ![A függvény-alkalmazás létrehozása][1-1]

2. <span data-ttu-id="f01ca-141">Az a **függvény App** panelen adja meg a következő értékeket, és válassza ki **OK** az alkalmazás létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="f01ca-141">On the **Function App** blade, enter the following values, and then select **OK** to create the app:</span></span>

    |<span data-ttu-id="f01ca-142">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="f01ca-142">**Setting**</span></span> | <span data-ttu-id="f01ca-143">**Érték**</span><span class="sxs-lookup"><span data-stu-id="f01ca-143">**Value**</span></span> | <span data-ttu-id="f01ca-144">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="f01ca-144">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="f01ca-145">**Alkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="f01ca-145">**App name**</span></span>|<span data-ttu-id="f01ca-146">PacketCaptureExample</span><span class="sxs-lookup"><span data-stu-id="f01ca-146">PacketCaptureExample</span></span>|<span data-ttu-id="f01ca-147">A függvény alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="f01ca-147">The name of the function app.</span></span>|
    |<span data-ttu-id="f01ca-148">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="f01ca-148">**Subscription**</span></span>|<span data-ttu-id="f01ca-149">[Az előfizetés] Az előfizetés, amelyre a függvény alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f01ca-149">[Your subscription]The subscription for which to create the function app.</span></span>||
    |<span data-ttu-id="f01ca-150">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="f01ca-150">**Resource Group**</span></span>|<span data-ttu-id="f01ca-151">PacketCaptureRG</span><span class="sxs-lookup"><span data-stu-id="f01ca-151">PacketCaptureRG</span></span>|<span data-ttu-id="f01ca-152">A függvény alkalmazást tartalmazó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="f01ca-152">The resource group to contain the function app.</span></span>|
    |<span data-ttu-id="f01ca-153">**Üzemeltetési terv**</span><span class="sxs-lookup"><span data-stu-id="f01ca-153">**Hosting Plan**</span></span>|<span data-ttu-id="f01ca-154">Felhasználás terv</span><span class="sxs-lookup"><span data-stu-id="f01ca-154">Consumption Plan</span></span>| <span data-ttu-id="f01ca-155">Milyen típusú tervezze meg a függvény alkalmazás használ.</span><span class="sxs-lookup"><span data-stu-id="f01ca-155">The type of plan your function app uses.</span></span> <span data-ttu-id="f01ca-156">Ezek fogyasztás vagy az Azure App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="f01ca-156">Options are Consumption or Azure App Service plan.</span></span> |
    |<span data-ttu-id="f01ca-157">**Hely**</span><span class="sxs-lookup"><span data-stu-id="f01ca-157">**Location**</span></span>|<span data-ttu-id="f01ca-158">USA középső régiója</span><span class="sxs-lookup"><span data-stu-id="f01ca-158">Central US</span></span>| <span data-ttu-id="f01ca-159">A régió, ahol a függvény alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f01ca-159">The region in which to create the function app.</span></span>|
    |<span data-ttu-id="f01ca-160">**Tárfiók**</span><span class="sxs-lookup"><span data-stu-id="f01ca-160">**Storage Account**</span></span>|<span data-ttu-id="f01ca-161">{automatikusan létrehozott}</span><span class="sxs-lookup"><span data-stu-id="f01ca-161">{autogenerated}</span></span>| <span data-ttu-id="f01ca-162">A storage-fiók, amelyet az Azure Functions általános célú tárfiókok esetében.</span><span class="sxs-lookup"><span data-stu-id="f01ca-162">The storage account that Azure Functions needs for general-purpose storage.</span></span>|

3. <span data-ttu-id="f01ca-163">Az a **PacketCaptureExample függvény alkalmazások** panelen válassza **funkciók** > **egyéni függvény**  >  **+**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-163">On the **PacketCaptureExample Function Apps** blade, select **Functions** > **Custom function** >**+**.</span></span>

4. <span data-ttu-id="f01ca-164">Válassza ki **HttpTrigger-Powershell**, majd adja meg a fennmaradó adatokat.</span><span class="sxs-lookup"><span data-stu-id="f01ca-164">Select **HttpTrigger-Powershell**, and then enter the remaining information.</span></span> <span data-ttu-id="f01ca-165">Végezetül, a függvény létrehozásához válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-165">Finally, to create the function, select **Create**.</span></span>

    |<span data-ttu-id="f01ca-166">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="f01ca-166">**Setting**</span></span> | <span data-ttu-id="f01ca-167">**Érték**</span><span class="sxs-lookup"><span data-stu-id="f01ca-167">**Value**</span></span> | <span data-ttu-id="f01ca-168">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="f01ca-168">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="f01ca-169">**A forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="f01ca-169">**Scenario**</span></span>|<span data-ttu-id="f01ca-170">Kísérleti</span><span class="sxs-lookup"><span data-stu-id="f01ca-170">Experimental</span></span>|<span data-ttu-id="f01ca-171">Forgatókönyv típusa</span><span class="sxs-lookup"><span data-stu-id="f01ca-171">Type of scenario</span></span>|
    |<span data-ttu-id="f01ca-172">**A függvény neve**</span><span class="sxs-lookup"><span data-stu-id="f01ca-172">**Name your function**</span></span>|<span data-ttu-id="f01ca-173">AlertPacketCapturePowerShell</span><span class="sxs-lookup"><span data-stu-id="f01ca-173">AlertPacketCapturePowerShell</span></span>|<span data-ttu-id="f01ca-174">A függvény neve</span><span class="sxs-lookup"><span data-stu-id="f01ca-174">Name of the function</span></span>|
    |<span data-ttu-id="f01ca-175">**Jogosultsági szint**</span><span class="sxs-lookup"><span data-stu-id="f01ca-175">**Authorization level**</span></span>|<span data-ttu-id="f01ca-176">Függvény</span><span class="sxs-lookup"><span data-stu-id="f01ca-176">Function</span></span>|<span data-ttu-id="f01ca-177">A függvény jogosultsági szint</span><span class="sxs-lookup"><span data-stu-id="f01ca-177">Authorization level for the function</span></span>|

![Funkciók – példa][functions1]

> [!NOTE]
> <span data-ttu-id="f01ca-179">A PowerShell sablon kísérleti, és nem rendelkezik teljes körű támogatása.</span><span class="sxs-lookup"><span data-stu-id="f01ca-179">The PowerShell template is experimental and does not have full support.</span></span>

<span data-ttu-id="f01ca-180">Testreszabások szükséges ehhez a példához, és a következő lépéseket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f01ca-180">Customizations are required for this example and are explained in the following steps.</span></span>

### <a name="add-modules"></a><span data-ttu-id="f01ca-181">Modulok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f01ca-181">Add modules</span></span>

<span data-ttu-id="f01ca-182">Hálózati figyelő PowerShell-parancsmagok használatához töltse fel a legújabb PowerShell-modul a függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f01ca-182">To use Network Watcher PowerShell cmdlets, upload the latest PowerShell module to the function app.</span></span>

1. <span data-ttu-id="f01ca-183">A legújabb Azure PowerShell-modulok telepítése a helyi gépén futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="f01ca-183">On your local machine with the latest Azure PowerShell modules installed, run the following PowerShell command:</span></span>

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    <span data-ttu-id="f01ca-184">Ez a példa lehetővé teszi az Azure PowerShell-modulok helyi elérési útja.</span><span class="sxs-lookup"><span data-stu-id="f01ca-184">This example gives you the local path of your Azure PowerShell modules.</span></span> <span data-ttu-id="f01ca-185">Ezek a mappák egy későbbi lépésben használt.</span><span class="sxs-lookup"><span data-stu-id="f01ca-185">These folders are used in a later step.</span></span> <span data-ttu-id="f01ca-186">Az ebben a forgatókönyvben használt modulokra:</span><span class="sxs-lookup"><span data-stu-id="f01ca-186">The modules that are used in this scenario are:</span></span>

    * <span data-ttu-id="f01ca-187">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="f01ca-187">AzureRM.Network</span></span>

    * <span data-ttu-id="f01ca-188">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="f01ca-188">AzureRM.Profile</span></span>

    * <span data-ttu-id="f01ca-189">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="f01ca-189">AzureRM.Resources</span></span>

    ![PowerShell mappák][functions5]

1. <span data-ttu-id="f01ca-191">Válassza ki **Alkalmazásbeállítások működéséhez** > **nyissa meg az alkalmazás-szerkesztőt**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-191">Select **Function app settings** > **Go to App Service Editor**.</span></span>

    ![Függvény-Alkalmazásbeállítások][functions2]

1. <span data-ttu-id="f01ca-193">Kattintson a jobb gombbal a **AlertPacketCapturePowershell** mappát, majd hozzon létre egy nevű és **azuremodules**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-193">Right-click the **AlertPacketCapturePowershell** folder, and then create a folder called **azuremodules**.</span></span> 

4. <span data-ttu-id="f01ca-194">Hozzon létre egy almappát minden modul, amelyekre szüksége van.</span><span class="sxs-lookup"><span data-stu-id="f01ca-194">Create a subfolder for each module that you need.</span></span>

    ![Mappa- és almappákkal együtt][functions3]

    * <span data-ttu-id="f01ca-196">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="f01ca-196">AzureRM.Network</span></span>

    * <span data-ttu-id="f01ca-197">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="f01ca-197">AzureRM.Profile</span></span>

    * <span data-ttu-id="f01ca-198">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="f01ca-198">AzureRM.Resources</span></span>

1. <span data-ttu-id="f01ca-199">Kattintson a jobb gombbal a **AzureRM.Network** almappát, és válassza **fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-199">Right-click the **AzureRM.Network** subfolder, and then select **Upload Files**.</span></span> 

6. <span data-ttu-id="f01ca-200">Nyissa meg az Azure-modulok.</span><span class="sxs-lookup"><span data-stu-id="f01ca-200">Go to your Azure modules.</span></span> <span data-ttu-id="f01ca-201">A helyi **AzureRM.Network** mappát, válassza ki az összes fájl a mappában.</span><span class="sxs-lookup"><span data-stu-id="f01ca-201">In the local **AzureRM.Network** folder, select all the files in the folder.</span></span> <span data-ttu-id="f01ca-202">Válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-202">Then select **OK**.</span></span> 

7. <span data-ttu-id="f01ca-203">Ismételje meg ezeket a lépéseket **AzureRM.Profile** és **AzureRM.Resources**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-203">Repeat these steps for **AzureRM.Profile** and **AzureRM.Resources**.</span></span>

    ![Fájlok feltöltése][functions6]

1. <span data-ttu-id="f01ca-205">Miután elkészült, minden egyes a PowerShell modul fájlok a helyi számítógépről kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f01ca-205">After you've finished, each folder should have the PowerShell module files from your local machine.</span></span>

    ![PowerShell-fájlok][functions7]

### <a name="authentication"></a><span data-ttu-id="f01ca-207">Authentication</span><span class="sxs-lookup"><span data-stu-id="f01ca-207">Authentication</span></span>

<span data-ttu-id="f01ca-208">A PowerShell-parancsmagok használatához hitelesítenie kell.</span><span class="sxs-lookup"><span data-stu-id="f01ca-208">To use the PowerShell cmdlets, you must authenticate.</span></span> <span data-ttu-id="f01ca-209">A függvény alkalmazás állítsa be a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="f01ca-209">You configure authentication in the function app.</span></span> <span data-ttu-id="f01ca-210">A hitelesítést, konfigurálja a környezeti változók, és egy titkosított kulcs fájlt feltölteni a függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f01ca-210">To configure authentication, you must configure environment variables and upload an encrypted key file to the function app.</span></span>

> [!NOTE]
> <span data-ttu-id="f01ca-211">Ebben a forgatókönyvben az Azure Functions hitelesítési megvalósításának csak egy példát biztosít.</span><span class="sxs-lookup"><span data-stu-id="f01ca-211">This scenario provides just one example of how to implement authentication with Azure Functions.</span></span> <span data-ttu-id="f01ca-212">Ehhez más módja van.</span><span class="sxs-lookup"><span data-stu-id="f01ca-212">There are other ways to do this.</span></span>

#### <a name="encrypted-credentials"></a><span data-ttu-id="f01ca-213">Titkosított hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="f01ca-213">Encrypted credentials</span></span>

<span data-ttu-id="f01ca-214">A következő PowerShell-parancsfájl létrehoz egy kulcs nevű **PassEncryptKey.key**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-214">The following PowerShell script creates a key file called **PassEncryptKey.key**.</span></span> <span data-ttu-id="f01ca-215">A megadott jelszó egy titkosított verzióját is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f01ca-215">It also provides an encrypted version of the password that's supplied.</span></span> <span data-ttu-id="f01ca-216">Ez a jelszó nem ugyanazt a jelszót, amely a hitelesítéshez használt Azure Active Directory-alkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="f01ca-216">This password is the same password that is defined for the Azure Active Directory application that's used for authentication.</span></span>

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

<span data-ttu-id="f01ca-217">A App Service-szerkesztőben a függvény alkalmazás, hozzon létre egy nevű **kulcsok** alatt **AlertPacketCapturePowerShell**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-217">In the App Service Editor of the function app, create a folder called **keys** under **AlertPacketCapturePowerShell**.</span></span> <span data-ttu-id="f01ca-218">Majd töltse fel a **PassEncryptKey.key** az előző PowerShell minta létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="f01ca-218">Then upload the **PassEncryptKey.key** file that you created in the previous PowerShell sample.</span></span>

![Funkciók kulcs][functions8]

### <a name="retrieve-values-for-environment-variables"></a><span data-ttu-id="f01ca-220">A környezeti változók értékeit beolvasása</span><span class="sxs-lookup"><span data-stu-id="f01ca-220">Retrieve values for environment variables</span></span>

<span data-ttu-id="f01ca-221">A végső vonatkozó követelmény akkor állíthatja be a környezeti változók értékeit hitelesítési eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="f01ca-221">The final requirement is to set up the environment variables that are necessary to access the values for authentication.</span></span> <span data-ttu-id="f01ca-222">Az alábbi lista tartalmazza a környezeti változókat, amelyek jönnek létre:</span><span class="sxs-lookup"><span data-stu-id="f01ca-222">The following list shows the environment variables that are created:</span></span>

* <span data-ttu-id="f01ca-223">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="f01ca-223">AzureClientID</span></span>

* <span data-ttu-id="f01ca-224">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="f01ca-224">AzureTenant</span></span>

* <span data-ttu-id="f01ca-225">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="f01ca-225">AzureCredPassword</span></span>


#### <a name="azureclientid"></a><span data-ttu-id="f01ca-226">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="f01ca-226">AzureClientID</span></span>

<span data-ttu-id="f01ca-227">Az ügyfél-azonosító, az alkalmazás Azonosítóját a kérelmet az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="f01ca-227">The client ID is the Application ID of an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="f01ca-228">Ha még nem rendelkezik az alkalmazás, futtassa az alábbi példát követve hozzon létre egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f01ca-228">If you don't already have an application to use, run the following example to create an application.</span></span>

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > <span data-ttu-id="f01ca-229">Az alkalmazás létrehozásakor használt jelszót kell ugyanazt a jelszót, amely a korábban létrehozott a fájl mentése során.</span><span class="sxs-lookup"><span data-stu-id="f01ca-229">The password that you use when creating the application should be the same password that you created earlier when saving the key file.</span></span>

1. <span data-ttu-id="f01ca-230">Válassza ki az Azure-portálon **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-230">In the Azure portal, select **Subscriptions**.</span></span> <span data-ttu-id="f01ca-231">Válassza ki az előfizetést, és válassza a **hozzáférés-vezérlés (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-231">Select the subscription to use, and then select **Access control (IAM)**.</span></span>

    ![IAM funkciók][functions9]

1. <span data-ttu-id="f01ca-233">Válassza ki a fiókot, és válassza a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-233">Choose the account to use, and then select **Properties**.</span></span> <span data-ttu-id="f01ca-234">Másolja át az azonosítót.</span><span class="sxs-lookup"><span data-stu-id="f01ca-234">Copy the Application ID.</span></span>

    ![Funkciók Alkalmazásazonosító][functions10]

#### <a name="azuretenant"></a><span data-ttu-id="f01ca-236">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="f01ca-236">AzureTenant</span></span>

<span data-ttu-id="f01ca-237">A következő PowerShell-példa futtatásával beszerezni a bérlő azonosítója:</span><span class="sxs-lookup"><span data-stu-id="f01ca-237">Obtain the tenant ID  by running the following PowerShell sample:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a><span data-ttu-id="f01ca-238">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="f01ca-238">AzureCredPassword</span></span>

<span data-ttu-id="f01ca-239">A AzureCredPassword környezeti változó értéke a értéket, hogy fut a következő PowerShell-példa.</span><span class="sxs-lookup"><span data-stu-id="f01ca-239">The value of the AzureCredPassword environment variable is the value that you get from running the following PowerShell sample.</span></span> <span data-ttu-id="f01ca-240">Ez a példa értéke megegyezik az előző megjelenített **titkosított hitelesítő adatok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f01ca-240">This example is the same one that's shown in the preceding **Encrypted credentials** section.</span></span> <span data-ttu-id="f01ca-241">A szükséges érték a kimenetét a `$Encryptedpassword` változó.</span><span class="sxs-lookup"><span data-stu-id="f01ca-241">The value that's needed is the output of the `$Encryptedpassword` variable.</span></span>  <span data-ttu-id="f01ca-242">Ez az a szolgáltatás egyszerű jelszót, amely korábban a PowerShell parancsfájl használatával titkosított.</span><span class="sxs-lookup"><span data-stu-id="f01ca-242">This is the service principal password that you encrypted by using the PowerShell script.</span></span>

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-the-environment-variables"></a><span data-ttu-id="f01ca-243">A környezeti változók tárolásához</span><span class="sxs-lookup"><span data-stu-id="f01ca-243">Store the environment variables</span></span>

1. <span data-ttu-id="f01ca-244">Nyissa meg a függvény alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="f01ca-244">Go to the function app.</span></span> <span data-ttu-id="f01ca-245">Válassza ki **Alkalmazásbeállítások működéséhez** > **Alkalmazásbeállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-245">Then select **Function app settings** > **Configure app settings**.</span></span>

    ![Alkalmazásbeállítások konfigurálása][functions11]

1. <span data-ttu-id="f01ca-247">A környezeti változókhoz és értékeikhez vegye fel az alkalmazás beállításaiban, és válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-247">Add the environment variables and their values to the app settings, and then select **Save**.</span></span>

    ![Alkalmazásbeállítások][functions12]

### <a name="add-powershell-to-the-function"></a><span data-ttu-id="f01ca-249">A függvény PowerShell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f01ca-249">Add PowerShell to the function</span></span>

<span data-ttu-id="f01ca-250">Az Azure függvényen belül a hálózati figyelőt a hívásokat idő már.</span><span class="sxs-lookup"><span data-stu-id="f01ca-250">It's now time to make calls into Network Watcher from within the Azure function.</span></span> <span data-ttu-id="f01ca-251">Ez a függvény végrehajtása a követelményektől függően eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="f01ca-251">Depending on the requirements, the implementation of this function can vary.</span></span> <span data-ttu-id="f01ca-252">Azonban az általános folyamat a kód a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="f01ca-252">However, the general flow of the code is as follows:</span></span>

1. <span data-ttu-id="f01ca-253">Folyamat-bemeneti paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="f01ca-253">Process input parameters.</span></span>
2. <span data-ttu-id="f01ca-254">Meglévő csomag lekérdezési korlátok ellenőrzése és a névütközések feloldásához rögzíti.</span><span class="sxs-lookup"><span data-stu-id="f01ca-254">Query existing packet captures to verify limits and resolve name conflicts.</span></span>
3. <span data-ttu-id="f01ca-255">Hozzon létre egy csomagrögzítéssel megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="f01ca-255">Create a packet capture with appropriate parameters.</span></span>
4. <span data-ttu-id="f01ca-256">A lekérdezési csomag rögzítése rendszeres időközönként, amíg nem fejeződik be.</span><span class="sxs-lookup"><span data-stu-id="f01ca-256">Poll packet capture periodically until it's complete.</span></span>
5. <span data-ttu-id="f01ca-257">Értesítse a felhasználót, hogy a csomag rögzítési munkamenet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="f01ca-257">Notify the user that the packet capture session is complete.</span></span>

<span data-ttu-id="f01ca-258">A következő példa a függvényben használható PowerShell-kódjába.</span><span class="sxs-lookup"><span data-stu-id="f01ca-258">The following example is PowerShell code that can be used in the function.</span></span> <span data-ttu-id="f01ca-259">Le kell cserélni a igénylő érték **subscriptionId**, **resourceGroupName**, és **storageAccountName**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-259">There are values that need to be replaced for **subscriptionId**, **resourceGroupName**, and **storageAccountName**.</span></span>

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID to save captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a><span data-ttu-id="f01ca-260">A függvény URL-cím beolvasása</span><span class="sxs-lookup"><span data-stu-id="f01ca-260">Retrieve the function URL</span></span> 
1. <span data-ttu-id="f01ca-261">A függvény létrehozása után konfigurálja a riasztás hívni az URL-címet, a függvény van társítva.</span><span class="sxs-lookup"><span data-stu-id="f01ca-261">After you've created your function, configure your alert to call the URL that's associated with the function.</span></span> <span data-ttu-id="f01ca-262">Ahhoz, hogy ezt az értéket, a függvény app másolja a függvény URL-címet.</span><span class="sxs-lookup"><span data-stu-id="f01ca-262">To get this value, copy the function URL from your function app.</span></span>

    ![A függvény URL-cím keresése][functions13]

2. <span data-ttu-id="f01ca-264">Másolja a függvény app függvény URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f01ca-264">Copy the function URL for your function app.</span></span>

    ![A függvény URL-cím másolása][2]

<span data-ttu-id="f01ca-266">Ha a webhook POST kérelem hasznos az egyéni tulajdonságok van szüksége, tekintse meg a [olyan webhook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f01ca-266">If you require custom properties in the payload of the webhook POST request, refer to [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="configure-an-alert-on-a-vm"></a><span data-ttu-id="f01ca-267">Olyan riasztást konfigurálhat a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="f01ca-267">Configure an alert on a VM</span></span>

<span data-ttu-id="f01ca-268">Riasztások beállítható úgy, hogy egyéni felhasználók értesítése, ha egy adott metrika áthalad küszöb alá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="f01ca-268">Alerts can be configured to notify individuals when a specific metric crosses a threshold that's assigned to it.</span></span> <span data-ttu-id="f01ca-269">Ebben a példában a riasztás a TCP-szegmens küldött, de az is lehet figyelmeztetés, ha sok más metrikákkal.</span><span class="sxs-lookup"><span data-stu-id="f01ca-269">In this example, the alert is on the TCP segments that are sent, but the alert can be triggered for many other metrics.</span></span> <span data-ttu-id="f01ca-270">Ebben a példában a riasztást egy webhook függvény hívása van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f01ca-270">In this example, an alert is configured to call a webhook to call the function.</span></span>

### <a name="create-the-alert-rule"></a><span data-ttu-id="f01ca-271">A riasztási szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="f01ca-271">Create the alert rule</span></span>

<span data-ttu-id="f01ca-272">Ugrás a meglévő virtuális gépből, és adja hozzá a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="f01ca-272">Go to an existing virtual machine, and then add an alert rule.</span></span> <span data-ttu-id="f01ca-273">Riasztások konfigurálása vonatkozó részletes dokumentációt a helyen találhatók [hozzon létre riasztásokat Azure figyelése az Azure-szolgáltatások - Azure-portálon](../monitoring-and-diagnostics/insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f01ca-273">More detailed documentation about configuring alerts can be found at [Create alerts in Azure Monitor for Azure services - Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md).</span></span> <span data-ttu-id="f01ca-274">Adja meg a következő értékeket a **riasztási szabály** panelt, és válassza **OK**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-274">Enter the following values in the **Alert rule** blade, and then select **OK**.</span></span>

  |<span data-ttu-id="f01ca-275">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="f01ca-275">**Setting**</span></span> | <span data-ttu-id="f01ca-276">**Érték**</span><span class="sxs-lookup"><span data-stu-id="f01ca-276">**Value**</span></span> | <span data-ttu-id="f01ca-277">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="f01ca-277">**Details**</span></span> |
  |---|---|---|
  |<span data-ttu-id="f01ca-278">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="f01ca-278">**Name**</span></span>|<span data-ttu-id="f01ca-279">TCP_Segments_Sent_Exceeded</span><span class="sxs-lookup"><span data-stu-id="f01ca-279">TCP_Segments_Sent_Exceeded</span></span>|<span data-ttu-id="f01ca-280">A riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="f01ca-280">Name of the alert rule.</span></span>|
  |<span data-ttu-id="f01ca-281">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="f01ca-281">**Description**</span></span>|<span data-ttu-id="f01ca-282">TCP-szegmens küldött meghaladja a küszöbértéket</span><span class="sxs-lookup"><span data-stu-id="f01ca-282">TCP segments sent exceeded threshold</span></span>|<span data-ttu-id="f01ca-283">A riasztási szabály leírása.</span><span class="sxs-lookup"><span data-stu-id="f01ca-283">The description for the alert rule.</span></span>||
  |<span data-ttu-id="f01ca-284">**Metrika**</span><span class="sxs-lookup"><span data-stu-id="f01ca-284">**Metric**</span></span>|<span data-ttu-id="f01ca-285">Küldött TCP-szegmens</span><span class="sxs-lookup"><span data-stu-id="f01ca-285">TCP segments sent</span></span>| <span data-ttu-id="f01ca-286">A metrika a riasztás aktiválásához használatára.</span><span class="sxs-lookup"><span data-stu-id="f01ca-286">The metric to use to trigger the alert.</span></span> |
  |<span data-ttu-id="f01ca-287">**Az állapot**</span><span class="sxs-lookup"><span data-stu-id="f01ca-287">**Condition**</span></span>|<span data-ttu-id="f01ca-288">Nagyobb mint</span><span class="sxs-lookup"><span data-stu-id="f01ca-288">Greater than</span></span>| <span data-ttu-id="f01ca-289">Az állapotot, amelyet használni a mérték kiértékelése során.</span><span class="sxs-lookup"><span data-stu-id="f01ca-289">The condition to use when evaluating the metric.</span></span>|
  |<span data-ttu-id="f01ca-290">**Küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="f01ca-290">**Threshold**</span></span>|<span data-ttu-id="f01ca-291">100</span><span class="sxs-lookup"><span data-stu-id="f01ca-291">100</span></span>| <span data-ttu-id="f01ca-292">A metrika a riasztást kiváltó értéke.</span><span class="sxs-lookup"><span data-stu-id="f01ca-292">The  value of the metric that triggers the alert.</span></span> <span data-ttu-id="f01ca-293">Ez az érték a környezet érvényes értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="f01ca-293">This value should be set to a valid value for your environment.</span></span>|
  |<span data-ttu-id="f01ca-294">**Időtartam**</span><span class="sxs-lookup"><span data-stu-id="f01ca-294">**Period**</span></span>|<span data-ttu-id="f01ca-295">Az elmúlt öt percben</span><span class="sxs-lookup"><span data-stu-id="f01ca-295">Over the last five minutes</span></span>| <span data-ttu-id="f01ca-296">Meghatározza, hogy az időszak, amelyben a küszöbértéket a metrika a(z) keres.</span><span class="sxs-lookup"><span data-stu-id="f01ca-296">Determines the period in which to look for the threshold on the metric.</span></span>|
  |<span data-ttu-id="f01ca-297">**Webhook**</span><span class="sxs-lookup"><span data-stu-id="f01ca-297">**Webhook**</span></span>|<span data-ttu-id="f01ca-298">[webhook URL-CÍMÉT függvény alkalmazásból]</span><span class="sxs-lookup"><span data-stu-id="f01ca-298">[webhook URL from function app]</span></span>| <span data-ttu-id="f01ca-299">A webhook URL-CÍMÉT az előző lépésben létrehozott függvény alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="f01ca-299">The webhook URL from the function app that was created in the previous steps.</span></span>|

> [!NOTE]
> <span data-ttu-id="f01ca-300">A TCP-szegmens metrika alapértelmezés szerint nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="f01ca-300">The TCP segments metric is not enabled by default.</span></span> <span data-ttu-id="f01ca-301">További tudnivalókért látogasson el további metrikák engedélyezésével kapcsolatos [figyelés engedélyezésekor és diagnosztikai](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="f01ca-301">Learn more about how to enable additional metrics by visiting [Enable monitoring and diagnostics](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span></span>

## <a name="review-the-results"></a><span data-ttu-id="f01ca-302">Tekintse át az eredményeket</span><span class="sxs-lookup"><span data-stu-id="f01ca-302">Review the results</span></span>

<span data-ttu-id="f01ca-303">A riasztási eseményindítók feltételeit, miután a csomagrögzítéssel jön létre.</span><span class="sxs-lookup"><span data-stu-id="f01ca-303">After the criteria for the alert triggers, a packet capture is created.</span></span> <span data-ttu-id="f01ca-304">Ugrás a hálózati figyelőt, és válassza **csomagrögzítéssel**.</span><span class="sxs-lookup"><span data-stu-id="f01ca-304">Go to Network Watcher, and then select **Packet capture**.</span></span> <span data-ttu-id="f01ca-305">Ezen a lapon kiválaszthatja a csomag-rögzítési fájl a csomagrögzítéssel letöltésére mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="f01ca-305">On this page, you can select the packet capture file link to download the packet capture.</span></span>

![A csomagrögzítéssel megtekintése][functions14]

<span data-ttu-id="f01ca-307">Ha a rögzítési fájlt helyileg van tárolva, a virtuális géphez a bejelentkezéssel kérheti le.</span><span class="sxs-lookup"><span data-stu-id="f01ca-307">If the capture file is stored locally, you can retrieve it by signing in to the virtual machine.</span></span>

<span data-ttu-id="f01ca-308">Az Azure storage-fiókok fájlok letöltésére vonatkozó utasításokért lásd: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="f01ca-308">For instructions about downloading files from Azure storage accounts, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="f01ca-309">Használhat egy másik eszköz [Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="f01ca-309">Another tool you can use is [Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="f01ca-310">A rögzítési letöltése után tekintheti által olvasható eszközzel egy **.cap** fájlt.</span><span class="sxs-lookup"><span data-stu-id="f01ca-310">After your capture has been downloaded, you can view it by using any tool that can read a **.cap** file.</span></span> <span data-ttu-id="f01ca-311">Az alábbiakban két eszközökre mutató hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="f01ca-311">Following are links to two of these tools:</span></span>

- [<span data-ttu-id="f01ca-312">Microsoft Message Analyzert</span><span class="sxs-lookup"><span data-stu-id="f01ca-312">Microsoft Message Analyzer</span></span>](https://technet.microsoft.com/library/jj649776.aspx)
- [<span data-ttu-id="f01ca-313">WireShark</span><span class="sxs-lookup"><span data-stu-id="f01ca-313">WireShark</span></span>](https://www.wireshark.org/)

## <a name="next-steps"></a><span data-ttu-id="f01ca-314">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f01ca-314">Next steps</span></span>

<span data-ttu-id="f01ca-315">Útmutató a csomag rögzítésekre megtekintéséhez látogasson el [csomag eseményrögzítés elemzésének rendelkező Wireshark](network-watcher-deep-packet-inspection.md).</span><span class="sxs-lookup"><span data-stu-id="f01ca-315">Learn how to view your packet captures by visiting [Packet capture analysis with Wireshark](network-watcher-deep-packet-inspection.md).</span></span>


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
