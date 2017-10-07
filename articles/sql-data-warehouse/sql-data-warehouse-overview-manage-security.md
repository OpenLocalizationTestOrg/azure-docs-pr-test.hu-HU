---
title: "az SQL Data Warehouse adatbázis aaaSecure |} Microsoft Docs"
description: "Tippek az Azure SQL Data Warehouse adatbázis védelme adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 8fa2f5ca-4cf5-4418-99a2-4dc745799850
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 5ef4d760e00be46bfe7808bc95dbe1e4b3a51165
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-database-in-sql-data-warehouse"></a>Az SQL Data Warehouse adatbázis védelme
> [!div class="op_single_selector"]
> * [Biztonság – áttekintés](sql-data-warehouse-overview-manage-security.md)
> * [Hitelesítés](sql-data-warehouse-authentication.md)
> * [Titkosítás (portál)](sql-data-warehouse-encryption-tde.md)
> * [Titkosítás (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Ez a cikk végigvezeti az Azure SQL Data Warehouse-adatbázis védelme hello alapjait. Különösen, ez a cikk segítséget erőforrásaival korlátozza a hozzáférést, az adatok védelme és figyelési tevékenységek egy adatbázison.

## <a name="connection-security"></a>Kapcsolatbiztonság
Kapcsolat biztonsági toohow korlátozza, és biztonságos tűzfalszabályok vagy a kapcsolat titkosítási használó kapcsolatok tooyour adatbázisra hivatkozik.

Tűzfalszabályok mindkét hello kiszolgáló és a hello adatbázis tooreject próbálnak meg kapcsolódni, amely nem rendelkezik explicit módon szerepel az engedélyezési listán IP-címekről használják. az alkalmazás ügyfél gép nyilvános IP- címének tooallow kapcsolatokat, akkor először létre kell hoznia egy kiszolgálószintű tűzfalszabályt hello Azure portálon, a REST API-t vagy a PowerShell használatával. Ajánlott eljárásként lehetőség szerint a tűzfalon keresztül engedélyezett hello IP-címtartományok korlátozására.  Azure SQL Data Warehouse tooaccess a helyi számítógépen, győződjön meg arról, a hálózati és a helyi számítógép hello tűzfala lehetővé teszi, hogy a 1433-as TCP-port kimenő kommunikációra.  További információkért lásd: [Azure SQL Database-tűzfal][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], és [sp_set_database_ firewall_rule][sp_set_database_firewall_rule].

Az SQL Data Warehouse kapcsolatok tooyour alapértelmezés szerint vannak titkosítva.  Kapcsolati beállítások toodisable módosítani a titkosítási figyelmen kívül hagyja.

## <a name="authentication"></a>Authentication
Hitelesítés, a személyazonosság toohello adatbázishoz kapcsolódáskor toohow hivatkozik. SQL Data Warehouse jelenleg támogatja az SQL Server-hitelesítés egy felhasználónév és jelszó, valamint egy Azure Active Directoryban. 

Hello logikai kiszolgáló létrehozása után az adatbázis, a "kiszolgáló-rendszergazda" Bejelentkezés a felhasználónevet és jelszót adott meg. Ezeket a hitelesítő adatokat használva hitelesítheti tooany adatbázis hello adatbázis tulajdonosa vagy a "dbo" SQL Server-hitelesítésen keresztül a kiszolgálón.

Ajánlott eljárásként, azonban a szervezet felhasználói használjon egy másik fiókot tooauthenticate. Így korlátozhatja hello jogosultságaitól toohello alkalmazás és a kártékony tevékenység hello kockázatok csökkentése, ha az alkalmazás kódjában sebezhető tooa SQL injektálási támadás. 

egy SQL-kiszolgáló hitelesített felhasználó toocreate csatlakozás toohello **fő** adatbázis a kiszolgálón a kiszolgáló-rendszergazdai bejelentkezés, és hozzon létre egy új bejelentkezés a kiszolgálóra.  Emellett egy jó ötlet toocreate a felhasználó már főadatbázis hello Azure SQL Data Warehouse-felhasználók számára. A felhasználó létrehozása a fő lehetővé teszi, hogy egy felhasználó toologin, például az SSMS használatával adatbázis nevének megadása nélkül.  Lehetővé teszi őket toouse hello object explorer tooview összes adatbázist egy SQL-kiszolgálón.

```sql
-- Connect toomaster database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Csatlakoztassa a tooyour **SQL Data Warehouse-adatbázis** az a kiszolgáló-rendszergazdai bejelentkezés, és hozzon létre egy adatbázis-felhasználó, a most létrehozott hello server bejelentkezés alapján.

```sql
-- Connect tooSQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

A felhasználó további műveletek, például a bejelentkezések létrehozása vagy új adatbázisok létrehozásához mindaddig végre, ha azok kell hozzárendelve toobe toohello `Loginmanager` és `dbmanager` szerepkörök hello master adatbázisban. Ezek a további szerepkörök és hitelesítő tooa SQL-adatbázis további információkért lásd: [adatbázisok és az Azure SQL Database bejelentkezések kezelése][Managing databases and logins in Azure SQL Database].  Az SQL Data Warehouse az Azure AD a további részletekért lásd: [csatlakozás az adatraktár által használó Azure Active Directory-hitelesítéssel tooSQL][Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication].

## <a name="authorization"></a>Engedélyezés
Engedélyezési toowhat hivatkozik egy Azure SQL Data Warehouse-adatbázis belül végrehajtható, és ez a felhasználói fiók szerepkörtagságok és engedélyek által szabályozott. Ajánlott eljárásként akkor adja meg az felhasználók hello szükséges legalacsonyabb jogosultságok. Az SQL Data Warehouse T-SQL lehetővé teszi a könnyen toomanage szerepkörök:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser tooread data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser toowrite data
```

hello server rendszergazdai fiók csatlakozik a db_owner, melynek hatóság toodo hello adatbázison belül minden tagja. Tartsa meg ezt a fiókot a sémafrissítések üzembe helyezéséhez és egyéb felügyeleti műveletekhez. Az alkalmazás toohello adatbázisból hello a korlátozott engedélyekkel tooconnect hello "ApplicationUser" fiók használata az alkalmazás számára szükséges legalacsonyabb jogosultságok.

Számos módon toofurther korlátot, a felhasználók mit tehetnek az Azure SQL Database.

* A részletes [engedélyek] [ Permissions] lehetővé teszik, hogy vezérlő milyen műveletek is az egyes oszlopok, táblák, nézetek, eljárások és egyéb objektumok hello adatbázisban. Használjon részletes engedélyeket toohave legtöbb vezérlő hello és hello minimálisan szükséges engedélyeket adja szükséges. hello részletes engedély rendszer némileg bonyolult, és hatékonyan esetén néhány vizsgálat toouse.
* [Adatbázis-szerepkörök] [ Database roles] eltérő db_datareader és db_datawriter lehet használt toocreate nagyobb teljesítményű alkalmazás felhasználói fiókok vagy kevésbé hatékony felügyeleti fiókok. hello beépített rögzített adatbázis-szerepkörök egy egyszerűen toogrant engedélyeket biztosító, de a szükségesnél több jogosultságokat eredményezhet.
* [Tárolt eljárások] [ Stored procedures] lehetnek használt toolimit hello műveletek hello adatbázison lehet tenni.

A portál felhasználói fiók szerepkör-hozzárendelések adatbázisok és a logikai kiszolgáló kezelése a klasszikus Azure portál hello vagy hello Azure Resource Manager API használatával szabályozza. A témakörrel kapcsolatos további információkért lásd: [Azure portál szerepköralapú hozzáférés-vezérlés][Role-based access control in Azure Portal].

## <a name="encryption"></a>Titkosítás
Az Azure SQL Data adatraktár átlátszó adatok titkosítás (TDE) abban a kártékony tevékenység hello fenyegetés valós idejű titkosítási és visszafejtési az adatok aktívan elvégzésével.  Az adatbázis titkosításakor társított biztonsági másolatok és a tranzakciós naplófájlok titkosítottak anélkül, hogy a módosítások tooyour alkalmazásokat. TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja. Az SQL-adatbázis adatbázis-titkosítási kulcs hello védi egy beépített kiszolgálói tanúsítványt. hello beépített kiszolgálói tanúsítvány egyedi SQL-adatbázis kiszolgálónként. Microsoft legalább 90 naponta automatikusan elforgatja ezeket a tanúsítványokat. az SQL Data Warehouse használt hello titkosítási algoritmus az AES-256. TDE általános ismertetését lásd: [átlátható adattitkosítási][Transparent Data Encryption].

Az adatbázis hello segítségével titkosíthatja [Azure Portal] [ Encryption with Portal] vagy [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Következő lépések
Részletek és a csatlakozó tooyour SQL Data Warehouse a különböző protokollokkal példák: [csatlakozzon az adatraktár tooSQL][Connect tooSQL Data Warehouse].

<!--Image references-->

<!--Article references-->
[Connect tooSQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL Database firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Database roles]: https://msdn.microsoft.com/library/ms189121.aspx
[Managing databases and logins in Azure SQL Database]: https://msdn.microsoft.com/library/ee336235.aspx
[Permissions]: https://msdn.microsoft.com/library/ms191291.aspx
[Stored procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data Encryption]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Role-based access control in Azure Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
