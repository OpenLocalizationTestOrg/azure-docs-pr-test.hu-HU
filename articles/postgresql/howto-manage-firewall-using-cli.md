---
title: "aaaCreate és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felületével |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate és kezelheti az Azure-adatbázis használata az Azure CLI parancssori PostgreSQL tűzfalszabályok."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 06e34c9e3996c2ec3df63191d794bc34d0ca0cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a><span data-ttu-id="69817-103">Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="69817-103">Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="69817-104">Kiszolgálószintű tűzfal-szabályokat engedélyezhetik a rendszergazdák toomanage hozzáférés tooan Azure adatbázis PostgreSQL-kiszolgáló egy adott IP-cím vagy az IP-címek.</span><span class="sxs-lookup"><span data-stu-id="69817-104">Server-level firewall rules enable administrators toomanage access tooan Azure Database for PostgreSQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="69817-105">Kényelmes Azure parancssori felület parancsait, segítségével létrehozása, frissítése, törlése, listában, és tűzfal-szabályok toomanage megjelenítése a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="69817-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules toomanage your server.</span></span> <span data-ttu-id="69817-106">Az áttekintést az Azure-adatbázis PostgreSQL tűzfalak, lásd: [PostgreSQL-kiszolgáló tűzfalszabályainak az Azure-adatbázis](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="69817-106">For an overview of Azure Database for PostgreSQL firewalls, see [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69817-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="69817-107">Prerequisites</span></span>
<span data-ttu-id="69817-108">Ez hogyan tooguide keresztül toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="69817-108">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="69817-109">Egy [PostgreSQL-kiszolgáló és adatbázis Azure-adatbázis](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="69817-109">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="69817-110">Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) parancssori segédprogram, vagy használjon hello Azure Cloud rendszerhéj hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="69817-110">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a><span data-ttu-id="69817-111">Tűzfalszabályok konfigurálása az Azure Database PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="69817-111">Configure firewall rules for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="69817-112">Hello [az postgres-tűzfalszabályt](/cli/azure/postgres/server/firewall-rule) parancsok is használt tooconfigure tűzfalszabályokat.</span><span class="sxs-lookup"><span data-stu-id="69817-112">hello [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule) commands are used tooconfigure firewall rules.</span></span>

## <a name="list-firewall-rules"></a><span data-ttu-id="69817-113">Tűzfalszabályok listája</span><span class="sxs-lookup"><span data-stu-id="69817-113">List firewall rules</span></span> 
<span data-ttu-id="69817-114">hello toolist hello meglévő kiszolgáló-tűzfalszabályok hello kiszolgálón futtassa [az postgres tűzfalszabály kiszolgálólista](/cli/azure/postgres/server/firewall-rule#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="69817-114">toolist hello existing server firewall rules on hello server, run hello [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule#list) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="69817-115">hello kimeneti hello szabályokat sorolja fel, ha vannak ilyenek, alapértelmezés szerint a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="69817-115">hello output lists hello rules if any, by default in JSON format.</span></span> <span data-ttu-id="69817-116">Hello kapcsoló segítségével `--output table` a tábla hello kimenetként olvashatóbb formátumban.</span><span class="sxs-lookup"><span data-stu-id="69817-116">You may use hello switch `--output table` for a more readable table format as hello output.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a><span data-ttu-id="69817-117">Tűzfalszabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="69817-117">Create firewall rule</span></span>
<span data-ttu-id="69817-118">egy új tűzfalszabály hello kiszolgálón, futtassa a hello toocreate [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="69817-118">toocreate a new firewall rule on hello server, run hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> 

<span data-ttu-id="69817-119">Ez a példa lehetővé teszi, hogy minden IP címek tooaccess hello kiszolgálók számos **mypgserver-20170401.postgres.database.azure.com**</span><span class="sxs-lookup"><span data-stu-id="69817-119">This example allows a range of all IP addresses tooaccess hello server **mypgserver-20170401.postgres.database.azure.com**</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
<span data-ttu-id="69817-120">tooallow egy egyes IP-cím tooaccess, adja meg ugyanazt a címet hello hello kezdő IP- és a záró IP-, ebben a példában szemléltetett módon.</span><span class="sxs-lookup"><span data-stu-id="69817-120">tooallow a singular IP address tooaccess, provide hello same address as hello Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
<span data-ttu-id="69817-121">Sikeres, akkor a hello parancs kimenetében létrehozott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="69817-121">Upon success, hello command output lists hello details of hello firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="69817-122">Ha hiba történik, a hello helyette kimeneti showserror üzenet szövegét.</span><span class="sxs-lookup"><span data-stu-id="69817-122">If there is a failure, hello output showserror message text instead.</span></span>

## <a name="update-firewall-rule"></a><span data-ttu-id="69817-123">Tűzfalszabály módosítása</span><span class="sxs-lookup"><span data-stu-id="69817-123">Update firewall rule</span></span> 
<span data-ttu-id="69817-124">Meglévő tűzfalszabály hello server használatával frissítse [az postgres server tűzfalszabály frissítés](/cli/azure/postgres/server/firewall-rule#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="69817-124">Update an existing firewall rule on hello server using [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule#update) command.</span></span> <span data-ttu-id="69817-125">Adja meg a meglévő tűzfalszabály hello bemeneti és a hello kezdő IP-cím és a záró IP attribútumok tooupdate hello nevét.</span><span class="sxs-lookup"><span data-stu-id="69817-125">Provide hello name of hello existing firewall rule as input, and hello start IP and end IP attributes tooupdate.</span></span>
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
<span data-ttu-id="69817-126">Sikeres, akkor a hello parancs kimenetében frissítette, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="69817-126">Upon success, hello command output lists hello details of hello firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="69817-127">Ha hiba történik, a hello helyette kimeneti showserror üzenet szövegét.</span><span class="sxs-lookup"><span data-stu-id="69817-127">If there is a failure, hello output showserror message text instead.</span></span>
> [!NOTE]
> <span data-ttu-id="69817-128">Ha hello tűzfalszabály nem létezik, az hello frissítés parancs végrehajtásakor létrejön.</span><span class="sxs-lookup"><span data-stu-id="69817-128">If hello firewall rule does not exist, it gets created by hello update command.</span></span>

## <a name="show-firewall-rule-details"></a><span data-ttu-id="69817-129">Tűzfal szabály részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="69817-129">Show firewall rule details</span></span>
<span data-ttu-id="69817-130">Is megjelenítheti hello meglévő tűzfal kiszolgáló részletei futtatásával [az postgres server tűzfalszabály megjelenítése](/cli/azure/postgres/server/firewall-rule#show) parancsot.</span><span class="sxs-lookup"><span data-stu-id="69817-130">You can also show hello existing firewall rule details for a server by running [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule#show) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="69817-131">Sikeres, akkor a hello parancs kimenetében megadott, alapértelmezés szerint a JSON formátum hello tűzfalszabály hello részleteit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="69817-131">Upon success, hello command output lists hello details of hello firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="69817-132">Ha hiba történik, a hello helyette kimeneti showserror üzenet szövegét.</span><span class="sxs-lookup"><span data-stu-id="69817-132">If there is a failure, hello output showserror message text instead.</span></span>

## <a name="delete-firewall-rule"></a><span data-ttu-id="69817-133">Tűzfalszabály törlése</span><span class="sxs-lookup"><span data-stu-id="69817-133">Delete firewall rule</span></span>
<span data-ttu-id="69817-134">toorevoke hozzáférést egy IP-címtartomány hello kiszolgálóról meglévő tűzfalszabály törlése a következő futtatásával hello [az postgres-tűzfalszabály törlése](/cli/azure/postgres/server/firewall-rule#delete) parancsot.</span><span class="sxs-lookup"><span data-stu-id="69817-134">toorevoke access for an IP range from hello server, delete an existing firewall rule by executing hello [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule#delete) command.</span></span> <span data-ttu-id="69817-135">Adja meg a meglévő tűzfalszabály hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="69817-135">Provide hello name of hello existing firewall rule.</span></span>
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="69817-136">Sikeres, akkor nincs nincs kimenete.</span><span class="sxs-lookup"><span data-stu-id="69817-136">Upon success, there is no output.</span></span> <span data-ttu-id="69817-137">Hiba esetén hello hibaüzenet-szöveg adja vissza.</span><span class="sxs-lookup"><span data-stu-id="69817-137">Upon failure, hello error message text is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69817-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69817-138">Next steps</span></span>
- <span data-ttu-id="69817-139">Hasonlóképpen, használhatja a webböngésző túl[hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok hello Azure-portál használatával](howto-manage-firewall-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="69817-139">Similarly, you can use a web browser too[Create and manage Azure Database for PostgreSQL firewall rules using hello Azure portal](howto-manage-firewall-using-portal.md)</span></span>
- <span data-ttu-id="69817-140">Több megismerkedett [PostgreSQL-kiszolgáló tűzfalszabályainak az Azure-adatbázis](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="69817-140">Understand more about [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>
- <span data-ttu-id="69817-141">Az Azure-adatbázis tooan PostgreSQL-kiszolgálóhoz csatlakozó útmutatásért lásd: [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="69817-141">For help in connecting tooan Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
