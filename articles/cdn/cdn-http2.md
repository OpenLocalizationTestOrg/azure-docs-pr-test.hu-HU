---
title: "HTTP/2-támogatás az Azure CDN |} Microsoft Docs"
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
ms.openlocfilehash: 4f8dd685c3ae89535217d7a17a01c5129ca7e6e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="http2-support-in-azure-cdn"></a><span data-ttu-id="94bcc-103">Az Azure CDN HTTP/2-támogatás</span><span class="sxs-lookup"><span data-stu-id="94bcc-103">HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="94bcc-104">A HTTP/2 HTTP/1.1\ nagyobb javítása.</span><span class="sxs-lookup"><span data-stu-id="94bcc-104">HTTP/2 is a major revision to HTTP/1.1\.</span></span> <span data-ttu-id="94bcc-105">Biztosít gyorsabban webes teljesítmény, a csökkentett válaszidő és a jobb felhasználói élmény, a megszokott HTTP-metódus állapotkódok és szemantikáját megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="94bcc-105">It provides faster web performance, reduced response time, and improved user experience, while maintaining the familiar HTTP methods, status codes, and semantics.</span></span> <span data-ttu-id="94bcc-106">Bár a HTTP/2 célja, hogy a HTTP és HTTPS használata, számos ügyfél webböngészőben csak támogatja a HTTP/2 TLS feletti.</span><span class="sxs-lookup"><span data-stu-id="94bcc-106">Though HTTP/2 is designed to work with HTTP and HTTPS, many client web browsers only support HTTP/2 over TLS.</span></span>

###<a name="http2-benefits"></a><span data-ttu-id="94bcc-107">A HTTP/2 előnyei</span><span class="sxs-lookup"><span data-stu-id="94bcc-107">HTTP/2 Benefits</span></span>

<span data-ttu-id="94bcc-108">A HTTP/2 előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="94bcc-108">The benefits of HTTP/2 include:</span></span>

*   <span data-ttu-id="94bcc-109">**Multiplexáló és feldolgozási**</span><span class="sxs-lookup"><span data-stu-id="94bcc-109">**Multiplexing and concurrency**</span></span>

    <span data-ttu-id="94bcc-110">HTTP 1.1 több és több erőforrás-kérelmek több TCP-kapcsolatot igényel, és minden egyes kapcsolathoz teljesítményigény társítva van.</span><span class="sxs-lookup"><span data-stu-id="94bcc-110">Using HTTP 1.1, multiple making multiple resource requests requires multiple TCP connections, and each connection has performance overhead associated with it.</span></span> <span data-ttu-id="94bcc-111">HTTP/2 lehetővé teszi, hogy több erőforrás vonatkozó egyetlen TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="94bcc-111">HTTP/2 allows multiple resources to be requested on a single TCP connection.</span></span>

*   <span data-ttu-id="94bcc-112">**Fejléc tömörítése**</span><span class="sxs-lookup"><span data-stu-id="94bcc-112">**Header compression**</span></span>

    <span data-ttu-id="94bcc-113">A HTTP-fejlécek szolgálatban erőforrások tömörítésével vezetéken lévő idő jelentősen csökkent.</span><span class="sxs-lookup"><span data-stu-id="94bcc-113">By compressing the HTTP headers for served resources, time on the wire is reduced significantly.</span></span>

*   <span data-ttu-id="94bcc-114">**Adatfolyam-függőségek**</span><span class="sxs-lookup"><span data-stu-id="94bcc-114">**Stream dependencies**</span></span>

    <span data-ttu-id="94bcc-115">Adatfolyam-függőségek engedélyezi az ügyfél megjelenítésével jelzi a kiszolgáló, amely erőforrások prioritással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="94bcc-115">Stream dependencies allow the client to indicate to the server which of resources have priority.</span></span>


##<a name="http2-browser-support"></a><span data-ttu-id="94bcc-116">A HTTP/2 webböngésző támogatása</span><span class="sxs-lookup"><span data-stu-id="94bcc-116">HTTP/2 Browser Support</span></span>

<span data-ttu-id="94bcc-117">Az összes ismertebb böngésző alkalmaztunk, hogy HTTP/2-támogatás az aktuális verzióban.</span><span class="sxs-lookup"><span data-stu-id="94bcc-117">All of the major browsers have implemented HTTP/2 support in their current versions.</span></span> <span data-ttu-id="94bcc-118">Nem támogatott böngészők lesz automatikusan tartalék HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="94bcc-118">Non-supported browsers will automatically fallback to HTTP/1.1.</span></span>

|<span data-ttu-id="94bcc-119">Böngésző</span><span class="sxs-lookup"><span data-stu-id="94bcc-119">Browser</span></span>|<span data-ttu-id="94bcc-120">Minimális verzió</span><span class="sxs-lookup"><span data-stu-id="94bcc-120">Minimum Version</span></span>|
|-------------|------------|
|<span data-ttu-id="94bcc-121">A Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="94bcc-121">Microsoft Edge</span></span>| <span data-ttu-id="94bcc-122">12</span><span class="sxs-lookup"><span data-stu-id="94bcc-122">12</span></span>|
|<span data-ttu-id="94bcc-123">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="94bcc-123">Google Chrome</span></span>| <span data-ttu-id="94bcc-124">43</span><span class="sxs-lookup"><span data-stu-id="94bcc-124">43</span></span>|
|<span data-ttu-id="94bcc-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="94bcc-125">Mozilla Firefox</span></span>| <span data-ttu-id="94bcc-126">38</span><span class="sxs-lookup"><span data-stu-id="94bcc-126">38</span></span>|
|<span data-ttu-id="94bcc-127">Opera</span><span class="sxs-lookup"><span data-stu-id="94bcc-127">Opera</span></span>| <span data-ttu-id="94bcc-128">32</span><span class="sxs-lookup"><span data-stu-id="94bcc-128">32</span></span>|
|<span data-ttu-id="94bcc-129">Safari</span><span class="sxs-lookup"><span data-stu-id="94bcc-129">Safari</span></span>| <span data-ttu-id="94bcc-130">9</span><span class="sxs-lookup"><span data-stu-id="94bcc-130">9</span></span>|

##<a name="enabling-http2-support-in-azure-cdn"></a><span data-ttu-id="94bcc-131">Az Azure CDN HTTP/2-támogatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="94bcc-131">Enabling HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="94bcc-132">Jelenleg aktív a HTTP/2 támogatási **Akamai Azure CDN** és **Azure CDN Verizon** profilok.</span><span class="sxs-lookup"><span data-stu-id="94bcc-132">Currently HTTP/2 support is active for **Azure CDN from Akamai** and **Azure CDN from Verizon** profiles.</span></span> <span data-ttu-id="94bcc-133">Az ügyfelek semmilyen további műveletet kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="94bcc-133">No further action is required from customers.</span></span>

##<a name="next-steps"></a><span data-ttu-id="94bcc-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94bcc-134">Next Steps</span></span>

<span data-ttu-id="94bcc-135">A HTTP/2 előnyeit működés közben, olvassa el [ebben a bemutatóban a Akamai](https://http2.akamai.com/demo).</span><span class="sxs-lookup"><span data-stu-id="94bcc-135">To see the benefits of HTTP/2 in action, see [this demo from Akamai](https://http2.akamai.com/demo).</span></span>

<span data-ttu-id="94bcc-136">A HTTP/2 kapcsolatos további információkért látogasson el a következőket:</span><span class="sxs-lookup"><span data-stu-id="94bcc-136">To learn more about HTTP/2, visit the following resources:</span></span>

*   [<span data-ttu-id="94bcc-137">A HTTP/2 specification-kezdőlap</span><span class="sxs-lookup"><span data-stu-id="94bcc-137">HTTP/2 specification homepage</span></span>](https://http2.github.io/)
*   [<span data-ttu-id="94bcc-138">Hivatalos HTTP/2 – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="94bcc-138">Official HTTP/2 FAQ</span></span>](https://http2.github.io/faq/)
*   [<span data-ttu-id="94bcc-139">Akamai HTTP/2-információk</span><span class="sxs-lookup"><span data-stu-id="94bcc-139">Akamai HTTP/2 information</span></span>](https://http2.akamai.com/)

<span data-ttu-id="94bcc-140">Azure CDN elérhető funkciókkal kapcsolatos további tudnivalókért tekintse meg a [Azure CDN áttekintése](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span><span class="sxs-lookup"><span data-stu-id="94bcc-140">To learn more about Azure CDN's available features, see the [Azure CDN Overview](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span></span>