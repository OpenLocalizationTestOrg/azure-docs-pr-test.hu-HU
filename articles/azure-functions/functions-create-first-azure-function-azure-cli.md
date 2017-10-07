---
title: "az első függvény eltávolítása hello Azure CLI aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan kiszolgáló nélküli végrehajtási a függvény az első Azure toocreate hello Azure parancssori felület."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 08/22/2017
ms.topic: hero-article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: erikre
ms.openlocfilehash: 5feed0045d4998b88b0e1bb50996cb7bb42b0822
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-using-hello-azure-cli"></a>Hozzon létre az első függvényét hello Azure parancssori felület használatával

A gyors üzembe helyezési oktatóanyag bemutatja, hogyan azon, hogyan toouse Azure Functions toocreate az első függvényét. Hello Azure CLI toocreate egy függvény alkalmazást, mert a kiszolgáló nélküli infrastruktúra hello, amelyen a függvényt használja. hello függvény a forráskód egy minta GitHub-tárházban van telepítve.    

A lépésekkel hello alatt a Mac, Windows vagy Linux számítógépen. 

## <a name="prerequisites"></a>Előfeltételek 

Ez a minta futtatásához rendelkeznie kell hello következő:

+ Aktív [GitHub](https://github.com)-fiók. 
+ Aktív Azure-előfizetés.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create). Az Azure-erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat (például a függvényalkalmazásokat, az adatbázisokat és a tárfiókokat).

hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup`.  
Ha nem a Cloud Shellt használja, először be kell jelentkeznie a(z) `az login` használatával.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a>Azure Storage-fiók létrehozása

Funkciók egy Azure Storage-fiók toomaintain állapota és a funkciók egyéb információkat használja. Hozzon létre egy tárfiókot hello segítségével létrehozott hello erőforráscsoportban [az storage-fiók létrehozása](/cli/azure/storage/account#create) parancsot.

A hello a következő parancsot, helyettesítse a saját globálisan egyedi tárfióknév hello megtapasztalhatja `<storage_name>` helyőrző. A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Hello storage-fiók létrehozása után a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

## <a name="create-a-function-app"></a>Függvényalkalmazás létrehozása

Rendelkeznie kell egy függvény app toohost hello a függvények végrehajtásához szükséges. hello függvény alkalmazást, a függvény kód kiszolgáló nélküli végrehajtási környezetet biztosít. Lehetővé teszi, hogy logikai egységbe csoportosítsa a függvényeket az erőforrások egyszerűbb kezelése, üzembe helyezése és megosztása érdekében. Hozzon létre egy függvény app hello [az functionapp létrehozása](/cli/azure/functionapp#create) parancsot. 

A hello a következő parancsot, helyettesítse a saját egyedi alkalmazás függvénynév hello megtapasztalhatja `<app_name>` helyőrző és hello tárfiók neve a `<storage_name>`. Hello `<app_name>` használatos az hello alapértelmezett DNS-tartomány hello függvény alkalmazáshoz, és így hello nevét kell toobe egyedi az Azure-ban minden alkalmazások között. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
Alapértelmezés szerint a függvény alkalmazások hello fogyasztás üzemeltetési terv, ami azt jelenti, hogy az erőforrások hozzáadása a funkciók által megkövetelt dinamikusan, és csak akkor kell fizetnie, ha függvények futnak hozza létre. További információkért lásd: [válasszon hello megfelelő üzemeltetési terv](functions-scale.md). 

Hello függvény alkalmazás létrehozása után a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Most, hogy egy függvény alkalmazást, hello GitHub minta adattárból hello tényleges függvény kódot is telepíthet.

## <a name="deploy-your-function-code"></a>A függvénykód létrehozása  

Nincsenek számos módon toocreate a funkciókódot az új függvény alkalmazás. Ez a témakör tooa minta GitHub-tárház csatlakozik. Mint korábban hello kód a következő cserélje le hello `<app_name>` helyőrzőt hello függvény alkalmazás létrehozott hello nevét. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Hello a telepítés utáni forrás lett beállítva, az Azure parancssori felület jeleníti meg információt a következő példa (az olvashatóság eltávolított null értékeket) hasonló toohello hello:

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## <a name="test-hello-function"></a>Hello függvény tesztelése

Használata cURL tootest hello funkció Mac vagy Linux rendszerű számítógépen vagy a Bash használatával a Windows rendszerbe. Hajtsa végre a következő cURL-parancsot, cseréje hello hello `<app_name>` helyőrzőt a függvény alkalmazás hello nevét. Hello lekérdezési karakterlánc hozzáfűzése `&name=<yourname>` toohello URL-CÍMÉT.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![A függvény válasza a böngészőben jelenik meg.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Ha nem érhető el, a parancssorban cURL, adjon meg hello hello címet a webböngésző URL-CÍMÉRE. Újra, cserélje le a hello `<app_name>` helyőrző hello nevet, a függvény alkalmazást, és a hozzáfűző hello lekérdezési karakterlánc `&name=<yourname>` toohello URL-cím és hello kérelem végrehajtása. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![A függvény válasza a böngészőben jelenik meg.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épül. Ha további quickstarts vagy hello oktatóanyagok toowork toocontinue tervezi, tegye a gyors üzembe helyezés létrehozott hello erőforrások nem törlése. Ha nem tervezi toocontinue, a következő parancs toodelete hello az összes erőforrást felhasználják hozta létre a gyors üzembe helyezés:

```azurecli-interactive
az group delete --name myResourceGroup
```
Amikor a rendszer kéri, írja be a `y` parancsot.

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
