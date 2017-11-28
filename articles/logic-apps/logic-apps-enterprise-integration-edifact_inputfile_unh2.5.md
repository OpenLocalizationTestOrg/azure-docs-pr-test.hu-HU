---
title: "Alkalmazások B2B edifact dekódolása aaaLogic megoldásához UNH2.5 - Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: 6d85242d0f828fa52cdc9689938f3ba1e51b1183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toohandle-edifact-documents-having-unh25-segment"></a><span data-ttu-id="ba432-103">Hogyan toohandle EDIFACT dokumentumok UNH2.5 rendelkező szegmens</span><span class="sxs-lookup"><span data-stu-id="ba432-103">How toohandle EDIFACT documents having UNH2.5 segment</span></span>
<span data-ttu-id="ba432-104">Ha UNH2.5 hello EDIFACT dokumentum szerepel, akkor használatos séma keresési.</span><span class="sxs-lookup"><span data-stu-id="ba432-104">When UNH2.5 is present in hello EDIFACT document, it is being used for schema lookup.</span></span> 

<span data-ttu-id="ba432-105">Példa: hello UNH mezője **EAN008** hello EDIFACT üzenetben</span><span class="sxs-lookup"><span data-stu-id="ba432-105">Example: hello UNH field is **EAN008** in hello EDIFACT message</span></span>  
<span data-ttu-id="ba432-106">UNH + SSDD1 + RENDELÉSEK: D: 03B: UN:**EAN008**"</span><span class="sxs-lookup"><span data-stu-id="ba432-106">UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'</span></span>  

<span data-ttu-id="ba432-107">Lépéseket toofollow toohandle köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="ba432-107">Steps toofollow toohandle hello message</span></span> 
1. <span data-ttu-id="ba432-108">Hello séma frissítése</span><span class="sxs-lookup"><span data-stu-id="ba432-108">Update hello schema</span></span>
2. <span data-ttu-id="ba432-109">Hello megállapodás beállításainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="ba432-109">Check hello agreement settings</span></span>  

## <a name="update-hello-schema"></a><span data-ttu-id="ba432-110">Hello séma frissítése</span><span class="sxs-lookup"><span data-stu-id="ba432-110">Update hello schema</span></span>
<span data-ttu-id="ba432-111">tooprocess üdvözlőüzenetére, toodeploy hello UNH2.5 gyökércsomópontjának neve rendelkező sémát kell.</span><span class="sxs-lookup"><span data-stu-id="ba432-111">tooprocess hello message, you need toodeploy a schema with hello UNH2.5 root node name.</span></span>  <span data-ttu-id="ba432-112">A megadott példa, hello séma legfelső szintű név lesz **EFACT_D03B_ORDERS_EAN008**</span><span class="sxs-lookup"><span data-stu-id="ba432-112">For given an example, hello schema root name would be **EFACT_D03B_ORDERS_EAN008**</span></span>  

<span data-ttu-id="ba432-113">Az egyes D03B_ORDERS az egy másik UNH2.5 szegmens toodeploy az egyéni séma kellene lennie.</span><span class="sxs-lookup"><span data-stu-id="ba432-113">For each D03B_ORDERS with a different UNH2.5 segment, you would have toodeploy an individual schema.</span></span>  

## <a name="add-schema-toohello-edifact-agreement"></a><span data-ttu-id="ba432-114">Adja hozzá a séma toohello EDIFACT-megállapodás</span><span class="sxs-lookup"><span data-stu-id="ba432-114">Add schema toohello EDIFACT agreement</span></span>
### <a name="edifact-decode"></a><span data-ttu-id="ba432-115">EDIFACT dekódolása</span><span class="sxs-lookup"><span data-stu-id="ba432-115">EDIFACT Decode</span></span>
<span data-ttu-id="ba432-116">tooDecode hello bejövő üzenet, hello séma konfigurálása hello EDIFACT megállapodás kap beállításai</span><span class="sxs-lookup"><span data-stu-id="ba432-116">tooDecode hello incoming message, configure hello schema in hello EDIFACT agreement receive settings</span></span>
1. <span data-ttu-id="ba432-117">Hello séma toohello integrációs fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ba432-117">Add hello schema toohello integration account</span></span>    
2. <span data-ttu-id="ba432-118">Hello séma konfigurálása a hello EDIFACT megállapodás megkapja a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ba432-118">Configure hello schema in hello EDIFACT agreement receive settings.</span></span> 
3. <span data-ttu-id="ba432-119">Jelöljön ki EDIFACT szerződést, majd **módosítsa a JSON**.</span><span class="sxs-lookup"><span data-stu-id="ba432-119">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="ba432-120">Hello Receive megállapodás UNH2.5 előnyei **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ba432-120">Add UNH2.5 value in hello Receive Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span></span>

### <a name="edifact-encode"></a><span data-ttu-id="ba432-121">EDIFACT kódolása</span><span class="sxs-lookup"><span data-stu-id="ba432-121">EDIFACT Encode</span></span>
<span data-ttu-id="ba432-122">tooEncode hello bejövő üzenet, hello séma hello EDIFACT megállapodás küldési beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ba432-122">tooEncode hello incoming message, configure hello schema in hello EDIFACT agreement send settings</span></span>
1. <span data-ttu-id="ba432-123">Hello séma toohello integrációs fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ba432-123">Add hello schema toohello integration account</span></span>    
2. <span data-ttu-id="ba432-124">Hello séma hello EDIFACT megállapodás küldési beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ba432-124">Configure hello schema in hello EDIFACT agreement send settings.</span></span> 
3. <span data-ttu-id="ba432-125">Jelöljön ki EDIFACT szerződést, majd **módosítsa a JSON**.</span><span class="sxs-lookup"><span data-stu-id="ba432-125">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="ba432-126">Hello küldése megállapodás UNH2.5 előnyei **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span><span class="sxs-lookup"><span data-stu-id="ba432-126">Add UNH2.5 value in hello Send Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba432-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba432-127">Next Steps</span></span>
* [<span data-ttu-id="ba432-128">További információ az integrációs fiók megállapodások</span><span class="sxs-lookup"><span data-stu-id="ba432-128">Learn more about integration account agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  