---
title: "aaaAzure számlázási vállalati API - egyenleg és összefoglalása |} Microsoft Docs"
description: "Tudnivalók Azure számlázási használati és RateCard API-k, amelyek az Azure erőforrás-felhasználás és trendek használt tooprovide betekintést."
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
ms.openlocfilehash: b031de2c347e5abeacd11743cc96024434518918
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a><span data-ttu-id="cd495-103">Jelentéskészítési API-k, a vállalati ügyfelek - elosztás és összegzése</span><span class="sxs-lookup"><span data-stu-id="cd495-103">Reporting APIs for Enterprise customers - Balance and Summary</span></span>

<span data-ttu-id="cd495-104">hello egyenleg és összefoglaló API kínál kiegyensúlyozza, új, Azure piactér szolgáltatási díjak, módosítását, és keretét díjak adatainak havi összegzése.</span><span class="sxs-lookup"><span data-stu-id="cd495-104">hello Balance and Summary API offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments, and overage charges.</span></span>


##<a name="request"></a><span data-ttu-id="cd495-105">Kérés</span><span class="sxs-lookup"><span data-stu-id="cd495-105">Request</span></span> 
<span data-ttu-id="cd495-106">Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="cd495-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="cd495-107">Ha nincs megadva a számlázott időszak, majd a hello aktuális elszámolási időszak adat.</span><span class="sxs-lookup"><span data-stu-id="cd495-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span>

|<span data-ttu-id="cd495-108">Módszer</span><span class="sxs-lookup"><span data-stu-id="cd495-108">Method</span></span> | <span data-ttu-id="cd495-109">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="cd495-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="cd495-110">GET</span><span class="sxs-lookup"><span data-stu-id="cd495-110">GET</span></span>| <span data-ttu-id="cd495-111">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / balancesummary</span><span class="sxs-lookup"><span data-stu-id="cd495-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/balancesummary</span></span>|
|<span data-ttu-id="cd495-112">GET</span><span class="sxs-lookup"><span data-stu-id="cd495-112">GET</span></span>| <span data-ttu-id="cd495-113">{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / balancesummary</span><span class="sxs-lookup"><span data-stu-id="cd495-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/balancesummary</span></span>|

> [!Note]
> <span data-ttu-id="cd495-114">toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.</span><span class="sxs-lookup"><span data-stu-id="cd495-114">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="cd495-115">Válasz</span><span class="sxs-lookup"><span data-stu-id="cd495-115">Response</span></span>

        {
            "id": "enrollments/100/billingperiods/201507/balancesummaries",
            "billingPeriodId": 201507,
            "currencyCode": "USD",
            "beginningBalance": 0,
            "endingBalance": 1.1,
            "newPurchases": 1,
            "adjustments": 1.1,
            "utilized": 1.1,
            "serviceOverage": 1,
            "chargesBilledSeparately": 1,
            "totalOverage": 1,
            "totalUsage": 1.1,
            "azureMarketplaceServiceCharges": 1,
            "newPurchasesDetails": [
                {
                "name": "",
                "value": 1
                }
            ],
            "adjustmentDetails": [
                {
                "name": "Promo Credit",
                "value": 1.1
                },
                {
                "name": "SIE Credit",
                "value": 1.0
                }
            ]
        }


<span data-ttu-id="cd495-116">**Válasz erőforrástulajdonság-meghatározások**</span><span class="sxs-lookup"><span data-stu-id="cd495-116">**Response property definitions**</span></span>

|<span data-ttu-id="cd495-117">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="cd495-117">Property Name</span></span>| <span data-ttu-id="cd495-118">Típus</span><span class="sxs-lookup"><span data-stu-id="cd495-118">Type</span></span>| <span data-ttu-id="cd495-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="cd495-119">Description</span></span>
|-|-|-|
|<span data-ttu-id="cd495-120">id</span><span class="sxs-lookup"><span data-stu-id="cd495-120">id</span></span>|<span data-ttu-id="cd495-121">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cd495-121">string</span></span>|<span data-ttu-id="cd495-122">hello egy adott számlázási időszakban és a beléptetési egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="cd495-122">hello unique Id for a specific billing period and enrollment</span></span>|
|<span data-ttu-id="cd495-123">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="cd495-123">billingPeriodId</span></span>|<span data-ttu-id="cd495-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cd495-124">string</span></span> |<span data-ttu-id="cd495-125">hello számlázási időszak-azonosító</span><span class="sxs-lookup"><span data-stu-id="cd495-125">hello billing period Id</span></span>|
|<span data-ttu-id="cd495-126">currencyCode</span><span class="sxs-lookup"><span data-stu-id="cd495-126">currencyCode</span></span>|<span data-ttu-id="cd495-127">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cd495-127">string</span></span> |<span data-ttu-id="cd495-128">hello pénznemkódot</span><span class="sxs-lookup"><span data-stu-id="cd495-128">hello currency code</span></span>|
|<span data-ttu-id="cd495-129">beginningBalance</span><span class="sxs-lookup"><span data-stu-id="cd495-129">beginningBalance</span></span>|<span data-ttu-id="cd495-130">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-130">decimal</span></span>| <span data-ttu-id="cd495-131">hello számlázási időszak kezdete egyenlege hello</span><span class="sxs-lookup"><span data-stu-id="cd495-131">hello beginning balance for hello billing period</span></span>|
|<span data-ttu-id="cd495-132">endingBalance</span><span class="sxs-lookup"><span data-stu-id="cd495-132">endingBalance</span></span>|<span data-ttu-id="cd495-133">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-133">decimal</span></span>| <span data-ttu-id="cd495-134">hello záró egyenleg hello számlázási időszak (időszakokra nyissa meg ezt a rendszer naponta frissíti)</span><span class="sxs-lookup"><span data-stu-id="cd495-134">hello ending balance for hello billing period (for open periods this will be updated daily)</span></span>|
|<span data-ttu-id="cd495-135">newPurchases</span><span class="sxs-lookup"><span data-stu-id="cd495-135">newPurchases</span></span>|<span data-ttu-id="cd495-136">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-136">decimal</span></span>| <span data-ttu-id="cd495-137">Új beszerzési végösszeg</span><span class="sxs-lookup"><span data-stu-id="cd495-137">Total new purchase amount</span></span>|
|<span data-ttu-id="cd495-138">beállításai</span><span class="sxs-lookup"><span data-stu-id="cd495-138">adjustments</span></span>|<span data-ttu-id="cd495-139">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-139">decimal</span></span>| <span data-ttu-id="cd495-140">Teljes módosítás összeg</span><span class="sxs-lookup"><span data-stu-id="cd495-140">Total adjustment amount</span></span>|
|<span data-ttu-id="cd495-141">használata</span><span class="sxs-lookup"><span data-stu-id="cd495-141">utilized</span></span>|<span data-ttu-id="cd495-142">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-142">decimal</span></span>| <span data-ttu-id="cd495-143">Teljes kötelezettségvállalás használata</span><span class="sxs-lookup"><span data-stu-id="cd495-143">Total Commitment usage</span></span>|
|<span data-ttu-id="cd495-144">serviceOverage</span><span class="sxs-lookup"><span data-stu-id="cd495-144">serviceOverage</span></span>|<span data-ttu-id="cd495-145">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-145">decimal</span></span>| <span data-ttu-id="cd495-146">Az Azure szolgáltatások túlhasználati</span><span class="sxs-lookup"><span data-stu-id="cd495-146">Overage for Azure services</span></span>|
|<span data-ttu-id="cd495-147">chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="cd495-147">chargesBilledSeparately</span></span>|<span data-ttu-id="cd495-148">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-148">decimal</span></span>| <span data-ttu-id="cd495-149">Külön számlázva díjak</span><span class="sxs-lookup"><span data-stu-id="cd495-149">Charges Billed separately</span></span>|
|<span data-ttu-id="cd495-150">totalOverage</span><span class="sxs-lookup"><span data-stu-id="cd495-150">totalOverage</span></span>|<span data-ttu-id="cd495-151">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-151">decimal</span></span>| <span data-ttu-id="cd495-152">serviceOverage + chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="cd495-152">serviceOverage + chargesBilledSeparately</span></span>|
|<span data-ttu-id="cd495-153">totalUsage</span><span class="sxs-lookup"><span data-stu-id="cd495-153">totalUsage</span></span>|<span data-ttu-id="cd495-154">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-154">decimal</span></span>| <span data-ttu-id="cd495-155">Azure-szolgáltatások kötelezettségvállalás + többlete</span><span class="sxs-lookup"><span data-stu-id="cd495-155">Azure service commitment + total Overage</span></span>|
|<span data-ttu-id="cd495-156">azureMarketplaceServiceCharges</span><span class="sxs-lookup"><span data-stu-id="cd495-156">azureMarketplaceServiceCharges</span></span>|<span data-ttu-id="cd495-157">Decimális</span><span class="sxs-lookup"><span data-stu-id="cd495-157">decimal</span></span>| <span data-ttu-id="cd495-158">Teljes költségek az Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="cd495-158">Total charges for Azure Marketplace</span></span>|
|<span data-ttu-id="cd495-159">newPurchasesDetails</span><span class="sxs-lookup"><span data-stu-id="cd495-159">newPurchasesDetails</span></span>|<span data-ttu-id="cd495-160">A név-érték pár karakterlánc-tömbben JSON</span><span class="sxs-lookup"><span data-stu-id="cd495-160">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="cd495-161">Új vásárlás listája</span><span class="sxs-lookup"><span data-stu-id="cd495-161">List of new purchases</span></span>|
|<span data-ttu-id="cd495-162">adjustmentDetails</span><span class="sxs-lookup"><span data-stu-id="cd495-162">adjustmentDetails</span></span>|<span data-ttu-id="cd495-163">A név-érték pár karakterlánc-tömbben JSON</span><span class="sxs-lookup"><span data-stu-id="cd495-163">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="cd495-164">(Az előléptetés jóváírás, SIE jóváírás stb.) beállításainak listája</span><span class="sxs-lookup"><span data-stu-id="cd495-164">List of Adjustments (Promo credit, SIE credit etc.)</span></span> |


<br/>
## <a name="see-also"></a><span data-ttu-id="cd495-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="cd495-165">See also</span></span>

* [<span data-ttu-id="cd495-166">Elszámolási időszakok API</span><span class="sxs-lookup"><span data-stu-id="cd495-166">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="cd495-167">Használat részletei API</span><span class="sxs-lookup"><span data-stu-id="cd495-167">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="cd495-168">Piactér-tároló ingyenesen elérhető API</span><span class="sxs-lookup"><span data-stu-id="cd495-168">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="cd495-169">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="cd495-169">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)