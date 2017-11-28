---
title: "aaaConfigure hello szolgáltatás paraméterek Azure adatbázisban a következő PostgreSQL |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure hello szolgáltatás paraméterek az Azure Database PostgreSQL használatára vonatkozó hello Azure CLI parancssori."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 84a11de24ba87fc0eb6744aaa4b53f65a183903d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a><span data-ttu-id="6f1aa-103">Testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="6f1aa-103">Customize server configuration parameters using Azure CLI</span></span>
<span data-ttu-id="6f1aa-104">Listáról, megjelenítése és a konfigurációs paraméterek hello parancssori felület (Azure CLI) használatával Azure PostgreSQL-kiszolgáló frissítése.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-104">You can list, show and update configuration parameters for an Azure PostgreSQL server using hello Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="6f1aa-105">Azonban csak egy részhalmazát motor konfigurációk kiszolgálói szinten ki vannak téve, és módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-105">However, only a subset of engine configurations are exposed at server-level and can be modified.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6f1aa-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6f1aa-106">Prerequisites</span></span>
<span data-ttu-id="6f1aa-107">Ez hogyan tooguide keresztül toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="6f1aa-107">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="6f1aa-108">A kiszolgáló és az adatbázis [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="6f1aa-108">A server and database [Create an Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="6f1aa-109">Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) parancssori segédprogram, vagy használjon hello Azure Cloud rendszerhéj hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a><span data-ttu-id="6f1aa-110">Lista kiszolgáló konfigurációs paraméterek az Azure Database PostgreSQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="6f1aa-110">List server configuration parameters for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="6f1aa-111">toolist összes módosíthatóvá paraméter a kiszolgáló, az értékek, futtassa hello [az postgres kiszolgálólista konfigurációs](/cli/azure/postgres/server/configuration#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-111">toolist all modifiable parameters in a server and their values, run hello [az postgres server configuration list](/cli/azure/postgres/server/configuration#list) command.</span></span>

<span data-ttu-id="6f1aa-112">Hello kiszolgáló konfigurációs paraméterek hello kiszolgáló listázhatja **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup**.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-112">You can list hello server configuration parameters for hello server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup**.</span></span>
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a><span data-ttu-id="6f1aa-113">Kiszolgálókonfiguráció paraméter részletek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="6f1aa-113">Show server configuration parameter details</span></span>
<span data-ttu-id="6f1aa-114">egy adott konfigurációs paramétert egy kiszolgálón, futtassa a hello tooshow adatait [az postgres kiszolgáló konfigurációs megjelenítése](/cli/azure/postgres/server/configuration#show) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-114">tooshow details about a particular configuration parameter for a server, run hello [az postgres server configuration show](/cli/azure/postgres/server/configuration#show)  command.</span></span>

<span data-ttu-id="6f1aa-115">Ez a példa bemutatja hello részleteit **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** alatt Erőforráscsoport **contoso.com.**</span><span class="sxs-lookup"><span data-stu-id="6f1aa-115">This example shows details of hello **log\_min\_messages** server configuration parameter for server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a><span data-ttu-id="6f1aa-116">Módosítsa a kiszolgáló konfigurációs paraméter értéke</span><span class="sxs-lookup"><span data-stu-id="6f1aa-116">Modify server configuration parameter value</span></span>
<span data-ttu-id="6f1aa-117">Egy bizonyos kiszolgáló konfigurációs paraméter értékének hello is módosíthatja, és ekkor frissül az alapul szolgáló konfigurációs érték hello hello PostgreSQL server motor.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-117">You can also modify hello value of a certain server configuration parameter, and this updates hello underlying configuration value for hello PostgreSQL server engine.</span></span> <span data-ttu-id="6f1aa-118">tooupdate hello konfigurációs használata hello [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-118">tooupdate hello configuration use hello [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) command.</span></span> 

<span data-ttu-id="6f1aa-119">tooupdate hello **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportbatartozó**myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="6f1aa-119">tooupdate hello **log\_min\_messages** server configuration parameter of server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
<span data-ttu-id="6f1aa-120">Ha azt szeretné, hogy egy konfigurációs paraméter értékének tooreset hello, egyszerűen választja ki választható hello tooleave `--value` paraméter, és hello szolgáltatás érvényes hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-120">If you want tooreset hello value of a configuration parameter, you simply choose tooleave out hello optional `--value` parameter, and hello service will apply hello default value.</span></span> <span data-ttu-id="6f1aa-121">A fenti példában az alábbihoz hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="6f1aa-121">In above example, it would look like:</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="6f1aa-122">Ez a művelet visszaállítja hello **napló\_min\_üzenetek** konfigurációs toohello alapértelmezett értéke **figyelmeztetés**.</span><span class="sxs-lookup"><span data-stu-id="6f1aa-122">This will reset hello **log\_min\_messages** configuration toohello default value **WARNING**.</span></span> <span data-ttu-id="6f1aa-123">További részletek a kiszolgálókonfiguráció és a megengedett értékek, dokumentációjában PostgreSQL a [kiszolgálókonfiguráció](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span><span class="sxs-lookup"><span data-stu-id="6f1aa-123">For further details on server configuration and permissible values, see PostgreSQL documentation on [Server Configuration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f1aa-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6f1aa-124">Next steps</span></span>
- <span data-ttu-id="6f1aa-125">tooconfigure és hozzáférést kiszolgálónaplókban, lásd: [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="6f1aa-125">tooconfigure and access server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
