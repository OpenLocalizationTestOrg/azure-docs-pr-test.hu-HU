---
title: "aaaScheduler korlátozza, és alapértelmezés szerint használt érték"
description: "A Feladatütemező korlátozásai és alapértelmezett értéke"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 6fe0600d3ce3249d5aab1b877369b175316b5437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-limits-and-defaults"></a><span data-ttu-id="49005-103">A Feladatütemező korlátozásai és alapértelmezett értéke</span><span class="sxs-lookup"><span data-stu-id="49005-103">Scheduler Limits and Defaults</span></span>
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a><span data-ttu-id="49005-104">A Feladatütemező kvóták, korlátok, alapértelmezett és szabályozások</span><span class="sxs-lookup"><span data-stu-id="49005-104">Scheduler Quotas, Limits, Defaults, and Throttles</span></span>
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="hello-x-ms-request-id-header"></a><span data-ttu-id="49005-105">az x-ms-request-id hello fejléc</span><span class="sxs-lookup"><span data-stu-id="49005-105">hello x-ms-request-id Header</span></span>
<span data-ttu-id="49005-106">Felé irányuló hello a Feladatütemező szolgáltatás összes kérelmet adja vissza egy nevű válaszfejléc**az x-ms-request-id**. Ezt a fejlécet, amely egyedileg azonosítja a hello kérelem nem átlátszó értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="49005-106">Every request made against hello Scheduler service returns a response header named**x-ms-request-id**. This header contains an opaque value that uniquely identifies hello request.</span></span>

<span data-ttu-id="49005-107">Ha a kérés következetesen futtatása sikertelen, és rendelkezik ellenőrizte, hogy hello kérelem megfelelően formázott van, az érték tooreport hello hiba tooMicrosoft használhatja.</span><span class="sxs-lookup"><span data-stu-id="49005-107">If a request is consistently failing and you have verified that hello request is properly formulated, you may use this value tooreport hello error tooMicrosoft.</span></span> <span data-ttu-id="49005-108">A jelentés tartalmazza a hello értékét az x-ms-request-id hello megközelítőleges időpont, amikor adott hello kérés érkezett, hello hello előfizetés, a feladatgyűjtemény és/vagy a feladat azonosítóját, és hello típusú, amely megkísérelte kérelem hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="49005-108">In your report, include hello value of x-ms-request-id, hello approximate time that hello request was made, hello identifier of hello subscription, job collection, and/or job, and hello type of operation that hello request attempted.</span></span>

## <a name="see-also"></a><span data-ttu-id="49005-109">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="49005-109">See Also</span></span>
 [<span data-ttu-id="49005-110">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="49005-110">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="49005-111">Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="49005-111">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="49005-112">Az ütemező hello Azure-portálon az első lépéseiben</span><span class="sxs-lookup"><span data-stu-id="49005-112">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="49005-113">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="49005-113">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="49005-114">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="49005-114">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="49005-115">Az Azure Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="49005-115">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="49005-116">Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="49005-116">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="49005-117">Kimenő hitelesítés az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="49005-117">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

