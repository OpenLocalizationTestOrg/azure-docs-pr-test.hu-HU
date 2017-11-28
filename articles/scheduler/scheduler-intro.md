---
title: aaaWhat az Azure Scheduler? | Microsoft Docs
description: "Azure Schedulerrel lehetővé teszi a toodeclaratively műveletek toorun hello felhőben írják le. A rendszer ezt követően a műveletek ütemezését és futtatását automatikusan végzi el."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 062e25ae473510264dc0038198c05e7ac1e86210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-scheduler"></a><span data-ttu-id="9f8a9-105">Mi az Azure Scheduler?</span><span class="sxs-lookup"><span data-stu-id="9f8a9-105">What is Azure Scheduler?</span></span>
<span data-ttu-id="9f8a9-106">Azure Schedulerrel lehetővé teszi a toodeclaratively műveletek toorun hello felhőben írják le.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-106">Azure Scheduler allows you toodeclaratively describe actions toorun in hello cloud.</span></span> <span data-ttu-id="9f8a9-107">A rendszer ezt követően a műveletek ütemezését és futtatását automatikusan végzi el.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-107">It then schedules and runs those actions automatically.</span></span>  <span data-ttu-id="9f8a9-108">A Feladatütemező ezt használatával hajtja végre [hello Azure-portálon](scheduler-get-started-portal.md), kód, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-108">Scheduler does this by using [hello Azure portal](scheduler-get-started-portal.md), code, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), or Azure PowerShell.</span></span>

<span data-ttu-id="9f8a9-109">A Scheduler elvégzi az ütemezett munkák létrehozását, karbantartását és meghívását.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-109">Scheduler creates, maintains, and invokes scheduled work.</span></span>  <span data-ttu-id="9f8a9-110">A Scheduler nem futtat számítási feladatokat vagy kódokat.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-110">Scheduler does not host any workloads or run any code.</span></span> <span data-ttu-id="9f8a9-111">A szolgáltatás csupán *meghívja* a máshol (az Azure-ban, helyszínen vagy másik szolgáltató által) futtatott kódokat.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-111">It only *invokes* code hosted elsewhere—in Azure, on-premises, or with another provider.</span></span> <span data-ttu-id="9f8a9-112">A meghívás a következőkön keresztül történhet: HTTP, HTTPS, tárolási sor, Service Bus-üzenetsor vagy Service Bus-témakör.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-112">It invokes via HTTP, HTTPS, a storage queue, a service bus queue, or a service bus topic.</span></span>

<span data-ttu-id="9f8a9-113">A Feladatütemező ütemezések [feladatok](scheduler-concepts-terms.md), tartja a feladat végrehajtásának eredménye előzményeit, hogy egy tekintse át, és deterministically és megbízhatóan ütemezések munkaterhelések toobe fut-e.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-113">Scheduler schedules [jobs](scheduler-concepts-terms.md), keeps a history of job execution results that one can review, and deterministically and reliably schedules workloads toobe run.</span></span> <span data-ttu-id="9f8a9-114">Azure WebJobs (Azure App Service Web Apps szolgáltatása hello része) és az egyéb Azure ütemezési szolgáltatása használja a Feladatütemező hello háttérben.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-114">Azure WebJobs (part of hello Web Apps feature in Azure App Service) and other Azure scheduling capabilities use Scheduler in hello background.</span></span> <span data-ttu-id="9f8a9-115">Hello [Feladatütemező REST API](https://msdn.microsoft.com/library/mt629143.aspx) segít az ilyen műveletek hello kommunikáció kezelése.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-115">hello [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) helps manage hello communication for these actions.</span></span> <span data-ttu-id="9f8a9-116">Ezért a Scheduler támogatja a [komplex és speciális, ismétlődő ütemezések](scheduler-advanced-complexity.md) egyszerű létrehozását.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-116">As such, Scheduler supports [complex schedules and advanced recurrence](scheduler-advanced-complexity.md) easily.</span></span>

<span data-ttu-id="9f8a9-117">Nincsenek több szolgáltatásokat, amelyek alkalmasak a Feladatütemező toohello használatát.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-117">There are several scenarios that lend themselves toohello usage of Scheduler.</span></span> <span data-ttu-id="9f8a9-118">Példa:</span><span class="sxs-lookup"><span data-stu-id="9f8a9-118">For example:</span></span>

* <span data-ttu-id="9f8a9-119">*Ismétlődő alkalmazásműveletek:* A Twitteren megjelenő adatok hírcsatornába foglalása.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-119">*Recurring application actions:* Periodically gathering data from Twitter into a feed.</span></span>
* <span data-ttu-id="9f8a9-120">*Napi karbantartás*: A naplók naponta történő törlése, biztonsági másolatok készítése és egyéb karbantartási feladatok.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-120">*Daily maintenance:* Daily pruning of logs, performing backups, and other maintenance tasks.</span></span> <span data-ttu-id="9f8a9-121">Például egy rendszergazda dönthet hello adatbázis tooback 1:00 órakor</span><span class="sxs-lookup"><span data-stu-id="9f8a9-121">For example, an administrator may choose tooback up hello database at 1:00 A.M.</span></span> <span data-ttu-id="9f8a9-122">minden nap hello következő kilenc hónapban.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-122">every day for hello next nine months.</span></span>

<span data-ttu-id="9f8a9-123">A Feladatütemező toocreate lehetővé teszi, frissítése, törlése, megtekintése és feladatok kezelése és [gyűjtemények feladat](scheduler-concepts-terms.md) programozott módon, a parancsfájlok segítségével, és hello portálon.</span><span class="sxs-lookup"><span data-stu-id="9f8a9-123">Scheduler allows you toocreate, update, delete, view, and manage jobs and [job collections](scheduler-concepts-terms.md) programmatically, by using scripts, and in hello portal.</span></span>

## <a name="see-also"></a><span data-ttu-id="9f8a9-124">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9f8a9-124">See also</span></span>
 [<span data-ttu-id="9f8a9-125">Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="9f8a9-125">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="9f8a9-126">Az ütemező hello Azure-portálon az első lépéseiben</span><span class="sxs-lookup"><span data-stu-id="9f8a9-126">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="9f8a9-127">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="9f8a9-127">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="9f8a9-128">Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező</span><span class="sxs-lookup"><span data-stu-id="9f8a9-128">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="9f8a9-129">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="9f8a9-129">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="9f8a9-130">Az Azure Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="9f8a9-130">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="9f8a9-131">Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="9f8a9-131">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="9f8a9-132">Azure Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="9f8a9-132">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="9f8a9-133">Kimenő hitelesítés az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="9f8a9-133">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

