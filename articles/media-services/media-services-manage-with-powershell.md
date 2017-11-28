---
title: "aaaManage Azure Media Services fiókok a PowerShell használatával"
description: "Ismerje meg, hogyan toomanage Azure Media Services fiókok a PowerShell-parancsmagokkal."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 17a10c25-d94f-421c-b6bc-ae0958e2ac96
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: juliako
ms.openlocfilehash: e8f97bb2393343e45fabf9c437b4fc09f2525dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a><span data-ttu-id="c9e66-103">A PowerShell segítségével az Azure Media Services fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="c9e66-103">Manage Azure Media Services Accounts with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c9e66-104">Portál</span><span class="sxs-lookup"><span data-stu-id="c9e66-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="c9e66-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9e66-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="c9e66-106">REST</span><span class="sxs-lookup"><span data-stu-id="c9e66-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="c9e66-107">toobe képes toocreate Azure Media Services-fiók, rendelkeznie kell egy Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="c9e66-107">toobe able toocreate an Azure Media Services account, you must have an Azure account.</span></span> <span data-ttu-id="c9e66-108">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="c9e66-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c9e66-109">További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="c9e66-109">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="c9e66-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c9e66-110">Overview</span></span>
<span data-ttu-id="c9e66-111">Ez a cikk az Azure Media Services (AMS) felsorolja hello Azure PowerShell-parancsmagok hello Azure Resource Manager keretében.</span><span class="sxs-lookup"><span data-stu-id="c9e66-111">This article lists hello Azure PowerShell cmdlets for Azure Media Services (AMS) in hello Azure Resource Manager framework.</span></span> <span data-ttu-id="c9e66-112">hello parancsmagok szerepel hello **Microsoft.Azure.Commands.Media** névtér.</span><span class="sxs-lookup"><span data-stu-id="c9e66-112">hello cmdlets exist in hello **Microsoft.Azure.Commands.Media** namespace.</span></span>

## <a name="versions"></a><span data-ttu-id="c9e66-113">Verziók</span><span class="sxs-lookup"><span data-stu-id="c9e66-113">Versions</span></span>
<span data-ttu-id="c9e66-114">**ApiVersion**: "2015-10-01"</span><span class="sxs-lookup"><span data-stu-id="c9e66-114">**ApiVersion**:   "2015-10-01"</span></span>

## <a name="new-azurermmediaservice"></a><span data-ttu-id="c9e66-115">New-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="c9e66-115">New-AzureRmMediaService</span></span>
<span data-ttu-id="c9e66-116">Media szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c9e66-116">Creates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="c9e66-117">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="c9e66-117">Syntax</span></span>
<span data-ttu-id="c9e66-118">A paraméterhalmaz: StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-118">Parameter Set: StorageAccountIdParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

<span data-ttu-id="c9e66-119">A paraméterhalmaz: StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-119">Parameter Set: StorageAccountsParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="c9e66-120">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="c9e66-120">Parameters</span></span>
<span data-ttu-id="c9e66-121">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-121">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-122">A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-122">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="c9e66-123">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-123">Aliases</span></span> | <span data-ttu-id="c9e66-124">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-125">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-125">Required?</span></span> |<span data-ttu-id="c9e66-126">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-126">true</span></span> |
| <span data-ttu-id="c9e66-127">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-127">Position?</span></span> |<span data-ttu-id="c9e66-128">0</span><span class="sxs-lookup"><span data-stu-id="c9e66-128">0</span></span> |
| <span data-ttu-id="c9e66-129">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-129">Default value</span></span> |<span data-ttu-id="c9e66-130">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-130">none</span></span> |
| <span data-ttu-id="c9e66-131">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-131">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-132">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-132">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-133">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-133">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-134">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-134">false</span></span> |

<span data-ttu-id="c9e66-135">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-135">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-136">Megadja a hello hello médiaszolgáltatás nevére.</span><span class="sxs-lookup"><span data-stu-id="c9e66-136">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="c9e66-137">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-137">Aliases</span></span> | <span data-ttu-id="c9e66-138">Név</span><span class="sxs-lookup"><span data-stu-id="c9e66-138">Name</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-139">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-139">Required?</span></span> |<span data-ttu-id="c9e66-140">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-140">true</span></span> |
| <span data-ttu-id="c9e66-141">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-141">Position?</span></span> |<span data-ttu-id="c9e66-142">1</span><span class="sxs-lookup"><span data-stu-id="c9e66-142">1</span></span> |
| <span data-ttu-id="c9e66-143">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-143">Default value</span></span> |<span data-ttu-id="c9e66-144">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-144">none</span></span> |
| <span data-ttu-id="c9e66-145">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-145">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-146">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-146">false</span></span> |
| <span data-ttu-id="c9e66-147">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-147">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-148">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-148">false</span></span> |

<span data-ttu-id="c9e66-149">**-Hely &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-149">**-Location &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-150">Hello media service hello erőforrás helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-150">Specifies hello resource location of hello media service.</span></span>

| <span data-ttu-id="c9e66-151">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-151">Aliases</span></span> | <span data-ttu-id="c9e66-152">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-152">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-153">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-153">Required?</span></span> |<span data-ttu-id="c9e66-154">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-154">true</span></span> |
| <span data-ttu-id="c9e66-155">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-155">Position?</span></span> |<span data-ttu-id="c9e66-156">2</span><span class="sxs-lookup"><span data-stu-id="c9e66-156">2</span></span> |
| <span data-ttu-id="c9e66-157">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-157">Default value</span></span> |<span data-ttu-id="c9e66-158">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-158">none</span></span> |
| <span data-ttu-id="c9e66-159">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-159">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-160">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-160">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-161">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-161">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-162">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-162">false</span></span> |

<span data-ttu-id="c9e66-163">**-StorageAccountId &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-163">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-164">Adja meg, amely hello media service társított elsődleges tárfiók.</span><span class="sxs-lookup"><span data-stu-id="c9e66-164">Specifies a primary storage account that associated with hello media service.</span></span>

* <span data-ttu-id="c9e66-165">Új tárfiók (Resource Manager API hello létre) csak a támogatott.</span><span class="sxs-lookup"><span data-stu-id="c9e66-165">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="c9e66-166">hello tárfiók létezése és rendelkezik hello hello media Service ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="c9e66-166">hello storage account must exist and has hello same location with hello media service.</span></span>

| <span data-ttu-id="c9e66-167">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-167">Aliases</span></span> | <span data-ttu-id="c9e66-168">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-168">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-169">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-169">Required?</span></span> |<span data-ttu-id="c9e66-170">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-170">true</span></span> |
| <span data-ttu-id="c9e66-171">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-171">Position?</span></span> |<span data-ttu-id="c9e66-172">3</span><span class="sxs-lookup"><span data-stu-id="c9e66-172">3</span></span> |
| <span data-ttu-id="c9e66-173">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-173">Default value</span></span> |<span data-ttu-id="c9e66-174">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-174">none</span></span> |
| <span data-ttu-id="c9e66-175">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-175">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-176">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-176">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-177">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="c9e66-177">Parameter set name</span></span> |<span data-ttu-id="c9e66-178">StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-178">StorageAccountIdParamSet</span></span> |
| <span data-ttu-id="c9e66-179">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-179">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-180">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-180">false</span></span> |

<span data-ttu-id="c9e66-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="c9e66-182">Adja meg a társított hello media Services, storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="c9e66-182">Specifies storage accounts that associated with hello media service.</span></span>

* <span data-ttu-id="c9e66-183">Új tárfiók (Resource Manager API hello létre) csak a támogatott.</span><span class="sxs-lookup"><span data-stu-id="c9e66-183">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="c9e66-184">hello tárfiók létezése és rendelkezik hello hello media Service ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="c9e66-184">hello storage account must exist and has hello same location with hello media service.</span></span>
* <span data-ttu-id="c9e66-185">Csak egy tárfiók elsődleges adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-185">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="c9e66-186">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-186">Aliases</span></span> | <span data-ttu-id="c9e66-187">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-187">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-188">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-188">Required?</span></span> |<span data-ttu-id="c9e66-189">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-189">true</span></span> |
| <span data-ttu-id="c9e66-190">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-190">Position?</span></span> |<span data-ttu-id="c9e66-191">3</span><span class="sxs-lookup"><span data-stu-id="c9e66-191">3</span></span> |
| <span data-ttu-id="c9e66-192">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-192">Default value</span></span> |<span data-ttu-id="c9e66-193">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-193">none</span></span> |
| <span data-ttu-id="c9e66-194">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-194">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-195">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-195">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-196">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="c9e66-196">Parameter set name</span></span> |<span data-ttu-id="c9e66-197">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-197">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="c9e66-198">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-198">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-199">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-199">false</span></span> |

<span data-ttu-id="c9e66-200">**-Címkéket &lt;Hashtable&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-200">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="c9e66-201">Megadja egy kivonattáblát a hello media service társított hello címkék.</span><span class="sxs-lookup"><span data-stu-id="c9e66-201">Specifies a hash table of hello tags that are associated with hello media service.</span></span>

* <span data-ttu-id="c9e66-202">Példa: @{"" ="érték1 tag1";" tag2 "=: Érték2"}</span><span class="sxs-lookup"><span data-stu-id="c9e66-202">Example: @{"tag1"="value1";"tag2"=:value2"}</span></span>

| <span data-ttu-id="c9e66-203">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-203">Aliases</span></span> | <span data-ttu-id="c9e66-204">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-204">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-205">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-205">Required?</span></span> |<span data-ttu-id="c9e66-206">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-206">false</span></span> |
| <span data-ttu-id="c9e66-207">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-207">Position?</span></span> |<span data-ttu-id="c9e66-208">nevű</span><span class="sxs-lookup"><span data-stu-id="c9e66-208">named</span></span> |
| <span data-ttu-id="c9e66-209">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-209">Default value</span></span> |<span data-ttu-id="c9e66-210">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-210">none</span></span> |
| <span data-ttu-id="c9e66-211">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-211">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-212">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-212">false</span></span> |
| <span data-ttu-id="c9e66-213">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-213">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-214">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-214">false</span></span> |

<span data-ttu-id="c9e66-215">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-215">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="c9e66-216">Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="c9e66-216">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="c9e66-217">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-217">Inputs</span></span>
<span data-ttu-id="c9e66-218">hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-218">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="c9e66-219">kimenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-219">Outputs</span></span>
<span data-ttu-id="c9e66-220">hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="c9e66-220">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="set-azurermmediaservice"></a><span data-ttu-id="c9e66-221">Set-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="c9e66-221">Set-AzureRmMediaService</span></span>
<span data-ttu-id="c9e66-222">Egy media Services frissíti.</span><span class="sxs-lookup"><span data-stu-id="c9e66-222">Updates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="c9e66-223">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="c9e66-223">Syntax</span></span>
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="c9e66-224">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="c9e66-224">Parameters</span></span>
<span data-ttu-id="c9e66-225">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-225">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-226">A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-226">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="c9e66-227">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-227">Aliases</span></span> | <span data-ttu-id="c9e66-228">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-228">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-229">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-229">Required?</span></span> |<span data-ttu-id="c9e66-230">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-230">true</span></span> |
| <span data-ttu-id="c9e66-231">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-231">Position?</span></span> |<span data-ttu-id="c9e66-232">0</span><span class="sxs-lookup"><span data-stu-id="c9e66-232">0</span></span> |
| <span data-ttu-id="c9e66-233">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-233">Default Value</span></span> |<span data-ttu-id="c9e66-234">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-234">none</span></span> |
| <span data-ttu-id="c9e66-235">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-235">Accept Pipeline Input?</span></span> |<span data-ttu-id="c9e66-236">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-236">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-237">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-237">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-238">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-238">false</span></span> |

<span data-ttu-id="c9e66-239">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-239">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-240">Megadja a hello hello médiaszolgáltatás nevére.</span><span class="sxs-lookup"><span data-stu-id="c9e66-240">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="c9e66-241">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-241">Aliases</span></span> | <span data-ttu-id="c9e66-242">Név</span><span class="sxs-lookup"><span data-stu-id="c9e66-242">Name</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-243">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-243">Required?</span></span> |<span data-ttu-id="c9e66-244">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="c9e66-244">True</span></span> |
| <span data-ttu-id="c9e66-245">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-245">Position?</span></span> |<span data-ttu-id="c9e66-246">1</span><span class="sxs-lookup"><span data-stu-id="c9e66-246">1</span></span> |
| <span data-ttu-id="c9e66-247">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-247">Default value</span></span> |<span data-ttu-id="c9e66-248">None</span><span class="sxs-lookup"><span data-stu-id="c9e66-248">None</span></span> |
| <span data-ttu-id="c9e66-249">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-249">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-250">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-250">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-251">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-251">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-252">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="c9e66-252">False</span></span> |

<span data-ttu-id="c9e66-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="c9e66-254">Adja meg a társított hello media Services, storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="c9e66-254">Specifies storage accounts that associated with hello media service.</span></span>

* <span data-ttu-id="c9e66-255">Új tárfiók (Resource Manager API hello létre) csak a támogatott.</span><span class="sxs-lookup"><span data-stu-id="c9e66-255">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="c9e66-256">hello tárfiók létezése és rendelkezik hello hello media Service ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="c9e66-256">hello storage account must exist and has hello same location with hello media service.</span></span>
* <span data-ttu-id="c9e66-257">Csak egy tárfiók elsődleges adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-257">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="c9e66-258">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-258">Aliases</span></span> | <span data-ttu-id="c9e66-259">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-259">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-260">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-260">Required?</span></span> |<span data-ttu-id="c9e66-261">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-261">false</span></span> |
| <span data-ttu-id="c9e66-262">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-262">Position?</span></span> |<span data-ttu-id="c9e66-263">nevű</span><span class="sxs-lookup"><span data-stu-id="c9e66-263">Named</span></span> |
| <span data-ttu-id="c9e66-264">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-264">Default value</span></span> |<span data-ttu-id="c9e66-265">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-265">none</span></span> |
| <span data-ttu-id="c9e66-266">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-266">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-267">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-267">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-268">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="c9e66-268">Parameter set name</span></span> |<span data-ttu-id="c9e66-269">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-269">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="c9e66-270">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-270">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-271">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-271">false</span></span> |

<span data-ttu-id="c9e66-272">**-Címkéket &lt;Hashtable&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-272">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="c9e66-273">Megadja egy kivonattáblát a media Services társított hello címkék.</span><span class="sxs-lookup"><span data-stu-id="c9e66-273">Specifies a hash table of hello tags that are associated with this media service.</span></span>

* <span data-ttu-id="c9e66-274">hello címkék hello media service társított cserélése hello ügyfél által megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="c9e66-274">hello tags that are associated with hello media service are replaced with value specified by hello customer.</span></span>

| <span data-ttu-id="c9e66-275">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-275">Aliases</span></span> | <span data-ttu-id="c9e66-276">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-276">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-277">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-277">Required?</span></span> |<span data-ttu-id="c9e66-278">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="c9e66-278">False</span></span> |
| <span data-ttu-id="c9e66-279">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-279">Position?</span></span> |<span data-ttu-id="c9e66-280">nevű</span><span class="sxs-lookup"><span data-stu-id="c9e66-280">Named</span></span> |
| <span data-ttu-id="c9e66-281">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-281">Default value</span></span> |<span data-ttu-id="c9e66-282">None</span><span class="sxs-lookup"><span data-stu-id="c9e66-282">None</span></span> |
| <span data-ttu-id="c9e66-283">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-283">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-284">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-284">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-285">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-285">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-286">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-286">false</span></span> |

<span data-ttu-id="c9e66-287">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-287">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="c9e66-288">Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="c9e66-288">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="c9e66-289">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-289">Inputs</span></span>
<span data-ttu-id="c9e66-290">hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-290">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="c9e66-291">kimenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-291">Outputs</span></span>
<span data-ttu-id="c9e66-292">hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="c9e66-292">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="remove-azurermmediaservice"></a><span data-ttu-id="c9e66-293">Remove-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="c9e66-293">Remove-AzureRmMediaService</span></span>
<span data-ttu-id="c9e66-294">Egy media Services eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-294">Removes a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="c9e66-295">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="c9e66-295">Syntax</span></span>
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="c9e66-296">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="c9e66-296">Parameters</span></span>
<span data-ttu-id="c9e66-297">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-297">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-298">A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-298">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="c9e66-299">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-299">Aliases</span></span> | <span data-ttu-id="c9e66-300">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-300">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-301">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-301">Required?</span></span> |<span data-ttu-id="c9e66-302">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-302">true</span></span> |
| <span data-ttu-id="c9e66-303">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-303">Position?</span></span> |<span data-ttu-id="c9e66-304">0</span><span class="sxs-lookup"><span data-stu-id="c9e66-304">0</span></span> |
| <span data-ttu-id="c9e66-305">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-305">Default value</span></span> |<span data-ttu-id="c9e66-306">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-306">none</span></span> |
| <span data-ttu-id="c9e66-307">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-307">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-308">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-308">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-309">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-309">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-310">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-310">false</span></span> |

<span data-ttu-id="c9e66-311">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-311">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-312">Megadja a hello hello médiaszolgáltatás nevére.</span><span class="sxs-lookup"><span data-stu-id="c9e66-312">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="c9e66-313">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-313">Aliases</span></span> | <span data-ttu-id="c9e66-314">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-314">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-315">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-315">Required?</span></span> |<span data-ttu-id="c9e66-316">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-316">true</span></span> |
| <span data-ttu-id="c9e66-317">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-317">Position?</span></span> |<span data-ttu-id="c9e66-318">2</span><span class="sxs-lookup"><span data-stu-id="c9e66-318">2</span></span> |
| <span data-ttu-id="c9e66-319">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-319">Default value</span></span> |<span data-ttu-id="c9e66-320">None</span><span class="sxs-lookup"><span data-stu-id="c9e66-320">None</span></span> |
| <span data-ttu-id="c9e66-321">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-321">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-322">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-322">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-323">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-323">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-324">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="c9e66-324">False</span></span> |

<span data-ttu-id="c9e66-325">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-325">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="c9e66-326">Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="c9e66-326">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="c9e66-327">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-327">Inputs</span></span>
<span data-ttu-id="c9e66-328">hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-328">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="c9e66-329">kimenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-329">Outputs</span></span>
<span data-ttu-id="c9e66-330">hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="c9e66-330">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="get-azurermmediaservice"></a><span data-ttu-id="c9e66-331">Get-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="c9e66-331">Get-AzureRmMediaService</span></span>
<span data-ttu-id="c9e66-332">Lekérdezi egy erőforráscsoportban található összes media services vagy egy media Services a megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="c9e66-332">Gets all media services in a resource group or a media service with a given name.</span></span>

### <a name="syntax"></a><span data-ttu-id="c9e66-333">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="c9e66-333">Syntax</span></span>
<span data-ttu-id="c9e66-334">ParameterSet: ResourceGroupParameterSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-334">ParameterSet: ResourceGroupParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

<span data-ttu-id="c9e66-335">ParameterSet: AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-335">ParameterSet: AccountNameParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="c9e66-336">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="c9e66-336">Parameters</span></span>
<span data-ttu-id="c9e66-337">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-337">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-338">A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-338">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="c9e66-339">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-339">Aliases</span></span> | <span data-ttu-id="c9e66-340">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-340">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-341">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-341">Required?</span></span> |<span data-ttu-id="c9e66-342">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-342">true</span></span> |
| <span data-ttu-id="c9e66-343">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-343">Position?</span></span> |<span data-ttu-id="c9e66-344">0</span><span class="sxs-lookup"><span data-stu-id="c9e66-344">0</span></span> |
| <span data-ttu-id="c9e66-345">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-345">Default value</span></span> |<span data-ttu-id="c9e66-346">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-346">none</span></span> |
| <span data-ttu-id="c9e66-347">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-347">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-348">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-348">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-349">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="c9e66-349">Parameter set name</span></span> |<span data-ttu-id="c9e66-350">ResourceGroupParameterSet, AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-350">ResourceGroupParameterSet, AccountNameParameterSet</span></span> |

<span data-ttu-id="c9e66-351">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-351">Accept wildcard characters?</span></span>   <span data-ttu-id="c9e66-352">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-352">false</span></span>

<span data-ttu-id="c9e66-353">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-353">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-354">Megadja a hello hello médiaszolgáltatás nevére.</span><span class="sxs-lookup"><span data-stu-id="c9e66-354">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="c9e66-355">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-355">Aliases</span></span> | <span data-ttu-id="c9e66-356">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-356">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-357">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-357">Required?</span></span> |<span data-ttu-id="c9e66-358">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-358">true</span></span> |
| <span data-ttu-id="c9e66-359">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-359">Position?</span></span> |<span data-ttu-id="c9e66-360">1</span><span class="sxs-lookup"><span data-stu-id="c9e66-360">1</span></span> |
| <span data-ttu-id="c9e66-361">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-361">Default value</span></span> |<span data-ttu-id="c9e66-362">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-362">none</span></span> |
| <span data-ttu-id="c9e66-363">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-363">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-364">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-364">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-365">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="c9e66-365">Parameter set name</span></span> |<span data-ttu-id="c9e66-366">AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="c9e66-366">AccountNameParameterSet</span></span> |
| <span data-ttu-id="c9e66-367">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-367">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-368">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-368">false</span></span> |

<span data-ttu-id="c9e66-369">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-369">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="c9e66-370">Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="c9e66-370">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="c9e66-371">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-371">Inputs</span></span>
<span data-ttu-id="c9e66-372">hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-372">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="c9e66-373">kimenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-373">Outputs</span></span>
<span data-ttu-id="c9e66-374">hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="c9e66-374">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="get-azurermmediaservicekeys"></a><span data-ttu-id="c9e66-375">Get-AzureRmMediaServiceKeys</span><span class="sxs-lookup"><span data-stu-id="c9e66-375">Get-AzureRmMediaServiceKeys</span></span>
<span data-ttu-id="c9e66-376">Lekérdezi a médiaszolgáltatás kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="c9e66-376">Gets keys of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="c9e66-377">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="c9e66-377">Syntax</span></span>
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="c9e66-378">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="c9e66-378">Parameters</span></span>
<span data-ttu-id="c9e66-379">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-379">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-380">A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-380">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="c9e66-381">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-381">Aliases</span></span> | <span data-ttu-id="c9e66-382">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-382">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-383">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-383">Required?</span></span> |<span data-ttu-id="c9e66-384">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-384">true</span></span> |
| <span data-ttu-id="c9e66-385">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-385">Position?</span></span> |<span data-ttu-id="c9e66-386">0</span><span class="sxs-lookup"><span data-stu-id="c9e66-386">0</span></span> |
| <span data-ttu-id="c9e66-387">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-387">Default value</span></span> |<span data-ttu-id="c9e66-388">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-388">none</span></span> |
| <span data-ttu-id="c9e66-389">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-389">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-390">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-390">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-391">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-391">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-392">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-392">false</span></span> |

<span data-ttu-id="c9e66-393">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-393">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-394">Megadja a hello hello médiaszolgáltatás nevére.</span><span class="sxs-lookup"><span data-stu-id="c9e66-394">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="c9e66-395">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-395">Aliases</span></span> | <span data-ttu-id="c9e66-396">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-396">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-397">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-397">Required?</span></span> |<span data-ttu-id="c9e66-398">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-398">true</span></span> |
| <span data-ttu-id="c9e66-399">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-399">Position?</span></span> |<span data-ttu-id="c9e66-400">1</span><span class="sxs-lookup"><span data-stu-id="c9e66-400">1</span></span> |
| <span data-ttu-id="c9e66-401">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-401">Default value</span></span> |<span data-ttu-id="c9e66-402">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-402">none</span></span> |
| <span data-ttu-id="c9e66-403">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-403">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-404">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-404">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-405">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-405">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-406">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-406">false</span></span> |

<span data-ttu-id="c9e66-407">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-407">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="c9e66-408">Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="c9e66-408">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="c9e66-409">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-409">Inputs</span></span>
<span data-ttu-id="c9e66-410">hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-410">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="c9e66-411">kimenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-411">Outputs</span></span>
<span data-ttu-id="c9e66-412">hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="c9e66-412">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="set-azurermmediaservicekey"></a><span data-ttu-id="c9e66-413">Set-AzureRmMediaServiceKey</span><span class="sxs-lookup"><span data-stu-id="c9e66-413">Set-AzureRmMediaServiceKey</span></span>
<span data-ttu-id="c9e66-414">Egy media Services elsődleges vagy másodlagos kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="c9e66-414">Regenerates a primary or secondary key of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="c9e66-415">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="c9e66-415">Syntax</span></span>
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="c9e66-416">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="c9e66-416">Parameters</span></span>
<span data-ttu-id="c9e66-417">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-417">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-418">A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-418">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="c9e66-419">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-419">Aliases</span></span> | <span data-ttu-id="c9e66-420">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-420">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-421">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-421">Required?</span></span> |<span data-ttu-id="c9e66-422">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-422">true</span></span> |
| <span data-ttu-id="c9e66-423">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-423">Position?</span></span> |<span data-ttu-id="c9e66-424">0</span><span class="sxs-lookup"><span data-stu-id="c9e66-424">0</span></span> |
| <span data-ttu-id="c9e66-425">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-425">Default value</span></span> |<span data-ttu-id="c9e66-426">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-426">none</span></span> |
| <span data-ttu-id="c9e66-427">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-427">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-428">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-428">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-429">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-429">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-430">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-430">false</span></span> |

<span data-ttu-id="c9e66-431">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-431">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-432">Megadja a hello hello médiaszolgáltatás nevére.</span><span class="sxs-lookup"><span data-stu-id="c9e66-432">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="c9e66-433">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-433">Aliases</span></span> | <span data-ttu-id="c9e66-434">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-434">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-435">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-435">Required?</span></span> |<span data-ttu-id="c9e66-436">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-436">true</span></span> |
| <span data-ttu-id="c9e66-437">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-437">Position?</span></span> |<span data-ttu-id="c9e66-438">1</span><span class="sxs-lookup"><span data-stu-id="c9e66-438">1</span></span> |
| <span data-ttu-id="c9e66-439">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-439">Default value</span></span> |<span data-ttu-id="c9e66-440">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-440">none</span></span> |
| <span data-ttu-id="c9e66-441">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-441">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-442">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-442">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-443">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-443">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-444">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-444">false</span></span> |

<span data-ttu-id="c9e66-445">**KeyType – &lt;KeyType&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-445">**-KeyType &lt;KeyType&gt;**</span></span>

<span data-ttu-id="c9e66-446">Adja meg a hello media service hello kulcstípusával.</span><span class="sxs-lookup"><span data-stu-id="c9e66-446">Specifies hello key type of hello media service.</span></span>

* <span data-ttu-id="c9e66-447">Elsődleges vagy másodlagos</span><span class="sxs-lookup"><span data-stu-id="c9e66-447">Primary or Secondary</span></span>

| <span data-ttu-id="c9e66-448">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-448">Aliases</span></span> | <span data-ttu-id="c9e66-449">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-449">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-450">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-450">Required?</span></span> |<span data-ttu-id="c9e66-451">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-451">true</span></span> |
| <span data-ttu-id="c9e66-452">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-452">Position?</span></span> |<span data-ttu-id="c9e66-453">2</span><span class="sxs-lookup"><span data-stu-id="c9e66-453">2</span></span> |
| <span data-ttu-id="c9e66-454">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-454">Default value</span></span> |<span data-ttu-id="c9e66-455">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-455">none</span></span> |
| <span data-ttu-id="c9e66-456">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-456">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-457">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-457">false</span></span> |
| <span data-ttu-id="c9e66-458">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-458">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-459">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-459">false</span></span> |

<span data-ttu-id="c9e66-460">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-460">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="c9e66-461">Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="c9e66-461">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="c9e66-462">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-462">Inputs</span></span>
<span data-ttu-id="c9e66-463">hello bemeneti típus hello hello típusú objektumokat toothe parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-463">hello input type is hello type of hello objects that you can pipe toothe cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="c9e66-464">kimenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-464">Outputs</span></span>
<span data-ttu-id="c9e66-465">hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="c9e66-465">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="sync-azurermmediaservicestoragekeys"></a><span data-ttu-id="c9e66-466">Sync-AzureRmMediaServiceStorageKeys</span><span class="sxs-lookup"><span data-stu-id="c9e66-466">Sync-AzureRmMediaServiceStorageKeys</span></span>
<span data-ttu-id="c9e66-467">Hello media service társított storage-fiók tárfiókkulcsainak szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="c9e66-467">Synchronizes storage account keys for a storage account associated with hello media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="c9e66-468">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="c9e66-468">Syntax</span></span>
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="c9e66-469">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="c9e66-469">Parameters</span></span>
<span data-ttu-id="c9e66-470">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-470">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-471">A media Services tartozik hello erőforrás csoport toowhich hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c9e66-471">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="c9e66-472">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-472">Aliases</span></span> | <span data-ttu-id="c9e66-473">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-473">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-474">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-474">Required?</span></span> |<span data-ttu-id="c9e66-475">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-475">true</span></span> |
| <span data-ttu-id="c9e66-476">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-476">Position?</span></span> |<span data-ttu-id="c9e66-477">0</span><span class="sxs-lookup"><span data-stu-id="c9e66-477">0</span></span> |
| <span data-ttu-id="c9e66-478">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-478">Default value</span></span> |<span data-ttu-id="c9e66-479">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-479">none</span></span> |
| <span data-ttu-id="c9e66-480">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-480">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-481">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-481">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-482">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-482">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-483">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-483">false</span></span> |

<span data-ttu-id="c9e66-484">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-484">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-485">Megadja a hello hello médiaszolgáltatás nevére.</span><span class="sxs-lookup"><span data-stu-id="c9e66-485">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="c9e66-486">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-486">Aliases</span></span> | <span data-ttu-id="c9e66-487">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-487">none</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-488">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-488">Required?</span></span> |<span data-ttu-id="c9e66-489">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-489">true</span></span> |
| <span data-ttu-id="c9e66-490">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-490">Position?</span></span> |<span data-ttu-id="c9e66-491">1</span><span class="sxs-lookup"><span data-stu-id="c9e66-491">1</span></span> |
| <span data-ttu-id="c9e66-492">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-492">Default value</span></span> |<span data-ttu-id="c9e66-493">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-493">none</span></span> |
| <span data-ttu-id="c9e66-494">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-494">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-495">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-495">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-496">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-496">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-497">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-497">false</span></span> |

<span data-ttu-id="c9e66-498">**-StorageAccountId &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-498">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="c9e66-499">Megadja a hello hello media szolgáltatáshoz tartozó tárfiók.</span><span class="sxs-lookup"><span data-stu-id="c9e66-499">Specifies hello storage account associated with hello media service.</span></span>

| <span data-ttu-id="c9e66-500">Aliasok</span><span class="sxs-lookup"><span data-stu-id="c9e66-500">Aliases</span></span> | <span data-ttu-id="c9e66-501">Azonosító</span><span class="sxs-lookup"><span data-stu-id="c9e66-501">Id</span></span> |
| --- | --- |
| <span data-ttu-id="c9e66-502">Kötelező?</span><span class="sxs-lookup"><span data-stu-id="c9e66-502">Required?</span></span> |<span data-ttu-id="c9e66-503">Igaz</span><span class="sxs-lookup"><span data-stu-id="c9e66-503">true</span></span> |
| <span data-ttu-id="c9e66-504">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="c9e66-504">Position?</span></span> |<span data-ttu-id="c9e66-505">2</span><span class="sxs-lookup"><span data-stu-id="c9e66-505">2</span></span> |
| <span data-ttu-id="c9e66-506">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="c9e66-506">Default value</span></span> |<span data-ttu-id="c9e66-507">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="c9e66-507">none</span></span> |
| <span data-ttu-id="c9e66-508">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="c9e66-508">Accept pipeline input?</span></span> |<span data-ttu-id="c9e66-509">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="c9e66-509">true(ByPropertyName)</span></span> |
| <span data-ttu-id="c9e66-510">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="c9e66-510">Accept wildcard characters?</span></span> |<span data-ttu-id="c9e66-511">hamis</span><span class="sxs-lookup"><span data-stu-id="c9e66-511">false</span></span> |

<span data-ttu-id="c9e66-512">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="c9e66-512">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="c9e66-513">Ez a parancsmag hello általános paramétereket támogatja:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction és - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="c9e66-513">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="c9e66-514">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-514">Inputs</span></span>
<span data-ttu-id="c9e66-515">hello bemeneti típus hello hello típusú objektumokat toohello parancsmag is átadhatja.</span><span class="sxs-lookup"><span data-stu-id="c9e66-515">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="c9e66-516">kimenetek</span><span class="sxs-lookup"><span data-stu-id="c9e66-516">Outputs</span></span>
<span data-ttu-id="c9e66-517">hello kimeneti típus parancsmag hello hello objektumok hello típusú bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="c9e66-517">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="next-step"></a><span data-ttu-id="c9e66-518">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="c9e66-518">Next step</span></span>
<span data-ttu-id="c9e66-519">Tekintse meg a Media Services tanulási útvonalai.</span><span class="sxs-lookup"><span data-stu-id="c9e66-519">Check out Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c9e66-520">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="c9e66-520">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

