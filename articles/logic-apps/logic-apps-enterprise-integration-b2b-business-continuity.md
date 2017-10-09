---
title: "aaaDisaster helyreállítási B2B integrációs fiók - Azure Logic Apps |} Microsoft Docs"
description: "Logic Apps B2B katasztrófa utáni helyreállítás"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e86564a3c5a2607d22514936c606e2843cba0416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a><span data-ttu-id="cbd39-103">Logic Apps B2B kereszt-régió katasztrófa utáni helyreállítás</span><span class="sxs-lookup"><span data-stu-id="cbd39-103">Logic Apps B2B cross-region disaster recovery</span></span>

<span data-ttu-id="cbd39-104">B2B munkaterhelések vonatkozhat például rendeléseket és a számlák pénz tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-104">B2B workloads involve money transactions like orders and invoices.</span></span> <span data-ttu-id="cbd39-105">A vész-események esetén fontos az olyan üzleti tooquickly helyreállítás toomeet hello a partnerekkel együttműködve egyeztetett vállalati szintű SLA-k.</span><span class="sxs-lookup"><span data-stu-id="cbd39-105">During a disaster event, it's critical for a business tooquickly recover toomeet hello business-level SLAs agreed upon with their partners.</span></span> <span data-ttu-id="cbd39-106">Ez a cikk bemutatja, hogyan toobuild üzletmenet-folytonosságának megtervezése B2B munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="cbd39-106">This article demonstrates how toobuild a business continuity plan for B2B workloads.</span></span> 

* <span data-ttu-id="cbd39-107">Vész-helyreállítási készültségi</span><span class="sxs-lookup"><span data-stu-id="cbd39-107">Disaster Recovery readiness</span></span> 
* <span data-ttu-id="cbd39-108">Toosecondary régió feladatátvételt egy vész-esemény</span><span class="sxs-lookup"><span data-stu-id="cbd39-108">Fail over toosecondary region during a disaster event</span></span> 
* <span data-ttu-id="cbd39-109">Tartalék megoldásként tooprimary régió katasztrófa esemény után</span><span class="sxs-lookup"><span data-stu-id="cbd39-109">Fall back tooprimary region after a disaster event</span></span>

## <a name="disaster-recovery-readiness"></a><span data-ttu-id="cbd39-110">Vész-helyreállítási készültségi</span><span class="sxs-lookup"><span data-stu-id="cbd39-110">Disaster Recovery readiness</span></span>  

1. <span data-ttu-id="cbd39-111">Egy másodlagos régióban azonosításához, és hozzon létre egy [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) hello másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-111">Identify a secondary region and create an [integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) in hello secondary region.</span></span>

2. <span data-ttu-id="cbd39-112">Partnerek, a sémák és a szükséges hello üzenet adatfolyamok, amikor a futtatási állapot hello kell replikálni toobe toosecondary régió integrációs fiók megállapodások hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="cbd39-112">Add partners, schemas, and agreements for hello required message flows where hello run status needs toobe replicated toosecondary region integration account.</span></span>

   > [!TIP]
   > <span data-ttu-id="cbd39-113">Ellenőrizze, hogy nincs konzisztencia hello integrációs fiók összetevő elnevezési régiók közötti.</span><span class="sxs-lookup"><span data-stu-id="cbd39-113">Make sure there's consistency in hello integration account artifact's naming convention across regions.</span></span> 

3. <span data-ttu-id="cbd39-114">futtatási állapot hello elsődleges régióban, toopull hello hello másodlagos régióban logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbd39-114">toopull hello run status from hello primary region, create a logic app in hello secondary region.</span></span> 

   <span data-ttu-id="cbd39-115">A logikai alkalmazás rendelkeznie kell egy *eseményindító* és egy *művelet*.</span><span class="sxs-lookup"><span data-stu-id="cbd39-115">This logic app should have a *trigger* and an *action*.</span></span> 
   <span data-ttu-id="cbd39-116">hello eseményindító csatlakoznia tooprimary régió integrációs fiókot, és hello művelet toosecondary régió integrációs fiókot kell csatlakozniuk.</span><span class="sxs-lookup"><span data-stu-id="cbd39-116">hello trigger should connect tooprimary region integration account, and hello action should connect toosecondary region integration account.</span></span> 
   <span data-ttu-id="cbd39-117">Hello időtartam alapján, hello eseményindító hello futtatása elsődleges régió állapot tábla kérdezi le, és lekéri a hello új rekordok, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="cbd39-117">Based on hello time interval, hello trigger polls hello primary region run status table and pulls hello new records, if any.</span></span> <span data-ttu-id="cbd39-118">hello művelet frissíti őket toosecondary régió integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-118">hello action updates them toosecondary region integration account.</span></span> 
   <span data-ttu-id="cbd39-119">Ezzel a megoldással tooget növekményes futási állapotának elsődleges régió toosecondary régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-119">This helps tooget incremental runtime status from primary region toosecondary region.</span></span>

4. <span data-ttu-id="cbd39-120">A Logic Apps-integráció fiók az üzletmenet folytonossága B2B protokollokat - X12, és a AS2, EDIFACT toosupport tervezték.</span><span class="sxs-lookup"><span data-stu-id="cbd39-120">Business continuity in Logic Apps integration account is designed toosupport based on B2B protocols - X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="cbd39-121">toofind részletes lépései, válassza ki a megfelelő hivatkozásokra hello.</span><span class="sxs-lookup"><span data-stu-id="cbd39-121">toofind detailed steps, select hello respective links.</span></span>

5. <span data-ttu-id="cbd39-122">a javaslat hello túlságosan toodeploy összes elsődleges régió erőforrást egy másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-122">hello recommendation is toodeploy all primary region resources in a secondary region too.</span></span> 

   <span data-ttu-id="cbd39-123">Elsődleges régió erőforrások közé tartoznak, az Azure SQL Database vagy Azure Cosmos DB, Azure Service Bus és az Azure Event Hubs üzenetküldési, az Azure API Management és az Azure App Service hello Azure Logic Apps szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="cbd39-123">Primary region resources include Azure SQL Database or Azure Cosmos DB, Azure Service Bus and Azure Event Hubs used for messaging, Azure API Management, and hello Azure Logic Apps feature in Azure App Service.</span></span>   

6. <span data-ttu-id="cbd39-124">A kapcsolatot az elsődleges régióban tooa másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="cbd39-124">Establish a connection from a primary region tooa secondary region.</span></span> <span data-ttu-id="cbd39-125">futtatási állapot egy elsődleges régióban toopull hello logikai alkalmazás létrehozása egy másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-125">toopull hello run status from a primary region, create a logic app in a secondary region.</span></span> 

   <span data-ttu-id="cbd39-126">hello logikai alkalmazás rendelkeznie kell egy eseményindító és egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="cbd39-126">hello logic app should have a trigger and an action.</span></span> 
   <span data-ttu-id="cbd39-127">hello eseményindító tooa elsődleges régió integrációs fiók csatlakoznia.</span><span class="sxs-lookup"><span data-stu-id="cbd39-127">hello trigger should connect tooa primary region integration account.</span></span> 
   <span data-ttu-id="cbd39-128">hello művelet tooa másodlagos régióba integrációs fiók csatlakoznia.</span><span class="sxs-lookup"><span data-stu-id="cbd39-128">hello action should connect tooa secondary region integration account.</span></span> 
   <span data-ttu-id="cbd39-129">Hello időtartam alapján, hello eseményindító hello futtatása elsődleges régió állapot tábla kérdezi le, és lekéri a hello új rekordok, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="cbd39-129">Based on hello time interval, hello trigger polls hello primary region run status table and pulls hello new records, if any.</span></span> 
   <span data-ttu-id="cbd39-130">hello művelet frissíti őket tooa másodlagos régióba integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-130">hello action updates them tooa secondary region integration account.</span></span> 
   <span data-ttu-id="cbd39-131">Ez az eljárás tooget növekményes futási állapotának segít hello elsődleges régió toohello másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-131">This process helps tooget incremental runtime status from hello primary region toohello secondary region.</span></span>

<span data-ttu-id="cbd39-132">A Logic Apps-integráció fiókban az üzletmenet folytonossága támogatja az AS2, EDIFACT és a hello B2B protokollok X12 alapján.</span><span class="sxs-lookup"><span data-stu-id="cbd39-132">Business continuity in a Logic Apps integration account provides support based on hello B2B protocols X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="cbd39-133">A X12 és az AS2 részletes lépéseiért lásd: [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) és [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="cbd39-133">For detailed steps on using X12 and AS2, see [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) and [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) in this article.</span></span>

## <a name="fail-over-tooa-secondary-region-during-a-disaster-event"></a><span data-ttu-id="cbd39-134">Tooa másodlagos régióba feladatátvételt egy vész-esemény</span><span class="sxs-lookup"><span data-stu-id="cbd39-134">Fail over tooa secondary region during a disaster event</span></span>

<span data-ttu-id="cbd39-135">A vészhelyreállítás események, amikor hello elsődleges régió nem érhető el az üzletmenet folytonosságának, közvetlen forgalom toohello másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="cbd39-135">During a disaster event, when hello primary region is not available for business continuity, direct traffic toohello secondary region.</span></span> <span data-ttu-id="cbd39-136">Egy másodlagos régióban segítségével gyorsan toomeet hello RPO/Helyreállításiidő megegyezésre által a partnerek működik egy üzleti toorecover.</span><span class="sxs-lookup"><span data-stu-id="cbd39-136">A secondary region helps a business toorecover functions quickly toomeet hello RPO/RTO agreed upon by their partners.</span></span> <span data-ttu-id="cbd39-137">Is minimalizálja erőfeszítéseket toofail keresztül egy régió tartozik tooanother régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-137">It also minimizes efforts toofail over from one region tooanother region.</span></span> 

<span data-ttu-id="cbd39-138">Nincs olyan várható késleltetés vezérlő számok másolása egy elsődleges régió tooa másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="cbd39-138">There is an expected latency while copying control numbers from a primary region tooa secondary region.</span></span> <span data-ttu-id="cbd39-139">ismétlődő létrehozott vezérlő küldése tooavoid toopartners számok katasztrófa esemény során, a hello ajánlás tooincrement hello vezérlő számok hello másodlagos régióba megállapodásokban használatával [PowerShell-parancsmagok](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="cbd39-139">tooavoid sending duplicate generated control numbers toopartners during a disaster event, hello recommendation is tooincrement hello control numbers in hello secondary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>

## <a name="fall-back-tooa-primary-region-post-disaster-event"></a><span data-ttu-id="cbd39-140">Tartalék megoldásként tooa elsődleges régió katasztrófa utáni esemény</span><span class="sxs-lookup"><span data-stu-id="cbd39-140">Fall back tooa primary region post-disaster event</span></span>

<span data-ttu-id="cbd39-141">toofall hátsó tooa elsődleges régió a rendelkezésre álló, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cbd39-141">toofall back tooa primary region when it is available, follow these steps:</span></span>

1. <span data-ttu-id="cbd39-142">Hello másodlagos régióban partnerektől származó üzenetek fogadását.</span><span class="sxs-lookup"><span data-stu-id="cbd39-142">Stop accepting messages from partners in hello secondary region.</span></span>  

2. <span data-ttu-id="cbd39-143">Használatával növelheti a generált hello vezérlő számokat az összes hello elsődleges régió szerződéshez [PowerShell-parancsmagok](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="cbd39-143">Increment hello generated control numbers for all hello primary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>  

3. <span data-ttu-id="cbd39-144">Hello másodlagos régióba toohello elsődleges régió közvetlen forgalmát.</span><span class="sxs-lookup"><span data-stu-id="cbd39-144">Direct traffic from hello secondary region toohello primary region.</span></span>

4. <span data-ttu-id="cbd39-145">Ellenőrizze, hogy a futtatási állapot hello elsődleges régióban húzza hello másodlagos régióban létrehozott hello logikai alkalmazás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="cbd39-145">Check that hello logic app created in hello secondary region for pulling run status from hello primary region is enabled.</span></span>

## <a name="x12"></a><span data-ttu-id="cbd39-146">X12</span><span class="sxs-lookup"><span data-stu-id="cbd39-146">X12</span></span> 

<span data-ttu-id="cbd39-147">X 12 dokumentumok EDI üzletmenet folytonossága vezérlő számok alapján:</span><span class="sxs-lookup"><span data-stu-id="cbd39-147">Business continuity for EDI X12 documents is based on control numbers:</span></span>

> [!TIP]
> <span data-ttu-id="cbd39-148">Is használhatja a hello [X12 quick start sablon](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logic Apps alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="cbd39-148">You can also use hello [X12 quick start template](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logic apps.</span></span> <span data-ttu-id="cbd39-149">Létrehozás elsődleges és másodlagos integrációs fiókok Előfeltételek toouse hello sablon.</span><span class="sxs-lookup"><span data-stu-id="cbd39-149">Creating primary and secondary integration accounts are prerequisites toouse hello template.</span></span> <span data-ttu-id="cbd39-150">hello sablon segítségével toocreate két a logic apps, kapott vezérlő számok egyet, majd egy másikat a létrehozott vezérlő számokat.</span><span class="sxs-lookup"><span data-stu-id="cbd39-150">hello template helps toocreate two logic apps, one for received control numbers and another for generated control numbers.</span></span> <span data-ttu-id="cbd39-151">Megfelelő eseményindítók és műveletek hello a logic apps, csatlakozás hello eseményindító toohello elsődleges integrációs fiók és a másodlagos integrációs toohello hello műveletfiókjához jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cbd39-151">Respective triggers and actions are created in hello logic apps, connecting hello trigger toohello primary integration account and hello action toohello secondary integration account.</span></span>

<span data-ttu-id="cbd39-152">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="cbd39-152">**Prerequisites**</span></span>

<span data-ttu-id="cbd39-153">bejövő üzenetek tooenable vész-helyreállítási beállításban válassza ismétlődő hello beállításokat hello X12 megállapodás fogadására.</span><span class="sxs-lookup"><span data-stu-id="cbd39-153">tooenable disaster recovery for inbound messages, select hello duplicate check settings in hello X12 agreement's Receive Settings.</span></span>

![Válassza ki a duplikált beállításokat](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. <span data-ttu-id="cbd39-155">Hozzon létre egy [logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) egy másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-155">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="cbd39-156">A Keresés **X12**, és válassza ki **X12-ellenőrzési számot módosításakor**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-156">Search on **X12**, and select **X12 - When a control number is modified**.</span></span>   

   ![X12 keresése](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   <span data-ttu-id="cbd39-158">hello eseményindító tooestablish egy kapcsolat tooan integrációs fiók megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="cbd39-158">hello trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="cbd39-159">hello eseményindító kell csatlakoztatott tooa elsődleges régió integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-159">hello trigger should be connected tooa primary region integration account.</span></span>

3. <span data-ttu-id="cbd39-160">Adja meg a kapcsolat nevét, válassza ki a *elsődleges régió integrációs fiók* hello a listában, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-160">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>   

   ![Elsődleges régió integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. <span data-ttu-id="cbd39-162">Hello **DateTime toostart vezérlő számú szinkronizálási** beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="cbd39-162">hello **DateTime toostart control number sync** setting is optional.</span></span> <span data-ttu-id="cbd39-163">Hello **gyakoriság** túl beállítható**nap**, **óra**, **perc**, vagy **második** időközzel.</span><span class="sxs-lookup"><span data-stu-id="cbd39-163">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![Dátum és idő és gyakorisága](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. <span data-ttu-id="cbd39-165">Válassza ki **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-165">Select **New step** > **Add an action**.</span></span>

   ![Új lépés, Action hozzáadása](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. <span data-ttu-id="cbd39-167">A Keresés **X12**, és válassza ki **X12-hozzáadásakor vagy módosításakor a vezérlő számok**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-167">Search on **X12**, and select **X12 - Add or update control numbers**.</span></span>   

   ![Hozzáadásakor vagy módosításakor a vezérlő számok](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. <span data-ttu-id="cbd39-169">tooconnect egy műveleti tooa másodlagos régióba integrációs fiókot válasszon **kapcsolat módosítása** > **új kapcsolat hozzáadása** hello elérhető integrációs fiókok listáját.</span><span class="sxs-lookup"><span data-stu-id="cbd39-169">tooconnect an action tooa secondary region integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="cbd39-170">Adja meg a kapcsolat nevét, válassza ki a *másodlagos régióba integrációs fiók* hello a listában, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-170">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span> 

   ![Másodlagos régióba integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. <span data-ttu-id="cbd39-172">Váltás tooraw bemenetek jobb felső sarkában található hello ikonra kattintva.</span><span class="sxs-lookup"><span data-stu-id="cbd39-172">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Kapcsoló tooraw bemenetek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. <span data-ttu-id="cbd39-174">Válassza ki a szervezet hello dinamikus tartalom objektumválasztó a, és mentse a hello logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cbd39-174">Select Body from hello dynamic content picker, and save hello logic app.</span></span>

   ![Dinamikus tartalom mezők](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   <span data-ttu-id="cbd39-176">Hello időtartam alapján, hello eseményindító kérdezze le az hello kapott elsődleges régió vezérlő tábla száma és hello új rekordok lekérése.</span><span class="sxs-lookup"><span data-stu-id="cbd39-176">Based on hello time interval, hello trigger polls hello primary region received control number table and pulls hello new records.</span></span> 
   <span data-ttu-id="cbd39-177">hello művelet frissíti a hello másodlagos régióba integrációs fiók hello rekordokat.</span><span class="sxs-lookup"><span data-stu-id="cbd39-177">hello action updates hello records in hello secondary region integration account.</span></span> 
   <span data-ttu-id="cbd39-178">Ha a nincsenek frissítések, hello eseményindító állapota megjelenik **kimaradnak**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-178">If there are no updates, hello trigger status appears as **Skipped**.</span></span>   

   ![Vezérlő tábla száma](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="cbd39-180">Hello időtartam alapján, hello növekményes futási állapotának replikálja az elsődleges régióban tooa másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="cbd39-180">Based on hello time interval, hello incremental runtime status replicates from a primary region tooa secondary region.</span></span> <span data-ttu-id="cbd39-181">A vészhelyreállítás események, ha hello elsődleges régióban nem áll rendelkezésre, közvetlen forgalom toohello másodlagos régióba az üzletmenet folytonossága érdekében.</span><span class="sxs-lookup"><span data-stu-id="cbd39-181">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="edifact"></a><span data-ttu-id="cbd39-182">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="cbd39-182">EDIFACT</span></span> 

<span data-ttu-id="cbd39-183">Az üzletmenet folytonossága EDI EDIFACT-dokumentumok vezérlő számok alapul.</span><span class="sxs-lookup"><span data-stu-id="cbd39-183">Business continuity for EDI EDIFACT documents is based on control numbers.</span></span>

<span data-ttu-id="cbd39-184">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="cbd39-184">**Prerequisites**</span></span>

<span data-ttu-id="cbd39-185">bejövő üzenetek tooenable vész-helyreállítási beállításban válassza ismétlődő hello beállításokat a EDIFACT megállapodás fogadására.</span><span class="sxs-lookup"><span data-stu-id="cbd39-185">tooenable disaster recovery for inbound messages, select hello duplicate check settings in your EDIFACT agreement's Receive Settings.</span></span>

![Válassza ki a duplikált beállításokat](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. <span data-ttu-id="cbd39-187">Hozzon létre egy [logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) egy másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-187">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="cbd39-188">A Keresés **EDIFACT**, és válassza ki **EDIFACT - ellenőrzési számot módosításakor**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-188">Search on **EDIFACT**, and select **EDIFACT - When a control number is modified**.</span></span>

   ![EDIFACT keresése](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   <span data-ttu-id="cbd39-190">hello eseményindító tooestablish egy kapcsolat tooan integrációs fiók megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="cbd39-190">hello trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="cbd39-191">hello eseményindító kell csatlakoztatott tooa elsődleges régió integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-191">hello trigger should be connected tooa primary region integration account.</span></span> 

3. <span data-ttu-id="cbd39-192">Adja meg a kapcsolat nevét, válassza ki a *elsődleges régió integrációs fiók* hello a listában, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-192">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>    

   ![Elsődleges régió integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. <span data-ttu-id="cbd39-194">Hello **DateTime toostart vezérlő számú szinkronizálási** beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="cbd39-194">hello **DateTime toostart control number sync** setting is optional.</span></span> <span data-ttu-id="cbd39-195">Hello **gyakoriság** túl beállítható**nap**, **óra**, **perc**, vagy **második** időközzel.</span><span class="sxs-lookup"><span data-stu-id="cbd39-195">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>    

   ![Dátum és idő és gyakorisága](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. <span data-ttu-id="cbd39-197">Válassza ki **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-197">Select **New step** > **Add an action**.</span></span>    

   ![Új lépés, Action hozzáadása](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. <span data-ttu-id="cbd39-199">A Keresés **EDIFACT**, és válassza ki **EDIFACT - hozzáadásakor vagy módosításakor a vezérlő számok**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-199">Search on **EDIFACT**, and select **EDIFACT - Add or update control numbers**.</span></span>   

   ![Hozzáadásakor vagy módosításakor a vezérlő számok](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. <span data-ttu-id="cbd39-201">tooconnect egy műveleti tooa másodlagos régióba integrációs fiókot válasszon **kapcsolat módosítása** > **új kapcsolat hozzáadása** hello elérhető integrációs fiókok listáját.</span><span class="sxs-lookup"><span data-stu-id="cbd39-201">tooconnect an action tooa secondary region integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="cbd39-202">Adja meg a kapcsolat nevét, válassza ki a *másodlagos régióba integrációs fiók* hello a listában, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-202">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span>

   ![Másodlagos régióba integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. <span data-ttu-id="cbd39-204">Váltás tooraw bemenetek jobb felső sarkában található hello ikonra kattintva.</span><span class="sxs-lookup"><span data-stu-id="cbd39-204">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Kapcsoló tooraw bemenetek](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. <span data-ttu-id="cbd39-206">Válassza ki a szervezet hello dinamikus tartalom objektumválasztó a, és mentse a hello logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cbd39-206">Select Body from hello dynamic content picker, and save hello logic app.</span></span>   

   ![Dinamikus tartalom mezők](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   <span data-ttu-id="cbd39-208">Hello időtartam alapján, hello eseményindító kérdezze le az hello kapott elsődleges régió vezérlő tábla száma és hello új rekordok lekérése.</span><span class="sxs-lookup"><span data-stu-id="cbd39-208">Based on hello time interval, hello trigger polls hello primary region received control number table and pulls hello new records.</span></span>
   <span data-ttu-id="cbd39-209">hello művelet frissíti a hello rekordok toohello másodlagos régióba integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-209">hello action updates hello records toohello secondary region integration account.</span></span> 
   <span data-ttu-id="cbd39-210">Ha a nincsenek frissítések, hello eseményindító állapota megjelenik **kimaradnak**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-210">If there are no updates, hello trigger status appears as **Skipped**.</span></span>

   ![Vezérlő tábla száma](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="cbd39-212">Hello időtartam alapján, hello növekményes futási állapotának replikálja az elsődleges régióban tooa másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="cbd39-212">Based on hello time interval, hello incremental runtime status replicates from a primary region tooa secondary region.</span></span> <span data-ttu-id="cbd39-213">A vészhelyreállítás események, ha hello elsődleges régióban nem áll rendelkezésre, közvetlen forgalom toohello másodlagos régióba az üzletmenet folytonossága érdekében.</span><span class="sxs-lookup"><span data-stu-id="cbd39-213">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="as2"></a><span data-ttu-id="cbd39-214">AS2</span><span class="sxs-lookup"><span data-stu-id="cbd39-214">AS2</span></span> 

<span data-ttu-id="cbd39-215">Az üzletmenet folytonossága hello AS2 protokollt használó dokumentumok hello Üzenetazonosítója és hello MIC érték alapul.</span><span class="sxs-lookup"><span data-stu-id="cbd39-215">Business continuity for documents that use hello AS2 protocol is based on hello message ID and hello MIC value.</span></span>

> [!TIP]
> <span data-ttu-id="cbd39-216">Is használhatja a hello [AS2 gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logic Apps alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="cbd39-216">You can also use hello [AS2 quick start template](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logic apps.</span></span> <span data-ttu-id="cbd39-217">Létrehozás elsődleges és másodlagos integrációs fiókok Előfeltételek toouse hello sablon.</span><span class="sxs-lookup"><span data-stu-id="cbd39-217">Creating primary and secondary integration accounts are prerequisites toouse hello template.</span></span> <span data-ttu-id="cbd39-218">hello sablon segítségével, amelyen egy eseményindító és művelet logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbd39-218">hello template helps create a logic app that has a trigger and an action.</span></span> <span data-ttu-id="cbd39-219">hello logikai alkalmazás kapcsolatot hoz létre egy eseményindító tooa elsődleges integrációs fiókot és egy művelet tooa másodlagos integrációs fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="cbd39-219">hello logic app creates a connection from a trigger tooa primary integration account and an action tooa secondary integration account.</span></span>

1. <span data-ttu-id="cbd39-220">Hozzon létre egy [logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) hello másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="cbd39-220">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in hello secondary region.</span></span>  

2. <span data-ttu-id="cbd39-221">A Keresés **AS2**, és válassza ki **AS2 - Ha a MIC érték jön létre**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-221">Search on **AS2**, and select **AS2 - When a MIC value is created**.</span></span>   

   ![AS2 keresése](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   <span data-ttu-id="cbd39-223">Egy eseményindító tooestablish egy kapcsolat tooan integrációs fiók kéri.</span><span class="sxs-lookup"><span data-stu-id="cbd39-223">A trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="cbd39-224">hello eseményindító kell csatlakoztatott tooa elsődleges régió integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-224">hello trigger should be connected tooa primary region integration account.</span></span> 
   
3. <span data-ttu-id="cbd39-225">Adja meg a kapcsolat nevét, válassza ki a *elsődleges régió integrációs fiók* hello a listában, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-225">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>

   ![Elsődleges régió integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. <span data-ttu-id="cbd39-227">Hello **DateTime toostart MIC érték szinkronizálási** beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="cbd39-227">hello **DateTime toostart MIC value sync** setting is optional.</span></span> <span data-ttu-id="cbd39-228">Hello **gyakoriság** túl beállítható**nap**, **óra**, **perc**, vagy **második** időközzel.</span><span class="sxs-lookup"><span data-stu-id="cbd39-228">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![Dátum és idő és gyakorisága](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. <span data-ttu-id="cbd39-230">Válassza ki **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-230">Select **New step** > **Add an action**.</span></span>  

   ![Új lépés, Action hozzáadása](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. <span data-ttu-id="cbd39-232">A Keresés **AS2**, és válassza ki **AS2 - hozzáadásakor vagy módosításakor a MIC tartalma**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-232">Search on **AS2**, and select **AS2 - Add or update MIC contents**.</span></span>  

   ![MIC hozzáadása vagy frissítése](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. <span data-ttu-id="cbd39-234">tooconnect egy műveleti tooa másodlagos integrációs fiókot válasszon **kapcsolat módosítása** > **új kapcsolat hozzáadása** hello elérhető integrációs fiókok listáját.</span><span class="sxs-lookup"><span data-stu-id="cbd39-234">tooconnect an action tooa secondary integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="cbd39-235">Adja meg a kapcsolat nevét, válassza ki a *másodlagos régióba integrációs fiók* hello a listában, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-235">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span>

   ![Másodlagos régióba integrációs fiók neve](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. <span data-ttu-id="cbd39-237">Váltás tooraw bemenetek jobb felső sarkában található hello ikonra kattintva.</span><span class="sxs-lookup"><span data-stu-id="cbd39-237">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Kapcsoló tooraw bemenetek](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. <span data-ttu-id="cbd39-239">Válassza ki a szervezet hello dinamikus tartalom objektumválasztó a, és mentse a hello logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cbd39-239">Select Body from hello dynamic content picker, and save hello logic app.</span></span>   

   ![Dinamikus tartalom](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   <span data-ttu-id="cbd39-241">Hello alatt az időtartam alatt, hello eseményindító kérdezze le az elsődleges régió tábla hello és hello új rekordok kéri le.</span><span class="sxs-lookup"><span data-stu-id="cbd39-241">Based on hello time interval, hello trigger polls hello primary region table and pulls hello new records.</span></span> <span data-ttu-id="cbd39-242">hello művelet frissíti őket toohello másodlagos régióba integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="cbd39-242">hello action updates them toohello secondary region integration account.</span></span> 
   <span data-ttu-id="cbd39-243">Ha a nincsenek frissítések, hello eseményindító állapota megjelenik **kimaradnak**.</span><span class="sxs-lookup"><span data-stu-id="cbd39-243">If there are no updates, hello trigger status appears as **Skipped**.</span></span>  

   ![Elsődleges régió tábla](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

<span data-ttu-id="cbd39-245">Hello növekményes futási állapotának hello elsődleges régió toohello másodlagos régióban hello időtartam alapján, replikálja.</span><span class="sxs-lookup"><span data-stu-id="cbd39-245">Based on hello time interval, hello incremental runtime status replicates from hello primary region toohello secondary region.</span></span> <span data-ttu-id="cbd39-246">A vészhelyreállítás események, ha hello elsődleges régióban nem áll rendelkezésre, közvetlen forgalom toohello másodlagos régióba az üzletmenet folytonossága érdekében.</span><span class="sxs-lookup"><span data-stu-id="cbd39-246">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="cbd39-247">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbd39-247">Next steps</span></span>

[<span data-ttu-id="cbd39-248">B2B üzenetek megfigyelése</span><span class="sxs-lookup"><span data-stu-id="cbd39-248">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md)

