---
title: "Azure számlázás vállalati API - időszakok számlázási |} Microsoft Docs"
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
ms.openlocfilehash: c6880b79189e0683387a7aafbd6fa4805b3b42ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a><span data-ttu-id="34980-103">A vállalati ügyfelek - számlázási időszakok jelentéskészítési API-k</span><span class="sxs-lookup"><span data-stu-id="34980-103">Reporting APIs for Enterprise customers - Billing Periods</span></span>

<span data-ttu-id="34980-104">A számlázási időszak API adatokkal a megadott beléptetési fordított időrendben elszámolási időszakok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="34980-104">The Billing Periods API returns a list of billing periods that have consumption data for the specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="34980-105">Minden időszak mutat az API útvonalat az adat - BalanceSummary, UsageDetails, Marktplace díjakat és árlista négy csoportjai tulajdonságot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="34980-105">Each Period contains a property pointing to the API route for the four sets of data - BalanceSummary, UsageDetails, Marktplace Charges, and PriceSheet.</span></span> <span data-ttu-id="34980-106">Ha az időszakban nem rendelkezik adatokat, a megfelelő tulajdonság értéke null.</span><span class="sxs-lookup"><span data-stu-id="34980-106">If the period does not have data, the corresponding property is null.</span></span> 


##<a name="request"></a><span data-ttu-id="34980-107">Kérés</span><span class="sxs-lookup"><span data-stu-id="34980-107">Request</span></span> 
<span data-ttu-id="34980-108">Hozzá kell adni közös fejléc tulajdonságokhoz megadott [Itt](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="34980-108">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> 

|<span data-ttu-id="34980-109">Módszer</span><span class="sxs-lookup"><span data-stu-id="34980-109">Method</span></span> | <span data-ttu-id="34980-110">Kérelem URI-azonosítója</span><span class="sxs-lookup"><span data-stu-id="34980-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="34980-111">GET</span><span class="sxs-lookup"><span data-stu-id="34980-111">GET</span></span>| <span data-ttu-id="34980-112">{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / billingperiods</span><span class="sxs-lookup"><span data-stu-id="34980-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingperiods</span></span>|

> [!Note]
> <span data-ttu-id="34980-113">API előzetes verzióját használja, a fenti URL-címben v1 v2 cserélje.</span><span class="sxs-lookup"><span data-stu-id="34980-113">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="34980-114">Válasz</span><span class="sxs-lookup"><span data-stu-id="34980-114">Response</span></span>
 
    
    
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
    

<span data-ttu-id="34980-115">**Válasz erőforrástulajdonság-meghatározások**</span><span class="sxs-lookup"><span data-stu-id="34980-115">**Response property definitions**</span></span>

|<span data-ttu-id="34980-116">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="34980-116">Property Name</span></span>| <span data-ttu-id="34980-117">Típus</span><span class="sxs-lookup"><span data-stu-id="34980-117">Type</span></span>| <span data-ttu-id="34980-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="34980-118">Description</span></span>
|-|-|-|
|<span data-ttu-id="34980-119">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="34980-119">billingPeriodId</span></span>| <span data-ttu-id="34980-120">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="34980-120">string</span></span>| <span data-ttu-id="34980-121">Egy adott számlázási időszak jelölő egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="34980-121">The unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="34980-122">billingStart</span><span class="sxs-lookup"><span data-stu-id="34980-122">billingStart</span></span>| <span data-ttu-id="34980-123">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="34980-123">datetime</span></span>| <span data-ttu-id="34980-124">ISO 8601 karakterlánc, amely az időszak kezdő dátuma</span><span class="sxs-lookup"><span data-stu-id="34980-124">ISO 8601 string representing the period start date</span></span>|
|<span data-ttu-id="34980-125">billingEnd</span><span class="sxs-lookup"><span data-stu-id="34980-125">billingEnd</span></span>| <span data-ttu-id="34980-126">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="34980-126">datetime</span></span>| <span data-ttu-id="34980-127">ISO 8601 karakterlánc, amely a záró dátuma</span><span class="sxs-lookup"><span data-stu-id="34980-127">ISO 8601 string representing the period end date</span></span>|
|<span data-ttu-id="34980-128">balanceSummary</span><span class="sxs-lookup"><span data-stu-id="34980-128">balanceSummary</span></span>| <span data-ttu-id="34980-129">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="34980-129">string</span></span>| <span data-ttu-id="34980-130">Az URL-címe, amely továbbítja az egyenleg összegző adatokat az ebben az időszakban</span><span class="sxs-lookup"><span data-stu-id="34980-130">The URL path that routes to the Balance Summary data for this period</span></span>|
|<span data-ttu-id="34980-131">usageDetails</span><span class="sxs-lookup"><span data-stu-id="34980-131">usageDetails</span></span>| <span data-ttu-id="34980-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="34980-132">string</span></span>| <span data-ttu-id="34980-133">Az URL-címet, amely a használat részleteiről adatoknak az ebben az időszakban</span><span class="sxs-lookup"><span data-stu-id="34980-133">The URL path that routes to the Usage Details data for this period</span></span>|
|<span data-ttu-id="34980-134">marketplaceCharges</span><span class="sxs-lookup"><span data-stu-id="34980-134">marketplaceCharges</span></span>| <span data-ttu-id="34980-135">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="34980-135">string</span></span>| <span data-ttu-id="34980-136">Az URL-címe, amely továbbítja a piactér díjtételekre vonatkozó adatot az ebben az időszakban</span><span class="sxs-lookup"><span data-stu-id="34980-136">The URL path that routes to the Marketplace Charges data for this period</span></span>|
|<span data-ttu-id="34980-137">Árlista</span><span class="sxs-lookup"><span data-stu-id="34980-137">priceSheet</span></span>| <span data-ttu-id="34980-138">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="34980-138">string</span></span>| <span data-ttu-id="34980-139">Az URL-címet, amely a árlista adatoknak az ebben az időszakban</span><span class="sxs-lookup"><span data-stu-id="34980-139">The URL path that routes to the PriceSheet data for this period</span></span>|

<br/>
## <a name="see-also"></a><span data-ttu-id="34980-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="34980-140">See also</span></span>

* [<span data-ttu-id="34980-141">Egyenleg és összegzés API</span><span class="sxs-lookup"><span data-stu-id="34980-141">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="34980-142">Használat részletei API</span><span class="sxs-lookup"><span data-stu-id="34980-142">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="34980-143">Piactér-tároló ingyenesen elérhető API</span><span class="sxs-lookup"><span data-stu-id="34980-143">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="34980-144">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="34980-144">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)