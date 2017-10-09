---
title: "aaaAzure SQL bejelentkezési adatok és a felhasználók |} Microsoft Docs"
description: "További információ az SQL-adatbázis biztonsági management, kifejezetten hogyan toomanage adatbázis-hozzáférés és a bejelentkezési biztonsági hello kiszolgálószintű fő fiókon keresztül."
keywords: "sql database biztonság,adatbázis biztonságának felügyelete,bejelentkezési biztonság,adatbázis biztonsága,adatbázis-hozzáférés"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 0a65a93f-d5dc-424b-a774-7ed62d996f8c
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/23/2017
ms.author: rickbyh
ms.openlocfilehash: dff889b9fed09146a10008c0d11ca9e71d91df5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="controlling-and-granting-database-access"></a>Adatbázis-hozzáférés szabályozása és biztosítása

Ha tűzfal-szabályok beállítása megtörtént, személyek is csatlakozzon az tooa SQL-adatbázis rendelkezésre álló hello rendszergazdai fiókok, hello adatbázis tulajdonosa, vagy hello adatbázis adatbázis-felhasználó.  

>  [!NOTE]  
>  Ez a témakör az tooAzure SQL server és tooboth SQL-adatbázis és az SQL Data Warehouse adatbázisok hello Azure SQL-kiszolgálón létrehozott vonatkozik. Az egyszerűség kedvéért SQL-adatbázis tooboth SQL-adatbázis és az SQL Data Warehouse hivatkozás során használt. 
>

> [!TIP]
> Az oktatóanyagok esetén lásd: [az Azure SQL Database biztonságos](sql-database-security-tutorial.md).
>


## <a name="unrestricted-administrative-accounts"></a>Nem korlátozott rendszergazdai fiókok
Kettő rendszergazdaként működő felügyeleti fiók létezik (**Kiszolgálói rendszergazdai** és **Active Directory-rendszergazdai**). tooidentify ezeket az SQL server rendszergazdai fiókok nyissa meg a hello Azure-portálon, és keresse meg az SQL server toohello tulajdonságait.

![SQL Server-rendszergazdák](./media/sql-database-manage-logins/sql-admins.png)

- **Kiszolgálói rendszergazda**   
Azure SQL Server-kiszolgáló létrehozásakor ki kell jelölnie egy **kiszolgáló-rendszergazdai felhasználónevet**. SQL server fiókot hoz létre, hogy egy bejelentkezésként hello master adatbázisban. Ez a fiók SQL Server-hitelesítéssel csatlakozik (felhasználónévvel és jelszóval). Ilyen fiókból csak egy létezhet.   
- **Azure Active Directory-rendszergazda**   
Egy Azure Active Directory-fiók (különálló vagy biztonságicsoport-fiók) is konfigurálható rendszergazdaként. Nem kötelező tooconfigure az Azure AD-rendszergazdaként, de az Azure AD-rendszergazdának meg kell adni, ha azt szeretné, hogy toouse az Azure AD fiókok tooconnect tooSQL adatbázis. Azure Active Directory elérés konfigurálásával kapcsolatos további információkért lásd: [csatlakozás tooSQL adatbázis vagy az SQL Data Warehouse által használata Azure Active Directory-hitelesítéssel](sql-database-aad-authentication.md) és [SSMS az Azure AD MFA SQL támogatása Adatbázis és az SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).
 

Hello **kiszolgálói rendszergazda** és **az Azure AD rendszergazdai** fiókok hello a következő jellemzőkkel rendelkezik:
- Ezek a hello csak fiókok, amely automatikusan képes kapcsolódni az SQL-adatbázis tooany hello kiszolgálón. (tooconnect tooa felhasználói adatbázis, más fiókokat kell hello hello tulajdonosának adatbázis, vagy egy felhasználói fiókkal kell rendelkeznie a hello felhasználói adatbázisban.)
- Ezeknek a fiókoknak felhasználói adatbázisokat adjon meg hello `dbo` felhasználói és azok jogosult összes hello hello felhasználói adatbázisban. (a felhasználói adatbázis tulajdonosa hello is ír a mezőbe hello adatbázis hello `dbo` felhasználói.) 
- Ezeknek a fiókoknak ne adjon meg hello `master` hello adatbázis `dbo` felhasználói és azok korlátozott engedélyekkel a főadatbázisban. 
- Ezek a fiókok nem tagjai hello szabványos SQL Server `sysadmin` rögzített kiszolgálói szerepkör, amely nem érhető el az SQL-adatbázis.  
- Ezek a fiókok adatbázisokat, bejelentkezéseket, master felhasználókat és kiszolgálószintű tűzfalszabályokat hozhatnak létre, módosíthatnak vagy vethetnek el.
- Ezek a fiókok hozzáadhat és eltávolíthat tagokat toohello `dbmanager` és `loginmanager` szerepkörök.
- Ezek a fiókok megtekintheti hello `sys.sql_logins` rendszertáblában.

### <a name="configuring-hello-firewall"></a>Hello tűzfal konfigurálása
Ha hello kiszolgálószintű tűzfal van konfigurálva egy egyedi IP-címet vagy tartományt, hello **SQL kiszolgáló rendszergazdája** és hello **Azure Active Directory-rendszergazda** toohello master adatbázis és az összes hello kapcsolódhatnak felhasználói adatbázisokat. hello kezdeti kiszolgálószintű tűzfal konfigurálható hello [Azure-portálon](sql-database-get-started-portal.md)használatával [PowerShell](sql-database-get-started-powershell.md) vagy hello segítségével [REST API](https://msdn.microsoft.com/library/azure/dn505712.aspx). A kapcsolat létrehozása után további kiszolgálószintű tűzfalszabályok is konfigurálhatók a [Transact-SQL](sql-database-configure-firewall-settings.md) segítségével.

### <a name="administrator-access-path"></a>Rendszergazdai hozzáférés elérési útja
Ha hello kiszolgálószintű tűzfal megfelelően van konfigurálva, hello **SQL kiszolgáló rendszergazdája** és hello **Azure Active Directory-rendszergazda** kapcsolódhatnak ügyfél eszközökkel, például az SQL Server Management Studio vagy SQL Server Data Tools összetevővel. Csak hello legújabb eszközök összes hello szolgáltatásokat és képességeket biztosítanak. hello alábbi ábrán egy tipikus konfigurációja hello két rendszergazdai fiókokat.

![Rendszergazdai hozzáférés elérési útja](./media/sql-database-manage-logins/1sql-db-administrator-access.png)

Egy nyitott port hello kiszolgálószintű tűzfal használata esetén a rendszergazdák kapcsolódhatnak tooany SQL-adatbázis.

### <a name="connecting-tooa-database-by-using-sql-server-management-studio"></a>Csatlakozás tooa adatbázis SQL Server Management Studio használatával
A kiszolgáló, adatbázis, kiszolgálószintű tűzfal-szabályok létrehozása, és használja az SQL Server Management Studio tooquery adatbázis segédlet, lásd: [Ismerkedés az Azure SQL Database-kiszolgálók, adatbázisok és tűzfalszabályok hello Azure-portál használatával és az SQL Server Management Studio](sql-database-get-started-portal.md).

> [!IMPORTANT]
> Javasoljuk, hogy mindig használjon hello Azure és az SQL-adatbázis a frissítések tooMicrosoft szinkronizálva tooremain Management Studio legújabb verzióját. [Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="additional-server-level-administrative-roles"></a>További kiszolgálószintű rendszergazdai szerepkörök
Továbbá toohello kiszolgálószintű rendszergazdai szerepkörei korábban tárgyalt, SQL-adatbázist biztosít két korlátozott rendszergazdai szerepkörei hello főadatbázis toowhich felhasználói fiókok lehet hozzáadni, amely a engedélyeket tooeither adatbázisok létrehozása vagy kezelése bejelentkezések.

### <a name="database-creators"></a>Adatbázis-létrehozók
Ezek a rendszergazdai szerepkörök egyik hello **dbmanager** szerepkör. Ezen szerepkör tagjai létrehozhatnak új adatbázisokat. toouse ezt a szerepkört hoz létre a felhasználó hello `master` adatbázisról, és hozzáadja a hello felhasználói toohello **dbmanager** adatbázis-szerepkör. adatbázis toocreate, hello vagy kell lennie a felhasználó hello master adatbázisban SQL Server-bejelentkezés alapján tartalmazott adatbázis-felhasználót egy Azure Active Directory felhasználó alapján.

1. Rendszergazdai fiók használatával, csatlakoztassa a toohello fő adatbázist.
2. Nem kötelező lépés: az SQL Server hitelesítési-bejelentkezés létrehozásával, hello segítségével [hozzon létre bejelentkezési](https://msdn.microsoft.com/library/ms189751.aspx) utasítás. Mintautasítás:
   
   ```
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```
   
   > [!NOTE]
   > Használjon erős jelszót a bejelentkezési vagy tartalmazottadatbázis-felhasználó létrehozásakor. További információkért lásd az [erős jelszavak](https://msdn.microsoft.com/library/ms161962.aspx) létrehozását ismertető cikket.
    
   tooimprove teljesítmény bejelentkezések (kiszolgálószintű résztvevők) ideiglenesen gyorsítótárba kerüljenek-e hello adatbázis szintjén. toorefresh hello hitelesítési gyorsítótárat, lásd: [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

3. Hello főadatbázisban, hozzon létre egy felhasználó hello segítségével [felhasználó létrehozása](https://msdn.microsoft.com/library/ms173463.aspx) utasítást. hello felhasználói egy Azure Active Directory hitelesítési tartalmazott adatbázis-felhasználó (Ha már konfigurálta a környezetet az Azure AD-alapú hitelesítés), vagy egy SQL Server hitelesítési tartalmazott adatbázis-felhasználó vagy egy SQL Server authentication felhasználóhoz egy SQL-alapú is lehet. Kiszolgálói hitelesítési bejelentkezési névként (hello előző lépésben létrehozott.) Mintautasítások:
   
   ```
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
   CREATE USER Tran WITH PASSWORD = '<strong_password>';
   CREATE USER Mary FROM LOGIN Mary; 
   ```

4. Hello új felhasználó, toohello **dbmanager** adatbázis-szerepkör hello segítségével [az ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx) utasítást. Mintautasítások:
   
   ```
   ALTER ROLE dbmanager ADD MEMBER Mary; 
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```
   
   > [!NOTE]
   > hello dbmanager, csak akkor adhat hozzá egy adatbázis-felhasználó toohello dbmanager szerepkör a master adatbázis egy adatbázis-szerepkör. Nem adhat hozzá egy kiszolgálószintű bejelentkezéssel toodatabase szintű szerepkört.
    
5. Szükség esetén konfiguráljon egy tűzfal szabály tooallow hello új felhasználói tooconnect. (hello új felhasználó előfordulhat, hogy elegendő meglévő tűzfalszabály.)

Most hello felhasználói toohello főadatbázis kapcsolódhatnak, és hozhat létre új adatbázisokat. hello fiók hello adatbázis létrehozása a hello hello adatbázis tulajdonosa lesz.

### <a name="login-managers"></a>Bejelentkezéskezelők
hello más rendszergazdai szerepkör a rendszer hello bejelentkezési manager. A szerepkör tagjai hozhat létre új bejelentkezési identitások hello master adatbázisban. Ha kívánja, akkor fejezheti be hello ugyanazokat a lépéseket (a bejelentkezési és a felhasználó létrehozása, majd adja meg a felhasználó toohello **loginmanager** szerepkör) tooenable a felhasználó új bejelentkezésekre toocreate hello fő. Bejelentkezések általában nem szükséges, mivel tartalmazott adatbázis-felhasználók hitelesítéséhez: hello segítségével a Microsoft javasolja adatbázis szintű felhasználókat bejelentkezések alapján használata helyett. További információt a [tartalmazottadatbázis-felhasználókkal kapcsolatos, az adatbázis hordozhatóvá tételével foglalkozó](https://msdn.microsoft.com/library/ff929188.aspx) cikkben talál.

## <a name="non-administrator-users"></a>Nem rendszergazdai felhasználók
Általában nem rendszergazdai fiókokat nem kell elérni toohello fő adatbázist. Hozzon létre a tartalmazott adatbázis-felhasználók hello segítségével hello adatbázis szintjén [felhasználó létrehozása (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx) utasítást. hello felhasználói egy Azure Active Directory hitelesítési tartalmazott adatbázis-felhasználó (Ha már konfigurálta a környezetet az Azure AD-alapú hitelesítés), vagy egy SQL Server hitelesítési tartalmazott adatbázis-felhasználó vagy egy SQL Server authentication felhasználóhoz egy SQL-alapú is lehet. Kiszolgálói hitelesítési bejelentkezési névként (hello előző lépésben létrehozott.) További információt a [tartalmazottadatbázis-felhasználókkal kapcsolatos, az adatbázis hordozhatóvá tételével foglalkozó](https://msdn.microsoft.com/library/ff929188.aspx) cikkben talál. 

toocreate felhasználók toohello adatbázis csatlakozni, és hajtsa végre a utasítások hasonló toohello példák a következő:

```
CREATE USER Mary FROM LOGIN Mary; 
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Csak az egyik hello rendszergazdák vagy hello adatbázis tulajdonosa hello kezdetben felhasználók hozhat létre. tooauthorize további felhasználók toocreate új felhasználók, adja meg, hogy a kiválasztott felhasználó hello `ALTER ANY USER` engedéllyel, például egy utasítás használatával:

```
GRANT ALTER ANY USER tooMary;
```

toogive további felhasználók teljes körű hozzáférést engedélyezzenek hello adatbázis, adja hozzá őket hello **db_owner** rögzített adatbázis-szerepkör használatával hello `ALTER ROLE` utasítást.

> [!NOTE]
> hello leggyakoribb oka toocreate adatbázis felhasználói bejelentkezések alapján akkor, ha rendelkezik kell adatbázisához toomultiple SQL Server-hitelesítési felhasználók. A felhasználók bejelentkezések alapján legyenek a feltételekhez toohello bejelentkezési, és csak egy jelszót, hogy a bejelentkezés változatlan marad. Az egyes adatbázisokban lévő tartalmazottadatbázis-felhasználók önálló entitások, és mindegyiknek saját jelszava van. Ez félreértésekhez vezethet a tartalmazott adatbázisok felhasználói esetében, ha nem ugyanazokat a jelszavakat használják.

### <a name="configuring-hello-database-level-firewall"></a>Hello adatbázis szintű tűzfal konfigurálása
Ajánlott eljárásként a nem rendszergazda felhasználók csak hello tűzfal toohello adatbázisokat használnak keresztül hozzáféréssel kell rendelkeznie. Helyett engedélyező azok IP-címek a hello kiszolgálószintű tűzfal- és engedélyezése tooall adatbázisokat, használja a hello [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) utasítás tooconfigure hello adatbázis szintű tűzfal. hello adatbázis szintű tűzfal hello portál használatával nem konfigurálható.

### <a name="non-administrator-access-path"></a>Nem rendszergazdai hozzáférés elérési útja
Hello adatbázis szintű tűzfal helyesen van konfigurálva, amikor a hello adatbázis felhasználók kapcsolódhatnak ügyfél eszközök, például az SQL Server Management Studio vagy SQL Server Data Tools használatával. Csak hello legújabb eszközök összes hello szolgáltatásokat és képességeket biztosítanak. hello a következő ábrán egy tipikus nem rendszergazdai hozzáférés elérési útját jeleníti meg.

![Nem rendszergazdai hozzáférés elérési útja](./media/sql-database-manage-logins/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Csoportok és szerepkörök
Hatékony kezelési toogroups és ne pedig egyéni felhasználóknak szerepkörök jogosultságait használja. 

- Azure Active Directory-hitelesítés használatakor az Azure Active Directory-felhasználókat helyezze egy Azure Active Directory-csoportba. Hello csoport tartalmazott adatbázis felhasználót kell létrehozni. Jelölje be egy vagy több adatbázis-felhasználók egy [adatbázis-szerepkör](https://msdn.microsoft.com/library/ms189121) , majd rendelje hozzá [engedélyek](https://msdn.microsoft.com/library/ms191291.aspx) toohello adatbázis-szerepkör.

- Ha az SQL Server-hitelesítést használ, hozzon létre tartalmazott adatbázis-felhasználók hello adatbázis. Jelölje be egy vagy több adatbázis-felhasználók egy [adatbázis-szerepkör](https://msdn.microsoft.com/library/ms189121) , majd rendelje hozzá [engedélyek](https://msdn.microsoft.com/library/ms191291.aspx) toohello adatbázis-szerepkör.

adatbázis-szerepkörök hello lehet hello beépített szerepkörök például **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter**, és **db_denydatareader**. **db_owner** gyakran használt toogrant teljes jogosultságot kapnak tooonly néhány felhasználó van. hello más rögzített adatbázis-szerepkörök egyszerű adatbázis gyors fejlesztés megszerzése során hasznosak, de a legtöbb éles környezetben használt adatbázisait nem ajánlott. Például hello **db_datareader** rögzített adatbázis-szerepkör biztosít az olvasási hozzáférés tooevery tábla hello adatbázis, amely általában több mint nem feltétlenül szükséges. Sokkal hatékonyabb toouse hello [SZEREPKÖR létrehozása](https://msdn.microsoft.com/library/ms187936.aspx) utasítás toocreate a saját felhasználói adatbázis-szerepkörök, és gondosan engedélyek minden egyes szerepkör hello legalább szükséges hello üzleti igényeknek. Ha egy felhasználó több szerepkör tagja, akkor hello engedélyek az összes összesítése.

## <a name="permissions"></a>Engedélyek
Az SQL Database-ben több mint 100 engedély adható vagy tagadható meg külön-külön. Ezek közül számos engedély beágyazott. Például hello `UPDATE` engedéllyel rendelkezik a séma tartalmaz hello `UPDATE` engedéllyel, hogy a séma belül minden táblában. A legtöbb engedély rendszerekhez hasonlóan engedély hello szolgáltatásmegtagadásos felülbírálja a támogatás. Beágyazott hello természetét és a engedélyek hello száma miatt is igénybe vehet gondos vizsgálat toodesign egy megfelelő engedélyekkel az rendszer tooproperly az adatbázis védelmét. Hello listája engedélye az kezdődnie [engedélyek (adatbázismotor)](https://msdn.microsoft.com/library/ms191291.aspx) és tekintse át a hello [poszter méretű kép](http://go.microsoft.com/fwlink/?LinkId=229142) hello engedélyek.


### <a name="considerations-and-restrictions"></a>Megfontolandó szempontok és korlátozások
Bejelentkezések és a felhasználók az SQL-adatbázis felügyeletekor vegye figyelembe a hello következőket:

* Csatlakoztatott toohello kell **fő** adatbázis-hello végrehajtásakor `CREATE/ALTER/DROP DATABASE` utasításokat.   
* adatbázis-felhasználó megfelelő toohello hello **kiszolgálói rendszergazda** bejelentkezési nem módosítható vagy eldobni. 
* Amerikai angol az alapértelmezett nyelve hello hello **kiszolgálói rendszergazda** bejelentkezési.
* Csak a rendszergazdák hello (**kiszolgálói rendszergazda** bejelentkezési vagy Azure AD-rendszergazda) és hello hello tagjai **dbmanager** hello szerepkörrel **fő** adatbázisa tartalmazza engedély tooexecute hello `CREATE DATABASE` és `DROP DATABASE` utasításokat.
* Csatlakoztatott toohello master adatbázisban kell lennie, hello végrehajtásakor `CREATE/ALTER/DROP LOGIN` utasításokat. A bejelentkezési adatok használata azonban nem javasolt. Helyette használja a tartalmazott adatbázis felhasználóit.
* tooconnect tooa felhasználói adatbázis hello adatbázis hello kapcsolat-karakterláncban hello nevét kell megadnia.
* Csak a kiszolgálószintű fő bejelentkezési és hello tagjai hello hello **loginmanager** hello szerepkörrel **fő** adatbázis rendelkezik engedéllyel tooexecute hello `CREATE LOGIN`, `ALTER LOGIN`, és `DROP LOGIN` utasításokat.
* Hello végrehajtásakor `CREATE/ALTER/DROP LOGIN` és `CREATE/ALTER/DROP DATABASE` egy ADO.NET alkalmazásban utasítások paraméteres parancsokat nem engedélyezett. További információkért lásd: [Parancsok és paraméterek](https://msdn.microsoft.com/library/ms254953.aspx).
* Hello végrehajtásakor `CREATE/ALTER/DROP DATABASE` és `CREATE/ALTER/DROP LOGIN` utasítások, minden ezekre az utasításokra kell lennie a Transact-SQL kötegben csak utasítás hello. Különben hiba történik. Például hello következő Transact-SQL ellenőrzi, hogy létezik-e hello adatbázis. Ha létezik, egy `DROP DATABASE` utasítás tooremove hello adatbázis neve. Mivel hello `DROP DATABASE` utasítás nem hello csak a következő utasítás hello kötegben, a végrehajtás alatt álló hello következő Transact-SQL-utasítás hibát eredményez.

  ```
  IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
  DROP DATABASE [database_name];
  GO
  ```

* Hello végrehajtásakor `CREATE USER` hello utasítást `FOR/FROM LOGIN` beállítás, kell lennie a Transact-SQL kötegben csak utasítás hello.
* Hello végrehajtásakor `ALTER USER` hello utasítást `WITH LOGIN` beállítás, kell lennie a Transact-SQL kötegben csak utasítás hello.
* túl`CREATE/ALTER/DROP` egy felhasználónak van szüksége a hello `ALTER ANY USER` hello adatbázis engedélyt.
* Ha egy adatbázis-szerepkör tulajdonosa hello megpróbál tooadd vagy egy másik adatbázis felhasználói tooor, amely az adatbázis-szerepkör eltávolítása, hello a következő hiba fordulhat: **felhasználó vagy a "Name" szerepkör nem létezik az adatbázisban.** Ez a hiba akkor fordul elő, mert hello felhasználó nem látható toohello tulajdonosa. tooresolve a probléma grant hello szerepkör tulajdonosa hello `VIEW DEFINITION` hello felhasználói engedélyt. 


## <a name="next-steps"></a>Következő lépések

- toolearn tűzfalszabályok, bővebben lásd: [Azure SQL Database-tűzfal](sql-database-firewall-configure.md).
- Minden hello SQL-adatbázis biztonsági funkcióinak áttekintéséért lásd: [SQL biztonsági áttekintése](sql-database-security-overview.md).
- Az oktatóanyagok esetén lásd: [az Azure SQL Database biztonságos](sql-database-security-tutorial.md).
- Információk a nézetekről és a tárolt eljárásokról: [Nézetek és tárolt eljárások létrehozása](https://msdn.microsoft.com/library/ms365311.aspx)
- További információ a hozzáférési tooa adatbázis-objektum megadása: [hozzáférés biztosítása tooa adatbázis-objektum](https://msdn.microsoft.com/library/ms365327.aspx)
