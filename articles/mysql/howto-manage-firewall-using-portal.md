---
title: "Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával |} Microsoft Docs"
description: "Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 33198e5a6e11df2db3a17fc96a0b3cd4b1a284e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-the-azure-portal"></a>Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával
Kiszolgálószintű tűzfal-szabályok lehetővé teszik a rendszergazdák az Azure-adatbázisának eléréséhez MySQL-kiszolgáló a megadott IP-cím vagy az IP-címek. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Kiszolgálószintű tűzfalszabály létrehozása az Azure Portalon

1. A MySQL server, a beállítások panelen elemcsoportban kattintson **kapcsolatbiztonsági** kapcsolatbiztonsági panel megnyitásához az Azure-adatbázis a MySQL.

   ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Kattintson a **hozzáadása a saját IP** olyan szabály létrehozására a számítógép IP-címmel, mivel az Azure rendszer által érzékelt az eszköztáron.

   ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Az IP-cím ellenőrzése a konfiguráció mentéséhez. Bizonyos esetekben az IP-cím, az Azure portál által megfigyelt eltér az internetes és az Azure-kiszolgálók eléréséhez használt IP-címét. Emiatt előfordulhat, hogy módosítani szeretné a kezdő IP- és a záró IP-, a várt módon szabály függvény végrehajtásához.

   Egy keresőmotor vagy más online eszköz használatával ellenőrizze a saját IP-cím (például keresés "Mi az az IP-cím").

   ![Mi az a saját IP Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Adjon hozzá további címtartományok. Az Azure-adatbázishoz MySQL tűzfal szabályait megadhatja a egyetlen IP-címet, vagy címtartományokat. Ha szeretné korlátozni a szabály, amely egy egyetlen IP-címet, írja be ugyanazt a címet a mezőben a kezdő IP- és a záró IP. Megnyitásáról a tűzfal lehetővé teszi a rendszergazdák és a felhasználók számára bármely adatbázis rendelkeznek érvényes hitelesítő adatokat a MySQL-kiszolgálón.

   ![Azure portál – tűzfalszabályok ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Kattintson a **mentése** az eszköztáron menteni a kiszolgálószintű tűzfalszabály. Várja meg, hogy a tűzfalszabályok frissítése sikeres volt-e a jóváhagyást.

   ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Meglévő kiszolgálószintű tűzfalszabályok kezelése az Azure Portalon
Ismételje meg a tűzfal-szabályok kezelése.
* Az aktuális számítógép hozzáadásához kattintson **+ saját IP-cím hozzáadása**.
* További IP-cím hozzáadásához írja be a **SZABÁLYNÉV**, **KEZDŐ IP-**, és **záró IP-Címnél**.
* Meglévő szabály módosításához kattintson a szabály valamelyik mezőjére, és adja meg a módosításokat.
* Meglévő szabály törléséhez kattintson a három pont [...], és kattintson a **törlése**.
* Kattintson a **Mentés** gombra a módosítások mentéséhez.

## <a name="next-steps"></a>Következő lépések
- A MySQL-kiszolgáló egy Azure-adatbázishoz szeretne csatlakozni, lásd: [adatkapcsolattárak MySQL az Azure-adatbázis](./concepts-connection-libraries.md)
