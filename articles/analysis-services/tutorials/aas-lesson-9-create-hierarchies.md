---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 9: hozzon létre hierarchiákat |} Microsoft Docs"Leírás: szolgáltatások: analysis-szolgáltatások documentationcenter:" Szerző: minewiskan manager: erikre szerkesztőben: "címkék:"

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="lesson-9-create-hierarchies"></a>9. lecke: Hierarchiák létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a leckében hierarchiákat hozhat létre. A hierarchiák szintekbe rendezett oszlopcsoportok. A földrajzi hierarchia például tartalmazhatja az Ország, Állam, Megye és Város alszinteket. Hierarchiák jelennek meg a jelentéskészítési ügyfél alkalmazás mezőlistán más oszlopokból külön így egyszerűbben ügyfél felhasználók toonavigate, és a jelentésbe foglalni. több, lásd: toolearn [hierarchiák](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)
  
toocreate hierarchiák, használja a hello modellek tervezőjében *diagramnézet*. A hierarchiák létrehozása és kezelése Adatnézetben nem támogatott.  
  
Becsült idő toocomplete Ez a lecke: **20 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 8: perspektívák létrehozásához](../tutorials/aas-lesson-8-create-perspectives.md).  
  
## <a name="create-hierarchies"></a>Hierarchiák létrehozása  
  
#### <a name="toocreate-a-category-hierarchy-in-hello-dimproduct-table"></a>a kategóriák hierarchiája hello DimProduct tábla toocreate  
  
1.  Hello modell Designer (diagramnézet), kattintson a jobb gombbal hello **DimProduct** tábla > **hierarchia létrehozása**. Új hierarchia hello tábla ablak hello alján jelenik meg. Nevezze át a hello hierarchia **kategória**.  
  
2.  Kattintással és húzással vigye a hello **ProductCategoryName** oszlop toohello új **kategória** hierarchia.  
  
3.  A hello **kategória** hierarchia, kattintson a jobb gombbal hello **ProductCategoryName** > **átnevezése**, majd írja be **kategória**.  
  
    > [!NOTE]  
    > Egy hierarchiában lévő oszlop átnevezése nem nevezhető át, hogy hello tábla oszlopa. Egy hierarchiában lévő oszlop hello tábla hello oszlopa egy ábrázolása legyen.  
  
4.  Kattintással és húzással vigye a hello **ProductSubcategoryName** oszlop toohello **kategória** hierarchia. Nevezze át a következőre: **Subcategory**. 
  
5.  Kattintson a jobb gombbal hello **ModelName** oszlop > **toohierarchy hozzáadása**, majd válassza ki **kategória**. Nevezze át a következőre: **Model**.

6.  Végül adja hozzá **EnglishProductName** toohello kategóriák hierarchiája. Nevezze át a következőre: **Product**.  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="toocreate-hierarchies-in-hello-dimdate-table"></a>hello DimDate tábla toocreate hierarchiák  
  
1.  A hello **DimDate** table, hozzon létre egy nevű hierarchiát **naptár**.  
  
3.  Adja hozzá a következő oszlopok sorrendben hello:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  A hello **DimDate** tábla, hozzon létre egy **pénzügyi** hierarchia. A következő oszlopok sorrendben hello a következők:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Végezetül a hello **DimDate** tábla, hozzon létre egy **ProductionCalendar** hierarchia. A következő oszlopok sorrendben hello a következők:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>A következő lépések
[10. lecke: Partíciók létrehozása](../tutorials/aas-lesson-10-create-partitions.md). 
  
  
