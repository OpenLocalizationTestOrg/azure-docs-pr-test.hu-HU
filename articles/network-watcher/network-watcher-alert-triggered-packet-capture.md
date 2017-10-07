---
title: "aaaUse csomag rögzítési toodo proaktív hálózati figyelés a riasztások és az Azure Functions |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate riasztást kiváltott csomagrögzítéssel rendelkező Azure hálózati figyelőt"
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
ms.openlocfilehash: 4722a831f3a9d5537c0e6f53daba4dfc35d0cf24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a><span data-ttu-id="58f2a-103">Proaktív hálózatfigyelési riasztások és az Azure Functions csomagrögzítéssel használata</span><span class="sxs-lookup"><span data-stu-id="58f2a-103">Use packet capture for proactive network monitoring with alerts and Azure Functions</span></span>

<span data-ttu-id="58f2a-104">Hálózati figyelő csomagrögzítéssel rögzítési munkamenetek tootrack forgalom mindkét virtuális gépeket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="58f2a-104">Network Watcher packet capture creates capture sessions tootrack traffic in and out of virtual machines.</span></span> <span data-ttu-id="58f2a-105">hello rögzítési fájl lehet egy szűrő definiált tootrack csak hello megjeleníteni kívánt toomonitor forgalmat.</span><span class="sxs-lookup"><span data-stu-id="58f2a-105">hello capture file can have a filter that is defined tootrack only hello traffic that you want toomonitor.</span></span> <span data-ttu-id="58f2a-106">Ezeket az adatokat a tárolási blob vagy helyileg hello vendég gépen majd tárolja.</span><span class="sxs-lookup"><span data-stu-id="58f2a-106">This data is then stored in a storage blob or locally on hello guest machine.</span></span>

<span data-ttu-id="58f2a-107">Ez a funkció más automatizálási esetekben, például az Azure Functions távolról is indítható.</span><span class="sxs-lookup"><span data-stu-id="58f2a-107">This capability can be started remotely from other automation scenarios such as Azure Functions.</span></span> <span data-ttu-id="58f2a-108">Csomag rögzítési ad meg hello funkció toorun proaktív rögzítésekre alapján definiált hálózati rendellenességek észlelését.</span><span class="sxs-lookup"><span data-stu-id="58f2a-108">Packet capture gives you hello capability toorun proactive captures based on defined network anomalies.</span></span> <span data-ttu-id="58f2a-109">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, első hálózati behatolások, a hibakeresési ügyfél-kiszolgáló közötti kommunikációt, és további információkat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="58f2a-109">Other uses include gathering network statistics, getting information about network intrusions, debugging client-server communications, and more.</span></span>

<span data-ttu-id="58f2a-110">Az Azure rendszerbe telepítendő erőforrásokat 24/7 fut.</span><span class="sxs-lookup"><span data-stu-id="58f2a-110">Resources that are deployed in Azure run 24/7.</span></span> <span data-ttu-id="58f2a-111">Alkalmazottak nem aktívan figyeli a hello állapotát az összes erőforrás 24/7.</span><span class="sxs-lookup"><span data-stu-id="58f2a-111">You and your staff cannot actively monitor hello status of all resources 24/7.</span></span> <span data-ttu-id="58f2a-112">Például mi történik, ha hiba fordul elő, hajnali 2 Órakor?</span><span class="sxs-lookup"><span data-stu-id="58f2a-112">For example, what happens if an issue occurs at 2 AM?</span></span>

<span data-ttu-id="58f2a-113">Használatával hálózati figyelőt, riasztások, és hello Azure ökoszisztémán belüli funkciókat proaktív reagálhat hello adatok és eszközök toosolve problémák a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="58f2a-113">By using Network Watcher, alerting, and functions from within hello Azure ecosystem, you can proactively respond with hello data and tools toosolve problems in your network.</span></span>

![Forgatókönyv][scenario]

## <a name="prerequisites"></a><span data-ttu-id="58f2a-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="58f2a-115">Prerequisites</span></span>

* <span data-ttu-id="58f2a-116">hello legújabb verziójának [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="58f2a-116">hello latest version of [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span></span>
* <span data-ttu-id="58f2a-117">Hálózati figyelőt meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="58f2a-117">An existing instance of Network Watcher.</span></span> <span data-ttu-id="58f2a-118">Ha még nem rendelkezik egy, [hálózati figyelőt példányt létrehozni](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="58f2a-118">If you don't already have one, [create an instance of Network Watcher](network-watcher-create.md).</span></span>
* <span data-ttu-id="58f2a-119">Egy meglévő virtuális gép hello ugyanabban a régióban, hálózati figyelőt, hello [Windows bővítmény](../virtual-machines/windows/extensions-nwa.md) vagy [Linux virtuálisgép-bővítmény](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="58f2a-119">An existing virtual machine in hello same region as Network Watcher with hello [Windows extension](../virtual-machines/windows/extensions-nwa.md) or [Linux virtual machine extension](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="58f2a-120">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="58f2a-120">Scenario</span></span>

<span data-ttu-id="58f2a-121">Ebben a példában a szokásosnál több TCP-szegmens küld, és azt szeretné, hogy toobe riasztást kap.</span><span class="sxs-lookup"><span data-stu-id="58f2a-121">In this example, your VM is sending more TCP segments than usual, and you want toobe alerted.</span></span> <span data-ttu-id="58f2a-122">TCP-szegmens az itt példaként szolgálnak, de a figyelmeztetési állapotot használhatja.</span><span class="sxs-lookup"><span data-stu-id="58f2a-122">TCP segments are used as an example here, but you can use any alert condition.</span></span>

<span data-ttu-id="58f2a-123">Amikor a rendszer megkérdezi, érdemes tooreceive csomagszintű adatok toounderstand miért kommunikációs növekedett.</span><span class="sxs-lookup"><span data-stu-id="58f2a-123">When you are alerted, you want tooreceive packet-level data toounderstand why communication has increased.</span></span> <span data-ttu-id="58f2a-124">Majd lépéseket tooreturn hello virtuális gép tooregular kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="58f2a-124">Then you can take steps tooreturn hello virtual machine tooregular communication.</span></span>

<span data-ttu-id="58f2a-125">Ez a forgatókönyv feltételezi, hogy rendelkezik-e hálózati figyelőt, és egy erőforráscsoport, érvényes virtuális gépet a meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="58f2a-125">This scenario assumes that you have an existing instance of Network Watcher and a resource group with a valid virtual machine.</span></span>

<span data-ttu-id="58f2a-126">a következő lista hello a akkor történik meg a hello munkafolyamat nyújt áttekintést:</span><span class="sxs-lookup"><span data-stu-id="58f2a-126">hello following list is an overview of hello workflow that takes place:</span></span>

1. <span data-ttu-id="58f2a-127">Figyelmeztetés jelenik meg a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="58f2a-127">An alert is triggered on your VM.</span></span>
1. <span data-ttu-id="58f2a-128">hello riasztás meghívja az Azure-függvény olyan webhook keresztül.</span><span class="sxs-lookup"><span data-stu-id="58f2a-128">hello alert calls your Azure function via a webhook.</span></span>
1. <span data-ttu-id="58f2a-129">Az Azure-függvény hello riasztás feldolgozza, és hálózati figyelőt csomag rögzítési munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="58f2a-129">Your Azure function processes hello alert and starts a Network Watcher packet capture session.</span></span>
1. <span data-ttu-id="58f2a-130">hello csomagrögzítéssel hello virtuális gép fut, és összegyűjti a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="58f2a-130">hello packet capture runs on hello VM and collects traffic.</span></span>
1. <span data-ttu-id="58f2a-131">hello csomagok rögzítési fájl feltöltése tooa tárfiók áttekintésre és elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="58f2a-131">hello packet capture file is uploaded tooa storage account for review and diagnosis.</span></span>

<span data-ttu-id="58f2a-132">tooautomate Ez a folyamat azt létrehozása és csatlakoznak egy riasztást a virtuális gép tootrigger hello esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="58f2a-132">tooautomate this process, we create and connect an alert on our VM tootrigger when hello incident occurs.</span></span> <span data-ttu-id="58f2a-133">Azt is létrehozhat egy függvény toocall a hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="58f2a-133">We also create a function toocall into Network Watcher.</span></span>

<span data-ttu-id="58f2a-134">Ebben a forgatókönyvben hello a következő:</span><span class="sxs-lookup"><span data-stu-id="58f2a-134">This scenario does hello following:</span></span>

* <span data-ttu-id="58f2a-135">Egy Azure függvény, amely elindítja a csomagrögzítéssel hoz létre.</span><span class="sxs-lookup"><span data-stu-id="58f2a-135">Creates an Azure function that starts a packet capture.</span></span>
* <span data-ttu-id="58f2a-136">Egy riasztási szabályt hoz létre a virtuális gépen, és konfigurálja a hello riasztási szabály toocall hello Azure függvény.</span><span class="sxs-lookup"><span data-stu-id="58f2a-136">Creates an alert rule on a virtual machine and configures hello alert rule toocall hello Azure function.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="58f2a-137">Egy Azure-függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="58f2a-137">Create an Azure function</span></span>

<span data-ttu-id="58f2a-138">első lépés hello toocreate Azure függvény tooprocess hello riasztást, és hozzon létre egy csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="58f2a-138">hello first step is toocreate an Azure function tooprocess hello alert and create a packet capture.</span></span>

1. <span data-ttu-id="58f2a-139">A hello [Azure-portálon](https://portal.azure.com), jelölje be **új** > **számítási** > **függvény App**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-139">In hello [Azure portal](https://portal.azure.com), select **New** > **Compute** > **Function App**.</span></span>

    ![A függvény-alkalmazás létrehozása][1-1]

2. <span data-ttu-id="58f2a-141">A hello **függvény App** panelen adja meg a következő értékek hello, és válassza ki **OK** toocreate hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="58f2a-141">On hello **Function App** blade, enter hello following values, and then select **OK** toocreate hello app:</span></span>

    |<span data-ttu-id="58f2a-142">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="58f2a-142">**Setting**</span></span> | <span data-ttu-id="58f2a-143">**Érték**</span><span class="sxs-lookup"><span data-stu-id="58f2a-143">**Value**</span></span> | <span data-ttu-id="58f2a-144">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="58f2a-144">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="58f2a-145">**Alkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="58f2a-145">**App name**</span></span>|<span data-ttu-id="58f2a-146">PacketCaptureExample</span><span class="sxs-lookup"><span data-stu-id="58f2a-146">PacketCaptureExample</span></span>|<span data-ttu-id="58f2a-147">hello függvény alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="58f2a-147">hello name of hello function app.</span></span>|
    |<span data-ttu-id="58f2a-148">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="58f2a-148">**Subscription**</span></span>|<span data-ttu-id="58f2a-149">[Az előfizetés] hello melyik toocreate hello függvény alkalmazás-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="58f2a-149">[Your subscription]hello subscription for which toocreate hello function app.</span></span>||
    |<span data-ttu-id="58f2a-150">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="58f2a-150">**Resource Group**</span></span>|<span data-ttu-id="58f2a-151">PacketCaptureRG</span><span class="sxs-lookup"><span data-stu-id="58f2a-151">PacketCaptureRG</span></span>|<span data-ttu-id="58f2a-152">hello erőforrás csoport toocontain hello függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="58f2a-152">hello resource group toocontain hello function app.</span></span>|
    |<span data-ttu-id="58f2a-153">**Üzemeltetési terv**</span><span class="sxs-lookup"><span data-stu-id="58f2a-153">**Hosting Plan**</span></span>|<span data-ttu-id="58f2a-154">Felhasználás terv</span><span class="sxs-lookup"><span data-stu-id="58f2a-154">Consumption Plan</span></span>| <span data-ttu-id="58f2a-155">hello típusú tervezze meg a függvény alkalmazás használ.</span><span class="sxs-lookup"><span data-stu-id="58f2a-155">hello type of plan your function app uses.</span></span> <span data-ttu-id="58f2a-156">Ezek fogyasztás vagy az Azure App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="58f2a-156">Options are Consumption or Azure App Service plan.</span></span> |
    |<span data-ttu-id="58f2a-157">**Hely**</span><span class="sxs-lookup"><span data-stu-id="58f2a-157">**Location**</span></span>|<span data-ttu-id="58f2a-158">USA középső régiója</span><span class="sxs-lookup"><span data-stu-id="58f2a-158">Central US</span></span>| <span data-ttu-id="58f2a-159">hello régióban mely toocreate hello függvény alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="58f2a-159">hello region in which toocreate hello function app.</span></span>|
    |<span data-ttu-id="58f2a-160">**Tárfiók**</span><span class="sxs-lookup"><span data-stu-id="58f2a-160">**Storage Account**</span></span>|<span data-ttu-id="58f2a-161">{automatikusan létrehozott}</span><span class="sxs-lookup"><span data-stu-id="58f2a-161">{autogenerated}</span></span>| <span data-ttu-id="58f2a-162">hello storage-fiók, amelyet az Azure Functions általános célú tárfiókok esetében.</span><span class="sxs-lookup"><span data-stu-id="58f2a-162">hello storage account that Azure Functions needs for general-purpose storage.</span></span>|

3. <span data-ttu-id="58f2a-163">A hello **PacketCaptureExample függvény alkalmazások** panelen válassza **funkciók** > **egyéni függvény**  >  **+**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-163">On hello **PacketCaptureExample Function Apps** blade, select **Functions** > **Custom function** >**+**.</span></span>

4. <span data-ttu-id="58f2a-164">Válassza ki **HttpTrigger-Powershell**, majd adja meg a fennmaradó információk hello.</span><span class="sxs-lookup"><span data-stu-id="58f2a-164">Select **HttpTrigger-Powershell**, and then enter hello remaining information.</span></span> <span data-ttu-id="58f2a-165">Végezetül toocreate hello függvény, jelölje be **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-165">Finally, toocreate hello function, select **Create**.</span></span>

    |<span data-ttu-id="58f2a-166">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="58f2a-166">**Setting**</span></span> | <span data-ttu-id="58f2a-167">**Érték**</span><span class="sxs-lookup"><span data-stu-id="58f2a-167">**Value**</span></span> | <span data-ttu-id="58f2a-168">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="58f2a-168">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="58f2a-169">**A forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="58f2a-169">**Scenario**</span></span>|<span data-ttu-id="58f2a-170">Kísérleti</span><span class="sxs-lookup"><span data-stu-id="58f2a-170">Experimental</span></span>|<span data-ttu-id="58f2a-171">Forgatókönyv típusa</span><span class="sxs-lookup"><span data-stu-id="58f2a-171">Type of scenario</span></span>|
    |<span data-ttu-id="58f2a-172">**A függvény neve**</span><span class="sxs-lookup"><span data-stu-id="58f2a-172">**Name your function**</span></span>|<span data-ttu-id="58f2a-173">AlertPacketCapturePowerShell</span><span class="sxs-lookup"><span data-stu-id="58f2a-173">AlertPacketCapturePowerShell</span></span>|<span data-ttu-id="58f2a-174">Hello függvény neve</span><span class="sxs-lookup"><span data-stu-id="58f2a-174">Name of hello function</span></span>|
    |<span data-ttu-id="58f2a-175">**Jogosultsági szint**</span><span class="sxs-lookup"><span data-stu-id="58f2a-175">**Authorization level**</span></span>|<span data-ttu-id="58f2a-176">Függvény</span><span class="sxs-lookup"><span data-stu-id="58f2a-176">Function</span></span>|<span data-ttu-id="58f2a-177">Jogosultsági szint hello függvényhez</span><span class="sxs-lookup"><span data-stu-id="58f2a-177">Authorization level for hello function</span></span>|

![Funkciók – példa][functions1]

> [!NOTE]
> <span data-ttu-id="58f2a-179">hello PowerShell sablon kísérleti, és nem rendelkezik teljes körű támogatása.</span><span class="sxs-lookup"><span data-stu-id="58f2a-179">hello PowerShell template is experimental and does not have full support.</span></span>

<span data-ttu-id="58f2a-180">Testreszabások szükséges ehhez a példához, és a lépéseket követve hello szakaszban találhatók.</span><span class="sxs-lookup"><span data-stu-id="58f2a-180">Customizations are required for this example and are explained in hello following steps.</span></span>

### <a name="add-modules"></a><span data-ttu-id="58f2a-181">Modulok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="58f2a-181">Add modules</span></span>

<span data-ttu-id="58f2a-182">Hálózati figyelő PowerShell-parancsmagjainak toouse hello legújabb PowerShell modul toohello függvény alkalmazás feltöltése.</span><span class="sxs-lookup"><span data-stu-id="58f2a-182">toouse Network Watcher PowerShell cmdlets, upload hello latest PowerShell module toohello function app.</span></span>

1. <span data-ttu-id="58f2a-183">A hello legújabb Azure PowerShell-modulok telepítése a helyi gépén futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="58f2a-183">On your local machine with hello latest Azure PowerShell modules installed, run hello following PowerShell command:</span></span>

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    <span data-ttu-id="58f2a-184">A példa tesz lehetővé, hogy hello helyi elérési útja az Azure PowerShell-modulok.</span><span class="sxs-lookup"><span data-stu-id="58f2a-184">This example gives you hello local path of your Azure PowerShell modules.</span></span> <span data-ttu-id="58f2a-185">Ezek a mappák egy későbbi lépésben használt.</span><span class="sxs-lookup"><span data-stu-id="58f2a-185">These folders are used in a later step.</span></span> <span data-ttu-id="58f2a-186">hello ebben a forgatókönyvben használt modulokra:</span><span class="sxs-lookup"><span data-stu-id="58f2a-186">hello modules that are used in this scenario are:</span></span>

    * <span data-ttu-id="58f2a-187">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="58f2a-187">AzureRM.Network</span></span>

    * <span data-ttu-id="58f2a-188">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="58f2a-188">AzureRM.Profile</span></span>

    * <span data-ttu-id="58f2a-189">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="58f2a-189">AzureRM.Resources</span></span>

    ![PowerShell mappák][functions5]

1. <span data-ttu-id="58f2a-191">Válassza ki **Alkalmazásbeállítások működéséhez** > **nyissa meg a szolgáltatás szerkesztő tooApp**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-191">Select **Function app settings** > **Go tooApp Service Editor**.</span></span>

    ![Függvény-Alkalmazásbeállítások][functions2]

1. <span data-ttu-id="58f2a-193">Kattintson a jobb gombbal hello **AlertPacketCapturePowershell** mappát, majd hozzon létre egy nevű és **azuremodules**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-193">Right-click hello **AlertPacketCapturePowershell** folder, and then create a folder called **azuremodules**.</span></span> 

4. <span data-ttu-id="58f2a-194">Hozzon létre egy almappát minden modul, amelyekre szüksége van.</span><span class="sxs-lookup"><span data-stu-id="58f2a-194">Create a subfolder for each module that you need.</span></span>

    ![Mappa- és almappákkal együtt][functions3]

    * <span data-ttu-id="58f2a-196">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="58f2a-196">AzureRM.Network</span></span>

    * <span data-ttu-id="58f2a-197">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="58f2a-197">AzureRM.Profile</span></span>

    * <span data-ttu-id="58f2a-198">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="58f2a-198">AzureRM.Resources</span></span>

1. <span data-ttu-id="58f2a-199">Kattintson a jobb gombbal hello **AzureRM.Network** almappát, és válassza **fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-199">Right-click hello **AzureRM.Network** subfolder, and then select **Upload Files**.</span></span> 

6. <span data-ttu-id="58f2a-200">Nyissa meg tooyour Azure modulok.</span><span class="sxs-lookup"><span data-stu-id="58f2a-200">Go tooyour Azure modules.</span></span> <span data-ttu-id="58f2a-201">Hello helyi **AzureRM.Network** mappát, válassza ki az összes hello fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="58f2a-201">In hello local **AzureRM.Network** folder, select all hello files in hello folder.</span></span> <span data-ttu-id="58f2a-202">Válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-202">Then select **OK**.</span></span> 

7. <span data-ttu-id="58f2a-203">Ismételje meg ezeket a lépéseket **AzureRM.Profile** és **AzureRM.Resources**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-203">Repeat these steps for **AzureRM.Profile** and **AzureRM.Resources**.</span></span>

    ![Fájlok feltöltése][functions6]

1. <span data-ttu-id="58f2a-205">Miután elkészült, minden egyes hello PowerShell modul fájlok a helyi számítógépről kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="58f2a-205">After you've finished, each folder should have hello PowerShell module files from your local machine.</span></span>

    ![PowerShell-fájlok][functions7]

### <a name="authentication"></a><span data-ttu-id="58f2a-207">Authentication</span><span class="sxs-lookup"><span data-stu-id="58f2a-207">Authentication</span></span>

<span data-ttu-id="58f2a-208">toouse hello PowerShell-parancsmagok, hitelesítenie kell.</span><span class="sxs-lookup"><span data-stu-id="58f2a-208">toouse hello PowerShell cmdlets, you must authenticate.</span></span> <span data-ttu-id="58f2a-209">Hitelesítés konfigurálása a hello függvény alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="58f2a-209">You configure authentication in hello function app.</span></span> <span data-ttu-id="58f2a-210">tooconfigure hitelesítési kell konfigurálnia a környezeti változók és egy titkosított fájl toohello függvény alkalmazás feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="58f2a-210">tooconfigure authentication, you must configure environment variables and upload an encrypted key file toohello function app.</span></span>

> [!NOTE]
> <span data-ttu-id="58f2a-211">Ebben a forgatókönyvben csak egy példa bemutatja, hogyan nyújt az Azure Functions tooimplement hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="58f2a-211">This scenario provides just one example of how tooimplement authentication with Azure Functions.</span></span> <span data-ttu-id="58f2a-212">Nincsenek más módokon toodo ez.</span><span class="sxs-lookup"><span data-stu-id="58f2a-212">There are other ways toodo this.</span></span>

#### <a name="encrypted-credentials"></a><span data-ttu-id="58f2a-213">Titkosított hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="58f2a-213">Encrypted credentials</span></span>

<span data-ttu-id="58f2a-214">a következő PowerShell-parancsfájl hello létrehoz egy kulcs nevű **PassEncryptKey.key**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-214">hello following PowerShell script creates a key file called **PassEncryptKey.key**.</span></span> <span data-ttu-id="58f2a-215">Egy megadott hello jelszó titkosított verzióját is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="58f2a-215">It also provides an encrypted version of hello password that's supplied.</span></span> <span data-ttu-id="58f2a-216">Ez a jelszó nem hello ugyanazt a jelszót, amely a hitelesítéshez használt hello Azure Active Directory-alkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="58f2a-216">This password is hello same password that is defined for hello Azure Active Directory application that's used for authentication.</span></span>

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

<span data-ttu-id="58f2a-217">A hello App Service-szerkesztő hello függvény alkalmazás, hozzon létre egy nevű **kulcsok** alatt **AlertPacketCapturePowerShell**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-217">In hello App Service Editor of hello function app, create a folder called **keys** under **AlertPacketCapturePowerShell**.</span></span> <span data-ttu-id="58f2a-218">Majd feltölteni hello **PassEncryptKey.key** hello előző PowerShell minta létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="58f2a-218">Then upload hello **PassEncryptKey.key** file that you created in hello previous PowerShell sample.</span></span>

![Funkciók kulcs][functions8]

### <a name="retrieve-values-for-environment-variables"></a><span data-ttu-id="58f2a-220">A környezeti változók értékeit beolvasása</span><span class="sxs-lookup"><span data-stu-id="58f2a-220">Retrieve values for environment variables</span></span>

<span data-ttu-id="58f2a-221">hello végső vonatkozó követelmény akkor tooset hello környezeti változókat, amelyek a hitelesítéshez szükséges tooaccess hello értékek fel.</span><span class="sxs-lookup"><span data-stu-id="58f2a-221">hello final requirement is tooset up hello environment variables that are necessary tooaccess hello values for authentication.</span></span> <span data-ttu-id="58f2a-222">hello alábbi lista mutatja azokat létrehozott hello környezeti változók:</span><span class="sxs-lookup"><span data-stu-id="58f2a-222">hello following list shows hello environment variables that are created:</span></span>

* <span data-ttu-id="58f2a-223">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="58f2a-223">AzureClientID</span></span>

* <span data-ttu-id="58f2a-224">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="58f2a-224">AzureTenant</span></span>

* <span data-ttu-id="58f2a-225">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="58f2a-225">AzureCredPassword</span></span>


#### <a name="azureclientid"></a><span data-ttu-id="58f2a-226">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="58f2a-226">AzureClientID</span></span>

<span data-ttu-id="58f2a-227">hello ügyfél-azonosító hello egy alkalmazás az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="58f2a-227">hello client ID is hello Application ID of an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="58f2a-228">Ha még nem rendelkezik az alkalmazás toouse, futtassa a következő példa toocreate hello egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="58f2a-228">If you don't already have an application toouse, run hello following example toocreate an application.</span></span>

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > <span data-ttu-id="58f2a-229">hello jelszó használni, amikor hello alkalmazás létrehozása hello kell ugyanazt a jelszót, amely a korábban létrehozott hello kulcsfájl mentése során.</span><span class="sxs-lookup"><span data-stu-id="58f2a-229">hello password that you use when creating hello application should be hello same password that you created earlier when saving hello key file.</span></span>

1. <span data-ttu-id="58f2a-230">Hello Azure-portálon, válassza ki **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-230">In hello Azure portal, select **Subscriptions**.</span></span> <span data-ttu-id="58f2a-231">Hello előfizetés toouse, majd válassza ki és **hozzáférés-vezérlés (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-231">Select hello subscription toouse, and then select **Access control (IAM)**.</span></span>

    ![IAM funkciók][functions9]

1. <span data-ttu-id="58f2a-233">Válassza ki a hello fiók toouse, majd válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-233">Choose hello account toouse, and then select **Properties**.</span></span> <span data-ttu-id="58f2a-234">Másolás hello azonosítót.</span><span class="sxs-lookup"><span data-stu-id="58f2a-234">Copy hello Application ID.</span></span>

    ![Funkciók Alkalmazásazonosító][functions10]

#### <a name="azuretenant"></a><span data-ttu-id="58f2a-236">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="58f2a-236">AzureTenant</span></span>

<span data-ttu-id="58f2a-237">A következő PowerShell-példa hello futtatásával beszerezni hello bérlő azonosítója:</span><span class="sxs-lookup"><span data-stu-id="58f2a-237">Obtain hello tenant ID  by running hello following PowerShell sample:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a><span data-ttu-id="58f2a-238">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="58f2a-238">AzureCredPassword</span></span>

<span data-ttu-id="58f2a-239">hello hello AzureCredPassword környezeti változó értéke hello értéket kapott a következő PowerShell-példa hello futtatását.</span><span class="sxs-lookup"><span data-stu-id="58f2a-239">hello value of hello AzureCredPassword environment variable is hello value that you get from running hello following PowerShell sample.</span></span> <span data-ttu-id="58f2a-240">Ez a példa hello van, hogy megjelenik-e a fenti hello ugyanaz **titkosított hitelesítő adatok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="58f2a-240">This example is hello same one that's shown in hello preceding **Encrypted credentials** section.</span></span> <span data-ttu-id="58f2a-241">érték, amely szükséges hello hello hello kimenete `$Encryptedpassword` változó.</span><span class="sxs-lookup"><span data-stu-id="58f2a-241">hello value that's needed is hello output of hello `$Encryptedpassword` variable.</span></span>  <span data-ttu-id="58f2a-242">Ez a hello szolgáltatás egyszerű jelszót, amely korábban titkosított hello PowerShell parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="58f2a-242">This is hello service principal password that you encrypted by using hello PowerShell script.</span></span>

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

### <a name="store-hello-environment-variables"></a><span data-ttu-id="58f2a-243">Tároló hello környezeti változók</span><span class="sxs-lookup"><span data-stu-id="58f2a-243">Store hello environment variables</span></span>

1. <span data-ttu-id="58f2a-244">Nyissa meg a toohello függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="58f2a-244">Go toohello function app.</span></span> <span data-ttu-id="58f2a-245">Válassza ki **Alkalmazásbeállítások működéséhez** > **Alkalmazásbeállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-245">Then select **Function app settings** > **Configure app settings**.</span></span>

    ![Alkalmazásbeállítások konfigurálása][functions11]

1. <span data-ttu-id="58f2a-247">Adja hozzá a hello környezeti változók és értékek toohello alkalmazás beállításai, és válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-247">Add hello environment variables and their values toohello app settings, and then select **Save**.</span></span>

    ![Alkalmazásbeállítások][functions12]

### <a name="add-powershell-toohello-function"></a><span data-ttu-id="58f2a-249">PowerShell toohello függvény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="58f2a-249">Add PowerShell toohello function</span></span>

<span data-ttu-id="58f2a-250">Most idő toomake hívja be a hálózati figyelőt hello Azure függvény belül van.</span><span class="sxs-lookup"><span data-stu-id="58f2a-250">It's now time toomake calls into Network Watcher from within hello Azure function.</span></span> <span data-ttu-id="58f2a-251">Ez a függvény végrehajtása hello hello követelményeitől függően eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="58f2a-251">Depending on hello requirements, hello implementation of this function can vary.</span></span> <span data-ttu-id="58f2a-252">Azonban hello általános folyamata hello-szabályzat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="58f2a-252">However, hello general flow of hello code is as follows:</span></span>

1. <span data-ttu-id="58f2a-253">Folyamat-bemeneti paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="58f2a-253">Process input parameters.</span></span>
2. <span data-ttu-id="58f2a-254">Meglévő csomagot tooverify korlátok rögzíti, és a névütközések feloldásához.</span><span class="sxs-lookup"><span data-stu-id="58f2a-254">Query existing packet captures tooverify limits and resolve name conflicts.</span></span>
3. <span data-ttu-id="58f2a-255">Hozzon létre egy csomagrögzítéssel megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="58f2a-255">Create a packet capture with appropriate parameters.</span></span>
4. <span data-ttu-id="58f2a-256">A lekérdezési csomag rögzítése rendszeres időközönként, amíg nem fejeződik be.</span><span class="sxs-lookup"><span data-stu-id="58f2a-256">Poll packet capture periodically until it's complete.</span></span>
5. <span data-ttu-id="58f2a-257">Hello felhasználó értesítése, hogy hello csomagok rögzítési munkamenet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="58f2a-257">Notify hello user that hello packet capture session is complete.</span></span>

<span data-ttu-id="58f2a-258">hello következő példa egy PowerShell-kódjába hello függvényben használható.</span><span class="sxs-lookup"><span data-stu-id="58f2a-258">hello following example is PowerShell code that can be used in hello function.</span></span> <span data-ttu-id="58f2a-259">Érték helyére toobe igénylő **subscriptionId**, **resourceGroupName**, és **storageAccountName**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-259">There are values that need toobe replaced for **subscriptionId**, **resourceGroupName**, and **storageAccountName**.</span></span>

```powershell
            #Import Azure PowerShell modules required toomake calls tooNetwork Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID toosave captures in
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


            #Get hello VM that fired hello alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get hello Network Watcher in hello VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by hello function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on hello VM that fired hello alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-hello-function-url"></a><span data-ttu-id="58f2a-260">Hello függvény URL-cím beolvasása</span><span class="sxs-lookup"><span data-stu-id="58f2a-260">Retrieve hello function URL</span></span> 
1. <span data-ttu-id="58f2a-261">Miután létrehozta a funkciót, a riasztás toocall hello URL-CÍMEK konfigurálása hello függvény társított.</span><span class="sxs-lookup"><span data-stu-id="58f2a-261">After you've created your function, configure your alert toocall hello URL that's associated with hello function.</span></span> <span data-ttu-id="58f2a-262">tooget ezt az értéket, hello függvény URL-CÍMÉT a függvény alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="58f2a-262">tooget this value, copy hello function URL from your function app.</span></span>

    ![Hello függvény URL-cím keresése][functions13]

2. <span data-ttu-id="58f2a-264">A függvény alkalmazás hello függvény URL-Címének másolása.</span><span class="sxs-lookup"><span data-stu-id="58f2a-264">Copy hello function URL for your function app.</span></span>

    ![Hello függvény URL-cím másolása][2]

<span data-ttu-id="58f2a-266">Ha egyéni tulajdonságok hello webhook POST kérelem hasznos hello van szüksége, tekintse meg a túl[olyan webhook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="58f2a-266">If you require custom properties in hello payload of hello webhook POST request, refer too[Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="configure-an-alert-on-a-vm"></a><span data-ttu-id="58f2a-267">Olyan riasztást konfigurálhat a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="58f2a-267">Configure an alert on a VM</span></span>

<span data-ttu-id="58f2a-268">A riasztások lehetnek konfigurált toonotify egyéni felhasználók számára, egy adott metrika ebbe a küszöbérték, amely hozzá van rendelve tooit.</span><span class="sxs-lookup"><span data-stu-id="58f2a-268">Alerts can be configured toonotify individuals when a specific metric crosses a threshold that's assigned tooit.</span></span> <span data-ttu-id="58f2a-269">Ebben a példában hello riasztás hello a TCP-szegmens küldött, de hello is lehet figyelmeztetés, ha sok más metrikákkal.</span><span class="sxs-lookup"><span data-stu-id="58f2a-269">In this example, hello alert is on hello TCP segments that are sent, but hello alert can be triggered for many other metrics.</span></span> <span data-ttu-id="58f2a-270">Ebben a példában a riasztás konfigurált toocall webhook toocall hello függvény.</span><span class="sxs-lookup"><span data-stu-id="58f2a-270">In this example, an alert is configured toocall a webhook toocall hello function.</span></span>

### <a name="create-hello-alert-rule"></a><span data-ttu-id="58f2a-271">Hello riasztási szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="58f2a-271">Create hello alert rule</span></span>

<span data-ttu-id="58f2a-272">Nyissa meg tooan meglévő virtuális gép, és adja hozzá a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="58f2a-272">Go tooan existing virtual machine, and then add an alert rule.</span></span> <span data-ttu-id="58f2a-273">Riasztások konfigurálása vonatkozó részletes dokumentációt a helyen találhatók [hozzon létre riasztásokat Azure figyelése az Azure-szolgáltatások - Azure-portálon](../monitoring-and-diagnostics/insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="58f2a-273">More detailed documentation about configuring alerts can be found at [Create alerts in Azure Monitor for Azure services - Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md).</span></span> <span data-ttu-id="58f2a-274">Adja meg a következő értékek hello hello **riasztási szabály** panelt, és válassza **OK**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-274">Enter hello following values in hello **Alert rule** blade, and then select **OK**.</span></span>

  |<span data-ttu-id="58f2a-275">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="58f2a-275">**Setting**</span></span> | <span data-ttu-id="58f2a-276">**Érték**</span><span class="sxs-lookup"><span data-stu-id="58f2a-276">**Value**</span></span> | <span data-ttu-id="58f2a-277">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="58f2a-277">**Details**</span></span> |
  |---|---|---|
  |<span data-ttu-id="58f2a-278">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="58f2a-278">**Name**</span></span>|<span data-ttu-id="58f2a-279">TCP_Segments_Sent_Exceeded</span><span class="sxs-lookup"><span data-stu-id="58f2a-279">TCP_Segments_Sent_Exceeded</span></span>|<span data-ttu-id="58f2a-280">Hello riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="58f2a-280">Name of hello alert rule.</span></span>|
  |<span data-ttu-id="58f2a-281">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="58f2a-281">**Description**</span></span>|<span data-ttu-id="58f2a-282">TCP-szegmens küldött meghaladja a küszöbértéket</span><span class="sxs-lookup"><span data-stu-id="58f2a-282">TCP segments sent exceeded threshold</span></span>|<span data-ttu-id="58f2a-283">riasztási szabály hello hello leírása.</span><span class="sxs-lookup"><span data-stu-id="58f2a-283">hello description for hello alert rule.</span></span>||
  |<span data-ttu-id="58f2a-284">**Metrika**</span><span class="sxs-lookup"><span data-stu-id="58f2a-284">**Metric**</span></span>|<span data-ttu-id="58f2a-285">Küldött TCP-szegmens</span><span class="sxs-lookup"><span data-stu-id="58f2a-285">TCP segments sent</span></span>| <span data-ttu-id="58f2a-286">hello metrika toouse tootrigger hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="58f2a-286">hello metric toouse tootrigger hello alert.</span></span> |
  |<span data-ttu-id="58f2a-287">**Az állapot**</span><span class="sxs-lookup"><span data-stu-id="58f2a-287">**Condition**</span></span>|<span data-ttu-id="58f2a-288">Nagyobb mint</span><span class="sxs-lookup"><span data-stu-id="58f2a-288">Greater than</span></span>| <span data-ttu-id="58f2a-289">hello feltétel toouse hello metrika kiértékelése során.</span><span class="sxs-lookup"><span data-stu-id="58f2a-289">hello condition toouse when evaluating hello metric.</span></span>|
  |<span data-ttu-id="58f2a-290">**Küszöbérték**</span><span class="sxs-lookup"><span data-stu-id="58f2a-290">**Threshold**</span></span>|<span data-ttu-id="58f2a-291">100</span><span class="sxs-lookup"><span data-stu-id="58f2a-291">100</span></span>| <span data-ttu-id="58f2a-292">hello hello mérőszám hello riasztást kiváltó érték.</span><span class="sxs-lookup"><span data-stu-id="58f2a-292">hello  value of hello metric that triggers hello alert.</span></span> <span data-ttu-id="58f2a-293">Ezt az értéket meg kell tooa érvényes értéket a környezetében.</span><span class="sxs-lookup"><span data-stu-id="58f2a-293">This value should be set tooa valid value for your environment.</span></span>|
  |<span data-ttu-id="58f2a-294">**Időtartam**</span><span class="sxs-lookup"><span data-stu-id="58f2a-294">**Period**</span></span>|<span data-ttu-id="58f2a-295">Hello utolsó öt perc alatt</span><span class="sxs-lookup"><span data-stu-id="58f2a-295">Over hello last five minutes</span></span>| <span data-ttu-id="58f2a-296">Meghatározza, hogy a hello metrika hello küszöb hello mely toolook időszak.</span><span class="sxs-lookup"><span data-stu-id="58f2a-296">Determines hello period in which toolook for hello threshold on hello metric.</span></span>|
  |<span data-ttu-id="58f2a-297">**Webhook**</span><span class="sxs-lookup"><span data-stu-id="58f2a-297">**Webhook**</span></span>|<span data-ttu-id="58f2a-298">[webhook URL-CÍMÉT függvény alkalmazásból]</span><span class="sxs-lookup"><span data-stu-id="58f2a-298">[webhook URL from function app]</span></span>| <span data-ttu-id="58f2a-299">hello webhook URL-CÍMÉT alkalmazásból hello függvény hello előző lépésekben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="58f2a-299">hello webhook URL from hello function app that was created in hello previous steps.</span></span>|

> [!NOTE]
> <span data-ttu-id="58f2a-300">hello TCP szegmensek metrika alapértelmezés szerint nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="58f2a-300">hello TCP segments metric is not enabled by default.</span></span> <span data-ttu-id="58f2a-301">További tudnivalók tooenable további metrikák ellátogatva [figyelés engedélyezésekor és diagnosztikai](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="58f2a-301">Learn more about how tooenable additional metrics by visiting [Enable monitoring and diagnostics](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span></span>

## <a name="review-hello-results"></a><span data-ttu-id="58f2a-302">Hello eredményeinek áttekintése</span><span class="sxs-lookup"><span data-stu-id="58f2a-302">Review hello results</span></span>

<span data-ttu-id="58f2a-303">Hello riasztási eseményindítók hello feltételek, miután a csomagrögzítéssel jön létre.</span><span class="sxs-lookup"><span data-stu-id="58f2a-303">After hello criteria for hello alert triggers, a packet capture is created.</span></span> <span data-ttu-id="58f2a-304">Nyissa meg tooNetwork megfigyelő, és válassza ki **csomagrögzítéssel**.</span><span class="sxs-lookup"><span data-stu-id="58f2a-304">Go tooNetwork Watcher, and then select **Packet capture**.</span></span> <span data-ttu-id="58f2a-305">Ezen a lapon kiválaszthatja a hello csomagok rögzítési fájl hivatkozás toodownload hello csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="58f2a-305">On this page, you can select hello packet capture file link toodownload hello packet capture.</span></span>

![A csomagrögzítéssel megtekintése][functions14]

<span data-ttu-id="58f2a-307">Ha hello rögzítési fájlt helyileg van tárolva, a bejelentkezéssel toohello virtuális gép kérheti le.</span><span class="sxs-lookup"><span data-stu-id="58f2a-307">If hello capture file is stored locally, you can retrieve it by signing in toohello virtual machine.</span></span>

<span data-ttu-id="58f2a-308">Az Azure storage-fiókok fájlok letöltésére vonatkozó utasításokért lásd: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="58f2a-308">For instructions about downloading files from Azure storage accounts, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="58f2a-309">Használhat egy másik eszköz [Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="58f2a-309">Another tool you can use is [Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="58f2a-310">A rögzítési letöltése után tekintheti által olvasható eszközzel egy **.cap** fájlt.</span><span class="sxs-lookup"><span data-stu-id="58f2a-310">After your capture has been downloaded, you can view it by using any tool that can read a **.cap** file.</span></span> <span data-ttu-id="58f2a-311">Az alábbiakban ezen eszközök hivatkozások tootwo:</span><span class="sxs-lookup"><span data-stu-id="58f2a-311">Following are links tootwo of these tools:</span></span>

- [<span data-ttu-id="58f2a-312">Microsoft Message Analyzert</span><span class="sxs-lookup"><span data-stu-id="58f2a-312">Microsoft Message Analyzer</span></span>](https://technet.microsoft.com/library/jj649776.aspx)
- [<span data-ttu-id="58f2a-313">WireShark</span><span class="sxs-lookup"><span data-stu-id="58f2a-313">WireShark</span></span>](https://www.wireshark.org/)

## <a name="next-steps"></a><span data-ttu-id="58f2a-314">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58f2a-314">Next steps</span></span>

<span data-ttu-id="58f2a-315">Ismerje meg, hogyan tooview a csomag rögzíti ellátogatva [csomag eseményrögzítés elemzésének rendelkező Wireshark](network-watcher-deep-packet-inspection.md).</span><span class="sxs-lookup"><span data-stu-id="58f2a-315">Learn how tooview your packet captures by visiting [Packet capture analysis with Wireshark](network-watcher-deep-packet-inspection.md).</span></span>


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
