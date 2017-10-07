---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 7: hozzon létre a fő teljesítménymutatók |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate a fő teljesítménymutatók a hello Azure Analysis Services-oktatóanyag projekt. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="lesson-7-create-key-performance-indicators"></a>7. lecke: Fő teljesítménymutatók létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a leckében fő teljesítménymutatókat (KPI-k) hozhat létre. KPI-k olyan használt toogauge teljesítményének által megadott értéket egy *Base* mérték, szemben a *cél* egy mérték, vagy abszolút értéke által meghatározott érték. A jelentéskészítési ügyfélalkalmazások, KPI-k biztosít üzleti szakemberek egy gyorsan és egyszerűen elvégezhető toounderstand üzleti sikeres vagy tooidentify trendek összegzését. több, lásd: toolearn [KPI-k](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)
  
Becsült idő toocomplete Ez a lecke: **15 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 6: mértékek létrehozása](../tutorials/aas-lesson-6-create-measures.md).   
  
## <a name="create-key-performance-indicators"></a>Fő teljesítménymutatók létrehozása  
  
#### <a name="toocreate-an-internetcurrentquartersalesperformance-kpi"></a>egy InternetCurrentQuarterSalesPerformance KPI toocreate  
  
1.  A hello modellek tervezőjében, kattintson a hello **FactInternetSales** tábla.  
  
2.  Hello mérték rácsban kattintson a cella üres.  
  
3.  Hello képletsávba, fent hello tábla írja be a következő képlet hello: 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    Ez a mérték hello alapjául szolgáló mértéket a hello KPI funkcionál.  
  
4.  Kattintson jobb gombbal az **InternetCurrentQuarterSalesPerformance** > **KPI létrehozása** lehetőségére.   
  
5.  A hello fő teljesítménymutató (KPI) párbeszédpanelen a **cél** kiválasztása **abszolút értékét**, és írja be **1.1**.  
  
7.  Hello a bal oldali (alacsony) csúszkát mezőben írja be a **1**, majd a hello a csúszka jobbra (nagy) mezőbe írja be **1.07**.  
  
8.  A **ikon stílus kiválasztása**hello (piros) rombusz háromszög (sárga) válassza ki, kör ikon (zöld) típus.
  
    ![aas-lesson7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > Értesítés hello bővíthető **leírások** címke alatt hello elérhető ikon stílusok. Leírások használata hello különböző KPI elemek toomake őket több azonosítható az ügyfélalkalmazások számára.  
  
9. Kattintson a **OK** toocomplete hello KPI-t.  
  
    Hello mérték rácsban, figyelje meg hello ikon következő toohello **InternetCurrentQuarterSalesPerformance** mérték. Ez az ikon azt mutatja, hogy ez a mérték a KPI alapértékeként szolgál.  
  
#### <a name="toocreate-an-internetcurrentquartermarginperformance-kpi"></a>egy InternetCurrentQuarterMarginPerformance KPI toocreate  
  
1.  A hello hello mérték rácsban **FactInternetSales** table, kattintson a cella üres.  
  
2.  Hello képletsávba, fent hello tábla írja be a következő képlet hello:  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  Kattintson a jobb gombbal az **InternetCurrentQuarterMarginPerformance** > **KPI létrehozása** lehetőségére.  
  
4.  A hello fő teljesítménymutató (KPI) párbeszédpanelen a **cél** kiválasztása **abszolút értéke**, és írja be **1,25**.   
  
5.  A bal oldali (alacsony) csúszkát mezőben hello csúsztassa be a amíg hello mezőben jelenik meg **0,8**, majd diák hello (magas) csúszkát mező, jobb gombbal, amíg hello mezőben jelenik meg, és **1,03**.  
  
6.  A **ikon stílus kiválasztása**, jelölje ki a hello rombusz (piros), a háromszög (sárga), a (zöld) kör ikon típusa, és kattintson **OK**.  
  
## <a name="whats-next"></a>A következő lépések
[8. lecke: Perspektívák létrehozása](../tutorials/aas-lesson-8-create-perspectives.md).
  
  
