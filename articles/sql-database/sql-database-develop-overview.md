---
title: "aaaSQL adatbázis alkalmazás fejlesztői áttekintés |} Microsoft Docs"
description: "Információ érhető el kapcsolat szalagtárak és ajánlott eljárások az adatbázis tooSQL csatlakozó alkalmazásokat."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: 17f04db600828f90c42c750c9abdb92cfa4ca817
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-application-development-overview"></a>SQL-adatbázis alkalmazások fejlesztői áttekintés
Ez a cikk végigvezeti a hello alapvető szempontot, amelyek egy fejlesztő célszerű tisztában lennie a kód tooconnect tooAzure SQL-adatbázis írásakor.

> [!TIP]
> Egy oktatóanyag megjelenítő, hogyan toocreate egy kiszolgálót, hozzon létre egy kiszolgáló-alapú tűzfal kiszolgáló tulajdonságainak megtekintése, SQL Server Management Studio eszközben lekérdezés hello master adatbázis használatával csatlakozzon, hozzon létre egy adatbázist és egy üres adatbázist, lekérdezési adatbázis tulajdonságai, csatlakozás az SQL Server Management Studio eszközt, és lekérdezés hello mintaadatbázis, lásd: [első lépéseket ismertető oktatóanyagban](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Nyelv és platform
Különböző programozási nyelvekhez és platformokhoz érhetők el kódminták. Található hivatkozások toohello kód minták: 

* További információ: [Adatkapcsolattárak az SQL Database-hez és az SQL Serverhez](sql-database-libraries.md)

## <a name="tools"></a>Eszközök 
Használhat olyan nyílt forráskódú eszközöket is, mint például a [Cheetah](https://github.com/wunderlist/cheetah), az [sql-cli](https://www.npmjs.com/package/sql-cli) és a [VS Code](https://code.visualstudio.com/). Ezen kívül az Azure SQL Database olyan Microsoft-eszközöket is támogat, mint például a [Visual Studio](https://www.visualstudio.com/downloads/) és az [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  Hello Azure felügyeleti portálján PowerShell, is használható, és a REST API-k segítségével további termelékenység kapnak.

## <a name="resource-limitations"></a>Erőforrás-korlátozások
Az Azure SQL Database kezeli hello erőforrások elérhető tooa adatbázist két különböző mechanizmusok alapján: erőforrások irányítás és kényszerítési a határértékeket.

* További információ: [Azure SQL Database erőforrás-korlátozások](sql-database-resource-limits.md)

## <a name="security"></a>Biztonság
Az Azure SQL Database erőforrásokat biztosít a hozzáférés korlátozásához, az adatok védelméhez és a tevékenységek monitorozásához az SQL Database-adatbázisokban.

* Bővebb információ: [Az SQL Database-adatbázis védelme](sql-database-security-overview.md)

## <a name="authentication"></a>Authentication
* Az Azure SQL Database az SQL Server-alapú és az [Azure Active Directory-alapú](sql-database-aad-authentication.md) hitelesítést használó felhasználókat és bejelentkezéseket is támogatja.
* Egy adott adatbázis helyett mulasztó toohello toospecify kell *fő* adatbázis.
* Nem használhatja a Transact-SQL hello **használata myDatabaseName;** utasítás SQL-adatbázis tooswitch tooanother adatbázison.
* További információ: [Az SQL Database biztonsága: adatbázis-hozzáférés és a bejelentkezési biztonság felügyelete](sql-database-manage-logins.md)

## <a name="resiliency"></a>Resiliency
Amikor egy átmeneti hiba akkor fordul elő, tooSQL adatbázis kapcsolódás közben, a kódot újra kell hello hívás.  Azt ajánljuk, újrapróbálkozási logika leállítási logika használja, így azt nem ne terhelje tovább hello SQL-adatbázis az újrapróbálkozás egyszerre több ügyfélnek.

* Kódminták: mintakódok, mely újrapróbálkozási logika, lásd: hello nyelvű-példák: [adatkapcsolattárak SQL Database és SQL Server](sql-database-libraries.md)
* További információ: [Az SQL Database-ügyfélprogramok hibaüzenetei](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Kapcsolatok kezelése
* Az ügyfél kapcsolat logikájában felülbírálása hello alapértelmezett időkorlát toobe 30 másodperc.  hello alapértelmezett 15 másodperc érték túl rövid a kapcsolatok függő hello internet.
* Ha használ egy [kapcsolatkészlet](http://msdn.microsoft.com/library/8xx3tyca.aspx), lehet, hogy tooclose hello kapcsolat hello azonnali, a program nem aktívan használja, és nem arra készül, tooreuse azt.

## <a name="network-considerations"></a>Hálózati kapcsolatos szempontok
* Hello számítógépen, amelyen az ügyfélprogram hello tűzfala lehetővé teszi a 1433-as port kimenő TCP-kommunikáció biztosítása.  További információk: [Az Azure SQL Database tűzfalának konfigurálása](sql-database-configure-firewall-settings.md)
* Ha az ügyfélprogram tooSQL adatbázis csatlakozik, amíg az ügyfél egy Azure virtuális gépen (VM) fut, nyissa meg bizonyos porttartományok hello virtuális gép. További információ: [portok túl az 1433-as ADO.NET 4.5 és az SQL-adatbázis](sql-database-develop-direct-route-ports-adonet-v12.md)
* Ügyfél kapcsolatok tooAzure SQL-adatbázis néha hello proxy megkerülése és hello adatbázis közvetlenül kommunikál. Ekkor válnak fontossá az 1433-astól különböző portok. További információ [Azure SQL adatbázis-kapcsolat architektúra](sql-database-connectivity-architecture.md) és [kívüli ADO.NET 4.5 és az SQL-adatbázis 1433-as portokon](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>A rugalmas bővítést adatok horizontális
Rugalmasan méretezhető egyszerűbben hello a Méretezés (és). 

* [Tervezési minták az Azure SQL Database-t használó több-bérlős SaaS-alkalmazásokhoz](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Adatfüggő útválasztás](sql-database-elastic-scale-data-dependent-routing.md)
* [Ismerkedés az Azure SQL Database rugalmas méretezési funkciójának előzetes verziójával](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Következő lépések
Fedezze fel az összes hello [képességek SQL-adatbázis](sql-database-technical-overview.md)
