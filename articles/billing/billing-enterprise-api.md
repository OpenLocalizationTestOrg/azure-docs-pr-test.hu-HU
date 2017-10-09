---
title: "Vállalati API-k számlázási aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 017cecc57ad6bdeb402b5d9d57fc95df9b033a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a><span data-ttu-id="0d274-103">A vállalati ügyfelek a Reporting API-k – áttekintés</span><span class="sxs-lookup"><span data-stu-id="0d274-103">Overview of Reporting APIs for Enterprise customers</span></span>
<span data-ttu-id="0d274-104">hello Reporting API-k engedélyezése vállalati Azure ügyfelek tooprogrammatically lekéréses használati és elszámolási adatok az elsődleges adatok elemzésére szolgáló eszközöket.</span><span class="sxs-lookup"><span data-stu-id="0d274-104">hello Reporting APIs enable Enterprise Azure customers tooprogrammatically pull consumption and billing data into preferred data analysis tools.</span></span> 

## <a name="enabling-data-access-toohello-api"></a><span data-ttu-id="0d274-105">Adatok hozzáférési toohello API engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0d274-105">Enabling data access toohello API</span></span>
* <span data-ttu-id="0d274-106">**Hozza létre vagy hello API-kulcs beolvasása** - toohello vállalati portálon, és hajtsa végre hello az oktatóanyagban a Súgó - Reporting API-k.</span><span class="sxs-lookup"><span data-stu-id="0d274-106">**Generate or retrieve hello API key** - Log in toohello Enterprise portal and follow hello tutorial under Help - Reporting APIs.</span></span> <span data-ttu-id="0d274-107">hello első szakasza a Súgó cikk azt ismerteti, hogyan toogenerate vagy olvashatók be hello API key hello a megadott regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="0d274-107">hello first section under this help article explains how toogenerate or retrieve hello API key for hello specified enrollment.</span></span>
* <span data-ttu-id="0d274-108">**Kulcsok benyújtása hello API** -hello API-kulcsot kell toobe átadott minden hívás a hitelesítéshez és engedélyezéshez.</span><span class="sxs-lookup"><span data-stu-id="0d274-108">**Passing keys in hello API** - hello API key needs toobe passed for each call for Authentication and Authorization.</span></span> <span data-ttu-id="0d274-109">hello következő kell tulajdonságot toobe toohello HTTP-fejlécek</span><span class="sxs-lookup"><span data-stu-id="0d274-109">hello following property needs toobe toohello HTTP headers</span></span>

|<span data-ttu-id="0d274-110">Fejléc kulcs kérése</span><span class="sxs-lookup"><span data-stu-id="0d274-110">Request Header Key</span></span> | <span data-ttu-id="0d274-111">Érték</span><span class="sxs-lookup"><span data-stu-id="0d274-111">Value</span></span>|
|-|-|
|<span data-ttu-id="0d274-112">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="0d274-112">Authorization</span></span>| <span data-ttu-id="0d274-113">Hello értéket adjon meg a következő formátumban: **tulajdonosi {API_KEY}**</span><span class="sxs-lookup"><span data-stu-id="0d274-113">Specify hello value in this format: **bearer {API_KEY}**</span></span> <br/> <span data-ttu-id="0d274-114">Példa: tulajdonosi eyr... 09</span><span class="sxs-lookup"><span data-stu-id="0d274-114">Example: bearer eyr....09</span></span>|

## <a name="consumption-apis"></a><span data-ttu-id="0d274-115">Felhasználás API-k</span><span class="sxs-lookup"><span data-stu-id="0d274-115">Consumption APIs</span></span>
<span data-ttu-id="0d274-116">A Swagger-végpont esetében elérhető [Itt](https://consumption.azure.com/swagger/ui/index) hello az API-k leírt, amely alatt kell engedélyezése könnyen introspection hello API és hello képességét toogenerate ügyfél SDK-k használatával [AutoRest](https://github.com/Azure/AutoRest) vagy [ Swagger CodeGen](http://swagger.io/swagger-codegen/).</span><span class="sxs-lookup"><span data-stu-id="0d274-116">A Swagger endpoint is available [here](https://consumption.azure.com/swagger/ui/index) for hello APIs described below which should enable easy introspection of hello API and hello ability toogenerate client SDKs using [AutoRest](https://github.com/Azure/AutoRest) or [Swagger CodeGen](http://swagger.io/swagger-codegen/).</span></span> <span data-ttu-id="0d274-117">2014. Előfordulhat, hogy 1 verziótól adatok az API-n keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0d274-117">Data beginning May 1, 2014 is available through this API.</span></span> 

* <span data-ttu-id="0d274-118">**Egyenleg és összegzése** – hello [egyenleg és összefoglaló API](billing-enterprise-api-balance-summary.md) kiegyensúlyozza, új, Azure piactér szolgáltatási díjak, módosításának és keretét díjak adatainak havi összegzése kínál.</span><span class="sxs-lookup"><span data-stu-id="0d274-118">**Balance and Summary** - hello [Balance and Summary API](billing-enterprise-api-balance-summary.md) offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments and overage charges.</span></span>

* <span data-ttu-id="0d274-119">**Használat részletei** – hello [használatának részletességgel API](billing-enterprise-api-usage-detail.md) kínál a felhasznált és a beléptetési díjak becsült napi bontásban.</span><span class="sxs-lookup"><span data-stu-id="0d274-119">**Usage Details** - hello [Usage Detail API](billing-enterprise-api-usage-detail.md) offers a daily breakdown of consumed quantities and estimated charges by an Enrollment.</span></span> <span data-ttu-id="0d274-120">hello eredmény példányok, mérőszámok és szervezeti egységek adatokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0d274-120">hello result also includes information on instances, meters and departments.</span></span> <span data-ttu-id="0d274-121">hello API számlázási időszak szerint, vagy a megadott kezdési és befejezési dátum kérdezhetők le.</span><span class="sxs-lookup"><span data-stu-id="0d274-121">hello API can be queried by Billing period or by a specified start and end date.</span></span> 

* <span data-ttu-id="0d274-122">**Piactér tároló kell fizetni** – hello [piactér tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) adja vissza a hello piactér használat alapú költségek lebontása napi hello megadott számlázási időszak vagy kezdő és befejező dátumok (egyszer díjak kimaradnak) .</span><span class="sxs-lookup"><span data-stu-id="0d274-122">**Marketplace Store Charge** - hello [Marketplace Store Charge API](billing-enterprise-api-marketplace-storecharge.md) returns hello usage-based marketplace charges breakdown by day for hello specified Billing Period or start and end dates (one time fees are not included).</span></span>

* <span data-ttu-id="0d274-123">**Árlista** – hello [Price Sheet API](billing-enterprise-api-pricesheet.md) hello alkalmazható arány biztosít minden mérni a regisztráció és a számlázási időszak hello.</span><span class="sxs-lookup"><span data-stu-id="0d274-123">**Price Sheet** - hello [Price Sheet API](billing-enterprise-api-pricesheet.md) provides hello applicable rate for each Meter for hello given Enrollment and Billing Period.</span></span> 

## <a name="helper-apis"></a><span data-ttu-id="0d274-124">Segéd API-k</span><span class="sxs-lookup"><span data-stu-id="0d274-124">Helper APIs</span></span>
 <span data-ttu-id="0d274-125">**Számlázási időszakok listában** – hello [számlázási időszakok API](billing-enterprise-api-billing-periods.md) időszakokat, amelyek hello beléptetési megadott fordított időrendben adatokkal rendelkeznie számlázási listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0d274-125">**List Billing Periods** - hello [Billing Periods API](billing-enterprise-api-billing-periods.md) returns a list of billing periods that have consumption data for hello specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="0d274-126">Minden időszak toohello API útvonalat az adat - BalanceSummary, UsageDetails, piactér díjakat és árlista hello négy csoportjai mutató tulajdonságot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0d274-126">Each Period contains a property pointing toohello API route for hello four sets of data - BalanceSummary, UsageDetails, Marketplace Charges, and Price Sheet.</span></span>


## <a name="api-response-codes"></a><span data-ttu-id="0d274-127">API-válaszkódok</span><span class="sxs-lookup"><span data-stu-id="0d274-127">API Response Codes</span></span>  
|<span data-ttu-id="0d274-128">Válasz állapotkódja</span><span class="sxs-lookup"><span data-stu-id="0d274-128">Response Status Code</span></span>|<span data-ttu-id="0d274-129">Üzenet</span><span class="sxs-lookup"><span data-stu-id="0d274-129">Message</span></span>|<span data-ttu-id="0d274-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="0d274-130">Description</span></span>|
|-|-|-|
|<span data-ttu-id="0d274-131">200</span><span class="sxs-lookup"><span data-stu-id="0d274-131">200</span></span>| <span data-ttu-id="0d274-132">OKÉ</span><span class="sxs-lookup"><span data-stu-id="0d274-132">OK</span></span>|<span data-ttu-id="0d274-133">Nem történt hiba</span><span class="sxs-lookup"><span data-stu-id="0d274-133">No error</span></span>|
|<span data-ttu-id="0d274-134">401</span><span class="sxs-lookup"><span data-stu-id="0d274-134">401</span></span>| <span data-ttu-id="0d274-135">Nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="0d274-135">Unauthorized</span></span>| <span data-ttu-id="0d274-136">Az API-kulcs nem található, érvénytelen, lejárt stb.</span><span class="sxs-lookup"><span data-stu-id="0d274-136">API Key not found, Invalid, Expired etc.</span></span>|
|<span data-ttu-id="0d274-137">404</span><span class="sxs-lookup"><span data-stu-id="0d274-137">404</span></span>| <span data-ttu-id="0d274-138">Nem érhető el</span><span class="sxs-lookup"><span data-stu-id="0d274-138">Unavailable</span></span>| <span data-ttu-id="0d274-139">A jelentés a végpont nem található</span><span class="sxs-lookup"><span data-stu-id="0d274-139">Report endpoint not found</span></span>|
|<span data-ttu-id="0d274-140">400</span><span class="sxs-lookup"><span data-stu-id="0d274-140">400</span></span>| <span data-ttu-id="0d274-141">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="0d274-141">Bad Request</span></span>| <span data-ttu-id="0d274-142">Érvénytelen paraméterek – dátumtartományok, EA számok stb.</span><span class="sxs-lookup"><span data-stu-id="0d274-142">Invalid params – Date ranges, EA numbers etc.</span></span>|
|<span data-ttu-id="0d274-143">500</span><span class="sxs-lookup"><span data-stu-id="0d274-143">500</span></span>| <span data-ttu-id="0d274-144">Kiszolgálóhiba</span><span class="sxs-lookup"><span data-stu-id="0d274-144">Server Error</span></span>| <span data-ttu-id="0d274-145">Unexoected hiba a kérelem feldolgozása</span><span class="sxs-lookup"><span data-stu-id="0d274-145">Unexoected error processing request</span></span>| 









