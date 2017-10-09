---
title: "aaaBuild egy kapacitású alkalmazást az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse különböző Azure-szolgáltatások toomaximize hello teljesítmény egy ASP.NET-alkalmazás az Azure-ban."
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: erikre
editor: 
ms.assetid: a4d49ac7-0f97-4997-84c5-cdb9c4465757
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/23/2017
ms.author: cephalin
ms.openlocfilehash: 7952647b49a82c286c6a737eb41a7f23a13fd75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a><span data-ttu-id="556a0-103">Az Azure-ban kapacitású webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="556a0-103">Build a hyper-scale web app in Azure</span></span>

<span data-ttu-id="556a0-104">Az oktatóanyag bemutatja, hogyan tooscale kimenő ASP.NET webalkalmazás az Azure toomaximize felhasználó kéri.</span><span class="sxs-lookup"><span data-stu-id="556a0-104">This tutorial shows you how tooscale out an ASP.NET web app in Azure toomaximize user requests.</span></span>

<span data-ttu-id="556a0-105">Az oktatóanyag elindítása előtt győződjön meg arról, hogy [hello Azure parancssori felület telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="556a0-105">Before starting this tutorial, ensure that [hello Azure CLI is installed](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span> <span data-ttu-id="556a0-106">Továbbá szüksége [Visual Studio](https://www.visualstudio.com/vs/) a helyi számítógép toorun hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="556a0-106">In addition, you need [Visual Studio](https://www.visualstudio.com/vs/) on your local machine toorun hello sample application.</span></span>

## <a name="step-1---get-sample-application"></a><span data-ttu-id="556a0-107">1. lépés – Get mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="556a0-107">Step 1 - Get sample application</span></span>
<span data-ttu-id="556a0-108">Ebben a lépésben beállíthatja hello helyi ASP.NET-projekt.</span><span class="sxs-lookup"><span data-stu-id="556a0-108">In this step, you set up hello local ASP.NET project.</span></span>

### <a name="clone-hello-application-repository"></a><span data-ttu-id="556a0-109">Klónozás hello alkalmazás tárház</span><span class="sxs-lookup"><span data-stu-id="556a0-109">Clone hello application repository</span></span>

<span data-ttu-id="556a0-110">Nyissa meg hello parancssori terminált az Ön által választott és `CD` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="556a0-110">Open hello command-line terminal of your choice and `CD` tooa working directory.</span></span> <span data-ttu-id="556a0-111">Ezt követően a futtatási hello következő tooclone hello mintaalkalmazás parancsokat.</span><span class="sxs-lookup"><span data-stu-id="556a0-111">Then, run hello following commands tooclone hello sample application.</span></span> 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-hello-sample-application-in-visual-studio"></a><span data-ttu-id="556a0-112">A Visual Studio hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="556a0-112">Run hello sample application in Visual Studio</span></span>

<span data-ttu-id="556a0-113">Nyissa meg a hello megoldást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="556a0-113">Open hello solution in Visual Studio.</span></span>

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

<span data-ttu-id="556a0-114">Típus `F5` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="556a0-114">Type `F5` toorun hello application.</span></span>

<span data-ttu-id="556a0-115">Ez a minta ASP.NET webalkalmazás hello alapértelmezett sablon származik, és továbbra is fennáll a felhasználói munkamenetek és felhasználási hello a kimeneti gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="556a0-115">This sample ASP.NET web application comes from hello default template, and persists user sessions and uses hello output cache.</span></span> <span data-ttu-id="556a0-116">Vessen egy pillantást `HighScaleApp\Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="556a0-116">Take a look at `HighScaleApp\Controllers\HomeController.cs`.</span></span> <span data-ttu-id="556a0-117">Hello `Index()` metódus ad adatok toohello munkamenet része.</span><span class="sxs-lookup"><span data-stu-id="556a0-117">hello `Index()` method adds a piece of data toohello session.</span></span>

```csharp
Session.Add("visited", "true"); 
```

<span data-ttu-id="556a0-118">És hello `About()` és `Contact()` módszerek a kimeneti gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="556a0-118">And hello `About()` and `Contact()` methods cache their output.</span></span>

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-tooazure"></a><span data-ttu-id="556a0-119">2. lépés - tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="556a0-119">Step 2 - Deploy tooAzure</span></span>
<span data-ttu-id="556a0-120">Ebben a lépésben egy Azure-webalkalmazás létrehozása és központi telepítése a minta ASP.NET alkalmazás tooit.</span><span class="sxs-lookup"><span data-stu-id="556a0-120">In this step, you create an Azure web app and deploy your sample ASP.NET application tooit.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="556a0-121">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="556a0-121">Create a resource group</span></span>   
<span data-ttu-id="556a0-122">Használjon [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) toocreate egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello Nyugat-Európában régióban.</span><span class="sxs-lookup"><span data-stu-id="556a0-122">Use [az group create](https://docs.microsoft.com/cli/azure/group#create) toocreate a [resource group](../azure-resource-manager/resource-group-overview.md) in hello West Europe region.</span></span> <span data-ttu-id="556a0-123">Erőforráscsoport meg, ahová az összes hello toomanage együtt, kívánt Azure-erőforrások, például a háttér hello web app és az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="556a0-123">A resource group is where you put all hello Azure resources that you want toomanage together, such as hello web app and any SQL Database back end.</span></span>

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

<span data-ttu-id="556a0-124">toosee milyen lehetséges értékei, akkor is használhat `---location`, használja a hello [az App Service lista-helyek](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) parancs.</span><span class="sxs-lookup"><span data-stu-id="556a0-124">toosee what possible values you can use for `---location`, use hello [az appservice list-locations](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="556a0-125">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="556a0-125">Create an App Service plan</span></span>
<span data-ttu-id="556a0-126">Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate "B1" [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="556a0-126">Use [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) toocreate a "B1" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

<span data-ttu-id="556a0-127">Az App Service-csomag a méretezési egység, ami magában foglalhatja az alkalmazások tooscale szeretné, hogy fel, vagy ki együtt over hello azonos tetszőleges számú App Service-infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="556a0-127">An App Service plan is a scale unit, which can include any number of apps that you want tooscale up or out together over hello same App Service infrastructure.</span></span> <span data-ttu-id="556a0-128">Minden terv is hozzá van rendelve egy [tarifacsomag](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="556a0-128">Each plan is also assigned a [pricing tier](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span></span> <span data-ttu-id="556a0-129">Magasabb szolgáltatásszintek jobb hardver- és további funkciók, például több kibővített példány.</span><span class="sxs-lookup"><span data-stu-id="556a0-129">Higher tiers include better hardware and more features, such as more scale-out instances.</span></span>

<span data-ttu-id="556a0-130">Ebben az oktatóanyagban B1 hello minimális réteghez, amely lehetővé teszi a bővített kapacitású toothree példányok esetén.</span><span class="sxs-lookup"><span data-stu-id="556a0-130">For this tutorial, B1 is hello minimum tier that enables scale out toothree instances.</span></span> <span data-ttu-id="556a0-131">Áthelyezheti az alkalmazás mindig felfelé vagy lefelé futtatásával később tarifacsomag hello [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span><span class="sxs-lookup"><span data-stu-id="556a0-131">You can always move your app up or down hello pricing tier later by running [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span></span> 

### <a name="create-a-web-app"></a><span data-ttu-id="556a0-132">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="556a0-132">Create a web app</span></span>
<span data-ttu-id="556a0-133">Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate egy egyedi nevet a webalkalmazás a `$appName`.</span><span class="sxs-lookup"><span data-stu-id="556a0-133">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate a web app with a unique name in `$appName`.</span></span>

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a><span data-ttu-id="556a0-134">Telepítési hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="556a0-134">Set deployment credentials</span></span>
<span data-ttu-id="556a0-135">Használjon [az App Service web beállítva üzembe helyező felhasználó](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) a fiók szintű telepítési hitelesítő adatait, az App Service tooset.</span><span class="sxs-lookup"><span data-stu-id="556a0-135">Use [az appservice web deployment user set](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) tooset your account-level deployment credentials for App Service.</span></span>

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a><span data-ttu-id="556a0-136">Git-telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="556a0-136">Configure Git deployment</span></span>
<span data-ttu-id="556a0-137">Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure helyi Git-telepítés.</span><span class="sxs-lookup"><span data-stu-id="556a0-137">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure local Git deployment.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

<span data-ttu-id="556a0-138">Ez a parancs lehetővé teszi, hogy a következőhöz hasonló hello kimenetnek:</span><span class="sxs-lookup"><span data-stu-id="556a0-138">This command gives you an output that looks like hello following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="556a0-139">Használjon hello visszaadott URL-cím tooconfigure a Git távoli.</span><span class="sxs-lookup"><span data-stu-id="556a0-139">Use hello returned URL tooconfigure your Git remote.</span></span> <span data-ttu-id="556a0-140">hello következő parancs hello megelőző példa a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="556a0-140">hello following command uses hello preceding output example.</span></span>

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-hello-sample-application"></a><span data-ttu-id="556a0-141">Hello mintaalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="556a0-141">Deploy hello sample application</span></span>
<span data-ttu-id="556a0-142">Meg vannak toodeploy most már készen áll a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="556a0-142">You are now ready toodeploy your sample application.</span></span> <span data-ttu-id="556a0-143">Futtassa az `git push` parancsot.</span><span class="sxs-lookup"><span data-stu-id="556a0-143">Run `git push`.</span></span>

```powershell
git push azure master
```

<span data-ttu-id="556a0-144">A jelszó megadását kéri, ha a megadott kulcsfájloknak hello-jelszót használja `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="556a0-144">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-tooazure-web-app"></a><span data-ttu-id="556a0-145">Keresse meg a tooAzure webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="556a0-145">Browse tooAzure web app</span></span>
<span data-ttu-id="556a0-146">Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee az alkalmazás fut élőben az Azure, a parancs futtatásához.</span><span class="sxs-lookup"><span data-stu-id="556a0-146">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee your app running live in Azure, run this command.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-tooredis"></a><span data-ttu-id="556a0-147">3. lépés - Csatlakozás tooRedis</span><span class="sxs-lookup"><span data-stu-id="556a0-147">Step 3 - Connect tooRedis</span></span>
<span data-ttu-id="556a0-148">Ebben a lépésben beállított Azure Redis Cache-gyorsítótár külső, közös elhelyezésű tooyour Azure-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="556a0-148">In this step, you set up Azure Redis Cache as an external, colocated cache tooyour Azure web app.</span></span> <span data-ttu-id="556a0-149">Gyorsan használhatja a Redis toocache a lap kimenete.</span><span class="sxs-lookup"><span data-stu-id="556a0-149">You can quickly utilize Redis toocache your page output.</span></span> <span data-ttu-id="556a0-150">Továbbá, amikor később kiterjesztése a webalkalmazások, Redis segít megőrizni a felhasználói munkamenetek több példánya megbízhatóan.</span><span class="sxs-lookup"><span data-stu-id="556a0-150">In addition, when you scale out your web apps later, Redis helps you persist user sessions across multiple instances reliably.</span></span>

### <a name="create-an-azure-redis-cache"></a><span data-ttu-id="556a0-151">Azure Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="556a0-151">Create an Azure Redis Cache</span></span>
<span data-ttu-id="556a0-152">Használjon [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate az Azure Redis Cache-gyorsítótár és menthet hello JSON kimeneti.</span><span class="sxs-lookup"><span data-stu-id="556a0-152">Use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate an Azure Redis Cache and save hello JSON output.</span></span> <span data-ttu-id="556a0-153">Egyedi nevet a `$cacheName`.</span><span class="sxs-lookup"><span data-stu-id="556a0-153">Use a unique name in `$cacheName`.</span></span>

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-hello-application-toouse-redis"></a><span data-ttu-id="556a0-154">Hello alkalmazás toouse Redis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="556a0-154">Configure hello application toouse Redis</span></span>
<span data-ttu-id="556a0-155">A gyorsítótár hello kapcsolati karakterlánc formátuma.</span><span class="sxs-lookup"><span data-stu-id="556a0-155">Format hello connection string for your cache.</span></span>

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

<span data-ttu-id="556a0-156">hello második sorban adjon meg, a következőhöz hasonló kimenetnek:</span><span class="sxs-lookup"><span data-stu-id="556a0-156">hello second line should give you an output that looks like this:</span></span>

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

<span data-ttu-id="556a0-157">A projekt gyökérkönyvtárában nevű webes konfigurációs fájl létrehozása a Visual Studio `redis.config` és a következő kód beillesztése hello bele.</span><span class="sxs-lookup"><span data-stu-id="556a0-157">In Visual Studio, create a web configuration file in your project root called `redis.config` and paste hello following code into it.</span></span> <span data-ttu-id="556a0-158">A `value`, hello kapcsolati karakterláncnak a következőről hello PowerShell kimeneti használja.</span><span class="sxs-lookup"><span data-stu-id="556a0-158">In `value`, use hello connection string from hello PowerShell output.</span></span>

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

<span data-ttu-id="556a0-159">Ha megnézzük hello `.gitignore` fájl a Git-tárházban, láthatja, hogy a fájl nem a verziókövetésből.</span><span class="sxs-lookup"><span data-stu-id="556a0-159">If you look at hello `.gitignore` file in your Git repository, you'll see that this file is excluded from source control.</span></span> <span data-ttu-id="556a0-160">Ezzel a módszerrel a bizalmas információk biztonságos maradjon.</span><span class="sxs-lookup"><span data-stu-id="556a0-160">That way your sensitive information is kept safe.</span></span> 

<span data-ttu-id="556a0-161">Nyissa meg `Web.config`.</span><span class="sxs-lookup"><span data-stu-id="556a0-161">Open `Web.config`.</span></span> <span data-ttu-id="556a0-162">Értesítés hello `<appSettings file="redis.config">` elemet, amely lekérdezi a létrehozott hello beállítás `redis.config`.</span><span class="sxs-lookup"><span data-stu-id="556a0-162">Notice hello `<appSettings file="redis.config">` element, which gets hello setting you created in `redis.config`.</span></span> 

<span data-ttu-id="556a0-163">Hello megjegyzésként található, amely tartalmazza a szakasz `<sessionState>` és `<caching>`.</span><span class="sxs-lookup"><span data-stu-id="556a0-163">Find hello commented section that includes `<sessionState>` and `<caching>`.</span></span> <span data-ttu-id="556a0-164">Állítsa vissza az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="556a0-164">Uncomment this section.</span></span>

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

<span data-ttu-id="556a0-165">Ez a kód keresi hello Redis kapcsolódási karakterlánc ben megadott `RedisConnection`.</span><span class="sxs-lookup"><span data-stu-id="556a0-165">This code looks for hello Redis connection string you defined in `RedisConnection`.</span></span> 

<span data-ttu-id="556a0-166">Az alkalmazás most már használja a Redis toomanage munkamenetek és a gyorsítótárazást.</span><span class="sxs-lookup"><span data-stu-id="556a0-166">Your application now uses Redis toomanage sessions and caching.</span></span> <span data-ttu-id="556a0-167">Típus `F5` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="556a0-167">Type `F5` toorun hello application.</span></span> <span data-ttu-id="556a0-168">Ha kívánja letöltheti a Redis felügyeleti ügyfél toovisualize hello most mentett adatok toohello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="556a0-168">If you like, you can download a Redis management client toovisualize hello data that is now saved toohello cache.</span></span>

### <a name="configure-hello-connection-string-in-azure"></a><span data-ttu-id="556a0-169">Hello kapcsolati karakterlánc konfigurálása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="556a0-169">Configure hello connection string in Azure</span></span>

<span data-ttu-id="556a0-170">Az alkalmazás toowork az Azure-ban, meg kell tooconfigure hello ugyanazt a Redis a kapcsolati karakterláncot az Azure web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="556a0-170">For your application toowork in Azure, you need tooconfigure hello same Redis connection string in your Azure web app.</span></span> <span data-ttu-id="556a0-171">Mivel a `redis.config` nem őrzi meg a verziókövetés, még nincs üzembe helyezett tooAzure Git-telepítés futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="556a0-171">Since `redis.config` is not maintained in source control, it is not deployed tooAzure when you run Git deployment.</span></span>

<span data-ttu-id="556a0-172">Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello kapcsolati karakterlánc hello azonos neve (`RedisConnection`).</span><span class="sxs-lookup"><span data-stu-id="556a0-172">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd hello connection string with hello same name (`RedisConnection`).</span></span>

<span data-ttu-id="556a0-173">az App Service web config appsettings frissítése – beállítások "RedisConnection = $connstring"--$appName--myResourceGroup erőforráscsoport név</span><span class="sxs-lookup"><span data-stu-id="556a0-173">az appservice web config appsettings update --settings "RedisConnection=$connstring" --name $appName --resource-group myResourceGroup</span></span>

<span data-ttu-id="556a0-174">Ne feledje, hogy `$connstring` formázott hello kapcsolati karakterláncot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="556a0-174">Remember that `$connstring` contains hello formatted connection string.</span></span>

### <a name="redeploy-hello-application-tooazure"></a><span data-ttu-id="556a0-175">Telepítse újra a hello alkalmazás tooAzure</span><span class="sxs-lookup"><span data-stu-id="556a0-175">Redeploy hello application tooAzure</span></span>
<span data-ttu-id="556a0-176">A módosítások tooAzure használja a Git-parancsok toopush</span><span class="sxs-lookup"><span data-stu-id="556a0-176">Use Git commands toopush your changes tooAzure</span></span>

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

<span data-ttu-id="556a0-177">A jelszó megadását kéri, ha a megadott kulcsfájloknak hello-jelszót használja `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="556a0-177">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="556a0-178">Keresse meg az Azure-webalkalmazásban toohello</span><span class="sxs-lookup"><span data-stu-id="556a0-178">Browse toohello Azure web app</span></span>
<span data-ttu-id="556a0-179">Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello módosítások élő Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="556a0-179">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) toosee hello changes live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-toomultiple-instances"></a><span data-ttu-id="556a0-180">4. lépés: méretezési toomultiple példányok</span><span class="sxs-lookup"><span data-stu-id="556a0-180">Step 4 - Scale toomultiple instances</span></span>
<span data-ttu-id="556a0-181">App Service-csomag hello hello skálázási egység az Azure web Apps.</span><span class="sxs-lookup"><span data-stu-id="556a0-181">hello App Service plan is hello scale unit for your Azure web apps.</span></span> <span data-ttu-id="556a0-182">a webalkalmazás kimenő tooscale, méretezhető hello App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="556a0-182">tooscale out your web app, you scale hello App Service plan.</span></span>

<span data-ttu-id="556a0-183">Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale kimenő hello az alkalmazásszolgáltatási csomag toothree példányok, amely hello hello B1 IP-címek által engedélyezett maximális számát.</span><span class="sxs-lookup"><span data-stu-id="556a0-183">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale out hello App Service plan toothree instances, which is hello maximum number allowed by hello B1 pricing tier.</span></span> <span data-ttu-id="556a0-184">Ne feledje, hogy B1 hello hello korábban az App Service-csomag létrehozásakor választott tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="556a0-184">Remember that B1 is hello pricing tier that you chose when you created hello App Service plan earlier.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a><span data-ttu-id="556a0-185">5 - skálázási földrajzilag lépés</span><span class="sxs-lookup"><span data-stu-id="556a0-185">Step 5 - Scale geographically</span></span>
<span data-ttu-id="556a0-186">Skálázás földrajzilag, amikor az alkalmazás futtatása az Azure-felhőbe hello több régióba.</span><span class="sxs-lookup"><span data-stu-id="556a0-186">When scaling geographically, you run your app in multiple regions of hello Azure cloud.</span></span> <span data-ttu-id="556a0-187">A telepítő kiegyenlíti az alkalmazás további földrajzi hely alapján, és úgy, hogy az alkalmazás szorosabb tooclient böngészők hello válaszidő csökkenti.</span><span class="sxs-lookup"><span data-stu-id="556a0-187">This setup load-balances your app further based on geography and lowers hello response time by placing your app closer tooclient browsers.</span></span>

<span data-ttu-id="556a0-188">Ebben a lépésben az ASP.NET web app tooa második régióban, méretezhető [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span><span class="sxs-lookup"><span data-stu-id="556a0-188">In this step, you scale your ASP.NET web app tooa second region with [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span></span> <span data-ttu-id="556a0-189">Hello lépés végén hello hogy egy webalkalmazást, Nyugat-Európában (már létrehozott) futó és a webalkalmazás fut (még nem hozott létre) Délkelet-Ázsiában.</span><span class="sxs-lookup"><span data-stu-id="556a0-189">At hello end of hello step, you will have a web app running in West Europe (already created) and a web app running in Southeast Asia (not yet created).</span></span> <span data-ttu-id="556a0-190">Mindkét alkalmazásban szolgáltató hello a Traffic Manager URL-címe megegyezik.</span><span class="sxs-lookup"><span data-stu-id="556a0-190">Both apps will be served from hello same Traffic Manager URL.</span></span>

### <a name="scale-up-hello-europe-app-toostandard-tier"></a><span data-ttu-id="556a0-191">Vertikális felskálázás hello Európa app tooStandard réteg</span><span class="sxs-lookup"><span data-stu-id="556a0-191">Scale up hello Europe app tooStandard tier</span></span>
<span data-ttu-id="556a0-192">Az App Service-ben az Azure Traffic Manager integrációs hello a Standard tarifacsomag szükséges.</span><span class="sxs-lookup"><span data-stu-id="556a0-192">In App Service, integration with Azure Traffic Manager requires hello Standard pricing tier.</span></span> <span data-ttu-id="556a0-193">Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale az App Service-csomag tooS1 fel.</span><span class="sxs-lookup"><span data-stu-id="556a0-193">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) tooscale up your App Service plan tooS1.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="556a0-194">Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="556a0-194">Create a Traffic Manager profile</span></span> 
<span data-ttu-id="556a0-195">Használjon [az hálózati forgalmat-manager-profil létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate egy Traffic Manager profilt, és adja hozzá tooyour erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="556a0-195">Use [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) toocreate a Traffic Manager profile and add it tooyour resource group.</span></span> <span data-ttu-id="556a0-196">Egy egyedi DNS-név $dnsName használja.</span><span class="sxs-lookup"><span data-stu-id="556a0-196">Use a unique DNS name in $dnsName.</span></span>

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> <span data-ttu-id="556a0-197">`--routing-method Performance`Megadja, hogy ez a profil [irányítja a felhasználói forgalom toohello legközelebbi végpont](../traffic-manager/traffic-manager-routing-methods.md).</span><span class="sxs-lookup"><span data-stu-id="556a0-197">`--routing-method Performance` specifies that this profile [routes user traffic toohello closest endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span></span>

### <a name="get-hello-resource-id-of-hello-europe-app"></a><span data-ttu-id="556a0-198">Erőforrás-azonosító hello hello Európa alkalmazás beszerzése</span><span class="sxs-lookup"><span data-stu-id="556a0-198">Get hello resource ID of hello Europe app</span></span>
<span data-ttu-id="556a0-199">Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello erőforrás-azonosítója a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="556a0-199">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resource ID of your web app.</span></span>

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-europe-app"></a><span data-ttu-id="556a0-200">A Traffic Manager-végpont hello Európa alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="556a0-200">Add a Traffic Manager endpoint for hello Europe app</span></span>
<span data-ttu-id="556a0-201">Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd egy végpont tooyour Traffic Manager-profil és a használata hello erőforrás-azonosító szerint webalkalmazás hello cél.</span><span class="sxs-lookup"><span data-stu-id="556a0-201">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd an endpoint tooyour Traffic Manager profile and use hello resource ID of your web app as hello target.</span></span>

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-hello-traffic-manager-endpoint-url"></a><span data-ttu-id="556a0-202">Hello Traffic Manager-végpont URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="556a0-202">Get hello Traffic Manager endpoint URL</span></span>
<span data-ttu-id="556a0-203">A Traffic Manager-profil adott pontok tooyour már meglévő webalkalmazás most már rendelkezik egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="556a0-203">Your Traffic Manager profile now has an endpoint that points tooyour existing web app.</span></span> <span data-ttu-id="556a0-204">Használjon [az hálózati forgalmat-kezelő profil megjelenítése](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget az URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="556a0-204">Use [az network traffic-manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) tooget its URL.</span></span> 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

<span data-ttu-id="556a0-205">Hello kimenetének másolása HTML-a böngészőbe.</span><span class="sxs-lookup"><span data-stu-id="556a0-205">Copy hello output into your browser.</span></span> <span data-ttu-id="556a0-206">A webalkalmazás ismét meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="556a0-206">You should see your web app again.</span></span>

### <a name="create-an-azure-redis-cache-in-asia"></a><span data-ttu-id="556a0-207">Hozzon létre egy Azure Redis Cache Ázsiában</span><span class="sxs-lookup"><span data-stu-id="556a0-207">Create an Azure Redis Cache in Asia</span></span>
<span data-ttu-id="556a0-208">Most az Azure web app toohello Délkelet-Ázsia régióba replikálja.</span><span class="sxs-lookup"><span data-stu-id="556a0-208">Now, you replicate your Azure web app toohello Southeast Asia region.</span></span> <span data-ttu-id="556a0-209">toostart, használjon [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate egy második Azure Redis Cache-ben Délkelet-Ázsia.</span><span class="sxs-lookup"><span data-stu-id="556a0-209">toostart, use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) toocreate a second Azure Redis Cache in Southeast Asia.</span></span> <span data-ttu-id="556a0-210">Ez a gyorsítótár Ázsiában alkalmazása közösen elhelyezett toobe kell.</span><span class="sxs-lookup"><span data-stu-id="556a0-210">This cache needs toobe colocated with your app in Asia.</span></span>

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

<span data-ttu-id="556a0-211">`--name $cacheName-asia`által biztosított hello gyorsítótár hello neve hello Nyugat-Európában gyorsítótár, a hello `-asia` utótag.</span><span class="sxs-lookup"><span data-stu-id="556a0-211">`--name $cacheName-asia` gives hello cache hello name of hello West Europe cache, with hello `-asia` suffix.</span></span>

### <a name="create-an-app-service-plan-in-asia"></a><span data-ttu-id="556a0-212">Az App Service-csomag létrehozása Ázsiában</span><span class="sxs-lookup"><span data-stu-id="556a0-212">Create an App Service plan in Asia</span></span>
<span data-ttu-id="556a0-213">Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate egy második App Service-csomag hello Délkelet-Ázsia régióban, azonos S1 réteg hello segítségével hello Nyugat-Európában terv szerint.</span><span class="sxs-lookup"><span data-stu-id="556a0-213">Use [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) toocreate a second App Service plan in hello Southeast Asia region, using hello same S1 tier as hello West Europe plan.</span></span>

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a><span data-ttu-id="556a0-214">A webalkalmazás létrehozása az Ázsia</span><span class="sxs-lookup"><span data-stu-id="556a0-214">Create a web app in Asia</span></span>
<span data-ttu-id="556a0-215">Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate egy másik webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="556a0-215">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) toocreate a second web app.</span></span>

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

<span data-ttu-id="556a0-216">`--name $appName-asia`által biztosított hello hello alkalmazásnév hello Nyugat-Európában alkalmazáshoz, a hello `-asia` utótag.</span><span class="sxs-lookup"><span data-stu-id="556a0-216">`--name $appName-asia` gives hello app hello name of hello West Europe app, with hello `-asia` suffix.</span></span>

### <a name="configure-hello-connection-string-for-redis"></a><span data-ttu-id="556a0-217">A Redis hello kapcsolati karakterlánc konfigurálása</span><span class="sxs-lookup"><span data-stu-id="556a0-217">Configure hello connection string for Redis</span></span>
<span data-ttu-id="556a0-218">Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello web app hello kapcsolati karakterláncot hello Délkelet-Ázsia gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="556a0-218">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd toohello web app hello connection string for hello Southeast Asia cache.</span></span>

<span data-ttu-id="556a0-219">az App Service web config appsettings frissítése – beállítások "RedisConnection$ ($redis.hostname) =: $($redis.sslPort), a jelszó$ ($redis.accessKeys.primaryKey) ssl = = True, abortConnect = False"--name $appName-Ázsia--erőforráscsoport myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="556a0-219">az appservice web config appsettings update --settings "RedisConnection=$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False" --name $appName-asia --resource-group myResourceGroup</span></span>

### <a name="configure-git-deployment-for-hello-asia-app"></a><span data-ttu-id="556a0-220">Hello Ázsia alkalmazás Git-KözpontiTelepítés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="556a0-220">Configure Git deployment for hello Asia app.</span></span>
<span data-ttu-id="556a0-221">Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure helyi Git-telepítésének hello második webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="556a0-221">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) tooconfigure local Git deployment for hello second web app.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="556a0-222">Ez a parancs lehetővé teszi, hogy a következőhöz hasonló hello kimenetnek:</span><span class="sxs-lookup"><span data-stu-id="556a0-222">This command gives you an output that looks like hello following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="556a0-223">Használjon hello visszaadott URL-cím tooconfigure egy második Git a helyi tárház távoli.</span><span class="sxs-lookup"><span data-stu-id="556a0-223">Use hello returned URL tooconfigure a second Git remote for your local repository.</span></span> <span data-ttu-id="556a0-224">hello következő parancs hello megelőző példa a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="556a0-224">hello following command uses hello preceding output example.</span></span>

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a><span data-ttu-id="556a0-225">A mintaalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="556a0-225">Deploy your sample application</span></span>
<span data-ttu-id="556a0-226">Futtatás `git push` toodeploy a minta alkalmazás toohello második Git távoli.</span><span class="sxs-lookup"><span data-stu-id="556a0-226">Run `git push` toodeploy your sample application toohello second Git remote.</span></span> 

```bash
git push azure-asia master
```

<span data-ttu-id="556a0-227">A jelszó megadását kéri, ha a megadott kulcsfájloknak hello-jelszót használja `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="556a0-227">When prompted for password, use hello password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-toohello-asia-app"></a><span data-ttu-id="556a0-228">Keresse meg a toohello Ázsia alkalmazás</span><span class="sxs-lookup"><span data-stu-id="556a0-228">Browse toohello Asia app</span></span>
<span data-ttu-id="556a0-229">Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify, hogy az alkalmazást működés közben fut az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="556a0-229">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) tooverify that your app is running live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-hello-resource-id-of-hello-asia-app"></a><span data-ttu-id="556a0-230">Erőforrás-azonosító hello hello Ázsia alkalmazás beszerzése</span><span class="sxs-lookup"><span data-stu-id="556a0-230">Get hello resource ID of hello Asia app</span></span>
<span data-ttu-id="556a0-231">Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello erőforrás-azonosító Délkelet-Ázsiában található webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="556a0-231">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) tooget hello resource ID of your web app in Southeast Asia.</span></span>

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-hello-asia-app"></a><span data-ttu-id="556a0-232">A Traffic Manager-végpont hello Ázsia alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="556a0-232">Add a Traffic Manager endpoint for hello Asia app</span></span>
<span data-ttu-id="556a0-233">Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd egy második végpont toohello Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="556a0-233">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) tooadd a second endpoint toohello Traffic Manager profile.</span></span>

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-tooweb-apps"></a><span data-ttu-id="556a0-234">Régió azonosítója tooweb alkalmazások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="556a0-234">Add region identifier tooweb apps</span></span>
<span data-ttu-id="556a0-235">Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd egy régióspecifikus környezeti változót.</span><span class="sxs-lookup"><span data-stu-id="556a0-235">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) tooadd a region-specific environment variable.</span></span>

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="556a0-236">Az alkalmazás kódjában már használja az alkalmazás-beállítás.</span><span class="sxs-lookup"><span data-stu-id="556a0-236">Your application code already uses this application setting.</span></span> <span data-ttu-id="556a0-237">Vessen egy pillantást `HighScaleApp\Views\Home\Index.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="556a0-237">Take a look at `HighScaleApp\Views\Home\Index.cshtml`.</span></span>

### <a name="complete"></a><span data-ttu-id="556a0-238">Fejezze be!</span><span class="sxs-lookup"><span data-stu-id="556a0-238">Complete!</span></span>

<span data-ttu-id="556a0-239">Most próbáljon meg a Traffic Manager-profil a különböző földrajzi régióban böngészőkkel történő tooaccess hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="556a0-239">Now, try tooaccess hello URL of your Traffic Manager profile from browsers in different geographical regions.</span></span> <span data-ttu-id="556a0-240">Ügyfélböngészők Európából megjelennek az "ASP.NET Nyugat-Európa", és az Ázsia ügyfélböngészőnek megjelennek az "ASP.NET Délkelet-Ázsia."</span><span class="sxs-lookup"><span data-stu-id="556a0-240">Client browsers from Europe should show "ASP.NET West Europe", and client browser from Asia should show "ASP.NET Southeast Asia."</span></span>

## <a name="more-resources"></a><span data-ttu-id="556a0-241">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="556a0-241">More resources</span></span>
