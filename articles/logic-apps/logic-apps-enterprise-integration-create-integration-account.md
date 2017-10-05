---
title: "Hozzon létre, csatolja, törlése vagy egy integrációs fiók áthelyezése az Azure logic apps |} Microsoft Docs"
description: "Integráció-fiók létrehozása, és csatolja a logic Apps alkalmazások"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 716e7b5bab8725dea0fd2b760d0e46e8e892c5b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-an-integration-account"></a><span data-ttu-id="a534c-103">Mi az az integráció fiókkal?</span><span class="sxs-lookup"><span data-stu-id="a534c-103">What is an integration account?</span></span>

<span data-ttu-id="a534c-104">Integráció fiók lehetővé teszi, hogy vállalati integrációs alkalmazások összetevők, beleértve a sémák, térképeket, tanúsítványok, partnerek és megállapodások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="a534c-104">An integration account allows enterprise integration apps to manage artifacts, including schemas, maps, certificates, partners and agreements.</span></span> <span data-ttu-id="a534c-105">Minden integrációs alkalmazást hoz létre egy integrációs fiók használatával hozzáférni a sémák, maps, tanúsítványokat, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="a534c-105">Any integration app you create uses an integration account to access these schemas, maps, certificates, and so on.</span></span>

## <a name="create-an-integration-account"></a><span data-ttu-id="a534c-106">Integráció-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a534c-106">Create an integration account</span></span>

1.  <span data-ttu-id="a534c-107">Jelentkezzen be az [Azure Portalra](http://portal.azure.com "Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="a534c-107">Sign in to the [Azure portal](http://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="a534c-108">A bal oldali menüben válassza ki a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="a534c-108">From the left menu, select **More services**.</span></span>

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="a534c-110">A keresőmezőbe írja be a "integrációt" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="a534c-110">In the search box, type "integration" for your filter.</span></span> <span data-ttu-id="a534c-111">Az eredmények listájában válassza **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="a534c-111">In the results list, select **Integration Accounts**.</span></span>

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="a534c-113">Válassza ki a lap tetején **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a534c-113">At the top of the page, choose **Add**.</span></span>

    ![Válasszon hozzáadása](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. <span data-ttu-id="a534c-115">Az integráció fiók neve, és válassza ki a használni kívánt Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a534c-115">Name your integration account and select the Azure subscription that you want to use.</span></span> <span data-ttu-id="a534c-116">Létrehozhat egy új **erőforráscsoport** vagy válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a534c-116">You can either create a new **Resource group** or select an existing resource group.</span></span> <span data-ttu-id="a534c-117">Válassza ki a **hely** integrációs fiókja üzemeltetésére és egy **árképzési szintjében**.</span><span class="sxs-lookup"><span data-stu-id="a534c-117">Then select a **Location** for hosting your integration account and a **Pricing Tier**.</span></span> 

    <span data-ttu-id="a534c-118">Ha elkészült, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a534c-118">When you're ready, choose **Create**.</span></span>

    ![Részletek a integrációs fiók megadása](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    <span data-ttu-id="a534c-120">Azure látja el a kijelölt helyre, amely 1 percen belül kell végrehajtani a integrációs fiókba.</span><span class="sxs-lookup"><span data-stu-id="a534c-120">Azure provisions your integration account  in the selected location, which should complete within 1 minute.</span></span>

5. <span data-ttu-id="a534c-121">Frissítse az oldalt.</span><span class="sxs-lookup"><span data-stu-id="a534c-121">Refresh the page.</span></span> <span data-ttu-id="a534c-122">Megjelenik az új integrációs fiókját.</span><span class="sxs-lookup"><span data-stu-id="a534c-122">You see your new integration account listed.</span></span>

    ![Új integrációs fiókja jelenik meg.](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

<span data-ttu-id="a534c-124">A következő fiókját az integráció a Logic Apps alkalmazást létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a534c-124">Next, link the integration account that you created to your logic app.</span></span> 

## <a name="link-an-integration-account-to-a-logic-app"></a><span data-ttu-id="a534c-125">Az integráció-fiók összekötése a logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a534c-125">Link an integration account to a logic app</span></span>

<span data-ttu-id="a534c-126">A hozzáférést a logic apps maps, sémákat, szerződések és más összetevők, az integráció fiókban, a Logic Apps alkalmazást az integráció-fiók összekötése.</span><span class="sxs-lookup"><span data-stu-id="a534c-126">To give your logic apps access to maps, schemas, agreements, and other artifacts in your integration account, link the integration account to your logic app.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="a534c-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a534c-127">Prerequisites</span></span>

* <span data-ttu-id="a534c-128">Integráció fiók</span><span class="sxs-lookup"><span data-stu-id="a534c-128">An integration account</span></span>
* <span data-ttu-id="a534c-129">A logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a534c-129">A logic app</span></span>

> [!NOTE] 
> <span data-ttu-id="a534c-130">Győződjön meg arról, hogy az integrációs fiók és a logic app szerepelnek a *azonos Azure-beli hely* megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="a534c-130">Make sure your integration account and logic app are in the *same Azure location* before you begin.</span></span>


1. <span data-ttu-id="a534c-131">Az Azure portálon válassza ki a Logic Apps alkalmazást, és ellenőrizze a logikai alkalmazás helyét.</span><span class="sxs-lookup"><span data-stu-id="a534c-131">In the Azure portal, select your logic app, and check your logic app's location.</span></span>

    ![Válassza ki a Logic Apps alkalmazást, a hely ellenőrzése](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. <span data-ttu-id="a534c-133">A **beállítások**, jelölje be **integrációs fiók**.</span><span class="sxs-lookup"><span data-stu-id="a534c-133">Under **Settings**, select **Integration Account**.</span></span>

    ![Válassza ki a "Integrációs fiók"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. <span data-ttu-id="a534c-135">Az a **válassza ki a integrációs fiókot** listára, válassza ki a integrációs fióknevet, amelyet a logikai alkalmazás csatolása.</span><span class="sxs-lookup"><span data-stu-id="a534c-135">From the **Select an Integration account** list, select the integration account you want to link to your logic app.</span></span> <span data-ttu-id="a534c-136">Linking befejezéséhez válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="a534c-136">To finish linking, choose **Save**.</span></span>

    ![Válassza ki a integrációs fiókját](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    <span data-ttu-id="a534c-138">Bemutatja az integráció, fiók van összekapcsolva a Logic Apps alkalmazást, és, hogy az integrációs-fiókban lévő összes összetevők a logikai alkalmazás már elérhetők értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="a534c-138">You get a notification that shows your integration account is linked to your logic app,  and that all artifacts in your integration account are now available to your logic app.</span></span>

    ![A Logic Apps alkalmazást integrációs fiókja van csatolva.](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

<span data-ttu-id="a534c-140">Most, hogy fiókja integráció a Logic Apps alkalmazást kapcsolódik, a B2B összekötők a logic apps segítségével is.</span><span class="sxs-lookup"><span data-stu-id="a534c-140">Now that your integration account is linked to your logic app, you can use the B2B connectors in your logic apps.</span></span> <span data-ttu-id="a534c-141">Néhány gyakori B2B összekötők XML-érvényesítés, és egybesimított fájl kódolási/dekódolási tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a534c-141">Some common B2B connectors include XML validation and flat file encode/decode.</span></span>  

## <a name="delete-your-integration-account"></a><span data-ttu-id="a534c-142">Az integráció fiók törlése</span><span class="sxs-lookup"><span data-stu-id="a534c-142">Delete your integration account</span></span>

1. <span data-ttu-id="a534c-143">Válassza ki **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="a534c-143">Select **More services**.</span></span>

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="a534c-145">A keresőmezőbe írja be a "integrációt" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="a534c-145">In the search box, type "integration" for your filter.</span></span> <span data-ttu-id="a534c-146">Az eredmények listájában válassza **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="a534c-146">In the results list, select **Integration Accounts**.</span></span>

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="a534c-148">Válassza ki a törölni kívánt integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="a534c-148">Select the integration account that you want to delete.</span></span>

    ![Válassza ki az integráció fiók törlése](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. <span data-ttu-id="a534c-150">A menüben válassza a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a534c-150">On the menu, choose **Delete**.</span></span>

    ![Válassza a "Delete"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. <span data-ttu-id="a534c-152">Erősítse meg, hogy az integrációs fiók törlése.</span><span class="sxs-lookup"><span data-stu-id="a534c-152">Confirm your choice to delete the integration account.</span></span>

## <a name="move-your-integration-account"></a><span data-ttu-id="a534c-153">Az integráció fiók áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a534c-153">Move your integration account</span></span>

<span data-ttu-id="a534c-154">Integráció fiók áthelyezése egy másik Azure-előfizetés vagy az erőforrás-csoportok, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a534c-154">To move an integration account to another Azure subscription or resource group, follow these steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a534c-155">Minden parancsfájl használata az új erőforrás-azonosítók integrációs fiók áthelyezése után frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="a534c-155">You must update all scripts to use the new resource IDs after you move an integration account.</span></span>

1. <span data-ttu-id="a534c-156">Válassza ki **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="a534c-156">Select **More services**.</span></span>

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="a534c-158">A keresőmezőbe írja be a "integrációt" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="a534c-158">In the search box, type "integration" for your filter.</span></span> <span data-ttu-id="a534c-159">Az eredmények listájában válassza **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="a534c-159">In the results list, select **Integration Accounts**.</span></span>

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. <span data-ttu-id="a534c-161">Válassza ki a integrációs fiókot, amelyet át szeretné helyezni.</span><span class="sxs-lookup"><span data-stu-id="a534c-161">Select the integration account that you want to move.</span></span> <span data-ttu-id="a534c-162">A **beállítások**, válassza a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="a534c-162">Under **Settings**, choose **Properties**.</span></span>

    ![Jelölje ki az integráció fiókot, helyezze át.](./media/logic-apps-enterprise-integration-accounts/move.png)

5. <span data-ttu-id="a534c-165">Erőforráscsoport vagy az Azure-integráció fiókjához tartozó előfizetés módosítása.</span><span class="sxs-lookup"><span data-stu-id="a534c-165">Change the resource group or Azure subscription that's associated with your integration account.</span></span>

    ![Válassza ki a módosítás erőforráscsoportba vagy módosítás előfizetés](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a><span data-ttu-id="a534c-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a534c-167">Next Steps</span></span>
* [<span data-ttu-id="a534c-168">További információ a megállapodások</span><span class="sxs-lookup"><span data-stu-id="a534c-168">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  

