---
title: "aaaDiagnose hibák - Azure Logic Apps |} Microsoft Docs"
description: "Ha nem működnek a logic apps közös módon toounderstand"
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
ms.openlocfilehash: 46d318625820034c95e6df3a71ab84c58f076dd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-logic-app-failures"></a><span data-ttu-id="dceca-103">Logic app hibák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="dceca-103">Diagnose logic app failures</span></span>
<span data-ttu-id="dceca-104">A a logic apps a problémákat vagy hibákat tapasztal, van néhány módszer segítségével jobb megértése érdekében ahol hello hibák érkező.</span><span class="sxs-lookup"><span data-stu-id="dceca-104">If you experience issues or failures with your logic apps, there are a few approaches can help you better understand where hello failures are coming from.</span></span>  

## <a name="azure-portal-tools"></a><span data-ttu-id="dceca-105">Az Azure portál eszközök</span><span class="sxs-lookup"><span data-stu-id="dceca-105">Azure portal tools</span></span>
<span data-ttu-id="dceca-106">hello Azure-portált biztosít sok eszközök toodiagnose minden logikai alkalmazás lépésről lépésre.</span><span class="sxs-lookup"><span data-stu-id="dceca-106">hello Azure portal provides many tools toodiagnose each logic app at each step.</span></span>

### <a name="trigger-history"></a><span data-ttu-id="dceca-107">Eseményindító előzmények</span><span class="sxs-lookup"><span data-stu-id="dceca-107">Trigger history</span></span>

<span data-ttu-id="dceca-108">Minden logikai alkalmazás legalább egy eseményindító tartozik.</span><span class="sxs-lookup"><span data-stu-id="dceca-108">Each logic app has at least one trigger.</span></span> <span data-ttu-id="dceca-109">Ha azt észleli, hogy alkalmazásokat nem kiváltó, tanulmányozni, hello eseményindító előzmények további információt.</span><span class="sxs-lookup"><span data-stu-id="dceca-109">If you notice that apps aren't firing, look first at hello trigger history for more information.</span></span> <span data-ttu-id="dceca-110">Hello eseményindító előzmények hello logika app'ss fő paneljén érheti el.</span><span class="sxs-lookup"><span data-stu-id="dceca-110">You can access hello trigger history on hello logic app'ss main blade.</span></span>

![Hello eseményindító előzmények keresése][1]

<span data-ttu-id="dceca-112">hello eseményindító előzmények összes eseményindító kísérletet a Logic Apps alkalmazást sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="dceca-112">hello trigger history lists all trigger attempts that your logic app made.</span></span> <span data-ttu-id="dceca-113">Kattintson minden eseményindító kísérlet toodrill hello részleteinek, különösen, bármilyen bemenetek vagy kimenettel, amelyek hello eseményindító kísérlet jön létre.</span><span class="sxs-lookup"><span data-stu-id="dceca-113">You can click each trigger attempt toodrill into hello details, specifically, any inputs or outputs that hello trigger attempt generated.</span></span> <span data-ttu-id="dceca-114">Ha nem sikerült az eseményindítók, jelölje ki a hello eseményindító kísérletet, és válassza a hello **kimenetek** tooreview bármely létrehozott hibaüzeneteket, például a FTP hitelesítő adatokat, amelyek nem érvényes hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="dceca-114">If you find failed triggers, select hello trigger attempt and choose hello **Outputs** link tooreview any generated error messages, for example, for FTP credentials that aren't valid.</span></span>

<span data-ttu-id="dceca-115">megjelenhet hello különböző állapotok az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="dceca-115">hello different statuses you might see are:</span></span>

* <span data-ttu-id="dceca-116">**Kihagyott**.</span><span class="sxs-lookup"><span data-stu-id="dceca-116">**Skipped**.</span></span> <span data-ttu-id="dceca-117">hello végpont lekérdezéses toocheck adatok, és hogy adatot nem volt elérhető választ kapott.</span><span class="sxs-lookup"><span data-stu-id="dceca-117">hello endpoint was polled toocheck for data and received a response that no data was available.</span></span>
* <span data-ttu-id="dceca-118">**Sikeres**.</span><span class="sxs-lookup"><span data-stu-id="dceca-118">**Succeeded**.</span></span> <span data-ttu-id="dceca-119">hello eseményindító, hogy az adatok volt elérhető választ kapott.</span><span class="sxs-lookup"><span data-stu-id="dceca-119">hello trigger received a response that data was available.</span></span> <span data-ttu-id="dceca-120">Ez az állapot manuális eseményindítót, ismétlődési eseményindító vagy egy lekérdezési eseményindító okozhatja.</span><span class="sxs-lookup"><span data-stu-id="dceca-120">This status might result from a manual trigger, a recurrence trigger, or a polling trigger.</span></span> <span data-ttu-id="dceca-121">Ez az állapot általában fűzni hello **Fired** állapotát, de előfordulhat, ha rendelkezik olyan feltétel vagy SplitOn parancsot a kód nézetre, amely nem volt megfelelő.</span><span class="sxs-lookup"><span data-stu-id="dceca-121">This status is usually accompanied by hello **Fired** status, but might not be if you have a condition or SplitOn command in code view that wasn't satisfied.</span></span>
* <span data-ttu-id="dceca-122">**Nem sikerült**.</span><span class="sxs-lookup"><span data-stu-id="dceca-122">**Failed**.</span></span> <span data-ttu-id="dceca-123">Egy hiba történt.</span><span class="sxs-lookup"><span data-stu-id="dceca-123">An error was generated.</span></span>

#### <a name="start-a-trigger-manually"></a><span data-ttu-id="dceca-124">Indítsa el manuálisan a eseményindító</span><span class="sxs-lookup"><span data-stu-id="dceca-124">Start a trigger manually</span></span>

<span data-ttu-id="dceca-125">Hello logic app toocheck egy rendelkezésre álló eseményindító azonnal Várakozás a következő hello ismétlődés nélkül, kattintson a **válasszon eseményindító** a hello fő panelje tooforce ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="dceca-125">If you want hello logic app toocheck for an available trigger immediately without waiting for hello next recurrence, click **Select Trigger** on hello main blade tooforce a check.</span></span> <span data-ttu-id="dceca-126">Ez a hivatkozás az Dropbox eseményindító például okoz hello munkafolyamat tooimmediately lekérdezési Dropbox új fájlokat.</span><span class="sxs-lookup"><span data-stu-id="dceca-126">For example, clicking this link with a Dropbox trigger causes hello workflow tooimmediately poll Dropbox for new files.</span></span>

### <a name="run-history"></a><span data-ttu-id="dceca-127">futtatási előzményei</span><span class="sxs-lookup"><span data-stu-id="dceca-127">Run history</span></span>

<span data-ttu-id="dceca-128">Minden égetett eseményindító futását eredményezi.</span><span class="sxs-lookup"><span data-stu-id="dceca-128">Every fired trigger results in a run.</span></span> <span data-ttu-id="dceca-129">Futtatási adatainak hello fő paneljén, alapján megtudhatja, mi történt hello munkafolyamat során számos részletet tartalmazó érheti el.</span><span class="sxs-lookup"><span data-stu-id="dceca-129">You can access run information from hello main blade, which contains many details that can help you understand what happened during hello workflow.</span></span>

![Futtatási előzményei hello keresése][2]

<span data-ttu-id="dceca-131">Futtatás a következő állapotok hello egyikét jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="dceca-131">A run displays one of hello following statuses:</span></span>

* <span data-ttu-id="dceca-132">**Sikeres**.</span><span class="sxs-lookup"><span data-stu-id="dceca-132">**Succeeded**.</span></span> <span data-ttu-id="dceca-133">Minden művelet sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="dceca-133">All actions succeeded.</span></span> <span data-ttu-id="dceca-134">Hiba történt, ha a hiba látta később hello munkafolyamat történt művelet.</span><span class="sxs-lookup"><span data-stu-id="dceca-134">If a failure happened, that failure was handled by an action that occurred later in hello workflow.</span></span> <span data-ttu-id="dceca-135">Ez azt jelenti, hogy hello hiba látta el egy műveletet, amely be lett állítva toorun sikertelen művelet után.</span><span class="sxs-lookup"><span data-stu-id="dceca-135">That is, hello failure was handled by an action that was set toorun after a failed action.</span></span>
* <span data-ttu-id="dceca-136">**Nem sikerült**.</span><span class="sxs-lookup"><span data-stu-id="dceca-136">**Failed**.</span></span> <span data-ttu-id="dceca-137">Legalább egy műveletet, amely később a hello munkafolyamat művelet nem kezelt hiba volt.</span><span class="sxs-lookup"><span data-stu-id="dceca-137">At least one action had a failure that was not handled by an action later in hello workflow.</span></span>
* <span data-ttu-id="dceca-138">**Megszakítva**.</span><span class="sxs-lookup"><span data-stu-id="dceca-138">**Cancelled**.</span></span> <span data-ttu-id="dceca-139">hello munkafolyamat működő állapotban volt, de a megszakítási kérelmet kapott.</span><span class="sxs-lookup"><span data-stu-id="dceca-139">hello workflow was running but received a cancel request.</span></span>
* <span data-ttu-id="dceca-140">**Futó**.</span><span class="sxs-lookup"><span data-stu-id="dceca-140">**Running**.</span></span> <span data-ttu-id="dceca-141">hello munkafolyamat jelenleg fut.</span><span class="sxs-lookup"><span data-stu-id="dceca-141">hello workflow is currently running.</span></span> <span data-ttu-id="dceca-142">Ez az állapot akkor fordulhat elő, a szabályozottan halmozott munkafolyamatok vagy aktuális árképzési terv hello miatt.</span><span class="sxs-lookup"><span data-stu-id="dceca-142">This status might occur for throttled workflows, or because of hello current pricing plan.</span></span> <span data-ttu-id="dceca-143">További információkért lásd: [művelet vonatkozó korlátozások árképzést ismertető oldalra hello](https://azure.microsoft.com/pricing/details/app-service/plans/).</span><span class="sxs-lookup"><span data-stu-id="dceca-143">For details, see [action limits on hello pricing page](https://azure.microsoft.com/pricing/details/app-service/plans/).</span></span> <span data-ttu-id="dceca-144">(A futtatási előzményei hello alatt megjelenő hello diagramok) diagnosztika konfigurálása is is információt nyújt a késleltetési eseményeket.</span><span class="sxs-lookup"><span data-stu-id="dceca-144">Configuring diagnostics (hello charts that appear under hello run history) also can provide information about any throttle events that happen.</span></span>

<span data-ttu-id="dceca-145">Ha éppen megtekintett futtatási előzményei, megtekintheti további részletekért.</span><span class="sxs-lookup"><span data-stu-id="dceca-145">When you are looking at a run history, you can drill in for more details.</span></span>  

#### <a name="trigger-outputs"></a><span data-ttu-id="dceca-146">Eseményindító kimenete</span><span class="sxs-lookup"><span data-stu-id="dceca-146">Trigger outputs</span></span>

<span data-ttu-id="dceca-147">Eseményindító kimenete, amely innen származik: hello eseményindító hello adatok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="dceca-147">Trigger outputs show hello data that came from hello trigger.</span></span> <span data-ttu-id="dceca-148">A kimenetek segítségével meghatározhatja, hogy az összes tulajdonság adott vissza a várt módon.</span><span class="sxs-lookup"><span data-stu-id="dceca-148">These outputs can help you determine whether all properties returned as expected.</span></span>

> [!NOTE]
> <span data-ttu-id="dceca-149">Ha megjelenik a telepített tartalmak nem világos, ismerje meg az Azure Logic Apps [kezeli a különböző típusú tartalmakat](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="dceca-149">If you see any content that you don't understand, learn how Azure Logic Apps [handles different content types](../logic-apps/logic-apps-content-type.md).</span></span>
> 

![Eseményindító kimeneti példák][3]

#### <a name="action-inputs-and-outputs"></a><span data-ttu-id="dceca-151">A művelet bemenetekhez és kimenetekhez</span><span class="sxs-lookup"><span data-stu-id="dceca-151">Action inputs and outputs</span></span>

<span data-ttu-id="dceca-152">Hello bemenetekhez és kimenetekhez, művelet fogadó részletezhető.</span><span class="sxs-lookup"><span data-stu-id="dceca-152">You can drill into hello inputs and outputs that an action received.</span></span> <span data-ttu-id="dceca-153">Ezek az adatok akkor hasznos, hello méretű és alakú hello kimenetek annak megértéséhez, valamint a hibaüzeneteket, előfordulhat, hogy létrejöttek-kereséshez.</span><span class="sxs-lookup"><span data-stu-id="dceca-153">This data is useful for understanding hello size and shape of hello outputs, and also for finding any error messages that might have been generated.</span></span>

![A művelet bemenetekhez és kimenetekhez][4]

## <a name="debug-workflow-runtime"></a><span data-ttu-id="dceca-155">A munkafolyamat futásidejű hibakeresése</span><span class="sxs-lookup"><span data-stu-id="dceca-155">Debug workflow runtime</span></span>

<span data-ttu-id="dceca-156">Hello bemenetek, kimenetek és eseményindítók futtató a figyelés, valamint hozzáadhatja az egyes lépéseket tooa munkafolyamat, amelyek segítenek a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="dceca-156">Along with monitoring hello inputs, outputs, and triggers of a run, you could add some steps tooa workflow that help with debugging.</span></span> 
<span data-ttu-id="dceca-157">[RequestBin](http://requestb.in) hatékony eszköz, amely a munkafolyamat lépésben adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="dceca-157">[RequestBin](http://requestb.in) is a powerful tool that you can add as a step in a workflow.</span></span> <span data-ttu-id="dceca-158">RequestBin használatával állíthat be egy HTTP-kérelmek inspector toodetermine hello pontos méretére, alakzat és HTTP-kérelem formátuma.</span><span class="sxs-lookup"><span data-stu-id="dceca-158">By using RequestBin, you can set up an HTTP request inspector toodetermine hello exact size, shape, and format of an HTTP request.</span></span> <span data-ttu-id="dceca-159">Hozzon létre egy RequestBin, és illessze be a hello URL-cím a logikai alkalmazás HTTP POST műveletet. a szervezet tootest, például kívánt tartalmat, kifejezés vagy egy másik lépés kimeneti.</span><span class="sxs-lookup"><span data-stu-id="dceca-159">You can create a RequestBin and paste hello URL in a logic app HTTP POST action with body content that you want tootest, for example, an expression or another step output.</span></span> <span data-ttu-id="dceca-160">Hello logikai alkalmazás futtatása után is frissítheti a RequestBin toosee hogyan hello kérés formátuma hello Logic Apps motortól generálásakor.</span><span class="sxs-lookup"><span data-stu-id="dceca-160">After you run hello logic app, you can refresh your RequestBin toosee how hello request was formed when generated from hello Logic Apps engine.</span></span>

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
