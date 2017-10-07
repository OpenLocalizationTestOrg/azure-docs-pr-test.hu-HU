---
title: "aaaAutomate Azure Naplóelemzés dolgozza fel a Microsoft Flow"
description: "Ismerje meg, hogyan használhatja a Microsoft Flow tooquickly ismételhető folyamatok automatizálása hello Azure Log Analytics-összekötő használatával."
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: bwren
ms.openlocfilehash: 52026df968682842cc9f8d55f6f9875c5f9c10b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-log-analytics-processes-with-hello-connector-for-microsoft-flow"></a><span data-ttu-id="f6879-103">Hello Connector Log Analytics-folyamatok automatizálása a Microsoft Flow</span><span class="sxs-lookup"><span data-stu-id="f6879-103">Automate Log Analytics processes with hello connector for Microsoft Flow</span></span>
<span data-ttu-id="f6879-104">[Microsoft Flow](https://ms.flow.microsoft.com) lehetővé teszi több száz műveletek segítségével számos különböző szolgáltatások toocreate automatizált munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="f6879-104">[Microsoft Flow](https://ms.flow.microsoft.com) allows you toocreate automated workflows using hundreds of actions for a variety of services.</span></span> <span data-ttu-id="f6879-105">Egy művelet kimenete változtatás nélkül használhatók bemeneti tooanother, hogy lehetővé teszi a különböző szolgáltatások toocreate integrációjával.</span><span class="sxs-lookup"><span data-stu-id="f6879-105">Output from one action can be used as input tooanother allowing you toocreate integration between different services.</span></span>  <span data-ttu-id="f6879-106">Microsoft Flow hello Azure Log Analytics-összekötő lehetővé teszik a napló megkeresi a Naplóelemzési által beolvasott adatokat tartalmazó toobuild munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="f6879-106">hello Azure Log Analytics connector for Microsoft Flow allow you toobuild workflows that include data retrieved by log searches in Log Analytics.</span></span>

<span data-ttu-id="f6879-107">Például az Office 365 e-mailben értesítést a Microsoft Flow toouse Naplóelemzési adatok felhasználásával, hozzon létre egy hibajelentést a Visual Studio Team Services, vagy közzététele a Slack üzenetet.</span><span class="sxs-lookup"><span data-stu-id="f6879-107">For example, you can use Microsoft Flow toouse Log Analytics data in an email notification from Office 365, create a bug in Visual Studio Team Services, or post a Slack message.</span></span>  <span data-ttu-id="f6879-108">Olyan egyszerű ütemezés, vagy az egyes művelet egy csatlakoztatott szolgáltatással, például amikor egy e-mail vagy egy tweetet érkezik a munkafolyamat indíthat el.</span><span class="sxs-lookup"><span data-stu-id="f6879-108">You can trigger a workflow by a simple schedule or from some action in a connected service such as when a mail or a tweet is received.</span></span>  


> [!NOTE]
> <span data-ttu-id="f6879-109">Microsoft Flow hello Azure Log Analytics-összekötő szükséges a munkaterület frissítése toobe toohello új Log Analytics lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="f6879-109">hello Azure Log Analytics connector for Microsoft Flow requires your workspace toobe upgraded toohello new Log Analytics query language.</span></span> <span data-ttu-id="f6879-110">További tudnivalók hello új nyelv, és hello eljárás tooupgrade lekérése a munkaterületen a [az Azure Naplóelemzés munkaterület toonew napló keresés frissítése](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="f6879-110">You can learn more about hello new language and get hello procedure tooupgrade your workspace at [Upgrade your Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>  

<span data-ttu-id="f6879-111">Ebben a cikkben hello oktatóanyag bemutatja, hogyan toocreate egy folyamatot, amely automatikusan elküldi hello e-mailben, hogyan használhatja a Microsoft Flow Naplóelemzési csak egy példa a Log Analyticshez napló keresési eredményeket.</span><span class="sxs-lookup"><span data-stu-id="f6879-111">hello tutorial in this article shows you how toocreate a flow that automatically sends hello results of a Log Analytics log search by email, just one example of how you can use Log Analytics in Microsoft Flow.</span></span> 


## <a name="step-1-create-a-flow"></a><span data-ttu-id="f6879-112">1. lépés: A folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6879-112">Step 1: Create a flow</span></span>
1. <span data-ttu-id="f6879-113">Jelentkezzen be a túl[Microsoft Flow](http://flow.microsoft.com), és válassza ki **saját Forgalomáramlás**.</span><span class="sxs-lookup"><span data-stu-id="f6879-113">Sign in too[Microsoft Flow](http://flow.microsoft.com), and select **My Flows**.</span></span>
2. <span data-ttu-id="f6879-114">Kattintson a **+ hozza létre az üres**.</span><span class="sxs-lookup"><span data-stu-id="f6879-114">Click **+ Create from blank**.</span></span>

## <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="f6879-115">2. lépés: A folyamat eseményindító létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6879-115">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="f6879-116">Kattintson a **keresés több száz összekötők és eseményindítók**.</span><span class="sxs-lookup"><span data-stu-id="f6879-116">Click **Search hundreds of connectors and triggers**.</span></span>
2. <span data-ttu-id="f6879-117">Típus **ütemezés** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f6879-117">Type **Schedule** in hello search box.</span></span>
3. <span data-ttu-id="f6879-118">Válassza ki **ütemezés**, majd válassza ki **ütemezés - ismétlődési**.</span><span class="sxs-lookup"><span data-stu-id="f6879-118">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
4. <span data-ttu-id="f6879-119">A hello **gyakoriság** mezőben jelölje be **nap** és hello **időköz** adja meg a **1**.</span><span class="sxs-lookup"><span data-stu-id="f6879-119">In hello **Frequency** box select **Day** and in hello **Interval** box, enter **1**.</span></span><br><br><span data-ttu-id="f6879-120">![Microsoft Flow eseményindító párbeszédpanel](media/log-analytics-flow-tutorial/flow01.png)</span><span class="sxs-lookup"><span data-stu-id="f6879-120">![Microsoft Flow trigger dialog box](media/log-analytics-flow-tutorial/flow01.png)</span></span>


## <a name="step-3-add-a-log-analytics-action"></a><span data-ttu-id="f6879-121">3. lépés: A Naplóelemzési művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f6879-121">Step 3: Add a Log Analytics action</span></span>
1. <span data-ttu-id="f6879-122">Kattintson a **+ új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f6879-122">Click **+ New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="f6879-123">Keresse meg **Analytics jelentkezzen**.</span><span class="sxs-lookup"><span data-stu-id="f6879-123">Search for **Log Analytics**.</span></span>
3. <span data-ttu-id="f6879-124">Kattintson a **Azure Log Analytics-lekérdezés futtatása, és eredményeinek képi megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="f6879-124">Click **Azure Log Analytics – Run query and visualize results**.</span></span><br><br><span data-ttu-id="f6879-125">![A Naplóelemzési lekérdezési ablakban futtassa](media/log-analytics-flow-tutorial/flow02.png)</span><span class="sxs-lookup"><span data-stu-id="f6879-125">![Log Analytics run query window](media/log-analytics-flow-tutorial/flow02.png)</span></span>

## <a name="step-4-configure-hello-log-analytics-action"></a><span data-ttu-id="f6879-126">4. lépés: Hello Naplóelemzési művelet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6879-126">Step 4: Configure hello Log Analytics action</span></span>

1. <span data-ttu-id="f6879-127">Adja meg a munkaterület, beleértve a hello előfizetési azonosító, erőforráscsoport, és a munkaterület neve hello adatait.</span><span class="sxs-lookup"><span data-stu-id="f6879-127">Specify hello details for your workspace including hello Subscription ID, Resource Group, and Workspace Name.</span></span>
2. <span data-ttu-id="f6879-128">Adja hozzá a következő Naplóelemzési lekérdezés toohello hello **lekérdezés** ablak.</span><span class="sxs-lookup"><span data-stu-id="f6879-128">Add hello following Log Analytics query toohello **Query** window.</span></span>  <span data-ttu-id="f6879-129">Ez csak a mintalekérdezést, és lecserélheti bármely más, hogy adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f6879-129">This is only a sample query, and you can replace with any other that returns data.</span></span>
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. <span data-ttu-id="f6879-130">Válassza ki **HTML tábla** a hello **diagramtípus**.</span><span class="sxs-lookup"><span data-stu-id="f6879-130">Select **HTML Table** for hello **Chart Type**.</span></span><br><br><span data-ttu-id="f6879-131">![Napló Analytics művelet](media/log-analytics-flow-tutorial/flow03.png)</span><span class="sxs-lookup"><span data-stu-id="f6879-131">![Log Analytics action](media/log-analytics-flow-tutorial/flow03.png)</span></span>

## <a name="step-5-configure-hello-flow-toosend-email"></a><span data-ttu-id="f6879-132">5. lépés: Hello folyamat toosend e-mail konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6879-132">Step 5: Configure hello flow toosend email</span></span>

1. <span data-ttu-id="f6879-133">Kattintson a **új lépés**, és kattintson a **+ új művelet**.</span><span class="sxs-lookup"><span data-stu-id="f6879-133">Click **New step**, and then click **+ Add an action**.</span></span>
2. <span data-ttu-id="f6879-134">Keresse meg **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="f6879-134">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="f6879-135">Kattintson a **Office 365 Outlook – az e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="f6879-135">Click **Office 365 Outlook – Send an email**.</span></span><br><br><span data-ttu-id="f6879-136">![Az Office 365 Outlook kiválasztási ablaka](media/log-analytics-flow-tutorial/flow04.png)</span><span class="sxs-lookup"><span data-stu-id="f6879-136">![Office 365 Outlook selection window](media/log-analytics-flow-tutorial/flow04.png)</span></span>

4. <span data-ttu-id="f6879-137">Adjon meg egy címzett e-mail címét hello hello **való** ablakot, és az üdvözlő e-mail tárgyát **tulajdonos**.</span><span class="sxs-lookup"><span data-stu-id="f6879-137">Specify hello email address of a recipient in hello **To** window and a subject for hello email in **Subject**.</span></span>
5. <span data-ttu-id="f6879-138">Kattintson bárhova hello **törzs** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f6879-138">Click anywhere in hello **Body** box.</span></span>  <span data-ttu-id="f6879-139">A **dinamikus tartalom** értékeket az előző műveletekből ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f6879-139">A **Dynamic content** window opens with values from previous actions.</span></span>  
6. <span data-ttu-id="f6879-140">Válassza ki **törzs**.</span><span class="sxs-lookup"><span data-stu-id="f6879-140">Select **Body**.</span></span>  <span data-ttu-id="f6879-141">Ez a hello Naplóelemzési művelet hello lekérdezést hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="f6879-141">This is hello results of hello query in hello Log Analytics action.</span></span>
6. <span data-ttu-id="f6879-142">Kattintson a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="f6879-142">Click **Show advanced options**.</span></span>
7. <span data-ttu-id="f6879-143">A hello **HTML** mezőben válassza **Igen**.</span><span class="sxs-lookup"><span data-stu-id="f6879-143">In hello **Is HTML** box, select **Yes**.</span></span><br><br><span data-ttu-id="f6879-144">![Az Office 365 e-mailek konfigurációs ablak](media/log-analytics-flow-tutorial/flow05.png)</span><span class="sxs-lookup"><span data-stu-id="f6879-144">![Office 365 email configuration window](media/log-analytics-flow-tutorial/flow05.png)</span></span>

## <a name="step-6-save-and-test-your-flow"></a><span data-ttu-id="f6879-145">6. lépés: Mentéséhez és a folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="f6879-145">Step 6: Save and test your flow</span></span>
1. <span data-ttu-id="f6879-146">A hello **folyamat nevének** mezőbe, vegye fel a folyamat nevét, majd kattintson **folyamat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f6879-146">In hello **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span><br><br><span data-ttu-id="f6879-147">![Mentés folyamata](media/log-analytics-flow-tutorial/flow06.png)</span><span class="sxs-lookup"><span data-stu-id="f6879-147">![Save flow](media/log-analytics-flow-tutorial/flow06.png)</span></span>
2. <span data-ttu-id="f6879-148">hello folyamata megtörtént, és amely megadott hello ütemezés naponta után fog futni.</span><span class="sxs-lookup"><span data-stu-id="f6879-148">hello flow is now created and will run after a day which is hello schedule you specified.</span></span> 
3. <span data-ttu-id="f6879-149">tooimmediately teszt hello folyamata, kattintson a **Futtatás most** , majd **folyamat futtatása**.</span><span class="sxs-lookup"><span data-stu-id="f6879-149">tooimmediately test hello flow, click **Run Now** and then **Run flow**.</span></span><br><br><span data-ttu-id="f6879-150">![Futtatási folyamata](media/log-analytics-flow-tutorial/flow07.png)</span><span class="sxs-lookup"><span data-stu-id="f6879-150">![Run flow](media/log-analytics-flow-tutorial/flow07.png)</span></span>
3. <span data-ttu-id="f6879-151">Hello folyamat befejezése után ellenőrizze a megadott hello címzett hello mail.</span><span class="sxs-lookup"><span data-stu-id="f6879-151">When hello flow completes, check hello mail of hello recipient that you specified.</span></span>  <span data-ttu-id="f6879-152">Kell egy e-mail törzse hasonló toohello következőre érkezett:</span><span class="sxs-lookup"><span data-stu-id="f6879-152">You should have received a mail with a body similar toohello following:</span></span><br><br>![Példa e-mailre](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a><span data-ttu-id="f6879-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6879-154">Next steps</span></span>

- <span data-ttu-id="f6879-155">További információ [Log Analytics-e jelentkezni a keresések](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="f6879-155">Learn more about [log searches in Log Analytics](log-analytics-log-search-new.md).</span></span>
- <span data-ttu-id="f6879-156">További információ [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f6879-156">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



