---
title: "aaaGetting (előzetes verzió) Azure SQL adatszinkronizálás használatába |} Microsoft Docs"
description: "Ez az oktatóanyag segítséget nyújt a Ismerkedés az Azure SQL adatszinkronizálás (előzetes verzió)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.openlocfilehash: 666d09237e42acc23ae3c8c81e60734a413f5949
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a>Ismerkedés az Azure SQL adatszinkronizálás (előzetes verzió)
Ebben az oktatóanyagban elsajátíthatja, hogyan tooset be Azure SQL adatszinkronizálás hozzon létre egy hibrid szinkronizálású csoport, amely tartalmazza az Azure SQL Database és az SQL Server-példány. hello új szinkronizálású csoport teljes van beállítva, és a beállított ütemezés hello szinkronizálja.

Ez az oktatóanyag feltételezi, hogy rendelkezik-e legalább némi tapasztalattal az SQL Database és SQL-kiszolgálóval. 

SQL adatszinkronizálás áttekintését lásd: [szinkronizálni az adatokat](sql-database-sync-data.md).

A teljes PowerShell példák hogyan tooconfigure SQL adatszinkronizálás, tekintse meg a következő cikkek hello:
-   [PowerShell toosync több Azure SQL-adatbázist használja](scripts/sql-database-sync-data-between-sql-databases.md)
-   [Egy Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti PowerShell toosync használata](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> hello teljes műszaki dokumentáció az Azure SQL Data szinkronizálás, korábban, az MSDN-en, állítsa be áll rendelkezésre, mert egy. PDF-dokumentumokat. Töltse le [Itt](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).

## <a name="step-1---create-sync-group"></a>1. lépés – a sync-csoport létrehozása

### <a name="locate-hello-data-sync-settings"></a>Keresse meg a beállítások hello adatok szinkronizálása

1.  A böngészőben nyissa meg a toohello Azure-portálon.

2.  Hello portal keresse meg az SQL-adatbázisok az irányítópultra, illetve hello SQL-adatbázisok ikon hello eszköztáron.

    ![Azure SQL-adatbázisok listája](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  A hello **SQL-adatbázisok** panelen, jelölje be hello meglévő SQL-adatbázis, amelyet toouse, hello hub adatbázis az adatok szinkronizálása. hello SQL adatbázis panel nyílik meg.

4.  Hello SQL adatbázis paneljén hello kijelölt adatbázis kiválasztása **tooother adatbázisok szinkronizálása**. hello adatszinkronizálás panel nyílik meg.

    ![Szinkronizálási tooother adatbázisok lehetősége](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Hozzon létre egy új szinkronizálási csoportot

1.  Hello adatszinkronizálás paneljén válassza **új szinkronizálású csoport**. Hello **új szinkronizálású csoport** panel nyílik meg az 1. lépésben, **szinkronizálási csoport létrehozása**, kijelölt. Hello **adatok szinkronizálása csoport létrehozása** panel is megnyílik.

2.  A hello **adatok szinkronizálása csoport létrehozása** panelen dolgok következő hello:

    1.  A hello **szinkronizálási csoportnév** mezőbe írja be a hello új szinkronizálási csoport nevét.

    2.  A hello **szinkronizálási metaadatokat tároló adatbázis** területen válassza ki, hogy toocreate egy új adatbázist (ajánlott) vagy toouse egy meglévő adatbázist.

        > [!NOTE]
        > A Microsoft azt javasolja, hogy hozzon létre egy új, üres adatbázis toouse, hello szinkronizálási metaadatokat tároló adatbázis. Adatszinkronizálás hoz létre táblák az adatbázisban, és a gyakori munkaterhelés futtatja. Ez az adatbázis automatikusan megosztva hello szinkronizálási metaadatokat tároló adatbázis az összes, a szinkronizálási csoportok hello a kiválasztott régióban. Hello szinkronizálási metaadatokat tároló adatbázis, a neve vagy a szolgáltatási szint nélkül eldobása nem módosítható.

        Ha úgy döntött, hogy **új adatbázis**, jelölje be **új adatbázis létrehozása.** Hello **SQL-adatbázis** panel nyílik meg. A hello **SQL-adatbázis** panelen nevet, és konfigurálja a hello új adatbázist. Válassza ki **OK**.

        Ha úgy döntött, hogy **létező adatbázis használata**, jelölje be hello adatbázis hello listából.

    3.  A hello **automatikus szinkronizálás** szakaszban, először válassza **a** vagy **ki**.

        Ha úgy döntött, hogy **a**, a hello **szinkronizálási gyakoriság** szakaszt, adjon meg egy számot, és válassza ki a másodperc, perc, órák vagy napok.

        ![Adja meg a szinkronizálás gyakorisága](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  A hello **ütközésének feloldása** területen válassza ki a "Hub wins" vagy "Tag wins."

        ![Adja meg az ütközések feloldása](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Válassza ki **OK** és várja meg, hello új szinkronizálási csoport toobe létrehozása és telepítése.

## <a name="step-2---add-sync-members"></a>2. lépés – szinkronizálás tagok hozzáadása

Miután hello új szinkronizálású csoport létrehozása és telepítése, 2. lépés **szinkronizálási tagok hozzáadása**, hello kijelölt **új szinkronizálású csoport** panelen.

A hello **Hub adatbázis** területen adja meg a meglévő hitelesítő hello hello SQL adatbázis-kiszolgáló a mely hello hub adatbázisa található. Ne adjon meg *új* ebben a szakaszban a hitelesítő adatokat.

![Központi adatbázis toosync csoport hozzá lett adva](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a>Egy Azure SQL adatbázis hozzáadása

A hello **Tagadatbázis** szakaszban, és opcionálisan adja hozzá az Azure SQL Database toohello szinkronizálású csoport kiválasztásával **hozzáadása egy Azure-adatbázis**. Hello **Azure-adatbázis beállítása** panel nyílik meg.

A hello **Azure-adatbázis beállítása** panelen dolgok következő hello:

1.  A hello **szinkronizálási tagneve** mezőbe, adjon meg egy nevet hello új szinkronizálási tag. Ez a név nem azonos azzal hello maga hello adatbázis nevét.

2.  A hello **előfizetés** mezőjét, jelölje be hello társított Azure-előfizetés más célra.

3.  A hello **Azure SQL Server** mezőjét, jelölje be hello meglévő SQL adatbázis-kiszolgáló.

4.  A hello **Azure SQL Database** mezőjét, jelölje be hello meglévő SQL-adatbázis.

5.  A hello **szinkronizálási irányban** mező mellett válassza ki a kétirányú szinkronizálás, toohello Hub, vagy a központ hello.

    ![Új SQL-adatbázis szinkronizálási tag hozzáadása](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  A hello **felhasználónév** és **jelszó** mezők, adja meg a hello meglévő hitelesítő adatait, mely hello tagadatbázis helyezkedik hello SQL adatbázis-kiszolgáló. Ne adjon meg *új* ebben a szakaszban a hitelesítő adatokat.

7.  Válassza ki **OK** és várja meg, hello új szinkronizálási tag toobe létrehozása és telepítése.

    ![Új SQL-adatbázis szinkronizálási tag hozzá lett adva.](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a>A helyszíni SQL Server adatbázis hozzáadása

A hello **Tagadatbázis** szakaszban, és opcionálisan adja hozzá egy helyi SQL Server toohello szinkronizálású csoport kiválasztásával **adja hozzá az On-Premises adatbázis**. Hello **konfigurálása a helyszíni** panel nyílik meg.

A hello **konfigurálása a helyszíni** panelen dolgok következő hello:

1.  Válassza ki **válasszon hello szinkronizálási ügynök átjáró**. Hello **válassza ki a szinkronizálási ügynök** panel nyílik meg.

    ![Hello szinkronizálási ügynök átjáró kiválasztása](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  A hello **válasszon hello szinkronizálási ügynök átjáró** panelen válassza ki e toouse egy meglévő ügynököt, vagy hozzon létre egy új ügynököt.

    Ha úgy döntött, hogy **meglévő ügynökök**, válassza ki a meglévő ügynököt hello listából hello.

    Ha úgy döntött, hogy **hozzon létre egy új ügynök**, dolgok következő hello:

    1.  A megadott hivatkozás hello ügynök szinkronizálási hello ügyfélszoftver letöltése, és telepítse azt hello számítógép hello SQL Server.
 
        > [!IMPORTANT]
        > Hogy tooopen kimenő TCP 1433-as port a hello tűzfal toolet hello ügyfélügynök hello kiszolgálóval való kommunikációhoz.


    2.  Adjon meg egy nevet hello ügynök.

    3.  Válassza ki **létrehozása és a kulcs létrehozása**.

    4.  Hello ügynök kulcs toohello vágólapra másolása.
        
        ![Új szinkronizálási ügynök létrehozása](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Válassza ki **OK** tooclose hello **válassza ki a szinkronizálási ügynök** panelen.

    6.  Hello SQL Server-számítógépen keresse meg és hello szinkronizálási Ügyfélügynök-alkalmazás futtatása.

        ![hello adatok szinkronizálása az ügyfélalkalmazás ügynök](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  Hello szinkronizálási ügynök alkalmazásban, válassza ki a **nyújt ügynök kulcs**. Hello **szinkronizálási metaadatok adatbázis konfigurációja** párbeszédpanel.

    8.  A hello **szinkronizálási metaadatok adatbázis konfigurációja** párbeszédpanelen illessze be hello ügynök kulcs átmásolva hello Azure-portálon. Emellett azokat a hello hello Azure SQL Database kiszolgálón, mely hello a metaadatokat tároló adatbázis található meglévő hitelesítő adatokat. (Ha létrehozott egy új metaadatokat tároló adatbázis, az adatbázis nem a hello hello hub adatbázis ugyanazon a kiszolgálón.) Válassza ki **OK** és hello konfigurációs toofinish várja.

        ![Hello ügynök kulcsot és a kiszolgáló hitelesítő adatok megadása](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Ha a tűzfal hibaüzenet jelenik meg ezen a ponton, hogy toocreate tűzfalszabály Azure tooallow érkező forgalmat hello SQL Server-számítógépen. Hello szabály manuálisan is létrehozhat, hello portálon, de azt tapasztalhatja, könnyebben toocreate azt az SQL Server Management Studio (SSMS). A szolgáltatáshoz az SSMS próbálja ki a tooconnect toohello hub adatbázis Azure-on. Adja meg a nevét, \<hub_database_name\>. database.windows.net. Kövesse hello hello párbeszédpanel bezárásához tooconfigure hello Azure tűzfalszabály. Térjen vissza toohello szinkronizálási Ügyfélügynök alkalmazást.

    9.  Hello szinkronizálási Ügyfélügynök alkalmazásban kattintson **regisztrálása** tooregister hello ügynököt az SQL Server-adatbázis. Hello **SQL Server-konfigurációs** párbeszédpanel.

        ![Hozzáadása és az SQL Server-adatbázisok konfigurálása](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. A hello **SQL Server-konfigurációs** párbeszédpanelen válassza ki azt hogy tooconnect SQL Server-hitelesítést vagy a Windows-hitelesítés használatával. Ha úgy dönt, hogy az SQL Server-hitelesítést, írja be a hello meglévő hitelesítő adatok. Adja meg hello SQL Server nevét és hello hello adatbázis nevét, amelyet az toosync. Válassza ki **tesztkapcsolat** tootest a beállításokat. Válassza ki **mentése**. hello regisztrált adatbázis hello listájában jelenik meg.

        ![SQL Server-adatbázis most már regisztrálva van](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. Most már bezárhatja hello szinkronizálási Ügyfélügynök alkalmazást.

    12. A hello hello portálról **konfigurálása a helyszíni** panelen válassza **válasszon hello adatbázis.** Hello **adatbázis kijelölése** panel nyílik meg.

    13. A hello **adatbázis kijelölése** paneljén, hello **szinkronizálási tagneve** mezőbe, adjon meg egy nevet hello új szinkronizálási tag. Ez a név nem azonos azzal hello maga hello adatbázis nevét. Válassza ki a hello adatbázis hello listából. A hello **szinkronizálási irányban** mező mellett válassza ki a kétirányú szinkronizálás, toohello Hub, vagy a központ hello.

        ![Válassza ki a helyi adatbázis hello](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Válassza ki **OK** tooclose hello **adatbázis kijelölése** panelen. Válassza ki **OK** tooclose hello **konfigurálása a helyszíni** panel megnyitásához, és a hello várakozás új tag toobe létrehozott és telepített szinkronizálása. Végezetül kattintson **OK** tooclose hello **szinkronizálási tagok kiválasztása** panelen.

        ![A helyi adatbázis hozzáadta a toosync csoportot](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  Adatszinkronizálás tooconnect tooSQL és hello helyi ügynök, vegye fel a szerepkörben toohello `DataSync_Executor`. Adatszinkronizálás hello SQL Server-példányon hoz létre ehhez a szerepkörhöz.

## <a name="step-3---configure-sync-group"></a>3. lépés - a szinkronizálási csoport konfigurálása

Miután új szinkronizálási csoporttagok hello létrehozott és telepített, 3. lépés, **konfigurálása szinkronizálású csoport**, hello kijelölt **új szinkronizálású csoport** panelen.

1.  A hello **táblák** panelen válassza egy adatbázis szinkronizálási hello listája csoport tagjai, és válassza **frissítési séma**.

2.  Rendelkezésre álló táblák hello listában jelölje ki, hogy szeretné-e toosync hello táblákat.

    ![Válassza ki a táblák toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Alapértelmezés szerint hello táblázat összes oszlopa van kiválasztva. Ha toosync összes hello oszlopok nem szeretné, tiltsa le a hello oszlopok, hogy nem szeretné, hogy toosync hello jelölőnégyzetét. Győződjön meg arról, hogy a kiválasztott tooleave hello elsődleges kulcsként megadott oszlop.

    ![Válassza ki a mezőket toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Végül válassza ki **mentése**.

## <a name="next-steps"></a>Következő lépések
Gratulálunk! Létrehozott egy szinkronizálású csoport, amely tartalmazza az SQL Database-példányt, és egy SQL Server-adatbázis.

SQL Database és SQL adatszinkronizálás kapcsolatos további információkért lásd:

-   [Hello teljes adatszinkronizálás SQL műszaki dokumentáció letöltése](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [Hello SQL adatok szinkronizálása REST API-dokumentáció letöltése](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [SQL-adatbázis – áttekintés](sql-database-technical-overview.md)
-   [Adatbázis életciklusának kezelésére](https://msdn.microsoft.com/library/jj907294.aspx)
