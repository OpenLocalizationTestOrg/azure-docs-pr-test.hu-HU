---
title: "aaaCreate és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával |} Microsoft Docs"
description: "Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 30dbdde4299df564fbf6581419e908186fe3b823
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-hello-azure-portal"></a>Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával
Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák tooaccess egy Azure-adatbázis MySQL-kiszolgáló a megadott IP-cím vagy az IP-címek. 

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon

1. Hello MySQL server panelen, a beállítások elemcsoportban kattintson **kapcsolatbiztonsági** tooopen hello kapcsolatbiztonsági panel hello MySQL az Azure-adatbázis.

   ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Kattintson a **hozzáadása a saját IP** a hello eszköztár toocreate egy szabály a számítógép, mivel hello Azure rendszer által érzékelt hello IP-címmel.

   ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Ellenőrizze az IP-címek hello konfiguráció mentése előtt. Bizonyos esetekben az Azure portál által megfigyelt hello IP-címe eltér használt hello IP-cím mikor fér hozzá hello a internetes és az Azure-kiszolgálók esetén. Ezért szükség lehet toochange hello kezdő IP- és a záró IP-toomake hello szabály függvény várt módon.

   Használjon egy keresőmotor vagy más online eszköz toocheck a saját IP-címet (például keresés "Mi az az IP-cím").

   ![Mi az a saját IP Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Adjon hozzá további címtartományok. Hello Azure adatbázis MySQL tűzfal hello szabályait megadhatja a egyetlen IP-címet, vagy címtartományokat. Ha azt szeretné, hogy toolimit hello szabály tooone egyetlen IP-címet, ugyanaz az kezdő IP- és a záró IP-cím hello mező típusa hello. Hello tűzfal megnyitása lehetővé teszi a rendszergazdák és felhasználók tooaccess bármely adatbázis hello MySQL server toowhich rendelkeznek érvényes hitelesítő adatokat.

   ![Azure portál – tűzfalszabályok ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Kattintson a **mentése** a hello eszköztár toosave a kiszolgálószintű tűzfalszabály. Várja meg, hogy sikeres volt-e hello frissítés toohello tűzfalszabályok hello megerősítése.

   ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a>Hello Azure-portálon keresztül meglévő kiszolgálószintű tűzfal-szabályok kezelése
Ismételje meg a hello lépéseket toomanage hello tűzfalszabályokat.
* tooadd hello aktuális számítógépet, kattintson a **+ saját IP-cím hozzáadása**.
* tooadd további IP-címeket, írja be a hello **SZABÁLYNÉV**, **KEZDŐ IP-**, és **záró IP-Címnél**.
* egy meglévő szabály toomodify kattintson valamelyik hello mezők hello szabályban, és módosítsa.
* egy meglévő toodelete szabály, hello három pont [...] kattintson, majd **törlése**.
* Kattintson a **mentése** toosave hello módosításokat.

## <a name="next-steps"></a>Következő lépések
- A csatlakozás az Azure-adatbázis tooan MySQL-kiszolgáló, lásd: [adatkapcsolattárak MySQL az Azure-adatbázis](./concepts-connection-libraries.md)
