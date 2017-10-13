---
title: "Az Azure Analysis Services oktatóanyaga – 5. lecke: Számított oszlopok létrehozása | Microsoft Docs"
description: "A lecke azt ismerteti, hogyan hozhat létre számított oszlopokat az Azure Analysis Services oktatóprojektjében."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/20/2017
ms.author: owend
ms.openlocfilehash: eab74fbadc6a143ca5a2bc57a1762539a6d489c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-5-create-calculated-columns"></a>5. lecke: Számított oszlopok létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a leckében számított oszlopok hozzáadásával hozhat létre adatokat a modellben. Számított oszlopokat (egyéni oszlopokként) az Adatok lekérése paranccsal, a Lekérdezésszerkesztővel, vagy később, az itt is látható módon, a modelltervezőben hozhat létre. További információ: [Számított oszlopok](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).
  
Öt új számított oszlopot hoz létre három különböző táblában. Az egyes feladatok lépései kismértékben eltérnek, ezzel is mutatva, hogy az oszlopok létrehozása, átnevezése, valamint a táblában való különféle helyeken való elhelyezése többféleképpen is végrehajtható.  

Ez a lecke lesz az első, ahol a DAX (Data Analysis Expressions) nyelvet használja. A DAX egy speciális nyelv, amellyel nagymértékben testre szabható képletkifejezéseket hozhat létre táblázatos modellekhez. Ebben az oktatóanyagban a DAX használatával hozhat létre számított oszlopokat, mértékeket és szerepkörszűrőket. További tudnivalók: [DAX a táblázatos modellekben](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular). 
  
A lecke elvégzésének várható időtartama: **15 perc**.  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. A leckében foglalt feladatok végrehajtása előtt el kell végeznie az előző leckét ([4. lecke: Kapcsolatok létrehozása](../tutorials/aas-lesson-4-create-relationships.md)). 
  
## <a name="create-calculated-columns"></a>Számított oszlopok létrehozása  
  
#### <a name="create-a-monthcalendar-calculated-column-in-the-dimdate-table"></a>MonthCalendar számított oszlop létrehozása a DimDate táblában  
  
1.  Kattintson a **Modell** menü **Modellnézet** > **Adatnézet** elemre.  
  
    Számított oszlopok csak a modelltervező Adatnézetében hozhatók létre.  
  
2.  A modelltervezőben kattintson a **DimDate** táblára (fülre).  
  
3.  Kattintson a jobb gombbal a **CalendarQuarter** oszlop fejlécére, majd kattintson az **Oszlop beszúrása** parancsra.  
  
    A rendszer beszúr egy **Számított oszlop 1** nevű oszlopot a **CalendarQuarter** oszlop bal oldalára.  
  
4.  A tábla feletti képletsávban gépelje be a következő DAX-képletet: Az automatikus kiegészítés segít az oszlopok és táblák teljes nevének beírásában, és felsorolja a rendelkezésre álló függvényeket.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    A rendszer ezután kitölti a számított oszlop összes sorát. Ha legörget a táblában, megfigyelheti, hogy az egyes sorokban található adatok alapján az oszlop sorai különböző értékekkel rendelkezhetnek.    
  
5.  Nevezze át az oszlopot a következőre: **MonthCalendar**. 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
A MonthCalendar számított oszlop rendezhető formátumban adja meg a hónapok nevét.  
  
#### <a name="create-a-dayofweek-calculated-column-in-the-dimdate-table"></a>DayOfWeek számított oszlop létrehozása a DimDate táblában  
  
1.  Az aktív **DimDate** táblában kattintson az **Oszlop** menüre, majd az **Oszlop hozzáadása** parancsra.  
  
2.  A képletsávban gépelje be a következő képletet:  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Ha végzett a képlet felépítésével, nyomja le az Enter billentyűt. Az új oszlop megjelenik a tábla jobb szélén.  
  
3.  Nevezze át az oszlopot a következőre: **DayOfWeek**.  
  
4.  Kattintson az oszlop fejlécére, majd húzza az **EnglishDayNameOfWeek** és a **DayNumberOfMonth** oszlop közé.  
  
    > [!TIP]  
    > Az oszlopok áthelyezésével megkönnyítheti a táblában való navigálást.  
  
A DayOfWeek számított oszlop rendezhető formátumban adja meg a hét napjainak nevét.  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-the-dimproduct-table"></a>ProductSubcategoryName számított oszlop létrehozása a DimProduct táblában  
  
  
1.  A **DimProduct** táblában görgessen a tábla jobb széléhez. Figyelje meg, hogy a jobb szélső oszlop az **Oszlop hozzáadása** (dőlt betűs) nevet viseli. Kattintson az oszlop fejlécére.  
  
2.  A képletsávban gépelje be a következő képletet:  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Nevezze át az oszlopot a következőre: **ProductSubcategoryName**.  
  
A ProductSubcategoryName számított oszlop használatával létrejön egy hierarchia a DimProduct táblában, amely tartalmazza a DimProductSubcategory tábla EnglishProductSubcategoryName oszlopának adatait. A hierarchiák csak egyetlen táblára terjedhetnek ki. Hierarchiákat később, a 9. leckében hozhat majd létre.  
  
#### <a name="create-a-productcategoryname-calculated-column-in-the-dimproduct-table"></a>ProductCategoryName számított oszlop létrehozása a DimProduct táblában  
  
1.  Az aktív **DimProduct** táblában kattintson az **Oszlop** menüre, majd az **Oszlop hozzáadása** parancsra.  
  
2.  A képletsávban gépelje be a következő képletet:  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Nevezze át az oszlopot a következőre: **ProductCategoryName**.  
  
A ProductCategoryName számított oszlop használatával létrejön egy hierarchia a DimProduct táblában, amely tartalmazza a DimProductCategory tábla EnglishProductCategoryName oszlopának adatait. A hierarchiák csak egyetlen táblára terjedhetnek ki.  
  
#### <a name="create-a-margin-calculated-column-in-the-factinternetsales-table"></a>Margin (Nyereség) számított oszlop létrehozása a FactInternetSales táblában  
  
1.  A modelltervezőben válassza ki a **FactInternetSales** táblát.  
  
2.  Hozzon létre egy új számított oszlopot a **SalesAmount** és a **TaxAmt** oszlop között.  
  
3.  A képletsávban gépelje be a következő képletet:  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Nevezze át az oszlopot a következőre: **Margin**.  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    A Margin számított oszlop használatával elemezhető az egyes értékesítések fedezeti mutatója.  
  
## <a name="whats-next"></a>A következő lépések
[6. lecke: Mértékek létrehozása](../tutorials/aas-lesson-6-create-measures.md).
  
  
  
