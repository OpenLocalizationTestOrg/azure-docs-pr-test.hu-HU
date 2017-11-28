---
title: "az üzleti vállalatközi (B2B) üzenetek - Azure Logic Apps aaaCreate partnerek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd partnerek tooyour integrációs hello vállalati integrációs csomag és a Logic Apps fiók"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8dc70a8f441fcf228ed178029dcdbac940d794b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a><span data-ttu-id="f9b8f-103">Hozzáadásakor vagy módosításakor a partnerek a munkafolyamat vállalatok szerződések</span><span class="sxs-lookup"><span data-stu-id="f9b8f-103">Add or update partners in business-to-business agreements in your workflow</span></span>

<span data-ttu-id="f9b8f-104">A partnerek olyan entitásokat, üzleti vállalatközi (B2B) tranzakciók részt, és az exchange-üzenetek között.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-104">Partners are entities that participate in business-to-business (B2B) transactions and exchange messages between each other.</span></span> <span data-ttu-id="f9b8f-105">Létrehozhat, és ezek a tranzakciók a másik szervezet képviselő partnerekkel, előtt is, amely azonosítja, és ellenőrzi az egyes küldött üzenetek az információkat.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-105">Before you can create partners that represent you and another organization in these transactions, you must both share information that identifies and validates messages sent by each other.</span></span> <span data-ttu-id="f9b8f-106">Miután vitassa meg ezeket az adatokat, és készen áll a toostart az üzleti kapcsolat, az integráció fiók toorepresent partnerek hozhat létre, mindkét.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-106">After you discuss these details and are ready toostart your business relationship, you can create partners in your integration account toorepresent you both.</span></span>

## <a name="what-roles-do-partners-have-in-your-integration-account"></a><span data-ttu-id="f9b8f-107">Milyen szerepkörök rendelkeznek partnerek integrációs fiókja?</span><span class="sxs-lookup"><span data-stu-id="f9b8f-107">What roles do partners have in your integration account?</span></span>

<span data-ttu-id="f9b8f-108">partnerek között váltott köszönőüzenetei toodefine részleteit, ezeket a partnerek között létrejött megállapodások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-108">toodefine details about hello messages exchanged between partners, you create agreements between those partners.</span></span> <span data-ttu-id="f9b8f-109">Azonban létrehozhat egy szerződést, mielőtt hozzá kellett adnia legalább két partner tooyour integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-109">However, before you can create an agreement, you must have added at least two partners tooyour integration account.</span></span> <span data-ttu-id="f9b8f-110">A szervezet hello hello megállapodás részét kell képeznie **fogadó partner**.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-110">Your organization must be part of hello agreement as hello **host partner**.</span></span> <span data-ttu-id="f9b8f-111">másik partnert hello vagy **Vendég partner** jelöli, mely a szervezet üzenetek szervezet hello.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-111">hello other partner, or **guest partner** represents hello organization that exchanges messages with your organization.</span></span> <span data-ttu-id="f9b8f-112">hello Vendég partner lehet egy másik vállalat, vagy akár egy részleget a saját szervezetében.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-112">hello guest partner can be another company, or even a department in your own organization.</span></span>

<span data-ttu-id="f9b8f-113">Miután hozzáadta a partnerek, létrehozhat egy szerződést.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-113">After you add these partners, you can create an agreement.</span></span>

<span data-ttu-id="f9b8f-114">Értesítések fogadásához és küldéséhez beállítások hello szempontból a birtokolt Partner hello irányulnak.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-114">Receive and Send settings are oriented from hello point of view of hello Hosted Partner.</span></span> <span data-ttu-id="f9b8f-115">Például hello kap a szerződés beállítások határozza meg, hogyan üzemeltetett hello partner kap egy Vendég partner küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-115">For example, hello receive settings in an agreement determine how hello hosted partner receives messages sent from a guest partner.</span></span> <span data-ttu-id="f9b8f-116">Hasonlóképpen hello küldési beállítások hello megállapodás azt jelzik, hogyan üzemeltetett hello partner küld üzeneteket toohello Vendég partner.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-116">Likewise, hello send settings on hello agreement indicate how hello hosted partner sends messages toohello guest partner.</span></span>

## <a name="how-toocreate-a-partner"></a><span data-ttu-id="f9b8f-117">Hogyan toocreate partner?</span><span class="sxs-lookup"><span data-stu-id="f9b8f-117">How toocreate a partner?</span></span>

1. <span data-ttu-id="f9b8f-118">Hello Azure-portálon, válassza ki **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-118">In hello Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="f9b8f-119">Hello szűrő keresési mezőbe, írja be a **integrációs**, majd jelölje be **integrációs fiókok** hello eredménylistában.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-119">In hello filter search box, enter **integration**, then select **Integration Accounts** in hello results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="f9b8f-120">Válassza ki a hello integrációs fiókra, amelyhez tooadd partnereivel.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-120">Select hello integration account where you want tooadd your partners.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="f9b8f-121">Jelölje be hello **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-121">Select hello **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. <span data-ttu-id="f9b8f-122">Hello partnerek paneljén válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-122">In hello Partners blade, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. <span data-ttu-id="f9b8f-123">Adja meg a partner nevét, majd válasszon egy **minősítő**.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-123">Enter a name for your partner, then select a **Qualifier**.</span></span> <span data-ttu-id="f9b8f-124">Végül adja meg a **érték** toohelp dokumentumok, amelyek az alkalmazások azonosítása.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-124">Finally, enter a **Value** toohelp identify documents that come into your apps.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. <span data-ttu-id="f9b8f-125">a partner létrehozási folyamata, jelölje be hello toosee hello folyamatban *harang* értesítési ikon.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-125">toosee hello progress for your partner creation process, select hello *bell* notification icon.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. <span data-ttu-id="f9b8f-126">tooconfirm, amely az új partnerek sikeresen létre lettek hozzáadva, jelölje be hello **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-126">tooconfirm that your new partners were successfully added, select hello **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    <span data-ttu-id="f9b8f-127">Hello partnerek csempe kiválasztása után is megjelenik az újonnan hozzáadott partnerek hello partnerek panelen.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-127">After you select hello Partners tile, you'll also see  newly added partners in hello Partners blade.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-tooedit-existing-partners-in-your-integration-account"></a><span data-ttu-id="f9b8f-128">Hogyan tooedit meglévő partnereknek az integráció-fiókban</span><span class="sxs-lookup"><span data-stu-id="f9b8f-128">How tooedit existing partners in your integration account</span></span>

1. <span data-ttu-id="f9b8f-129">Jelölje be hello **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-129">Select hello **Partners** tile.</span></span>
2. <span data-ttu-id="f9b8f-130">Ha megnyílt az hello partnerek panelen, válassza ki a tooedit kívánt hello partnert.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-130">After hello Partners blade opens, select hello partner you want tooedit.</span></span>
3. <span data-ttu-id="f9b8f-131">A hello **frissítés Partner** csempére, hajtsa végre a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-131">On hello **Update Partner** tile, make your changes.</span></span>
4. <span data-ttu-id="f9b8f-132">Miután elkészült, válassza ki azt **mentése**, toocancel a módosításokat, válassza ki vagy **elvetése**.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-132">After you're done, choose **Save**, or toocancel your changes, select **Discard**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-toodelete-a-partner"></a><span data-ttu-id="f9b8f-133">Hogyan toodelete partner</span><span class="sxs-lookup"><span data-stu-id="f9b8f-133">How toodelete a partner</span></span>

1. <span data-ttu-id="f9b8f-134">Jelölje be hello **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-134">Select hello **Partners** tile.</span></span>
2. <span data-ttu-id="f9b8f-135">Miután hello Partner panel nyílik meg, válassza ki a megjeleníteni kívánt toodelete hello partnert.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-135">After hello Partner blade opens, select hello partner that you want toodelete.</span></span>
3. <span data-ttu-id="f9b8f-136">Válasszon **törlése**.</span><span class="sxs-lookup"><span data-stu-id="f9b8f-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a><span data-ttu-id="f9b8f-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f9b8f-137">Next steps</span></span>
* [<span data-ttu-id="f9b8f-138">További információ a megállapodások</span><span class="sxs-lookup"><span data-stu-id="f9b8f-138">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  

