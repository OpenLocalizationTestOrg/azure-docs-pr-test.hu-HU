---
title: "Számlázási vállalati API - időszakok számlázási aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: d4e17f25b22729a7f213306fb019ee0dbeca87ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a><span data-ttu-id="071ab-103">A vállalati ügyfelek - számlázási időszakok jelentéskészítési API-k</span><span class="sxs-lookup"><span data-stu-id="071ab-103">Reporting APIs for Enterprise customers - Billing Periods</span></span>

<span data-ttu-id="071ab-104">hello számlázási időszakok API adja vissza a hello adatokkal rendelkező időszakok számlázási listája beléptetési meg fordított időrendi sorrendben.</span><span class="sxs-lookup"><span data-stu-id="071ab-104">hello Billing Periods API returns a list of billing periods that have consumption data for hello specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="071ab-105">Minden időszak toohello API útvonalat az adat - BalanceSummary, UsageDetails, Marktplace díjakat és árlista hello négy csoportjai mutató tulajdonságot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="071ab-105">Each Period contains a property pointing toohello API route for hello four sets of data - BalanceSummary, UsageDetails, Marktplace Charges, and PriceSheet.</span></span> <span data-ttu-id="071ab-106">Hello időszak nincs adatokat, ha hello megfelelő tulajdonsága null értékű.</span><span class="sxs-lookup"><span data-stu-id="071ab-106">If hello period does not have data, hello corresponding property is null.</span></span> 


##<a name="request"></a><span data-ttu-id="071ab-107">Kérés</span><span class="sxs-lookup"><span data-stu-id="071ab-107">Request</span></span> 
<span data-ttu-id="071ab-108">Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="071ab-108">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> 

|<span data-ttu-id="071ab-109">Módszer</span><span class="sxs-lookup"><span data-stu-id="071ab-109">Method</span></span> | <span data-ttu-id="071ab-110">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="071ab-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="071ab-111">GET</span><span class="sxs-lookup"><span data-stu-id="071ab-111">GET</span></span>| <span data-ttu-id="071ab-112">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / billingperiods</span><span class="sxs-lookup"><span data-stu-id="071ab-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingperiods</span></span>|

> [!Note]
> <span data-ttu-id="071ab-113">toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.</span><span class="sxs-lookup"><span data-stu-id="071ab-113">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="071ab-114">Válasz</span><span class="sxs-lookup"><span data-stu-id="071ab-114">Response</span></span>
 
    
    
      [
            {
                "billingPeriodId": "201704",
                "billingStart": "2017-04-01T00:00:00Z",
                "billingEnd": "2017-04-30T11:59:59Z",
                "balanceSummary": "/v1/enrollments/100/billingperiods/201704/balancesummary",
                "usageDetails": "/v1/enrollments/100/billingperiods/201704/usagedetails",
                "marketplaceCharges": "/v1/enrollments/100/billingperiods/201704/marketplacecharges",
                "priceSheet": "/v1/enrollments/100/billingperiods/201704/pricesheet"
            },          
            ....
      ]
    

<span data-ttu-id="071ab-115">**Válasz erőforrástulajdonság-meghatározások**</span><span class="sxs-lookup"><span data-stu-id="071ab-115">**Response property definitions**</span></span>

|<span data-ttu-id="071ab-116">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="071ab-116">Property Name</span></span>| <span data-ttu-id="071ab-117">Típus</span><span class="sxs-lookup"><span data-stu-id="071ab-117">Type</span></span>| <span data-ttu-id="071ab-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="071ab-118">Description</span></span>
|-|-|-|
|<span data-ttu-id="071ab-119">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="071ab-119">billingPeriodId</span></span>| <span data-ttu-id="071ab-120">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="071ab-120">string</span></span>| <span data-ttu-id="071ab-121">hello egy adott számlázási időszak jelölő egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="071ab-121">hello unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="071ab-122">billingStart</span><span class="sxs-lookup"><span data-stu-id="071ab-122">billingStart</span></span>| <span data-ttu-id="071ab-123">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="071ab-123">datetime</span></span>| <span data-ttu-id="071ab-124">ISO 8601 karakterlánc, amely hello időszak kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="071ab-124">ISO 8601 string representing hello period start date</span></span>|
|<span data-ttu-id="071ab-125">billingEnd</span><span class="sxs-lookup"><span data-stu-id="071ab-125">billingEnd</span></span>| <span data-ttu-id="071ab-126">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="071ab-126">datetime</span></span>| <span data-ttu-id="071ab-127">ISO 8601 karakterlánc, amely hello záró dátuma</span><span class="sxs-lookup"><span data-stu-id="071ab-127">ISO 8601 string representing hello period end date</span></span>|
|<span data-ttu-id="071ab-128">balanceSummary</span><span class="sxs-lookup"><span data-stu-id="071ab-128">balanceSummary</span></span>| <span data-ttu-id="071ab-129">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="071ab-129">string</span></span>| <span data-ttu-id="071ab-130">hello URL-címet, amely toohello egyenleg összegző adatokat az ebben az időszakban</span><span class="sxs-lookup"><span data-stu-id="071ab-130">hello URL path that routes toohello Balance Summary data for this period</span></span>|
|<span data-ttu-id="071ab-131">usageDetails</span><span class="sxs-lookup"><span data-stu-id="071ab-131">usageDetails</span></span>| <span data-ttu-id="071ab-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="071ab-132">string</span></span>| <span data-ttu-id="071ab-133">hello URL-címet, amely az adott időszakra vonatkozó toohello használati adatainak</span><span class="sxs-lookup"><span data-stu-id="071ab-133">hello URL path that routes toohello Usage Details data for this period</span></span>|
|<span data-ttu-id="071ab-134">marketplaceCharges</span><span class="sxs-lookup"><span data-stu-id="071ab-134">marketplaceCharges</span></span>| <span data-ttu-id="071ab-135">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="071ab-135">string</span></span>| <span data-ttu-id="071ab-136">hello URL-címet, amely az adott időszakra vonatkozó toohello piactér díjtételekre vonatkozó adatot</span><span class="sxs-lookup"><span data-stu-id="071ab-136">hello URL path that routes toohello Marketplace Charges data for this period</span></span>|
|<span data-ttu-id="071ab-137">Árlista</span><span class="sxs-lookup"><span data-stu-id="071ab-137">priceSheet</span></span>| <span data-ttu-id="071ab-138">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="071ab-138">string</span></span>| <span data-ttu-id="071ab-139">hello URL-címet, amely az adott időszakra vonatkozó toohello árlista adatok</span><span class="sxs-lookup"><span data-stu-id="071ab-139">hello URL path that routes toohello PriceSheet data for this period</span></span>|

<br/>
## <a name="see-also"></a><span data-ttu-id="071ab-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="071ab-140">See also</span></span>

* [<span data-ttu-id="071ab-141">Egyenleg és összegzés API</span><span class="sxs-lookup"><span data-stu-id="071ab-141">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="071ab-142">Használat részletei API</span><span class="sxs-lookup"><span data-stu-id="071ab-142">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="071ab-143">Piactér-tároló ingyenesen elérhető API</span><span class="sxs-lookup"><span data-stu-id="071ab-143">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="071ab-144">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="071ab-144">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)