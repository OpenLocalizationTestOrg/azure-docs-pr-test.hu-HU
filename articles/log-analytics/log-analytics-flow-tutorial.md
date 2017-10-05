---
title: "A Microsoft Flow Azure Log Analytics-folyamatok automatizálása"
description: "Ismerje meg, hogyan használhatja a Microsoft Flow gyorsan automatizálása ismételhető az Azure Log Analytics-összekötő használatával."
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
ms.openlocfilehash: 4955f90de06cd720d78c5bb60c0adcd7dc633245
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a><span data-ttu-id="b84e1-103">Összekötő Log Analytics-folyamatok automatizálása a Microsoft Flow</span><span class="sxs-lookup"><span data-stu-id="b84e1-103">Automate Log Analytics processes with the connector for Microsoft Flow</span></span>
<span data-ttu-id="b84e1-104">[Microsoft Flow](https://ms.flow.microsoft.com) lehetővé teszi automatizált munkafolyamatok műveletek több száz segítségével különböző szolgáltatások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b84e1-104">[Microsoft Flow](https://ms.flow.microsoft.com) allows you to create automated workflows using hundreds of actions for a variety of services.</span></span> <span data-ttu-id="b84e1-105">Egy művelet kimenetében használható bemenetként a másikra, hogy lehetővé teszi különböző szolgáltatások közötti integrációt.</span><span class="sxs-lookup"><span data-stu-id="b84e1-105">Output from one action can be used as input to another allowing you to create integration between different services.</span></span>  <span data-ttu-id="b84e1-106">Az Azure Log Analytics-összekötő Microsoft Flow engedélyezi, hogy a napló megkeresi a Naplóelemzési által beolvasott adatokat tartalmazó munkafolyamatok építhetők fel.</span><span class="sxs-lookup"><span data-stu-id="b84e1-106">The Azure Log Analytics connector for Microsoft Flow allow you to build workflows that include data retrieved by log searches in Log Analytics.</span></span>

<span data-ttu-id="b84e1-107">Microsoft Flow segítségével például az Office 365 e-mailben értesítést Naplóelemzési adatok használja, hozzon létre egy hibajelentést a Visual Studio Team Services vagy közzététele a Slack üzenetet.</span><span class="sxs-lookup"><span data-stu-id="b84e1-107">For example, you can use Microsoft Flow to use Log Analytics data in an email notification from Office 365, create a bug in Visual Studio Team Services, or post a Slack message.</span></span>  <span data-ttu-id="b84e1-108">Olyan egyszerű ütemezés, vagy az egyes művelet egy csatlakoztatott szolgáltatással, például amikor egy e-mail vagy egy tweetet érkezik a munkafolyamat indíthat el.</span><span class="sxs-lookup"><span data-stu-id="b84e1-108">You can trigger a workflow by a simple schedule or from some action in a connected service such as when a mail or a tweet is received.</span></span>  


> [!NOTE]
> <span data-ttu-id="b84e1-109">Az Azure Log Analytics-összekötő Microsoft Flow szükséges a munkaterület frissíteni kell az új Naplóelemzési lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="b84e1-109">The Azure Log Analytics connector for Microsoft Flow requires your workspace to be upgraded to the new Log Analytics query language.</span></span> <span data-ttu-id="b84e1-110">További információ az új nyelv, és a frissítés a következő munkaterület eljárás első [Azure Naplóelemzési munkaterület frissítése új naplófájl-keresési](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="b84e1-110">You can learn more about the new language and get the procedure to upgrade your workspace at [Upgrade your Azure Log Analytics workspace to new log search](log-analytics-log-search-upgrade.md).</span></span>  

<span data-ttu-id="b84e1-111">Ebben a cikkben az oktatóanyag bemutatja, hogyan hozzon létre egy folyamatot, amely automatikusan elküldi e-mailben, hogyan használhatja a Microsoft Flow Naplóelemzési csak egy példa Naplóelemzési napló keresés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="b84e1-111">The tutorial in this article shows you how to create a flow that automatically sends the results of a Log Analytics log search by email, just one example of how you can use Log Analytics in Microsoft Flow.</span></span> 


## <a name="step-1-create-a-flow"></a><span data-ttu-id="b84e1-112">1. lépés: A folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b84e1-112">Step 1: Create a flow</span></span>
1. <span data-ttu-id="b84e1-113">Jelentkezzen be [Microsoft Flow](http://flow.microsoft.com), és válassza ki **saját Forgalomáramlás**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-113">Sign in to [Microsoft Flow](http://flow.microsoft.com), and select **My Flows**.</span></span>
2. <span data-ttu-id="b84e1-114">Kattintson a **+ hozza létre az üres**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-114">Click **+ Create from blank**.</span></span>

## <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="b84e1-115">2. lépés: A folyamat eseményindító létrehozása</span><span class="sxs-lookup"><span data-stu-id="b84e1-115">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="b84e1-116">Kattintson a **keresés több száz összekötők és eseményindítók**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-116">Click **Search hundreds of connectors and triggers**.</span></span>
2. <span data-ttu-id="b84e1-117">Típus **ütemezés** be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="b84e1-117">Type **Schedule** in the search box.</span></span>
3. <span data-ttu-id="b84e1-118">Válassza ki **ütemezés**, majd válassza ki **ütemezés - ismétlődési**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-118">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
4. <span data-ttu-id="b84e1-119">Az a **gyakoriság** mezőben jelölje be **nap** és a a **időköz** adja meg a **1**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-119">In the **Frequency** box select **Day** and in the **Interval** box, enter **1**.</span></span><br><br><span data-ttu-id="b84e1-120">![Microsoft Flow eseményindító párbeszédpanel](media/log-analytics-flow-tutorial/flow01.png)</span><span class="sxs-lookup"><span data-stu-id="b84e1-120">![Microsoft Flow trigger dialog box](media/log-analytics-flow-tutorial/flow01.png)</span></span>


## <a name="step-3-add-a-log-analytics-action"></a><span data-ttu-id="b84e1-121">3. lépés: A Naplóelemzési művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b84e1-121">Step 3: Add a Log Analytics action</span></span>
1. <span data-ttu-id="b84e1-122">Kattintson a **+ új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-122">Click **+ New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="b84e1-123">Keresse meg **Analytics jelentkezzen**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-123">Search for **Log Analytics**.</span></span>
3. <span data-ttu-id="b84e1-124">Kattintson a **Azure Log Analytics-lekérdezés futtatása, és eredményeinek képi megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-124">Click **Azure Log Analytics – Run query and visualize results**.</span></span><br><br><span data-ttu-id="b84e1-125">![A Naplóelemzési lekérdezési ablakban futtassa](media/log-analytics-flow-tutorial/flow02.png)</span><span class="sxs-lookup"><span data-stu-id="b84e1-125">![Log Analytics run query window](media/log-analytics-flow-tutorial/flow02.png)</span></span>

## <a name="step-4-configure-the-log-analytics-action"></a><span data-ttu-id="b84e1-126">4. lépés: Konfigurálja a Naplóelemzési művelet</span><span class="sxs-lookup"><span data-stu-id="b84e1-126">Step 4: Configure the Log Analytics action</span></span>

1. <span data-ttu-id="b84e1-127">Adja meg a munkaterület, beleértve az előfizetési azonosító, erőforráscsoport, és a munkaterület neve adatait.</span><span class="sxs-lookup"><span data-stu-id="b84e1-127">Specify the details for your workspace including the Subscription ID, Resource Group, and Workspace Name.</span></span>
2. <span data-ttu-id="b84e1-128">Adja hozzá a következő Log Analytics-lekérdezés futtatásával a **lekérdezés** ablak.</span><span class="sxs-lookup"><span data-stu-id="b84e1-128">Add the following Log Analytics query to the **Query** window.</span></span>  <span data-ttu-id="b84e1-129">Ez csak a mintalekérdezést, és lecserélheti bármely más, hogy adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b84e1-129">This is only a sample query, and you can replace with any other that returns data.</span></span>
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. <span data-ttu-id="b84e1-130">Válassza ki **HTML tábla** a a **diagramtípus**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-130">Select **HTML Table** for the **Chart Type**.</span></span><br><br><span data-ttu-id="b84e1-131">![Napló Analytics művelet](media/log-analytics-flow-tutorial/flow03.png)</span><span class="sxs-lookup"><span data-stu-id="b84e1-131">![Log Analytics action](media/log-analytics-flow-tutorial/flow03.png)</span></span>

## <a name="step-5-configure-the-flow-to-send-email"></a><span data-ttu-id="b84e1-132">5. lépés: az e-mail üzenetek küldéséhez a folyamat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b84e1-132">Step 5: Configure the flow to send email</span></span>

1. <span data-ttu-id="b84e1-133">Kattintson a **új lépés**, és kattintson a **+ új művelet**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-133">Click **New step**, and then click **+ Add an action**.</span></span>
2. <span data-ttu-id="b84e1-134">Keresse meg **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-134">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="b84e1-135">Kattintson a **Office 365 Outlook – az e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-135">Click **Office 365 Outlook – Send an email**.</span></span><br><br><span data-ttu-id="b84e1-136">![Az Office 365 Outlook kiválasztási ablaka](media/log-analytics-flow-tutorial/flow04.png)</span><span class="sxs-lookup"><span data-stu-id="b84e1-136">![Office 365 Outlook selection window](media/log-analytics-flow-tutorial/flow04.png)</span></span>

4. <span data-ttu-id="b84e1-137">A címzett e-mail címét adja meg a **való** ablakot, és az e-mail tárgyát **tulajdonos**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-137">Specify the email address of a recipient in the **To** window and a subject for the email in **Subject**.</span></span>
5. <span data-ttu-id="b84e1-138">Kattintson bárhova a **törzs** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b84e1-138">Click anywhere in the **Body** box.</span></span>  <span data-ttu-id="b84e1-139">A **dinamikus tartalom** értékeket az előző műveletekből ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b84e1-139">A **Dynamic content** window opens with values from previous actions.</span></span>  
6. <span data-ttu-id="b84e1-140">Válassza ki **törzs**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-140">Select **Body**.</span></span>  <span data-ttu-id="b84e1-141">Ez az a Naplóelemzési művelet: a lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="b84e1-141">This is the results of the query in the Log Analytics action.</span></span>
6. <span data-ttu-id="b84e1-142">Kattintson a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-142">Click **Show advanced options**.</span></span>
7. <span data-ttu-id="b84e1-143">Az a **HTML** mezőben válassza **Igen**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-143">In the **Is HTML** box, select **Yes**.</span></span><br><br><span data-ttu-id="b84e1-144">![Az Office 365 e-mailek konfigurációs ablak](media/log-analytics-flow-tutorial/flow05.png)</span><span class="sxs-lookup"><span data-stu-id="b84e1-144">![Office 365 email configuration window](media/log-analytics-flow-tutorial/flow05.png)</span></span>

## <a name="step-6-save-and-test-your-flow"></a><span data-ttu-id="b84e1-145">6. lépés: Mentéséhez és a folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="b84e1-145">Step 6: Save and test your flow</span></span>
1. <span data-ttu-id="b84e1-146">Az a **folyamat nevének** mezőbe, vegye fel a folyamat nevét, majd kattintson **folyamat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-146">In the **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span><br><br><span data-ttu-id="b84e1-147">![Mentés folyamata](media/log-analytics-flow-tutorial/flow06.png)</span><span class="sxs-lookup"><span data-stu-id="b84e1-147">![Save flow](media/log-analytics-flow-tutorial/flow06.png)</span></span>
2. <span data-ttu-id="b84e1-148">A folyamat megtörtént, és egy napon, amely a megadott ütemezés fog futni.</span><span class="sxs-lookup"><span data-stu-id="b84e1-148">The flow is now created and will run after a day which is the schedule you specified.</span></span> 
3. <span data-ttu-id="b84e1-149">A folyamat azonnal teszteléséhez kattintson **Futtatás most** , majd **folyamat futtatása**.</span><span class="sxs-lookup"><span data-stu-id="b84e1-149">To immediately test the flow, click **Run Now** and then **Run flow**.</span></span><br><br><span data-ttu-id="b84e1-150">![Futtatási folyamata](media/log-analytics-flow-tutorial/flow07.png)</span><span class="sxs-lookup"><span data-stu-id="b84e1-150">![Run flow](media/log-analytics-flow-tutorial/flow07.png)</span></span>
3. <span data-ttu-id="b84e1-151">A folyamat befejezése után ellenőrizze a megadott címzett e-mail.</span><span class="sxs-lookup"><span data-stu-id="b84e1-151">When the flow completes, check the mail of the recipient that you specified.</span></span>  <span data-ttu-id="b84e1-152">Kell egy a következőhöz hasonló szervezethez mail érkezett:</span><span class="sxs-lookup"><span data-stu-id="b84e1-152">You should have received a mail with a body similar to the following:</span></span><br><br>![Példa e-mailre](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a><span data-ttu-id="b84e1-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b84e1-154">Next steps</span></span>

- <span data-ttu-id="b84e1-155">További információ [Log Analytics-e jelentkezni a keresések](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="b84e1-155">Learn more about [log searches in Log Analytics](log-analytics-log-search-new.md).</span></span>
- <span data-ttu-id="b84e1-156">További információ [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b84e1-156">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



