---
title: "B2B kommunikációs - Azure Logic Apps-megállapodások |} Microsoft Docs"
description: "Hozza létre az egyezményeket, partnerek B2B helyzetekben képes kommunikálni az Azure Logic Apps és a vállalati integrációs csomag"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: LADocs
ms.openlocfilehash: 7ce0860272901f3b4e4cf3d63f7361d539f64741
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a><span data-ttu-id="b10ee-103">Az Azure Logic Apps és vállalati integrációs csomag B2B kommunikációhoz egyezmények</span><span class="sxs-lookup"><span data-stu-id="b10ee-103">Partner agreements for B2B communication with Azure Logic Apps and Enterprise Integration Pack</span></span>

<span data-ttu-id="b10ee-104">Szerződések lehetővé teszik az üzleti entitásokat zökkenőmentesen az iparági szabványos protokollok segítségével kommunikálnak, és a legfontosabb feladatai közé tartoznak az üzleti vállalatközi (B2B) kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="b10ee-104">Agreements let business entities communicate seamlessly using industry standard protocols and are the cornerstone for business-to-business (B2B) communication.</span></span> <span data-ttu-id="b10ee-105">A logic apps a vállalati integrációs csomaggal B2B forgatókönyvei engedélyezésekor szerződés egy B2B kereskedelmi partnereknek közötti kommunikáció elrendezéssel.</span><span class="sxs-lookup"><span data-stu-id="b10ee-105">When enabling B2B scenarios for logic apps with the Enterprise Integration Pack, an agreement is a communications arrangement between B2B trading partners.</span></span> <span data-ttu-id="b10ee-106">Ez a szerződés alapján, hogy a partnerek szeretne létrehozni, és a protokoll a kommunikáció vagy átviteli-specifikus.</span><span class="sxs-lookup"><span data-stu-id="b10ee-106">This agreement is based on the communications that the partners want to establish and is protocol or transport-specific.</span></span>

<span data-ttu-id="b10ee-107">Vállalati integrációs protokoll vagy átviteli megfelelést támogatja:</span><span class="sxs-lookup"><span data-stu-id="b10ee-107">Enterprise integration supports these protocol or transport standards:</span></span>

* [<span data-ttu-id="b10ee-108">AS2</span><span class="sxs-lookup"><span data-stu-id="b10ee-108">AS2</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="b10ee-109">X12</span><span class="sxs-lookup"><span data-stu-id="b10ee-109">X12</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="b10ee-110">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="b10ee-110">EDIFACT</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a><span data-ttu-id="b10ee-111">Miért érdemes használni a megállapodások</span><span class="sxs-lookup"><span data-stu-id="b10ee-111">Why use agreements</span></span>

<span data-ttu-id="b10ee-112">Szerződések használata esetén az alábbiakban néhány gyakori előnyei:</span><span class="sxs-lookup"><span data-stu-id="b10ee-112">Here are some common benefits when using agreements:</span></span>

* <span data-ttu-id="b10ee-113">Lehetővé teszi a különböző szervezetek és vállalatok számára az exchange-adatokat egy jól ismert formátumban.</span><span class="sxs-lookup"><span data-stu-id="b10ee-113">Enables different organizations and businesses to exchange information in a well-known format.</span></span>
* <span data-ttu-id="b10ee-114">Növeli a hatékonyságot, amikor a B2B műveleteket végző</span><span class="sxs-lookup"><span data-stu-id="b10ee-114">Improves efficiency when conducting B2B transactions</span></span>
* <span data-ttu-id="b10ee-115">Könnyen létrehozására, kezelésére és integrációs alkalmazások vállalati létrehozásakor használt</span><span class="sxs-lookup"><span data-stu-id="b10ee-115">Easy to create, manage, and use when creating enterprise integration apps</span></span>

## <a name="how-to-create-agreements"></a><span data-ttu-id="b10ee-116">Hogyan hozza létre az egyezményeket</span><span class="sxs-lookup"><span data-stu-id="b10ee-116">How to create agreements</span></span>

* [<span data-ttu-id="b10ee-117">AS2-egyezmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="b10ee-117">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="b10ee-118">Hozzon létre egy X12 megállapodás</span><span class="sxs-lookup"><span data-stu-id="b10ee-118">Create an X12 agreement</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="b10ee-119">EDIFACT-egyezmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="b10ee-119">Create an EDIFACT agreement</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="how-to-use-an-agreement"></a><span data-ttu-id="b10ee-120">Szerződés használata</span><span class="sxs-lookup"><span data-stu-id="b10ee-120">How to use an agreement</span></span>

<span data-ttu-id="b10ee-121">Létrehozhat [a logic apps](logic-apps-what-are-logic-apps.md "további információ a Logic apps") B2B képességekkel létrehozott szerződés használatával.</span><span class="sxs-lookup"><span data-stu-id="b10ee-121">You can create [logic apps](logic-apps-what-are-logic-apps.md "Learn about Logic apps") with B2B capabilities by using an agreement that you created.</span></span>

## <a name="how-to-edit-an-agreement"></a><span data-ttu-id="b10ee-122">Szerződés szerkesztése</span><span class="sxs-lookup"><span data-stu-id="b10ee-122">How to edit an agreement</span></span>

<span data-ttu-id="b10ee-123">Ilyen szerkesztheti az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b10ee-123">You can edit any agreement by following these steps:</span></span>

1. <span data-ttu-id="b10ee-124">Válassza ki a frissíteni kívánt szerződés rendelkező integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="b10ee-124">Select the integration account that has the agreement you want to update.</span></span>

2. <span data-ttu-id="b10ee-125">Válassza ki a **megállapodások** csempére.</span><span class="sxs-lookup"><span data-stu-id="b10ee-125">Choose the **Agreements** tile.</span></span>

3. <span data-ttu-id="b10ee-126">Az a **megállapodások** panelen válassza ki a szerződést.</span><span class="sxs-lookup"><span data-stu-id="b10ee-126">On the **Agreements** blade, select the agreement.</span></span>

4. <span data-ttu-id="b10ee-127">Válasszon **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="b10ee-127">Choose **Edit**.</span></span> <span data-ttu-id="b10ee-128">A módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b10ee-128">Make your changes.</span></span>

5. <span data-ttu-id="b10ee-129">A módosítások mentéséhez válasszon **OK**.</span><span class="sxs-lookup"><span data-stu-id="b10ee-129">To save your changes, choose **OK**.</span></span>

## <a name="how-to-delete-an-agreement"></a><span data-ttu-id="b10ee-130">Szerződés törlése</span><span class="sxs-lookup"><span data-stu-id="b10ee-130">How to delete an agreement</span></span>

<span data-ttu-id="b10ee-131">Ilyen törölheti a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="b10ee-131">You can delete any agreement by following these steps:</span></span>

1. <span data-ttu-id="b10ee-132">Válassza ki a törölni kívánt szerződés rendelkező integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="b10ee-132">Select the integration account that has the agreement you want to delete.</span></span>
2. <span data-ttu-id="b10ee-133">Válassza ki a **megállapodások** csempére.</span><span class="sxs-lookup"><span data-stu-id="b10ee-133">Choose the **Agreements** tile.</span></span>
3. <span data-ttu-id="b10ee-134">Az a **megállapodások** panelen válassza ki a szerződést.</span><span class="sxs-lookup"><span data-stu-id="b10ee-134">On the **Agreements** blade, select the agreement.</span></span>
4. <span data-ttu-id="b10ee-135">Válasszon **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b10ee-135">Choose **Delete**.</span></span>
5. <span data-ttu-id="b10ee-136">Győződjön meg arról, hogy valóban törli a kijelölt szerződést.</span><span class="sxs-lookup"><span data-stu-id="b10ee-136">Confirm that you want to delete the selected agreement.</span></span>

    <span data-ttu-id="b10ee-137">A szerződések panel nem jelenik meg a törölt szerződést.</span><span class="sxs-lookup"><span data-stu-id="b10ee-137">The Agreements blade no longer shows the deleted agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b10ee-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b10ee-138">Next steps</span></span>
* [<span data-ttu-id="b10ee-139">AS2-egyezmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="b10ee-139">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
