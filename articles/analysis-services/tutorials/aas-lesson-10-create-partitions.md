---
<span data-ttu-id="a2f1f-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 10: partíciókat |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate particionálja hello Azure Analysis Services-oktatóanyag projektben.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-101">title: aaa"Azure Analysis Services tutorial lesson 10: Create partitions | Microsoft Docs" description: Describes how toocreate partitions in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="a2f1f-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="a2f1f-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="a2f1f-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="a2f1f-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-10-create-partitions"></a><span data-ttu-id="a2f1f-104">10. lecke: Partíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a2f1f-104">Lesson 10: Create partitions</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="a2f1f-105">Ez a lecke partíciók toodivide hello FactInternetSales táblának kisebb logikai részekre feldolgozott (frissítése) független a többi partíció lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-105">In this lesson, you create partitions toodivide hello FactInternetSales table into smaller logical parts that can be processed (refreshed) independent of other partitions.</span></span> <span data-ttu-id="a2f1f-106">Alapértelmezés szerint minden tábla szerepel a modellben van egy partíciót, amely tartalmazza az összes hello tábla oszlopainak és sorainak.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-106">By default, every table you include in your model has one partition, which includes all hello table’s columns and rows.</span></span> <span data-ttu-id="a2f1f-107">Hello FactInternetSales táblának, az azt szeretnénk toodivide hello adatok évente; egy partíciót az egyes hello tábla öt évet.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-107">For hello FactInternetSales table, we want toodivide hello data by year; one partition for each of hello table’s five years.</span></span> <span data-ttu-id="a2f1f-108">Ezután mindegyik partíció egymástól függetlenül kezelhető.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-108">Each partition can then be processed independently.</span></span> <span data-ttu-id="a2f1f-109">több, lásd: toolearn [partíciók](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="a2f1f-109">toolearn more, see [Partitions](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span></span> 
  
<span data-ttu-id="a2f1f-110">Becsült idő toocomplete Ez a lecke: **15 perc**</span><span class="sxs-lookup"><span data-stu-id="a2f1f-110">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="a2f1f-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a2f1f-111">Prerequisites</span></span>  
<span data-ttu-id="a2f1f-112">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-112">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="a2f1f-113">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 9: hozzon létre hierarchiákat](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="a2f1f-113">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 9: Create Hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>  
  
## <a name="create-partitions"></a><span data-ttu-id="a2f1f-114">Partíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a2f1f-114">Create partitions</span></span>  
  
#### <a name="toocreate-partitions-in-hello-factinternetsales-table"></a><span data-ttu-id="a2f1f-115">hello FactInternetSales táblának toocreate a partíciók</span><span class="sxs-lookup"><span data-stu-id="a2f1f-115">toocreate partitions in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="a2f1f-116">A Tabular Model Explorerben bontsa ki a **Táblák** fát, és kattintson a jobb gombbal a **FactInternetSales** > **Partíciók** elemre.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-116">In Tabular Model Explorer, expand **Tables**, and then right-click **FactInternetSales** > **Partitions**.</span></span>  
  
2.  <span data-ttu-id="a2f1f-117">A partíció-kezelőben kattintson **másolási**, majd módosítsa a hello neve túl**FactInternetSales2010**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-117">In Partition Manager, click **Copy**, and then change hello name too**FactInternetSales2010**.</span></span>
  
    <span data-ttu-id="a2f1f-118">Mivel hello partíció tooinclude csak azok a sorok adott időszakon belüli, hello év 2010, módosítania kell a hello lekérdezési kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-118">Because you want hello partition tooinclude only those rows within a certain period, for hello year 2010, you must modify hello query expression.</span></span>
  
4.  <span data-ttu-id="a2f1f-119">Kattintson a **tervezési** tooopen szerkesztő lekérdezni, és kattintson a hello **FactInternetSales2010** lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-119">Click **Design** tooopen Query Editor, and then click hello **FactInternetSales2010** query.</span></span>

5.  <span data-ttu-id="a2f1f-120">A képen kattintson a lefelé mutató nyílra a hello hello **orderdate oszlopra** oszlop fejlécére, és kattintson **dátum/idő szűrők** > **közötti**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-120">In preview, click hello down arrow in hello **OrderDate** column heading, and then click **Date/Time Filters** > **Between**.</span></span>

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  <span data-ttu-id="a2f1f-122">Hello sorok szűrése párbeszédpanelen a **szűrési feltételek: orderdate oszlopra**, hagyja **ez utáni vagy egyenlő**, majd a hello dátum mezőben adja meg **1/1/2010**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-122">In hello Filter Rows dialog box, in **Show rows where: OrderDate**, leave **is after or equal to**, and then in hello date field, enter **1/1/2010**.</span></span> <span data-ttu-id="a2f1f-123">Hello hagyja **és** operátor kiválasztva, majd válassza ki **előtt**, majd hello dátum mezőjében adja meg **2011-1-1**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-123">Leave hello **And** operator selected, then select **is before**, then in hello date field, enter **1/1/2011**, and then click **OK**.</span></span>

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    <span data-ttu-id="a2f1f-125">Észreveheti, hogy a lekérdezésszerkesztőben az ALKALMAZOTT LÉPÉSEK között egy további lépés látható, Szűrt sorok néven.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-125">Notice in Query Editor, in APPLIED STEPS, you see another step named Filtered Rows.</span></span> <span data-ttu-id="a2f1f-126">Ez a szűrő csak rendelési dátumokat tooselect 2010.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-126">This filter is tooselect only order dates from 2010.</span></span>

8.  <span data-ttu-id="a2f1f-127">Kattintson az **Importálás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-127">Click **Import**.</span></span>

    <span data-ttu-id="a2f1f-128">A Partíciókezelőben, figyelje meg a kifejezés most már rendelkezik egy további szűrt záradék hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-128">In Partition Manager, notice hello query expression now has an additional Filtered Rows clause.</span></span>

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    <span data-ttu-id="a2f1f-130">A jelen nyilatkozat határozza meg, ez a partíció ezeket ahol hello orderdate oszlopra van hello 2010 naptári év hello szűrt sorokat záradékban megadott sorok csak hello adatok tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-130">This statement specifies this partition should include only hello data in those rows where hello OrderDate is in hello 2010 calendar year as specified in hello filtered rows clause.</span></span>  
  
  
#### <a name="toocreate-a-partition-for-hello-2011-year"></a><span data-ttu-id="a2f1f-131">egy partíció hello 2011 év toocreate</span><span class="sxs-lookup"><span data-stu-id="a2f1f-131">toocreate a partition for hello 2011 year</span></span>  
  
1.  <span data-ttu-id="a2f1f-132">Hello partíciók listájában kattintson hello **FactInternetSales2010** létrehozott partíció, és kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-132">In hello partitions list, click hello **FactInternetSales2010** partition you created, and then click **Copy**.</span></span>  <span data-ttu-id="a2f1f-133">Hello szolgáltatáspartíció neve túl módosítása**FactInternetSales2011**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-133">Change hello partition name too**FactInternetSales2011**.</span></span> 

    <span data-ttu-id="a2f1f-134">Nem kell toouse Lekérdezésszerkesztő toocreate egy új szűrt záradék.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-134">You do not need toouse Query Editor toocreate a new filtered rows clause.</span></span> <span data-ttu-id="a2f1f-135">Hello lekérdezés másolatát 2010 hozott létre, mert toodo szüksége olyan módosítást enyhe hello lekérdezésben 2011.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-135">Because you created a copy of hello query for 2010, all you need toodo is make a slight change in hello query for 2011.</span></span>
  
2.  <span data-ttu-id="a2f1f-136">A **lekérdezési kifejezésben**, a-ahhoz, hogy a partíció tooinclude csak a hello sorok 2011 év, cserélje le a szűrt sorok záradékot hello hello éveket **2011** és **2012**, osztályban, például:</span><span class="sxs-lookup"><span data-stu-id="a2f1f-136">In **Query Expression**, in-order for this partition tooinclude only those rows for hello 2011 year, replace hello years in hello Filtered Rows clause with **2011** and **2012**, respectively, like:</span></span>  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="toocreate-partitions-for-2012-2013-and-2014"></a><span data-ttu-id="a2f1f-137">toocreate partíciók 2012, a 2013-as és a 2014.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-137">toocreate partitions for 2012, 2013, and 2014.</span></span>  
  
- <span data-ttu-id="a2f1f-138">Hajtsa végre a hello előző lépéseket, partíciók létrehozása 2012, a 2013-as és a 2014, az adott év hello hello szűrt sorok záradék tooinclude csak sorok éveket módosítása.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-138">Follow hello previous steps, creating partitions for 2012, 2013, and 2014, changing hello years in hello Filtered Rows clause tooinclude only rows for that year.</span></span> 
  

## <a name="delete-hello-factinternetsales-partition"></a><span data-ttu-id="a2f1f-139">Hello FactInternetSales partíció törlése</span><span class="sxs-lookup"><span data-stu-id="a2f1f-139">Delete hello FactInternetSales partition</span></span>
<span data-ttu-id="a2f1f-140">Most, hogy partíciók évente, törölheti hello FactInternetSales partíció; Hozzon létre megakadályozza az összes folyamat kiválasztásakor, partíció feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-140">Now that you have partitions for each year, you can delete hello FactInternetSales partition; preventing overlap when choosing Process all when processing partitions.</span></span>

#### <a name="toodelete-hello-factinternetsales-partition"></a><span data-ttu-id="a2f1f-141">toodelete hello FactInternetSales partíció</span><span class="sxs-lookup"><span data-stu-id="a2f1f-141">toodelete hello FactInternetSales partition</span></span>
-  <span data-ttu-id="a2f1f-142">Kattintson a hello FactInternetSales partíciót, majd **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-142">Click hello FactInternetSales partition, and then click **Delete**.</span></span>



## <a name="process-partitions"></a><span data-ttu-id="a2f1f-143">Partíciók feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a2f1f-143">Process partitions</span></span>  
<span data-ttu-id="a2f1f-144">A Partíciókezelőben, figyelje meg hello **utolsó feldolgozott** oszlop az egyes hello mutat be, ezek a partíciók soha nem dolgozott létrehozott új partíciók.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-144">In Partition Manager, notice hello **Last Processed** column for each of hello new partitions you created shows these partitions have never been processed.</span></span> <span data-ttu-id="a2f1f-145">Partíciók létrehozása kell futtatni egy folyamatot partíciók, sem a folyamat művelet toorefresh hello táblaadatok ezek a partíciók.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-145">When you create partitions, you should run a Process Partitions or Process Table operation toorefresh hello data in those partitions.</span></span>  
  
#### <a name="tooprocess-hello-factinternetsales-partitions"></a><span data-ttu-id="a2f1f-146">tooprocess hello FactInternetSales partíciók</span><span class="sxs-lookup"><span data-stu-id="a2f1f-146">tooprocess hello FactInternetSales partitions</span></span>  
  
1.  <span data-ttu-id="a2f1f-147">Kattintson a **OK** tooclose partíciókezelő.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-147">Click **OK** tooclose Partition Manager.</span></span>  
  
2.  <span data-ttu-id="a2f1f-148">Kattintson a hello **FactInternetSales** tábla, majd kattintson az hello **modell** menü > **folyamat** > **folyamat partíciók**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-148">Click hello **FactInternetSales** table, then click hello **Model** menu > **Process** > **Process Partitions**.</span></span>  
  
3.  <span data-ttu-id="a2f1f-149">A hello folyamat partíciók párbeszédpanelen ellenőrizze **mód** értéke túl**folyamat alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-149">In hello Process Partitions dialog box, verify **Mode** is set too**Process Default**.</span></span>  
  
4.  <span data-ttu-id="a2f1f-150">Jelölje be a hello jelölőnégyzetet a hello **folyamat** oszlop az egyes hello öt particionálja a létrehozott, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-150">Select hello checkbox in hello **Process** column for each of hello five partitions you created, and then click **OK**.</span></span>  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    <span data-ttu-id="a2f1f-152">Ha a megszemélyesítési hitelesítő adatokat kéri, írja be a hello Windows rendszerbeli felhasználónevet és jelszót a megadott a 2.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-152">If you're prompted for Impersonation credentials, enter hello Windows user name and password you specified in Lesson 2.</span></span>  
  
    <span data-ttu-id="a2f1f-153">Hello **adatfeldolgozási** párbeszédpanel jelenik meg, és minden partíció esetében folyamat részleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-153">hello **Data Processing** dialog box appears and displays process details for each partition.</span></span> <span data-ttu-id="a2f1f-154">Láthatja, hogy az egyes partíciók esetében eltérő mennyiségű sor átvitele történik meg.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-154">Notice that a different number of rows for each partition are transferred.</span></span> <span data-ttu-id="a2f1f-155">Mindegyik partíció csak azok a sorok hello hello SQL-utasításban lévő WHERE záradéknak megadott hello év magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-155">Each partition includes only those rows for hello year specified in hello WHERE clause in hello SQL Statement.</span></span> <span data-ttu-id="a2f1f-156">Ha feldolgozása befejeződött, lépjen tovább, és hello adatfeldolgozási párbeszédpanel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="a2f1f-156">When processing is finished, go ahead and close hello Data Processing dialog box.</span></span>  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a><span data-ttu-id="a2f1f-158">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2f1f-158">What's next?</span></span>
<span data-ttu-id="a2f1f-159">Nyissa meg toohello következő lecke: [lecke 11: szerepkörök létrehozása](../tutorials/aas-lesson-11-create-roles.md).</span><span class="sxs-lookup"><span data-stu-id="a2f1f-159">Go toohello next lesson: [Lesson 11: Create Roles](../tutorials/aas-lesson-11-create-roles.md).</span></span> 
