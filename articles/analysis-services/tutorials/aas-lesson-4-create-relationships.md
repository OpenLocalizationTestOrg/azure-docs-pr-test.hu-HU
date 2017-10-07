---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 4: kapcsolatok létrehozása |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate kapcsolatok hello Azure Analysis Services-oktatóanyag projekt. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="lesson-4-create-relationships"></a>4. lecke: Kapcsolatok létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a lecke hello kapcsolatokat, amelyek létrehozásakor automatikusan importált adatok ellenőrzése, és adja hozzá az új különböző táblák közötti kapcsolatokat. Egy kapcsolat, amely meghatározza, hogyan lehessen őket e-táblázatok adatait hello két tábla közötti kapcsolat. Például hello DimProduct hello DimProductSubcategory táblázat és adatkészletekben hello tényt, hogy minden egyes termék tartozik tooa alkategóriát alapján. több, lásd: toolearn [kapcsolatok](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular).
  
Becsült idő toocomplete Ez a lecke: **10 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 3: be van jelölve dátumtáblázatként](../tutorials/aas-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Meglévő kapcsolatok áttekintése és új kapcsolatok létrehozása  
Adatok beolvasása használatával importálta az adatokat, amikor hét táblák kapott hello AdventureWorksDW2014 adatbázis. Amikor az adatok importálása relációs adatforrás, meglévő kapcsolatok általában automatikusan importált hello adatokkal együtt. Mielőtt azonban folytatná a modell elkészítését, ellenőriznie kell, hogy a táblák közti kapcsolatok megfelelően lettek-e létrehozva. Ebben az oktatóanyagban három új kapcsolatot fog felvenni.  
  
#### <a name="tooreview-existing-relationships"></a>tooreview meglévő kapcsolatok  
  
1.  Kattintson a hello **modell** menü > **modell nézet** > **diagramnézet**.  

    hello modellek tervezőjében megjelenik Diagram nézet megjelenítése közöttük sorok importált összes hello tábla grafikus formátumban. hello sorok táblák között hello kapcsolatokat automatikusan létrehozott hello adatok importálásakor jelzik.
    
    ![aas-lesson4-diagram](../tutorials/media/aas-lesson4-diagram.png)
  
    Tartalmazzák hello táblák a lehető legtöbb minitérkép vezérlők hello modellek tervezőjében hello jobb alsó sarkában. Is kattintson és táblák toodifferent helyek, húzza a táblák közelebb összegyűjtésével, vagy meghatározott sorrendben helyezve. Helyezze át a táblák hello kapcsolatok hello táblák között már nincs hatással. tooview egy adott tábla oszlopainak összes hello kattintson és húzza a egy tábla biztonsági tooexpand vagy méretét.  
  
2.  Kattintson a hello folytonos vonal hello között **DimCustomer** tábla és hello **DimGeography** tábla. hello folytonos vonal e két tábla között látható, a kapcsolat aktív, ez azt jelenti, hogy az alapértelmezés szerint használt DAX-képlet kiszámításakor.  
  
    Értesítés hello **GeographyKey** hello oszlopa **DimCustomer** tábla és hello **GeographyKey** hello oszlopa **DimGeography** tábla most már minden megjelenhetnek használata a. Ezekben az oszlopokban használt hello kapcsolatban. hello kapcsolat tulajdonságainak is megjelennek hello **tulajdonságok** ablak.  
  
    > [!TIP]  
    > Továbbá toousing hello diagram nézetben modellek tervezőjében, használhatja a táblázatban minden táblák közötti hello kapcsolatok kezelése párbeszédpanel bezárásához tooshow hello kapcsolatok. A Tabular Model Explorerben kattintson a jobb gombbal a **Kapcsolatok** > **Kapcsolatok kezelése** elemre.
  
3.  Ellenőrizze a kapcsolat a következő hello hozta létre, ha hello táblákat hello AdventureWorksDW adatbázis importált:  
  
    |Aktív|Tábla|Kapcsolódó keresési tábla|  
    |----------|---------|------------------------|  
    |Igen|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Igen|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Igen|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Igen|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Igen|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Ha hello kapcsolatok bármelyike hiányzik, ellenőrizze, hogy a modell az alábbiak tartalmazza a következő táblák hello: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory és FactInternetSales. Ha ugyanazon adatforrás-kapcsolat importálásának: hello táblák alkalommal külön, ezek a táblázatok közötti kapcsolatok nem jönnek létre, és manuálisan kell létrehozni.  

### <a name="take-a-closer-look"></a>Közelebbi vizsgálat
A Diagram nézet figyelje meg nyíl, csillag és egy számot a táblák közötti kapcsolathoz hello hello sorok.

![aas-lesson4-line](../tutorials/media/aas-lesson4-line.png)

hello nyíl hello szűrés irányának jeleníti meg. hello csillag látható, ez a táblázat több oldalán hello hello kapcsolat számossága, és hello egyik azt jelzi, ez a táblázat hello hello kapcsolat egyik oldala. Ha a kapcsolat; tooedit van szüksége például hello kapcsolat szűrés irányának vagy számossága módosításához kattintson duplán a hello kapcsolat sor tooopen hello kapcsolat szerkesztése párbeszédpanel.

![aas-lesson4-edit](../tutorials/media/aas-lesson4-edit.png)

Ezek a szolgáltatások speciális adatmodelleket célja, és ebben az oktatóanyagban külső hello hatókörét. több, lásd: toolearn [szűrők az Analysis Services táblázatos modellek közötti kétirányú](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services).

Egyes esetekben szükség lehet toocreate további kapcsolatokat a táblák között a modell toosupport az egyes üzleti logikát. Ebben az oktatóanyagban kell toocreate három további kapcsolatok hello FactInternetSales táblának és hello DimDate tábla között.  
  
#### <a name="tooadd-new-relationships-between-tables"></a>tooadd új kapcsolatokat a táblák között  
  
1.  A hello modellek tervezőjében, a hello **FactInternetSales** tábla, kattintson és tartsa lenyomva az hello **orderdate oszlopra** oszlopban, majd húzza hello kurzor toohello **dátum** hello oszlopa **DimDate** tábla, majd engedje fel.  

    Folytonos vonal ekkor megjelenik egy aktív kapcsolat hello között létrehozott **orderdate oszlopra** hello oszlopa **Internet értékesítési** tábla és hello **dátum** hello oszlopa **Dátum** tábla. 
  
      ![aas-lesson4-new](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > Kapcsolatok létrehozásakor hello számossága és a szűrés irányát hello elsődleges és hello kapcsolódó keresési tábla közötti automatikusan ki van jelölve.  
  
2.  A hello **FactInternetSales** tábla, kattintson és tartsa lenyomva az hello **DueDate** oszlopban, majd húzza hello kurzor toohello **dátum** hello oszlopa **DimDate** tábla, majd engedje fel.  
  
    Megjelenik egy létrehozott egy hello inaktív kapcsolatát ábrázoló **DueDate** hello oszlopa **FactInternetSales** tábla és hello **dátum** oszlopa Hello **DimDate** tábla. A táblák között több kapcsolattal is rendelkezhet, azonban egy időben csak egy kapcsolat lehet aktív. Inaktív kapcsolatok tehető aktív tooperform különleges összesítések egyéni DAX-kifejezésekben.  
  
3.  Végül hozzon létre még egy kapcsolatot. A hello **FactInternetSales** tábla, kattintson és tartsa lenyomva az hello **szállítás** oszlopban, majd húzza hello kurzor toohello **dátum** hello oszlopa **DimDate** tábla, majd engedje fel.  
    
     ![aas-lesson4-newinactive](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>A következő lépések
[5. lecke: Számított oszlopok létrehozása](../tutorials/aas-lesson-5-create-calculated-columns.md)
  
  
  
