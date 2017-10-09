---
title: "Application Insights tölcsérek aaaAzure"
description: "Ismerje meg, hogyan használhatja a tölcsérek toodiscover hogyan ügyfelek az alkalmazással való interakció."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 2a6125cf596570cfaee30bb3ff757916e90d7676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="discover-how-customers-are-using-your-application-with-hello-application-insights-funnels"></a><span data-ttu-id="a07af-103">Hogyan használják az ügyfelek a az alkalmazás az Application Insights tölcsérek hello felderítése</span><span class="sxs-lookup"><span data-stu-id="a07af-103">Discover how customers are using your application with hello Application Insights Funnels</span></span>

<span data-ttu-id="a07af-104">Hello kiemelkedő fontosságú tooyour üzleti ismertetése felhasználói élményének paraméter.</span><span class="sxs-lookup"><span data-stu-id="a07af-104">Understanding customer experience is of hello utmost importance tooyour business.</span></span> <span data-ttu-id="a07af-105">Ha az alkalmazás több szakaszt tartalmaz, a legtöbb ügyfél értékesítés hello teljes folyamaton keresztül, vagy ha befejezése esetén hello folyamat valamikor szüksége lesz tooknow.</span><span class="sxs-lookup"><span data-stu-id="a07af-105">If your application involves multiple stages, you will need tooknow if most customers are progressing through hello entire process, or if they are ending hello process at some point.</span></span> <span data-ttu-id="a07af-106">a webalkalmazás lépések egy sorozatát keresztül hello előmenetel "tölcsér" néven ismert.</span><span class="sxs-lookup"><span data-stu-id="a07af-106">hello progression through a series of steps in a web application is known as a "funnel".</span></span> <span data-ttu-id="a07af-107">A felhasználók és a figyelő részletes átváltása hello Application Insights tölcsérek toogain végértékeket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a07af-107">You can use hello Application Insights Funnels toogain insights into your users and monitor step-by-step conversion rates.</span></span> 

## <a name="get-started-with-hello-funnels-blade"></a><span data-ttu-id="a07af-108">Ismerkedés a hello tölcsérek panel</span><span class="sxs-lookup"><span data-stu-id="a07af-108">Get started with hello Funnels blade</span></span>
<span data-ttu-id="a07af-109">hello legegyszerűbb módja toolearn kapcsolatos tölcsérek toowalk, bár egy példa.</span><span class="sxs-lookup"><span data-stu-id="a07af-109">hello easiest way toolearn about Funnels is toowalk though an example.</span></span> <span data-ttu-id="a07af-110">hello alábbi ábrák bemutatják, egy e-kereskedelemhez kapcsolódó üzleti hello lépéseket tulajdonosainak időt vesz igénybe toolearn, hogy az ügyfelek hogyan kommunikáljanak az webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="a07af-110">hello following illustrations demonstrate hello steps owners of an e-commerce business would take toolearn how their customers interact with their web application.</span></span>  

### <a name="create-your-funnel"></a><span data-ttu-id="a07af-111">A tölcsér létrehozása</span><span class="sxs-lookup"><span data-stu-id="a07af-111">Create your funnel</span></span>
<span data-ttu-id="a07af-112">A tölcsér létrehozása előtt kell toodecide tooanswer kívánt hello kérdést.</span><span class="sxs-lookup"><span data-stu-id="a07af-112">Before you create your funnel, you need toodecide on hello question you want tooanswer.</span></span> <span data-ttu-id="a07af-113">Például érdemes tooknow megtekintése a kezdőlapján kattintson a hirdetmény hány ügyfél.</span><span class="sxs-lookup"><span data-stu-id="a07af-113">For example, you might want tooknow how many customers viewing your home page click on an advertisement.</span></span> <span data-ttu-id="a07af-114">Ebben a példában hello tulajdonosainak hello Fiber a Fabrikam vállalat szeretné, hogy az ügyfelek, akik felvett elemek tootheir bevásárlókocsiból vásárlás hello elmúlt hónap során vásárol tooknow hello százaléka.</span><span class="sxs-lookup"><span data-stu-id="a07af-114">In this example, hello owners of hello Fabrikam Fiber company want tooknow hello percentage of customers who make a purchase after adding items tootheir shopping cart during hello last month.</span></span>

<span data-ttu-id="a07af-115">Az alábbiakban a tölcsér érvénybe toocreate hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a07af-115">Here are hello steps they take toocreate their funnel.</span></span>

1. <span data-ttu-id="a07af-116">Gombra hello új hello tölcsérek panelen.</span><span class="sxs-lookup"><span data-stu-id="a07af-116">Click hello New button on hello Funnels blade.</span></span>
1. <span data-ttu-id="a07af-117">Válassza ki az "Előző hónap" hello időtartomány hello **időtartomány** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="a07af-117">Select hello time range of "Last month" from hello **Time Range** drop-down.</span></span> 
1. <span data-ttu-id="a07af-118">Jelölje be hello **termék oldalát** hello esemény **1. lépés** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="a07af-118">Select hello **Product page** event from hello **Step 1** drop-down list.</span></span> 
1. <span data-ttu-id="a07af-119">Jelölje be hello **Hozzáadás tooshopping bevásárlókocsiból** hello esemény **2. lépés** legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="a07af-119">Select hello **Add tooshopping cart** event from hello **Step 2** drop-down list.</span></span>
1. <span data-ttu-id="a07af-120">Jelölje be hello **kattintson a vételi** hello esemény **3. lépés** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="a07af-120">Select hello **Click purchase** event from hello **Step 3** drop-down list.</span></span>
1. <span data-ttu-id="a07af-121">A név toohello tölcsér, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="a07af-121">Add a name toohello funnel and click **Save**.</span></span>

<span data-ttu-id="a07af-122">hello következő ábra azt mutatja be, hello adatok hello tölcsérek panel állít elő.</span><span class="sxs-lookup"><span data-stu-id="a07af-122">hello following illustration demonstrates hello data hello Funnels blade generates.</span></span> <span data-ttu-id="a07af-123">Az itt hello Fabrikam tulajdonosok tekintheti meg, hogy hello múlt héten, során az ügyfelek, akik hozzá egy elem tootheir vásárlás 22.7 % befejezett hello beszerzési kosárhoz.</span><span class="sxs-lookup"><span data-stu-id="a07af-123">From here hello Fabrikam owners can see that during hello last week, 22.7% of their customers who added an item tootheir shopping cart completed hello purchase.</span></span> <span data-ttu-id="a07af-124">Is láthatják, hogy 1 % hello vevő hirdetmény kattintott, miután befejezte a vásárlást kijelentkezteti hello termék oldalát, valamint a 20 %-át az ügyfelek előtt.</span><span class="sxs-lookup"><span data-stu-id="a07af-124">They can also see that 1% of hello customers clicked an advertisement before visiting hello product page, and 20% of their customers signed out after completing their purchase.</span></span>


![Adatok tölcsérek panelről](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a><span data-ttu-id="a07af-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a07af-126">Next steps</span></span>
  * [<span data-ttu-id="a07af-127">Használat – áttekintés</span><span class="sxs-lookup"><span data-stu-id="a07af-127">Usage overview</span></span>](app-insights-usage-overview.md)
  * [<span data-ttu-id="a07af-128">Felhasználók, a munkamenetek és az események</span><span class="sxs-lookup"><span data-stu-id="a07af-128">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
  * [<span data-ttu-id="a07af-129">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="a07af-129">Retention</span></span>](app-insights-usage-retention.md)
  * [<span data-ttu-id="a07af-130">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="a07af-130">Workbooks</span></span>](app-insights-usage-workbooks.md)
  * [<span data-ttu-id="a07af-131">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a07af-131">Add user context</span></span>](app-insights-usage-send-user-context.md)
