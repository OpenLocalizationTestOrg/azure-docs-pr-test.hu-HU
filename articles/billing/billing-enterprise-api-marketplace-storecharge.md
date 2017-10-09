---
title: "aaaAzure számlázási vállalati API - piactér díjak |} Microsoft Docs"
description: "További információk a hello Reporting API-k, amelyek lehetővé teszik a vállalati Azure ügyfelek toopull adatokkal programozott módon."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: cdf2836b52df06a4bf5ed71a476fe33662c5363c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a><span data-ttu-id="7d4af-103">A vállalati ügyfelek - piactér tároló kell fizetni jelentéskészítési API-k</span><span class="sxs-lookup"><span data-stu-id="7d4af-103">Reporting APIs for Enterprise customers - Marketplace Store Charge</span></span>

<span data-ttu-id="7d4af-104">hello piactér tároló ingyenesen elérhető API értéket ad vissza hello piactér használat alapú költségek bontás hello nap megadott számlázási időszak vagy kezdő és befejező dátumok (egyszer díjak nem szerepelnek).</span><span class="sxs-lookup"><span data-stu-id="7d4af-104">hello Marketplace Store Charge API returns hello usage-based marketplace charges breakdown by day for hello specified Billing Period or start and end dates (one time fees are not included).</span></span>

##<a name="request"></a><span data-ttu-id="7d4af-105">Kérés</span><span class="sxs-lookup"><span data-stu-id="7d4af-105">Request</span></span> 
<span data-ttu-id="7d4af-106">Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="7d4af-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="7d4af-107">Ha nincs megadva a számlázott időszak, majd a hello aktuális elszámolási időszak adat.</span><span class="sxs-lookup"><span data-stu-id="7d4af-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span> <span data-ttu-id="7d4af-108">Egyéni időtartományhoz hello megkezdve adható meg, és dátumparaméterekkel, amelyek hello formátum éééé-hh-nn hello maximális támogatott időben tartománya 36 hónap végén.</span><span class="sxs-lookup"><span data-stu-id="7d4af-108">Custom time ranges can be specified with hello start and end date parameters that are in hello format yyyy-MM-dd, hello maximum supported time range is 36 months.</span></span>  

|<span data-ttu-id="7d4af-109">Módszer</span><span class="sxs-lookup"><span data-stu-id="7d4af-109">Method</span></span> | <span data-ttu-id="7d4af-110">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="7d4af-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="7d4af-111">GET</span><span class="sxs-lookup"><span data-stu-id="7d4af-111">GET</span></span>|<span data-ttu-id="7d4af-112">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="7d4af-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacecharges</span></span>|
|<span data-ttu-id="7d4af-113">GET</span><span class="sxs-lookup"><span data-stu-id="7d4af-113">GET</span></span>|<span data-ttu-id="7d4af-114">{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="7d4af-114">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/marketplacecharges</span></span>|
|<span data-ttu-id="7d4af-115">GET</span><span class="sxs-lookup"><span data-stu-id="7d4af-115">GET</span></span>|<span data-ttu-id="7d4af-116">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10</span><span class="sxs-lookup"><span data-stu-id="7d4af-116">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacechargesbycustomdate?startTime=2017-01-01&endTime=2017-01-10</span></span>|

> [!Note]
> <span data-ttu-id="7d4af-117">toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.</span><span class="sxs-lookup"><span data-stu-id="7d4af-117">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="7d4af-118">Válasz</span><span class="sxs-lookup"><span data-stu-id="7d4af-118">Response</span></span>
 
    
        [
            {
                "id": "id",
                "subscriptionGuid": "00000000-0000-0000-0000-000000000000",
                "subscriptionName": "subName",
                "meterId": "2core",
                "usageStartDate": "2015-09-17T00:00:00Z",
                "usageEndDate": "2015-09-17T23:59:59Z",
                "offerName": "Virtual LoadMaster™ (VLM) for Azure",
                "resourceGroup": "Res group",
                "instanceId": "id",
                "additionalInfo": "{\"ImageType\":null,\"ServiceType\":\"Medium\"}",
                "tags": "",
                "orderNumber": "order",
                "unitOfMeasure": "",
                "costCenter": "100",
                "accountId": 100,
                "accountName": "Account Name",
                "accountOwnerId": "account@live.com",
                "departmentId": 101,
                "departmentName": "Department 1",
                "publisherName": "Publisher 1",
                "planName": "Plan name",
                "consumedQuantity": 1.15,
                "resourceRate": 0.1,
                "extendedCost": 1.11
            },
            ...
        ]
    

<span data-ttu-id="7d4af-119">**Válasz erőforrástulajdonság-meghatározások**</span><span class="sxs-lookup"><span data-stu-id="7d4af-119">**Response property definitions**</span></span>

|<span data-ttu-id="7d4af-120">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="7d4af-120">Property Name</span></span>| <span data-ttu-id="7d4af-121">Típus</span><span class="sxs-lookup"><span data-stu-id="7d4af-121">Type</span></span>| <span data-ttu-id="7d4af-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="7d4af-122">Description</span></span>
|-|-|-|
|<span data-ttu-id="7d4af-123">id</span><span class="sxs-lookup"><span data-stu-id="7d4af-123">id</span></span>|<span data-ttu-id="7d4af-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-124">string</span></span>|<span data-ttu-id="7d4af-125">Hello piactér kell fizetni elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="7d4af-125">Unique Id for hello marketplace charge item</span></span>|
|<span data-ttu-id="7d4af-126">subscriptionGuid</span><span class="sxs-lookup"><span data-stu-id="7d4af-126">subscriptionGuid</span></span>|<span data-ttu-id="7d4af-127">GUID</span><span class="sxs-lookup"><span data-stu-id="7d4af-127">Guid</span></span>|<span data-ttu-id="7d4af-128">hello előfizetés Guid</span><span class="sxs-lookup"><span data-stu-id="7d4af-128">hello Subscription Guid</span></span>|
|<span data-ttu-id="7d4af-129">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="7d4af-129">subscriptionName</span></span>|<span data-ttu-id="7d4af-130">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-130">string</span></span>|<span data-ttu-id="7d4af-131">hello előfizetés neve</span><span class="sxs-lookup"><span data-stu-id="7d4af-131">hello Subscription Name</span></span>|
|<span data-ttu-id="7d4af-132">meterId</span><span class="sxs-lookup"><span data-stu-id="7d4af-132">meterId</span></span>|<span data-ttu-id="7d4af-133">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-133">string</span></span>|<span data-ttu-id="7d4af-134">A kibocsátott mérő hello azonosítója</span><span class="sxs-lookup"><span data-stu-id="7d4af-134">Id for hello emitted Meter</span></span>|
|<span data-ttu-id="7d4af-135">usageStartDate</span><span class="sxs-lookup"><span data-stu-id="7d4af-135">usageStartDate</span></span>|<span data-ttu-id="7d4af-136">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="7d4af-136">DateTime</span></span>|<span data-ttu-id="7d4af-137">Hello használati rekord kezdési időpontja</span><span class="sxs-lookup"><span data-stu-id="7d4af-137">Start time for hello usage record</span></span>|
|<span data-ttu-id="7d4af-138">usageEndDate</span><span class="sxs-lookup"><span data-stu-id="7d4af-138">usageEndDate</span></span>|<span data-ttu-id="7d4af-139">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="7d4af-139">DateTime</span></span>|<span data-ttu-id="7d4af-140">Hello használati rekord befejezési időpontja</span><span class="sxs-lookup"><span data-stu-id="7d4af-140">End time for hello usage record</span></span>|
|<span data-ttu-id="7d4af-141">offerName</span><span class="sxs-lookup"><span data-stu-id="7d4af-141">offerName</span></span>|<span data-ttu-id="7d4af-142">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-142">string</span></span>|<span data-ttu-id="7d4af-143">hello ajánlat neve</span><span class="sxs-lookup"><span data-stu-id="7d4af-143">hello Offer name</span></span>|
|<span data-ttu-id="7d4af-144">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="7d4af-144">resourceGroup</span></span>|<span data-ttu-id="7d4af-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-145">string</span></span>|<span data-ttu-id="7d4af-146">hello erőforrás csoport</span><span class="sxs-lookup"><span data-stu-id="7d4af-146">hello resource Group</span></span>|
|<span data-ttu-id="7d4af-147">instanceId</span><span class="sxs-lookup"><span data-stu-id="7d4af-147">instanceId</span></span>|<span data-ttu-id="7d4af-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-148">string</span></span>|<span data-ttu-id="7d4af-149">Példányazonosító</span><span class="sxs-lookup"><span data-stu-id="7d4af-149">Instance Id</span></span>|
|<span data-ttu-id="7d4af-150">additionalinfo részben</span><span class="sxs-lookup"><span data-stu-id="7d4af-150">additionalInfo</span></span>|<span data-ttu-id="7d4af-151">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-151">string</span></span>|<span data-ttu-id="7d4af-152">További információ a JSON-karakterláncban</span><span class="sxs-lookup"><span data-stu-id="7d4af-152">Additional info JSON string</span></span>|
|<span data-ttu-id="7d4af-153">tags</span><span class="sxs-lookup"><span data-stu-id="7d4af-153">tags</span></span>|<span data-ttu-id="7d4af-154">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-154">string</span></span>|<span data-ttu-id="7d4af-155">Címke JSON-karakterláncban</span><span class="sxs-lookup"><span data-stu-id="7d4af-155">Tag JSON string</span></span>|
|<span data-ttu-id="7d4af-156">orderNumber</span><span class="sxs-lookup"><span data-stu-id="7d4af-156">orderNumber</span></span>|<span data-ttu-id="7d4af-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-157">string</span></span>|<span data-ttu-id="7d4af-158">hello sorszámú</span><span class="sxs-lookup"><span data-stu-id="7d4af-158">hello order number</span></span>|
|<span data-ttu-id="7d4af-159">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="7d4af-159">unitOfMeasure</span></span>|<span data-ttu-id="7d4af-160">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-160">string</span></span>|<span data-ttu-id="7d4af-161">Hello mérő mértékegységét</span><span class="sxs-lookup"><span data-stu-id="7d4af-161">Unit of measure for hello meter</span></span>|
|<span data-ttu-id="7d4af-162">CostCenter</span><span class="sxs-lookup"><span data-stu-id="7d4af-162">costCenter</span></span>|<span data-ttu-id="7d4af-163">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-163">string</span></span>|<span data-ttu-id="7d4af-164">költség hello center</span><span class="sxs-lookup"><span data-stu-id="7d4af-164">hello cost center</span></span>|
|<span data-ttu-id="7d4af-165">accountId</span><span class="sxs-lookup"><span data-stu-id="7d4af-165">accountId</span></span>|<span data-ttu-id="7d4af-166">int</span><span class="sxs-lookup"><span data-stu-id="7d4af-166">int</span></span>|<span data-ttu-id="7d4af-167">hello fiók azonosítója</span><span class="sxs-lookup"><span data-stu-id="7d4af-167">hello account Id</span></span>|
|<span data-ttu-id="7d4af-168">Fióknév</span><span class="sxs-lookup"><span data-stu-id="7d4af-168">accountName</span></span>|<span data-ttu-id="7d4af-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-169">string</span></span> |<span data-ttu-id="7d4af-170">hello fiók neve</span><span class="sxs-lookup"><span data-stu-id="7d4af-170">hello Account Name</span></span>|
|<span data-ttu-id="7d4af-171">accountOwnerId</span><span class="sxs-lookup"><span data-stu-id="7d4af-171">accountOwnerId</span></span>|<span data-ttu-id="7d4af-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-172">string</span></span>|<span data-ttu-id="7d4af-173">hello fiókazonosító tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="7d4af-173">hello Account Owner Id</span></span>|
|<span data-ttu-id="7d4af-174">departmentId</span><span class="sxs-lookup"><span data-stu-id="7d4af-174">departmentId</span></span>|<span data-ttu-id="7d4af-175">int</span><span class="sxs-lookup"><span data-stu-id="7d4af-175">int</span></span>|<span data-ttu-id="7d4af-176">hello részleg azonosítója</span><span class="sxs-lookup"><span data-stu-id="7d4af-176">hello department Id</span></span>|
|<span data-ttu-id="7d4af-177">departmentname nevű</span><span class="sxs-lookup"><span data-stu-id="7d4af-177">departmentName</span></span>|<span data-ttu-id="7d4af-178">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-178">string</span></span>|<span data-ttu-id="7d4af-179">hello osztály neve</span><span class="sxs-lookup"><span data-stu-id="7d4af-179">hello department name</span></span>|
|<span data-ttu-id="7d4af-180">publisherName</span><span class="sxs-lookup"><span data-stu-id="7d4af-180">publisherName</span></span>|<span data-ttu-id="7d4af-181">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-181">string</span></span>|<span data-ttu-id="7d4af-182">hello közzétevő neve</span><span class="sxs-lookup"><span data-stu-id="7d4af-182">hello publisher name</span></span>|
|<span data-ttu-id="7d4af-183">planName</span><span class="sxs-lookup"><span data-stu-id="7d4af-183">planName</span></span>|<span data-ttu-id="7d4af-184">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d4af-184">string</span></span>|<span data-ttu-id="7d4af-185">hello csomag neve</span><span class="sxs-lookup"><span data-stu-id="7d4af-185">hello Plan name</span></span>|
|<span data-ttu-id="7d4af-186">consumedQuantity</span><span class="sxs-lookup"><span data-stu-id="7d4af-186">consumedQuantity</span></span>|<span data-ttu-id="7d4af-187">Decimális</span><span class="sxs-lookup"><span data-stu-id="7d4af-187">decimal</span></span>|<span data-ttu-id="7d4af-188">Ezen időszak alatt felhasznált mennyiség</span><span class="sxs-lookup"><span data-stu-id="7d4af-188">Consumed Quantity during this time period</span></span>|
|<span data-ttu-id="7d4af-189">resourceRate</span><span class="sxs-lookup"><span data-stu-id="7d4af-189">resourceRate</span></span>|<span data-ttu-id="7d4af-190">Decimális</span><span class="sxs-lookup"><span data-stu-id="7d4af-190">decimal</span></span>|<span data-ttu-id="7d4af-191">Hello mérő Egységár</span><span class="sxs-lookup"><span data-stu-id="7d4af-191">Unit price for hello meter</span></span>|
|<span data-ttu-id="7d4af-192">extendedCost</span><span class="sxs-lookup"><span data-stu-id="7d4af-192">extendedCost</span></span>|<span data-ttu-id="7d4af-193">Decimális</span><span class="sxs-lookup"><span data-stu-id="7d4af-193">decimal</span></span>|<span data-ttu-id="7d4af-194">Becsült költség felhasznált mennyiség és kiterjesztett alapján</span><span class="sxs-lookup"><span data-stu-id="7d4af-194">Estimated charge based on Consumed Quantity and Extended cost</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="7d4af-195">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7d4af-195">See also</span></span>

* [<span data-ttu-id="7d4af-196">Elszámolási időszakok API</span><span class="sxs-lookup"><span data-stu-id="7d4af-196">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="7d4af-197">Használat részletei API</span><span class="sxs-lookup"><span data-stu-id="7d4af-197">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="7d4af-198">Egyenleg és összegzés API</span><span class="sxs-lookup"><span data-stu-id="7d4af-198">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="7d4af-199">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="7d4af-199">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)