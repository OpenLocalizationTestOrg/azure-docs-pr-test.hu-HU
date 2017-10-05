---
title: "A PowerShell segítségével az Azure Media Services fiókok kezelése"
description: "Útmutató: Azure Media Services-fiókok a PowerShell-parancsmagokkal kezelheti."
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
ms.openlocfilehash: 3d999d9e27844bc0164cc3572522b9ec022118a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a><span data-ttu-id="f1b9f-103">A PowerShell segítségével az Azure Media Services fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="f1b9f-103">Manage Azure Media Services Accounts with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f1b9f-104">Portal</span><span class="sxs-lookup"><span data-stu-id="f1b9f-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="f1b9f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1b9f-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="f1b9f-106">REST</span><span class="sxs-lookup"><span data-stu-id="f1b9f-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="f1b9f-107">Nem fogja tudni az Azure Media Services-fiók létrehozása, rendelkeznie kell egy Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-107">To be able to create an Azure Media Services account, you must have an Azure account.</span></span> <span data-ttu-id="f1b9f-108">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f1b9f-109">További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-109">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="f1b9f-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f1b9f-110">Overview</span></span>
<span data-ttu-id="f1b9f-111">Ez a cikk az Azure Media Services (AMS) tartalmazza az Azure PowerShell-parancsmagok az Azure Resource Manager keretében.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-111">This article lists the Azure PowerShell cmdlets for Azure Media Services (AMS) in the Azure Resource Manager framework.</span></span> <span data-ttu-id="f1b9f-112">A parancsmagok szerepel a **Microsoft.Azure.Commands.Media** névtér.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-112">The cmdlets exist in the **Microsoft.Azure.Commands.Media** namespace.</span></span>

## <a name="versions"></a><span data-ttu-id="f1b9f-113">Verziók</span><span class="sxs-lookup"><span data-stu-id="f1b9f-113">Versions</span></span>
<span data-ttu-id="f1b9f-114">**ApiVersion**: "2015-10-01"</span><span class="sxs-lookup"><span data-stu-id="f1b9f-114">**ApiVersion**:   "2015-10-01"</span></span>

## <a name="new-azurermmediaservice"></a><span data-ttu-id="f1b9f-115">New-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="f1b9f-115">New-AzureRmMediaService</span></span>
<span data-ttu-id="f1b9f-116">Media szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-116">Creates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="f1b9f-117">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-117">Syntax</span></span>
<span data-ttu-id="f1b9f-118">A paraméterhalmaz: StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-118">Parameter Set: StorageAccountIdParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

<span data-ttu-id="f1b9f-119">A paraméterhalmaz: StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-119">Parameter Set: StorageAccountsParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="f1b9f-120">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-120">Parameters</span></span>
<span data-ttu-id="f1b9f-121">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-121">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-122">Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-122">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="f1b9f-123">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-123">Aliases</span></span> | <span data-ttu-id="f1b9f-124">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-125">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-125">Required?</span></span> |<span data-ttu-id="f1b9f-126">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-126">true</span></span> |
| <span data-ttu-id="f1b9f-127">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-127">Position?</span></span> |<span data-ttu-id="f1b9f-128">0</span><span class="sxs-lookup"><span data-stu-id="f1b9f-128">0</span></span> |
| <span data-ttu-id="f1b9f-129">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-129">Default value</span></span> |<span data-ttu-id="f1b9f-130">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-130">none</span></span> |
| <span data-ttu-id="f1b9f-131">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-131">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-132">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-132">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-133">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-133">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-134">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-134">false</span></span> |

<span data-ttu-id="f1b9f-135">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-135">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-136">Megadja a media Services nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-136">Specifies the name of the media service.</span></span>

| <span data-ttu-id="f1b9f-137">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-137">Aliases</span></span> | <span data-ttu-id="f1b9f-138">Név</span><span class="sxs-lookup"><span data-stu-id="f1b9f-138">Name</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-139">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-139">Required?</span></span> |<span data-ttu-id="f1b9f-140">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-140">true</span></span> |
| <span data-ttu-id="f1b9f-141">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-141">Position?</span></span> |<span data-ttu-id="f1b9f-142">1</span><span class="sxs-lookup"><span data-stu-id="f1b9f-142">1</span></span> |
| <span data-ttu-id="f1b9f-143">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-143">Default value</span></span> |<span data-ttu-id="f1b9f-144">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-144">none</span></span> |
| <span data-ttu-id="f1b9f-145">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-145">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-146">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-146">false</span></span> |
| <span data-ttu-id="f1b9f-147">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-147">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-148">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-148">false</span></span> |

<span data-ttu-id="f1b9f-149">**-Hely &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-149">**-Location &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-150">A media Services erőforrás helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-150">Specifies the resource location of the media service.</span></span>

| <span data-ttu-id="f1b9f-151">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-151">Aliases</span></span> | <span data-ttu-id="f1b9f-152">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-152">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-153">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-153">Required?</span></span> |<span data-ttu-id="f1b9f-154">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-154">true</span></span> |
| <span data-ttu-id="f1b9f-155">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-155">Position?</span></span> |<span data-ttu-id="f1b9f-156">2</span><span class="sxs-lookup"><span data-stu-id="f1b9f-156">2</span></span> |
| <span data-ttu-id="f1b9f-157">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-157">Default value</span></span> |<span data-ttu-id="f1b9f-158">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-158">none</span></span> |
| <span data-ttu-id="f1b9f-159">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-159">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-160">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-160">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-161">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-161">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-162">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-162">false</span></span> |

<span data-ttu-id="f1b9f-163">**-StorageAccountId &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-163">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-164">Meghatározza, hogy a media Services társított elsődleges tárfiók.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-164">Specifies a primary storage account that associated with the media service.</span></span>

* <span data-ttu-id="f1b9f-165">Új tárfiók (Resource Manager API-val létrehozott) csak a támogatott.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-165">New storage account (created with the Resource Manager API) supported only.</span></span>
* <span data-ttu-id="f1b9f-166">A tárfiók már léteznie kell, és ugyanazon a helyen, a media szolgáltatással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-166">The storage account must exist and has the same location with the media service.</span></span>

| <span data-ttu-id="f1b9f-167">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-167">Aliases</span></span> | <span data-ttu-id="f1b9f-168">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-168">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-169">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-169">Required?</span></span> |<span data-ttu-id="f1b9f-170">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-170">true</span></span> |
| <span data-ttu-id="f1b9f-171">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-171">Position?</span></span> |<span data-ttu-id="f1b9f-172">3</span><span class="sxs-lookup"><span data-stu-id="f1b9f-172">3</span></span> |
| <span data-ttu-id="f1b9f-173">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-173">Default value</span></span> |<span data-ttu-id="f1b9f-174">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-174">none</span></span> |
| <span data-ttu-id="f1b9f-175">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-175">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-176">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-176">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-177">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="f1b9f-177">Parameter set name</span></span> |<span data-ttu-id="f1b9f-178">StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-178">StorageAccountIdParamSet</span></span> |
| <span data-ttu-id="f1b9f-179">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-179">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-180">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-180">false</span></span> |

<span data-ttu-id="f1b9f-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="f1b9f-182">Meghatározza, hogy a media Services társított tárfiókokat.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-182">Specifies storage accounts that associated with the media service.</span></span>

* <span data-ttu-id="f1b9f-183">Új tárfiók (Resource Manager API-val létrehozott) csak a támogatott.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-183">New storage account (created with the Resource Manager API) supported only.</span></span>
* <span data-ttu-id="f1b9f-184">A tárfiók már léteznie kell, és ugyanazon a helyen, a media szolgáltatással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-184">The storage account must exist and has the same location with the media service.</span></span>
* <span data-ttu-id="f1b9f-185">Csak egy tárfiók elsődleges adhat meg.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-185">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="f1b9f-186">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-186">Aliases</span></span> | <span data-ttu-id="f1b9f-187">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-187">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-188">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-188">Required?</span></span> |<span data-ttu-id="f1b9f-189">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-189">true</span></span> |
| <span data-ttu-id="f1b9f-190">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-190">Position?</span></span> |<span data-ttu-id="f1b9f-191">3</span><span class="sxs-lookup"><span data-stu-id="f1b9f-191">3</span></span> |
| <span data-ttu-id="f1b9f-192">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-192">Default value</span></span> |<span data-ttu-id="f1b9f-193">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-193">none</span></span> |
| <span data-ttu-id="f1b9f-194">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-194">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-195">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-195">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-196">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="f1b9f-196">Parameter set name</span></span> |<span data-ttu-id="f1b9f-197">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-197">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="f1b9f-198">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-198">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-199">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-199">false</span></span> |

<span data-ttu-id="f1b9f-200">**-Címkéket &lt;Hashtable&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-200">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="f1b9f-201">Megadja egy kivonattáblát a címkék a media Services társított.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-201">Specifies a hash table of the tags that are associated with the media service.</span></span>

* <span data-ttu-id="f1b9f-202">Példa: @{"" ="érték1 tag1";" tag2 "=: Érték2"}</span><span class="sxs-lookup"><span data-stu-id="f1b9f-202">Example: @{"tag1"="value1";"tag2"=:value2"}</span></span>

| <span data-ttu-id="f1b9f-203">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-203">Aliases</span></span> | <span data-ttu-id="f1b9f-204">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-204">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-205">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-205">Required?</span></span> |<span data-ttu-id="f1b9f-206">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-206">false</span></span> |
| <span data-ttu-id="f1b9f-207">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-207">Position?</span></span> |<span data-ttu-id="f1b9f-208">nevű</span><span class="sxs-lookup"><span data-stu-id="f1b9f-208">named</span></span> |
| <span data-ttu-id="f1b9f-209">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-209">Default value</span></span> |<span data-ttu-id="f1b9f-210">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-210">none</span></span> |
| <span data-ttu-id="f1b9f-211">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-211">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-212">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-212">false</span></span> |
| <span data-ttu-id="f1b9f-213">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-213">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-214">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-214">false</span></span> |

<span data-ttu-id="f1b9f-215">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-215">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="f1b9f-216">Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-216">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="f1b9f-217">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-217">Inputs</span></span>
<span data-ttu-id="f1b9f-218">A bemeneti típus a parancsmagnak átadható objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-218">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="f1b9f-219">kimenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-219">Outputs</span></span>
<span data-ttu-id="f1b9f-220">A kimeneti típus a parancsmag által létrehozott objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-220">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="set-azurermmediaservice"></a><span data-ttu-id="f1b9f-221">Set-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="f1b9f-221">Set-AzureRmMediaService</span></span>
<span data-ttu-id="f1b9f-222">Egy media Services frissíti.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-222">Updates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="f1b9f-223">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-223">Syntax</span></span>
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="f1b9f-224">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-224">Parameters</span></span>
<span data-ttu-id="f1b9f-225">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-225">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-226">Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-226">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="f1b9f-227">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-227">Aliases</span></span> | <span data-ttu-id="f1b9f-228">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-228">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-229">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-229">Required?</span></span> |<span data-ttu-id="f1b9f-230">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-230">true</span></span> |
| <span data-ttu-id="f1b9f-231">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-231">Position?</span></span> |<span data-ttu-id="f1b9f-232">0</span><span class="sxs-lookup"><span data-stu-id="f1b9f-232">0</span></span> |
| <span data-ttu-id="f1b9f-233">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-233">Default Value</span></span> |<span data-ttu-id="f1b9f-234">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-234">none</span></span> |
| <span data-ttu-id="f1b9f-235">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-235">Accept Pipeline Input?</span></span> |<span data-ttu-id="f1b9f-236">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-236">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-237">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-237">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-238">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-238">false</span></span> |

<span data-ttu-id="f1b9f-239">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-239">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-240">Megadja a media Services nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-240">Specifies the name of the media service.</span></span>

| <span data-ttu-id="f1b9f-241">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-241">Aliases</span></span> | <span data-ttu-id="f1b9f-242">Név</span><span class="sxs-lookup"><span data-stu-id="f1b9f-242">Name</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-243">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-243">Required?</span></span> |<span data-ttu-id="f1b9f-244">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-244">True</span></span> |
| <span data-ttu-id="f1b9f-245">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-245">Position?</span></span> |<span data-ttu-id="f1b9f-246">1</span><span class="sxs-lookup"><span data-stu-id="f1b9f-246">1</span></span> |
| <span data-ttu-id="f1b9f-247">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-247">Default value</span></span> |<span data-ttu-id="f1b9f-248">None</span><span class="sxs-lookup"><span data-stu-id="f1b9f-248">None</span></span> |
| <span data-ttu-id="f1b9f-249">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-249">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-250">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-250">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-251">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-251">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-252">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-252">False</span></span> |

<span data-ttu-id="f1b9f-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="f1b9f-254">Meghatározza, hogy a media Services társított tárfiókokat.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-254">Specifies storage accounts that associated with the media service.</span></span>

* <span data-ttu-id="f1b9f-255">Új tárfiók (Resource Manager API-val létrehozott) csak a támogatott.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-255">New storage account (created with the Resource Manager API) supported only.</span></span>
* <span data-ttu-id="f1b9f-256">A tárfiók már léteznie kell, és ugyanazon a helyen, a media szolgáltatással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-256">The storage account must exist and has the same location with the media service.</span></span>
* <span data-ttu-id="f1b9f-257">Csak egy tárfiók elsődleges adhat meg.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-257">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="f1b9f-258">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-258">Aliases</span></span> | <span data-ttu-id="f1b9f-259">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-259">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-260">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-260">Required?</span></span> |<span data-ttu-id="f1b9f-261">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-261">false</span></span> |
| <span data-ttu-id="f1b9f-262">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-262">Position?</span></span> |<span data-ttu-id="f1b9f-263">nevű</span><span class="sxs-lookup"><span data-stu-id="f1b9f-263">Named</span></span> |
| <span data-ttu-id="f1b9f-264">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-264">Default value</span></span> |<span data-ttu-id="f1b9f-265">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-265">none</span></span> |
| <span data-ttu-id="f1b9f-266">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-266">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-267">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-267">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-268">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="f1b9f-268">Parameter set name</span></span> |<span data-ttu-id="f1b9f-269">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-269">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="f1b9f-270">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-270">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-271">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-271">false</span></span> |

<span data-ttu-id="f1b9f-272">**-Címkéket &lt;Hashtable&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-272">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="f1b9f-273">Megadja egy kivonattáblát a címkék a media Services társított.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-273">Specifies a hash table of the tags that are associated with this media service.</span></span>

* <span data-ttu-id="f1b9f-274">A media Services társított címkék értékkel az ügyfél által megadott helyett.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-274">The tags that are associated with the media service are replaced with value specified by the customer.</span></span>

| <span data-ttu-id="f1b9f-275">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-275">Aliases</span></span> | <span data-ttu-id="f1b9f-276">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-276">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-277">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-277">Required?</span></span> |<span data-ttu-id="f1b9f-278">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-278">False</span></span> |
| <span data-ttu-id="f1b9f-279">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-279">Position?</span></span> |<span data-ttu-id="f1b9f-280">nevű</span><span class="sxs-lookup"><span data-stu-id="f1b9f-280">Named</span></span> |
| <span data-ttu-id="f1b9f-281">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-281">Default value</span></span> |<span data-ttu-id="f1b9f-282">None</span><span class="sxs-lookup"><span data-stu-id="f1b9f-282">None</span></span> |
| <span data-ttu-id="f1b9f-283">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-283">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-284">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-284">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-285">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-285">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-286">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-286">false</span></span> |

<span data-ttu-id="f1b9f-287">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-287">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="f1b9f-288">Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-288">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="f1b9f-289">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-289">Inputs</span></span>
<span data-ttu-id="f1b9f-290">A bemeneti típus a parancsmagnak átadható objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-290">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="f1b9f-291">kimenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-291">Outputs</span></span>
<span data-ttu-id="f1b9f-292">A kimeneti típus a parancsmag által létrehozott objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-292">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="remove-azurermmediaservice"></a><span data-ttu-id="f1b9f-293">Remove-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="f1b9f-293">Remove-AzureRmMediaService</span></span>
<span data-ttu-id="f1b9f-294">Egy media Services eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-294">Removes a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="f1b9f-295">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-295">Syntax</span></span>
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="f1b9f-296">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-296">Parameters</span></span>
<span data-ttu-id="f1b9f-297">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-297">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-298">Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-298">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="f1b9f-299">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-299">Aliases</span></span> | <span data-ttu-id="f1b9f-300">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-300">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-301">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-301">Required?</span></span> |<span data-ttu-id="f1b9f-302">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-302">true</span></span> |
| <span data-ttu-id="f1b9f-303">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-303">Position?</span></span> |<span data-ttu-id="f1b9f-304">0</span><span class="sxs-lookup"><span data-stu-id="f1b9f-304">0</span></span> |
| <span data-ttu-id="f1b9f-305">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-305">Default value</span></span> |<span data-ttu-id="f1b9f-306">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-306">none</span></span> |
| <span data-ttu-id="f1b9f-307">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-307">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-308">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-308">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-309">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-309">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-310">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-310">false</span></span> |

<span data-ttu-id="f1b9f-311">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-311">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-312">Megadja a media Services nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-312">Specifies the name of the media service.</span></span>

| <span data-ttu-id="f1b9f-313">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-313">Aliases</span></span> | <span data-ttu-id="f1b9f-314">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-314">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-315">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-315">Required?</span></span> |<span data-ttu-id="f1b9f-316">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-316">true</span></span> |
| <span data-ttu-id="f1b9f-317">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-317">Position?</span></span> |<span data-ttu-id="f1b9f-318">2</span><span class="sxs-lookup"><span data-stu-id="f1b9f-318">2</span></span> |
| <span data-ttu-id="f1b9f-319">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-319">Default value</span></span> |<span data-ttu-id="f1b9f-320">None</span><span class="sxs-lookup"><span data-stu-id="f1b9f-320">None</span></span> |
| <span data-ttu-id="f1b9f-321">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-321">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-322">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-322">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-323">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-323">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-324">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-324">False</span></span> |

<span data-ttu-id="f1b9f-325">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-325">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="f1b9f-326">Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-326">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="f1b9f-327">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-327">Inputs</span></span>
<span data-ttu-id="f1b9f-328">A bemeneti típus a parancsmagnak átadható objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-328">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="f1b9f-329">kimenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-329">Outputs</span></span>
<span data-ttu-id="f1b9f-330">A kimeneti típus a parancsmag által létrehozott objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-330">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="get-azurermmediaservice"></a><span data-ttu-id="f1b9f-331">Get-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="f1b9f-331">Get-AzureRmMediaService</span></span>
<span data-ttu-id="f1b9f-332">Lekérdezi egy erőforráscsoportban található összes media services vagy egy media Services a megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-332">Gets all media services in a resource group or a media service with a given name.</span></span>

### <a name="syntax"></a><span data-ttu-id="f1b9f-333">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-333">Syntax</span></span>
<span data-ttu-id="f1b9f-334">ParameterSet: ResourceGroupParameterSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-334">ParameterSet: ResourceGroupParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

<span data-ttu-id="f1b9f-335">ParameterSet: AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-335">ParameterSet: AccountNameParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="f1b9f-336">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-336">Parameters</span></span>
<span data-ttu-id="f1b9f-337">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-337">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-338">Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-338">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="f1b9f-339">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-339">Aliases</span></span> | <span data-ttu-id="f1b9f-340">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-340">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-341">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-341">Required?</span></span> |<span data-ttu-id="f1b9f-342">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-342">true</span></span> |
| <span data-ttu-id="f1b9f-343">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-343">Position?</span></span> |<span data-ttu-id="f1b9f-344">0</span><span class="sxs-lookup"><span data-stu-id="f1b9f-344">0</span></span> |
| <span data-ttu-id="f1b9f-345">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-345">Default value</span></span> |<span data-ttu-id="f1b9f-346">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-346">none</span></span> |
| <span data-ttu-id="f1b9f-347">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-347">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-348">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-348">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-349">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="f1b9f-349">Parameter set name</span></span> |<span data-ttu-id="f1b9f-350">ResourceGroupParameterSet, AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-350">ResourceGroupParameterSet, AccountNameParameterSet</span></span> |

<span data-ttu-id="f1b9f-351">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-351">Accept wildcard characters?</span></span>   <span data-ttu-id="f1b9f-352">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-352">false</span></span>

<span data-ttu-id="f1b9f-353">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-353">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-354">Megadja a media Services nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-354">Specifies the name of the media service.</span></span>

| <span data-ttu-id="f1b9f-355">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-355">Aliases</span></span> | <span data-ttu-id="f1b9f-356">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-356">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-357">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-357">Required?</span></span> |<span data-ttu-id="f1b9f-358">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-358">true</span></span> |
| <span data-ttu-id="f1b9f-359">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-359">Position?</span></span> |<span data-ttu-id="f1b9f-360">1</span><span class="sxs-lookup"><span data-stu-id="f1b9f-360">1</span></span> |
| <span data-ttu-id="f1b9f-361">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-361">Default value</span></span> |<span data-ttu-id="f1b9f-362">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-362">none</span></span> |
| <span data-ttu-id="f1b9f-363">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-363">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-364">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-364">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-365">A paraméter neve</span><span class="sxs-lookup"><span data-stu-id="f1b9f-365">Parameter set name</span></span> |<span data-ttu-id="f1b9f-366">AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="f1b9f-366">AccountNameParameterSet</span></span> |
| <span data-ttu-id="f1b9f-367">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-367">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-368">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-368">false</span></span> |

<span data-ttu-id="f1b9f-369">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-369">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="f1b9f-370">Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-370">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="f1b9f-371">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-371">Inputs</span></span>
<span data-ttu-id="f1b9f-372">A bemeneti típus a parancsmagnak átadható objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-372">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="f1b9f-373">kimenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-373">Outputs</span></span>
<span data-ttu-id="f1b9f-374">A kimeneti típus a parancsmag által létrehozott objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-374">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="get-azurermmediaservicekeys"></a><span data-ttu-id="f1b9f-375">Get-AzureRmMediaServiceKeys</span><span class="sxs-lookup"><span data-stu-id="f1b9f-375">Get-AzureRmMediaServiceKeys</span></span>
<span data-ttu-id="f1b9f-376">Lekérdezi a médiaszolgáltatás kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-376">Gets keys of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="f1b9f-377">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-377">Syntax</span></span>
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="f1b9f-378">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-378">Parameters</span></span>
<span data-ttu-id="f1b9f-379">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-379">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-380">Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-380">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="f1b9f-381">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-381">Aliases</span></span> | <span data-ttu-id="f1b9f-382">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-382">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-383">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-383">Required?</span></span> |<span data-ttu-id="f1b9f-384">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-384">true</span></span> |
| <span data-ttu-id="f1b9f-385">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-385">Position?</span></span> |<span data-ttu-id="f1b9f-386">0</span><span class="sxs-lookup"><span data-stu-id="f1b9f-386">0</span></span> |
| <span data-ttu-id="f1b9f-387">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-387">Default value</span></span> |<span data-ttu-id="f1b9f-388">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-388">none</span></span> |
| <span data-ttu-id="f1b9f-389">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-389">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-390">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-390">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-391">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-391">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-392">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-392">false</span></span> |

<span data-ttu-id="f1b9f-393">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-393">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-394">Megadja a media Services nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-394">Specifies the name of the media service.</span></span>

| <span data-ttu-id="f1b9f-395">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-395">Aliases</span></span> | <span data-ttu-id="f1b9f-396">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-396">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-397">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-397">Required?</span></span> |<span data-ttu-id="f1b9f-398">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-398">true</span></span> |
| <span data-ttu-id="f1b9f-399">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-399">Position?</span></span> |<span data-ttu-id="f1b9f-400">1</span><span class="sxs-lookup"><span data-stu-id="f1b9f-400">1</span></span> |
| <span data-ttu-id="f1b9f-401">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-401">Default value</span></span> |<span data-ttu-id="f1b9f-402">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-402">none</span></span> |
| <span data-ttu-id="f1b9f-403">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-403">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-404">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-404">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-405">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-405">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-406">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-406">false</span></span> |

<span data-ttu-id="f1b9f-407">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-407">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="f1b9f-408">Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-408">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="f1b9f-409">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-409">Inputs</span></span>
<span data-ttu-id="f1b9f-410">A bemeneti típus a parancsmagnak átadható objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-410">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="f1b9f-411">kimenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-411">Outputs</span></span>
<span data-ttu-id="f1b9f-412">A kimeneti típus a parancsmag által létrehozott objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-412">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="set-azurermmediaservicekey"></a><span data-ttu-id="f1b9f-413">Set-AzureRmMediaServiceKey</span><span class="sxs-lookup"><span data-stu-id="f1b9f-413">Set-AzureRmMediaServiceKey</span></span>
<span data-ttu-id="f1b9f-414">Egy media Services elsődleges vagy másodlagos kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-414">Regenerates a primary or secondary key of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="f1b9f-415">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-415">Syntax</span></span>
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="f1b9f-416">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-416">Parameters</span></span>
<span data-ttu-id="f1b9f-417">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-417">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-418">Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-418">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="f1b9f-419">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-419">Aliases</span></span> | <span data-ttu-id="f1b9f-420">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-420">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-421">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-421">Required?</span></span> |<span data-ttu-id="f1b9f-422">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-422">true</span></span> |
| <span data-ttu-id="f1b9f-423">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-423">Position?</span></span> |<span data-ttu-id="f1b9f-424">0</span><span class="sxs-lookup"><span data-stu-id="f1b9f-424">0</span></span> |
| <span data-ttu-id="f1b9f-425">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-425">Default value</span></span> |<span data-ttu-id="f1b9f-426">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-426">none</span></span> |
| <span data-ttu-id="f1b9f-427">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-427">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-428">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-428">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-429">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-429">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-430">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-430">false</span></span> |

<span data-ttu-id="f1b9f-431">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-431">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-432">Megadja a media Services nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-432">Specifies the name of the media service.</span></span>

| <span data-ttu-id="f1b9f-433">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-433">Aliases</span></span> | <span data-ttu-id="f1b9f-434">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-434">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-435">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-435">Required?</span></span> |<span data-ttu-id="f1b9f-436">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-436">true</span></span> |
| <span data-ttu-id="f1b9f-437">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-437">Position?</span></span> |<span data-ttu-id="f1b9f-438">1</span><span class="sxs-lookup"><span data-stu-id="f1b9f-438">1</span></span> |
| <span data-ttu-id="f1b9f-439">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-439">Default value</span></span> |<span data-ttu-id="f1b9f-440">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-440">none</span></span> |
| <span data-ttu-id="f1b9f-441">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-441">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-442">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-442">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-443">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-443">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-444">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-444">false</span></span> |

<span data-ttu-id="f1b9f-445">**KeyType – &lt;KeyType&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-445">**-KeyType &lt;KeyType&gt;**</span></span>

<span data-ttu-id="f1b9f-446">Megadja a media Services kulcs típusát.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-446">Specifies the key type of the media service.</span></span>

* <span data-ttu-id="f1b9f-447">Elsődleges vagy másodlagos</span><span class="sxs-lookup"><span data-stu-id="f1b9f-447">Primary or Secondary</span></span>

| <span data-ttu-id="f1b9f-448">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-448">Aliases</span></span> | <span data-ttu-id="f1b9f-449">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-449">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-450">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-450">Required?</span></span> |<span data-ttu-id="f1b9f-451">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-451">true</span></span> |
| <span data-ttu-id="f1b9f-452">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-452">Position?</span></span> |<span data-ttu-id="f1b9f-453">2</span><span class="sxs-lookup"><span data-stu-id="f1b9f-453">2</span></span> |
| <span data-ttu-id="f1b9f-454">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-454">Default value</span></span> |<span data-ttu-id="f1b9f-455">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-455">none</span></span> |
| <span data-ttu-id="f1b9f-456">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-456">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-457">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-457">false</span></span> |
| <span data-ttu-id="f1b9f-458">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-458">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-459">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-459">false</span></span> |

<span data-ttu-id="f1b9f-460">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-460">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="f1b9f-461">Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-461">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="f1b9f-462">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-462">Inputs</span></span>
<span data-ttu-id="f1b9f-463">A bemeneti típus a parancsmagnak átadható objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-463">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="f1b9f-464">kimenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-464">Outputs</span></span>
<span data-ttu-id="f1b9f-465">A kimeneti típus a parancsmag által létrehozott objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-465">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="sync-azurermmediaservicestoragekeys"></a><span data-ttu-id="f1b9f-466">Sync-AzureRmMediaServiceStorageKeys</span><span class="sxs-lookup"><span data-stu-id="f1b9f-466">Sync-AzureRmMediaServiceStorageKeys</span></span>
<span data-ttu-id="f1b9f-467">A media Services társított storage-fiók tárfiókkulcsainak szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-467">Synchronizes storage account keys for a storage account associated with the media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="f1b9f-468">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-468">Syntax</span></span>
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="f1b9f-469">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-469">Parameters</span></span>
<span data-ttu-id="f1b9f-470">**-ResourceGroupName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-470">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-471">Megadja az erőforráscsoporthoz, amelyhez a media Services tartozik nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-471">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="f1b9f-472">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-472">Aliases</span></span> | <span data-ttu-id="f1b9f-473">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-473">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-474">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-474">Required?</span></span> |<span data-ttu-id="f1b9f-475">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-475">true</span></span> |
| <span data-ttu-id="f1b9f-476">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-476">Position?</span></span> |<span data-ttu-id="f1b9f-477">0</span><span class="sxs-lookup"><span data-stu-id="f1b9f-477">0</span></span> |
| <span data-ttu-id="f1b9f-478">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-478">Default value</span></span> |<span data-ttu-id="f1b9f-479">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-479">none</span></span> |
| <span data-ttu-id="f1b9f-480">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-480">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-481">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-481">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-482">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-482">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-483">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-483">false</span></span> |

<span data-ttu-id="f1b9f-484">**-AccountName &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-484">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-485">Megadja a media Services nevét.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-485">Specifies the name of the media service.</span></span>

| <span data-ttu-id="f1b9f-486">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-486">Aliases</span></span> | <span data-ttu-id="f1b9f-487">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-487">none</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-488">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-488">Required?</span></span> |<span data-ttu-id="f1b9f-489">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-489">true</span></span> |
| <span data-ttu-id="f1b9f-490">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-490">Position?</span></span> |<span data-ttu-id="f1b9f-491">1</span><span class="sxs-lookup"><span data-stu-id="f1b9f-491">1</span></span> |
| <span data-ttu-id="f1b9f-492">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-492">Default value</span></span> |<span data-ttu-id="f1b9f-493">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-493">none</span></span> |
| <span data-ttu-id="f1b9f-494">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-494">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-495">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-495">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-496">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-496">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-497">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-497">false</span></span> |

<span data-ttu-id="f1b9f-498">**-StorageAccountId &lt;karakterlánc&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-498">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="f1b9f-499">Adja meg a tárfiókot, a media Services társított.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-499">Specifies the storage account associated with the media service.</span></span>

| <span data-ttu-id="f1b9f-500">Aliasok</span><span class="sxs-lookup"><span data-stu-id="f1b9f-500">Aliases</span></span> | <span data-ttu-id="f1b9f-501">Azonosító</span><span class="sxs-lookup"><span data-stu-id="f1b9f-501">Id</span></span> |
| --- | --- |
| <span data-ttu-id="f1b9f-502">Kötelező megadni?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-502">Required?</span></span> |<span data-ttu-id="f1b9f-503">Igaz</span><span class="sxs-lookup"><span data-stu-id="f1b9f-503">true</span></span> |
| <span data-ttu-id="f1b9f-504">Hová kerüljön?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-504">Position?</span></span> |<span data-ttu-id="f1b9f-505">2</span><span class="sxs-lookup"><span data-stu-id="f1b9f-505">2</span></span> |
| <span data-ttu-id="f1b9f-506">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1b9f-506">Default value</span></span> |<span data-ttu-id="f1b9f-507">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="f1b9f-507">none</span></span> |
| <span data-ttu-id="f1b9f-508">Fogadja el a feldolgozási sor beviteli?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-508">Accept pipeline input?</span></span> |<span data-ttu-id="f1b9f-509">TRUE(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="f1b9f-509">true(ByPropertyName)</span></span> |
| <span data-ttu-id="f1b9f-510">Helyettesítő karakterek elfogadása?</span><span class="sxs-lookup"><span data-stu-id="f1b9f-510">Accept wildcard characters?</span></span> |<span data-ttu-id="f1b9f-511">hamis</span><span class="sxs-lookup"><span data-stu-id="f1b9f-511">false</span></span> |

<span data-ttu-id="f1b9f-512">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1b9f-512">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="f1b9f-513">Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-513">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="f1b9f-514">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-514">Inputs</span></span>
<span data-ttu-id="f1b9f-515">A bemeneti típus a parancsmagnak átadható objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-515">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="f1b9f-516">kimenetek</span><span class="sxs-lookup"><span data-stu-id="f1b9f-516">Outputs</span></span>
<span data-ttu-id="f1b9f-517">A kimeneti típus a parancsmag által létrehozott objektumok típusa.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-517">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="next-step"></a><span data-ttu-id="f1b9f-518">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="f1b9f-518">Next step</span></span>
<span data-ttu-id="f1b9f-519">Tekintse meg a Media Services tanulási útvonalai.</span><span class="sxs-lookup"><span data-stu-id="f1b9f-519">Check out Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f1b9f-520">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="f1b9f-520">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

