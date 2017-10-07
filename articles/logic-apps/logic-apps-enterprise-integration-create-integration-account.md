---
title: "aaaCreate, csatolja, törléséhez vagy egy integrációs fiók áthelyezése az Azure logic apps |} Microsoft Docs"
description: "Hogyan toocreate integrációs fiókot, és hivatkozás tooyour logic Apps alkalmazások"
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
ms.openlocfilehash: fda6c91723b3e3624ee176df112ba8b6c9800273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-integration-account"></a><span data-ttu-id="620f3-103">Mi az az integráció fiókkal?</span><span class="sxs-lookup"><span data-stu-id="620f3-103">What is an integration account?</span></span>

<span data-ttu-id="620f3-104">Integráció fiók lehetővé teszi a vállalati integrációs alkalmazások toomanage összetevők, beleértve a sémák, térképeket, tanúsítványok, partnerek és szerződéseket.</span><span class="sxs-lookup"><span data-stu-id="620f3-104">An integration account allows enterprise integration apps toomanage artifacts, including schemas, maps, certificates, partners and agreements.</span></span> <span data-ttu-id="620f3-105">Bármely integrációs alkalmazást hoz létre egy integrációs fiók tooaccess használ, a sémák, térképeket, tanúsítványok és stb.</span><span class="sxs-lookup"><span data-stu-id="620f3-105">Any integration app you create uses an integration account tooaccess these schemas, maps, certificates, and so on.</span></span>

## <a name="create-an-integration-account"></a><span data-ttu-id="620f3-106">Integráció-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="620f3-106">Create an integration account</span></span>

1.  <span data-ttu-id="620f3-107">Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="620f3-107">Sign in toohello [Azure portal](http://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="620f3-108">Hello bal oldali menüben válassza ki a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="620f3-108">From hello left menu, select **More services**.</span></span>

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="620f3-110">Hello a keresőmezőbe írja be a "integrációt" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="620f3-110">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="620f3-111">Hello eredmények listában válassza ki a **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="620f3-111">In hello results list, select **Integration Accounts**.</span></span>

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="620f3-113">Hello hello oldal tetején, válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="620f3-113">At hello top of hello page, choose **Add**.</span></span>

    ![Válasszon hozzáadása](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. <span data-ttu-id="620f3-115">Integráció fiókja és -válassza hello Azure-előfizetést, amelyet az toouse nevet.</span><span class="sxs-lookup"><span data-stu-id="620f3-115">Name your integration account and select hello Azure subscription that you want toouse.</span></span> <span data-ttu-id="620f3-116">Létrehozhat egy új **erőforráscsoport** vagy válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="620f3-116">You can either create a new **Resource group** or select an existing resource group.</span></span> <span data-ttu-id="620f3-117">Válassza ki a **hely** integrációs fiókja üzemeltetésére és egy **árképzési szintjében**.</span><span class="sxs-lookup"><span data-stu-id="620f3-117">Then select a **Location** for hosting your integration account and a **Pricing Tier**.</span></span> 

    <span data-ttu-id="620f3-118">Ha elkészült, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="620f3-118">When you're ready, choose **Create**.</span></span>

    ![Részletek a integrációs fiók megadása](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    <span data-ttu-id="620f3-120">Azure látja el a integrációs fiók hello kiválasztott helyen, amely 1 percen belül kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="620f3-120">Azure provisions your integration account  in hello selected location, which should complete within 1 minute.</span></span>

5. <span data-ttu-id="620f3-121">Frissítse a hello lapot.</span><span class="sxs-lookup"><span data-stu-id="620f3-121">Refresh hello page.</span></span> <span data-ttu-id="620f3-122">Megjelenik az új integrációs fiókját.</span><span class="sxs-lookup"><span data-stu-id="620f3-122">You see your new integration account listed.</span></span>

    ![Új integrációs fiókja jelenik meg.](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

<span data-ttu-id="620f3-124">A következő fiókját hello integrációs adott létrehozott tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="620f3-124">Next, link hello integration account that you created tooyour logic app.</span></span> 

## <a name="link-an-integration-account-tooa-logic-app"></a><span data-ttu-id="620f3-125">Hivatkozásra egy integrációs fiók tooa logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="620f3-125">Link an integration account tooa logic app</span></span>

<span data-ttu-id="620f3-126">toogive a logic apps toomaps, sémákat, szerződések és egyéb összetevők integrációs fiókjában, hivatkozás hello integrációs fiók tooyour logikai alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="620f3-126">toogive your logic apps access toomaps, schemas, agreements, and other artifacts in your integration account, link hello integration account tooyour logic app.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="620f3-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="620f3-127">Prerequisites</span></span>

* <span data-ttu-id="620f3-128">Integráció fiók</span><span class="sxs-lookup"><span data-stu-id="620f3-128">An integration account</span></span>
* <span data-ttu-id="620f3-129">A logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="620f3-129">A logic app</span></span>

> [!NOTE] 
> <span data-ttu-id="620f3-130">Gondoskodjon arról, hogy az integrációs fiók és a logic app hello *azonos Azure-beli hely* megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="620f3-130">Make sure your integration account and logic app are in hello *same Azure location* before you begin.</span></span>


1. <span data-ttu-id="620f3-131">Hello Azure-portálon válassza ki a Logic Apps alkalmazást, és ellenőrizze a logikai alkalmazás helyét.</span><span class="sxs-lookup"><span data-stu-id="620f3-131">In hello Azure portal, select your logic app, and check your logic app's location.</span></span>

    ![Válassza ki a Logic Apps alkalmazást, a hely ellenőrzése](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. <span data-ttu-id="620f3-133">A **beállítások**, jelölje be **integrációs fiók**.</span><span class="sxs-lookup"><span data-stu-id="620f3-133">Under **Settings**, select **Integration Account**.</span></span>

    ![Válassza ki a "Integrációs fiók"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. <span data-ttu-id="620f3-135">A hello **válassza ki a integrációs fiókot** listájában, jelölje be hello integrációs fióknevet, amelyet a toolink tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="620f3-135">From hello **Select an Integration account** list, select hello integration account you want toolink tooyour logic app.</span></span> <span data-ttu-id="620f3-136">linking, toofinish válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="620f3-136">toofinish linking, choose **Save**.</span></span>

    ![Válassza ki a integrációs fiókját](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    <span data-ttu-id="620f3-138">Bemutatja az integráció fiók csatolt tooyour logic app, valamint arról, hogy az integrációs-fiókban lévő összes összetevők most elérhető tooyour logikai alkalmazás értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="620f3-138">You get a notification that shows your integration account is linked tooyour logic app,  and that all artifacts in your integration account are now available tooyour logic app.</span></span>

    ![A Logic Apps alkalmazást kapcsolódó tooyour integrációs fiók](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

<span data-ttu-id="620f3-140">Most, hogy az integráció fiók csatolt tooyour logic app, hello B2B összekötők a logic apps segítségével is.</span><span class="sxs-lookup"><span data-stu-id="620f3-140">Now that your integration account is linked tooyour logic app, you can use hello B2B connectors in your logic apps.</span></span> <span data-ttu-id="620f3-141">Néhány gyakori B2B összekötők XML-érvényesítés, és egybesimított fájl kódolási/dekódolási tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="620f3-141">Some common B2B connectors include XML validation and flat file encode/decode.</span></span>  

## <a name="delete-your-integration-account"></a><span data-ttu-id="620f3-142">Az integráció fiók törlése</span><span class="sxs-lookup"><span data-stu-id="620f3-142">Delete your integration account</span></span>

1. <span data-ttu-id="620f3-143">Válassza ki **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="620f3-143">Select **More services**.</span></span>

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="620f3-145">Hello a keresőmezőbe írja be a "integrációt" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="620f3-145">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="620f3-146">Hello eredmények listában válassza ki a **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="620f3-146">In hello results list, select **Integration Accounts**.</span></span>

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="620f3-148">Válassza ki, hogy szeretné-e toodelete hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="620f3-148">Select hello integration account that you want toodelete.</span></span>

    ![Válassza ki az integráció fiók toodelete](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. <span data-ttu-id="620f3-150">A hello menüben válasszon **törlése**.</span><span class="sxs-lookup"><span data-stu-id="620f3-150">On hello menu, choose **Delete**.</span></span>

    ![Válassza a "Delete"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. <span data-ttu-id="620f3-152">A choice toodelete hello integrációs visszaigazoláshoz.</span><span class="sxs-lookup"><span data-stu-id="620f3-152">Confirm your choice toodelete hello integration account.</span></span>

## <a name="move-your-integration-account"></a><span data-ttu-id="620f3-153">Az integráció fiók áthelyezése</span><span class="sxs-lookup"><span data-stu-id="620f3-153">Move your integration account</span></span>

<span data-ttu-id="620f3-154">toomove egy integrációs fiók tooanother Azure előfizetés vagy az erőforrás-csoportok, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="620f3-154">toomove an integration account tooanother Azure subscription or resource group, follow these steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="620f3-155">Integráció fiók áthelyezése után frissítenie kell az összes parancsfájlok toouse hello új erőforrás-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="620f3-155">You must update all scripts toouse hello new resource IDs after you move an integration account.</span></span>

1. <span data-ttu-id="620f3-156">Válassza ki **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="620f3-156">Select **More services**.</span></span>

    ![Válassza ki a "Szolgáltatás"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="620f3-158">Hello a keresőmezőbe írja be a "integrációt" a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="620f3-158">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="620f3-159">Hello eredmények listában válassza ki a **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="620f3-159">In hello results list, select **Integration Accounts**.</span></span>

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. <span data-ttu-id="620f3-161">Válassza ki, hogy szeretné-e toomove hello integrációs fiókot.</span><span class="sxs-lookup"><span data-stu-id="620f3-161">Select hello integration account that you want toomove.</span></span> <span data-ttu-id="620f3-162">A **beállítások**, válassza a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="620f3-162">Under **Settings**, choose **Properties**.</span></span>

    ![Válassza ki az integráció fiók toomove.](./media/logic-apps-enterprise-integration-accounts/move.png)

5. <span data-ttu-id="620f3-165">Hello erőforráscsoport vagy az Azure-integráció fiókjához tartozó előfizetés módosítása.</span><span class="sxs-lookup"><span data-stu-id="620f3-165">Change hello resource group or Azure subscription that's associated with your integration account.</span></span>

    ![Válassza ki a módosítás erőforráscsoportba vagy módosítás előfizetés](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a><span data-ttu-id="620f3-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="620f3-167">Next Steps</span></span>
* [<span data-ttu-id="620f3-168">További információ a megállapodások</span><span class="sxs-lookup"><span data-stu-id="620f3-168">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  

