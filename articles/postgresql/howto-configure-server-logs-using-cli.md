---
title: "Konfigurálja, és el a kiszolgáló naplóiban PostgreSQL Azure parancssori felületével |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan konfigurálja, és a kiszolgálói naplók az Azure-adatbázis el az Azure CLI parancssorból PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 26f8e12c493904f722cad5191ee053feff20f7fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a><span data-ttu-id="6f951-103">Konfigurálja, és hozzáférést kiszolgálónaplókban olvashatók Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="6f951-103">Configure and access server logs using Azure CLI</span></span>
<span data-ttu-id="6f951-104">Letöltheti a PostgreSQL hibanaplókat a parancssori felület (Azure CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="6f951-104">You can download the PostgreSQL server error logs using the Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="6f951-105">Tranzakciós naplók elérése azonban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6f951-105">However, access to transaction logs is not supported.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6f951-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6f951-106">Prerequisites</span></span>
<span data-ttu-id="6f951-107">Ez az útmutató Útmutató lépéseit, az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="6f951-107">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="6f951-108">Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="6f951-108">An [Azure Database for PostgreSQL server](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="6f951-109">Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) parancssori segédprogram, vagy használja az Azure-felhő rendszerhéj a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="6f951-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command-line utility or use the Azure Cloud Shell in the browser.</span></span>

## <a name="configure-logging-for-azure-database-for-postgresql"></a><span data-ttu-id="6f951-110">PostgreSQL az Azure-adatbázis naplózásának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6f951-110">Configure logging for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="6f951-111">A lekérdezés naplókat, valamint a hibanaplókat eléréséhez kiszolgálót úgy is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="6f951-111">You can configure the server to access query logs and error logs.</span></span> <span data-ttu-id="6f951-112">A hibanaplókban automatikus vákuumszivattyú és a kapcsolat és az ellenőrzőpontok információkat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="6f951-112">Error logs can contain auto-vacuum, connection, and checkpoints information.</span></span>
1. <span data-ttu-id="6f951-113">Naplózás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="6f951-113">Turn on logging</span></span>
2. <span data-ttu-id="6f951-114">Update naplóját\_utasítás és a naplófájlok\_min\_időtartam\_utasításban lekérdezés naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6f951-114">Update log\_statement and log\_min\_duration\_statement to enable query logging</span></span>
3. <span data-ttu-id="6f951-115">Frissítse a megőrzési időtartam</span><span class="sxs-lookup"><span data-stu-id="6f951-115">Update retention period</span></span>

<span data-ttu-id="6f951-116">További információkért lásd: [kiszolgáló konfigurációs paraméterek testreszabása](howto-configure-server-parameters-using-cli.md).</span><span class="sxs-lookup"><span data-stu-id="6f951-116">For more information, see [customizing server configuration parameters](howto-configure-server-parameters-using-cli.md).</span></span>

## <a name="list-logs-for-azure-database-for-postgresql-server"></a><span data-ttu-id="6f951-117">Lista naplók az Azure Database PostgreSQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="6f951-117">List logs for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="6f951-118">A kiszolgáló elérhető naplófájlokat sorolják fel, futtassa a [az postgres server-naplók lista](/cli/azure/postgres/server-logs#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6f951-118">To list the available log files for your server, run the [az postgres server-logs list](/cli/azure/postgres/server-logs#list) command.</span></span>

<span data-ttu-id="6f951-119">A kiszolgáló a naplófájlokban listázhatja **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup**, és közvetlen a nevű szövegfájlba **napló\_fájlok\_lista.txt.**</span><span class="sxs-lookup"><span data-stu-id="6f951-119">You can list the log files for server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup**, and direct it to a text file called **log\_files\_list.txt.**</span></span>
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a><span data-ttu-id="6f951-120">Naplók helyi letöltése a kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="6f951-120">Download logs locally from the server</span></span>
<span data-ttu-id="6f951-121">A [az postgres server-naplók letöltése](/cli/azure/postgres/server-logs#download) parancs lehetővé teszi, hogy töltse le a kiszolgáló külön naplófájlba.</span><span class="sxs-lookup"><span data-stu-id="6f951-121">The [az postgres server-logs download](/cli/azure/postgres/server-logs#download) command allows you to download individual log files for your server.</span></span> 

<span data-ttu-id="6f951-122">Ez a példa letölti az adott naplófájlban a kiszolgáló **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup** a helyi környezet.</span><span class="sxs-lookup"><span data-stu-id="6f951-122">This example downloads the specific log file for the server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup** to your local environment.</span></span>
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a><span data-ttu-id="6f951-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6f951-123">Next steps</span></span>
- <span data-ttu-id="6f951-124">A kiszolgáló naplóiban kapcsolatos további információkért lásd: [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="6f951-124">To learn more about server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
- <span data-ttu-id="6f951-125">A kiszolgáló paraméterek további információkért lásd: [testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával](howto-configure-server-parameters-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="6f951-125">For more information on server parameters, see [Customize server configuration parameters using Azure CLI](howto-configure-server-parameters-using-cli.md)</span></span>
