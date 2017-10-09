---
title: "SQL-összekötő aaaGeneric |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure Microsoft általános SQL-összekötő."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2eab8f0894e83ab4738b9f2deb05b03cdc9a9d43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-technical-reference"></a>Általános SQL összekötő műszaki útmutatója
Ez a cikk ismerteti a hello általános SQL-összekötőt. hello cikk vonatkozik toohello a következő termékek:

* A Microsoft Identity Manager 2016 (MIM2016)
* A Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Kell használnia a 4.1.3671.0 gyorsjavítás vagy újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2, hello összekötő rendelkezésre áll hello letölthető [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

toosee az összekötő működés közben, lásd: hello [részletes általános SQL összekötő](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) cikk.

## <a name="overview-of-hello-generic-sql-connector"></a>Hello általános SQL-összekötő áttekintése
hello általános SQL-összekötő lehetővé teszi toointegrate hello szinkronizálási szolgáltatás, amely nyújt, ODBC-kapcsolat adatbázis rendszerrel.  

Magas szintű szempontjából a következő funkciók hello hello hello összekötő aktuális kiadása támogatja:

| Szolgáltatás | Támogatás |
| --- | --- |
| Csatlakoztatott adatforrás |hello összekötő minden 64 bites ODBC-illesztőprogramok használata támogatott. Hello következő tesztelték: <li>Microsoft SQL Server és SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 & 11 g</li><li>MySQL 5.x</li> |
| Forgatókönyvek |<li>Objektum életciklusának kezelésére</li><li>Jelszókezelés</li> |
| Műveletek |<li>Teljes importálás és a különbözeti importálás, exportálás</li><li>Az exportált: Hozzáadása, törlése, frissítés, és cserélje le</li><li>Jelszó beállítása, a jelszó módosítása</li> |
| Séma |<li>Az objektumok és attribútumok dinamikus felderítése</li> |

### <a name="prerequisites"></a>Előfeltételek
Összekötő hello használata előtt győződjön meg arról hello következő hello szinkronizálási kiszolgálón rendelkezik:

* Microsoft keretrendszer 4.5.2-es vagy újabb verzió
* 64 bites ügyfél illesztőprogramok

### <a name="permissions-in-connected-data-source"></a>Engedélyek szükségesek a csatlakoztatott adatforrás
toocreate hello támogatott műveletek elvégzését általános SQL Connector, kell rendelkeznie:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Portok és protokollok
Hello hello ODBC-illesztőprogram toowork szükséges portok hello adatbázis gyártójának dokumentációjában talál.

## <a name="create-a-new-connector"></a>Új összekötő létrehozása
tooCreate általános SQL-összekötőhöz, a **szinkronizálási szolgáltatás** kiválasztása **kezelőügynök** és **létrehozása**. Jelölje be hello **általános SQL (Microsoft)** összekötő.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Kapcsolatok
hello összekötő egy ODBC Adatforrásnevet fájlt használja a hálózati kapcsolatot. Hozzon létre hello DSN fájl használatával **ODBC adatforrások** hello start menüjében alatt található **felügyeleti eszközök**. Hello felügyeleti eszköz, hozzon létre egy **fájl DSN** , az összekötő toohello megadható.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

hello kapcsolat képernyő esetén hello először hoz létre egy új általános SQL-összekötőt. Először tooprovide hello a következő információkat:

* DSN fájl elérési útja
* Authentication
  * Felhasználónév
  * Jelszó

hello adatbázis támogatnia kell az alábbi hitelesítési módszerek egyikét:

* **Windows-hitelesítés**: hello adatbázis hitelesítése hello Windows hitelesítő adatok tooverify hello felhasználót használja. hello felhasználónév és jelszó megadott hello adatbázissal használt tooauthenticate. Ez a fiók engedélyek toohello adatbázis szükséges.
* **SQL-hitelesítés**: hello adatbázis által használt hello felhasználónév és jelszó hitelesítéséhez definiált egy hello kapcsolat képernyő tooconnect toohello adatbázis. Ha hello felhasználói név/jelszavak hello DSN fájl tárolja, a megadott hello kapcsolat képernyőn hello hitelesítő adatok prioritást élveznek.
* **Az Azure SQL-adatbázis hitelesítési**: további információkért lásd: [tooSQL adatbázis által használata Azure Active Directory-hitelesítéssel csatlakozzon](../../sql-database/sql-database-aad-authentication.md).

**Megkülönböztető név horgonyzási**: ezt a beállítást, ha hello DN hello horgonyattribútum is használatos. Egy egyszerű végrehajtását is használható, de szintén tartalmazza a következő korlátozás hello:

* Összekötő csak egy objektumtípus támogatja. Ezért az összes hivatkozás attribútumok csak hivatkozhat hello ugyanazon objektumtípust.

**Exportálás típusa: Az objektum a név felülírandó**: exportálás során a csak néhány attribútum módosult, amikor ki hello teljes objektum attribútumainak exportálja, és felülírja a meglévő objektum hello.

### <a name="schema-1-detect-object-types"></a>Séma 1 (Hibakeresés objektumtípusok)
Ezen a lapon fog tooconfigure hello összekötő Mitől folyamatos toofind hello különböző objektumtípusokra hello adatbázisban.

Minden objektum típus, egy partíciót mutatják be, és konfigurálva van a további **konfigurálása partíciók és hierarchiák**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Az objektum típusa észlelési módszer**: hello összekötő támogatja az objektum típusa észlelési módszerek.

* **Rögzített értékével**: hello listáját objektumtípusok vesszővel tagolt listáját tartalmazzák. Például: `User,Group,Department`.  
  ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
* **Tábla/nézet vagy tárolt eljárás**: hello tábla/nézet vagy tárolt eljárás neve hello és hello oszlopnevet, amely objektumtípusok hello listáját tartalmazza. Ha egy tárolt eljárás használata is adja meg paraméterek azt hello formátumban **[Name]: [irány]: [érték]**. Adja meg az egyes paramétereket külön sorban (használja a Ctrl + Enter tooget új sor).  
  ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
* **SQL-lekérdezés**: Ez a beállítás lehetővé teszi egy SQL-lekérdezést csak egy oszlop objektumtípusokat, például visszaadó tooprovide `SELECT [Column Name] FROM TABLENAME`. hello visszaadott oszlopnak kell lennie (varchar) karakterlánc típusú.

### <a name="schema-2-detect-attribute-types"></a>Séma 2 (Hibakeresés attribútum esetében)
Ezen a lapon fog tooconfigure hogyan hello attribútumnevek és típusok fog toobe észlelte. minden észlelt hello előző lapon objektumtípus hello konfigurációs beállítások listája.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Attribútum-típus észlelési metódusát**: hello összekötő támogatja az attribútum típusa észlelési módszerek minden észlelt objektumtípus séma 1 képernyőn.

* **Tábla/nézet vagy tárolt eljárás**: hello tábla/nézet vagy tárolt eljárás, amelyeket használt toofind hello attribútumnevek hello nevét adja meg. Ha egy tárolt eljárás használata is adja meg paraméterek azt hello formátumban **[Name]: [irány]: [érték]**. Adja meg az egyes paramétereket külön sorban (használja a Ctrl + Enter tooget új sor). toodetect hello attribútumnevek többértékű attribútumban adja meg a táblák vagy nézetek vesszővel tagolt listája. Többértékű forgatókönyvek nem támogatottak. Ha a szülő és gyermek tábla ugyanazon oszlopnevek rendelkeznek.
* **SQL-lekérdezés**: Ez a beállítás lehetővé teszi egy SQL-lekérdezést csak egy oszlop attribútum nevekkel, például visszaadó tooprovide `SELECT [Column Name] FROM TABLENAME`. hello visszaadott oszlopnak kell lennie (varchar) karakterlánc típusú.

### <a name="schema-3-define-anchor-and-dn"></a>Séma 3 (Define horgony és DN)
Ez a lap lehetővé teszi, hogy tooconfigure horgony és a megkülönböztető név attribútum az egyes észlelt objektum. Több attribútumok toomake hello rögzítési egyedi választhatja ki.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

* Többértékű és logikai típusú attribútumok nem találhatók.
* Ugyanaz az attribútum nem használható megkülönböztető Nevet és a horgony, kivéve, ha **DN horgonyzási** hello kapcsolat lapon van kiválasztva.
* Ha **DN horgonyzási** kijelölt hello kapcsolat oldalon ezen a lapon csak hello megkülönböztető név attribútum szükséges. Ez az attribútum hello horgonyattribútum akkor is használható.

  ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Séma 4 (Define attribútum típusát, hivatkozás és iránya)
Ez a lap lehetővé teszi tooconfigure hello attribútum típus, például az egész szám, a bináris, vagy a logikai és a minden attribútum irányát. Összes attribútum oldalról **séma 2** vannak felsorolva, beleértve a többértékű attribútumok.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

* **DataType**: toomap hello típus toothose attribútumtípust hello szinkronizálási motor által ismert használt. hello alapértelmezett toouse hello azonos adja meg, ahogy hello SQL-séma észlelt, de dátum és idő és a hivatkozás nem könnyen felismerhető. A, toospecify kell **DateTime** vagy **hivatkozás**.
* **Irány**: hello attribútum irány tooImport, exportálás vagy ImportExport állíthatja be. ImportExport alapértelmezett beállítás.

![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Megjegyzések:

* Ha az attribútum típusa nem észleli az összekötőt hello, hello karakteres adattípusúnak használ.
* **Táblák egymásba** egy oszlopból adatbázistáblák tekinthető. Oracle nem meghatározott sorrendben hello sorok egy beágyazott tábla tárolja. Azonban amikor hello beágyazott tábla egy PL/SQL változóba, hello sort kap 1-gyel kezdődő egymást követő alsó indexek. Amely lehetővé teszi az tömbszerű hozzáférés tooindividual sorokat.
* **VARRYS** hello összekötő használata nem támogatott.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Séma 5 (Define partíció hivatkozás attribútumok)
Ezen a lapon konfigurálnia minden hivatkozási attribútum melyik partíció (Objektumtípus) attribútum hivatkozik.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Ha **DN horgonyzási**, azonos objektumtípusra, egyet a hivatkozott hello hello kell használnia. Egy másik objektum típusa nem lehet hivatkozni.

>[!NOTE]
Már létezik a beállítás a hello 2017. márciusi frissítés indítása "*" Ez a beállítás esetén a választott, akkor minden lehetséges Tagtípusok importált lesz.

![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/any-option.png)

>[!IMPORTANT]
 Előfordulhat, hogy 2017 frissítésétől hello "*" más néven **bármilyen olyan beállítás** megváltozott toosupport folyamata exportálására és importálásra. Ha azt szeretné, toouse ezt a beállítást a többértékű tábla/nézet hello objektumtípus tartalmazó attribútum kell rendelkeznie.

![](./media/active-directory-aadconnectsync-connector-genericsql/any-02.png)

 </br> Ha a "*" van kiválasztva, majd hello objektumtípus hello oszloppal hello nevét is meg kell adni.</br> ![](./media/active-directory-aadconnectsync-connector-genericsql/any-03.png)

Az importálás után toohello valami hasonló, az alábbi képen látható:

  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Globális paraméterek
hello globális paraméterek lap esetében használt tooconfigure különbözeti importálás, dátum és idő formátumban, és a jelszó metódusát.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)



Általános SQL-összekötő hello módszerek különbözeti importálás a következő hello támogatja:

* **Eseményindító**: lásd: [eseményindítók használatával különbözeti nézetek generálása](https://technet.microsoft.com/library/cc708665.aspx).
* **Vízjel**: egy általános módszer bármely adatbázis esetében használható. hello vízjel lekérdezés előre hello forgalmazótól alapján van feltöltve. A vízjel oszlop minden tábla/nézet használt jelen kell lennie. Ebben az oszlopban kell nyomon követnie, mint a beszúrások és a frissítések toohello táblákat és a függő (többértékű vagy gyermek) táblákat. hello Synchronization Service és a hello adatbázis-kiszolgáló közötti kell szinkronizálni. Ha nem, bizonyos bejegyzések hello különbözeti importálás lehet megadva.  
  Korlátozás:
  * Vízjel stratégia támogatási törölt objektumok nem létezik.
* **Pillanatkép**: (csak a Microsoft SQL Server működik) [készített pillanatfelvételek segítségével történő különbözeti nézetek létrehozása](https://technet.microsoft.com/library/cc720640.aspx)
* **A változáskövetés**: (csak a Microsoft SQL Server működik) [kapcsolatos változások követése](https://msdn.microsoft.com/library/bb933875.aspx)  
  Korlátozások vonatkoznak:
  * Megkülönböztető név attribútum & horgonyzása hello kijelölt objektum hello tábla elsődleges kulcsaként részének kell lennie.
  * SQL-lekérdezés során az importálás és exportálás a változások követése nem támogatott.

**További paraméterek**: Adja meg a hello adatbázis kiszolgáló időzóna azt jelzi, hogy ha az adatbázis-kiszolgáló található. Ez az érték használható toosupport hello dátum és idő attribútumok különböző formátumokba.

hello összekötő dátuma és időpontja mindig UTC formátumban tárolja. toobe képes toocorrectly átalakítása hello dátumát és időpontját, hello időzóna hello adatbázis-kiszolgáló és a hello formátumának meg kell adni. hello formátum .net formátumban kell megadni.

Exportálás során minden dátum-idő attribútumot meg kell adni a toohello összekötő UTC idő formátumban.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Jelszó konfigurálása**: hello összekötő jelszó-szinkronizálás képességeket biztosít, és támogatja és jelszó módosítása.

hello összekötő toosupport jelszó-szinkronizálás két módszert kínál:

* **Tárolt eljárás**: ennél a módszernél két tárolt eljárások toosupport Set & Jelszó módosítása. Írja be az összes paraméterének hozzáadása, és módosítsa a jelszó-művelet hello **beállítva jelszó SP** és **Módosítsa jelszavát SP** alábbi példában rendre, egy paramétereket.
  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
* **Jelszó bővítmény**: ennél a módszernél jelszó bővítmény-dll-(bővítmény dll-fájl neve, amely a kiterjesztés hello tooprovide hello kell [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) felület). Jelszó bővítményszerelvény bővítmény mappába kell helyezni, így hello összekötő hello DLL futásidőben tölti be.
  ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Akkor is tooenable hello jelszókezelés a hello **konfigurálása bővítmény** lap.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása
Az oldalon hello partíciók és hierarchiák minden objektumtípusokat jelöl ki. Minden objektum saját partíció típus.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Felülbírálhatja hello definiált hello értékek **kapcsolat** vagy **globális paraméterek** lap.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Horgonyok konfigurálása
Ezen a lapon csak olvasható óta hello horgonyzási már meg van adva. hello kijelölt horgonyattribútum mindig hozzáfűzi a hello objektum típusa tooensure marad egyedi objektumtípusok között.

![a központi jellegűek](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Futtatási lépés paraméter konfigurálása
Ezeket a lépéseket a futtatási profil szerepel a hello összekötő hello vannak konfigurálva. Ezek a konfigurációk hello adatok importálása és exportálása a tényleges munkát.

### <a name="full-and-delta-import"></a>Teljes és különbözeti importálás
Általános SQL Connector támogatása teljes és különbözeti importálás ezekkel a módszerekkel:

* Tábla
* Nézet
* Tárolt eljárás
* SQL-lekérdezés

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tábla/nézet**  
tooimport többértékű attribútumok objektum esetén vannak tooprovide hello vesszővel tagolt tábla vagy nézet nevét **nevét a többértékű tábla/nézetek** és a megfelelő illesztési feltételek hello **feltételCsatlakozás**hello szülő táblával.

Példa: Szeretné tooimport hello alkalmazott objektum összes többértékű attribútum. Nincsenek alkalmazott (fő tábla) és a részleg (többértékű) nevű táblát.
A következő hello:

* Típus **alkalmazott** a **tábla/nézet/SP**.
* Típus részlege **nevét a többértékű tábla/nézetek**.
* Írja be a hello illesztési feltétel alkalmazott & részlege között **illesztési feltétel**, például `Employee.DEPTID=Department.DepartmentID`.
  ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Tárolt eljárások**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

* Ha sok adatot, a tárolt eljárások tooimplement tördelési ajánlott.
* A tárolt eljárás toosupport tördelési kell tooprovide Start Index és a záró Index. Lásd: [keresztül nagy mennyiségű adat hatékonyan lapozás](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndexés @EndIndex cserélése a végrehajtás során konfigurált megfelelő lapra méretértéket **konfigurálása lépés** lap. Például ha hello összekötő lekéri az első és az ajánlott méretet hello értéke 500, ilyen esetben @StartIndex 1 lenne és @EndIndex 500. Ezek az értékek növekszik, ha az összekötő a következő lapjain kéri le, és módosítsa a hello @StartIndex & @EndIndex érték.
* tooexecute paraméteres tárolt eljárás, hello paraméterek megadásához a `[Name]:[Direction]:[Value]` formátumban. Adja meg mindegyik paraméter (használata Ctrl + Enter tooget új sor) külön sorban.
* Általános SQL-összekötő csatolt kiszolgálók az importálási művelet is támogatja a Microsoft SQL Server kiszolgálón. Ha információt be kell olvasni a csatolt kiszolgáló egy táblából, majd tábla kell megadni hello formátumban:`[ServerName].[Database].[Schema].[TableName]`
* Általános SQL-összekötő csak azokat az objektumokat, amelyek hasonló szerkezete (az alias nevét és az adatokat egyaránt típus) között információkat és a séma észlelési lépések futtatásával támogatja. Hello választásakor objektumhoz a séma és futtatási lépés: a megadott adatok különböző, az SQL-összekötő nem tudja toosupport az ilyen típusú forgatókönyvek.

**SQL-lekérdezés**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

* Több eredményhalmazt lekérdezések nem támogatott.
* SQL-lekérdezés által támogatott hello tördelési, és adja meg a kezdő Index és a záró Index, a változó toosupport tördelési.

### <a name="delta-import"></a>Különbözeti importálás
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Különbözeti importálás konfigurálása azonban a teljes importálás képest további konfigurálást igényel.

* Ha hello eseményindító vagy pillanatképét megközelítés tootrack a változási különbözeteket, majd adja meg a Előzménytábla vagy pillanatképét adatbázis **Előzménytábla vagy pillanatképét adatbázisnév** mezőben.
* Szükség tooprovide illesztési feltétel közötti előzménytábla és a szülőtábla, például`Employee.ID=History.EmployeeID`
* hello tranzakció tootrack hello a szülőtábla hello előzmények táblából, meg kell adnia hello művelet információt (Add/Update/Delete) tartalmazó hello oszlop neve.
* Ha vízjel tootrack változáskülönbözeteit, majd adja meg hello oszlopnév hello művelet adatokat tartalmazó **vízjel megjelölés oszlopnév**.
* Hello **attribútum módosítása** hello változástípushoz kötelező. Ez az oszlop, amely akkor fordul elő, hello elsődleges tábla képezi le, vagy többértékű tábla tooa hello különbözeti nézet típusának módosítása. Ez az oszlop tartalmazhat hello Modify_Attribute Változástípus attribútum szintű módosítása vagy egy hozzáadása, módosítása, vagy Delete objektumszintű változástípushoz típusának módosítása. Ha valami nem hello alapértelmezett érték hozzáadása, módosítása, vagy törölje, majd ezeket az értékeket ezzel a beállítással adhatja meg.

### <a name="export"></a>Exportálás
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Általános SQL Connector támogatása exportálási négy támogatott módszerek használatával, mint:

* Tábla
* Nézet
* Tárolt eljárás
* SQL-lekérdezés

**Tábla/nézet**  
Ha úgy dönt, hogy a tábla/nézet beállítás hello, hello összekötő hello megfelelő lekérdezések toodo hello exportálási állít elő.

**Tárolt eljárások**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Ha hello tárolt eljárás lehetőséget választja, az exportáláshoz három különböző tárolt eljárások tooperform Insert/Update/Delete művelet.

* **Adja hozzá SP nevét**: az SP fut, ha minden objektum rendelkezik tooconnector hello megfelelő tábla beszúrásához.
* **Frissítés SP neve**: az SP fut, ha bármely objektum származik tooconnector frissítésének hello megfelelő táblában.
* **SP neve**: az SP fut, ha bármely objektum tooconnector hello megfelelő tábla törlésre.
* Ki a következő paraméter értéke toohello tárolt eljárásként használt hello séma attribútum. Például `EmployeeName: INPUT: @EmployeeName` (EmployeeName hello összekötő séma választotta, és hello összekötő hello megfelelő értéket lecseréli exportálás közben)
* toorun paraméteres tárolt eljárás, adja meg a paraméterek `[Name]:[Direction]:[Value]` formátumban. Adja meg mindegyik paraméter (használata Ctrl + Enter tooget új sor) külön sorban.

**SQL-lekérdezés**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Ha úgy dönt, hogy hello SQL lekérdezési lehetőség, exportáláshoz három különböző tooperform Insert/Update/Delete művelet lekérdezi.

* **Helyezze be a lekérdezés**: Ez a lekérdezés fut, ha minden objektum rendelkezik tooconnector hello megfelelő tábla beszúrásához.
* **Lekérdezés frissítése**: Ez a lekérdezés fut, ha bármely objektum származik tooconnector frissítésének hello megfelelő táblában.
* **Törölje a lekérdezés**: Ez a lekérdezés fut, ha minden objektum rendelkezik tooconnector hello megfelelő tábla törlésre.
* Hello séma használt lekérdezésként paraméter értéke toohello, például a kijelölt attribútum`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Hibaelhárítás
* Hogyan tooenable naplózási tootroubleshoot hello összekötő információkért lásd: hello [hogyan ETW-nyomkövetés összekötők tooEnable](http://go.microsoft.com/fwlink/?LinkId=335731).
