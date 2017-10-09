---
title: "aaaAnalyze adatok Azure Machine Learning segítségével |} Microsoft Docs"
description: "Használja az Azure Machine Learning toobuild egy prediktív gépi tanulási modellt az Azure SQL Data Warehouse tárolt adatok alapján."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 337a2cd77aaad4467683827c56e5015b262b2554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a>Adatok elemzése Azure Machine Learning segítségével
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Ez az oktatóanyag használja toobuild Azure Machine Learning a prediktív gépi tanulási modellt az Azure SQL Data Warehouse tárolt adatok alapján. Pontosabban célzott marketingkampányt ez felépíti az Adventure Works hello kerékpárt üzemi, által előrejelzésére, ha egy ügyfél valószínűleg toobuy kerékpárt vagy nem.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Előfeltételek
az oktatóanyag teljesítéséhez toostep, lesz szüksége:

* Egy SQL Data Warehouse, előre feltöltve az AdventureWorksDW mintaadataival. tooprovision a, lásd: [SQL Data Warehouse létrehozása] [ Create a SQL Data Warehouse] és tooload hello minta adatok kiválasztásához. Ha már rendelkezik egy adatraktárral, de nincsenek mintaadatai, [a mintaadatokat manuálisan is betöltheti][load sample data manually].

## <a name="1-get-hello-data"></a>1. Hello adatok beolvasása
hello adatok olyan hello hello AdventureWorksDW adatbázis dbo.vTargetMail nézetében történik. tooread ezeket az adatokat:

1. Jelentkezzen be az [Azure Machine Learning Studio][Azure Machine Learning studio] szolgáltatásba, majd kattintson a Saját kísérletek elemre.
2. Kattintson a **+ ÚJ** opcióra, és válassza ki az **Üres kísérlet** opciót.
3. Adjon nevet a Célzott marketing kísérletnek.
4. A csomóponthúzási hello **olvasó** modulnak a vásznon a hello hello modulpanelről.
5. Adja meg az SQL Data Warehouse-adatbázis hello adatait hello tulajdonságok panelen.
6. Adja meg a hello adatbázist **lekérdezés** tooread hello érdeklő adatok.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Futtassa a hello kísérlet kattintva **futtatása** hello kísérletvászon alatt.
![Hello kísérlet futtatása][1]

Hello kísérletet, akkor futtatásának sikeres befejezése után kattintson a hello olvasó modul alján hello hello kimeneti portra, és válassza ki **Visualize** toosee hello importálta az adatokat.
![Importált adatok megtekintése][3]

## <a name="2-clean-hello-data"></a>2. Tiszta hello adatok
tooclean hello adatok, dobja el azokat az oszlopokat, hello modell nem kapcsolódik. toodo ezt:

1. A csomóponthúzási hello **Projektoszlopok** modult hello vászonra.
2. Kattintson a **Oszlopválasztás** a hello tulajdonságai panelen toospecify toodrop kívánt oszlopokat.
   ![Projektoszlopok][4]
3. Két oszlop kizárása: CustomerAlternateKey és GeographyKey.
   ![Felesleges oszlopok eltávolítása][5]

## <a name="3-build-hello-model"></a>3. Hello modell létrehozása
Osztjuk hello adatok 80 – 20: 80 % tootrain gépi tanulási modell és 20 % tootest hello modell. Használunk a bináris osztályozási problémához "Két osztályú" algoritmusokat hello használni.

1. A csomóponthúzási hello **vegyes** modult hello vászonra.
2. Adja meg a 0,8 sorok hello első kimeneti adathalmazban hello tulajdonságok panelen.
   ![Adatok felosztása tanítási és tesztelési adatkészletre][6]
3. A csomóponthúzási hello **két osztályú súlyozott döntési fa** modult hello vászonra.
4. A csomóponthúzási hello **tanítási modell** hello modult a vászonra, és adja meg a hello bemeneti adatok. Kattintson a **Oszlopválasztás** hello tulajdonságok panelen.
   * Első bemenet: gépi tanulási algoritmus.
   * Második bemenet: adatok tootrain hello algoritmus a.
     ![Csatlakozás hello tanítási modell modulhoz][7]
5. Jelölje be hello **BikeBuyer** oszlop szerint oszlop toopredict hello.
   ![Válassza ki az oszlop toopredict][8]

## <a name="4-score-hello-model"></a>4. Pontszám hello modell
Most teszteljük, hogyan hello modell a tesztadatokat hajt végre. Egy teljesen más algoritmust toosee, amely jobb teljesítményt az általunk választott hello algoritmust összehasonlítjuk.

1. A csomóponthúzási **Score Model** modult hello vászonra.
    Első bemenet: tanított modell második bemenet: Tesztadatok ![pontszám hello modell][9]
2. A csomóponthúzási hello **két osztályú Bayes Pontozó gépet** hello kísérletvászonra be. Hogyan ennek az algoritmusnak a összehasonlító toohello két osztályú súlyozott döntési fa összehasonlítjuk.
3. Másolás és a Beillesztés hello modulok tanítási és pontszám modell hello vászonra.
4. A csomóponthúzási hello **modell kiértékelése** hello vászonra toocompare hello két algoritmusok modult.
5. **Futtatás** hello kísérlet.
   ![Hello kísérlet futtatása][10]
6. Hello kimeneti port hello hello modell kiértékelése modul alsó részén kattintson, és kattintson a képi megjelenítés opcióra.
   ![Kiértékelés eredményeinek képi megjelenítése][11]

hello metrikák hello: ROC-görbe, pontosság-visszahívási diagram és növekedési görbe. A metrikák megnézi, láthatja, hogy hello első modell jobban, mint a második hello végre. toolook: hello mi hello első modell előrejelzésének, kattintson a hello pontszám modell kimeneti portjára, majd kattintson a képi megjelenítés opcióra.
![Pontszám modell eredményeinek képi megjelenítése][12]

Két további oszlopokat hozzá tooyour tesztelési adatkészletnél jelenik meg.

* Pontozott valószínűség: hello annak valószínűségét, hogy az ügyfél kerékpárvásárló-e.
* Pontozott címkék: hello által elvégzett osztályozás hello modell – kerékpárvásárló (1) avagy sem (0). A címkézés valószínűségi küszöbértéke too50 % van beállítva, és módosítható.

Hello kerékpárvásárló (tényleges) hello pontozott címkék (előrejelzés) az összehasonlítása, láthatja, milyen mértékben hello modell hajtott végre. Következő lépésként használhatja a modell toomake előrejelzéseket új ügyfelek számára, és közzéteheti webszolgáltatásként vagy eredményeit, hátsó tooSQL Data Warehouse írási.

## <a name="next-steps"></a>Következő lépések
több prediktív gépi tanulási modellek létrehozásával kapcsolatos toolearn tekintse meg a túl[bemutatása tooMachine Azure tanulási][Introduction tooMachine Learning on Azure].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction tooMachine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
