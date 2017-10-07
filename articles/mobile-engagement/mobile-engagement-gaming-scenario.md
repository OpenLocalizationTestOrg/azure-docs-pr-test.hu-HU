---
title: "a Mobile Engagement megvalósítási aaaAzure játék alkalmazás"
description: "Alkalmazás forgatókönyv tooimplement Azure Mobile Engagement játékok"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b82b4a868a33f42e5b759e43e66103556c097f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a><span data-ttu-id="762b9-103">Mobile Engagement az Játékalkalmazásról megvalósítása</span><span class="sxs-lookup"><span data-stu-id="762b9-103">Implement Mobile Engagement with Gaming App</span></span>
## <a name="overview"></a><span data-ttu-id="762b9-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="762b9-104">Overview</span></span>
<span data-ttu-id="762b9-105">Egy játék kezdeti elindította az új alapú halászati role-play/stratégia játék alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="762b9-105">A gaming start-up has launched a new fishing based role-play/strategy game app.</span></span> <span data-ttu-id="762b9-106">hello játék már működik, és 6 hónapig.</span><span class="sxs-lookup"><span data-stu-id="762b9-106">hello game has been up and running for 6 months.</span></span> <span data-ttu-id="762b9-107">A game hatalmas sikeres, és több millió letöltések rendelkezik pedig hello megőrzési nagyon magas összehasonlított tooother kezdeti játék alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="762b9-107">This game is a huge success, and it has millions of downloads and hello retention is very high compared tooother start-up game apps.</span></span> <span data-ttu-id="762b9-108">Hello negyedéves felülvizsgálati értekezleten az érdekelt felek elfogadja tooincrease átlagos bevétel felhasználónként (ARPU) van szükségük.</span><span class="sxs-lookup"><span data-stu-id="762b9-108">At hello quarterly review meeting, stakeholders agree they need tooincrease average revenue per user (ARPU).</span></span> <span data-ttu-id="762b9-109">Prémium szintű a játékbeli csomagok elérhetők, különleges ajánlatokat.</span><span class="sxs-lookup"><span data-stu-id="762b9-109">Premium in-game packages are available as special offers.</span></span> <span data-ttu-id="762b9-110">A game csomagok lehetővé teszik a felhasználók tooupgrade hello Megjelenés és a teljesítmény halászati sorok és lures vagy tackles hello játékban.</span><span class="sxs-lookup"><span data-stu-id="762b9-110">These game packs allow users tooupgrade hello appearance and performance of their fishing lines and lures or tackles in hello game.</span></span> <span data-ttu-id="762b9-111">Azonban a csomag értékesítési nem nagyon alacsony.</span><span class="sxs-lookup"><span data-stu-id="762b9-111">However, package sales are very low.</span></span> <span data-ttu-id="762b9-112">Úgy döntenek, első tooanalyze hello felhasználói élmény analytics eszközzel, és ezután egy bevonási program tooincrease értékesítési használatával toodevelop speciális Szegmentálás.</span><span class="sxs-lookup"><span data-stu-id="762b9-112">So they decide first tooanalyze hello customer experience with an analytics tool, and then toodevelop an engagement program tooincrease sales using advanced segmentation.</span></span>

<span data-ttu-id="762b9-113">Hello alapján [Azure Mobile Engagement – első lépések útmutató ajánlott eljárásoknak megfelelő beállításában](mobile-engagement-getting-started-best-practices.md) engagement-stratégia kialakítása.</span><span class="sxs-lookup"><span data-stu-id="762b9-113">Based on hello [Azure Mobile Engagement - Getting Started Guide with Best practices](mobile-engagement-getting-started-best-practices.md) they build an engagement strategy.</span></span>

## <a name="objectives-and-kpis"></a><span data-ttu-id="762b9-114">Célok és a KPI-k</span><span class="sxs-lookup"><span data-stu-id="762b9-114">Objectives and KPIs</span></span>
<span data-ttu-id="762b9-115">Hello játék kulcsfontosságú érintettekkel felel meg.</span><span class="sxs-lookup"><span data-stu-id="762b9-115">Key stakeholders for hello game meet.</span></span> <span data-ttu-id="762b9-116">Az összes fogadja el az egyik fő cél - tooincrease prémium csomag értékesítési 15 %-kal.</span><span class="sxs-lookup"><span data-stu-id="762b9-116">All agree on one main objective - tooincrease premium package sales by 15%.</span></span> <span data-ttu-id="762b9-117">Ezek toomeasure üzleti a fő teljesítménymutatók (KPI-k) és a meghajtó a célkitűzés létrehozása</span><span class="sxs-lookup"><span data-stu-id="762b9-117">They create Business Key Performance Indicators (KPIs) toomeasure and drive this objective</span></span>

* <span data-ttu-id="762b9-118">Mely szintjén hello játék ezeket a csomagokat vásárolt?</span><span class="sxs-lookup"><span data-stu-id="762b9-118">On which level of hello game are these packages purchased?</span></span>
* <span data-ttu-id="762b9-119">Mi az a hello bevétel felhasználónként, munkamenetenként, heti és havi?</span><span class="sxs-lookup"><span data-stu-id="762b9-119">What is hello revenue per user, per session, per week, and per month?</span></span>
* <span data-ttu-id="762b9-120">Mik azok a hello kedvenc beszerzési típusok?</span><span class="sxs-lookup"><span data-stu-id="762b9-120">What are hello favorite purchase types?</span></span>

<span data-ttu-id="762b9-121">Hello 1. rész [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) azt ismerteti, hogyan toodefine hello célkitűzések és a KPI-k.</span><span class="sxs-lookup"><span data-stu-id="762b9-121">Part 1 of hello [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) explains how toodefine hello objectives and KPIs.</span></span> 

<span data-ttu-id="762b9-122">Hello üzleti KPI-k már meg van adva hello Mobile termék Manager bevonási KPI-k toodetermine új felhasználói trendek és a megőrzési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="762b9-122">With hello Business KPIs now defined, hello Mobile Product Manager creates Engagement KPIs toodetermine new user trends and retention.</span></span>

* <span data-ttu-id="762b9-123">Megőrzési figyelése és a következő időközönként hello használja: napi, 2 naponta, hetente, havonta, és minden harmadik hónapban</span><span class="sxs-lookup"><span data-stu-id="762b9-123">Monitor retention and use across hello following intervals: daily, every 2 days, weekly, monthly and every 3 months</span></span>
* <span data-ttu-id="762b9-124">Aktív felhasználók száma</span><span class="sxs-lookup"><span data-stu-id="762b9-124">Active user counts</span></span>
* <span data-ttu-id="762b9-125">tárolja a hello hello alkalmazás minősítése</span><span class="sxs-lookup"><span data-stu-id="762b9-125">hello app rating in hello store</span></span>

<span data-ttu-id="762b9-126">A következő műszaki KPI-k hello hello informatikai csapat javaslatai alapján, a következő kérdések tooanswer hello fel lettek hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="762b9-126">Based on recommendations from hello IT team, hello following technical KPIs were added tooanswer hello following questions:</span></span>

* <span data-ttu-id="762b9-127">Mi az a felhasználó elérési út (mely megnyitják, mennyi időt igénybe rajta)</span><span class="sxs-lookup"><span data-stu-id="762b9-127">What is my user path (which page is visited, how much time users spend on it)</span></span>
* <span data-ttu-id="762b9-128">Összeomlások vagy munkamenetenként észlelt hibák száma</span><span class="sxs-lookup"><span data-stu-id="762b9-128">Number of crashes or bugs encountered per session</span></span>
* <span data-ttu-id="762b9-129">Milyen operációs rendszer verzióit futtatják a felhasználók?</span><span class="sxs-lookup"><span data-stu-id="762b9-129">What OS versions are my users running?</span></span>
* <span data-ttu-id="762b9-130">Mi az az hello átlagos mérete a képernyő a felhasználók számára?</span><span class="sxs-lookup"><span data-stu-id="762b9-130">What is hello average size of screen for my users?</span></span>
* <span data-ttu-id="762b9-131">Milyen típusú internetkapcsolattal rendelkeznek a felhasználók?</span><span class="sxs-lookup"><span data-stu-id="762b9-131">What kind of internet connectivity do my users have?</span></span>

<span data-ttu-id="762b9-132">Minden KPI-hez tartozó hello Mobile termék Manager hello adatok elemzőnek szüksége van, és hol helyezkedik el saját forgatókönyv határozza meg.</span><span class="sxs-lookup"><span data-stu-id="762b9-132">For each KPI hello Mobile Product Manager specifies hello data she needs and where it is located in her playbook.</span></span>

## <a name="engagement-program-and-integration"></a><span data-ttu-id="762b9-133">Bevonási program és az integráció</span><span class="sxs-lookup"><span data-stu-id="762b9-133">Engagement program and integration</span></span>
<span data-ttu-id="762b9-134">Egy speciális bevonási program létrehozása, előtt hello Mobile projekt igazgató feladata hello projekt rendelkeznie kell egy részletes ismertetése, hogyan és mikor termékek hello felhasználók vannak-e használni.</span><span class="sxs-lookup"><span data-stu-id="762b9-134">Before building an advanced engagement program, hello Mobile Project Director in charge of hello project should have a deep understanding of how and when products are consumed by hello users.</span></span>

<span data-ttu-id="762b9-135">Hello Mobile projekt igazgató 3 hónap után összegyűjtötte elég adat tooenhance alkalmazásbeli leküldéses értesítési eladásait.</span><span class="sxs-lookup"><span data-stu-id="762b9-135">After 3 months, hello Mobile Project Director has collected enough data tooenhance his in-app push notification sales.</span></span> <span data-ttu-id="762b9-136">Ezután Tanulja meg, amelyek:</span><span class="sxs-lookup"><span data-stu-id="762b9-136">He learns that:</span></span>

* <span data-ttu-id="762b9-137">hello első beszerzési általában 14 hello szintjén történik.</span><span class="sxs-lookup"><span data-stu-id="762b9-137">hello first purchase generally happens at hello level 14.</span></span> <span data-ttu-id="762b9-138">Ezekben az esetekben 90 %-át hello beszerzési új legendary fegyverek $3.</span><span class="sxs-lookup"><span data-stu-id="762b9-138">For 90% of those cases, hello purchase is new legendary weapons for $3.</span></span>
* <span data-ttu-id="762b9-139">Ezekben az esetekben 80 %-át, a felhasználók, akik a beszerzési hello termékkel, és további végzett vásárol.</span><span class="sxs-lookup"><span data-stu-id="762b9-139">In 80 % of those cases, users who have made a purchase, continue with hello product and make more purchases.</span></span>
* <span data-ttu-id="762b9-140">Felhasználók, akik hello szint 20, indítsa el a toospend több mint 10 $vagy hét.</span><span class="sxs-lookup"><span data-stu-id="762b9-140">Users who have passed hello level 20, start toospend more than $10/week.</span></span>
* <span data-ttu-id="762b9-141">A felhasználók általában toobuy prémium csomagok 16, 24 és 32 szinten.</span><span class="sxs-lookup"><span data-stu-id="762b9-141">Users tend toobuy premium packages at level 16, 24 and 32.</span></span>

<span data-ttu-id="762b9-142">Köszönjük toothis elemzés, Mobile projekt igazgató hello úgy dönt, toocreate meghatározott leküldéses értesítési sorozatok tooincrease app Sales.</span><span class="sxs-lookup"><span data-stu-id="762b9-142">Thanks toothis analysis hello Mobile Project Director decides toocreate specific push notification sequences tooincrease in app sales.</span></span> <span data-ttu-id="762b9-143">Három leküldéses folyamatok, amelyek ezután meghívja hoz: üdvözli a program, értékesítési Program és inaktív Program.</span><span class="sxs-lookup"><span data-stu-id="762b9-143">He creates three push sequences which he calls: Welcome program, Sales Program, and Inactive Program.</span></span> <span data-ttu-id="762b9-144">További információt talál a toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]</span><span class="sxs-lookup"><span data-stu-id="762b9-144">For more information refer toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks) ![][1]</span></span>

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
