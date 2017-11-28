---
title: "Az Azure Analysis Services oktatóanyaga – 10. lecke: Partíciók létrehozása | Microsoft Docs"
description: "A lecke a partíciók létrehozását ismerteti az Azure Analysis Services oktatóprojektjében."
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
ms.openlocfilehash: df74d9cbdcf4916c24955e491767589e72389155
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-10-create-partitions"></a><span data-ttu-id="fee7e-103">10. lecke: Partíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fee7e-103">Lesson 10: Create partitions</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="fee7e-104">Ebben a leckében partíciókat hoz létre a FactInternetSales tábla kisebb logikai részekre való felosztásához, amelyek a többi partíciótól függetlenül dolgozhatók fel (frissíthetők).</span><span class="sxs-lookup"><span data-stu-id="fee7e-104">In this lesson, you create partitions to divide the FactInternetSales table into smaller logical parts that can be processed (refreshed) independent of other partitions.</span></span> <span data-ttu-id="fee7e-105">Alapértelmezés szerint a modellekben foglalt táblák mindegyike egy partícióval rendelkezik, amely az adott tábla összes oszlopát és sorát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fee7e-105">By default, every table you include in your model has one partition, which includes all the table’s columns and rows.</span></span> <span data-ttu-id="fee7e-106">A FactInternetSales tábla esetében az adatokat éves bontásban szeretnénk kezelni, a táblában szereplő mind az öt év esetében évenként egy-egy partícióban.</span><span class="sxs-lookup"><span data-stu-id="fee7e-106">For the FactInternetSales table, we want to divide the data by year; one partition for each of the table’s five years.</span></span> <span data-ttu-id="fee7e-107">Ezután mindegyik partíció egymástól függetlenül kezelhető.</span><span class="sxs-lookup"><span data-stu-id="fee7e-107">Each partition can then be processed independently.</span></span> <span data-ttu-id="fee7e-108">További tudnivalókért lásd a [partíciókat](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="fee7e-108">To learn more, see [Partitions](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span></span> 
  
<span data-ttu-id="fee7e-109">A lecke elvégzésének várható időtartama: **15 perc**.</span><span class="sxs-lookup"><span data-stu-id="fee7e-109">Estimated time to complete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="fee7e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fee7e-110">Prerequisites</span></span>  
<span data-ttu-id="fee7e-111">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="fee7e-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="fee7e-112">A leckében foglalt feladatok végrehajtása előtt el kell végeznie az előző leckét ([9. lecke: Hierarchiák létrehozása](../tutorials/aas-lesson-9-create-hierarchies.md)).</span><span class="sxs-lookup"><span data-stu-id="fee7e-112">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 9: Create Hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>  
  
## <a name="create-partitions"></a><span data-ttu-id="fee7e-113">Partíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fee7e-113">Create partitions</span></span>  
  
#### <a name="to-create-partitions-in-the-factinternetsales-table"></a><span data-ttu-id="fee7e-114">A FactInternetSales tábla partícióinak létrehozása</span><span class="sxs-lookup"><span data-stu-id="fee7e-114">To create partitions in the FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="fee7e-115">A Tabular Model Explorerben bontsa ki a **Táblák** fát, és kattintson a jobb gombbal a **FactInternetSales** > **Partíciók** elemre.</span><span class="sxs-lookup"><span data-stu-id="fee7e-115">In Tabular Model Explorer, expand **Tables**, and then right-click **FactInternetSales** > **Partitions**.</span></span>  
  
2.  <span data-ttu-id="fee7e-116">A partíciókezelőben kattintson a **Másolás** parancsra, és változtassa a nevet **FactInternetSales2010** értékre.</span><span class="sxs-lookup"><span data-stu-id="fee7e-116">In Partition Manager, click **Copy**, and then change the name to **FactInternetSales2010**.</span></span>
  
    <span data-ttu-id="fee7e-117">Mivel a partícióba csak egy adott időszakhoz (a 2010-es évhez) tartozó sorokat szeretne belefoglalni, módosítania kell a lekérdezés kifejezését.</span><span class="sxs-lookup"><span data-stu-id="fee7e-117">Because you want the partition to include only those rows within a certain period, for the year 2010, you must modify the query expression.</span></span>
  
4.  <span data-ttu-id="fee7e-118">Kattintson a **Tervezés** gombra a lekérdezésszerkesztő megnyitásához, majd kattintson a **FactInternetSales2010** lekérdezésre.</span><span class="sxs-lookup"><span data-stu-id="fee7e-118">Click **Design** to open Query Editor, and then click the **FactInternetSales2010** query.</span></span>

5.  <span data-ttu-id="fee7e-119">Az előnézetben kattintson az **OrderDate** sor fejlécére, majd a **Dátum-/időszűrők** > **Között** elemére.</span><span class="sxs-lookup"><span data-stu-id="fee7e-119">In preview, click the down arrow in the **OrderDate** column heading, and then click **Date/Time Filters** > **Between**.</span></span>

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  <span data-ttu-id="fee7e-121">A Sorok szűrése párbeszédpanelen, a **Sorok mutatása, ahol: OrderDate** mezőnél hagyja meg az **ez utáni vagy egyenlő** beállítást, majd a dátummezőben adja meg az **1/1/2010** értéket.</span><span class="sxs-lookup"><span data-stu-id="fee7e-121">In the Filter Rows dialog box, in **Show rows where: OrderDate**, leave **is after or equal to**, and then in the date field, enter **1/1/2010**.</span></span> <span data-ttu-id="fee7e-122">Hagyja az **És** operátort kiválasztva, majd válassza a **korábbi a következőnél** beállítást, a dátummezőben adja meg az **1/1/2011** értéket, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fee7e-122">Leave the **And** operator selected, then select **is before**, then in the date field, enter **1/1/2011**, and then click **OK**.</span></span>

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    <span data-ttu-id="fee7e-124">Észreveheti, hogy a lekérdezésszerkesztőben az ALKALMAZOTT LÉPÉSEK között egy további lépés látható, Szűrt sorok néven.</span><span class="sxs-lookup"><span data-stu-id="fee7e-124">Notice in Query Editor, in APPLIED STEPS, you see another step named Filtered Rows.</span></span> <span data-ttu-id="fee7e-125">Ezzel a szűrővel a 2010-es dátummal rendelkező megrendelésekre lehet szűrni.</span><span class="sxs-lookup"><span data-stu-id="fee7e-125">This filter is to select only order dates from 2010.</span></span>

8.  <span data-ttu-id="fee7e-126">Kattintson az **Importálás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fee7e-126">Click **Import**.</span></span>

    <span data-ttu-id="fee7e-127">A partíciókezelőben látható, hogy a lekérdezési kifejezés egy újabb szűrt sorok záradékkal bővült.</span><span class="sxs-lookup"><span data-stu-id="fee7e-127">In Partition Manager, notice the query expression now has an additional Filtered Rows clause.</span></span>

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    <span data-ttu-id="fee7e-129">Ez az utasítás meghatározza, hogy a partíció csak olyan sorok adatait tartalmazhatja, ahol az OrderDate a 2010-es naptári évre esik, ahogy azt a szűrt sorok záradék megadja.</span><span class="sxs-lookup"><span data-stu-id="fee7e-129">This statement specifies this partition should include only the data in those rows where the OrderDate is in the 2010 calendar year as specified in the filtered rows clause.</span></span>  
  
  
#### <a name="to-create-a-partition-for-the-2011-year"></a><span data-ttu-id="fee7e-130">Partíció létrehozása a 2011-es évhez</span><span class="sxs-lookup"><span data-stu-id="fee7e-130">To create a partition for the 2011 year</span></span>  
  
1.  <span data-ttu-id="fee7e-131">A partíciók listájában kattintson a korábban létrehozott **FactInternetSales2010** partícióra, majd a **Másolás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="fee7e-131">In the partitions list, click the **FactInternetSales2010** partition you created, and then click **Copy**.</span></span>  <span data-ttu-id="fee7e-132">Módosítsa a partíció nevét a **FactInternetSales2011** értékre.</span><span class="sxs-lookup"><span data-stu-id="fee7e-132">Change the partition name to **FactInternetSales2011**.</span></span> 

    <span data-ttu-id="fee7e-133">A lekérdezésszerkesztőben nem kell új szűrt sorok záradékot létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="fee7e-133">You do not need to use Query Editor to create a new filtered rows clause.</span></span> <span data-ttu-id="fee7e-134">Mivel létrehozta a 2010-es lekérdezés másolatát, csupán minimálisan kell módosítania azt, a 2011-es évre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="fee7e-134">Because you created a copy of the query for 2010, all you need to do is make a slight change in the query for 2011.</span></span>
  
2.  <span data-ttu-id="fee7e-135">Annak érdekében, hogy a partíció csak a 2011-es év sorait tartalmazza, a **Lekérdezés kifejezésében** cserélje le a megfelelő évszámokat a Szűrt sorok záradékban a **2011** és **2012** értékre, ahogy az alábbiakban is látható:</span><span class="sxs-lookup"><span data-stu-id="fee7e-135">In **Query Expression**, in-order for this partition to include only those rows for the 2011 year, replace the years in the Filtered Rows clause with **2011** and **2012**, respectively, like:</span></span>  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="to-create-partitions-for-2012-2013-and-2014"></a><span data-ttu-id="fee7e-136">Partíció létrehozása a 2012-es, 2013-as és 2014-es évhez</span><span class="sxs-lookup"><span data-stu-id="fee7e-136">To create partitions for 2012, 2013, and 2014.</span></span>  
  
- <span data-ttu-id="fee7e-137">A fenti lépéseket követve hozzon létre partíciókat a 2012-es, 2013-as és 2014-es évhez, és az évszámokat a Szűrt sorok záradékban a megfelelő évnek megfelelően módosítsa.</span><span class="sxs-lookup"><span data-stu-id="fee7e-137">Follow the previous steps, creating partitions for 2012, 2013, and 2014, changing the years in the Filtered Rows clause to include only rows for that year.</span></span> 
  

## <a name="delete-the-factinternetsales-partition"></a><span data-ttu-id="fee7e-138">A FactInternetSales partíció törlése</span><span class="sxs-lookup"><span data-stu-id="fee7e-138">Delete the FactInternetSales partition</span></span>
<span data-ttu-id="fee7e-139">Most, hogy minden évhez létrehozott egy partíciót, törölheti a FactInternetSales partíciót, így megelőzi az átfedéseket az Összes feldolgozása parancs a partíciók feldolgozása során történő végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="fee7e-139">Now that you have partitions for each year, you can delete the FactInternetSales partition; preventing overlap when choosing Process all when processing partitions.</span></span>

#### <a name="to-delete-the-factinternetsales-partition"></a><span data-ttu-id="fee7e-140">A FactInternetSales partíció törlése</span><span class="sxs-lookup"><span data-stu-id="fee7e-140">To delete the FactInternetSales partition</span></span>
-  <span data-ttu-id="fee7e-141">Kattintson a FactInternetSales partícióra, majd a **Törlés** parancsra.</span><span class="sxs-lookup"><span data-stu-id="fee7e-141">Click the FactInternetSales partition, and then click **Delete**.</span></span>



## <a name="process-partitions"></a><span data-ttu-id="fee7e-142">Partíciók feldolgozása</span><span class="sxs-lookup"><span data-stu-id="fee7e-142">Process partitions</span></span>  
<span data-ttu-id="fee7e-143">A partíciókezelőben láthatja, hogy az egyes létrehozott partíciók **Utoljára feldolgozva** oszlopában látható, hogy a partíciók még soha nem lettek feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="fee7e-143">In Partition Manager, notice the **Last Processed** column for each of the new partitions you created shows these partitions have never been processed.</span></span> <span data-ttu-id="fee7e-144">A partíciók létrehozásakor érdemes futtatnia egy Partíciók feldolgozása vagy Tábla feldolgozása műveletet az adott partíciók adatainak frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fee7e-144">When you create partitions, you should run a Process Partitions or Process Table operation to refresh the data in those partitions.</span></span>  
  
#### <a name="to-process-the-factinternetsales-partitions"></a><span data-ttu-id="fee7e-145">A FactInternetSales-partíciók feldolgozása</span><span class="sxs-lookup"><span data-stu-id="fee7e-145">To process the FactInternetSales partitions</span></span>  
  
1.  <span data-ttu-id="fee7e-146">A partíciókezelő bezárásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fee7e-146">Click **OK** to close Partition Manager.</span></span>  
  
2.  <span data-ttu-id="fee7e-147">Kattintson a **FactInternetSales** táblára, majd a **Modell** menü **Feldolgozás** > **Partíciók feldolgozása** elemére.</span><span class="sxs-lookup"><span data-stu-id="fee7e-147">Click the **FactInternetSales** table, then click the **Model** menu > **Process** > **Process Partitions**.</span></span>  
  
3.  <span data-ttu-id="fee7e-148">A Partíciók feldolgozása párbeszédpanelen bizonyosodjon meg róla, hogy a **Mód** **Alapértelmezés feldolgozása** értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="fee7e-148">In the Process Partitions dialog box, verify **Mode** is set to **Process Default**.</span></span>  
  
4.  <span data-ttu-id="fee7e-149">Jelölje be a jelölőnégyzetet a **Feldolgozás** oszlopban mind az öt létrehozott partíciónál, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fee7e-149">Select the checkbox in the **Process** column for each of the five partitions you created, and then click **OK**.</span></span>  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    <span data-ttu-id="fee7e-151">Ha a rendszer bekéri a megszemélyesítési hitelesítő adatait, adja meg a 2. leckében megadott Windows-felhasználónevet és -jelszót.</span><span class="sxs-lookup"><span data-stu-id="fee7e-151">If you're prompted for Impersonation credentials, enter the Windows user name and password you specified in Lesson 2.</span></span>  
  
    <span data-ttu-id="fee7e-152">Megjelenik az **Adatfeldolgozás** párbeszédpanel, amelyen az egyes partíciók feldolgozásának részletei láthatók.</span><span class="sxs-lookup"><span data-stu-id="fee7e-152">The **Data Processing** dialog box appears and displays process details for each partition.</span></span> <span data-ttu-id="fee7e-153">Láthatja, hogy az egyes partíciók esetében eltérő mennyiségű sor átvitele történik meg.</span><span class="sxs-lookup"><span data-stu-id="fee7e-153">Notice that a different number of rows for each partition are transferred.</span></span> <span data-ttu-id="fee7e-154">Mindegyik partíció csak az SQL-utasítás WHERE záradékában megadott évhez tartozó sorokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fee7e-154">Each partition includes only those rows for the year specified in the WHERE clause in the SQL Statement.</span></span> <span data-ttu-id="fee7e-155">A feldolgozás végeztével nyugodtan zárja be az Adatfeldolgozás párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="fee7e-155">When processing is finished, go ahead and close the Data Processing dialog box.</span></span>  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a><span data-ttu-id="fee7e-157">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="fee7e-157">What's next?</span></span>
<span data-ttu-id="fee7e-158">Tovább a következő leckére: [11. lecke: Szerepkörök létrehozása](../tutorials/aas-lesson-11-create-roles.md).</span><span class="sxs-lookup"><span data-stu-id="fee7e-158">Go to the next lesson: [Lesson 11: Create Roles](../tutorials/aas-lesson-11-create-roles.md).</span></span> 
