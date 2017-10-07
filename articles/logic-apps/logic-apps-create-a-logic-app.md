---
title: "aaaCreate a felhőalapú alkalmazások és a felhőszolgáltatás - Azure Logic Apps közötti első munkafolyamat |} Microsoft Docs"
description: "Üzleti folyamatok automatizálása a rendszerintegrációs és vállalati alkalmazásintegrációs (EAI) forgatókönyvekhez Azure Logic Appsban létrehozott és futtatott munkafolyamatok segítségével"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: "munkafolyamat, felhőalapú alkalmazások, felhőszolgáltatások, üzleti folyamatok, rendszerintegráció, vállalati alkalmazásintegráció, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.openlocfilehash: 17ec589b1c8923b5ad3e6479fc856b6ac81754ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-logic-app-workflow-tooautomate-processes-between-cloud-apps-and-cloud-services"></a><span data-ttu-id="bdcda-104">Az első logikai alkalmazás létrehozása tooautomate munkafolyamatok közötti felhőalapú alkalmazások és a cloud services csomag</span><span class="sxs-lookup"><span data-stu-id="bdcda-104">Create your first logic app workflow tooautomate processes between cloud apps and cloud services</span></span>

<span data-ttu-id="bdcda-105">Könnyebben és gyorsabban automatizálhatja az üzleti folyamatokat kód megírása nélkül is, ha az [Azure Logic Apps](logic-apps-what-are-logic-apps.md) segítségével hoz létre és futtat munkafolyamatokat.</span><span class="sxs-lookup"><span data-stu-id="bdcda-105">Without writing any code, you can automate business processes more easily and quickly when you create and run workflows with [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span></span> <span data-ttu-id="bdcda-106">A első példa bemutatja, hogyan toocreate egy alapszintű logikája app munkafolyamat, amely ellenőrzi az RSS-hírcsatorna egy webhelyen új tartalom.</span><span class="sxs-lookup"><span data-stu-id="bdcda-106">This first example shows how toocreate a basic logic app workflow that checks an RSS feed for new content on a website.</span></span> <span data-ttu-id="bdcda-107">Új elemek hello webhely adatcsatorna jelenik meg, amikor a hello logikai alkalmazás e-mailt küld az Outlook vagy Gmailes fiókból.</span><span class="sxs-lookup"><span data-stu-id="bdcda-107">When new items appear in hello website's feed, hello logic app sends email from an Outlook or Gmail account.</span></span>

<span data-ttu-id="bdcda-108">toocreate, és futtassa a logic app, ezek az elemek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="bdcda-108">toocreate and run a logic app, you need these items:</span></span>

* <span data-ttu-id="bdcda-109">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="bdcda-109">An Azure subscription.</span></span> <span data-ttu-id="bdcda-110">Ha nem rendelkezik előfizetéssel, [kezdhet egy ingyenes Azure-fiókkal](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="bdcda-110">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="bdcda-111">Egyéb esetben [regisztrálhat használatalapú fizetéses előfizetésre](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="bdcda-111">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

  <span data-ttu-id="bdcda-112">Az Azure-előfizetés a logikaialkalmazás-használat számlázására szolgál.</span><span class="sxs-lookup"><span data-stu-id="bdcda-112">Your Azure subscription is used for billing logic app usage.</span></span> <span data-ttu-id="bdcda-113">Ismerje meg, hogyan működik a [használatmérés](../logic-apps/logic-apps-pricing.md) és a [díjszabás](https://azure.microsoft.com/pricing/details/logic-apps) az Azure Logic Apps esetében.</span><span class="sxs-lookup"><span data-stu-id="bdcda-113">Learn how [usage metering](../logic-apps/logic-apps-pricing.md) and [pricing](https://azure.microsoft.com/pricing/details/logic-apps) work for Azure Logic Apps.</span></span>

<span data-ttu-id="bdcda-114">A példához a következő elemek is szükségesek:</span><span class="sxs-lookup"><span data-stu-id="bdcda-114">Also, this example requires these items:</span></span>

* <span data-ttu-id="bdcda-115">Outlook.com-, Office 365 Outlook- vagy Gmail-fiók</span><span class="sxs-lookup"><span data-stu-id="bdcda-115">An Outlook.com, Office 365 Outlook, or Gmail account</span></span>

    > [!TIP]
    > <span data-ttu-id="bdcda-116">Ha rendelkezik személyes [Microsoft-fiókkal](https://account.microsoft.com/account), akkor Outlook.com-fiókja van.</span><span class="sxs-lookup"><span data-stu-id="bdcda-116">If you have a personal [Microsoft account](https://account.microsoft.com/account), you have an Outlook.com account.</span></span> <span data-ttu-id="bdcda-117">Egyéb esetben, ha rendelkezik Azure munkahelyi vagy iskolai fiókkal, **Office 365 Outlook**-fiókja van.</span><span class="sxs-lookup"><span data-stu-id="bdcda-117">Otherwise, if you have an Azure work or school account, you have an **Office 365 Outlook** account.</span></span>

* <span data-ttu-id="bdcda-118">A hivatkozás tooa webhely RSS-hírcsatornára.</span><span class="sxs-lookup"><span data-stu-id="bdcda-118">A link tooa website's RSS feed.</span></span> <span data-ttu-id="bdcda-119">Ez a példa hello [RSS-hírcsatorna a legfontosabb események hello CNN.com webhelyről](http://rss.cnn.com/rss/cnn_topstories.rss):`http://rss.cnn.com/rss/cnn_topstories.rss`</span><span class="sxs-lookup"><span data-stu-id="bdcda-119">This example uses hello [RSS feed for top stories from hello CNN.com website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span></span>

## <a name="add-a-trigger-that-starts-your-workflow"></a><span data-ttu-id="bdcda-120">Eseményindító hozzáadása a munkafolyamat indításához</span><span class="sxs-lookup"><span data-stu-id="bdcda-120">Add a trigger that starts your workflow</span></span>

<span data-ttu-id="bdcda-121">A [ *eseményindító* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) olyan esemény, amely elindítja a logic app munkafolyamatot és hello első elem, amelyet a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bdcda-121">A [*trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts your logic app workflow and is hello first item that your logic app needs.</span></span>

1. <span data-ttu-id="bdcda-122">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="bdcda-122">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="bdcda-123">Hello bal oldali menüből **új** > **vállalati integrációs** > **logikai alkalmazás** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="bdcda-123">From hello left menu, choose **New** > **Enterprise Integration** > **Logic App** as shown here:</span></span>

     ![Azure Portal, Új, Vállalati integráció, Logikai alkalmazás](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > <span data-ttu-id="bdcda-125">Dönthet úgy is **új**, hello a keresőmezőbe írja be a `logic app`, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="bdcda-125">You can also choose **New**, then in hello search box, type `logic app`, and press Enter.</span></span> <span data-ttu-id="bdcda-126">Válassza a **Logikai alkalmazás** > **Létrehozás** elemet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-126">Then choose **Logic App** > **Create**.</span></span>

3. <span data-ttu-id="bdcda-127">Nevezze el a logikai alkalmazást, majd válassza ki az Azure-előfizetését.</span><span class="sxs-lookup"><span data-stu-id="bdcda-127">Name your logic app and select your Azure subscription.</span></span> <span data-ttu-id="bdcda-128">Most hozzon létre vagy válasszon ki egy Azure-erőforráscsoportot, amely segít a kapcsolódó Azure-erőforrások rendszerezésében és kezelésében.</span><span class="sxs-lookup"><span data-stu-id="bdcda-128">Now create or select an Azure resource group, which helps you organize and manage related Azure resources.</span></span> <span data-ttu-id="bdcda-129">Végül válassza ki a Logic Apps alkalmazást üzemeltető hello datacenter helyét.</span><span class="sxs-lookup"><span data-stu-id="bdcda-129">Finally, select hello datacenter location for hosting your logic app.</span></span> <span data-ttu-id="bdcda-130">Ha elkészült, válassza ki a **PIN-kód toodashboard** , majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-130">When you're ready, choose **Pin toodashboard** and then **Create**.</span></span>

     ![Logikai alkalmazás részletei](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > <span data-ttu-id="bdcda-132">Ha bejelöli **PIN-kód toodashboard**, a Logic Apps alkalmazást a telepítés utáni hello Azure irányítópulton megjelenik, és automatikusan megnyílik.</span><span class="sxs-lookup"><span data-stu-id="bdcda-132">When you select **Pin toodashboard**, your logic app appears on hello Azure dashboard after deployment, and opens automatically.</span></span> <span data-ttu-id="bdcda-133">Ha nem szerepel a Logic Apps alkalmazást hello irányítópultot, hello **összes erőforrás** csempére, válassza a **lásd: több**, és válassza ki a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bdcda-133">If your logic app doesn't appear on hello dashboard, on hello **All resources** tile, choose **See More**, and select your logic app.</span></span> <span data-ttu-id="bdcda-134">Vagy a hello bal oldali menüben válassza a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-134">Or on hello left menu, choose **More services**.</span></span> <span data-ttu-id="bdcda-135">A **Vállalati integráció** résznél válassza a **Logikai alkalmazások** elemet, majd válassza ki a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bdcda-135">Under **Enterprise Integration**, choose **Logic Apps**, and select your logic app.</span></span>

4. <span data-ttu-id="bdcda-136">A Logic Apps alkalmazást az hello megnyitásakor első alkalommal a hello Logic App Designer látható sablonok használható tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="bdcda-136">When you open your logic app for hello first time, hello Logic App Designer shows templates that you can use tooget started.</span></span> <span data-ttu-id="bdcda-137">Jelen esetben válassza az **Üres logikai alkalmazás** elemet, így az alapoktól építheti fel saját logikai alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="bdcda-137">For now, choose **Blank Logic App** so you can build your logic app from scratch.</span></span>

    <span data-ttu-id="bdcda-138">hello Logic App Designer megnyílik, és megjeleníti az elérhető szolgáltatások és *eseményindítók* , amely a Logic Apps alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="bdcda-138">hello Logic App Designer opens and shows  available services and *triggers* that  you can use in your logic app.</span></span>

5. <span data-ttu-id="bdcda-139">Hello keresési mezőbe, írja be a `RSS`, és válassza ki az ehhez az eseményindítóhoz: **RSS - hírcsatorna elem meg van nyitva**</span><span class="sxs-lookup"><span data-stu-id="bdcda-139">In hello search box, type `RSS`, and select this trigger: **RSS - When a feed item is published**</span></span> 

    ![RSS-eseményindító](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. <span data-ttu-id="bdcda-141">Adja meg, amelyet az tootrack hello webhely RSS-hírcsatorna hello hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="bdcda-141">Enter hello link for hello website's RSS feed that you want tootrack.</span></span> 

     <span data-ttu-id="bdcda-142">Módosíthatja a **Gyakoriság** és az **Időköz** értékeket is.</span><span class="sxs-lookup"><span data-stu-id="bdcda-142">You can also change **Frequency** and **Interval**.</span></span> 
     <span data-ttu-id="bdcda-143">Ezek a beállítások határozzák meg, hogy a logikai alkalmazás milyen gyakran keres új elemeket és ad vissza minden megtalált elemet az adott időtartományon belül.</span><span class="sxs-lookup"><span data-stu-id="bdcda-143">These settings determine how often your logic app checks for new items and returns all items found during that time span.</span></span>

     <span data-ttu-id="bdcda-144">Ebben a példában most ellenőrizze naponta a legfontosabb események toohello CNN webhelyen közzétett.</span><span class="sxs-lookup"><span data-stu-id="bdcda-144">For this example, let's check every day for   top stories posted toohello CNN website.</span></span>

     ![Eseményindító beállítása RSS-hírcsatornával, gyakorisággal és időközzel](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. <span data-ttu-id="bdcda-146">Egyelőre mentse a munkáját.</span><span class="sxs-lookup"><span data-stu-id="bdcda-146">Save your work for now.</span></span> <span data-ttu-id="bdcda-147">(A hello Tervező parancssávon válassza **mentése**.)</span><span class="sxs-lookup"><span data-stu-id="bdcda-147">(On hello designer command bar, choose **Save**.)</span></span>

   ![A logikai alkalmazás mentése](media/logic-apps-create-a-logic-app/save-logic-app.png)

   <span data-ttu-id="bdcda-149">Ha menti, a logikai alkalmazás élő kerül, de jelenleg a Logic Apps alkalmazást csak keres új elemek hello megadott RSS-hírcsatornára.</span><span class="sxs-lookup"><span data-stu-id="bdcda-149">When you save, your logic app goes live, but currently, your logic app only checks for new items in hello specified RSS feed.</span></span> 
   <span data-ttu-id="bdcda-150">toomake ebben a példában hasznosabb, azt a logikai alkalmazás végző után az eseményindító művelet hozzáadása következik be.</span><span class="sxs-lookup"><span data-stu-id="bdcda-150">toomake this example more useful, we add an action that your logic app performs after your trigger fires.</span></span>

## <a name="add-an-action-that-responds-tooyour-trigger"></a><span data-ttu-id="bdcda-151">Amely válaszol a tooyour eseményindító művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bdcda-151">Add an action that responds tooyour trigger</span></span>

<span data-ttu-id="bdcda-152">A [*művelet*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) a logikai alkalmazás munkafolyamata által végrehajtott feladat.</span><span class="sxs-lookup"><span data-stu-id="bdcda-152">An [*action*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="bdcda-153">Egy eseményindító tooyour logikai alkalmazás hozzáadása után az, hogy az eseményindító által létrehozott adatok egy művelet tooperform műveletek is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="bdcda-153">After you add a trigger tooyour logic app, you can add an action tooperform operations with data generated by that trigger.</span></span> <span data-ttu-id="bdcda-154">A példa kedvéért azt most művelet hozzáadása, amely e-mailt küld, amikor új elemek jelennek meg a hello webhely RSS-hírcsatornára.</span><span class="sxs-lookup"><span data-stu-id="bdcda-154">For our example, we now add an action that sends email when new items appear in hello website's RSS feed.</span></span>

1. <span data-ttu-id="bdcda-155">Hello Designer alapján az eseményindító válasszon **új lépés** > **művelet hozzáadása** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="bdcda-155">In hello designer, under your trigger, choose **New step** > **Add an action** as shown here:</span></span>

   ![Művelet hozzáadása](media/logic-apps-create-a-logic-app/add-new-action.png)

   <span data-ttu-id="bdcda-157">hello a Tervező látható szövegrészt [összekötőket](../connectors/apis-list.md) , hogy egy művelet tooperform kiválaszthatja, ha az eseményindító következik be.</span><span class="sxs-lookup"><span data-stu-id="bdcda-157">hello designer shows [available connectors](../connectors/apis-list.md) so that you can select an action tooperform when your trigger fires.</span></span>

2. <span data-ttu-id="bdcda-158">Az Outlook vagy Gmailes e-mail fiókja alapján, kövesse az hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="bdcda-158">Based on your email account, follow hello steps for Outlook or Gmail.</span></span>

   * <span data-ttu-id="bdcda-159">toosend e-mailt az Outlook fiókból hello keresési mezőbe, írja be `outlook`.</span><span class="sxs-lookup"><span data-stu-id="bdcda-159">toosend email from your Outlook account, in hello search box, enter `outlook`.</span></span> <span data-ttu-id="bdcda-160">A **Szolgáltatások** területen válassza az **Outlook.com** elemet személyes Microsoft-fiók, illetve az **Office 365 Outlook** elemet Azure munkahelyi vagy iskolai fiókok esetében.</span><span class="sxs-lookup"><span data-stu-id="bdcda-160">Under **Services**, choose **Outlook.com** for personal Microsoft accounts, or choose **Office 365 Outlook** for Azure work or school accounts.</span></span> 
   <span data-ttu-id="bdcda-161">A **Műveletek** területen válassza az **E-mal küldése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bdcda-161">Under **Actions**, select **Send an email**.</span></span>

       ![Az Outlook „E-mail küldése” műveletének kiválasztása](media/logic-apps-create-a-logic-app/actions.png)

   * <span data-ttu-id="bdcda-163">toosend e-mail fiókjából Gmail, hello keresési mezőbe, írja be `gmail`.</span><span class="sxs-lookup"><span data-stu-id="bdcda-163">toosend email from your Gmail account, in hello search box, enter `gmail`.</span></span> 
   <span data-ttu-id="bdcda-164">A **Műveletek** területen válassza az **E-mal küldése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bdcda-164">Under **Actions**, select **Send email**.</span></span>

       ![Válassza a „Gmail – E-mail küldése” elemet](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. <span data-ttu-id="bdcda-166">Amikor a rendszer kéri a hitelesítő adatokat, jelentkezzen be a hello felhasználónevet és jelszót e-mail fiókjához.</span><span class="sxs-lookup"><span data-stu-id="bdcda-166">When you're prompted for credentials, sign in with hello username and password for your email account.</span></span> 

4. <span data-ttu-id="bdcda-167">Hello részletesen a művelet, például hello cél e-mail címét, és hello adatok tooinclude hello paramétereinek választani hello e-mailben, például:</span><span class="sxs-lookup"><span data-stu-id="bdcda-167">Provide hello details for this action, like hello destination email address, and choose hello parameters for hello data tooinclude in hello email, for example:</span></span>

   ![Válassza az adatok tooinclude e-mailben](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    <span data-ttu-id="bdcda-169">Ha az Outlookot választja, a logikai alkalmazás a következőhöz hasonlóan nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="bdcda-169">So if you chose Outlook,  your logic app might look like this example:</span></span>

    ![Az elkészült logikai alkalmazás](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  <span data-ttu-id="bdcda-171">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bdcda-171">Save your changes.</span></span> <span data-ttu-id="bdcda-172">(A hello Tervező parancssávon válassza **mentése**.)</span><span class="sxs-lookup"><span data-stu-id="bdcda-172">(On hello designer command bar, choose **Save**.)</span></span>

6. <span data-ttu-id="bdcda-173">Mostantól manuálisan futtathatja a logikai alkalmazást tesztelés céljából.</span><span class="sxs-lookup"><span data-stu-id="bdcda-173">You can now manually run your logic app for testing.</span></span> <span data-ttu-id="bdcda-174">A hello Tervező parancssávon válassza **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-174">On hello designer command bar, choose **Run**.</span></span> <span data-ttu-id="bdcda-175">Ellenkező esetben hogy a Logic Apps alkalmazást ellenőrizze hello megadott RSS-hírcsatorna a beállított hello ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="bdcda-175">Otherwise, you can let your logic app check hello specified RSS feed based on hello schedule that you set up.</span></span>

   <span data-ttu-id="bdcda-176">Ha a logikai alkalmazás új elemek talál, a hello logikai alkalmazást a kijelölt adatokat tartalmazó e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="bdcda-176">If your logic app finds new items, hello logic app sends email that includes your selected data.</span></span> 
   <span data-ttu-id="bdcda-177">Ha új elemek találhatók, a logikai alkalmazás kihagyja, hogy e-mailt küld hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-177">If no new items are found, your logic app skips hello action that sends email.</span></span>

7. <span data-ttu-id="bdcda-178">toomonitor és ellenőrizze a Logic Apps alkalmazást tartozó futtatása és indul el, előzmények a logic app menüben válasszon **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-178">toomonitor and check your logic app's run and trigger history, on your logic app menu, choose **Overview**.</span></span>

   ![A logikai alkalmazás futtatási és eseményindítási előzményeinek figyelése és ellenőrzése](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > <span data-ttu-id="bdcda-180">Ha nem találja a hello adatokat várt, hello parancssávon, válasszon **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-180">If you don't find hello data that you expect, on hello command bar, try choosing **Refresh**.</span></span>

   <span data-ttu-id="bdcda-181">További információk a Logic Apps alkalmazást állapot toolearn vagy futtatása és előzmények vagy toodiagnose a Logic Apps alkalmazást indítható el, lásd: [hibaelhárítása a Logic Apps alkalmazást](logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="bdcda-181">toolearn more about your logic app's status or run and trigger history, or toodiagnose your logic app, see [Troubleshoot your logic app](logic-apps-diagnosing-failures.md).</span></span>

      > [!NOTE]
      > <span data-ttu-id="bdcda-182">A logikai alkalmazás addig fut, amíg ki nem kapcsolja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bdcda-182">Your logic app continues running until you turn off your app.</span></span> <span data-ttu-id="bdcda-183">Válassza ki az alkalmazás most a logic app menüjében tooturn **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-183">tooturn off your app for now, on your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="bdcda-184">A hello parancssávon válassza **letiltása**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-184">On hello command bar, choose **Disable**.</span></span>

<span data-ttu-id="bdcda-185">Gratulálunk, sikeresen beállította és futtatta az első logikai alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="bdcda-185">Congratulations, you just set up and run your first basic logic app.</span></span> <span data-ttu-id="bdcda-186">Azt is megtanulta, milyen egyszerű a folyamatokat automatizáló munkafolyamat létrehozása, valamint a felhőalapú alkalmazások és felhőszolgáltatások integrálása – mindez kód nélkül.</span><span class="sxs-lookup"><span data-stu-id="bdcda-186">You also learned how easily you can create workflows that automate processes, and integrate cloud apps and cloud services - all without code.</span></span>

## <a name="manage-your-logic-app"></a><span data-ttu-id="bdcda-187">A logikai alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="bdcda-187">Manage your logic app</span></span>

<span data-ttu-id="bdcda-188">toomanage az alkalmazás feladatokat hajthat végre hasonló hello állapotának, szerkesztése, előzményeinek megtekintése, kapcsolja ki, vagy törölje a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bdcda-188">toomanage your app, you can perform tasks like check hello status, edit, view history, turn off, or delete your logic app.</span></span>

1. <span data-ttu-id="bdcda-189">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="bdcda-189">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="bdcda-190">A hello bal oldali menüben válassza a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-190">On hello left menu, choose **More services**.</span></span> <span data-ttu-id="bdcda-191">A **Vállalati integráció** résznél válassza a **Logikai alkalmazások** elemet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-191">Under **Enterprise Integration**, choose **Logic Apps**.</span></span> <span data-ttu-id="bdcda-192">Válassza ki a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bdcda-192">Select your logic app.</span></span> 

   <span data-ttu-id="bdcda-193">Hello logic app menüjében találja meg a logic app felügyeleti feladatok:</span><span class="sxs-lookup"><span data-stu-id="bdcda-193">In hello logic app menu, you can find these logic app management tasks:</span></span>

   |<span data-ttu-id="bdcda-194">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="bdcda-194">Task</span></span>|<span data-ttu-id="bdcda-195">Lépések</span><span class="sxs-lookup"><span data-stu-id="bdcda-195">Steps</span></span>| 
   |:---|:---| 
   | <span data-ttu-id="bdcda-196">Az alkalmazás állapotának, végrehajtási előzményeinek és általános információinak megtekintése</span><span class="sxs-lookup"><span data-stu-id="bdcda-196">View your app's status, execution history, and general information</span></span>| <span data-ttu-id="bdcda-197">Válassza az **Áttekintés** elemet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-197">Choose **Overview**.</span></span>| 
   | <span data-ttu-id="bdcda-198">Az alkalmazás szerkesztése</span><span class="sxs-lookup"><span data-stu-id="bdcda-198">Edit your app</span></span> | <span data-ttu-id="bdcda-199">Válassza a **Logikaialkalmazás-tervező** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bdcda-199">Choose **Logic App Designer**.</span></span> | 
   | <span data-ttu-id="bdcda-200">Az alkalmazáshoz tartozó munkafolyamat JSON-definíciójának megtekintése</span><span class="sxs-lookup"><span data-stu-id="bdcda-200">View your app's workflow JSON definition</span></span> | <span data-ttu-id="bdcda-201">Válassza a **Logikai alkalmazás kódnézete** elemet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-201">Choose **Logic App Code View**.</span></span> | 
   | <span data-ttu-id="bdcda-202">A logikai alkalmazáson végrehajtott műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="bdcda-202">View operations performed on your logic app</span></span> | <span data-ttu-id="bdcda-203">Válassza a **Tevékenységnapló** elemet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-203">Choose **Activity log**.</span></span> | 
   | <span data-ttu-id="bdcda-204">A logikai alkalmazás korábbi verzióinak megtekintése</span><span class="sxs-lookup"><span data-stu-id="bdcda-204">View past versions for your logic app</span></span> | <span data-ttu-id="bdcda-205">Válassza a **Verziók** elemet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-205">Choose **Versions**.</span></span> | 
   | <span data-ttu-id="bdcda-206">Az alkalmazás ideiglenes kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="bdcda-206">Turn off your app temporarily</span></span> | <span data-ttu-id="bdcda-207">Válassza ki **áttekintése**, majd a hello parancssávon válassza **tiltsa le a**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-207">Choose **Overview**, then on hello command bar, choose **Disable**.</span></span> | 
   | <span data-ttu-id="bdcda-208">Az alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="bdcda-208">Delete your app</span></span> | <span data-ttu-id="bdcda-209">Válassza ki **áttekintése**, majd a hello parancssávon válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="bdcda-209">Choose **Overview**, then on hello command bar, choose **Delete**.</span></span> <span data-ttu-id="bdcda-210">Adja meg a logikai alkalmazás nevét, majd válassza a **Törlés** elemet.</span><span class="sxs-lookup"><span data-stu-id="bdcda-210">Enter your logic app's name, and choose **Delete**.</span></span> | 

## <a name="get-help"></a><span data-ttu-id="bdcda-211">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="bdcda-211">Get help</span></span>

<span data-ttu-id="bdcda-212">tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="bdcda-212">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="bdcda-213">toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="bdcda-213">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bdcda-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdcda-214">Next steps</span></span>

*  [<span data-ttu-id="bdcda-215">Feltételek hozzáadása és munkafolyamatok futtatása</span><span class="sxs-lookup"><span data-stu-id="bdcda-215">Add conditions and run workflows</span></span>](../logic-apps/logic-apps-use-logic-app-features.md)
*    [<span data-ttu-id="bdcda-216">A logikai alkalmazások sablonjai</span><span class="sxs-lookup"><span data-stu-id="bdcda-216">Logic app templates</span></span>](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [<span data-ttu-id="bdcda-217">Logikai alkalmazások létrehozása Azure Resource Manager-sablonokból</span><span class="sxs-lookup"><span data-stu-id="bdcda-217">Create logic apps from Azure Resource Manager templates</span></span>](../logic-apps/logic-apps-arm-provision.md)
