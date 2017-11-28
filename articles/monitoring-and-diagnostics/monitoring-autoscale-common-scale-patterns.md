---
title: "annak a közös automatikus skálázás aaaOverview |} Microsoft Docs"
description: "Ismerje meg, hogy az erőforrás néhány hello közös minták tooauto skálázása az Azure-ban."
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
ms.openlocfilehash: fc5bd97852e0af01aa32940c99721ab8e21033ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-common-autoscale-patterns"></a><span data-ttu-id="73805-103">Közös automatikus skálázás minták áttekintése</span><span class="sxs-lookup"><span data-stu-id="73805-103">Overview of common autoscale patterns</span></span>
<span data-ttu-id="73805-104">Ez a cikk ismerteti, néhány hello közös minták tooscale az erőforrás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="73805-104">This article describes some of hello common patterns tooscale your resource in Azure.</span></span>

<span data-ttu-id="73805-105">Az Azure a figyelő automatikus méretezési csak tooVirtual gép méretezési készletek (VMSS), felhőszolgáltatások, app service-csomagokról és app service Environment-környezetek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="73805-105">Azure Monitor auto scale applies only tooVirtual Machine Scale Sets (VMSS), cloud services, app service plans and app service environments.</span></span> 

# <a name="lets-get-started"></a><span data-ttu-id="73805-106">Lehetővé teszi, hogy első lépései</span><span class="sxs-lookup"><span data-stu-id="73805-106">Lets get started</span></span>

<span data-ttu-id="73805-107">Ez a cikk feltételezi, hogy Ön ismeri a automatikus méretezési.</span><span class="sxs-lookup"><span data-stu-id="73805-107">This article assumes that you are familiar with auto scale.</span></span> <span data-ttu-id="73805-108">Is [elindított ide tooscale lekérni az erőforrás][1].</span><span class="sxs-lookup"><span data-stu-id="73805-108">You can [get started here tooscale your resource][1].</span></span> <span data-ttu-id="73805-109">hello közé tartoznak a következők hello közös méretezési minták.</span><span class="sxs-lookup"><span data-stu-id="73805-109">hello following are some of hello common scale patterns.</span></span>

## <a name="scale-based-on-cpu"></a><span data-ttu-id="73805-110">A skála CPU alapján</span><span class="sxs-lookup"><span data-stu-id="73805-110">Scale based on CPU</span></span>

<span data-ttu-id="73805-111">A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és</span><span class="sxs-lookup"><span data-stu-id="73805-111">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="73805-112">Azt szeretné, hogy tooscale alapján CPU out/skála.</span><span class="sxs-lookup"><span data-stu-id="73805-112">You want tooscale out/scale in based on CPU.</span></span>
- <span data-ttu-id="73805-113">Továbbá azt szeretné, hogy tooensure példányok minimális száma.</span><span class="sxs-lookup"><span data-stu-id="73805-113">Additionally, you want tooensure there is a minimum number of instances.</span></span> 
- <span data-ttu-id="73805-114">Emellett kívánt méretezheti a példányok maximális száma toohello számos beállított tooensure.</span><span class="sxs-lookup"><span data-stu-id="73805-114">Also, you want tooensure that you set a maximum limit toohello number of instances you can scale to.</span></span>

![A skála CPU alapján][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a><span data-ttu-id="73805-116">Hétköznapokon vs hétvégén másképp méretezése</span><span class="sxs-lookup"><span data-stu-id="73805-116">Scale differently on weekdays vs weekends</span></span>

<span data-ttu-id="73805-117">A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és</span><span class="sxs-lookup"><span data-stu-id="73805-117">You have a web app (/VMSS/cloud service role) and</span></span>

- <span data-ttu-id="73805-118">(A hétköznapokon) alapértelmezés szerint a 3 példányokat szeretne.</span><span class="sxs-lookup"><span data-stu-id="73805-118">You want 3 instances by default (on weekdays)</span></span>
- <span data-ttu-id="73805-119">Nem várt forgalom hétvégén, és ezért szeretne tooscale too1 példány le hétvégén.</span><span class="sxs-lookup"><span data-stu-id="73805-119">You don't expect traffic on weekends and hence you want tooscale down too1 instance on weekends.</span></span>

![Hétköznapokon vs hétvégén másképp méretezése][3]

## <a name="scale-differently-during-holidays"></a><span data-ttu-id="73805-121">Méretezhető másképp ünnepek során</span><span class="sxs-lookup"><span data-stu-id="73805-121">Scale differently during holidays</span></span>

<span data-ttu-id="73805-122">A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és</span><span class="sxs-lookup"><span data-stu-id="73805-122">You have a web app (/VMSS/cloud service role) and</span></span> 

- <span data-ttu-id="73805-123">Azt szeretné, hogy tooscale fel/le alapértelmezés szerint a CPU-használat alapján.</span><span class="sxs-lookup"><span data-stu-id="73805-123">You want tooscale up/down based on CPU usage by default</span></span>
- <span data-ttu-id="73805-124">Azonban ünnepi (vagy adott nap fontos a vállalati) során szeretné, hogy toooverride hello alapértelmezett beállításokat, és további kapacitással rendelkeznek a rendelkezésére.</span><span class="sxs-lookup"><span data-stu-id="73805-124">However, during holiday season (or specific days that are important for your business) you want toooverride hello defaults and have more capacity at your disposal.</span></span>

![Skála eltérően a munkaszüneti napokat is][4]

## <a name="scale-based-on-custom-metric"></a><span data-ttu-id="73805-126">Egyéni metrika alapuló méretezési</span><span class="sxs-lookup"><span data-stu-id="73805-126">Scale based on custom metric</span></span>

<span data-ttu-id="73805-127">Rendelkezik egy előtér-webkiszolgáló és egy API-réteghez, amelynek hello háttérrendszerrel kommunikál.</span><span class="sxs-lookup"><span data-stu-id="73805-127">You have a web front end and a API tier that communicates with hello backend.</span></span> 

- <span data-ttu-id="73805-128">Azt szeretné, hogy a hello előtér egyéni események alapján tooscale hello API réteg (Példa: azt szeretné, hogy a kivételt folyamat alapján hello elemszáma bevásárlókocsiból vásárlás hello tooscale)</span><span class="sxs-lookup"><span data-stu-id="73805-128">You want tooscale hello API tier based on custom events in hello front end (example: You want tooscale your checkout process based on hello number of items in hello shopping cart)</span></span>

![Egyéni metrika alapuló méretezési][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png