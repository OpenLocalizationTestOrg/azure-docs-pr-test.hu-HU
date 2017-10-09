---
title: "Azure CDN aaaHTTP/2 támogatást |} Microsoft Docs"
description: "Ismerje meg a HTTP/2 és a CDN támogatása."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 2e5e5345e8cf5c40e080ebf18b4f13a239a5aac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="http2-support-in-azure-cdn"></a><span data-ttu-id="cc510-103">Az Azure CDN HTTP/2-támogatás</span><span class="sxs-lookup"><span data-stu-id="cc510-103">HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="cc510-104">A HTTP/2 egy nagyobb javítása tooHTTP/1.1\.</span><span class="sxs-lookup"><span data-stu-id="cc510-104">HTTP/2 is a major revision tooHTTP/1.1\.</span></span> <span data-ttu-id="cc510-105">Biztosít gyorsabban webes teljesítmény, a csökkentett válaszidő és a jobb felhasználói élmény, hello ismerős HTTP-metódus állapotkódok és szemantikáját megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="cc510-105">It provides faster web performance, reduced response time, and improved user experience, while maintaining hello familiar HTTP methods, status codes, and semantics.</span></span> <span data-ttu-id="cc510-106">Bár a HTTP/2 HTTP és HTTPS tervezett toowork, számos ügyfél webböngésző csak támogatja a HTTP/2 TLS feletti.</span><span class="sxs-lookup"><span data-stu-id="cc510-106">Though HTTP/2 is designed toowork with HTTP and HTTPS, many client web browsers only support HTTP/2 over TLS.</span></span>

###<a name="http2-benefits"></a><span data-ttu-id="cc510-107">A HTTP/2 előnyei</span><span class="sxs-lookup"><span data-stu-id="cc510-107">HTTP/2 Benefits</span></span>

<span data-ttu-id="cc510-108">a HTTP/2 hello előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="cc510-108">hello benefits of HTTP/2 include:</span></span>

*   <span data-ttu-id="cc510-109">**Multiplexáló és feldolgozási**</span><span class="sxs-lookup"><span data-stu-id="cc510-109">**Multiplexing and concurrency**</span></span>

    <span data-ttu-id="cc510-110">HTTP 1.1 több és több erőforrás-kérelmek több TCP-kapcsolatot igényel, és minden egyes kapcsolathoz teljesítményigény társítva van.</span><span class="sxs-lookup"><span data-stu-id="cc510-110">Using HTTP 1.1, multiple making multiple resource requests requires multiple TCP connections, and each connection has performance overhead associated with it.</span></span> <span data-ttu-id="cc510-111">A HTTP/2 lehetővé teszi, hogy több erőforrás toobe egyetlen TCP-kapcsolatot kért.</span><span class="sxs-lookup"><span data-stu-id="cc510-111">HTTP/2 allows multiple resources toobe requested on a single TCP connection.</span></span>

*   <span data-ttu-id="cc510-112">**Fejléc tömörítése**</span><span class="sxs-lookup"><span data-stu-id="cc510-112">**Header compression**</span></span>

    <span data-ttu-id="cc510-113">Hello HTTP-fejlécek szolgálatban erőforrások tömörítésével hello keresztülhaladnak a hálózaton idő jelentősen csökkent.</span><span class="sxs-lookup"><span data-stu-id="cc510-113">By compressing hello HTTP headers for served resources, time on hello wire is reduced significantly.</span></span>

*   <span data-ttu-id="cc510-114">**Adatfolyam-függőségek**</span><span class="sxs-lookup"><span data-stu-id="cc510-114">**Stream dependencies**</span></span>

    <span data-ttu-id="cc510-115">Az adatfolyam függőségek hello ügyfél tooindicate toohello kiszolgálót, amely az erőforrások prioritással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cc510-115">Stream dependencies allow hello client tooindicate toohello server which of resources have priority.</span></span>


##<a name="http2-browser-support"></a><span data-ttu-id="cc510-116">A HTTP/2 webböngésző támogatása</span><span class="sxs-lookup"><span data-stu-id="cc510-116">HTTP/2 Browser Support</span></span>

<span data-ttu-id="cc510-117">Az összes ismertebb böngésző hello alkalmaztunk, hogy HTTP/2-támogatás az aktuális verzióban.</span><span class="sxs-lookup"><span data-stu-id="cc510-117">All of hello major browsers have implemented HTTP/2 support in their current versions.</span></span> <span data-ttu-id="cc510-118">Nem támogatott böngészőkkel automatikusan tartalék tooHTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="cc510-118">Non-supported browsers will automatically fallback tooHTTP/1.1.</span></span>

|<span data-ttu-id="cc510-119">Böngésző</span><span class="sxs-lookup"><span data-stu-id="cc510-119">Browser</span></span>|<span data-ttu-id="cc510-120">Minimális verzió</span><span class="sxs-lookup"><span data-stu-id="cc510-120">Minimum Version</span></span>|
|-------------|------------|
|<span data-ttu-id="cc510-121">A Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="cc510-121">Microsoft Edge</span></span>| <span data-ttu-id="cc510-122">12</span><span class="sxs-lookup"><span data-stu-id="cc510-122">12</span></span>|
|<span data-ttu-id="cc510-123">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="cc510-123">Google Chrome</span></span>| <span data-ttu-id="cc510-124">43</span><span class="sxs-lookup"><span data-stu-id="cc510-124">43</span></span>|
|<span data-ttu-id="cc510-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="cc510-125">Mozilla Firefox</span></span>| <span data-ttu-id="cc510-126">38</span><span class="sxs-lookup"><span data-stu-id="cc510-126">38</span></span>|
|<span data-ttu-id="cc510-127">Opera</span><span class="sxs-lookup"><span data-stu-id="cc510-127">Opera</span></span>| <span data-ttu-id="cc510-128">32</span><span class="sxs-lookup"><span data-stu-id="cc510-128">32</span></span>|
|<span data-ttu-id="cc510-129">Safari</span><span class="sxs-lookup"><span data-stu-id="cc510-129">Safari</span></span>| <span data-ttu-id="cc510-130">9</span><span class="sxs-lookup"><span data-stu-id="cc510-130">9</span></span>|

##<a name="enabling-http2-support-in-azure-cdn"></a><span data-ttu-id="cc510-131">Az Azure CDN HTTP/2-támogatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cc510-131">Enabling HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="cc510-132">Jelenleg aktív a HTTP/2 támogatási **Akamai Azure CDN** és **Azure CDN Verizon** profilok.</span><span class="sxs-lookup"><span data-stu-id="cc510-132">Currently HTTP/2 support is active for **Azure CDN from Akamai** and **Azure CDN from Verizon** profiles.</span></span> <span data-ttu-id="cc510-133">Az ügyfelek semmilyen további műveletet kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="cc510-133">No further action is required from customers.</span></span>

##<a name="next-steps"></a><span data-ttu-id="cc510-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc510-134">Next Steps</span></span>

<span data-ttu-id="cc510-135">toosee hello előnyei HTTP/2 működés közben, lásd: [ebben a bemutatóban a Akamai](https://http2.akamai.com/demo).</span><span class="sxs-lookup"><span data-stu-id="cc510-135">toosee hello benefits of HTTP/2 in action, see [this demo from Akamai](https://http2.akamai.com/demo).</span></span>

<span data-ttu-id="cc510-136">toolearn bővebben HTTP/2, látogasson el a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="cc510-136">toolearn more about HTTP/2, visit hello following resources:</span></span>

*   [<span data-ttu-id="cc510-137">A HTTP/2 specification-kezdőlap</span><span class="sxs-lookup"><span data-stu-id="cc510-137">HTTP/2 specification homepage</span></span>](https://http2.github.io/)
*   [<span data-ttu-id="cc510-138">Hivatalos HTTP/2 – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="cc510-138">Official HTTP/2 FAQ</span></span>](https://http2.github.io/faq/)
*   [<span data-ttu-id="cc510-139">Akamai HTTP/2-információk</span><span class="sxs-lookup"><span data-stu-id="cc510-139">Akamai HTTP/2 information</span></span>](https://http2.akamai.com/)

<span data-ttu-id="cc510-140">További információ az Azure CDN szolgáltatásai toolearn lásd: hello [Azure CDN áttekintése](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span><span class="sxs-lookup"><span data-stu-id="cc510-140">toolearn more about Azure CDN's available features, see hello [Azure CDN Overview](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span></span>
