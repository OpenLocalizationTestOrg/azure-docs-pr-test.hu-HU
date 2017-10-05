---
title: "Hibák - Azure Logic Apps diagnosztizálása |} Microsoft Docs"
description: "Ha nem működnek a logic apps megismerése gyakori módjai"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 814e6f93088cdd96b0a663d2a7494b5a11470d99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-logic-app-failures"></a><span data-ttu-id="bda00-103">Logic app hibák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="bda00-103">Diagnose logic app failures</span></span>
<span data-ttu-id="bda00-104">A a logic apps a problémákat vagy hibákat tapasztal, van néhány módszer segítségével könnyebben áttekinthető, ahol a hibák érkező.</span><span class="sxs-lookup"><span data-stu-id="bda00-104">If you experience issues or failures with your logic apps, there are a few approaches can help you better understand where the failures are coming from.</span></span>  

## <a name="azure-portal-tools"></a><span data-ttu-id="bda00-105">Az Azure portál eszközök</span><span class="sxs-lookup"><span data-stu-id="bda00-105">Azure portal tools</span></span>
<span data-ttu-id="bda00-106">Az Azure-portálon az egyes logikai alkalmazás lépésről lépésre diagnosztizálása érdekében számos eszközt kínál.</span><span class="sxs-lookup"><span data-stu-id="bda00-106">The Azure portal provides many tools to diagnose each logic app at each step.</span></span>

### <a name="trigger-history"></a><span data-ttu-id="bda00-107">Eseményindító előzmények</span><span class="sxs-lookup"><span data-stu-id="bda00-107">Trigger history</span></span>

<span data-ttu-id="bda00-108">Minden logikai alkalmazás legalább egy eseményindító tartozik.</span><span class="sxs-lookup"><span data-stu-id="bda00-108">Each logic app has at least one trigger.</span></span> <span data-ttu-id="bda00-109">Ha azt észleli, hogy alkalmazásokat nem kiváltó, tanulmányozni, az eseményindító előzmények további információt.</span><span class="sxs-lookup"><span data-stu-id="bda00-109">If you notice that apps aren't firing, look first at the trigger history for more information.</span></span> <span data-ttu-id="bda00-110">Az eseményindító előzmények logika app'ss fő panelje érheti el.</span><span class="sxs-lookup"><span data-stu-id="bda00-110">You can access the trigger history on the logic app'ss main blade.</span></span>

![Az eseményindító előzmények keresése][1]

<span data-ttu-id="bda00-112">Az eseményindító előzmények összes eseményindító kísérletet a Logic Apps alkalmazást sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="bda00-112">The trigger history lists all trigger attempts that your logic app made.</span></span> <span data-ttu-id="bda00-113">Kattinthat, és részletesen, kifejezetten eseményindító megkísérel, a bemeneti adatok vagy kimenettel, amelyek a eseményindító kísérlet jön létre.</span><span class="sxs-lookup"><span data-stu-id="bda00-113">You can click each trigger attempt to drill into the details, specifically, any inputs or outputs that the trigger attempt generated.</span></span> <span data-ttu-id="bda00-114">Ha sikertelen eseményindítók, jelölje ki a eseményindító kísérletet, és válassza a **kimenetek** átnézni mutató hivatkozás létrehozott hibaüzeneteket, például a FTP hitelesítő adatokat, amelyek nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="bda00-114">If you find failed triggers, select the trigger attempt and choose the **Outputs** link to review any generated error messages, for example, for FTP credentials that aren't valid.</span></span>

<span data-ttu-id="bda00-115">Láthatja a különböző állapotok az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="bda00-115">The different statuses you might see are:</span></span>

* <span data-ttu-id="bda00-116">**Kihagyott**.</span><span class="sxs-lookup"><span data-stu-id="bda00-116">**Skipped**.</span></span> <span data-ttu-id="bda00-117">A végpont lett lekérdezett adatok kereséséhez, és hogy adatot nem volt elérhető választ kapott.</span><span class="sxs-lookup"><span data-stu-id="bda00-117">The endpoint was polled to check for data and received a response that no data was available.</span></span>
* <span data-ttu-id="bda00-118">**Sikeres**.</span><span class="sxs-lookup"><span data-stu-id="bda00-118">**Succeeded**.</span></span> <span data-ttu-id="bda00-119">Az eseményindító, hogy az adatok volt elérhető választ kapott.</span><span class="sxs-lookup"><span data-stu-id="bda00-119">The trigger received a response that data was available.</span></span> <span data-ttu-id="bda00-120">Ez az állapot manuális eseményindítót, ismétlődési eseményindító vagy egy lekérdezési eseményindító okozhatja.</span><span class="sxs-lookup"><span data-stu-id="bda00-120">This status might result from a manual trigger, a recurrence trigger, or a polling trigger.</span></span> <span data-ttu-id="bda00-121">Ez az állapot általában csatolni a **Fired** állapotát, de előfordulhat, ha rendelkezik olyan feltétel vagy SplitOn parancsot a kód nézetre, amely nem volt megfelelő.</span><span class="sxs-lookup"><span data-stu-id="bda00-121">This status is usually accompanied by the **Fired** status, but might not be if you have a condition or SplitOn command in code view that wasn't satisfied.</span></span>
* <span data-ttu-id="bda00-122">**Nem sikerült**.</span><span class="sxs-lookup"><span data-stu-id="bda00-122">**Failed**.</span></span> <span data-ttu-id="bda00-123">Egy hiba történt.</span><span class="sxs-lookup"><span data-stu-id="bda00-123">An error was generated.</span></span>

#### <a name="start-a-trigger-manually"></a><span data-ttu-id="bda00-124">Indítsa el manuálisan a eseményindító</span><span class="sxs-lookup"><span data-stu-id="bda00-124">Start a trigger manually</span></span>

<span data-ttu-id="bda00-125">A logikai alkalmazást egy rendelkezésre álló eseményindító azonnal vár a következő ismétlődés nélkül kereséséhez, kattintson a **válasszon eseményindító** a fő panelen a ellenőrzés kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="bda00-125">If you want the logic app to check for an available trigger immediately without waiting for the next recurrence, click **Select Trigger** on the main blade to force a check.</span></span> <span data-ttu-id="bda00-126">Például az Dropbox eseményindító a hivatkozásra kattintva okoz a munkafolyamat azonnali lekérdezésére Dropbox új fájlokat.</span><span class="sxs-lookup"><span data-stu-id="bda00-126">For example, clicking this link with a Dropbox trigger causes the workflow to immediately poll Dropbox for new files.</span></span>

### <a name="run-history"></a><span data-ttu-id="bda00-127">futtatási előzményei</span><span class="sxs-lookup"><span data-stu-id="bda00-127">Run history</span></span>

<span data-ttu-id="bda00-128">Minden égetett eseményindító futását eredményezi.</span><span class="sxs-lookup"><span data-stu-id="bda00-128">Every fired trigger results in a run.</span></span> <span data-ttu-id="bda00-129">Futtatási információkat tartalmaz, amelyek alapján megtudhatja, mi történt a munkafolyamat során számos részletet fő paneljén érheti el.</span><span class="sxs-lookup"><span data-stu-id="bda00-129">You can access run information from the main blade, which contains many details that can help you understand what happened during the workflow.</span></span>

![A futtatási előzményei keresése][2]

<span data-ttu-id="bda00-131">Futtatás a következő állapotok egyikét jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="bda00-131">A run displays one of the following statuses:</span></span>

* <span data-ttu-id="bda00-132">**Sikeres**.</span><span class="sxs-lookup"><span data-stu-id="bda00-132">**Succeeded**.</span></span> <span data-ttu-id="bda00-133">Minden művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="bda00-133">All actions succeeded.</span></span> <span data-ttu-id="bda00-134">Hiba történt, ha a hiba látta, amely később a munkafolyamatban történt művelet.</span><span class="sxs-lookup"><span data-stu-id="bda00-134">If a failure happened, that failure was handled by an action that occurred later in the workflow.</span></span> <span data-ttu-id="bda00-135">Ez azt jelenti, hogy a hiba látta el egy műveletet, amely után a sikertelen művelet lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="bda00-135">That is, the failure was handled by an action that was set to run after a failed action.</span></span>
* <span data-ttu-id="bda00-136">**Nem sikerült**.</span><span class="sxs-lookup"><span data-stu-id="bda00-136">**Failed**.</span></span> <span data-ttu-id="bda00-137">Legalább egy műveletet, amely később a munkafolyamatban művelet nem kezelt hiba volt.</span><span class="sxs-lookup"><span data-stu-id="bda00-137">At least one action had a failure that was not handled by an action later in the workflow.</span></span>
* <span data-ttu-id="bda00-138">**Megszakítva**.</span><span class="sxs-lookup"><span data-stu-id="bda00-138">**Cancelled**.</span></span> <span data-ttu-id="bda00-139">A munkafolyamat működő állapotban volt, de a megszakítási kérelmet kapott.</span><span class="sxs-lookup"><span data-stu-id="bda00-139">The workflow was running but received a cancel request.</span></span>
* <span data-ttu-id="bda00-140">**Futó**.</span><span class="sxs-lookup"><span data-stu-id="bda00-140">**Running**.</span></span> <span data-ttu-id="bda00-141">A munkafolyamat jelenleg fut.</span><span class="sxs-lookup"><span data-stu-id="bda00-141">The workflow is currently running.</span></span> <span data-ttu-id="bda00-142">Ez az állapot akkor fordulhat elő, a szabályozottan halmozott munkafolyamatok, vagy a jelenlegi tarifacsomag miatt.</span><span class="sxs-lookup"><span data-stu-id="bda00-142">This status might occur for throttled workflows, or because of the current pricing plan.</span></span> <span data-ttu-id="bda00-143">További információkért lásd: [művelet vonatkozó korlátozások a tarifákat tartalmazó oldalt](https://azure.microsoft.com/pricing/details/app-service/plans/).</span><span class="sxs-lookup"><span data-stu-id="bda00-143">For details, see [action limits on the pricing page](https://azure.microsoft.com/pricing/details/app-service/plans/).</span></span> <span data-ttu-id="bda00-144">(A diagramok futtatási előzményeit alatt megjelenő) diagnosztika konfigurálása is is információt nyújt a késleltetési eseményeket.</span><span class="sxs-lookup"><span data-stu-id="bda00-144">Configuring diagnostics (the charts that appear under the run history) also can provide information about any throttle events that happen.</span></span>

<span data-ttu-id="bda00-145">Ha éppen megtekintett futtatási előzményei, megtekintheti további részletekért.</span><span class="sxs-lookup"><span data-stu-id="bda00-145">When you are looking at a run history, you can drill in for more details.</span></span>  

#### <a name="trigger-outputs"></a><span data-ttu-id="bda00-146">Eseményindító kimenete</span><span class="sxs-lookup"><span data-stu-id="bda00-146">Trigger outputs</span></span>

<span data-ttu-id="bda00-147">Eseményindító kimenetének megjelenítése az adatokat, amely innen származik: az eseményindító.</span><span class="sxs-lookup"><span data-stu-id="bda00-147">Trigger outputs show the data that came from the trigger.</span></span> <span data-ttu-id="bda00-148">A kimenetek segítségével meghatározhatja, hogy az összes tulajdonság adott vissza a várt módon.</span><span class="sxs-lookup"><span data-stu-id="bda00-148">These outputs can help you determine whether all properties returned as expected.</span></span>

> [!NOTE]
> <span data-ttu-id="bda00-149">Ha megjelenik a telepített tartalmak nem világos, ismerje meg az Azure Logic Apps [kezeli a különböző típusú tartalmakat](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="bda00-149">If you see any content that you don't understand, learn how Azure Logic Apps [handles different content types](../logic-apps/logic-apps-content-type.md).</span></span>
> 

![Eseményindító kimeneti példák][3]

#### <a name="action-inputs-and-outputs"></a><span data-ttu-id="bda00-151">A művelet bemenetekhez és kimenetekhez</span><span class="sxs-lookup"><span data-stu-id="bda00-151">Action inputs and outputs</span></span>

<span data-ttu-id="bda00-152">A be- és kimenetekkel, művelet fogadó részletezhető.</span><span class="sxs-lookup"><span data-stu-id="bda00-152">You can drill into the inputs and outputs that an action received.</span></span> <span data-ttu-id="bda00-153">Ezek az adatok akkor hasznos, méretének és a kimenetek alakját megértéséhez, valamint a létrehozott előfordulhat, hogy hibaüzeneteket kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="bda00-153">This data is useful for understanding the size and shape of the outputs, and also for finding any error messages that might have been generated.</span></span>

![A művelet bemenetekhez és kimenetekhez][4]

## <a name="debug-workflow-runtime"></a><span data-ttu-id="bda00-155">A munkafolyamat futásidejű hibakeresése</span><span class="sxs-lookup"><span data-stu-id="bda00-155">Debug workflow runtime</span></span>

<span data-ttu-id="bda00-156">A bemenetek, kimenetek és eseményindítók futtató a figyelés, valamint néhány lépést sikerült hozzá a munkafolyamathoz, amelyek segítenek a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="bda00-156">Along with monitoring the inputs, outputs, and triggers of a run, you could add some steps to a workflow that help with debugging.</span></span> 
<span data-ttu-id="bda00-157">[RequestBin](http://requestb.in) hatékony eszköz, amely a munkafolyamat lépésben adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="bda00-157">[RequestBin](http://requestb.in) is a powerful tool that you can add as a step in a workflow.</span></span> <span data-ttu-id="bda00-158">RequestBin használatával állíthat be egy HTTP-kérelem inspector a pontos méretét, alakzat és HTTP-kérések formátum meghatározása.</span><span class="sxs-lookup"><span data-stu-id="bda00-158">By using RequestBin, you can set up an HTTP request inspector to determine the exact size, shape, and format of an HTTP request.</span></span> <span data-ttu-id="bda00-159">Hozzon létre egy RequestBin, és illessze be az URL-cím a logikai alkalmazás HTTP POST műveletet. a szervezet szeretné tesztelni, például tartalom, kifejezés vagy egy másik lépés kimeneti.</span><span class="sxs-lookup"><span data-stu-id="bda00-159">You can create a RequestBin and paste the URL in a logic app HTTP POST action with body content that you want to test, for example, an expression or another step output.</span></span> <span data-ttu-id="bda00-160">Futtatása után a logikai alkalmazást hogy hogyan jött létre a kérelmet a Logic Apps motor előállítja a RequestBin is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="bda00-160">After you run the logic app, you can refresh your RequestBin to see how the request was formed when generated from the Logic Apps engine.</span></span>

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
