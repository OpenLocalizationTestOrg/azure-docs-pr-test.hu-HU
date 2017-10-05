---
title: "Állítsa be a paramétereket az Azure-adatbázis PostgreSQL |} Microsoft Docs"
description: "Ez a cikk ismerteti a paramétereket az Azure-adatbázis konfigurálása az Azure CLI parancssorból PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: c8a3b5a0225c2cede180d8d57681f2e1a6c6cc3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a><span data-ttu-id="c28fc-103">Testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="c28fc-103">Customize server configuration parameters using Azure CLI</span></span>
<span data-ttu-id="c28fc-104">Listáról, megjelenítése, és a parancssori felület (Azure CLI) használatával Azure PostgreSQL-kiszolgáló konfigurációs paraméterek frissítését.</span><span class="sxs-lookup"><span data-stu-id="c28fc-104">You can list, show and update configuration parameters for an Azure PostgreSQL server using the Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="c28fc-105">Azonban csak egy részhalmazát motor konfigurációk kiszolgálói szinten ki vannak téve, és módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="c28fc-105">However, only a subset of engine configurations are exposed at server-level and can be modified.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c28fc-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c28fc-106">Prerequisites</span></span>
<span data-ttu-id="c28fc-107">Ez az útmutató Útmutató lépéseit, az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="c28fc-107">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="c28fc-108">A kiszolgáló és az adatbázis [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="c28fc-108">A server and database [Create an Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="c28fc-109">Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) sor segédprogram parancsot, vagy használja az Azure-felhő rendszerhéj a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="c28fc-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use the Azure Cloud Shell in the browser.</span></span>

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a><span data-ttu-id="c28fc-110">Lista kiszolgáló konfigurációs paraméterek az Azure Database PostgreSQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="c28fc-110">List server configuration parameters for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="c28fc-111">Minden módosíthatóvá paraméter a kiszolgáló, az értékek listában, futtassa a [az postgres kiszolgálólista konfigurációs](/cli/azure/postgres/server/configuration#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c28fc-111">To list all modifiable parameters in a server and their values, run the [az postgres server configuration list](/cli/azure/postgres/server/configuration#list) command.</span></span>

<span data-ttu-id="c28fc-112">A kiszolgáló konfigurációs paraméterek, a kiszolgáló listázhatja **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup**.</span><span class="sxs-lookup"><span data-stu-id="c28fc-112">You can list the server configuration parameters for the server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup**.</span></span>
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a><span data-ttu-id="c28fc-113">Kiszolgálókonfiguráció paraméter részletek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="c28fc-113">Show server configuration parameter details</span></span>
<span data-ttu-id="c28fc-114">Egy adott konfigurációs paraméter a kiszolgáló részletei láthatók, futtassa a [az postgres kiszolgáló konfigurációs megjelenítése](/cli/azure/postgres/server/configuration#show) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c28fc-114">To show details about a particular configuration parameter for a server, run the [az postgres server configuration show](/cli/azure/postgres/server/configuration#show)  command.</span></span>

<span data-ttu-id="c28fc-115">Ez a példa bemutatja részleteit a **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **contoso.com.**</span><span class="sxs-lookup"><span data-stu-id="c28fc-115">This example shows details of the **log\_min\_messages** server configuration parameter for server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a><span data-ttu-id="c28fc-116">Módosítsa a kiszolgáló konfigurációs paraméter értéke</span><span class="sxs-lookup"><span data-stu-id="c28fc-116">Modify server configuration parameter value</span></span>
<span data-ttu-id="c28fc-117">Egy bizonyos kiszolgáló konfigurációs paraméter értéke is módosíthatja, és ezzel frissíti a PostgreSQL-kiszolgáló motor alapul szolgáló konfigurációs érték.</span><span class="sxs-lookup"><span data-stu-id="c28fc-117">You can also modify the value of a certain server configuration parameter, and this updates the underlying configuration value for the PostgreSQL server engine.</span></span> <span data-ttu-id="c28fc-118">Frissíteni a konfigurációt használja a [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c28fc-118">To update the configuration use the [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) command.</span></span> 

<span data-ttu-id="c28fc-119">Frissítése az **napló\_min\_üzenetek** kiszolgáló konfigurációs paraméter kiszolgáló **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **contoso.com.**</span><span class="sxs-lookup"><span data-stu-id="c28fc-119">To update the **log\_min\_messages** server configuration parameter of server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
<span data-ttu-id="c28fc-120">Ha szeretne egy konfigurációs paraméter értékének alaphelyzetbe állítása, egyszerűen választja hagyja ki az opcionális `--value` paraméter, és a szolgáltatás az alapértelmezett érték vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="c28fc-120">If you want to reset the value of a configuration parameter, you simply choose to leave out the optional `--value` parameter, and the service will apply the default value.</span></span> <span data-ttu-id="c28fc-121">A fenti példában az alábbihoz hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="c28fc-121">In above example, it would look like:</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="c28fc-122">Ez a művelet visszaállítja a **napló\_min\_üzenetek** konfigurációját, és az alapértelmezett érték **figyelmeztetés**.</span><span class="sxs-lookup"><span data-stu-id="c28fc-122">This will reset the **log\_min\_messages** configuration to the default value **WARNING**.</span></span> <span data-ttu-id="c28fc-123">További részletek a kiszolgálókonfiguráció és a megengedett értékek, dokumentációjában PostgreSQL a [kiszolgálókonfiguráció](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span><span class="sxs-lookup"><span data-stu-id="c28fc-123">For further details on server configuration and permissible values, see PostgreSQL documentation on [Server Configuration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c28fc-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c28fc-124">Next steps</span></span>
- <span data-ttu-id="c28fc-125">Konfigurálja, és hozzáférést kiszolgálónaplókban, [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="c28fc-125">To configure and access server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
