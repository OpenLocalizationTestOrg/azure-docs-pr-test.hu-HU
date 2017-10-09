---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 10: partíciókat |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate particionálja hello Azure Analysis Services-oktatóanyag projektben. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="lesson-10-create-partitions"></a>10. lecke: Partíciók létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a lecke partíciók toodivide hello FactInternetSales táblának kisebb logikai részekre feldolgozott (frissítése) független a többi partíció lehet létrehozni. Alapértelmezés szerint minden tábla szerepel a modellben van egy partíciót, amely tartalmazza az összes hello tábla oszlopainak és sorainak. Hello FactInternetSales táblának, az azt szeretnénk toodivide hello adatok évente; egy partíciót az egyes hello tábla öt évet. Ezután mindegyik partíció egymástól függetlenül kezelhető. több, lásd: toolearn [partíciók](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular). 
  
Becsült idő toocomplete Ez a lecke: **15 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 9: hozzon létre hierarchiákat](../tutorials/aas-lesson-9-create-hierarchies.md).  
  
## <a name="create-partitions"></a>Partíciók létrehozása  
  
#### <a name="toocreate-partitions-in-hello-factinternetsales-table"></a>hello FactInternetSales táblának toocreate a partíciók  
  
1.  A Tabular Model Explorerben bontsa ki a **Táblák** fát, és kattintson a jobb gombbal a **FactInternetSales** > **Partíciók** elemre.  
  
2.  A partíció-kezelőben kattintson **másolási**, majd módosítsa a hello neve túl**FactInternetSales2010**.
  
    Mivel hello partíció tooinclude csak azok a sorok adott időszakon belüli, hello év 2010, módosítania kell a hello lekérdezési kifejezésben.
  
4.  Kattintson a **tervezési** tooopen szerkesztő lekérdezni, és kattintson a hello **FactInternetSales2010** lekérdezés.

5.  A képen kattintson a lefelé mutató nyílra a hello hello **orderdate oszlopra** oszlop fejlécére, és kattintson **dátum/idő szűrők** > **közötti**.

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  Hello sorok szűrése párbeszédpanelen a **szűrési feltételek: orderdate oszlopra**, hagyja **ez utáni vagy egyenlő**, majd a hello dátum mezőben adja meg **1/1/2010**. Hello hagyja **és** operátor kiválasztva, majd válassza ki **előtt**, majd hello dátum mezőjében adja meg **2011-1-1**, és kattintson a **OK**.

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    Észreveheti, hogy a lekérdezésszerkesztőben az ALKALMAZOTT LÉPÉSEK között egy további lépés látható, Szűrt sorok néven. Ez a szűrő csak rendelési dátumokat tooselect 2010.

8.  Kattintson az **Importálás** gombra.

    A Partíciókezelőben, figyelje meg a kifejezés most már rendelkezik egy további szűrt záradék hello lekérdezés.

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    A jelen nyilatkozat határozza meg, ez a partíció ezeket ahol hello orderdate oszlopra van hello 2010 naptári év hello szűrt sorokat záradékban megadott sorok csak hello adatok tartalmaznia kell.  
  
  
#### <a name="toocreate-a-partition-for-hello-2011-year"></a>egy partíció hello 2011 év toocreate  
  
1.  Hello partíciók listájában kattintson hello **FactInternetSales2010** létrehozott partíció, és kattintson a **másolási**.  Hello szolgáltatáspartíció neve túl módosítása**FactInternetSales2011**. 

    Nem kell toouse Lekérdezésszerkesztő toocreate egy új szűrt záradék. Hello lekérdezés másolatát 2010 hozott létre, mert toodo szüksége olyan módosítást enyhe hello lekérdezésben 2011.
  
2.  A **lekérdezési kifejezésben**, a-ahhoz, hogy a partíció tooinclude csak a hello sorok 2011 év, cserélje le a szűrt sorok záradékot hello hello éveket **2011** és **2012**, osztályban, például:  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="toocreate-partitions-for-2012-2013-and-2014"></a>toocreate partíciók 2012, a 2013-as és a 2014.  
  
- Hajtsa végre a hello előző lépéseket, partíciók létrehozása 2012, a 2013-as és a 2014, az adott év hello hello szűrt sorok záradék tooinclude csak sorok éveket módosítása. 
  

## <a name="delete-hello-factinternetsales-partition"></a>Hello FactInternetSales partíció törlése
Most, hogy partíciók évente, törölheti hello FactInternetSales partíció; Hozzon létre megakadályozza az összes folyamat kiválasztásakor, partíció feldolgozása során.

#### <a name="toodelete-hello-factinternetsales-partition"></a>toodelete hello FactInternetSales partíció
-  Kattintson a hello FactInternetSales partíciót, majd **törlése**.



## <a name="process-partitions"></a>Partíciók feldolgozása  
A Partíciókezelőben, figyelje meg hello **utolsó feldolgozott** oszlop az egyes hello mutat be, ezek a partíciók soha nem dolgozott létrehozott új partíciók. Partíciók létrehozása kell futtatni egy folyamatot partíciók, sem a folyamat művelet toorefresh hello táblaadatok ezek a partíciók.  
  
#### <a name="tooprocess-hello-factinternetsales-partitions"></a>tooprocess hello FactInternetSales partíciók  
  
1.  Kattintson a **OK** tooclose partíciókezelő.  
  
2.  Kattintson a hello **FactInternetSales** tábla, majd kattintson az hello **modell** menü > **folyamat** > **folyamat partíciók**.  
  
3.  A hello folyamat partíciók párbeszédpanelen ellenőrizze **mód** értéke túl**folyamat alapértelmezett**.  
  
4.  Jelölje be a hello jelölőnégyzetet a hello **folyamat** oszlop az egyes hello öt particionálja a létrehozott, és kattintson **OK**.  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    Ha a megszemélyesítési hitelesítő adatokat kéri, írja be a hello Windows rendszerbeli felhasználónevet és jelszót a megadott a 2.  
  
    Hello **adatfeldolgozási** párbeszédpanel jelenik meg, és minden partíció esetében folyamat részleteit jeleníti meg. Láthatja, hogy az egyes partíciók esetében eltérő mennyiségű sor átvitele történik meg. Mindegyik partíció csak azok a sorok hello hello SQL-utasításban lévő WHERE záradéknak megadott hello év magában foglalja. Ha feldolgozása befejeződött, lépjen tovább, és hello adatfeldolgozási párbeszédpanel bezárásához.  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a>A következő lépések
Nyissa meg toohello következő lecke: [lecke 11: szerepkörök létrehozása](../tutorials/aas-lesson-11-create-roles.md). 
