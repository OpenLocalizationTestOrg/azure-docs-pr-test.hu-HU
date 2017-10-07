---
title: "Árlista számlázási vállalati API - aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 4cfe694c63fba266d117054b046d9caacb3b7197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a><span data-ttu-id="4dcc8-103">A vállalati ügyfelek - árlista jelentéskészítési API-k</span><span class="sxs-lookup"><span data-stu-id="4dcc8-103">Reporting APIs for Enterprise customers - Price Sheet</span></span>

<span data-ttu-id="4dcc8-104">hello Price Sheet API biztosít minden mérni a regisztráció és a számlázási időszak hello hello alkalmazható sebessége.</span><span class="sxs-lookup"><span data-stu-id="4dcc8-104">hello Price Sheet API provides hello applicable rate for each Meter for hello given Enrollment and Billing Period.</span></span>

##<a name="request"></a><span data-ttu-id="4dcc8-105">Kérés</span><span class="sxs-lookup"><span data-stu-id="4dcc8-105">Request</span></span>
<span data-ttu-id="4dcc8-106">Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="4dcc8-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="4dcc8-107">Ha nincs megadva a számlázott időszak, majd a hello aktuális elszámolási időszak adat.</span><span class="sxs-lookup"><span data-stu-id="4dcc8-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span>

|<span data-ttu-id="4dcc8-108">Módszer</span><span class="sxs-lookup"><span data-stu-id="4dcc8-108">Method</span></span> | <span data-ttu-id="4dcc8-109">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="4dcc8-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="4dcc8-110">GET</span><span class="sxs-lookup"><span data-stu-id="4dcc8-110">GET</span></span>|<span data-ttu-id="4dcc8-111">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / árlista</span><span class="sxs-lookup"><span data-stu-id="4dcc8-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet</span></span>|
|<span data-ttu-id="4dcc8-112">GET</span><span class="sxs-lookup"><span data-stu-id="4dcc8-112">GET</span></span>|<span data-ttu-id="4dcc8-113">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ {billingPeriod} / árlista</span><span class="sxs-lookup"><span data-stu-id="4dcc8-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet</span></span>|

> [!Note]
> <span data-ttu-id="4dcc8-114">toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.</span><span class="sxs-lookup"><span data-stu-id="4dcc8-114">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="4dcc8-115">Válasz</span><span class="sxs-lookup"><span data-stu-id="4dcc8-115">Response</span></span>

    
        [
            {
                "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
                "billingPeriodId": "201704",
                "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
                "meterName": "A1 VM",
                "unitOfMeasure": "100 Hours",
                "includedQuantity": 0,
                "partNumber": "N7H-00015",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            {
                "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
                "billingPeriodId": "201404",
                "meterId": "dc210ecb-97e8-4522-8134-5385494233c0",
                "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
                "unitOfMeasure": "100 GB",
                "includedQuantity": 0,
                "partNumber": "N9H-00402",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            ...
        ]
    

> [!Note]
><span data-ttu-id="4dcc8-116">Ha hello előnézeti API-t használ, a meterId mező nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="4dcc8-116">If you are using hello Preview API, meterId field is not available.</span></span>
>

<span data-ttu-id="4dcc8-117">**Válasz erőforrástulajdonság-meghatározások**</span><span class="sxs-lookup"><span data-stu-id="4dcc8-117">**Response property definitions**</span></span>

|<span data-ttu-id="4dcc8-118">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="4dcc8-118">Property Name</span></span>| <span data-ttu-id="4dcc8-119">Típus</span><span class="sxs-lookup"><span data-stu-id="4dcc8-119">Type</span></span>| <span data-ttu-id="4dcc8-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="4dcc8-120">Description</span></span>
|-|-|-|
|<span data-ttu-id="4dcc8-121">id</span><span class="sxs-lookup"><span data-stu-id="4dcc8-121">id</span></span>| <span data-ttu-id="4dcc8-122">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4dcc8-122">string</span></span>| <span data-ttu-id="4dcc8-123">hello egy adott árlista elemet (mérő számlázási időszak szerint) jelölő elem egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="4dcc8-123">hello unique Id that represents a particular PriceSheet item (meter by billing period)</span></span>|
|<span data-ttu-id="4dcc8-124">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="4dcc8-124">billingPeriodId</span></span>| <span data-ttu-id="4dcc8-125">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4dcc8-125">string</span></span>| <span data-ttu-id="4dcc8-126">hello egy adott számlázási időszak jelölő egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="4dcc8-126">hello unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="4dcc8-127">meterId</span><span class="sxs-lookup"><span data-stu-id="4dcc8-127">meterId</span></span>| <span data-ttu-id="4dcc8-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4dcc8-128">string</span></span>| <span data-ttu-id="4dcc8-129">hello mérő hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4dcc8-129">hello identifier for hello meter.</span></span> <span data-ttu-id="4dcc8-130">Ez lehet a csatlakoztatott toohello használati meterId.</span><span class="sxs-lookup"><span data-stu-id="4dcc8-130">It can be mapped toohello usage meterId.</span></span>|
|<span data-ttu-id="4dcc8-131">meterName</span><span class="sxs-lookup"><span data-stu-id="4dcc8-131">meterName</span></span>| <span data-ttu-id="4dcc8-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4dcc8-132">string</span></span>| <span data-ttu-id="4dcc8-133">hello mérő neve</span><span class="sxs-lookup"><span data-stu-id="4dcc8-133">hello meter name</span></span>|
|<span data-ttu-id="4dcc8-134">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="4dcc8-134">unitOfMeasure</span></span>| <span data-ttu-id="4dcc8-135">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4dcc8-135">string</span></span>| <span data-ttu-id="4dcc8-136">hello mértékegység méri hello szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4dcc8-136">hello Unit of Measure for measuring hello service</span></span>|
|<span data-ttu-id="4dcc8-137">includedQuantity</span><span class="sxs-lookup"><span data-stu-id="4dcc8-137">includedQuantity</span></span>| <span data-ttu-id="4dcc8-138">Decimális</span><span class="sxs-lookup"><span data-stu-id="4dcc8-138">decimal</span></span>| <span data-ttu-id="4dcc8-139">Részét képező mennyiség</span><span class="sxs-lookup"><span data-stu-id="4dcc8-139">Quantity that is included</span></span> |
|<span data-ttu-id="4dcc8-140">partNumber</span><span class="sxs-lookup"><span data-stu-id="4dcc8-140">partNumber</span></span>| <span data-ttu-id="4dcc8-141">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4dcc8-141">string</span></span>| <span data-ttu-id="4dcc8-142">hello mérő társított hello cikkszám</span><span class="sxs-lookup"><span data-stu-id="4dcc8-142">hello part number associated with hello Meter</span></span>|
|<span data-ttu-id="4dcc8-143">Egységár</span><span class="sxs-lookup"><span data-stu-id="4dcc8-143">unitPrice</span></span>| <span data-ttu-id="4dcc8-144">Decimális</span><span class="sxs-lookup"><span data-stu-id="4dcc8-144">decimal</span></span>| <span data-ttu-id="4dcc8-145">hello mérő hello Egységár</span><span class="sxs-lookup"><span data-stu-id="4dcc8-145">hello unit price for hello meter</span></span>|
|<span data-ttu-id="4dcc8-146">currencyCode</span><span class="sxs-lookup"><span data-stu-id="4dcc8-146">currencyCode</span></span>| <span data-ttu-id="4dcc8-147">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4dcc8-147">string</span></span>| <span data-ttu-id="4dcc8-148">hello pénznemkódot a hello Egységár</span><span class="sxs-lookup"><span data-stu-id="4dcc8-148">hello currency code for hello unitPrice</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="4dcc8-149">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4dcc8-149">See also</span></span>

* [<span data-ttu-id="4dcc8-150">Elszámolási időszakok API</span><span class="sxs-lookup"><span data-stu-id="4dcc8-150">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="4dcc8-151">Használat részletei API</span><span class="sxs-lookup"><span data-stu-id="4dcc8-151">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md)

* [<span data-ttu-id="4dcc8-152">Egyenleg és összegzés API</span><span class="sxs-lookup"><span data-stu-id="4dcc8-152">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="4dcc8-153">Piactér-tároló ingyenesen elérhető API</span><span class="sxs-lookup"><span data-stu-id="4dcc8-153">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md)
