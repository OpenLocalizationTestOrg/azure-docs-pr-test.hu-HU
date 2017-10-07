---
title: "aaaDatabase alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés |} Microsoft Docs"
description: "Bemutatja, hogy a fejlesztő kell követnie, MySQL alkalmazás kód tooconnect tooAzure adatbázis írásakor kialakítási szempontok"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: f08df605eba21b4ba4b43565c0a7ded95779a171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Alkalmazások fejlesztése MySQL az Azure-adatbázis – áttekintés 
Ez a cikk ismerteti, hogy a fejlesztő kell követnie, MySQL alkalmazás kód tooconnect tooAzure adatbázis írásakor kialakítási szempontok 

> [!TIP]
> Egy oktatóanyag megjelenítő, hogyan toocreate egy kiszolgálót, hozzon létre egy kiszolgáló-alapú tűzfal kiszolgáló tulajdonságainak megtekintése, létrehozásakor adatbázis, kapcsolódás és lekérdezés munkaterület és mysql.exe használatával, lásd: [tervezése az első Azure-beli MySQL database](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Nyelv és platform
Különböző programozási nyelvekhez és platformokhoz érhetők el kódminták. Hivatkozásait megtalálhatja toohello kód minták: [kapcsolat szalagtárak használt tooconnect tooAzure adatbázis MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Eszközök
Azure MySQL-adatbázis hello MySQL közösségi telepített verzióját használja, MySQL munkaterület vagy MySQL segédprogramok mysql.exe, mint például az általános kezelőeszközöket kompatibilis [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), stb. Hello Azure-portálon, az Azure CLI és a REST API-k toointeract hello adatbázis szolgáltatással is használja.

## <a name="resource-limitations"></a>Erőforrás-korlátozások
Azure-beli MySQL adatbázis hello erőforrások elérhető tooa kiszolgálói kétféle különböző módon kezelő: 
- Erőforrások irányítás 
- A korlátozások érvényesítése.

## <a name="security"></a>Biztonság
Azure-beli MySQL adatbázis korlátozó hozzáférési, a védelmet nyújtó adatokat, a konfigurálását a felhasználók és a szerepkör és a figyelési tevékenységek a MySQL-adatbázis forrásokat biztosít.

## <a name="authentication"></a>Authentication
Azure-beli MySQL adatbázis támogatja a kiszolgáló hitelesítése a felhasználók és bejelentkezések.

## <a name="resiliency"></a>Resiliency
Amikor egy átmeneti hiba akkor fordul elő, tooMySQL adatbázis kapcsolódás közben, a kódot újra kell hello hívás. Hello újrapróbálkozási logika használata leállás logikát, azt javasoljuk, így azt nem ne terhelje tovább hello SQL-adatbázis az újrapróbálkozás egyszerre több ügyfélnek.

- Kódminták: mintakódok, mely újrapróbálkozási logika, lásd: hello nyelvű-példák: [kapcsolat szalagtárak használt tooconnect tooAzure adatbázis MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Kapcsolatok kezelése
Adatbázis-kapcsolatok egy korlátozott erőforrás, ezért azt javasoljuk kapcsolat ésszerű használata a MySQL-adatbázis elérésekor tooachieve jobb teljesítményt.
- Hello adatbázist kapcsolatkészletezést vagy állandó kapcsolat használatával.
- Hello adatbázist élettartamát rövid kapcsolat használatával. 
- Használja újrapróbálkozási logika hello csatlakozási kísérlet, toocatch hibák miatt tooconcurrent kapcsolatok hello ponton az alkalmazás elérte a megengedett maximális hello. A hello újrapróbálkozási logika, rövid késleltetés beállítása és majd várjon, amíg egy véletlenszerű idő előtt hello további csatlakozási kísérletek.
