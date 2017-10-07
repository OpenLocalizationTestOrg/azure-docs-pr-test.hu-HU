---
title: "aaaAzure erőforrás-kezelő kérelmekre vonatkozó korlátokat |} Microsoft Docs"
description: "Ismerteti, hogyan kérelmezi a toouse az Azure Resource Manager-szabályozás, ha a rendszer elérte az előfizetési korlátozásait."
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
ms.openlocfilehash: ea274f3145af36aac753730e1f280d8a8b59c3bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-resource-manager-requests"></a><span data-ttu-id="089ec-103">Erőforrás-kezelő szabályozás</span><span class="sxs-lookup"><span data-stu-id="089ec-103">Throttling Resource Manager requests</span></span>
<span data-ttu-id="089ec-104">Minden egyes előfizetésekhez és bérlői erőforrás-kezelő korlátok kérelmek too15, óránként 000 és olvasási kérések too1, óránként 200.</span><span class="sxs-lookup"><span data-stu-id="089ec-104">For each subscription and tenant, Resource Manager limits read requests too15,000 per hour and write requests too1,200 per hour.</span></span> <span data-ttu-id="089ec-105">Ezek a korlátozások alkalmazásához tooeach Azure Resource Manager-példány; több példánya van minden Azure-régiót, és Azure Resource Manager üzembe helyezett tooall Azure régióban.</span><span class="sxs-lookup"><span data-stu-id="089ec-105">These limits apply tooeach Azure Resource Manager instance; there are multiple instances in every Azure region, and Azure Resource Manager is deployed tooall Azure regions.</span></span>  <span data-ttu-id="089ec-106">Így a, a gyakorlatban korlátok hatékonyan sokkal nagyobb, mint a fent felsorolt, felhasználóként kérelmek általában által kiszolgált számos különböző példányai.</span><span class="sxs-lookup"><span data-stu-id="089ec-106">So, in practice, limits are effectively much higher than those listed above, as user requests are generally serviced by many different instances.</span></span>

<span data-ttu-id="089ec-107">Ha az alkalmazás vagy a parancsfájl eléri a működés felső korlátjának, akkor toothrottle a kérelmek.</span><span class="sxs-lookup"><span data-stu-id="089ec-107">If your application or script reaches these limits, you need toothrottle your requests.</span></span> <span data-ttu-id="089ec-108">Ez a témakör bemutatja, hogyan fennmaradó toodetermine hello kérelmek hello korlát elérése előtt van, és hogyan toorespond, amikor elérte hello vonatkozó korlátozást.</span><span class="sxs-lookup"><span data-stu-id="089ec-108">This topic shows you how toodetermine hello remaining requests you have before reaching hello limit, and how toorespond when you have reached hello limit.</span></span>

<span data-ttu-id="089ec-109">Hello korlát elérésekor kapni hello HTTP-állapotkód: **429-es jelű túl sok kérelem**.</span><span class="sxs-lookup"><span data-stu-id="089ec-109">When you reach hello limit, you receive hello HTTP status code **429 Too many requests**.</span></span>

<span data-ttu-id="089ec-110">kérelem hello hatókörön belüli tooeither van, az előfizetés vagy a bérlőt.</span><span class="sxs-lookup"><span data-stu-id="089ec-110">hello number of requests is scoped tooeither your subscription or your tenant.</span></span> <span data-ttu-id="089ec-111">Ha több, egyidejű alkalmazások az előfizetéséhez, a kérelmet benyújtó hello kérelmeinek azon alkalmazások felvétele együtt fennmaradó kérelmek toodetermine hello száma.</span><span class="sxs-lookup"><span data-stu-id="089ec-111">If you have multiple, concurrent applications making requests in your subscription, hello requests from those applications are added together toodetermine hello number of remaining requests.</span></span>

<span data-ttu-id="089ec-112">Hatókörű előfizetési kérelmek azok hello tartalmaz, amely átadja az előfizetés-azonosítóval, például az előfizetés hello erőforráscsoportok lekérése során.</span><span class="sxs-lookup"><span data-stu-id="089ec-112">Subscription scoped requests are ones hello involve passing your subscription id, such as retrieving hello resource groups in your subscription.</span></span> <span data-ttu-id="089ec-113">Bérlői hatókörű kérelmek nem tartalmaznak az előfizetés-azonosítóval, mint érvényes Azure helyek lekérése.</span><span class="sxs-lookup"><span data-stu-id="089ec-113">Tenant scoped requests do not include your subscription id, such as retrieving valid Azure locations.</span></span>

## <a name="remaining-requests"></a><span data-ttu-id="089ec-114">Fennmaradó kérelmek</span><span class="sxs-lookup"><span data-stu-id="089ec-114">Remaining requests</span></span>
<span data-ttu-id="089ec-115">Válaszfejlécek megvizsgálásával azt is meghatározhatja hello fennmaradó kérések száma.</span><span class="sxs-lookup"><span data-stu-id="089ec-115">You can determine hello number of remaining requests by examining response headers.</span></span> <span data-ttu-id="089ec-116">Minden egyes kérelem fennmaradó olvasási és írási kérések száma hello értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="089ec-116">Each request includes values for hello number of remaining read and write requests.</span></span> <span data-ttu-id="089ec-117">hello következő táblázatban láthatja, hogy ezeket az értékeket a hello válaszfejlécek.</span><span class="sxs-lookup"><span data-stu-id="089ec-117">hello following table describes hello response headers you can examine for those values:</span></span>

| <span data-ttu-id="089ec-118">Válaszfejléc</span><span class="sxs-lookup"><span data-stu-id="089ec-118">Response header</span></span> | <span data-ttu-id="089ec-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="089ec-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="089ec-120">x-MS-ratelimit-remaining-Subscription-reads</span><span class="sxs-lookup"><span data-stu-id="089ec-120">x-ms-ratelimit-remaining-subscription-reads</span></span> |<span data-ttu-id="089ec-121">Előfizetés hatókörű beolvassa a fennmaradó</span><span class="sxs-lookup"><span data-stu-id="089ec-121">Subscription scoped reads remaining</span></span> |
| <span data-ttu-id="089ec-122">x-MS-ratelimit-remaining-Subscription-writes</span><span class="sxs-lookup"><span data-stu-id="089ec-122">x-ms-ratelimit-remaining-subscription-writes</span></span> |<span data-ttu-id="089ec-123">Előfizetés hatókörű ír fennmaradó</span><span class="sxs-lookup"><span data-stu-id="089ec-123">Subscription scoped writes remaining</span></span> |
| <span data-ttu-id="089ec-124">x-MS-ratelimit-remaining-tenant-reads</span><span class="sxs-lookup"><span data-stu-id="089ec-124">x-ms-ratelimit-remaining-tenant-reads</span></span> |<span data-ttu-id="089ec-125">Bérlői hatókörű beolvassa a fennmaradó</span><span class="sxs-lookup"><span data-stu-id="089ec-125">Tenant scoped reads remaining</span></span> |
| <span data-ttu-id="089ec-126">x-MS-ratelimit-remaining-tenant-writes</span><span class="sxs-lookup"><span data-stu-id="089ec-126">x-ms-ratelimit-remaining-tenant-writes</span></span> |<span data-ttu-id="089ec-127">Bérlői hatókörű ír fennmaradó</span><span class="sxs-lookup"><span data-stu-id="089ec-127">Tenant scoped writes remaining</span></span> |
| <span data-ttu-id="089ec-128">x-MS-ratelimit-remaining-Subscription-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="089ec-128">x-ms-ratelimit-remaining-subscription-resource-requests</span></span> |<span data-ttu-id="089ec-129">Előfizetés fennmaradó erőforrás típusú kérések hatókörét.</span><span class="sxs-lookup"><span data-stu-id="089ec-129">Subscription scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="089ec-130">Ezt a fejlécértéket csak akkor ad vissza, ha a szolgáltatás felülírta hello alapértelmezett korlát.</span><span class="sxs-lookup"><span data-stu-id="089ec-130">This header value is only returned if a service has overridden hello default limit.</span></span> <span data-ttu-id="089ec-131">Erőforrás-kezelő hozzáadása hello előfizetés olvasások és írások helyett ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="089ec-131">Resource Manager adds this value instead of hello subscription reads or writes.</span></span> |
| <span data-ttu-id="089ec-132">x-MS-ratelimit-remaining-Subscription-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="089ec-132">x-ms-ratelimit-remaining-subscription-resource-entities-read</span></span> |<span data-ttu-id="089ec-133">Előfizetés az erőforrás típusa adatgyűjtési kérelmek fennmaradó hatókörét.</span><span class="sxs-lookup"><span data-stu-id="089ec-133">Subscription scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="089ec-134">Ezt a fejlécértéket csak akkor ad vissza, ha a szolgáltatás felülírta hello alapértelmezett korlát.</span><span class="sxs-lookup"><span data-stu-id="089ec-134">This header value is only returned if a service has overridden hello default limit.</span></span> <span data-ttu-id="089ec-135">Ez az érték számos hello fennmaradó adatgyűjtési kérelmek (lista erőforrások).</span><span class="sxs-lookup"><span data-stu-id="089ec-135">This value provides hello number of remaining collection requests (list resources).</span></span> |
| <span data-ttu-id="089ec-136">x-MS-ratelimit-remaining-tenant-Resource-Requests</span><span class="sxs-lookup"><span data-stu-id="089ec-136">x-ms-ratelimit-remaining-tenant-resource-requests</span></span> |<span data-ttu-id="089ec-137">Bérlői erőforrás típusú kérések fennmaradó hatókörét.</span><span class="sxs-lookup"><span data-stu-id="089ec-137">Tenant scoped resource type requests remaining.</span></span><br /><br /><span data-ttu-id="089ec-138">A bérlői szintű kérelmek csak hozzáadja az ezt a fejlécet, és csak akkor, ha a szolgáltatás felülírta hello alapértelmezett korlát.</span><span class="sxs-lookup"><span data-stu-id="089ec-138">This header is only added for requests at tenant level, and only if a service has overridden hello default limit.</span></span> <span data-ttu-id="089ec-139">Erőforrás-kezelő hozzáadása hello bérlői olvasások és írások helyett ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="089ec-139">Resource Manager adds this value instead of hello tenant reads or writes.</span></span> |
| <span data-ttu-id="089ec-140">x-MS-ratelimit-remaining-tenant-Resource-entities-Read</span><span class="sxs-lookup"><span data-stu-id="089ec-140">x-ms-ratelimit-remaining-tenant-resource-entities-read</span></span> |<span data-ttu-id="089ec-141">Bérlői erőforrás típusa adatgyűjtési kérelmek fennmaradó hatókörét.</span><span class="sxs-lookup"><span data-stu-id="089ec-141">Tenant scoped resource type collection requests remaining.</span></span><br /><br /><span data-ttu-id="089ec-142">A bérlői szintű kérelmek csak hozzáadja az ezt a fejlécet, és csak akkor, ha a szolgáltatás felülírta hello alapértelmezett korlát.</span><span class="sxs-lookup"><span data-stu-id="089ec-142">This header is only added for requests at tenant level, and only if a service has overridden hello default limit.</span></span> |

## <a name="retrieving-hello-header-values"></a><span data-ttu-id="089ec-143">Hello térközkaraktert beolvasása</span><span class="sxs-lookup"><span data-stu-id="089ec-143">Retrieving hello header values</span></span>
<span data-ttu-id="089ec-144">A fejléc értékei a kód vagy parancsfájl beolvasása ugyanolyan helyzetet teremt, mint bármely fejléc értékének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="089ec-144">Retrieving these header values in your code or script is no different than retrieving any header value.</span></span> 

<span data-ttu-id="089ec-145">Például a **C#**, visszaállíthatja az hello Fejlécérték egy **HttpWebResponse** nevű objektum **válasz** a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="089ec-145">For example, in **C#**, you retrieve hello header value from an **HttpWebResponse** object named **response** with hello following code:</span></span>

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

<span data-ttu-id="089ec-146">A **PowerShell**, hello állomásfejléc-érték lekérése az Invoke-WebRequest műveletet.</span><span class="sxs-lookup"><span data-stu-id="089ec-146">In **PowerShell**, you retrieve hello header value from an Invoke-WebRequest operation.</span></span>

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

<span data-ttu-id="089ec-147">Vagy, ha szeretné, hogy toosee hello fennmaradó vonatkozó kérések hibakeresés, megadhatja a hello **-Debug** paraméter a **PowerShell** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="089ec-147">Or, if want toosee hello remaining requests for debugging, you can provide hello **-Debug** parameter on your **PowerShell** cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -Debug
```

<span data-ttu-id="089ec-148">Többek között a következő válasz érték hello sok értéket visszaadó:</span><span class="sxs-lookup"><span data-stu-id="089ec-148">Which returns many values, including hello following response value:</span></span>

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

<span data-ttu-id="089ec-149">A **Azure CLI**, visszaállíthatja az hello állomásfejléc-érték használatával hello részletesebb lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="089ec-149">In **Azure CLI**, you retrieve hello header value by using hello more verbose option.</span></span>

```azurecli
azure group list -vv --json
```

<span data-ttu-id="089ec-150">Több értékhez, beleértve a következő objektum hello visszaadó:</span><span class="sxs-lookup"><span data-stu-id="089ec-150">Which returns many values, including hello following object:</span></span>

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

## <a name="waiting-before-sending-next-request"></a><span data-ttu-id="089ec-151">Várakozás a következő kérelem elküldése előtt</span><span class="sxs-lookup"><span data-stu-id="089ec-151">Waiting before sending next request</span></span>
<span data-ttu-id="089ec-152">Hello kérelmi korlátjának elérésekor az erőforrás-kezelő adja vissza hello **429** HTTP-állapotkód és egy **újrapróbálkozási után** hello fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="089ec-152">When you reach hello request limit, Resource Manager returns hello **429** HTTP status code and a **Retry-After** value in hello header.</span></span> <span data-ttu-id="089ec-153">Hello **újrapróbálkozási után** érték hello következő kérelem elküldése előtt megvárja-e az alkalmazás másodperc (vagy alvó) hello számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="089ec-153">hello **Retry-After** value specifies hello number of seconds your application should wait (or sleep) before sending hello next request.</span></span> <span data-ttu-id="089ec-154">Hello Retry paraméter értéke letelte előtt kérelmet küld, ha a kérelem nincs feldolgozva, és próbálkozzon újra új értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="089ec-154">If you send a request before hello retry value has elapsed, your request is not processed and a new retry value is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="089ec-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="089ec-155">Next steps</span></span>

* <span data-ttu-id="089ec-156">Korlátozásai és a kvóták kapcsolatos további információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="089ec-156">For more information about limits and quotas, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>
* <span data-ttu-id="089ec-157">toolearn aszinkron REST kérések kezelésével kapcsolatban lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="089ec-157">toolearn about handling asynchronous REST requests, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
