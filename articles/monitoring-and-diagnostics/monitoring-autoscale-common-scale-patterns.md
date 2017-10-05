---
title: "Közös automatikus skálázás minták áttekintése |} Microsoft Docs"
description: "Ismerje meg a közös minták automatikus méretezési némelyike az erőforrás az Azure-ban."
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
ms.openlocfilehash: fce51546e041c8989d813c3935e058c52b38ba77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-common-autoscale-patterns"></a><span data-ttu-id="b2114-103">Közös automatikus skálázás minták áttekintése</span><span class="sxs-lookup"><span data-stu-id="b2114-103">Overview of common autoscale patterns</span></span>
<span data-ttu-id="b2114-104">Ez a cikk ismerteti az egyes közös mintázatokat az erőforrás méretezése az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b2114-104">This article describes some of the common patterns to scale your resource in Azure.</span></span>

<span data-ttu-id="b2114-105">Az Azure a figyelő automatikus méretezési csak a virtuális gép méretezési készletek (VMSS), a felhőszolgáltatások, az app service-csomagokról és a app service Environment-környezetek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b2114-105">Azure Monitor auto scale applies only to Virtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="b2114-106">Lehetővé teszi, hogy első lépései</span><span class="sxs-lookup"><span data-stu-id="b2114-106">Lets get started</span></span>

<span data-ttu-id="b2114-107">Ez a cikk feltételezi, hogy Ön ismeri a automatikus méretezési.</span><span class="sxs-lookup"><span data-stu-id="b2114-107">This article assumes that you are familiar with auto scale.</span></span> <span data-ttu-id="b2114-108">Is [első lépések az erőforrás méretezése][1].</span><span class="sxs-lookup"><span data-stu-id="b2114-108">You can [get started here to scale your resource][1].</span></span> <span data-ttu-id="b2114-109">Az alábbiakban néhány gyakori méretezési mintázatokat.</span><span class="sxs-lookup"><span data-stu-id="b2114-109">The following are some of the common scale patterns.</span></span>

## <a name="scale-based-on-cpu"></a><span data-ttu-id="b2114-110">A skála CPU alapján</span><span class="sxs-lookup"><span data-stu-id="b2114-110">Scale based on CPU</span></span>

<span data-ttu-id="b2114-111">A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és</span><span class="sxs-lookup"><span data-stu-id="b2114-111">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="b2114-112">Kívánt méretezési out/skála alapján CPU.</span><span class="sxs-lookup"><span data-stu-id="b2114-112">You want to scale out/scale in based on CPU.</span></span>
- <span data-ttu-id="b2114-113">Emellett szeretné gondoskodjon arról, hogy a példányok minimális száma.</span><span class="sxs-lookup"><span data-stu-id="b2114-113">Additionally, you want to ensure there is a minimum number of instances.</span></span> 
- <span data-ttu-id="b2114-114">Emellett szeretne biztosítani a maximális korlát értékre révén példányainak számát.</span><span class="sxs-lookup"><span data-stu-id="b2114-114">Also, you want to ensure that you set a maximum limit to the number of instances you can scale to.</span></span>

![A skála CPU alapján][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a><span data-ttu-id="b2114-116">Hétköznapokon vs hétvégén másképp méretezése</span><span class="sxs-lookup"><span data-stu-id="b2114-116">Scale differently on weekdays vs weekends</span></span>

<span data-ttu-id="b2114-117">A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és</span><span class="sxs-lookup"><span data-stu-id="b2114-117">You have a web app (/VMSS/cloud service role) and</span></span>

- <span data-ttu-id="b2114-118">(A hétköznapokon) alapértelmezés szerint a 3 példányokat szeretne.</span><span class="sxs-lookup"><span data-stu-id="b2114-118">You want 3 instances by default (on weekdays)</span></span>
- <span data-ttu-id="b2114-119">Nem várt forgalom hétvégén, és ezért szeretné hétvégén le 1 példány méretezése.</span><span class="sxs-lookup"><span data-stu-id="b2114-119">You don't expect traffic on weekends and hence you want to scale down to 1 instance on weekends.</span></span>

![Hétköznapokon vs hétvégén másképp méretezése][3]

## <a name="scale-differently-during-holidays"></a><span data-ttu-id="b2114-121">Méretezhető másképp ünnepek során</span><span class="sxs-lookup"><span data-stu-id="b2114-121">Scale differently during holidays</span></span>

<span data-ttu-id="b2114-122">A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és</span><span class="sxs-lookup"><span data-stu-id="b2114-122">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="b2114-123">Kívánt felfelé vagy lefelé méretezési alapértelmezés szerint a CPU-használat alapján</span><span class="sxs-lookup"><span data-stu-id="b2114-123">You want to scale up/down based on CPU usage by default</span></span>
- <span data-ttu-id="b2114-124">Azonban során ünnepi (vagy adott nap, a vállalkozása számára fontos) szeretne felülírja az alapértelmezett értékeket, és további kapacitás a rendelkezésére.</span><span class="sxs-lookup"><span data-stu-id="b2114-124">However, during holiday season (or specific days that are important for your business) you want to override the defaults and have more capacity at your disposal.</span></span>

![Skála eltérően a munkaszüneti napokat is][4]

## <a name="scale-based-on-custom-metric"></a><span data-ttu-id="b2114-126">Egyéni metrika alapuló méretezési</span><span class="sxs-lookup"><span data-stu-id="b2114-126">Scale based on custom metric</span></span>

<span data-ttu-id="b2114-127">Rendelkezik egy előtér-webkiszolgáló és a háttérkiszolgáló kommunikáló API réteget.</span><span class="sxs-lookup"><span data-stu-id="b2114-127">You have a web front end and a API tier that communicates with the backend.</span></span> 

- <span data-ttu-id="b2114-128">Az API-réteg elérhető az előtérben lévő egyéni események alapján méretezésére (Példa: a bevásárlókocsiban elemek száma. a kivétel folyamat méretezésére)</span><span class="sxs-lookup"><span data-stu-id="b2114-128">You want to scale the API tier based on custom events in the front end (example: You want to scale your checkout process based on the number of items in the shopping cart)</span></span>

![Egyéni metrika alapuló méretezési][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png