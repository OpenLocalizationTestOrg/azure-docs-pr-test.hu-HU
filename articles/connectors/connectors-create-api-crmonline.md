---
title: "Csatlakozás az Azure Logic Apps Dynamics 365 (online) |} Microsoft Docs"
description: "Munkafolyamatokat logic app által kezelt Dynamics 365 (online) entitásokat a Dynamics 365-összekötő által nyújtott API-n keresztül"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: d35647921ff540167a3a591fb489d3bab031a5c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-dynamics-365-from-logic-app-workflows"></a><span data-ttu-id="6914a-103">Dynamics 365 csatlakoztatja a logic app munkafolyamatok</span><span class="sxs-lookup"><span data-stu-id="6914a-103">Connect to Dynamics 365 from logic app workflows</span></span>

<span data-ttu-id="6914a-104">A Logic Apps (online) a Dynamics 365 csatlakozhat, és hozzon létre hasznos üzleti flow-rekordok létrehozása, elemek frissítését vagy a rekordok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6914a-104">With Logic Apps, you can connect to Dynamics 365 (online) and create useful business flows that create records, update items, or return a list of records.</span></span> <span data-ttu-id="6914a-105">A Dynamics 365-összekötővel a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="6914a-105">With the Dynamics 365 connector, you can:</span></span>

* <span data-ttu-id="6914a-106">Az üzleti folyamat Dynamics 365 (online) származó adatok alapján történő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6914a-106">Build your business flow based on the data you get from Dynamics 365 (online).</span></span>
* <span data-ttu-id="6914a-107">Amely válaszol, és végezze el a kimeneti elérhető egyéb műveletek műveleteket használni.</span><span class="sxs-lookup"><span data-stu-id="6914a-107">Use actions that get a response and then make the output available for other actions.</span></span> <span data-ttu-id="6914a-108">Például frissítésekor elem Dynamics 365 (online), az Office 365-tel e-mailt küldhet.</span><span class="sxs-lookup"><span data-stu-id="6914a-108">For example, when an item is updated in Dynamics 365 (online), you can send an email using Office 365.</span></span>

<span data-ttu-id="6914a-109">Ez a témakör bemutatja, hogyan hozzon létre egy logikai alkalmazás, amely Dynamics 365 létrehoz egy feladatot, amikor egy új vezető Dynamics 365 jön létre.</span><span class="sxs-lookup"><span data-stu-id="6914a-109">This topic shows you how to create a logic app that creates a task in Dynamics 365 whenever a new lead is created in Dynamics 365.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6914a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6914a-110">Prerequisites</span></span>
* <span data-ttu-id="6914a-111">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="6914a-111">An Azure account.</span></span>
* <span data-ttu-id="6914a-112">Dynamics 365 (online) fiók.</span><span class="sxs-lookup"><span data-stu-id="6914a-112">A Dynamics 365 (online) account.</span></span>

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a><span data-ttu-id="6914a-113">Hozzon létre egy feladatot, amikor egy új vezető Dynamics 365 hoz létre</span><span class="sxs-lookup"><span data-stu-id="6914a-113">Create a task when a new lead is created in Dynamics 365</span></span>

1.  <span data-ttu-id="6914a-114">[Jelentkezzen be Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6914a-114">[Sign in to Azure](https://portal.azure.com).</span></span>

2.  <span data-ttu-id="6914a-115">Az Azure search mezőbe írja be `Logic apps`, és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="6914a-115">In the Azure search box, type `Logic apps`, and press ENTER.</span></span>

      ![Logic Apps alkalmazások keresése](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  <span data-ttu-id="6914a-117">A **a Logic apps**, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6914a-117">Under **Logic apps**, click **Add**.</span></span>

      ![LogicApp hozzáadása](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  <span data-ttu-id="6914a-119">A logikai alkalmazás létrehozása, végezze el a **neve**, **előfizetés**, **erőforráscsoport**, és **hely** mezőket, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6914a-119">To create the logic app, complete the **Name**, **Subscription**, **Resource Group**, and **Location** fields, and then click **Create**.</span></span>

5.  <span data-ttu-id="6914a-120">Válassza ki az új logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6914a-120">Select the new logic app.</span></span> <span data-ttu-id="6914a-121">Amikor megjelenik a **sikeres telepítés** értesítést, kattintson a **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="6914a-121">When you receive the **Deployment Succeeded** notification, click **Refresh**.</span></span>

6.  <span data-ttu-id="6914a-122">A **Fejlesztőeszközök**, kattintson a **Logic App Designer**.</span><span class="sxs-lookup"><span data-stu-id="6914a-122">Under **Development Tools**, click **Logic App Designer**.</span></span> <span data-ttu-id="6914a-123">Sablon, kattintson a **üres logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6914a-123">In the template list, click **Blank Logic App**.</span></span>

7.  <span data-ttu-id="6914a-124">Írja be a keresőmezőbe, `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="6914a-124">In the search box, type `Dynamics 365`.</span></span> <span data-ttu-id="6914a-125">Válassza ki a Dynamics 365 eseményindítók listáról **Dynamics 365 – amikor létrejön egy bejegyzés**.</span><span class="sxs-lookup"><span data-stu-id="6914a-125">From the Dynamics 365 triggers list, select **Dynamics 365 – When a record is created**.</span></span>

8.  <span data-ttu-id="6914a-126">Ha jelentkezzen be a Dynamics 365 kéri, tegye meg.</span><span class="sxs-lookup"><span data-stu-id="6914a-126">If you are prompted to sign in to Dynamics 365, do so now.</span></span>

9.  <span data-ttu-id="6914a-127">Az eseményindító részleteit adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="6914a-127">In the trigger details, enter the following information:</span></span>

  * <span data-ttu-id="6914a-128">**Szervezet neve**.</span><span class="sxs-lookup"><span data-stu-id="6914a-128">**Organization Name**.</span></span> <span data-ttu-id="6914a-129">Válassza ki a Dynamics 365 példányt, amelyet a logikai alkalmazás figyelésére.</span><span class="sxs-lookup"><span data-stu-id="6914a-129">Select the Dynamics 365 instance that you want the logic app to listen to.</span></span>

  * <span data-ttu-id="6914a-130">**Entitás neve**.</span><span class="sxs-lookup"><span data-stu-id="6914a-130">**Entity Name**.</span></span> <span data-ttu-id="6914a-131">Jelölje ki a figyelni kívánt entitás.</span><span class="sxs-lookup"><span data-stu-id="6914a-131">Select the entity that you want to listen to.</span></span> <span data-ttu-id="6914a-132">Ez az esemény úgy működik, mint egy eseményindító indítsa el a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6914a-132">This event acts as a trigger to start the logic app.</span></span> 
  <span data-ttu-id="6914a-133">Ebben a bemutatóban **érdeklődők** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6914a-133">In this walkthrough, **Leads** is selected.</span></span>

  * <span data-ttu-id="6914a-134">**Milyen gyakran kívánt elemek kereséséhez?**</span><span class="sxs-lookup"><span data-stu-id="6914a-134">**How often do you want to check for items?**</span></span> <span data-ttu-id="6914a-135">Ezeket az értékeket be, milyen gyakran kérdezze le a logikai alkalmazást az eseményindító kapcsolódó frissítések.</span><span class="sxs-lookup"><span data-stu-id="6914a-135">These values set how often the logic app checks for updates related to the trigger.</span></span> <span data-ttu-id="6914a-136">Az alapértelmezett érték három percenként frissítések kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="6914a-136">The default setting is to check for updates every three minutes.</span></span>

    * <span data-ttu-id="6914a-137">**Gyakoriság**.</span><span class="sxs-lookup"><span data-stu-id="6914a-137">**Frequency**.</span></span> <span data-ttu-id="6914a-138">Válassza ki a másodperc, perc, órák vagy napok.</span><span class="sxs-lookup"><span data-stu-id="6914a-138">Select seconds, minutes, hours, or days.</span></span>

    * <span data-ttu-id="6914a-139">**Időköz**.</span><span class="sxs-lookup"><span data-stu-id="6914a-139">**Interval**.</span></span> <span data-ttu-id="6914a-140">Adja meg másodpercben, perc, óra vagy előtt a következő ellenőrzés átadni kívánt napok számát.</span><span class="sxs-lookup"><span data-stu-id="6914a-140">Enter the number of seconds, minutes, hours, or days that you want to pass before the next check.</span></span>

      ![Logic App eseményindító részletei](./media/connectors-create-api-crmonline/trigger-details.png)

10. <span data-ttu-id="6914a-142">Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6914a-142">Click **New step**, and then click **Add an action**.</span></span>

11. <span data-ttu-id="6914a-143">Írja be a keresőmezőbe, `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="6914a-143">In the search box, type `Dynamics 365`.</span></span> <span data-ttu-id="6914a-144">Válassza a műveletek listájának **Dynamics 365 – új rekord létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6914a-144">From the actions list, select **Dynamics 365 – Create a new record**.</span></span>

12. <span data-ttu-id="6914a-145">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="6914a-145">Enter the following information:</span></span>

    * <span data-ttu-id="6914a-146">**Szervezet neve**.</span><span class="sxs-lookup"><span data-stu-id="6914a-146">**Organization Name**.</span></span> <span data-ttu-id="6914a-147">Válassza ki a Dynamics 365-példányt, amelyen a folyamatot a rekord létrehozására.</span><span class="sxs-lookup"><span data-stu-id="6914a-147">Select the Dynamics 365 instance where you want the flow to create the record.</span></span> 
    <span data-ttu-id="6914a-148">Figyelje meg, hogy ez a példány nem feltétlenül a példányt, amelyen az esemény akkor váltódik ki, a.</span><span class="sxs-lookup"><span data-stu-id="6914a-148">Notice that this instance doesn’t have to be the same instance where the event is triggered from.</span></span>

    * <span data-ttu-id="6914a-149">**Entitás neve**.</span><span class="sxs-lookup"><span data-stu-id="6914a-149">**Entity Name**.</span></span> <span data-ttu-id="6914a-150">Jelölje ki az esemény kiváltásakor rekordot kell létrehozni kívánt entitás.</span><span class="sxs-lookup"><span data-stu-id="6914a-150">Select the entity that you want to create a record when the event is triggered.</span></span> 
    <span data-ttu-id="6914a-151">Ebben a bemutatóban **feladatok** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6914a-151">In this walkthrough, **Tasks** is selected.</span></span>

13. <span data-ttu-id="6914a-152">Kattintson a **tulajdonos** meg.</span><span class="sxs-lookup"><span data-stu-id="6914a-152">Click in the **Subject** box that appears.</span></span> <span data-ttu-id="6914a-153">A dinamikus tartalom megjelenő listában kiválaszthatja azt a mezők valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="6914a-153">From the dynamic content list that appears, you can select either of these fields:</span></span>

    * <span data-ttu-id="6914a-154">**Vezetéknév**.</span><span class="sxs-lookup"><span data-stu-id="6914a-154">**Last Name**.</span></span> <span data-ttu-id="6914a-155">Ez a mező kiválasztása szúr be a tevékenység, a tulajdonos mezőben a vezetéknevet az érdeklődési, a feladat rekord létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6914a-155">Selecting this field inserts the last name for the lead into the Subject field for the task, when the task record is created.</span></span>
    * <span data-ttu-id="6914a-156">**A témakör**.</span><span class="sxs-lookup"><span data-stu-id="6914a-156">**Topic**.</span></span> <span data-ttu-id="6914a-157">A témakör az érdeklődési mező kiválasztása ebben a mezőben szúr be a tevékenység, a tulajdonos mezőben, a feladat rekord létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6914a-157">Selecting this field inserts the Topic field for the lead into the Subject field for the task, when the task record is created.</span></span> 
    <span data-ttu-id="6914a-158">Kattintson a **témakör** azt, hogy a **tulajdonos** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6914a-158">Click **Topic** to add that to the **Subject** box.</span></span>

      ![Logic App hozzon létre új rekord részletei](./media/connectors-create-api-crmonline/create-record-details.png)

14. <span data-ttu-id="6914a-160">A Logic App Designer eszköztáron kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6914a-160">On the Logic App Designer toolbar, click **Save**.</span></span>

    ![Logic App Designer eszköztáron menteni](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. <span data-ttu-id="6914a-162">A logikai alkalmazás elindításához kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="6914a-162">To start the Logic App, click **Run**.</span></span>

    ![Logic App Designer eszköztáron menteni](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. <span data-ttu-id="6914a-164">Most hozzon létre rendszer Dynamics 365 az értékesítés, és tekintse meg a folyamat a művelet!</span><span class="sxs-lookup"><span data-stu-id="6914a-164">Now create a lead record in Dynamics 365 for Sales and see your flow in action!</span></span>

## <a name="set-advanced-options-for-a-logic-app-step"></a><span data-ttu-id="6914a-165">A logic app lépés speciális beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="6914a-165">Set advanced options for a logic app step</span></span>

<span data-ttu-id="6914a-166">Adja meg a logic app lépésben adatok szűrése, kattintson a **speciális beállítások megjelenítése** a lépés, majd adja hozzá egy szűrőt, vagy az order lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="6914a-166">To specify how to filter data in a logic app step, click **Show advanced options** in that step, then add a filter or order by query.</span></span>

<span data-ttu-id="6914a-167">A szűrő lekérdezés segítségével például a fiók neve csak az aktív fiókok és a sorrend beolvasása.</span><span class="sxs-lookup"><span data-stu-id="6914a-167">For example, you can use a filter query to get only active accounts and order by the account name.</span></span> <span data-ttu-id="6914a-168">Ez a feladat végrehajtásához adja meg az OData-szűrő lekérdezés `statuscode eq 1`, és válassza ki **fióknév** a dinamikus tartalom listából.</span><span class="sxs-lookup"><span data-stu-id="6914a-168">To perform this task, enter the OData filter query `statuscode eq 1`, and select **Account Name** from the dynamic content list.</span></span> <span data-ttu-id="6914a-169">További információ: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) és [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span><span class="sxs-lookup"><span data-stu-id="6914a-169">More information: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) and [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span></span>

![Speciális beállítások logikai alkalmazás](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a><span data-ttu-id="6914a-171">Gyakorlati tanácsok a speciális beállításokkal</span><span class="sxs-lookup"><span data-stu-id="6914a-171">Best practices when using advanced options</span></span>

<span data-ttu-id="6914a-172">Érték mező hozzáadásakor meg kell egyeznie a mező típusa, írjon be egy értéket, vagy kiválaszthat egy értéket a dinamikus tartalom listából.</span><span class="sxs-lookup"><span data-stu-id="6914a-172">When you add a value to a field, you must match the field type whether you type a value or select a value from the dynamic content list.</span></span>

<span data-ttu-id="6914a-173">Mező típusa</span><span class="sxs-lookup"><span data-stu-id="6914a-173">Field type</span></span>  |<span data-ttu-id="6914a-174">A használat módja</span><span class="sxs-lookup"><span data-stu-id="6914a-174">How to use</span></span>  |<span data-ttu-id="6914a-175">Hol található</span><span class="sxs-lookup"><span data-stu-id="6914a-175">Where to find</span></span>  |<span data-ttu-id="6914a-176">Név</span><span class="sxs-lookup"><span data-stu-id="6914a-176">Name</span></span>  |<span data-ttu-id="6914a-177">Adattípus</span><span class="sxs-lookup"><span data-stu-id="6914a-177">Data type</span></span>  
---------|---------|---------|---------|---------
<span data-ttu-id="6914a-178">Szövegmező</span><span class="sxs-lookup"><span data-stu-id="6914a-178">Text fields</span></span>|<span data-ttu-id="6914a-179">Szövegmezők egyetlen sor szöveget vagy a dinamikus tartalmat, amely szöveges típusú mező szükséges.</span><span class="sxs-lookup"><span data-stu-id="6914a-179">Text fields require a single line of text or dynamic content that is a text type field.</span></span> <span data-ttu-id="6914a-180">Például a kategória és alkategória mező.</span><span class="sxs-lookup"><span data-stu-id="6914a-180">Examples include the Category and Sub-Category fields.</span></span>|<span data-ttu-id="6914a-181">Beállítások > testreszabások > a rendszer testreszabását > entitások > Feladat > mezők</span><span class="sxs-lookup"><span data-stu-id="6914a-181">Settings > Customizations > Customize the System > Entities > Task > Fields</span></span> |<span data-ttu-id="6914a-182">category</span><span class="sxs-lookup"><span data-stu-id="6914a-182">category</span></span> |<span data-ttu-id="6914a-183">Egysoros szövegmező</span><span class="sxs-lookup"><span data-stu-id="6914a-183">Single Line of Text</span></span>        
<span data-ttu-id="6914a-184">Egész mezők</span><span class="sxs-lookup"><span data-stu-id="6914a-184">Integer fields</span></span> | <span data-ttu-id="6914a-185">Egyes mezők egész szám vagy a dinamikus tartalmat a mezőnek egész típus szükséges.</span><span class="sxs-lookup"><span data-stu-id="6914a-185">Some fields require integer or dynamic content that is an integer type field.</span></span> <span data-ttu-id="6914a-186">Például készültségi szint és időtartama.</span><span class="sxs-lookup"><span data-stu-id="6914a-186">Examples include Percent Complete and Duration.</span></span> |<span data-ttu-id="6914a-187">Beállítások > testreszabások > a rendszer testreszabását > entitások > Feladat > mezők</span><span class="sxs-lookup"><span data-stu-id="6914a-187">Settings > Customizations > Customize the System > Entities > Task > Fields</span></span> |<span data-ttu-id="6914a-188">KészültségiSzint</span><span class="sxs-lookup"><span data-stu-id="6914a-188">percentcomplete</span></span> |<span data-ttu-id="6914a-189">Egész szám</span><span class="sxs-lookup"><span data-stu-id="6914a-189">Whole Number</span></span>         
<span data-ttu-id="6914a-190">Dátummezők</span><span class="sxs-lookup"><span data-stu-id="6914a-190">Date fields</span></span> | <span data-ttu-id="6914a-191">Egyes mezők dátummal hh/nn/éééé formátumban vagy dinamikus tartalmat a dátum típusú mező szükséges.</span><span class="sxs-lookup"><span data-stu-id="6914a-191">Some fields require a date entered in mm/dd/yyyy format or dynamic content that is a date type field.</span></span> <span data-ttu-id="6914a-192">Ilyen például létrehozva, kezdő dátum, Tényleges kezdés utolsó tartsa idő, a tényleges befejezési és a határidő.</span><span class="sxs-lookup"><span data-stu-id="6914a-192">Examples include Created On, Start Date, Actual Start, Last on Hold Time, Actual End, and Due Date.</span></span> | <span data-ttu-id="6914a-193">Beállítások > testreszabások > a rendszer testreszabását > entitások > Feladat > mezők</span><span class="sxs-lookup"><span data-stu-id="6914a-193">Settings > Customizations > Customize the System > Entities > Task > Fields</span></span> |<span data-ttu-id="6914a-194">createdon</span><span class="sxs-lookup"><span data-stu-id="6914a-194">createdon</span></span> |<span data-ttu-id="6914a-195">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6914a-195">Date and Time</span></span>
<span data-ttu-id="6914a-196">Írja be a szükséges mind a rekord azonosítója és a keresési mezők</span><span class="sxs-lookup"><span data-stu-id="6914a-196">Fields that require both a record ID and lookup type</span></span> |<span data-ttu-id="6914a-197">Néhány egy másik entitás bejegyzés hivatkozó mező kötelező, a Rekordazonosító és a keresési típus.</span><span class="sxs-lookup"><span data-stu-id="6914a-197">Some fields that reference another entity record require both the record ID and the lookup type.</span></span> |<span data-ttu-id="6914a-198">Beállítások > testreszabások > a rendszer testreszabását > entitások > fiók > mezők</span><span class="sxs-lookup"><span data-stu-id="6914a-198">Settings > Customizations > Customize the System > Entities > Account > Fields</span></span>  | <span data-ttu-id="6914a-199">accountid</span><span class="sxs-lookup"><span data-stu-id="6914a-199">accountid</span></span>  | <span data-ttu-id="6914a-200">Elsődleges kulcs</span><span class="sxs-lookup"><span data-stu-id="6914a-200">Primary Key</span></span>

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a><span data-ttu-id="6914a-201">További példák a mezőket, amelyeknek szükséges a rekord azonosítója és a keresési írja be.</span><span class="sxs-lookup"><span data-stu-id="6914a-201">More examples of fields that require both a record ID and lookup type</span></span>
<span data-ttu-id="6914a-202">Az előző táblázatban kibővítve, az alábbiakban további példák a mezőket, amelyeknek nem működnek a dinamikus tartalom listán kijelölt értékekkel.</span><span class="sxs-lookup"><span data-stu-id="6914a-202">Expanding on the previous table, here are more examples of fields that don't work with values selected from the dynamic content list.</span></span> <span data-ttu-id="6914a-203">Ehelyett ezeket a mezőket kell mindkét egy Azonosítót és a keresési rekordtípus a mezőkben a powerapps segítségével.</span><span class="sxs-lookup"><span data-stu-id="6914a-203">Instead, these fields require both a record ID and lookup type entered into the fields in PowerApps.</span></span>  
* <span data-ttu-id="6914a-204">Tulajdonos és a tulajdonos típusa.</span><span class="sxs-lookup"><span data-stu-id="6914a-204">Owner and Owner Type.</span></span> <span data-ttu-id="6914a-205">A tulajdonos mezőben kell lennie egy érvényes felhasználó vagy csoport rekord.</span><span class="sxs-lookup"><span data-stu-id="6914a-205">The Owner field must be a valid user or team record ID.</span></span> <span data-ttu-id="6914a-206">A tulajdonos típusúnak kell lennie, vagy **systemusers** vagy **csapatok**.</span><span class="sxs-lookup"><span data-stu-id="6914a-206">The Owner Type must be either **systemusers** or **teams**.</span></span>
* <span data-ttu-id="6914a-207">Ügyfél és az ügyfél típusa.</span><span class="sxs-lookup"><span data-stu-id="6914a-207">Customer and Customer Type.</span></span> <span data-ttu-id="6914a-208">Az ügyfél mező kell egy érvényes fiókot vagy lépjen kapcsolatba a rekordazonosító.</span><span class="sxs-lookup"><span data-stu-id="6914a-208">The Customer field must be a valid account or contact record ID.</span></span> <span data-ttu-id="6914a-209">A tulajdonos típusúnak kell lennie, vagy **fiókok** vagy **névjegyek**.</span><span class="sxs-lookup"><span data-stu-id="6914a-209">The Owner Type must be either **accounts** or **contacts**.</span></span>
* <span data-ttu-id="6914a-210">Valamint típus tekintetében.</span><span class="sxs-lookup"><span data-stu-id="6914a-210">Regarding and Regarding Type.</span></span> <span data-ttu-id="6914a-211">A kapcsolódó elemek mezőt kell egy érvényes Rekordazonosítója, például egy fiók vagy lépjen kapcsolatba a rekordazonosító.</span><span class="sxs-lookup"><span data-stu-id="6914a-211">The Regarding field must be a valid record ID, such as an account or contact record ID.</span></span> <span data-ttu-id="6914a-212">A vonatkozó típusnak kell lennie a Keresés a rekord, például a **fiókok** vagy **névjegyek**.</span><span class="sxs-lookup"><span data-stu-id="6914a-212">The Regarding Type must be the lookup type for the record, such as **accounts** or **contacts**.</span></span>

<span data-ttu-id="6914a-213">Ez a feladat létrehozása művelet példa ad hozzá egy fiók bejegyzést, amely megfelel a hozzáadná a vonatkozó mező a feladat azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6914a-213">The following task creation action example adds an account record that corresponds to the record ID adding it to the regarding field of the task.</span></span>

![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid-type-account.png)

<span data-ttu-id="6914a-215">Ebben a példában is hozzárendeli a feladat egy adott felhasználó az a felhasználói rekord azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="6914a-215">This example also assigns the task to a specific user based on the user's record ID.</span></span>

![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid-type-user.png)

<span data-ttu-id="6914a-217">A Rekordazonosító található, a következő részben: *található a rekord azonosítója*</span><span class="sxs-lookup"><span data-stu-id="6914a-217">To find a record's ID, see the following section: *Find the record ID*</span></span>

## <a name="find-the-record-id"></a><span data-ttu-id="6914a-218">Található a rekord azonosítója</span><span class="sxs-lookup"><span data-stu-id="6914a-218">Find the record ID</span></span>

1. <span data-ttu-id="6914a-219">Nyissa meg a rekord, például egy fiók rekordot.</span><span class="sxs-lookup"><span data-stu-id="6914a-219">Open a record, such as an account record.</span></span>

2. <span data-ttu-id="6914a-220">Kattintson a Műveletek eszköztár **kiugró** ![felbukkanó rekord](./media/connectors-create-api-crmonline/popout-record.png).</span><span class="sxs-lookup"><span data-stu-id="6914a-220">On the actions toolbar, click **Pop Out** ![popout record](./media/connectors-create-api-crmonline/popout-record.png).</span></span>
<span data-ttu-id="6914a-221">Azt is megteheti, a műveletek a teljes URL-címet az alapértelmezett e-mail program történő másolásához kattintson eszköztár **e-MAILT A hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="6914a-221">Alternatively, on the actions toolbar, to copy the full URL into your default email program, click **EMAIL A LINK**.</span></span>

   <span data-ttu-id="6914a-222">A Rekordazonosító Between az URL-címének karakterek kódolása %d 7b és %7 jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6914a-222">The record ID is displayed in between the %7b and %7d encoding characters of the URL.</span></span>

   ![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a><span data-ttu-id="6914a-224">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6914a-224">Troubleshooting</span></span>
<span data-ttu-id="6914a-225">A logikai alkalmazás egy hibás lépés elhárításához az esemény részletes állapotjelzés megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6914a-225">To troubleshoot a failed step in a logic app, view the status details of the event.</span></span>

1. <span data-ttu-id="6914a-226">A **Logic Apps**, válassza ki a Logic Apps alkalmazást, és kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="6914a-226">Under **Logic Apps**, select your logic app, and then click **Overview**.</span></span> 

   <span data-ttu-id="6914a-227">Az összegzési területen látható, és a futási állapotát biztosít a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6914a-227">The Summary area is shown and provides the run status for the logic app.</span></span> 

   ![Logikai alkalmazás futtatási állapot](./media/connectors-create-api-crmonline/tshoot1.png)

2. <span data-ttu-id="6914a-229">Összes sikertelen futtatása kapcsolatos további információk megtekintéséhez kattintson a hibás esemény.</span><span class="sxs-lookup"><span data-stu-id="6914a-229">To view more information about any failed runs, click the failed event.</span></span> <span data-ttu-id="6914a-230">Bontsa ki a hibás lépést, kattintson a lépés.</span><span class="sxs-lookup"><span data-stu-id="6914a-230">To expand a failed step, click that step.</span></span>

   ![Bontsa ki a hibás lépést](./media/connectors-create-api-crmonline/tshoot2.png)

   <span data-ttu-id="6914a-232">A lépés részletei jelennek meg, és segíthet a hiba okának elhárítása.</span><span class="sxs-lookup"><span data-stu-id="6914a-232">The step details appear and can help troubleshoot the cause of the failure.</span></span>

   ![Nem sikerült lépés részletei](./media/connectors-create-api-crmonline/tshoot3.png)

<span data-ttu-id="6914a-234">A logic apps hibaelhárítással kapcsolatos további információkért lásd: [logic app hibák diagnosztizálása](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="6914a-234">For more information about troubleshooting logic apps, see [Diagnosing logic app failures](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="6914a-235">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="6914a-235">Connector-specific details</span></span>

<span data-ttu-id="6914a-236">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/crm/).</span><span class="sxs-lookup"><span data-stu-id="6914a-236">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/crm/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6914a-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6914a-237">Next steps</span></span>
<span data-ttu-id="6914a-238">Az egyéb rendelkezésre álló összekötők Logic Apps, megismerkedhet a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="6914a-238">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
