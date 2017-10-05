---
title: "Az Azure Mobile Engagement megvalósítási játék alkalmazás"
description: "Azure Mobile Engagement végrehajtásához szóló forgatókönyvet játékok"
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
ms.openlocfilehash: 0ca35a3d634db8eb5c63afacba046a35b8a3e7ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a><span data-ttu-id="65f96-103">Mobile Engagement az Játékalkalmazásról megvalósítása</span><span class="sxs-lookup"><span data-stu-id="65f96-103">Implement Mobile Engagement with Gaming App</span></span>
## <a name="overview"></a><span data-ttu-id="65f96-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="65f96-104">Overview</span></span>
<span data-ttu-id="65f96-105">Egy játék kezdeti elindította az új alapú halászati role-play/stratégia játék alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="65f96-105">A gaming start-up has launched a new fishing based role-play/strategy game app.</span></span> <span data-ttu-id="65f96-106">A game már működik, és 6 hónapig.</span><span class="sxs-lookup"><span data-stu-id="65f96-106">The game has been up and running for 6 months.</span></span> <span data-ttu-id="65f96-107">A game hatalmas sikeres, és több millió letöltések rendelkezik pedig a megőrzési nagyon magas kezdeti játék alkalmazások képest.</span><span class="sxs-lookup"><span data-stu-id="65f96-107">This game is a huge success, and it has millions of downloads and the retention is very high compared to other start-up game apps.</span></span> <span data-ttu-id="65f96-108">A negyedéves felülvizsgálati értekezleten érdekelt felek elfogadja átlagos bevétel felhasználónként (ARPU) növelnie kell azokat.</span><span class="sxs-lookup"><span data-stu-id="65f96-108">At the quarterly review meeting, stakeholders agree they need to increase average revenue per user (ARPU).</span></span> <span data-ttu-id="65f96-109">Prémium szintű a játékbeli csomagok elérhetők, különleges ajánlatokat.</span><span class="sxs-lookup"><span data-stu-id="65f96-109">Premium in-game packages are available as special offers.</span></span> <span data-ttu-id="65f96-110">Ezek a csomagok játék engedélyezése a felhasználók számára a Megjelenés és a teljesítmény halászati sorok és lures vagy a játék tackles frissítése.</span><span class="sxs-lookup"><span data-stu-id="65f96-110">These game packs allow users to upgrade the appearance and performance of their fishing lines and lures or tackles in the game.</span></span> <span data-ttu-id="65f96-111">Azonban a csomag értékesítési nem nagyon alacsony.</span><span class="sxs-lookup"><span data-stu-id="65f96-111">However, package sales are very low.</span></span> <span data-ttu-id="65f96-112">Úgy döntenek, először a felhasználói élmény analytics eszközzel elemzése, és majd fejlesztése az engagement program értékesítési használatával növelje speciális Szegmentálás.</span><span class="sxs-lookup"><span data-stu-id="65f96-112">So they decide first to analyze the customer experience with an analytics tool, and then to develop an engagement program to increase sales using advanced segmentation.</span></span>

<span data-ttu-id="65f96-113">Alapján a [Azure Mobile Engagement – első lépések útmutató ajánlott eljárásoknak megfelelő beállításában](mobile-engagement-getting-started-best-practices.md) engagement-stratégia kialakítása.</span><span class="sxs-lookup"><span data-stu-id="65f96-113">Based on the [Azure Mobile Engagement - Getting Started Guide with Best practices](mobile-engagement-getting-started-best-practices.md) they build an engagement strategy.</span></span>

## <a name="objectives-and-kpis"></a><span data-ttu-id="65f96-114">Célok és a KPI-k</span><span class="sxs-lookup"><span data-stu-id="65f96-114">Objectives and KPIs</span></span>
<span data-ttu-id="65f96-115">A game megfeleljenek a kulcsfontosságú érintettekkel.</span><span class="sxs-lookup"><span data-stu-id="65f96-115">Key stakeholders for the game meet.</span></span> <span data-ttu-id="65f96-116">Az összes fogadja el az egyik fő cél - növeléséhez a prémium csomag értékesítési 15 %.</span><span class="sxs-lookup"><span data-stu-id="65f96-116">All agree on one main objective - to increase premium package sales by 15%.</span></span> <span data-ttu-id="65f96-117">Hoznak létre üzleti a fő teljesítménymutatók (KPI-k) mérték, és a meghajtó ebben a célkitűzésben</span><span class="sxs-lookup"><span data-stu-id="65f96-117">They create Business Key Performance Indicators (KPIs) to measure and drive this objective</span></span>

* <span data-ttu-id="65f96-118">A game milyen szintű a ezeket a csomagokat vásárolt?</span><span class="sxs-lookup"><span data-stu-id="65f96-118">On which level of the game are these packages purchased?</span></span>
* <span data-ttu-id="65f96-119">Mi az a bevétel felhasználónként, munkamenetenként, heti és havi?</span><span class="sxs-lookup"><span data-stu-id="65f96-119">What is the revenue per user, per session, per week, and per month?</span></span>
* <span data-ttu-id="65f96-120">Mik a kedvenc beszerzési típusok?</span><span class="sxs-lookup"><span data-stu-id="65f96-120">What are the favorite purchase types?</span></span>

<span data-ttu-id="65f96-121">/ 1. rész a [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) ismerteti, hogyan adható meg a célok és a KPI-k.</span><span class="sxs-lookup"><span data-stu-id="65f96-121">Part 1 of the [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) explains how to define the objectives and KPIs.</span></span> 

<span data-ttu-id="65f96-122">Az üzleti KPI-k már meg van adva a Mobile termék Manager hoz létre új felhasználói trendeket és a megőrzési meghatározásához bevonási KPI-k.</span><span class="sxs-lookup"><span data-stu-id="65f96-122">With the Business KPIs now defined, the Mobile Product Manager creates Engagement KPIs to determine new user trends and retention.</span></span>

* <span data-ttu-id="65f96-123">Megőrzési figyelése és a következő időszakok használja: napi, 2 naponta, hetente, havonta, és minden harmadik hónapban</span><span class="sxs-lookup"><span data-stu-id="65f96-123">Monitor retention and use across the following intervals: daily, every 2 days, weekly, monthly and every 3 months</span></span>
* <span data-ttu-id="65f96-124">Aktív felhasználók száma</span><span class="sxs-lookup"><span data-stu-id="65f96-124">Active user counts</span></span>
* <span data-ttu-id="65f96-125">Az áruházbeli alkalmazás minősítése</span><span class="sxs-lookup"><span data-stu-id="65f96-125">The app rating in the store</span></span>

<span data-ttu-id="65f96-126">Az informatikai csapat javaslatai alapján, a következő műszaki KPI-k lettek hozzáadva a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="65f96-126">Based on recommendations from the IT team, the following technical KPIs were added to answer the following questions:</span></span>

* <span data-ttu-id="65f96-127">Mi az a felhasználó elérési út (mely megnyitják, mennyi időt igénybe rajta)</span><span class="sxs-lookup"><span data-stu-id="65f96-127">What is my user path (which page is visited, how much time users spend on it)</span></span>
* <span data-ttu-id="65f96-128">Összeomlások vagy munkamenetenként észlelt hibák száma</span><span class="sxs-lookup"><span data-stu-id="65f96-128">Number of crashes or bugs encountered per session</span></span>
* <span data-ttu-id="65f96-129">Milyen operációs rendszer verzióit futtatják a felhasználók?</span><span class="sxs-lookup"><span data-stu-id="65f96-129">What OS versions are my users running?</span></span>
* <span data-ttu-id="65f96-130">Mi az, hogy a felhasználók számára képernyő átlagos mérete?</span><span class="sxs-lookup"><span data-stu-id="65f96-130">What is the average size of screen for my users?</span></span>
* <span data-ttu-id="65f96-131">Milyen típusú internetkapcsolattal rendelkeznek a felhasználók?</span><span class="sxs-lookup"><span data-stu-id="65f96-131">What kind of internet connectivity do my users have?</span></span>

<span data-ttu-id="65f96-132">Minden KPI-hez tartozó Mobile termék megadja az elemzőnek szüksége van, és hol helyezkedik el saját alkalmazástervezési adatokat.</span><span class="sxs-lookup"><span data-stu-id="65f96-132">For each KPI the Mobile Product Manager specifies the data she needs and where it is located in her playbook.</span></span>

## <a name="engagement-program-and-integration"></a><span data-ttu-id="65f96-133">Bevonási program és az integráció</span><span class="sxs-lookup"><span data-stu-id="65f96-133">Engagement program and integration</span></span>
<span data-ttu-id="65f96-134">Egy speciális bevonási program létrehozása, előtt a mobileszköz projekt igazgató feladata a projekt ismeri részletesen bemutatja, hogyan, és a felhasználók által felhasznált termékek kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="65f96-134">Before building an advanced engagement program, the Mobile Project Director in charge of the project should have a deep understanding of how and when products are consumed by the users.</span></span>

<span data-ttu-id="65f96-135">3 hónap után a mobilalkalmazás-projekt igazgató összegyűjtötte elég adat alkalmazásbeli leküldéses értesítési eladásait javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="65f96-135">After 3 months, the Mobile Project Director has collected enough data to enhance his in-app push notification sales.</span></span> <span data-ttu-id="65f96-136">Ezután Tanulja meg, amelyek:</span><span class="sxs-lookup"><span data-stu-id="65f96-136">He learns that:</span></span>

* <span data-ttu-id="65f96-137">Az első beszerzési általában akkor fordul elő, 14 szinten.</span><span class="sxs-lookup"><span data-stu-id="65f96-137">The first purchase generally happens at the level 14.</span></span> <span data-ttu-id="65f96-138">Ezekben az esetekben 90 %-át a vásárlást új legendary fegyverek $3.</span><span class="sxs-lookup"><span data-stu-id="65f96-138">For 90% of those cases, the purchase is new legendary weapons for $3.</span></span>
* <span data-ttu-id="65f96-139">Ezekben az esetekben 80 %-át, a felhasználók, akik végzett a vásárlást, a termék folytatni, és ellenőrizze a további vásárol.</span><span class="sxs-lookup"><span data-stu-id="65f96-139">In 80 % of those cases, users who have made a purchase, continue with the product and make more purchases.</span></span>
* <span data-ttu-id="65f96-140">A szint 20, akik elindíthatók egy több mint 10 $vagy hét.</span><span class="sxs-lookup"><span data-stu-id="65f96-140">Users who have passed the level 20, start to spend more than $10/week.</span></span>
* <span data-ttu-id="65f96-141">Prémium szintű csomagok szinten 16, 24 és 32 megvásárlása a felhasználók általában.</span><span class="sxs-lookup"><span data-stu-id="65f96-141">Users tend to buy premium packages at level 16, 24 and 32.</span></span>

<span data-ttu-id="65f96-142">Az elemzés környezetnek köszönhetően a mobilalkalmazás-projekt igazgató úgy dönt, hogy létrehozása adott leküldéses értesítési sorozatok app Sales növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="65f96-142">Thanks to this analysis the Mobile Project Director decides to create specific push notification sequences to increase in app sales.</span></span> <span data-ttu-id="65f96-143">Három leküldéses folyamatok, amelyek ezután meghívja hoz: üdvözli a program, értékesítési Program és inaktív Program.</span><span class="sxs-lookup"><span data-stu-id="65f96-143">He creates three push sequences which he calls: Welcome program, Sales Program, and Inactive Program.</span></span> <span data-ttu-id="65f96-144">További információ a [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]</span><span class="sxs-lookup"><span data-stu-id="65f96-144">For more information refer to the [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks) ![][1]</span></span>

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
