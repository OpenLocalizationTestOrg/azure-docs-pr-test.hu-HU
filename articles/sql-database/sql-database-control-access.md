---
title: "aaaGranting hozzáférés tooAzure SQL-adatbázis |} Microsoft Docs"
description: "További információk a hozzáférés tooMicrosoft Azure SQL Database megadása."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/06/2017
ms.author: rickbyh
ms.openlocfilehash: 4c32fafa7e98b731ff2f9bf4666da7e4a3145286
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-access-control"></a>Az Azure SQL Database hozzáférés-vezérlése
tooprovide biztonsági, SQL-adatbázis szabályozza a hozzáférést igénylő felhasználók tooprove hitelesítési mechanizmusok korlátozása kapcsolat IP-címe, tűzfal-szabályokat a személyazonosságukat, és korlátozza a felhasználók toospecific műveletek és az adatok engedélyezési mechanizmusokat. 

> [!IMPORTANT]
> Hello SQL-adatbázis biztonsági funkcióinak áttekintéséért lásd: [SQL biztonsági áttekintése](sql-database-security-overview.md). Az oktatóanyagok esetén lásd: [az Azure SQL Database biztonságos](sql-database-security-tutorial.md).

## <a name="firewall-and-firewall-rules"></a>Tűzfal és tűzfalszabályok
A Microsoft Azure SQL Database egy relációs adatbázis-szolgáltatást nyújt az Azure és egyéb internetalapú alkalmazások számára. az adatok védelméhez toohelp, tűzfalak tooyour adatbázis-kiszolgáló minden hozzáférés tiltása, amíg megadhatja, hogy mely számítógépek rendelkeznek engedéllyel. hello tűzfal engedélyezi a hozzáférést toodatabases származó IP-cím az egyes kérelmek hello alapján. További információkért lásd: [Az Azure SQL Database-tűzfalszabályok áttekintése](sql-database-firewall-configure.md).

hello Azure SQL Database szolgáltatásban csak az 1433-as TCP-porton keresztül érhető el. a számítógép egy SQL-adatbázis tooaccess győződjön meg arról, hogy az ügyfél számítógép tűzfal engedélyezi-e az 1433-as TCP-port kimenő TCP-kommunikáció. Ha más alkalmazásoknak nincs szüksége rá, akkor blokkolja az 1433-as TCP-port bejövő kapcsolatait. 

Hello csatlakozási folyamat részeként az Azure virtuális gépek közötti kapcsolatok átirányított tooa másik IP-címet és portot, egyedi minden feldolgozói szerepkör esetében. hello portszám van 11000 too11999 hello közé. További információ a TCP-portok: [kívüli ADO.NET 4.5 és SQL Database2 1433-as portokon](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Authentication

Az SQL Database két hitelesítési típust támogat:

* A felhasználónévvel és jelszóval végzett **SQL-hitelesítést**. Hello logikai kiszolgáló létrehozása után az adatbázis, a "kiszolgáló-rendszergazda" Bejelentkezés a felhasználónevet és jelszót adott meg. Ezeket a hitelesítő adatokat használva hitelesítheti tooany adatbázis a kiszolgálón hello adatbázis tulajdonosa vagy a "dbo." 
* Az **Azure Active Directory-alapú hitelesítést**, amely az Azure Active Directory által felügyelt identitásokat használ, és a felügyelt és integrált tartományok is támogatják. [Amikor csak lehet](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode), használja az Active Directory-hitelesítést (beépített biztonság). Ha azt szeretné, hogy toouse Azure Active Directory-hitelesítéssel, létre kell hoznia egy másik kiszolgáló rendszergazdája hello "Azure AD admin," tooadminister az Azure Active Directory-felhasználók és csoportok engedélyezett nevű. Ez a rendszergazda a normál kiszolgálói rendszergazdák által elvégezhető összes műveletet is végrehajthatja. Lásd: [Kapcsolódás adatbázis által használata Azure Active Directory-hitelesítéssel tooSQL](sql-database-aad-authentication.md) a forgatókönyv bemutatja, hogyan toocreate az Azure AD rendszergazdai tooenable Azure Active Directory-hitelesítéssel.

hello adatbázismotor 30 percnél hosszabb ideig tétlen kapcsolatok bezárása után. kapcsolat hello bejelentkezési újra kell a használat előtt. Folyamatosan aktív kapcsolatok tooSQL adatbázis megkövetelése (hello adatbázismotor által végzett) újbóli engedélyezés legalább 10 óránként. hello adatbázismotor kísérletet tett újbóli engedélyezés hello eredetileg elküldött jelszóval, és nincs felhasználói bevitel szükség. A megfelelő teljesítmény érdekében a jelszavát az SQL-adatbázis, hello kapcsolat esetén nem hitelesíthető, még akkor is, ha hello kapcsolat alaphelyzetbe állítása miatt tooconnection készletezését. Ez eltér a helyi SQL Server hello viselkedését. Hello jelszava módosult, mert hello kapcsolat kezdetben volt-e, ha hello kapcsolat kell végződnie, és egy új kapcsolatot létesíteni a hello új jelszó használatával. Hello rendelkező felhasználó `KILL DATABASE CONNECTION` engedély is explicit módon állítsa le a kapcsolat tooSQL adatbázis hello segítségével [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql) parancsot.

Felhasználói fiókok hello főadatbázisban hozhatók létre, és kaphatnak hello kiszolgálón lévő összes adatbázis az engedélyeket, vagy azok hello adatbázis (néven tartalmazott felhasználók) hozhatók létre. A bejelentkezések létrehozásával és kezelésével kapcsolatos információért lásd: [Bejelentkezések kezelése](sql-database-manage-logins.md). tooenhance hordozhatóságáról és scalabilty, használja a tartalmazott adatbázis felhasználók tooenhance méretezhetőséget. A tartalmazott felhasználókról további információ a [tartalmazottadatbázis-felhasználókkal kapcsolatos, az adatbázis hordozhatóvá tételével](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), a [CREATE USER-rel (Transact-SQL utasítás)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) és a [tartalmazott adatbázisokkal](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) foglalkozó cikkekben található.

Ajánlott eljárásként használja az alkalmazás kell egy külön fiókot tooauthenticate--korlátozása hello jogosultságaitól toohello alkalmazás és a kártékony tevékenység hello kockázatok csökkentése, ha az alkalmazás kódjában sebezhető tooa SQL-injektálás így támadás. hello ajánlott megközelítés toocreate egy [tartalmazott adatbázis-felhasználó](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), amely lehetővé teszi, hogy az alkalmazás tooauthenticate közvetlenül toohello adatbázis. 

## <a name="authorization"></a>Engedélyezés

Engedélyezési toowhat hivatkozik egy felhasználó egy Azure SQL Database belül végrehajtható, és ez által a felhasználói fiók adatbázis [szerepkörtagságok](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) és [objektumszintű engedélyeket](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine). Ajánlott eljárásként akkor adja meg az felhasználók hello szükséges legalacsonyabb jogosultságok. hello server rendszergazdai fiók csatlakozik a db_owner, melynek hatóság toodo hello adatbázison belül minden tagja. Tartsa meg ezt a fiókot a sémafrissítések üzembe helyezéséhez és egyéb felügyeleti műveletekhez. Az alkalmazás toohello adatbázisból hello a korlátozott engedélyekkel tooconnect hello "ApplicationUser" fiók használata az alkalmazás számára szükséges legalacsonyabb jogosultságok. További információk: [Bejelentkezések kezelése](sql-database-manage-logins.md).

Általában csak a rendszergazdák kell elérni a toohello `master` adatbázis. Rendszeres tooeach felhasználói adatbázist tartalmazott adatbázis nem rendszergazda felhasználó hozott létre az egyes adatbázisok keresztül kell lennie. Tartalmazott adatbázis-felhasználók használatakor nem kell hello toocreate bejelentkezésekre `master` adatbázis. További információt a [tartalmazottadatbázis-felhasználókkal kapcsolatos, az adatbázis hordozhatóvá tételével foglalkozó](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) cikkben talál.

Meg kell Ismerkedjen meg a következő funkciók használt toolimit vagy magasabb szintű engedélyek hello:   
* [Megszemélyesítés](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) és [modul-aláíró](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) kell használt toosecurely emelhet engedélyek ideiglenesen.
* A [sorszintű biztonság](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) használatával korlátozhatja, hogy a felhasználó mely sorokhoz férhessen hozzá.
* [Adatok maszkolása](sql-database-dynamic-data-masking-get-started.md) használt toolimit a bizalmas adatok felfedésnek lehet.
* [Tárolt eljárások](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) lehetnek használt toolimit hello műveletek hello adatbázison lehet tenni.

## <a name="next-steps"></a>Következő lépések

- Hello SQL-adatbázis biztonsági funkcióinak áttekintéséért lásd: [SQL biztonsági áttekintése](sql-database-security-overview.md).
- toolearn tűzfalszabályok, bővebben lásd: [tűzfal-szabályok](sql-database-firewall-configure.md).
- bejelentkezések, és a felhasználók toolearn lásd [bejelentkezések kezelése](sql-database-manage-logins.md). 
- Az előrejelzéses figyeléssel tárgyalását lásd: [Database Auditing](sql-database-auditing.md) és [SQL adatbázis Fenyegetésészlelés](sql-database-threat-detection.md).
- Az oktatóanyagok esetén lásd: [az Azure SQL Database biztonságos](sql-database-security-tutorial.md).
