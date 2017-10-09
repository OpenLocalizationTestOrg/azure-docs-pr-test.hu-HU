---
title: "az Azure-ban automatikus skálázás használatába aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale az erőforrás az Azure-ban."
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: rajram
ms.openlocfilehash: 6b3c3f4529018dcaf9691c538fec63dfbb3cea06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-autoscale-in-azure"></a><span data-ttu-id="ef7e8-103">Ismerkedés az Azure-ban automatikus skálázás</span><span class="sxs-lookup"><span data-stu-id="ef7e8-103">Get started with Autoscale in Azure</span></span>
<span data-ttu-id="ef7e8-104">Ez a cikk ismerteti, hogyan tooset az automatikus skálázási beállításokat az erőforrás hello Microsoft Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-104">This article describes how tooset up your Autoscale settings for your resource in hello Microsoft Azure portal.</span></span>

<span data-ttu-id="ef7e8-105">Az Azure a figyelő automatikus skálázási csak toovirtual gép méretezési csoportok, felhőalapú szolgáltatások, Azure App Service-csomagokról és App Service-környezetek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-105">Azure Monitor Autoscale applies only toovirtual machine scale sets, cloud services, Azure App Service plans, and App Service environments.</span></span> 

## <a name="discover-hello-autoscale-settings-in-your-subscription"></a><span data-ttu-id="ef7e8-106">Hello automatikus skálázási beállításokat az előfizetésében felderítése</span><span class="sxs-lookup"><span data-stu-id="ef7e8-106">Discover hello Autoscale settings in your subscription</span></span>
<span data-ttu-id="ef7e8-107">Felderíthetők az összes hello erőforrásokat, amelynek automatikus skálázás alkalmazható Azure-figyelőben.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-107">You can discover all hello resources for which Autoscale is applicable in Azure Monitor.</span></span> <span data-ttu-id="ef7e8-108">Lépésenkénti útmutató lépéseit követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="ef7e8-108">Use hello following steps for a step-by-step walkthrough:</span></span>

1. <span data-ttu-id="ef7e8-109">Nyissa meg hello [Azure-portálon.][1]</span><span class="sxs-lookup"><span data-stu-id="ef7e8-109">Open hello [Azure portal.][1]</span></span>
2. <span data-ttu-id="ef7e8-110">Kattintson a hello Azure figyelő ikonra hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-110">Click hello Azure Monitor icon in hello left pane.</span></span>
  <span data-ttu-id="ef7e8-111">![Nyissa meg az Azure-figyelő][2]</span><span class="sxs-lookup"><span data-stu-id="ef7e8-111">![Open Azure Monitor][2]</span></span>
3. <span data-ttu-id="ef7e8-112">Kattintson a **automatikus skálázás** tooview mely automatikus skálázás összes hello erőforrásokat kell alkalmazni, a jelenlegi állapotuk automatikus skálázás együtt.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-112">Click **Autoscale** tooview all hello resources for which Autoscale is applicable, along with their current Autoscale status.</span></span>
  <span data-ttu-id="ef7e8-113">![Az Azure a figyelő automatikus skálázás felderítése][3]</span><span class="sxs-lookup"><span data-stu-id="ef7e8-113">![Discover Autoscale in Azure Monitor][3]</span></span>

<span data-ttu-id="ef7e8-114">Használhat hello keresőablak hello felső tooscope hello lista tooselect erőforrásokhoz egy adott erőforráscsoport, adott erőforrástípusokra, vagy egy adott erőforrás le.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-114">You can use hello filter pane at hello top tooscope down hello list tooselect resources in a specific resource group, specific resource types, or a specific resource.</span></span>

<span data-ttu-id="ef7e8-115">Az egyes erőforrások hello aktuális példányszám és hello automatikus skálázás állapot talál.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-115">For each resource, you will find hello current instance count and hello Autoscale status.</span></span> <span data-ttu-id="ef7e8-116">hello automatikus skálázás állapota lehet:</span><span class="sxs-lookup"><span data-stu-id="ef7e8-116">hello Autoscale status can be:</span></span>

- <span data-ttu-id="ef7e8-117">**Nincs konfigurálva**: nincs engedélyezve automatikus skálázás még ennél az erőforrásnál.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-117">**Not configured**: You have not enabled Autoscale yet for this resource.</span></span>
- <span data-ttu-id="ef7e8-118">**Engedélyezett**: automatikus skálázás engedélyezett ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-118">**Enabled**: You have enabled Autoscale for this resource.</span></span>
- <span data-ttu-id="ef7e8-119">**Letiltott**: automatikus skálázás le van tiltva ennél az erőforrásnál.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-119">**Disabled**: You have disabled Autoscale for this resource.</span></span>

## <a name="create-your-first-autoscale-setting"></a><span data-ttu-id="ef7e8-120">Az első automatikus skálázási beállítás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef7e8-120">Create your first Autoscale setting</span></span>

<span data-ttu-id="ef7e8-121">Most már halad át egy egyszerű lépésenkénti útmutató toocreate az első automatikus skálázási beállítás.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-121">Let's now go through a simple step-by-step walkthrough toocreate your first Autoscale setting.</span></span>

1. <span data-ttu-id="ef7e8-122">Nyissa meg hello **automatikus skálázás** panel az Azure-figyelő, és válassza ki a megjeleníteni kívánt tooscale erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-122">Open hello **Autoscale** blade in Azure Monitor and select a resource that you want tooscale.</span></span> <span data-ttu-id="ef7e8-123">(hello lépések használni a webalkalmazáshoz tartozó App Service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-123">(hello following steps use an App Service plan associated with a web app.</span></span> <span data-ttu-id="ef7e8-124">Is [az első ASP.NET-webalkalmazás létrehozása az Azure-ban 5 perc múlva.] [4])</span><span class="sxs-lookup"><span data-stu-id="ef7e8-124">You can [create your first ASP.NET web app in Azure in 5 minutes.][4])</span></span>
2. <span data-ttu-id="ef7e8-125">Vegye figyelembe, hogy hello aktuális példányszám 1.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-125">Note that hello current instance count is 1.</span></span> <span data-ttu-id="ef7e8-126">Kattintson a **engedélyezze az automatikus skálázás**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-126">Click **Enable autoscale**.</span></span>
  <span data-ttu-id="ef7e8-127">![Új webalkalmazás skálázási beállítást][5]</span><span class="sxs-lookup"><span data-stu-id="ef7e8-127">![Scale setting for new web app][5]</span></span>
3. <span data-ttu-id="ef7e8-128">Adjon meg egy nevet hello skálázási beállítást, és kattintson **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-128">Provide a name for hello scale setting, and then click **Add a rule**.</span></span> <span data-ttu-id="ef7e8-129">Figyelje meg hello skálázási szabály beállítások hello jobb oldalán környezetben ablaktábla megnyitása.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-129">Notice hello scale rule options that open as a context pane on hello right side.</span></span> <span data-ttu-id="ef7e8-130">Alapértelmezés szerint ez a beállítás hello beállítás tooscale a példányok száma 1, ha hello processzorszázaléka hello erőforrás nagyobb, mint 70 százalék.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-130">By default, this sets hello option tooscale your instance count by 1 if hello CPU percentage of hello resource exceeds 70 percent.</span></span> <span data-ttu-id="ef7e8-131">Az alapértelmezett értéken hagyja, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-131">Leave it at its default values and click **Add**.</span></span>
  <span data-ttu-id="ef7e8-132">![A webes alkalmazás skálázási beállítás létrehozása][6]</span><span class="sxs-lookup"><span data-stu-id="ef7e8-132">![Create scale setting for a web app][6]</span></span>
4. <span data-ttu-id="ef7e8-133">Most létrehozott az első skálázási szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-133">You've now created your first scale rule.</span></span> <span data-ttu-id="ef7e8-134">Vegye figyelembe, hogy UX azt javasolja, hogy ajánlott eljárásokat, és hogy hello "toohave ajánlott legalább egy skálázási szabály."</span><span class="sxs-lookup"><span data-stu-id="ef7e8-134">Note that hello UX recommends best practices and states that "It is recommended toohave at least one scale in rule."</span></span> <span data-ttu-id="ef7e8-135">toodo így:</span><span class="sxs-lookup"><span data-stu-id="ef7e8-135">toodo so:</span></span>
  
    <span data-ttu-id="ef7e8-136">a.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-136">a.</span></span> <span data-ttu-id="ef7e8-137">Kattintson a **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-137">Click **Add a rule**.</span></span> 

    <span data-ttu-id="ef7e8-138">b.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-138">b.</span></span> <span data-ttu-id="ef7e8-139">Állítsa be **operátor** túl**kisebb, mint**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-139">Set **Operator** too**Less than**.</span></span>

    <span data-ttu-id="ef7e8-140">c.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-140">c.</span></span> <span data-ttu-id="ef7e8-141">Állítsa be **küszöbérték** túl**20**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-141">Set **Threshold** too**20**.</span></span>

    <span data-ttu-id="ef7e8-142">d.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-142">d.</span></span> <span data-ttu-id="ef7e8-143">Állítsa be **művelet** túl**számolva csökkentése**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-143">Set **Operation** too**Decrease count by**.</span></span>

   <span data-ttu-id="ef7e8-144">Most már rendelkeznie kell a skálázási beállítást, hogy méretezik out/méretezik a CPU-használat alapján.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-144">You should now have a scale setting that scales out/scales in based on CPU usage.</span></span>
   <span data-ttu-id="ef7e8-145">![A skála CPU alapján][8]</span><span class="sxs-lookup"><span data-stu-id="ef7e8-145">![Scale based on CPU][8]</span></span>
5. <span data-ttu-id="ef7e8-146">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-146">Click **Save**.</span></span>

<span data-ttu-id="ef7e8-147">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="ef7e8-147">Congratulations!</span></span> <span data-ttu-id="ef7e8-148">Most már sikeresen létrehozta a CPU-használat alapján a webalkalmazás első skálázási beállítás tooautoscale.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-148">You've now successfully created your first scale setting tooautoscale your web app based on CPU usage.</span></span>

> [!NOTE] 
> <span data-ttu-id="ef7e8-149">hello azonos lépésekre vonatkozó tooget lépések a virtuálisgép-méretezési készlet vagy a felhőbeli szerepkör-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-149">hello same steps are applicable tooget started with a virtual machine scale set or cloud service role.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="ef7e8-150">Egyéb szempontok</span><span class="sxs-lookup"><span data-stu-id="ef7e8-150">Other considerations</span></span>
### <a name="scale-based-on-a-schedule"></a><span data-ttu-id="ef7e8-151">A skála a ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="ef7e8-151">Scale based on a schedule</span></span>
<span data-ttu-id="ef7e8-152">Ezenkívül tooscale alapján a CPU, beállíthatja a skála másképp hello hét adott napjaira.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-152">In addition tooscale based on CPU, you can set your scale differently for specific days of hello week.</span></span>

1. <span data-ttu-id="ef7e8-153">Kattintson a **méretezési feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-153">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="ef7e8-154">Hello skála mód és hello szabályok beállítása az hello azonos hello alapértelmezett feltételként.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-154">Setting hello scale mode and hello rules is hello same as hello default condition.</span></span>
3. <span data-ttu-id="ef7e8-155">Válassza ki **ismételje meg az adott napokra** hello ütemezéshez.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-155">Select **Repeat specific days** for hello schedule.</span></span>
4. <span data-ttu-id="ef7e8-156">Válassza ki a hello nap és hello kezdő/záró idő, amikor hello méretezési feltétel alkalmazni kívánja a.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-156">Select hello days and hello start/end time for when hello scale condition should be applied.</span></span>

![Skála feltétel ütemezésen alapuló][9]
### <a name="scale-differently-on-specific-dates"></a><span data-ttu-id="ef7e8-158">Az adott dátumok másképp méretezése</span><span class="sxs-lookup"><span data-stu-id="ef7e8-158">Scale differently on specific dates</span></span>
<span data-ttu-id="ef7e8-159">Továbbá a CPU-alapú tooscale, adhatja meg a skála másképp konkrét dátumokat.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-159">In addition tooscale based on CPU, you can set your scale differently for specific dates.</span></span>

1. <span data-ttu-id="ef7e8-160">Kattintson a **méretezési feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-160">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="ef7e8-161">Hello skála mód és hello szabályok beállítása az hello azonos hello alapértelmezett feltételként.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-161">Setting hello scale mode and hello rules is hello same as hello default condition.</span></span>
3. <span data-ttu-id="ef7e8-162">Válassza ki **adja meg a kezdő és záró dátum** hello ütemezéshez.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-162">Select **Specify start/end dates** for hello schedule.</span></span>
4. <span data-ttu-id="ef7e8-163">Jelölje ki a hello kezdő és záró dátumát és hello kezdő/záró időpontját amikor hello méretezési feltételt kell alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-163">Select hello start/end dates and hello start/end time for when hello scale condition should be applied.</span></span>

![Skála feltétel dátumok alapján][10]

### <a name="view-hello-scale-history-of-your-resource"></a><span data-ttu-id="ef7e8-165">Az erőforrás hello méretezési előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="ef7e8-165">View hello scale history of your resource</span></span>
<span data-ttu-id="ef7e8-166">Amikor az erőforrás méretezése felfelé vagy lefelé, eseményt naplózott, hello műveletnaplóban.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-166">Whenever your resource is scaled up or down, an event is logged in hello activity log.</span></span> <span data-ttu-id="ef7e8-167">Megtekintheti hello méretezési előzmények az erőforrás hello az elmúlt 24 óra toohello váltásával **futtatási előzményei** fülre.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-167">You can view hello scale history of your resource for hello past 24 hours by switching toohello **Run history** tab.</span></span>

![futtatási előzményei][11]

<span data-ttu-id="ef7e8-169">Ha azt szeretné, hogy tooview hello teljes méretezési előzmények (az akár too90 nap), jelölje be **ide toosee további részleteket**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-169">If you want tooview hello complete scale history (for up too90 days), select **Click here toosee more details**.</span></span> <span data-ttu-id="ef7e8-170">hello tevékenységnapló nyílik meg, előre kiválasztott erőforrás és kategória automatikus skálázási.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-170">hello activity log opens, with Autoscale pre-selected for your resource and category.</span></span>

### <a name="view-hello-scale-definition-of-your-resource"></a><span data-ttu-id="ef7e8-171">Hello méretezési meghatározása az erőforrás megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="ef7e8-171">View hello scale definition of your resource</span></span>
<span data-ttu-id="ef7e8-172">Automatikus skálázás egy Azure Resource Manager szerinti erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-172">Autoscale is an Azure Resource Manager resource.</span></span> <span data-ttu-id="ef7e8-173">Megtekintheti hello méretezési definition JSON toohello váltásával **JSON** fülre.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-173">You can view hello scale definition in JSON by switching toohello **JSON** tab.</span></span>

![Skála meghatározása][12]

<span data-ttu-id="ef7e8-175">Módosíthatja a JSON-ban közvetlenül, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-175">You can make changes in JSON directly, if required.</span></span> <span data-ttu-id="ef7e8-176">Ezek a változások azok mentése után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-176">These changes will be reflected after you save them.</span></span>

### <a name="disable-autoscale-and-manually-scale-your-instances"></a><span data-ttu-id="ef7e8-177">Tiltsa le az automatikus skálázás, és manuálisan méretezhető, a példányok</span><span class="sxs-lookup"><span data-stu-id="ef7e8-177">Disable Autoscale and manually scale your instances</span></span>
<span data-ttu-id="ef7e8-178">Előfordulhat, ha szeretné, hogy az aktuális skálázási beállítást toodisable, és manuálisan méretezhető, az erőforrás időpontokat.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-178">There might be times when you want toodisable your current scale setting and manually scale your resource.</span></span>

<span data-ttu-id="ef7e8-179">Kattintson a hello **letiltásával** hello felső gombra.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-179">Click hello **Disable autoscale** button at hello top.</span></span>
<span data-ttu-id="ef7e8-180">![Tiltsa le az automatikus skálázás][13]</span><span class="sxs-lookup"><span data-stu-id="ef7e8-180">![Disable Autoscale][13]</span></span>

> [!NOTE] 
> <span data-ttu-id="ef7e8-181">Ez a beállítás letiltja a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-181">This option disables your configuration.</span></span> <span data-ttu-id="ef7e8-182">Azonban is vissza tooit automatikus skálázás megismételni engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-182">However, you can get back tooit after you enable Autoscale again.</span></span> 

<span data-ttu-id="ef7e8-183">Beállíthatja, hogy szeretné-e tooscale toomanually példányok hello száma.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-183">You can now set hello number of instances that you want tooscale toomanually.</span></span>

![Manuális méretezési készlet][14]

<span data-ttu-id="ef7e8-185">Gombra kattintva térhet vissza tooAutoscale mindig **engedélyezze az automatikus skálázás** , majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ef7e8-185">You can always return tooAutoscale by clicking **Enable autoscale** and then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef7e8-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef7e8-186">Next steps</span></span>
- [<span data-ttu-id="ef7e8-187">Hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés minden automatikus skálázási motor műveletek</span><span class="sxs-lookup"><span data-stu-id="ef7e8-187">Create an Activity Log Alert toomonitor all Autoscale engine operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [<span data-ttu-id="ef7e8-188">Hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés az összes sikertelen automatikus skálázás méretezési-a/kibővített műveletek</span><span class="sxs-lookup"><span data-stu-id="ef7e8-188">Create an Activity Log Alert toomonitor all failed Autoscale scale-in/scale-out operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

