---
title: az Azure RemoteApp PowerShell-parancsmagok aaaUse |} Microsoft Docs
description: Megtudhatja, hogyan toouse Windows PowerShell-parancsmagok az Azure Remoteappban.
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 7d3d5ded-6f73-4de6-a8ac-c1d622e842a2
ms.service: remoteapp
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a09cae2093e2c3a4a2ed673b5d148a22ceb935f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a><span data-ttu-id="2af51-103">Az Azure RemoteApp Windows PowerShell-parancsmagok használata</span><span class="sxs-lookup"><span data-stu-id="2af51-103">Use Windows PowerShell cmdlets with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2af51-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="2af51-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="2af51-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="2af51-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

 <span data-ttu-id="2af51-106">Hello Azure RemoteApp PowerShell-parancsmagok tooadminister használja, és a gyűjtemények kezelése.</span><span class="sxs-lookup"><span data-stu-id="2af51-106">You can use hello Azure RemoteApp PowerShell cmdlets tooadminister and maintain your collections.</span></span> <span data-ttu-id="2af51-107">A következő lépések információk tooget hello használata.</span><span class="sxs-lookup"><span data-stu-id="2af51-107">Use hello following information tooget started.</span></span>

## <a name="get-hello-cmdlets"></a><span data-ttu-id="2af51-108">Hello parancsmagok beszerzése</span><span class="sxs-lookup"><span data-stu-id="2af51-108">Get hello cmdlets</span></span>
- - -
<span data-ttu-id="2af51-109">Először töltse le a hello Azure Powershell-parancsmagok [Itt](http://go.microsoft.com/?linkid=9811175), hello RemoteApp-parancsmagokat is szerepelnek benne.</span><span class="sxs-lookup"><span data-stu-id="2af51-109">First download hello Azure Powershell cmdlets [here](http://go.microsoft.com/?linkid=9811175), hello RemoteApp cmdlets are included in it.</span></span> 

<span data-ttu-id="2af51-110">Tekintse meg a hello [Azure RemoteApp a parancsmag súgójában talál](/powershell/module/azure?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="2af51-110">Check out hello [Azure RemoteApp cmdlet help](/powershell/module/azure?view=azuresmps-3.7.0).</span></span>

## <a name="configure-azure-cmdlets-toouse-your-subscription"></a><span data-ttu-id="2af51-111">Azure-parancsmagokkal toouse az előfizetés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2af51-111">Configure Azure cmdlets toouse your subscription</span></span>
- - -
<span data-ttu-id="2af51-112">Hajtsa végre a [Ez az útmutató](/powershell/azure/overview) , szemben az Azure-előfizetéshez hello parancsmagokat használhatja.</span><span class="sxs-lookup"><span data-stu-id="2af51-112">Follow [this guide](/powershell/azure/overview) so you can use hello cmdlets against your Azure subscription.</span></span>

<span data-ttu-id="2af51-113">Ezen lépések tooget gyorsan használhatja:</span><span class="sxs-lookup"><span data-stu-id="2af51-113">You can use these steps tooget started quickly:</span></span>

1. <span data-ttu-id="2af51-114">Töltse le és telepítse a hello [Azure PowerShell-parancsmagok](http://go.microsoft.com/?linkid=9811175).</span><span class="sxs-lookup"><span data-stu-id="2af51-114">Download and install hello [Azure PowerShell cmdlets](http://go.microsoft.com/?linkid=9811175).</span></span>
2. <span data-ttu-id="2af51-115">Indítsa el a Microsoft Azure Powershellt.</span><span class="sxs-lookup"><span data-stu-id="2af51-115">Launch Microsoft Azure PowerShell.</span></span>
3. <span data-ttu-id="2af51-116">Futtatás **Add-AzureAccount** tooauthenticate tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2af51-116">Run **Add-AzureAccount** tooauthenticate tooyour Azure subscription.</span></span> <span data-ttu-id="2af51-117">Amikor a rendszer kéri, adja meg a hello azonos felhasználónév és jelszó használható toosign tooAzure portálon.</span><span class="sxs-lookup"><span data-stu-id="2af51-117">When prompted, enter hello same user name and password that you use toosign in tooAzure portal.</span></span>  
4. <span data-ttu-id="2af51-118">Futtatás **Get-AzureSubscription** toolist hello előfizetések a felhasználói fiókjához.</span><span class="sxs-lookup"><span data-stu-id="2af51-118">Run **Get-AzureSubscription** toolist hello subscriptions associated with your user account.</span></span> 
5. <span data-ttu-id="2af51-119">Futtatás **válasszon-AzureSubscription - SubscriptionName &lt;előfizetés neve&gt;**  vagy **válasszon-AzureSubscription - előfizetés-azonosító &lt;előfizetés-azonosító&gt;**  toospecify hello előfizetés toouse.</span><span class="sxs-lookup"><span data-stu-id="2af51-119">Run **Select-AzureSubscription -SubscriptionName &lt;subscription name&gt;** or **Select-AzureSubscription -SubscriptionId &lt;subscription ID&gt;** toospecify hello subscription toouse.</span></span>

<span data-ttu-id="2af51-120">Gratulálunk, az Azure PowerShell-konzolon konfigurált és készen állnak a toouse.</span><span class="sxs-lookup"><span data-stu-id="2af51-120">Congratulations, your Azure PowerShell console is configured and ready toouse.</span></span> <span data-ttu-id="2af51-121">Vegye figyelembe, hogy szüksége lesz toorepeate lépéseket 2 – 5 hello hello Azure PowerShell konzol minden egyes indításakor.</span><span class="sxs-lookup"><span data-stu-id="2af51-121">Be aware that you'll need toorepeate steps 2 through 5 each time you start hello hello Azure PowerShell console.</span></span>  


## <a name="list-all-collections"></a><span data-ttu-id="2af51-122">Minden gyűjtemény felsorolása</span><span class="sxs-lookup"><span data-stu-id="2af51-122">List all collections</span></span>
- - -
     <span data-ttu-id="2af51-123">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="2af51-123">Get-AzureRemoteAppCollection</span></span>

## <a name="delete-a-collection"></a><span data-ttu-id="2af51-124">A gyűjtemény törlése</span><span class="sxs-lookup"><span data-stu-id="2af51-124">Delete a collection</span></span>
- - -
    <span data-ttu-id="2af51-125">Remove-AzureRemoteAppCollection<enter collection name></span><span class="sxs-lookup"><span data-stu-id="2af51-125">Remove-AzureRemoteAppCollection <enter collection name></span></span>

<span data-ttu-id="2af51-126">Példa: `Remove-AzureRemoteAppCollection ContosoProduction`.</span><span class="sxs-lookup"><span data-stu-id="2af51-126">Example:  `Remove-AzureRemoteAppCollection ContosoProduction`.</span></span>

## <a name="create-a-cloud-collection"></a><span data-ttu-id="2af51-127">Felhőalapú gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="2af51-127">Create a cloud collection</span></span>
- - -
<span data-ttu-id="2af51-128">Használata egyszerű, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2af51-128">It's simple, run hello following command:</span></span>

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

<span data-ttu-id="2af51-129">hello fent parancs automatikusan közzéteszi a Microsoft Office 365-alkalmazások (Excel, a OneNote, Outlook, a PowerPoint, Visio és Word).</span><span class="sxs-lookup"><span data-stu-id="2af51-129">hello above command automatically publishes Microsoft Office 365 applications (Excel, OneNote, Outlook, PowerPoint, Visio and Word).</span></span>

<span data-ttu-id="2af51-130">Webhelycsoport létrehozása eltarthat 30 perc vagy hosszabb toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2af51-130">Collection creation can take 30 minutes or longer toocomplete.</span></span> <span data-ttu-id="2af51-131">Ezért ez a parancs visszaadja a követési azonosító, amely a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="2af51-131">Therefore, this command returns a tracking ID that you can use as follows:</span></span>

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

<span data-ttu-id="2af51-132">Hello gyűjtemény után a felhasználó toohello gyűjtemény hello a következő parancsot a is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="2af51-132">After hello collection is done, you can add users toohello collection with hello following command:</span></span>

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

<span data-ttu-id="2af51-133">És elkészült!</span><span class="sxs-lookup"><span data-stu-id="2af51-133">And you're done!</span></span> <span data-ttu-id="2af51-134">A felhasználónak kell lennie képes tooconnect toohello alkalmazást hello Azure RemoteApp-ügyfélen található [Itt](https://www.remoteapp.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="2af51-134">That user should be able tooconnect toohello application using hello Azure RemoteApp client found [here](https://www.remoteapp.windowsazure.com/).</span></span>

## <a name="available-cmdlets"></a><span data-ttu-id="2af51-135">Elérhető parancsmagok</span><span class="sxs-lookup"><span data-stu-id="2af51-135">Available cmdlets</span></span>
<span data-ttu-id="2af51-136">Az egyéb parancsok, amelyek tudunk számos, hello dokumentációja őket hamarosan hamarosan:</span><span class="sxs-lookup"><span data-stu-id="2af51-136">There are lots of other commands that we have, hello documentation for them will be coming shortly:</span></span>

<span data-ttu-id="2af51-137">Alapszintű RemoteApp-gyűjtemény parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="2af51-137">Basic RemoteApp Collection cmdlets:</span></span> 

* <span data-ttu-id="2af51-138">New-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="2af51-138">New-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="2af51-139">Get-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="2af51-139">Get-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="2af51-140">Set-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="2af51-140">Set-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="2af51-141">Update-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="2af51-141">Update-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="2af51-142">Remove-AzureRemoteAppCollection</span><span class="sxs-lookup"><span data-stu-id="2af51-142">Remove-AzureRemoteAppCollection</span></span>
* <span data-ttu-id="2af51-143">Add-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="2af51-143">Add-AzureRemoteAppUser</span></span>
* <span data-ttu-id="2af51-144">Get-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="2af51-144">Get-AzureRemoteAppUser</span></span>
* <span data-ttu-id="2af51-145">Remove-AzureRemoteAppUser</span><span class="sxs-lookup"><span data-stu-id="2af51-145">Remove-AzureRemoteAppUser</span></span>
* <span data-ttu-id="2af51-146">Get-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="2af51-146">Get-AzureRemoteAppSession</span></span>
* <span data-ttu-id="2af51-147">Disconnect-AzureRemoteAppSession</span><span class="sxs-lookup"><span data-stu-id="2af51-147">Disconnect-AzureRemoteAppSession</span></span>
* <span data-ttu-id="2af51-148">Invoke-AzureRemoteAppSessionLogoff</span><span class="sxs-lookup"><span data-stu-id="2af51-148">Invoke-AzureRemoteAppSessionLogoff</span></span>
* <span data-ttu-id="2af51-149">Send-AzureRemoteAppSessionMessage</span><span class="sxs-lookup"><span data-stu-id="2af51-149">Send-AzureRemoteAppSessionMessage</span></span>
* <span data-ttu-id="2af51-150">Get-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="2af51-150">Get-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="2af51-151">Get-AzureRemoteAppStartMenuProgram</span><span class="sxs-lookup"><span data-stu-id="2af51-151">Get-AzureRemoteAppStartMenuProgram</span></span>
* <span data-ttu-id="2af51-152">Publish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="2af51-152">Publish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="2af51-153">Unpublish-AzureRemoteAppProgram</span><span class="sxs-lookup"><span data-stu-id="2af51-153">Unpublish-AzureRemoteAppProgram</span></span>
* <span data-ttu-id="2af51-154">Get-AzureRemoteAppCollectionUsageDetails</span><span class="sxs-lookup"><span data-stu-id="2af51-154">Get-AzureRemoteAppCollectionUsageDetails</span></span>
* <span data-ttu-id="2af51-155">Get-AzureRemoteAppCollectionUsageSummary</span><span class="sxs-lookup"><span data-stu-id="2af51-155">Get-AzureRemoteAppCollectionUsageSummary</span></span>
* <span data-ttu-id="2af51-156">Get-AzureRemoteAppPlan</span><span class="sxs-lookup"><span data-stu-id="2af51-156">Get-AzureRemoteAppPlan</span></span>

<span data-ttu-id="2af51-157">RemoteApp virtuális hálózatot parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="2af51-157">RemoteApp virtual network cmdlets:</span></span>

* <span data-ttu-id="2af51-158">New-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="2af51-158">New-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="2af51-159">Get-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="2af51-159">Get-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="2af51-160">Set-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="2af51-160">Set-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="2af51-161">Remove-AzureRemoteAppVNet</span><span class="sxs-lookup"><span data-stu-id="2af51-161">Remove-AzureRemoteAppVNet</span></span>
* <span data-ttu-id="2af51-162">Get-AzureRemoteAppVpnDevice</span><span class="sxs-lookup"><span data-stu-id="2af51-162">Get-AzureRemoteAppVpnDevice</span></span>
* <span data-ttu-id="2af51-163">Get--AzureRemoteAppVpnDeviceConfigScript</span><span class="sxs-lookup"><span data-stu-id="2af51-163">Get-- AzureRemoteAppVpnDeviceConfigScript</span></span>
* <span data-ttu-id="2af51-164">Reset-AzureRemoteAppVpnSharedKey</span><span class="sxs-lookup"><span data-stu-id="2af51-164">Reset-AzureRemoteAppVpnSharedKey</span></span>

<span data-ttu-id="2af51-165">RemoteApp sablon-rendszerkép parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="2af51-165">RemoteApp template image cmdlets:</span></span>

* <span data-ttu-id="2af51-166">New-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="2af51-166">New-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="2af51-167">Get-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="2af51-167">Get-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="2af51-168">Rename-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="2af51-168">Rename-AzureRemoteAppTemplateImage</span></span>
* <span data-ttu-id="2af51-169">Remove-AzureRemoteAppTemplateImage</span><span class="sxs-lookup"><span data-stu-id="2af51-169">Remove-AzureRemoteAppTemplateImage</span></span>

<span data-ttu-id="2af51-170">Más RemoteApp-parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="2af51-170">Other RemoteApp cmdlets:</span></span>

* <span data-ttu-id="2af51-171">Get-AzureRemoteAppLocation</span><span class="sxs-lookup"><span data-stu-id="2af51-171">Get-AzureRemoteAppLocation</span></span>
* <span data-ttu-id="2af51-172">Get-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="2af51-172">Get-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="2af51-173">Set-AzureRemoteAppWorkspace</span><span class="sxs-lookup"><span data-stu-id="2af51-173">Set-AzureRemoteAppWorkspace</span></span>
* <span data-ttu-id="2af51-174">Get-AzureRemoteAppOperationResult</span><span class="sxs-lookup"><span data-stu-id="2af51-174">Get-AzureRemoteAppOperationResult</span></span>

