---
title: "Azure parancssori felület használatával PostgreSQL aaaConfigure és hozzáférést kiszolgálónaplókban |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure és hozzáférési hello-kiszolgáló naplóit Azure-adatbázis használata az Azure CLI parancssori PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 924d4e7634cc48405bcb0f813cf61d8b72ad8aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a><span data-ttu-id="f7ede-103">Konfigurálja, és hozzáférést kiszolgálónaplókban olvashatók Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="f7ede-103">Configure and access server logs using Azure CLI</span></span>
<span data-ttu-id="f7ede-104">Hello PostgreSQL server hibanaplójában talál hello parancssori felület (Azure CLI) használatával töltheti le.</span><span class="sxs-lookup"><span data-stu-id="f7ede-104">You can download hello PostgreSQL server error logs using hello Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="f7ede-105">Hozzáférés tootransaction naplókat azonban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f7ede-105">However, access tootransaction logs is not supported.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="f7ede-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f7ede-106">Prerequisites</span></span>
<span data-ttu-id="f7ede-107">Ez hogyan tooguide keresztül toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="f7ede-107">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="f7ede-108">Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="f7ede-108">An [Azure Database for PostgreSQL server](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="f7ede-109">Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) parancssori segédprogram, vagy használjon hello Azure Cloud rendszerhéj hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="f7ede-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command-line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="configure-logging-for-azure-database-for-postgresql"></a><span data-ttu-id="f7ede-110">PostgreSQL az Azure-adatbázis naplózásának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f7ede-110">Configure logging for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="f7ede-111">Hello tooaccess lekérdezés naplói és a hibanaplókat is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="f7ede-111">You can configure hello server tooaccess query logs and error logs.</span></span> <span data-ttu-id="f7ede-112">A hibanaplókban automatikus vákuumszivattyú és a kapcsolat és az ellenőrzőpontok információkat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="f7ede-112">Error logs can contain auto-vacuum, connection, and checkpoints information.</span></span>
1. <span data-ttu-id="f7ede-113">Naplózás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="f7ede-113">Turn on logging</span></span>
2. <span data-ttu-id="f7ede-114">Update naplóját\_utasítás és a naplófájlok\_min\_időtartam\_utasítás tooenable lekérdezések naplózása</span><span class="sxs-lookup"><span data-stu-id="f7ede-114">Update log\_statement and log\_min\_duration\_statement tooenable query logging</span></span>
3. <span data-ttu-id="f7ede-115">Frissítse a megőrzési időtartam</span><span class="sxs-lookup"><span data-stu-id="f7ede-115">Update retention period</span></span>

<span data-ttu-id="f7ede-116">További információkért lásd: [kiszolgáló konfigurációs paraméterek testreszabása](howto-configure-server-parameters-using-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f7ede-116">For more information, see [customizing server configuration parameters](howto-configure-server-parameters-using-cli.md).</span></span>

## <a name="list-logs-for-azure-database-for-postgresql-server"></a><span data-ttu-id="f7ede-117">Lista naplók az Azure Database PostgreSQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f7ede-117">List logs for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="f7ede-118">toolist hello elérhető naplófájlokban található a kiszolgálón, futtassa a hello [az postgres server-naplók lista](/cli/azure/postgres/server-logs#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="f7ede-118">toolist hello available log files for your server, run hello [az postgres server-logs list](/cli/azure/postgres/server-logs#list) command.</span></span>

<span data-ttu-id="f7ede-119">Hello naplófájlok kiszolgáló listázhatja **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup**, és közvetlen azt tooa szöveg nevű **napló\_fájlok\_lista.txt.**</span><span class="sxs-lookup"><span data-stu-id="f7ede-119">You can list hello log files for server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup**, and direct it tooa text file called **log\_files\_list.txt.**</span></span>
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-hello-server"></a><span data-ttu-id="f7ede-120">Naplók letöltése helyileg hello kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="f7ede-120">Download logs locally from hello server</span></span>
<span data-ttu-id="f7ede-121">Hello [az postgres server-naplók letöltése](/cli/azure/postgres/server-logs#download) parancs toodownload külön naplófájlba a kiszolgáló lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="f7ede-121">hello [az postgres server-logs download](/cli/azure/postgres/server-logs#download) command allows you toodownload individual log files for your server.</span></span> 

<span data-ttu-id="f7ede-122">Ez a példa letöltések hello hello kiszolgáló adott naplófájl **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup** tooyour helyi környezetben.</span><span class="sxs-lookup"><span data-stu-id="f7ede-122">This example downloads hello specific log file for hello server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup** tooyour local environment.</span></span>
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a><span data-ttu-id="f7ede-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7ede-123">Next steps</span></span>
- <span data-ttu-id="f7ede-124">toolearn server-naplók kapcsolatos további információkért lásd: [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="f7ede-124">toolearn more about server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
- <span data-ttu-id="f7ede-125">A kiszolgáló paraméterek további információkért lásd: [testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával](howto-configure-server-parameters-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="f7ede-125">For more information on server parameters, see [Customize server configuration parameters using Azure CLI](howto-configure-server-parameters-using-cli.md)</span></span>
