---
title: "aaaIn memórián belüli online Tranzakciófeldolgozási javítja az SQL túlhasználat telj |} Microsoft Docs"
description: "Fogadja el a memórián belüli online Tranzakciófeldolgozási tooimprove tranzakciós teljesítmény meglévő SQL-adatbázisban."
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: MightyPen
ms.assetid: c2bf14a0-905b-47b4-afb6-efe9a61147d5
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: jodebrui
ms.openlocfilehash: 1bed796069565eb6741953837cf0477c2ddd8519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-in-memory-oltp-tooimprove-your-application-performance-in-sql-database"></a>Használja a memórián belüli online Tranzakciófeldolgozási tooimprove az alkalmazások teljesítményéről, az SQL-adatbázis
[A memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md) lehet a tranzakció-feldolgozást, adatfeldolgozást és átmeneti adatáttelepítések esetében használt tooimprove hello teljesítményének [prémium](sql-database-service-tiers.md) Azure SQL-adatbázisok nélkül növelése hello árképzési szint. 

> [!NOTE] 
> Megtudhatja, hogyan [kvórum kulcsának adatbázis munkaterhelés megduplázódik, miközben csökkenti a DTU az SQL Database 70 %-kal](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)


Kövesse a lépéseket tooadopt memórián belüli online Tranzakciófeldolgozási a meglévő adatbázis.

## <a name="step-1-ensure-you-are-using-a-premium-database"></a>1. lépés: Győződjön meg arról, a prémium szintű adatbázist használ
A memórián belüli online Tranzakciófeldolgozási csak Premium adatbázisokat támogatja. A memóriában támogatott, ha hello visszaadott eredmény 1 (0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* rövidítése *szélsőséges tranzakció feldolgozása*



## <a name="step-2-identify-objects-toomigrate-tooin-memory-oltp"></a>2. lépés: Azonosítsa objektumok toomigrate tooIn memórián belüli online Tranzakciófeldolgozási
SSMS tartalmaz egy **tranzakció teljesítményét elemző áttekintése** jelentést, amely egy aktív munkaterhelés adatbázis is futtathatók. hello a jelentés azonosítja a táblák és tárolt eljárások a deduplikációra kijelölt áttelepítési tooIn memórián belüli online Tranzakciófeldolgozási.

Az SSMS, toogenerate hello jelentés:

* A hello **Object Explorer**, kattintson a jobb gombbal az adatbázis-csomópont.
* Kattintson a **jelentések** > **szabványos jelentések** > **tranzakció teljesítményét elemző áttekintése**.

További információkért lásd: [meghatározása, ha tábla vagy tárolt eljárás kell legelterjedtebb tooIn memórián belüli online Tranzakciófeldolgozási](http://msdn.microsoft.com/library/dn205133.aspx).

## <a name="step-3-create-a-comparable-test-database"></a>3. lépés: Összehasonlítható teszt-adatbázis létrehozása
Tegyük fel, hogy hello jelentés azt jelzi, az adatbázis egy táblát, amely éppen előnyös konvertálta tooa memóriaoptimalizált tábla. Azt javasoljuk, hogy először tesztelje a tooconfirm hello arra utal, hogy végzett teszteléséhez.

Az éles adatbázis test másolatának van szüksége. hello tesztadatbázis kell lennie: hello azonos szolgáltatási réteg szintjét az éles adatbázis.

tooease tesztelése, tudjon végezni az adatbázis tesztelése az alábbiak szerint:

1. Kapcsolódás toohello tesztadatbázis az SSMS használatával.
2. hello lekérdezésekben WITH (SNAPSHOT) lehetőséget, és állítsa hello adatbázis-beállítás, ahogy az következő T-SQL-utasítást hello kellene tooavoid:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>4. lépés: A tábla áttelepítése
Létre kell hoznia és memóriaoptimalizált másolatot feltöltése hello tábla tootest szeretné. Létrehozhat használatával:

* hello szolgáltatáshoz az ssms hasznos memória optimalizálási varázsló.
* Kézi T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>Memória optimalizálási varázsló szolgáltatáshoz az ssms
toouse az áttelepítési lehetőség:

1. Csatlakozás toohello tesztadatbázis ssms alkalmazásával.
2. A hello **Object Explorer**, hello táblán kattintson a jobb gombbal, és kattintson a **memória optimalizálási Advisor**.
   
   * Hello **tábla memória optimalizáló Advisor** varázsló jelenik meg.
3. Hello varázslóban kattintson **áttelepítési érvényesítési** (vagy hello **következő** gomb) toosee Ha hello tábla tartozik-e a memóriaoptimalizált táblákban nem támogatott funkciók nem támogatott. További információkért lásd:
   
   * Hello *memória optimalizálási ellenőrzőlista* a [memória optimalizálási Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
   * [Nem támogatja a memórián belüli online Tranzakciófeldolgozási Transact-SQL szerkezetek](http://msdn.microsoft.com/library/dn246937.aspx).
   * [Áttelepítése tooIn memórián belüli online Tranzakciófeldolgozási](http://msdn.microsoft.com/library/dn247639.aspx).
4. Ha hello táblának nincs nem támogatott funkciókat, hello advisor végezhet hello tényleges séma- és adatáttelepítés meg.

#### <a name="manual-t-sql"></a>Kézi T-SQL
toouse az áttelepítési lehetőség:

1. Csatlakozás tooyour tesztadatbázis SSMS (vagy egy hasonló segédprogramot) használatával.
2. Szerezze be a hello teljes T-SQL-parancsfájlt a táblázat és az indexeket.
   
   * Az SSMS kattintson jobb gombbal a tábla csomópontra.
   * Kattintson a **a táblázatban parancsfájl** > **hozhat létre** > **új lekérdezési ablakba**.
3. A hello parancsfájl ablakban, adja hozzá a WITH (MEMORY_OPTIMIZED = ON) toohello CREATE TABLE utasítás.
4. Ha egy FÜRTÖZÖTT index, módosíthatja tooNONCLUSTERED.
5. Nevezze át a meglévő tábla hello SP_RENAME használatával.
6. A szerkesztett CREATE TABLE parancsfájl futtatásával hozzon létre hello új memóriaoptimalizált hello tábla példányát.
7. Másolja a hello adatok tooyour memóriaoptimalizált tábla INSERT használatával... VÁLASSZA KI * BE:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>(Választható) 5. lépés: telepítse át a tárolt eljárások
hello memórián belüli szolgáltatás módosíthatja is javítja a teljesítményt a tárolt eljárást.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>A natív módon lefordított tárolt eljárásokkal kapcsolatos szempontok
Egy natív módon lefordított tárolt eljárás a következő beállításokat a T-SQL WITH záradék hello kell rendelkeznie:

* WITH NATIVE_COMPILATION BEÁLLÍTÁST
* Sémához kötési: hello tárolt eljárás jelentése táblákhoz az oszlopdefiníciók megváltozott olyan módon, ami hatással lenne a hello tárolt eljárás csak akkor dobható el, hogy hello tárolt eljárás nem lehet.

Egy natív modult kell használni a nagy [ATOMI blokkokban](http://msdn.microsoft.com/library/dn452281.aspx) tranzakció-kezelésre. Az explicit BEGIN TRANSACTION, vagy a ROLLBACK TRANSACTION Role szerepkör nincs. A kód egy üzleti szabály megsértését észleli, ha azt is leáll hello atomi blokkot, amelynek a [THROW](http://msdn.microsoft.com/library/ee677615.aspx) utasítást.

### <a name="typical-create-procedure-for-natively-compiled"></a>A tipikus CREATE PROCEDURE natív módon lefordított.
T-SQL hello toocreate natív módon lefordított tárolt eljárás általában a következő sablon hasonló toohello:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* Hello TRANSACTION_ISOLATION_LEVEL PILLANATKÉP esetén hello leggyakoribb értéke hello natív módon lefordított tárolt eljárást. Azonban egy részhalmazát hello egyéb értékek is támogatottak:
  
  * ISMÉTELHETŐ OLVASÁS
  * A SZERIALIZÁLHATÓ
* hello NYELVI érték hello sys.languages nézetben jelen kell lennie.

### <a name="how-toomigrate-a-stored-procedure"></a>Hogyan toomigrate tárolt eljárás
hello áttelepítés lépései a következők:

1. Szerezze be a hello CREATE PROCEDURE parancsfájl toohello rendszeres értelmezett tárolt eljárást.
2. A fejléc toomatch hello előző sablon újraírási.
3. Annak megállapítása, hogy használja-e ki a szolgáltatásokat, amelyeket nem támogat natív módon lefordított tárolt eljárások hello tárolt eljárás T-SQL-kódot. Lehetséges megoldások megvalósításához, ha szükséges.
   
   * További információ: [áttelepítésekor fellépő hibák tárolt eljárások natív módon lefordított](http://msdn.microsoft.com/library/dn296678.aspx).
4. Nevezze át a régi tárolt eljárás hello SP_RENAME használatával. Vagy egyszerűen ELDOBNI.
5. A szerkesztett létrehozása eljárás T-SQL-parancsfájl futtatása.

## <a name="step-6-run-your-workload-in-test"></a>6. lépés: A számítási feladatok futtatása teszt
Futtassa a munkaterhelés a hasonló toohello munkaterhelés az éles adatbázis futó adatbázis tesztelése. Ez kódproblémájáról hello jobb teljesítménye a hello memórián belüli funkciójának használatát a táblák és tárolt eljárások megvalósítani.

Hello munkaterhelés fő attribútumok pedig a következők:

* Egyidejű kapcsolatok száma.
* Olvasási/írási arányt.

tootailor és futtatási hello teszt munkaterhelés, érdemes lehet hello hasznos ostress.exe eszközt, amely a [Itt](sql-database-in-memory.md).

toominimize hálózati késés, a teszt futtatása a hello azonos Azure földrajzi régióban hello adatbázis esetén.

## <a name="step-7-post-implementation-monitoring"></a>7. lépés: A megvalósítás utáni figyelése
Vegye figyelembe, hogy éles környezetben a memórián belüli megvalósítások hatásait hello teljesítmény figyelése:

* [Figyelje a memórián belüli tároló](sql-database-in-memory-oltp-monitoring.md).
* [Az Azure SQL Database felügyelete dinamikus felügyeleti nézetek használatával](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Memórián belüli online Tranzakciófeldolgozási (a memóriában optimalizálása)](http://msdn.microsoft.com/library/dn133186.aspx)
* [Bevezetés tooNatively tárolt eljárások fordítása](http://msdn.microsoft.com/library/dn133184.aspx)
* [Memória optimalizálási Advisor](http://msdn.microsoft.com/library/dn284308.aspx)

