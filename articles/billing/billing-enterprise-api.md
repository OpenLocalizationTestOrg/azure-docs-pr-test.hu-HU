---
title: "Azure számlázás vállalati API-k |} Microsoft Docs"
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
ms.openlocfilehash: e3a5f9bcd6b54a51c29df649f1ae8ac185b153a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a><span data-ttu-id="ebd70-103">A vállalati ügyfelek a Reporting API-k – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ebd70-103">Overview of Reporting APIs for Enterprise customers</span></span>
<span data-ttu-id="ebd70-104">A Reporting API-k engedélyezése a vállalati Azure-ügyfelek számára használati és elszámolási adatok programozott módon le az elsődleges adatok elemzésére szolgáló eszközöket.</span><span class="sxs-lookup"><span data-stu-id="ebd70-104">The Reporting APIs enable Enterprise Azure customers to programmatically pull consumption and billing data into preferred data analysis tools.</span></span> 

## <a name="enabling-data-access-to-the-api"></a><span data-ttu-id="ebd70-105">Az API adataihoz hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ebd70-105">Enabling data access to the API</span></span>
* <span data-ttu-id="ebd70-106">**Hozza létre, vagy az API-kulcs beolvasása** - a vállalati portál és kövesse az oktatóanyag a Súgó - Reporting API-k.</span><span class="sxs-lookup"><span data-stu-id="ebd70-106">**Generate or retrieve the API key** - Log in to the Enterprise portal and follow the tutorial under Help - Reporting APIs.</span></span> <span data-ttu-id="ebd70-107">Az első szakasza a súgócikk ismerteti, hogyan létrehozásához vagy a megadott beléptetési API-kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ebd70-107">The first section under this help article explains how to generate or retrieve the API key for the specified enrollment.</span></span>
* <span data-ttu-id="ebd70-108">**Kulcsok átadja a API** -az API-kulcsot kell átadni minden hívás a hitelesítéshez és engedélyezéshez.</span><span class="sxs-lookup"><span data-stu-id="ebd70-108">**Passing keys in the API** - The API key needs to be passed for each call for Authentication and Authorization.</span></span> <span data-ttu-id="ebd70-109">A következő tulajdonság kell lennie, hogy a HTTP-fejlécek</span><span class="sxs-lookup"><span data-stu-id="ebd70-109">The following property needs to be to the HTTP headers</span></span>

|<span data-ttu-id="ebd70-110">Fejléc kulcs kérése</span><span class="sxs-lookup"><span data-stu-id="ebd70-110">Request Header Key</span></span> | <span data-ttu-id="ebd70-111">Érték</span><span class="sxs-lookup"><span data-stu-id="ebd70-111">Value</span></span>|
|-|-|
|<span data-ttu-id="ebd70-112">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="ebd70-112">Authorization</span></span>| <span data-ttu-id="ebd70-113">Adja meg az értéket a következő formátumban: **tulajdonosi {API_KEY}**</span><span class="sxs-lookup"><span data-stu-id="ebd70-113">Specify the value in this format: **bearer {API_KEY}**</span></span> <br/> <span data-ttu-id="ebd70-114">Példa: tulajdonosi eyr... 09</span><span class="sxs-lookup"><span data-stu-id="ebd70-114">Example: bearer eyr....09</span></span>|

## <a name="consumption-apis"></a><span data-ttu-id="ebd70-115">Felhasználás API-k</span><span class="sxs-lookup"><span data-stu-id="ebd70-115">Consumption APIs</span></span>
<span data-ttu-id="ebd70-116">A Swagger-végpont esetében elérhető [Itt](https://consumption.azure.com/swagger/ui/index) az API-k leírt, amely alatt API könnyen introspection és képességét ügyfél SDK-k használatával engedélyezze a [AutoRest](https://github.com/Azure/AutoRest) vagy [Swagger CodeGen](http://swagger.io/swagger-codegen/).</span><span class="sxs-lookup"><span data-stu-id="ebd70-116">A Swagger endpoint is available [here](https://consumption.azure.com/swagger/ui/index) for the APIs described below which should enable easy introspection of the API and the ability to generate client SDKs using [AutoRest](https://github.com/Azure/AutoRest) or [Swagger CodeGen](http://swagger.io/swagger-codegen/).</span></span> <span data-ttu-id="ebd70-117">2014. Előfordulhat, hogy 1 verziótól adatok az API-n keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="ebd70-117">Data beginning May 1, 2014 is available through this API.</span></span> 

* <span data-ttu-id="ebd70-118">**Egyenleg és összegzése** – a [egyenleg és összefoglaló API](billing-enterprise-api-balance-summary.md) kiegyensúlyozza, új, Azure piactér szolgáltatási díjak, módosításának és keretét díjak adatainak havi összegzése kínál.</span><span class="sxs-lookup"><span data-stu-id="ebd70-118">**Balance and Summary** - The [Balance and Summary API](billing-enterprise-api-balance-summary.md) offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments and overage charges.</span></span>

* <span data-ttu-id="ebd70-119">**Használat részleteiről** – a [használatának részletességgel API](billing-enterprise-api-usage-detail.md) kínál a felhasznált és a beléptetési díjak becsült napi bontásban.</span><span class="sxs-lookup"><span data-stu-id="ebd70-119">**Usage Details** - The [Usage Detail API](billing-enterprise-api-usage-detail.md) offers a daily breakdown of consumed quantities and estimated charges by an Enrollment.</span></span> <span data-ttu-id="ebd70-120">Az eredmény a példányok, a mérőszámok és a szervezeti adatokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ebd70-120">The result also includes information on instances, meters and departments.</span></span> <span data-ttu-id="ebd70-121">Az API-t számlázási időszak, vagy a megadott kezdő és záró dátum kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="ebd70-121">The API can be queried by Billing period or by a specified start and end date.</span></span> 

* <span data-ttu-id="ebd70-122">**Piactér-tároló kell fizetni** – a [piactér tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) ad vissza a piactér használat alapú költségek bontásban tartalmazza a megadott számlázási időszak vagy a kezdő és befejező dátumok (egyszer díjak nincsenek) nap.</span><span class="sxs-lookup"><span data-stu-id="ebd70-122">**Marketplace Store Charge** - The [Marketplace Store Charge API](billing-enterprise-api-marketplace-storecharge.md) returns the usage-based marketplace charges breakdown by day for the specified Billing Period or start and end dates (one time fees are not included).</span></span>

* <span data-ttu-id="ebd70-123">**Árlista** – a [Price Sheet API](billing-enterprise-api-pricesheet.md) alkalmazható sebessége biztosít minden a megadott regisztrációs és számlázási időszak.</span><span class="sxs-lookup"><span data-stu-id="ebd70-123">**Price Sheet** - The [Price Sheet API](billing-enterprise-api-pricesheet.md) provides the applicable rate for each Meter for the given Enrollment and Billing Period.</span></span> 

## <a name="helper-apis"></a><span data-ttu-id="ebd70-124">Segéd API-k</span><span class="sxs-lookup"><span data-stu-id="ebd70-124">Helper APIs</span></span>
 <span data-ttu-id="ebd70-125">**Számlázási időszakok listában** – a [számlázási időszakok API](billing-enterprise-api-billing-periods.md) időszakokat, amelyek a megadott regisztrációs fordított időrendben adatokkal rendelkezik számlázási listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ebd70-125">**List Billing Periods** - The [Billing Periods API](billing-enterprise-api-billing-periods.md) returns a list of billing periods that have consumption data for the specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="ebd70-126">Minden időszak mutat az API útvonalat az adat - BalanceSummary, UsageDetails, piactér díjakat és árlista négy csoportjai tulajdonságot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ebd70-126">Each Period contains a property pointing to the API route for the four sets of data - BalanceSummary, UsageDetails, Marketplace Charges, and Price Sheet.</span></span>


## <a name="api-response-codes"></a><span data-ttu-id="ebd70-127">API-válaszkódok</span><span class="sxs-lookup"><span data-stu-id="ebd70-127">API Response Codes</span></span>  
|<span data-ttu-id="ebd70-128">Válasz állapotkódja</span><span class="sxs-lookup"><span data-stu-id="ebd70-128">Response Status Code</span></span>|<span data-ttu-id="ebd70-129">Üzenet</span><span class="sxs-lookup"><span data-stu-id="ebd70-129">Message</span></span>|<span data-ttu-id="ebd70-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="ebd70-130">Description</span></span>|
|-|-|-|
|<span data-ttu-id="ebd70-131">200</span><span class="sxs-lookup"><span data-stu-id="ebd70-131">200</span></span>| <span data-ttu-id="ebd70-132">OKÉ</span><span class="sxs-lookup"><span data-stu-id="ebd70-132">OK</span></span>|<span data-ttu-id="ebd70-133">Nem történt hiba</span><span class="sxs-lookup"><span data-stu-id="ebd70-133">No error</span></span>|
|<span data-ttu-id="ebd70-134">401</span><span class="sxs-lookup"><span data-stu-id="ebd70-134">401</span></span>| <span data-ttu-id="ebd70-135">Nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="ebd70-135">Unauthorized</span></span>| <span data-ttu-id="ebd70-136">Az API-kulcs nem található, érvénytelen, lejárt stb.</span><span class="sxs-lookup"><span data-stu-id="ebd70-136">API Key not found, Invalid, Expired etc.</span></span>|
|<span data-ttu-id="ebd70-137">404</span><span class="sxs-lookup"><span data-stu-id="ebd70-137">404</span></span>| <span data-ttu-id="ebd70-138">Nem érhető el</span><span class="sxs-lookup"><span data-stu-id="ebd70-138">Unavailable</span></span>| <span data-ttu-id="ebd70-139">A jelentés a végpont nem található</span><span class="sxs-lookup"><span data-stu-id="ebd70-139">Report endpoint not found</span></span>|
|<span data-ttu-id="ebd70-140">400</span><span class="sxs-lookup"><span data-stu-id="ebd70-140">400</span></span>| <span data-ttu-id="ebd70-141">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="ebd70-141">Bad Request</span></span>| <span data-ttu-id="ebd70-142">Érvénytelen paraméterek – dátumtartományok, EA számok stb.</span><span class="sxs-lookup"><span data-stu-id="ebd70-142">Invalid params – Date ranges, EA numbers etc.</span></span>|
|<span data-ttu-id="ebd70-143">500</span><span class="sxs-lookup"><span data-stu-id="ebd70-143">500</span></span>| <span data-ttu-id="ebd70-144">Kiszolgálóhiba</span><span class="sxs-lookup"><span data-stu-id="ebd70-144">Server Error</span></span>| <span data-ttu-id="ebd70-145">Unexoected hiba a kérelem feldolgozása</span><span class="sxs-lookup"><span data-stu-id="ebd70-145">Unexoected error processing request</span></span>| 









