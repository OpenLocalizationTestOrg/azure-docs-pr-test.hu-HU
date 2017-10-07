---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 5: számított oszlopok létrehozása |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate számított oszlopok hello Azure Analysis Services-oktatóanyag projektben. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-5-create-calculated-columns"></a>5. lecke: Számított oszlopok létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a leckében számított oszlopok hozzáadásával hozhat létre adatokat a modellben. Számított oszlopok (egyéni oszlopokként) hozhat létre adatok beolvasása paranccsal, ha a hello Lekérdezésszerkesztő, vagy később a hello modell Tervező hasonló elvégezheti itt. több, lásd: toolearn [számított oszlopok](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).
  
Öt új számított oszlopot hoz létre három különböző táblában. hello lépések kissé eltérőek számos módon toocreate oszlopok, nevezze át őket, és helyezze el őket a különböző helyeken egy tábla minden feladat.  

Ez a lecke lesz az első, ahol a DAX (Data Analysis Expressions) nyelvet használja. A DAX egy speciális nyelv, amellyel nagymértékben testre szabható képletkifejezéseket hozhat létre táblázatos modellekhez. Ebben az oktatóanyagban használja a DAX toocreate számított oszlopokat, mértékeket és szerepkör-szűrők. több, lásd: toolearn [táblázatos modellek DAX](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular). 
  
Becsült idő toocomplete Ez a lecke: **15 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 4: hozzon létre kapcsolatokat](../tutorials/aas-lesson-4-create-relationships.md). 
  
## <a name="create-calculated-columns"></a>Számított oszlopok létrehozása  
  
#### <a name="create-a-monthcalendar-calculated-column-in-hello-dimdate-table"></a>A MonthCalendar számított oszlop hello DimDate tábla létrehozása  
  
1.  Kattintson a hello **modell** menü > **modell nézet** > **adatnézet**.  
  
    Számított oszlopok csak hello modell tervezővel adatnézetben hozhatók létre.  
  
2.  A hello modellek tervezőjében, kattintson a hello **DimDate** tábla (lap).  
  
3.  Kattintson a jobb gombbal hello **CalendarQuarter** oszlop fejlécére, és kattintson **oszlop beszúrása**.  
  
    Egy olyan új oszlop neve **számított oszlop 1** hello beszúrt toohello balra van **negyedév** oszlop.  
  
4.  Hello képletsávba hello táblázat felett, írja be a következő DAX-képlet hello: írja be az automatikus kiegészítés segítségével hello oszlopok és táblák teljesen minősített nevek, és listák hello funkcióit.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    Értékek majd fel van töltve hello számított oszlop összes hello soraihoz. Ha görgessen lefelé hello táblázatot, látható sorok rendelkezhet eltérő értékek tartoznak ebben az oszlopban lévő minden egyes sorára hello adatok alapján.    
  
5.  Ez az oszlop átnevezése túl**MonthCalendar**. 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
hello MonthCalendar számított oszlop elnevezi rendezhető hónap.  
  
#### <a name="create-a-dayofweek-calculated-column-in-hello-dimdate-table"></a>A hét napját számított oszlop hello DimDate tábla létrehozása  
  
1.  A hello **DimDate** tábla még mindig aktív, kattintson a hello **oszlop** menüben, majd kattintson **oszlop hozzáadása**.  
  
2.  Hello képletsávba írja be a következő képlet hello:  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Hello képlet szerkesztésének befejezése után nyomja le az ENTER BILLENTYŰT. hello új oszlop jobb szélén hello tábla toohello jelenik meg.  
  
3.  Nevezze át a hello oszlop túl**DayOfWeek**.  
  
4.  Hello oszlopának fejlécére kattintva, és húzza hello oszlop közötti hello **EnglishDayNameOfWeek** oszlop és hello **DayNumberOfMonth** oszlop.  
  
    > [!TIP]  
    > Oszlop áthelyezése a táblázat segítségével könnyebben toonavigate.  
  
hello DayOfWeek számított oszlop elnevezi rendezhető hello hét napja.  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-hello-dimproduct-table"></a>ProductSubcategoryName számított oszlop hello DimProduct tábla létrehozása  
  
  
1.  A hello **DimProduct** table, görgessen toohello hello tábla a jobb szélén. Értesítés hello jobb szélső oszlop neve **oszlop hozzáadása** (dőlt), kattintson a hello oszlopfejlécre.  
  
2.  Hello képletsávba írja be a következő képlet hello:  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Nevezze át a hello oszlop túl**ProductSubcategoryName**.  
  
hello ProductSubcategoryName számított oszlop használt toocreate egy hierarchiában lévő hello DimProduct táblázatot, amely hello DimProductSubcategory tábla hello EnglishProductSubcategoryName oszlop adatait tartalmazza. A hierarchiák csak egyetlen táblára terjedhetnek ki. Hierarchiákat később, a 9. leckében hozhat majd létre.  
  
#### <a name="create-a-productcategoryname-calculated-column-in-hello-dimproduct-table"></a>ProductCategoryName számított oszlop hello DimProduct tábla létrehozása  
  
1.  A hello **DimProduct** tábla még mindig aktív, kattintson a hello **oszlop** menüben, majd kattintson **oszlop hozzáadása**.  
  
2.  Hello képletsávba írja be a következő képlet hello:  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Nevezze át a hello oszlop túl**ProductCategoryName**.  
  
hello ProductCategoryName számított oszlop használt toocreate egy hierarchiában lévő hello DimProduct táblázatot, amely hello DimProductCategory tábla hello EnglishProductCategoryName oszlop adatait tartalmazza. A hierarchiák csak egyetlen táblára terjedhetnek ki.  
  
#### <a name="create-a-margin-calculated-column-in-hello-factinternetsales-table"></a>Hozzon létre egy margó számított oszlop hello FactInternetSales táblának  
  
1.  A hello modellek tervezőjében, válassza ki a hello **FactInternetSales** tábla.  
  
2.  Hozzon létre egy új számított oszlop közötti hello **SalesAmount** oszlop és hello **TaxAmt** oszlop.  
  
3.  Hello képletsávba írja be a következő képlet hello:  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Nevezze át a hello oszlop túl**margó**.  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    hello margó számított oszlop használt tooanalyze nyereség margók minden forgalom számára.  
  
## <a name="whats-next"></a>A következő lépések
[6. lecke: Mértékek létrehozása](../tutorials/aas-lesson-6-create-measures.md).
  
  
  
