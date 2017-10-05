---
title: "Adatbázis alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés |} Microsoft Docs"
description: "Bemutatja, hogy a fejlesztő kell követnie, MySQL Azure adatbázishoz való kapcsolódáshoz alkalmazáskód írásakor kialakítási szempontok"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 350dd775e172120d806d1193877a34d94f4d3f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés 
Ez a cikk ismerteti, hogy a fejlesztő kell követnie, MySQL Azure adatbázishoz való kapcsolódáshoz alkalmazáskód írásakor kialakítási szempontok 

> [!TIP]
> Az oktatóanyag bemutatja, hogyan hozzon létre egy kiszolgálót, hozzon létre egy kiszolgáló-alapú tűzfal, kiszolgáló tulajdonságainak megtekintése, adatbázis létrehozása, csatlakozás és lekérdezés munkaterület és mysql.exe használatával, lásd: [tervezése az első Azure-beli MySQL database](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Nyelv és platform
Különböző programozási nyelvekhez és platformokhoz érhetők el kódminták. A következő mintakódok hivatkozásait megtalálhatja: [MySQL Azure adatbázishoz való kapcsolódáshoz használt kapcsolódási függvénytárak](concepts-connection-libraries.md)

## <a name="tools"></a>Eszközök
Azure MySQL-adatbázis MySQL közösségi verzióját használja, MySQL munkaterület vagy MySQL segédprogramok mysql.exe, mint például az általános kezelőeszközöket kompatibilis [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), stb. Az Azure portál, az Azure CLI és a REST API-k használatával is kommunikálni az adatbázis-szolgáltatás.

## <a name="resource-limitations"></a>Erőforrás-korlátozások
Azure-beli MySQL adatbázis két különböző mechanizmusok használatával egy kiszolgáló számára elérhető erőforrások kezelése: 
- Erőforrások irányítás 
- A korlátozások érvényesítése.

## <a name="security"></a>Biztonság
Azure-beli MySQL adatbázis korlátozó hozzáférési, a védelmet nyújtó adatokat, a konfigurálását a felhasználók és a szerepkör és a figyelési tevékenységek a MySQL-adatbázis forrásokat biztosít.

## <a name="authentication"></a>Authentication
Azure-beli MySQL adatbázis támogatja a kiszolgáló hitelesítése a felhasználók és bejelentkezések.

## <a name="resiliency"></a>Resiliency
Amikor egy átmeneti hiba akkor fordul elő, amikor kapcsolódni próbált MySQL-adatbázis, a kódot kell próbálja meg újra a hívást. Javasoljuk az újrapróbálkozási logika használata készítsen biztonsági logika, így azt nem ne terhelje tovább az SQL-adatbázis az újrapróbálkozás egyszerre több ügyfélnek.

- Kódminták: mintakódok, mely újrapróbálkozási logika, lásd: a következő nyelvű-példák: [MySQL Azure adatbázishoz való kapcsolódáshoz használt kapcsolódási függvénytárak](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Kapcsolatok kezelése
Adatbázis-kapcsolatok egy korlátozott erőforrás, a jobb teljesítmény érdekében a MySQL-adatbázis elérésekor kapcsolat ésszerű használata javasolt.
- Kapcsolatkészletezést vagy állandó kapcsolat használatával érik el az adatbázist.
- Rövid kapcsolat élettartamát használatával érik el az adatbázist. 
- Újrapróbálkozási logika helyén a kapcsolódási kísérlet az alkalmazás használatához van szüksége a hiba oka, hogy az egyidejű kapcsolatok száma elérte a megengedett. Az újrapróbálkozási logika be egy rövid ideig eltart, majd várjon véletlenszerű ideje előtt a további csatlakozási kísérletek.