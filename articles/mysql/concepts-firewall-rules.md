---
title: "Azure-adatbázis a MySQL-kiszolgáló tűzfalszabályainak |} Microsoft Docs"
description: "A témakör ismerteti a tűzfalszabályok a MySQL-kiszolgálóhoz tartozó Azure-adatbázis."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 7456cef7a5da665ee3d70df64265b8186a79f9b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Kiszolgáló tűzfalszabályainak MySQL Azure-adatbázis
Tűzfalak tagadni a hozzáférést minden az adatbázis-kiszolgáló csak akkor adja meg, mely számítógépek rendelkeznek engedéllyel. A tűzfal engedélyezi a hozzáférést a kiszolgálóhoz, az egyes kérelmek az eredeti IP-címe alapján.

A tűzfal konfigurálásakor olyan tűzfalszabályokat adhat meg, amelyek meghatározzák az elfogadható IP-címtartományokat. Tűzfal-szabályokat hozhat létre a kiszolgálói szinten.

**Tűzfal-szabályok:** ezek a szabályok hozzáférés engedélyezése ügyfelek számára a teljes Azure-adatbázis MySQL-kiszolgáló esetén ez azt jelenti, hogy összes adatbázis belül az azonos logikai kiszolgáló. Kiszolgálószintű tűzfal-szabályokat konfigurálhatja az Azure-portál használatával vagy az Azure parancssori felület parancsait. Kiszolgálószintű tűzfal-szabályok létrehozása, az előfizetés tulajdonosa vagy egy előfizetés közreműködői kell lennie.

## <a name="firewall-overview"></a>Tűzfal áttekintése
Az adatbázis elérésére az Azure-adatbázis MySQL-kiszolgáló alapértelmezés szerint a tűzfal blokkolja. A kiszolgáló egy másik számítógépről használatának megkezdéséhez meg kell adnia egy vagy több kiszolgálószintű engedélyezésére szolgáló tűzfalszabályok a kiszolgáló elérését. Használja a tűzfalszabályok adja meg, melyik IP-címtartományok engedélyezi az internetről. A tűzfalszabályok nincs hatással a hozzáférést az Azure portál magát.

Kapcsolódási kísérletek az internetről és az Azure először eltelik a tűzfalon keresztül azok képes elérni az Azure-adatbázis MySQL-adatbázis, az alábbi ábrán látható módon:

![A tűzfal működése áramló – példa](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Csatlakozás az internetről
Az Azure-adatbázishoz a MySQL-kiszolgálón lévő összes adatbázis kiszolgálószintű tűzfal-szabályokat alkalmazni.

Ha a kérés IP-címe a kiszolgálószintű tűzfalszabályokban megadott tartományok egyikében található, a tűzfal engedélyezi a csatlakozást.

Ha az IP-cím, a kérelem nem az adatbázis-szintjére, vagy a kiszolgálószintű tűzfalszabály megadott tartományok belül van, a kapcsolódási kérelem sikertelen lesz.

## <a name="programmatically-managing-firewall-rules"></a>Tűzfalszabályok szoftveres kezelése
Az Azure portálon kívül tűzfalszabályok programozott módon, Azure parancssori Felülettel kezelhetők. Lásd még: [hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felület használatával](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-firewall"></a>Az adatbázistűzfal hibaelhárítása
Amikor a Microsoft Azure-adatbázis eléréséhez a MySQL-kiszolgáló szolgáltatás nem a várt módon működnek várja, vegye figyelembe a következő szempontokat:

* **Az engedélyezési listán módosításai nem lépnek érvénybe még:** lehet, mint amennyit egy 5 perces késleltetést használ az vált, az Azure-adatbázishoz a MySQL-kiszolgáló tűzfal konfigurációjának érvénybe léptetéséhez.

* **A bejelentkezés nem engedélyezett, vagy helytelen jelszót használt:** egy bejelentkezési azonosító nem rendelkezik engedélyekkel a MySQL-kiszolgálóhoz tartozó Azure-adatbázis, vagy a használt jelszó nem megfelelő, ha a kapcsolat a MySQL-kiszolgálóhoz tartozó Azure-adatbázis megtagadva. Egy tűzfalbeállítás létrehozása lehetővé teszi az ügyfelek számára a kiszolgálóhoz való csatlakozás megkísérlését, azonban minden egyes ügyfélnek meg kell adnia a szükséges biztonsági hitelesítő adatokat.

* **Dinamikus IP-cím**: Ha az internetkapcsolata dinamikus IP-címkezeléssel rendelkezik, és problémákat okoz a tűzfalon való átjutás, próbálja ki a következő megoldások valamelyikét:

* Az internetszolgáltató (ISP) kérjen a MySQL-kiszolgálóhoz tartozó Azure-adatbázis elérő ügyfélszámítógépeken rendelt IP-címtartományt, és adja hozzá az IP-címtartományt, egy tűzfalszabályt.

* Állítson be statikus IP-címeket az ügyfélszámítógépei számára, majd adja meg az IP-címeket tűzfalszabályokként.

## <a name="next-steps"></a>Következő lépések

[Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával](./howto-manage-firewall-using-portal.md)
[hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felület használatával](./howto-manage-firewall-using-cli.md)
