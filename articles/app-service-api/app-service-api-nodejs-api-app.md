---
title: "Node.js API-alkalmazás az Azure App Service platformon | Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre Node.js RESTful API-t, és hogyan telepítheti egy Azure App Service platformon futó API-alkalmazásba."
services: app-service\api
documentationcenter: node
author: bradygaster
manager: erikre
editor: 
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 06/13/2017
ms.author: rachelap
ms.openlocfilehash: 806585edd43b9d2d678bfa41523e4d9d40af8cba
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a><span data-ttu-id="c2d2f-103">Node.js RESTful API buildjének elkészítése és telepítése Azure-ban futó API-alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="c2d2f-103">Build a Node.js RESTful API and deploy it to an API app in Azure</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="c2d2f-104">Ez a rövid útmutató bemutatja, hogyan hozhat létre REST API-t a Node.js [Express](http://expressjs.com/) verzióval, [Swagger](http://swagger.io/) definícióval és [API-alkalmazásként](app-service-api-apps-why-best-platform.md) üzembe helyezve az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-104">This quickstart shows how to create a REST API, written with Node.js [Express](http://expressjs.com/), using a [Swagger](http://swagger.io/) definition and deploying it as an [API app](app-service-api-apps-why-best-platform.md) on Azure.</span></span> <span data-ttu-id="c2d2f-105">Az alkalmazást parancssori eszközökkel hozza létre, az erőforrásokat az [Azure parancssori felülettel](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) konfigurálja, majd az alkalmazást a Git használatával helyezi üzembe.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-105">You create the app using command-line tools, configure resources with the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and deploy the app using Git.</span></span>  <span data-ttu-id="c2d2f-106">Amikor végzett, Azure-on futó, működő minta REST API áll rendelkezésére majd.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-106">When you've finished, you have a working sample REST API running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2d2f-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2d2f-107">Prerequisites</span></span>

* [<span data-ttu-id="c2d2f-108">Git</span><span class="sxs-lookup"><span data-stu-id="c2d2f-108">Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="c2d2f-109"> Node.js és NPM</span><span class="sxs-lookup"><span data-stu-id="c2d2f-109">Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c2d2f-110">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakör az Azure CLI 2.0-s vagy annál újabb verziójának futtatását követeli meg.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-110">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c2d2f-111">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-111">Run `az --version` to find the version.</span></span> <span data-ttu-id="c2d2f-112">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c2d2f-112">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-your-environment"></a><span data-ttu-id="c2d2f-113">A környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="c2d2f-113">Prepare your environment</span></span>

1. <span data-ttu-id="c2d2f-114">A következő parancsot terminálablakban futtatva klónozhatja a mintát a helyi gépen.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-114">In a terminal window, run the following command to clone the sample to your local machine.</span></span>

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. <span data-ttu-id="c2d2f-115">Váltson arra a könyvtárra, amelyben a mintakód megtalálható.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-115">Change to the directory that contains the sample code.</span></span>

    ```bash
    cd app-service-api-node-contact-list
    ```

3. <span data-ttu-id="c2d2f-116">Telepítse a helyi gépen a [Swaggerize](https://www.npmjs.com/package/swaggerize-express) eszközt.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-116">Install [Swaggerize](https://www.npmjs.com/package/swaggerize-express) on your local machine.</span></span> <span data-ttu-id="c2d2f-117">A Swaggerize olyan eszköz, amely Swagger-definícióból generál Node.js-kódot a REST API-hoz.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-117">Swaggerize is a tool that generates Node.js code for your REST API from a Swagger definition.</span></span>

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a><span data-ttu-id="c2d2f-118">Node.js-kód generálása</span><span class="sxs-lookup"><span data-stu-id="c2d2f-118">Generate Node.js code</span></span> 

<span data-ttu-id="c2d2f-119">Az oktatóanyagnak ez a fejezete az API-fejlesztésnek azt a folyamatát modellezi, amelynek során először létrehozzuk a Swagger-metaadatokat, majd ezek segítségével automatikusan generáljuk az API kiszolgálói kódját.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-119">This section of the tutorial models an API development workflow in which you create Swagger metadata first and use that to scaffold (auto-generate) server code for the API.</span></span> 

<span data-ttu-id="c2d2f-120">Lépjen a *start*mappa könyvtárába, majd futtassa a `yo swaggerize` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-120">Change directory to the *start* folder, then run `yo swaggerize`.</span></span> <span data-ttu-id="c2d2f-121">A Swaggerize az *api.json* fájlban lévő Swagger-definícióból hoz létre Node.js-projektet az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-121">Swaggerize creates a Node.js project for your API from the Swagger definition in *api.json*.</span></span>

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

<span data-ttu-id="c2d2f-122">Amikor a Swaggerize egy projekt nevének megadását kéri, használja a *ContactList* nevet.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-122">When Swaggerize asks for a project name, use *ContactList*.</span></span>
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like to call this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-the-project-code"></a><span data-ttu-id="c2d2f-123">Projektkód testreszabása</span><span class="sxs-lookup"><span data-stu-id="c2d2f-123">Customize the project code</span></span>

1. <span data-ttu-id="c2d2f-124">Másolja a *lib* mappát a `yo swaggerize` által létrehozott *ContactList* mappába, majd lépjen a *ContactList* könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-124">Copy the *lib* folder into the *ContactList* folder created by `yo swaggerize`, then change directory into *ContactList*.</span></span>

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. <span data-ttu-id="c2d2f-125">Telepítse a `jsonpath` és a `swaggerize-ui` NPM-modulokat.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-125">Install the `jsonpath` and `swaggerize-ui` NPM modules.</span></span> 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. <span data-ttu-id="c2d2f-126">Cserélje a *handlers/contacts.js* fájlban lévő kódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="c2d2f-126">Replace the code in the *handlers/contacts.js* with the following code:</span></span> 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    <span data-ttu-id="c2d2f-127">A kód a *lib/contactRepository.js* által kiszolgált *lib/contacts.json* fájlban lévő JSON-adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-127">This code uses the JSON data stored in *lib/contacts.json* served by *lib/contactRepository.js*.</span></span> <span data-ttu-id="c2d2f-128">Az új *contacts.js* kód az adattárban található valamennyi névjegyet JSON-adattartalomként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-128">The new *contacts.js* code returns all contacts in the repository as a JSON payload.</span></span> 

4. <span data-ttu-id="c2d2f-129">Cserélje a **handlers/contacts/{id}.js** fájlban lévő kódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="c2d2f-129">Replace the code in the **handlers/contacts/{id}.js** file with the following code:</span></span>

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    <span data-ttu-id="c2d2f-130">Ez a kód elérésiút-változó használatát teszi lehetővé annak érdekében, hogy csak az adott azonosítóval rendelkező névjegyet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-130">This code lets you use a path variable to return only the contact with a given ID.</span></span>

5. <span data-ttu-id="c2d2f-131">Cserélje a **server.js** fájlban lévő kódot az alábbira:</span><span class="sxs-lookup"><span data-stu-id="c2d2f-131">Replace the code in **server.js** with the following code:</span></span>

    ```javascript
    'use strict';

    var port = process.env.PORT || 8000; 

    var http = require('http');
    var express = require('express');
    var bodyParser = require('body-parser');
    var swaggerize = require('swaggerize-express');
    var swaggerUi = require('swaggerize-ui'); 
    var path = require('path');
    var fs = require("fs");
    
    fs.existsSync = fs.existsSync || require('path').existsSync;

    var app = express();

    var server = http.createServer(app);

    app.use(bodyParser.json());

    app.use(swaggerize({
        api: path.resolve('./config/swagger.json'),
        handlers: path.resolve('./handlers'),
        docspath: '/swagger' 
    }));

    // change four
    app.use('/docs', swaggerUi({
        docs: '/swagger'  
    }));

    server.listen(port, function () { 
    });
    ```   

    <span data-ttu-id="c2d2f-132">Ez a kód kis módosításokat hajt végre az Azure App Service platformmal való használat lehetővé tétele érdekében, és interaktív webes felületet tesz közzé az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-132">This code makes some small changes to let it work with Azure App Service and exposes an interactive web interface for your API.</span></span>

### <a name="test-the-api-locally"></a><span data-ttu-id="c2d2f-133">API tesztelése helyileg</span><span class="sxs-lookup"><span data-stu-id="c2d2f-133">Test the API locally</span></span>

1. <span data-ttu-id="c2d2f-134">A Node.js-alkalmazás indítása</span><span class="sxs-lookup"><span data-stu-id="c2d2f-134">Start up the Node.js app</span></span>
    ```bash
    npm start
    ```
    
2. <span data-ttu-id="c2d2f-135">A http://localhost:8000/contacts címet megnyitva megtekintheti a teljes névjegylistához tartozó JSON-kimenetet.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-135">Browse to http://localhost:8000/contacts to view the JSON for the entire contact list.</span></span>
   
   ```json
    {
        "id": 1,
        "name": "Barney Poland",
        "email": "barney@contoso.com"
    },
    {
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    },
    {
        "id": 3,
        "name": "Lora Riggs",
        "email": "lora@contoso.com"
    }
   ```

3. <span data-ttu-id="c2d2f-136">A http://localhost:8000/contacts/2 címet megnyitva megtekintheti a kettes számú `id`hoz tartozó névjegyet.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-136">Browse to http://localhost:8000/contacts/2 to view the contact with an `id` of two.</span></span>
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. <span data-ttu-id="c2d2f-137">Tesztelje az API-t a http://localhost:8000/docs címen lévő Swagger webes felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-137">Test the API using the Swagger web interface at http://localhost:8000/docs.</span></span>
   
    ![Swagger webes felület](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <span data-ttu-id="c2d2f-139"><a id="createapiapp"></a> API-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2d2f-139"><a id="createapiapp"></a> Create an API App</span></span>

<span data-ttu-id="c2d2f-140">Ebben a szakaszban az Azure CLI 2.0 használatával hozhatja létre az API Azure App Service szolgáltatásban való üzemeltetéséhez szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-140">In this section, you use the Azure CLI 2.0 to create the resources to host the API on Azure App Service.</span></span> 

1.  <span data-ttu-id="c2d2f-141">Jelentkezzen be az Azure-előfizetésbe az [az login](/cli/azure/#login) paranccsal, és kövesse a képernyőn látható utasításokat.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-141">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span>

    ```azurecli-interactive
    az login
    ```

2. <span data-ttu-id="c2d2f-142">Ha több Azure-előfizetéssel is rendelkezik, módosítsa az alapértelmezett előfizetést a kívántra.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-142">If you have multiple Azure subscriptions, change the default subscription to the desired one.</span></span>

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-the-api-with-git"></a><span data-ttu-id="c2d2f-143">API üzembe helyezése GIT segítségével</span><span class="sxs-lookup"><span data-stu-id="c2d2f-143">Deploy the API with Git</span></span>

<span data-ttu-id="c2d2f-144">Úgy tudja telepíteni a kódot az API-alkalmazásba, hogy az Azure App Service-ben található helyi Git-tárházból beküldi a véglegesítéseket.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-144">Deploy your code to the API app by pushing commits from your local Git repository to Azure App Service.</span></span>

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. <span data-ttu-id="c2d2f-145">Inicializáljon új tárházat a *ContactList* könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-145">Initialize a new repo in the *ContactList* directory.</span></span> 

    ```bash
    git init .
    ```

3. <span data-ttu-id="c2d2f-146">Zárja ki az npm által, az oktatóanyag korábbi lépésében létrehozott *node_modules* könyvtárat a Gitből.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-146">Exclude the *node_modules* directory created by npm in an earlier step in the tutorial from Git.</span></span> <span data-ttu-id="c2d2f-147">Hozzon létre új `.gitignore`-fájlt a jelenlegi könyvtárban, majd a fájl valamelyik új sorában adja hozzá a következő szöveget.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-147">Create a new `.gitignore` file in the current directory and add the following text on a new line anywhere in the file.</span></span>

    ```
    node_modules/
    ```
    <span data-ttu-id="c2d2f-148">Ellenőrizze, hogy a rendszer figyelmen kívül hagyja-e a `node_modules`-mappát a `git status` állapottal.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-148">Confirm the `node_modules` folder is being ignored with  `git status`.</span></span>

4. <span data-ttu-id="c2d2f-149">Véglegesítse a tárházon végzett módosításokat.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-149">Commit the changes to the repo.</span></span>
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push to Azure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-the-api--in-azure"></a><span data-ttu-id="c2d2f-150">Az API tesztelése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c2d2f-150">Test the API  in Azure</span></span>

1. <span data-ttu-id="c2d2f-151">Nyissa meg valamilyen böngészőben a http://app_name.azurewebsites.net/contacts webhelyet.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-151">Open a browser to http://app_name.azurewebsites.net/contacts.</span></span> <span data-ttu-id="c2d2f-152">Ugyanazt a visszaadott JSON-t láthatja, mint amikor az oktatóanyag egyik korábbi szakaszában helyileg leadta a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-152">You see the same JSON returned as when you made the request locally earlier in the tutorial.</span></span>

   ```json
   {
       "id": 1,
       "name": "Barney Poland",
       "email": "barney@contoso.com"
   },
   {
       "id": 2,
       "name": "Lacy Barrera",
       "email": "lacy@contoso.com"
   },
   {
       "id": 3,
       "name": "Lora Riggs",
       "email": "lora@contoso.com"
   }
   ```

2. <span data-ttu-id="c2d2f-153">Nyissa meg valamelyik böngészőben a `http://app_name.azurewebsites.net/docs` végpontot, és próbálja ki az Azure-ban futó Swagger felhasználói felületet.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-153">In a browser, go to the `http://app_name.azurewebsites.net/docs` endpoint to try out the Swagger UI running on Azure.</span></span>

    ![Swagger Ii](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    <span data-ttu-id="c2d2f-155">Úgy helyezheti üzembe mostantól a minta API frissítéseit az Azure-ban, hogy a véglegesítéseket egyszerűen beküldi az Azure Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="c2d2f-155">You can now deploy updates to the sample API to Azure simply by pushing commits to the Azure Git repository.</span></span>

## <a name="clean-up"></a><span data-ttu-id="c2d2f-156">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c2d2f-156">Clean up</span></span>

<span data-ttu-id="c2d2f-157">Az ebben az oktatóanyagban létrehozott erőforrások törléséhez futtassa a következő Azure CLI parancsot:</span><span class="sxs-lookup"><span data-stu-id="c2d2f-157">To clean up the resources created in this quickstart, run the following Azure CLI command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a><span data-ttu-id="c2d2f-158">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="c2d2f-158">Next step</span></span> 
> [!div class="nextstepaction"]
> [<span data-ttu-id="c2d2f-159">API-alkalmazások használata JavaScript-ügyfelekkel a CORS segítségével</span><span class="sxs-lookup"><span data-stu-id="c2d2f-159">Consume API apps from JavaScript clients with CORS</span></span>](app-service-api-cors-consume-javascript.md)

