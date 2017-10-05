---
title: "Azure számlázási vállalati API - piactér díjak |} Microsoft Docs"
description: "További tudnivalók a Reporting API-k, amelyek lehetővé teszik a vállalati Azure ügyfelek való lekérésére programozott módon fogyasztási adatokhoz."
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
ms.openlocfilehash: 5539623f7ae35e14b6dafe6fdf9efe4bcaba4fd3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a><span data-ttu-id="47901-103">A vállalati ügyfelek - piactér tároló kell fizetni jelentéskészítési API-k</span><span class="sxs-lookup"><span data-stu-id="47901-103">Reporting APIs for Enterprise customers - Marketplace Store Charge</span></span>

<span data-ttu-id="47901-104">A piactér tároló ingyenesen elérhető API piactér használat alapú költségek bontásban tartalmazza a megadott számlázási időszak vagy a kezdő és befejező dátumok (egyszer díjak nincsenek) napi adja vissza.</span><span class="sxs-lookup"><span data-stu-id="47901-104">The Marketplace Store Charge API returns the usage-based marketplace charges breakdown by day for the specified Billing Period or start and end dates (one time fees are not included).</span></span>

##<a name="request"></a><span data-ttu-id="47901-105">Kérés</span><span class="sxs-lookup"><span data-stu-id="47901-105">Request</span></span> 
<span data-ttu-id="47901-106">Hozzá kell adni közös fejléc tulajdonságokhoz megadott [Itt](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="47901-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="47901-107">Ha a számlázott időszak nincs megadva, majd az aktuális elszámolási időszak adat.</span><span class="sxs-lookup"><span data-stu-id="47901-107">If a billing period is not specified, then data for the current billing period is returned.</span></span> <span data-ttu-id="47901-108">Egyéni időtartományhoz megadhatók a kezdő és befejező dátum paraméterek éééé-hh-nn formátum a, a maximális támogatott időtartomány 36 hónap.</span><span class="sxs-lookup"><span data-stu-id="47901-108">Custom time ranges can be specified with the start and end date parameters that are in the format yyyy-MM-dd, the maximum supported time range is 36 months.</span></span>  

|<span data-ttu-id="47901-109">Módszer</span><span class="sxs-lookup"><span data-stu-id="47901-109">Method</span></span> | <span data-ttu-id="47901-110">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="47901-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="47901-111">GET</span><span class="sxs-lookup"><span data-stu-id="47901-111">GET</span></span>|<span data-ttu-id="47901-112">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="47901-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacecharges</span></span>|
|<span data-ttu-id="47901-113">GET</span><span class="sxs-lookup"><span data-stu-id="47901-113">GET</span></span>|<span data-ttu-id="47901-114">{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="47901-114">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/marketplacecharges</span></span>|
|<span data-ttu-id="47901-115">GET</span><span class="sxs-lookup"><span data-stu-id="47901-115">GET</span></span>|<span data-ttu-id="47901-116">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10</span><span class="sxs-lookup"><span data-stu-id="47901-116">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacechargesbycustomdate?startTime=2017-01-01&endTime=2017-01-10</span></span>|

> [!Note]
> <span data-ttu-id="47901-117">API előzetes verzióját használja, a fenti URL-címben v1 v2 cserélje.</span><span class="sxs-lookup"><span data-stu-id="47901-117">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="47901-118">Válasz</span><span class="sxs-lookup"><span data-stu-id="47901-118">Response</span></span>
 
    
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
    

<span data-ttu-id="47901-119">**Válasz erőforrástulajdonság-meghatározások**</span><span class="sxs-lookup"><span data-stu-id="47901-119">**Response property definitions**</span></span>

|<span data-ttu-id="47901-120">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="47901-120">Property Name</span></span>| <span data-ttu-id="47901-121">Típus</span><span class="sxs-lookup"><span data-stu-id="47901-121">Type</span></span>| <span data-ttu-id="47901-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="47901-122">Description</span></span>
|-|-|-|
|<span data-ttu-id="47901-123">id</span><span class="sxs-lookup"><span data-stu-id="47901-123">id</span></span>|<span data-ttu-id="47901-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-124">string</span></span>|<span data-ttu-id="47901-125">A piactér kell fizetni elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="47901-125">Unique Id for the marketplace charge item</span></span>|
|<span data-ttu-id="47901-126">subscriptionGuid</span><span class="sxs-lookup"><span data-stu-id="47901-126">subscriptionGuid</span></span>|<span data-ttu-id="47901-127">GUID</span><span class="sxs-lookup"><span data-stu-id="47901-127">Guid</span></span>|<span data-ttu-id="47901-128">Az előfizetés Guid</span><span class="sxs-lookup"><span data-stu-id="47901-128">The Subscription Guid</span></span>|
|<span data-ttu-id="47901-129">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="47901-129">subscriptionName</span></span>|<span data-ttu-id="47901-130">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-130">string</span></span>|<span data-ttu-id="47901-131">Az előfizetés neve</span><span class="sxs-lookup"><span data-stu-id="47901-131">The Subscription Name</span></span>|
|<span data-ttu-id="47901-132">meterId</span><span class="sxs-lookup"><span data-stu-id="47901-132">meterId</span></span>|<span data-ttu-id="47901-133">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-133">string</span></span>|<span data-ttu-id="47901-134">A kibocsátott mérő azonosítója</span><span class="sxs-lookup"><span data-stu-id="47901-134">Id for the emitted Meter</span></span>|
|<span data-ttu-id="47901-135">usageStartDate</span><span class="sxs-lookup"><span data-stu-id="47901-135">usageStartDate</span></span>|<span data-ttu-id="47901-136">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="47901-136">DateTime</span></span>|<span data-ttu-id="47901-137">Indítsa el a használati rekord</span><span class="sxs-lookup"><span data-stu-id="47901-137">Start time for the usage record</span></span>|
|<span data-ttu-id="47901-138">usageEndDate</span><span class="sxs-lookup"><span data-stu-id="47901-138">usageEndDate</span></span>|<span data-ttu-id="47901-139">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="47901-139">DateTime</span></span>|<span data-ttu-id="47901-140">A használati rekord befejezési időpontja</span><span class="sxs-lookup"><span data-stu-id="47901-140">End time for the usage record</span></span>|
|<span data-ttu-id="47901-141">offerName</span><span class="sxs-lookup"><span data-stu-id="47901-141">offerName</span></span>|<span data-ttu-id="47901-142">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-142">string</span></span>|<span data-ttu-id="47901-143">Az ajánlat neve</span><span class="sxs-lookup"><span data-stu-id="47901-143">The Offer name</span></span>|
|<span data-ttu-id="47901-144">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="47901-144">resourceGroup</span></span>|<span data-ttu-id="47901-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-145">string</span></span>|<span data-ttu-id="47901-146">Az erőforrás csoport</span><span class="sxs-lookup"><span data-stu-id="47901-146">The resource Group</span></span>|
|<span data-ttu-id="47901-147">instanceId</span><span class="sxs-lookup"><span data-stu-id="47901-147">instanceId</span></span>|<span data-ttu-id="47901-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-148">string</span></span>|<span data-ttu-id="47901-149">Példányazonosító</span><span class="sxs-lookup"><span data-stu-id="47901-149">Instance Id</span></span>|
|<span data-ttu-id="47901-150">additionalinfo részben</span><span class="sxs-lookup"><span data-stu-id="47901-150">additionalInfo</span></span>|<span data-ttu-id="47901-151">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-151">string</span></span>|<span data-ttu-id="47901-152">További információ a JSON-karakterláncban</span><span class="sxs-lookup"><span data-stu-id="47901-152">Additional info JSON string</span></span>|
|<span data-ttu-id="47901-153">tags</span><span class="sxs-lookup"><span data-stu-id="47901-153">tags</span></span>|<span data-ttu-id="47901-154">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-154">string</span></span>|<span data-ttu-id="47901-155">Címke JSON-karakterláncban</span><span class="sxs-lookup"><span data-stu-id="47901-155">Tag JSON string</span></span>|
|<span data-ttu-id="47901-156">orderNumber</span><span class="sxs-lookup"><span data-stu-id="47901-156">orderNumber</span></span>|<span data-ttu-id="47901-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-157">string</span></span>|<span data-ttu-id="47901-158">A sorszám</span><span class="sxs-lookup"><span data-stu-id="47901-158">The order number</span></span>|
|<span data-ttu-id="47901-159">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="47901-159">unitOfMeasure</span></span>|<span data-ttu-id="47901-160">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-160">string</span></span>|<span data-ttu-id="47901-161">A mérő mértékegysége</span><span class="sxs-lookup"><span data-stu-id="47901-161">Unit of measure for the meter</span></span>|
|<span data-ttu-id="47901-162">CostCenter</span><span class="sxs-lookup"><span data-stu-id="47901-162">costCenter</span></span>|<span data-ttu-id="47901-163">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-163">string</span></span>|<span data-ttu-id="47901-164">A költségközpont</span><span class="sxs-lookup"><span data-stu-id="47901-164">The cost center</span></span>|
|<span data-ttu-id="47901-165">accountId</span><span class="sxs-lookup"><span data-stu-id="47901-165">accountId</span></span>|<span data-ttu-id="47901-166">int</span><span class="sxs-lookup"><span data-stu-id="47901-166">int</span></span>|<span data-ttu-id="47901-167">A fiók azonosítója</span><span class="sxs-lookup"><span data-stu-id="47901-167">The account Id</span></span>|
|<span data-ttu-id="47901-168">Fióknév</span><span class="sxs-lookup"><span data-stu-id="47901-168">accountName</span></span>|<span data-ttu-id="47901-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-169">string</span></span> |<span data-ttu-id="47901-170">A fiók neve</span><span class="sxs-lookup"><span data-stu-id="47901-170">The Account Name</span></span>|
|<span data-ttu-id="47901-171">accountOwnerId</span><span class="sxs-lookup"><span data-stu-id="47901-171">accountOwnerId</span></span>|<span data-ttu-id="47901-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-172">string</span></span>|<span data-ttu-id="47901-173">A fiók tulajdonosa azonosítója</span><span class="sxs-lookup"><span data-stu-id="47901-173">The Account Owner Id</span></span>|
|<span data-ttu-id="47901-174">departmentId</span><span class="sxs-lookup"><span data-stu-id="47901-174">departmentId</span></span>|<span data-ttu-id="47901-175">int</span><span class="sxs-lookup"><span data-stu-id="47901-175">int</span></span>|<span data-ttu-id="47901-176">A szervezeti egység azonosítója</span><span class="sxs-lookup"><span data-stu-id="47901-176">The department Id</span></span>|
|<span data-ttu-id="47901-177">departmentname nevű</span><span class="sxs-lookup"><span data-stu-id="47901-177">departmentName</span></span>|<span data-ttu-id="47901-178">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-178">string</span></span>|<span data-ttu-id="47901-179">Az osztály neve</span><span class="sxs-lookup"><span data-stu-id="47901-179">The department name</span></span>|
|<span data-ttu-id="47901-180">publisherName</span><span class="sxs-lookup"><span data-stu-id="47901-180">publisherName</span></span>|<span data-ttu-id="47901-181">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-181">string</span></span>|<span data-ttu-id="47901-182">A közzétevő neve</span><span class="sxs-lookup"><span data-stu-id="47901-182">The publisher name</span></span>|
|<span data-ttu-id="47901-183">planName</span><span class="sxs-lookup"><span data-stu-id="47901-183">planName</span></span>|<span data-ttu-id="47901-184">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="47901-184">string</span></span>|<span data-ttu-id="47901-185">A csomag neve</span><span class="sxs-lookup"><span data-stu-id="47901-185">The Plan name</span></span>|
|<span data-ttu-id="47901-186">consumedQuantity</span><span class="sxs-lookup"><span data-stu-id="47901-186">consumedQuantity</span></span>|<span data-ttu-id="47901-187">Decimális</span><span class="sxs-lookup"><span data-stu-id="47901-187">decimal</span></span>|<span data-ttu-id="47901-188">Ezen időszak alatt felhasznált mennyiség</span><span class="sxs-lookup"><span data-stu-id="47901-188">Consumed Quantity during this time period</span></span>|
|<span data-ttu-id="47901-189">resourceRate</span><span class="sxs-lookup"><span data-stu-id="47901-189">resourceRate</span></span>|<span data-ttu-id="47901-190">Decimális</span><span class="sxs-lookup"><span data-stu-id="47901-190">decimal</span></span>|<span data-ttu-id="47901-191">A mérő Egységár</span><span class="sxs-lookup"><span data-stu-id="47901-191">Unit price for the meter</span></span>|
|<span data-ttu-id="47901-192">extendedCost</span><span class="sxs-lookup"><span data-stu-id="47901-192">extendedCost</span></span>|<span data-ttu-id="47901-193">Decimális</span><span class="sxs-lookup"><span data-stu-id="47901-193">decimal</span></span>|<span data-ttu-id="47901-194">Becsült költség felhasznált mennyiség és kiterjesztett alapján</span><span class="sxs-lookup"><span data-stu-id="47901-194">Estimated charge based on Consumed Quantity and Extended cost</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="47901-195">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="47901-195">See also</span></span>

* [<span data-ttu-id="47901-196">Elszámolási időszakok API</span><span class="sxs-lookup"><span data-stu-id="47901-196">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="47901-197">Használat részletei API</span><span class="sxs-lookup"><span data-stu-id="47901-197">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="47901-198">Egyenleg és összegzés API</span><span class="sxs-lookup"><span data-stu-id="47901-198">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="47901-199">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="47901-199">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)