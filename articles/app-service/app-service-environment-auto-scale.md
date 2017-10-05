---
title: "Automatikus skálázás és az App Service Environment-környezet 1-es verzió"
description: "Automatikus skálázás és az App Service Environment-környezet"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: f32affd285f3918feb0e893543f2a28f678b7b10
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a><span data-ttu-id="e3c9f-103">Automatikus skálázás és az App Service Environment-környezet 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="e3c9f-103">Autoscaling and App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="e3c9f-104">Ez a cikk az App Service Environment-környezet v1 tárgya.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-104">This article is about the App Service Environment v1.</span></span>  <span data-ttu-id="e3c9f-105">Az App Service-környezet, amely könnyebben használható, és nagyobb teljesítményű infrastruktúra fut egy újabb verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-105">There is a newer version of the App Service Environment that is easier  to use and runs on more powerful infrastructure.</span></span> <span data-ttu-id="e3c9f-106">További információt az új verzió útmutató a [az App Service Environment bemutatása](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="e3c9f-106">To learn more about the new version start with the [Introduction to the App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

<span data-ttu-id="e3c9f-107">Támogatja az Azure App Service-környezetek *automatikus skálázás*.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-107">Azure App Service environments support *autoscaling*.</span></span> <span data-ttu-id="e3c9f-108">Használhatja az automatikus skálázás egyedi feldolgozókészletek metrikák vagy ütemezés alapján.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-108">You can autoscale individual worker pools based on metrics or schedule.</span></span>

![A feldolgozókészleten automatikus skálázás beállításai.][intro]

<span data-ttu-id="e3c9f-110">Automatikus skálázás optimalizálja az erőforrás-használat automatikusan növekvő és zsugorítását App Service-környezet elférjen a költségvetést, és vagy betöltése.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-110">Autoscaling optimizes your resource utilization by automatically growing and shrinking an App Service environment to fit your budget and or load profile.</span></span>

## <a name="configure-worker-pool-autoscale"></a><span data-ttu-id="e3c9f-111">Munkavégző készlet automatikus skálázás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3c9f-111">Configure worker pool autoscale</span></span>
<span data-ttu-id="e3c9f-112">Az automatikus skálázás funkciói végezheti el a **beállítások** lapján a munkavégző készletét.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-112">You can access the autoscale functionality from the **Settings** tab of the worker pool.</span></span>

![A feldolgozókészleten beállítások lapján.][settings-scale]

<span data-ttu-id="e3c9f-114">Ezekből a felületet kell viszonylag jól ismert, mert ugyanazt a felhasználói élményt, hogy látni, ha az App Service-csomag méretezni.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-114">From there, the interface should be fairly familiar since it is the same experience that you see when you scale an App Service plan.</span></span> 

![Manuális skálázási beállításokat.][scale-manual]

<span data-ttu-id="e3c9f-116">Az automatikus skálázási profilok is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-116">You can also configure an autoscale profile.</span></span>

![Automatikus skálázási beállításokat.][scale-profile]

<span data-ttu-id="e3c9f-118">Automatikus skálázási profilok hasznosak a skála korlátozható.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-118">Autoscale profiles are useful to set limits on your scale.</span></span> <span data-ttu-id="e3c9f-119">Ezzel a módszerrel lehet egy egységes élmény úgy, hogy egy felső határa (2) egy alsó határ skálaértéknél (1) és egy előre jelezhető ráfordítás cap beállításával teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-119">This way, you can have a consistent performance experience by setting a lower bound scale value (1) and a predictable spend cap by setting an upper bound (2).</span></span>

![A profil beállítását.][scale-profile2]

<span data-ttu-id="e3c9f-121">Egy profil megadása után az automatikus skálázási szabályok méretezési felfelé vagy lefelé a munkavégző készletét. a profil által meghatározott határain belül található példányok száma is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-121">After you define a profile, you can add autoscale rules to scale up or down the number of instances in the worker pool within the bounds defined by the profile.</span></span> <span data-ttu-id="e3c9f-122">Automatikus skálázási szabályok metrikák alapulnak.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-122">Autoscale rules are based on metrics.</span></span>

![Skálázási szabályhoz.][scale-rule]

 <span data-ttu-id="e3c9f-124">A feldolgozókészleten vagy előtér-metrikák automatikus skálázási szabályok meghatározásához használható.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-124">Any worker pool or front-end metrics can be used to define autoscale rules.</span></span> <span data-ttu-id="e3c9f-125">Ezeket a mérési metrikák az erőforrás panel diagramokban figyelése, vagy állítson be riasztásokat a rendszer.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-125">These metrics are the same metrics you can monitor in the resource blade graphs or set alerts for.</span></span>

## <a name="autoscale-example"></a><span data-ttu-id="e3c9f-126">Automatikus skálázás – példa</span><span class="sxs-lookup"><span data-stu-id="e3c9f-126">Autoscale example</span></span>
<span data-ttu-id="e3c9f-127">Egy App Service Environment-környezet automatikus skálázás legjobb érdekében a forgatókönyv segítségével mutatható be.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-127">Autoscale of an App Service environment can best be illustrated by walking through a scenario.</span></span>

<span data-ttu-id="e3c9f-128">Ez a cikk ismerteti a szükséges szempontok, automatikus skálázás beállításához.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-128">This article explains all the necessary considerations when you set up autoscale.</span></span> <span data-ttu-id="e3c9f-129">A cikk végigvezeti az olyan műveleteket, amelyek, ha a figyelembe a automatikus skálázás App Service-környezetek, amelyek App Service Environment-környezet.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-129">The article walks you through the interactions that come into play when you factor in autoscaling App Service environments that are hosted in App Service Environment.</span></span>

### <a name="scenario-introduction"></a><span data-ttu-id="e3c9f-130">A forgatókönyv bemutatása</span><span class="sxs-lookup"><span data-stu-id="e3c9f-130">Scenario introduction</span></span>
<span data-ttu-id="e3c9f-131">Frank a sysadmin (rendszergazda), akik az App Service-környezetek át a munkaterheléseket, általa része lett telepítve a vállalati.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-131">Frank is a sysadmin for an enterprise who has migrated a portion of the workloads that he manages to an App Service environment.</span></span>

<span data-ttu-id="e3c9f-132">Az App Service Environment-környezet van konfigurálva manuális méretezési az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-132">The App Service environment is configured to manual scale as follows:</span></span>

* <span data-ttu-id="e3c9f-133">**Első végződik:** 3</span><span class="sxs-lookup"><span data-stu-id="e3c9f-133">**Front ends:** 3</span></span>
* <span data-ttu-id="e3c9f-134">**A feldolgozókészleten 1**: 10</span><span class="sxs-lookup"><span data-stu-id="e3c9f-134">**Worker pool 1**: 10</span></span>
* <span data-ttu-id="e3c9f-135">**A feldolgozókészleten 2**: 5</span><span class="sxs-lookup"><span data-stu-id="e3c9f-135">**Worker pool 2**: 5</span></span>
* <span data-ttu-id="e3c9f-136">**A feldolgozókészleten 3**: 5</span><span class="sxs-lookup"><span data-stu-id="e3c9f-136">**Worker pool 3**: 5</span></span>

<span data-ttu-id="e3c9f-137">1 feldolgozókészletek használjuk a termelési számítási feladatokhoz, feldolgozókészletek 2 és 3 feldolgozókészletek a minőségi megbízhatósági (QA) és a fejlesztői munkaterhelések szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-137">Worker pool 1 is used for production workloads, while worker pool 2 and worker pool 3 are used for quality assurance (QA) and development workloads.</span></span>

<span data-ttu-id="e3c9f-138">Az App Service-csomagokról QA és fejlesztői manuális méretezési vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-138">The App Service plans for QA and dev are configured to manual scale.</span></span> <span data-ttu-id="e3c9f-139">Az App Service-csomag üzemi automatikus skálázási értéke változások a terhelési és a forgalom kezelésére.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-139">The production App Service plan is set to autoscale to deal with variations in load and traffic.</span></span>

<span data-ttu-id="e3c9f-140">Frank tisztában van az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-140">Frank is very familiar with the application.</span></span> <span data-ttu-id="e3c9f-141">Tudja, hogy a terhelést csúcsidőszakon 9:00-kor és 18:00:00 közé esnek, mivel ez egy alkalmazottak által használt során az office-üzletági (LOB) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-141">He knows that the peak hours for load are between 9:00 AM and 6:00 PM because this is a line-of-business (LOB) application that employees use while they are in the office.</span></span> <span data-ttu-id="e3c9f-142">Ezt követően használati elutasítja azokat a felhasználókat adott napon szabásakor.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-142">Usage drops after that when users are done for that day.</span></span> <span data-ttu-id="e3c9f-143">Csúcsidőszakon kívüli nincs még néhány betöltése mert felhasználók férhetnek hozzá az alkalmazás távolról a mobileszközök és az otthoni számítógépek használatával.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-143">Outside peak hours, there is still some load because users can access the app remotely by using their mobile devices or home PCs.</span></span> <span data-ttu-id="e3c9f-144">Az üzemi App Service-csomag már be van állítva a következő szabályokat a CPU-használat alapján automatikus skálázásra:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-144">The production App Service plan is already configured to autoscale based on CPU usage with the following rules:</span></span>

![Az ÜZLETÁGI alkalmazás beállításait.][asp-scale]

| <span data-ttu-id="e3c9f-146">**Automatikus skálázási profil – létrehozását – az App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-146">**Autoscale profile – Weekdays – App Service plan**</span></span> | <span data-ttu-id="e3c9f-147">**Automatikus skálázási profil – hétvégéket – az App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-147">**Autoscale profile – Weekends – App Service plan**</span></span> |
| --- | --- |
| <span data-ttu-id="e3c9f-148">**Name:** hétköznap profil</span><span class="sxs-lookup"><span data-stu-id="e3c9f-148">**Name:** Weekday profile</span></span> |<span data-ttu-id="e3c9f-149">**Name:** hétvégi profil</span><span class="sxs-lookup"><span data-stu-id="e3c9f-149">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="e3c9f-150">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-150">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="e3c9f-151">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-151">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="e3c9f-152">**Profil:** hétköznapokon</span><span class="sxs-lookup"><span data-stu-id="e3c9f-152">**Profile:** Weekdays</span></span> |<span data-ttu-id="e3c9f-153">**Profil:** hétvégi</span><span class="sxs-lookup"><span data-stu-id="e3c9f-153">**Profile:** Weekend</span></span> |
| <span data-ttu-id="e3c9f-154">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="e3c9f-154">**Type:** Recurrence</span></span> |<span data-ttu-id="e3c9f-155">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="e3c9f-155">**Type:** Recurrence</span></span> |
| <span data-ttu-id="e3c9f-156">**Tartomány cél:** 5-20 példányok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-156">**Target range:** 5 to 20 instances</span></span> |<span data-ttu-id="e3c9f-157">**Tartomány cél:** 3 – 10 példányok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-157">**Target range:** 3 to 10 instances</span></span> |
| <span data-ttu-id="e3c9f-158">**Nap:** hétfő, kedd, szerda, csütörtök, péntek</span><span class="sxs-lookup"><span data-stu-id="e3c9f-158">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="e3c9f-159">**Nap:** , szombat és vasárnap</span><span class="sxs-lookup"><span data-stu-id="e3c9f-159">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="e3c9f-160">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="e3c9f-160">**Start time:** 9:00 AM</span></span> |<span data-ttu-id="e3c9f-161">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="e3c9f-161">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="e3c9f-162">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="e3c9f-162">**Time zone:** UTC-08</span></span> |<span data-ttu-id="e3c9f-163">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="e3c9f-163">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="e3c9f-164">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-164">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="e3c9f-165">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-165">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="e3c9f-166">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="e3c9f-166">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="e3c9f-167">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="e3c9f-167">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="e3c9f-168">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-168">**Metric:** CPU %</span></span> |<span data-ttu-id="e3c9f-169">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-169">**Metric:** CPU %</span></span> |
| <span data-ttu-id="e3c9f-170">**Művelet:** nagyobb, mint 60 %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-170">**Operation:** Greater than 60%</span></span> |<span data-ttu-id="e3c9f-171">**Művelet:** 80 %-nál nagyobb</span><span class="sxs-lookup"><span data-stu-id="e3c9f-171">**Operation:** Greater than 80%</span></span> |
| <span data-ttu-id="e3c9f-172">**Időtartam:** 5 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-172">**Duration:** 5 Minutes</span></span> |<span data-ttu-id="e3c9f-173">**Időtartam:** 10 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-173">**Duration:** 10 Minutes</span></span> |
| <span data-ttu-id="e3c9f-174">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-174">**Time aggregation:** Average</span></span> |<span data-ttu-id="e3c9f-175">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-175">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="e3c9f-176">**Művelet:** számának növeléséhez a 2. régiója</span><span class="sxs-lookup"><span data-stu-id="e3c9f-176">**Action:** Increase count by 2</span></span> |<span data-ttu-id="e3c9f-177">**Művelet:** számának növeléséhez 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-177">**Action:** Increase count by 1</span></span> |
| <span data-ttu-id="e3c9f-178">**Ritkán használt adatok le (perc):** 15</span><span class="sxs-lookup"><span data-stu-id="e3c9f-178">**Cool down (minutes):** 15</span></span> |<span data-ttu-id="e3c9f-179">**Ritkán használt adatok le (perc):** 20</span><span class="sxs-lookup"><span data-stu-id="e3c9f-179">**Cool down (minutes):** 20</span></span> |
|  | |
| <span data-ttu-id="e3c9f-180">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-180">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="e3c9f-181">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-181">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="e3c9f-182">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="e3c9f-182">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="e3c9f-183">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="e3c9f-183">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="e3c9f-184">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-184">**Metric:** CPU %</span></span> |<span data-ttu-id="e3c9f-185">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-185">**Metric:** CPU %</span></span> |
| <span data-ttu-id="e3c9f-186">**Művelet:** 30 %-nál kisebb</span><span class="sxs-lookup"><span data-stu-id="e3c9f-186">**Operation:** Less than 30%</span></span> |<span data-ttu-id="e3c9f-187">**Művelet:** 20 %-nál kisebb</span><span class="sxs-lookup"><span data-stu-id="e3c9f-187">**Operation:** Less than 20%</span></span> |
| <span data-ttu-id="e3c9f-188">**Időtartam:** 10 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-188">**Duration:** 10 minutes</span></span> |<span data-ttu-id="e3c9f-189">**Időtartam:** 15 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-189">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="e3c9f-190">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-190">**Time aggregation:** Average</span></span> |<span data-ttu-id="e3c9f-191">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-191">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="e3c9f-192">**Művelet:** számának csökkentése 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-192">**Action:** Decrease count by 1</span></span> |<span data-ttu-id="e3c9f-193">**Művelet:** számának csökkentése 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-193">**Action:** Decrease count by 1</span></span> |
| <span data-ttu-id="e3c9f-194">**Ritkán használt adatok le (perc):** 20</span><span class="sxs-lookup"><span data-stu-id="e3c9f-194">**Cool down (minutes):** 20</span></span> |<span data-ttu-id="e3c9f-195">**Ritkán használt adatok le (perc):** 10</span><span class="sxs-lookup"><span data-stu-id="e3c9f-195">**Cool down (minutes):** 10</span></span> |

### <a name="app-service-plan-inflation-rate"></a><span data-ttu-id="e3c9f-196">Az alkalmazásszolgáltatási csomag inflációja</span><span class="sxs-lookup"><span data-stu-id="e3c9f-196">App Service plan inflation rate</span></span>
<span data-ttu-id="e3c9f-197">App Service-csomagokról konfigurált automatikus skálázásra ehhez óránként sebességet.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-197">App Service plans that are configured to autoscale do so at a maximum rate per hour.</span></span> <span data-ttu-id="e3c9f-198">Ezt az értéket a megadott automatikus skálázási szabály megadott értékek alapján lehet kiszámolni.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-198">This rate can be calculated based on the values provided on the autoscale rule.</span></span>

<span data-ttu-id="e3c9f-199">Megismerés és kiszámítása a *App Service-csomag inflációja* fontos az App Service-környezet automatikus skálázás, mert a feldolgozókészleten méretezési módosításai nincsenek azonnali.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-199">Understanding and calculating the *App Service plan inflation rate* is important for App Service environment autoscale because scale changes to a worker pool are not instantaneous.</span></span>

<span data-ttu-id="e3c9f-200">Az App Service-csomag inflációja kiszámítása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-200">The App Service plan inflation rate is calculated as follows:</span></span>

![Az alkalmazásszolgáltatási csomag inflációja számítási.][ASP-Inflation]

<span data-ttu-id="e3c9f-202">Az automatikus skálázás – a hét napja profil a termelési App Service-csomag vertikális Felskálázás szabály alapján:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-202">Based on the Autoscale – Scale Up rule for the Weekday profile of the production App Service plan:</span></span>

![App Service csomag infláció arány alapján automatikus skálázás – vertikális Felskálázás szabály létrehozását.][Equation1]

<span data-ttu-id="e3c9f-204">Automatikus skálázási – a hétvégi profil a termelési App Service-csomag vertikális Felskálázás szabály esetében a képlet kellene tartoznia:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-204">In the case of the Autoscale – Scale Up rule for the Weekend profile of the production App Service plan, the formula would resolve to:</span></span>

![App Service csomag infláció arány hétvégéket automatikus skálázás – vertikális Felskálázás szabály alapján.][Equation2]

<span data-ttu-id="e3c9f-206">Ez az érték is kerülhet sor, lefelé méretezéshez műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-206">This value can also be calculated for scale-down operations.</span></span>

<span data-ttu-id="e3c9f-207">Az automatikus skálázás – a hét napja profil az App Service-csomag, az üzemi méretezési le szabály alapján ez lenne az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-207">Based on the Autoscale – Scale Down rule for the Weekday profile of the production App Service plan, this would look as follows:</span></span>

![App Service csomag infláció arány alapján automatikus skálázás – méretezési le szabály létrehozását.][Equation3]

<span data-ttu-id="e3c9f-209">Automatikus skálázási – az App Service-csomag üzemi hétvégi profiljának méretezési le szabály esetében a képlet kellene tartoznia:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-209">In the case of the Autoscale – Scale Down rule for the Weekend profile of the production App Service plan, the formula would resolve to:</span></span>  

![App Service csomag infláció arány hétvégéket automatikus skálázás – méretezési le szabály alapján.][Equation4]

<span data-ttu-id="e3c9f-211">Az App Service-csomag üzemi milyen mértékben növelhető a maximális sebesség nyolc példányok/óra a héten és négy példányok/óra a hétvégén.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-211">The production App Service plan can grow at a maximum rate of eight instances/hour during the week and four instances/hour during the weekend.</span></span> <span data-ttu-id="e3c9f-212">Azt is megjelenhetnek példányok négy példányok/óra során a hét sebességet és hat példányok/óra során tartozik.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-212">It can release instances at a maximum rate of four instances/hour during the week and six instances/hour during weekends.</span></span>

<span data-ttu-id="e3c9f-213">Ha több App Service-csomagokról a feldolgozókészleten alatt tárolt, kell számítani a *teljes inflációja* az összes az App Service-csomagok, amelyeket a inflációja összege az adott munkavégző készletét tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-213">If multiple App Service plans are being hosted in a worker pool, you have to calculate the *total inflation rate* as the sum of the inflation rate for all the App Service plans that are being hosting in that worker pool.</span></span>

![A feldolgozókészleten üzemeltetett több App Service-csomagokról teljes inflációja számítását.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a><span data-ttu-id="e3c9f-215">Az App Service-csomag inflációja segítségével munkavégző készlet automatikus skálázási szabályok definiálása</span><span class="sxs-lookup"><span data-stu-id="e3c9f-215">Use the App Service plan inflation rate to define worker pool autoscale rules</span></span>
<span data-ttu-id="e3c9f-216">Munkavégző készletbe üzemeltető App Service-csomagokról konfigurált automatikus skálázásra kell kapacitás puffert lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-216">Worker pools that host App Service plans that are configured to autoscale need to be allocated a buffer of capacity.</span></span> <span data-ttu-id="e3c9f-217">A puffer lehetővé teszi, hogy az automatikus skálázási művelet növelhető vagy csökkenthető az App Service-csomag, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-217">The buffer allows for the autoscale operations to grow and shrink the App Service plan as needed.</span></span> <span data-ttu-id="e3c9f-218">A minimális puffer lenne a számított teljes App Service megtervezése inflációja.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-218">The minimum buffer would be the calculated Total App Service Plan Inflation Rate.</span></span>

<span data-ttu-id="e3c9f-219">App Service-környezet skálázási műveleteinek néhány történő alkalmazása időbe telhet, mert további fordulhat elő, amíg folyamatban van egy skálázási műveletet igény szerinti módosítások változásait kell fiókot.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-219">Because App Service environment scale operations take some time to apply, any change should account for further demand changes that could happen while a scale operation is in progress.</span></span> <span data-ttu-id="e3c9f-220">Ez a késés teljesítése érdekében azt javasoljuk, hogy használ a számított teljes App Service megtervezése inflációja a hozzáadott példányok minimális számának minden automatikus skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-220">To accommodate this latency, we recommend that you use the calculated Total App Service Plan Inflation Rate as the minimum number of instances that are added for each autoscale operation.</span></span>

<span data-ttu-id="e3c9f-221">Az információ Frank határozhatja meg a következő automatikus skálázási profil és a szabályok:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-221">With this information, Frank can define the following autoscale profile and rules:</span></span>

![Automatikus skálázási profil szabályok LOB például.][Worker-Pool-Scale]

| <span data-ttu-id="e3c9f-223">**Automatikus skálázási profil – hétköznapokon**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-223">**Autoscale profile – Weekdays**</span></span> | <span data-ttu-id="e3c9f-224">**Automatikus skálázási profil – hétvégéket**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-224">**Autoscale profile – Weekends**</span></span> |
| --- | --- |
| <span data-ttu-id="e3c9f-225">**Name:** hétköznap profil</span><span class="sxs-lookup"><span data-stu-id="e3c9f-225">**Name:** Weekday profile</span></span> |<span data-ttu-id="e3c9f-226">**Name:** hétvégi profil</span><span class="sxs-lookup"><span data-stu-id="e3c9f-226">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="e3c9f-227">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-227">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="e3c9f-228">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-228">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="e3c9f-229">**Profil:** hétköznapokon</span><span class="sxs-lookup"><span data-stu-id="e3c9f-229">**Profile:** Weekdays</span></span> |<span data-ttu-id="e3c9f-230">**Profil:** hétvégi</span><span class="sxs-lookup"><span data-stu-id="e3c9f-230">**Profile:** Weekend</span></span> |
| <span data-ttu-id="e3c9f-231">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="e3c9f-231">**Type:** Recurrence</span></span> |<span data-ttu-id="e3c9f-232">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="e3c9f-232">**Type:** Recurrence</span></span> |
| <span data-ttu-id="e3c9f-233">**Tartomány cél:** 13-25 példányok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-233">**Target range:** 13 to 25 instances</span></span> |<span data-ttu-id="e3c9f-234">**Tartomány cél:** 6 – 15 példányok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-234">**Target range:** 6 to 15 instances</span></span> |
| <span data-ttu-id="e3c9f-235">**Nap:** hétfő, kedd, szerda, csütörtök, péntek</span><span class="sxs-lookup"><span data-stu-id="e3c9f-235">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="e3c9f-236">**Nap:** , szombat és vasárnap</span><span class="sxs-lookup"><span data-stu-id="e3c9f-236">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="e3c9f-237">**Kezdési időpont:** 7:00-kor</span><span class="sxs-lookup"><span data-stu-id="e3c9f-237">**Start time:** 7:00 AM</span></span> |<span data-ttu-id="e3c9f-238">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="e3c9f-238">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="e3c9f-239">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="e3c9f-239">**Time zone:** UTC-08</span></span> |<span data-ttu-id="e3c9f-240">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="e3c9f-240">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="e3c9f-241">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-241">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="e3c9f-242">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-242">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="e3c9f-243">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-243">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="e3c9f-244">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-244">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="e3c9f-245">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="e3c9f-245">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="e3c9f-246">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="e3c9f-246">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="e3c9f-247">**Művelet:** legalább 8</span><span class="sxs-lookup"><span data-stu-id="e3c9f-247">**Operation:** Less than 8</span></span> |<span data-ttu-id="e3c9f-248">**Művelet:** legalább 3</span><span class="sxs-lookup"><span data-stu-id="e3c9f-248">**Operation:** Less than 3</span></span> |
| <span data-ttu-id="e3c9f-249">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-249">**Duration:** 20 minutes</span></span> |<span data-ttu-id="e3c9f-250">**Időtartam:** 30 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-250">**Duration:** 30 minutes</span></span> |
| <span data-ttu-id="e3c9f-251">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-251">**Time aggregation:** Average</span></span> |<span data-ttu-id="e3c9f-252">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-252">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="e3c9f-253">**Művelet:** számának növeléséhez 8</span><span class="sxs-lookup"><span data-stu-id="e3c9f-253">**Action:** Increase count by 8</span></span> |<span data-ttu-id="e3c9f-254">**Művelet:** növeléséhez száma 3</span><span class="sxs-lookup"><span data-stu-id="e3c9f-254">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="e3c9f-255">**Ritkán használt adatok le (perc):** 180</span><span class="sxs-lookup"><span data-stu-id="e3c9f-255">**Cool down (minutes):** 180</span></span> |<span data-ttu-id="e3c9f-256">**Ritkán használt adatok le (perc):** 180</span><span class="sxs-lookup"><span data-stu-id="e3c9f-256">**Cool down (minutes):** 180</span></span> |
|  | |
| <span data-ttu-id="e3c9f-257">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-257">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="e3c9f-258">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-258">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="e3c9f-259">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-259">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="e3c9f-260">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-260">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="e3c9f-261">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="e3c9f-261">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="e3c9f-262">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="e3c9f-262">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="e3c9f-263">**Művelet:** 8-nál nagyobb</span><span class="sxs-lookup"><span data-stu-id="e3c9f-263">**Operation:** Greater than 8</span></span> |<span data-ttu-id="e3c9f-264">**Művelet:** 3-nál nagyobb</span><span class="sxs-lookup"><span data-stu-id="e3c9f-264">**Operation:** Greater than 3</span></span> |
| <span data-ttu-id="e3c9f-265">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-265">**Duration:** 20 minutes</span></span> |<span data-ttu-id="e3c9f-266">**Időtartam:** 15 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-266">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="e3c9f-267">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-267">**Time aggregation:** Average</span></span> |<span data-ttu-id="e3c9f-268">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-268">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="e3c9f-269">**Művelet:** számának csökkentése 2</span><span class="sxs-lookup"><span data-stu-id="e3c9f-269">**Action:** Decrease count by 2</span></span> |<span data-ttu-id="e3c9f-270">**Művelet:** csökkentése száma 3</span><span class="sxs-lookup"><span data-stu-id="e3c9f-270">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="e3c9f-271">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="e3c9f-271">**Cool down (minutes):** 120</span></span> |<span data-ttu-id="e3c9f-272">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="e3c9f-272">**Cool down (minutes):** 120</span></span> |

<span data-ttu-id="e3c9f-273">A célként megadott tartomány-profilban megadott az App Service-csomag + puffer-profilban megadott minimális példányainak számítja ki.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-273">The Target range defined in the profile is calculated by the minimum instances defined in the profile for the App Service plan + buffer.</span></span>

<span data-ttu-id="e3c9f-274">A maximális tartomány lenne a munkavégző készletben tárolt összes App Service-csomagokról az összes maximális tartomány összege.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-274">The Maximum range would be the sum of all the maximum ranges for all App Service plans hosted in the worker pool.</span></span>

<span data-ttu-id="e3c9f-275">A skálázási szabályok növekedése számát létre kell hozni legalább az App Service csomag inflációja bővített X 1.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-275">The Increase count for the scale up rules should be set to at least 1X the App Service Plan Inflation Rate for scale up.</span></span>

<span data-ttu-id="e3c9f-276">Csökkentse száma módosítható egy 1/2 X vagy az App Service csomag inflációja bővített X 1 közötti le.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-276">Decrease count can be adjusted to something between 1/2X or 1X the App Service Plan Inflation Rate for scale down.</span></span>

### <a name="autoscale-for-front-end-pool"></a><span data-ttu-id="e3c9f-277">Automatikus skálázás előtér-készlet</span><span class="sxs-lookup"><span data-stu-id="e3c9f-277">Autoscale for front-end pool</span></span>
<span data-ttu-id="e3c9f-278">Az előtér-automatikus skálázási szabályok egyszerűbbek, mint a feldolgozókészletek.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-278">Rules for front-end autoscale are simpler than for worker pools.</span></span> <span data-ttu-id="e3c9f-279">Elsősorban akkor</span><span class="sxs-lookup"><span data-stu-id="e3c9f-279">Primarily, you should</span></span>  
<span data-ttu-id="e3c9f-280">Győződjön meg arról, hogy a mérés és a cooldown időzítők időtartama fontolja meg, hogy az App Service-csomagot a skálázási műveletek nem azonnali.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-280">make sure that duration of the measurement and the cooldown timers consider that scale operations on an App Service plan are not instantaneous.</span></span>

<span data-ttu-id="e3c9f-281">Ebben a forgatókönyvben a Frank tudja, hogy a Hibaarány előtér-webkiszolgálóinak eléri a 80 %-os processzorhasználatot megnő, és beállítja az automatikus skálázási szabály, amely növeli következőképpen példányok:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-281">For this scenario, Frank knows that the error rate increases after front ends reach 80% CPU utilization and sets the autoscale rule to increase instances as follows:</span></span>

![Automatikus skálázási beállításokat előtér-készlet.][Front-End-Scale]

| <span data-ttu-id="e3c9f-283">**Automatikus skálázási profil – első karakterlánccal végződik-e**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-283">**Autoscale profile – Front ends**</span></span> |
| --- |
| <span data-ttu-id="e3c9f-284">**Name:** automatikus skálázás – első karakterlánccal végződik-e</span><span class="sxs-lookup"><span data-stu-id="e3c9f-284">**Name:** Autoscale – Front ends</span></span> |
| <span data-ttu-id="e3c9f-285">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-285">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="e3c9f-286">**Profil:** mindennapi</span><span class="sxs-lookup"><span data-stu-id="e3c9f-286">**Profile:** Everyday</span></span> |
| <span data-ttu-id="e3c9f-287">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="e3c9f-287">**Type:** Recurrence</span></span> |
| <span data-ttu-id="e3c9f-288">**Tartomány cél:** 3 – 10 példányok</span><span class="sxs-lookup"><span data-stu-id="e3c9f-288">**Target range:** 3 to 10 instances</span></span> |
| <span data-ttu-id="e3c9f-289">**Nap:** mindennapi</span><span class="sxs-lookup"><span data-stu-id="e3c9f-289">**Days:** Everyday</span></span> |
| <span data-ttu-id="e3c9f-290">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="e3c9f-290">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="e3c9f-291">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="e3c9f-291">**Time zone:** UTC-08</span></span> |
|  |
| <span data-ttu-id="e3c9f-292">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-292">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="e3c9f-293">**Erőforrás:** előtér-készlet</span><span class="sxs-lookup"><span data-stu-id="e3c9f-293">**Resource:** Front-end pool</span></span> |
| <span data-ttu-id="e3c9f-294">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-294">**Metric:** CPU %</span></span> |
| <span data-ttu-id="e3c9f-295">**Művelet:** nagyobb, mint 60 %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-295">**Operation:** Greater than 60%</span></span> |
| <span data-ttu-id="e3c9f-296">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-296">**Duration:** 20 minutes</span></span> |
| <span data-ttu-id="e3c9f-297">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-297">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="e3c9f-298">**Művelet:** növeléséhez száma 3</span><span class="sxs-lookup"><span data-stu-id="e3c9f-298">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="e3c9f-299">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="e3c9f-299">**Cool down (minutes):** 120</span></span> |
|  |
| <span data-ttu-id="e3c9f-300">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="e3c9f-300">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="e3c9f-301">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="e3c9f-301">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="e3c9f-302">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="e3c9f-302">**Metric:** CPU %</span></span> |
| <span data-ttu-id="e3c9f-303">**Művelet:** 30 %-nál kisebb</span><span class="sxs-lookup"><span data-stu-id="e3c9f-303">**Operation:** Less than 30%</span></span> |
| <span data-ttu-id="e3c9f-304">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="e3c9f-304">**Duration:** 20 Minutes</span></span> |
| <span data-ttu-id="e3c9f-305">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="e3c9f-305">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="e3c9f-306">**Művelet:** csökkentése száma 3</span><span class="sxs-lookup"><span data-stu-id="e3c9f-306">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="e3c9f-307">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="e3c9f-307">**Cool down (minutes):** 120</span></span> |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
