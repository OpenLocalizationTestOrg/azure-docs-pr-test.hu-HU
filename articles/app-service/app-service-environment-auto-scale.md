---
title: "aaaAutoscaling és az App Service Environment-környezet 1-es verzió"
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
ms.openlocfilehash: 1a03cf494309e80596b64471d1a067b2f64a9fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a><span data-ttu-id="8cc4b-103">Automatikus skálázás és az App Service Environment-környezet 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="8cc4b-103">Autoscaling and App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="8cc4b-104">Ez a cikk hello App Service Environment-környezet v1 tárgya.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-104">This article is about hello App Service Environment v1.</span></span>  <span data-ttu-id="8cc4b-105">App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-105">There is a newer version of hello App Service Environment that is easier  toouse and runs on more powerful infrastructure.</span></span> <span data-ttu-id="8cc4b-106">több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="8cc4b-106">toolearn more about hello new version start with hello [Introduction toohello App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

<span data-ttu-id="8cc4b-107">Támogatja az Azure App Service-környezetek *automatikus skálázás*.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-107">Azure App Service environments support *autoscaling*.</span></span> <span data-ttu-id="8cc4b-108">Használhatja az automatikus skálázás egyedi feldolgozókészletek metrikák vagy ütemezés alapján.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-108">You can autoscale individual worker pools based on metrics or schedule.</span></span>

![A feldolgozókészleten automatikus skálázás beállításai.][intro]

<span data-ttu-id="8cc4b-110">Automatikus skálázás optimalizálja az erőforrás-használat automatikusan növekvő, és az App Service-környezet toofit zsugorítását a keret és vagy profil.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-110">Autoscaling optimizes your resource utilization by automatically growing and shrinking an App Service environment toofit your budget and or load profile.</span></span>

## <a name="configure-worker-pool-autoscale"></a><span data-ttu-id="8cc4b-111">Munkavégző készlet automatikus skálázás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8cc4b-111">Configure worker pool autoscale</span></span>
<span data-ttu-id="8cc4b-112">Hello automatikus skálázási funkciót elérhető hello **beállítások** hello feldolgozókészletek lapján.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-112">You can access hello autoscale functionality from hello **Settings** tab of hello worker pool.</span></span>

![Hello feldolgozókészletek beállítások lapján.][settings-scale]

<span data-ttu-id="8cc4b-114">Ott hello felületet kell viszonylag jól ismert, mert hello tervezze meg, hogy látni, amikor egy App Service méretezni ugyanazt a felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-114">From there, hello interface should be fairly familiar since it is hello same experience that you see when you scale an App Service plan.</span></span> 

![Manuális skálázási beállításokat.][scale-manual]

<span data-ttu-id="8cc4b-116">Az automatikus skálázási profilok is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-116">You can also configure an autoscale profile.</span></span>

![Automatikus skálázási beállításokat.][scale-profile]

<span data-ttu-id="8cc4b-118">Automatikus skálázási profilok olyan hasznos tooset vonatkozó korlátozások a skála.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-118">Autoscale profiles are useful tooset limits on your scale.</span></span> <span data-ttu-id="8cc4b-119">Ezzel a módszerrel lehet egy egységes élmény úgy, hogy egy felső határa (2) egy alsó határ skálaértéknél (1) és egy előre jelezhető ráfordítás cap beállításával teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-119">This way, you can have a consistent performance experience by setting a lower bound scale value (1) and a predictable spend cap by setting an upper bound (2).</span></span>

![A profil beállítását.][scale-profile2]

<span data-ttu-id="8cc4b-121">Után egy profil megadásához felfelé vagy lefelé hello feldolgozókészletek hello-profil által meghatározott hello határokon belül található példányok száma hello automatikus skálázási szabályok tooscale adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-121">After you define a profile, you can add autoscale rules tooscale up or down hello number of instances in hello worker pool within hello bounds defined by hello profile.</span></span> <span data-ttu-id="8cc4b-122">Automatikus skálázási szabályok metrikák alapulnak.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-122">Autoscale rules are based on metrics.</span></span>

![Skálázási szabályhoz.][scale-rule]

 <span data-ttu-id="8cc4b-124">A feldolgozókészleten vagy előtér-metrikák használt toodefine automatikus skálázási szabályok lehet.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-124">Any worker pool or front-end metrics can be used toodefine autoscale rules.</span></span> <span data-ttu-id="8cc4b-125">Ezeket a mérési metrikák hello erőforrás panel diagramokban figyelése, vagy állítsa be a riasztások hello.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-125">These metrics are hello same metrics you can monitor in hello resource blade graphs or set alerts for.</span></span>

## <a name="autoscale-example"></a><span data-ttu-id="8cc4b-126">Automatikus skálázás – példa</span><span class="sxs-lookup"><span data-stu-id="8cc4b-126">Autoscale example</span></span>
<span data-ttu-id="8cc4b-127">Egy App Service Environment-környezet automatikus skálázás legjobb érdekében a forgatókönyv segítségével mutatható be.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-127">Autoscale of an App Service environment can best be illustrated by walking through a scenario.</span></span>

<span data-ttu-id="8cc4b-128">Ez a cikk ismerteti az összes hello szükséges szempontok automatikus skálázás beállításához.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-128">This article explains all hello necessary considerations when you set up autoscale.</span></span> <span data-ttu-id="8cc4b-129">hello a cikk bemutatja, hogyan hello figyelembe a automatikus skálázás App Service Environment-környezetben üzemeltetett App Service-környezetek, amelyek kapcsolati lejátszását.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-129">hello article walks you through hello interactions that come into play when you factor in autoscaling App Service environments that are hosted in App Service Environment.</span></span>

### <a name="scenario-introduction"></a><span data-ttu-id="8cc4b-130">A forgatókönyv bemutatása</span><span class="sxs-lookup"><span data-stu-id="8cc4b-130">Scenario introduction</span></span>
<span data-ttu-id="8cc4b-131">Frank a sysadmin (rendszergazda), akik át lett telepítve, hogy tooan App Service Environment-környezet kezel hello munkaterhelések része a vállalati.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-131">Frank is a sysadmin for an enterprise who has migrated a portion of hello workloads that he manages tooan App Service environment.</span></span>

<span data-ttu-id="8cc4b-132">hello App Service-környezet skálázási toomanual a következőképpen konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-132">hello App Service environment is configured toomanual scale as follows:</span></span>

* <span data-ttu-id="8cc4b-133">**Első végződik:** 3</span><span class="sxs-lookup"><span data-stu-id="8cc4b-133">**Front ends:** 3</span></span>
* <span data-ttu-id="8cc4b-134">**A feldolgozókészleten 1**: 10</span><span class="sxs-lookup"><span data-stu-id="8cc4b-134">**Worker pool 1**: 10</span></span>
* <span data-ttu-id="8cc4b-135">**A feldolgozókészleten 2**: 5</span><span class="sxs-lookup"><span data-stu-id="8cc4b-135">**Worker pool 2**: 5</span></span>
* <span data-ttu-id="8cc4b-136">**A feldolgozókészleten 3**: 5</span><span class="sxs-lookup"><span data-stu-id="8cc4b-136">**Worker pool 3**: 5</span></span>

<span data-ttu-id="8cc4b-137">1 feldolgozókészletek használjuk a termelési számítási feladatokhoz, feldolgozókészletek 2 és 3 feldolgozókészletek a minőségi megbízhatósági (QA) és a fejlesztői munkaterhelések szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-137">Worker pool 1 is used for production workloads, while worker pool 2 and worker pool 3 are used for quality assurance (QA) and development workloads.</span></span>

<span data-ttu-id="8cc4b-138">hello App Service-csomagok a QA és fejlesztői toomanual méretezési konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-138">hello App Service plans for QA and dev are configured toomanual scale.</span></span> <span data-ttu-id="8cc4b-139">hello éles App Service-csomag tooautoscale toodeal rendelkező változata állítja be a terhelési és a forgalom.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-139">hello production App Service plan is set tooautoscale toodeal with variations in load and traffic.</span></span>

<span data-ttu-id="8cc4b-140">Frank hello alkalmazással tisztában.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-140">Frank is very familiar with hello application.</span></span> <span data-ttu-id="8cc4b-141">Tudja, hogy a hello csúcsidőszakon terhelést 9:00-kor és 18:00:00 közé esnek, mivel ez egy alkalmazottak által használt során hello office-üzletági (LOB) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-141">He knows that hello peak hours for load are between 9:00 AM and 6:00 PM because this is a line-of-business (LOB) application that employees use while they are in hello office.</span></span> <span data-ttu-id="8cc4b-142">Ezt követően használati elutasítja azokat a felhasználókat adott napon szabásakor.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-142">Usage drops after that when users are done for that day.</span></span> <span data-ttu-id="8cc4b-143">Csúcsidőszakon kívüli nincs még néhány betöltése mert felhasználók férhetnek hozzá hello app távolról a mobileszközök és az otthoni számítógépek használatával.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-143">Outside peak hours, there is still some load because users can access hello app remotely by using their mobile devices or home PCs.</span></span> <span data-ttu-id="8cc4b-144">App Service-csomag már hello éles CPU-használat alapján a következő szabályok hello tooautoscale konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-144">hello production App Service plan is already configured tooautoscale based on CPU usage with hello following rules:</span></span>

![Az ÜZLETÁGI alkalmazás beállításait.][asp-scale]

| <span data-ttu-id="8cc4b-146">**Automatikus skálázási profil – létrehozását – az App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-146">**Autoscale profile – Weekdays – App Service plan**</span></span> | <span data-ttu-id="8cc4b-147">**Automatikus skálázási profil – hétvégéket – az App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-147">**Autoscale profile – Weekends – App Service plan**</span></span> |
| --- | --- |
| <span data-ttu-id="8cc4b-148">**Name:** hétköznap profil</span><span class="sxs-lookup"><span data-stu-id="8cc4b-148">**Name:** Weekday profile</span></span> |<span data-ttu-id="8cc4b-149">**Name:** hétvégi profil</span><span class="sxs-lookup"><span data-stu-id="8cc4b-149">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="8cc4b-150">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-150">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="8cc4b-151">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-151">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="8cc4b-152">**Profil:** hétköznapokon</span><span class="sxs-lookup"><span data-stu-id="8cc4b-152">**Profile:** Weekdays</span></span> |<span data-ttu-id="8cc4b-153">**Profil:** hétvégi</span><span class="sxs-lookup"><span data-stu-id="8cc4b-153">**Profile:** Weekend</span></span> |
| <span data-ttu-id="8cc4b-154">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="8cc4b-154">**Type:** Recurrence</span></span> |<span data-ttu-id="8cc4b-155">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="8cc4b-155">**Type:** Recurrence</span></span> |
| <span data-ttu-id="8cc4b-156">**Tartomány cél:** 5 too20 példányok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-156">**Target range:** 5 too20 instances</span></span> |<span data-ttu-id="8cc4b-157">**Tartomány cél:** 3 too10 példányok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-157">**Target range:** 3 too10 instances</span></span> |
| <span data-ttu-id="8cc4b-158">**Nap:** hétfő, kedd, szerda, csütörtök, péntek</span><span class="sxs-lookup"><span data-stu-id="8cc4b-158">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="8cc4b-159">**Nap:** , szombat és vasárnap</span><span class="sxs-lookup"><span data-stu-id="8cc4b-159">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="8cc4b-160">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="8cc4b-160">**Start time:** 9:00 AM</span></span> |<span data-ttu-id="8cc4b-161">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="8cc4b-161">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="8cc4b-162">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="8cc4b-162">**Time zone:** UTC-08</span></span> |<span data-ttu-id="8cc4b-163">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="8cc4b-163">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="8cc4b-164">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-164">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="8cc4b-165">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-165">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="8cc4b-166">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="8cc4b-166">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="8cc4b-167">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="8cc4b-167">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="8cc4b-168">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-168">**Metric:** CPU %</span></span> |<span data-ttu-id="8cc4b-169">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-169">**Metric:** CPU %</span></span> |
| <span data-ttu-id="8cc4b-170">**Művelet:** nagyobb, mint 60 %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-170">**Operation:** Greater than 60%</span></span> |<span data-ttu-id="8cc4b-171">**Művelet:** 80 %-nál nagyobb</span><span class="sxs-lookup"><span data-stu-id="8cc4b-171">**Operation:** Greater than 80%</span></span> |
| <span data-ttu-id="8cc4b-172">**Időtartam:** 5 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-172">**Duration:** 5 Minutes</span></span> |<span data-ttu-id="8cc4b-173">**Időtartam:** 10 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-173">**Duration:** 10 Minutes</span></span> |
| <span data-ttu-id="8cc4b-174">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-174">**Time aggregation:** Average</span></span> |<span data-ttu-id="8cc4b-175">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-175">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="8cc4b-176">**Művelet:** számának növeléséhez a 2. régiója</span><span class="sxs-lookup"><span data-stu-id="8cc4b-176">**Action:** Increase count by 2</span></span> |<span data-ttu-id="8cc4b-177">**Művelet:** számának növeléséhez 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-177">**Action:** Increase count by 1</span></span> |
| <span data-ttu-id="8cc4b-178">**Ritkán használt adatok le (perc):** 15</span><span class="sxs-lookup"><span data-stu-id="8cc4b-178">**Cool down (minutes):** 15</span></span> |<span data-ttu-id="8cc4b-179">**Ritkán használt adatok le (perc):** 20</span><span class="sxs-lookup"><span data-stu-id="8cc4b-179">**Cool down (minutes):** 20</span></span> |
|  | |
| <span data-ttu-id="8cc4b-180">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-180">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="8cc4b-181">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-181">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="8cc4b-182">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="8cc4b-182">**Resource:** Production (App Service Environment)</span></span> |<span data-ttu-id="8cc4b-183">**Erőforrás:** éles (App Service Environment-környezet)</span><span class="sxs-lookup"><span data-stu-id="8cc4b-183">**Resource:** Production (App Service Environment)</span></span> |
| <span data-ttu-id="8cc4b-184">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-184">**Metric:** CPU %</span></span> |<span data-ttu-id="8cc4b-185">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-185">**Metric:** CPU %</span></span> |
| <span data-ttu-id="8cc4b-186">**Művelet:** 30 %-nál kisebb</span><span class="sxs-lookup"><span data-stu-id="8cc4b-186">**Operation:** Less than 30%</span></span> |<span data-ttu-id="8cc4b-187">**Művelet:** 20 %-nál kisebb</span><span class="sxs-lookup"><span data-stu-id="8cc4b-187">**Operation:** Less than 20%</span></span> |
| <span data-ttu-id="8cc4b-188">**Időtartam:** 10 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-188">**Duration:** 10 minutes</span></span> |<span data-ttu-id="8cc4b-189">**Időtartam:** 15 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-189">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="8cc4b-190">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-190">**Time aggregation:** Average</span></span> |<span data-ttu-id="8cc4b-191">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-191">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="8cc4b-192">**Művelet:** számának csökkentése 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-192">**Action:** Decrease count by 1</span></span> |<span data-ttu-id="8cc4b-193">**Művelet:** számának csökkentése 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-193">**Action:** Decrease count by 1</span></span> |
| <span data-ttu-id="8cc4b-194">**Ritkán használt adatok le (perc):** 20</span><span class="sxs-lookup"><span data-stu-id="8cc4b-194">**Cool down (minutes):** 20</span></span> |<span data-ttu-id="8cc4b-195">**Ritkán használt adatok le (perc):** 10</span><span class="sxs-lookup"><span data-stu-id="8cc4b-195">**Cool down (minutes):** 10</span></span> |

### <a name="app-service-plan-inflation-rate"></a><span data-ttu-id="8cc4b-196">Az alkalmazásszolgáltatási csomag inflációja</span><span class="sxs-lookup"><span data-stu-id="8cc4b-196">App Service plan inflation rate</span></span>
<span data-ttu-id="8cc4b-197">App Service-csomagok, amelyek a konfigurált tooautoscale ehhez óránként sebességet.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-197">App Service plans that are configured tooautoscale do so at a maximum rate per hour.</span></span> <span data-ttu-id="8cc4b-198">Ezt az értéket a megadott automatikus skálázási szabály hello hello értékek alapján lehet kiszámolni.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-198">This rate can be calculated based on hello values provided on hello autoscale rule.</span></span>

<span data-ttu-id="8cc4b-199">Megismerés és kiszámítása hello *App Service-csomag inflációja* fontos az App Service-környezet automatikus skálázási, mert nincsenek azonnali méretezési módosítások tooa munkavégző készletét.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-199">Understanding and calculating hello *App Service plan inflation rate* is important for App Service environment autoscale because scale changes tooa worker pool are not instantaneous.</span></span>

<span data-ttu-id="8cc4b-200">az alkalmazásszolgáltatási csomag inflációja hello kiszámítása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-200">hello App Service plan inflation rate is calculated as follows:</span></span>

![Az alkalmazásszolgáltatási csomag inflációja számítási.][ASP-Inflation]

<span data-ttu-id="8cc4b-202">Hello automatikus skálázás – hello hétköznap profil hello termelési App Service-csomag vertikális Felskálázás szabály alapján:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-202">Based on hello Autoscale – Scale Up rule for hello Weekday profile of hello production App Service plan:</span></span>

![App Service csomag infláció arány alapján automatikus skálázás – vertikális Felskálázás szabály létrehozását.][Equation1]

<span data-ttu-id="8cc4b-204">Az automatikus skálázás – hello hétvégi profil hello termelési App Service-csomag vertikális Felskálázás szabály hello hello esetben hello képlet kellene tartoznia:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-204">In hello case of hello Autoscale – Scale Up rule for hello Weekend profile of hello production App Service plan, hello formula would resolve to:</span></span>

![App Service csomag infláció arány hétvégéket automatikus skálázás – vertikális Felskálázás szabály alapján.][Equation2]

<span data-ttu-id="8cc4b-206">Ez az érték is kerülhet sor, lefelé méretezéshez műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-206">This value can also be calculated for scale-down operations.</span></span>

<span data-ttu-id="8cc4b-207">Hello automatikus skálázás – hello hétköznap profil hello termelési App Service-csomag méretezési le szabály alapján ez lenne az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-207">Based on hello Autoscale – Scale Down rule for hello Weekday profile of hello production App Service plan, this would look as follows:</span></span>

![App Service csomag infláció arány alapján automatikus skálázás – méretezési le szabály létrehozását.][Equation3]

<span data-ttu-id="8cc4b-209">Az automatikus skálázás – hello hétvégi profil hello termelési App Service-csomag méretezési le szabály hello hello esetben hello képlet kellene tartoznia:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-209">In hello case of hello Autoscale – Scale Down rule for hello Weekend profile of hello production App Service plan, hello formula would resolve to:</span></span>  

![App Service csomag infláció arány hétvégéket automatikus skálázás – méretezési le szabály alapján.][Equation4]

<span data-ttu-id="8cc4b-211">hello éles App Service-csomag milyen mértékben növelhető a maximális sebesség hello héten nyolc példányok/óra és négy példányok óránként hello hétvégén.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-211">hello production App Service plan can grow at a maximum rate of eight instances/hour during hello week and four instances/hour during hello weekend.</span></span> <span data-ttu-id="8cc4b-212">Azt is megjelenhetnek példányok négy példányok/óra hello héten sebességet és hat példányok/óra során tartozik.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-212">It can release instances at a maximum rate of four instances/hour during hello week and six instances/hour during weekends.</span></span>

<span data-ttu-id="8cc4b-213">Ha több App Service-csomagokról a feldolgozókészleten alatt tárolt, hogy van-e toocalculate hello *teljes inflációja* , hogy az összes olyan App Service-csomagok hello éppen hello inflációja hello munkavégző készlethez tartozó üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-213">If multiple App Service plans are being hosted in a worker pool, you have toocalculate hello *total inflation rate* as hello sum of hello inflation rate for all hello App Service plans that are being hosting in that worker pool.</span></span>

![A feldolgozókészleten üzemeltetett több App Service-csomagokról teljes inflációja számítását.][ASP-Total-Inflation]

### <a name="use-hello-app-service-plan-inflation-rate-toodefine-worker-pool-autoscale-rules"></a><span data-ttu-id="8cc4b-215">App Service használata hello inflációja toodefine munkavégző készlet automatikus skálázási szabályok megtervezése</span><span class="sxs-lookup"><span data-stu-id="8cc4b-215">Use hello App Service plan inflation rate toodefine worker pool autoscale rules</span></span>
<span data-ttu-id="8cc4b-216">Munkavégző készletbe üzemeltető App Service-csomagok, amelyek a konfigurált tooautoscale kell kapacitás puffert lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-216">Worker pools that host App Service plans that are configured tooautoscale need to be allocated a buffer of capacity.</span></span> <span data-ttu-id="8cc4b-217">hello puffer lehetővé teszi, hogy hello automatikus skálázási műveletek toogrow, és az App Service-csomag igazodjon.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-217">hello buffer allows for hello autoscale operations toogrow and shrink the App Service plan as needed.</span></span> <span data-ttu-id="8cc4b-218">hello minimális puffer hello számított teljes App Service megtervezése inflációja lesz.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-218">hello minimum buffer would be hello calculated Total App Service Plan Inflation Rate.</span></span>

<span data-ttu-id="8cc4b-219">App Service-környezet skálázási műveleteinek igénybe vehet néhány alkalommal tooapply, mert további fordulhat elő, amíg folyamatban van egy skálázási műveletet igény szerinti módosítások változásait kell fiókot.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-219">Because App Service environment scale operations take some time tooapply, any change should account for further demand changes that could happen while a scale operation is in progress.</span></span> <span data-ttu-id="8cc4b-220">tooaccommodate a késés, azt javasoljuk, hogy használja a program teljes App Service megtervezése inflációja hello hello minden automatikus skálázási művelet hozzáadott-példányok minimális száma.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-220">tooaccommodate this latency, we recommend that you use hello calculated Total App Service Plan Inflation Rate as hello minimum number of instances that are added for each autoscale operation.</span></span>

<span data-ttu-id="8cc4b-221">Az információ Frank hello adhatja meg a következő automatikus skálázási profil és a szabályok:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-221">With this information, Frank can define hello following autoscale profile and rules:</span></span>

![Automatikus skálázási profil szabályok LOB például.][Worker-Pool-Scale]

| <span data-ttu-id="8cc4b-223">**Automatikus skálázási profil – hétköznapokon**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-223">**Autoscale profile – Weekdays**</span></span> | <span data-ttu-id="8cc4b-224">**Automatikus skálázási profil – hétvégéket**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-224">**Autoscale profile – Weekends**</span></span> |
| --- | --- |
| <span data-ttu-id="8cc4b-225">**Name:** hétköznap profil</span><span class="sxs-lookup"><span data-stu-id="8cc4b-225">**Name:** Weekday profile</span></span> |<span data-ttu-id="8cc4b-226">**Name:** hétvégi profil</span><span class="sxs-lookup"><span data-stu-id="8cc4b-226">**Name:** Weekend profile</span></span> |
| <span data-ttu-id="8cc4b-227">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-227">**Scale by:** Schedule and performance rules</span></span> |<span data-ttu-id="8cc4b-228">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-228">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="8cc4b-229">**Profil:** hétköznapokon</span><span class="sxs-lookup"><span data-stu-id="8cc4b-229">**Profile:** Weekdays</span></span> |<span data-ttu-id="8cc4b-230">**Profil:** hétvégi</span><span class="sxs-lookup"><span data-stu-id="8cc4b-230">**Profile:** Weekend</span></span> |
| <span data-ttu-id="8cc4b-231">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="8cc4b-231">**Type:** Recurrence</span></span> |<span data-ttu-id="8cc4b-232">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="8cc4b-232">**Type:** Recurrence</span></span> |
| <span data-ttu-id="8cc4b-233">**Tartomány cél:** 13 too25 példányok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-233">**Target range:** 13 too25 instances</span></span> |<span data-ttu-id="8cc4b-234">**Tartomány cél:** 6 too15 példányok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-234">**Target range:** 6 too15 instances</span></span> |
| <span data-ttu-id="8cc4b-235">**Nap:** hétfő, kedd, szerda, csütörtök, péntek</span><span class="sxs-lookup"><span data-stu-id="8cc4b-235">**Days:** Monday, Tuesday, Wednesday, Thursday, Friday</span></span> |<span data-ttu-id="8cc4b-236">**Nap:** , szombat és vasárnap</span><span class="sxs-lookup"><span data-stu-id="8cc4b-236">**Days:** Saturday, Sunday</span></span> |
| <span data-ttu-id="8cc4b-237">**Kezdési időpont:** 7:00-kor</span><span class="sxs-lookup"><span data-stu-id="8cc4b-237">**Start time:** 7:00 AM</span></span> |<span data-ttu-id="8cc4b-238">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="8cc4b-238">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="8cc4b-239">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="8cc4b-239">**Time zone:** UTC-08</span></span> |<span data-ttu-id="8cc4b-240">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="8cc4b-240">**Time zone:** UTC-08</span></span> |
|  | |
| <span data-ttu-id="8cc4b-241">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-241">**Autoscale rule (Scale Up)**</span></span> |<span data-ttu-id="8cc4b-242">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-242">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="8cc4b-243">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-243">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="8cc4b-244">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-244">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="8cc4b-245">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="8cc4b-245">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="8cc4b-246">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="8cc4b-246">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="8cc4b-247">**Művelet:** legalább 8</span><span class="sxs-lookup"><span data-stu-id="8cc4b-247">**Operation:** Less than 8</span></span> |<span data-ttu-id="8cc4b-248">**Művelet:** legalább 3</span><span class="sxs-lookup"><span data-stu-id="8cc4b-248">**Operation:** Less than 3</span></span> |
| <span data-ttu-id="8cc4b-249">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-249">**Duration:** 20 minutes</span></span> |<span data-ttu-id="8cc4b-250">**Időtartam:** 30 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-250">**Duration:** 30 minutes</span></span> |
| <span data-ttu-id="8cc4b-251">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-251">**Time aggregation:** Average</span></span> |<span data-ttu-id="8cc4b-252">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-252">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="8cc4b-253">**Művelet:** számának növeléséhez 8</span><span class="sxs-lookup"><span data-stu-id="8cc4b-253">**Action:** Increase count by 8</span></span> |<span data-ttu-id="8cc4b-254">**Művelet:** növeléséhez száma 3</span><span class="sxs-lookup"><span data-stu-id="8cc4b-254">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="8cc4b-255">**Ritkán használt adatok le (perc):** 180</span><span class="sxs-lookup"><span data-stu-id="8cc4b-255">**Cool down (minutes):** 180</span></span> |<span data-ttu-id="8cc4b-256">**Ritkán használt adatok le (perc):** 180</span><span class="sxs-lookup"><span data-stu-id="8cc4b-256">**Cool down (minutes):** 180</span></span> |
|  | |
| <span data-ttu-id="8cc4b-257">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-257">**Autoscale rule (Scale Down)**</span></span> |<span data-ttu-id="8cc4b-258">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-258">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="8cc4b-259">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-259">**Resource:** Worker pool 1</span></span> |<span data-ttu-id="8cc4b-260">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-260">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="8cc4b-261">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="8cc4b-261">**Metric:** WorkersAvailable</span></span> |<span data-ttu-id="8cc4b-262">**A metrika:** WorkersAvailable</span><span class="sxs-lookup"><span data-stu-id="8cc4b-262">**Metric:** WorkersAvailable</span></span> |
| <span data-ttu-id="8cc4b-263">**Művelet:** 8-nál nagyobb</span><span class="sxs-lookup"><span data-stu-id="8cc4b-263">**Operation:** Greater than 8</span></span> |<span data-ttu-id="8cc4b-264">**Művelet:** 3-nál nagyobb</span><span class="sxs-lookup"><span data-stu-id="8cc4b-264">**Operation:** Greater than 3</span></span> |
| <span data-ttu-id="8cc4b-265">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-265">**Duration:** 20 minutes</span></span> |<span data-ttu-id="8cc4b-266">**Időtartam:** 15 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-266">**Duration:** 15 minutes</span></span> |
| <span data-ttu-id="8cc4b-267">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-267">**Time aggregation:** Average</span></span> |<span data-ttu-id="8cc4b-268">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-268">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="8cc4b-269">**Művelet:** számának csökkentése 2</span><span class="sxs-lookup"><span data-stu-id="8cc4b-269">**Action:** Decrease count by 2</span></span> |<span data-ttu-id="8cc4b-270">**Művelet:** csökkentése száma 3</span><span class="sxs-lookup"><span data-stu-id="8cc4b-270">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="8cc4b-271">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="8cc4b-271">**Cool down (minutes):** 120</span></span> |<span data-ttu-id="8cc4b-272">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="8cc4b-272">**Cool down (minutes):** 120</span></span> |

<span data-ttu-id="8cc4b-273">a példányok minimális hello az App Service-csomag hello-profil + puffer definiált hello hello profilban megadott cél tartomány számítja ki.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-273">hello Target range defined in hello profile is calculated by hello minimum instances defined in the profile for hello App Service plan + buffer.</span></span>

<span data-ttu-id="8cc4b-274">hello maximális tartomány lenne az összes hello maximális tartomány hello munkavégző készletben tárolt összes App Service-csomagokról hello összege.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-274">hello Maximum range would be hello sum of all hello maximum ranges for all App Service plans hosted in hello worker pool.</span></span>

<span data-ttu-id="8cc4b-275">hello hello skálázási szabályok száma növelni kell set tooat legalább 1 X az App Service csomag inflációja bővített fel.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-275">hello Increase count for hello scale up rules should be set tooat least 1X the App Service Plan Inflation Rate for scale up.</span></span>

<span data-ttu-id="8cc4b-276">Csökkentse száma lehet módosított toosomething 1/2 X között, vagy 1 X App Service megtervezése inflációja hello méretezési le.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-276">Decrease count can be adjusted toosomething between 1/2X or 1X hello App Service Plan Inflation Rate for scale down.</span></span>

### <a name="autoscale-for-front-end-pool"></a><span data-ttu-id="8cc4b-277">Automatikus skálázás előtér-készlet</span><span class="sxs-lookup"><span data-stu-id="8cc4b-277">Autoscale for front-end pool</span></span>
<span data-ttu-id="8cc4b-278">Az előtér-automatikus skálázási szabályok egyszerűbbek, mint a feldolgozókészletek.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-278">Rules for front-end autoscale are simpler than for worker pools.</span></span> <span data-ttu-id="8cc4b-279">Elsősorban akkor</span><span class="sxs-lookup"><span data-stu-id="8cc4b-279">Primarily, you should</span></span>  
<span data-ttu-id="8cc4b-280">Győződjön meg arról, hogy hello mérési és hello cooldown időzítők időtartama vegye figyelembe, hogy az App Service-csomagot a skálázási műveletek nem azonnali.</span><span class="sxs-lookup"><span data-stu-id="8cc4b-280">make sure that duration of hello measurement and hello cooldown timers consider that scale operations on an App Service plan are not instantaneous.</span></span>

<span data-ttu-id="8cc4b-281">Ebben a forgatókönyvben a Frank tudja, hogy hello Hibaarány előtér-webkiszolgálóinak eléri a 80 %-os processzorhasználatot megnő, és beállítja hello automatikus skálázási szabály tooincrease példányok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8cc4b-281">For this scenario, Frank knows that hello error rate increases after front ends reach 80% CPU utilization and sets hello autoscale rule tooincrease instances as follows:</span></span>

![Automatikus skálázási beállításokat előtér-készlet.][Front-End-Scale]

| <span data-ttu-id="8cc4b-283">**Automatikus skálázási profil – első karakterlánccal végződik-e**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-283">**Autoscale profile – Front ends**</span></span> |
| --- |
| <span data-ttu-id="8cc4b-284">**Name:** automatikus skálázás – első karakterlánccal végződik-e</span><span class="sxs-lookup"><span data-stu-id="8cc4b-284">**Name:** Autoscale – Front ends</span></span> |
| <span data-ttu-id="8cc4b-285">**A skála:** ütemezés és a teljesítmény szabályok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-285">**Scale by:** Schedule and performance rules</span></span> |
| <span data-ttu-id="8cc4b-286">**Profil:** mindennapi</span><span class="sxs-lookup"><span data-stu-id="8cc4b-286">**Profile:** Everyday</span></span> |
| <span data-ttu-id="8cc4b-287">**Típus:** ismétlődési</span><span class="sxs-lookup"><span data-stu-id="8cc4b-287">**Type:** Recurrence</span></span> |
| <span data-ttu-id="8cc4b-288">**Tartomány cél:** 3 too10 példányok</span><span class="sxs-lookup"><span data-stu-id="8cc4b-288">**Target range:** 3 too10 instances</span></span> |
| <span data-ttu-id="8cc4b-289">**Nap:** mindennapi</span><span class="sxs-lookup"><span data-stu-id="8cc4b-289">**Days:** Everyday</span></span> |
| <span data-ttu-id="8cc4b-290">**Kezdési időpont:** 9:00-kor</span><span class="sxs-lookup"><span data-stu-id="8cc4b-290">**Start time:** 9:00 AM</span></span> |
| <span data-ttu-id="8cc4b-291">**Időzóna:** UTC-08</span><span class="sxs-lookup"><span data-stu-id="8cc4b-291">**Time zone:** UTC-08</span></span> |
|  |
| <span data-ttu-id="8cc4b-292">**Automatikus skálázási szabály (vertikális Felskálázás)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-292">**Autoscale rule (Scale Up)**</span></span> |
| <span data-ttu-id="8cc4b-293">**Erőforrás:** előtér-készlet</span><span class="sxs-lookup"><span data-stu-id="8cc4b-293">**Resource:** Front-end pool</span></span> |
| <span data-ttu-id="8cc4b-294">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-294">**Metric:** CPU %</span></span> |
| <span data-ttu-id="8cc4b-295">**Művelet:** nagyobb, mint 60 %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-295">**Operation:** Greater than 60%</span></span> |
| <span data-ttu-id="8cc4b-296">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-296">**Duration:** 20 minutes</span></span> |
| <span data-ttu-id="8cc4b-297">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-297">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="8cc4b-298">**Művelet:** növeléséhez száma 3</span><span class="sxs-lookup"><span data-stu-id="8cc4b-298">**Action:** Increase count by 3</span></span> |
| <span data-ttu-id="8cc4b-299">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="8cc4b-299">**Cool down (minutes):** 120</span></span> |
|  |
| <span data-ttu-id="8cc4b-300">**Automatikus skálázási szabály (méretezési le)**</span><span class="sxs-lookup"><span data-stu-id="8cc4b-300">**Autoscale rule (Scale Down)**</span></span> |
| <span data-ttu-id="8cc4b-301">**Erőforrás:** feldolgozókészletek 1</span><span class="sxs-lookup"><span data-stu-id="8cc4b-301">**Resource:** Worker pool 1</span></span> |
| <span data-ttu-id="8cc4b-302">**A metrika:** CPU %</span><span class="sxs-lookup"><span data-stu-id="8cc4b-302">**Metric:** CPU %</span></span> |
| <span data-ttu-id="8cc4b-303">**Művelet:** 30 %-nál kisebb</span><span class="sxs-lookup"><span data-stu-id="8cc4b-303">**Operation:** Less than 30%</span></span> |
| <span data-ttu-id="8cc4b-304">**Időtartam:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="8cc4b-304">**Duration:** 20 Minutes</span></span> |
| <span data-ttu-id="8cc4b-305">**Összesítési idő:** átlagos</span><span class="sxs-lookup"><span data-stu-id="8cc4b-305">**Time aggregation:** Average</span></span> |
| <span data-ttu-id="8cc4b-306">**Művelet:** csökkentése száma 3</span><span class="sxs-lookup"><span data-stu-id="8cc4b-306">**Action:** Decrease count by 3</span></span> |
| <span data-ttu-id="8cc4b-307">**Ritkán használt adatok le (perc):** 120</span><span class="sxs-lookup"><span data-stu-id="8cc4b-307">**Cool down (minutes):** 120</span></span> |

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
