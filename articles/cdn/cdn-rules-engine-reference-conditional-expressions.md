---
title: "aaaAzure CDN szabályok motor feltételes kifejezések |} Microsoft Docs"
description: "Az Azure CDN referenciadokumentációt szabályok motor egyezés feltételek és a szolgáltatások."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 39d0754c34a577f77ca87b6fd92e2b6a9e4ff8fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a><span data-ttu-id="82d15-103">Az Azure CDN-szabályok a feltételes kifejezések motor</span><span class="sxs-lookup"><span data-stu-id="82d15-103">Azure CDN rules engine conditional expressions</span></span>
<span data-ttu-id="82d15-104">Ez a témakör részletesen ismerteti a feltételes kifejezések hello az Azure Content Delivery Network (CDN) [szabálymotor](cdn-rules-engine.md).</span><span class="sxs-lookup"><span data-stu-id="82d15-104">This topic lists detailed descriptions of hello Conditional Expressions for Azure Content Delivery Network (CDN) [Rules Engine](cdn-rules-engine.md).</span></span>

<span data-ttu-id="82d15-105">hello első szabály része hello feltételes kifejezés.</span><span class="sxs-lookup"><span data-stu-id="82d15-105">hello first part of a rule is hello Conditional Expression.</span></span>

<span data-ttu-id="82d15-106">A feltételes kifejezés</span><span class="sxs-lookup"><span data-stu-id="82d15-106">Conditional Expression</span></span> | <span data-ttu-id="82d15-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="82d15-107">Description</span></span>
-----------------------|-------------
<span data-ttu-id="82d15-108">HA</span><span class="sxs-lookup"><span data-stu-id="82d15-108">IF</span></span> | <span data-ttu-id="82d15-109">Egy IF kifejezés mindig egy szabály következő utasításnak elsőként hello részét képezi.</span><span class="sxs-lookup"><span data-stu-id="82d15-109">An IF expression is always a part of hello first statement in a rule.</span></span> <span data-ttu-id="82d15-110">Mint minden más feltételes kifejezés ez IF utasítást kapcsolódó egyezést kell lennie.</span><span class="sxs-lookup"><span data-stu-id="82d15-110">Like all other conditional expressions, this IF statement must be associated with a match.</span></span> <span data-ttu-id="82d15-111">Ha nincsenek további feltételes kifejezések meghatározása a megfelelés, amelyeknek teljesülniük kell ahhoz funkciókat esetleg alkalmazott tooa kérelem hello feltételnek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="82d15-111">If no additional conditional expressions are defined, then this match determines hello criterion that must be met before a set of features may be applied tooa request.</span></span>
<span data-ttu-id="82d15-112">HA</span><span class="sxs-lookup"><span data-stu-id="82d15-112">AND IF</span></span> | <span data-ttu-id="82d15-113">ÉS IF kifejezés csak a következő típusú feltételes kifejezések: Ha, és ha hello után adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="82d15-113">An AND IF expression may only be added after hello following types of conditional expressions:IF,AND IF.</span></span> <span data-ttu-id="82d15-114">Azt jelzi, hogy van-e egy másik feltétel, amelyeknek teljesülniük kell hello kezdeti IF utasítást.</span><span class="sxs-lookup"><span data-stu-id="82d15-114">It indicates that there is another condition that must be met for hello initial IF statement.</span></span>
<span data-ttu-id="82d15-115">MÁSKÜLÖNBEN HA</span><span class="sxs-lookup"><span data-stu-id="82d15-115">ELSE IF</span></span>| <span data-ttu-id="82d15-116">MÁS IF kifejezés határozza meg a szolgáltatások adott toothis más IF utasítást készlete előtt teljesítendő alternatív feltételt.</span><span class="sxs-lookup"><span data-stu-id="82d15-116">An ELSE IF expression specifies an alternative condition that must be met before a set of features specific toothis ELSE IF statement takes place.</span></span> <span data-ttu-id="82d15-117">az ELSE IF utasítást hello jelenléte azt jelzi, hogy hello előző utasítás hello végéhez.</span><span class="sxs-lookup"><span data-stu-id="82d15-117">hello presence of an ELSE IF statement indicates hello end of hello previous statement.</span></span> <span data-ttu-id="82d15-118">hello csak a feltételes kifejezés, amely után az ELSE IF utasítást egy másik más IF utasítást kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="82d15-118">hello only conditional expression that may be placed after an ELSE IF statement is another ELSE IF statement.</span></span> <span data-ttu-id="82d15-119">Ez azt jelenti, hogy az ELSE IF utasítást csak az használt toospecify egyetlen további feltétel teljesül toobe rendelkező lehet.</span><span class="sxs-lookup"><span data-stu-id="82d15-119">This means that an ELSE IF statement may only be used toospecify a single additional condition that has toobe met.</span></span>

<span data-ttu-id="82d15-120">**Példa**: ![CDN feltétel felel meg.](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span><span class="sxs-lookup"><span data-stu-id="82d15-120">**Example**: ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)</span></span>

 > [!TIP]
   > <span data-ttu-id="82d15-121">Egy későbbi szabály felülbírálhatják az előző szabályok által megadott hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="82d15-121">A subsequent rule may override hello actions specified by a previous rule.</span></span> <span data-ttu-id="82d15-122">Példa: Általános szabály bérlőkulcshoz kapcsolódó összes kérelem jogkivonat-alapú hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="82d15-122">Example: A catch-all rule secures all requests via Token-Based Authentication.</span></span> <span data-ttu-id="82d15-123">Egy másik szabály lehet létrehozni közvetlenül alatta toomake bizonyos típusú kérelmet egy kivételt.</span><span class="sxs-lookup"><span data-stu-id="82d15-123">Another rule may be created directly below it toomake an exception for certain types of requests.</span></span>

### <a name="next-steps"></a><span data-ttu-id="82d15-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82d15-124">Next steps</span></span>
* [<span data-ttu-id="82d15-125">Az Azure CDN áttekintése</span><span class="sxs-lookup"><span data-stu-id="82d15-125">Azure CDN Overview</span></span>](cdn-overview.md)
* [<span data-ttu-id="82d15-126">Szabályok motor referencia</span><span class="sxs-lookup"><span data-stu-id="82d15-126">Rules Engine Reference</span></span>](cdn-rules-engine-reference.md)
* [<span data-ttu-id="82d15-127">Szabályok motor egyezés feltételek</span><span class="sxs-lookup"><span data-stu-id="82d15-127">Rules Engine Match Conditions</span></span>](cdn-rules-engine-reference-match-conditions.md)
* [<span data-ttu-id="82d15-128">Szabályok adatbázismotor-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="82d15-128">Rules Engine Features</span></span>](cdn-rules-engine-reference-features.md)
* [<span data-ttu-id="82d15-129">Alapértelmezett HTTP működés használata hello szabályok felülbírálása</span><span class="sxs-lookup"><span data-stu-id="82d15-129">Overriding default HTTP behavior using hello rules engine</span></span>](cdn-rules-engine.md)
