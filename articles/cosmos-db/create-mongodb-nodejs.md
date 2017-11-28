---
title: "a MongoDB app tooAzure Cosmos DB Node.js használatával aaaConnect |} Microsoft Docs"
description: "Ismerje meg, hogy egy meglévő Node.js MongoDB-alkalmazás tooAzure Cosmos DB tooconnect"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/19/2017
ms.author: mimig
ms.openlocfilehash: 4bc4f17a31d8c18d1ce5e3f002462f4d48eeb1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app"></a><span data-ttu-id="994ec-103">Azure Cosmos DB: Meglévő Node.js MongoDB-webalkalmazás migrálása</span><span class="sxs-lookup"><span data-stu-id="994ec-103">Azure Cosmos DB: Migrate an existing Node.js MongoDB web app</span></span> 

<span data-ttu-id="994ec-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="994ec-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="994ec-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="994ec-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="994ec-106">A gyors üzembe helyezés bemutatja, hogyan toouse egy meglévő [MongoDB](mongodb-introduction.md) írt Node.js alkalmazást, és csatlakoztassa tooyour Azure Cosmos DB adatbázis, amely támogatja a MongoDB-ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="994ec-106">This quickstart demonstrates how toouse an existing [MongoDB](mongodb-introduction.md) app written in Node.js and connect it tooyour Azure Cosmos DB database, which supports MongoDB client connections.</span></span> <span data-ttu-id="994ec-107">Más szóval a Node.js-alkalmazás csak tudja, hogy az tooa adatbázis MongoDB API-k használatával csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="994ec-107">In other words, your Node.js application only knows that it's connecting tooa database using MongoDB APIs.</span></span> <span data-ttu-id="994ec-108">Áttetsző toohello alkalmazás, amely adatok hello Azure Cosmos DB van tárolva.</span><span class="sxs-lookup"><span data-stu-id="994ec-108">It is transparent toohello application that hello data is stored in Azure Cosmos DB.</span></span>

<span data-ttu-id="994ec-109">Az útmutató végére MEAN-alkalmazása (MongoDB, Express, AngularJS és Node.js) az [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) rendszert használva fog futni.</span><span class="sxs-lookup"><span data-stu-id="994ec-109">When you are done, you will have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running on [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> 

![Az Azure App Service-ben futó MEAN.js alkalmazás](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="994ec-111">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="994ec-111">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="994ec-112">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="994ec-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="994ec-113">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="994ec-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="994ec-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="994ec-114">Prerequisites</span></span> 
<span data-ttu-id="994ec-115">Parancssori felület tooAzure, meg kell továbbá [Node.js](https://nodejs.org/) és [Git](http://www.git-scm.com/downloads) telepítve helyileg toorun `npm` és `git` parancsok.</span><span class="sxs-lookup"><span data-stu-id="994ec-115">In addition tooAzure CLI, you need [Node.js](https://nodejs.org/) and [Git](http://www.git-scm.com/downloads) installed locally toorun `npm` and `git` commands.</span></span>

<span data-ttu-id="994ec-116">Emellett ajánlott rendelkeznie a Node.js használatához szükséges ismeretekkel.</span><span class="sxs-lookup"><span data-stu-id="994ec-116">You should have working knowledge of Node.js.</span></span> <span data-ttu-id="994ec-117">A gyors üzembe helyezés nincs tervezett toohelp általában a Node.js-alkalmazások fejlesztésével kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="994ec-117">This quickstart is not intended toohelp you with developing Node.js applications in general.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="994ec-118">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="994ec-118">Clone hello sample application</span></span>

<span data-ttu-id="994ec-119">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="994ec-119">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

<span data-ttu-id="994ec-120">Futtassa a következő parancsok tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="994ec-120">Run hello following commands tooclone hello sample repository.</span></span> <span data-ttu-id="994ec-121">Ez a minta-tárház tartalmaz hello alapértelmezett [MEAN.js](http://meanjs.org/) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="994ec-121">This sample repository contains hello default [MEAN.js](http://meanjs.org/) application.</span></span> 

```bash
git clone https://github.com/prashanthmadi/mean
```

## <a name="run-hello-application"></a><span data-ttu-id="994ec-122">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="994ec-122">Run hello application</span></span>

<span data-ttu-id="994ec-123">Telepítse a szükséges hello csomagok, és indítsa el a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="994ec-123">Install hello required packages and start hello application.</span></span>

```bash
cd mean
npm install
npm start
```

## <a name="log-in-tooazure"></a><span data-ttu-id="994ec-124">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="994ec-124">Log in tooAzure</span></span>

<span data-ttu-id="994ec-125">Ha egy telepített Azure CLI-t használ, jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="994ec-125">If you are using an installed Azure CLI, log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> <span data-ttu-id="994ec-126">Ezt a lépést kihagyhatja, ha a hello Azure Cloud rendszerhéj használata.</span><span class="sxs-lookup"><span data-stu-id="994ec-126">You can skip this step if you're using hello Azure Cloud Shell.</span></span>

```azurecli
az login 
``` 
   
## <a name="add-hello-azure-cosmos-db-module"></a><span data-ttu-id="994ec-127">Hello Azure Cosmos DB modul hozzá lesz adva</span><span class="sxs-lookup"><span data-stu-id="994ec-127">Add hello Azure Cosmos DB module</span></span>

<span data-ttu-id="994ec-128">Ha egy telepített Azure CLI használata esetén ellenőrizze toosee, ha hello `cosmosdb` összetevője már telepítve van a hello futtatásával `az` parancsot.</span><span class="sxs-lookup"><span data-stu-id="994ec-128">If you are using an installed Azure CLI, check toosee if hello `cosmosdb` component is already installed by running hello `az` command.</span></span> <span data-ttu-id="994ec-129">Ha `cosmosdb` a hello alap parancsok listáját, akkor folytassa a következő toohello parancs.</span><span class="sxs-lookup"><span data-stu-id="994ec-129">If `cosmosdb` is in hello list of base commands, proceed toohello next command.</span></span> <span data-ttu-id="994ec-130">Ezt a lépést kihagyhatja, ha a hello Azure Cloud rendszerhéj használata.</span><span class="sxs-lookup"><span data-stu-id="994ec-130">You can skip this step if you're using hello Azure Cloud Shell.</span></span>

<span data-ttu-id="994ec-131">Ha `cosmosdb` nincs a hello alap parancsok listáját, telepítse újra [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="994ec-131">If `cosmosdb` is not in hello list of base commands, reinstall [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="994ec-132">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="994ec-132">Create a resource group</span></span>

<span data-ttu-id="994ec-133">Hozzon létre egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a hello [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="994ec-133">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="994ec-134">Az Azure-erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat (például webappokat, adatbázisokat és tárfiókokat).</span><span class="sxs-lookup"><span data-stu-id="994ec-134">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="994ec-135">hello alábbi példa létrehoz egy erőforráscsoport hello Nyugat-Európában régióban.</span><span class="sxs-lookup"><span data-stu-id="994ec-135">hello following example creates a resource group in hello West Europe region.</span></span> <span data-ttu-id="994ec-136">Adjon egyedi nevet az hello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="994ec-136">Choose a unique name for hello resource group.</span></span>

<span data-ttu-id="994ec-137">Ha Azure-felhő rendszerhéj használ, kattintson a **azt próbálja**, kövesse a képernyőn megjelenő utasításokat toologin hello, majd hello parancs átmásolja hello parancssort.</span><span class="sxs-lookup"><span data-stu-id="994ec-137">If you are using Azure Cloud Shell, click **Try It**, follow hello onscreen prompts toologin, then copy hello command into hello command prompt.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="994ec-138">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="994ec-138">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="994ec-139">Hozzon létre egy Azure Cosmos DB fiókot hello [az cosmosdb létrehozása](/cli/azure/cosmosdb#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="994ec-139">Create an Azure Cosmos DB account with hello [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="994ec-140">A hello adja a következő parancsot, helyettesítse a saját egyedi Azure Cosmos DB fióknév hello megtapasztalhatja `<cosmosdb-name>` helyőrző.</span><span class="sxs-lookup"><span data-stu-id="994ec-140">In hello following command, please substitute your own unique Azure Cosmos DB account name where you see hello `<cosmosdb-name>` placeholder.</span></span> <span data-ttu-id="994ec-141">A egyedi név az Azure Cosmos DB végpontjának részeként használható (`https://<cosmosdb-name>.documents.azure.com/`), így a hello nevének kell toobe egyedi összes Azure Cosmos DB fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="994ec-141">This unique name will be used as part of your Azure Cosmos DB endpoint (`https://<cosmosdb-name>.documents.azure.com/`), so hello name needs toobe unique across all Azure Cosmos DB accounts in Azure.</span></span> 

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

<span data-ttu-id="994ec-142">Hello `--kind MongoDB` paraméter lehetővé teszi, hogy a MongoDB-ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="994ec-142">hello `--kind MongoDB` parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="994ec-143">Hello Azure Cosmos DB fiók létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="994ec-143">When hello Azure Cosmos DB account is created, hello Azure CLI shows information similar toohello following example.</span></span> 

> [!NOTE]
> <span data-ttu-id="994ec-144">A példa JSON hello alapértelmezett hello Azure CLI kimeneti formátumban.</span><span class="sxs-lookup"><span data-stu-id="994ec-144">This example uses JSON as hello Azure CLI output format, which is hello default.</span></span> <span data-ttu-id="994ec-145">egy másik kimeneti toouse formátum, lásd: [kimeneti formátumot az Azure CLI 2.0 parancsok](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="994ec-145">toouse another output format, see [Output formats for Azure CLI 2.0 commands](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span></span>

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb-name>.documents.azure.com:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb-name>",
  "kind": "MongoDB",
  "location": "West Europe",
  "name": "<cosmosdb-name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## <a name="connect-your-nodejs-application-toohello-database"></a><span data-ttu-id="994ec-146">Csatlakozás a Node.js-alkalmazás toohello adatbázis</span><span class="sxs-lookup"><span data-stu-id="994ec-146">Connect your Node.js application toohello database</span></span>

<span data-ttu-id="994ec-147">Ebben a lépésben csatlakoztatja a MEAN.js alkalmazás tooan Azure Cosmos DB mintaadatbázis imént létrehozott, a MongoDB-kapcsolati karakterlánc használatával.</span><span class="sxs-lookup"><span data-stu-id="994ec-147">In this step, you connect your MEAN.js sample application tooan Azure Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

<a name="devconfig"></a>
## <a name="configure-hello-connection-string-in-your-nodejs-application"></a><span data-ttu-id="994ec-148">Node.js-alkalmazásában hello kapcsolati karakterlánc konfigurálása</span><span class="sxs-lookup"><span data-stu-id="994ec-148">Configure hello connection string in your Node.js application</span></span>

<span data-ttu-id="994ec-149">A MEAN.js-tárházban nyissa meg a `config/env/local-development.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="994ec-149">In your MEAN.js repository, open `config/env/local-development.js`.</span></span>

<span data-ttu-id="994ec-150">Cserélje le a következő kód hello hello fájl tartalma.</span><span class="sxs-lookup"><span data-stu-id="994ec-150">Replace hello content of this file with hello following code.</span></span> <span data-ttu-id="994ec-151">Ne feledje tooalso cserélje le a két hello `<cosmosdb-name>` helyőrzőt a Azure Cosmos DB fióknevet.</span><span class="sxs-lookup"><span data-stu-id="994ec-151">Be sure tooalso replace hello two `<cosmosdb-name>` placeholders with your Azure Cosmos DB account name.</span></span>

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-hello-key"></a><span data-ttu-id="994ec-152">Hello kulcs beolvasása</span><span class="sxs-lookup"><span data-stu-id="994ec-152">Retrieve hello key</span></span>

<span data-ttu-id="994ec-153">A sorrend tooconnect tooan Azure Cosmos DB adatbázisban be kell hello adatbázis kulcsot.</span><span class="sxs-lookup"><span data-stu-id="994ec-153">In order tooconnect tooan Azure Cosmos DB database, you need hello database key.</span></span> <span data-ttu-id="994ec-154">Használjon hello [az cosmosdb lista-kulcsok](/cli/azure/cosmosdb#list-keys) parancs tooretrieve hello elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="994ec-154">Use hello [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command tooretrieve hello primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

<span data-ttu-id="994ec-155">hello Azure CLI információkat a következő példa hasonló toohello kimenete.</span><span class="sxs-lookup"><span data-stu-id="994ec-155">hello Azure CLI outputs information similar toohello following example.</span></span> 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

<span data-ttu-id="994ec-156">Másolja a hello értékének `primaryMasterKey`.</span><span class="sxs-lookup"><span data-stu-id="994ec-156">Copy hello value of `primaryMasterKey`.</span></span> <span data-ttu-id="994ec-157">Illessze be a hello keresztül `<primary_master_key>` a `local-development.js`.</span><span class="sxs-lookup"><span data-stu-id="994ec-157">Paste this over hello  `<primary_master_key>` in `local-development.js`.</span></span>

<span data-ttu-id="994ec-158">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="994ec-158">Save your changes.</span></span>

### <a name="run-hello-application-again"></a><span data-ttu-id="994ec-159">Futtassa újból a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="994ec-159">Run hello application again.</span></span>

<span data-ttu-id="994ec-160">Futtassa ismét az `npm start` parancsot.</span><span class="sxs-lookup"><span data-stu-id="994ec-160">Run `npm start` again.</span></span> 

```bash
npm start
```

<span data-ttu-id="994ec-161">Konzolüzenet kell most közli, hogy hello fejlesztési környezet megfelelően működik, és.</span><span class="sxs-lookup"><span data-stu-id="994ec-161">A console message should now tell you that hello development environment is up and running.</span></span> 

<span data-ttu-id="994ec-162">Keresse meg a túl`http://localhost:3000` a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="994ec-162">Navigate too`http://localhost:3000` in a browser.</span></span> <span data-ttu-id="994ec-163">Kattintson a **regisztráció** hello felső menüre, és próbálja meg toocreate két üres a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="994ec-163">Click **Sign Up** in hello top menu and try toocreate two dummy users.</span></span> 

<span data-ttu-id="994ec-164">hello MEAN.js mintaalkalmazás felhasználói adatok hello adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="994ec-164">hello MEAN.js sample application stores user data in hello database.</span></span> <span data-ttu-id="994ec-165">Ha sikeres, és automatikusan bejelentkezik MEAN.js hello létrehozott felhasználó, majd a Azure Cosmos DB kapcsolat működik.</span><span class="sxs-lookup"><span data-stu-id="994ec-165">If you are successful and MEAN.js automatically signs into hello created user, then your Azure Cosmos DB connection is working.</span></span> 

![MEAN.js sikeresen csatlakozik tooMongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a><span data-ttu-id="994ec-167">Adatok megtekintése az Adatkezelőben</span><span class="sxs-lookup"><span data-stu-id="994ec-167">View data in Data Explorer</span></span>

<span data-ttu-id="994ec-168">Egy Azure Cosmos DB által tárolt adatok elérhető tooview, a lekérdezés és a futási logikájának be hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="994ec-168">Data stored by an Azure Cosmos DB is available tooview, query, and run business-logic on in hello Azure portal.</span></span>

<span data-ttu-id="994ec-169">tooview, lekérdezni, és az előző lépésben létrehozott hello, bejelentkezési toohello hello felhasználói adatok együttműködve [Azure-portálon](https://portal.azure.com) a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="994ec-169">tooview, query, and work with hello user data created in hello previous step, login toohello [Azure portal](https://portal.azure.com) in your web browser.</span></span>

<span data-ttu-id="994ec-170">Hello felső keresési mezőbe írja be Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="994ec-170">In hello top Search box, type Azure Cosmos DB.</span></span> <span data-ttu-id="994ec-171">Amikor megnyílik a Cosmos DB-fiók panelje, válassza ki Cosmos DB-fiókját.</span><span class="sxs-lookup"><span data-stu-id="994ec-171">When your Cosmos DB account blade opens, select your Cosmos DB account.</span></span> <span data-ttu-id="994ec-172">A bal oldali navigációs hello kattintson a Data Explorer.</span><span class="sxs-lookup"><span data-stu-id="994ec-172">In hello left navigation, click Data Explorer.</span></span> <span data-ttu-id="994ec-173">Bontsa ki a gyűjteményt hello Gyűjtemények ablaktáblán, és ezután hello dokumentumok megtekintése hello gyűjteményben, hello adatait, és még létrehozása és tárolt eljárások, eseményindítók és felhasználó által megadott függvények futtatása.</span><span class="sxs-lookup"><span data-stu-id="994ec-173">Expand your collection in hello Collections pane, and then you can view hello documents in hello collection, query hello data, and even create and run stored procedures, triggers, and UDFs.</span></span> 

![Az Azure-portálon hello adatkezelő](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


## <a name="deploy-hello-nodejs-application-tooazure"></a><span data-ttu-id="994ec-175">Node.js-alkalmazás tooAzure hello telepítése</span><span class="sxs-lookup"><span data-stu-id="994ec-175">Deploy hello Node.js application tooAzure</span></span>

<span data-ttu-id="994ec-176">Ebben a lépésben a MongoDB-csatlakoztatott Node.js-alkalmazás tooAzure Cosmos DB telepít.</span><span class="sxs-lookup"><span data-stu-id="994ec-176">In this step, you deploy your MongoDB-connected Node.js application tooAzure Cosmos DB.</span></span>

<span data-ttu-id="994ec-177">Talán észrevette, hogy módosultak a korábbi hello konfigurációs fájl van hello fejlesztőkörnyezet (`/config/env/local-development.js`).</span><span class="sxs-lookup"><span data-stu-id="994ec-177">You may have noticed that hello configuration file that you changed earlier is for hello development environment (`/config/env/local-development.js`).</span></span> <span data-ttu-id="994ec-178">Ha az alkalmazás tooApp szolgáltatást telepíti, akkor fog futni hello éles környezetben alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="994ec-178">When you deploy your application tooApp Service, it will run in hello production environment by default.</span></span> <span data-ttu-id="994ec-179">Ezért most kell toomake hello azonos toohello megfelelő konfigurációs fájl módosítása.</span><span class="sxs-lookup"><span data-stu-id="994ec-179">So now, you need toomake hello same change toohello respective configuration file.</span></span>

<span data-ttu-id="994ec-180">A MEAN.js-tárházban nyissa meg a `config/env/production.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="994ec-180">In your MEAN.js repository, open `config/env/production.js`.</span></span>

<span data-ttu-id="994ec-181">A hello `db` objektumazonosító, cserélje le a hello értékének `uri` regisztrációja, mivel a következő példa hello megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="994ec-181">In hello `db` object, replace hello value of `uri` as show in hello following example.</span></span> <span data-ttu-id="994ec-182">Lehet, hogy tooreplace hello helyőrzők mint előtt.</span><span class="sxs-lookup"><span data-stu-id="994ec-182">Be sure tooreplace hello placeholders as before.</span></span>

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> <span data-ttu-id="994ec-183">Hello `ssl=true` beállítás fontos, mert [Azure Cosmos DB SSL megkövetelése](connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="994ec-183">hello `ssl=true` option is important because [Azure Cosmos DB requires SSL](connect-mongodb-account.md#connection-string-requirements).</span></span> 
>
>

<span data-ttu-id="994ec-184">A Terminálszolgáltatások hello minden a módosítások véglegesítése a Git.</span><span class="sxs-lookup"><span data-stu-id="994ec-184">In hello terminal, commit all your changes into Git.</span></span> <span data-ttu-id="994ec-185">Mindkét parancsok toorun másolhatja azokat együtt.</span><span class="sxs-lookup"><span data-stu-id="994ec-185">You can copy both commands toorun them together.</span></span>

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a><span data-ttu-id="994ec-186">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="994ec-186">Clean up resources</span></span>

<span data-ttu-id="994ec-187">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="994ec-187">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="994ec-188">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="994ec-188">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="994ec-189">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="994ec-189">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="994ec-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="994ec-190">Next steps</span></span>

<span data-ttu-id="994ec-191">Az a gyors üzembe helyezés, hogy megismerte hogyan toocreate egy Azure Cosmos DB fiókot, és hozzon létre egy hello adatkezelő használatával MongoDB-gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="994ec-191">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and create a MongoDB collection using hello Data Explorer.</span></span> <span data-ttu-id="994ec-192">Most már telepítheti át a MongoDB-adatok tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="994ec-192">You can now migrate your MongoDB data tooAzure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="994ec-193">MongoDB adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="994ec-193">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)
