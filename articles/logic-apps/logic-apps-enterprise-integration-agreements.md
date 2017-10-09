---
title: "B2B kommunikációhoz - Azure Logic Apps aaaAgreements |} Microsoft Docs"
description: "Hozza létre az egyezményeket, partnerek B2B helyzetekben képes kommunikálni az Azure Logic Apps és hello vállalati integrációs csomag"
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
ms.openlocfilehash: 499edcbab1cd67fbc169e393c3cad7b81658a250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a><span data-ttu-id="ae529-103">Az Azure Logic Apps és vállalati integrációs csomag B2B kommunikációhoz egyezmények</span><span class="sxs-lookup"><span data-stu-id="ae529-103">Partner agreements for B2B communication with Azure Logic Apps and Enterprise Integration Pack</span></span>

<span data-ttu-id="ae529-104">Szerződések lehetővé teszik az üzleti entitásokat zökkenőmentesen az iparági szabványos protokollok segítségével kommunikálnak, és üzleti vállalatközi (B2B) kommunikáció hello legfontosabb feladatai közé tartoznak.</span><span class="sxs-lookup"><span data-stu-id="ae529-104">Agreements let business entities communicate seamlessly using industry standard protocols and are hello cornerstone for business-to-business (B2B) communication.</span></span> <span data-ttu-id="ae529-105">A logic apps hello vállalati integrációs csomag a B2B forgatókönyvei engedélyezésekor szerződés egy B2B kereskedelmi partnereknek közötti kommunikáció elrendezéssel.</span><span class="sxs-lookup"><span data-stu-id="ae529-105">When enabling B2B scenarios for logic apps with hello Enterprise Integration Pack, an agreement is a communications arrangement between B2B trading partners.</span></span> <span data-ttu-id="ae529-106">Ez a szerződés hello hello partnerek kívánt kommunikációs túl létrehozásához, és protokoll-vagy átviteli-specifikus alapul.</span><span class="sxs-lookup"><span data-stu-id="ae529-106">This agreement is based on hello communications that hello partners want too establish and is protocol or transport-specific.</span></span>

<span data-ttu-id="ae529-107">Vállalati integrációs protokoll vagy átviteli megfelelést támogatja:</span><span class="sxs-lookup"><span data-stu-id="ae529-107">Enterprise integration supports these protocol or transport standards:</span></span>

* [<span data-ttu-id="ae529-108">AS2</span><span class="sxs-lookup"><span data-stu-id="ae529-108">AS2</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="ae529-109">X12</span><span class="sxs-lookup"><span data-stu-id="ae529-109">X12</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="ae529-110">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="ae529-110">EDIFACT</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a><span data-ttu-id="ae529-111">Miért érdemes használni a megállapodások</span><span class="sxs-lookup"><span data-stu-id="ae529-111">Why use agreements</span></span>

<span data-ttu-id="ae529-112">Szerződések használata esetén az alábbiakban néhány gyakori előnyei:</span><span class="sxs-lookup"><span data-stu-id="ae529-112">Here are some common benefits when using agreements:</span></span>

* <span data-ttu-id="ae529-113">Lehetővé teszi, hogy más szervezetek és vállalatok tooexchange adatokat egy jól ismert formátumban.</span><span class="sxs-lookup"><span data-stu-id="ae529-113">Enables different organizations and businesses tooexchange information in a well-known format.</span></span>
* <span data-ttu-id="ae529-114">Növeli a hatékonyságot, amikor a B2B műveleteket végző</span><span class="sxs-lookup"><span data-stu-id="ae529-114">Improves efficiency when conducting B2B transactions</span></span>
* <span data-ttu-id="ae529-115">Könnyen toocreate, kezelése és integrációs alkalmazások vállalati létrehozásakor használt</span><span class="sxs-lookup"><span data-stu-id="ae529-115">Easy toocreate, manage, and use when creating enterprise integration apps</span></span>

## <a name="how-toocreate-agreements"></a><span data-ttu-id="ae529-116">Hogyan toocreate megállapodások</span><span class="sxs-lookup"><span data-stu-id="ae529-116">How toocreate agreements</span></span>

* [<span data-ttu-id="ae529-117">AS2-egyezmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae529-117">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="ae529-118">Hozzon létre egy X12 megállapodás</span><span class="sxs-lookup"><span data-stu-id="ae529-118">Create an X12 agreement</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="ae529-119">EDIFACT-egyezmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae529-119">Create an EDIFACT agreement</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="how-toouse-an-agreement"></a><span data-ttu-id="ae529-120">Hogyan toouse szerződés</span><span class="sxs-lookup"><span data-stu-id="ae529-120">How toouse an agreement</span></span>

<span data-ttu-id="ae529-121">Létrehozhat [a logic apps](logic-apps-what-are-logic-apps.md "további információ a Logic apps") B2B képességekkel létrehozott szerződés használatával.</span><span class="sxs-lookup"><span data-stu-id="ae529-121">You can create [logic apps](logic-apps-what-are-logic-apps.md "Learn about Logic apps") with B2B capabilities by using an agreement that you created.</span></span>

## <a name="how-tooedit-an-agreement"></a><span data-ttu-id="ae529-122">Hogyan tooedit szerződés</span><span class="sxs-lookup"><span data-stu-id="ae529-122">How tooedit an agreement</span></span>

<span data-ttu-id="ae529-123">Ilyen szerkesztheti az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ae529-123">You can edit any agreement by following these steps:</span></span>

1. <span data-ttu-id="ae529-124">Válassza ki a kívánt tooupdate hello megállapodás rendelkező hello integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="ae529-124">Select hello integration account that has hello agreement you want tooupdate.</span></span>

2. <span data-ttu-id="ae529-125">Válassza ki a hello **megállapodások** csempére.</span><span class="sxs-lookup"><span data-stu-id="ae529-125">Choose hello **Agreements** tile.</span></span>

3. <span data-ttu-id="ae529-126">A hello **megállapodások** panelen, jelölje be hello szerződést.</span><span class="sxs-lookup"><span data-stu-id="ae529-126">On hello **Agreements** blade, select hello agreement.</span></span>

4. <span data-ttu-id="ae529-127">Válasszon **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="ae529-127">Choose **Edit**.</span></span> <span data-ttu-id="ae529-128">A módosításokat.</span><span class="sxs-lookup"><span data-stu-id="ae529-128">Make your changes.</span></span>

5. <span data-ttu-id="ae529-129">toosave a módosításokat, válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae529-129">toosave your changes, choose **OK**.</span></span>

## <a name="how-toodelete-an-agreement"></a><span data-ttu-id="ae529-130">Hogyan toodelete szerződés</span><span class="sxs-lookup"><span data-stu-id="ae529-130">How toodelete an agreement</span></span>

<span data-ttu-id="ae529-131">Ilyen törölheti a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="ae529-131">You can delete any agreement by following these steps:</span></span>

1. <span data-ttu-id="ae529-132">Válassza ki a kívánt toodelete hello megállapodás rendelkező hello integrációs fiók.</span><span class="sxs-lookup"><span data-stu-id="ae529-132">Select hello integration account that has hello agreement you want toodelete.</span></span>
2. <span data-ttu-id="ae529-133">Válassza ki a hello **megállapodások** csempére.</span><span class="sxs-lookup"><span data-stu-id="ae529-133">Choose hello **Agreements** tile.</span></span>
3. <span data-ttu-id="ae529-134">A hello **megállapodások** panelen, jelölje be hello szerződést.</span><span class="sxs-lookup"><span data-stu-id="ae529-134">On hello **Agreements** blade, select hello agreement.</span></span>
4. <span data-ttu-id="ae529-135">Válasszon **törlése**.</span><span class="sxs-lookup"><span data-stu-id="ae529-135">Choose **Delete**.</span></span>
5. <span data-ttu-id="ae529-136">Győződjön meg arról, hogy szeretné-e toodelete hello kijelölt szerződés.</span><span class="sxs-lookup"><span data-stu-id="ae529-136">Confirm that you want toodelete hello selected agreement.</span></span>

    <span data-ttu-id="ae529-137">hello megállapodások panel nem jelenik meg a törölt hello szerződést.</span><span class="sxs-lookup"><span data-stu-id="ae529-137">hello Agreements blade no longer shows hello deleted agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae529-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae529-138">Next steps</span></span>
* [<span data-ttu-id="ae529-139">AS2-egyezmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae529-139">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
