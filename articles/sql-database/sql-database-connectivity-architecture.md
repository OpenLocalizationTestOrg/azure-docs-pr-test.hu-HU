---
title: "SQL adatbázis-kapcsolat architektúra aaaAzure |} Microsoft Docs"
description: "Ez a dokumentum ismerteti a hello Azure SQLDB kapcsolat architektúra az Azure-ban vagy az Azure-on kívüli."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/05/2017
ms.author: carlrab
ms.openlocfilehash: 917df6d88a16f1b841b617fb2a53025b4d14d034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a>Az Azure SQL Database kapcsolat architektúrája 

Ez a cikk ismerteti a hello Azure SQL adatbázis-kapcsolat architektúra, és elmagyarázza, hogyan hello különböző összetevői működni toodirect forgalom tooyour példányát az Azure SQL Database. Ezek az Azure SQL Database összetevőit működik toodirect hálózati forgalom toohello Azure-adatbázis, a csatlakozás az Azure ügyfelekkel és az Azure-on kívüli csatlakozó ügyfelek. Ez a cikk a parancsfájl minták toochange kapcsolat módját is tartalmazza, és a hello szempontok kapcsolódó toochanging hello alapértelmezett kapcsolat beállításokat. Ha kérdése van a cikk elolvasása után, forduljon a következő Dhruv dmalik@microsoft.com. 

## <a name="connectivity-architecture"></a>Kapcsolati architektúra

a következő diagram hello magas szintű áttekintést nyújt az Azure SQL adatbázis-kapcsolat architektúra hello. 

![architektúra áttekintése](./media/sql-database-connectivity-architecture/architecture-overview.png)


hello következő lépések bemutatják, hogyan a kapcsolat azért hello Azure SQL Database szoftver betöltési-terheléselosztó Állapotfigyelője és hello Azure SQL Database átjárón keresztül létesített tooan Azure SQL-adatbázis.

- Azure-ban vagy az Azure-on kívüli ügyfelek csatlakozni toohello SLB, amelyek egy nyilvános IP-címmel rendelkezik, és az 1433-as porton figyel.
- hello SLB irányítja a forgalmat toohello Azure SQL Database átjáró.
- hello átjáró hello forgalom toohello megfelelő proxy köztes irányítja át.
- hello proxy köztes hello forgalom toohello megfelelő Azure SQL adatbázis irányítja át.

> [!IMPORTANT]
> Ezeket az összetevőket tartalmaz elosztott szolgáltatásmegtagadásos (DDoS-) szolgáltatás védelmi beépített hello hálózati és hello app réteget.
>

## <a name="connectivity-from-within-azure"></a>Kapcsolat az Azure-ban

Ha csatlakozik Azure-ban, a kapcsolatokat. a kapcsolat házirenddel rendelkezhetnek a **átirányítási** alapértelmezés szerint. A házirend a **átirányítási** azt jelenti, hogy kapcsolatok hello TCP-munkamenet végeztével a meghatározott toohello Azure SQL database, hello ügyfélmunkamenethez majd toohello proxy köztes módosítása toohello céljának virtuális IP-származó átirányítva amely hello proxy köztes "hello Azure SQL Database" átjáró toothat. Ezt követően minden ezt követő csomagok flow hello proxy köztes hello Azure SQL Database átjáró megkerülésével közvetlenül keresztül. hello a következő ábra szemlélteti a forgalom áramlását.

![architektúra áttekintése](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Azure-on kívüli kapcsolat

Ha a külső Azure kapcsolódik, a kapcsolatokat. a kapcsolat házirenddel rendelkezhetnek a **Proxy** alapértelmezés szerint. A házirend a **Proxy** azt jelenti, hogy hello TCP munkamenet hello Azure SQL Database átjárón keresztül, és minden későbbi csomagok flow keresztül hello átjáró. hello a következő ábra szemlélteti a forgalom áramlását.

![architektúra áttekintése](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Az Azure SQL Database átjáró IP-címek

tooconnect tooan Azure SQL-adatbázis a helyszíni erőforrások, szükség van tooallow kimenő hálózati forgalom toohello Azure SQL Database átjáró az Azure-régiót. A kapcsolatok csak lépjen hello átjárón keresztül Proxy módban, amely hello alapértelmezett esetén a helyszíni erőforrások csatlakoztatása kapcsolódáskor.

a következő táblázat hello hello elsődleges és másodlagos IP-cím, az összes adatterületek hello Azure SQL Database-átjáró. Egyes régiókhoz a rendszer két IP-cím. Ezeken a területeken hello elsődleges IP-címét az aktuális IP-cím hello hello átjáró és hello második IP-cím a feladatátvételi IP-címet. hello feladatátvételi címe hello cím toowhich előfordulhat, hogy helyezze át a kiszolgáló tookeep hello szolgáltatás rendelkezésre állása magas, azt. Ezek a régiókban javasoljuk, hogy engedélyezi a kimenő tooboth hello IP-címek. hello második IP-cím a Microsoft tulajdonában van, és nem figyel a szolgáltatások által az Azure SQL Database tooaccept kapcsolatok aktiválásáig.

| Régió neve | Elsődleges IP-cím | Másodlagos IP-cím |
| --- | --- |--- |
| Kelet-Ausztrália | 191.238.66.109 | 13.75.149.87 |
| Délkelet-Ausztrália | 191.239.192.109 | 13.73.109.251 |
| Dél-Brazília | 104.41.11.5 | |    
| Közép-Kanada | 40.85.224.249 | |    
| Kelet-Kanada | 40.86.226.166 | |
| USA középső régiója | 23.99.160.139 | 13.67.215.62 |
| Kelet-Ázsia | 191.234.2.139 | 52.175.33.150 |
| 1 USA keleti régiója | 191.238.6.43 | 40.121.158.30 |
| USA 2. keleti régiója | 191.239.224.107 | 40.79.84.180 |
| Közép-India | 104.211.96.159  | |   
| Dél-India | 104.211.224.146  | |
| Nyugat-India | 104.211.160.80 | |
| Kelet-Japán | 191.237.240.43 | 13.78.61.196 |
| Nyugat-Japán | 191.238.68.11 | 104.214.148.156 |
| Korea középső régiója | 52.231.32.42 | |
| Korea déli régiója | 52.231.200.86 |  |
| USA északi középső régiója | 23.98.55.75 | 23.96.178.199 |
| Észak-Európa | 191.235.193.75 | 40.113.93.91 |
| USA déli középső régiója | 23.98.162.75 | 13.66.62.124 |
| Délkelet-Ázsia | 23.100.117.95 | 104.43.15.0 |
| Egyesült Királyság északi régiója | 13.87.97.210 | |
| Egyesült Királyság déli régiója 1 | 51.140.184.11 | |    
| Egyesült Királyság 2. déli régiója | 13.87.34.7 | |
| Az Egyesült Királyság nyugati régiója | 51.141.8.11  | |
| USA nyugati középső régiója | 13.78.145.25 | |
| Nyugat-Európa | 191.237.232.75 | 40.68.37.158 |
| 1 USA nyugati régiója | 23.99.34.75 | 104.42.238.205 |
| USA nyugati régiója, 2. | 13.66.226.202  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a>Módosítsa a kapcsolatkezelési házirendet az Azure SQL Database

az Azure SQL Database kapcsolatkezelési házirendet az Azure SQL Database-kiszolgáló esetén használjon hello toochange hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx). 

- Ha a kapcsolat házirend túl**Proxy**, az összes hálózati csomagok folyamata hello Azure SQL Database átjárón keresztül. Ezt a beállítást, a tooallow kimenő tooonly hello Azure SQL Database átjáró IP kell. Beállítás használatával **Proxy** rendelkezik további késést biztosít beállítását **átirányítási**. 
- Ha a kapcsolat szabályzatot állítja **átirányítási**, az összes hálózati csomag flow közvetlenül toohello köztes proxy. Ezt a beállítást kell tooallow kimenő toomultiple IP-címek. 

## <a name="script-toochange-connection-settings"></a>Parancsfájl toochange kapcsolatbeállításai

> [!IMPORTANT]
> Ez a parancsfájl szükséges hello [Azure PowerShell modul](/powershell/azure/install-azurerm-ps).
>

a következő PowerShell-parancsfájl hello bemutatja, hogyan toochange hello kapcsolatkezelési házirendet.

```powershell
import-module azureRm
Login-AzureRmAccount

$tenantId =  #your AAD tenant ID
$subscriptionId = #Azure SubscriptionID
$uri = #AAD uri
$authUrl = "https://login.microsoftonline.com/$tenantId"
$serverName = #sqldb server name 
$resourceGroupName=#sqldb resource group
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl

$result = $AuthContext.AcquireToken("https://management.core.windows.net/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#getting hello current connection property
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting hello property too‘Proxy’
$connectionType=”Proxy” <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a>Következő lépések

- Hogyan toochange hello Azure SQL Database kapcsolatkezelési házirendet az Azure SQL Database-kiszolgáló információkért lásd: [REST API létrehozása vagy frissítési kiszolgáló kapcsolatkezelési házirendet az hello](https://msdn.microsoft.com/library/azure/mt604439.aspx).
- Információ az Azure SQL Database kapcsolat viselkedésről ADO.NET 4.5 vagy újabb verzióját használó ügyfelek számára, lásd: [kívüli ADO.NET 4.5 1433-as portokon](sql-database-develop-direct-route-ports-adonet-v12.md).
- Általános alkalmazás fejlesztési, témakör [SQL adatbázis alkalmazás fejlesztői áttekintés](sql-database-develop-overview.md).
