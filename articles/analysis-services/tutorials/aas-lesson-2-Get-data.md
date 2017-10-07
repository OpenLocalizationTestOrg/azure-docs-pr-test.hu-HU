---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 2: adatok beolvasása |} Microsoft Docs"Leírás: ismerteti, hogyan tooget-importálási adatok hello Azure Analysis Services-oktatóanyag projekt. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---

# <a name="lesson-2-get-data"></a>2. lecke: Az adatok beszerzése

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a lecke használja az adatok beolvasása az SSDT tooconnect toohello AdventureWorksDW2014 mintaadatbázis, válassza az adatok, villámnézet és szűrés, és importálja a modell munkaterületre.  
  
Az Adatok lekérése használatával olyan források széles választékából importálhatók adatok mint: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, fájlok és egyéb lehetőségek. Az adatok emellett lekérdezhetők Power Query M-képletekkel is.
  
Becsült idő toocomplete Ez a lecke: **10 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 1: hozzon létre egy új táblázatos modell projektet](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="create-a-connection"></a>Kapcsolat létrehozása  
  
#### <a name="toocreate-a-connection-toohello-adventureworksdw2014-database"></a>egy kapcsolat toohello AdventureWorksDW2014 adatbázis toocreate  
  
1.  A Táblázatos modelltallózóban kattintson a jobb gombbal az **Adatforrások** > **Importálás adatforrásból** elemére.  
  
    Ekkor elindul az adatok beolvasása, amely végigvezeti a kapcsolódó tooa adatforrás. Ha nem jelenik meg a táblázatos modell Explorer **Megoldáskezelőben**, kattintson duplán a **Model.bim** tooopen hello modell hello tervezőben. 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  Az Adatok lekérése felületen kattintson az **Adatbázis** > **SQL Server-adatbázis** > **Csatlakozás** elemre.  
  
3.  A hello **SQL Server-adatbázis** párbeszédpanelen, a **Server**, írja be a hello hello kiszolgálón, amelyre telepítve hello AdventureWorksDW2014 adatbázis nevét, és kattintson **Connect**.  

4.  Amikor a rendszer kéri tooenter hitelesítő adatokat, Analysis Services tooconnect toohello adatforrást importálásakor és feldolgozásakor az adatok használ a toospecify hello hitelesítő adatok szükségesek. A **Megszemélyesítési mód** alatt válassza a **Fiók megszemélyesítése** lehetőséget, majd adja meg a hitelesítő adatokat, és kattintson a **Csatlakozás** parancsra. Olyan fiókot használjon ahol hello jelszó lejár nem ajánlott.

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > Egy Windows felhasználói fiókot és jelszót segítségével biztosítja a legbiztonságosabb módszer hello csatlakozó tooa adatforrás.
  
5.  A Navigátor, válassza ki a hello **AdventureWorksDW2014** adatbázisából, és kattintson a **OK**. Ez a hello kapcsolat toohello adatbázist hoz létre. 
  
6.  A Navigátor, válassza ki a következő táblák hello jelölőnégyzetét hello: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**,  **DimProductCategory**, **DimProductSubcategory**, és **FactInternetSales**.  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
Miután az OK gombra kattintott, megnyílik a Lekérdezésszerkesztő. Hello a következő szakaszban jelölje ki azt szeretné, hogy tooimport csak hello adatokat.

  
## <a name="filter-hello-table-data"></a>Hello tábla adatok szűrése  
Hello AdventureWorksDW2014 mintaadatbázis táblák nem a modell szükséges tooinclude adatokkal rendelkeznek. Lehetséges, ha azt szeretné, toofilter szükségtelen toosave memórián belüli adatterülethez hello modell által használt ki. Ön kiszűréséhez hello oszlopok a tábla, azok még nem lettek importálva hello a munkaterület-adatbázis és a model adatbázisban hello után üzembe helyezéséig. 
  
#### <a name="toofilter-hello-table-data-before-importing"></a>toofilter hello tábla adatok importálása előtt  
  
1.  A lekérdezés-szerkesztő, válassza ki a hello **DimCustomer** tábla. Hello DimCustomer táblázat hello datasource (a AdventureWorksDWQ2014 mintaadatbázis) nézete jelenik meg. 
  
2.  Válassza ki egyszerre (Ctrl + kattintással) a **SpanishEducation**, **FrenchEducation**, **SpanishOccupation** és **FrenchOccupation** táblát, majd kattintson valamelyikre a jobb gombbal, és válassza az **Oszlopok eltávolítása** parancsot. 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    Mivel ezekben az oszlopokban hello értékei nem megfelelő tooInternet értékesítési elemzés, nincs nincs szükség tooimport ezeket az oszlopokat. A szükségtelen oszlopok eltávolításával csökken a modell mérete, és a modell hatékonyabbá válik.  
  
4.  Minden tábla oszlopainak a következő hello eltávolításával táblák fennmaradó hello szűrése:  
    
    **DimDate**
    
      |Oszlop|  
      |--------|  
      |DateKey|  
      |**SpanishDayNameOfWeek**|  
      |**FrenchDayNameOfWeek**|  
      |**SpanishMonthName**|  
      |**FrenchMonthName**|  
  
    **DimGeography**
  
      |Oszlop|  
      |-------------|  
      |**SpanishCountryRegionName**|  
      |**FrenchCountryRegionName**|  
      |**IpAddressLocator**|  
  
    **DimProduct**
  
      |Oszlop|  
      |-----------|  
      |**SpanishProductName**|  
      |**FrenchProductName**|  
      |**FrenchDescription**|  
      |**ChineseDescription**|  
      |**ArabicDescription**|  
      |**HebrewDescription**|  
      |**ThaiDescription**|  
      |**GermanDescription**|  
      |**JapaneseDescription**|  
      |**TurkishDescription**|  
  
    **DimProductCategory**
  
      |Oszlop|  
      |--------------------|  
      |**SpanishProductCategoryName**|  
      |**FrenchProductCategoryName**|  
  
    **DimProductSubcategory**
  
      |Oszlop|  
      |-----------------------|  
      |**SpanishProductSubcategoryName**|  
      |**FrenchProductSubcategoryName**|  
  
    **FactInternetSales**
  
      |Oszlop|  
      |------------------|  
      |**OrderDateKey**|  
      |**DueDateKey**|  
      |**ShipDateKey**|   
  
## <a name="Import"></a>Importálás hello a kiválasztott táblák és az oszlop adattípusa  
Most, a nyomtatási, és kiszűri a szükségtelen adatokat, hello egyéb hello szeretné, hogy importálhatja. hello varázsló importálja hello tábla adatai mellett táblák közötti kapcsolatokat. Új táblákat és oszlopokat hello modell jönnek létre, és ki szűrt adat nem lesz importálva.  
  
#### <a name="tooimport-hello-selected-tables-and-column-data"></a>tooimport hello a kiválasztott táblák és az oszlop adattípusa  
  
1.  Tekintse át a kiválasztott elemeket. Ha mindent rendben talál, kattintson az **Importálás** parancsra. hello adatfeldolgozási párbeszédpanel a munkaterület-adatbázisba az adatforrás az importált adatok hello állapotát jeleníti meg.
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  Kattintson a **Bezárás** gombra.  

  
## <a name="save-your-model-project"></a>A modellprojekt mentése  
Fontos toofrequently mentése a jelentésmodell-projekt.  
  
#### <a name="toosave-hello-model-project"></a>toosave hello jelentésmodell-projekt  
  
-   Kattintson a **Fájl** > **Az összes mentése** elemre.  
  
## <a name="whats-next"></a>A következő lépések
[3. lecke: Megjelölés dátumtáblaként](../tutorials/aas-lesson-3-mark-as-date-table.md)

  
  
