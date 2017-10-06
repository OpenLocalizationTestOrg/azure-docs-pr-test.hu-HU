---
cím: aaa "Azure Analysis Services útmutató kiegészítő lecke: dinamikus biztonsági |} Microsoft Docs"Leírás: ismerteti, hogyan toouse dinamikus biztonsági sor használatával hello Azure Analysis Services-oktatóanyag szűrők.
szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="supplemental-lesson---dynamic-security"></a>Kiegészítő lecke – Dinamikus biztonság

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a kiegészítő leckében további, dinamikus biztonságot megvalósító szerepkört hoz létre. Dinamikus biztonsági hello felhasználói nevét vagy a bejelentkezési azonosító alapján hello éppen bejelentkezett felhasználó sorszintű biztonságot nyújt. 
  
dinamikus biztonsági tooimplement, akkor hello felhasználó nevét, mely toohello modell csatlakozhat, és keresse meg a modell objektumokat és adatokat tartalmazó tábla tooyour modell hozzáadása. Ez az oktatóanyag segítségével létrehozott hello modell az Adventure Works; hello környezetben van azonban toocomplete Ez rész, hozzá kell adni a felhasználók a saját tartomány tartalmazó tábla. A hozzáadott hello felhasználónevek nem kell hello jelszavakat. egy EmployeeSecurity tábla, a felhasználók a saját tartomány mintát toocreate szolgáltatással hello beillesztés, beillesztés, az Excel-számolótábla alkalmazott adatait. Egy valós forgatókönyv tartalmazó felhasználónevek hello tábla általában lenne egy tábla tényleges adatbázisból adatforrásként; például egy valós DimEmployee táblát.  
  
tooimplement dinamikus biztonsági, két DAX-funkciók használata: [USERNAME függvény (DAX)](http://msdn.microsoft.com/22dddc4b-1648-4c89-8c93-f1151162b93f) és [LOOKUPVALUE függvény (DAX)](http://msdn.microsoft.com/73a51c4d-131c-4c33-a139-b1342d10caab). Ezek a függvények a sorszűrő képletben alkalmazva egy új szerepkörben vannak meghatározva. Hello LOOKUPVALUE függvény használatával hello képlet hello EmployeeSecurity táblából értékkel adható meg. hello képlet Ezután adja át, hogy a érték toohello felhasználónév funkció, amely a bejelentkezett felhasználó hello hello felhasználónevét határozza meg toothis szerepkör tartozik. hello felhasználói kereshet csak hello szerepkör sorszűrőket által megadott adatokat. Ebben az esetben kell megadni, hogy az alkalmazottak értékesítési csak tallózással Internet értékesítési adatok hello értékesítési régiókat tagja.  
  
Ezeket a feladatokat, amelyek egyedi toothis Adventure Works táblázatos modell forgatókönyv azonban csak akkor nem feltétlenül vonatkozik tooa valós forgatókönyvvel használja, így azonosítja. Minden tevékenység hello feladat hello célját leíró további adatokat is tartalmaz.  
  
Becsült idő toocomplete Ez a lecke: **30 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a kiegészítőlecke-témakör a táblázatos modellezésről szóló oktatóanyag része, amelyet sorrendben kell elvégezni. Ez a kiegészítő lecke hello feladatok elvégzése előtt kell befejeződött az összes korábbi során tapasztalatokat.  
  
## <a name="add-hello-dimsalesterritory-table-toohello-aw-internet-sales-tabular-model-project"></a>Hello DimSalesTerritory tábla toohello AW Internet értékesítési táblázatos jelentésmodell-projekt hozzáadása  
tooimplement dinamikus biztonsági Adventure Works példánkban, hozzá kell adnia a két további táblákon tooyour modell. hello ad hozzá (a értékesítés területén) DimSalesTerritory az első tábla hello azonos AdventureWorksDW adatbázishoz. Egy sor szűrő toohello SalesTerritory táblázat, amely meghatározza az adott adatok hello később telepítését bejelentkezett felhasználó hello tallózhatja.  
  
#### <a name="tooadd-hello-dimsalesterritory-table"></a>tooadd hello DimSalesTerritory tábla  
  
1.  A Táblázatos modelltallózó **Adatforrások** szakaszában kattintson a jobb gombbal a kapcsolatra, majd kattintson az **Új táblázatok importálása** gombra.  

    Hello megszemélyesítési hitelesítő párbeszédpanel megjelenésekor írja be a hello megszemélyesítési hitelesítő adatokat használja a 2: adatok hozzáadása.
  
2.  A Navigátor, válassza ki a hello **DimSalesTerritory** tábla, és kattintson a **OK**.    
  
3.  A lekérdezés-szerkesztőben kattintson hello **DimSalesTerritory** lekérdezése, és távolítsa el a **SalesTerritoryAlternateKey** oszlop.  
  
7.  Kattintson az **Importálás** gombra.  
  
    hello új hozzá toohello modell munkaterületen. A AW Internet értékesítési táblázatos modell majd importálja a objektumok és hello DimSalesTerritory forrástábla adatait.  
  
9. Miután hello tábla importálása sikeresen megtörtént, kattintson **Bezárás**.  

## <a name="add-a-table-with-user-name-data"></a>A felhasználónevekre vonatkozó adatokat tartalmazó táblázat hozzáadása  
hello DimEmployee tábla hello AdventureWorksDW mintaadatbázis hello AdventureWorks tartomány felhasználóinak tartalmazza. Ezek a felhasználónevek nem léteznek a saját környezetében. A modellben létre kell hoznia egy táblázatot, amely a szervezet aktuális felhasználói közül tartalmaz néhányat példaként (legalább hármat). Majd adja hozzá ezeket a felhasználókat tagok toohello új szerepkörként. Hello jelszavak a hello minta felhasználónevek nem szükséges, de a saját tartomány kell a tényleges Windows-felhasználó nevét.  
  
#### <a name="tooadd-an-employeesecurity-table"></a>egy EmployeeSecurity tábla tooadd  
  
1.  Nyissa meg a Microsoft Excelt, és hozzon létre egy munkalapot.  
  
2.  Hello a következő táblázat, beleértve a fejlécsor hello, másolja és illessze be hello munkalap.  

    ```
      |EmployeeId|SalesTerritoryId|FirstName|LastName|LoginId|  
      |---------------|----------------------|--------------|-------------|------------|  
      |1|2|<user first name>|<user last name>|\<domain\username>|  
      |1|3|<user first name>|<user last name>|\<domain\username>|  
      |2|4|<user first name>|<user last name>|\<domain\username>|  
      |3|5|<user first name>|<user last name>|\<domain\username>|  
    ```

3.  Cserélje le hello utónevét, vezetéknevét és tartomány\felhasználónév hello nevét és a szervezet a három felhasználói bejelentkezési azonosítókat. PUT hello hello első két sorának EmployeeId 1, a felhasználó tartozik, mint egy értékesítési terület toomore megjelenítése a felhasználónak. Hello EmployeeId és SalesTerritoryId mezőt hagyja szerint.  
  
4.  Hello munkalap mentése mint **SampleEmployee**.  
  
5.  Hello munkalapon levő cellák összes hello hello fejlécekkel együtt alkalmazott adatokkal, majd kattintson a jobb gombbal a kiválasztott hello adatokat, és kattintson **másolási**.  
  
6.  Az SSDT, kattintson a hello **szerkesztése** menüben, majd kattintson **Beillesztés**.  
  
    Beillesztés szürkén jelenik meg, ha egyetlen oszlop sem bármely táblájának hello modell-Tervező ablakában kattintson, és próbálkozzon újra.  
  
7.  A hello **Beillesztés Preview** párbeszédpanel **táblanév**, típus **EmployeeSecurity**.  
  
8.  A **beillesztett adatokat toobe**, ellenőrizze a hello adatok tartalmazzák az összes hello felhasználói adatok és hello SampleEmployee munkalapról fejlécek.  
  
9. Ellenőrizze, hogy az **Első sor elemeinek használata oszlopfejlécként** be van-e jelölve, majd kattintson az **OK** gombra.  
  
    Hello SampleEmployee munkalapról másolt alkalmazott adatokra EmployeeSecurity nevű új tábla létrehozása.  
  
## <a name="create-relationships-between-factinternetsales-dimgeography-and-dimsalesterritory-table"></a>Kapcsolatok létrehozása a FactInternetSales, a DimGeography és a DimSalesTerritory táblák között  
hello FactInternetSales DimGeography és DimSalesTerritory tábla összes egy közös oszlop SalesTerritoryId tartalmazhat. hello SalesTerritoryId oszlop hello DimSalesTerritory tábla minden egyes értékesítési terület-azonosító értékeket tartalmaz.  
  
#### <a name="toocreate-relationships-between-hello-factinternetsales-dimgeography-and-hello-dimsalesterritory-table"></a>hello FactInternetSales DimGeography és hello DimSalesTerritory tábla toocreate kapcsolatai  
  
1.  A Diagram nézetben hello **DimGeography** tábla, kattintson és tartsa lenyomva az hello **SalesTerritoryId** oszlopban, majd húzza hello kurzor toohello **SalesTerritoryId** hello oszlopa **DimSalesTerritory** tábla, majd engedje fel.  
  
2.  A hello **FactInternetSales** tábla, kattintson és tartsa lenyomva az hello **SalesTerritoryId** oszlopban, majd húzza hello kurzor toohello **SalesTerritoryId** hello oszlopa **DimSalesTerritory** tábla, majd engedje fel.  
  
    Értesítés hello aktív tulajdonság az adott kapcsolathoz értéke HAMIS, ami azt jelenti, inaktív. hello FactInternetSales táblának már van egy másik aktív kapcsolat.  
  
## <a name="hide-hello-employeesecurity-table-from-client-applications"></a>Hello EmployeeSecurity tábla az ügyfélalkalmazásokból elrejtése  
Ebben a feladatban elrejtése hello EmployeeSecurity tábla, lekapcsolva tartja a ügyfélalkalmazás listán jelennek meg. Ne feledje, hogy a táblázat elrejtése nem nyújt a táblázat számára védelmet. A felhasználók továbbra is lekérdezhetik az EmployeeSecurity tábla adatait, ha tisztában vannak annak módjával. toosecure hello EmployeeSecurity tábla adatai, hogy a felhasználók képesek tooquery alatt az adatok, a későbbi tevékenység a szűrő alkalmazása.  
  
#### <a name="toohide-hello-employeesecurity-table-from-client-applications"></a>toohide hello EmployeeSecurity tábla ügyfélalkalmazások  
  
-   A hello modellek tervezőjében, a Diagram nézet megnyitásához kattintson a jobb gombbal hello **alkalmazott** tábla fejlécére, és kattintson a **elrejtése az ügyféleszközök elől**.  
  
## <a name="create-a-sales-employees-by-territory-user-role"></a>Területenkénti értékesítési alkalmazottak felhasználói szerepkör létrehozása  
Ebben a feladatban egy felhasználói szerepkört hozhat létre. Ez a szerepkör szűrője sor látható toousers mely hello DimSalesTerritory sorainak megadása. hello majd szűrőhöz hello egy-a-többhöz kapcsolat irányát tooall más táblák kapcsolódó tooDimSalesTerritory. Is, amely biztosítja a hello teljes EmployeeSecurity táblázat lekérdezhető bármely felhasználó, amelynek hello szerepkör tagja nem szűrőt alkalmaz.  
  
> [!NOTE]  
> hello értékesítési alkalmazottak hoz létre, ez a lecke területén szerepkör tagjai toobrowse (lekérdezés) csak értékesítési adatainak vagy hello értékesítési terület toowhich tartoznak korlátozza. Ha hozzáad egy felhasználót egy tag toohello értékesítési az alkalmazottak által területén szerepkör, amely tagja a szerepkörnek létre is létezik, [lecke 11: szerepkörök létrehozása](../tutorials/aas-lesson-11-create-roles.md), engedélyek kombinációja kap. Amikor egy felhasználó tagja több szerepkörök, hello engedélyek és az egyes szerepkörhöz definiált sorszűrőket halmozódik. Ez azt jelenti, hogy hello felhasználó jogosult hello nagyobb szerepkörök hello kombinációja határozza meg.  
  
#### <a name="toocreate-a-sales-employees-by-territory-user-role"></a>egy eladási alkalmazottak területén felhasználói szerepkör által toocreate  
  
1.  Az SSDT, kattintson a hello **modell** menüben, majd kattintson **szerepkörök**.  
  
2.  A **Szerepkörkezelőben** kattintson az **Új** lehetőségre.  
  
    Új szerepkör hello nincs engedély toohello lista hozzáadásával.  
  
3.  Hello új szerepkört, kattintson, majd a hello **neve** oszlop, nevezze át a hello szerepkör túl**értékesítési alkalmazottak terület**.  
  
4.  A hello **engedélyek** oszlopban kattintson hello legördülő listára, és válassza a hello **olvasási** engedéllyel.  
  
5.  Kattintson a hello **tagok** fülre, majd **Hozzáadás**.  
  
6.  A hello **felhasználó vagy csoport kiválasztása** párbeszédpanel **tooselect nevű Enter hello objektum**, típus hello első minta használt felhasználónévhez hello EmployeeSecurity tábla létrehozásakor. Kattintson a **Névellenőrzés** tooverify hello felhasználó neve érvényes, és kattintson **Ok**.  
  
    Ismételje meg ezt a lépést, hello hozzáadása más felhasználó példanevek hello EmployeeSecurity tábla létrehozásakor használt.  
  
7.  Kattintson a hello **sorszűrőket** fülre.  
  
8.  A hello **EmployeeSecurity** táblájában hello **DAX-szűrő** oszlop, a következő képlet típus hello:  
  
    ```
      =FALSE()  
    ```
  
    A képlet határozza meg, hogy az összes oszlop megoldásához toohello HAMIS logikai értékként. Nincs hello EmployeeSecurity tábla oszlopait hello értékesítési alkalmazottak területén felhasználói szerepkör tagjának kérdezhetők le.  
  
9. A hello **DimSalesTerritory** tábla, a következő képlet típus hello:  

    ```  
    ='Sales Territory'[Sales Territory Id]=LOOKUPVALUE('Employee Security'[Sales Territory Id], 
      'Employee Security'[Login Id], USERNAME(), 
      'Employee Security'[Sales Territory Id], 
      'Sales Territory'[Sales Territory Id]) 
    ```
  
    Ez a képlet hello LOOKUPVALUE függvény adja vissza hello DimEmployeeSecurity [SalesTerritoryId] oszlop összes értékének ahol hello EmployeeSecurity [LoginId] van hello ugyanaz, mint hello aktuális bejelentkezett Windows-felhasználónév és a EmployeeSecurity [ SalesTerritoryId] van hello ugyanaz, mint a hello DimSalesTerritory [SalesTerritoryId].  
  
    hello LOOKUPVALUE által visszaadott értékesítési terület azonosítók készlete nem használt toorestrict hello hello DimSalesTerritory táblázatban látható sorok. Hello hello sorára SalesTerritoryID esetén hello készlet hello LOOKUPVALUE függvény által visszaadott azonosítókat csak sorok jelennek meg.  
  
10. A Szerepkörkezelőben kattintson az **OK** gombra.  
  
## <a name="test-hello-sales-employees-by-territory-user-role"></a>Hello értékesítési alkalmazottak területén felhasználói szerepkör által tesztelése  
Ebben a feladatban használható SSDT tootest hello hatékonyságát hello értékesítési alkalmazottak területén felhasználói szerepkör által hello elemzés az Excelben funkciót. Adjon meg egy hello felhasználónevek toohello EmployeeSecurity tábla hozzáadott és hello szerepkör tagjaként. Ez a felhasználónév majd lesz hello tényleges felhasználónevet észlelt a hello kapcsolat létrehozása az Excel és hello modell között.  
  
#### <a name="tootest-hello-sales-employees-by-territory-user-role"></a>tootest hello értékesítési alkalmazottak területén felhasználói szerepkör  
  
1.  Az SSDT, kattintson a hello **modell** menüben, majd kattintson **elemzés az Excelben**.  
  
2.  A hello **elemzés az Excelben** párbeszédpanel **megadása hello felhasználó neve vagy szerepe toouse tooconnect toohello modell**, jelölje be **más Windows-felhasználó**, és kattintson a **Tallózás**.  
  
3.  A hello **felhasználó vagy csoport kiválasztása** párbeszédpanel **adja meg a hello objektum neve tooselect**, írjon be egy felhasználónevet, hello EmployeeSecurity táblázat foglalja, és kattintson a **Névellenőrzés**.  
  
4.  Kattintson a **Ok** tooclose hello **felhasználó vagy csoport kiválasztása** párbeszédpanel, és kattintson **Ok** tooclose hello **elemzés az Excelben** párbeszédpanel megnyitásához.  
  
    Megnyílik az Excel, és megjelenít egy új munkafüzetet. Ekkor automatikusan létrejön egy kimutatás. hello kimutatástábla mezői lista tartalmazza a legtöbb hello adatok mezőket az új modell érhető el.  
  
    Értesítés hello EmployeeSecurity tábla nincs hello kimutatástábla mezői listája látható. Ezt a táblázatot elrejtette az ügyféleszközök elől az előző feladatban.  
  
5.  A hello **mezők** listájában, a **∑ Internet értékesítési** (mértékek), jelölje be hello **InternetTotalSales** mérték. hello mérték bekerül a hello **értékek** mezőket.  
  
6.  Jelölje be hello **SalesTerritoryId** hello oszlopát **DimSalesTerritory** tábla. hello oszlop bekerül a hello **sorfeliratok** mezőket.  
  
    Értékesítési adataitól csak hello egy régió toowhich hello hatékony használt felhasználónévhez jelenik meg értesítés Internet tartozik. Ha egy másik oszlop, például a város hello DimGeography táblából sort címke mező, csak városokat hello értékesítési terület toowhich hello hatékony felhasználó tartozik, jelennek meg.  
  
    Ez a felhasználó nem keresse meg, illetve bármely Internet tartoznak egyik hello eltérő területek értékesítési adatokat lekérdezni. Ez a korlátozás van, mert hello hello DimSalesTerritory tábla, hello értékesítési alkalmazottak területén felhasználói szerepkör által meghatározott Sorszűrő biztosítja az összes adat adatokat kapcsolódó tooother értékesítési régiókat.  
  
## <a name="see-also"></a>Lásd még:  
[USERNAME függvény (DAX)](https://msdn.microsoft.com/library/hh230954.aspx)  
[LOOKUPVALUE függvény (DAX)](https://msdn.microsoft.com/library/gg492170.aspx)  
[CUSTOMDATA függvény (DAX)](https://msdn.microsoft.com/library/hh213140.aspx)  