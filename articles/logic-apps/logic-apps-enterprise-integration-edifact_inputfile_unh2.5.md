---
title: "Logic Apps B2B edifact dekódolása megoldásához UNH2.5 - Azure Logic Apps |} Microsoft Docs"
description: "Az Azure Logic Apps B2B edifact dekódolása UNH2.5 feloldása"
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
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 62ad8183cc6e9f56255b2729a04ee7710d00a21a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-handle-edifact-documents-having-unh25-segment"></a><span data-ttu-id="ef289-103">EDIFACT dokumentumokon UNH2.5 szegmens kezelésének módját</span><span class="sxs-lookup"><span data-stu-id="ef289-103">How to handle EDIFACT documents having UNH2.5 segment</span></span>
<span data-ttu-id="ef289-104">UNH2.5 megtalálható-e a EDIFACT dokumentumot, ha használatban van séma kereséshez.</span><span class="sxs-lookup"><span data-stu-id="ef289-104">When UNH2.5 is present in the EDIFACT document, it is being used for schema lookup.</span></span> 

<span data-ttu-id="ef289-105">Példa: A UNH mezője **EAN008** EDIFACT üzenetben</span><span class="sxs-lookup"><span data-stu-id="ef289-105">Example: The UNH field is **EAN008** in the EDIFACT message</span></span>  
<span data-ttu-id="ef289-106">UNH + SSDD1 + RENDELÉSEK: D: 03B: UN:**EAN008**"</span><span class="sxs-lookup"><span data-stu-id="ef289-106">UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'</span></span>  

<span data-ttu-id="ef289-107">Az üzenet kezelni a követendő lépések</span><span class="sxs-lookup"><span data-stu-id="ef289-107">Steps to follow to handle the message</span></span> 
1. <span data-ttu-id="ef289-108">A séma frissítése</span><span class="sxs-lookup"><span data-stu-id="ef289-108">Update the schema</span></span>
2. <span data-ttu-id="ef289-109">A szerződés beállításainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="ef289-109">Check the agreement settings</span></span>  

## <a name="update-the-schema"></a><span data-ttu-id="ef289-110">A séma frissítése</span><span class="sxs-lookup"><span data-stu-id="ef289-110">Update the schema</span></span>
<span data-ttu-id="ef289-111">Az üzenet feldolgozására is telepíteni kell a sémát UNH2.5 gyökércsomópontjának neve.</span><span class="sxs-lookup"><span data-stu-id="ef289-111">To process the message, you need to deploy a schema with the UNH2.5 root node name.</span></span>  <span data-ttu-id="ef289-112">A megadott példa, a séma legfelső szintű név lesz **EFACT_D03B_ORDERS_EAN008**</span><span class="sxs-lookup"><span data-stu-id="ef289-112">For given an example, the schema root name would be **EFACT_D03B_ORDERS_EAN008**</span></span>  

<span data-ttu-id="ef289-113">Az egyes D03B_ORDERS az egy másik UNH2.5 szegmens kellene az egyéni séma telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ef289-113">For each D03B_ORDERS with a different UNH2.5 segment, you would have to deploy an individual schema.</span></span>  

## <a name="add-schema-to-the-edifact-agreement"></a><span data-ttu-id="ef289-114">Adja hozzá a séma a EDIFACT-megállapodás</span><span class="sxs-lookup"><span data-stu-id="ef289-114">Add schema to the EDIFACT agreement</span></span>
### <a name="edifact-decode"></a><span data-ttu-id="ef289-115">EDIFACT dekódolása</span><span class="sxs-lookup"><span data-stu-id="ef289-115">EDIFACT Decode</span></span>
<span data-ttu-id="ef289-116">A bejövő üzenet dekódolni, konfigurálja a séma a EDIFACT megállapodás kap beállításai</span><span class="sxs-lookup"><span data-stu-id="ef289-116">To Decode the incoming message, configure the schema in the EDIFACT agreement receive settings</span></span>
1. <span data-ttu-id="ef289-117">A séma az integráció fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ef289-117">Add the schema to the integration account</span></span>    
2. <span data-ttu-id="ef289-118">A séma konfigurálása a EDIFACT a szerződés megkapja a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ef289-118">Configure the schema in the EDIFACT agreement receive settings.</span></span> 
3. <span data-ttu-id="ef289-119">Jelöljön ki EDIFACT szerződést, majd **módosítsa a JSON**.</span><span class="sxs-lookup"><span data-stu-id="ef289-119">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="ef289-120">A fogadási szerződés UNH2.5 előnyei **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ef289-120">Add UNH2.5 value in the Receive Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span></span>

### <a name="edifact-encode"></a><span data-ttu-id="ef289-121">EDIFACT kódolása</span><span class="sxs-lookup"><span data-stu-id="ef289-121">EDIFACT Encode</span></span>
<span data-ttu-id="ef289-122">A bejövő üzenet kódolására, konfigurálja a séma EDIFACT megállapodás küldési beállításai</span><span class="sxs-lookup"><span data-stu-id="ef289-122">To Encode the incoming message, configure the schema in the EDIFACT agreement send settings</span></span>
1. <span data-ttu-id="ef289-123">A séma az integráció fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ef289-123">Add the schema to the integration account</span></span>    
2. <span data-ttu-id="ef289-124">Adja meg a séma a EDIFACT megállapodás küldés beállításait.</span><span class="sxs-lookup"><span data-stu-id="ef289-124">Configure the schema in the EDIFACT agreement send settings.</span></span> 
3. <span data-ttu-id="ef289-125">Jelöljön ki EDIFACT szerződést, majd **módosítsa a JSON**.</span><span class="sxs-lookup"><span data-stu-id="ef289-125">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="ef289-126">A Küldés szerződés UNH2.5 előnyei **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span><span class="sxs-lookup"><span data-stu-id="ef289-126">Add UNH2.5 value in the Send Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef289-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef289-127">Next Steps</span></span>
* [<span data-ttu-id="ef289-128">További információ az integrációs fiók megállapodások</span><span class="sxs-lookup"><span data-stu-id="ef289-128">Learn more about integration account agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  