---
title: "a Node.js és a MongoDB webalkalmazás az Azure-ban aaaBuild |} Microsoft Docs"
description: "Megtudhatja, hogyan tooget egy Node.js-alkalmazás az Azure-ban működik, a kapcsolat tooa Cosmos DB adatbázis MongoDB kapcsolati karakterláncot."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 532251c51ed6f8513e6e366393e889b67a85e5b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a><span data-ttu-id="913c5-103">Az Azure-ban Node.js és a MongoDB webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="913c5-103">Build a Node.js and MongoDB web app in Azure</span></span>

<span data-ttu-id="913c5-104">Az Azure Web Apps jól skálázható, önálló javítási a webhelyszolgáltató biztosít.</span><span class="sxs-lookup"><span data-stu-id="913c5-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="913c5-105">Ez az oktatóanyag bemutatja, hogyan toocreate egy Node.js webalkalmazás az Azure-ban, és csatlakoztassa tooa MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="913c5-105">This tutorial shows how toocreate a Node.js web app in Azure and connect it tooa MongoDB database.</span></span> <span data-ttu-id="913c5-106">Amikor elkészült, konfigurálnia kell egy átlagos alkalmazás (MongoDB, Express, AngularJS és Node.js) fut a [Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="913c5-106">When you're done, you'll have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running in [Azure App Service](app-service-web-overview.md).</span></span> <span data-ttu-id="913c5-107">Az egyszerűség kedvéért hello mintaalkalmazást használ hello [MEAN.js webes keretrendszer](http://meanjs.org/).</span><span class="sxs-lookup"><span data-stu-id="913c5-107">For simplicity, hello sample application uses hello [MEAN.js web framework](http://meanjs.org/).</span></span>

![Az Azure App Service-ben futó MEAN.js alkalmazás](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="913c5-109">Ismertetett témák:</span><span class="sxs-lookup"><span data-stu-id="913c5-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="913c5-110">MongoDB-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="913c5-110">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="913c5-111">Csatlakozás egy Node.js-alkalmazás tooMongoDB</span><span class="sxs-lookup"><span data-stu-id="913c5-111">Connect a Node.js app tooMongoDB</span></span>
> * <span data-ttu-id="913c5-112">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="913c5-112">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="913c5-113">Hello adatmodell frissítése, és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="913c5-113">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="913c5-114">Az adatfolyam diagnosztikai naplók az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="913c5-114">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="913c5-115">Az Azure-portálon hello hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="913c5-115">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="913c5-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="913c5-116">Prerequisites</span></span>

<span data-ttu-id="913c5-117">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="913c5-117">toocomplete this tutorial:</span></span>

1. [<span data-ttu-id="913c5-118">A Git telepítése</span><span class="sxs-lookup"><span data-stu-id="913c5-118">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="913c5-119">Telepítse a Node.js-t és az NPM-et</span><span class="sxs-lookup"><span data-stu-id="913c5-119">Install Node.js and NPM</span></span>](https://nodejs.org/)
1. <span data-ttu-id="913c5-120">[Telepítse a Gulp.js](http://gulpjs.com/) (által megkövetelt [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span><span class="sxs-lookup"><span data-stu-id="913c5-120">[Install Gulp.js](http://gulpjs.com/) (required by [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span></span>
1. [<span data-ttu-id="913c5-121">Telepítheti és futtathatja a MongoDB Community Edition</span><span class="sxs-lookup"><span data-stu-id="913c5-121">Install and run MongoDB Community Edition</span></span>](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="913c5-122">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="913c5-122">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="913c5-123">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="913c5-123">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="913c5-124">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="913c5-124">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-mongodb"></a><span data-ttu-id="913c5-125">Teszt helyi mongodb-Protokolltámogatással</span><span class="sxs-lookup"><span data-stu-id="913c5-125">Test local MongoDB</span></span>

<span data-ttu-id="913c5-126">Nyissa meg hello terminálablakot, és `cd` toohello `bin` a MongoDB telepítési könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="913c5-126">Open hello terminal window and `cd` toohello `bin` directory of your MongoDB installation.</span></span> <span data-ttu-id="913c5-127">A terminálablakot toorun összes hello parancsokat használhatja az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="913c5-127">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

<span data-ttu-id="913c5-128">Futtatás `mongo` a hello terminál tooconnect tooyour helyi MongoDB-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="913c5-128">Run `mongo` in hello terminal tooconnect tooyour local MongoDB server.</span></span>

```bash
mongo
```

<span data-ttu-id="913c5-129">Ha a kapcsolódás sikeres, majd a MongoDB adatbázis már fut.</span><span class="sxs-lookup"><span data-stu-id="913c5-129">If your connection is successful, then your MongoDB database is already running.</span></span> <span data-ttu-id="913c5-130">Ha nem, győződjön meg arról, hogy a helyi MongoDB adatbázis indította a következő hello készítésével [MongoDB Community Edition telepítése](https://docs.mongodb.com/manual/administration/install-community/).</span><span class="sxs-lookup"><span data-stu-id="913c5-130">If not, make sure that your local MongoDB database is started by following hello steps at [Install MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span></span> <span data-ttu-id="913c5-131">Gyakran, MongoDB telepítve van, de továbbra is szükséges toostart azt futtatásával `mongod`.</span><span class="sxs-lookup"><span data-stu-id="913c5-131">Often, MongoDB is installed, but you still need toostart it by running `mongod`.</span></span> 

<span data-ttu-id="913c5-132">Ha elkészült a MongoDB adatbázis tesztelése, írja be a `Ctrl+C` a Terminálszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="913c5-132">When you're done testing your MongoDB database, type `Ctrl+C` in hello terminal.</span></span> 

## <a name="create-local-nodejs-app"></a><span data-ttu-id="913c5-133">Helyi Node.js-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="913c5-133">Create local Node.js app</span></span>

<span data-ttu-id="913c5-134">Ebben a lépésben beállíthatja hello helyi Node.js-projektet.</span><span class="sxs-lookup"><span data-stu-id="913c5-134">In this step, you set up hello local Node.js project.</span></span>

### <a name="clone-hello-sample-application"></a><span data-ttu-id="913c5-135">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="913c5-135">Clone hello sample application</span></span>

<span data-ttu-id="913c5-136">A hello terminálablakot `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="913c5-136">In hello terminal window, `cd` tooa working directory.</span></span>  

<span data-ttu-id="913c5-137">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="913c5-137">Run hello following command tooclone hello sample repository.</span></span> 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

<span data-ttu-id="913c5-138">Ez a minta-tárház hello másolatát tartalmazza [MEAN.js tárház](https://github.com/meanjs/mean).</span><span class="sxs-lookup"><span data-stu-id="913c5-138">This sample repository contains a copy of hello [MEAN.js repository](https://github.com/meanjs/mean).</span></span> <span data-ttu-id="913c5-139">Az App Service módosított toorun (további információkért lásd: hello MEAN.js tárház [információs fájl](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span><span class="sxs-lookup"><span data-stu-id="913c5-139">It is modified toorun on App Service (for more information, see hello MEAN.js repository [README file](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span></span>

### <a name="run-hello-application"></a><span data-ttu-id="913c5-140">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="913c5-140">Run hello application</span></span>

<span data-ttu-id="913c5-141">Futtassa a következő parancsok tooinstall szükséges hello csomagok hello és hello alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="913c5-141">Run hello following commands tooinstall hello required packages and start hello application.</span></span>

```bash
cd meanjs
npm install
npm start
```

<span data-ttu-id="913c5-142">Amikor hello alkalmazás teljesen betöltődik, valami hasonló toohello a következő üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="913c5-142">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

<span data-ttu-id="913c5-143">Keresse meg a böngészőben toohttp://localhost:3000.</span><span class="sxs-lookup"><span data-stu-id="913c5-143">Navigate toohttp://localhost:3000 in a browser.</span></span> <span data-ttu-id="913c5-144">Kattintson a **regisztráció** a felső menüben hello és tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="913c5-144">Click **Sign Up** in hello top menu and create a test user.</span></span> 

<span data-ttu-id="913c5-145">hello MEAN.js mintaalkalmazás felhasználói adatok hello adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="913c5-145">hello MEAN.js sample application stores user data in hello database.</span></span> <span data-ttu-id="913c5-146">Ha a felhasználó létrehozásának és a bejelentkezés sikeres, majd az alkalmazás írása az adatok toohello helyi MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="913c5-146">If you are successful at creating a user and signing in, then your app is writing data toohello local MongoDB database.</span></span>

![MEAN.js sikeresen csatlakozik tooMongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

<span data-ttu-id="913c5-148">Válassza ki **Admin > kezelése cikkek** tooadd egyes cikkeket.</span><span class="sxs-lookup"><span data-stu-id="913c5-148">Select **Admin > Manage Articles** tooadd some articles.</span></span>

<span data-ttu-id="913c5-149">toostop Node.js bármikor, nyomja le az ENTER `Ctrl+C` a Terminálszolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="913c5-149">toostop Node.js at any time, press `Ctrl+C` in hello terminal.</span></span> 

## <a name="create-production-mongodb"></a><span data-ttu-id="913c5-150">Éles MongoDB létrehozása</span><span class="sxs-lookup"><span data-stu-id="913c5-150">Create production MongoDB</span></span>

<span data-ttu-id="913c5-151">Ebben a lépésben az Azure-ban MongoDB adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="913c5-151">In this step, you create a MongoDB database in Azure.</span></span> <span data-ttu-id="913c5-152">Ha az alkalmazás telepített tooAzure, használja a felhő adatbázis.</span><span class="sxs-lookup"><span data-stu-id="913c5-152">When your app is deployed tooAzure, it uses this cloud database.</span></span>

<span data-ttu-id="913c5-153">Ez az oktatóanyag használja a MongoDB, [Azure Cosmos DB](/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="913c5-153">For MongoDB, this tutorial uses [Azure Cosmos DB](/azure/documentdb/).</span></span> <span data-ttu-id="913c5-154">Cosmos DB támogatja a MongoDB-ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="913c5-154">Cosmos DB supports MongoDB client connections.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="913c5-155">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="913c5-155">Log in tooAzure</span></span>

<span data-ttu-id="913c5-156">Az Azure-ban hello Azure CLI 2.0 toocreate hello szükséges erőforrásokat toohost az alkalmazás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="913c5-156">You'll use hello Azure CLI 2.0 toocreate hello resources needed toohost your app in Azure.</span></span> <span data-ttu-id="913c5-157">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="913c5-157">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="913c5-158">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="913c5-158">Create a resource group</span></span>

<span data-ttu-id="913c5-159">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-159">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="913c5-160">hello alábbi példa létrehoz egy erőforráscsoport hello Nyugat-Európában régióban.</span><span class="sxs-lookup"><span data-stu-id="913c5-160">hello following example creates a resource group in hello West Europe region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

<span data-ttu-id="913c5-161">Használjon hello [az App Service lista-helyek](/cli/azure/appservice#list-locations) Azure CLI parancssori toolist elérhető helyről.</span><span class="sxs-lookup"><span data-stu-id="913c5-161">Use hello [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command toolist available locations.</span></span> 

### <a name="create-a-cosmos-db-account"></a><span data-ttu-id="913c5-162">A Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="913c5-162">Create a Cosmos DB account</span></span>

<span data-ttu-id="913c5-163">Hozzon létre egy Cosmos DB fiókot hello [az cosmosdb létrehozása](/cli/azure/cosmosdb#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-163">Create a Cosmos DB account with hello [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="913c5-164">Helyettesítse hello Cosmos DB egyedi nevet a következő parancsot, a hello  *\<cosmosdb_name >* helyőrző.</span><span class="sxs-lookup"><span data-stu-id="913c5-164">In hello following command, substitute a unique Cosmos DB name for hello *\<cosmosdb_name>* placeholder.</span></span> <span data-ttu-id="913c5-165">Ez a név hello Cosmos DB végpont, hello részeként használatos `https://<cosmosdb_name>.documents.azure.com/`, így a hello nevének kell toobe egyedi összes Cosmos DB fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="913c5-165">This name is used as hello part of hello Cosmos DB endpoint, `https://<cosmosdb_name>.documents.azure.com/`, so hello name needs toobe unique across all Cosmos DB accounts in Azure.</span></span> <span data-ttu-id="913c5-166">hello neve csak kisbetűket, számokat és hello kötőjel (-) karaktert kell tartalmaznia, és 3 – 50 karakter közé kell esnie.</span><span class="sxs-lookup"><span data-stu-id="913c5-166">hello name must contain only lowercase letters, numbers, and hello hyphen (-) character, and must be between 3 and 50 characters long.</span></span>

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

<span data-ttu-id="913c5-167">Hello *--kind MongoDB* paraméter lehetővé teszi, hogy a MongoDB-ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="913c5-167">hello *--kind MongoDB* parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="913c5-168">Hello Cosmos DB fiók létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="913c5-168">When hello Cosmos DB account is created, hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "consistencyPolicy":
  {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "failoverPolicies": 
  ...
  < Output truncated for readability >
}
```

## <a name="connect-app-tooproduction-mongodb"></a><span data-ttu-id="913c5-169">Kapcsolódás app tooproduction mongodb-Protokolltámogatással</span><span class="sxs-lookup"><span data-stu-id="913c5-169">Connect app tooproduction MongoDB</span></span>

<span data-ttu-id="913c5-170">Ebben a lépésben csatlakoztatja a MEAN.js alkalmazás toohello Cosmos DB mintaadatbázis imént létrehozott, a MongoDB-kapcsolati karakterlánc használatával.</span><span class="sxs-lookup"><span data-stu-id="913c5-170">In this step, you connect your MEAN.js sample application toohello Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

### <a name="retrieve-hello-database-key"></a><span data-ttu-id="913c5-171">Hello adatbázis-kulcs beolvasása</span><span class="sxs-lookup"><span data-stu-id="913c5-171">Retrieve hello database key</span></span>

<span data-ttu-id="913c5-172">tooconnect toohello Cosmos DB adatbázisban, be kell hello adatbázis kulcsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-172">tooconnect toohello Cosmos DB database, you need hello database key.</span></span> <span data-ttu-id="913c5-173">Használjon hello [az cosmosdb lista-kulcsok](/cli/azure/cosmosdb#list-keys) parancs tooretrieve hello elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="913c5-173">Use hello [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command tooretrieve hello primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

<span data-ttu-id="913c5-174">hello Azure CLI információk hasonló toohello a következő példa látható:</span><span class="sxs-lookup"><span data-stu-id="913c5-174">hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Másolja a hello értékének `primaryMasterKey`. <span data-ttu-id="913c5-176">A következő lépésben hello tájékoztatásra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="913c5-176">You need this information in hello next step.</span></span>

<a name="devconfig"></a>
### <a name="configure-hello-connection-string-in-your-nodejs-application"></a><span data-ttu-id="913c5-177">Node.js-alkalmazásában hello kapcsolati karakterlánc konfigurálása</span><span class="sxs-lookup"><span data-stu-id="913c5-177">Configure hello connection string in your Node.js application</span></span>

<span data-ttu-id="913c5-178">Nyissa meg a MEAN.js tárház _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="913c5-178">In your MEAN.js repository, open _config/env/production.js_.</span></span>

<span data-ttu-id="913c5-179">A hello `db` objektumazonosító, hello értékét `uri`:</span><span class="sxs-lookup"><span data-stu-id="913c5-179">In hello `db` object, update hello value of `uri`:</span></span>

* <span data-ttu-id="913c5-180">Cserélje le a két hello  *\<cosmosdb_name >* helyőrzőt a Cosmos DB adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="913c5-180">Replace hello two *\<cosmosdb_name>* placeholders with your Cosmos DB database name.</span></span>
* <span data-ttu-id="913c5-181">Cserélje le a hello  *\<primary_master_key >* helyőrző hello előző lépésben másolt hello kulccsal.</span><span class="sxs-lookup"><span data-stu-id="913c5-181">Replace hello *\<primary_master_key>* placeholder with hello key you copied in hello previous step.</span></span>

<span data-ttu-id="913c5-182">hello következő kód bemutatja hello `db` objektum:</span><span class="sxs-lookup"><span data-stu-id="913c5-182">hello following code shows hello `db` object:</span></span>

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

<span data-ttu-id="913c5-183">Hello `ssl=true` beállítást azért szükség, mert [Cosmos DB SSL megkövetelése](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="913c5-183">hello `ssl=true` option is required because [Cosmos DB requires SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 

<span data-ttu-id="913c5-184">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="913c5-184">Save your changes.</span></span>

### <a name="test-hello-application-in-production-mode"></a><span data-ttu-id="913c5-185">Hello alkalmazást tesztelheti éles módban</span><span class="sxs-lookup"><span data-stu-id="913c5-185">Test hello application in production mode</span></span> 

<span data-ttu-id="913c5-186">Futtassa a következő parancs toominify és köteg parancsfájlok hello éles környezet hello.</span><span class="sxs-lookup"><span data-stu-id="913c5-186">Run hello following command toominify and bundle scripts for hello production environment.</span></span> <span data-ttu-id="913c5-187">Ez a folyamat hello éles környezetben szükséges hello fájlokat generál.</span><span class="sxs-lookup"><span data-stu-id="913c5-187">This process generates hello files needed by hello production environment.</span></span>

```bash
gulp prod
```

<span data-ttu-id="913c5-188">Futtassa a következő parancs toouse hello kapcsolati karakterlánc konfigurált hello _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="913c5-188">Run hello following command toouse hello connection string you configured in _config/env/production.js_.</span></span>

```bash
NODE_ENV=production node server.js
```

<span data-ttu-id="913c5-189">`NODE_ENV=production`Beállítja a hello környezeti változó, amely közli a Node.js toorun hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="913c5-189">`NODE_ENV=production` sets hello environment variable that tells Node.js toorun in hello production environment.</span></span>  <span data-ttu-id="913c5-190">`node server.js`elindul a Node.js-kiszolgáló hello `server.js` az adattár gyökérkönyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="913c5-190">`node server.js` starts hello Node.js server with `server.js` in your repository root.</span></span> <span data-ttu-id="913c5-191">Ez az, hogy be van töltve a Node.js-alkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="913c5-191">This is how your Node.js application is loaded in Azure.</span></span> 

<span data-ttu-id="913c5-192">Hello app be van töltve, ellenőrizze, hogy fut-e hello éles környezetben toomake:</span><span class="sxs-lookup"><span data-stu-id="913c5-192">When hello app is loaded, check toomake sure that it's running in hello production environment:</span></span>

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

<span data-ttu-id="913c5-193">Keresse meg a böngészőben toohttp://localhost:8443.</span><span class="sxs-lookup"><span data-stu-id="913c5-193">Navigate toohttp://localhost:8443 in a browser.</span></span> <span data-ttu-id="913c5-194">Kattintson a **regisztráció** a felső menüben hello és tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="913c5-194">Click **Sign Up** in hello top menu and create a test user.</span></span> <span data-ttu-id="913c5-195">Ha sikeres felhasználó létrehozása, és jelentkezzen be, majd az alkalmazás ír az toohello Cosmos DB-adatbázist az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="913c5-195">If you are successful creating a user and signing in, then your app is writing data toohello Cosmos DB database in Azure.</span></span> 

<span data-ttu-id="913c5-196">A Terminálszolgáltatások hello, leállításához írja be a Node.js `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="913c5-196">In hello terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

## <a name="deploy-app-tooazure"></a><span data-ttu-id="913c5-197">Alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="913c5-197">Deploy app tooAzure</span></span>

<span data-ttu-id="913c5-198">Ebben a lépésben a MongoDB-csatlakoztatott Node.js-alkalmazás tooAzure App Service telepítése.</span><span class="sxs-lookup"><span data-stu-id="913c5-198">In this step, you deploy your MongoDB-connected Node.js application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="913c5-199">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="913c5-199">Create an App Service plan</span></span>

<span data-ttu-id="913c5-200">Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-200">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="913c5-201">hello alábbi példakód létrehozza az App Service-csomag nevű _myAppServicePlan_ hello segítségével **szabad** IP-címek:</span><span class="sxs-lookup"><span data-stu-id="913c5-201">hello following example creates an App Service plan named _myAppServicePlan_ using hello **FREE** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="913c5-202">Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="913c5-202">When hello App Service plan is created, hello Azure CLI shows information similar toohello following example:</span></span>

```json 
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
```

### <a name="create-a-web-app"></a><span data-ttu-id="913c5-203">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="913c5-203">Create a web app</span></span>

<span data-ttu-id="913c5-204">A webalkalmazás létrehozása az hello `myAppServicePlan` hello az App Service-csomag [az webalkalmazás létrehozása](/cli/azure/webapp#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-204">Create a web app in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="913c5-205">hello web app ad meg egy üzemeltetési tér toodeploy a kódot, és egy URL-címet biztosít az Ön tooview hello telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="913c5-205">hello web app gives you a hosting space toodeploy your code and provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="913c5-206">Toocreate hello webes alkalmazás használata.</span><span class="sxs-lookup"><span data-stu-id="913c5-206">Use  toocreate hello web app.</span></span> 

<span data-ttu-id="913c5-207">A következő parancsot, hello hello helyébe  *\<alkalmazás_neve >* helyőrzőt egy egyedi alkalmazásnevet.</span><span class="sxs-lookup"><span data-stu-id="913c5-207">In hello following command, replace hello *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="913c5-208">Ez a név csak használja hello alapértelmezett URL-cím hello részeként hello web app alkalmazásban így hello nevének kell toobe egyedi az Azure App Service-ben minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="913c5-208">This name is used as hello part of hello default URL for hello web app, so hello name needs toobe unique across all apps in Azure App Service.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="913c5-209">Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="913c5-209">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-an-environment-variable"></a><span data-ttu-id="913c5-210">Egy környezeti változó beállítása</span><span class="sxs-lookup"><span data-stu-id="913c5-210">Configure an environment variable</span></span>

<span data-ttu-id="913c5-211">A hello oktatóanyag, akkor szoftveresen kötött hello adatbázis-kapcsolati karakterláncot a korábbi _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="913c5-211">Earlier in hello tutorial, you hardcoded hello database connection string in _config/env/production.js_.</span></span> <span data-ttu-id="913c5-212">Biztonsági szempontból ajánlott összhangban kívánt tookeep a bizalmas adatokat a Git-tárház kívül.</span><span class="sxs-lookup"><span data-stu-id="913c5-212">In keeping with security best practice, you want tookeep this sensitive data out of your Git repository.</span></span> <span data-ttu-id="913c5-213">Az Azure-ban futó alkalmazások hanem egy környezeti változó fogja használni.</span><span class="sxs-lookup"><span data-stu-id="913c5-213">For your app running in Azure, you'll use an environment variable instead.</span></span>

<span data-ttu-id="913c5-214">Az App Service-ben, a környezeti változók beállítása _Alkalmazásbeállítások_ hello segítségével [az webapp config appsettings frissítése](/cli/azure/webapp/config/appsettings#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-214">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update) command.</span></span> 

<span data-ttu-id="913c5-215">hello alábbi példa konfigurál egy `MONGODB_URI` Alkalmazásbeállítás az Azure web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="913c5-215">hello following example configures a `MONGODB_URI` app setting in your Azure web app.</span></span> <span data-ttu-id="913c5-216">Cserélje le a hello  *\<alkalmazás_neve >*,  *\<cosmosdb_name >*, és  *\<primary_master_key >* helyőrzők.</span><span class="sxs-lookup"><span data-stu-id="913c5-216">Replace hello *\<app_name>*, *\<cosmosdb_name>*, and *\<primary_master_key>* placeholders.</span></span>

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

<span data-ttu-id="913c5-217">A Node.js-kódnak érheti el az alkalmazás setting `process.env.MONGODB_URI`, ahogy bármely környezeti változó elérésére.</span><span class="sxs-lookup"><span data-stu-id="913c5-217">In Node.js code, you access this app setting with `process.env.MONGODB_URI`, just like you would access any environment variable.</span></span> 

<span data-ttu-id="913c5-218">Most visszavonása a változtatások too_config/env/production.js_ a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="913c5-218">Now, undo your changes too_config/env/production.js_ with hello following command:</span></span>

```bash
git checkout -- .
```

<span data-ttu-id="913c5-219">Nyissa meg _config/env/production.js_ újra.</span><span class="sxs-lookup"><span data-stu-id="913c5-219">Open _config/env/production.js_ again.</span></span> <span data-ttu-id="913c5-220">Vegye figyelembe, hogy hello alapértelmezett MEAN.js alkalmazás már konfigurált toouse hello `MONGODB_URI` létrehozott környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="913c5-220">Note that hello default MEAN.js app is already configured toouse hello `MONGODB_URI` environment variable that you created.</span></span>

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a><span data-ttu-id="913c5-221">A Git helyi üzemelő példányának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="913c5-221">Configure local git deployment</span></span> 

<span data-ttu-id="913c5-222">Használjon hello [beállítva az az webalkalmazás üzembe helyező felhasználó](/cli/azure/webapp/deployment/user#set) toocreate hitelesítő adatokat a központi telepítési parancsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-222">Use hello [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command toocreate credentials for deployment.</span></span>

<span data-ttu-id="913c5-223">Az alkalmazás tooAzure App Service többek között az FTP, a helyi Git, a GitHub, a Visual Studio Team Services és a Bitbucketből különböző módokon telepítheti.</span><span class="sxs-lookup"><span data-stu-id="913c5-223">You can deploy your application tooAzure App Service in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="913c5-224">Az FTP és a helyi Git esetében szükség a központi telepítés hello server tooauthenticate konfigurált toohave a központi felhasználói.</span><span class="sxs-lookup"><span data-stu-id="913c5-224">For FTP and local Git, it is necessary toohave a deployment user configured on hello server tooauthenticate your deployment.</span></span> <span data-ttu-id="913c5-225">A központi felhasználói fiók szintű pedig különböző Azure-előfizetés fiókjából.</span><span class="sxs-lookup"><span data-stu-id="913c5-225">This deployment user is account-level and is different from your Azure subscription account.</span></span> <span data-ttu-id="913c5-226">Csak akkor kell tooconfigure a központi felhasználói egyszer.</span><span class="sxs-lookup"><span data-stu-id="913c5-226">You only need tooconfigure this deployment user once.</span></span>

<span data-ttu-id="913c5-227">Hello a következő parancsban cserélje le  *\<felhasználónév->* és  *\<jelszó >* új felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="913c5-227">In hello following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="913c5-228">hello felhasználó nevének egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="913c5-228">hello user name must be unique.</span></span> <span data-ttu-id="913c5-229">hello jelszó legalább 8 karakter hosszúságúnak kell lennie, és két a következő három elemek hello: betűket, számokat, szimbólumokat.</span><span class="sxs-lookup"><span data-stu-id="913c5-229">hello password must be at least eight characters long, with two of hello following three elements:  letters, numbers, symbols.</span></span> <span data-ttu-id="913c5-230">Ha egy ` 'Conflict'. Details: 409` hiba, a módosítás hello felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="913c5-230">If you get a ` 'Conflict'. Details: 409` error, change hello username.</span></span> <span data-ttu-id="913c5-231">` 'Bad Request'. Details: 400` hibaüzenet esetén használjon erősebb jelszót.</span><span class="sxs-lookup"><span data-stu-id="913c5-231">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="913c5-232">Rekord hello felhasználónév és jelszó használható a későbbi lépésekben hello alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="913c5-232">Record hello user name and password for use in later steps when you deploy hello app.</span></span>

<span data-ttu-id="913c5-233">Használjon hello [az webalkalmazás központi telepítési forrás config-helyi-git](/cli/azure/webapp/deployment/source#config-local-git) parancs tooconfigure helyi Git hozzáférés toohello Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="913c5-233">Use hello [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command tooconfigure local Git access toohello Azure web app.</span></span> 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

<span data-ttu-id="913c5-234">Hello központi felhasználói van konfigurálva, hello Azure CLI formátuma a következő hello hello telepítés URL-CÍMÉT az Azure webalkalmazás szerepel:</span><span class="sxs-lookup"><span data-stu-id="913c5-234">When hello deployment user is configured, hello Azure CLI shows hello deployment URL for your Azure web app in hello following format:</span></span>

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

<span data-ttu-id="913c5-235">Másolja át a hello terminálról kimeneti hello hello következő lépésben lesz használható.</span><span class="sxs-lookup"><span data-stu-id="913c5-235">Copy hello output from hello terminal, as it will be used in hello next step.</span></span> 

### <a name="push-tooazure-from-git"></a><span data-ttu-id="913c5-236">A Git Push tooAzure</span><span class="sxs-lookup"><span data-stu-id="913c5-236">Push tooAzure from Git</span></span>

<span data-ttu-id="913c5-237">Adjon hozzá egy helyi Git-tárház Azure távoli tooyour.</span><span class="sxs-lookup"><span data-stu-id="913c5-237">Add an Azure remote tooyour local Git repository.</span></span> 

```bash
git remote add azure <paste_copied_url_here> 
```

<span data-ttu-id="913c5-238">Azure távoli toodeploy toohello a Node.js-alkalmazás leküldési.</span><span class="sxs-lookup"><span data-stu-id="913c5-238">Push toohello Azure remote toodeploy your Node.js application.</span></span> <span data-ttu-id="913c5-239">A korábbi hello létrehozása központi felhasználói hello részeként megadott hello jelszó bekéri.</span><span class="sxs-lookup"><span data-stu-id="913c5-239">You will be prompted for hello password you supplied earlier as part of hello creation of hello deployment user.</span></span> 

```bash
git push azure master
```

<span data-ttu-id="913c5-240">A telepítés során az Azure App Service a telepítés előrehaladását a Git kommunikál.</span><span class="sxs-lookup"><span data-stu-id="913c5-240">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 5, done.
Delta compression using up too4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

<span data-ttu-id="913c5-241">Azt tapasztalhatja, hogy fut-e a központi telepítési folyamat hello [Gulp](http://gulpjs.com/) után `npm install`.</span><span class="sxs-lookup"><span data-stu-id="913c5-241">You may notice that hello deployment process runs [Gulp](http://gulpjs.com/) after `npm install`.</span></span> <span data-ttu-id="913c5-242">App Service nem Gulp vagy Grunt feladatok futnak a telepítés során, így ez a minta-tárház két további fájlok a a root directory tooenable:</span><span class="sxs-lookup"><span data-stu-id="913c5-242">App Service does not run Gulp or Grunt tasks during deployment, so this sample repository has two additional files in its root directory tooenable it:</span></span> 

- <span data-ttu-id="913c5-243">_.Deployment_ – Ez a fájl közli az App Service toorun `bash deploy.sh` hello egyéni telepítési parancsfájlként.</span><span class="sxs-lookup"><span data-stu-id="913c5-243">_.deployment_ - This file tells App Service toorun `bash deploy.sh` as hello custom deployment script.</span></span>
- <span data-ttu-id="913c5-244">_Deploy.SH_ -hello egyéni telepítési parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="913c5-244">_deploy.sh_ - hello custom deployment script.</span></span> <span data-ttu-id="913c5-245">Ha hello fájl tekintse át, látni fogja, hogy fusson `gulp prod` után `npm install` és `bower install`.</span><span class="sxs-lookup"><span data-stu-id="913c5-245">If you review hello file, you will see that it runs `gulp prod` after `npm install` and `bower install`.</span></span> 

<span data-ttu-id="913c5-246">Ez a megközelítés tooadd bármely lépés tooyour Git-alapú üzembe helyezési használhatja.</span><span class="sxs-lookup"><span data-stu-id="913c5-246">You can use this approach tooadd any step tooyour Git-based deployment.</span></span> <span data-ttu-id="913c5-247">Újraindítja az Azure-webalkalmazásban bármikor, ha az App Service automatizálási feladatok nem futtassa újra.</span><span class="sxs-lookup"><span data-stu-id="913c5-247">If you restart your Azure web app at any point, App Service doesn't rerun these automation tasks.</span></span>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="913c5-248">Keresse meg az Azure-webalkalmazásban toohello</span><span class="sxs-lookup"><span data-stu-id="913c5-248">Browse toohello Azure web app</span></span> 

<span data-ttu-id="913c5-249">Keresse meg a telepített toohello web app a webböngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="913c5-249">Browse toohello deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
``` 

<span data-ttu-id="913c5-250">Kattintson a **regisztráció** a felső menüben hello, és hozzon létre egy üres felhasználót.</span><span class="sxs-lookup"><span data-stu-id="913c5-250">Click **Sign Up** in hello top menu and create a dummy user.</span></span> 

<span data-ttu-id="913c5-251">Ha sikeres, és automatikusan bejelentkezik hello app toohello felhasználó, akkor az MEAN.js-alkalmazás létrehozása az Azure-ban van kapcsolat toohello (Cosmos DB) MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="913c5-251">If you are successful and hello app automatically signs in toohello created user, then your MEAN.js app in Azure has connectivity toohello MongoDB (Cosmos DB) database.</span></span> 

![Az Azure App Service-ben futó MEAN.js alkalmazás](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="913c5-253">Válassza ki **Admin > kezelése cikkek** tooadd egyes cikkeket.</span><span class="sxs-lookup"><span data-stu-id="913c5-253">Select **Admin > Manage Articles** tooadd some articles.</span></span> 

<span data-ttu-id="913c5-254">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="913c5-254">**Congratulations!**</span></span> <span data-ttu-id="913c5-255">Adatalapú Node.js-alkalmazás fut az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="913c5-255">You're running a data-driven Node.js app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="913c5-256">Frissítés adatmodell, és helyezze üzembe újra</span><span class="sxs-lookup"><span data-stu-id="913c5-256">Update data model and redeploy</span></span>

<span data-ttu-id="913c5-257">Ebben a lépésben hello módosítása `article` adatok modell, és a módosítás tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="913c5-257">In this step, you change hello `article` data model and publish your change tooAzure.</span></span>

### <a name="update-hello-data-model"></a><span data-ttu-id="913c5-258">Hello adatmodell frissítése</span><span class="sxs-lookup"><span data-stu-id="913c5-258">Update hello data model</span></span>

<span data-ttu-id="913c5-259">Nyissa meg _modules/articles/server/models/article.server.model.js_.</span><span class="sxs-lookup"><span data-stu-id="913c5-259">Open _modules/articles/server/models/article.server.model.js_.</span></span>

<span data-ttu-id="913c5-260">A `ArticleSchema`, vegye fel a `String` nevű típus `comment`.</span><span class="sxs-lookup"><span data-stu-id="913c5-260">In `ArticleSchema`, add a `String` type called `comment`.</span></span> <span data-ttu-id="913c5-261">Amikor elkészült, a séma kódot kell néznek ki:</span><span class="sxs-lookup"><span data-stu-id="913c5-261">When you're done, your schema code should look like this:</span></span>

```javascript
var ArticleSchema = new Schema({
  ...,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  },
  comment: {
    type: String,
    default: '',
    trim: true
  }
});
```

### <a name="update-hello-articles-code"></a><span data-ttu-id="913c5-262">Hello cikkek kód frissítése</span><span class="sxs-lookup"><span data-stu-id="913c5-262">Update hello articles code</span></span>

<span data-ttu-id="913c5-263">Hello részeinek frissítése a `articles` code toouse `comment`.</span><span class="sxs-lookup"><span data-stu-id="913c5-263">Update hello rest of your `articles` code toouse `comment`.</span></span>

<span data-ttu-id="913c5-264">Öt fájl toomodify szüksége van: hello server vezérlő és hello négy ügyfél nézeteket.</span><span class="sxs-lookup"><span data-stu-id="913c5-264">There are five files you need toomodify: hello server controller and hello four client views.</span></span> 

<span data-ttu-id="913c5-265">Nyissa meg _modules/articles/server/controllers/articles.server.controller.js_.</span><span class="sxs-lookup"><span data-stu-id="913c5-265">Open _modules/articles/server/controllers/articles.server.controller.js_.</span></span>

<span data-ttu-id="913c5-266">A hello `update` működik, vegye fel a hozzárendelés `article.comment`.</span><span class="sxs-lookup"><span data-stu-id="913c5-266">In hello `update` function, add an assignment for `article.comment`.</span></span> <span data-ttu-id="913c5-267">hello következő kód bemutatja befejeződött hello `update` függvény:</span><span class="sxs-lookup"><span data-stu-id="913c5-267">hello following code shows hello completed `update` function:</span></span>

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

<span data-ttu-id="913c5-268">Nyissa meg _modules/articles/client/views/view-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="913c5-268">Open _modules/articles/client/views/view-article.client.view.html_.</span></span>

<span data-ttu-id="913c5-269">Hello záró felett `</section>` címkét, adja hozzá a következő sor toodisplay hello `comment` hello egyéb hello cikk együtt:</span><span class="sxs-lookup"><span data-stu-id="913c5-269">Just above hello closing `</section>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

<span data-ttu-id="913c5-270">Nyissa meg _modules/articles/client/views/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="913c5-270">Open _modules/articles/client/views/list-articles.client.view.html_.</span></span>

<span data-ttu-id="913c5-271">Hello záró felett `</a>` címkét, adja hozzá a következő sor toodisplay hello `comment` hello egyéb hello cikk együtt:</span><span class="sxs-lookup"><span data-stu-id="913c5-271">Just above hello closing `</a>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

<span data-ttu-id="913c5-272">Nyissa meg _modules/articles/client/views/admin/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="913c5-272">Open _modules/articles/client/views/admin/list-articles.client.view.html_.</span></span>

<span data-ttu-id="913c5-273">Belső hello `<div class="list-group">` elem és fölött hello záró `</a>` címkét, adja hozzá a következő sor toodisplay hello `comment` hello egyéb hello cikk együtt:</span><span class="sxs-lookup"><span data-stu-id="913c5-273">Inside hello `<div class="list-group">` element and just above hello closing `</a>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

<span data-ttu-id="913c5-274">Nyissa meg _modules/articles/client/views/admin/form-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="913c5-274">Open _modules/articles/client/views/admin/form-article.client.view.html_.</span></span>

<span data-ttu-id="913c5-275">Hello található `<div class="form-group">` elem, amely hello Küldés gomb, mely dolgozunk tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="913c5-275">Find hello `<div class="form-group">` element that contains hello submit button, which looks like this:</span></span>

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

<span data-ttu-id="913c5-276">Ez a tag fölött hozzáadása egy másik `<div class="form-group">` elem, amelynek segítségével a hello szerkesztése `comment` mező.</span><span class="sxs-lookup"><span data-stu-id="913c5-276">Just above this tag, add another `<div class="form-group">` element that lets people edit hello `comment` field.</span></span> <span data-ttu-id="913c5-277">Az új elem kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="913c5-277">Your new element should look like this:</span></span>

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="913c5-278">A módosításokat a helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="913c5-278">Test your changes locally</span></span>

<span data-ttu-id="913c5-279">Mentse az összes módosítást.</span><span class="sxs-lookup"><span data-stu-id="913c5-279">Save all your changes.</span></span>

<span data-ttu-id="913c5-280">Tesztelje a módosításokat az éles üzemmódhoz újból.</span><span class="sxs-lookup"><span data-stu-id="913c5-280">Test your changes in production mode again.</span></span>

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> <span data-ttu-id="913c5-281">Ne feledje, hogy a _config/env/production.js_ vissza lett állítva, és hello `MONGODB_URI` környezeti változó csak akkor állítható be, az Azure web app alkalmazásban, és nem a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="913c5-281">Remember that your _config/env/production.js_ has been reverted, and hello `MONGODB_URI` environment variable is only set in your Azure web app and not on your local machine.</span></span> <span data-ttu-id="913c5-282">A konfigurációs fájl hello tekinti meg, ha található-e, hogy hello éles konfigurációs alapértelmezés szerint toouse helyi MongoDB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="913c5-282">If you look at hello config file, you find that hello production configuration defaults toouse a local MongoDB database.</span></span> <span data-ttu-id="913c5-283">Ezzel biztosíthatja, hogy nem touch termelési adatok helyben a kódmódosításokat tesztelésekor.</span><span class="sxs-lookup"><span data-stu-id="913c5-283">This makes sure that you don't touch production data when you test your code changes locally.</span></span>

<span data-ttu-id="913c5-284">Keresse meg a túl`http://localhost:8443` a böngészőben, és győződjön meg arról, hogy be van jelentkezve.</span><span class="sxs-lookup"><span data-stu-id="913c5-284">Navigate too`http://localhost:8443` in a browser and make sure that you're signed in.</span></span>

<span data-ttu-id="913c5-285">Válassza ki **Admin > kezelése cikkek**, majd adja hozzá a cikk hello kiválasztásával  **+**  gomb.</span><span class="sxs-lookup"><span data-stu-id="913c5-285">Select **Admin > Manage Articles**, then add an article by selecting hello **+** button.</span></span>

<span data-ttu-id="913c5-286">Új lásd hello `Comment` szövegmező most.</span><span class="sxs-lookup"><span data-stu-id="913c5-286">You see hello new `Comment` textbox now.</span></span>

![A hozzáadott megjegyzés mező tooArticles](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

<span data-ttu-id="913c5-288">A Terminálszolgáltatások hello, leállításához írja be a Node.js `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="913c5-288">In hello terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

### <a name="publish-changes-tooazure"></a><span data-ttu-id="913c5-289">Módosítások tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="913c5-289">Publish changes tooAzure</span></span>

<span data-ttu-id="913c5-290">A Git a változtatások véglegesítése a határidő, majd továbbítsa azokat hello kód módosítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="913c5-290">Commit your changes in Git, then push hello code changes tooAzure.</span></span>

```bash
git commit -am "added article comment"
git push azure master
```

<span data-ttu-id="913c5-291">Egyszer hello `git push` befejezése, keresse meg az Azure-webalkalmazásban tooyour és hello új funkcióinak kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="913c5-291">Once hello `git push` is complete, navigate tooyour Azure web app and try out hello new functionality.</span></span>

![Adatbázis és a modell módosítások közzétéve tooAzure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

<span data-ttu-id="913c5-293">Ha korábban hozzáadott olyan cikkek, továbbra is láthatja azokat.</span><span class="sxs-lookup"><span data-stu-id="913c5-293">If you added any articles earlier, you still can see them.</span></span> <span data-ttu-id="913c5-294">A Cosmos DB meglévő adatait nem elvész.</span><span class="sxs-lookup"><span data-stu-id="913c5-294">Existing data in your Cosmos DB is not lost.</span></span> <span data-ttu-id="913c5-295">Emellett a frissítések toohello adatséma és a meglévő adatok sértetlenek.</span><span class="sxs-lookup"><span data-stu-id="913c5-295">Also, your updates toohello data schema and leaves your existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="913c5-296">Diagnosztikai naplók adatfolyam</span><span class="sxs-lookup"><span data-stu-id="913c5-296">Stream diagnostic logs</span></span> 

<span data-ttu-id="913c5-297">A Node.js-alkalmazás futtatása az Azure App Service-ben, közben kaphat hello konzol naplók védőeszközön tooyour terminál.</span><span class="sxs-lookup"><span data-stu-id="913c5-297">While your Node.js application runs in Azure App Service, you can get hello console logs piped tooyour terminal.</span></span> <span data-ttu-id="913c5-298">Ily módon kaphat hello ugyanazon diagnosztikai üzenetek toohelp hibakeresése az alkalmazáshibák.</span><span class="sxs-lookup"><span data-stu-id="913c5-298">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="913c5-299">streamelés esetén használjon hello toostart napló [az webapp napló végéről](/cli/azure/webapp/log#tail) parancsot.</span><span class="sxs-lookup"><span data-stu-id="913c5-299">toostart log streaming, use hello [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

<span data-ttu-id="913c5-300">Napló adatfolyam megkezdése, után frissítse az Azure-webalkalmazásban a hello böngésző tooget néhány webes forgalom.</span><span class="sxs-lookup"><span data-stu-id="913c5-300">Once log streaming has started, refresh your Azure web app in hello browser tooget some web traffic.</span></span> <span data-ttu-id="913c5-301">Ekkor megjelenik konzolnaplófájlokban adatcsatornán terminál tooyour.</span><span class="sxs-lookup"><span data-stu-id="913c5-301">You now see console logs piped tooyour terminal.</span></span>

<span data-ttu-id="913c5-302">Írja be a bármikor streaming leállítási napló `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="913c5-302">Stop log streaming at any time by typing `Ctrl+C`.</span></span> 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="913c5-303">Az Azure-webalkalmazásban kezelése</span><span class="sxs-lookup"><span data-stu-id="913c5-303">Manage your Azure web app</span></span>

<span data-ttu-id="913c5-304">Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toosee hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="913c5-304">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span>

<span data-ttu-id="913c5-305">Hello bal oldali menüben kattintson **alkalmazásszolgáltatások**, majd kattintson az Azure-webalkalmazásban hello nevére.</span><span class="sxs-lookup"><span data-stu-id="913c5-305">From hello left menu, click **App Services**, then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

<span data-ttu-id="913c5-307">Alapértelmezés szerint hello portal jeleníti meg a webalkalmazás **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="913c5-307">By default, hello portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="913c5-308">Ezen az oldalon megtekintheti az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="913c5-308">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="913c5-309">Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="913c5-309">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="913c5-310">hello lapok hello bal oldalán található hello hello különböző konfigurációs lapok megnyithatja megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="913c5-310">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span>

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a><span data-ttu-id="913c5-312">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="913c5-312">Next steps</span></span>

<span data-ttu-id="913c5-313">Hogy mit tudott:</span><span class="sxs-lookup"><span data-stu-id="913c5-313">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="913c5-314">MongoDB-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="913c5-314">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="913c5-315">Csatlakozás egy Node.js-alkalmazás tooMongoDB</span><span class="sxs-lookup"><span data-stu-id="913c5-315">Connect a Node.js app tooMongoDB</span></span>
> * <span data-ttu-id="913c5-316">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="913c5-316">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="913c5-317">Hello adatmodell frissítése, és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="913c5-317">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="913c5-318">Stream naplók az Azure tooyour terminál</span><span class="sxs-lookup"><span data-stu-id="913c5-318">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="913c5-319">Az Azure-portálon hello hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="913c5-319">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="913c5-320">Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="913c5-320">Advance toohello next tutorial toolearn how toomap a custom DNS name tooyour web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="913c5-321">Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése</span><span class="sxs-lookup"><span data-stu-id="913c5-321">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
