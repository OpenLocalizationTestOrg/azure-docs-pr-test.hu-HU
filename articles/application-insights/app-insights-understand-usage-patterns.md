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
ms.date: 06/17/2017
ms.author: cfreeman
ms.openlocfilehash: 3a90cfd11cb193e303136504df44008ffd04a290
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="discover-how-customers-are-using-your-application-with-hello-application-insights-funnels"></a><span data-ttu-id="be2e2-103">Hogyan használják az ügyfelek a az alkalmazás az Application Insights tölcsérek hello felderítése</span><span class="sxs-lookup"><span data-stu-id="be2e2-103">Discover how customers are using your application with hello Application Insights Funnels</span></span>

<span data-ttu-id="be2e2-104">Hello kiemelkedő fontosságú tooyour üzleti ismertetése felhasználói élményének paraméter.</span><span class="sxs-lookup"><span data-stu-id="be2e2-104">Understanding customer experience is of hello utmost importance tooyour business.</span></span> <span data-ttu-id="be2e2-105">Ha az alkalmazás több szakaszt tartalmaz, a legtöbb ügyfél értékesítés hello teljes folyamaton keresztül, vagy ha befejezése esetén hello folyamat valamikor szüksége lesz tooknow.</span><span class="sxs-lookup"><span data-stu-id="be2e2-105">If your application involves multiple stages, you will need tooknow if most customers are progressing through hello entire process, or if they are ending hello process at some point.</span></span> <span data-ttu-id="be2e2-106">a webalkalmazás lépések egy sorozatát keresztül hello előmenetel "tölcsér" néven ismert.</span><span class="sxs-lookup"><span data-stu-id="be2e2-106">hello progression through a series of steps in a web application is known as a "funnel".</span></span> <span data-ttu-id="be2e2-107">A felhasználók és a figyelő részletes átváltása hello Application Insights tölcsérek toogain végértékeket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="be2e2-107">You can use hello Application Insights Funnels toogain insights into your users and monitor step-by-step conversion rates.</span></span> 

## <a name="get-started-with-hello-funnels-blade"></a><span data-ttu-id="be2e2-108">Ismerkedés a hello tölcsérek panel</span><span class="sxs-lookup"><span data-stu-id="be2e2-108">Get started with hello Funnels blade</span></span>
<span data-ttu-id="be2e2-109">hello legegyszerűbb módja toolearn kapcsolatos tölcsérek toowalk, bár egy példa.</span><span class="sxs-lookup"><span data-stu-id="be2e2-109">hello easiest way toolearn about Funnels is toowalk though an example.</span></span> <span data-ttu-id="be2e2-110">hello alábbi ábrák bemutatják, egy e-kereskedelemhez kapcsolódó üzleti hello lépéseket tulajdonosainak időt vesz igénybe toolearn, hogy az ügyfelek hogyan kommunikáljanak az webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="be2e2-110">hello following illustrations demonstrate hello steps owners of an e-commerce business would take toolearn how their customers interact with their web application.</span></span>  

### <a name="create-your-funnel"></a><span data-ttu-id="be2e2-111">A tölcsér létrehozása</span><span class="sxs-lookup"><span data-stu-id="be2e2-111">Create your funnel</span></span>
<span data-ttu-id="be2e2-112">A tölcsér létrehozása előtt kell toodecide tooanswer kívánt hello kérdést.</span><span class="sxs-lookup"><span data-stu-id="be2e2-112">Before you create your funnel, you need toodecide on hello question you want tooanswer.</span></span> <span data-ttu-id="be2e2-113">Például érdemes tooknow megtekintése a kezdőlapján kattintson a hirdetmény hány ügyfél.</span><span class="sxs-lookup"><span data-stu-id="be2e2-113">For example, you might want tooknow how many customers viewing your home page click on an advertisement.</span></span> <span data-ttu-id="be2e2-114">Ebben a példában hello tulajdonosainak hello Fiber a Fabrikam vállalat szeretné, hogy az ügyfelek, akik felvett elemek tootheir bevásárlókocsiból vásárlás hello elmúlt hónap során vásárol tooknow hello százaléka.</span><span class="sxs-lookup"><span data-stu-id="be2e2-114">In this example, hello owners of hello Fabrikam Fiber company want tooknow hello percentage of customers who make a purchase after adding items tootheir shopping cart during hello last month.</span></span>

<span data-ttu-id="be2e2-115">Az alábbiakban a tölcsér érvénybe toocreate hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="be2e2-115">Here are hello steps they take toocreate their funnel.</span></span>

1. <span data-ttu-id="be2e2-116">Gombra hello új hello tölcsérek panelen.</span><span class="sxs-lookup"><span data-stu-id="be2e2-116">Click hello New button on hello Funnels blade.</span></span>
1. <span data-ttu-id="be2e2-117">Válassza ki az "Előző hónap" hello időtartomány hello **időtartomány** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="be2e2-117">Select hello time range of "Last month" from hello **Time Range** drop-down.</span></span> 
1. <span data-ttu-id="be2e2-118">Jelölje be hello **termék oldalát** hello esemény **1. lépés** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="be2e2-118">Select hello **Product page** event from hello **Step 1** drop-down list.</span></span> 
1. <span data-ttu-id="be2e2-119">Jelölje be hello **Hozzáadás tooshopping bevásárlókocsiból** hello esemény **2. lépés** legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="be2e2-119">Select hello **Add tooshopping cart** event from hello **Step 2** drop-down list.</span></span>
1. <span data-ttu-id="be2e2-120">Jelölje be hello **kattintson a vételi** hello esemény **3. lépés** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="be2e2-120">Select hello **Click purchase** event from hello **Step 3** drop-down list.</span></span>
1. <span data-ttu-id="be2e2-121">A név toohello tölcsér, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="be2e2-121">Add a name toohello funnel and click **Save**.</span></span>

<span data-ttu-id="be2e2-122">hello következő ábra azt mutatja be, hello adatok hello tölcsérek panel állít elő.</span><span class="sxs-lookup"><span data-stu-id="be2e2-122">hello following illustration demonstrates hello data hello Funnels blade generates.</span></span> <span data-ttu-id="be2e2-123">Az itt hello Fabrikam tulajdonosok tekintheti meg, hogy hello múlt héten, során az ügyfelek, akik hozzá egy elem tootheir vásárlás 22.7 % befejezett hello beszerzési kosárhoz.</span><span class="sxs-lookup"><span data-stu-id="be2e2-123">From here hello Fabrikam owners can see that during hello last week, 22.7% of their customers who added an item tootheir shopping cart completed hello purchase.</span></span> <span data-ttu-id="be2e2-124">Is láthatják, hogy 1 % hello vevő hirdetmény kattintott, miután befejezte a vásárlást kijelentkezteti hello termék oldalát, valamint a 20 %-át az ügyfelek előtt.</span><span class="sxs-lookup"><span data-stu-id="be2e2-124">They can also see that 1% of hello customers clicked an advertisement before visiting hello product page, and 20% of their customers signed out after completing their purchase.</span></span>


![Adatok tölcsérek panelről](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a><span data-ttu-id="be2e2-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be2e2-126">Next steps</span></span>
- <span data-ttu-id="be2e2-127">További információ [használatelemzést](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="be2e2-127">Learn more about [usage analysis](app-insights-usage-overview.md).</span></span> 
