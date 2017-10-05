---
title: "Az Azure Resource Manager kérelmekre vonatkozó korlátokat |} Microsoft Docs"
description: "Ismerteti, hogyan használható az Azure Resource Manager által szabályozás, amikor a rendszer elérte az előfizetési korlátozásait."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: 6d7eeaf460674c3ab98425a5412ffa465b9ffd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="throttling-resource-manager-requests"></a><span data-ttu-id="a05b6-103">Erőforrás-kezelő szabályozás</span><span class="sxs-lookup"><span data-stu-id="a05b6-103">Throttling Resource Manager requests</span></span>
<span data-ttu-id="a05b6-104">Minden egyes előfizetésekhez és bérlői erőforrás-kezelő korlátok óránként 15 000 kérelmek és olvasási kérések 1200 óránként.</span><span class="sxs-lookup"><span data-stu-id="a05b6-104">For each subscription and tenant, Resource Manager limits read requests to 15,000 per hour and write requests to 1,200 per hour.</span></span> <span data-ttu-id="a05b6-105">Ezek a korlátozások vonatkoznak az Azure Resource Manager feltünteti; több példánya van minden Azure-régiót, és az Azure Resource Manager a rendszer minden Azure-régió.</span><span class="sxs-lookup"><span data-stu-id="a05b6-105">These limits apply to each Azure Resource Manager instance; there are multiple instances in every Azure region, and Azure Resource Manager is deployed to all Azure regions.</span></span>  <span data-ttu-id="a05b6-106">Így a, a gyakorlatban korlátok hatékonyan sokkal nagyobb, mint a fent felsorolt, felhasználóként kérelmek általában által kiszolgált számos különböző példányai.</span><span class="sxs-lookup"><span data-stu-id="a05b6-106">So, in practice, limits are effectively much higher than those listed above, as user requests are generally serviced by many different instances.</span></span>

<span data-ttu-id="a05b6-107">Ha az alkalmazás vagy a parancsfájl eléri a működés felső korlátjának, amelyekkel korlátozhatja a kérelmeket szüksége.</span><span class="sxs-lookup"><span data-stu-id="a05b6-107">If your application or script reaches these limits, you need to throttle your requests.</span></span> <span data-ttu-id="a05b6-108">Ez a témakör bemutatja, hogyan határozza meg a fennmaradó kérelmek korlát elérése előtt, és hogyan válaszol, amikor elérte a maximális.</span><span class="sxs-lookup"><span data-stu-id="a05b6-108">This topic shows you how to determine the remaining requests you have before reaching the limit, and how to respond when you have reached the limit.</span></span>

<span data-ttu-id="a05b6-109">Amikor eléri a határértéket, kapja-e a HTTP-állapotkód: **429-es jelű túl sok kérelem**.</span><span class="sxs-lookup"><span data-stu-id="a05b6-109">When you reach the limit, you receive the HTTP status code **429 Too many requests**.</span></span>

<span data-ttu-id="a05b6-110">A kérelmek száma az előfizetés vagy a bérlő hatókörét.</span><span class="sxs-lookup"><span data-stu-id="a05b6-110">The number of requests is scoped to either your subscription or your tenant.</span></span> <span data-ttu-id="a05b6-111">Ha több, egyidejű alkalmazások kérelmet benyújtó az előfizetésében szereplő azon alkalmazások kérelmeinek felvétele fennmaradó kérések száma meghatározása érdekében.</span><span class="sxs-lookup"><span data-stu-id="a05b6-111">If you have multiple, concurrent applications making requests in your subscription, the requests from those applications are added together to determine the number of remaining requests.</span></span>

<span data-ttu-id="a05b6-112">Az előfizetés hatókörrel rendelkező kérelmek azok a átadja az előfizetés-azonosítóval, például az erőforrás beolvasása involve csoportosítja az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="a05b6-112">Subscription scoped requests are ones the involve passing your subscription id, such as retrieving the resource groups in your subscription.</span></span> <span data-ttu-id="a05b6-113">Bérlői hatókörű kérelmek nem tartalmaznak az előfizetés-azonosítóval, mint érvényes Azure helyek lekérése.</span><span class="sxs-lookup"><span data-stu-id="a05b6-113">Tenant scoped requests do not include your subscription id, such as retrieving valid Azure locations.</span></span>

## <a name="remaining-requests"></a><span data-ttu-id="a05b6-114">Fennmaradó kérelmek</span><span class="sxs-lookup"><span data-stu-id="a05b6-114">Remaining requests</span></span>
<span data-ttu-id="a05b6-115">Válaszfejlécek megvizsgálásával azt is meghatározhatja a fennmaradó kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="a05b6-115">You can determine the number of remaining requests by examining response headers.</span></span> <span data-ttu-id="a05b6-116">Minden egyes kérelem fennmaradó olvasási és írási kérések száma értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a05b6-116">Each request includes values for the number of remaining read and write requests.</span></span> <span data-ttu-id="a05b6-117">A következő táblázat ismerteti a válaszfejlécek ezeket az értékeket a ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="a05b6-117">The following table describes the response headers you can examine for those values:</span></span>

| <span data-ttu-id="a05b6-118">Válaszfejléc</span><span class="sxs-lookup"><span data-stu-id="a05b6-118">Response header</span></span> | <span data-ttu-id="a05b6-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="a05b6-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a05b6-120">x-MS-ratelimit-remaining-Subscription-reads</span><span class="sxs-lookup"><span data-stu-id="a05b6-120">x-ms-ratelimit-remaining-subscription-reads</span></span> |<span data-ttu-id="a05b6-121">Előfizetés hatókörű beolvassa a fennmaradó</span><span class="sxs-lookup"><span data-stu-id="a05b6-121">Subscription scoped reads remaining</span></span> |
| <span data-ttu-id="a05b6-122">x-MS-ratelimit-remaining-Subscription-writes</span><span class="sxs-lookup"><span data-stu-id="a05b6-122">x-ms-ratelimit-remaining-subscription-writes</span></span> |<span data-ttu-id="a05b6-123">Előfizetés hatókörű ír fennmaradó</span><span class="sxs-lookup"><span data-stu-id="a05b6-123">Subscription scoped writes remaining</span></span> |
| <span data-ttu-id="a05b6-124">x-MS-ratelimit-remaining-tenant-reads</span><span class="sxs-lookup"><span data-stu-id="a05b6-124">x-ms-ratelimit-remaining-tenant-reads</span></span> |<span data-ttu-id="a05b6-125">Bérlői hatókörű beolvassa a fennmaradó</span><span class="sxs-lookup"><span data-stu-id="a05b6-125">Tenant scoped reads remaining</span></span> |
| <span data-ttu-id="a05b6-126">x-MS-ratelimit-remaining-tenant-writes</span><span class="sxs-lookup"><span data-stu-id="a05b6-126">x-ms-ratelimit-remaining-tenant-writes</span></span> |<span data-ttu-id="a05b6-127">Bérlői hatókörű ír fennmaradó</span><span class="sxs-lookup"><span data-stu-id="a05b6-127">Tenant scoped writes remaining</span></span> |
| <span data-ttu-id="a05b6-128">x-MS-ratelimit-remaining-Subscription-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="a05b6-128">x-ms-ratelimit-remaining-subscription-resource-requests</span></span> |<span data-ttu-id="a05b6-129">Előfizetés fennmaradó erőforrás típusú kérések hatókörét.</span><span class="sxs-lookup"><span data-stu-id="a05b6-129">Subscription scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="a05b6-130">Ezt a fejlécértéket csak akkor ad vissza, ha a szolgáltatás felülírta az alapértelmezett határérték.</span><span class="sxs-lookup"><span data-stu-id="a05b6-130">This header value is only returned if a service has overridden the default limit.</span></span> <span data-ttu-id="a05b6-131">Erőforrás-kezelő hozzáadása az előfizetés olvasások és írások helyett ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="a05b6-131">Resource Manager adds this value instead of the subscription reads or writes.</span></span> |
| <span data-ttu-id="a05b6-132">x-MS-ratelimit-remaining-Subscription-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="a05b6-132">x-ms-ratelimit-remaining-subscription-resource-entities-read</span></span> |<span data-ttu-id="a05b6-133">Előfizetés az erőforrás típusa adatgyűjtési kérelmek fennmaradó hatókörét.</span><span class="sxs-lookup"><span data-stu-id="a05b6-133">Subscription scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="a05b6-134">Ezt a fejlécértéket csak akkor ad vissza, ha a szolgáltatás felülírta az alapértelmezett határérték.</span><span class="sxs-lookup"><span data-stu-id="a05b6-134">This header value is only returned if a service has overridden the default limit.</span></span> <span data-ttu-id="a05b6-135">Ez az érték a fennmaradó adatgyűjtési kérelmek (lista erőforrások) számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a05b6-135">This value provides the number of remaining collection requests (list resources).</span></span> |
| <span data-ttu-id="a05b6-136">x-MS-ratelimit-remaining-tenant-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="a05b6-136">x-ms-ratelimit-remaining-tenant-resource-requests</span></span> |<span data-ttu-id="a05b6-137">Bérlői erőforrás típusú kérések fennmaradó hatókörét.</span><span class="sxs-lookup"><span data-stu-id="a05b6-137">Tenant scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="a05b6-138">A bérlői szintű kérelmek csak hozzáadja az ezt a fejlécet, és csak akkor, ha a szolgáltatás felülírta az alapértelmezett határérték.</span><span class="sxs-lookup"><span data-stu-id="a05b6-138">This header is only added for requests at tenant level, and only if a service has overridden the default limit.</span></span> <span data-ttu-id="a05b6-139">Erőforrás-kezelő felveszi ezt az értéket a bérlő olvasások és írások helyett.</span><span class="sxs-lookup"><span data-stu-id="a05b6-139">Resource Manager adds this value instead of the tenant reads or writes.</span></span> |
| <span data-ttu-id="a05b6-140">x-MS-ratelimit-remaining-tenant-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="a05b6-140">x-ms-ratelimit-remaining-tenant-resource-entities-read</span></span> |<span data-ttu-id="a05b6-141">Bérlői erőforrás típusa adatgyűjtési kérelmek fennmaradó hatókörét.</span><span class="sxs-lookup"><span data-stu-id="a05b6-141">Tenant scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="a05b6-142">A bérlői szintű kérelmek csak hozzáadja az ezt a fejlécet, és csak akkor, ha a szolgáltatás felülírta az alapértelmezett határérték.</span><span class="sxs-lookup"><span data-stu-id="a05b6-142">This header is only added for requests at tenant level, and only if a service has overridden the default limit.</span></span> |

## <a name="retrieving-the-header-values"></a><span data-ttu-id="a05b6-143">A fejléc értékei beolvasása</span><span class="sxs-lookup"><span data-stu-id="a05b6-143">Retrieving the header values</span></span>
<span data-ttu-id="a05b6-144">A fejléc értékei a kód vagy parancsfájl beolvasása ugyanolyan helyzetet teremt, mint bármely fejléc értékének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a05b6-144">Retrieving these header values in your code or script is no different than retrieving any header value.</span></span> 

<span data-ttu-id="a05b6-145">Például a **C#**, visszaállíthatja a Fejlécérték egy **HttpWebResponse** nevű objektum **válasz** az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="a05b6-145">For example, in **C#**, you retrieve the header value from an **HttpWebResponse** object named **response** with the following code:</span></span>

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

<span data-ttu-id="a05b6-146">A **PowerShell**, a fejléc értékének lekérése az Invoke-WebRequest műveletet.</span><span class="sxs-lookup"><span data-stu-id="a05b6-146">In **PowerShell**, you retrieve the header value from an Invoke-WebRequest operation.</span></span>

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

<span data-ttu-id="a05b6-147">Vagy, ha szeretne látni a fennmaradó kérelmek hibakeresési, megadhatja a **-Debug** paraméter a **PowerShell** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a05b6-147">Or, if want to see the remaining requests for debugging, you can provide the **-Debug** parameter on your **PowerShell** cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -Debug
```

<span data-ttu-id="a05b6-148">Több értékhez, többek között a következő válasz értéket visszaadó:</span><span class="sxs-lookup"><span data-stu-id="a05b6-148">Which returns many values, including the following response value:</span></span>

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

<span data-ttu-id="a05b6-149">A **Azure CLI**, a részletesebb beállítás használatával beolvashatja a fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="a05b6-149">In **Azure CLI**, you retrieve the header value by using the more verbose option.</span></span>

```azurecli
azure group list -vv --json
```

<span data-ttu-id="a05b6-150">Visszaadó sok értéket, beleértve a következő objektumot:</span><span class="sxs-lookup"><span data-stu-id="a05b6-150">Which returns many values, including the following object:</span></span>

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a><span data-ttu-id="a05b6-151">Várakozás a következő kérelem elküldése előtt</span><span class="sxs-lookup"><span data-stu-id="a05b6-151">Waiting before sending next request</span></span>
<span data-ttu-id="a05b6-152">Ha eléri a kérelmi korlátjának, erőforrás-kezelő adja vissza a **429** HTTP-állapotkód és egy **újrapróbálkozási után** a fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="a05b6-152">When you reach the request limit, Resource Manager returns the **429** HTTP status code and a **Retry-After** value in the header.</span></span> <span data-ttu-id="a05b6-153">A **újrapróbálkozási után** érték másodperc megvárja-e az alkalmazás (vagy alvó) számát adja meg a következő kérelem elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="a05b6-153">The **Retry-After** value specifies the number of seconds your application should wait (or sleep) before sending the next request.</span></span> <span data-ttu-id="a05b6-154">Az újrapróbálási értéket letelte előtt kérelmet küld, ha a kérelem nincs feldolgozva, és próbálkozzon újra új értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a05b6-154">If you send a request before the retry value has elapsed, your request is not processed and a new retry value is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a05b6-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a05b6-155">Next steps</span></span>

* <span data-ttu-id="a05b6-156">Korlátozásai és a kvóták kapcsolatos további információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="a05b6-156">For more information about limits and quotas, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>
* <span data-ttu-id="a05b6-157">Aszinkron REST-kérések kezelésével kapcsolatos információkért lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="a05b6-157">To learn about handling asynchronous REST requests, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
