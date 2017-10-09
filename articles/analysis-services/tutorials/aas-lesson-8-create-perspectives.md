---
title: "AAA \"Azure Analysis Services-oktatóanyag lecke 8 létrehozása perspektívák |} Microsoft dokumentumok\""
description: "Ismerteti, hogyan toocreate nézetből hello Azure Analysis Services-oktatóanyag projekt."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: 25391813e1969ecb22af4d6f9c1ccd8358d812fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-8-create-perspectives"></a><span data-ttu-id="fdc34-103">8. lecke: Perspektívák létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdc34-103">Lesson 8: Create perspectives</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="fdc34-104">Ebben a leckében internetes értékesítési perspektívát hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="fdc34-104">In this lesson, you create an Internet Sales perspective.</span></span> <span data-ttu-id="fdc34-105">A perspektíva a modell egy olyan megtekinthető alkészletét határozza meg, amely célzott, üzletspecifikus vagy alkalmazásspecifikus szempontokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="fdc34-105">A perspective defines a viewable subset of a model that provides focused, business-specific, or application-specific viewpoints.</span></span> <span data-ttu-id="fdc34-106">Amikor egy felhasználó tooa modell perspektívát használatával kapcsolódik, láthatják modell objektumokra (táblákat, oszlopokat, mértékeket, hierarchiák és KPI-k) a perspektívában definiált mezőként.</span><span class="sxs-lookup"><span data-stu-id="fdc34-106">When a user connects tooa model by using a perspective, they see only those model objects (tables, columns, measures, hierarchies, and KPIs) as fields defined in that perspective.</span></span> <span data-ttu-id="fdc34-107">több, lásd: toolearn [perspektívák](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="fdc34-107">toolearn more, see [Perspectives](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span></span>
  
<span data-ttu-id="fdc34-108">hello hoz létre, ez a lecke Internet értékesítési perspektíva hello DimCustomer tábla objektum nem tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fdc34-108">hello Internet Sales perspective you create in this lesson excludes hello DimCustomer table object.</span></span> <span data-ttu-id="fdc34-109">Amikor létrehoz egy perspektívát, amely nem tartalmazza az egyes objektumok nézetből, az objektum továbbra is létezik hello modell.</span><span class="sxs-lookup"><span data-stu-id="fdc34-109">When you create a perspective that excludes certain objects from view, that object still exists in hello model.</span></span> <span data-ttu-id="fdc34-110">Ez ugyanakkor nem látható a jelentéskészítő ügyfélmező listájában.</span><span class="sxs-lookup"><span data-stu-id="fdc34-110">However, it is not visible in a reporting client field list.</span></span> <span data-ttu-id="fdc34-111">A számított oszlopok és mértékek attól függetlenül végezhetnek számításokat a kizárt objektumadatokból, hogy egy perspektíva részét képezik-e.</span><span class="sxs-lookup"><span data-stu-id="fdc34-111">Calculated columns and measures either included in a perspective or not can still calculate from object data that is excluded.</span></span>  
  
<span data-ttu-id="fdc34-112">hello Ez a lecke célja toodescribe hogyan toocreate szempontok és a táblázatos modell hello szerkesztőeszközök megismerése.</span><span class="sxs-lookup"><span data-stu-id="fdc34-112">hello purpose of this lesson is toodescribe how toocreate perspectives and become familiar with hello tabular model authoring tools.</span></span> <span data-ttu-id="fdc34-113">Ha később bontsa ki a modell tooinclude további táblákon, toodefine különböző szempontjait hello modell, például a leltározási és a Sales további szempontok hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="fdc34-113">If you later expand this model tooinclude additional tables, you can create additional perspectives toodefine different viewpoints of hello model, for example, Inventory and Sales.</span></span>  
  
<span data-ttu-id="fdc34-114">Becsült idő toocomplete Ez a lecke: **öt perc**</span><span class="sxs-lookup"><span data-stu-id="fdc34-114">Estimated time toocomplete this lesson: **Five minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="fdc34-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fdc34-115">Prerequisites</span></span>  
<span data-ttu-id="fdc34-116">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="fdc34-116">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="fdc34-117">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 7: hozzon létre a fő teljesítménymutatók](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span><span class="sxs-lookup"><span data-stu-id="fdc34-117">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  
  
## <a name="create-perspectives"></a><span data-ttu-id="fdc34-118">Perspektívák létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdc34-118">Create perspectives</span></span>  
  
#### <a name="toocreate-an-internet-sales-perspective"></a><span data-ttu-id="fdc34-119">toocreate egy Internet értékesítési perspektíva</span><span class="sxs-lookup"><span data-stu-id="fdc34-119">toocreate an Internet Sales perspective</span></span>  
  
1.  <span data-ttu-id="fdc34-120">Kattintson a hello **modell** menü > **perspektívák** > **létrehozása és kezelése**.</span><span class="sxs-lookup"><span data-stu-id="fdc34-120">Click hello **Model** menu > **Perspectives** > **Create and Manage**.</span></span>  
  
2.  <span data-ttu-id="fdc34-121">A hello **perspektívák** párbeszédpanel, kattintson a **új perspektíva**.</span><span class="sxs-lookup"><span data-stu-id="fdc34-121">In hello **Perspectives** dialog box, click **New Perspective**.</span></span>  
  
3.  <span data-ttu-id="fdc34-122">Kattintson duplán a hello **új perspektíva** oszlop fejlécére, majd nevezze át **Internet értékesítési**.</span><span class="sxs-lookup"><span data-stu-id="fdc34-122">Double-click hello **New Perspective** column heading, and then rename **Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="fdc34-123">Válassza ki az összes hello táblák hello *kivéve* **DimCustomer**.</span><span class="sxs-lookup"><span data-stu-id="fdc34-123">Select hello all hello tables *except* **DimCustomer**.</span></span>  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    <span data-ttu-id="fdc34-125">Egy újabb lecke használhatja hello elemzés az Excelben funkciót tootest ehhez a perspektívához.</span><span class="sxs-lookup"><span data-stu-id="fdc34-125">In a later lesson, you use hello Analyze in Excel feature tootest this perspective.</span></span> <span data-ttu-id="fdc34-126">hello Excel kimutatástáblázatot mezők hello DimCustomer tábla kivételével minden táblázat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fdc34-126">hello Excel PivotTable Fields List includes each table except hello DimCustomer table.</span></span>  

## <a name="whats-next"></a><span data-ttu-id="fdc34-127">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdc34-127">What's next?</span></span>
<span data-ttu-id="fdc34-128">[9. lecke: Hierarchiák létrehozása](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="fdc34-128">[Lesson 9: Create hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>
  
  
  
  
