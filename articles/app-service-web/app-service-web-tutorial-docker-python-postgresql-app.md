---
title: "aaaBuild egy Docker Python és PostgreSQL webalkalmazást az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooget a Docker Python-alkalmazások használata az Azure-ban, a kapcsolat tooa PostgreSQL-adatbázis használati ideje."
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
ms.openlocfilehash: e594ef9ec8c04ef2bf725e5f998691f3fb8cf815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a><span data-ttu-id="fd25b-103">Az Azure-ban Docker Python és PostgreSQL webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd25b-103">Build a Docker Python and PostgreSQL web app in Azure</span></span>

<span data-ttu-id="fd25b-104">Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít.</span><span class="sxs-lookup"><span data-stu-id="fd25b-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="fd25b-105">Ez az oktatóanyag bemutatja, hogyan toocreate egy alapszintű Docker Python webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fd25b-105">This tutorial shows how toocreate a basic Docker Python web app in Azure.</span></span> <span data-ttu-id="fd25b-106">Az alkalmazás tooa PostgreSQL-adatbázishoz fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="fd25b-106">You'll connect this app tooa PostgreSQL database.</span></span> <span data-ttu-id="fd25b-107">Amikor elkészült, konfigurálnia kell egy Python Flask-alkalmazás egy Docker-tároló belül futó [Azure App Service Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fd25b-107">When you're done, you'll have a Python Flask application running within a Docker container on [Azure App Service Web Apps](app-service-web-overview.md).</span></span>

![Docker Python Flask-alkalmazás az Azure App Service-ben](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

<span data-ttu-id="fd25b-109">A macOS hajtsa végre az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="fd25b-109">You can follow hello steps below on macOS.</span></span> <span data-ttu-id="fd25b-110">Linux és Windows utasításokat a legtöbb esetben hello azonos, de hello különbségek nem részletes ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="fd25b-110">Linux and Windows instructions are hello same in most cases, but hello differences are not detailed in this tutorial.</span></span>
 
## <a name="prerequisites"></a><span data-ttu-id="fd25b-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fd25b-111">Prerequisites</span></span>

<span data-ttu-id="fd25b-112">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="fd25b-112">toocomplete this tutorial:</span></span>

1. [<span data-ttu-id="fd25b-113">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="fd25b-113">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="fd25b-114">Telepítse a Pythont</span><span class="sxs-lookup"><span data-stu-id="fd25b-114">Install Python</span></span>](https://www.python.org/downloads/)
1. [<span data-ttu-id="fd25b-115">Telepítheti és futtathatja PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="fd25b-115">Install and run PostgreSQL</span></span>](https://www.postgresql.org/download/)
1. [<span data-ttu-id="fd25b-116">Telepítse a Docker Community Edition</span><span class="sxs-lookup"><span data-stu-id="fd25b-116">Install Docker Community Edition</span></span>](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fd25b-117">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fd25b-117">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="fd25b-118">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="fd25b-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="fd25b-119">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fd25b-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-postgresql-installation-and-create-a-database"></a><span data-ttu-id="fd25b-120">Adatbázis létrehozása és helyi PostgreSQL-telepítés sikerességének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fd25b-120">Test local PostgreSQL installation and create a database</span></span>

<span data-ttu-id="fd25b-121">Nyissa meg a hello terminálablakot és `psql postgres` tooconnect tooyour helyi PostgreSQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="fd25b-121">Open hello terminal window and run `psql postgres` tooconnect tooyour local PostgreSQL server.</span></span>

```bash
psql postgres
```

<span data-ttu-id="fd25b-122">Ha a kapcsolódás sikeres, a rendszer fut a PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="fd25b-122">If your connection is successful, your PostgreSQL database is running.</span></span> <span data-ttu-id="fd25b-123">Ha nem, győződjön meg arról, hogy elindult-e a helyi PostgresQL-adatbázisban: hello lépéseket követve [letölti - PostgreSQL Core terjesztési](https://www.postgresql.org/download/).</span><span class="sxs-lookup"><span data-stu-id="fd25b-123">If not, make sure that your local PostgresQL database is started by following hello steps at [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).</span></span>

<span data-ttu-id="fd25b-124">Adatbázis létrehozása *eventregistration* és nevű külön adatbázis-felhasználó beállítása *manager* jelszóval *supersecretpass*.</span><span class="sxs-lookup"><span data-stu-id="fd25b-124">Create a database called *eventregistration* and set up a separate database user named *manager* with password *supersecretpass*.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```
<span data-ttu-id="fd25b-125">Típus *\q* tooexit hello PostgreSQL-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="fd25b-125">Type *\q* tooexit hello PostgreSQL client.</span></span> 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a><span data-ttu-id="fd25b-126">Helyi Python Flask-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd25b-126">Create local Python Flask application</span></span>

<span data-ttu-id="fd25b-127">Ebben a lépésben beállíthatja hello helyi Python Flask-projektet.</span><span class="sxs-lookup"><span data-stu-id="fd25b-127">In this step, you set up hello local Python Flask project.</span></span>

### <a name="clone-hello-sample-application"></a><span data-ttu-id="fd25b-128">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="fd25b-128">Clone hello sample application</span></span>

<span data-ttu-id="fd25b-129">Nyissa meg hello terminálablakot, és `CD` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="fd25b-129">Open hello terminal window, and `CD` tooa working directory.</span></span>  

<span data-ttu-id="fd25b-130">Futtatási hello következő parancsok tooclone hello minta tárház, majd a toohello *0,1-initialapp* kiadási.</span><span class="sxs-lookup"><span data-stu-id="fd25b-130">Run hello following commands tooclone hello sample repository and go toohello *0.1-initialapp* release.</span></span>

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

<span data-ttu-id="fd25b-131">Ez a minta-tárház tartalmaz egy [Flask](http://flask.pocoo.org/) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd25b-131">This sample repository contains a [Flask](http://flask.pocoo.org/) application.</span></span> 

### <a name="run-hello-application"></a><span data-ttu-id="fd25b-132">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="fd25b-132">Run hello application</span></span>

> [!NOTE] 
> <span data-ttu-id="fd25b-133">Egy későbbi lépésben egy Docker-tároló toouse hello éles adatbázissal kiépítésével, egyszerűsíti a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="fd25b-133">In a later step you simplify this process by building a Docker container toouse with hello production database.</span></span>

<span data-ttu-id="fd25b-134">Telepítse a szükséges hello csomagok, és indítsa el a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd25b-134">Install hello required packages and start hello application.</span></span>

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="fd25b-135">Amikor hello alkalmazás teljesen betöltődik, valami hasonló toohello a következő üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fd25b-135">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="fd25b-136">Keresse meg a böngészőben toohttp://127.0.0.1:5000.</span><span class="sxs-lookup"><span data-stu-id="fd25b-136">Navigate toohttp://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="fd25b-137">Kattintson a **regisztrálni!**</span><span class="sxs-lookup"><span data-stu-id="fd25b-137">Click **Register!**</span></span> <span data-ttu-id="fd25b-138">és tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fd25b-138">and create a test user.</span></span>

![Helyileg futó Python Flask-alkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

<span data-ttu-id="fd25b-140">hello Flask mintaalkalmazás felhasználói adatok hello adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="fd25b-140">hello Flask sample application stores user data in hello database.</span></span> <span data-ttu-id="fd25b-141">Ha a felhasználó regisztrálása sikeres, az alkalmazás ír adatok toohello helyi PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="fd25b-141">If you are successful at registering a user, your app is writing data toohello local PostgreSQL database.</span></span>

<span data-ttu-id="fd25b-142">toostop hello Flask server bármikor, írja be a Terminálszolgáltatások hello Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="fd25b-142">toostop hello Flask server at anytime, type Ctrl+C in hello terminal.</span></span> 

## <a name="create-a-production-postgresql-database"></a><span data-ttu-id="fd25b-143">Hozzon létre egy PostgreSQL-adatbázisban</span><span class="sxs-lookup"><span data-stu-id="fd25b-143">Create a production PostgreSQL database</span></span>

<span data-ttu-id="fd25b-144">Ebben a lépésben létrehoz egy PostgreSQL-adatbázisban az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fd25b-144">In this step, you create a PostgreSQL database in Azure.</span></span> <span data-ttu-id="fd25b-145">Ha az alkalmazás telepített tooAzure, a felhő adatbázist fogja használni.</span><span class="sxs-lookup"><span data-stu-id="fd25b-145">When your app is deployed tooAzure, it will use this cloud database.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="fd25b-146">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="fd25b-146">Log in tooAzure</span></span>

<span data-ttu-id="fd25b-147">Biztos, hogy most a folyamatban toouse hello Azure CLI 2.0 toocreate hello erőforrások szükséges toohost a Python-alkalmazás az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="fd25b-147">You are now going toouse hello Azure CLI 2.0 toocreate hello resources needed toohost your Python application in Azure App Service.</span></span>  <span data-ttu-id="fd25b-148">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="fd25b-148">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="fd25b-149">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="fd25b-149">Create a resource group</span></span>

<span data-ttu-id="fd25b-150">Hozzon létre egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a hello [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="fd25b-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create).</span></span> 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="fd25b-151">hello alábbi példa létrehoz egy erőforráscsoport hello USA nyugati régiója régióban:</span><span class="sxs-lookup"><span data-stu-id="fd25b-151">hello following example creates a resource group in hello West US region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

<span data-ttu-id="fd25b-152">Használjon hello [az App Service lista-helyek](/cli/azure/appservice#list-locations) Azure CLI parancssori toolist elérhető helyről.</span><span class="sxs-lookup"><span data-stu-id="fd25b-152">Use hello [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command toolist available locations.</span></span>

### <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="fd25b-153">Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="fd25b-153">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="fd25b-154">Egy PostgreSQL-kiszolgáló létrehozása hello [az postgres kiszolgáló létrehozni](/cli/azure/documentdb#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="fd25b-154">Create a PostgreSQL server with hello [az postgres server create](/cli/azure/documentdb#create) command.</span></span>

<span data-ttu-id="fd25b-155">Hello a következő parancsot, helyettesítse egy egyedi kiszolgálónevet a hello  *\<postgresql_name >* helyőrző és egy felhasználónevet hello  *\<admin_username >* helyőrző .</span><span class="sxs-lookup"><span data-stu-id="fd25b-155">In hello following command, substitute a unique server name for hello *\<postgresql_name>* placeholder and a user name for hello *\<admin_username>* placeholder.</span></span> <span data-ttu-id="fd25b-156">hello kiszolgálónevet a PostgreSQL-végpontot részeként használatos (`https://<postgresql_name>.postgres.database.azure.com`), így a hello nevének kell toobe egyedi az Azure-ban minden kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="fd25b-156">hello server name is used as part of your PostgreSQL endpoint (`https://<postgresql_name>.postgres.database.azure.com`), so hello name needs toobe unique across all servers in Azure.</span></span> <span data-ttu-id="fd25b-157">hello felhasználónév hello kezdeti adatbázis rendszergazdai felhasználói fiók megadása.</span><span class="sxs-lookup"><span data-stu-id="fd25b-157">hello user name is for hello initial database admin user account.</span></span> <span data-ttu-id="fd25b-158">Biztosan felszólító toopick a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="fd25b-158">You are prompted toopick a password for this user.</span></span>

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

<span data-ttu-id="fd25b-159">Hello Azure adatbázis PostgreSQL-kiszolgáló létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="fd25b-159">When hello Azure Database for PostgreSQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="create-a-firewall-rule-for-hello-azure-database-for-postgresql-server"></a><span data-ttu-id="fd25b-160">Hozzon létre egy tűzfalszabályt hello Azure adatbázis PostgreSQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="fd25b-160">Create a firewall rule for hello Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="fd25b-161">Futtassa az Azure parancssori felület toohello parancs tooallow adatbázist az összes IP-cím a következő hello.</span><span class="sxs-lookup"><span data-stu-id="fd25b-161">Run hello following Azure CLI command tooallow access toohello database from all IP addresses.</span></span>

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

<span data-ttu-id="fd25b-162">hello Azure CLI megerősíti, hogy a hello tűzfalszabály létrehozása a kimeneti hasonló toohello a következő példa:</span><span class="sxs-lookup"><span data-stu-id="fd25b-162">hello Azure CLI confirms hello firewall rule creation with output similar toohello following example:</span></span>

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

## <a name="connect-your-python-flask-application-toohello-database"></a><span data-ttu-id="fd25b-163">Csatlakozás a Python Flask toohello adatbázis</span><span class="sxs-lookup"><span data-stu-id="fd25b-163">Connect your Python Flask application toohello database</span></span>

<span data-ttu-id="fd25b-164">Ebben a lépésben csatlakoztatja a Python Flask minta alkalmazás toohello Azure adatbázis létrehozott PostgreSQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="fd25b-164">In this step, you connect your Python Flask sample application toohello Azure Database for PostgreSQL server you created.</span></span>

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a><span data-ttu-id="fd25b-165">Hozzon létre egy üres adatbázist, és állítson be egy új adatbázis-alkalmazás felhasználói</span><span class="sxs-lookup"><span data-stu-id="fd25b-165">Create an empty database and set up a new database application user</span></span>

<span data-ttu-id="fd25b-166">Hozzon létre egy adatbázis-felhasználó tooa egyetlen adatbázist csak.</span><span class="sxs-lookup"><span data-stu-id="fd25b-166">Create a database user with access tooa single database only.</span></span> <span data-ttu-id="fd25b-167">Ezen hitelesítő adatok tooavoid jogosultságot ad a hello Alkalmazáskiszolgáló teljes körű hozzáférési toohello fogja használni.</span><span class="sxs-lookup"><span data-stu-id="fd25b-167">You'll use these credentials tooavoid giving hello application full access toohello server.</span></span>

<span data-ttu-id="fd25b-168">Csatlakozás toohello adatbázis (felszólítja a rendszergazdai jelszavát).</span><span class="sxs-lookup"><span data-stu-id="fd25b-168">Connect toohello database (you're prompted for your admin password).</span></span>

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

<span data-ttu-id="fd25b-169">Hello PostgreSQL CLI hello adatbázis és a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fd25b-169">Create hello database and user from hello PostgreSQL CLI.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```

<span data-ttu-id="fd25b-170">Típus *\q* tooexit hello PostgreSQL-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="fd25b-170">Type *\q* tooexit hello PostgreSQL client.</span></span>

### <a name="test-hello-application-locally-against-hello-azure-postgresql-database"></a><span data-ttu-id="fd25b-171">Helyi adatbázison hello Azure PostgreSQL hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="fd25b-171">Test hello application locally against hello Azure PostgreSQL database</span></span> 

<span data-ttu-id="fd25b-172">Ha visszalép most toohello *app* mappában található hello klónozott Github-tárházban, hello Python Flask-alkalmazást futtathatja hello adatbázis környezeti változók frissítésével.</span><span class="sxs-lookup"><span data-stu-id="fd25b-172">Going back now toohello *app* folder of hello cloned Github repository, you can run hello Python Flask application by updating hello database environment variables.</span></span>

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="fd25b-173">Amikor hello alkalmazás teljesen betöltődik, valami hasonló toohello a következő üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fd25b-173">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="fd25b-174">Keresse meg a böngészőben toohttp://127.0.0.1:5000.</span><span class="sxs-lookup"><span data-stu-id="fd25b-174">Navigate toohttp://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="fd25b-175">Kattintson a **regisztrálni!**</span><span class="sxs-lookup"><span data-stu-id="fd25b-175">Click **Register!**</span></span> <span data-ttu-id="fd25b-176">és egy tesztelési regisztráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fd25b-176">and create a test registration.</span></span> <span data-ttu-id="fd25b-177">Az Azure-ban most ír az toohello-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="fd25b-177">You are now writing data toohello database in Azure.</span></span>

![Helyileg futó Python Flask-alkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-hello-application-from-a-docker-container"></a><span data-ttu-id="fd25b-179">Hello alkalmazást futtat egy Docker-tárolójából.</span><span class="sxs-lookup"><span data-stu-id="fd25b-179">Running hello application from a Docker Container</span></span>

<span data-ttu-id="fd25b-180">Hello Docker-tároló lemezképet létre.</span><span class="sxs-lookup"><span data-stu-id="fd25b-180">Build hello Docker container image.</span></span>

```bash
cd ..
docker build -t flask-postgresql-sample .
```

<span data-ttu-id="fd25b-181">Docker visszaigazolja, hogy sikeresen létrejött informatikai hello tároló.</span><span class="sxs-lookup"><span data-stu-id="fd25b-181">Docker displays a confirmation that it successfully created hello container.</span></span>

```bash
Successfully built 7548f983a36b
```

<span data-ttu-id="fd25b-182">Adja hozzá a környezeti változók tooan környezeti változó adatbázisfájl *db.env*.</span><span class="sxs-lookup"><span data-stu-id="fd25b-182">Add database environment variables tooan environment variable file *db.env*.</span></span> <span data-ttu-id="fd25b-183">hello app toohello PostgreSQL éles adatbázis az Azure-ban fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="fd25b-183">hello app will connect toohello PostgreSQL production database in Azure.</span></span>

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

<span data-ttu-id="fd25b-184">Futtatta belül hello Docker-tároló hello alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="fd25b-184">Run hello app from within hello Docker container.</span></span> <span data-ttu-id="fd25b-185">hello következő parancs hello környezeti változó fájlt adja meg, és leképezi hello alapértelmezett Flask port 5000 toolocal 5000-es port.</span><span class="sxs-lookup"><span data-stu-id="fd25b-185">hello following command specifies hello environment variable file and maps hello default Flask port 5000 toolocal port 5000.</span></span>

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

<span data-ttu-id="fd25b-186">hello korábban látott hasonló toowhat eredménye.</span><span class="sxs-lookup"><span data-stu-id="fd25b-186">hello output is similar toowhat you saw earlier.</span></span> <span data-ttu-id="fd25b-187">Hello kezdeti adatbázis áttelepítés azonban már nincs szüksége a toobe végre, és ezért a rendszer kihagyja.</span><span class="sxs-lookup"><span data-stu-id="fd25b-187">However, hello initial database migration no longer needs toobe performed and therefore is skipped.</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="fd25b-188">hello adatbázis már tartalmazza a korábban létrehozott hello regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="fd25b-188">hello database already contains hello registration you created previously.</span></span>

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-hello-docker-container-tooa-container-registry"></a><span data-ttu-id="fd25b-190">Hello Docker tároló tooa tároló beállításjegyzék feltöltése</span><span class="sxs-lookup"><span data-stu-id="fd25b-190">Upload hello Docker container tooa container registry</span></span>

<span data-ttu-id="fd25b-191">Ebben a lépésben hello Docker tároló tooa tároló beállításjegyzék feltöltése.</span><span class="sxs-lookup"><span data-stu-id="fd25b-191">In this step, you upload hello Docker container tooa container registry.</span></span> <span data-ttu-id="fd25b-192">Azure-tároló beállításjegyzék fogja használni, de más például Docker Hub népszerű ők is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fd25b-192">You'll use Azure Container Registry, but you could also use other popular ones such as Docker Hub.</span></span>

### <a name="create-an-azure-container-registry"></a><span data-ttu-id="fd25b-193">Azure Container Registry létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd25b-193">Create an Azure Container Registry</span></span>

<span data-ttu-id="fd25b-194">Cserélje le a következő parancs toocreate tároló beállításjegyzékbeli hello  *\<registry_name >* az Ön által választott egyedi Azure tároló beállításjegyzék néven.</span><span class="sxs-lookup"><span data-stu-id="fd25b-194">In hello following command toocreate a container registry replace *\<registry_name>* with a unique Azure container registry name of your choice.</span></span>

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

<span data-ttu-id="fd25b-195">Kimenet</span><span class="sxs-lookup"><span data-stu-id="fd25b-195">Output</span></span>
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

### <a name="retrieve-hello-registry-credentials-for-pushing-and-pulling-docker-images"></a><span data-ttu-id="fd25b-196">Hello beállításjegyzék hitelesítő adatokat kérdez le, és húzza a Docker képek beolvasása</span><span class="sxs-lookup"><span data-stu-id="fd25b-196">Retrieve hello registry credentials for pushing and pulling Docker images</span></span>

<span data-ttu-id="fd25b-197">tooshow beállításjegyzék hitelesítő adatait, először engedélyezze a felügyeleti mód.</span><span class="sxs-lookup"><span data-stu-id="fd25b-197">tooshow registry credentials, enable admin mode first.</span></span>

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

<span data-ttu-id="fd25b-198">Megjelenik a két jelszó.</span><span class="sxs-lookup"><span data-stu-id="fd25b-198">You see two passwords.</span></span> <span data-ttu-id="fd25b-199">Jegyezze fel a hello felhasználónevet és hello első jelszót.</span><span class="sxs-lookup"><span data-stu-id="fd25b-199">Make note of hello user name and hello first password.</span></span>

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

### <a name="upload-your-docker-container-tooazure-container-registry"></a><span data-ttu-id="fd25b-200">A Docker-tároló tooAzure tároló beállításjegyzék feltöltése</span><span class="sxs-lookup"><span data-stu-id="fd25b-200">Upload your Docker container tooAzure Container Registry</span></span>

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-hello-docker-python-flask-application-tooazure"></a><span data-ttu-id="fd25b-201">Hello Docker Python Flask alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="fd25b-201">Deploy hello Docker Python Flask application tooAzure</span></span>

<span data-ttu-id="fd25b-202">Ebben a lépésben a Docker tároló-alapú Python Flask alkalmazás tooAzure App Service telepítése.</span><span class="sxs-lookup"><span data-stu-id="fd25b-202">In this step, you deploy your Docker container-based Python Flask application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="fd25b-203">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd25b-203">Create an App Service plan</span></span>

<span data-ttu-id="fd25b-204">Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="fd25b-204">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="fd25b-205">hello alábbi példa létrehoz egy Linux-alapú App Service-csomag nevű *myAppServicePlan* hello S1 IP-címek segítségével:</span><span class="sxs-lookup"><span data-stu-id="fd25b-205">hello following example creates a Linux-based App Service plan named *myAppServicePlan* using hello S1 pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

<span data-ttu-id="fd25b-206">Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="fd25b-206">When hello App Service plan is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="create-a-web-app"></a><span data-ttu-id="fd25b-207">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd25b-207">Create a web app</span></span>

<span data-ttu-id="fd25b-208">A webalkalmazás létrehozása az hello *myAppServicePlan* hello az App Service-csomag [az webalkalmazás létrehozása](/cli/azure/webapp#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="fd25b-208">Create a web app in hello *myAppServicePlan* App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="fd25b-209">hello web app ad meg egy üzemeltetési tér toodeploy a kódot, és egy URL-címet biztosít az Ön tooview hello telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd25b-209">hello web app gives you a hosting space toodeploy your code and provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="fd25b-210">Toocreate hello webes alkalmazás használata.</span><span class="sxs-lookup"><span data-stu-id="fd25b-210">Use  toocreate hello web app.</span></span> 

<span data-ttu-id="fd25b-211">A következő parancsot, hello hello helyébe  *\<alkalmazás_neve >* helyőrzőt egy egyedi alkalmazásnevet.</span><span class="sxs-lookup"><span data-stu-id="fd25b-211">In hello following command, replace hello *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="fd25b-212">Ez a név része hello alapértelmezett URL-cím hello webalkalmazás, így hello nevének kell toobe egyedi az Azure App Service-ben minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="fd25b-212">This name is part of hello default URL for hello web app, so hello name needs toobe unique across all apps in Azure App Service.</span></span> 

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="fd25b-213">Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="fd25b-213">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-hello-database-environment-variables"></a><span data-ttu-id="fd25b-214">Hello adatbázis környezeti változók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd25b-214">Configure hello database environment variables</span></span>

<span data-ttu-id="fd25b-215">Hello az oktatóanyagban a korábbi megadott környezeti változók tooconnect tooyour PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="fd25b-215">Earlier in hello tutorial, you defined environment variables tooconnect tooyour PostgreSQL database.</span></span>

<span data-ttu-id="fd25b-216">Az App Service-ben, a környezeti változók beállítása _Alkalmazásbeállítások_ hello segítségével [az webapp appsettings konfiguráció](/cli/azure/webapp/config#set) parancsot.</span><span class="sxs-lookup"><span data-stu-id="fd25b-216">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings set](/cli/azure/webapp/config#set) command.</span></span> 

<span data-ttu-id="fd25b-217">hello alábbi példa adja meg adatbázis-kapcsolat adatai hello Alkalmazásbeállítások.</span><span class="sxs-lookup"><span data-stu-id="fd25b-217">hello following example specifies hello database connection details as app settings.</span></span> <span data-ttu-id="fd25b-218">Ugyancsak hello *PORT* változó toomap PORTJÁVAL 5000 a Docker-tároló tooreceive HTTP-forgalmat a 80-as PORT.</span><span class="sxs-lookup"><span data-stu-id="fd25b-218">It also uses hello *PORT* variable toomap PORT 5000 from your Docker Container tooreceive HTTP traffic on PORT 80.</span></span>

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a><span data-ttu-id="fd25b-219">Docker-tároló telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd25b-219">Configure Docker container deployment</span></span> 

<span data-ttu-id="fd25b-220">App Service automatikusan letölteni és futtatni egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="fd25b-220">AppService can automatically download and run a Docker container.</span></span>

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

<span data-ttu-id="fd25b-221">Amikor a hello Docker-tároló frissítésére vagy hello beállításainak módosítása, indítsa újra a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fd25b-221">Whenever you update hello Docker container or change hello settings, restart hello app.</span></span> <span data-ttu-id="fd25b-222">Újraindítása biztosítja, hogy minden beállítások érvényesek, és hello legújabb tároló lekért hello beállításjegyzék van.</span><span class="sxs-lookup"><span data-stu-id="fd25b-222">Restarting ensures that all settings are applied and hello latest container is pulled from hello registry.</span></span>

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="fd25b-223">Keresse meg az Azure-webalkalmazásban toohello</span><span class="sxs-lookup"><span data-stu-id="fd25b-223">Browse toohello Azure web app</span></span> 

<span data-ttu-id="fd25b-224">Keresse meg a telepített toohello web app a webböngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="fd25b-224">Browse toohello deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> <span data-ttu-id="fd25b-225">hello webalkalmazás hosszabb tooload vesz igénybe, mert hello tároló toobe letöltődnek és működni kezdenek hello tároló konfigurációjának módosítása után.</span><span class="sxs-lookup"><span data-stu-id="fd25b-225">hello web app takes longer tooload because hello container has toobe downloaded and started after hello container configuration is changed.</span></span>

<span data-ttu-id="fd25b-226">Megjelenik a korábban regisztrált vendégek hello előző lépésben mentett toohello Azure éles adatbázist.</span><span class="sxs-lookup"><span data-stu-id="fd25b-226">You see previously registered guests that were saved toohello Azure production database in hello previous step.</span></span>

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

<span data-ttu-id="fd25b-228">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="fd25b-228">**Congratulations!**</span></span> <span data-ttu-id="fd25b-229">Futtatja egy Docker tároló-alapú Python Flask-alkalmazás az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="fd25b-229">You're running a Docker container-based Python Flask app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="fd25b-230">Frissítés adatmodell, és helyezze üzembe újra</span><span class="sxs-lookup"><span data-stu-id="fd25b-230">Update data model and redeploy</span></span>

<span data-ttu-id="fd25b-231">Ebben a lépésben a résztvevők tooeach eseményregisztráció hello száma hello vendég modell frissítésével hozzá.</span><span class="sxs-lookup"><span data-stu-id="fd25b-231">In this step, you add hello number of attendees tooeach event registration by updating hello Guest model.</span></span>

<span data-ttu-id="fd25b-232">Tekintse meg a hello *0,2-áttelepítési* kiadás az alábbi git parancs hello:</span><span class="sxs-lookup"><span data-stu-id="fd25b-232">Check out hello *0.2-migration* release with hello following git command:</span></span>

```bash
git checkout tags/0.2-migration
```

<span data-ttu-id="fd25b-233">Ebben a kiadásban már elvégezték a szükséges módosításokat tooviews, tartományvezérlők és modell hello.</span><span class="sxs-lookup"><span data-stu-id="fd25b-233">This release already made hello necessary changes tooviews, controllers, and model.</span></span> <span data-ttu-id="fd25b-234">Is keresztül létrehozott adatbázis áttelepítés *alembic* (`flask db migrate`).</span><span class="sxs-lookup"><span data-stu-id="fd25b-234">It also includes a database migration generated via *alembic* (`flask db migrate`).</span></span> <span data-ttu-id="fd25b-235">A következő git parancs hello keresztül végrehajtott valamennyi módosítást látható:</span><span class="sxs-lookup"><span data-stu-id="fd25b-235">You can see all changes made via hello following git command:</span></span>

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="fd25b-236">A módosításokat a helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="fd25b-236">Test your changes locally</span></span>

<span data-ttu-id="fd25b-237">Futtassa a következő parancsok tootest hello a módosítások helyileg futó hello flask kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="fd25b-237">Run hello following commands tootest your changes locally by running hello flask server.</span></span>

<span data-ttu-id="fd25b-238">Mac / Linux:</span><span class="sxs-lookup"><span data-stu-id="fd25b-238">Mac / Linux:</span></span>
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="fd25b-239">Keresse meg a böngésző tooview hello módosítások toohttp://127.0.0.1:5000.</span><span class="sxs-lookup"><span data-stu-id="fd25b-239">Navigate toohttp://127.0.0.1:5000 in your browser tooview hello changes.</span></span> <span data-ttu-id="fd25b-240">Hozzon létre egy teszt regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="fd25b-240">Create a test registration.</span></span>

![Docker tároló-alapú Python Flask helyileg futó alkalmazásba](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-tooazure"></a><span data-ttu-id="fd25b-242">Módosítások tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="fd25b-242">Publish changes tooAzure</span></span>

<span data-ttu-id="fd25b-243">Hello új docker lemezkép, toohello tároló beállításjegyzék leküldeni, és indítsa újra hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fd25b-243">Build hello new docker image, push it toohello container registry, and restart hello app.</span></span>

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

<span data-ttu-id="fd25b-244">Keresse meg az Azure-webalkalmazásban tooyour, és újra hello új funkcióinak kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="fd25b-244">Navigate tooyour Azure web app and try out hello new functionality again.</span></span> <span data-ttu-id="fd25b-245">Hozzon létre egy másik eseményregisztráció.</span><span class="sxs-lookup"><span data-stu-id="fd25b-245">Create another event registration.</span></span>

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask-alkalmazás az Azure App Service-ben](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="fd25b-247">Az Azure-webalkalmazásban kezelése</span><span class="sxs-lookup"><span data-stu-id="fd25b-247">Manage your Azure web app</span></span>

<span data-ttu-id="fd25b-248">Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toosee hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd25b-248">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span>

<span data-ttu-id="fd25b-249">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevére.</span><span class="sxs-lookup"><span data-stu-id="fd25b-249">From hello left menu, click **App Services**, then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

<span data-ttu-id="fd25b-251">Alapértelmezés szerint hello portal jeleníti meg a webalkalmazás **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="fd25b-251">By default, hello portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="fd25b-252">Ezen az oldalon megtekintheti az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="fd25b-252">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="fd25b-253">Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="fd25b-253">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="fd25b-254">hello lapok hello bal oldalán található hello hello különböző konfigurációs lapok megnyithatja megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="fd25b-254">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span>

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a><span data-ttu-id="fd25b-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd25b-256">Next steps</span></span>

<span data-ttu-id="fd25b-257">Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fd25b-257">Advance toohello next tutorial toolearn how toomap a custom DNS name tooyour web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="fd25b-258">Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése</span><span class="sxs-lookup"><span data-stu-id="fd25b-258">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
