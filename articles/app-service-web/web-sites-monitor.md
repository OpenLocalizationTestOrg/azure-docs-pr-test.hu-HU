---
title: az Azure App Service Apps aaaMonitor |} Microsoft Docs
description: "Ismerje meg, hogyan toomonitor alkalmazások az Azure App Service segítségével hello Azure portálon."
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 80d5a466102a894a49d04ae35aa54cc1d05a58df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a><span data-ttu-id="6e119-103">Útmutató: az Azure App Service-alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="6e119-103">How to: Monitor Apps in Azure App Service</span></span>
<span data-ttu-id="6e119-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) hello a beépített felügyeleti funkciókat biztosítja [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6e119-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) provides built in monitoring functionality in hello [Azure Portal](https://portal.azure.com).</span></span>
<span data-ttu-id="6e119-105">Ez magában foglalja a hello képességét tooreview **kvóták** és **metrikák** egy alkalmazást, valamint a hello beállítása App Service-csomag **riasztások** és még **Skálázás** alapján automatikusan metrikákat.</span><span class="sxs-lookup"><span data-stu-id="6e119-105">This includes hello ability tooreview **quotas** and **metrics** for an app as well as hello App Service plan, setting up **alerts** and even **scaling** automatically based on these metrics.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a><span data-ttu-id="6e119-106">Understanding kvóták és metrikák</span><span class="sxs-lookup"><span data-stu-id="6e119-106">Understanding Quotas and Metrics</span></span>
### <a name="quotas"></a><span data-ttu-id="6e119-107">Kvóták</span><span class="sxs-lookup"><span data-stu-id="6e119-107">Quotas</span></span>
<span data-ttu-id="6e119-108">Az App Service-ben üzemeltetett alkalmazások tulajdonos toocertain *korlátok* használhatnak erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="6e119-108">Applications hosted in App Service are subject toocertain *limits* on the resources they can use.</span></span> <span data-ttu-id="6e119-109">hello korlátokat határoznak hello **App Service-csomag** hello alkalmazással társított alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6e119-109">hello limits are defined by hello **App Service plan** associated with hello app.</span></span>

<span data-ttu-id="6e119-110">Ha a hello alkalmazás egy **szabad** vagy **megosztott** tervezi, akkor hello vonatkozó korlátozások hello erőforrások hello alkalmazás által meghatározott **kvóták**.</span><span class="sxs-lookup"><span data-stu-id="6e119-110">If hello application is hosted in a **Free** or **Shared** plan, then hello limits on hello resources hello app can use are defined by **Quotas**.</span></span>

<span data-ttu-id="6e119-111">Ha hello alkalmazás a egy **alapvető**, **szabványos** vagy **prémium** tervezi, akkor -beállításokat használhatnak hello erőforrások hello vonatkozó korlátozások hello **mérete** (Kis, közepes, nagy méretű) és **száma példány** (1, 2, 3,...) a hello **App Service-csomag**.</span><span class="sxs-lookup"><span data-stu-id="6e119-111">If hello application is hosted in a **Basic**, **Standard** or **Premium** plan, then hello limits on hello resources they can use are set by hello **size** (Small, Medium, Large) and **instance count** (1, 2, 3, ...) of hello **App Service plan**.</span></span>

<span data-ttu-id="6e119-112">**Kvóták** a **szabad** vagy **megosztott** -alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="6e119-112">**Quotas** for **Free** or **Shared** apps are:</span></span>

* <span data-ttu-id="6e119-113">**CPU(Short)**</span><span class="sxs-lookup"><span data-stu-id="6e119-113">**CPU(Short)**</span></span>
  * <span data-ttu-id="6e119-114">Az alkalmazás egy 5 perces időszakban engedélyezett CPU mennyisége.</span><span class="sxs-lookup"><span data-stu-id="6e119-114">Amount of CPU allowed for this application in a 5-minute interval.</span></span> <span data-ttu-id="6e119-115">Ez a kvóta újra 5 percenként állítja be.</span><span class="sxs-lookup"><span data-stu-id="6e119-115">This quota re-sets every 5 minutes.</span></span>
* <span data-ttu-id="6e119-116">**CPU(Day)**</span><span class="sxs-lookup"><span data-stu-id="6e119-116">**CPU(Day)**</span></span>
  * <span data-ttu-id="6e119-117">Ehhez az alkalmazáshoz egy napon belül engedélyezett Processzor teljes mennyisége.</span><span class="sxs-lookup"><span data-stu-id="6e119-117">Total amount of CPU allowed for this application in a day.</span></span> <span data-ttu-id="6e119-118">Ez a kvóta újra beállítja az UTC idő szerint éjfélkor 24 óránként.</span><span class="sxs-lookup"><span data-stu-id="6e119-118">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="6e119-119">**Memória**</span><span class="sxs-lookup"><span data-stu-id="6e119-119">**Memory**</span></span>
  * <span data-ttu-id="6e119-120">Teljes memóriamennyiség engedélyezett ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6e119-120">Total amount of memory allowed for this application.</span></span>
* <span data-ttu-id="6e119-121">**Sávszélesség**</span><span class="sxs-lookup"><span data-stu-id="6e119-121">**Bandwidth**</span></span>
  * <span data-ttu-id="6e119-122">Kimenő sávszélesség engedélyezett ehhez az alkalmazáshoz egy nap teljes mennyisége.</span><span class="sxs-lookup"><span data-stu-id="6e119-122">Total amount of outgoing bandwidth allowed for this application in a day.</span></span>
    <span data-ttu-id="6e119-123">Ez a kvóta újra beállítja az UTC idő szerint éjfélkor 24 óránként.</span><span class="sxs-lookup"><span data-stu-id="6e119-123">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="6e119-124">**Fájlrendszer**</span><span class="sxs-lookup"><span data-stu-id="6e119-124">**Filesystem**</span></span>
  * <span data-ttu-id="6e119-125">Teljes méretű tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="6e119-125">Total amount of storage allowed.</span></span>

<span data-ttu-id="6e119-126">csak kvóta alkalmazható tooapps üzemeltetett hello **alapvető**, **szabványos** és **prémium** tervek van **fájlrendszer**.</span><span class="sxs-lookup"><span data-stu-id="6e119-126">hello only quota applicable tooapps hosted on **Basic**, **Standard** and **Premium** plans is **Filesystem**.</span></span>

<span data-ttu-id="6e119-127">További információ a hello adott kvóták, korlátok és funkciók érhetők el másik App Service termékváltozatok hello itt található: [Azure előfizetés szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md#app-service-limits)</span><span class="sxs-lookup"><span data-stu-id="6e119-127">More information about hello specific quotas, limits and features available to hello different App Service SKUs can be found here: [Azure Subscription Service Limits](../azure-subscription-service-limits.md#app-service-limits)</span></span>

#### <a name="quota-enforcement"></a><span data-ttu-id="6e119-128">Kvóta kényszerítése</span><span class="sxs-lookup"><span data-stu-id="6e119-128">Quota Enforcement</span></span>
<span data-ttu-id="6e119-129">Ha az alkalmazás a használatát meghaladja hello **Processzor (rövid)**, **Processzor (nap)**, vagy **sávszélesség** kvóta majd hello alkalmazás le lesz állítva, amíg újra beállítja hello kvóta.</span><span class="sxs-lookup"><span data-stu-id="6e119-129">If an application in its usage exceeds hello **CPU (short)**, **CPU (Day)**, or **bandwidth** quota then hello application will be stopped until hello quota re-sets.</span></span> <span data-ttu-id="6e119-130">Ebben az időszakban az összes bejövő kérelmet eredményezi egy **HTTP 403**.</span><span class="sxs-lookup"><span data-stu-id="6e119-130">During this time, all incoming requests will result in an **HTTP 403**.</span></span>
![][http403]

<span data-ttu-id="6e119-131">Ha alkalmazás hello **memória** kvóta túllépése, akkor a hello alkalmazás rendszer indulnak újra.</span><span class="sxs-lookup"><span data-stu-id="6e119-131">If hello application **memory** quota is exceeded, then hello application will be re-started.</span></span>

<span data-ttu-id="6e119-132">Ha hello **fájlrendszer** kvóta túllépése, akkor minden írási művelet sikertelen lesz, beleértve toologs írása.</span><span class="sxs-lookup"><span data-stu-id="6e119-132">If hello **Filesystem** quota is exceeded, then any write operation will fail, this includes writing toologs.</span></span>

<span data-ttu-id="6e119-133">Kvóták növelhető vagy eltávolította a alkalmazás az App Service-csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="6e119-133">Quotas can be increased or removed from your app by upgrading your App Service plan.</span></span>

### <a name="metrics"></a><span data-ttu-id="6e119-134">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="6e119-134">Metrics</span></span>
<span data-ttu-id="6e119-135">**Metrikák** hello alkalmazást, illetve az App Service-csomag viselkedés ismertetik.</span><span class="sxs-lookup"><span data-stu-id="6e119-135">**Metrics** provide information about hello app, or App Service plan's behavior.</span></span>

<span data-ttu-id="6e119-136">Az egy **alkalmazás**, hello elérhető mérőszámok vannak:</span><span class="sxs-lookup"><span data-stu-id="6e119-136">For an **Application**, hello available metrics are:</span></span>

* <span data-ttu-id="6e119-137">**Átlagos válaszidő**</span><span class="sxs-lookup"><span data-stu-id="6e119-137">**Average Response Time**</span></span>
  * <span data-ttu-id="6e119-138">hello átlagos idő a hello app tooserve kéréseket az ms.</span><span class="sxs-lookup"><span data-stu-id="6e119-138">hello average time taken for hello app tooserve requests in ms.</span></span>
* <span data-ttu-id="6e119-139">**Munkakészlet átlagos memória**</span><span class="sxs-lookup"><span data-stu-id="6e119-139">**Average memory working set**</span></span>
  * <span data-ttu-id="6e119-140">a MIB hello alkalmazás által használt memória mennyisége átlagos hello.</span><span class="sxs-lookup"><span data-stu-id="6e119-140">hello average amount of memory in MiBs used by hello app.</span></span>
* <span data-ttu-id="6e119-141">**CPU-idő**</span><span class="sxs-lookup"><span data-stu-id="6e119-141">**CPU Time**</span></span>
  * <span data-ttu-id="6e119-142">hello összeg CPU hello alkalmazás által felhasznált másodpercben.</span><span class="sxs-lookup"><span data-stu-id="6e119-142">hello amount of CPU in seconds consumed by hello app.</span></span> <span data-ttu-id="6e119-143">További információ a metrika lásd: [CPU-időt és a CPU százaléka](#cpu-time-vs-cpu-percentage)</span><span class="sxs-lookup"><span data-stu-id="6e119-143">For more information about this metric see: [CPU time vs CPU percentage](#cpu-time-vs-cpu-percentage)</span></span>
* <span data-ttu-id="6e119-144">**Az adatok**</span><span class="sxs-lookup"><span data-stu-id="6e119-144">**Data In**</span></span>
  * <span data-ttu-id="6e119-145">Bejövő sávszélesség MIB hello alkalmazás által használt hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="6e119-145">hello amount of incoming bandwidth consumed by hello app in MiBs.</span></span>
* <span data-ttu-id="6e119-146">**Kimenő adatforgalmat**</span><span class="sxs-lookup"><span data-stu-id="6e119-146">**Data Out**</span></span>
  * <span data-ttu-id="6e119-147">kimenő sávszélesség MIB hello alkalmazás által használt hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="6e119-147">hello amount of outgoing bandwidth consumed by hello app in MiBs.</span></span>
* <span data-ttu-id="6e119-148">**HTTP 2xx**</span><span class="sxs-lookup"><span data-stu-id="6e119-148">**Http 2xx**</span></span>
  * <span data-ttu-id="6e119-149">A http-állapotkódot eredményez kérelmek száma > = 200, de < 300.</span><span class="sxs-lookup"><span data-stu-id="6e119-149">Count of requests resulting in a http status code >= 200 but < 300.</span></span>
* <span data-ttu-id="6e119-150">**HTTP 3xx**</span><span class="sxs-lookup"><span data-stu-id="6e119-150">**Http 3xx**</span></span>
  * <span data-ttu-id="6e119-151">A http-állapotkódot eredményez kérelmek száma > = < 400, de 300.</span><span class="sxs-lookup"><span data-stu-id="6e119-151">Count of requests resulting in a http status code >= 300 but < 400.</span></span>
* <span data-ttu-id="6e119-152">**HTTP 401-es**</span><span class="sxs-lookup"><span data-stu-id="6e119-152">**Http 401**</span></span>
  * <span data-ttu-id="6e119-153">HTTP 401-es állapotkód eredményezve kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="6e119-153">Count of requests resulting in HTTP 401 status code.</span></span>
* <span data-ttu-id="6e119-154">**A HTTP 403**</span><span class="sxs-lookup"><span data-stu-id="6e119-154">**Http 403**</span></span>
  * <span data-ttu-id="6e119-155">403-as HTTP-állapotkódot eredményez kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="6e119-155">Count of requests resulting in HTTP 403 status code.</span></span>
* <span data-ttu-id="6e119-156">**HTTP 404-es**</span><span class="sxs-lookup"><span data-stu-id="6e119-156">**Http 404**</span></span>
  * <span data-ttu-id="6e119-157">HTTP 404 állapotkódot eredményez kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="6e119-157">Count of requests resulting in HTTP 404 status code.</span></span>
* <span data-ttu-id="6e119-158">**406 http**</span><span class="sxs-lookup"><span data-stu-id="6e119-158">**Http 406**</span></span>
  * <span data-ttu-id="6e119-159">HTTP 406-állapotkódot eredményez kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="6e119-159">Count of requests resulting in HTTP 406 status code.</span></span>
* <span data-ttu-id="6e119-160">**HTTP 4xx**</span><span class="sxs-lookup"><span data-stu-id="6e119-160">**Http 4xx**</span></span>
  * <span data-ttu-id="6e119-161">A http-állapotkódot eredményez kérelmek száma > = 400, de a < 500.</span><span class="sxs-lookup"><span data-stu-id="6e119-161">Count of requests resulting in a http status code >= 400 but < 500.</span></span>
* <span data-ttu-id="6e119-162">**HTTP-kiszolgáló hibák**</span><span class="sxs-lookup"><span data-stu-id="6e119-162">**Http Server Errors**</span></span>
  * <span data-ttu-id="6e119-163">A http-állapotkódot eredményez kérelmek száma > = 500, de a < 600.</span><span class="sxs-lookup"><span data-stu-id="6e119-163">Count of requests resulting in a http status code >= 500 but < 600.</span></span>
* <span data-ttu-id="6e119-164">**Memória munkakészlete**</span><span class="sxs-lookup"><span data-stu-id="6e119-164">**Memory working set**</span></span>
  * <span data-ttu-id="6e119-165">A MIB hello alkalmazás által használt memória pillanatnyi mérete.</span><span class="sxs-lookup"><span data-stu-id="6e119-165">Current amount of memory used by hello app in MiBs.</span></span>
* <span data-ttu-id="6e119-166">**Kérések**</span><span class="sxs-lookup"><span data-stu-id="6e119-166">**Requests**</span></span>
  * <span data-ttu-id="6e119-167">Az eredményül kapott HTTP-állapotkód függetlenül kérelmek teljes száma.</span><span class="sxs-lookup"><span data-stu-id="6e119-167">Total number of requests regardless of their resulting HTTP status code.</span></span>

<span data-ttu-id="6e119-168">Az egy **App Service-csomag**, hello elérhető mérőszámok vannak:</span><span class="sxs-lookup"><span data-stu-id="6e119-168">For an **App Service plan**, hello available metrics are:</span></span>

> [!NOTE]
> <span data-ttu-id="6e119-169">App Service-csomag metrikái csak érhetők el a tervek **alapvető**, **szabványos** és **prémium** Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="6e119-169">App Service plan metrics are only available for plans in **Basic**, **Standard** and **Premium** SKU.</span></span>
> 
> 

* <span data-ttu-id="6e119-170">**Processzor**</span><span class="sxs-lookup"><span data-stu-id="6e119-170">**CPU Percentage**</span></span>
  * <span data-ttu-id="6e119-171">hello átlagos CPU hello csomag összes példányán keresztül használt.</span><span class="sxs-lookup"><span data-stu-id="6e119-171">hello average CPU used across all instances of hello plan.</span></span>
* <span data-ttu-id="6e119-172">**Memóriahasználat (%)**</span><span class="sxs-lookup"><span data-stu-id="6e119-172">**Memory Percentage**</span></span>
  * <span data-ttu-id="6e119-173">hello átlagos memória hello csomag összes példányán keresztül használt.</span><span class="sxs-lookup"><span data-stu-id="6e119-173">hello average memory used across all instances of hello plan.</span></span>
* <span data-ttu-id="6e119-174">**Az adatok**</span><span class="sxs-lookup"><span data-stu-id="6e119-174">**Data In**</span></span>
  * <span data-ttu-id="6e119-175">hello átlagos bejövő keresztül használt sávszélesség hello csomag összes példányán.</span><span class="sxs-lookup"><span data-stu-id="6e119-175">hello average incoming bandwidth used across all instances of hello plan.</span></span>
* <span data-ttu-id="6e119-176">**Kimenő adatforgalmat**</span><span class="sxs-lookup"><span data-stu-id="6e119-176">**Data Out**</span></span>
  * <span data-ttu-id="6e119-177">hello átlagos kimenő hello csomag összes példányán keresztül használt sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="6e119-177">hello average outgoing bandwidth used across all instances of hello plan.</span></span>
* <span data-ttu-id="6e119-178">**Lemezvárólista hossza**</span><span class="sxs-lookup"><span data-stu-id="6e119-178">**Disk Queue Length**</span></span>
  * <span data-ttu-id="6e119-179">hello átlagos száma olvasási és írási váró kérelmek tárolón.</span><span class="sxs-lookup"><span data-stu-id="6e119-179">hello average number of both read and write requests that were queued on storage.</span></span> <span data-ttu-id="6e119-180">A magas Lemezvárólista hossza arra utal, hogy az alkalmazás tooexcessive lemez i/o-miatt előfordulhat, hogy lehet lelassult.</span><span class="sxs-lookup"><span data-stu-id="6e119-180">A high disk queue length is an indication of an application that might be slowing down due tooexcessive disk I/O.</span></span>
* <span data-ttu-id="6e119-181">**HTTP-várólista hossza**</span><span class="sxs-lookup"><span data-stu-id="6e119-181">**Http Queue Length**</span></span>
  * <span data-ttu-id="6e119-182">hello toosit volt hello várólista előtt teljesítését HTTP-kérelmek átlagos száma.</span><span class="sxs-lookup"><span data-stu-id="6e119-182">hello average number of HTTP requests that had toosit on hello queue before being fulfilled.</span></span> <span data-ttu-id="6e119-183">Egy magas vagy növekvő HTTP-várólista hossza a jelenség a terv túl nagy terhelés alatt.</span><span class="sxs-lookup"><span data-stu-id="6e119-183">A high or increasing HTTP Queue length is a symptom of a plan under heavy load.</span></span>

### <a name="cpu-time-vs-cpu-percentage"></a><span data-ttu-id="6e119-184">CPU-időt és a CPU százaléka</span><span class="sxs-lookup"><span data-stu-id="6e119-184">CPU time vs CPU percentage</span></span>
<!-- toodo: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

<span data-ttu-id="6e119-185">Nincsenek 2 metrikákat, amelyek CPU-használat tükrözik.</span><span class="sxs-lookup"><span data-stu-id="6e119-185">There are 2 metrics that reflect CPU usage.</span></span> <span data-ttu-id="6e119-186">**CPU-idő** és **processzor**</span><span class="sxs-lookup"><span data-stu-id="6e119-186">**CPU time** and **CPU percentage**</span></span>

<span data-ttu-id="6e119-187">**CPU-idő** akkor hasznos, az üzemeltetett alkalmazások **szabad** vagy **megosztott** tervek, mert azok egyik hello alkalmazás által használt CPU percben értendő.</span><span class="sxs-lookup"><span data-stu-id="6e119-187">**CPU Time** is useful for apps hosted in **Free** or **Shared** plans since one of their quotas is defined in CPU minutes used by hello app.</span></span>

<span data-ttu-id="6e119-188">**Processzor** a hello ugyanakkor akkor hasznos, az üzemeltetett alkalmazások **alapvető**, **szabványos** és **prémium** tervek, mert azok kiterjeszthető, és ez a metrika egy jó hello oka minden példányára összesített használatát.</span><span class="sxs-lookup"><span data-stu-id="6e119-188">**CPU percentage** on hello other hand is useful for apps hosted in **basic**, **standard** and **premium** plans since they can be scaled out and this metric is a good indication of hello overall usage across all instances.</span></span>

## <a name="metrics-granularity-and-retention-policy"></a><span data-ttu-id="6e119-189">Metrikák Granularitási és az adatmegőrzési házirend</span><span class="sxs-lookup"><span data-stu-id="6e119-189">Metrics Granularity and Retention Policy</span></span>
<span data-ttu-id="6e119-190">Az alkalmazás és az app service-csomag metrikáinak naplózza, és összesítve hello szolgáltatást, amely hello Granularitás van, és az adatmegőrzési házirendek a következő:</span><span class="sxs-lookup"><span data-stu-id="6e119-190">Metrics for an application and app service plan are logged and aggregated by hello service with hello following granularities and retention policies:</span></span>

* <span data-ttu-id="6e119-191">**Perc** granularitási metrikák megmaradnak a **48 óra**</span><span class="sxs-lookup"><span data-stu-id="6e119-191">**Minute** granularity metrics are retained for **48 hours**</span></span>
* <span data-ttu-id="6e119-192">**Óra** granularitási metrikák megmaradnak a **30 napban**</span><span class="sxs-lookup"><span data-stu-id="6e119-192">**Hour** granularity metrics are retained for **30 days**</span></span>
* <span data-ttu-id="6e119-193">**Nap** granularitási metrikák megmaradnak a **90 nap**</span><span class="sxs-lookup"><span data-stu-id="6e119-193">**Day** granularity metrics are retained for **90 days**</span></span>

## <a name="monitoring-quotas-and-metrics-in-hello-azure-portal"></a><span data-ttu-id="6e119-194">Hello Azure Portal ellenőrzése kvóták és metrikákat.</span><span class="sxs-lookup"><span data-stu-id="6e119-194">Monitoring Quotas and Metrics in hello Azure Portal.</span></span>
<span data-ttu-id="6e119-195">Megtekintheti a különböző hello hello állapotának **kvóták** és **metrikák** hello kérelmet érintő [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6e119-195">You can review hello status of hello different **quotas** and **metrics** affecting an application in hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="6e119-196">![][quotas]
**Kvóták** található a beállítások >**kvóták**.</span><span class="sxs-lookup"><span data-stu-id="6e119-196">![][quotas]
**Quotas** can be found under Settings>**Quotas**.</span></span> <span data-ttu-id="6e119-197">hello UX lehetővé teszi, hogy tekintse át: (1) hello kvóták nevét, (2) a átállítási időközt, (3) a jelenlegi korlátozását és (4) aktuális értékét.</span><span class="sxs-lookup"><span data-stu-id="6e119-197">hello UX allows you to review: (1) hello quotas name, (2) its reset interval, (3) its current limit and (4) current value.</span></span>

<span data-ttu-id="6e119-198">![][metrics]
**Metrikák** közvetlenül hello erőforráspanelen való hozzáférést lehet.</span><span class="sxs-lookup"><span data-stu-id="6e119-198">![][metrics]
**Metrics** can be access directly from hello resource blade.</span></span> <span data-ttu-id="6e119-199">Testre szabhatja hello diagram: (1) **kattintson** , és válassza ki (2) **diagram szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="6e119-199">You can also customize hello chart by: (1) **click** on it, and select (2) **edit chart**.</span></span>
<span data-ttu-id="6e119-200">Itt módosíthatja a hello (3) **időtartománynak**, (4) **diagramtípus**, és (5) **metrikák** toodisplay.</span><span class="sxs-lookup"><span data-stu-id="6e119-200">From here you can change hello (3) **time range**, (4) **chart type**, and (5) **metrics** toodisplay.</span></span>  

<span data-ttu-id="6e119-201">Ön talál további információt itt metrikák: [szolgáltatás metrikát](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="6e119-201">You can learn more about metrics here: [Monitor service metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

## <a name="alerts-and-autoscale"></a><span data-ttu-id="6e119-202">Riasztások és automatikus skálázás</span><span class="sxs-lookup"><span data-stu-id="6e119-202">Alerts and Autoscale</span></span>
<span data-ttu-id="6e119-203">Metrikák egy alkalmazás vagy az App Service-csomag tooalerts, kaphat további toolearn kell csatlakoztatnia a következő témakörben: [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="6e119-203">Metrics for an App or App Service plan can be hooked up tooalerts, toolearn more about this, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span></span>

<span data-ttu-id="6e119-204">App Service alkalmazás üzemeltetett a basic, standard vagy prémium szintű App Service-csomagok támogatja **automatikus skálázás**.</span><span class="sxs-lookup"><span data-stu-id="6e119-204">App Service apps hosted in basic, standard or premium App Service Plans support **autoscale**.</span></span> <span data-ttu-id="6e119-205">Ez lehetővé teszi tooconfigure szabályokat, amelyek az App Service-csomag metrikái figyelése és növelhető és csökkenthető hello példányszám további erőforrások rendelkezésre bocsátása, igény szerint vagy mentése pénz túlzott rendelkezés hello alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="6e119-205">This allows you tooconfigure rules that monitor the App Service plan metrics and can increase or decrease hello instance count providing additional resources as needed, or saving money when hello application is over-provision.</span></span> <span data-ttu-id="6e119-206">Automatikus méretezésének Itt többet is megtudhat: [hogyan tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) és itt [ajánlott eljárások az Azure a figyelő automatikus skálázás](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span><span class="sxs-lookup"><span data-stu-id="6e119-206">You can learn more about auto scale here: [How tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) and here [Best practices for Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span></span>

> [!NOTE]
> <span data-ttu-id="6e119-207">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="6e119-207">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6e119-208">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="6e119-208">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="6e119-209">A változások</span><span class="sxs-lookup"><span data-stu-id="6e119-209">What's changed</span></span>
* <span data-ttu-id="6e119-210">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6e119-210">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
