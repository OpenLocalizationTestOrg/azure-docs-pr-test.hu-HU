---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 6: mértékek létrehozása |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate hello Azure Analysis Services-oktatóanyag projektben méri. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-6-create-measures"></a>6. lecke: Mértékek létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a lecke a modellben szereplő mértékek toobe hoz létre. Hasonló toohello számított oszlopok létrehozott, a mérték létrehozása DAX-képlet használatával számítás. A számított oszlopokkal ellentétben azonban a mértékek kiértékelése a felhasználó által kiválasztott *szűrőkkel* történik. Például egy adott oszlopban szeletelő hozzá vagy toohello sorfeliratok mező egy kimutatásban. Minden hello szűrő cella értékét majd hello alkalmazott mérték kiszámítása. A mértékek általában hatékony, rugalmas számítások, amelyet az tooinclude szinte minden táblázatos modellek tooperform dinamikus számítások numerikus adatokon. több, lásd: toolearn [intézkedések](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).
  
toocreate intézkedések használata hello *mérték rács*. Alapértelmezés szerint mindegyik tábla rendelkezik egy üres mérték ráccsal, azonban általában nem mindegyik táblához szokás mértéket létrehozni. hello mérték rács hello modellek tervezőjében adatok nézetben táblához alatt jelenik meg. vagy az toohide hello mérték rács egy táblához, kattintson a hello **tábla** menüben, majd kattintson **mérték rácsvonalak megjelenítése**.  
  
Egy üres cella hello mérték rácsban, majd írja be egy DAX-képlet hello képletsávba is létrehoz egy mértéket. Ha ENTER toocomplete hello képlet, hello mérték kattintson, majd hello cella jelenik meg. Intézkedések oszlop a szabványos aggregátumfüggvény használatával is létrehozhat, és kattintson hello AutoSzum gombbal (**∑**) hello eszköztáron. Hello AutoSzum funkció használatával létrehozott intézkedések hello mérték cellában hello oszlop közvetlenül alatt jelenik meg, de lehet áthelyezni.  
  
Ez a lecke létrehozhat mértékeket mindkét írjon be egy DAX-képlet hello képletsávba, és hello AutoSzum funkció használatával.  
  
Becsült idő toocomplete Ez a lecke: **30 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 5: számított oszlopok létrehozása](../tutorials/aas-lesson-5-create-calculated-columns.md).  
  
## <a name="create-measures"></a>Mértékek létrehozása  
  
#### <a name="toocreate-a-dayscurrentquartertodate-measure-in-hello-dimdate-table"></a>hello DimDate tábla DaysCurrentQuarterToDate mérték toocreate  
  
1.  A hello modellek tervezőjében, kattintson a hello **DimDate** tábla.  
  
2.  Hello mérték rácsban kattintson a bal felső üres cella hello.  
  
3.  Hello képletsávba írja be a következő képlet hello:  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    Értesítés hello bal felső cella már tartalmaz egy mérték neve **DaysCurrentQuarterToDate**, utána pedig hello eredmény **92**.
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    Számított oszlop, ellentétben a mérték képletekkel adhatja meg hello mérték nevét kettősponttal, hello képlet kifejezés követi.

  
#### <a name="toocreate-a-daysincurrentquarter-measure-in-hello-dimdate-table"></a>hello DimDate tábla DaysInCurrentQuarter mérték toocreate  
  
1.  A hello **DimDate** tábla még mindig aktív hello modell Designer hello mérték rácsban, kattintson az üres cella hello alatt létrehozott hello mérték.  
  
2.  Hello képletsávba írja be a következő képlet hello:  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    Amikor hoz létre egy teljes időtartam és a hello összehasonlítás aránya előző időszak. hello képlet kell hello eltelt hello időszak arányának kiszámításához, és hasonlítsa össze azokat toohello azonos arányban hello az előző időszak. Ez az eset, [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] által biztosított hello arányban eltelt hello jelenlegi időszakhoz.  
  
#### <a name="toocreate-an-internetdistinctcountsalesorder-measure-in-hello-factinternetsales-table"></a>toocreate hello FactInternetSales táblának InternetDistinctCountSalesOrder intézkedésként  
  
1.  Kattintson a hello **FactInternetSales** tábla.   
  
2.  Kattintson a hello **SalesOrderNumber** oszlop fejlécére.  
  
3.  Hello eszköztáron kattintson a Tovább hello lefelé mutató nyíl toohello AutoSzum (**∑**) gombra, és válassza **DistinctCount**.  
  
    hello AutoSzum funkció automatikusan létrehoz egy mértéket hello kiválasztott oszlop hello DistinctCount szabványos összesítési képlet szerint.  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  Hello mérték rácsban, kattintson az új mérték hello, majd a hello **tulajdonságok** ablakban, a **mérték neve**, nevezze át a hello mérték túl**InternetDistinctCountSalesOrder**. 
 
  
#### <a name="toocreate-additional-measures-in-hello-factinternetsales-table"></a>hello FactInternetSales táblának toocreate további intézkedések  
  
1.  Hello AutoSzum funkcióval, hozzon létre, és a következő intézkedéseket hello neve:  

    |Oszlop|Mérték neve|AutoSzum (∑)|Képlet|  
    |----------------|----------|-----------------|-----------|  
    |SalesOrderLineNumber|InternetOrderLinesCount|Darabszám|=DARAB2([SalesOrderLineNumber])|  
    |OrderQuantity|InternetTotalUnits|Összeg|=SZUM([OrderQuantity])|  
    |DiscountAmount|InternetTotalDiscountAmount|Összeg|=SZUM([DiscountAmount])|  
    |TotalProductCost|InternetTotalProductCost|Összeg|=SZUM([TotalProductCost])|  
    |SalesAmount|InternetTotalSales|Összeg|=SZUM([SalesAmount])|  
    |Margin|InternetTotalMargin|Összeg|=SZUM([Margin])|  
    |TaxAmt|InternetTotalTaxAmt|Összeg|=SZUM([TaxAmt])|  
    |Freight|InternetTotalFreight|Összeg|=SZUM([Freight])|  
  
2.  Egy üres cella hello mérték rácsban, és hello képletsávba, a Létrehozás elemre kattintva, és neve hello következő intézkedéseket sorrendben:  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
Hello FactInternetSales táblának készült intézkedések lehet használt tooanalyze kritikus pénzügyi adatok, például értékesítési, a költségek és a nyereség margó hello kijelölt Felhasználószűrő által meghatározott elemek.  
  
## <a name="whats-next"></a>A következő lépések
[7. lecke: Fő teljesítménymutatók létrehozása](../tutorials/aas-lesson-7-create-key-performance-indicators.md)  

  
