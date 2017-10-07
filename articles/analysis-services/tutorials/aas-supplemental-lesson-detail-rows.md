---
cím: aaa "Azure Analysis Services útmutató kiegészítő lecke: sorok |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate a részletes sorok kifejezés hello Azure Analysis Services-oktatóanyag.
szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="supplemental-lesson---detail-rows"></a>Kiegészítő lecke – Részletsorok

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a kiegészítő lecke használhatja hello DAX-szerkesztő toodefine egyéni részletes sorok kifejezés. A részletes sorok kifejezés egyik tulajdonságának egy mérték, és a végfelhasználók hello összesített eredmények mérték további információt nyújt. 
  
Becsült idő toocomplete Ez a lecke: **10 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a kiegészítőlecke-témakör a táblázatos modellezésről szóló oktatóanyag része. Ez a kiegészítő lecke hello feladatok elvégzése előtt kell befejeződött az összes korábbi során tapasztalatokat, vagy egy befejezett Adventure Works Internet értékesítési minta jelentésmodell-projekt.  
  
## <a name="what-do-we-need-toosolve"></a>Mit kell toosolve?
Nézzük hello részleteit a InternetTotalSales mérték, mielőtt hozzáad egy részletes sorok kifejezés.

1.  Az SSDT, kattintson a hello **modell** menü > **elemzés az Excelben** tooopen Excel, és hozzon létre egy üres kimutatást.
  
2.  A **kimutatástábla mezői**, vegye fel a hello **InternetTotalSales** hello FactInternetSales táblának túl mérjük**értékek**, **CalendarYear**hello DimDate a tábla túl**oszlopok**, és **EnglishCountryRegionName** túl**sorok**. A kimutatás most biztosítanak számunkra összesített eredmények hello InternetTotalSales intézkedés régiók és év. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. A kimutatás hello kattintson duplán egy összesített értéket év és a terület neve. Itt azt duplán kattint Ausztrália és hello hello érték év 2014. Megnyílik egy új, nem hasznos adatokat tartalmazó lap.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Mi azt kellene itt toosee Ez hozzájárul toohello adatok sorok és oszlopok tartalmazó tábla lesz, mint a InternetTotalSales mérték eredményét összesíti. toodo, hogy hozzá lehessen adni egy részletes sorok kifejezés hello mérték tulajdonságainál.

## <a name="add-a-detail-rows-expression"></a>Részletsor-kifejezés hozzáadása

#### <a name="toocreate-a-detail-rows-expression"></a>a részletes sorok kifejezés toocreate 
  
1. Az SSDT, hello FactInternetSales táblának mérték rácsban, kattintson a hello **InternetTotalSales** mérték. 

2. A **tulajdonságok** > **részletes sorok kifejezés**, hello szerkesztő gomb tooopen hello DAX-szerkesztőben kattintson.

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. A DAX-szerkesztőben, adja meg a kifejezés a következő hello:

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    Ebben a kifejezésben meghatározza a nevét, oszlopok, és a felhasználó duplán kattint egy kimutatás vagy a jelentés egy összesített eredmény hello FactInternetSales táblának és a kapcsolódó táblák mérték eredményeket ad vissza.

4. Vissza az Excel programban a 3. lépésben létrehozott hello lap törölje, majd kattintson duplán egy összesített értéket. Megadott idő definiált hello mérték részletességi sorok kifejezés tulajdonsággal, új lapon nyílik meg sokkal több hasznos adatait tartalmazó.

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. Helyezze ismét üzembe a modellt.

  
## <a name="see-also"></a>Lásd még:  
[SELECTCOLUMNS függvény (DAX)](https://msdn.microsoft.com/library/mt761759.aspx)   
[Kiegészítő lecke – Dinamikus biztonság](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Kiegészítő lecke – Hézagos hierarchiák](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
