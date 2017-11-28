---
title: "Ismerkedés az Azure-ban automatikus skálázás |} Microsoft Docs"
description: "Ismerje meg, hogy az erőforrás méretezése az Azure-ban."
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
ms.openlocfilehash: 68cb624b3ef4a77e7cfc949979e0b1949c2e5535
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-autoscale-in-azure"></a><span data-ttu-id="dd869-103">Ismerkedés az Azure-ban automatikus skálázás</span><span class="sxs-lookup"><span data-stu-id="dd869-103">Get started with Autoscale in Azure</span></span>
<span data-ttu-id="dd869-104">A cikkből megtudhatja, hogyan állíthat be az automatikus skálázási beállításai a Microsoft Azure-portálon az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="dd869-104">This article describes how to set up your Autoscale settings for your resource in the Microsoft Azure portal.</span></span>

<span data-ttu-id="dd869-105">Az Azure a figyelő automatikus skálázás csak a virtuálisgép-méretezési csoportok, felhőalapú szolgáltatások, Azure App Service-csomagokról és App Service-környezetek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="dd869-105">Azure Monitor Autoscale applies only to virtual machine scale sets, cloud services, Azure App Service plans, and App Service environments.</span></span> 

## <a name="discover-the-autoscale-settings-in-your-subscription"></a><span data-ttu-id="dd869-106">Az automatikus skálázási beállításokat az előfizetésében felderítése</span><span class="sxs-lookup"><span data-stu-id="dd869-106">Discover the Autoscale settings in your subscription</span></span>
<span data-ttu-id="dd869-107">Is felderítheti, amelyek automatikus skálázás esetében alkalmazható Azure figyelőben összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="dd869-107">You can discover all the resources for which Autoscale is applicable in Azure Monitor.</span></span> <span data-ttu-id="dd869-108">Részletes útmutató a következő lépéseket használhatja:</span><span class="sxs-lookup"><span data-stu-id="dd869-108">Use the following steps for a step-by-step walkthrough:</span></span>

1. <span data-ttu-id="dd869-109">Nyissa meg a [Azure-portálon.][1]</span><span class="sxs-lookup"><span data-stu-id="dd869-109">Open the [Azure portal.][1]</span></span>
2. <span data-ttu-id="dd869-110">Kattintson az Azure-figyelő ikonra a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="dd869-110">Click the Azure Monitor icon in the left pane.</span></span>
  <span data-ttu-id="dd869-111">![Nyissa meg az Azure-figyelő][2]</span><span class="sxs-lookup"><span data-stu-id="dd869-111">![Open Azure Monitor][2]</span></span>
3. <span data-ttu-id="dd869-112">Kattintson a **automatikus skálázás** megtekintéséhez, amelynek automatikus skálázás kell alkalmazni, és azok állapotának automatikus skálázás összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="dd869-112">Click **Autoscale** to view all the resources for which Autoscale is applicable, along with their current Autoscale status.</span></span>
  <span data-ttu-id="dd869-113">![Az Azure a figyelő automatikus skálázás felderítése][3]</span><span class="sxs-lookup"><span data-stu-id="dd869-113">![Discover Autoscale in Azure Monitor][3]</span></span>

<span data-ttu-id="dd869-114">A keresőablak lefelé a listában hatókör legfelül segítségével válassza ki azokat az erőforrásokat egy adott erőforráscsoport, adott erőforrástípusokra, vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="dd869-114">You can use the filter pane at the top to scope down the list to select resources in a specific resource group, specific resource types, or a specific resource.</span></span>

<span data-ttu-id="dd869-115">Minden erőforrás található az aktuális példányszám és az automatikus skálázás állapotát.</span><span class="sxs-lookup"><span data-stu-id="dd869-115">For each resource, you will find the current instance count and the Autoscale status.</span></span> <span data-ttu-id="dd869-116">Az automatikus skálázás állapota lehet:</span><span class="sxs-lookup"><span data-stu-id="dd869-116">The Autoscale status can be:</span></span>

- <span data-ttu-id="dd869-117">**Nincs konfigurálva**: nincs engedélyezve automatikus skálázás még ennél az erőforrásnál.</span><span class="sxs-lookup"><span data-stu-id="dd869-117">**Not configured**: You have not enabled Autoscale yet for this resource.</span></span>
- <span data-ttu-id="dd869-118">**Engedélyezett**: automatikus skálázás engedélyezett ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="dd869-118">**Enabled**: You have enabled Autoscale for this resource.</span></span>
- <span data-ttu-id="dd869-119">**Letiltott**: automatikus skálázás le van tiltva ennél az erőforrásnál.</span><span class="sxs-lookup"><span data-stu-id="dd869-119">**Disabled**: You have disabled Autoscale for this resource.</span></span>

## <a name="create-your-first-autoscale-setting"></a><span data-ttu-id="dd869-120">Az első automatikus skálázási beállítás létrehozása</span><span class="sxs-lookup"><span data-stu-id="dd869-120">Create your first Autoscale setting</span></span>

<span data-ttu-id="dd869-121">Most már halad át egy egyszerű lépésenkénti útmutató az első automatikus skálázási beállítás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="dd869-121">Let's now go through a simple step-by-step walkthrough to create your first Autoscale setting.</span></span>

1. <span data-ttu-id="dd869-122">Nyissa meg a **automatikus skálázás** panel az Azure-figyelő, és válassza a skálázni kívánt erőforrás.</span><span class="sxs-lookup"><span data-stu-id="dd869-122">Open the **Autoscale** blade in Azure Monitor and select a resource that you want to scale.</span></span> <span data-ttu-id="dd869-123">(Az alábbi lépéseket használni a webalkalmazáshoz tartozó App Service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="dd869-123">(The following steps use an App Service plan associated with a web app.</span></span> <span data-ttu-id="dd869-124">Is [az első ASP.NET-webalkalmazás létrehozása az Azure-ban 5 perc múlva.] [4])</span><span class="sxs-lookup"><span data-stu-id="dd869-124">You can [create your first ASP.NET web app in Azure in 5 minutes.][4])</span></span>
2. <span data-ttu-id="dd869-125">Vegye figyelembe, hogy az aktuális példányszám 1.</span><span class="sxs-lookup"><span data-stu-id="dd869-125">Note that the current instance count is 1.</span></span> <span data-ttu-id="dd869-126">Kattintson a **engedélyezze az automatikus skálázás**.</span><span class="sxs-lookup"><span data-stu-id="dd869-126">Click **Enable autoscale**.</span></span>
  <span data-ttu-id="dd869-127">![Új webalkalmazás skálázási beállítást][5]</span><span class="sxs-lookup"><span data-stu-id="dd869-127">![Scale setting for new web app][5]</span></span>
3. <span data-ttu-id="dd869-128">Adjon meg egy nevet a skálázási beállítást, és kattintson **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dd869-128">Provide a name for the scale setting, and then click **Add a rule**.</span></span> <span data-ttu-id="dd869-129">Nyissa meg a figyelmeztetés a skálázási szabályhoz, amelyet a környezet ablaktáblán a jobb oldalon.</span><span class="sxs-lookup"><span data-stu-id="dd869-129">Notice the scale rule options that open as a context pane on the right side.</span></span> <span data-ttu-id="dd869-130">Alapértelmezés szerint ez a beállítás méretezése a példányok száma 1, ha az erőforrás CPU aránya meghaladja a 70 %-os állítja be.</span><span class="sxs-lookup"><span data-stu-id="dd869-130">By default, this sets the option to scale your instance count by 1 if the CPU percentage of the resource exceeds 70 percent.</span></span> <span data-ttu-id="dd869-131">Az alapértelmezett értéken hagyja, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="dd869-131">Leave it at its default values and click **Add**.</span></span>
  <span data-ttu-id="dd869-132">![A webes alkalmazás skálázási beállítás létrehozása][6]</span><span class="sxs-lookup"><span data-stu-id="dd869-132">![Create scale setting for a web app][6]</span></span>
4. <span data-ttu-id="dd869-133">Most létrehozott az első skálázási szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="dd869-133">You've now created your first scale rule.</span></span> <span data-ttu-id="dd869-134">Vegye figyelembe, hogy a UX ajánlott eljárások azt javasolja, és jelzi, hogy "javasoljuk, hogy van legalább egy skálázási szabály."</span><span class="sxs-lookup"><span data-stu-id="dd869-134">Note that the UX recommends best practices and states that "It is recommended to have at least one scale in rule."</span></span> <span data-ttu-id="dd869-135">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="dd869-135">To do so:</span></span>
  
    <span data-ttu-id="dd869-136">a.</span><span class="sxs-lookup"><span data-stu-id="dd869-136">a.</span></span> <span data-ttu-id="dd869-137">Kattintson a **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dd869-137">Click **Add a rule**.</span></span> 

    <span data-ttu-id="dd869-138">b.</span><span class="sxs-lookup"><span data-stu-id="dd869-138">b.</span></span> <span data-ttu-id="dd869-139">Állítsa be **operátor** való **kisebb, mint**.</span><span class="sxs-lookup"><span data-stu-id="dd869-139">Set **Operator** to **Less than**.</span></span>

    <span data-ttu-id="dd869-140">c.</span><span class="sxs-lookup"><span data-stu-id="dd869-140">c.</span></span> <span data-ttu-id="dd869-141">Állítsa be **küszöbérték** való **20**.</span><span class="sxs-lookup"><span data-stu-id="dd869-141">Set **Threshold** to **20**.</span></span>

    <span data-ttu-id="dd869-142">d.</span><span class="sxs-lookup"><span data-stu-id="dd869-142">d.</span></span> <span data-ttu-id="dd869-143">Állítsa be **művelet** való **számolva csökkentése**.</span><span class="sxs-lookup"><span data-stu-id="dd869-143">Set **Operation** to **Decrease count by**.</span></span>

   <span data-ttu-id="dd869-144">Most már rendelkeznie kell a skálázási beállítást, hogy méretezik out/méretezik a CPU-használat alapján.</span><span class="sxs-lookup"><span data-stu-id="dd869-144">You should now have a scale setting that scales out/scales in based on CPU usage.</span></span>
   <span data-ttu-id="dd869-145">![A skála CPU alapján][8]</span><span class="sxs-lookup"><span data-stu-id="dd869-145">![Scale based on CPU][8]</span></span>
5. <span data-ttu-id="dd869-146">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="dd869-146">Click **Save**.</span></span>

<span data-ttu-id="dd869-147">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="dd869-147">Congratulations!</span></span> <span data-ttu-id="dd869-148">Most már sikeresen létrehozta a CPU-használat alapján a webalkalmazás automatikus skálázásra az első skálázási beállítást.</span><span class="sxs-lookup"><span data-stu-id="dd869-148">You've now successfully created your first scale setting to autoscale your web app based on CPU usage.</span></span>

> [!NOTE] 
> <span data-ttu-id="dd869-149">Ugyanezek a lépések Ismerkedjen meg a virtuálisgép-méretezési csoport vagy szerepkör-szolgáltatás a felhő vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="dd869-149">The same steps are applicable to get started with a virtual machine scale set or cloud service role.</span></span>

## <a name="other-considerations"></a><span data-ttu-id="dd869-150">Egyéb szempontok</span><span class="sxs-lookup"><span data-stu-id="dd869-150">Other considerations</span></span>
### <a name="scale-based-on-a-schedule"></a><span data-ttu-id="dd869-151">A skála a ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="dd869-151">Scale based on a schedule</span></span>
<span data-ttu-id="dd869-152">Mellett a skála alapján a CPU beállíthatja a skála eltérően a hét adott napjaira.</span><span class="sxs-lookup"><span data-stu-id="dd869-152">In addition to scale based on CPU, you can set your scale differently for specific days of the week.</span></span>

1. <span data-ttu-id="dd869-153">Kattintson a **méretezési feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dd869-153">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="dd869-154">A skála mód és a szabályok beállítását megegyezik az alapértelmezett feltétel.</span><span class="sxs-lookup"><span data-stu-id="dd869-154">Setting the scale mode and the rules is the same as the default condition.</span></span>
3. <span data-ttu-id="dd869-155">Válassza ki **ismételje meg az adott napokra** az ütemezéshez.</span><span class="sxs-lookup"><span data-stu-id="dd869-155">Select **Repeat specific days** for the schedule.</span></span>
4. <span data-ttu-id="dd869-156">Válassza ki a nap és a kezdő/záró idő, amikor a skála feltétel alkalmazni kívánja a.</span><span class="sxs-lookup"><span data-stu-id="dd869-156">Select the days and the start/end time for when the scale condition should be applied.</span></span>

![Skála feltétel ütemezésen alapuló][9]
### <a name="scale-differently-on-specific-dates"></a><span data-ttu-id="dd869-158">Az adott dátumok másképp méretezése</span><span class="sxs-lookup"><span data-stu-id="dd869-158">Scale differently on specific dates</span></span>
<span data-ttu-id="dd869-159">Skála alapján a CPU, mellett adhatja meg a skála másképp konkrét dátumokat.</span><span class="sxs-lookup"><span data-stu-id="dd869-159">In addition to scale based on CPU, you can set your scale differently for specific dates.</span></span>

1. <span data-ttu-id="dd869-160">Kattintson a **méretezési feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dd869-160">Click **Add a scale condition**.</span></span>
2. <span data-ttu-id="dd869-161">A skála mód és a szabályok beállítását megegyezik az alapértelmezett feltétel.</span><span class="sxs-lookup"><span data-stu-id="dd869-161">Setting the scale mode and the rules is the same as the default condition.</span></span>
3. <span data-ttu-id="dd869-162">Válassza ki **adja meg a kezdő és záró dátum** az ütemezéshez.</span><span class="sxs-lookup"><span data-stu-id="dd869-162">Select **Specify start/end dates** for the schedule.</span></span>
4. <span data-ttu-id="dd869-163">Válassza ki a kezdő és záró dátum és a kezdő/záró idő, amikor a skála feltétel alkalmazni kívánja a.</span><span class="sxs-lookup"><span data-stu-id="dd869-163">Select the start/end dates and the start/end time for when the scale condition should be applied.</span></span>

![Skála feltétel dátumok alapján][10]

### <a name="view-the-scale-history-of-your-resource"></a><span data-ttu-id="dd869-165">Az erőforrás méretezési előzményeinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="dd869-165">View the scale history of your resource</span></span>
<span data-ttu-id="dd869-166">Amikor az erőforrás méretezése felfelé vagy lefelé, eseményt naplózott, a műveletnaplóban.</span><span class="sxs-lookup"><span data-stu-id="dd869-166">Whenever your resource is scaled up or down, an event is logged in the activity log.</span></span> <span data-ttu-id="dd869-167">Megtekintheti az erőforrás méretezési előzményeinek vonatkozóan az elmúlt 24 óra átvált a **futtatási előzményei** fülre.</span><span class="sxs-lookup"><span data-stu-id="dd869-167">You can view the scale history of your resource for the past 24 hours by switching to the **Run history** tab.</span></span>

![futtatási előzményei][11]

<span data-ttu-id="dd869-169">Ha szeretné megtekinteni a teljes méretezési előzmények (90 nap), jelölje be **Ide kattintva további részleteket**.</span><span class="sxs-lookup"><span data-stu-id="dd869-169">If you want to view the complete scale history (for up to 90 days), select **Click here to see more details**.</span></span> <span data-ttu-id="dd869-170">A műveletnapló nyílik meg, előre kiválasztott erőforrás és kategória automatikus skálázási.</span><span class="sxs-lookup"><span data-stu-id="dd869-170">The activity log opens, with Autoscale pre-selected for your resource and category.</span></span>

### <a name="view-the-scale-definition-of-your-resource"></a><span data-ttu-id="dd869-171">A skála meghatározása az erőforrás megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="dd869-171">View the scale definition of your resource</span></span>
<span data-ttu-id="dd869-172">Automatikus skálázás egy Azure Resource Manager szerinti erőforrás.</span><span class="sxs-lookup"><span data-stu-id="dd869-172">Autoscale is an Azure Resource Manager resource.</span></span> <span data-ttu-id="dd869-173">Megtekintheti a skála definition a JSON-ban átvált a **JSON** fülre.</span><span class="sxs-lookup"><span data-stu-id="dd869-173">You can view the scale definition in JSON by switching to the **JSON** tab.</span></span>

![Skála meghatározása][12]

<span data-ttu-id="dd869-175">Módosíthatja a JSON-ban közvetlenül, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="dd869-175">You can make changes in JSON directly, if required.</span></span> <span data-ttu-id="dd869-176">Ezek a változások azok mentése után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dd869-176">These changes will be reflected after you save them.</span></span>

### <a name="disable-autoscale-and-manually-scale-your-instances"></a><span data-ttu-id="dd869-177">Tiltsa le az automatikus skálázás, és manuálisan méretezhető, a példányok</span><span class="sxs-lookup"><span data-stu-id="dd869-177">Disable Autoscale and manually scale your instances</span></span>
<span data-ttu-id="dd869-178">Előfordulhat, hogy mikor szeretné letiltani az aktuális skálázási beállítást, és manuálisan méretezhető, az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="dd869-178">There might be times when you want to disable your current scale setting and manually scale your resource.</span></span>

<span data-ttu-id="dd869-179">Kattintson a **letiltásával** gombra az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="dd869-179">Click the **Disable autoscale** button at the top.</span></span>
<span data-ttu-id="dd869-180">![Tiltsa le az automatikus skálázás][13]</span><span class="sxs-lookup"><span data-stu-id="dd869-180">![Disable Autoscale][13]</span></span>

> [!NOTE] 
> <span data-ttu-id="dd869-181">Ez a beállítás letiltja a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="dd869-181">This option disables your configuration.</span></span> <span data-ttu-id="dd869-182">Azonban is vissza az automatikus skálázás megismételni engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="dd869-182">However, you can get back to it after you enable Autoscale again.</span></span> 

<span data-ttu-id="dd869-183">Beállíthatja, hogy manuálisan méretezésére példányok száma.</span><span class="sxs-lookup"><span data-stu-id="dd869-183">You can now set the number of instances that you want to scale to manually.</span></span>

![Manuális méretezési készlet][14]

<span data-ttu-id="dd869-185">Kattintva mindig visszatérhet automatikus skálázás **engedélyezze az automatikus skálázás** , majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="dd869-185">You can always return to Autoscale by clicking **Enable autoscale** and then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd869-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd869-186">Next steps</span></span>
- [<span data-ttu-id="dd869-187">Hozzon létre egy tevékenység napló riasztási előfizetés minden automatikus skálázási motor műveletek figyelése</span><span class="sxs-lookup"><span data-stu-id="dd869-187">Create an Activity Log Alert to monitor all Autoscale engine operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [<span data-ttu-id="dd869-188">Hozzon létre egy tevékenység napló a riasztást az előfizetés az összes sikertelen automatikus skálázás méretezési-a/kibővített műveletek figyelése</span><span class="sxs-lookup"><span data-stu-id="dd869-188">Create an Activity Log Alert to monitor all failed Autoscale scale-in/scale-out operations on your subscription</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

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

