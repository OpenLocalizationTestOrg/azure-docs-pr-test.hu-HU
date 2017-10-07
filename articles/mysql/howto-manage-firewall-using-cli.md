---
title: "aaaCreate és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felületével |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate és kezelheti az Azure-adatbázis használata az Azure CLI parancssori MySQL tűzfalszabályok."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: b2f0b4ccf34ce88e3a5e72a64d3f8c78a5bc2a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a><span data-ttu-id="9f8e2-103">Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="9f8e2-103">Create and manage Azure Database for MySQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="9f8e2-104">Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák toomanage hozzáférés tooan Azure adatbázis MySQL-kiszolgáló egy adott IP-cím vagy az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-104">Server-level firewall rules enable administrators toomanage access tooan Azure Database for MySQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="9f8e2-105">Kényelmes Azure parancssori felület parancsait, segítségével létrehozása, frissítése, törlése, listában, és tűzfal-szabályok toomanage megjelenítése a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules toomanage your server.</span></span> <span data-ttu-id="9f8e2-106">Az áttekintést az Azure-adatbázis MySQL tűzfalak, lásd: [a MySQL-kiszolgáló tűzfalszabályainak Azure-adatbázis](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="9f8e2-106">For an overview of Azure Database for MySQL firewalls, see [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f8e2-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9f8e2-107">Prerequisites</span></span>
* [<span data-ttu-id="9f8e2-108">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="9f8e2-108">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)
* <span data-ttu-id="9f8e2-109">Telepítse az Azure Python SDK PostgreSQL és a MySQL-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9f8e2-109">Install Azure Python SDK for PostgreSQL and MySQL Services</span></span>
* <span data-ttu-id="9f8e2-110">Hello Azure CLI összetevő PostgreSQL és a MySQL-szolgáltatások telepítése</span><span class="sxs-lookup"><span data-stu-id="9f8e2-110">Install hello Azure CLI component for PostgreSQL and MySQL services</span></span>
* <span data-ttu-id="9f8e2-111">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="9f8e2-111">Create an Azure Database for MySQL server</span></span>

## <a name="firewall-rule-commands"></a><span data-ttu-id="9f8e2-112">A tűzfalszabály parancsok:</span><span class="sxs-lookup"><span data-stu-id="9f8e2-112">Firewall-Rule Commands:</span></span>
<span data-ttu-id="9f8e2-113">Hello **az mysql-tűzfalszabályt** a parancsnak az Azure CLI toocreate törlése, a listában, megjelenítése, és tűzfalszabályainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-113">hello **az mysql server firewall-rule** command is used from Azure CLI toocreate, delete, list, show, and update firewall rules.</span></span>

<span data-ttu-id="9f8e2-114">Parancsok:</span><span class="sxs-lookup"><span data-stu-id="9f8e2-114">Commands:</span></span>
- <span data-ttu-id="9f8e2-115">**Hozzon létre**: hozzon létre egy Azure-beli MySQL tűzfalszabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-115">**create**: Create an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="9f8e2-116">**Törlés**: az Azure-beli MySQL tűzfalszabály törlése.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-116">**delete**: Delete an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="9f8e2-117">**lista** : hello Azure MySQL kiszolgáló tűzfalszabályainak listában.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-117">**list** : List hello Azure MySQL server firewall rules.</span></span>
- <span data-ttu-id="9f8e2-118">**megjelenítése** : hello részletek az Azure-beli MySQL-kiszolgálók tűzfalszabály megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-118">**show** : Show hello details of an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="9f8e2-119">**frissítés**: az Azure-beli MySQL tűzfalszabály módosítása.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-119">**update**: Update an Azure MySQL server firewall rule.</span></span>

## <a name="login-tooazure-and-list-your-azure-database-for-mysql-servers"></a><span data-ttu-id="9f8e2-120">Bejelentkezési tooAzure és a MySQL-kiszolgálók az Azure-adatbázis listája</span><span class="sxs-lookup"><span data-stu-id="9f8e2-120">Login tooAzure and List your Azure Database for MySQL Servers</span></span>
<span data-ttu-id="9f8e2-121">Az Azure parancssori felület biztonságosan kapcsolódnak az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-121">Securely connect Azure CLI with your Azure account.</span></span> <span data-ttu-id="9f8e2-122">Használjon hello **az bejelentkezési** toodo ez parancsot.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-122">Use hello **az login** command toodo this.</span></span>

1. <span data-ttu-id="9f8e2-123">Futtassa a parancsot követő hello parancssorból hello.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-123">Run hello following command from hello command line.</span></span>
```azurecli
az login
```
<span data-ttu-id="9f8e2-124">Ez a parancs egy kódot toouse hello következő lépésben lesz kimeneti.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-124">This command will output a code toouse in hello next step.</span></span>

2. <span data-ttu-id="9f8e2-125">Egy webes böngésző tooopen hello lapon [https://aka.ms/devicelogin](https://aka.ms/devicelogin) , és írja be a kódját hello.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-125">Use a web browser tooopen hello page [https://aka.ms/devicelogin](https://aka.ms/devicelogin) and enter hello code.</span></span>

3. <span data-ttu-id="9f8e2-126">Hello parancssorba jelentkezzen be Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-126">At hello prompt, log in using your Azure credentials.</span></span>

4. <span data-ttu-id="9f8e2-127">A bejelentkezési azonosító jogosult, ha előfizetések listája kinyomtatja hello konzolon.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-127">Once your login is authorized, a list of subscriptions will be printed in hello console.</span></span> <span data-ttu-id="9f8e2-128">Másolja a szükséges hello előfizetés tooset hello aktuális előfizetés toobe használt soron hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-128">Copy hello id of hello desired subscription tooset hello current subscription toobe used moving forward.</span></span>
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. <span data-ttu-id="9f8e2-129">Ha bizonytalan hello nevek hello Azure adatbázisok előfizetésbe és erőforráscsoportba csoport MySQL-kiszolgálók listája</span><span class="sxs-lookup"><span data-stu-id="9f8e2-129">List hello Azure Databases for MySQL servers for your subscription and resource group if you are unsure of hello names.</span></span>

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   <span data-ttu-id="9f8e2-130">Vegye figyelembe a hello name attribútum hello listázása, ez az használt toospecify mely MySQL server toowork meg.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-130">Note hello name attribute in hello listing, which will be used toospecify which MySQL server toowork on.</span></span> <span data-ttu-id="9f8e2-131">Ha szükséges, az adott kiszolgáló toousing hello name attribútum tooconfirm nevének helyességéről, győződjön meg arról, hogy hello részletek:</span><span class="sxs-lookup"><span data-stu-id="9f8e2-131">If needed, confirm hello details for that server toousing hello name attribute tooconfirm name is correct:</span></span>

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a><span data-ttu-id="9f8e2-132">Tűzfalszabályok listája a MySQL-kiszolgálóhoz tartozó Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="9f8e2-132">List Firewall Rules on Azure Database for MySQL Server</span></span> 
<span data-ttu-id="9f8e2-133">Hello kiszolgálónevet és hello az erőforráscsoport neve, a lista hello meglévő kiszolgáló tűzfalszabályainak hello kiszolgálón használata.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-133">Using hello server name and hello resource group name, list hello existing server firewall rules on hello server.</span></span> <span data-ttu-id="9f8e2-134">Figyelje meg, hogy hello server name attribútum van megadva a hello **--kiszolgáló** váltson, és nem hello **--name** váltani.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-134">Notice that hello server name attribute is specified in hello **--server** switch and not hello **--name** switch.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
<span data-ttu-id="9f8e2-135">hello kimeneti hello szabályok felsorolja, ha vannak ilyenek, alapértelmezés szerint a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-135">hello output will list hello rules if any, by default in JSON format.</span></span> <span data-ttu-id="9f8e2-136">Hello kapcsoló segítségével **--eredménytábla** a tábla hello kimenetként olvashatóbb formátumban.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-136">You may use hello switch **--output table** for a more readable table format as hello output.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="9f8e2-137">Tűzfalszabály létrehozásához Azure-adatbázis a MySQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9f8e2-137">Create Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="9f8e2-138">Hello Azure-beli MySQL kiszolgáló nevét és az erőforráscsoport neve hello segítségével hozzon létre egy új tűzfalszabály hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-138">Using hello Azure MySQL server name and hello resource group name, create a new firewall rule on hello server.</span></span> <span data-ttu-id="9f8e2-139">Nevezze el a hello szabály, a hello kezdő IP- és a záró IP hello szabály toocover tooallow hozzáférést egy adott IP-címek számára.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-139">Provide a name for hello rule, hello start IP, and end IP for hello rule toocover a range of IP addresses tooallow access.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
<span data-ttu-id="9f8e2-140">Egyes IP címek toobe hozzáférése engedélyezett, adja meg ugyanazt a címet hello hello kezdő IP- és a záró IP-, ebben a példában szemléltetett módon.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-140">For a singular IP address toobe allowed access, provide hello same address as hello Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
<span data-ttu-id="9f8e2-141">Sikeres, akkor hello parancs kimenetében megjelennek létrehozott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-141">Upon success, hello command output will list hello details of hello firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="9f8e2-142">Ha hiba történik, hello kimenet megjeleníti hibaüzenet-szöveg helyette.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-142">If there is a failure, hello output will show error message text instead.</span></span>

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="9f8e2-143">Az Azure Database-tűzfalszabály MySQL-kiszolgáló frissítése</span><span class="sxs-lookup"><span data-stu-id="9f8e2-143">Update Firewall Rule on Azure Database for MySQL server</span></span> 
<span data-ttu-id="9f8e2-144">A hello Azure MySQL kiszolgálónevet és hello erőforráscsoport-név, frissítse a meglévő tűzfalszabály hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-144">Using hello Azure MySQL server name and hello resource group name, update an existing firewall rule on hello server.</span></span> <span data-ttu-id="9f8e2-145">Adja meg a meglévő tűzfalszabály hello bemeneti és a hello kezdő IP-cím és a záró IP attribútumok tooupdate hello nevét.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-145">Provide hello name of hello existing firewall rule as input, and hello start IP and end IP attributes tooupdate.</span></span>
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
<span data-ttu-id="9f8e2-146">Sikeres, akkor hello parancs kimenetében megjelennek frissítette, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-146">Upon success, hello command output will list hello details of hello firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="9f8e2-147">Ha hiba történik, hello kimenet megjeleníti hibaüzenet-szöveg helyette.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-147">If there is a failure, hello output will show error message text instead.</span></span>

> [!NOTE]
> <span data-ttu-id="9f8e2-148">Ha hello tűzfalszabály nem létezik, akkor hello frissítés parancs létrejön.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-148">If hello firewall rule does not exist, it will be created by hello update command.</span></span>

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a><span data-ttu-id="9f8e2-149">Azure-adatbázis a tűzfalszabály részletei MySQL kiszolgáló megjelenítése</span><span class="sxs-lookup"><span data-stu-id="9f8e2-149">Show Firewall Rule Details on Azure Database for MySQL Server</span></span>
<span data-ttu-id="9f8e2-150">A hello Azure MySQL kiszolgálónevet és hello erőforráscsoport-név, megjelenítése hello meglévő tűzfalszabály részletei hello kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-150">Using hello Azure MySQL server name and hello resource group name, show hello existing firewall rule details from hello server.</span></span> <span data-ttu-id="9f8e2-151">Meglévő tűzfalszabály hello hello neve meg bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-151">Provide hello name of hello existing firewall rule as input.</span></span>
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="9f8e2-152">Sikeres, akkor hello parancs kimenetében megjelennek megadott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-152">Upon success, hello command output will list hello details of hello firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="9f8e2-153">Ha hiba történik, hello kimenet megjeleníti hibaüzenet-szöveg helyette.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-153">If there is a failure, hello output will show error message text instead.</span></span>

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="9f8e2-154">Törli a tűzfalszabályt Azure-adatbázis a MySQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9f8e2-154">Delete Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="9f8e2-155">A hello Azure MySQL kiszolgálónevet és hello erőforráscsoport-név, távolítsa el a meglévő tűzfalszabály hello kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-155">Using hello Azure MySQL server name and hello resource group name, remove an existing firewall rule from hello server.</span></span> <span data-ttu-id="9f8e2-156">Adja meg a meglévő tűzfalszabály hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-156">Provide hello name of hello existing firewall rule.</span></span>
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="9f8e2-157">Sikeres, akkor nincs nincs kimenete.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-157">Upon success, there is no output.</span></span> <span data-ttu-id="9f8e2-158">Hiba esetén hello hibaüzenet-szöveg eredmény.</span><span class="sxs-lookup"><span data-stu-id="9f8e2-158">Upon failure, hello error message text will be returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f8e2-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f8e2-159">Next steps</span></span>
- <span data-ttu-id="9f8e2-160">Több megismerkedett [a MySQL-kiszolgáló tűzfalszabályainak Azure-adatbázis](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="9f8e2-160">Understand more about [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md)</span></span>
- [<span data-ttu-id="9f8e2-161">Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="9f8e2-161">Create and manage Azure Database for MySQL firewall rules using hello Azure portal</span></span>](./howto-manage-firewall-using-portal.md)
