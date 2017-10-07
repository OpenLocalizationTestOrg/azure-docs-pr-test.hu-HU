---
title: "aaaCreate és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával |} Microsoft Docs"
description: "Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6a41a077168657769e442401e9df9931aa809240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-hello-azure-portal"></a>Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával
Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák tooaccess egy Azure-adatbázis PostgreSQL-kiszolgáló a megadott IP-cím vagy az IP-címek. 

## <a name="prerequisites"></a>Előfeltételek
Ez hogyan tooguide keresztül toostep, lesz szüksége:
- A kiszolgáló [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon
1. Hello PostgreSQL server panelen, a beállítások elemcsoportban kattintson **kapcsolatbiztonsági** tooopen hello kapcsolatbiztonsági panel hello PostgreSQL az Azure-adatbázis.

  ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Kattintson a **hozzáadása a saját IP** hello eszköztáron. Ekkor létrejön egy szabály automatikusan a számítógép, mint hello Azure rendszer által észlelt hello IP-címmel.

  ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Ellenőrizze az IP-címek hello konfiguráció mentése előtt. Bizonyos esetekben az Azure portál által megfigyelt hello IP-címe eltér használt hello IP-cím mikor fér hozzá hello a internetes és az Azure-kiszolgálók esetén. Ezért szükség lehet toochange hello kezdő IP- és a záró IP-toomake hello szabály függvény várt módon.
Egy keresőmotor vagy más online eszköz toocheck a saját IP-cím (például a Bing keresési "Mi az a saját IP") használja.

  ![Mi az a saját IP Bing keresése](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Adjon hozzá további címtartományok. Hello Azure adatbázis PostgreSQL tűzfal hello szabályait megadhatja a egyetlen IP-címet, vagy címtartományokat. Ha azt szeretné, hogy toolimit hello szabály tooone egyetlen IP-címet, ugyanaz az kezdő IP- és a záró IP-cím hello mező típusa hello. Hello tűzfal megnyitása lehetővé teszi, hogy a rendszergazdák és felhasználók toologin tooany adatbázis hello PostgreSQL server toowhich rendelkeznek érvényes hitelesítő adatokat.

  ![Azure portál – tűzfalszabályok ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Kattintson a **mentése** a hello eszköztár toosave a kiszolgálószintű tűzfalszabály. Várja meg, hogy sikeres volt-e hello frissítés toohello tűzfalszabályok hello megerősítése.

  ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-hello-azure-portal"></a>Hello Azure-portálon keresztül meglévő kiszolgálószintű tűzfal-szabályok kezelése
Ismételje meg a hello lépéseket toomanage hello tűzfalszabályokat.
* tooadd hello aktuális számítógépet, gombra hello túl + **hozzáadása a saját IP**. Kattintson a **mentése** toosave hello módosításokat.
* a hello tooadd további IP-címeket, írja be a szabály nevét, a kezdő IP-címe és a záró IP-cím. Kattintson a **mentése** toosave hello módosításokat.
* egy meglévő szabály toomodify kattintson valamelyik hello mezők hello szabályban, és módosítsa. Kattintson a **mentése** toosave hello módosításokat.
* toodelete egy meglévő szabály hello három pont [...] kattintson, majd távolítsa el a hello szabály törlése. Kattintson a **mentése** toosave hello módosításokat.

## <a name="next-steps"></a>Következő lépések
- Hasonlóképpen, akkor lehet parancsprogramot futtatni a túl[hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felület használatával](howto-manage-firewall-using-cli.md)
- Az Azure-adatbázis tooan PostgreSQL-kiszolgálóhoz csatlakozó útmutatásért lásd: [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)
