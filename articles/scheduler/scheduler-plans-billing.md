---
title: "aaaPlans és az Azure Scheduler számlázási"
description: "Csomagok és a számlázás az Azure Schedulerrel"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 051b8eeb1ea19678b3cef4db3237ebf04c8b0e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plans-and-billing-in-azure-scheduler"></a><span data-ttu-id="0c50b-103">Csomagok és a számlázás az Azure Schedulerrel</span><span class="sxs-lookup"><span data-stu-id="0c50b-103">Plans and Billing in Azure Scheduler</span></span>
## <a name="job-collection-plans"></a><span data-ttu-id="0c50b-104">Feladat gyűjtési terveket</span><span class="sxs-lookup"><span data-stu-id="0c50b-104">Job Collection Plans</span></span>
<span data-ttu-id="0c50b-105">Feladatgyűjtemények számlázható entitás hello Azure Scheduler.</span><span class="sxs-lookup"><span data-stu-id="0c50b-105">Job collections are hello billable entity in Azure Scheduler.</span></span> <span data-ttu-id="0c50b-106">Feladatgyűjtemények feladatok száma tartalmazhat, és a három csomagokban – ingyenes, Standard és Premium –, az alábbiakban található származnak.</span><span class="sxs-lookup"><span data-stu-id="0c50b-106">Job collections contain a number of jobs and come in three plans – Free, Standard, and Premium – that are described below.</span></span>

| <span data-ttu-id="0c50b-107">**Feladat gyűjtési terv**</span><span class="sxs-lookup"><span data-stu-id="0c50b-107">**Job Collection Plan**</span></span> | <span data-ttu-id="0c50b-108">**Feladat gyűjteményenként feladatok maximális száma**</span><span class="sxs-lookup"><span data-stu-id="0c50b-108">**Max # of Jobs per Job Collection**</span></span> | <span data-ttu-id="0c50b-109">**Maximális ismétlődésére**</span><span class="sxs-lookup"><span data-stu-id="0c50b-109">**Max Recurrence**</span></span> | <span data-ttu-id="0c50b-110">**Maximális Feladatgyűjteményei előfizetésenként**</span><span class="sxs-lookup"><span data-stu-id="0c50b-110">**Max Job Collections per Subscription**</span></span> | <span data-ttu-id="0c50b-111">**Korlátok**</span><span class="sxs-lookup"><span data-stu-id="0c50b-111">**Limits**</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="0c50b-112">**Ingyenes**</span><span class="sxs-lookup"><span data-stu-id="0c50b-112">**Free**</span></span> |<span data-ttu-id="0c50b-113">5 feladatok feladat gyűjteményenként</span><span class="sxs-lookup"><span data-stu-id="0c50b-113">5 jobs per job collection</span></span> |<span data-ttu-id="0c50b-114">Óránként egyszer.</span><span class="sxs-lookup"><span data-stu-id="0c50b-114">Once per hour.</span></span> <span data-ttu-id="0c50b-115">Óránként többször nem hajtható végre feladatok</span><span class="sxs-lookup"><span data-stu-id="0c50b-115">Cannot execute jobs more often than once an hour</span></span> |<span data-ttu-id="0c50b-116">Egy előfizetés mentése too1 szabad feladatgyűjteményt engedélyezett</span><span class="sxs-lookup"><span data-stu-id="0c50b-116">A subscription is allowed up too1 free job collection</span></span> |<span data-ttu-id="0c50b-117">Nem használható [HTTP kimenő engedélyezési objektum](scheduler-outbound-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="0c50b-117">Cannot use [HTTP outbound authorization object](scheduler-outbound-authentication.md)</span></span> |
| <span data-ttu-id="0c50b-118">**Standard**</span><span class="sxs-lookup"><span data-stu-id="0c50b-118">**Standard**</span></span> |<span data-ttu-id="0c50b-119">50 feladatok feladat gyűjteményenként</span><span class="sxs-lookup"><span data-stu-id="0c50b-119">50 jobs per job collection</span></span> |<span data-ttu-id="0c50b-120">Percenkénti.</span><span class="sxs-lookup"><span data-stu-id="0c50b-120">Once per minute.</span></span> <span data-ttu-id="0c50b-121">Percenként egyszer többször nem hajtható végre feladatok</span><span class="sxs-lookup"><span data-stu-id="0c50b-121">Cannot execute jobs more often than once a minute</span></span> |<span data-ttu-id="0c50b-122">Egy előfizetés mentése too100 szabványos feladatgyűjteményei engedélyezett</span><span class="sxs-lookup"><span data-stu-id="0c50b-122">A subscription is allowed up too100 standard job collections</span></span> |<span data-ttu-id="0c50b-123">Hozzáférés toofull a ütemező szolgáltatáskészlete</span><span class="sxs-lookup"><span data-stu-id="0c50b-123">Access toofull feature set of Scheduler</span></span> |
| <span data-ttu-id="0c50b-124">**P10 Premium**</span><span class="sxs-lookup"><span data-stu-id="0c50b-124">**P10 Premium**</span></span> |<span data-ttu-id="0c50b-125">50 feladatok feladat gyűjteményenként</span><span class="sxs-lookup"><span data-stu-id="0c50b-125">50 jobs per job collection</span></span> |<span data-ttu-id="0c50b-126">Percenkénti.</span><span class="sxs-lookup"><span data-stu-id="0c50b-126">Once per minute.</span></span> <span data-ttu-id="0c50b-127">Percenként egyszer többször nem hajtható végre feladatok</span><span class="sxs-lookup"><span data-stu-id="0c50b-127">Cannot execute jobs more often than once a minute</span></span> |<span data-ttu-id="0c50b-128">Egy előfizetés mentése too10, 000 P10 prémium feladatgyűjteményei engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="0c50b-128">A subscription is allowed up too10,000 P10 Premium job collections.</span></span> <span data-ttu-id="0c50b-129"><a href="mailto:wapteams@microsoft.com">Kapcsolatfelvétel</a> több.</span><span class="sxs-lookup"><span data-stu-id="0c50b-129"><a href="mailto:wapteams@microsoft.com">Contact us</a> for more.</span></span> |<span data-ttu-id="0c50b-130">Hozzáférés toofull a ütemező szolgáltatáskészlete</span><span class="sxs-lookup"><span data-stu-id="0c50b-130">Access toofull feature set of Scheduler</span></span> |
| <span data-ttu-id="0c50b-131">**P20 prémium**</span><span class="sxs-lookup"><span data-stu-id="0c50b-131">**P20 Premium**</span></span> |<span data-ttu-id="0c50b-132">1000 feladatok feladat gyűjteményenként</span><span class="sxs-lookup"><span data-stu-id="0c50b-132">1000 jobs per job collection</span></span> |<span data-ttu-id="0c50b-133">Percenkénti.</span><span class="sxs-lookup"><span data-stu-id="0c50b-133">Once per minute.</span></span> <span data-ttu-id="0c50b-134">Percenként egyszer többször nem hajtható végre feladatok</span><span class="sxs-lookup"><span data-stu-id="0c50b-134">Cannot execute jobs more often than once a minute</span></span> |<span data-ttu-id="0c50b-135">Egy előfizetés mentése too10, 000 P20 prémium feladatgyűjteményei engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="0c50b-135">A subscription is allowed up too10,000 P20 Premium job collections.</span></span> <span data-ttu-id="0c50b-136"><a href="mailto:wapteams@microsoft.com">Kapcsolatfelvétel</a> több.</span><span class="sxs-lookup"><span data-stu-id="0c50b-136"><a href="mailto:wapteams@microsoft.com">Contact us</a> for more.</span></span> |<span data-ttu-id="0c50b-137">Hozzáférés toofull a ütemező szolgáltatáskészlete</span><span class="sxs-lookup"><span data-stu-id="0c50b-137">Access toofull feature set of Scheduler</span></span> |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a><span data-ttu-id="0c50b-138">Frissítések és a feladat gyűjtési terveket Downgrades</span><span class="sxs-lookup"><span data-stu-id="0c50b-138">Upgrades and Downgrades of Job Collection Plans</span></span>
<span data-ttu-id="0c50b-139">Előfordulhat, hogy frissítse vagy visszaminősítését egy feladat gyűjtési terv bármikor hello ingyenes, a Standard és Premium tervek között.</span><span class="sxs-lookup"><span data-stu-id="0c50b-139">You may upgrade or downgrade a job collection plan anytime among hello Free, Standard, and Premium plans.</span></span> <span data-ttu-id="0c50b-140">Azonban ha alacsonyabb verziójúra változtatása tooa szabad feladatgyűjteményt, hello alacsonyabb szintre való visszalépést sikertelenek lehetnek hello a következő okok valamelyike miatt:</span><span class="sxs-lookup"><span data-stu-id="0c50b-140">However, when downgrading tooa free job collection, hello downgrade may fail for one of hello following reasons:</span></span>

* <span data-ttu-id="0c50b-141">Hello előfizetésben már tartalmaz szabad feladatgyűjteményt van</span><span class="sxs-lookup"><span data-stu-id="0c50b-141">A free job collection already exists in hello subscription</span></span>
* <span data-ttu-id="0c50b-142">A feladat, hello feladatgyűjteményben rendelkezik egy magasabb ismétlődési, mint a feladatok az ingyenes feladatgyűjtemények megengedett.</span><span class="sxs-lookup"><span data-stu-id="0c50b-142">A job in hello job collection has a higher recurrence than allowed for jobs in free job collections.</span></span> <span data-ttu-id="0c50b-143">hello tartalmaz szabad feladatgyűjteményt engedélyezett maximális ismétlődésére van óránként egyszer</span><span class="sxs-lookup"><span data-stu-id="0c50b-143">hello maximum recurrence allowed in a free job collection is once per hour</span></span>
* <span data-ttu-id="0c50b-144">5-nél több feladatok hello feladatgyűjteményben nincsenek</span><span class="sxs-lookup"><span data-stu-id="0c50b-144">There are more than 5 jobs in hello job collection</span></span>
* <span data-ttu-id="0c50b-145">A feladat hello feladatgyűjteményben rendelkezik HTTP vagy HTTPS PROTOKOLLT használó művelet egy [HTTP kimenő engedélyezési objektum](scheduler-outbound-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="0c50b-145">A job in hello job collection has an HTTP or HTTPS action that uses an [HTTP outbound authorization object](scheduler-outbound-authentication.md)</span></span>

## <a name="billing-and-azure-plans"></a><span data-ttu-id="0c50b-146">Számlázási és az Azure-csomagok</span><span class="sxs-lookup"><span data-stu-id="0c50b-146">Billing and Azure Plans</span></span>
<span data-ttu-id="0c50b-147">Előfizetések nem szabad felszámított feladatgyűjteményei.</span><span class="sxs-lookup"><span data-stu-id="0c50b-147">Subscriptions are not charged for free job collections.</span></span> <span data-ttu-id="0c50b-148">Ha több mint 100 szabványos feladatgyűjteményei (10 szabványos, számlázási egységek), akkor célszerű egy jobb üzlet toohave összes sikertelen feladat-gyűjtemények hello prémium csomagban.</span><span class="sxs-lookup"><span data-stu-id="0c50b-148">If you have more than 100 standard job collections (10 standard billing units), then it's a better deal toohave all job collections in hello premium plan.</span></span>

<span data-ttu-id="0c50b-149">Ha egy szabványos feladatgyűjtemény és egy prémium szintű feladatgyűjtemény,-e számlázott egy standard számlázási egység *és* egy prémium szintű számlázási egységet.</span><span class="sxs-lookup"><span data-stu-id="0c50b-149">If you have one standard job collection and one premium job collection, you are billed one standard billing unit *and* one premium billing unit.</span></span> <span data-ttu-id="0c50b-150">hello Feladatütemező szolgáltatás váltók hello tooeither standard vagy prémium; beállított aktív feladat gyűjtemények száma alapján Ennek a magyarázatát a következő két szakasz hello további.</span><span class="sxs-lookup"><span data-stu-id="0c50b-150">hello Scheduler service bills based on hello number of active job collections that are set tooeither standard or premium; this is explained further in hello next two sections.</span></span>

## <a name="standard-billable-units"></a><span data-ttu-id="0c50b-151">Standard számlázható egység</span><span class="sxs-lookup"><span data-stu-id="0c50b-151">Standard Billable Units</span></span>
<span data-ttu-id="0c50b-152">Egy szabványos számlázható egység fel too10 szabványos feladatgyűjteményei tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="0c50b-152">A standard billable unit can include up too10 standard job collections.</span></span> <span data-ttu-id="0c50b-153">Mivel a szabványos feladatgyűjtemény feladat gyűjteményenként too50 feladatot is rendelkeznek, egy standard számlázási egységet lehetővé teszi, hogy egy előfizetés toohave too500 feladatot – tooalmost 22 millió feladat végrehajtások havonta fel.</span><span class="sxs-lookup"><span data-stu-id="0c50b-153">Since a standard job collection can have up too50 jobs per job collection, one standard billing unit allows a subscription toohave up too500 jobs – up tooalmost 22 million job executions per month.</span></span>

<span data-ttu-id="0c50b-154">Ha szabványos feladatgyűjteményei 1 és 10 között, akkor 1 standard számlázási egység fogjuk számlázni.</span><span class="sxs-lookup"><span data-stu-id="0c50b-154">If you have between 1 and 10 standard job collections, you'll be billed for 1 standard billing unit.</span></span> <span data-ttu-id="0c50b-155">Ha szabványos feladatgyűjteményei 11 és 20 közötti, hogy 2 standard számlázási egység fogjuk számlázni.</span><span class="sxs-lookup"><span data-stu-id="0c50b-155">If you have between 11 and 20 standard job collections, you'll be billed for 2 standard billing units.</span></span> <span data-ttu-id="0c50b-156">Ha 21 és 30 szabványos feladatgyűjteményei között, 3 standard számlázási egység fogjuk számlázni, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="0c50b-156">If you have between 21 and 30 standard job collections, you'll be billed for 3 standard billing units, and so on.</span></span>

## <a name="p10-premium-billable-units"></a><span data-ttu-id="0c50b-157">P10 Prémium számlázható egységek</span><span class="sxs-lookup"><span data-stu-id="0c50b-157">P10 Premium Billable Units</span></span>
<span data-ttu-id="0c50b-158">Egy P10 prémium számlázható egység fel too10, 000 P10 prémium feladatgyűjteményei tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="0c50b-158">A P10 premium billable unit can include up too10,000 P10 premium job collections.</span></span> <span data-ttu-id="0c50b-159">Mivel egy P10 prémium feladatgyűjtemény feladat gyűjteményenként too50 feladatot is rendelkeznek, egy prémium szintű számlázási egységet lehetővé teszi, hogy egy előfizetés toohave too500, 000 feladatok be – tooalmost 22 milliárd feladat végrehajtások havonta fel.</span><span class="sxs-lookup"><span data-stu-id="0c50b-159">Since a P10 premium job collection can have up too50 jobs per job collection, one premium billing unit allows a subscription toohave up too500,000 jobs – up tooalmost 22 billion job executions per month.</span></span>

<span data-ttu-id="0c50b-160">Ha prémium szintű feladat gyűjtemények 1 és 10 000 között, akkor 1 P10 prémium számlázási egység fogjuk számlázni.</span><span class="sxs-lookup"><span data-stu-id="0c50b-160">If you have between 1 and 10,000 premium job collections, you'll be billed for 1 P10 premium billing unit.</span></span> <span data-ttu-id="0c50b-161">Ha 10,001 és 20 000 prémium feladatgyűjteményei között, 2 P10 prémium számlázási egység fogjuk számlázni, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="0c50b-161">If you have between 10,001 and 20,000 premium job collections, you'll be billed for 2 P10 premium billing units, and so on.</span></span>

<span data-ttu-id="0c50b-162">Ebből kifolyólag a gyűjteményeknél P10 prémium feladat hello ugyanezeket a funkciókat, hello szabványos feladatgyűjteményei de adjon meg ár szünet abban az esetben, ha az alkalmazás által igényelt feladatgyűjteményei számos.</span><span class="sxs-lookup"><span data-stu-id="0c50b-162">Thus, P10 premium job collections have hello same functionality as hello standard job collections but provide a price break in case your application requires a lot of job collections.</span></span>

## <a name="p20-premium-billable-units"></a><span data-ttu-id="0c50b-163">P20 Prémium számlázható egységek</span><span class="sxs-lookup"><span data-stu-id="0c50b-163">P20 Premium Billable Units</span></span>
<span data-ttu-id="0c50b-164">P20 prémium számlázható egység fel too5, 000 P20 prémium feladatgyűjteményei tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="0c50b-164">A P20 premium billable unit can include up too5,000 P20 premium job collections.</span></span> <span data-ttu-id="0c50b-165">Óta egy P20 prémium feladatgyűjtemény legfeljebb too1, 000 feladatok feladat gyűjteményenként tartalmazhat egy prémium szintű számlázási egységet lehetővé teszi, hogy egy előfizetés toohave be too5, 000 000 feladatok – tooalmost 220 milliárd feladat végrehajtások havonta fel.</span><span class="sxs-lookup"><span data-stu-id="0c50b-165">Since a P20 premium job collection can have up too1,000 jobs per job collection, one premium billing unit allows a subscription toohave up too5,000,000 jobs – up tooalmost 220 billion job executions per month.</span></span>

<span data-ttu-id="0c50b-166">P20 prémium feladatgyűjteményei hello P10 prémium feladatgyűjteményeket, ugyanazokat a képességeket biztosít, azonban további méretezhetőséget / feladatgyűjtemény és egy nagyobb feladatok teljes száma általános P10 prémium így Ön toohave-nál nagyobb számú feladatok is támogatja.</span><span class="sxs-lookup"><span data-stu-id="0c50b-166">P20 premium job collections provides hello same capabilities as P10 premium job collections but also supports a greater number jobs per job collection and a greater total number of jobs overall than P10 premium allowing you toohave more scalability.</span></span>

## <a name="billing-and-active-status"></a><span data-ttu-id="0c50b-167">Számlázási és aktív állapota</span><span class="sxs-lookup"><span data-stu-id="0c50b-167">Billing and Active Status</span></span>
<span data-ttu-id="0c50b-168">Feladatgyűjtemények mindig aktívak, kivéve, ha a teljes előfizetés toobilling problémák miatt egyes ideiglenes letiltott állapotba állapotba került.</span><span class="sxs-lookup"><span data-stu-id="0c50b-168">Job collections are always active unless your entire subscription has gone into some temporary disabled state due toobilling issues.</span></span> <span data-ttu-id="0c50b-169">hello csak az, hogy a feladatgyűjtemény nem lesz számlázva módon tooensure tooeither állítsa toohello *szabad* terv vagy toodelete hello feladatgyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="0c50b-169">hello only way tooensure that a job collection is not billed is tooeither set it toohello *Free* plan or toodelete hello job collection.</span></span>

<span data-ttu-id="0c50b-170">Bár előfordulhat, hogy letiltja a feladatokhoz minden esetben a feladatgyűjtemény egyetlen műveletben, nem változtatja meg hello feladatgyűjtemény hello számlázási állapota – hello feladatgyűjtemény fog *továbbra is* kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="0c50b-170">Although you may disable all jobs within a job collection in a single operation, it does not change hello billing status of hello job collection – hello job collection will *still* be billed.</span></span> <span data-ttu-id="0c50b-171">Hasonlóképpen üres feladatgyűjteményei aktív számít, és lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="0c50b-171">Similarly, empty job collections are considered active and will be billed.</span></span>

## <a name="pricing"></a><span data-ttu-id="0c50b-172">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="0c50b-172">Pricing</span></span>
<span data-ttu-id="0c50b-173">A díjszabás részleteit, lásd: [Feladatütemező árképzési](https://azure.microsoft.com/pricing/details/scheduler/).</span><span class="sxs-lookup"><span data-stu-id="0c50b-173">For pricing details, please see [Scheduler Pricing](https://azure.microsoft.com/pricing/details/scheduler/).</span></span>

## <a name="see-also"></a><span data-ttu-id="0c50b-174">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0c50b-174">See Also</span></span>
 [<span data-ttu-id="0c50b-175">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="0c50b-175">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="0c50b-176">Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="0c50b-176">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="0c50b-177">Az ütemező hello Azure-portálon az első lépéseiben</span><span class="sxs-lookup"><span data-stu-id="0c50b-177">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="0c50b-178">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="0c50b-178">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="0c50b-179">Az Azure Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="0c50b-179">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="0c50b-180">Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="0c50b-180">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="0c50b-181">Azure Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="0c50b-181">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="0c50b-182">Kimenő hitelesítés az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="0c50b-182">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)
