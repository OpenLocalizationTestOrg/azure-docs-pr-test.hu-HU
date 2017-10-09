---
title: "aaaCreate és kezelheti az Azure-adatbázis MySQL kiszolgáló Azure-portál használatával |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan gyorsan létrehozhat egy új Azure-adatbázist a MySQL-kiszolgáló és hello Azure portál használatával hello kiszolgáló kezeléséhez."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c532df43b3d2124d7bd34875b32d52357f162af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Létrehozása és kezelése az Azure-adatbázis MySQL-kiszolgáló Azure-portál használatával
Ez a cikk ismerteti, hogyan gyorsan létrehozhat egy új Azure-adatbázist a MySQL-kiszolgáló és hello Azure portál használatával hello kiszolgáló kezeléséhez. Kiszolgáló kezelése tartalmaz kiszolgálóadatok & adatbázisok megtekintéséhez, a jelszó alaphelyzetbe állítása és a hello kiszolgáló törlése.

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon
Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Azure-adatbázis létrehozása MySQL-kiszolgálóhoz
Kövesse ezeket a lépéseket toocreate egy "mysqlserver4demo" nevű MySQL-kiszolgálóhoz tartozó Azure-adatbázis

1 kattintással **új** hello bal felső sarkában hello Azure-portálon található gombra.

2-válassza **adatbázisok** hello oldalát, és válassza ki a **MySQL az Azure-adatbázis** hello adatbázisok oldalról.

> Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis jön létre egy adott csoportján [számítási és tárolási](./concepts-compute-unit-and-storage.md) erőforrásokat. hello adatbázis egy Azure erőforráscsoporton belül, és a MySQL-kiszolgáló egy Azure-adatbázis létrehozása.

![– új-kiszolgáló létrehozása](./media/howto-create-manage-server-portal/create-new-server.png)

3 - hello Azure adatbázis MySQL űrlap a következő információ hello kitöltötte:

| **Űrlapmező** | **Mező leírása** |
|----------------|-----------------------|
| *Kiszolgálónév* | Azure-beli mysql (a kiszolgáló neve, globálisan egyedi) |
| *Előfizetés* | MySQLaaS (válassza ki a legördülő lista) |
| *Erőforráscsoport* | erőforrás (hozzon létre egy új erőforráscsoportot, vagy használjon egy meglévőt) |
| *Kiszolgálói rendszergazdai bejelentkezés* | myadmin (állítsa be a rendszergazdafiók nevét) |
| *Jelszó* | a telepítő rendszergazda fiók jelszava |
| *Jelszó megerősítése* | erősítse meg a rendszergazdafiók jelszavát |
| *Hely* | Észak-Európa (válassza ki a Észak-Európában és USA nyugati régiója között) |
| *Verzió* | 5.6 (Azure-adatbázis kiválasztása a MySQL-kiszolgáló verziója) |

4 kattintással **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új kiszolgáló. Számítási egység közötti 50-100 alapszintű rétegben, 100, 200 normál rétegben, konfigurálhatók, és adható tárterület belefoglalt mennyisége alapján. Ez az útmutató útmutató most válassza 50 számítási egység és 50GB. Kattintson a **OK** toosave a kijelölés.
![Hozzon létre-kiszolgáló--tarifacsomagot](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5 kattintással **létrehozása** tooprovision hello kiszolgáló. Az üzembe helyezés eltarthat néhány percig.

> Ellenőrizze a hello **PIN-kód toodashboard** beállítás tooallow könnyű nyomon a központi telepítés.
> [!NOTE]
> Bár az alapszintű rétegben too1000GB és 10000GB, a Standard szint is támogatottak lesznek a tároláshoz, Public Preview hello maximális tárolási ideiglenesen továbbra is korlátozott too1000GB. 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis frissítése
Új kiszolgáló üzembe helyezése után felhasználó rendelkezik-e 2 beállítások tooedit egy meglévő kiszolgálóról: alaphelyzetbe állítja a rendszergazdai jelszót vagy skálája fel/le hello server hello számítási – egység módosításával.

### <a name="change-hello-administrator-user-password"></a>Hello rendszergazda felhasználói jelszó módosítása
1 - kiszolgálón hello **áttekintése** panelen kattintson a **jelszó-átállítási** toopopulate egy jelszó bemeneti és a megerősítő ablakot.

2 - hello jelszó hello ablakban az alábbi, új jelszó megadására és megerősítésére: ![jelszó alaphelyzetbe állítása](./media/howto-create-manage-server-portal/reset-password.png)

3 kattintással **OK** toosave hello új jelszót.

### <a name="scale-updown-by-changing-compute-units"></a>Fel/le méretezhető számítási egység módosítása

1 – a hello server panelen a **beállítások**, kattintson a **tarifacsomag** tooopen hello árazás réteg panel az Azure-adatbázis hello MySQL-kiszolgáló.

4 lépés 2 követhető **MySQL-kiszolgáló Azure-adatbázis létrehozása** toochange számítási egység hello azonos IP-címek.

## <a name="delete-an-azure-database-for-mysql-server"></a>Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis törlése

1 - kiszolgálón hello **áttekintése** panelen kattintson a **törlése** parancs gomb tooopen hello törlése megerősítő panelen.

2-típus hello helyes kiszolgálónevet hello panelről dupla megerősítés beviteli mezőbe.

3 kattintással **törlése** tooconfirm törlésekor a művelet újra gombra, és várja meg, "Törlése sikeres" előugró hello értesítési sáv.

## <a name="list-hello-azure-database-for-mysql-databases"></a>Lista hello Azure-adatbázis a MySQL-adatbázisok
Hello kiszolgálón **áttekintése** panelen görgessen lefelé, amíg megjelenik a hello adatbázis hello alsó csempét. Összes hello adatbázis hello táblázatban jelennek meg. Kattintson a **törlése** parancs gomb tooopen hello törlése megerősítő panelen.

![megjelenítése-adatbázisok](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Egy Azure-adatbázis MySQL-kiszolgáló részleteinek megjelenítése
Kattintson a **tulajdonságok** alatt **beállítások** hello kiszolgálón panel nyílik hello **tulajdonságok** panelen. Majd hello kiszolgálóval kapcsolatos összes részletes információk is megtekinthetők.

## <a name="next-steps"></a>Következő lépések

[Gyors üzembe helyezés: Azure-adatbázis létrehozása a MySQL-kiszolgáló Azure-portál használatával](./quickstart-create-mysql-server-database-using-azure-portal.md)
