---
title: "aaaTroubleshoot közös kapcsolódási problémák tooAzure SQL-adatbázis"
description: "Lépéseket tooidentify és hárítsa el közös csatlakozási hibáinak az Azure SQL Database."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: ac463d1c-aec8-443d-b66e-fa5eadcccfa8
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: eb5f2d7b123a76942c7e4a84a7f475344fbcb144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-connection-issues-tooazure-sql-database"></a>Kapcsolódási problémák tooAzure SQL-adatbázis hibáinak elhárítása
Amikor hello kapcsolat tooAzure SQL-adatbázis nem sikerül, [hibaüzenetek](sql-database-develop-error-messages.md). Ez a cikk központi Ez a témakör segítséget nyújt az Azure SQL Database kapcsolódási problémáinak elhárítása. Okozna [általános okok hello](#cause) kapcsolódási problémák javasolja [hibaelhárításhoz](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , amely segít az identitás hello problémát, és hibaelhárítási lépéseket toosolve biztosít [ átmeneti hibák](#troubleshoot-transient-errors) és [állandó vagy nem átmeneti hiba](#troubleshoot-persistent-errors). 

Ha hibát tapasztal hello kapcsolat, próbáljon hello hibaelhárítása a cikkben ismertetett lépéseket.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Ok
A következő hello oka lehet kapcsolódási problémák léptek fel:

* Hiba tooapply ajánlott eljárások és tervezési útmutató hello alkalmazás tervezési folyamat során.  Lásd: [SQL adatbázis-fejlesztői áttekintés](sql-database-develop-overview.md) tooget elindult.
* Az Azure SQL Database újrakonfigurálása
* Tűzfalbeállítások
* A kapcsolat időkorlátja
* Helytelen bejelentkezési adatok
* Az egyes Azure SQL Database-erőforrásokat elérte a maximális korlátot

Általában kapcsolódási problémák tooAzure SQL-adatbázis besorolhatók az alábbiak szerint:

* [Átmeneti hibák (rövid élettartamú vagy időszakos)](#troubleshoot-transient-errors)
* [Állandó nem átmeneti hiba (hibák, rendszeresen ismétlődő)](#troubleshoot-persistent-errors)

## <a name="try-hello-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Próbálja ki az Azure SQL adatbázis-csatlakozási problémák hello hibaelhárítót
Ha egy adott kapcsolat hiba merül fel, próbálkozzon [ezzel az eszközzel](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), amely segítségével gyorsan identitás és a probléma megoldásához.

## <a name="troubleshoot-transient-errors"></a>Átmeneti hibáinak elhárítása

Az alkalmazások tooan Azure SQL adatbázis csatlakoznak, a hello a következő hibaüzenet jelenhet meg:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry hello connection later. If hello problem persists, contact customer support, and provide them hello session tracing ID of <z>"
```

> [!NOTE]
> Ez a hibaüzenet akkor általában átmeneti jellegű (rövid élettartamú).
> 
> 

Ez a hiba akkor fordul elő, amikor hello folyamatban van az Azure-adatbázis áthelyezése (vagy újrakonfigurálása) és az alkalmazás megszakad a kapcsolat toohello SQL-adatbázis. SQL adatbázis újrakonfigurálás-események esetén a tervezett események (például szoftverfrissítésre) vagy egy nem tervezett esemény (például egy folyamat összeomlási, vagy a terheléselosztás) miatt. A legtöbb újrakonfigurálás események általában rövid élettartamú, és be kell 60 másodpercnél nagyobb, mint. Azonban ezek az események időnként eltarthat hosszabb toofinish, például ha a nagy tranzakció a hosszan futó helyreállítás miatt.

### <a name="steps-tooresolve-transient-connectivity-issues"></a>Lépéseket tooresolve átmeneti problémák

1. Ellenőrizze a hello [Microsoft Azure szolgáltatás irányítópultját](https://azure.microsoft.com/status) a bármely ismert kimaradások melyik hello során jelenített hello alkalmazás hello időszakban történt.
2. Alkalmazások, amelyek kapcsolódnak tooa felhőalapú szolgáltatás, például az Azure SQL Database kell rendszeres újrakonfigurálás események várható és megvalósításához logika toohandle ezeket a hibákat a felszínre hozhatják alkalmazás hibák toousers, ezek helyett próbálkozzon újra. Felülvizsgálati hello [átmeneti hibák](sql-database-connectivity-issues.md) szakasz és hello ajánlott eljárások és tervezési irányelveket: [SQL adatbázis-fejlesztői áttekintés](sql-database-develop-overview.md) további információkat, valamint általános próbálja meg újból a stratégiák. Majd, tekintse meg a kódot minták [adatkapcsolattárak SQL Database és SQL Server](sql-database-libraries.md) a részletekért.
3. Egy adatbázis megközelíti a teljes erőforrás kapacitásukkal működjenek, mivel azt is úgy tűnik, toobe egy átmeneti kapcsolati probléma. Lásd: [teljesítménnyel kapcsolatos problémák elhárításakor](sql-database-troubleshoot-performance.md).
4. Ha kapcsolódási problémái vannak folytatásához, vagy ha hello, amelynek az alkalmazás hello hiba lép fel, mint 60 másodperc, vagy ha egy adott napon többször hello hiba jelenik meg, az Azure támogatási kérelmet fájl kiválasztásával **beolvasásatámogatja.** a hello [Azure támogatási](https://azure.microsoft.com/support/options) hely.

## <a name="troubleshoot-persistent-errors"></a>Állandó hibák elhárítása
Hello alkalmazás tartósan tooconnect tooAzure SQL-adatbázis nem sikerül, ha problémát hello következő általában jelzi:

* Tűzfal-konfiguráció. hello Azure SQL database vagy az ügyféloldali tűzfal blokkolja a kapcsolatok tooAzure SQL-adatbázis.
* Hálózati hello ügyféloldalon újrakonfigurálása: például egy új IP-címet vagy proxykiszolgálón.
* Felhasználói hiba történt: például elírt kapcsolat paraméterek, például hello kiszolgálónév hello kapcsolati karakterláncban.

### <a name="steps-tooresolve-persistent-connectivity-issues"></a>Lépéseket tooresolve állandó csatlakozási problémák
1. Állítson be [tűzfal-szabályok](sql-database-configure-firewall-settings.md) tooallow hello ügyfél IP-címét. Ideiglenes tesztelési célokra 0.0.0.0 használata IP-címtartomány kezdő- és 255.255.255.255 használja, mint a záró IP-címtartomány hello hello tűzfal-szabály beállítására. Megnyílik a hello tooall IP-címeket. Ha a hálózati probléma így megoldódik, ez a szabály eltávolítása, és hozzon létre egy tűzfalszabályt egy megfelelően korlátozott IP-cím vagy a címtartományt. 
2. Hello ügyfél és a hello Internet közötti összes tűzfalnak ellenőrizze, hogy a 1433-as port kimenő kapcsolatok nyitva-e. Felülvizsgálati [hello Windows tűzfal tooAllow SQL Server-hozzáférés konfigurálása](https://msdn.microsoft.com/library/cc646023.aspx) és [hibrid identitás szükséges portok és protokollok](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports) további mutatók kapcsolódó tooadditional portok, amely a tooopen kell Az Azure Active Directory-hitelesítés.
3. Ellenőrizze a kapcsolati karakterlánc és más csatlakozási beállításait. Lásd: hello hello kapcsolati karakterlánc szakasz [kapcsolódási problémák a témakör](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4. Ellenőrizze a szolgáltatás állapotát hello irányítópulton. Ha úgy gondolja, hogy nincs regionális kimaradás, lásd: [kimaradás helyreállíthatók](sql-database-disaster-recovery.md) lépéseket toorecover tooa új régióhoz.

## <a name="next-steps"></a>Következő lépések
* [Az Azure SQL Database teljesítménnyel kapcsolatos problémák elhárítása](sql-database-troubleshoot-performance.md)
* [Keresési hello dokumentáció a Microsoft Azure](http://azure.microsoft.com/search/documentation/)
* [Nézet hello legújabb frissítések toohello Azure SQL Database szolgáltatásban](http://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>További források
* [SQL adatbázis-fejlesztői áttekintés](sql-database-develop-overview.md)
* [Általános átmeneti hiba-kezelési útmutatás](../best-practices-retry-general.md)
* [Adatkapcsolattárak SQL Database és SQL Server](sql-database-libraries.md)

