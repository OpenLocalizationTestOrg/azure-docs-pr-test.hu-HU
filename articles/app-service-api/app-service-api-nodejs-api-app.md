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
# <a name="build-a-nodejs-restful-api-and-deploy-it-tooan-api-app-in-azure"></a>Node.js RESTful API létrehozása, és telepítse azt tooan API-alkalmazás az Azure-ban
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

A gyors üzembe helyezés bemutatja, hogyan toocreate egy REST API-t készült Node.js [Express](http://expressjs.com/)használatával egy [Swagger](http://swagger.io/) definíció- és telepíti azokat, mint egy [API-alkalmazás](app-service-api-apps-why-best-platform.md) Azure. Hozzon létre hello alkalmazást parancssori eszközökkel, konfigurálja erőforrásokat hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), és a Git használatával hello alkalmazás telepítése.  Amikor végzett, Azure-on futó, működő minta REST API áll rendelkezésére majd.

## <a name="prerequisites"></a>Előfeltételek

* [Git](https://git-scm.com/)
* [ Node.js és NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="prepare-your-environment"></a>A környezet előkészítése

1. Egy terminálablakot futtassa a következő parancs tooclone hello minta tooyour helyi számítógép hello.

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. Módosítsa a hello mintakódot tartalmazó toohello könyvtár.

    ```bash
    cd app-service-api-node-contact-list
    ```

3. Telepítse a helyi gépen a [Swaggerize](https://www.npmjs.com/package/swaggerize-express) eszközt. A Swaggerize olyan eszköz, amely Swagger-definícióból generál Node.js-kódot a REST API-hoz.

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a>Node.js-kód generálása 

Ebben a szakaszban hello oktatóanyag modellek egy API fejlesztési munkafolyamat, amelyben először hozza létre a Swagger-metaadatokat, és használja, hogy tooscaffold (automatikus létrehozása) hello API kiszolgálói kódját. 

Módosítsa a könyvtárat toohello *start* mappát, majd futtassa `yo swaggerize`. Swaggerize létrehoz egy Node.js-projektet az API-hello Swagger-definíciójában a *api.json*.

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

Amikor a Swaggerize egy projekt nevének megadását kéri, használja a *ContactList* nevet.
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like toocall this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-hello-project-code"></a>Hello projekt kód testreszabása

1. Másolás hello *lib* hello mappából *ContactList* által létrehozott mappa `yo swaggerize`, majd módosítsa a könyvtárat a *ContactList*.

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. Telepítse a hello `jsonpath` és `swaggerize-ui` NPM modult. 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. Cserélje le a hello hello kód *handlers/contacts.js* a hello a következő kódot: 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    Ez a kód hello JSON-adatokat használja *lib/contactrepository.js* által kiszolgált *lib/Contacts.JSON*. új hello *contacts.js* kódot az összes névjegy hello tárházban JSON-adatként adja vissza. 

4. Cserélje le a hello hello kód **handlers/contacts/{id}.js** hello kód a következő fájlt:

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    Ez a kód lehetővé teszi, hogy a elérési utat változó tooreturn csak hello lépjen kapcsolatba a megadott azonosítóval.

5. Cserélje le a hello kód **server.js** a hello a következő kódot:

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

    Ez a kód lehetővé teszi bizonyos kisebb változásokkal toolet azt az Azure App Service működik, és egy interaktív webes felület mutatja meg az API.

### <a name="test-hello-api-locally"></a>Teszt hello API helyileg

1. Hello Node.js-alkalmazás indítása
    ```bash
    npm start
    ```
    
2. Keresse meg a toohttp://localhost:8000 / tooview hello JSON kapcsolódik a hello teljes, a ismerőslistájába.
   
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

3. Keresse meg a toohttp://localhost:8000/contacts/2 tooview hello kapcsolatba lépnek egy `id` két.
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. Teszt hello API http://localhost:8000/docs: hello Swagger webes felhasználói felületének használatával.
   
    ![Swagger webes felület](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a> API-alkalmazás létrehozása

Ebben a szakaszban a hello Azure CLI 2.0 toocreate hello erőforrások toohost hello API Azure App Service használhatja. 

1.  Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.

    ```azurecli-interactive
    az login
    ```

2. Ha több Azure-előfizetéssel rendelkezik, a módosítás hello alapértelmezett előfizetés toohello szükséges egy.

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-hello-api-with-git"></a>A Git hello API telepítése

A kód toohello API-alkalmazás telepítése a helyi Git-tárház tooAzure App Service a véglegesítéseknek.

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. Egy új, hello a tárház inicializálása *ContactList* könyvtár. 

    ```bash
    git init .
    ```

3. Hello kizárása *node_modules* hozta létre a korábbi lépésben npm hello oktatóanyag a Git könyvtár. Hozzon létre egy új `.gitignore` hello aktuális könyvtárban található fájlt, és adja hozzá a következő szöveg bárhol hello fájlban egy új sor hello.

    ```
    node_modules/
    ```
    Erősítse meg a hello `node_modules` mappa figyelmen kívül hagyja a `git status`.

4. Hello módosítások toohello tárház véglegesítése.
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push tooAzure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-hello-api--in-azure"></a>Teszt hello API az Azure-ban

1. Nyissa meg a böngésző toohttp://app_name.azurewebsites.net/contacts. Megjelenik a hello az oktatóanyag korábbi helyileg hello kérelem elküldésekor vissza azonos JSON hello.

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

2. Egy böngészőben nyissa meg toohello `http://app_name.azurewebsites.net/docs` végpont tootry kimenő hello Swagger felhasználói Felületet Azure-on futó.

    ![Swagger Ii](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    Most már telepítheti frissítések toohello minta API tooAzure egyszerűen véglegesítések toohello Azure Git-tárház küldésével.

## <a name="clean-up"></a>A fölöslegessé vált elemek eltávolítása

a gyors üzembe helyezés, futtassa a következő parancs az Azure parancssori felület hello létrehozott hello erőforrások tooclean:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a>Következő lépés 
> [!div class="nextstepaction"]
> [API-alkalmazások használata JavaScript-ügyfelekkel a CORS segítségével](app-service-api-cors-consume-javascript.md)

