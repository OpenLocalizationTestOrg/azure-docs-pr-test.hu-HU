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
# <a name="create-your-first-function-using-hello-azure-cli"></a><span data-ttu-id="6b4c6-103">Hozzon létre az első függvényét hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="6b4c6-103">Create your first function using hello Azure CLI</span></span>

<span data-ttu-id="6b4c6-104">A gyors üzembe helyezési oktatóanyag bemutatja, hogyan azon, hogyan toouse Azure Functions toocreate az első függvényét.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-104">This quickstart tutorial walks through how toouse Azure Functions toocreate your first function.</span></span> <span data-ttu-id="6b4c6-105">Hello Azure CLI toocreate egy függvény alkalmazást, mert a kiszolgáló nélküli infrastruktúra hello, amelyen a függvényt használja.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-105">You use hello Azure CLI toocreate a function app, which is hello serverless infrastructure that hosts your function.</span></span> <span data-ttu-id="6b4c6-106">hello függvény a forráskód egy minta GitHub-tárházban van telepítve.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-106">hello function code itself is deployed from a GitHub sample repository.</span></span>    

<span data-ttu-id="6b4c6-107">A lépésekkel hello alatt a Mac, Windows vagy Linux számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-107">You can follow hello steps below using a Mac, Windows, or Linux computer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6b4c6-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6b4c6-108">Prerequisites</span></span> 

<span data-ttu-id="6b4c6-109">Ez a minta futtatásához rendelkeznie kell hello következő:</span><span class="sxs-lookup"><span data-stu-id="6b4c6-109">Before running this sample, you must have hello following:</span></span>

+ <span data-ttu-id="6b4c6-110">Aktív [GitHub](https://github.com)-fiók.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-110">An active [GitHub](https://github.com) account.</span></span> 
+ <span data-ttu-id="6b4c6-111">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-111">An active Azure subscription.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6b4c6-112">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-112">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="6b4c6-113">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="6b4c6-114">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6b4c6-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="6b4c6-115">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="6b4c6-115">Create a resource group</span></span>

<span data-ttu-id="6b4c6-116">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="6b4c6-116">Create a resource group with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="6b4c6-117">Az Azure-erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat (például a függvényalkalmazásokat, az adatbázisokat és a tárfiókokat).</span><span class="sxs-lookup"><span data-stu-id="6b4c6-117">An Azure resource group is a logical container into which Azure resources like function apps, databases, and storage accounts are deployed and managed.</span></span>

<span data-ttu-id="6b4c6-118">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-118">hello following example creates a resource group named `myResourceGroup`.</span></span>  
<span data-ttu-id="6b4c6-119">Ha nem a Cloud Shellt használja, először be kell jelentkeznie a(z) `az login` használatával.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-119">If you are not using Cloud Shell, you must first sign in using `az login`.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a><span data-ttu-id="6b4c6-120">Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b4c6-120">Create an Azure Storage account</span></span>

<span data-ttu-id="6b4c6-121">Funkciók egy Azure Storage-fiók toomaintain állapota és a funkciók egyéb információkat használja.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-121">Functions uses an Azure Storage account toomaintain state and other information about your functions.</span></span> <span data-ttu-id="6b4c6-122">Hozzon létre egy tárfiókot hello segítségével létrehozott hello erőforráscsoportban [az storage-fiók létrehozása](/cli/azure/storage/account#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-122">Create a storage account in hello resource group you created by using hello [az storage account create](/cli/azure/storage/account#create) command.</span></span>

<span data-ttu-id="6b4c6-123">A hello a következő parancsot, helyettesítse a saját globálisan egyedi tárfióknév hello megtapasztalhatja `<storage_name>` helyőrző.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-123">In hello following command, substitute your own globally unique storage account name where you see hello `<storage_name>` placeholder.</span></span> <span data-ttu-id="6b4c6-124">A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-124">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

<span data-ttu-id="6b4c6-125">Hello storage-fiók létrehozása után a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="6b4c6-125">After hello storage account has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

## <a name="create-a-function-app"></a><span data-ttu-id="6b4c6-126">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b4c6-126">Create a function app</span></span>

<span data-ttu-id="6b4c6-127">Rendelkeznie kell egy függvény app toohost hello a függvények végrehajtásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-127">You must have a function app toohost hello execution of your functions.</span></span> <span data-ttu-id="6b4c6-128">hello függvény alkalmazást, a függvény kód kiszolgáló nélküli végrehajtási környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-128">hello function app provides an environment for serverless execution of your function code.</span></span> <span data-ttu-id="6b4c6-129">Lehetővé teszi, hogy logikai egységbe csoportosítsa a függvényeket az erőforrások egyszerűbb kezelése, üzembe helyezése és megosztása érdekében.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-129">It lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> <span data-ttu-id="6b4c6-130">Hozzon létre egy függvény app hello [az functionapp létrehozása](/cli/azure/functionapp#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-130">Create a function app by using hello [az functionapp create](/cli/azure/functionapp#create) command.</span></span> 

<span data-ttu-id="6b4c6-131">A hello a következő parancsot, helyettesítse a saját egyedi alkalmazás függvénynév hello megtapasztalhatja `<app_name>` helyőrző és hello tárfiók neve a `<storage_name>`.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-131">In hello following command, substitute your own unique function app name where you see hello `<app_name>` placeholder and hello storage account name for  `<storage_name>`.</span></span> <span data-ttu-id="6b4c6-132">Hello `<app_name>` használatos az hello alapértelmezett DNS-tartomány hello függvény alkalmazáshoz, és így hello nevét kell toobe egyedi az Azure-ban minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-132">hello `<app_name>` is used as hello default DNS domain for hello function app, and so hello name needs toobe unique across all apps in Azure.</span></span> 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
<span data-ttu-id="6b4c6-133">Alapértelmezés szerint a függvény alkalmazások hello fogyasztás üzemeltetési terv, ami azt jelenti, hogy az erőforrások hozzáadása a funkciók által megkövetelt dinamikusan, és csak akkor kell fizetnie, ha függvények futnak hozza létre.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-133">By default, a function app is created with hello Consumption hosting plan, which means that resources are added dynamically as required by your functions and you only pay when functions are running.</span></span> <span data-ttu-id="6b4c6-134">További információkért lásd: [válasszon hello megfelelő üzemeltetési terv](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="6b4c6-134">For more information, see [Choose hello correct hosting plan](functions-scale.md).</span></span> 

<span data-ttu-id="6b4c6-135">Hello függvény alkalmazás létrehozása után a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="6b4c6-135">After hello function app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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

<span data-ttu-id="6b4c6-136">Most, hogy egy függvény alkalmazást, hello GitHub minta adattárból hello tényleges függvény kódot is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-136">Now that you have a function app, you can deploy hello actual function code from hello GitHub sample repository.</span></span>

## <a name="deploy-your-function-code"></a><span data-ttu-id="6b4c6-137">A függvénykód létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b4c6-137">Deploy your function code</span></span>  

<span data-ttu-id="6b4c6-138">Nincsenek számos módon toocreate a funkciókódot az új függvény alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-138">There are several ways toocreate your function code in your new function app.</span></span> <span data-ttu-id="6b4c6-139">Ez a témakör tooa minta GitHub-tárház csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-139">This topic connects tooa sample repository in GitHub.</span></span> <span data-ttu-id="6b4c6-140">Mint korábban hello kód a következő cserélje le hello `<app_name>` helyőrzőt hello függvény alkalmazás létrehozott hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-140">As before, in hello following code replace hello `<app_name>` placeholder with hello name of hello function app you created.</span></span> 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
<span data-ttu-id="6b4c6-141">Hello a telepítés utáni forrás lett beállítva, az Azure parancssori felület jeleníti meg információt a következő példa (az olvashatóság eltávolított null értékeket) hasonló toohello hello:</span><span class="sxs-lookup"><span data-stu-id="6b4c6-141">After hello deployment source been set, hello Azure CLI shows information similar toohello following example (null values removed for readability):</span></span>

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

## <a name="test-hello-function"></a><span data-ttu-id="6b4c6-142">Hello függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="6b4c6-142">Test hello function</span></span>

<span data-ttu-id="6b4c6-143">Használata cURL tootest hello funkció Mac vagy Linux rendszerű számítógépen vagy a Bash használatával a Windows rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-143">Use cURL tootest hello deployed function on a Mac or Linux computer or using Bash on Windows.</span></span> <span data-ttu-id="6b4c6-144">Hajtsa végre a következő cURL-parancsot, cseréje hello hello `<app_name>` helyőrzőt a függvény alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-144">Execute hello following cURL command, replacing hello `<app_name>` placeholder with hello name of your function app.</span></span> <span data-ttu-id="6b4c6-145">Hello lekérdezési karakterlánc hozzáfűzése `&name=<yourname>` toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-145">Append hello query string `&name=<yourname>` toohello URL.</span></span>

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![A függvény válasza a böngészőben jelenik meg.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

<span data-ttu-id="6b4c6-147">Ha nem érhető el, a parancssorban cURL, adjon meg hello hello címet a webböngésző URL-CÍMÉRE.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-147">If you don't have cURL available in your command line, enter hello same URL in hello address of your web browser.</span></span> <span data-ttu-id="6b4c6-148">Újra, cserélje le a hello `<app_name>` helyőrző hello nevet, a függvény alkalmazást, és a hozzáfűző hello lekérdezési karakterlánc `&name=<yourname>` toohello URL-cím és hello kérelem végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-148">Again, replace hello `<app_name>` placeholder with hello name of your function app, and append hello query string `&name=<yourname>` toohello URL and execute hello request.</span></span> 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![A függvény válasza a böngészőben jelenik meg.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a><span data-ttu-id="6b4c6-150">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6b4c6-150">Clean up resources</span></span>

<span data-ttu-id="6b4c6-151">Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épül.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-151">Other quickstarts in this collection build upon this quickstart.</span></span> <span data-ttu-id="6b4c6-152">Ha további quickstarts vagy hello oktatóanyagok toowork toocontinue tervezi, tegye a gyors üzembe helyezés létrehozott hello erőforrások nem törlése.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-152">If you plan toocontinue on toowork with subsequent quickstarts or with hello tutorials, do not clean up hello resources created in this quickstart.</span></span> <span data-ttu-id="6b4c6-153">Ha nem tervezi toocontinue, a következő parancs toodelete hello az összes erőforrást felhasználják hozta létre a gyors üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="6b4c6-153">If you do not plan toocontinue, use hello following command toodelete all resources created by this quickstart:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```
<span data-ttu-id="6b4c6-154">Amikor a rendszer kéri, írja be a `y` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6b4c6-154">Type `y` when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b4c6-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b4c6-155">Next steps</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
