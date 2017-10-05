---
title: "Az üzleti vállalatközi (B2B) üzenetek - Azure Logic Apps partnerek létrehozása |} Microsoft Docs"
description: "Útmutató a vállalati integrációs csomag és a Logic Apps integrációs fiókjába partnerek hozzáadása"
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
ms.openlocfilehash: 950cb449b53f400f0f0f860caf5415bbb5212269
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a><span data-ttu-id="7d9cd-103">Hozzáadásakor vagy módosításakor a partnerek a munkafolyamat vállalatok szerződések</span><span class="sxs-lookup"><span data-stu-id="7d9cd-103">Add or update partners in business-to-business agreements in your workflow</span></span>

<span data-ttu-id="7d9cd-104">A partnerek olyan entitásokat, üzleti vállalatközi (B2B) tranzakciók részt, és az exchange-üzenetek között.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-104">Partners are entities that participate in business-to-business (B2B) transactions and exchange messages between each other.</span></span> <span data-ttu-id="7d9cd-105">Létrehozhat, és ezek a tranzakciók a másik szervezet képviselő partnerekkel, előtt is, amely azonosítja, és ellenőrzi az egyes küldött üzenetek az információkat.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-105">Before you can create partners that represent you and another organization in these transactions, you must both share information that identifies and validates messages sent by each other.</span></span> <span data-ttu-id="7d9cd-106">Miután vitassa meg ezeket az adatokat, és az üzleti kapcsolat készen áll, az integráció fiókban partnerek számára, hogy megfelelnek az Ön is hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-106">After you discuss these details and are ready to start your business relationship, you can create partners in your integration account to represent you both.</span></span>

## <a name="what-roles-do-partners-have-in-your-integration-account"></a><span data-ttu-id="7d9cd-107">Milyen szerepkörök rendelkeznek partnerek integrációs fiókja?</span><span class="sxs-lookup"><span data-stu-id="7d9cd-107">What roles do partners have in your integration account?</span></span>

<span data-ttu-id="7d9cd-108">Adja meg a partnerek között váltott üzenetek részleteit, ezeket a partnerek között létrejött megállapodások kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-108">To define details about the messages exchanged between partners, you create agreements between those partners.</span></span> <span data-ttu-id="7d9cd-109">Azonban létrehozhat egy szerződést, mielőtt hozzá kellett adnia legalább két partner integrációs fiókjába.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-109">However, before you can create an agreement, you must have added at least two partners to your integration account.</span></span> <span data-ttu-id="7d9cd-110">A szervezet, a szerződés részét kell képeznie a **fogadó partner**.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-110">Your organization must be part of the agreement as the **host partner**.</span></span> <span data-ttu-id="7d9cd-111">A másik partnert vagy **Vendég partner** azt a szervezetet, amely az üzeneteket a szervezet jelöli.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-111">The other partner, or **guest partner** represents the organization that exchanges messages with your organization.</span></span> <span data-ttu-id="7d9cd-112">A Vendég partner lehet egy másik vállalat, vagy akár egy részleget a saját szervezetében.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-112">The guest partner can be another company, or even a department in your own organization.</span></span>

<span data-ttu-id="7d9cd-113">Miután hozzáadta a partnerek, létrehozhat egy szerződést.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-113">After you add these partners, you can create an agreement.</span></span>

<span data-ttu-id="7d9cd-114">Értesítések fogadásához és küldéséhez a beállítások a üzemeltetett Partner szempontból irányulnak.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-114">Receive and Send settings are oriented from the point of view of the Hosted Partner.</span></span> <span data-ttu-id="7d9cd-115">Például egy szerződést a fogadási beállítások határozzák meg, hogyan az üzemeltetett partner kap egy Vendég partner küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-115">For example, the receive settings in an agreement determine how the hosted partner receives messages sent from a guest partner.</span></span> <span data-ttu-id="7d9cd-116">Hasonlóképpen a Küldés beállításai a szerződésben azt jelzik, hogyan az üzemeltetett partner üzeneteket küld a Vendég partner.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-116">Likewise, the send settings on the agreement indicate how the hosted partner sends messages to the guest partner.</span></span>

## <a name="how-to-create-a-partner"></a><span data-ttu-id="7d9cd-117">Hogyan hozhat létre a partner?</span><span class="sxs-lookup"><span data-stu-id="7d9cd-117">How to create a partner?</span></span>

1. <span data-ttu-id="7d9cd-118">Válassza ki az Azure-portálon **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-118">In the Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="7d9cd-119">A szűrő a keresőmezőbe írja be **integrációs**, majd jelölje be **integrációs fiókok** az eredménylistában.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-119">In the filter search box, enter **integration**, then select **Integration Accounts** in the results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="7d9cd-120">Válassza ki az integráció fiókra, amelyhez a partnerek hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-120">Select the integration account where you want to add your partners.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="7d9cd-121">Válassza ki a **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-121">Select the **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. <span data-ttu-id="7d9cd-122">A partnerek paneljén válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-122">In the Partners blade, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. <span data-ttu-id="7d9cd-123">Adja meg a partner nevét, majd válasszon egy **minősítő**.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-123">Enter a name for your partner, then select a **Qualifier**.</span></span> <span data-ttu-id="7d9cd-124">Végül adja meg a **érték** dokumentumok, amelyek az alkalmazások azonosítása.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-124">Finally, enter a **Value** to help identify documents that come into your apps.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. <span data-ttu-id="7d9cd-125">A partner-létrehozási folyamat előrehaladásának megtekintéséhez válassza ki a *harang* értesítési ikon.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-125">To see the progress for your partner creation process, select the *bell* notification icon.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. <span data-ttu-id="7d9cd-126">Győződjön meg arról, hogy az új partnerek sikerült hozzáadni, jelölje be a **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-126">To confirm that your new partners were successfully added, select the **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    <span data-ttu-id="7d9cd-127">Miután kiválasztotta a partnerek csempe, azt is megtudhatja újonnan hozzáadott partnerek a partnerek a panelen.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-127">After you select the Partners tile, you'll also see  newly added partners in the Partners blade.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a><span data-ttu-id="7d9cd-128">Az integráció fiókban meglévő partnerek szerkesztése</span><span class="sxs-lookup"><span data-stu-id="7d9cd-128">How to edit existing partners in your integration account</span></span>

1. <span data-ttu-id="7d9cd-129">Válassza ki a **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-129">Select the **Partners** tile.</span></span>
2. <span data-ttu-id="7d9cd-130">Panel megnyitása után a partnerek, válassza ki a szerkeszteni kívánt partnert.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-130">After the Partners blade opens, select the partner you want to edit.</span></span>
3. <span data-ttu-id="7d9cd-131">Az a **frissítés Partner** csempére, hajtsa végre a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-131">On the **Update Partner** tile, make your changes.</span></span>
4. <span data-ttu-id="7d9cd-132">Miután elkészült, válassza ki azt **mentése**, vagy a Mégse gombra a módosítások, jelöljön ki **elvetése**.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-132">After you're done, choose **Save**, or to cancel your changes, select **Discard**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a><span data-ttu-id="7d9cd-133">Egy partner törlése</span><span class="sxs-lookup"><span data-stu-id="7d9cd-133">How to delete a partner</span></span>

1. <span data-ttu-id="7d9cd-134">Válassza ki a **partnerek** csempére.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-134">Select the **Partners** tile.</span></span>
2. <span data-ttu-id="7d9cd-135">A Partner után panel nyílik meg, válassza ki a törölni kívánt partnert.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-135">After the Partner blade opens, select the partner that you want to delete.</span></span>
3. <span data-ttu-id="7d9cd-136">Válasszon **törlése**.</span><span class="sxs-lookup"><span data-stu-id="7d9cd-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a><span data-ttu-id="7d9cd-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d9cd-137">Next steps</span></span>
* [<span data-ttu-id="7d9cd-138">További információ a megállapodások</span><span class="sxs-lookup"><span data-stu-id="7d9cd-138">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  

