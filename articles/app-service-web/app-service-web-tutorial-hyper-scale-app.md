---
title: "Hozza létre a kapacitású alkalmazását az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogy egy ASP.NET-alkalmazás az Azure-ban a teljesítmény maximalizálásához különböző Azure-szolgáltatások használatával."
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
ms.openlocfilehash: eac9c5b0d8d0f7802d88e6f4f27d9d23c406e025
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-hyper-scale-web-app-in-azure"></a><span data-ttu-id="ce547-103">Az Azure-ban kapacitású webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce547-103">Build a hyper-scale web app in Azure</span></span>

<span data-ttu-id="ce547-104">Az oktatóanyag bemutatja, hogyan horizontális felskálázás az Azure-felhasználói kéréseinek maximális ASP.NET webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ce547-104">This tutorial shows you how to scale out an ASP.NET web app in Azure to maximize user requests.</span></span>

<span data-ttu-id="ce547-105">Az oktatóanyag elindítása előtt győződjön meg arról, hogy [az Azure parancssori felület telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ce547-105">Before starting this tutorial, ensure that [the Azure CLI is installed](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span> <span data-ttu-id="ce547-106">Továbbá szüksége [Visual Studio](https://www.visualstudio.com/vs/) a helyi számítógépen futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ce547-106">In addition, you need [Visual Studio](https://www.visualstudio.com/vs/) on your local machine to run the sample application.</span></span>

## <a name="step-1---get-sample-application"></a><span data-ttu-id="ce547-107">1. lépés – Get mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="ce547-107">Step 1 - Get sample application</span></span>
<span data-ttu-id="ce547-108">Ebben a lépésben beállíthatja a helyi ASP.NET-projekt.</span><span class="sxs-lookup"><span data-stu-id="ce547-108">In this step, you set up the local ASP.NET project.</span></span>

### <a name="clone-the-application-repository"></a><span data-ttu-id="ce547-109">Az alkalmazás tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="ce547-109">Clone the application repository</span></span>

<span data-ttu-id="ce547-110">Nyissa meg a kívánt parancssori terminált, az Ön által választott és `CD` egy működő könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="ce547-110">Open the command-line terminal of your choice and `CD` to a working directory.</span></span> <span data-ttu-id="ce547-111">Ezután futtassa a következő parancsok futtatásával klónozza a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ce547-111">Then, run the following commands to clone the sample application.</span></span> 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-the-sample-application-in-visual-studio"></a><span data-ttu-id="ce547-112">Futtassa a mintaalkalmazást a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="ce547-112">Run the sample application in Visual Studio</span></span>

<span data-ttu-id="ce547-113">Nyissa meg a megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="ce547-113">Open the solution in Visual Studio.</span></span>

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

<span data-ttu-id="ce547-114">Típus `F5` az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ce547-114">Type `F5` to run the application.</span></span>

<span data-ttu-id="ce547-115">Ez a minta ASP.NET webalkalmazásként való kezelése az alapértelmezett sablon származik, és továbbra is fennáll a felhasználói munkamenetek és a kimeneti gyorsítótár használja.</span><span class="sxs-lookup"><span data-stu-id="ce547-115">This sample ASP.NET web application comes from the default template, and persists user sessions and uses the output cache.</span></span> <span data-ttu-id="ce547-116">Vessen egy pillantást `HighScaleApp\Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="ce547-116">Take a look at `HighScaleApp\Controllers\HomeController.cs`.</span></span> <span data-ttu-id="ce547-117">A `Index()` módszer az adatok a munkamenethez ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="ce547-117">The `Index()` method adds a piece of data to the session.</span></span>

```csharp
Session.Add("visited", "true"); 
```

<span data-ttu-id="ce547-118">És a `About()` és `Contact()` módszerek a kimeneti gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="ce547-118">And the `About()` and `Contact()` methods cache their output.</span></span>

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-to-azure"></a><span data-ttu-id="ce547-119">– 2. lépés: telepítse az Azure</span><span class="sxs-lookup"><span data-stu-id="ce547-119">Step 2 - Deploy to Azure</span></span>
<span data-ttu-id="ce547-120">Ebben a lépésben Azure-webalkalmazás létrehozása és központi telepítése a mintaalkalmazást ASP.NET hozzá.</span><span class="sxs-lookup"><span data-stu-id="ce547-120">In this step, you create an Azure web app and deploy your sample ASP.NET application to it.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="ce547-121">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ce547-121">Create a resource group</span></span>   
<span data-ttu-id="ce547-122">Használjon [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) létrehozásához egy [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) Nyugat-Európában régióban.</span><span class="sxs-lookup"><span data-stu-id="ce547-122">Use [az group create](https://docs.microsoft.com/cli/azure/group#create) to create a [resource group](../azure-resource-manager/resource-group-overview.md) in the West Europe region.</span></span> <span data-ttu-id="ce547-123">Erőforráscsoport az összes Azure-erőforrást, amely a kezelni kívánt együtt, mint például a web app és az SQL-adatbázis háttér ahová.</span><span class="sxs-lookup"><span data-stu-id="ce547-123">A resource group is where you put all the Azure resources that you want to manage together, such as the web app and any SQL Database back end.</span></span>

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

<span data-ttu-id="ce547-124">Milyen lehetséges értékei, megjelenítéséhez használható `---location`, használja a [az App Service lista-helyek](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) parancsot.</span><span class="sxs-lookup"><span data-stu-id="ce547-124">To see what possible values you can use for `---location`, use the [az appservice list-locations](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="ce547-125">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce547-125">Create an App Service plan</span></span>
<span data-ttu-id="ce547-126">Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) létrehozása a "B1" [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ce547-126">Use [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) to create a "B1" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

<span data-ttu-id="ce547-127">Az App Service-csomag a méretezési egység, ami magában foglalhatja tetszőleges számú vertikális vagy horizontális együtt ugyanazon az App Service-infrastruktúrán keresztül kívánt alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ce547-127">An App Service plan is a scale unit, which can include any number of apps that you want to scale up or out together over the same App Service infrastructure.</span></span> <span data-ttu-id="ce547-128">Minden terv is hozzá van rendelve egy [tarifacsomag](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="ce547-128">Each plan is also assigned a [pricing tier](https://azure.microsoft.com/en-us/pricing/details/app-service/).</span></span> <span data-ttu-id="ce547-129">Magasabb szolgáltatásszintek jobb hardver- és további funkciók, például több kibővített példány.</span><span class="sxs-lookup"><span data-stu-id="ce547-129">Higher tiers include better hardware and more features, such as more scale-out instances.</span></span>

<span data-ttu-id="ce547-130">Ebben az oktatóanyagban B1 esetén a minimális rétegben, amely lehetővé teszi a bővített kapacitású három alkalmazáspéldányra.</span><span class="sxs-lookup"><span data-stu-id="ce547-130">For this tutorial, B1 is the minimum tier that enables scale out to three instances.</span></span> <span data-ttu-id="ce547-131">Mindig áthelyezheti az alkalmazás felfelé vagy lefelé a tarifacsomag később futtatásával [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span><span class="sxs-lookup"><span data-stu-id="ce547-131">You can always move your app up or down the pricing tier later by running [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update).</span></span> 

### <a name="create-a-web-app"></a><span data-ttu-id="ce547-132">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce547-132">Create a web app</span></span>
<span data-ttu-id="ce547-133">Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) egy egyedi nevet a webalkalmazás létrehozásához `$appName`.</span><span class="sxs-lookup"><span data-stu-id="ce547-133">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) to create a web app with a unique name in `$appName`.</span></span>

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a><span data-ttu-id="ce547-134">Telepítési hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="ce547-134">Set deployment credentials</span></span>
<span data-ttu-id="ce547-135">Használjon [az App Service web beállítva üzembe helyező felhasználó](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) az App Service a fiók szintű üzembe helyezési hitelesítő adatok beállításához.</span><span class="sxs-lookup"><span data-stu-id="ce547-135">Use [az appservice web deployment user set](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set) to set your account-level deployment credentials for App Service.</span></span>

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a><span data-ttu-id="ce547-136">Git-telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ce547-136">Configure Git deployment</span></span>
<span data-ttu-id="ce547-137">Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) helyi Git-telepítés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ce547-137">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) to configure local Git deployment.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

<span data-ttu-id="ce547-138">Ez a parancs, amely a következőhöz hasonló kimenetnek biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ce547-138">This command gives you an output that looks like the following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="ce547-139">A visszaadott URL-cím segítségével konfigurálhatja a Git távoli.</span><span class="sxs-lookup"><span data-stu-id="ce547-139">Use the returned URL to configure your Git remote.</span></span> <span data-ttu-id="ce547-140">A következő parancs az előző példa konzolkimenetben.</span><span class="sxs-lookup"><span data-stu-id="ce547-140">The following command uses the preceding output example.</span></span>

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-the-sample-application"></a><span data-ttu-id="ce547-141">A mintaalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="ce547-141">Deploy the sample application</span></span>
<span data-ttu-id="ce547-142">Most már készen áll a mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="ce547-142">You are now ready to deploy your sample application.</span></span> <span data-ttu-id="ce547-143">Futtassa az `git push` parancsot.</span><span class="sxs-lookup"><span data-stu-id="ce547-143">Run `git push`.</span></span>

```powershell
git push azure master
```

<span data-ttu-id="ce547-144">Ha a jelszó megadását kéri, használja a jelszót, amely a megadott névvel `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="ce547-144">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-azure-web-app"></a><span data-ttu-id="ce547-145">Keresse meg az Azure web apphoz</span><span class="sxs-lookup"><span data-stu-id="ce547-145">Browse to Azure web app</span></span>
<span data-ttu-id="ce547-146">Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) fut élőben az Azure-ban az alkalmazás megtekintéséhez futtassa a parancsot.</span><span class="sxs-lookup"><span data-stu-id="ce547-146">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to see your app running live in Azure, run this command.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-to-redis"></a><span data-ttu-id="ce547-147">Lépés a 3 - való csatlakozáshoz</span><span class="sxs-lookup"><span data-stu-id="ce547-147">Step 3 - Connect to Redis</span></span>
<span data-ttu-id="ce547-148">Ebben a lépésben beállított Azure Redis Cache egy külső, közös elhelyezésű gyorsítótár az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ce547-148">In this step, you set up Azure Redis Cache as an external, colocated cache to your Azure web app.</span></span> <span data-ttu-id="ce547-149">A lap kimenete gyorsítótárazásához Redis gyorsan használhatja.</span><span class="sxs-lookup"><span data-stu-id="ce547-149">You can quickly utilize Redis to cache your page output.</span></span> <span data-ttu-id="ce547-150">Továbbá, amikor később kiterjesztése a webalkalmazások, Redis segít megőrizni a felhasználói munkamenetek több példánya megbízhatóan.</span><span class="sxs-lookup"><span data-stu-id="ce547-150">In addition, when you scale out your web apps later, Redis helps you persist user sessions across multiple instances reliably.</span></span>

### <a name="create-an-azure-redis-cache"></a><span data-ttu-id="ce547-151">Azure Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce547-151">Create an Azure Redis Cache</span></span>
<span data-ttu-id="ce547-152">Használjon [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) Azure Redis Cache létrehozása és mentése a JSON kimeneti.</span><span class="sxs-lookup"><span data-stu-id="ce547-152">Use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) to create an Azure Redis Cache and save the JSON output.</span></span> <span data-ttu-id="ce547-153">Egyedi nevet a `$cacheName`.</span><span class="sxs-lookup"><span data-stu-id="ce547-153">Use a unique name in `$cacheName`.</span></span>

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-the-application-to-use-redis"></a><span data-ttu-id="ce547-154">Az alkalmazás használata a Redis beállítása</span><span class="sxs-lookup"><span data-stu-id="ce547-154">Configure the application to use Redis</span></span>
<span data-ttu-id="ce547-155">A gyorsítótár a kapcsolati karakterlánc formátuma.</span><span class="sxs-lookup"><span data-stu-id="ce547-155">Format the connection string for your cache.</span></span>

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

<span data-ttu-id="ce547-156">A második sorban adjon meg, a következőhöz hasonló kimenetnek:</span><span class="sxs-lookup"><span data-stu-id="ce547-156">The second line should give you an output that looks like this:</span></span>

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

<span data-ttu-id="ce547-157">A projekt gyökérkönyvtárában nevű webes konfigurációs fájl létrehozása a Visual Studio `redis.config` és az alábbi kód beillesztése.</span><span class="sxs-lookup"><span data-stu-id="ce547-157">In Visual Studio, create a web configuration file in your project root called `redis.config` and paste the following code into it.</span></span> <span data-ttu-id="ce547-158">A `value`, használja a kapcsolati karakterláncot a PowerShell-kimenetből.</span><span class="sxs-lookup"><span data-stu-id="ce547-158">In `value`, use the connection string from the PowerShell output.</span></span>

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

<span data-ttu-id="ce547-159">Ha megnézi a `.gitignore` fájl a Git-tárházban, láthatja, hogy a fájl nem a verziókövetésből.</span><span class="sxs-lookup"><span data-stu-id="ce547-159">If you look at the `.gitignore` file in your Git repository, you'll see that this file is excluded from source control.</span></span> <span data-ttu-id="ce547-160">Ezzel a módszerrel a bizalmas információk biztonságos maradjon.</span><span class="sxs-lookup"><span data-stu-id="ce547-160">That way your sensitive information is kept safe.</span></span> 

<span data-ttu-id="ce547-161">Nyissa meg `Web.config`.</span><span class="sxs-lookup"><span data-stu-id="ce547-161">Open `Web.config`.</span></span> <span data-ttu-id="ce547-162">Figyelje meg a `<appSettings file="redis.config">` elemet, amely lekérdezi a létrehozott beállítást `redis.config`.</span><span class="sxs-lookup"><span data-stu-id="ce547-162">Notice the `<appSettings file="redis.config">` element, which gets the setting you created in `redis.config`.</span></span> 

<span data-ttu-id="ce547-163">Keresse meg a megjegyzésekkel szakaszt, amely tartalmazza az `<sessionState>` és `<caching>`.</span><span class="sxs-lookup"><span data-stu-id="ce547-163">Find the commented section that includes `<sessionState>` and `<caching>`.</span></span> <span data-ttu-id="ce547-164">Állítsa vissza az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ce547-164">Uncomment this section.</span></span>

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

<span data-ttu-id="ce547-165">Ez a kód keresi a Redis kapcsolódási karakterlánc ben megadott `RedisConnection`.</span><span class="sxs-lookup"><span data-stu-id="ce547-165">This code looks for the Redis connection string you defined in `RedisConnection`.</span></span> 

<span data-ttu-id="ce547-166">Az alkalmazás most Redis munkamenetek és a gyorsítótár kezelésére használ.</span><span class="sxs-lookup"><span data-stu-id="ce547-166">Your application now uses Redis to manage sessions and caching.</span></span> <span data-ttu-id="ce547-167">Típus `F5` az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ce547-167">Type `F5` to run the application.</span></span> <span data-ttu-id="ce547-168">Ha kívánja letöltheti a Redis-kezelési ügynök, a gyorsítótár most mentett adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ce547-168">If you like, you can download a Redis management client to visualize the data that is now saved to the cache.</span></span>

### <a name="configure-the-connection-string-in-azure"></a><span data-ttu-id="ce547-169">A kapcsolati karakterlánc konfigurálása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ce547-169">Configure the connection string in Azure</span></span>

<span data-ttu-id="ce547-170">Az alkalmazás működéséhez az Azure-ban az azonos Redis-kapcsolati karakterlánc konfigurálása az Azure web app alkalmazásban kell.</span><span class="sxs-lookup"><span data-stu-id="ce547-170">For your application to work in Azure, you need to configure the same Redis connection string in your Azure web app.</span></span> <span data-ttu-id="ce547-171">Mivel a `redis.config` nem őrzi meg a verziókövetés, még nincs telepítve az Azure Git-telepítés futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="ce547-171">Since `redis.config` is not maintained in source control, it is not deployed to Azure when you run Git deployment.</span></span>

<span data-ttu-id="ce547-172">Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) neve a kapcsolati karakterlánc hozzáadása (`RedisConnection`).</span><span class="sxs-lookup"><span data-stu-id="ce547-172">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add the connection string with the same name (`RedisConnection`).</span></span>

<span data-ttu-id="ce547-173">az App Service web config appsettings frissítése – beállítások "RedisConnection = $connstring"--$appName--myResourceGroup erőforráscsoport név</span><span class="sxs-lookup"><span data-stu-id="ce547-173">az appservice web config appsettings update --settings "RedisConnection=$connstring" --name $appName --resource-group myResourceGroup</span></span>

<span data-ttu-id="ce547-174">Ne feledje, hogy `$connstring` a formázott kapcsolati karakterláncot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ce547-174">Remember that `$connstring` contains the formatted connection string.</span></span>

### <a name="redeploy-the-application-to-azure"></a><span data-ttu-id="ce547-175">Telepítse újra az alkalmazást az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="ce547-175">Redeploy the application to Azure</span></span>
<span data-ttu-id="ce547-176">Továbbítsa a módosításokat az Azure Git-parancsok segítségével</span><span class="sxs-lookup"><span data-stu-id="ce547-176">Use Git commands to push your changes to Azure</span></span>

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

<span data-ttu-id="ce547-177">Ha a jelszó megadását kéri, használja a jelszót, amely a megadott névvel `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="ce547-177">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="ce547-178">Keresse meg az Azure-webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="ce547-178">Browse to the Azure web app</span></span>
<span data-ttu-id="ce547-179">Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) a változtatások élő Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ce547-179">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to see the changes live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-to-multiple-instances"></a><span data-ttu-id="ce547-180">4. lépés: több példány méret</span><span class="sxs-lookup"><span data-stu-id="ce547-180">Step 4 - Scale to multiple instances</span></span>
<span data-ttu-id="ce547-181">Az App Service-csomag a skálázási egység az Azure web Apps.</span><span class="sxs-lookup"><span data-stu-id="ce547-181">The App Service plan is the scale unit for your Azure web apps.</span></span> <span data-ttu-id="ce547-182">A webes alkalmazás horizontális, az App Service-csomag vertikális.</span><span class="sxs-lookup"><span data-stu-id="ce547-182">To scale out your web app, you scale the App Service plan.</span></span>

<span data-ttu-id="ce547-183">Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) B1 tarifacsomag engedélyezett kiterjesztése az App Service-csomag három alkalmazáspéldányra, amely a maximális szám.</span><span class="sxs-lookup"><span data-stu-id="ce547-183">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) to scale out the App Service plan to three instances, which is the maximum number allowed by the B1 pricing tier.</span></span> <span data-ttu-id="ce547-184">Ne feledje, hogy B1 az árképzési szint, amikor az App Service-csomag korábban létrehozott választott-e.</span><span class="sxs-lookup"><span data-stu-id="ce547-184">Remember that B1 is the pricing tier that you chose when you created the App Service plan earlier.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a><span data-ttu-id="ce547-185">5 - skálázási földrajzilag lépés</span><span class="sxs-lookup"><span data-stu-id="ce547-185">Step 5 - Scale geographically</span></span>
<span data-ttu-id="ce547-186">Skálázás földrajzilag, amikor az alkalmazás futtatása az Azure-felhő több régióba.</span><span class="sxs-lookup"><span data-stu-id="ce547-186">When scaling geographically, you run your app in multiple regions of the Azure cloud.</span></span> <span data-ttu-id="ce547-187">A telepítő kiegyenlíti az alkalmazás további földrajzi hely alapján, és csökkenti a válaszidő úgy, hogy az alkalmazás közelebb ügyfélböngészők.</span><span class="sxs-lookup"><span data-stu-id="ce547-187">This setup load-balances your app further based on geography and lowers the response time by placing your app closer to client browsers.</span></span>

<span data-ttu-id="ce547-188">Ebben a lépésben egy második terület az ASP.NET webalkalmazás skálázása [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span><span class="sxs-lookup"><span data-stu-id="ce547-188">In this step, you scale your ASP.NET web app to a second region with [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/).</span></span> <span data-ttu-id="ce547-189">A lépés végén kell egy webalkalmazást, Nyugat-Európában (már létrehozott) futó és egy webalkalmazást, Délkelet-Ázsia (még nem hozott létre) futtatja.</span><span class="sxs-lookup"><span data-stu-id="ce547-189">At the end of the step, you will have a web app running in West Europe (already created) and a web app running in Southeast Asia (not yet created).</span></span> <span data-ttu-id="ce547-190">Mindkét alkalmazásban a Traffic Manager URL-CÍMÉRE a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="ce547-190">Both apps will be served from the same Traffic Manager URL.</span></span>

### <a name="scale-up-the-europe-app-to-standard-tier"></a><span data-ttu-id="ce547-191">A Standard csomag Európa alkalmazás méretezése</span><span class="sxs-lookup"><span data-stu-id="ce547-191">Scale up the Europe app to Standard tier</span></span>
<span data-ttu-id="ce547-192">Az App Service-ben integráció az Azure Traffic Manager csak a Standard tarifacsomagban használható.</span><span class="sxs-lookup"><span data-stu-id="ce547-192">In App Service, integration with Azure Traffic Manager requires the Standard pricing tier.</span></span> <span data-ttu-id="ce547-193">Használjon [az App Service-csomag frissítése](https://docs.microsoft.com/cli/azure/appservice/plan#update) méretezést kívánó S1 az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="ce547-193">Use [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) to scale up your App Service plan to S1.</span></span> 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="ce547-194">Traffic Manager-profil létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce547-194">Create a Traffic Manager profile</span></span> 
<span data-ttu-id="ce547-195">Használjon [az hálózati forgalmat-manager-profil létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) Traffic Manager-profil létrehozásához, és adja hozzá az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="ce547-195">Use [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) to create a Traffic Manager profile and add it to your resource group.</span></span> <span data-ttu-id="ce547-196">Egy egyedi DNS-név $dnsName használja.</span><span class="sxs-lookup"><span data-stu-id="ce547-196">Use a unique DNS name in $dnsName.</span></span>

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> <span data-ttu-id="ce547-197">`--routing-method Performance`Megadja, hogy ez a profil [továbbítja a felhasználói forgalomnak a legközelebbi végpont](../traffic-manager/traffic-manager-routing-methods.md).</span><span class="sxs-lookup"><span data-stu-id="ce547-197">`--routing-method Performance` specifies that this profile [routes user traffic to the closest endpoint](../traffic-manager/traffic-manager-routing-methods.md).</span></span>

### <a name="get-the-resource-id-of-the-europe-app"></a><span data-ttu-id="ce547-198">Az erőforrás-azonosítója a Európa alkalmazás beszerzése</span><span class="sxs-lookup"><span data-stu-id="ce547-198">Get the resource ID of the Europe app</span></span>
<span data-ttu-id="ce547-199">Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) az erőforrás-azonosítója a webes alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ce547-199">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) to get the resource ID of your web app.</span></span>

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-europe-app"></a><span data-ttu-id="ce547-200">A Traffic Manager-végpont a Európa alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce547-200">Add a Traffic Manager endpoint for the Europe app</span></span>
<span data-ttu-id="ce547-201">Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) végpont felvétele a Traffic Manager-profil és a webes alkalmazás erőforrás-azonosítója a célként használni.</span><span class="sxs-lookup"><span data-stu-id="ce547-201">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) to add an endpoint to your Traffic Manager profile and use the resource ID of your web app as the target.</span></span>

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-the-traffic-manager-endpoint-url"></a><span data-ttu-id="ce547-202">A Traffic Manager-végpont URL-cím lekérése</span><span class="sxs-lookup"><span data-stu-id="ce547-202">Get the Traffic Manager endpoint URL</span></span>
<span data-ttu-id="ce547-203">A Traffic Manager-profil most már olyan végponttal, amely a már meglévő webalkalmazás mutat.</span><span class="sxs-lookup"><span data-stu-id="ce547-203">Your Traffic Manager profile now has an endpoint that points to your existing web app.</span></span> <span data-ttu-id="ce547-204">Használjon [az hálózati forgalmat-kezelő profil megjelenítése](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) lekérni az URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="ce547-204">Use [az network traffic-manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) to get its URL.</span></span> 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

<span data-ttu-id="ce547-205">Másolja át a kimenetet a böngészőbe.</span><span class="sxs-lookup"><span data-stu-id="ce547-205">Copy the output into your browser.</span></span> <span data-ttu-id="ce547-206">A webalkalmazás ismét meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="ce547-206">You should see your web app again.</span></span>

### <a name="create-an-azure-redis-cache-in-asia"></a><span data-ttu-id="ce547-207">Hozzon létre egy Azure Redis Cache Ázsiában</span><span class="sxs-lookup"><span data-stu-id="ce547-207">Create an Azure Redis Cache in Asia</span></span>
<span data-ttu-id="ce547-208">Az Azure-webalkalmazásban, Délkelet-Ázsia régió replikálni.</span><span class="sxs-lookup"><span data-stu-id="ce547-208">Now, you replicate your Azure web app to the Southeast Asia region.</span></span> <span data-ttu-id="ce547-209">Indításához használja [az redis létrehozása](https://docs.microsoft.com/en-us/cli/azure/redis#create) egy második Azure Redis Cache létrehozása a Délkelet-Ázsia.</span><span class="sxs-lookup"><span data-stu-id="ce547-209">To start, use [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) to create a second Azure Redis Cache in Southeast Asia.</span></span> <span data-ttu-id="ce547-210">Ez a gyorsítótár kell lennie a közös elhelyezésű Ázsiában alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="ce547-210">This cache needs to be colocated with your app in Asia.</span></span>

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

<span data-ttu-id="ce547-211">`--name $cacheName-asia`Nyugat-Európában gyorsítótár nevét a gyorsítótár biztosít a a `-asia` utótag.</span><span class="sxs-lookup"><span data-stu-id="ce547-211">`--name $cacheName-asia` gives the cache the name of the West Europe cache, with the `-asia` suffix.</span></span>

### <a name="create-an-app-service-plan-in-asia"></a><span data-ttu-id="ce547-212">Az App Service-csomag létrehozása Ázsiában</span><span class="sxs-lookup"><span data-stu-id="ce547-212">Create an App Service plan in Asia</span></span>
<span data-ttu-id="ce547-213">Használjon [az App Service-csomagot hozzon létre](https://docs.microsoft.com/cli/azure/appservice/plan#create) egy második App Service-csomagot a régióban Délkelet-Ázsiában, az azonos S1 tartozó használja, mint a Nyugat-Európában terv létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ce547-213">Use [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) to create a second App Service plan in the Southeast Asia region, using the same S1 tier as the West Europe plan.</span></span>

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a><span data-ttu-id="ce547-214">A webalkalmazás létrehozása az Ázsia</span><span class="sxs-lookup"><span data-stu-id="ce547-214">Create a web app in Asia</span></span>
<span data-ttu-id="ce547-215">Használjon [az App Service webalkalmazás létrehozása](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) hozhat létre egy második webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ce547-215">Use [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) to create a second web app.</span></span>

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

<span data-ttu-id="ce547-216">`--name $appName-asia`Nyugat-Európában alkalmazás nevét az alkalmazás biztosítja a a `-asia` utótag.</span><span class="sxs-lookup"><span data-stu-id="ce547-216">`--name $appName-asia` gives the app the name of the West Europe app, with the `-asia` suffix.</span></span>

### <a name="configure-the-connection-string-for-redis"></a><span data-ttu-id="ce547-217">A Redis a kapcsolati karakterlánc konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ce547-217">Configure the connection string for Redis</span></span>
<span data-ttu-id="ce547-218">Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) hozzáadása a webes alkalmazás a kapcsolati karakterláncot a Délkelet-Ázsia gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="ce547-218">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add to the web app the connection string for the Southeast Asia cache.</span></span>

<span data-ttu-id="ce547-219">az App Service web config appsettings frissítése – beállítások "RedisConnection$ ($redis.hostname) =: $($redis.sslPort), a jelszó$ ($redis.accessKeys.primaryKey) ssl = = True, abortConnect = False"--name $appName-Ázsia--erőforráscsoport myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ce547-219">az appservice web config appsettings update --settings "RedisConnection=$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False" --name $appName-asia --resource-group myResourceGroup</span></span>

### <a name="configure-git-deployment-for-the-asia-app"></a><span data-ttu-id="ce547-220">Az Ázsia alkalmazás Git-KözpontiTelepítés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ce547-220">Configure Git deployment for the Asia app.</span></span>
<span data-ttu-id="ce547-221">Használjon [az App Service web verziókezelő config-helyi-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) helyi Git-telepítésének a második webalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ce547-221">Use [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) to configure local Git deployment for the second web app.</span></span>

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="ce547-222">Ez a parancs, amely a következőhöz hasonló kimenetnek biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ce547-222">This command gives you an output that looks like the following:</span></span>

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

<span data-ttu-id="ce547-223">A visszaadott URL-cím segítségével konfigurálhatja a helyi tárház távoli Git-egy második.</span><span class="sxs-lookup"><span data-stu-id="ce547-223">Use the returned URL to configure a second Git remote for your local repository.</span></span> <span data-ttu-id="ce547-224">A következő parancs az előző példa konzolkimenetben.</span><span class="sxs-lookup"><span data-stu-id="ce547-224">The following command uses the preceding output example.</span></span>

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a><span data-ttu-id="ce547-225">A mintaalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="ce547-225">Deploy your sample application</span></span>
<span data-ttu-id="ce547-226">Futtatás `git push` a mintaalkalmazást a második távoli Git való telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ce547-226">Run `git push` to deploy your sample application to the second Git remote.</span></span> 

```bash
git push azure-asia master
```

<span data-ttu-id="ce547-227">Ha a jelszó megadását kéri, használja a jelszót, amely a megadott névvel `az appservice web deployment user set`.</span><span class="sxs-lookup"><span data-stu-id="ce547-227">When prompted for password, use the password that you specified when you ran `az appservice web deployment user set`.</span></span>

### <a name="browse-to-the-asia-app"></a><span data-ttu-id="ce547-228">Keresse meg az Ázsia alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ce547-228">Browse to the Asia app</span></span>
<span data-ttu-id="ce547-229">Használjon [az App Service web Tallózás](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) ellenőrizze, hogy az alkalmazás az Azure-ban élő fut-e.</span><span class="sxs-lookup"><span data-stu-id="ce547-229">Use [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) to verify that your app is running live in Azure.</span></span>

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-the-resource-id-of-the-asia-app"></a><span data-ttu-id="ce547-230">Az erőforrás-azonosítója az Ázsia alkalmazás beszerzése</span><span class="sxs-lookup"><span data-stu-id="ce547-230">Get the resource ID of the Asia app</span></span>
<span data-ttu-id="ce547-231">Használjon [az App Service web megjelenítése](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) beolvasása a erőforrás-azonosítója a webalkalmazás, Délkelet-Ázsia.</span><span class="sxs-lookup"><span data-stu-id="ce547-231">Use [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) to get the resource ID of your web app in Southeast Asia.</span></span>

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-asia-app"></a><span data-ttu-id="ce547-232">A Traffic Manager-végpontot az Ázsia alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce547-232">Add a Traffic Manager endpoint for the Asia app</span></span>
<span data-ttu-id="ce547-233">Használjon [az hálózati forgalmat-manager-végpont létrehozása](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) egy másik végpont hozzáadása a Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="ce547-233">Use [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) to add a second endpoint to the Traffic Manager profile.</span></span>

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-to-web-apps"></a><span data-ttu-id="ce547-234">A webalkalmazások régióazonosító hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce547-234">Add region identifier to web apps</span></span>
<span data-ttu-id="ce547-235">Használjon [az App Service web config appsettings frissítése](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) hozzáadása egy régióspecifikus környezeti változót.</span><span class="sxs-lookup"><span data-stu-id="ce547-235">Use [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) to add a region-specific environment variable.</span></span>

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

<span data-ttu-id="ce547-236">Az alkalmazás kódjában már használja az alkalmazás-beállítás.</span><span class="sxs-lookup"><span data-stu-id="ce547-236">Your application code already uses this application setting.</span></span> <span data-ttu-id="ce547-237">Vessen egy pillantást `HighScaleApp\Views\Home\Index.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="ce547-237">Take a look at `HighScaleApp\Views\Home\Index.cshtml`.</span></span>

### <a name="complete"></a><span data-ttu-id="ce547-238">Fejezze be!</span><span class="sxs-lookup"><span data-stu-id="ce547-238">Complete!</span></span>

<span data-ttu-id="ce547-239">Most próbáljon meg a Traffic Manager-profil URL-CÍMÉT a különböző földrajzi régióban böngészőkkel történő eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ce547-239">Now, try to access the URL of your Traffic Manager profile from browsers in different geographical regions.</span></span> <span data-ttu-id="ce547-240">Ügyfélböngészők Európából megjelennek az "ASP.NET Nyugat-Európa", és az Ázsia ügyfélböngészőnek megjelennek az "ASP.NET Délkelet-Ázsia."</span><span class="sxs-lookup"><span data-stu-id="ce547-240">Client browsers from Europe should show "ASP.NET West Europe", and client browser from Asia should show "ASP.NET Southeast Asia."</span></span>

## <a name="more-resources"></a><span data-ttu-id="ce547-241">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="ce547-241">More resources</span></span>
