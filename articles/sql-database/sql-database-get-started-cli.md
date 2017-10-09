---
title: "Azure CLI: SQL-adatbázis létrehozása | Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy SQL Database logikai kiszolgálóhoz kiszolgálószintű tűzfalszabályt és használó adatbázisok hello Azure parancssori felület."
keywords: "oktatóanyag az SQL Database használatához, SQL-adatbázis létrehozása"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: 9b1ffb17eabeb70a000ff0c997128832b07aa4fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-hello-azure-cli"></a>Hozzon létre egy Azure SQL-adatbázis hello Azure parancssori felület használatával

hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez útmutató segítségével hello Azure CLI toodeploy részletei az Azure SQL-adatbázis egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md).

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez a témakör van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="define-variables"></a>Változók meghatározása

A gyors üzembe helyezési parancsfájlok hello változókat ad meg.

```azurecli-interactive
# hello data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = westeurope
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# hello ip address range that you want tooallow tooaccess your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# hello database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello segítségével [az csoport létrehozása](/cli/azure/group#create) parancsot. Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat. hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helyét.

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a>Hozzon létre egy logikai kiszolgálót

Hozzon létre egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md) hello segítségével [létrehozása az sql server](/cli/azure/sql/server#create) parancsot. A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel. hello alábbi példa hoz létre egy véletlenszerűen előállított nevű kiszolgálót az erőforráscsoportban egy rendszergazdai bejelentkezés nevű `ServerAdmin` és egy jelszót `ChangeYourAdminPassword1`. Igény szerint cserélje le ezeket az előre meghatározott értékeket.

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a>Konfiguráljon egy kiszolgálói tűzfalszabályt

Hozzon létre egy [Azure SQL Database kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) hello segítségével [létrehozása az sql server tűzfal](/cli/azure/sql/server/firewall-rule#create) parancsot. Egy kiszolgálószintű tűzfalszabályt lehetővé teszi, hogy a külső alkalmazás, például az SQL Server Management Studio vagy hello SQLCMD segédprogram tooconnect tooa SQL-adatbázis hello SQL Database szolgáltatás tűzfalon keresztül. A következő példa hello hello tűzfal van csak megnyitva más Azure-erőforrások. külső kapcsolatot tooenable, módosítás hello IP-cím tooan megfelelő címet a környezetben. tooopen összes IP-cím, IP-cím és 255.255.255.255 indítása zárócíme hello hello 0.0.0.0 használják.  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> Az SQL Database az 1433-as porton kommunikál. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát. Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg megnyitja az 1433-as port.
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a>Hozzon létre egy adatbázist mintaadatokkal hello kiszolgáló

Hozzon létre egy adatbázist, egy [S0 teljesítményszint szükséges](sql-database-service-tiers.md) hello segítségével hello Server [az sql-adatbázis létrehozása](/cli/azure/sql/db#create) parancsot. hello alábbi példa létrehoz egy nevű adatbázist `mySampleDatabase` és terhelések hello AdventureWorksLT mintaadatokat az adatbázisba. Cserélje le ezeket az előre megadott értékek a kívánt módon (a gyűjtemény build a gyors üzembe helyezési hello értékek esetén más gyors üzembe helyezések).

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épít. 

> [!TIP]
> Ha az ezt követő gyors üzembe helyezések toowork toocontinue tervezi, karbantartása hello erőforrások létrehozása a gyors indításának mellőzése. Ha nem tervezi toocontinue, használja a hello által a hello Azure-portálon a gyors üzembe helyezési lépéseket toodelete összes erőforrás létrehozása után.
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a>Következő lépések

Most, hogy rendelkezik egy adatbázissal, csatlakoztathatja a kedvenc eszközeit, és lekérdezéseket hajthat végre velük. További információkért válassza ki az eszközt az alábbiak közül:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

