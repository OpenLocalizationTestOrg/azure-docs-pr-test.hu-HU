---
title: "Azure SQL-adatbázis aaaCopy |} Microsoft Docs"
description: "Azure SQL-adatbázis másolatának létrehozása"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5aaf6bcd-3839-49b5-8c77-cbdf786e359b
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 64a297d819d6da89600fda60abe8394ae405abfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-an-azure-sql-database"></a>Azure SQL-adatbázis másolása

Az Azure SQL Database is tartalmaz több módszerét tranzakciós úton konzisztens másolat készítését egy meglévő Azure SQL adatbázis-vagy hello az azonos vagy egy másik kiszolgálóhoz. SQL-adatbázis hello Azure-portálon, a PowerShell vagy a T-SQL használatával másolhatja. 

## <a name="overview"></a>Áttekintés

Egy adatbázis-példány hello forrásadatbázis hello időpontban hello kérelmet a meglévő pillanatkép. Kiválaszthatja ugyanarra a kiszolgálóra vagy egy másik kiszolgálót, a szolgáltatás és teljesítményszintet szintje vagy hello belül különböző teljesítményszintet hello azonos szolgáltatási réteg (kiadás). Hello Másolás befejezése után egy teljesen működőképes, független adatbázis válik. Ezen a ponton frissítése vagy tooany edition használni azt. hello bejelentkezéseket, felhasználók és engedélyek függetlenül is kezelhetők.  

## <a name="logins-in-hello-database-copy"></a>Az adatbázis-másolat hello bejelentkezések

Egy adatbázis toohello másolásakor azonos logikai kiszolgáló, hello azonos bejelentkezések mindkét adatbázissal is használhatók. hello rendszerbiztonsági tag toocopy hello adatbázist használ hello új adatbázis hello adatbázis tulajdonosa lesz. Minden adatbázis-felhasználók, az engedélyek és a biztonsági azonosítók (SID) másolt toohello adatbázis-másolat.  

Egy adatbázis tooa másik logikai kiszolgáló másolásakor a hello rendszerbiztonsági tag hello új kiszolgálóra lesz hello adatbázis tulajdonosa hello új adatbázis. Használatakor [tartalmazott adatbázis-felhasználók](sql-database-manage-logins.md) adatelérési, győződjön meg arról, hogy mindkét hello elsődleges és másodlagos adatbázisok mindig hello ugyanazon felhasználó hitelesítő adatait, így az, hogy hello másolás után végezhető el, azonnal férhetnek hozzá a hello azonos hitelesítő adatok. 

Ha [Azure Active Directory](../active-directory/active-directory-whatis.md), teljesen megszüntetheti hello másolatot a hitelesítő adatok kezelése hello szükségességét. Azonban hello adatbázis tooa új kiszolgáló másolásakor hello bejelentkezés-alapú hozzáférés előfordulhat, hogy nem működik, mert hello bejelentkezések nem léteznek hello új kiszolgálón. bejelentkezések kezelése adatbázis tooa másik logikai kiszolgálót, másolásakor toolearn lásd [hogyan toomanage Azure SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md). 

Sikeres hello másolását követően, és mielőtt más felhasználók újra vannak társítva, csak hello bejelentkezési, másolása, hello kezdeményezett hello adatbázis-tulajdonos, bejelentkezhet toohello új adatbázist. tooresolve bejelentkezések hello másolása művelet befejezése után, lásd: [bejelentkezések megoldásához](#resolve-logins).

## <a name="copy-a-database-by-using-hello-azure-portal"></a>Adatbázis másolása hello Azure-portál használatával

egy adatbázis használatával hello Azure portál, az adatbázis megnyitása hello oldaláról, és kattintson a toocopy **másolási**. 

   ![Az adatbázis másolása](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Adatbázis másolása a PowerShell használatával

a PowerShell használata hello adatbázis toocopy [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) parancsmag. 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Tekintse meg a teljes minta parancsfájlt [adatbázis tooa új kiszolgáló másolása](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Adatbázis másolása Transact-SQL használatával

Jelentkezzen be toohello master adatbázis hello kiszolgálószintű fő bejelentkezéssel vagy hello bejelentkezési azonosítót, amely a létrehozni kívánt toocopy hello adatbázis. -Adatbázis másolása toosucceed bejelentkezéseket, amelyek nincsenek hello kiszolgálószintű egyszerű hello dbmanager szerepkör tagjának kell lennie. További információ a bejelentkezési és kapcsolódó toohello kiszolgáló: [bejelentkezések kezelése](sql-database-manage-logins.md).

Hello forrásadatbázis hello másolásának megkezdése [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) utasítást. Az utasítás végrehajtása kezdeményezi hello adatbázis másolása folyamat. Mivel az adatbázis másolása egy aszinkron folyamat, hello CREATE DATABASE utasítás hello adatbázis-Másolás befejezése előtt adja vissza.

### <a name="copy-a-sql-database-toohello-same-server"></a>Másolja át egy SQL-adatbázis toohello ugyanarra a kiszolgálóra
Jelentkezzen be toohello master adatbázis hello kiszolgálószintű fő bejelentkezéssel vagy hello bejelentkezési azonosítót, amely a létrehozni kívánt toocopy hello adatbázis. -Adatbázis másolása toosucceed bejelentkezéseket, amelyek nincsenek hello kiszolgálószintű egyszerű hello dbmanager szerepkör tagjának kell lennie.

Ez a parancs átmásolja Database1 tooa új adatbázis Database2 megnevezett hello ugyanarra a kiszolgálóra. Attól függően, hogy az adatbázis hello méretét a művelet másolása hello is igénybe vehet néhány alkalommal toocomplete.

    -- Execute on hello master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-tooa-different-server"></a>SQL adatbázis tooa másik kiszolgálót másolása

Jelentkezzen be toohello master adatbázis hello célkiszolgáló, hello SQL adatbázis-kiszolgáló létrehozása toobe hello új adatbázis esetén. Használjon olyan bejelentkezési azonosítót, rendelkezik hello ugyanazt a nevet és jelszót az hello forrásadatbázis forráskiszolgálón hello SQL adatbázis hello adatbázis tulajdonosa. hello bejelentkezési hello célkiszolgálón kell hello dbmanager szerepkör tagjai vagy is hello kiszolgálószintű fő bejelentkezéssel.

Ez a parancs Database1 kiszolgáló1 tooa új adatbázis Database2 nevű kiszolgáló2 másolja át. Attól függően, hogy az adatbázis hello méretét a művelet másolása hello is igénybe vehet néhány alkalommal toocomplete.

    -- Execute on hello master database of hello target server (server2)
    -- Start copying from Server1 tooServer2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-hello-progress-of-hello-copying-operation"></a>A Másolás művelet hello hello előrehaladást

Hello másolási folyamat figyelése hello sys.databases és sys.dm_database_copies nézet lekérdezésével. Amíg hello másolása folyamatban van, hello **state_desc** hello sys.databases nézetben hello új adatbázis oszlop értéke túl**MÁSOLÁSA**.

* Ha hello másolása sikertelen, hello **state_desc** hello sys.databases nézetben hello új adatbázis oszlop értéke túl**FELTÉTELEZHETŐ**. Új adatbázis hello hello DROP utasítást hajt végre, és próbálkozzon újra később.
* Ha hello másolása sikeres, hello **state_desc** hello sys.databases nézetben hello új adatbázis oszlop értéke túl**ONLINE**. hello másolása befejeződött, és új adatbázis hello rendszeres adatbázis független hello forrásadatbázis módosítható.

> [!NOTE]
> Ha úgy dönt, hogy toocancel az hello másolja, amíg folyamatban van, a végrehajtást hello [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) hello új adatbázis utasítást. Másik lehetőségként hello forrásadatbázison hello DROP DATABASE utasítás végrehajtása is visszavonja hello másolása folyamat.
> 

## <a name="resolve-logins"></a>Oldja meg a bejelentkezési adatok

Miután új adatbázis hello hello célkiszolgálón online állapotban, a hello [ALTER felhasználói](https://msdn.microsoft.com/library/ms176060.aspx) utasítás tooremap hello felhasználók hello új adatbázis-toologins hello célkiszolgálón. tooresolve árva felhasználók [árva felhasználók hibaelhárítása](https://msdn.microsoft.com/library/ms175475.aspx). Lásd még: [hogyan toomanage Azure SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md).

Hello új adatbázisban lévő összes felhasználó számára, hogy a hello forrásadatbázis hello engedélyek megőrzése mellett. adatbázis-másolat hello elindító felhasználó hello hello adatbázis hello új adatbázis tulajdonosa lesz, és hozzá van rendelve egy új biztonsági azonosítóval (SID). Sikeres hello másolását követően, és mielőtt más felhasználók újra vannak társítva, csak hello bejelentkezési, másolása, hello kezdeményezett hello adatbázis-tulajdonos, bejelentkezhet toohello új adatbázist.

Tekintse meg a felhasználók és bejelentkezések kezelése, adatbázis tooa másik logikai kiszolgálót, másolásakor toolearn [hogyan toomanage Azure SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Következő lépések

* Bejelentkezések kapcsolatos információkért lásd: [bejelentkezések kezelése](sql-database-manage-logins.md) és [hogyan toomanage Azure SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md).
* tooexport adatbázisba, lásd: [hello adatbázis tooa BACPAC exportálása](sql-database-export.md).
