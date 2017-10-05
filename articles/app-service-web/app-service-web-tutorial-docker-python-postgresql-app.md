---
title: "Build egy Docker Python és PostgreSQL webalkalmazást az Azure-ban |} Microsoft Docs"
description: "Útmutató a Docker Python alkalmazást az Azure PostgreSQL adatbázis-kapcsolat használata."
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
editor: 
ms.assetid: 2bada123-ef18-44e5-be71-e16323b20466
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: e70f85a1eb4a6e1a81e0ca4fae228ca97deca6fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a><span data-ttu-id="6005f-103">Az Azure-ban Docker Python és PostgreSQL webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6005f-103">Build a Docker Python and PostgreSQL web app in Azure</span></span>

<span data-ttu-id="6005f-104">Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít.</span><span class="sxs-lookup"><span data-stu-id="6005f-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="6005f-105">Ez az oktatóanyag bemutatja, hogyan alapvető Docker Python-webalkalmazás létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6005f-105">This tutorial shows how to create a basic Docker Python web app in Azure.</span></span> <span data-ttu-id="6005f-106">Ez az alkalmazás PostgreSQL-adatbázishoz szeretne összekapcsolni.</span><span class="sxs-lookup"><span data-stu-id="6005f-106">You'll connect this app to a PostgreSQL database.</span></span> <span data-ttu-id="6005f-107">Amikor elkészült, konfigurálnia kell egy Python Flask-alkalmazás egy Docker-tároló belül futó [Azure App Service Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6005f-107">When you're done, you'll have a Python Flask application running within a Docker container on [Azure App Service Web Apps](app-service-web-overview.md).</span></span>

![Docker Python Flask-alkalmazás az Azure App Service-ben](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

<span data-ttu-id="6005f-109">A macOS hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6005f-109">You can follow the steps below on macOS.</span></span> <span data-ttu-id="6005f-110">Linux és Windows utasításokat legtöbbször azonos, de az eltéréseket nem részletes ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="6005f-110">Linux and Windows instructions are the same in most cases, but the differences are not detailed in this tutorial.</span></span>
 
## <a name="prerequisites"></a><span data-ttu-id="6005f-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6005f-111">Prerequisites</span></span>

<span data-ttu-id="6005f-112">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="6005f-112">To complete this tutorial:</span></span>

1. [<span data-ttu-id="6005f-113">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="6005f-113">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="6005f-114">Telepítse a Pythont</span><span class="sxs-lookup"><span data-stu-id="6005f-114">Install Python</span></span>](https://www.python.org/downloads/)
1. [<span data-ttu-id="6005f-115">Telepítheti és futtathatja PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6005f-115">Install and run PostgreSQL</span></span>](https://www.postgresql.org/download/)
1. [<span data-ttu-id="6005f-116">Telepítse a Docker Community Edition</span><span class="sxs-lookup"><span data-stu-id="6005f-116">Install Docker Community Edition</span></span>](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6005f-117">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="6005f-117">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="6005f-118">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="6005f-118">Run `az --version` to find the version.</span></span> <span data-ttu-id="6005f-119">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6005f-119">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-postgresql-installation-and-create-a-database"></a><span data-ttu-id="6005f-120">Adatbázis létrehozása és helyi PostgreSQL-telepítés sikerességének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6005f-120">Test local PostgreSQL installation and create a database</span></span>

<span data-ttu-id="6005f-121">Nyissa meg a terminálablakot, és futtassa `psql postgres` a helyi PostgreSQL-kiszolgálóhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="6005f-121">Open the terminal window and run `psql postgres` to connect to your local PostgreSQL server.</span></span>

```bash
psql postgres
```

<span data-ttu-id="6005f-122">Ha a kapcsolódás sikeres, a rendszer fut a PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="6005f-122">If your connection is successful, your PostgreSQL database is running.</span></span> <span data-ttu-id="6005f-123">Ha nem, győződjön meg arról, hogy a helyi PostgresQL-adatbázisban van-e indítva a következő lépéseket követve [letölti - PostgreSQL Core terjesztési](https://www.postgresql.org/download/).</span><span class="sxs-lookup"><span data-stu-id="6005f-123">If not, make sure that your local PostgresQL database is started by following the steps at [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).</span></span>

<span data-ttu-id="6005f-124">Adatbázis létrehozása *eventregistration* és nevű külön adatbázis-felhasználó beállítása *manager* jelszóval *supersecretpass*.</span><span class="sxs-lookup"><span data-stu-id="6005f-124">Create a database called *eventregistration* and set up a separate database user named *manager* with password *supersecretpass*.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
<span data-ttu-id="6005f-125">Típus *\q* való kilépéshez a PostgreSQL-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="6005f-125">Type *\q* to exit the PostgreSQL client.</span></span> 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a><span data-ttu-id="6005f-126">Helyi Python Flask-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6005f-126">Create local Python Flask application</span></span>

<span data-ttu-id="6005f-127">Ebben a lépésben beállíthatja a helyi Python Flask-projektet.</span><span class="sxs-lookup"><span data-stu-id="6005f-127">In this step, you set up the local Python Flask project.</span></span>

### <a name="clone-the-sample-application"></a><span data-ttu-id="6005f-128">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="6005f-128">Clone the sample application</span></span>

<span data-ttu-id="6005f-129">Nyissa meg a terminálablakot, és `CD` egy működő könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="6005f-129">Open the terminal window, and `CD` to a working directory.</span></span>  

<span data-ttu-id="6005f-130">A minta-tárház klónozása, és folytassa a következő parancsokat a *0,1-initialapp* kiadási.</span><span class="sxs-lookup"><span data-stu-id="6005f-130">Run the following commands to clone the sample repository and go to the *0.1-initialapp* release.</span></span>

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

<span data-ttu-id="6005f-131">Ez a minta-tárház tartalmaz egy [Flask](http://flask.pocoo.org/) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6005f-131">This sample repository contains a [Flask](http://flask.pocoo.org/) application.</span></span> 

### <a name="run-the-application"></a><span data-ttu-id="6005f-132">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="6005f-132">Run the application</span></span>

> [!NOTE] 
> <span data-ttu-id="6005f-133">Egy későbbi lépésben épület egy Docker-tároló, hogy az éles adatbázist használ, egyszerűsíti a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="6005f-133">In a later step you simplify this process by building a Docker container to use with the production database.</span></span>

<span data-ttu-id="6005f-134">Telepítse a szükséges csomagokat, és indítsa el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6005f-134">Install the required packages and start the application.</span></span>

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="6005f-135">Az alkalmazás teljesen betöltődik, lásd a következő üzenet hasonló:</span><span class="sxs-lookup"><span data-stu-id="6005f-135">When the app is fully loaded, you see something similar to the following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

<span data-ttu-id="6005f-136">Nyissa meg a böngészőben http://127.0.0.1:5000.</span><span class="sxs-lookup"><span data-stu-id="6005f-136">Navigate to http://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="6005f-137">Kattintson a **regisztrálni!**</span><span class="sxs-lookup"><span data-stu-id="6005f-137">Click **Register!**</span></span> <span data-ttu-id="6005f-138">és tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6005f-138">and create a test user.</span></span>

![Helyileg futó Python Flask-alkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

<span data-ttu-id="6005f-140">A Flask mintaalkalmazás felhasználói adatot tárol az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="6005f-140">The Flask sample application stores user data in the database.</span></span> <span data-ttu-id="6005f-141">Ha a felhasználó regisztrálása sikeres, az alkalmazás a helyi PostgreSQL-adatbázisból adatokat ír.</span><span class="sxs-lookup"><span data-stu-id="6005f-141">If you are successful at registering a user, your app is writing data to the local PostgreSQL database.</span></span>

<span data-ttu-id="6005f-142">A Flask kiszolgáló bármikor leállításához írja be a Terminálszolgáltatások Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="6005f-142">To stop the Flask server at anytime, type Ctrl+C in the terminal.</span></span> 

## <a name="create-a-production-postgresql-database"></a><span data-ttu-id="6005f-143">Hozzon létre egy PostgreSQL-adatbázisban</span><span class="sxs-lookup"><span data-stu-id="6005f-143">Create a production PostgreSQL database</span></span>

<span data-ttu-id="6005f-144">Ebben a lépésben létrehoz egy PostgreSQL-adatbázisban az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6005f-144">In this step, you create a PostgreSQL database in Azure.</span></span> <span data-ttu-id="6005f-145">Az alkalmazás telepítésekor az Azure-ba, a felhő adatbázist fogja használni.</span><span class="sxs-lookup"><span data-stu-id="6005f-145">When your app is deployed to Azure, it will use this cloud database.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="6005f-146">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="6005f-146">Log in to Azure</span></span>

<span data-ttu-id="6005f-147">Most kívánja használni az Azure CLI 2.0 létrehozása az Azure App Service-ben a Python-alkalmazás üzemeltetéséhez szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6005f-147">You are now going to use the Azure CLI 2.0 to create the resources needed to host your Python application in Azure App Service.</span></span>  <span data-ttu-id="6005f-148">Jelentkezzen be az Azure-előfizetésbe az [az login](/cli/azure/#login) paranccsal, és kövesse a képernyőn látható utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6005f-148">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="6005f-149">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="6005f-149">Create a resource group</span></span>

<span data-ttu-id="6005f-150">Hozzon létre egy [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md) az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="6005f-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with the [az group create](/cli/azure/group#create).</span></span> 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="6005f-151">A következő példa egy erőforráscsoportot az USA nyugati régiója régióban hoz létre:</span><span class="sxs-lookup"><span data-stu-id="6005f-151">The following example creates a resource group in the West US region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

<span data-ttu-id="6005f-152">Használja a [az App Service lista-helyek](/cli/azure/appservice#list-locations) lista elérhető helyről az Azure parancssori felület parancsot.</span><span class="sxs-lookup"><span data-stu-id="6005f-152">Use the [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command to list available locations.</span></span>

### <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="6005f-153">Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="6005f-153">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="6005f-154">Hozzon létre egy PostgreSQL-kiszolgáló ezzel a [az postgres kiszolgáló létrehozni](/cli/azure/documentdb#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6005f-154">Create a PostgreSQL server with the [az postgres server create](/cli/azure/documentdb#create) command.</span></span>

<span data-ttu-id="6005f-155">Az alábbi parancs helyettesítse be egy egyedi kiszolgálónevet számára a  *\<postgresql_name >* helyőrző és egy felhasználói nevet a  *\<admin_username >* helyőrző.</span><span class="sxs-lookup"><span data-stu-id="6005f-155">In the following command, substitute a unique server name for the *\<postgresql_name>* placeholder and a user name for the *\<admin_username>* placeholder.</span></span> <span data-ttu-id="6005f-156">A kiszolgáló nevét használja a PostgreSQL-végpontot részeként (`https://<postgresql_name>.postgres.database.azure.com`), így a nevének egyedinek kell lennie az Azure-ban minden kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6005f-156">The server name is used as part of your PostgreSQL endpoint (`https://<postgresql_name>.postgres.database.azure.com`), so the name needs to be unique across all servers in Azure.</span></span> <span data-ttu-id="6005f-157">A felhasználónév megadása a kezdeti adatbázis rendszergazdai felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="6005f-157">The user name is for the initial database admin user account.</span></span> <span data-ttu-id="6005f-158">Válassza ki a felhasználó jelszavát kéri.</span><span class="sxs-lookup"><span data-stu-id="6005f-158">You are prompted to pick a password for this user.</span></span>

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

<span data-ttu-id="6005f-159">PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis létrehozásakor az Azure parancssori felület kapcsolatos adatokat jeleníti meg az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="6005f-159">When the Azure Database for PostgreSQL server is created, the Azure CLI shows information similar to the following example:</span></span>

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a><span data-ttu-id="6005f-160">Hozzon létre egy tűzfalszabályt PostgreSQL-kiszolgáló Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="6005f-160">Create a firewall rule for the Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="6005f-161">A következő parancsot az Azure parancssori felület összes IP-címet az adatbázis hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6005f-161">Run the following Azure CLI command to allow access to the database from all IP addresses.</span></span>

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

<span data-ttu-id="6005f-162">Az Azure parancssori felület megerősíti, hogy a kimenet az alábbihoz hasonló tűzfalszabály létrehozása:</span><span class="sxs-lookup"><span data-stu-id="6005f-162">The Azure CLI confirms the firewall rule creation with output similar to the following example:</span></span>

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-to-the-database"></a><span data-ttu-id="6005f-163">Kapcsolódás az adatbázishoz a Python Flask-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="6005f-163">Connect your Python Flask application to the database</span></span>

<span data-ttu-id="6005f-164">Ebben a lépésben a Python Flask mintaalkalmazást PostgreSQL-kiszolgáló létrehozta az Azure-adatbázishoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="6005f-164">In this step, you connect your Python Flask sample application to the Azure Database for PostgreSQL server you created.</span></span>

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a><span data-ttu-id="6005f-165">Hozzon létre egy üres adatbázist, és állítson be egy új adatbázis-alkalmazás felhasználói</span><span class="sxs-lookup"><span data-stu-id="6005f-165">Create an empty database and set up a new database application user</span></span>

<span data-ttu-id="6005f-166">Hozzon létre egy adatbázis-felhasználó csak egy önálló adatbázis elérésére.</span><span class="sxs-lookup"><span data-stu-id="6005f-166">Create a database user with access to a single database only.</span></span> <span data-ttu-id="6005f-167">Elkerülése érdekében adjon az alkalmazás teljes hozzáférést a kiszolgáló ezeket a hitelesítő adatokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="6005f-167">You'll use these credentials to avoid giving the application full access to the server.</span></span>

<span data-ttu-id="6005f-168">Kapcsolódni az adatbázishoz (felszólítja a rendszergazdai jelszavát).</span><span class="sxs-lookup"><span data-stu-id="6005f-168">Connect to the database (you're prompted for your admin password).</span></span>

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

<span data-ttu-id="6005f-169">Az adatbázis és a felhasználó létrehozása a PostgreSQL parancssori.</span><span class="sxs-lookup"><span data-stu-id="6005f-169">Create the database and user from the PostgreSQL CLI.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

<span data-ttu-id="6005f-170">Típus *\q* való kilépéshez a PostgreSQL-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="6005f-170">Type *\q* to exit the PostgreSQL client.</span></span>

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a><span data-ttu-id="6005f-171">Az alkalmazás helyileg a Azure PostgreSQL-adatbázison tesztelése</span><span class="sxs-lookup"><span data-stu-id="6005f-171">Test the application locally against the Azure PostgreSQL database</span></span> 

<span data-ttu-id="6005f-172">Visszalépés most a *app* mappa a klónozott Github tárház, futtathatja a Python Flask-alkalmazás az adatbázis környezeti változók frissítésével.</span><span class="sxs-lookup"><span data-stu-id="6005f-172">Going back now to the *app* folder of the cloned Github repository, you can run the Python Flask application by updating the database environment variables.</span></span>

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="6005f-173">Az alkalmazás teljesen betöltődik, lásd a következő üzenet hasonló:</span><span class="sxs-lookup"><span data-stu-id="6005f-173">When the app is fully loaded, you see something similar to the following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

<span data-ttu-id="6005f-174">Nyissa meg a böngészőben http://127.0.0.1:5000.</span><span class="sxs-lookup"><span data-stu-id="6005f-174">Navigate to http://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="6005f-175">Kattintson a **regisztrálni!**</span><span class="sxs-lookup"><span data-stu-id="6005f-175">Click **Register!**</span></span> <span data-ttu-id="6005f-176">és egy tesztelési regisztráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6005f-176">and create a test registration.</span></span> <span data-ttu-id="6005f-177">Adatokat ír az adatbázisba az Azure-ban most.</span><span class="sxs-lookup"><span data-stu-id="6005f-177">You are now writing data to the database in Azure.</span></span>

![Helyileg futó Python Flask-alkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a><span data-ttu-id="6005f-179">Futtatni az alkalmazást egy Docker-tároló</span><span class="sxs-lookup"><span data-stu-id="6005f-179">Running the application from a Docker Container</span></span>

<span data-ttu-id="6005f-180">A Docker tároló lemezképet létre.</span><span class="sxs-lookup"><span data-stu-id="6005f-180">Build the Docker container image.</span></span>

```bash
cd ..
docker build -t flask-postgresql-sample .
```

<span data-ttu-id="6005f-181">Docker visszaigazolja sikeresen létrejött a tároló.</span><span class="sxs-lookup"><span data-stu-id="6005f-181">Docker displays a confirmation that it successfully created the container.</span></span>

```bash
Successfully built 7548f983a36b
```

<span data-ttu-id="6005f-182">Adatbázis környezeti változók hozzá egy környezeti változó fájl *db.env*.</span><span class="sxs-lookup"><span data-stu-id="6005f-182">Add database environment variables to an environment variable file *db.env*.</span></span> <span data-ttu-id="6005f-183">Az alkalmazás az Azure-ban PostgreSQL adatbázisba fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="6005f-183">The app will connect to the PostgreSQL production database in Azure.</span></span>

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

<span data-ttu-id="6005f-184">Futtassa az alkalmazást a belül a Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="6005f-184">Run the app from within the Docker container.</span></span> <span data-ttu-id="6005f-185">A következő parancsot a környezeti változó fájlt adja meg, és helyi port 5000 van leképezve az alapértelmezett Flask 5000-es port.</span><span class="sxs-lookup"><span data-stu-id="6005f-185">The following command specifies the environment variable file and maps the default Flask port 5000 to local port 5000.</span></span>

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

<span data-ttu-id="6005f-186">A kimeneti mi korábban látott hasonlít.</span><span class="sxs-lookup"><span data-stu-id="6005f-186">The output is similar to what you saw earlier.</span></span> <span data-ttu-id="6005f-187">A kezdeti adatbázis az áttelepítés azonban már nem hajtható végre, és ezért a rendszer kihagyja.</span><span class="sxs-lookup"><span data-stu-id="6005f-187">However, the initial database migration no longer needs to be performed and therefore is skipped.</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

<span data-ttu-id="6005f-188">Az adatbázis már tartalmaz a korábban létrehozott regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="6005f-188">The database already contains the registration you created previously.</span></span>

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a><span data-ttu-id="6005f-190">Töltse fel a Docker-tároló egy tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="6005f-190">Upload the Docker container to a container registry</span></span>

<span data-ttu-id="6005f-191">Ebben a lépésben a Docker-tároló egy tároló beállításjegyzék feltöltése.</span><span class="sxs-lookup"><span data-stu-id="6005f-191">In this step, you upload the Docker container to a container registry.</span></span> <span data-ttu-id="6005f-192">Azure-tároló beállításjegyzék fogja használni, de más például Docker Hub népszerű ők is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6005f-192">You'll use Azure Container Registry, but you could also use other popular ones such as Docker Hub.</span></span>

### <a name="create-an-azure-container-registry"></a><span data-ttu-id="6005f-193">Azure Container Registry létrehozása</span><span class="sxs-lookup"><span data-stu-id="6005f-193">Create an Azure Container Registry</span></span>

<span data-ttu-id="6005f-194">Cserélje le az alábbi parancs segítségével hozza létre a tároló beállításkulcs  *\<registry_name >* az Ön által választott egyedi Azure tároló beállításjegyzék néven.</span><span class="sxs-lookup"><span data-stu-id="6005f-194">In the following command to create a container registry replace *\<registry_name>* with a unique Azure container registry name of your choice.</span></span>

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

<span data-ttu-id="6005f-195">Kimenet</span><span class="sxs-lookup"><span data-stu-id="6005f-195">Output</span></span>
```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a><span data-ttu-id="6005f-196">A beállításjegyzék hitelesítő adatokat kérdez le, és húzza a Docker képek beolvasása</span><span class="sxs-lookup"><span data-stu-id="6005f-196">Retrieve the registry credentials for pushing and pulling Docker images</span></span>

<span data-ttu-id="6005f-197">Beállításjegyzék hitelesítő adatok megjelenítéséhez engedélyezéséhez először rendszergazdai módot.</span><span class="sxs-lookup"><span data-stu-id="6005f-197">To show registry credentials, enable admin mode first.</span></span>

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

<span data-ttu-id="6005f-198">Megjelenik a két jelszó.</span><span class="sxs-lookup"><span data-stu-id="6005f-198">You see two passwords.</span></span> <span data-ttu-id="6005f-199">Jegyezze meg a felhasználónevet és az első jelszót.</span><span class="sxs-lookup"><span data-stu-id="6005f-199">Make note of the user name and the first password.</span></span>

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-to-azure-container-registry"></a><span data-ttu-id="6005f-200">Töltse fel a Docker-tároló Azure tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="6005f-200">Upload your Docker container to Azure Container Registry</span></span>

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a><span data-ttu-id="6005f-201">Az Azure-bA a Docker Python Flask-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6005f-201">Deploy the Docker Python Flask application to Azure</span></span>

<span data-ttu-id="6005f-202">Ebben a lépésben alkalmazást telepít központilag a Docker tároló-alapú Python Flask Azure App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6005f-202">In this step, you deploy your Docker container-based Python Flask application to Azure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="6005f-203">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="6005f-203">Create an App Service plan</span></span>

<span data-ttu-id="6005f-204">Hozzon létre egy App Service-csomagot az [az appservice plan create](/cli/azure/appservice/plan#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="6005f-204">Create an App Service plan with the [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="6005f-205">Az alábbi példa létrehoz egy Linux-alapú App Service-csomag nevű *myAppServicePlan* használatával a S1 árképzési szintjüket:</span><span class="sxs-lookup"><span data-stu-id="6005f-205">The following example creates a Linux-based App Service plan named *myAppServicePlan* using the S1 pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

<span data-ttu-id="6005f-206">Az App Service-csomag létrehozásakor az Azure parancssori felület kapcsolatos adatokat jeleníti meg az alábbi példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="6005f-206">When the App Service plan is created, the Azure CLI shows information similar to the following example:</span></span>

```json 
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
``` 

### <a name="create-a-web-app"></a><span data-ttu-id="6005f-207">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6005f-207">Create a web app</span></span>

<span data-ttu-id="6005f-208">A webalkalmazás létrehozása a *myAppServicePlan* az App Service-csomag a [az webalkalmazás létrehozása](/cli/azure/webapp#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6005f-208">Create a web app in the *myAppServicePlan* App Service plan with the [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="6005f-209">A webes alkalmazás lehetővé teszi az üzemeltető adható meg a kód telepítésére, és biztosítja, hogy a telepített alkalmazás megtekintése egy URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6005f-209">The web app gives you a hosting space to deploy your code and provides a URL for you to view the deployed application.</span></span> <span data-ttu-id="6005f-210">A webalkalmazás létrehozásához használja.</span><span class="sxs-lookup"><span data-stu-id="6005f-210">Use  to create the web app.</span></span> 

<span data-ttu-id="6005f-211">A következő parancsban cserélje le a  *\<alkalmazás_neve >* helyőrzőt egy egyedi alkalmazásnevet.</span><span class="sxs-lookup"><span data-stu-id="6005f-211">In the following command, replace the *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="6005f-212">Ez a név része a web app alkalmazásban alapértelmezett URL-CÍMÉT, a nevének egyedinek kell lennie az Azure App Service-ben minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="6005f-212">This name is part of the default URL for the web app, so the name needs to be unique across all apps in Azure App Service.</span></span> 

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="6005f-213">A webalkalmazás létrehozása után az Azure CLI az alábbi példához hasonló információkat jelenít meg:</span><span class="sxs-lookup"><span data-stu-id="6005f-213">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span> 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-the-database-environment-variables"></a><span data-ttu-id="6005f-214">Konfigurálja az adatbázis környezeti változók</span><span class="sxs-lookup"><span data-stu-id="6005f-214">Configure the database environment variables</span></span>

<span data-ttu-id="6005f-215">Az oktatóanyag korábbi részében definiált környezeti változók a PostgreSQL-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="6005f-215">Earlier in the tutorial, you defined environment variables to connect to your PostgreSQL database.</span></span>

<span data-ttu-id="6005f-216">Az App Service-ben, a környezeti változók beállítása _Alkalmazásbeállítások_ használatával a [az webapp appsettings konfiguráció](/cli/azure/webapp/config#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6005f-216">In App Service, you set environment variables as _app settings_ by using the [az webapp config appsettings set](/cli/azure/webapp/config#set) command.</span></span> 

<span data-ttu-id="6005f-217">A következő példa az adatbázis-kapcsolat adatai Alkalmazásbeállítások adja meg.</span><span class="sxs-lookup"><span data-stu-id="6005f-217">The following example specifies the database connection details as app settings.</span></span> <span data-ttu-id="6005f-218">Is használja a *PORT* PORT 5000-leképezés változót a Docker-tároló, a 80-as PORT HTTP-forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="6005f-218">It also uses the *PORT* variable to map PORT 5000 from your Docker Container to receive HTTP traffic on PORT 80.</span></span>

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a><span data-ttu-id="6005f-219">Docker-tároló telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6005f-219">Configure Docker container deployment</span></span> 

<span data-ttu-id="6005f-220">App Service automatikusan letölteni és futtatni egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="6005f-220">AppService can automatically download and run a Docker container.</span></span>

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

<span data-ttu-id="6005f-221">Amikor a Docker-tároló frissítésére, vagy módosítsa a beállításokat, indítsa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6005f-221">Whenever you update the Docker container or change the settings, restart the app.</span></span> <span data-ttu-id="6005f-222">Újraindítása biztosítja, hogy minden beállítások érvényesek, és a legújabb tároló van lekért a beállításjegyzékből.</span><span class="sxs-lookup"><span data-stu-id="6005f-222">Restarting ensures that all settings are applied and the latest container is pulled from the registry.</span></span>

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="6005f-223">Keresse meg az Azure-webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="6005f-223">Browse to the Azure web app</span></span> 

<span data-ttu-id="6005f-224">Tallózással keresse meg a telepített webalkalmazás webböngészővel.</span><span class="sxs-lookup"><span data-stu-id="6005f-224">Browse to the deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> <span data-ttu-id="6005f-225">A webalkalmazás betölteni, mert a tároló rendelkezik letöltődnek és a tároló konfigurációjának módosítása után elindult hosszabb időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6005f-225">The web app takes longer to load because the container has to be downloaded and started after the container configuration is changed.</span></span>

<span data-ttu-id="6005f-226">Az éles Azure-adatbázishoz az előző lépésben mentett korábban regisztrált vendégek láthatja.</span><span class="sxs-lookup"><span data-stu-id="6005f-226">You see previously registered guests that were saved to the Azure production database in the previous step.</span></span>

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

<span data-ttu-id="6005f-228">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="6005f-228">**Congratulations!**</span></span> <span data-ttu-id="6005f-229">Futtatja egy Docker tároló-alapú Python Flask-alkalmazás az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="6005f-229">You're running a Docker container-based Python Flask app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="6005f-230">Frissítés adatmodell, és helyezze üzembe újra</span><span class="sxs-lookup"><span data-stu-id="6005f-230">Update data model and redeploy</span></span>

<span data-ttu-id="6005f-231">Ebben a lépésben hozzá a résztvevők száma minden eseményregisztráció vendég modell frissítésével.</span><span class="sxs-lookup"><span data-stu-id="6005f-231">In this step, you add the number of attendees to each event registration by updating the Guest model.</span></span>

<span data-ttu-id="6005f-232">Tekintse meg a *0,2-áttelepítési* kiadás az alábbi git-paranccsal:</span><span class="sxs-lookup"><span data-stu-id="6005f-232">Check out the *0.2-migration* release with the following git command:</span></span>

```bash
git checkout tags/0.2-migration
```

<span data-ttu-id="6005f-233">Ebben a kiadásban a nézeteket, a tartományvezérlőket és a modell már elvégezték a szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6005f-233">This release already made the necessary changes to views, controllers, and model.</span></span> <span data-ttu-id="6005f-234">Is keresztül létrehozott adatbázis áttelepítés *alembic* (`flask db migrate`).</span><span class="sxs-lookup"><span data-stu-id="6005f-234">It also includes a database migration generated via *alembic* (`flask db migrate`).</span></span> <span data-ttu-id="6005f-235">A következő git-parancs használatával végrehajtott valamennyi módosítást látható:</span><span class="sxs-lookup"><span data-stu-id="6005f-235">You can see all changes made via the following git command:</span></span>

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="6005f-236">A módosításokat a helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="6005f-236">Test your changes locally</span></span>

<span data-ttu-id="6005f-237">A következő parancsokat a módosítások ellenőrzéséhez helyileg a flask kiszolgáló futtatásával.</span><span class="sxs-lookup"><span data-stu-id="6005f-237">Run the following commands to test your changes locally by running the flask server.</span></span>

<span data-ttu-id="6005f-238">Mac / Linux:</span><span class="sxs-lookup"><span data-stu-id="6005f-238">Mac / Linux:</span></span>
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="6005f-239">Nyissa meg a böngészőben, hogy a változások http://127.0.0.1:5000.</span><span class="sxs-lookup"><span data-stu-id="6005f-239">Navigate to http://127.0.0.1:5000 in your browser to view the changes.</span></span> <span data-ttu-id="6005f-240">Hozzon létre egy teszt regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="6005f-240">Create a test registration.</span></span>

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a><span data-ttu-id="6005f-242">Változások közzétételére Azure</span><span class="sxs-lookup"><span data-stu-id="6005f-242">Publish changes to Azure</span></span>

<span data-ttu-id="6005f-243">Az új docker-lemezkép, hogy a tároló beállításjegyzék, és indítsa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6005f-243">Build the new docker image, push it to the container registry, and restart the app.</span></span>

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

<span data-ttu-id="6005f-244">Keresse meg az Azure-webalkalmazásban, és újra próbálja ki az új funkciókat.</span><span class="sxs-lookup"><span data-stu-id="6005f-244">Navigate to your Azure web app and try out the new functionality again.</span></span> <span data-ttu-id="6005f-245">Hozzon létre egy másik eseményregisztráció.</span><span class="sxs-lookup"><span data-stu-id="6005f-245">Create another event registration.</span></span>

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask-alkalmazás az Azure App Service-ben](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="6005f-247">Az Azure-webalkalmazásban kezelése</span><span class="sxs-lookup"><span data-stu-id="6005f-247">Manage your Azure web app</span></span>

<span data-ttu-id="6005f-248">Lépjen a [Azure-portálon](https://portal.azure.com) létrehozott webalkalmazás megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6005f-248">Go to the [Azure portal](https://portal.azure.com) to see the web app you created.</span></span>

<span data-ttu-id="6005f-249">A bal oldali menüben kattintson az **App Services** lehetőségre, majd az Azure-webapp nevére.</span><span class="sxs-lookup"><span data-stu-id="6005f-249">From the left menu, click **App Services**, then click the name of your Azure web app.</span></span>

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

<span data-ttu-id="6005f-251">Alapértelmezés szerint a portál megjeleníti a webalkalmazás **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="6005f-251">By default, the portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="6005f-252">Ezen az oldalon megtekintheti az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="6005f-252">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="6005f-253">Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="6005f-253">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="6005f-254">A lap bal oldalán a lapok megnyithatja a különböző konfigurációs lapok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="6005f-254">The tabs on the left side of the page show the different configuration pages you can open.</span></span>

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a><span data-ttu-id="6005f-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6005f-256">Next steps</span></span>

<span data-ttu-id="6005f-257">A következő oktatóanyag megtudhatja, hogyan képezheti egy egyéni DNS-nevet a webalkalmazás továbblépés.</span><span class="sxs-lookup"><span data-stu-id="6005f-257">Advance to the next tutorial to learn how to map a custom DNS name to your web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="6005f-258">Meglévő egyéni DNS-név hozzákapcsolása az Azure-webalkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="6005f-258">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
