---
title: "a kiszolgáló MySQL az Azure-adatbázis aaaHow tooRestore |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toorestore egy kiszolgáló, a Azure-adatbázisban a MySQL használatára vonatkozó hello Azure-portálon."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b990d5b37c5d4924de9571192b923e3c81094ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobackup-and-restore-a-server-in-azure-database-for-mysql-using-hello-azure-portal"></a>Hogyan tooBackup és a kiszolgáló, a Azure-adatbázisban a MySQL használatára vonatkozó visszaállítási hello Azure-portálon

## <a name="backup-happens-automatically"></a>Automatikusan megtörténik a biztonsági mentés
Amikor MySQL az Azure-adatbázist használja, hello adatbázis-szolgáltatás automatikusan teszi hello szolgáltatás biztonsági másolatot, 5 percenként. 

hello biztonsági mentések esetén érhetők el a 7 napja alapszintű rétegben, és 35 napon Standard csomag használata esetén. További információkért lásd: [MySQL szolgáltatási szinteket az Azure-adatbázis](concepts-service-tiers.md)

Az automatikus biztonsági mentési szolgáltatás használatával állítsa vissza hello kiszolgáló és az adatbázisok egy új kiszolgáló tooan korábbi időpontban be.

## <a name="restore-in-hello-azure-portal"></a>Állítsa vissza a hello Azure-portálon
Azure MySQL-adatbázis lehetővé teszi toorestore hello server hátsó tooa pont idő és tooa hello server új példányát. Használhatja az új kiszolgáló toorecover adatait. 

Például ha egy táblázat véletlenül dobva délben ma, akkor sikerült toohello idő előtt déltől visszaállítása, hiányzik a táblázat és adatainak áttelepítését hello kiszolgáló új példányt hello beolvasása.

hello lépések visszaállítási hello minta kiszolgáló tooa pont időpont:

1. Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com/)

2. Keresse meg a MySQL-kiszolgálóhoz tartozó Azure-adatbázis. Hello bal oldali ablaktáblában jelöljön ki **összes erőforrás**, majd válassza ki a kiszolgálót hello listából.

3.  A hello hello server – áttekintés panel felső részén kattintson **visszaállítása** hello eszköztáron. hello visszaállítás panel nyílik meg.
![Visszaállítás gombra.](./media/howto-restore-server-portal/click-restore-button.png)

4. Töltse ki hello visszaállítási űrlapot hello szükséges információkkal:

- **Visszaállítási pont (idő szerint UTC)**: hello Dátumválasztó és idő kiválasztása jelöljön ki egy időpontban toorestore számára. hello megadott ideje UTC formátumban, ezért az UTC valószínűleg kell tooconvert hello helyi idő.
- **Toonew-kiszolgáló helyreállítása**: Adjon meg egy új kiszolgálót neve toorestore hello meglévő kiszolgálójáról.
- **Hely**: hello régió automatikusan hello forrás server régió tölti fel, és nem módosítható.
- **IP-címek**: hello automatikusan árképzési szint választott tölti fel hello azonos árképzési hello forráskiszolgáló réteg, és itt nem módosíthatók. 
![PITR visszaállítása](./media/howto-restore-server-portal/pitr-restore.png)

5. Kattintson a **OK** toorestore hello server toorestore tooa pont időben. 

6. Hello visszaállítás befejezése után keresse meg a hello tooverify hello adatbázisok volt visszaállítása az elvártaknak megfelelően létrehozott új kiszolgálót.

## <a name="next-steps"></a>Következő lépések
- [Adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)