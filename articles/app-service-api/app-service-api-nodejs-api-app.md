---
title: "aaaNode.js API-alkalmazás az Azure App Service szolgáltatásban |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate Node.js RESTful API, és telepítse azt tooan API-alkalmazás az Azure App Service-ben."
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
ms.openlocfilehash: 3b3229c1453b6ca4d06bef26f476e92afda4e244
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-tooan-api-app-in-azure"></a><span data-ttu-id="cdce6-103">Node.js RESTful API létrehozása, és telepítse azt tooan API-alkalmazás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="cdce6-103">Build a Node.js RESTful API and deploy it tooan API app in Azure</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="cdce6-104">A gyors üzembe helyezés bemutatja, hogyan toocreate egy REST API-t készült Node.js [Express](http://expressjs.com/)használatával egy [Swagger](http://swagger.io/) definíció- és telepíti azokat, mint egy [API-alkalmazás](app-service-api-apps-why-best-platform.md) Azure.</span><span class="sxs-lookup"><span data-stu-id="cdce6-104">This quickstart shows how toocreate a REST API, written with Node.js [Express](http://expressjs.com/), using a [Swagger](http://swagger.io/) definition and deploying it as an [API app](app-service-api-apps-why-best-platform.md) on Azure.</span></span> <span data-ttu-id="cdce6-105">Hozzon létre hello alkalmazást parancssori eszközökkel, konfigurálja erőforrásokat hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git használatával hello alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="cdce6-105">You create hello app using command-line tools, configure resources with hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and deploy hello app using Git.</span></span>  <span data-ttu-id="cdce6-106">Amikor végzett, Azure-on futó, működő minta REST API áll rendelkezésére majd.</span><span class="sxs-lookup"><span data-stu-id="cdce6-106">When you've finished, you have a working sample REST API running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cdce6-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cdce6-107">Prerequisites</span></span>

* [<span data-ttu-id="cdce6-108">Git</span><span class="sxs-lookup"><span data-stu-id="cdce6-108">Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="cdce6-109"> Node.js és NPM</span><span class="sxs-lookup"><span data-stu-id="cdce6-109">Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="cdce6-110">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="cdce6-110">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="cdce6-111">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="cdce6-111">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="cdce6-112">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="cdce6-112">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-your-environment"></a><span data-ttu-id="cdce6-113">A környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="cdce6-113">Prepare your environment</span></span>

1. <span data-ttu-id="cdce6-114">Egy terminálablakot futtassa a következő parancs tooclone hello minta tooyour helyi számítógép hello.</span><span class="sxs-lookup"><span data-stu-id="cdce6-114">In a terminal window, run hello following command tooclone hello sample tooyour local machine.</span></span>

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. <span data-ttu-id="cdce6-115">Módosítsa a hello mintakódot tartalmazó toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cdce6-115">Change toohello directory that contains hello sample code.</span></span>

    ```bash
    cd app-service-api-node-contact-list
    ```

3. <span data-ttu-id="cdce6-116">Telepítse a helyi gépen a [Swaggerize](https://www.npmjs.com/package/swaggerize-express) eszközt.</span><span class="sxs-lookup"><span data-stu-id="cdce6-116">Install [Swaggerize](https://www.npmjs.com/package/swaggerize-express) on your local machine.</span></span> <span data-ttu-id="cdce6-117">A Swaggerize olyan eszköz, amely Swagger-definícióból generál Node.js-kódot a REST API-hoz.</span><span class="sxs-lookup"><span data-stu-id="cdce6-117">Swaggerize is a tool that generates Node.js code for your REST API from a Swagger definition.</span></span>

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a><span data-ttu-id="cdce6-118">Node.js-kód generálása</span><span class="sxs-lookup"><span data-stu-id="cdce6-118">Generate Node.js code</span></span> 

<span data-ttu-id="cdce6-119">Ebben a szakaszban hello oktatóanyag modellek egy API fejlesztési munkafolyamat, amelyben először hozza létre a Swagger-metaadatokat, és használja, hogy tooscaffold (automatikus létrehozása) hello API kiszolgálói kódját.</span><span class="sxs-lookup"><span data-stu-id="cdce6-119">This section of hello tutorial models an API development workflow in which you create Swagger metadata first and use that tooscaffold (auto-generate) server code for hello API.</span></span> 

<span data-ttu-id="cdce6-120">Módosítsa a könyvtárat toohello *start* mappát, majd futtassa `yo swaggerize`.</span><span class="sxs-lookup"><span data-stu-id="cdce6-120">Change directory toohello *start* folder, then run `yo swaggerize`.</span></span> <span data-ttu-id="cdce6-121">Swaggerize létrehoz egy Node.js-projektet az API-hello Swagger-definíciójában a *api.json*.</span><span class="sxs-lookup"><span data-stu-id="cdce6-121">Swaggerize creates a Node.js project for your API from hello Swagger definition in *api.json*.</span></span>

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

<span data-ttu-id="cdce6-122">Amikor a Swaggerize egy projekt nevének megadását kéri, használja a *ContactList* nevet.</span><span class="sxs-lookup"><span data-stu-id="cdce6-122">When Swaggerize asks for a project name, use *ContactList*.</span></span>
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like toocall this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-hello-project-code"></a><span data-ttu-id="cdce6-123">Hello projekt kód testreszabása</span><span class="sxs-lookup"><span data-stu-id="cdce6-123">Customize hello project code</span></span>

1. <span data-ttu-id="cdce6-124">Másolás hello *lib* hello mappából *ContactList* által létrehozott mappa `yo swaggerize`, majd módosítsa a könyvtárat a *ContactList*.</span><span class="sxs-lookup"><span data-stu-id="cdce6-124">Copy hello *lib* folder into hello *ContactList* folder created by `yo swaggerize`, then change directory into *ContactList*.</span></span>

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. <span data-ttu-id="cdce6-125">Telepítse a hello `jsonpath` és `swaggerize-ui` NPM modult.</span><span class="sxs-lookup"><span data-stu-id="cdce6-125">Install hello `jsonpath` and `swaggerize-ui` NPM modules.</span></span> 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. <span data-ttu-id="cdce6-126">Cserélje le a hello hello kód *handlers/contacts.js* a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="cdce6-126">Replace hello code in hello *handlers/contacts.js* with hello following code:</span></span> 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    <span data-ttu-id="cdce6-127">Ez a kód hello JSON-adatokat használja *lib/contactrepository.js* által kiszolgált *lib/Contacts.JSON*.</span><span class="sxs-lookup"><span data-stu-id="cdce6-127">This code uses hello JSON data stored in *lib/contacts.json* served by *lib/contactRepository.js*.</span></span> <span data-ttu-id="cdce6-128">új hello *contacts.js* kódot az összes névjegy hello tárházban JSON-adatként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cdce6-128">hello new *contacts.js* code returns all contacts in hello repository as a JSON payload.</span></span> 

4. <span data-ttu-id="cdce6-129">Cserélje le a hello hello kód **handlers/contacts/{id}.js** hello kód a következő fájlt:</span><span class="sxs-lookup"><span data-stu-id="cdce6-129">Replace hello code in hello **handlers/contacts/{id}.js** file with hello following code:</span></span>

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    <span data-ttu-id="cdce6-130">Ez a kód lehetővé teszi, hogy a elérési utat változó tooreturn csak hello lépjen kapcsolatba a megadott azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cdce6-130">This code lets you use a path variable tooreturn only hello contact with a given ID.</span></span>

5. <span data-ttu-id="cdce6-131">Cserélje le a hello kód **server.js** a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="cdce6-131">Replace hello code in **server.js** with hello following code:</span></span>

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

    <span data-ttu-id="cdce6-132">Ez a kód lehetővé teszi bizonyos kisebb változásokkal toolet azt az Azure App Service működik, és egy interaktív webes felület mutatja meg az API.</span><span class="sxs-lookup"><span data-stu-id="cdce6-132">This code makes some small changes toolet it work with Azure App Service and exposes an interactive web interface for your API.</span></span>

### <a name="test-hello-api-locally"></a><span data-ttu-id="cdce6-133">Teszt hello API helyileg</span><span class="sxs-lookup"><span data-stu-id="cdce6-133">Test hello API locally</span></span>

1. <span data-ttu-id="cdce6-134">Hello Node.js-alkalmazás indítása</span><span class="sxs-lookup"><span data-stu-id="cdce6-134">Start up hello Node.js app</span></span>
    ```bash
    npm start
    ```
    
2. <span data-ttu-id="cdce6-135">Keresse meg a toohttp://localhost:8000 / tooview hello JSON kapcsolódik a hello teljes, a ismerőslistájába.</span><span class="sxs-lookup"><span data-stu-id="cdce6-135">Browse toohttp://localhost:8000/contacts tooview hello JSON for hello entire contact list.</span></span>
   
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

3. <span data-ttu-id="cdce6-136">Keresse meg a toohttp://localhost:8000/contacts/2 tooview hello kapcsolatba lépnek egy `id` két.</span><span class="sxs-lookup"><span data-stu-id="cdce6-136">Browse toohttp://localhost:8000/contacts/2 tooview hello contact with an `id` of two.</span></span>
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. <span data-ttu-id="cdce6-137">Teszt hello API http://localhost:8000/docs: hello Swagger webes felhasználói felületének használatával.</span><span class="sxs-lookup"><span data-stu-id="cdce6-137">Test hello API using hello Swagger web interface at http://localhost:8000/docs.</span></span>
   
    ![Swagger webes felület](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <span data-ttu-id="cdce6-139"><a id="createapiapp"></a> API-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cdce6-139"><a id="createapiapp"></a> Create an API App</span></span>

<span data-ttu-id="cdce6-140">Ebben a szakaszban a hello Azure CLI 2.0 toocreate hello erőforrások toohost hello API Azure App Service használhatja.</span><span class="sxs-lookup"><span data-stu-id="cdce6-140">In this section, you use hello Azure CLI 2.0 toocreate hello resources toohost hello API on Azure App Service.</span></span> 

1.  <span data-ttu-id="cdce6-141">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="cdce6-141">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

    ```azurecli-interactive
    az login
    ```

2. <span data-ttu-id="cdce6-142">Ha több Azure-előfizetéssel rendelkezik, a módosítás hello alapértelmezett előfizetés toohello szükséges egy.</span><span class="sxs-lookup"><span data-stu-id="cdce6-142">If you have multiple Azure subscriptions, change hello default subscription toohello desired one.</span></span>

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-hello-api-with-git"></a><span data-ttu-id="cdce6-143">A Git hello API telepítése</span><span class="sxs-lookup"><span data-stu-id="cdce6-143">Deploy hello API with Git</span></span>

<span data-ttu-id="cdce6-144">A kód toohello API-alkalmazás telepítése a helyi Git-tárház tooAzure App Service a véglegesítéseknek.</span><span class="sxs-lookup"><span data-stu-id="cdce6-144">Deploy your code toohello API app by pushing commits from your local Git repository tooAzure App Service.</span></span>

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. <span data-ttu-id="cdce6-145">Egy új, hello a tárház inicializálása *ContactList* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cdce6-145">Initialize a new repo in hello *ContactList* directory.</span></span> 

    ```bash
    git init .
    ```

3. <span data-ttu-id="cdce6-146">Hello kizárása *node_modules* hozta létre a korábbi lépésben npm hello oktatóanyag a Git könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cdce6-146">Exclude hello *node_modules* directory created by npm in an earlier step in hello tutorial from Git.</span></span> <span data-ttu-id="cdce6-147">Hozzon létre egy új `.gitignore` hello aktuális könyvtárban található fájlt, és adja hozzá a következő szöveg bárhol hello fájlban egy új sor hello.</span><span class="sxs-lookup"><span data-stu-id="cdce6-147">Create a new `.gitignore` file in hello current directory and add hello following text on a new line anywhere in hello file.</span></span>

    ```
    node_modules/
    ```
    <span data-ttu-id="cdce6-148">Erősítse meg a hello `node_modules` mappa figyelmen kívül hagyja a `git status`.</span><span class="sxs-lookup"><span data-stu-id="cdce6-148">Confirm hello `node_modules` folder is being ignored with  `git status`.</span></span>

4. <span data-ttu-id="cdce6-149">Hello módosítások toohello tárház véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="cdce6-149">Commit hello changes toohello repo.</span></span>
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push tooAzure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-hello-api--in-azure"></a><span data-ttu-id="cdce6-150">Teszt hello API az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="cdce6-150">Test hello API  in Azure</span></span>

1. <span data-ttu-id="cdce6-151">Nyissa meg a böngésző toohttp://app_name.azurewebsites.net/contacts.</span><span class="sxs-lookup"><span data-stu-id="cdce6-151">Open a browser toohttp://app_name.azurewebsites.net/contacts.</span></span> <span data-ttu-id="cdce6-152">Megjelenik a hello az oktatóanyag korábbi helyileg hello kérelem elküldésekor vissza azonos JSON hello.</span><span class="sxs-lookup"><span data-stu-id="cdce6-152">You see hello same JSON returned as when you made hello request locally earlier in hello tutorial.</span></span>

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

2. <span data-ttu-id="cdce6-153">Egy böngészőben nyissa meg toohello `http://app_name.azurewebsites.net/docs` végpont tootry kimenő hello Swagger felhasználói Felületet Azure-on futó.</span><span class="sxs-lookup"><span data-stu-id="cdce6-153">In a browser, go toohello `http://app_name.azurewebsites.net/docs` endpoint tootry out hello Swagger UI running on Azure.</span></span>

    ![Swagger Ii](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    <span data-ttu-id="cdce6-155">Most már telepítheti frissítések toohello minta API tooAzure egyszerűen véglegesítések toohello Azure Git-tárház küldésével.</span><span class="sxs-lookup"><span data-stu-id="cdce6-155">You can now deploy updates toohello sample API tooAzure simply by pushing commits toohello Azure Git repository.</span></span>

## <a name="clean-up"></a><span data-ttu-id="cdce6-156">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cdce6-156">Clean up</span></span>

<span data-ttu-id="cdce6-157">a gyors üzembe helyezés, futtassa a következő parancs az Azure parancssori felület hello létrehozott hello erőforrások tooclean:</span><span class="sxs-lookup"><span data-stu-id="cdce6-157">tooclean up hello resources created in this quickstart, run hello following Azure CLI command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a><span data-ttu-id="cdce6-158">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="cdce6-158">Next step</span></span> 
> [!div class="nextstepaction"]
> [<span data-ttu-id="cdce6-159">API-alkalmazások használata JavaScript-ügyfelekkel a CORS segítségével</span><span class="sxs-lookup"><span data-stu-id="cdce6-159">Consume API apps from JavaScript clients with CORS</span></span>](app-service-api-cors-consume-javascript.md)

