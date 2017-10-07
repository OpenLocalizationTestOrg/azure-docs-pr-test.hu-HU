---
title: "egyéni mértéket az Azure által az automatikus skálázási lépései aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale az erőforrás által egyéni mértéket az Azure-ban."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: d3e268ec322698d0d367361cd9c156b21e0fb6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a><span data-ttu-id="b8c4b-103">Ismerkedés az automatikus skálázási által egyéni mértéket az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b8c4b-103">Get started with auto scale by custom metric in Azure</span></span>
<span data-ttu-id="b8c4b-104">Ez a cikk ismerteti, hogyan tooscale az erőforrás által az Azure portálon egyéni metrika.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-104">This article describes how tooscale your resource by a custom metric in Azure portal.</span></span>

<span data-ttu-id="b8c4b-105">Az Azure a figyelő automatikus méretezési csak tooVirtual gép méretezési készletek (VMSS), felhőszolgáltatások, app service-csomagokról és app service Environment-környezetek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-105">Azure Monitor auto scale applies only tooVirtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="b8c4b-106">Lehetővé teszi, hogy első lépései</span><span class="sxs-lookup"><span data-stu-id="b8c4b-106">Lets get started</span></span>
<span data-ttu-id="b8c4b-107">Ez a cikk feltételezi, hogy rendelkezik-e egy webalkalmazást az application insights szolgáltatással konfigurált.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-107">This article assumes that you have a web app with application insights configured.</span></span> <span data-ttu-id="b8c4b-108">Ha egy már nem rendelkezik, akkor [Application Insights beállítása az ASP.NET-webhely][1]</span><span class="sxs-lookup"><span data-stu-id="b8c4b-108">If you don't have one already, you can [set up Application Insights for your ASP.NET website][1]</span></span>

- <span data-ttu-id="b8c4b-109">Nyissa meg [Azure-portálon][2]</span><span class="sxs-lookup"><span data-stu-id="b8c4b-109">Open [Azure portal][2]</span></span>
- <span data-ttu-id="b8c4b-110">Kattintson a bal oldali navigációs panelen hello Azure figyelő ikonra.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-110">Click on Azure Monitor icon in hello left navigation pane.</span></span>
  <span data-ttu-id="b8c4b-111">![Az Azure-figyelő indítása][3]</span><span class="sxs-lookup"><span data-stu-id="b8c4b-111">![Launch Azure Monitor][3]</span></span>
- <span data-ttu-id="b8c4b-112">Kattintson az automatikus skálázási beállítás tooview, amelyen az automatikus méretezési nem alkalmazható, az automatikus skálázás aktuális állapotával együtt minden hello erőforrások ![automatikus méretezési Azure figyelőben felderítése][4]</span><span class="sxs-lookup"><span data-stu-id="b8c4b-112">Click on Autoscale setting tooview all hello resources for which auto scale is applicable, along with its current autoscale status ![Discover auto scale in Azure monitor][4]</span></span>
- <span data-ttu-id="b8c4b-113">Azure-figyelő a "automatikus skálázási" panel megnyitásához, és válassza ki a kívánt tooscale erőforrás</span><span class="sxs-lookup"><span data-stu-id="b8c4b-113">Open 'Autoscale' blade in Azure Monitor and select a resource you want tooscale</span></span>
> <span data-ttu-id="b8c4b-114">Megjegyzés: hello lépéseket az app service-csomag, amely rendelkezik beállított app insights webes alkalmazáshoz kapcsolódó használja.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-114">Note: hello steps below use an app service plan associated with a web app that has app insights configured.</span></span>
- <span data-ttu-id="b8c4b-115">Hello skálázási beállítást a panelen hello erőforrás láthatja, hogy hello aktuális példányszám 1.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-115">In hello scale setting blade for hello resource, notice that hello current instance count is 1.</span></span> <span data-ttu-id="b8c4b-116">Kattintson az "Enable automatikus skálázás".</span><span class="sxs-lookup"><span data-stu-id="b8c4b-116">Click on 'Enable autoscale'.</span></span>
  <span data-ttu-id="b8c4b-117">![Új webalkalmazás skálázási beállítást][5]</span><span class="sxs-lookup"><span data-stu-id="b8c4b-117">![Scale setting for new web app][5]</span></span>
- <span data-ttu-id="b8c4b-118">Adjon meg egy nevet a hello méretezés beállítása és hello kattintson a "Szabály hozzáadása".</span><span class="sxs-lookup"><span data-stu-id="b8c4b-118">Provide a name for hello scale setting, and hello click on "Add a rule".</span></span> <span data-ttu-id="b8c4b-119">Figyelje meg hello skálázási szabály beállítások egy környezet ablaktáblát hello jobb oldalt megnyíló.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-119">Notice hello scale rule options that opens as a context pane in hello right hand side.</span></span> <span data-ttu-id="b8c4b-120">Alapértelmezés szerint állítja a hello beállítás tooscale példány száma 1, ha hello CPU percetage hello erőforrás meghaladja a 70 %.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-120">By default, it sets hello option tooscale your instance count by 1 if hello CPU percetage of hello resource exceeds 70%.</span></span> <span data-ttu-id="b8c4b-121">Hello metrika forrás módosítása hello felső túl "Application Insights", válassza ki hello app insights-erőforrás hello "Resource" legördülő, és jelölje ki hello egyéni metrika alapján kívánja tooscale.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-121">Change hello metric source at hello top too"Application Insights", select hello app insights resource in hello 'Resource' dropdown and then select hello custom metric based on which you want tooscale.</span></span>
  <span data-ttu-id="b8c4b-122">![Egyéni metrika szerint][6]</span><span class="sxs-lookup"><span data-stu-id="b8c4b-122">![Scale by custom metric][6]</span></span>
- <span data-ttu-id="b8c4b-123">A fenti hasonló toohello lépés, amelyek a méretezhető és csökkentése hello méretezési száma 1 hello egyéni metrika esetén is a küszöbérték alá skálázási szabály hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-123">Similar toohello step above, add a scale rule that will scale in and decrease hello scale count by 1 if hello custom metric is below a threshold.</span></span>
  <span data-ttu-id="b8c4b-124">![A skála cpu alapján][7]</span><span class="sxs-lookup"><span data-stu-id="b8c4b-124">![Scale based on cpu][7]</span></span>
- <span data-ttu-id="b8c4b-125">Ön példány korlátok hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-125">Set hello you instance limits.</span></span> <span data-ttu-id="b8c4b-126">Például, ha azt szeretné, attól függően, hogy az egyéni mérték ingadozását hello 2 – 5 példányai között tooscale, állítsa be "minimális" túl "2", "maximális" túl "5" és "alapértelmezett" túl 2</span><span class="sxs-lookup"><span data-stu-id="b8c4b-126">For example, if you want tooscale between 2-5 instances depending on hello custom metric fluctuations, set 'minimum' too'2', 'maximum' too'5' and 'default' too'2'</span></span>
> <span data-ttu-id="b8c4b-127">Megjegyzés: hello erőforrás metrikáit olvasása probléma merül fel, és hello aktuális kapacitás hello alapértelmezett kapacitásértéket alatt van, majd tooensure hello rendelkezésre állási hello erőforrás automatikus skálázási lehessen méretezni toohello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-127">Note: In case there is a problem reading hello resource metrics and hello current capacity is below hello default capacity, then tooensure hello availability of hello resource, Autoscale will scale out toohello default value.</span></span> <span data-ttu-id="b8c4b-128">Ha hello aktuális kapacitás már nagyobb, mint az alapértelmezett kapacitásértéket, automatikus skálázás nem méretezni a.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-128">If hello current capacity is already higher than default capacity, Autoscale will not scale in.</span></span>
- <span data-ttu-id="b8c4b-129">Kattintson a "Mentés"</span><span class="sxs-lookup"><span data-stu-id="b8c4b-129">Click on 'Save'</span></span>

<span data-ttu-id="b8c4b-130">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="b8c4b-130">Congratulations.</span></span> <span data-ttu-id="b8c4b-131">Miután sikeresen megtörtént a skálázási beállítás tooauto méretezni a webalkalmazás egyéni metrika alapján.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-131">You now now succesfully created your scale setting tooauto scale your web app based on a custom metric.</span></span>

> <span data-ttu-id="b8c4b-132">Megjegyzés: hello azonos lépésekre vonatkozó tooget VMSS vagy a felhőbeli szolgáltatás szerepkör elindult.</span><span class="sxs-lookup"><span data-stu-id="b8c4b-132">Note: hello same steps are applicable tooget started with a VMSS or cloud service role.</span></span>

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
