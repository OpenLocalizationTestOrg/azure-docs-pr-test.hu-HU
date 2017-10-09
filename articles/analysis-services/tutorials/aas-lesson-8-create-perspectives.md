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
# <a name="lesson-8-create-perspectives"></a>8. lecke: Perspektívák létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a leckében internetes értékesítési perspektívát hozhat létre. A perspektíva a modell egy olyan megtekinthető alkészletét határozza meg, amely célzott, üzletspecifikus vagy alkalmazásspecifikus szempontokat biztosít. Amikor egy felhasználó tooa modell perspektívát használatával kapcsolódik, láthatják modell objektumokra (táblákat, oszlopokat, mértékeket, hierarchiák és KPI-k) a perspektívában definiált mezőként. több, lásd: toolearn [perspektívák](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).
  
hello hoz létre, ez a lecke Internet értékesítési perspektíva hello DimCustomer tábla objektum nem tartalmazza. Amikor létrehoz egy perspektívát, amely nem tartalmazza az egyes objektumok nézetből, az objektum továbbra is létezik hello modell. Ez ugyanakkor nem látható a jelentéskészítő ügyfélmező listájában. A számított oszlopok és mértékek attól függetlenül végezhetnek számításokat a kizárt objektumadatokból, hogy egy perspektíva részét képezik-e.  
  
hello Ez a lecke célja toodescribe hogyan toocreate szempontok és a táblázatos modell hello szerkesztőeszközök megismerése. Ha később bontsa ki a modell tooinclude további táblákon, toodefine különböző szempontjait hello modell, például a leltározási és a Sales további szempontok hozhat létre.  
  
Becsült idő toocomplete Ez a lecke: **öt perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 7: hozzon létre a fő teljesítménymutatók](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  
  
## <a name="create-perspectives"></a>Perspektívák létrehozása  
  
#### <a name="toocreate-an-internet-sales-perspective"></a>toocreate egy Internet értékesítési perspektíva  
  
1.  Kattintson a hello **modell** menü > **perspektívák** > **létrehozása és kezelése**.  
  
2.  A hello **perspektívák** párbeszédpanel, kattintson a **új perspektíva**.  
  
3.  Kattintson duplán a hello **új perspektíva** oszlop fejlécére, majd nevezze át **Internet értékesítési**.  
  
4.  Válassza ki az összes hello táblák hello *kivéve* **DimCustomer**.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    Egy újabb lecke használhatja hello elemzés az Excelben funkciót tootest ehhez a perspektívához. hello Excel kimutatástáblázatot mezők hello DimCustomer tábla kivételével minden táblázat tartalmazza.  

## <a name="whats-next"></a>A következő lépések
[9. lecke: Hierarchiák létrehozása](../tutorials/aas-lesson-9-create-hierarchies.md).
  
  
  
  
