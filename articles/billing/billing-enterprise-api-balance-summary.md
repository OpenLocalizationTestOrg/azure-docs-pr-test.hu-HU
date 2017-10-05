---
title: "Azure számlázás vállalati API - elosztás és összegzése |} Microsoft Docs"
description: "Tudnivalók Azure számlázási használati és RateCard API-k, amely biztosítja a trendeket és az Azure erőforrás-felhasználás."
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
ms.openlocfilehash: f6b149f0e656d2263705048aa5b644f5bb4a5712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a><span data-ttu-id="e2e0d-103">Jelentéskészítési API-k, a vállalati ügyfelek - elosztás és összegzése</span><span class="sxs-lookup"><span data-stu-id="e2e0d-103">Reporting APIs for Enterprise customers - Balance and Summary</span></span>

<span data-ttu-id="e2e0d-104">Az egyenleg és összefoglaló API kínál kiegyensúlyozza, új, Azure piactér szolgáltatási díjak, módosítását, és keretét díjak adatainak havi összegzése.</span><span class="sxs-lookup"><span data-stu-id="e2e0d-104">The Balance and Summary API offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments, and overage charges.</span></span>


##<a name="request"></a><span data-ttu-id="e2e0d-105">Kérés</span><span class="sxs-lookup"><span data-stu-id="e2e0d-105">Request</span></span> 
<span data-ttu-id="e2e0d-106">Hozzá kell adni közös fejléc tulajdonságokhoz megadott [Itt](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="e2e0d-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="e2e0d-107">Ha a számlázott időszak nincs megadva, majd az aktuális elszámolási időszak adat.</span><span class="sxs-lookup"><span data-stu-id="e2e0d-107">If a billing period is not specified, then data for the current billing period is returned.</span></span>

|<span data-ttu-id="e2e0d-108">Módszer</span><span class="sxs-lookup"><span data-stu-id="e2e0d-108">Method</span></span> | <span data-ttu-id="e2e0d-109">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="e2e0d-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="e2e0d-110">GET</span><span class="sxs-lookup"><span data-stu-id="e2e0d-110">GET</span></span>| <span data-ttu-id="e2e0d-111">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / balancesummary</span><span class="sxs-lookup"><span data-stu-id="e2e0d-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/balancesummary</span></span>|
|<span data-ttu-id="e2e0d-112">GET</span><span class="sxs-lookup"><span data-stu-id="e2e0d-112">GET</span></span>| <span data-ttu-id="e2e0d-113">{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / balancesummary</span><span class="sxs-lookup"><span data-stu-id="e2e0d-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/balancesummary</span></span>|

> [!Note]
> <span data-ttu-id="e2e0d-114">API előzetes verzióját használja, a fenti URL-címben v1 v2 cserélje.</span><span class="sxs-lookup"><span data-stu-id="e2e0d-114">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="e2e0d-115">Válasz</span><span class="sxs-lookup"><span data-stu-id="e2e0d-115">Response</span></span>

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


<span data-ttu-id="e2e0d-116">**Válasz erőforrástulajdonság-meghatározások**</span><span class="sxs-lookup"><span data-stu-id="e2e0d-116">**Response property definitions**</span></span>

|<span data-ttu-id="e2e0d-117">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="e2e0d-117">Property Name</span></span>| <span data-ttu-id="e2e0d-118">Típus</span><span class="sxs-lookup"><span data-stu-id="e2e0d-118">Type</span></span>| <span data-ttu-id="e2e0d-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="e2e0d-119">Description</span></span>
|-|-|-|
|<span data-ttu-id="e2e0d-120">id</span><span class="sxs-lookup"><span data-stu-id="e2e0d-120">id</span></span>|<span data-ttu-id="e2e0d-121">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2e0d-121">string</span></span>|<span data-ttu-id="e2e0d-122">Egy adott számlázási időszakban és a beléptetési egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="e2e0d-122">The unique Id for a specific billing period and enrollment</span></span>|
|<span data-ttu-id="e2e0d-123">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="e2e0d-123">billingPeriodId</span></span>|<span data-ttu-id="e2e0d-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2e0d-124">string</span></span> |<span data-ttu-id="e2e0d-125">A számlázott időszak azonosítója</span><span class="sxs-lookup"><span data-stu-id="e2e0d-125">The billing period Id</span></span>|
|<span data-ttu-id="e2e0d-126">currencyCode</span><span class="sxs-lookup"><span data-stu-id="e2e0d-126">currencyCode</span></span>|<span data-ttu-id="e2e0d-127">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2e0d-127">string</span></span> |<span data-ttu-id="e2e0d-128">A pénznemkódot</span><span class="sxs-lookup"><span data-stu-id="e2e0d-128">The currency code</span></span>|
|<span data-ttu-id="e2e0d-129">beginningBalance</span><span class="sxs-lookup"><span data-stu-id="e2e0d-129">beginningBalance</span></span>|<span data-ttu-id="e2e0d-130">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-130">decimal</span></span>| <span data-ttu-id="e2e0d-131">A számlázott időszak kezdete egyenlege</span><span class="sxs-lookup"><span data-stu-id="e2e0d-131">The beginning balance for the billing period</span></span>|
|<span data-ttu-id="e2e0d-132">endingBalance</span><span class="sxs-lookup"><span data-stu-id="e2e0d-132">endingBalance</span></span>|<span data-ttu-id="e2e0d-133">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-133">decimal</span></span>| <span data-ttu-id="e2e0d-134">A befejezési elosztás a számlázott időszak (időszakokra nyissa meg ezt a rendszer naponta frissíti)</span><span class="sxs-lookup"><span data-stu-id="e2e0d-134">The ending balance for the billing period (for open periods this will be updated daily)</span></span>|
|<span data-ttu-id="e2e0d-135">newPurchases</span><span class="sxs-lookup"><span data-stu-id="e2e0d-135">newPurchases</span></span>|<span data-ttu-id="e2e0d-136">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-136">decimal</span></span>| <span data-ttu-id="e2e0d-137">Új beszerzési végösszeg</span><span class="sxs-lookup"><span data-stu-id="e2e0d-137">Total new purchase amount</span></span>|
|<span data-ttu-id="e2e0d-138">beállításai</span><span class="sxs-lookup"><span data-stu-id="e2e0d-138">adjustments</span></span>|<span data-ttu-id="e2e0d-139">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-139">decimal</span></span>| <span data-ttu-id="e2e0d-140">Teljes módosítás összeg</span><span class="sxs-lookup"><span data-stu-id="e2e0d-140">Total adjustment amount</span></span>|
|<span data-ttu-id="e2e0d-141">használata</span><span class="sxs-lookup"><span data-stu-id="e2e0d-141">utilized</span></span>|<span data-ttu-id="e2e0d-142">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-142">decimal</span></span>| <span data-ttu-id="e2e0d-143">Teljes kötelezettségvállalás használata</span><span class="sxs-lookup"><span data-stu-id="e2e0d-143">Total Commitment usage</span></span>|
|<span data-ttu-id="e2e0d-144">serviceOverage</span><span class="sxs-lookup"><span data-stu-id="e2e0d-144">serviceOverage</span></span>|<span data-ttu-id="e2e0d-145">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-145">decimal</span></span>| <span data-ttu-id="e2e0d-146">Az Azure szolgáltatások túlhasználati</span><span class="sxs-lookup"><span data-stu-id="e2e0d-146">Overage for Azure services</span></span>|
|<span data-ttu-id="e2e0d-147">chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="e2e0d-147">chargesBilledSeparately</span></span>|<span data-ttu-id="e2e0d-148">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-148">decimal</span></span>| <span data-ttu-id="e2e0d-149">Külön számlázva díjak</span><span class="sxs-lookup"><span data-stu-id="e2e0d-149">Charges Billed separately</span></span>|
|<span data-ttu-id="e2e0d-150">totalOverage</span><span class="sxs-lookup"><span data-stu-id="e2e0d-150">totalOverage</span></span>|<span data-ttu-id="e2e0d-151">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-151">decimal</span></span>| <span data-ttu-id="e2e0d-152">serviceOverage + chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="e2e0d-152">serviceOverage + chargesBilledSeparately</span></span>|
|<span data-ttu-id="e2e0d-153">totalUsage</span><span class="sxs-lookup"><span data-stu-id="e2e0d-153">totalUsage</span></span>|<span data-ttu-id="e2e0d-154">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-154">decimal</span></span>| <span data-ttu-id="e2e0d-155">Azure-szolgáltatások kötelezettségvállalás + többlete</span><span class="sxs-lookup"><span data-stu-id="e2e0d-155">Azure service commitment + total Overage</span></span>|
|<span data-ttu-id="e2e0d-156">azureMarketplaceServiceCharges</span><span class="sxs-lookup"><span data-stu-id="e2e0d-156">azureMarketplaceServiceCharges</span></span>|<span data-ttu-id="e2e0d-157">Decimális</span><span class="sxs-lookup"><span data-stu-id="e2e0d-157">decimal</span></span>| <span data-ttu-id="e2e0d-158">Teljes költségek az Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="e2e0d-158">Total charges for Azure Marketplace</span></span>|
|<span data-ttu-id="e2e0d-159">newPurchasesDetails</span><span class="sxs-lookup"><span data-stu-id="e2e0d-159">newPurchasesDetails</span></span>|<span data-ttu-id="e2e0d-160">A név-érték pár karakterlánc-tömbben JSON</span><span class="sxs-lookup"><span data-stu-id="e2e0d-160">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="e2e0d-161">Új vásárlás listája</span><span class="sxs-lookup"><span data-stu-id="e2e0d-161">List of new purchases</span></span>|
|<span data-ttu-id="e2e0d-162">adjustmentDetails</span><span class="sxs-lookup"><span data-stu-id="e2e0d-162">adjustmentDetails</span></span>|<span data-ttu-id="e2e0d-163">A név-érték pár karakterlánc-tömbben JSON</span><span class="sxs-lookup"><span data-stu-id="e2e0d-163">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="e2e0d-164">(Az előléptetés jóváírás, SIE jóváírás stb.) beállításainak listája</span><span class="sxs-lookup"><span data-stu-id="e2e0d-164">List of Adjustments (Promo credit, SIE credit etc.)</span></span> |


<br/>
## <a name="see-also"></a><span data-ttu-id="e2e0d-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e2e0d-165">See also</span></span>

* [<span data-ttu-id="e2e0d-166">Elszámolási időszakok API</span><span class="sxs-lookup"><span data-stu-id="e2e0d-166">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="e2e0d-167">Használat részletei API</span><span class="sxs-lookup"><span data-stu-id="e2e0d-167">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="e2e0d-168">Piactér-tároló ingyenesen elérhető API</span><span class="sxs-lookup"><span data-stu-id="e2e0d-168">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="e2e0d-169">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="e2e0d-169">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)