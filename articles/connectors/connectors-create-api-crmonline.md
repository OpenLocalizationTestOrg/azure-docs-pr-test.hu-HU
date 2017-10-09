---
title: aaaConnect tooDynamics 365 (online) az Azure Logic Apps |} Microsoft Docs
description: "Munkafolyamatokat logic app, Dynamics 365 (online) entitások hello hello Dynamics 365-összekötő által nyújtott API használatával kezelhető"
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
ms.openlocfilehash: 183d7a6b8e5d2c0eecc70da0da3806e06c382df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toodynamics-365-from-logic-app-workflows"></a><span data-ttu-id="0f81d-103">TooDynamics 365 csatlakoztatja a logic app munkafolyamatok</span><span class="sxs-lookup"><span data-stu-id="0f81d-103">Connect tooDynamics 365 from logic app workflows</span></span>

<span data-ttu-id="0f81d-104">A Logic Apps tooDynamics 365 (online) csatlakozhat, és hozzon létre hasznos üzleti flow-rekordok létrehozása, elemek frissítését vagy a rekordok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0f81d-104">With Logic Apps, you can connect tooDynamics 365 (online) and create useful business flows that create records, update items, or return a list of records.</span></span> <span data-ttu-id="0f81d-105">Hello Dynamics 365-összekötővel a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="0f81d-105">With hello Dynamics 365 connector, you can:</span></span>

* <span data-ttu-id="0f81d-106">A Dynamics 365 (online) kap hello adatok alapján üzleti folyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0f81d-106">Build your business flow based on hello data you get from Dynamics 365 (online).</span></span>
* <span data-ttu-id="0f81d-107">Amely válaszol, és ellenőrizze az egyéb műveletek lehetőség hello kimeneti műveleteket használni.</span><span class="sxs-lookup"><span data-stu-id="0f81d-107">Use actions that get a response and then make hello output available for other actions.</span></span> <span data-ttu-id="0f81d-108">Például frissítésekor elem Dynamics 365 (online), az Office 365-tel e-mailt küldhet.</span><span class="sxs-lookup"><span data-stu-id="0f81d-108">For example, when an item is updated in Dynamics 365 (online), you can send an email using Office 365.</span></span>

<span data-ttu-id="0f81d-109">Ez a témakör bemutatja, hogyan toocreate logikai alkalmazás, amely feladatot hoz létre a Dynamics 365 amikor létrejön egy új vezető Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="0f81d-109">This topic shows you how toocreate a logic app that creates a task in Dynamics 365 whenever a new lead is created in Dynamics 365.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f81d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0f81d-110">Prerequisites</span></span>
* <span data-ttu-id="0f81d-111">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="0f81d-111">An Azure account.</span></span>
* <span data-ttu-id="0f81d-112">Dynamics 365 (online) fiók.</span><span class="sxs-lookup"><span data-stu-id="0f81d-112">A Dynamics 365 (online) account.</span></span>

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a><span data-ttu-id="0f81d-113">Hozzon létre egy feladatot, amikor egy új vezető Dynamics 365 hoz létre</span><span class="sxs-lookup"><span data-stu-id="0f81d-113">Create a task when a new lead is created in Dynamics 365</span></span>

1.  <span data-ttu-id="0f81d-114">[Jelentkezzen be tooAzure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0f81d-114">[Sign in tooAzure](https://portal.azure.com).</span></span>

2.  <span data-ttu-id="0f81d-115">Hello Azure keresési mezőbe, írja be a `Logic apps`, és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="0f81d-115">In hello Azure search box, type `Logic apps`, and press ENTER.</span></span>

      ![Logic Apps alkalmazások keresése](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  <span data-ttu-id="0f81d-117">A **a Logic apps**, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-117">Under **Logic apps**, click **Add**.</span></span>

      ![LogicApp hozzáadása](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  <span data-ttu-id="0f81d-119">toocreate hello logic app, teljes hello **neve**, **előfizetés**, **erőforráscsoport**, és **hely** mezők, és kattintson a  **Hozzon létre**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-119">toocreate hello logic app, complete hello **Name**, **Subscription**, **Resource Group**, and **Location** fields, and then click **Create**.</span></span>

5.  <span data-ttu-id="0f81d-120">Válassza ki az új logikai alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="0f81d-120">Select hello new logic app.</span></span> <span data-ttu-id="0f81d-121">Hello érkezésekor **sikeres telepítés** értesítést, kattintson a **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-121">When you receive hello **Deployment Succeeded** notification, click **Refresh**.</span></span>

6.  <span data-ttu-id="0f81d-122">A **Fejlesztőeszközök**, kattintson a **Logic App Designer**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-122">Under **Development Tools**, click **Logic App Designer**.</span></span> <span data-ttu-id="0f81d-123">Hello listájában kattintson **üres logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-123">In hello template list, click **Blank Logic App**.</span></span>

7.  <span data-ttu-id="0f81d-124">Hello keresési mezőbe, írja be a `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="0f81d-124">In hello search box, type `Dynamics 365`.</span></span> <span data-ttu-id="0f81d-125">A hello Dynamics 365 elindítja a listán, válassza a **Dynamics 365 – amikor létrejön egy bejegyzés**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-125">From hello Dynamics 365 triggers list, select **Dynamics 365 – When a record is created**.</span></span>

8.  <span data-ttu-id="0f81d-126">Ha a tooDynamics 365 felszólító toosign, tegye meg.</span><span class="sxs-lookup"><span data-stu-id="0f81d-126">If you are prompted toosign in tooDynamics 365, do so now.</span></span>

9.  <span data-ttu-id="0f81d-127">Hello eseményindító részleteit adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="0f81d-127">In hello trigger details, enter hello following information:</span></span>

  * <span data-ttu-id="0f81d-128">**Szervezet neve**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-128">**Organization Name**.</span></span> <span data-ttu-id="0f81d-129">Válassza ki, amelyet hello logic app toolisten való hello Dynamics 365 példányt.</span><span class="sxs-lookup"><span data-stu-id="0f81d-129">Select hello Dynamics 365 instance that you want hello logic app toolisten to.</span></span>

  * <span data-ttu-id="0f81d-130">**Entitás neve**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-130">**Entity Name**.</span></span> <span data-ttu-id="0f81d-131">Válassza ki, amelyet a toolisten hello entitást.</span><span class="sxs-lookup"><span data-stu-id="0f81d-131">Select hello entity that you want toolisten to.</span></span> <span data-ttu-id="0f81d-132">Ez az esemény úgy működik, mint egy eseményindító toostart hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0f81d-132">This event acts as a trigger toostart hello logic app.</span></span> 
  <span data-ttu-id="0f81d-133">Ebben a bemutatóban **érdeklődők** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0f81d-133">In this walkthrough, **Leads** is selected.</span></span>

  * <span data-ttu-id="0f81d-134">**Milyen gyakran szeretné toocheck elemek?**</span><span class="sxs-lookup"><span data-stu-id="0f81d-134">**How often do you want toocheck for items?**</span></span> <span data-ttu-id="0f81d-135">Ezek az értékek gyakoriságának beállítása hello logikai alkalmazás keres frissítéseket kapcsolódó toohello eseményindító.</span><span class="sxs-lookup"><span data-stu-id="0f81d-135">These values set how often hello logic app checks for updates related toohello trigger.</span></span> <span data-ttu-id="0f81d-136">hello alapértelmezett érték három percenként frissítések toocheck.</span><span class="sxs-lookup"><span data-stu-id="0f81d-136">hello default setting is toocheck for updates every three minutes.</span></span>

    * <span data-ttu-id="0f81d-137">**Gyakoriság**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-137">**Frequency**.</span></span> <span data-ttu-id="0f81d-138">Válassza ki a másodperc, perc, órák vagy napok.</span><span class="sxs-lookup"><span data-stu-id="0f81d-138">Select seconds, minutes, hours, or days.</span></span>

    * <span data-ttu-id="0f81d-139">**Időköz**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-139">**Interval**.</span></span> <span data-ttu-id="0f81d-140">Adja meg másodpercben, perc, óra hello számát, vagy Ezután ellenőrizze, hogy szeretné-e előtt hello toopass nap.</span><span class="sxs-lookup"><span data-stu-id="0f81d-140">Enter hello number of seconds, minutes, hours, or days that you want toopass before hello next check.</span></span>

      ![Logic App eseményindító részletei](./media/connectors-create-api-crmonline/trigger-details.png)

10. <span data-ttu-id="0f81d-142">Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-142">Click **New step**, and then click **Add an action**.</span></span>

11. <span data-ttu-id="0f81d-143">Hello keresési mezőbe, írja be a `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="0f81d-143">In hello search box, type `Dynamics 365`.</span></span> <span data-ttu-id="0f81d-144">Hello műveletek listájából válassza ki **Dynamics 365 – új rekord létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-144">From hello actions list, select **Dynamics 365 – Create a new record**.</span></span>

12. <span data-ttu-id="0f81d-145">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="0f81d-145">Enter hello following information:</span></span>

    * <span data-ttu-id="0f81d-146">**Szervezet neve**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-146">**Organization Name**.</span></span> <span data-ttu-id="0f81d-147">Válassza ki a hello Dynamics 365-példányt, ahol hello folyamat toocreate hello rekord.</span><span class="sxs-lookup"><span data-stu-id="0f81d-147">Select hello Dynamics 365 instance where you want hello flow toocreate hello record.</span></span> 
    <span data-ttu-id="0f81d-148">Figyelje meg, hogy ez a példány nem rendelkezik azonos hello toobe ahol hello esemény akkor váltódik ki, a példány.</span><span class="sxs-lookup"><span data-stu-id="0f81d-148">Notice that this instance doesn’t have toobe hello same instance where hello event is triggered from.</span></span>

    * <span data-ttu-id="0f81d-149">**Entitás neve**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-149">**Entity Name**.</span></span> <span data-ttu-id="0f81d-150">Válassza ki a megjeleníteni kívánt toocreate rekord hello esemény kiváltásakor hello entitást.</span><span class="sxs-lookup"><span data-stu-id="0f81d-150">Select hello entity that you want toocreate a record when hello event is triggered.</span></span> 
    <span data-ttu-id="0f81d-151">Ebben a bemutatóban **feladatok** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0f81d-151">In this walkthrough, **Tasks** is selected.</span></span>

13. <span data-ttu-id="0f81d-152">Kattintson a hello **tulajdonos** meg.</span><span class="sxs-lookup"><span data-stu-id="0f81d-152">Click in hello **Subject** box that appears.</span></span> <span data-ttu-id="0f81d-153">Hello dinamikus tartalom megjelenő listában kiválaszthatja a mezők valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="0f81d-153">From hello dynamic content list that appears, you can select either of these fields:</span></span>

    * <span data-ttu-id="0f81d-154">**Vezetéknév**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-154">**Last Name**.</span></span> <span data-ttu-id="0f81d-155">Hello vezetéknevet hello átfutási kiválasztása ebben a mezőben szúr be hello Tárgy mező hello feladathoz, hello feladat rekord létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0f81d-155">Selecting this field inserts hello last name for hello lead into hello Subject field for hello task, when hello task record is created.</span></span>
    * <span data-ttu-id="0f81d-156">**A témakör**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-156">**Topic**.</span></span> <span data-ttu-id="0f81d-157">A mező beszúrása hello témakör mezője hello átfutási hello tulajdonos mezőbe hello tevékenység kiválasztása esetén hello feladat rekord létrehozása esetén.</span><span class="sxs-lookup"><span data-stu-id="0f81d-157">Selecting this field inserts hello Topic field for hello lead into hello Subject field for hello task, when hello task record is created.</span></span> 
    <span data-ttu-id="0f81d-158">Kattintson a **témakör** tooadd adott toohello **tulajdonos** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0f81d-158">Click **Topic** tooadd that toohello **Subject** box.</span></span>

      ![Logic App hozzon létre új rekord részletei](./media/connectors-create-api-crmonline/create-record-details.png)

14. <span data-ttu-id="0f81d-160">Hello Logic App Designer eszköztáron kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-160">On hello Logic App Designer toolbar, click **Save**.</span></span>

    ![Logic App Designer eszköztáron menteni](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. <span data-ttu-id="0f81d-162">toostart hello Logic App, kattintson a **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-162">toostart hello Logic App, click **Run**.</span></span>

    ![Logic App Designer eszköztáron menteni](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. <span data-ttu-id="0f81d-164">Most hozzon létre rendszer Dynamics 365 az értékesítés, és tekintse meg a folyamat a művelet!</span><span class="sxs-lookup"><span data-stu-id="0f81d-164">Now create a lead record in Dynamics 365 for Sales and see your flow in action!</span></span>

## <a name="set-advanced-options-for-a-logic-app-step"></a><span data-ttu-id="0f81d-165">A logic app lépés speciális beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="0f81d-165">Set advanced options for a logic app step</span></span>

<span data-ttu-id="0f81d-166">Hogyan toofilter adatok egy logic app lépésben kattintson toospecify **speciális beállítások megjelenítése** a lépés, majd adja hozzá egy szűrőt, vagy az order lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="0f81d-166">toospecify how toofilter data in a logic app step, click **Show advanced options** in that step, then add a filter or order by query.</span></span>

<span data-ttu-id="0f81d-167">Például egy szűrő lekérdezés tooget csak az aktív fiókok használata, és a rendezés hello fiók neve.</span><span class="sxs-lookup"><span data-stu-id="0f81d-167">For example, you can use a filter query tooget only active accounts and order by hello account name.</span></span> <span data-ttu-id="0f81d-168">tooperform ennek a feladatnak, adja meg az OData-szűrő lekérdezés hello `statuscode eq 1`, és válassza ki **fióknév** hello dinamikus tartalom listából.</span><span class="sxs-lookup"><span data-stu-id="0f81d-168">tooperform this task, enter hello OData filter query `statuscode eq 1`, and select **Account Name** from hello dynamic content list.</span></span> <span data-ttu-id="0f81d-169">További információ: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) és [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span><span class="sxs-lookup"><span data-stu-id="0f81d-169">More information: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) and [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span></span>

![Speciális beállítások logikai alkalmazás](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a><span data-ttu-id="0f81d-171">Gyakorlati tanácsok a speciális beállításokkal</span><span class="sxs-lookup"><span data-stu-id="0f81d-171">Best practices when using advanced options</span></span>

<span data-ttu-id="0f81d-172">Érték tooa mező hozzáadásakor meg kell egyeznie hello mező típusa, írjon be egy értéket, vagy kiválaszthat egy értéket a hello dinamikus tartalom listából.</span><span class="sxs-lookup"><span data-stu-id="0f81d-172">When you add a value tooa field, you must match hello field type whether you type a value or select a value from hello dynamic content list.</span></span>

<span data-ttu-id="0f81d-173">Mező típusa</span><span class="sxs-lookup"><span data-stu-id="0f81d-173">Field type</span></span>  |<span data-ttu-id="0f81d-174">Hogyan toouse</span><span class="sxs-lookup"><span data-stu-id="0f81d-174">How toouse</span></span>  |<span data-ttu-id="0f81d-175">Ha toofind</span><span class="sxs-lookup"><span data-stu-id="0f81d-175">Where toofind</span></span>  |<span data-ttu-id="0f81d-176">Név</span><span class="sxs-lookup"><span data-stu-id="0f81d-176">Name</span></span>  |<span data-ttu-id="0f81d-177">Adattípus</span><span class="sxs-lookup"><span data-stu-id="0f81d-177">Data type</span></span>  
---------|---------|---------|---------|---------
<span data-ttu-id="0f81d-178">Szövegmező</span><span class="sxs-lookup"><span data-stu-id="0f81d-178">Text fields</span></span>|<span data-ttu-id="0f81d-179">Szövegmezők egyetlen sor szöveget vagy a dinamikus tartalmat, amely szöveges típusú mező szükséges.</span><span class="sxs-lookup"><span data-stu-id="0f81d-179">Text fields require a single line of text or dynamic content that is a text type field.</span></span> <span data-ttu-id="0f81d-180">Például hello kategória és alkategória mezőket.</span><span class="sxs-lookup"><span data-stu-id="0f81d-180">Examples include hello Category and Sub-Category fields.</span></span>|<span data-ttu-id="0f81d-181">Beállítások > testreszabások > Testreszabás hello rendszer > entitások > Feladat > mezők</span><span class="sxs-lookup"><span data-stu-id="0f81d-181">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="0f81d-182">category</span><span class="sxs-lookup"><span data-stu-id="0f81d-182">category</span></span> |<span data-ttu-id="0f81d-183">Egysoros szövegmező</span><span class="sxs-lookup"><span data-stu-id="0f81d-183">Single Line of Text</span></span>        
<span data-ttu-id="0f81d-184">Egész mezők</span><span class="sxs-lookup"><span data-stu-id="0f81d-184">Integer fields</span></span> | <span data-ttu-id="0f81d-185">Egyes mezők egész szám vagy a dinamikus tartalmat a mezőnek egész típus szükséges.</span><span class="sxs-lookup"><span data-stu-id="0f81d-185">Some fields require integer or dynamic content that is an integer type field.</span></span> <span data-ttu-id="0f81d-186">Például készültségi szint és időtartama.</span><span class="sxs-lookup"><span data-stu-id="0f81d-186">Examples include Percent Complete and Duration.</span></span> |<span data-ttu-id="0f81d-187">Beállítások > testreszabások > Testreszabás hello rendszer > entitások > Feladat > mezők</span><span class="sxs-lookup"><span data-stu-id="0f81d-187">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="0f81d-188">KészültségiSzint</span><span class="sxs-lookup"><span data-stu-id="0f81d-188">percentcomplete</span></span> |<span data-ttu-id="0f81d-189">Egész szám</span><span class="sxs-lookup"><span data-stu-id="0f81d-189">Whole Number</span></span>         
<span data-ttu-id="0f81d-190">Dátummezők</span><span class="sxs-lookup"><span data-stu-id="0f81d-190">Date fields</span></span> | <span data-ttu-id="0f81d-191">Egyes mezők dátummal hh/nn/éééé formátumban vagy dinamikus tartalmat a dátum típusú mező szükséges.</span><span class="sxs-lookup"><span data-stu-id="0f81d-191">Some fields require a date entered in mm/dd/yyyy format or dynamic content that is a date type field.</span></span> <span data-ttu-id="0f81d-192">Ilyen például létrehozva, kezdő dátum, Tényleges kezdés utolsó tartsa idő, a tényleges befejezési és a határidő.</span><span class="sxs-lookup"><span data-stu-id="0f81d-192">Examples include Created On, Start Date, Actual Start, Last on Hold Time, Actual End, and Due Date.</span></span> | <span data-ttu-id="0f81d-193">Beállítások > testreszabások > Testreszabás hello rendszer > entitások > Feladat > mezők</span><span class="sxs-lookup"><span data-stu-id="0f81d-193">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="0f81d-194">createdon</span><span class="sxs-lookup"><span data-stu-id="0f81d-194">createdon</span></span> |<span data-ttu-id="0f81d-195">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="0f81d-195">Date and Time</span></span>
<span data-ttu-id="0f81d-196">Írja be a szükséges mind a rekord azonosítója és a keresési mezők</span><span class="sxs-lookup"><span data-stu-id="0f81d-196">Fields that require both a record ID and lookup type</span></span> |<span data-ttu-id="0f81d-197">Néhány mező egy másik entitás bejegyzés hivatkozó kötelező a hello Rekordazonosítója és hello keresési típus.</span><span class="sxs-lookup"><span data-stu-id="0f81d-197">Some fields that reference another entity record require both hello record ID and hello lookup type.</span></span> |<span data-ttu-id="0f81d-198">Beállítások > testreszabások > Testreszabás hello rendszer > entitások > fiók > mezők</span><span class="sxs-lookup"><span data-stu-id="0f81d-198">Settings > Customizations > Customize hello System > Entities > Account > Fields</span></span>  | <span data-ttu-id="0f81d-199">accountid</span><span class="sxs-lookup"><span data-stu-id="0f81d-199">accountid</span></span>  | <span data-ttu-id="0f81d-200">Elsődleges kulcs</span><span class="sxs-lookup"><span data-stu-id="0f81d-200">Primary Key</span></span>

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a><span data-ttu-id="0f81d-201">További példák a mezőket, amelyeknek szükséges a rekord azonosítója és a keresési írja be.</span><span class="sxs-lookup"><span data-stu-id="0f81d-201">More examples of fields that require both a record ID and lookup type</span></span>
<span data-ttu-id="0f81d-202">Bővíteni hello előző táblán, az alábbiakban további példák a mezőket, amelyeknek hello dinamikus tartalom listában kijelölt értékek nem működik.</span><span class="sxs-lookup"><span data-stu-id="0f81d-202">Expanding on hello previous table, here are more examples of fields that don't work with values selected from hello dynamic content list.</span></span> <span data-ttu-id="0f81d-203">Ehelyett ezeket a mezőket kell mindkét egy rekord azonosítója és a keresési típusú hello mezőkbe a powerapps segítségével.</span><span class="sxs-lookup"><span data-stu-id="0f81d-203">Instead, these fields require both a record ID and lookup type entered into hello fields in PowerApps.</span></span>  
* <span data-ttu-id="0f81d-204">Tulajdonos és a tulajdonos típusa.</span><span class="sxs-lookup"><span data-stu-id="0f81d-204">Owner and Owner Type.</span></span> <span data-ttu-id="0f81d-205">hello tulajdonos írjon be egy érvényes felhasználó vagy csoport rekord.</span><span class="sxs-lookup"><span data-stu-id="0f81d-205">hello Owner field must be a valid user or team record ID.</span></span> <span data-ttu-id="0f81d-206">hello Tulajdonostípusa kell lennie, vagy **systemusers** vagy **csapatok**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-206">hello Owner Type must be either **systemusers** or **teams**.</span></span>
* <span data-ttu-id="0f81d-207">Ügyfél és az ügyfél típusa.</span><span class="sxs-lookup"><span data-stu-id="0f81d-207">Customer and Customer Type.</span></span> <span data-ttu-id="0f81d-208">hello ügyfél mező kell egy érvényes fiókot vagy lépjen kapcsolatba a rekordazonosító.</span><span class="sxs-lookup"><span data-stu-id="0f81d-208">hello Customer field must be a valid account or contact record ID.</span></span> <span data-ttu-id="0f81d-209">hello Tulajdonostípusa kell lennie, vagy **fiókok** vagy **névjegyek**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-209">hello Owner Type must be either **accounts** or **contacts**.</span></span>
* <span data-ttu-id="0f81d-210">Valamint típus tekintetében.</span><span class="sxs-lookup"><span data-stu-id="0f81d-210">Regarding and Regarding Type.</span></span> <span data-ttu-id="0f81d-211">hello vonatkozó mező kell érvényes Rekordazonosítója, például egy fiók vagy lépjen kapcsolatba a rekordazonosító.</span><span class="sxs-lookup"><span data-stu-id="0f81d-211">hello Regarding field must be a valid record ID, such as an account or contact record ID.</span></span> <span data-ttu-id="0f81d-212">hello vonatkozó típusnak kell lennie hello keresési hello rekord, például a **fiókok** vagy **névjegyek**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-212">hello Regarding Type must be hello lookup type for hello record, such as **accounts** or **contacts**.</span></span>

<span data-ttu-id="0f81d-213">hello következő feladat létrehozása művelet példakóddal felveheti egy fiók bejegyzést, amely megfelel a vonatkozó hello feladat mező toohello hozzáadná toohello Rekordazonosítója.</span><span class="sxs-lookup"><span data-stu-id="0f81d-213">hello following task creation action example adds an account record that corresponds toohello record ID adding it toohello regarding field of hello task.</span></span>

![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid-type-account.png)

<span data-ttu-id="0f81d-215">Ebben a példában is rendel hello feladat tooa adott felhasználó az hello felhasználói rekord azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="0f81d-215">This example also assigns hello task tooa specific user based on hello user's record ID.</span></span>

![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid-type-user.png)

<span data-ttu-id="0f81d-217">egy olyan rekordot toofind tartozó azonosító, tekintse meg a következő szakasz hello: *található hello rekord azonosítója*</span><span class="sxs-lookup"><span data-stu-id="0f81d-217">toofind a record's ID, see hello following section: *Find hello record ID*</span></span>

## <a name="find-hello-record-id"></a><span data-ttu-id="0f81d-218">Található hello rekord azonosítója</span><span class="sxs-lookup"><span data-stu-id="0f81d-218">Find hello record ID</span></span>

1. <span data-ttu-id="0f81d-219">Nyissa meg a rekord, például egy fiók rekordot.</span><span class="sxs-lookup"><span data-stu-id="0f81d-219">Open a record, such as an account record.</span></span>

2. <span data-ttu-id="0f81d-220">Hello műveletek eszköztáron kattintson **kiugró** ![felbukkanó rekord](./media/connectors-create-api-crmonline/popout-record.png).</span><span class="sxs-lookup"><span data-stu-id="0f81d-220">On hello actions toolbar, click **Pop Out** ![popout record](./media/connectors-create-api-crmonline/popout-record.png).</span></span>
<span data-ttu-id="0f81d-221">Másik lehetőségként kattintson hello műveletek eszköztár toocopy hello teljes URL-címet az alapértelmezett e-mail programba **e-MAILT A hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-221">Alternatively, on hello actions toolbar, toocopy hello full URL into your default email program, click **EMAIL A LINK**.</span></span>

   <span data-ttu-id="0f81d-222">hello Rekordazonosítója Between hello 7b és %7 %d karakter hello URL-kódolást jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0f81d-222">hello record ID is displayed in between hello %7b and %7d encoding characters of hello URL.</span></span>

   ![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a><span data-ttu-id="0f81d-224">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="0f81d-224">Troubleshooting</span></span>
<span data-ttu-id="0f81d-225">tootroubleshoot egy hibás lépés a logikai alkalmazás, hello hello esemény állapot részleteinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="0f81d-225">tootroubleshoot a failed step in a logic app, view hello status details of hello event.</span></span>

1. <span data-ttu-id="0f81d-226">A **Logic Apps**, válassza ki a Logic Apps alkalmazást, és kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="0f81d-226">Under **Logic Apps**, select your logic app, and then click **Overview**.</span></span> 

   <span data-ttu-id="0f81d-227">hello összegzési területen látható, és futtassa hello állapot biztosít hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0f81d-227">hello Summary area is shown and provides hello run status for hello logic app.</span></span> 

   ![Logikai alkalmazás futtatási állapot](./media/connectors-create-api-crmonline/tshoot1.png)

2. <span data-ttu-id="0f81d-229">tooview további információ a bármely sikertelen fut, kattintson a sikertelen hello esemény.</span><span class="sxs-lookup"><span data-stu-id="0f81d-229">tooview more information about any failed runs, click hello failed event.</span></span> <span data-ttu-id="0f81d-230">tooexpand egy hibás lépés, kattintson az adott lépésre.</span><span class="sxs-lookup"><span data-stu-id="0f81d-230">tooexpand a failed step, click that step.</span></span>

   ![Bontsa ki a hibás lépést](./media/connectors-create-api-crmonline/tshoot2.png)

   <span data-ttu-id="0f81d-232">hello lépés részletei jelennek meg, és segít a hello hello hiba okának elhárítása.</span><span class="sxs-lookup"><span data-stu-id="0f81d-232">hello step details appear and can help troubleshoot hello cause of hello failure.</span></span>

   ![Nem sikerült lépés részletei](./media/connectors-create-api-crmonline/tshoot3.png)

<span data-ttu-id="0f81d-234">A logic apps hibaelhárítással kapcsolatos további információkért lásd: [logic app hibák diagnosztizálása](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="0f81d-234">For more information about troubleshooting logic apps, see [Diagnosing logic app failures](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="0f81d-235">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="0f81d-235">Connector-specific details</span></span>

<span data-ttu-id="0f81d-236">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/crm/).</span><span class="sxs-lookup"><span data-stu-id="0f81d-236">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/crm/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0f81d-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f81d-237">Next steps</span></span>
<span data-ttu-id="0f81d-238">Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="0f81d-238">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
