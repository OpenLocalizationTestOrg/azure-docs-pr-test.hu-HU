---
title: "Az első munkafolyamat létrehozása felhőalapú alkalmazások és felhőszolgáltatások között – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 204bf123509729b60b55c306050cef54aa7fecc5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services"></a><span data-ttu-id="91cfb-104">Az első logikai alkalmazás munkafolyamatának létrehozása folyamatok automatizálásához a felhőalapú alkalmazások és felhőszolgáltatások között</span><span class="sxs-lookup"><span data-stu-id="91cfb-104">Create your first logic app workflow to automate processes between cloud apps and cloud services</span></span>

<span data-ttu-id="91cfb-105">Könnyebben és gyorsabban automatizálhatja az üzleti folyamatokat kód megírása nélkül is, ha az [Azure Logic Apps](logic-apps-what-are-logic-apps.md) segítségével hoz létre és futtat munkafolyamatokat.</span><span class="sxs-lookup"><span data-stu-id="91cfb-105">Without writing any code, you can automate business processes more easily and quickly when you create and run workflows with [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span></span> <span data-ttu-id="91cfb-106">Az első példa bemutatja, hogyan hozhat létre alapszintű logikaialkalmazás-munkafolyamatot, amely egy webhely RSS-hírcsatornájában keres új tartalmat.</span><span class="sxs-lookup"><span data-stu-id="91cfb-106">This first example shows how to create a basic logic app workflow that checks an RSS feed for new content on a website.</span></span> <span data-ttu-id="91cfb-107">Ha új elemek jelennek meg a webhely hírcsatornájában, a logikai alkalmazás e-mailt küld az Outlook- vagy Gmail-fiókból.</span><span class="sxs-lookup"><span data-stu-id="91cfb-107">When new items appear in the website's feed, the logic app sends email from an Outlook or Gmail account.</span></span>

<span data-ttu-id="91cfb-108">Logikai alkalmazás létrehozásához és futtatásához a következő elemekre van szükség:</span><span class="sxs-lookup"><span data-stu-id="91cfb-108">To create and run a logic app, you need these items:</span></span>

* <span data-ttu-id="91cfb-109">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="91cfb-109">An Azure subscription.</span></span> <span data-ttu-id="91cfb-110">Ha nem rendelkezik előfizetéssel, [kezdhet egy ingyenes Azure-fiókkal](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="91cfb-110">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="91cfb-111">Egyéb esetben [regisztrálhat használatalapú fizetéses előfizetésre](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="91cfb-111">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

  <span data-ttu-id="91cfb-112">Az Azure-előfizetés a logikaialkalmazás-használat számlázására szolgál.</span><span class="sxs-lookup"><span data-stu-id="91cfb-112">Your Azure subscription is used for billing logic app usage.</span></span> <span data-ttu-id="91cfb-113">Ismerje meg, hogyan működik a [használatmérés](../logic-apps/logic-apps-pricing.md) és a [díjszabás](https://azure.microsoft.com/pricing/details/logic-apps) az Azure Logic Apps esetében.</span><span class="sxs-lookup"><span data-stu-id="91cfb-113">Learn how [usage metering](../logic-apps/logic-apps-pricing.md) and [pricing](https://azure.microsoft.com/pricing/details/logic-apps) work for Azure Logic Apps.</span></span>

<span data-ttu-id="91cfb-114">A példához a következő elemek is szükségesek:</span><span class="sxs-lookup"><span data-stu-id="91cfb-114">Also, this example requires these items:</span></span>

* <span data-ttu-id="91cfb-115">Outlook.com-, Office 365 Outlook- vagy Gmail-fiók</span><span class="sxs-lookup"><span data-stu-id="91cfb-115">An Outlook.com, Office 365 Outlook, or Gmail account</span></span>

    > [!TIP]
    > <span data-ttu-id="91cfb-116">Ha rendelkezik személyes [Microsoft-fiókkal](https://account.microsoft.com/account), akkor Outlook.com-fiókja van.</span><span class="sxs-lookup"><span data-stu-id="91cfb-116">If you have a personal [Microsoft account](https://account.microsoft.com/account), you have an Outlook.com account.</span></span> <span data-ttu-id="91cfb-117">Egyéb esetben, ha rendelkezik Azure munkahelyi vagy iskolai fiókkal, **Office 365 Outlook**-fiókja van.</span><span class="sxs-lookup"><span data-stu-id="91cfb-117">Otherwise, if you have an Azure work or school account, you have an **Office 365 Outlook** account.</span></span>

* <span data-ttu-id="91cfb-118">Egy webhely RSS-hírcsatornájára mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="91cfb-118">A link to a website's RSS feed.</span></span> <span data-ttu-id="91cfb-119">Ez a példa a [CNN.com webhely legfontosabb híreit tartalmazó RSS-hírcsatornáját használja](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span><span class="sxs-lookup"><span data-stu-id="91cfb-119">This example uses the [RSS feed for top stories from the CNN.com website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span></span>

## <a name="add-a-trigger-that-starts-your-workflow"></a><span data-ttu-id="91cfb-120">Eseményindító hozzáadása a munkafolyamat indításához</span><span class="sxs-lookup"><span data-stu-id="91cfb-120">Add a trigger that starts your workflow</span></span>

<span data-ttu-id="91cfb-121">Az [*eseményindító*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) egy olyan esemény, amely elindítja a logikai alkalmazás munkafolyamatát, valamint az első elem, amelyre a logikai alkalmazásnak szüksége van.</span><span class="sxs-lookup"><span data-stu-id="91cfb-121">A [*trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts your logic app workflow and is the first item that your logic app needs.</span></span>

1. <span data-ttu-id="91cfb-122">Jelentkezzen be az [Azure Portalra](https://portal.azure.com "Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="91cfb-122">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="91cfb-123">A bal oldali menüből válassza az **Új** > **Vállalati integráció** > **Logikai alkalmazás** elemet az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="91cfb-123">From the left menu, choose **New** > **Enterprise Integration** > **Logic App** as shown here:</span></span>

     ![Azure Portal, Új, Vállalati integráció, Logikai alkalmazás](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > <span data-ttu-id="91cfb-125">Vagy válassza az **Új** elemet, írja be a keresőmezőbe a `logic app` kifejezést, majd nyomja le az Enter billentyűt.</span><span class="sxs-lookup"><span data-stu-id="91cfb-125">You can also choose **New**, then in the search box, type `logic app`, and press Enter.</span></span> <span data-ttu-id="91cfb-126">Válassza a **Logikai alkalmazás** > **Létrehozás** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-126">Then choose **Logic App** > **Create**.</span></span>

3. <span data-ttu-id="91cfb-127">Nevezze el a logikai alkalmazást, majd válassza ki az Azure-előfizetését.</span><span class="sxs-lookup"><span data-stu-id="91cfb-127">Name your logic app and select your Azure subscription.</span></span> <span data-ttu-id="91cfb-128">Most hozzon létre vagy válasszon ki egy Azure-erőforráscsoportot, amely segít a kapcsolódó Azure-erőforrások rendszerezésében és kezelésében.</span><span class="sxs-lookup"><span data-stu-id="91cfb-128">Now create or select an Azure resource group, which helps you organize and manage related Azure resources.</span></span> <span data-ttu-id="91cfb-129">Végül válassza ki az adatközpont helyét a logikai alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="91cfb-129">Finally, select the datacenter location for hosting your logic app.</span></span> <span data-ttu-id="91cfb-130">Ha elkészült, válassza a **Rögzítés az irányítópulton**, majd a **Létrehozás** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-130">When you're ready, choose **Pin to dashboard** and then **Create**.</span></span>

     ![Logikai alkalmazás részletei](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > <span data-ttu-id="91cfb-132">Amikor kiválasztja a **Rögzítés az irányítópulton** elemet, a logikai alkalmazás az üzembe helyezés után megjelenik az Azure irányítópulton, és automatikusan megnyílik.</span><span class="sxs-lookup"><span data-stu-id="91cfb-132">When you select **Pin to dashboard**, your logic app appears on the Azure dashboard after deployment, and opens automatically.</span></span> <span data-ttu-id="91cfb-133">Ha a logikai alkalmazás nem jelenik meg az irányítópulton, a **Minden erőforrás** csempén válassza a **Több megjelenítése** lehetőséget, majd válassza ki a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91cfb-133">If your logic app doesn't appear on the dashboard, on the **All resources** tile, choose **See More**, and select your logic app.</span></span> <span data-ttu-id="91cfb-134">Vagy a bal oldali menüben válassza a **További szolgáltatások** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-134">Or on the left menu, choose **More services**.</span></span> <span data-ttu-id="91cfb-135">A **Vállalati integráció** résznél válassza a **Logikai alkalmazások** elemet, majd válassza ki a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91cfb-135">Under **Enterprise Integration**, choose **Logic Apps**, and select your logic app.</span></span>

4. <span data-ttu-id="91cfb-136">A logikai alkalmazás első megnyitásakor, a Logikaialkalmazás-tervező megjeleníti a kezdéshez használható sablonokat.</span><span class="sxs-lookup"><span data-stu-id="91cfb-136">When you open your logic app for the first time, the Logic App Designer shows templates that you can use to get started.</span></span> <span data-ttu-id="91cfb-137">Jelen esetben válassza az **Üres logikai alkalmazás** elemet, így az alapoktól építheti fel saját logikai alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="91cfb-137">For now, choose **Blank Logic App** so you can build your logic app from scratch.</span></span>

    <span data-ttu-id="91cfb-138">A Logic App Designer megnyílik, és megjeleníti az elérhető szolgáltatások és *eseményindítók* , amely a Logic Apps alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="91cfb-138">The Logic App Designer opens and shows  available services and *triggers* that  you can use in your logic app.</span></span>

5. <span data-ttu-id="91cfb-139">A keresőmezőbe írja be a(z) `RSS` kifejezést, majd válassza ki a következő eseményindítót: **RSS – Egy új hírcsatornaelem közzétételekor**</span><span class="sxs-lookup"><span data-stu-id="91cfb-139">In the search box, type `RSS`, and select this trigger: **RSS - When a feed item is published**</span></span> 

    ![RSS-eseményindító](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. <span data-ttu-id="91cfb-141">Írja be a webhely követni kívánt RSS-hírcsatornájára mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="91cfb-141">Enter the link for the website's RSS feed that you want to track.</span></span> 

     <span data-ttu-id="91cfb-142">Módosíthatja a **Gyakoriság** és az **Időköz** értékeket is.</span><span class="sxs-lookup"><span data-stu-id="91cfb-142">You can also change **Frequency** and **Interval**.</span></span> 
     <span data-ttu-id="91cfb-143">Ezek a beállítások határozzák meg, hogy a logikai alkalmazás milyen gyakran keres új elemeket és ad vissza minden megtalált elemet az adott időtartományon belül.</span><span class="sxs-lookup"><span data-stu-id="91cfb-143">These settings determine how often your logic app checks for new items and returns all items found during that time span.</span></span>

     <span data-ttu-id="91cfb-144">Ebben a példában minden nap ellenőrizni fogja a CNN webhelyén közzétett legfontosabb híreket.</span><span class="sxs-lookup"><span data-stu-id="91cfb-144">For this example, let's check every day for   top stories posted to the CNN website.</span></span>

     ![Eseményindító beállítása RSS-hírcsatornával, gyakorisággal és időközzel](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. <span data-ttu-id="91cfb-146">Egyelőre mentse a munkáját.</span><span class="sxs-lookup"><span data-stu-id="91cfb-146">Save your work for now.</span></span> <span data-ttu-id="91cfb-147">(A tervező parancssávján válassza a **Mentés** elemet.)</span><span class="sxs-lookup"><span data-stu-id="91cfb-147">(On the designer command bar, choose **Save**.)</span></span>

   ![A logikai alkalmazás mentése](media/logic-apps-create-a-logic-app/save-logic-app.png)

   <span data-ttu-id="91cfb-149">Mentéskor a rendszer élesíti a logikai alkalmazást, de az jelenleg csak a megadott RSS-hírcsatornában keres új elemeket.</span><span class="sxs-lookup"><span data-stu-id="91cfb-149">When you save, your logic app goes live, but currently, your logic app only checks for new items in the specified RSS feed.</span></span> 
   <span data-ttu-id="91cfb-150">A példa hasznosabbá tételéhez adjunk hozzá egy műveletet, amelyet a logikai alkalmazás az eseményindító aktiválása után hajt végre.</span><span class="sxs-lookup"><span data-stu-id="91cfb-150">To make this example more useful, we add an action that your logic app performs after your trigger fires.</span></span>

## <a name="add-an-action-that-responds-to-your-trigger"></a><span data-ttu-id="91cfb-151">Az eseményindítóra válaszoló művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91cfb-151">Add an action that responds to your trigger</span></span>

<span data-ttu-id="91cfb-152">A [*művelet*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) a logikai alkalmazás munkafolyamata által végrehajtott feladat.</span><span class="sxs-lookup"><span data-stu-id="91cfb-152">An [*action*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="91cfb-153">Miután hozzáadott egy eseményindítót a logikai alkalmazáshoz, hozzáadhat egy műveletet is, amely további műveleteket végez el az eseményindító által előállított adatokkal.</span><span class="sxs-lookup"><span data-stu-id="91cfb-153">After you add a trigger to your logic app, you can add an action to perform operations with data generated by that trigger.</span></span> <span data-ttu-id="91cfb-154">A példában egy olyan műveletet adunk hozzá, amely e-mailt küld, ha új elem jelenik meg a webhely RSS-hírcsatornájában.</span><span class="sxs-lookup"><span data-stu-id="91cfb-154">For our example, we now add an action that sends email when new items appear in the website's RSS feed.</span></span>

1. <span data-ttu-id="91cfb-155">A tervezőben az eseményindító területén válassza az **Új lépés** > **Művelet hozzáadása** elemet az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="91cfb-155">In the designer, under your trigger, choose **New step** > **Add an action** as shown here:</span></span>

   ![Művelet hozzáadása](media/logic-apps-create-a-logic-app/add-new-action.png)

   <span data-ttu-id="91cfb-157">A tervező megjeleníti az [elérhető összekötőket](../connectors/apis-list.md), így választhat, hogy milyen művelet legyen végrehajtva az eseményindító aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="91cfb-157">The designer shows [available connectors](../connectors/apis-list.md) so that you can select an action to perform when your trigger fires.</span></span>

2. <span data-ttu-id="91cfb-158">E-mail fiókja függvényében kövesse az Outlookra vagy a Gmailre vonatkozó lépéseket.</span><span class="sxs-lookup"><span data-stu-id="91cfb-158">Based on your email account, follow the steps for Outlook or Gmail.</span></span>

   * <span data-ttu-id="91cfb-159">E-mail küldéséhez az Outlook-fiókjából írja be a következő kifejezést a keresőmezőbe: `outlook`.</span><span class="sxs-lookup"><span data-stu-id="91cfb-159">To send email from your Outlook account, in the search box, enter `outlook`.</span></span> <span data-ttu-id="91cfb-160">A **Szolgáltatások** területen válassza az **Outlook.com** elemet személyes Microsoft-fiók, illetve az **Office 365 Outlook** elemet Azure munkahelyi vagy iskolai fiókok esetében.</span><span class="sxs-lookup"><span data-stu-id="91cfb-160">Under **Services**, choose **Outlook.com** for personal Microsoft accounts, or choose **Office 365 Outlook** for Azure work or school accounts.</span></span> 
   <span data-ttu-id="91cfb-161">A **Műveletek** területen válassza az **E-mal küldése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="91cfb-161">Under **Actions**, select **Send an email**.</span></span>

       ![Az Outlook „E-mail küldése” műveletének kiválasztása](media/logic-apps-create-a-logic-app/actions.png)

   * <span data-ttu-id="91cfb-163">E-mail küldéséhez a Gmail-fiókjából írja be a következő kifejezést a keresőmezőbe: `gmail`.</span><span class="sxs-lookup"><span data-stu-id="91cfb-163">To send email from your Gmail account, in the search box, enter `gmail`.</span></span> 
   <span data-ttu-id="91cfb-164">A **Műveletek** területen válassza az **E-mal küldése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="91cfb-164">Under **Actions**, select **Send email**.</span></span>

       ![Válassza a „Gmail – E-mail küldése” elemet](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. <span data-ttu-id="91cfb-166">Ha a rendszer hitelesítő adatok megadását kéri, jelentkezzen be az e-mail-fiókjához tartozó felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="91cfb-166">When you're prompted for credentials, sign in with the username and password for your email account.</span></span> 

4. <span data-ttu-id="91cfb-167">Adja meg a művelet részleteit (például a cél e-mail-címét), majd válassza ki az e-mailben szerepeltetni kívánt adatok paramétereit, például:</span><span class="sxs-lookup"><span data-stu-id="91cfb-167">Provide the details for this action, like the destination email address, and choose the parameters for the data to include in the email, for example:</span></span>

   ![Az e-mailben szerepeltetni kívánt adatok kiválasztása](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    <span data-ttu-id="91cfb-169">Ha az Outlookot választja, a logikai alkalmazás a következőhöz hasonlóan nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="91cfb-169">So if you chose Outlook,  your logic app might look like this example:</span></span>

    ![Az elkészült logikai alkalmazás](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  <span data-ttu-id="91cfb-171">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="91cfb-171">Save your changes.</span></span> <span data-ttu-id="91cfb-172">(A tervező parancssávján válassza a **Mentés** elemet.)</span><span class="sxs-lookup"><span data-stu-id="91cfb-172">(On the designer command bar, choose **Save**.)</span></span>

6. <span data-ttu-id="91cfb-173">Mostantól manuálisan futtathatja a logikai alkalmazást tesztelés céljából.</span><span class="sxs-lookup"><span data-stu-id="91cfb-173">You can now manually run your logic app for testing.</span></span> <span data-ttu-id="91cfb-174">A tervező parancssávján válassza a **Futtatás** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-174">On the designer command bar, choose **Run**.</span></span> <span data-ttu-id="91cfb-175">Egyéb esetben beállíthat egy ütemezést, amely alapján a logikai alkalmazás ellenőrzi a megadott RSS-hírcsatornát.</span><span class="sxs-lookup"><span data-stu-id="91cfb-175">Otherwise, you can let your logic app check the specified RSS feed based on the schedule that you set up.</span></span>

   <span data-ttu-id="91cfb-176">Ha a logikai alkalmazás új elemeket talál, a kiválasztott adatokat tartalmazó e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="91cfb-176">If your logic app finds new items, the logic app sends email that includes your selected data.</span></span> 
   <span data-ttu-id="91cfb-177">Ha nem talál új elemeket, a logikai alkalmazás kihagyja az e-mail küldésére vonatkozó műveletet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-177">If no new items are found, your logic app skips the action that sends email.</span></span>

7. <span data-ttu-id="91cfb-178">A logikai alkalmazás futtatási és eseményindítási előzményeinek figyeléséhez és ellenőrzéséhez válassza az **Áttekintés** lehetőséget a logikai alkalmazás menüjében.</span><span class="sxs-lookup"><span data-stu-id="91cfb-178">To monitor and check your logic app's run and trigger history, on your logic app menu, choose **Overview**.</span></span>

   ![A logikai alkalmazás futtatási és eseményindítási előzményeinek figyelése és ellenőrzése](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > <span data-ttu-id="91cfb-180">Ha nem jelennek meg a várt adatok, válassza a **Frissítés** elemet a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="91cfb-180">If you don't find the data that you expect, on the command bar, try choosing **Refresh**.</span></span>

   <span data-ttu-id="91cfb-181">A logikai alkalmazás állapotával, illetve a futtatási és eseményindítási előzményeivel kapcsolatos további információkért, valamint a logikai alkalmazás diagnosztizáláshoz tekintse meg a [logikai alkalmazás hibaelhárításával foglalkozó szakaszt](logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="91cfb-181">To learn more about your logic app's status or run and trigger history, or to diagnose your logic app, see [Troubleshoot your logic app](logic-apps-diagnosing-failures.md).</span></span>

      > [!NOTE]
      > <span data-ttu-id="91cfb-182">A logikai alkalmazás addig fut, amíg ki nem kapcsolja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91cfb-182">Your logic app continues running until you turn off your app.</span></span> <span data-ttu-id="91cfb-183">Az alkalmazás ideiglenes kikapcsolásához a logikai alkalmazás menüjében válassza az **Áttekintés** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-183">To turn off your app for now, on your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="91cfb-184">A parancssávon válassza a **Letiltás** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-184">On the command bar, choose **Disable**.</span></span>

<span data-ttu-id="91cfb-185">Gratulálunk, sikeresen beállította és futtatta az első logikai alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="91cfb-185">Congratulations, you just set up and run your first basic logic app.</span></span> <span data-ttu-id="91cfb-186">Azt is megtanulta, milyen egyszerű a folyamatokat automatizáló munkafolyamat létrehozása, valamint a felhőalapú alkalmazások és felhőszolgáltatások integrálása – mindez kód nélkül.</span><span class="sxs-lookup"><span data-stu-id="91cfb-186">You also learned how easily you can create workflows that automate processes, and integrate cloud apps and cloud services - all without code.</span></span>

## <a name="manage-your-logic-app"></a><span data-ttu-id="91cfb-187">A logikai alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="91cfb-187">Manage your logic app</span></span>

<span data-ttu-id="91cfb-188">Az alkalmazás kezeléséhez olyan műveleteket hajthat végre, mint például az állapot ellenőrzése, a szerkesztés, az előzmények megtekintése, a kikapcsolás vagy a logikai alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="91cfb-188">To manage your app, you can perform tasks like check the status, edit, view history, turn off, or delete your logic app.</span></span>

1. <span data-ttu-id="91cfb-189">Jelentkezzen be az [Azure Portalra](https://portal.azure.com "Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="91cfb-189">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="91cfb-190">A bal oldali menüben válassza a **További szolgáltatások** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-190">On the left menu, choose **More services**.</span></span> <span data-ttu-id="91cfb-191">A **Vállalati integráció** résznél válassza a **Logikai alkalmazások** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-191">Under **Enterprise Integration**, choose **Logic Apps**.</span></span> <span data-ttu-id="91cfb-192">Válassza ki a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="91cfb-192">Select your logic app.</span></span> 

   <span data-ttu-id="91cfb-193">A logikai alkalmazás menüjében a következő logikaialkalmazás-kezelési feladatokat találja:</span><span class="sxs-lookup"><span data-stu-id="91cfb-193">In the logic app menu, you can find these logic app management tasks:</span></span>

   |<span data-ttu-id="91cfb-194">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="91cfb-194">Task</span></span>|<span data-ttu-id="91cfb-195">Lépések</span><span class="sxs-lookup"><span data-stu-id="91cfb-195">Steps</span></span>| 
   |:---|:---| 
   | <span data-ttu-id="91cfb-196">Az alkalmazás állapotának, végrehajtási előzményeinek és általános információinak megtekintése</span><span class="sxs-lookup"><span data-stu-id="91cfb-196">View your app's status, execution history, and general information</span></span>| <span data-ttu-id="91cfb-197">Válassza az **Áttekintés** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-197">Choose **Overview**.</span></span>| 
   | <span data-ttu-id="91cfb-198">Az alkalmazás szerkesztése</span><span class="sxs-lookup"><span data-stu-id="91cfb-198">Edit your app</span></span> | <span data-ttu-id="91cfb-199">Válassza a **Logikaialkalmazás-tervező** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="91cfb-199">Choose **Logic App Designer**.</span></span> | 
   | <span data-ttu-id="91cfb-200">Az alkalmazáshoz tartozó munkafolyamat JSON-definíciójának megtekintése</span><span class="sxs-lookup"><span data-stu-id="91cfb-200">View your app's workflow JSON definition</span></span> | <span data-ttu-id="91cfb-201">Válassza a **Logikai alkalmazás kódnézete** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-201">Choose **Logic App Code View**.</span></span> | 
   | <span data-ttu-id="91cfb-202">A logikai alkalmazáson végrehajtott műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="91cfb-202">View operations performed on your logic app</span></span> | <span data-ttu-id="91cfb-203">Válassza a **Tevékenységnapló** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-203">Choose **Activity log**.</span></span> | 
   | <span data-ttu-id="91cfb-204">A logikai alkalmazás korábbi verzióinak megtekintése</span><span class="sxs-lookup"><span data-stu-id="91cfb-204">View past versions for your logic app</span></span> | <span data-ttu-id="91cfb-205">Válassza a **Verziók** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-205">Choose **Versions**.</span></span> | 
   | <span data-ttu-id="91cfb-206">Az alkalmazás ideiglenes kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="91cfb-206">Turn off your app temporarily</span></span> | <span data-ttu-id="91cfb-207">Válassza az **Áttekintés** elemet, majd a parancssávon válassza a **Letiltás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="91cfb-207">Choose **Overview**, then on the command bar, choose **Disable**.</span></span> | 
   | <span data-ttu-id="91cfb-208">Az alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="91cfb-208">Delete your app</span></span> | <span data-ttu-id="91cfb-209">Válassza az **Áttekintés** elemet, majd a parancssávon válassza a **Törlés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="91cfb-209">Choose **Overview**, then on the command bar, choose **Delete**.</span></span> <span data-ttu-id="91cfb-210">Adja meg a logikai alkalmazás nevét, majd válassza a **Törlés** elemet.</span><span class="sxs-lookup"><span data-stu-id="91cfb-210">Enter your logic app's name, and choose **Delete**.</span></span> | 

## <a name="get-help"></a><span data-ttu-id="91cfb-211">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="91cfb-211">Get help</span></span>

<span data-ttu-id="91cfb-212">Látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps), ahol kérdéseket tehet fel és válaszolhat meg, valamint megtudhatja, mivel foglalkoznak az Azure Logic Apps más felhasználói.</span><span class="sxs-lookup"><span data-stu-id="91cfb-212">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="91cfb-213">Ha szeretné segíteni az Azure Logic Apps és összekötők fejlesztését, szavazzon az ötletekre az [Azure Logic Apps felhasználói visszajelzések oldalán](http://aka.ms/logicapps-wish), vagy küldje be saját javaslatait.</span><span class="sxs-lookup"><span data-stu-id="91cfb-213">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="91cfb-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91cfb-214">Next steps</span></span>

*  [<span data-ttu-id="91cfb-215">Feltételek hozzáadása és munkafolyamatok futtatása</span><span class="sxs-lookup"><span data-stu-id="91cfb-215">Add conditions and run workflows</span></span>](../logic-apps/logic-apps-use-logic-app-features.md)
*    [<span data-ttu-id="91cfb-216">A logikai alkalmazások sablonjai</span><span class="sxs-lookup"><span data-stu-id="91cfb-216">Logic app templates</span></span>](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [<span data-ttu-id="91cfb-217">Logikai alkalmazások létrehozása Azure Resource Manager-sablonokból</span><span class="sxs-lookup"><span data-stu-id="91cfb-217">Create logic apps from Azure Resource Manager templates</span></span>](../logic-apps/logic-apps-arm-provision.md)
