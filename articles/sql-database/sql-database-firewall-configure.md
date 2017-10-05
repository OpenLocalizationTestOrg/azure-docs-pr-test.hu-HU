---
title: "Az Azure SQL adatbázis tűzfalszabályainak |} Microsoft Docs"
description: "Ismerje meg, hogyan konfigurálhat egy SQL Database-tűzfalat a hozzáférések kezelése céljából kiszolgálói és adatbázisszintű tűzfalszabályokkal."
keywords: "adatbázistűzfal"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 04/10/2017
ms.author: rickbyh
ms.openlocfilehash: 583c91376418d20d34db17d57d3fa14a1e71cd3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sql-database-server-level-and-database-level-firewall-rules"></a>Az Azure SQL Database kiszolgáló- és adatbázis tűzfalszabályok 

A Microsoft Azure SQL Database egy relációs adatbázis-szolgáltatást nyújt az Azure és egyéb internetalapú alkalmazások számára. Az adatok védelme érdekében a tűzfalak mindaddig megakadályozzák az adatbázis-kiszolgáló elérését, amíg meg nem adja, hogy mely számítógépek rendelkeznek ehhez engedéllyel. A tűzfal biztosítja az adatbázisokhoz való hozzáférést az egyes kérések kiindulási IP-címe alapján.

## <a name="overview"></a>Áttekintés

Kezdetben a tűzfal az Azure SQL Server összes Transact-SQL-hozzáférését blokkolja. Az Azure SQL server használatának megkezdéséhez meg kell adnia egy vagy több kiszolgálószintű tűzfalszabályt, amely engedélyezi az Azure SQL-kiszolgáló eléréséhez. A tűzfalszabályok segítségével adja meg, hogy az internet felől mely IP-címtartományok engedélyezettek, valamint hogy az Azure-alkalmazások megpróbálhatnak-e csatlakozni az Azure SQL Serverhez.

Ha az Azure SQL Serveren csak egyetlen adatbázishoz szeretne hozzáférést biztosítani, akkor az adott adatbázishoz egy adatbázisszintű szabályt kell létrehoznia. Adjon meg egy olyan IP-címtartományt az adatbázis tűzfalszabályához, amely a kiszolgálószintű tűzfalszabályban megadott IP-címtartományon kívül van, és ügyeljen arra, hogy az ügyfél IP-címe az adatbázisszintű szabályban megadott tartományon belülre essen.

Az internetről és az Azure-ból érkező kapcsolódási kísérleteknek először át kell jutniuk a tűzfalon, mielőtt elérnék az Azure SQL Servert vagy az SQL Database-t, ahogyan az a következő ábrán látható:

   ![A tűzfal-konfigurációt bemutató ábra.][1]

* **Kiszolgálószintű tűzfalszabályok:** Ezek a szabályok hozzáférést biztosítanak az ügyfelek számára a teljes Azure SQL Serverhez, azaz az egyazon logikai kiszolgálón található összes adatbázishoz. Ezek a szabályok a **fő** adatbázisban vannak tárolva. A kiszolgálószintű tűzfalszabályok a portálon vagy Transact-SQL utasításokkal konfigurálhatók. Az Azure Portal vagy a PowerShell használatával csak az előfizetés tulajdonosa vagy az előfizetés közreműködői hozhatnak létre kiszolgálószintű tűzfalszabályokat. Ha kiszolgálószintű tűzfalszabályt kíván létrehozni a Transact-SQL használatával, akkor kiszolgálószintű fő bejelentkezőként vagy az Azure Active Directory rendszergazdájaként kell csatlakoznia az SQL Database-példányhoz (ez egyben azt is jelenti, hogy először kiszolgálószintű tűzfalszabályt kell létrehoznia egy Azure-szintű engedélyekkel rendelkező felhasználóval).
* **Adatbázis-adatbázisszintű tűzfalszabályok:** ezek a szabályok engedélyezi az ügyfelek bizonyos (biztonságos) adatbázisok belül az azonos logikai kiszolgáló eléréséhez. Hozhatja létre ezeket a szabályokat az egyes adatbázisok (beleértve a **fő** database0) és az egyes adatbázisok tárolódnak. Adatbázis-szintű tűzfalszabályok csak akkor lehet Transact-SQL-utasítások segítségével konfigurált csak az első kiszolgálószintű tűzfal konfigurálása után. Ha egy olyan IP-címtartományt ad meg az adatbázisszintű tűzfalszabályban, amely kívül esik a kiszolgálószintű tűzfalszabályban megadott tartományon, akkor csak az adatbázisszinten megadott tartományba eső IP-címekkel rendelkező ügyfelek érhetik el az adatbázist. Egy adatbázishoz legfeljebb 128 adatbázisszintű tűzfalszabály adható meg. Adatbázis-szintű tűzfalszabályok fő- és felhasználói adatbázisok csak hozható létre és kezelhetők a Transact-SQL. Az adatbázis-szintű tűzfalszabályok beállítása további információkért lásd: a példa későbbi részében a következő cikket, és tekintse meg [sp_set_database_firewall_rule (Azure SQL-adatbázisok)](https://msdn.microsoft.com/library/dn270010.aspx).

**Javaslat:** A Microsoft a biztonság és az adatbázis hordozhatóságának fokozása érdekében adatbázisszintű tűzfalszabályok használatát javasolja, amikor erre lehetőség van. Kiszolgálószintű tűzfalszabályokat használhat a rendszergazdákra vonatkozóan, vagy ha több, azonos hozzáférési követelményt támasztó adatbázissal rendelkezik, és nem szeretne időt tölteni az egyes adatbázisok egyenként való konfigurálásával.

> [!Note]
> Az üzletmenet folytonossága és a hordozható adatbázisok közötti kapcsolatról [a vészhelyreállítás hitelesítési követelményeit](sql-database-geo-replication-security-config.md)ismertető cikkből tájékozódhat.
>

### <a name="connecting-from-the-internet"></a>Csatlakozás az internetről

Amikor egy számítógép megpróbál csatlakozni az adatbázis-kiszolgálóhoz az internetről, a tűzfal először az adatbázisszintű tűzfalszabályoknak megfelelően ellenőrzi a kérés eredeti IP-címét, a kapcsolati kérésben szereplő adatbázisra vonatkozóan:

* Ha a kérés IP-címe a adatbázisszintű tűzfalszabályokban megadott tartományok egyikében található, a tűzfal engedélyezi a csatlakozást a szabályt tartalmazó SQL-adatbázishoz.
* Ha a kérés IP-címe nem található meg az adatbázisszintű tűzfalszabályokban megadott tartományok egyikében sem, a tűzfal ellenőrzi a kiszolgálószintű tűzfalszabályokat. Ha a kérés IP-címe a kiszolgálószintű tűzfalszabályokban megadott tartományok egyikében található, a tűzfal engedélyezi a csatlakozást. A kiszolgálószintű tűzfalszabályok az összes SQL-adatbázisra érvényesek az Azure SQL Serveren.  
* Ha az IP-cím, a kérelem nem az adatbázis-szintjére, vagy a kiszolgálószintű tűzfalszabály megadott tartományok belül van, a kapcsolódási kérelem sikertelen lesz.

> [!NOTE]
> Ha a helyi számítógépről szeretné elérni az Azure SQL Database-t, akkor ellenőrizze, hogy a hálózaton és a helyi számítógépen lévő tűzfal engedélyezi-e a kimenő kommunikációt az 1433-as TCP-porton keresztül.
> 

### <a name="connecting-from-azure"></a>Csatlakozás az Azure-ból
Annak engedélyezéséhez, hogy az Azure-alkalmazások csatlakozhassanak az Azure SQL Serverhez, engedélyezni kell az Azure-kapcsolatokat. Amikor egy Azure-alkalmazás megkísérel csatlakozni az adatbázis-kiszolgálóhoz, a tűzfal ellenőrzi, hogy az Azure-kapcsolatok engedélyezve vannak-e. A 0.0.0.0 kezdő- és zárócímet tartalmazó tűzfalbeállítás jelzi, hogy ezek a kapcsolatok engedélyezettek. Ha a csatlakozási kísérlet nem engedélyezett, a kérés nem éri el az Azure SQL Database-kiszolgálót.

> [!IMPORTANT]
> Ez a beállítás konfigurálja a tűzfalat arra, hogy engedélyezzen minden, az Azure felől érkező kapcsolatot, beleértve a más ügyfelek előfizetéseiből érkező kapcsolatokat is. Ezen beállítás kiválasztásakor győződjön meg arról, hogy a bejelentkezési és felhasználói engedélyei a hozzáféréseket az arra jogosult felhasználókra korlátozzák.
> 

## <a name="creating-and-managing-firewall-rules"></a>Tűzfal-szabályok létrehozását és kezelését
Az első kiszolgálószintű tűzfal beállítás segítségével hozhatók létre a [Azure-portálon](https://portal.azure.com/) vagy programozott módon [Azure PowerShell](https://msdn.microsoft.com/library/azure/dn546724.aspx), [Azure CLI](/cli/azure/sql/server/firewall-rule#create), vagy a [REST API](https://msdn.microsoft.com/library/azure/dn505712.aspx). A további kiszolgálószintű tűzfalszabályok ugyanezekkel a módszerekkel, vagy a Transact-SQL-lel hozhatók létre és kezelhetők. 

> [!IMPORTANT]
> Adatbázis-szintű tűzfalszabályok csak hozhatók létre, és a Transact-SQL használatával kezelhetők. 
>

A teljesítmény javítása érdekében a kiszolgálószintű tűzfalszabályokat átmenetileg adatbázisszinten is gyorsítótárazza a rendszer. A gyorsítótár frissítésével kapcsolatban lásd: [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). 

> [!TIP]
> Használhat [SQL Database Auditing](sql-database-auditing.md) kiszolgáló- és adatbázis tűzfal naplózása.
>

### <a name="azure-portal"></a>Azure Portal

Egy kiszolgálószintű tűzfalszabályt beállítani az Azure portálon, vagy lépjen a – áttekintés oldalra az Azure SQL database vagy az Áttekintés lap az Azure Database logikai kiszolgáló.

> [!TIP]
> Az oktatóanyagok esetén lásd: [hozzon létre egy Azure portál használatával DB](sql-database-get-started-portal.md).
>

**Az adatbázis – áttekintés oldalra**

1. Az adatbázis – áttekintés oldalra egy kiszolgálószintű tűzfalszabályt beállításához kattintson **kiszolgáló tűzfalának beállítása** az eszköztáron a következő ábrán látható módon: A **tűzfalbeállítások** az SQL Database-kiszolgálóhoz tartozó lapon nyílik meg.

      ![kiszolgálói tűzfalszabály](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Kattintson a **ügyfél IP-cím hozzáadása** annak a számítógépnek az IP-cím hozzáadása az eszköztáron jelenleg használ, és kattintson a **mentése**. A rendszer létrehoz egy kiszolgálószintű tűzfalszabályt az aktuális IP-címhez.

      ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

**A kiszolgáló – áttekintés oldalra**

A kiszolgálóhoz tartozó – Áttekintés lapon nyit meg, hogy bemutatja a kiszolgáló teljesen minősített neve (például **mynewserver20170403.database.windows.net**) és további konfigurációs lehetőségeket.

1. A kiszolgáló – Áttekintés lap egy kiszolgálószintű szabály beállításához kattintson **tűzfal** a beállítások, amint az az alábbi ábrán a bal oldali menüben: 

     ![a logikai kiszolgáló – áttekintés](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Kattintson a **ügyfél IP-cím hozzáadása** annak a számítógépnek az IP-cím hozzáadása az eszköztáron jelenleg használ, és kattintson a **mentése**. A rendszer létrehoz egy kiszolgálószintű tűzfalszabályt az aktuális IP-címhez.

     ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

### <a name="transact-sql"></a>Transact-SQL
| Katalógusnézet vagy tárolt eljárás | Szint | Leírás |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Kiszolgáló |Az aktuális kiszolgálószintű tűzfalszabályok megjelenítése |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok létrehozása vagy frissítése |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok eltávolítása |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Adatbázis |Az aktuális adatbázisszintű tűzfalszabályok megjelenítése |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Adatbázis |Adatbázisszintű tűzfalszabályok létrehozása vagy frissítése |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Adatbázisok |Adatbázisszintű tűzfalszabályok eltávolítása |


Az alábbi példák tekintse át a meglévő szabályokat, egy adott IP-címek a kiszolgálóra Contoso engedélyezése és tűzfalszabály törlése:
   
```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```
  
Ezután adjon hozzá egy tűzfalszabályt.
   
```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Kiszolgálószintű tűzfalszabály törléséhez futtassa az sp_delete_firewall_rule tárolt eljárást. Az alábbi példában a szabály ContosoFirewallRule nevű törlése:
   
```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```   

### <a name="azure-powershell"></a>Azure PowerShell
| Parancsmag | Szint | Leírás |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Kiszolgáló |Az aktuális kiszolgálószintű tűzfalszabályokat adja vissza |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Kiszolgáló |Új kiszolgálószintű tűzfalszabály létrehozása |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Kiszolgáló |Meglévő kiszolgálószintű tűzfalszabály tulajdonságainak frissítése |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok eltávolítása |


Az alábbi mintakód egy kiszolgálószintű tűzfalszabályt PowerShell használatával:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> PowerShell az adott környezetben a gyors, tekintse meg a [létrehozása DB - PowerShell](sql-database-get-started-powershell.md) és [hozzon létre egy adatbázist, és a PowerShell használatával tűzfalszabályok konfigurálása](scripts/sql-database-create-and-configure-database-powershell.md)
>

### <a name="azure-cli"></a>Azure CLI
| Parancsmag | Szint | Leírás |
| --- | --- | --- |
| [az sql server tűzfal létrehozása](/cli/azure/sql/server/firewall-rule#create) | Engedélyezi a kiszolgálón lévő összes SQL-adatbázis elérését a megadott IP-címtartomány egy tűzfalszabályt hoz létre.|
| [az sql server tűzfal törlése](/cli/azure/sql/server/firewall-rule#delete)| Törli a tűzfalszabályt.|
| [az sql server tűzfal listájára](/cli/azure/sql/server/firewall-rule#list)| A tűzfal-szabályokat sorolja fel.|
| [az sql server tűzfalszabály megjelenítése](/cli/azure/sql/server/firewall-rule#show)| Egy tűzfalszabály részleteit jeleníti meg.|
| [frissítés az sql server tűzfal szabály AX](/cli/azure/sql/server/firewall-rule#update)| Egy tűzfalszabály frissíti.

Az alábbi mintakód egy kiszolgálószintű tűzfalszabályt, az Azure parancssori felület használatával: 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Például egy Azure CLI-t az adott környezetben a gyors üzembe helyezési, lásd: [létrehozása KCSA - Azure CLI](sql-database-get-started-cli.md) és [egy önálló adatbázis létrehozása és konfigurálása egy tűzfalszabályt, az Azure parancssori felület használatával](scripts/sql-database-create-and-configure-database-cli.md)
>

### <a name="rest-api"></a>REST API
| API | Szint | Leírás |
| --- | --- | --- |
| [List Firewall Rules](https://msdn.microsoft.com/library/azure/dn505715.aspx) |Kiszolgáló |Az aktuális kiszolgálószintű tűzfalszabályok megjelenítése |
| [Create Firewall Rule](https://msdn.microsoft.com/library/azure/dn505712.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok létrehozása vagy frissítése |
| [Set Firewall Rule](https://msdn.microsoft.com/library/azure/dn505707.aspx) |Kiszolgáló |Meglévő kiszolgálószintű tűzfalszabály tulajdonságainak frissítése |
| [Delete Firewall Rule](https://msdn.microsoft.com/library/azure/dn505706.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok eltávolítása |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabályt és egy adatbázis-adatbázisszintű tűzfalszabály
Q. Egy adatbázis felhasználói kell teljesen különíteni az egy másik adatbázis?   
  Ha igen, hozzáférést adatbázis szintű tűzfalszabályok használatával. Ezzel elkerülhető használatával kiszolgálószintű tűzfal-szabályok, amelyek lehetővé teszik az összes adatbázisra, a védelmekkel mélységének csökkentése a tűzfalon a hozzáférést.   
 
Q. Az IP-címen a felhasználói hozzáférést igényelnek az összes adatbázis?   
  Kiszolgálószintű tűzfal-szabályok használatával konfigurálnia kell a tűzfalszabályok száma csökkenthető.   

Q. Személyt vagy csapatot a tűzfal-szabályok konfigurálása rendelkezik az Azure-portálon, a PowerShell vagy a REST API-en keresztüli hozzáférés?   
  Kiszolgálószintű tűzfal-szabályokat kell használnia. Adatbázis-szintű tűzfalszabályok csak Transact-SQL használatával konfigurálható.  

Q. Van személyt vagy csapatot a tűzfalszabályok beállítása magas szintű engedéllyel rendelkezik az adatbázis szintjén tiltani?   
  Kiszolgálószintű tűzfal-szabályok használata. Felhasználó számára legalább használatával Transact-SQL-adatbázis szintű tűzfalszabályok beállítása `CONTROL DATABASE` engedéllyel az adatbázis szintjén.  

Q. Központilag kezelése tűzfalszabályok számos személyt vagy csapatot konfigurálásához, vagy a tűzfalszabályok naplózás van (akár 100 egység) adatbázisok?   
  Ez a beállítás attól függ, a szükségleteinek és környezet. Előfordulhat, hogy a kiszolgálószintű tűzfal-szabályok beállítása egyszerűbb, de scripting szabályok szinten konfigurálható az adatbázis. Akkor is, ha a kiszolgálószintű tűzfal-szabályokat használ, szükség lehet az adatbázis-tűzfalszabályok, hogy megtudja, naplózási és a felhasználók `CONTROL` engedéllyel az adatbázishoz a létrehozott adatbázis szintű tűzfalszabályokat.   

Q. Mindkét kiszolgáló- és adatbázis tűzfalszabályok vegyesen használata   
  Igen. Egyes felhasználók, rendszergazdák például módosítania kell kiszolgálószintű tűzfal-szabályokat. Más felhasználók, például egy adatbázis-alkalmazás felhasználói adatbázis-szintű tűzfalszabályok módosítania kell.   

## <a name="troubleshooting-the-database-firewall"></a>Az adatbázistűzfal hibaelhárítása
A következő szempontokat vegye figyelembe, ha a Microsoft Azure SQL Database szolgáltatáshoz való hozzáférése nem a várt módon működik:

* **Helyi tűzfal konfigurációja:** Mielőtt a számítógépe hozzáférne az Azure SQL Database szolgáltatáshoz, lehet, hogy létre kell hoznia egy tűzfalkivételt a számítógépén az 1433-as TCP-port számára. Ha az Azure-felhő határain belül létesít kapcsolatokat, előfordulhat, hogy további portokat is meg kell nyitnia. További információkért lásd: a **SQL-adatbázis: kívül és belül** szakasza [kívüli ADO.NET 4.5 és az SQL-adatbázis 1433-as portokon](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Hálózati címfordítás (NAT):** A NAT miatt a számítógépe által az Azure SQL Database szolgáltatáshoz való csatlakozáshoz használt IP-cím eltérhet a számítógép IP-konfigurációs beállításai között megjelenő IP-címtől. A számítógép által az Azure-hoz való csatlakozáshoz használt IP-cím megtekintéséhez jelentkezzen be a portálra, és lépjen az adatbázist futtató kiszolgáló **Konfigurálás** lapjára. Az **Engedélyezett IP-címek** szakasz alatt jelenik meg az **Ügyfél aktuális IP-címe**. Kattintson a **Hozzáadás** lehetőségre az **Engedélyezett IP-címek** között, ha szeretné engedélyezni a számítógép számára a kiszolgáló elérését.
* **Az engedélyezési lista módosításai még nem léptek érvénybe:** Az Azure SQL Database tűzfalkonfigurációján végzett módosítások érvénybe lépéséig akár öt perc is eltelhet.
* **A bejelentkezés nem engedélyezett, vagy helytelen jelszó lett használva:** Ha egy bejelentkezés nem rendelkezik engedélyekkel az Azure SQL Database-kiszolgálón, vagy a használt jelszó érvénytelen, akkor a rendszer megtagadja az Azure SQL Database-kiszolgálóhoz való csatlakozást. Egy tűzfalbeállítás létrehozása lehetővé teszi az ügyfelek számára a kiszolgálóhoz való csatlakozás megkísérlését, azonban minden egyes ügyfélnek meg kell adnia a szükséges biztonsági hitelesítő adatokat. A bejelentkezések előkészítésével kapcsolatos további információkért lásd az adatbázisok, bejelentkezések és felhasználók Azure SQL Database-ben történő kezelésével foglalkozó cikket.
* **Dinamikus IP-cím**: Ha az internetkapcsolata dinamikus IP-címkezeléssel rendelkezik, és problémákat okoz a tűzfalon való átjutás, próbálja ki a következő megoldások valamelyikét:
  
  * Kérje el az internetszolgáltatójától az Azure SQL Database-kiszolgáló eléréséhez használt ügyfélszámítógépeihez társított IP-címtartományt, majd adja meg ezt az IP-címtartományt egy tűzfalszabályként.
  * Állítson be statikus IP-címeket az ügyfélszámítógépei számára, majd adja meg az IP-címeket tűzfalszabályokként.

## <a name="next-steps"></a>Következő lépések

- Az adatbázis és a kiszolgálószintű tűzfalszabály létrehozása a gyors üzembe helyezési, lásd: [hozzon létre egy Azure SQL-adatbázis](sql-database-get-started-portal.md).
- Ha nyílt forráskódú vagy külső alkalmazásokból szeretne kapcsolódni az Azure SQL Database-hez, lásd az [SQL Database gyors üzembe helyezési ügyfélkódmintáival](https://msdn.microsoft.com/library/azure/ee336282.aspx) foglalkozó cikket.
- Kapcsolatban további portok megnyitásához szükséges információk, a **SQL-adatbázis: kívül és belül** szakasza [portok túl az 1433-as ADO.NET 4.5 és az SQL-adatbázis](sql-database-develop-direct-route-ports-adonet-v12.md)
- Az Azure SQL Database biztonsági áttekintését lásd: [biztonságossá tétele az adatbázis](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
