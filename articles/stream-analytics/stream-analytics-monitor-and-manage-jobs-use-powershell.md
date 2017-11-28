---
title: "aaaMonitor és a PowerShell-lel Stream Analytics-feladatok kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure PowerShell és a parancsmagok toomonitor és a Stream Analytics-feladatok kezelése."
keywords: az Azure powershell, az azure powershell-parancsmagok, a powershell-paranccsal, powershell-parancsprogramok
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 514f454e-d18c-4081-8304-ab48577e15e8
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 44abc82f1c44a5ebc1701badd6547b84dac239b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a><span data-ttu-id="6e5f4-104">Figyelheti és kezelheti a Stream Analytics-feladatok Azure PowerShell-parancsmagokkal</span><span class="sxs-lookup"><span data-stu-id="6e5f4-104">Monitor and manage Stream Analytics jobs with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="6e5f4-105">Megtudhatja, hogyan toomonitor és felügyelhetők a Stream Analytics erőforrások az Azure PowerShell-parancsmagok és a powershell-parancsprogramok, amelyek alapvető Stream Analytics-feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-105">Learn how toomonitor and manage Stream Analytics resources with Azure PowerShell cmdlets and powershell scripting that execute basic Stream Analytics tasks.</span></span>

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="6e5f4-106">Azure PowerShell-parancsmagok futtatását a Stream Analytics előfeltételei</span><span class="sxs-lookup"><span data-stu-id="6e5f4-106">Prerequisites for running Azure PowerShell cmdlets for Stream Analytics</span></span>
* <span data-ttu-id="6e5f4-107">Azure-erőforráscsoport létrehozása az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-107">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="6e5f4-108">hello az alábbiakban látható egy minta Azure PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-108">hello following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="6e5f4-109">Azure PowerShell információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="6e5f4-109">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

<span data-ttu-id="6e5f4-110">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-110">Azure PowerShell 0.9.8:</span></span>  

         # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

<span data-ttu-id="6e5f4-111">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-111">Azure PowerShell 1.0:</span></span>  

         # Log in tooyour Azure account
        Login-AzureRmAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> <span data-ttu-id="6e5f4-112">Hozta létre a Stream Analytics-feladatok nincs figyelés alapértelmezés szerint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-112">Stream Analytics jobs created programmatically do not have monitoring enabled by default.</span></span>  <span data-ttu-id="6e5f4-113">Manuálisan engedélyezheti hello Azure Portal toohello feladat figyelő lapon lépjen a megfigyelést és vagy az Ön hello engedélyezése gombra kattintva teheti meg programozott módon helyen hello lépéseket követve [Azure Stream Analytics - figyelő adatfolyam Elemzés feladatok programozottan](stream-analytics-monitor-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="6e5f4-113">You can manually enable monitoring in hello Azure Portal by navigating toohello job’s Monitor page and clicking hello Enable button or you can do this programmatically by following hello steps located at [Azure Stream Analytics - Monitor Stream Analytics Jobs Programatically](stream-analytics-monitor-jobs.md).</span></span>
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="6e5f4-114">A Stream Analytics az Azure PowerShell-parancsmagjai</span><span class="sxs-lookup"><span data-stu-id="6e5f4-114">Azure PowerShell cmdlets for Stream Analytics</span></span>
<span data-ttu-id="6e5f4-115">a következő Azure PowerShell-parancsmagok hello használt toomonitor lehetnek és Azure Stream Analytics-feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-115">hello following Azure PowerShell cmdlets can be used toomonitor and manage Azure Stream Analytics jobs.</span></span> <span data-ttu-id="6e5f4-116">Vegye figyelembe, hogy az Azure PowerShell különböző verziói.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-116">Note that Azure PowerShell has different versions.</span></span> 
<span data-ttu-id="6e5f4-117">**A felsorolt hello első parancs az Azure PowerShell 0.9.8-as hello példákban hello második parancs az Azure PowerShell 1.0 van.**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-117">**In hello examples listed hello first command is for Azure PowerShell 0.9.8, hello second command is for Azure PowerShell 1.0.**</span></span> <span data-ttu-id="6e5f4-118">hello Azure PowerShell 1.0 parancsok mindig lesz "AzureRM" hello parancsban.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-118">hello Azure PowerShell 1.0 commands will always have "AzureRM" in hello command.</span></span>

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a><span data-ttu-id="6e5f4-119">Get-AzureStreamAnalyticsJob |} Get-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="6e5f4-119">Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="6e5f4-120">Hello Azure-előfizetés vagy a megadott erőforráscsoport összes Stream Analytics-feladatok listája, vagy egy adott feladat erőforráscsoporton belül feladat információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-120">Lists all Stream Analytics jobs defined in hello Azure subscription or specified resource group, or gets job information about a specific job within a resource group.</span></span>

<span data-ttu-id="6e5f4-121">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-121">**Example 1**</span></span>

<span data-ttu-id="6e5f4-122">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-122">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob

<span data-ttu-id="6e5f4-123">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-123">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob

<span data-ttu-id="6e5f4-124">A PowerShell-parancs minden hello Stream Analytics-feladatok információ hello Azure-előfizetés adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-124">This PowerShell command returns information about all hello Stream Analytics jobs in hello Azure subscription.</span></span>

<span data-ttu-id="6e5f4-125">**2. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-125">**Example 2**</span></span>

<span data-ttu-id="6e5f4-126">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-126">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="6e5f4-127">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-127">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="6e5f4-128">A PowerShell-parancs minden hello Stream Analytics-feladatok információ hello erőforráscsoport StreamAnalytics-alapértelmezett-közép-amerikai adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-128">This PowerShell command returns information about all hello Stream Analytics jobs in hello resource group StreamAnalytics-Default-Central-US.</span></span>

<span data-ttu-id="6e5f4-129">**3. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-129">**Example 3**</span></span>

<span data-ttu-id="6e5f4-130">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-130">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="6e5f4-131">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-131">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="6e5f4-132">A PowerShell-parancs hello Stream Analytics-feladat StreamingJob információ hello erőforráscsoport StreamAnalytics-alapértelmezett-közép-amerikai adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-132">This PowerShell command returns information about hello Stream Analytics job StreamingJob in hello resource group StreamAnalytics-Default-Central-US.</span></span>

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a><span data-ttu-id="6e5f4-133">Get-AzureStreamAnalyticsInput |} Get-AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-133">Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="6e5f4-134">Felsorolja az összes megadott Stream Analytics-feladatban definiált hello bemenet, vagy egy adott bevitel információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-134">Lists all of hello inputs that are defined in a specified Stream Analytics job, or gets information about a specific input.</span></span>

<span data-ttu-id="6e5f4-135">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-135">**Example 1**</span></span>

<span data-ttu-id="6e5f4-136">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-136">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="6e5f4-137">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-137">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="6e5f4-138">A PowerShell-parancs hello feladat StreamingJob definiált összes hello bemenet információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-138">This PowerShell command returns information about all hello inputs defined in hello job StreamingJob.</span></span>

<span data-ttu-id="6e5f4-139">**2. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-139">**Example 2**</span></span>

<span data-ttu-id="6e5f4-140">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-140">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="6e5f4-141">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-141">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="6e5f4-142">A PowerShell-parancs hello feladat StreamingJob definiált EntryStream nevű hello bemeneti információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-142">This PowerShell command returns information about hello input named EntryStream defined in hello job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a><span data-ttu-id="6e5f4-143">Get-AzureStreamAnalyticsOutput |} Get-AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-143">Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="6e5f4-144">Felsorolja az összes megadott Stream Analytics-feladatban definiált hello kimenetek, vagy egy adott kimeneti információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-144">Lists all of hello outputs that are defined in a specified Stream Analytics job, or gets information about a specific output.</span></span>

<span data-ttu-id="6e5f4-145">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-145">**Example 1**</span></span>

<span data-ttu-id="6e5f4-146">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-146">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="6e5f4-147">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-147">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="6e5f4-148">A PowerShell-parancs hello feladat StreamingJob definiált hello kimenetek információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-148">This PowerShell command returns information about hello outputs defined in hello job StreamingJob.</span></span>

<span data-ttu-id="6e5f4-149">**2. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-149">**Example 2**</span></span>

<span data-ttu-id="6e5f4-150">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-150">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="6e5f4-151">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-151">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="6e5f4-152">A PowerShell-parancs hello feladat StreamingJob meghatározott kimeneti nevű hello kimeneti információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-152">This PowerShell command returns information about hello output named Output defined in hello job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a><span data-ttu-id="6e5f4-153">Get-AzureStreamAnalyticsQuota |} Get-AzureRMStreamAnalyticsQuota</span><span class="sxs-lookup"><span data-stu-id="6e5f4-153">Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota</span></span>
<span data-ttu-id="6e5f4-154">Az adatfolyam-egységek meghatározott hello kvóta információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-154">Gets information about hello quota of streaming units in a specified region.</span></span>

<span data-ttu-id="6e5f4-155">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-155">**Example 1**</span></span>

<span data-ttu-id="6e5f4-156">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-156">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="6e5f4-157">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-157">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="6e5f4-158">A PowerShell-parancs hello kvóta, valamint a folyamatos átviteli egységeket információ hello központi US régió adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-158">This PowerShell command returns information about hello quota and usage of streaming units in hello Central US region.</span></span>

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a><span data-ttu-id="6e5f4-159">Get-AzureStreamAnalyticsTransformation |} GetAzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="6e5f4-159">Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="6e5f4-160">Egy Stream Analytics-feladatban definiált átalakítási információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-160">Gets information about a specific transformation defined in a Stream Analytics job.</span></span>

<span data-ttu-id="6e5f4-161">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-161">**Example 1**</span></span>

<span data-ttu-id="6e5f4-162">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-162">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="6e5f4-163">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-163">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="6e5f4-164">A PowerShell-parancs StreamingJob meghívta hello feladat StreamingJob hello átalakítása információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-164">This PowerShell command returns information about hello transformation called StreamingJob in hello job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a><span data-ttu-id="6e5f4-165">Új AzureStreamAnalyticsInput |} Új AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-165">New-AzureStreamAnalyticsInput | New-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="6e5f4-166">Létrehoz egy új bemeneti belül a Stream Analytics-feladat, vagy egy meglévő megadott bemeneti adatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-166">Creates a new input within a Stream Analytics job, or updates an existing specified input.</span></span>

<span data-ttu-id="6e5f4-167">hello hello bemeneti név adható meg hello .JSON kiterjesztésű fájlt vagy hello parancssorban.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-167">hello name of hello input can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="6e5f4-168">Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-168">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="6e5f4-169">Ha nem ad meg és adjon meg egy már létező bemeneti hello – Force paramétert hello parancsmag ekkor megkérdezi, hogy tooreplace hello meglévő bemeneti.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-169">If you specify an input that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing input.</span></span>

<span data-ttu-id="6e5f4-170">Ha megadja hello – Force paramétert, és adja meg egy létező adjon meg nevet, a hello bemeneti váltja megerősítés nélküli megadására.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-170">If you specify hello –Force parameter and specify an existing input name, hello input will be replaced without confirmation.</span></span>

<span data-ttu-id="6e5f4-171">A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [bemeneti létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] hello szakasza [Stream Analytics felügyeleti REST API-t Referenciatárában][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="6e5f4-171">For detailed information on hello JSON file structure and contents, refer toohello [Create Input (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-input] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="6e5f4-172">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-172">**Example 1**</span></span>

<span data-ttu-id="6e5f4-173">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-173">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="6e5f4-174">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-174">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="6e5f4-175">A PowerShell-parancs létrehoz egy új bemeneti Input.json hello fájlból.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-175">This PowerShell command creates a new input from hello file Input.json.</span></span> <span data-ttu-id="6e5f4-176">Ha a meglévő bemeneti hello nevet hello bemeneti szolgáltatásdefiníciós fájlban megadott névvel már definiálva van, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-176">If an existing input with hello name specified in hello input definition file is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="6e5f4-177">**2. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-177">**Example 2**</span></span>

<span data-ttu-id="6e5f4-178">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-178">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="6e5f4-179">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-179">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="6e5f4-180">A PowerShell-parancs egy új bemeneti EntryStream nevű hello feladatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-180">This PowerShell command creates a new input in hello job called EntryStream.</span></span> <span data-ttu-id="6e5f4-181">Ha egy meglévő bemeneti ezen a néven már definiálva van, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-181">If an existing input with this name is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="6e5f4-182">**3. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-182">**Example 3**</span></span>

<span data-ttu-id="6e5f4-183">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-183">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="6e5f4-184">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-184">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="6e5f4-185">A PowerShell-parancs a felváltja hello meglévő bemeneti forrás EntryStream meghívásra hello definíciós fájlból hello hello meghatározását.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-185">This PowerShell command replaces hello definition of hello existing input source called EntryStream with hello definition from hello file.</span></span>

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a><span data-ttu-id="6e5f4-186">Új AzureStreamAnalyticsJob |} Új AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="6e5f4-186">New-AzureStreamAnalyticsJob | New-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="6e5f4-187">Létrehoz egy új Stream Analytics-feladat a Microsoft Azure-ban, vagy frissíti a létező megadott feladatnak hello meghatározása.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-187">Creates a new Stream Analytics job in Microsoft Azure, or updates hello definition of an existing specified job.</span></span>

<span data-ttu-id="6e5f4-188">hello feladat neve hello hello .JSON kiterjesztésű fájlt vagy a parancssori hello adható meg.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-188">hello name of hello job can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="6e5f4-189">Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-189">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="6e5f4-190">Ha nem ad meg és adja meg a feladat nevét, amely már létezik hello – Force paramétert hello parancsmag ekkor megkérdezi, hogy tooreplace hello meglévő feladat.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-190">If you specify a job name that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing job.</span></span>

<span data-ttu-id="6e5f4-191">Ha megadja hello – Force paramétert, és adjon meg egy létező feladat, hello feladatdefiníció váltja megerősítés nélküli megadására.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-191">If you specify hello –Force parameter and specify an existing job name, hello job definition will be replaced without confirmation.</span></span>

<span data-ttu-id="6e5f4-192">A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [Stream Analytics-feladat létrehozása] [ msdn-rest-api-create-stream-analytics-job] hello szakasza [Stream Analytics felügyeleti REST API-referencia Szalagtár][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="6e5f4-192">For detailed information on hello JSON file structure and contents, refer toohello [Create Stream Analytics Job][msdn-rest-api-create-stream-analytics-job] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="6e5f4-193">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-193">**Example 1**</span></span>

<span data-ttu-id="6e5f4-194">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-194">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="6e5f4-195">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-195">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="6e5f4-196">A PowerShell-parancs létrehoz egy új feladatot a JobDefinition.json hello definícióból.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-196">This PowerShell command creates a new job from hello definition in JobDefinition.json.</span></span> <span data-ttu-id="6e5f4-197">Ha egy meglévő feladat hello nevet hello feladat szolgáltatásdefiníciós fájlban megadott névvel már van definiálva, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-197">If an existing job with hello name specified in hello job definition file is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="6e5f4-198">**2. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-198">**Example 2**</span></span>

<span data-ttu-id="6e5f4-199">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-199">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="6e5f4-200">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-200">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="6e5f4-201">A PowerShell-parancs hello feladatdefiníció StreamingJob a felváltja.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-201">This PowerShell command replaces hello job definition for StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a><span data-ttu-id="6e5f4-202">Új AzureStreamAnalyticsOutput |} Új AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-202">New-AzureStreamAnalyticsOutput | New-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="6e5f4-203">Létrehoz egy új kimeneti belül a Stream Analytics-feladat, vagy frissíti a meglévő kimenettel.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-203">Creates a new output within a Stream Analytics job, or updates an existing output.</span></span>  

<span data-ttu-id="6e5f4-204">hello .JSON kiterjesztésű fájlt vagy a parancssori hello hello kimenet hello neve adható meg.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-204">hello name of hello output can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="6e5f4-205">Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-205">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="6e5f4-206">Ha egy már létező kimeneti adja meg, és ne adjon meg hello – Force paramétert hello parancsmag ekkor megkérdezi, hogy tooreplace hello meglévő kimeneti.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-206">If you specify an output that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing output.</span></span>

<span data-ttu-id="6e5f4-207">Ha megadja hello – Force paramétert, és adja meg a név egy meglévő kimeneti hello kimeneti váltja megerősítés nélküli megadására.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-207">If you specify hello –Force parameter and specify an existing output name, hello output will be replaced without confirmation.</span></span>

<span data-ttu-id="6e5f4-208">A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [kimeneti létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] hello szakasza [Stream Analytics felügyeleti REST API-n Referenciatárában][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="6e5f4-208">For detailed information on hello JSON file structure and contents, refer toohello [Create Output (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-output] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="6e5f4-209">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-209">**Example 1**</span></span>

<span data-ttu-id="6e5f4-210">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-210">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="6e5f4-211">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-211">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="6e5f4-212">A PowerShell-parancs egy "kimeneti" nevű hello feladat StreamingJob új kimenetet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-212">This PowerShell command creates a new output called "output" in hello job StreamingJob.</span></span> <span data-ttu-id="6e5f4-213">Ha egy meglévő kimeneti ezen a néven már definiálva van, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-213">If an existing output with this name is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="6e5f4-214">**2. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-214">**Example 2**</span></span>

<span data-ttu-id="6e5f4-215">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-215">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="6e5f4-216">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-216">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="6e5f4-217">A PowerShell-parancs a "kimeneti" hello feladat StreamingJob hello definíciója váltja fel.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-217">This PowerShell command replaces hello definition for "output" in hello job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a><span data-ttu-id="6e5f4-218">Új AzureStreamAnalyticsTransformation |} Új AzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="6e5f4-218">New-AzureStreamAnalyticsTransformation | New-AzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="6e5f4-219">Létrehoz egy új átalakítása belül a Stream Analytics-feladat, vagy frissíti a meglévő átalakítása hello.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-219">Creates a new transformation within a Stream Analytics job, or updates hello existing transformation.</span></span>

<span data-ttu-id="6e5f4-220">hello .JSON kiterjesztésű fájlt vagy a parancssori hello hello átalakítása hello neve adható meg.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-220">hello name of hello transformation can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="6e5f4-221">Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-221">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="6e5f4-222">Ha nem adja meg és adja meg, hogy már létezik az átalakítás hello – Force paramétert hello parancsmag felhasználói jóváhagyást kér-e tooreplace hello meglévő átalakítása.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-222">If you specify a transformation that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing transformation.</span></span>

<span data-ttu-id="6e5f4-223">Ha megadja hello – Force paramétert, és adjon meg egy meglévő átalakítása, hello átalakítása váltja megerősítés nélküli megadására.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-223">If you specify hello –Force parameter and specify an existing transformation name, hello transformation will be replaced without confirmation.</span></span>

<span data-ttu-id="6e5f4-224">A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [átalakítása létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] hello szakasza [Stream Analytics felügyeleti REST API Referenciatárában][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="6e5f4-224">For detailed information on hello JSON file structure and contents, refer toohello [Create Transformation (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-transformation] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="6e5f4-225">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-225">**Example 1**</span></span>

<span data-ttu-id="6e5f4-226">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-226">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="6e5f4-227">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-227">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="6e5f4-228">A PowerShell-parancs hello feladat StreamingJob StreamingJobTransform nevű új átalakítás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-228">This PowerShell command creates a new transformation called StreamingJobTransform in hello job StreamingJob.</span></span> <span data-ttu-id="6e5f4-229">Ha egy meglévő átalakítást már definiálva van ilyen nevű, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-229">If an existing transformation is already defined with this name, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="6e5f4-230">**2. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-230">**Example 2**</span></span>

<span data-ttu-id="6e5f4-231">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-231">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

<span data-ttu-id="6e5f4-232">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-232">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 <span data-ttu-id="6e5f4-233">A PowerShell-parancs a felváltja hello feladat StreamingJob hello StreamingJobTransform meghatározása.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-233">This PowerShell command replaces hello definition of StreamingJobTransform in hello job StreamingJob.</span></span>

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a><span data-ttu-id="6e5f4-234">Remove-AzureStreamAnalyticsInput |} Remove-AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-234">Remove-AzureStreamAnalyticsInput | Remove-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="6e5f4-235">Aszinkron módon törli egy adott bevitel a Stream Analytics-feladat, a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-235">Asynchronously deletes a specific input from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="6e5f4-236">Ha megad hello – Force paramétert, bemeneti hello törli megerősítés nélküli megadására.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-236">If you specify hello –Force parameter, hello input will be deleted without confirmation.</span></span>

<span data-ttu-id="6e5f4-237">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-237">**Example 1**</span></span>

<span data-ttu-id="6e5f4-238">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-238">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="6e5f4-239">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-239">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="6e5f4-240">A PowerShell-parancs eltávolítja a bemeneti EventStream hello feladat StreamingJob hello.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-240">This PowerShell command removes hello input EventStream in hello job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a><span data-ttu-id="6e5f4-241">Remove-AzureStreamAnalyticsJob |} Remove-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="6e5f4-241">Remove-AzureStreamAnalyticsJob | Remove-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="6e5f4-242">Aszinkron módon törli egy adott, a Microsoft Azure Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-242">Asynchronously deletes a specific Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="6e5f4-243">Ha megad hello – Force paramétert hello feladat lesz törölve megerősítés nélküli megadására.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-243">If you specify hello –Force parameter, hello job will be deleted without confirmation.</span></span>

<span data-ttu-id="6e5f4-244">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-244">**Example 1**</span></span>

<span data-ttu-id="6e5f4-245">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-245">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="6e5f4-246">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-246">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="6e5f4-247">A PowerShell-parancs hello feladat StreamingJob eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-247">This PowerShell command removes hello job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a><span data-ttu-id="6e5f4-248">Remove-AzureStreamAnalyticsOutput |} Remove-AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-248">Remove-AzureStreamAnalyticsOutput | Remove-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="6e5f4-249">A megadott kimeneti aszinkron módon töröl egy Microsoft Azure Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-249">Asynchronously deletes a specific output from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="6e5f4-250">Ha megad hello – Force paramétert hello kimenet lesz törölve megerősítés nélküli megadására.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-250">If you specify hello –Force parameter, hello output will be deleted without confirmation.</span></span>

<span data-ttu-id="6e5f4-251">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-251">**Example 1**</span></span>

<span data-ttu-id="6e5f4-252">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-252">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="6e5f4-253">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-253">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="6e5f4-254">A PowerShell-parancs eltávolítja hello a kimeneti hello feladat StreamingJob kimenetet.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-254">This PowerShell command removes hello output Output in hello job StreamingJob.</span></span>  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a><span data-ttu-id="6e5f4-255">Start-AzureStreamAnalyticsJob |} Start-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="6e5f4-255">Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="6e5f4-256">Aszinkron módon telepíti, és a Microsoft Azure Stream Analytics-feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-256">Asynchronously deploys and starts a Stream Analytics job in Microsoft Azure.</span></span>

<span data-ttu-id="6e5f4-257">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-257">**Example 1**</span></span>

<span data-ttu-id="6e5f4-258">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-258">Azure PowerShell 0.9.8:</span></span>  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="6e5f4-259">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-259">Azure PowerShell 1.0:</span></span>  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="6e5f4-260">A PowerShell-parancs indításakor hello StreamingJob egyéni kimeneti kezdési időpont beállítása tooDecember 12, 2012, 12:12:12 feladat UTC.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-260">This PowerShell command starts hello job StreamingJob with a custom output start time set tooDecember 12, 2012, 12:12:12 UTC.</span></span>

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a><span data-ttu-id="6e5f4-261">STOP-AzureStreamAnalyticsJob |} STOP-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="6e5f4-261">Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="6e5f4-262">Aszinkron módon történik a Stream Analytics-feladat, a Microsoft Azure-beli leáll, és felszabadítása az erőforrásokhoz, melyeket volt használatban.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-262">Asynchronously stops a Stream Analytics job from running in Microsoft Azure and de-allocates resources that were that were being used.</span></span> <span data-ttu-id="6e5f4-263">hello feladatdefiníció és metaadatok elérhető marad hello Azure-portál és a felügyeleti API-k, az előfizetésen belül, hogy hello feladat szerkeszthető és újraindul.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-263">hello job definition and metadata will remain available within your subscription through both hello Azure portal and management APIs, such that hello job can be edited and restarted.</span></span> <span data-ttu-id="6e5f4-264">Nem kell fizetnie a feladat leállt hello állapotban.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-264">You will not be charged for a job in hello stopped state.</span></span>

<span data-ttu-id="6e5f4-265">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-265">**Example 1**</span></span>

<span data-ttu-id="6e5f4-266">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-266">Azure PowerShell 0.9.8:</span></span>  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="6e5f4-267">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-267">Azure PowerShell 1.0:</span></span>  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="6e5f4-268">A PowerShell-parancs hello feladat StreamingJob leállítása.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-268">This PowerShell command stops hello job StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a><span data-ttu-id="6e5f4-269">Teszt-AzureStreamAnalyticsInput |} Teszt-AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-269">Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="6e5f4-270">Tesztek hello képességét Stream Analytics tooconnect tooa megadott bemenetet.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-270">Tests hello ability of Stream Analytics tooconnect tooa specified input.</span></span>

<span data-ttu-id="6e5f4-271">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-271">**Example 1**</span></span>

<span data-ttu-id="6e5f4-272">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-272">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="6e5f4-273">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-273">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="6e5f4-274">A PowerShell parancsot tesztek hello kapcsolati állapotához hello bemeneti EntryStream a StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-274">This PowerShell command tests hello connection status of hello input EntryStream in StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a><span data-ttu-id="6e5f4-275">Teszt-AzureStreamAnalyticsOutput |} Teszt-AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="6e5f4-275">Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="6e5f4-276">A megadott kimeneti tesztek hello képességét Stream Analytics tooconnect tooa.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-276">Tests hello ability of Stream Analytics tooconnect tooa specified output.</span></span>

<span data-ttu-id="6e5f4-277">**1. példa**</span><span class="sxs-lookup"><span data-stu-id="6e5f4-277">**Example 1**</span></span>

<span data-ttu-id="6e5f4-278">Az Azure PowerShell 0.9.8-as:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-278">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="6e5f4-279">Az Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="6e5f4-279">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="6e5f4-280">A PowerShell parancsot tesztek hello kapcsolati állapotához hello kimeneti StreamingJob-kimenetet.</span><span class="sxs-lookup"><span data-stu-id="6e5f4-280">This PowerShell command tests hello connection status of hello output Output in StreamingJob.</span></span>  

## <a name="get-support"></a><span data-ttu-id="6e5f4-281">Támogatás kérése</span><span class="sxs-lookup"><span data-stu-id="6e5f4-281">Get support</span></span>
<span data-ttu-id="6e5f4-282">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="6e5f4-282">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6e5f4-283">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e5f4-283">Next steps</span></span>
* [<span data-ttu-id="6e5f4-284">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="6e5f4-284">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="6e5f4-285">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="6e5f4-285">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="6e5f4-286">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="6e5f4-286">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="6e5f4-287">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="6e5f4-287">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="6e5f4-288">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="6e5f4-288">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

