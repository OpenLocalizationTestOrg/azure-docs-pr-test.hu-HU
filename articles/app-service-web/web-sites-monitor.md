---
title: "Az Azure App Service-alkalmazások figyelése |} Microsoft Docs"
description: "Útmutató az Azure App Service-alkalmazások figyelése az Azure portál használatával."
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
ms.openlocfilehash: 25d3776920d683fffedcd8ac6ed0e84dfe875974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a><span data-ttu-id="ea4b7-103">Útmutató: az Azure App Service-alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="ea4b7-103">How to: Monitor Apps in Azure App Service</span></span>
<span data-ttu-id="ea4b7-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a beépített felügyeleti funkciókat biztosítja a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ea4b7-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) provides built in monitoring functionality in the [Azure Portal](https://portal.azure.com).</span></span>
<span data-ttu-id="ea4b7-105">Ez lehetővé teszi a tekintse át **kvóták** és **metrikák** egy alkalmazást, valamint az App Service-csomag, beállítása **riasztások** és még **skálázás** alapján automatikusan metrikákat.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-105">This includes the ability to review **quotas** and **metrics** for an app as well as the App Service plan, setting up **alerts** and even **scaling** automatically based on these metrics.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a><span data-ttu-id="ea4b7-106">Understanding kvóták és metrikák</span><span class="sxs-lookup"><span data-stu-id="ea4b7-106">Understanding Quotas and Metrics</span></span>
### <a name="quotas"></a><span data-ttu-id="ea4b7-107">Kvóták</span><span class="sxs-lookup"><span data-stu-id="ea4b7-107">Quotas</span></span>
<span data-ttu-id="ea4b7-108">Az App Service-ben üzemeltetett alkalmazások vannak bizonyos *korlátok* használhatnak erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-108">Applications hosted in App Service are subject to certain *limits* on the resources they can use.</span></span> <span data-ttu-id="ea4b7-109">Határozza meg a korlátok a **App Service-csomag** az alkalmazással társított alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-109">The limits are defined by the **App Service plan** associated with the app.</span></span>

<span data-ttu-id="ea4b7-110">Ha az alkalmazás a egy **szabad** vagy **megosztott** tervezi, akkor határozza meg az erőforrások, az alkalmazás használati korlátait **kvóták**.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-110">If the application is hosted in a **Free** or **Shared** plan, then the limits on the resources the app can use are defined by **Quotas**.</span></span>

<span data-ttu-id="ea4b7-111">Ha az alkalmazás a egy **alapvető**, **szabványos** vagy **prémium** tervez, használati korlátait használhatják az erőforrásokat-beállításokat, majd a **mérete** (kis, közepes, nagy) és **példány száma** (1, 2, 3,...), a **App Service-csomag**.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-111">If the application is hosted in a **Basic**, **Standard** or **Premium** plan, then the limits on the resources they can use are set by the **size** (Small, Medium, Large) and **instance count** (1, 2, 3, ...) of the **App Service plan**.</span></span>

<span data-ttu-id="ea4b7-112">**Kvóták** a **szabad** vagy **megosztott** -alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="ea4b7-112">**Quotas** for **Free** or **Shared** apps are:</span></span>

* <span data-ttu-id="ea4b7-113">**CPU(Short)**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-113">**CPU(Short)**</span></span>
  * <span data-ttu-id="ea4b7-114">Az alkalmazás egy 5 perces időszakban engedélyezett CPU mennyisége.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-114">Amount of CPU allowed for this application in a 5-minute interval.</span></span> <span data-ttu-id="ea4b7-115">Ez a kvóta újra 5 percenként állítja be.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-115">This quota re-sets every 5 minutes.</span></span>
* <span data-ttu-id="ea4b7-116">**CPU(Day)**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-116">**CPU(Day)**</span></span>
  * <span data-ttu-id="ea4b7-117">Ehhez az alkalmazáshoz egy napon belül engedélyezett Processzor teljes mennyisége.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-117">Total amount of CPU allowed for this application in a day.</span></span> <span data-ttu-id="ea4b7-118">Ez a kvóta újra beállítja az UTC idő szerint éjfélkor 24 óránként.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-118">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="ea4b7-119">**Memória**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-119">**Memory**</span></span>
  * <span data-ttu-id="ea4b7-120">Teljes memóriamennyiség engedélyezett ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-120">Total amount of memory allowed for this application.</span></span>
* <span data-ttu-id="ea4b7-121">**Sávszélesség**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-121">**Bandwidth**</span></span>
  * <span data-ttu-id="ea4b7-122">Kimenő sávszélesség engedélyezett ehhez az alkalmazáshoz egy nap teljes mennyisége.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-122">Total amount of outgoing bandwidth allowed for this application in a day.</span></span>
    <span data-ttu-id="ea4b7-123">Ez a kvóta újra beállítja az UTC idő szerint éjfélkor 24 óránként.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-123">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="ea4b7-124">**Fájlrendszer**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-124">**Filesystem**</span></span>
  * <span data-ttu-id="ea4b7-125">Teljes méretű tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-125">Total amount of storage allowed.</span></span>

<span data-ttu-id="ea4b7-126">A futó alkalmazások alkalmazandó csak kvóta **alapvető**, **szabványos** és **prémium** tervek van **fájlrendszer**.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-126">The only quota applicable to apps hosted on **Basic**, **Standard** and **Premium** plans is **Filesystem**.</span></span>

<span data-ttu-id="ea4b7-127">További információ a konkrét kvóták, korlátozások és funkciók érhetők el a másik App Service termékváltozatok itt található: [Azure előfizetés szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md#app-service-limits)</span><span class="sxs-lookup"><span data-stu-id="ea4b7-127">More information about the specific quotas, limits and features available to the different App Service SKUs can be found here: [Azure Subscription Service Limits](../azure-subscription-service-limits.md#app-service-limits)</span></span>

#### <a name="quota-enforcement"></a><span data-ttu-id="ea4b7-128">Kvóta kényszerítése</span><span class="sxs-lookup"><span data-stu-id="ea4b7-128">Quota Enforcement</span></span>
<span data-ttu-id="ea4b7-129">Ha az alkalmazás a használatát meghaladja a **Processzor (rövid)**, **Processzor (nap)**, vagy **sávszélesség** kvóta, majd az alkalmazás le lesz állítva, amíg újra beállítja a kvótát.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-129">If an application in its usage exceeds the **CPU (short)**, **CPU (Day)**, or **bandwidth** quota then the application will be stopped until the quota re-sets.</span></span> <span data-ttu-id="ea4b7-130">Ebben az időszakban az összes bejövő kérelmet eredményezi egy **HTTP 403**.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-130">During this time, all incoming requests will result in an **HTTP 403**.</span></span>
![][http403]

<span data-ttu-id="ea4b7-131">Ha az alkalmazás **memória** kvóta túllépése, majd az alkalmazás indulnak újra lesz.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-131">If the application **memory** quota is exceeded, then the application will be re-started.</span></span>

<span data-ttu-id="ea4b7-132">Ha a **fájlrendszer** kvóta túllépése, akkor minden írási művelet sikertelen lesz, írás a következő naplókban is beleértve.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-132">If the **Filesystem** quota is exceeded, then any write operation will fail, this includes writing to logs.</span></span>

<span data-ttu-id="ea4b7-133">Kvóták növelhető vagy eltávolította a alkalmazás az App Service-csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-133">Quotas can be increased or removed from your app by upgrading your App Service plan.</span></span>

### <a name="metrics"></a><span data-ttu-id="ea4b7-134">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="ea4b7-134">Metrics</span></span>
<span data-ttu-id="ea4b7-135">**Metrikák** információval szolgálnak az alkalmazást, illetve az App Service-csomag viselkedését.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-135">**Metrics** provide information about the app, or App Service plan's behavior.</span></span>

<span data-ttu-id="ea4b7-136">Az egy **alkalmazás**, a rendelkezésre álló adatok gyűjtése le van:</span><span class="sxs-lookup"><span data-stu-id="ea4b7-136">For an **Application**, the available metrics are:</span></span>

* <span data-ttu-id="ea4b7-137">**Átlagos válaszidő**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-137">**Average Response Time**</span></span>
  * <span data-ttu-id="ea4b7-138">Átlagos ideje az alkalmazásnak, hogy az ms-kérelmek kiszolgálását.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-138">The average time taken for the app to serve requests in ms.</span></span>
* <span data-ttu-id="ea4b7-139">**Munkakészlet átlagos memória**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-139">**Average memory working set**</span></span>
  * <span data-ttu-id="ea4b7-140">A MIB-adatbázisból az alkalmazás által használt memória átlagos mennyisége.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-140">The average amount of memory in MiBs used by the app.</span></span>
* <span data-ttu-id="ea4b7-141">**CPU-idő**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-141">**CPU Time**</span></span>
  * <span data-ttu-id="ea4b7-142">Az alkalmazás által felhasznált másodpercben CPU összege.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-142">The amount of CPU in seconds consumed by the app.</span></span> <span data-ttu-id="ea4b7-143">További információ a metrika lásd: [CPU-időt és a CPU százaléka](#cpu-time-vs-cpu-percentage)</span><span class="sxs-lookup"><span data-stu-id="ea4b7-143">For more information about this metric see: [CPU time vs CPU percentage](#cpu-time-vs-cpu-percentage)</span></span>
* <span data-ttu-id="ea4b7-144">**Az adatok**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-144">**Data In**</span></span>
  * <span data-ttu-id="ea4b7-145">A MIB-adatbázisból az alkalmazás által használt bejövő sávszélesség mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-145">The amount of incoming bandwidth consumed by the app in MiBs.</span></span>
* <span data-ttu-id="ea4b7-146">**Kimenő adatforgalmat**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-146">**Data Out**</span></span>
  * <span data-ttu-id="ea4b7-147">A MIB-adatbázisból az alkalmazás által felhasznált kimenő sávszélesség mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-147">The amount of outgoing bandwidth consumed by the app in MiBs.</span></span>
* <span data-ttu-id="ea4b7-148">**HTTP 2xx**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-148">**Http 2xx**</span></span>
  * <span data-ttu-id="ea4b7-149">A http-állapotkódot eredményez kérelmek száma > = 200, de < 300.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-149">Count of requests resulting in a http status code >= 200 but < 300.</span></span>
* <span data-ttu-id="ea4b7-150">**HTTP 3xx**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-150">**Http 3xx**</span></span>
  * <span data-ttu-id="ea4b7-151">A http-állapotkódot eredményez kérelmek száma > = < 400, de 300.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-151">Count of requests resulting in a http status code >= 300 but < 400.</span></span>
* <span data-ttu-id="ea4b7-152">**HTTP 401-es**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-152">**Http 401**</span></span>
  * <span data-ttu-id="ea4b7-153">HTTP 401-es állapotkód eredményezve kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-153">Count of requests resulting in HTTP 401 status code.</span></span>
* <span data-ttu-id="ea4b7-154">**A HTTP 403**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-154">**Http 403**</span></span>
  * <span data-ttu-id="ea4b7-155">403-as HTTP-állapotkódot eredményez kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-155">Count of requests resulting in HTTP 403 status code.</span></span>
* <span data-ttu-id="ea4b7-156">**HTTP 404-es**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-156">**Http 404**</span></span>
  * <span data-ttu-id="ea4b7-157">HTTP 404 állapotkódot eredményez kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-157">Count of requests resulting in HTTP 404 status code.</span></span>
* <span data-ttu-id="ea4b7-158">**406 http**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-158">**Http 406**</span></span>
  * <span data-ttu-id="ea4b7-159">HTTP 406-állapotkódot eredményez kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-159">Count of requests resulting in HTTP 406 status code.</span></span>
* <span data-ttu-id="ea4b7-160">**HTTP 4xx**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-160">**Http 4xx**</span></span>
  * <span data-ttu-id="ea4b7-161">A http-állapotkódot eredményez kérelmek száma > = 400, de a < 500.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-161">Count of requests resulting in a http status code >= 400 but < 500.</span></span>
* <span data-ttu-id="ea4b7-162">**HTTP-kiszolgáló hibák**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-162">**Http Server Errors**</span></span>
  * <span data-ttu-id="ea4b7-163">A http-állapotkódot eredményez kérelmek száma > = 500, de a < 600.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-163">Count of requests resulting in a http status code >= 500 but < 600.</span></span>
* <span data-ttu-id="ea4b7-164">**Memória munkakészlete**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-164">**Memory working set**</span></span>
  * <span data-ttu-id="ea4b7-165">A MIB-adatbázisból az alkalmazás által használt memória pillanatnyi mérete.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-165">Current amount of memory used by the app in MiBs.</span></span>
* <span data-ttu-id="ea4b7-166">**Kérések**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-166">**Requests**</span></span>
  * <span data-ttu-id="ea4b7-167">Az eredményül kapott HTTP-állapotkód függetlenül kérelmek teljes száma.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-167">Total number of requests regardless of their resulting HTTP status code.</span></span>

<span data-ttu-id="ea4b7-168">Az egy **App Service-csomag**, a rendelkezésre álló adatok gyűjtése le van:</span><span class="sxs-lookup"><span data-stu-id="ea4b7-168">For an **App Service plan**, the available metrics are:</span></span>

> [!NOTE]
> <span data-ttu-id="ea4b7-169">App Service-csomag metrikái csak érhetők el a tervek **alapvető**, **szabványos** és **prémium** Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-169">App Service plan metrics are only available for plans in **Basic**, **Standard** and **Premium** SKU.</span></span>
> 
> 

* <span data-ttu-id="ea4b7-170">**Processzor**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-170">**CPU Percentage**</span></span>
  * <span data-ttu-id="ea4b7-171">Az átlagos CPU, a csomag összes példányán keresztül használt.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-171">The average CPU used across all instances of the plan.</span></span>
* <span data-ttu-id="ea4b7-172">**Memóriahasználat (%)**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-172">**Memory Percentage**</span></span>
  * <span data-ttu-id="ea4b7-173">A csomag összes példányán keresztül használt átlagos memória.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-173">The average memory used across all instances of the plan.</span></span>
* <span data-ttu-id="ea4b7-174">**Az adatok**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-174">**Data In**</span></span>
  * <span data-ttu-id="ea4b7-175">A csomag összes példányán keresztül használt átlagos bejövő sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-175">The average incoming bandwidth used across all instances of the plan.</span></span>
* <span data-ttu-id="ea4b7-176">**Kimenő adatforgalmat**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-176">**Data Out**</span></span>
  * <span data-ttu-id="ea4b7-177">A csomag összes példányán keresztül használt sávszélesség kimenő átlaga.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-177">The average outgoing bandwidth used across all instances of the plan.</span></span>
* <span data-ttu-id="ea4b7-178">**Lemezvárólista hossza**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-178">**Disk Queue Length**</span></span>
  * <span data-ttu-id="ea4b7-179">Átlagos olvasási és írási váró kérelmek tárolón.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-179">The average number of both read and write requests that were queued on storage.</span></span> <span data-ttu-id="ea4b7-180">A magas Lemezvárólista hossza arra utal, hogy egy alkalmazás, amely túlzott lemezes i/o-miatt előfordulhat, hogy lehet lelassult.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-180">A high disk queue length is an indication of an application that might be slowing down due to excessive disk I/O.</span></span>
* <span data-ttu-id="ea4b7-181">**HTTP-várólista hossza**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-181">**Http Queue Length**</span></span>
  * <span data-ttu-id="ea4b7-182">Volna, hogy a várólista előtt teljesítését HTTP-kérelmek átlagos száma.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-182">The average number of HTTP requests that had to sit on the queue before being fulfilled.</span></span> <span data-ttu-id="ea4b7-183">Egy magas vagy növekvő HTTP-várólista hossza a jelenség a terv túl nagy terhelés alatt.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-183">A high or increasing HTTP Queue length is a symptom of a plan under heavy load.</span></span>

### <a name="cpu-time-vs-cpu-percentage"></a><span data-ttu-id="ea4b7-184">CPU-időt és a CPU százaléka</span><span class="sxs-lookup"><span data-stu-id="ea4b7-184">CPU time vs CPU percentage</span></span>
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

<span data-ttu-id="ea4b7-185">Nincsenek 2 metrikákat, amelyek CPU-használat tükrözik.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-185">There are 2 metrics that reflect CPU usage.</span></span> <span data-ttu-id="ea4b7-186">**CPU-idő** és **processzor**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-186">**CPU time** and **CPU percentage**</span></span>

<span data-ttu-id="ea4b7-187">**CPU-idő** akkor hasznos, az üzemeltetett alkalmazások **szabad** vagy **megosztott** tervek, mert azok közül az alkalmazás által használt CPU percben értendő.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-187">**CPU Time** is useful for apps hosted in **Free** or **Shared** plans since one of their quotas is defined in CPU minutes used by the app.</span></span>

<span data-ttu-id="ea4b7-188">**Processzor** viszont akkor hasznos, az üzemeltetett alkalmazások **alapvető**, **szabványos** és **prémium** tervek, mert azok kiterjeszthető, és ez a mérőszám a teljes használati jól jelzi minden példányára.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-188">**CPU percentage** on the other hand is useful for apps hosted in **basic**, **standard** and **premium** plans since they can be scaled out and this metric is a good indication of the overall usage across all instances.</span></span>

## <a name="metrics-granularity-and-retention-policy"></a><span data-ttu-id="ea4b7-189">Metrikák Granularitási és az adatmegőrzési házirend</span><span class="sxs-lookup"><span data-stu-id="ea4b7-189">Metrics Granularity and Retention Policy</span></span>
<span data-ttu-id="ea4b7-190">Az alkalmazás és az app service-csomag metrikáinak naplózza, és a szolgáltatás a következő Granularitás van, és az adatmegőrzési összesítve:</span><span class="sxs-lookup"><span data-stu-id="ea4b7-190">Metrics for an application and app service plan are logged and aggregated by the service with the following granularities and retention policies:</span></span>

* <span data-ttu-id="ea4b7-191">**Perc** granularitási metrikák megmaradnak a **48 óra**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-191">**Minute** granularity metrics are retained for **48 hours**</span></span>
* <span data-ttu-id="ea4b7-192">**Óra** granularitási metrikák megmaradnak a **30 napban**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-192">**Hour** granularity metrics are retained for **30 days**</span></span>
* <span data-ttu-id="ea4b7-193">**Nap** granularitási metrikák megmaradnak a **90 nap**</span><span class="sxs-lookup"><span data-stu-id="ea4b7-193">**Day** granularity metrics are retained for **90 days**</span></span>

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a><span data-ttu-id="ea4b7-194">Figyelési kvótái és az Azure portálon metrikákat.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-194">Monitoring Quotas and Metrics in the Azure Portal.</span></span>
<span data-ttu-id="ea4b7-195">Megtekintheti a különböző állapotának **kvóták** és **metrikák** érintő az alkalmazás a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ea4b7-195">You can review the status of the different **quotas** and **metrics** affecting an application in the [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="ea4b7-196">![][quotas]
**Kvóták** található a beállítások >**kvóták**.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-196">![][quotas]
**Quotas** can be found under Settings>**Quotas**.</span></span> <span data-ttu-id="ea4b7-197">A UX lehetővé teszi, hogy tekintse át: (1) a kvóták nevét, (2) a átállítási időközt, (3) a jelenlegi korlátozását és (4) aktuális értékét.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-197">The UX allows you to review: (1) the quotas name, (2) its reset interval, (3) its current limit and (4) current value.</span></span>

<span data-ttu-id="ea4b7-198">![][metrics]
**Metrikák** közvetlenül az erőforrás panel való hozzáférést lehet.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-198">![][metrics]
**Metrics** can be access directly from the resource blade.</span></span> <span data-ttu-id="ea4b7-199">Testre szabhatja a diagram: (1) **kattintson** , és válassza ki (2) **diagram szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-199">You can also customize the chart by: (1) **click** on it, and select (2) **edit chart**.</span></span>
<span data-ttu-id="ea4b7-200">Itt módosíthatja a (3) **időtartománynak**, (4) **diagramtípus**, és (5) **metrikák** megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-200">From here you can change the (3) **time range**, (4) **chart type**, and (5) **metrics** to display.</span></span>  

<span data-ttu-id="ea4b7-201">Ön talál további információt itt metrikák: [szolgáltatás metrikát](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="ea4b7-201">You can learn more about metrics here: [Monitor service metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

## <a name="alerts-and-autoscale"></a><span data-ttu-id="ea4b7-202">Riasztások és automatikus skálázás</span><span class="sxs-lookup"><span data-stu-id="ea4b7-202">Alerts and Autoscale</span></span>
<span data-ttu-id="ea4b7-203">Metrikák egy alkalmazás vagy az App Service-csomag is kell csatlakoztatnia a riasztásokat, további információt, a következő témakörben: [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="ea4b7-203">Metrics for an App or App Service plan can be hooked up to alerts, to learn more about this, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span></span>

<span data-ttu-id="ea4b7-204">App Service alkalmazás üzemeltetett a basic, standard vagy prémium szintű App Service-csomagok támogatja **automatikus skálázás**.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-204">App Service apps hosted in basic, standard or premium App Service Plans support **autoscale**.</span></span> <span data-ttu-id="ea4b7-205">Ez lehetővé teszi szabályokat, amelyek az App Service-csomag metrikái figyelése és növelheti vagy csökkentheti a példányok száma, így további erőforrások, igény szerint konfigurálhatja vagy mentése pénz, ha az alkalmazás túlzott kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-205">This allows you to configure rules that monitor the App Service plan metrics and can increase or decrease the instance count providing additional resources as needed, or saving money when the application is over-provision.</span></span> <span data-ttu-id="ea4b7-206">Automatikus méretezésének Itt többet is megtudhat: [méretezési hogyan](../monitoring-and-diagnostics/insights-how-to-scale.md) és itt [ajánlott eljárások az Azure a figyelő automatikus skálázás](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span><span class="sxs-lookup"><span data-stu-id="ea4b7-206">You can learn more about auto scale here: [How to Scale](../monitoring-and-diagnostics/insights-how-to-scale.md) and here [Best practices for Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span></span>

> [!NOTE]
> <span data-ttu-id="ea4b7-207">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-207">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="ea4b7-208">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="ea4b7-208">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="ea4b7-209">A változások</span><span class="sxs-lookup"><span data-stu-id="ea4b7-209">What's changed</span></span>
* <span data-ttu-id="ea4b7-210">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="ea4b7-210">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
