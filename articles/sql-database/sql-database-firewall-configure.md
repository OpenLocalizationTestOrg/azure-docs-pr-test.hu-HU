---
title: "SQL Database tűzfalszabályai aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure SQL adatbázis-kiszolgáló- és adatbázis tűzfal-szabályok toomanage hozzáféréssel rendelkező tűzfal."
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
ms.openlocfilehash: 6a8cdf629d0d0e55421a5e9f9b894a21371be568
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-server-level-and-database-level-firewall-rules"></a>Az Azure SQL Database kiszolgáló- és adatbázis tűzfalszabályok 

A Microsoft Azure SQL Database egy relációs adatbázis-szolgáltatást nyújt az Azure és egyéb internetalapú alkalmazások számára. az adatok védelméhez toohelp, tűzfalak tooyour adatbázis-kiszolgáló minden hozzáférés tiltása, amíg megadhatja, hogy mely számítógépek rendelkeznek engedéllyel. hello tűzfal engedélyezi a hozzáférést toodatabases származó IP-cím az egyes kérelmek hello alapján.

## <a name="overview"></a>Áttekintés

Kezdetben minden Transact-SQL hozzáférés tooyour Azure SQL server hello tűzfal blokkolja. az Azure SQL server használatával toobegin meg kell adnia egy vagy több kiszolgálószintű tűzfalszabályt, amely engedélyezi hozzáférés tooyour Azure SQL-kiszolgálót. Hello tűzfal szabályok toospecify mely IP-címtartományt, az Internet engedélyezettek hello használja, és hogy az Azure-alkalmazások megpróbálhatnak tooconnect tooyour Azure SQL server.

tooselectively grant hozzáférés toojust hello adatbázisokat az Azure SQL Server egyik, hello szükséges adatbázis adatbázis-szintű szabályt kell létrehoznia. Adjon meg egy IP-címtartomány a hello adatbázishoz tartozó tűzfalszabály, amely hello IP-címtartományt megadott hello kiszolgálószintű tűzfalszabályt, és győződjön meg arról, hogy hello ügyfél IP-címe hello hello adatbázis szintű szabályban megadott hello közé esik-e.

A kapcsolódási kísérletek hello Internet és Azure kell először telnie hello tűzfalon keresztül érik az Azure SQL server vagy SQL-adatbázis, ahogy az ábra a következő hello:

   ![A tűzfal-konfigurációt bemutató ábra.][1]

* **Kiszolgálószintű tűzfal-szabályokat:** ezek a szabályok engedélyezése az ügyfelek tooaccess a teljes Azure SQL server, ez azt jelenti, hogy az összes hello adatbázis belül hello azonos logikai kiszolgáló. Ezek a szabályok be vannak tárolva hello **fő** adatbázis. Kiszolgálószintű tűzfal-szabályokat hello portál használatával, vagy a Transact-SQL-utasítások segítségével konfigurálható. hello Azure-portálon vagy a PowerShell használatával toocreate kiszolgálószintű tűzfal szabályokat, a hello előfizetés tulajdonosa vagy egy előfizetés közreműködői kell lennie. toocreate egy kiszolgálószintű tűzfalszabályt Transact-SQL használatával, csatlakoznia kell SQL Database-példányt toohello hello kiszolgálószintű fő bejelentkezéssel vagy hello Azure Active Directory-rendszergazda (ami azt jelenti, hogy egy kiszolgálószintű tűzfalszabályt először létre kell hozni a felhasználó Azure-szintű engedélyekkel).
* **Adatbázis-adatbázisszintű tűzfalszabályok:** ezek a szabályok engedélyezése az ügyfelek tooaccess bizonyos (biztonságos) adatbázisok hello belül azonos logikai kiszolgáló. Hozhatja létre ezeket a szabályokat az egyes adatbázisok (beleértve a hello **fő** database0) és tárolásuk hello egyes adatbázisokat. Adatbázis-szintű tűzfalszabályok csak Transact-SQL-utasítások segítségével konfigurálhatja, és csak a beállítása után hello első kiszolgálószintű tűzfal. Ha az IP-címtartományok hello adatbázis-adatbázisszintű tűzfalszabály, amely megadott hello kiszolgálószintű tűzfalszabály külső hello tartomány ad meg, csak ezek az ügyfelek IP-címmel rendelkező hello adatbázis szintű tartományban hozzáférhessen hello adatbázishoz. Egy adatbázishoz legfeljebb 128 adatbázisszintű tűzfalszabály adható meg. Adatbázis-szintű tűzfalszabályok fő- és felhasználói adatbázisok csak hozható létre és kezelhetők a Transact-SQL. Az adatbázis-szintű tűzfalszabályok beállítása további információkért lásd: hello példa későbbi részében a következő cikket, és tekintse meg [sp_set_database_firewall_rule (Azure SQL-adatbázisok)](https://msdn.microsoft.com/library/dn270010.aspx).

**Javaslat:** Microsoft azt javasolja, hogy adatbázis-szintű tűzfalszabályok segítségével amikor csak lehetséges tooenhance biztonsági és toomake az adatbázis több hordozható. Kiszolgálószintű tűzfal-szabályok használata a rendszergazdák számára, és ha sok adatbázis, amely rendelkezik hello ugyanazt a hozzáférési követelményeket, és nem szeretné, minden adatbázisnak külön-külön konfigurálása toospend idő.

> [!Note]
> Az üzletmenet folytonossága hello környezetben hordozható adatbázisokkal kapcsolatos információk: [hitelesítésének követelményei a vész-helyreállítási](sql-database-geo-replication-security-config.md).
>

### <a name="connecting-from-hello-internet"></a>A hello Internet csatlakoztatása

Amikor egy számítógép megpróbál tooconnect tooyour adatbázis-kiszolgálójának hello Internet, hello tűzfal először ellenőrzi hello származó hello adatbázis szintű adatbázisra vonatkozó tűzfalszabályok, hello hello kapcsolatot igénylő hello kérelmet IP-címe:

* Ha hello IP-cím hello kérelem hello adatbázis szintű tűzfalszabályok megadott hello tartományok egyikébe esik, hello kapcsolat toohello hello szabályt tartalmazó SQL-adatbázis kapnak.
* Ha hello IP-cím hello kérelem nem hello adatbázis-adatbázisszintű tűzfalszabály megadott hello tartományok egyikébe esik, hello kiszolgálószintű tűzfal-szabályokat a rendszer ellenőrzi. Ha hello IP-cím hello kérelem hello kiszolgálószintű tűzfal szabályokban megadott hello tartományok egyikébe esik, hello kapcsolat kapnak. Kiszolgálószintű tűzfal-szabályokat alkalmazni tooall SQL-adatbázisok hello Azure SQL-kiszolgálón.  
* Ha hello IP-cím hello kérelem nincs belül hello tartományok hello adatbázis szintű valamelyikében vagy megadott kiszolgálószintű tűzfal-szabályok, hello kapcsolódási kérelem sikertelen lesz.

> [!NOTE]
> az Azure SQL Database tooaccess a helyi számítógépen, győződjön meg arról, a hálózati és a helyi számítógép hello tűzfala lehetővé teszi, hogy a 1433-as TCP-port kimenő kommunikációra.
> 

### <a name="connecting-from-azure"></a>Csatlakozás az Azure-ból
az Azure tooconnect tooyour Azure SQL server Azure-kapcsolatok tooallow alkalmazások engedélyezve kell lennie. Amikor egy alkalmazás az Azure-ból próbál tooconnect tooyour adatbázis-kiszolgáló, a hello tűzfal ellenőrzi, hogy az Azure-kapcsolatok engedélyezve legyenek-e. A tűzfal beállítása a kezdő és záró cím egyenlő too0.0.0.0 azt jelzi, hogy ezek a kapcsolatok számára engedélyezett. Hello kapcsolódási kísérlet nem engedélyezett, ha a hello kérelem nem éri el hello Azure SQL Database-kiszolgálóhoz.

> [!IMPORTANT]
> Ez a beállítás állítja be a hello tűzfal tooallow más ügyfelek hello előfizetések az Azure többek között a következőket kapcsolatoktól összes kapcsolatokat. Ha ezzel a beállítással, a bejelentkezési győződjön meg arról, valamint a felhasználói engedélyek korlátozása hozzáférés tooonly engedéllyel rendelkező felhasználók.
> 

## <a name="creating-and-managing-firewall-rules"></a>Tűzfal-szabályok létrehozását és kezelését
hello első kiszolgálószintű tűzfal beállítás hello segítségével hozhatók létre [Azure-portálon](https://portal.azure.com/) vagy programozott módon [Azure PowerShell](https://msdn.microsoft.com/library/azure/dn546724.aspx), [Azure CLI](/cli/azure/sql/server/firewall-rule#create), vagy hello [REST API-t](https://msdn.microsoft.com/library/azure/dn505712.aspx). A további kiszolgálószintű tűzfalszabályok ugyanezekkel a módszerekkel, vagy a Transact-SQL-lel hozhatók létre és kezelhetők. 

> [!IMPORTANT]
> Adatbázis-szintű tűzfalszabályok csak hozhatók létre, és a Transact-SQL használatával kezelhetők. 
>

tooimprove teljesítmény, a kiszolgálószintű tűzfal szabályok ideiglenesen gyorsítótárba kerüljenek-e hello adatbázis szintjén. toorefresh hello gyorsítótár, lásd: [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). 

> [!TIP]
> Használhat [SQL Database Auditing](sql-database-auditing.md) tooaudit kiszolgáló- és adatbázis tűzfal módosításokat.
>

### <a name="azure-portal"></a>Azure Portal

tooset egy kiszolgálószintű tűzfalszabályt hello Azure-portálon, vagy lépjen toohello – áttekintés oldalra Azure SQL adatbázis vagy hello áttekintése lapon a Azure Database logikai kiszolgáló.

> [!TIP]
> Az oktatóanyagok esetén lásd: [Azure-portálon hozzon létre egy DB használatával hello](sql-database-get-started-portal.md).
>

**Az adatbázis – áttekintés oldalra**

1. tooset egy kiszolgálószintű tűzfalszabályt hello adatbázis – áttekintés lapon kattintson a **kiszolgáló tűzfalának beállítása** hello eszköztáron látható hello kép a következő módon: hello **tűzfalbeállítások** hello SQL lapja Megnyílik az adatbázis-kiszolgáló.

      ![kiszolgálói tűzfalszabály](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Kattintson a **ügyfél IP-cím hozzáadása** hello eszköztár tooadd hello IP-cím hello számítógép jelenleg használ, és kattintson a **mentése**. A rendszer létrehoz egy kiszolgálószintű tűzfalszabályt az aktuális IP-címhez.

      ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

**A kiszolgáló – áttekintés oldalra**

hello áttekintő lapjára jut a kiszolgáló megnyílik, teljes mértékben hello megjelenítő minősített kiszolgáló neve (például **mynewserver20170403.database.windows.net**) és további konfigurációs lehetőségeket.

1. tooset egy kiszolgálószintű szabály áttekintése lapon kattintson a **tűzfal** hello bal oldali menüben a beállítások, amint az a következő kép hello: 

     ![a logikai kiszolgáló – áttekintés](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Kattintson a **ügyfél IP-cím hozzáadása** hello eszköztár tooadd hello IP-cím hello számítógép jelenleg használ, és kattintson a **mentése**. A rendszer létrehoz egy kiszolgálószintű tűzfalszabályt az aktuális IP-címhez.

     ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

### <a name="transact-sql"></a>Transact-SQL
| Katalógusnézet vagy tárolt eljárás | Szint | Leírás |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Kiszolgáló |Megjeleníti az aktuális kiszolgálószintű tűzfal-szabályokat hello |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok létrehozása vagy frissítése |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok eltávolítása |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Adatbázis |Megjeleníti a hello aktuális adatbázis szintű tűzfalszabályok |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Adatbázis |Létrehozza vagy frissíti az adatbázis-szintű tűzfalszabályok hello |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Adatbázisok |Adatbázisszintű tűzfalszabályok eltávolítása |


hello alábbi példák tekintse át a meglévő szabályok hello hello kiszolgálóra Contoso egy adott IP-címek engedélyezése és a tűzfalszabály törlése:
   
```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```
  
Ezután adjon hozzá egy tűzfalszabályt.
   
```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

egy kiszolgálószintű tűzfalszabályt toodelete hello sp_delete_firewall_rule tárolt eljárás végrehajtása. hello alábbi példa hello szabály törlése ContosoFirewallRule nevű:
   
```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```   

### <a name="azure-powershell"></a>Azure PowerShell
| Parancsmag | Szint | Leírás |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Kiszolgáló |Hello aktuális kiszolgálószintű tűzfal-szabályokat adja vissza |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Kiszolgáló |Új kiszolgálószintű tűzfalszabály létrehozása |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Kiszolgáló |Meglévő kiszolgálószintű tűzfalszabály hello tulajdonságainak frissítése |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok eltávolítása |


a következő példa hello beállítja egy kiszolgálószintű tűzfalszabályt PowerShell használatával:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> PowerShell hello környezetben a gyors, tekintse meg a [létrehozása DB - PowerShell](sql-database-get-started-powershell.md) és [hozzon létre egy adatbázist, és a PowerShell használatával tűzfalszabályok konfigurálása](scripts/sql-database-create-and-configure-database-powershell.md)
>

### <a name="azure-cli"></a>Azure CLI
| Parancsmag | Szint | Leírás |
| --- | --- | --- |
| [az sql server tűzfal létrehozása](/cli/azure/sql/server/firewall-rule#create) | Létrehoz egy tűzfal szabály tooallow hozzáférés tooall SQL-adatbázisok hello megadott IP-címtartomány a hello kiszolgálóján.|
| [az sql server tűzfal törlése](/cli/azure/sql/server/firewall-rule#delete)| Törli a tűzfalszabályt.|
| [az sql server tűzfal listájára](/cli/azure/sql/server/firewall-rule#list)| Hello tűzfal-szabályokat sorolja fel.|
| [az sql server tűzfalszabály megjelenítése](/cli/azure/sql/server/firewall-rule#show)| Egy tűzfalszabály hello részleteit jeleníti meg.|
| [frissítés az sql server tűzfal szabály AX](/cli/azure/sql/server/firewall-rule#update)| Egy tűzfalszabály frissíti.

a következő példa hello beállítja egy kiszolgálószintű tűzfalszabályt hello Azure parancssori felület használatával: 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Például egy Azure CLI hello környezetben gyors, lásd: [létrehozása KCSA - Azure CLI](sql-database-get-started-cli.md) és [egy önálló adatbázis létrehozása és konfigurálása a tűzfalszabályok hello Azure parancssori felület használatával](scripts/sql-database-create-and-configure-database-cli.md)
>

### <a name="rest-api"></a>REST API
| API | Szint | Leírás |
| --- | --- | --- |
| [List Firewall Rules](https://msdn.microsoft.com/library/azure/dn505715.aspx) |Kiszolgáló |Megjeleníti az aktuális kiszolgálószintű tűzfal-szabályokat hello |
| [Create Firewall Rule](https://msdn.microsoft.com/library/azure/dn505712.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok létrehozása vagy frissítése |
| [Set Firewall Rule](https://msdn.microsoft.com/library/azure/dn505707.aspx) |Kiszolgáló |Meglévő kiszolgálószintű tűzfalszabály hello tulajdonságainak frissítése |
| [Tűzfalszabály törlése](https://msdn.microsoft.com/library/azure/dn505706.aspx) |Kiszolgáló |Kiszolgálószintű tűzfalszabályok eltávolítása |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabályt és egy adatbázis-adatbázisszintű tűzfalszabály
Q. Egy adatbázis felhasználói kell teljesen különíteni az egy másik adatbázis?   
  Ha igen, hozzáférést adatbázis szintű tűzfalszabályok használatával. Ezzel elkerülhető a kiszolgálószintű tűzfal-szabályok, amelyek lehetővé teszik a hozzáférést hello tűzfalon keresztül tooall adatbázisok, csökkenti a védelmekkel hello mélysége használatával.   
 
Q. Hajtsa végre az hello IP-cím felhasználói kell adatbázisához tooall?   
  Használja a kiszolgálószintű tűzfal szabályok tooreduce hello ennyiszer tűzfalszabályokat kell konfigurálnia.   

Q. Hello személyt vagy csapatot csak a hello tűzfalszabályok beállítása rendelkezik hello Azure-portálon PowerShell, vagy a REST API hello keresztül hozzáférést?   
  Kiszolgálószintű tűzfal-szabályokat kell használnia. Adatbázis-szintű tűzfalszabályok csak Transact-SQL használatával konfigurálható.  

Q. Hello személyt vagy csapatot hello tűzfalszabályok beállítása tiltott hello adatbázis szintjén magas szintű engedéllyel rendelkezik?   
  Kiszolgálószintű tűzfal-szabályok használata. Felhasználó számára legalább használatával Transact-SQL-adatbázis szintű tűzfalszabályok beállítása `CONTROL DATABASE` engedély hello adatbázis szintjén.  

Q. Hello személyt vagy csapatot konfigurálásához, vagy a naplózás hello tűzfalszabályok, központilag kezelése tűzfalszabályok számos van (akár 100 egység) adatbázisok?   
  Ez a beállítás attól függ, a szükségleteinek és környezet. Kiszolgálószintű tűzfal-szabályokat lehet, hogy könnyebben tooconfigure, de a parancsfájl-kezelési szabályok konfigurálható hello adatbázis szintű. És akkor is, ha a kiszolgálószintű tűzfal-szabályokat használ, tooaudit hello adatbázis-tűzfalszabályok, toosee akkor, ha a felhasználók `CONTROL` engedéllyel rendelkezik a hello adatbázis adatbázis-szintű tűzfalszabályok hozott létre.   

Q. Mindkét kiszolgáló- és adatbázis tűzfalszabályok vegyesen használata   
  Igen. Egyes felhasználók, rendszergazdák például módosítania kell kiszolgálószintű tűzfal-szabályokat. Más felhasználók, például egy adatbázis-alkalmazás felhasználói adatbázis-szintű tűzfalszabályok módosítania kell.   

## <a name="troubleshooting-hello-database-firewall"></a>Hello adatbázis tűzfal hibaelhárítása
Vegye figyelembe a következő pontokat, amikor hozzáférési toohello Microsoft Azure SQL Database szolgáltatás nem a várt módon működnek várt hello:

* **Helyi tűzfal konfigurációját:** a számítógép hozzáférhessen az Azure SQL Database, szükség lehet olyan érvényes tűzfalkivétel toocreate az 1433-as TCP-port a számítógépen. Ha kapcsolatok belül hello Azure felhőben határt, előfordulhat, hogy tooopen további portokat. További információkért lásd: hello **SQL-adatbázis: kívül és belül** szakasza [kívüli ADO.NET 4.5 és az SQL-adatbázis 1433-as portokon](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Hálózati címfordítás (NAT):** kellő tooNAT, a számítógép tooconnect tooAzure SQL-adatbázis hello IP-cím látható a számítógép IP-konfigurációs beállítások eltérhetnek által használt hello IP-cím. a számítógép tooview hello IP-cím tooconnect tooAzure használva toohello portal-e jelentkezni, és keresse meg a toohello **konfigurálása** az adatbázist üzemeltető kiszolgálón hello lapon. A hello **engedélyezett IP-címek** című szakaszba hello **aktuális ügyfél IP-cím** jelenik meg. Kattintson a **Hozzáadás** toohello **engedélyezett IP-címek** tooallow a számítógép tooaccess hello kiszolgáló.
* **Módosítások toohello engedélyezési lista rendelkezik még nincsenek érvényben:** lehet, mint amennyit egy 5 perces késleltetést használ az toohello Azure SQL Database tűzfal konfigurációs tootake hatás változik.
* **hello bejelentkezési nincs engedélyezve, vagy helytelen jelszót használt:** Ha egy bejelentkezési azonosító nem rendelkezik engedélyekkel hello Azure SQL adatbázis-kiszolgálón, vagy hello használt jelszó nem megfelelő, hello kapcsolat toohello Azure SQL adatbázis-kiszolgáló megtagadva. Csak egy tűzfal-beállítás létrehozásához biztosít az ügyfelek számára egy lehetőség tooattempt tooyour server; kapcsolódás minden ügyfél hello szükséges biztonsági adatokat kell megadni. A bejelentkezések előkészítésével kapcsolatos további információkért lásd az adatbázisok, bejelentkezések és felhasználók Azure SQL Database-ben történő kezelésével foglalkozó cikket.
* **Dinamikus IP-cím:** Ha az internethez, a dinamikus IP-címzést és tapasztal hello tűzfalon keresztül történt, próbálkozzon a következő megoldások hello egyikét:
  
  * Kérje meg az internetszolgáltató (ISP) hello IP-címtartomány hozzárendelt tooyour ügyfélszámítógépek számára, hogy hozzáférési hello Azure SQL Database-kiszolgálóhoz, és adja hozzá a hello IP-címtartomány tűzfal szabály.
  * Első statikus IP-címzés helyette az ügyfélszámítógépek számára, és adja hozzá a hello IP-címek, tűzfal-szabályokat.

## <a name="next-steps"></a>Következő lépések

- Az adatbázis és a kiszolgálószintű tűzfalszabály létrehozása a gyors üzembe helyezési, lásd: [hozzon létre egy Azure SQL-adatbázis](sql-database-get-started-portal.md).
- Az összekötő tooan Azure SQL-adatbázis nyílt forráskódú vagy harmadik féltől származó alkalmazások útmutatásért lásd: [gyors üzembe helyezési Ügyfélkód minták tooSQL adatbázis](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Hogy szükség lehet a tooopen portokon további információ: hello **SQL-adatbázis: kívül és belül** szakasza [portok túl az 1433-as ADO.NET 4.5 és az SQL-adatbázis](sql-database-develop-direct-route-ports-adonet-v12.md)
- Az Azure SQL Database biztonsági áttekintését lásd: [biztonságossá tétele az adatbázis](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
