---
title: az SQL Data Warehouse aaaTransactions |} Microsoft Docs
description: "Ötletek a tranzakciók az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 7c541648553238443b407666612561918096eb61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-in-sql-data-warehouse"></a>Az SQL Data Warehouse-tranzakciók
Módon teheti meg, az SQL Data Warehouse hello adatraktár-számítási feladat részeként támogatja a tranzakciókat. Azonban tooensure hello teljesítmény az SQL Data Warehouse megmarad, egyes funkciók korlátozva, ha léptékű összehasonlított tooSQL kiszolgáló. Ez a cikk kiemeli a hello különbségeket, és listák hello mások. 

## <a name="transaction-isolation-levels"></a>Tranzakció elkülönítési szinten
Az SQL Data Warehouse megvalósítja az ACID-tranzakciókat. Azonban hello hello tranzakciós támogatás elkülönítési korlátozódik túl`READ UNCOMMITTED` és ez nem módosítható. Megvalósíthat számos programozási módszer inkonzisztencia tooprevent adatok olvassa be, ha ez problémát jelent meg. hello legnépszerűbb módszerek kihasználja CTAS és a tábla partíciós váltás (gyakran más néven mozgó ablak mintát hello) tooprevent felhasználók továbbra is előkészítés alatt álló adatok lekérdezésére. Győződjön meg arról, hogy előtti szűrő hello adatokat is a népszerű megközelítés nézetek.  

## <a name="transaction-size"></a>Tranzakció mérete
Egyetlen módosítása tranzakció mérete korlátozott. hello korlát ma alkalmazza ") eloszlása feladatonként (". Ezért hello teljes foglalási kiszámítható hello korlát megszorozzuk hello terjesztési száma. tooapproximate hello sorok maximális számát hello tranzakcióban hello terjesztési cap nullával való minden egyes sorára hello teljes mérete. A változó hosszúságú oszloppal vegye figyelembe véve az átlagos oszlop hossza, nem pedig hello maximális méret használatával.

Hello tábla hello alább következő feltételezéseket végzett:

* Az adatok még akkor is, terjesztési történt. 
* hello átlagos sor hossza 250 bájt

| [DWU][DWU] | Cap eloszlása (GB) | Azokat a Terjesztéseket száma | Tranzakció maximális (GB) | # Sorok eloszlása | Tranzakciónként sorok maximális száma |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1.5 |60 |90 |6,000,000 |360,000,000 |
| DW300 |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400 |3 |60 |180 |12,000,000 |720,000,000 |
| DW500 |3.75 |60 |225 |15,000,000 |900,000,000 |
| DW600 |4.5 |60 |270 |18,000,000 |1,080,000,000 |
| DW1000 |7.5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1200 |9 |60 |540 |36,000,000 |2,160,000,000 |
| DW1500 |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000 |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW3000 |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW6000 |45 |60 |2,700 |180,000,000 |10,800,000,000 |

hello tranzakció maximális mérete / tranzakció vagy műveletet alkalmazza. Minden egyidejű tranzakciókat keresztül nem lesz alkalmazva. Ezért az egyes tranzakciókra, engedélyezett toowrite ezt a adatok toohello napló mennyiséget. 

toooptimize minimalizálása érdekében hello toohello napló írt adatok mennyiségét, és tekintse meg a toohello [tranzakciók gyakorlati tanácsok] [ Transactions best practices] cikk.

> [!WARNING]
> tranzakció mérete csak érhető el, a KIVONATOLÓ vagy az elosztott ROUND_ROBIN táblák, ahol hello terjednek a hello adatok maximális hello is van. Ha hello tranzakció az adatok írása kihasználtságot módon toohello terjesztéseket majd hello határértéke valószínűleg toobe elérte előzetes toohello tranzakció maximális méretet.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>Tranzakció állapota
Az SQL Data Warehouse használ hello XACT_STATE() függvény tooreport hello értékével -2 sikertelen tranzakció. Ez azt jelenti, hogy hello tranzakció sikertelen volt, és csak visszaállítás meg van jelölve

> [!NOTE]
> hello-2 hello XACT_STATE függvény toodenote egy sikertelen tranzakció jelöli másképp viselkednek tooSQL kiszolgáló használatához. SQL Server hello-1 érték toorepresent vissza nem vonható véglegesítésű tranzakcióból használja. SQL Server egy tranzakción belül hibák tűri megjelölve, vissza nem vonható véglegesítésű toobe rendelkező nélkül. Például `SELECT 1/0` volna hibát jelez, de nem kényszerítik ki a tranzakció vissza nem vonható véglegesítésű állapotba kerülnek. SQL Server is lehetővé teszi az olvasási műveletek hello vissza nem vonható véglegesítésű tranzakcióban. Azonban az SQL Data Warehouse nem teszik ezt. Hiba esetén egy SQL Data Warehouse tranzakción belül automatikusan módba lép, -2 hello állapotát, és nem fogja tudni toomake esetleges további kiválasztása utasítások amíg hello utasítás vissza lett állítva. Éppen ezért fontos, hogy az alkalmazás kódja toosee XACT_STATE() használ, akkor esetleg toomake kódmódosításra toocheck.
> 
> 

Például az SQL Serverben láthatja a tranzakciót, amelynek a következőképpen néz ki:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Ha nem adja meg a kódot, mert az újabb majd jelenik meg hibaüzenet a következő hello:

Üzenet 111233, szint 16 állapot 1, 1 sor 111233; hello aktuális tranzakció megszakadt, és minden függő módosítás rendelkezik vissza lett állítva. OK: A tranzakció csak visszaállítási állapotban nem kifejezetten vissza lett állítva egy DDL, DML vagy SELECT utasítás előtt.

Még nem fog hello ERROR_ * funkciók hello kimenetét.

Az SQL Data Warehouse hello kód toobe kis mértékben meg van szüksége:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

hello várt működést Mostantól követi. hello hiba hello tranzakcióban kezeli, és hello ERROR_ * funkciók elvárt adjon meg értékeket.

Megváltozott csak adott hello `ROLLBACK` a hello a tranzakció toohappen volna, mielőtt hello hello hello hiba adatainak olvasása `CATCH` blokkot.

## <a name="errorline-function"></a>Error_Line() függvény
Akkor is érdemes megjegyezni, hogy az SQL Data Warehouse nem valósítja meg, vagy hello ERROR_LINE() funkció támogatja. Ha ez a kód szüksége lesz a tooremove azt az SQL Data Warehouse szolgáltatással kompatibilis toobe. Helyette a kódban lekérdezés címkék tooimplement funkciókat. Tekintse meg a toohello [címke] [ LABEL] cikk további részleteket a szolgáltatást.

## <a name="using-throw-and-raiserror"></a>Segítségével THROW és RAISERROR
THROW hello korszerűbb implementációja az SQL Data Warehouse kivételt váltson ki, de a RAISERROR is támogatott. Néhány dologban is érdemes figyelmet toohowever fizet.

* Felhasználó által definiált hibaüzenetek számok nem lehet hello THROW 100 000-150 000 tartományon
* RAISERROR hibaüzenetek 50 000 van rögzítve.
* Ilyen hibaszámú használata nem támogatott.

## <a name="limitiations"></a>Limitiations
Az SQL Data Warehouse néhány egyéb korlátozások is tootransactions rendelkezik.

Ezek a következők:

* Elosztott tranzakciók
* Nem engedélyezett beágyazott tranzakciók
* Nem engedélyezett pontokon menthet
* Elnevezett tranzakciók
* Nem jelölt tranzakciók
* Nem támogatott például DDL `CREATE TABLE` belül a felhasználó definiált tranzakció

## <a name="next-steps"></a>Következő lépések
toolearn optimalizálása tranzakciók, bővebben lásd: [tranzakciók gyakorlati tanácsok][Transactions best practices].  Lásd az SQL Data Warehouse gyakorlati tanácsokat, toolearn [gyakorlati tanácsok az SQL Data Warehouse][SQL Data Warehouse best practices].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
