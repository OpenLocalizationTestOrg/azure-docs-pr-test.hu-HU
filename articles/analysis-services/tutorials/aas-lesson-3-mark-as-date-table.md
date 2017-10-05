---
title: "Azure Analysis Services oktatóanyag – 3. lecke: Megjelölés dátumtáblaként | Microsoft Docs"
description: "A lecke a dátumtáblák megjelölését ismerteti az Azure Analysis Services oktatóprojektjében."
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
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: c62f2726fef5219155a08b70c61162c914600d1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-3-mark-as-date-table"></a><span data-ttu-id="2b8aa-103">3. lecke: Megjelölés dátumtáblaként</span><span class="sxs-lookup"><span data-stu-id="2b8aa-103">Lesson 3: Mark as Date Table</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="2b8aa-104">A 2. leckében (Az adatok beszerzése) importálta a DimDate nevű táblát.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-104">In Lesson 2: Get data, you imported a dimension table named DimDate.</span></span> <span data-ttu-id="2b8aa-105">Ha a modellben ennek a táblának DimDate a neve, *Dátumtábla* néven is szokás nevezni, mivel ez tartalmazza a dátum- és időadatokat.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-105">While in your model this table is named DimDate, it can also be known as a *Date table*, in that it contains date and time data.</span></span>  
  
<span data-ttu-id="2b8aa-106">A DAX időintelligenciára vonatkozó funkcióinak használatakor, például ha a mértékegységeket később hozza létre, meg kell határoznia egy *Dátumtáblát* és egy egyedi azonosító *Dátumoszlopot* a táblában.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-106">Whenever you use DAX time-intelligence functions, like when you create measures later, you must specify properties which include a *Date table* and a unique identifier *Date column* in that table.</span></span>
  
<span data-ttu-id="2b8aa-107">Ebben a leckében megjelöli a DimDate táblát *Dátumtáblaként* és a Date oszlopot (a Dátumtáblában) *Dátumoszlopként* (egyedi azonosító).</span><span class="sxs-lookup"><span data-stu-id="2b8aa-107">In this lesson, you mark the DimDate table as the *Date table* and the Date column (in the Date table) as the *Date column* (unique identifier).</span></span>  

<span data-ttu-id="2b8aa-108">Mielőtt megjelölné a dátumtáblát és dátumoszlopot, érdemes lehet rendbe tenni a modellt, hogy könnyebben értelmezhető legyen.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-108">Before you mark the date table and date column, it's a good time to do a little housekeeping to make your model easier to understand.</span></span> <span data-ttu-id="2b8aa-109">A DimDate táblában figyelje meg a **FullDateAlternateKey** elnevezésű oszlopot.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-109">Notice in the DimDate table a column named **FullDateAlternateKey**.</span></span> <span data-ttu-id="2b8aa-110">Ez az oszlop egy-egy sort tartalmaz a táblában lévő mindegyik naptári naphoz.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-110">This column contains one row for every day in each calendar year included in the table.</span></span> <span data-ttu-id="2b8aa-111">Ezt az oszlopot sokat fogja használni a mértékegységképletekben és jelentésekben.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-111">You use this column a lot in measure formulas and in reports.</span></span> <span data-ttu-id="2b8aa-112">A FullDateAlternateKey elnevezés azonban nem túl jól azonosítja az oszlop funkcióját.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-112">But, FullDateAlternateKey isn't really a good identifier for this column.</span></span> <span data-ttu-id="2b8aa-113">Ha átkereszteli **Date** névre, könnyebb lesz azonosítania és felhasználnia a képletekben.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-113">You rename it to **Date**, making it easier to identify and include in formulas.</span></span> <span data-ttu-id="2b8aa-114">Amikor lehetséges, érdemes átneveznie a különféle objektumokat, azaz a táblákat és oszlopokat, hogy könnyebben azonosíthatók legyenek az SSDT-ben és az ügyfelek jelentési rendszereiben, például a Power BI-ba és az Excelben.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-114">Whenever possible, it's a good idea to rename objects like tables and columns to make them easier to identify in SSDT and client reporting applications like Power BI and Excel.</span></span> 
  
<span data-ttu-id="2b8aa-115">A lecke elvégzésének várható időtartama: **3 perc**.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-115">Estimated time to complete this lesson: **Three minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="2b8aa-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2b8aa-116">Prerequisites</span></span>  
<span data-ttu-id="2b8aa-117">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-117">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="2b8aa-118">A leckében foglalt feladatok végrehajtása előtt el kell végeznie az előző leckét ([2. lecke: Az adatok beszerzése](../tutorials/aas-lesson-2-get-data.md)).</span><span class="sxs-lookup"><span data-stu-id="2b8aa-118">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 2: Get data](../tutorials/aas-lesson-2-get-data.md).</span></span> 

### <a name="to-rename-the-fulldatealternatekey-column"></a><span data-ttu-id="2b8aa-119">A FullDateAlternateKey oszlop átnevezése</span><span class="sxs-lookup"><span data-stu-id="2b8aa-119">To rename the FullDateAlternateKey column</span></span>

1.  <span data-ttu-id="2b8aa-120">A modelltervezőben kattintson a **DimDate** táblára.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-120">In the model designer, click the **DimDate** table.</span></span>

2.  <span data-ttu-id="2b8aa-121">Kattintson duplán a **FullDateAlternateKey** oszlop fejlécére, majd nevezze át **Date** névre.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-121">Double-click the header for the **FullDateAlternateKey** column, and then rename it to **Date**.</span></span>

  
### <a name="to-set-mark-as-date-table"></a><span data-ttu-id="2b8aa-122">A Megjelölés dátumtáblaként beállítás megadása</span><span class="sxs-lookup"><span data-stu-id="2b8aa-122">To set Mark as Date Table</span></span>  
  
1.  <span data-ttu-id="2b8aa-123">Válassza ki a **Dátum** oszlopot, majd a **Tulajdonságok** ablakban az **Adattípus** alatt bizonyosodjon meg róla, hogy a **Dátum** lehetőség van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-123">Select the **Date** column, and then in the **Properties** window, under **Data Type**, make sure  **Date** is selected.</span></span>  
  
2.  <span data-ttu-id="2b8aa-124">Kattintson a **Tábla** menüre, majd a **Dátum**, és végül a **Megjelölés dátumtáblaként** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-124">Click the **Table** menu, then click **Date**, and then click **Mark as Date Table**.</span></span>  
  
3.  <span data-ttu-id="2b8aa-125">A **Megjelölés dátumtáblaként** párbeszédpanelen, a **Dátum** listában válassza a **Date** oszlopot egyedi azonosítóként.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-125">In the **Mark as Date Table** dialog box, in the **Date** listbox, select the **Date** column as the unique identifier.</span></span> <span data-ttu-id="2b8aa-126">Alapértelmezés szerint általában ez van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-126">It's usually selected by default.</span></span> <span data-ttu-id="2b8aa-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b8aa-127">Click **OK**.</span></span> 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a><span data-ttu-id="2b8aa-129">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b8aa-129">What's next?</span></span>
<span data-ttu-id="2b8aa-130">[4. lecke: Kapcsolatok létrehozása](../tutorials/aas-lesson-4-create-relationships.md)</span><span class="sxs-lookup"><span data-stu-id="2b8aa-130">[Lesson 4: Create relationships](../tutorials/aas-lesson-4-create-relationships.md).</span></span>
  
