---
title: "CORS támogatás az App Service-ben | Microsoft Docs"
description: "Megtudhatja, hogyan használhatja a CORS-támogatást az Azure App Service platformon."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: alkarche
ms.openlocfilehash: f8373cf5b2e06e6c71bce51cd9e9d5123eea7cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="consume-an-api-app-from-javascript-using-cors"></a><span data-ttu-id="49a28-103">API-alkalmazások felhasználása JavaScriptből a CORS használatával</span><span class="sxs-lookup"><span data-stu-id="49a28-103">Consume an API app from JavaScript using CORS</span></span>
<span data-ttu-id="49a28-104">Az App Service beépített támogatást nyújt a [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) szolgáltatáshoz, amely lehetővé teszi, hogy a JavaScript-ügyfelek tartományok között hívjanak meg az API-alkalmazások által nyújtott API-kat.</span><span class="sxs-lookup"><span data-stu-id="49a28-104">App Service offers built-in support for [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), which enables JavaScript clients to make cross-domain calls to APIs that are hosted in API apps.</span></span> <span data-ttu-id="49a28-105">Az App Service segítségével úgy állíthatja be a saját API-ja CORS-elérését, hogy nem kell az API-ba kódot írnia.</span><span class="sxs-lookup"><span data-stu-id="49a28-105">App Service lets you configure CORS access to your API without writing any code in your API.</span></span>

<span data-ttu-id="49a28-106">Ez a cikk két részből áll:</span><span class="sxs-lookup"><span data-stu-id="49a28-106">This article contains two sections:</span></span>

* <span data-ttu-id="49a28-107">[A CORS konfigurálásának módja](#corsconfig) című rész általánosságban ismerteti, hogyan kell a CORS szolgáltatást konfigurálni tetszőleges API-alkalmazáshoz, webalkalmazáshoz vagy mobilalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="49a28-107">The [How to configure CORS](#corsconfig) section explains in general how to configure CORS for any API app, web app, or mobile app.</span></span> <span data-ttu-id="49a28-108">Ez a rész minden, az App Service által támogatott keretrendszerre alkalmazható, beleértve a .NET, a Node.js és a Java keretrendszert.</span><span class="sxs-lookup"><span data-stu-id="49a28-108">It applies equally to all frameworks that are supported by App Service, including .NET, Node.js, and Java.</span></span> 
* <span data-ttu-id="49a28-109">A [.NET-bevezető oktatóanyagok folytatása](#tutorialstart) résztől kezdve a cikk oktató funkciót tölt be, és [az első API-alkalmazásokba való bevezető oktatóanyag](app-service-api-dotnet-get-started.md) tartalmára építve mutatja be a CORS-támogatást.</span><span class="sxs-lookup"><span data-stu-id="49a28-109">Starting with the [Continuing the .NET getting-started tutorials](#tutorialstart) section, the article is a tutorial that demonstrates CORS support by building on what you did in [the first API Apps getting started tutorial](app-service-api-dotnet-get-started.md).</span></span> 

## <span data-ttu-id="49a28-110"><a id="corsconfig"></a> A CORS konfigurálása az Azure App Service platformon</span><span class="sxs-lookup"><span data-stu-id="49a28-110"><a id="corsconfig"></a> How to configure CORS in Azure App Service</span></span>
<span data-ttu-id="49a28-111">A CORS szolgáltatást konfigurálhatja az Azure Portalon vagy az [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) eszközeinek használatával.</span><span class="sxs-lookup"><span data-stu-id="49a28-111">You can configure CORS in the Azure portal or by using [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tools.</span></span>

#### <a name="configure-cors-in-the-azure-portal"></a><span data-ttu-id="49a28-112">A CORS konfigurálása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="49a28-112">Configure CORS in the Azure portal</span></span>
1. <span data-ttu-id="49a28-113">Nyissa meg böngészőben az [Azure Portalt](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="49a28-113">In a browser go to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="49a28-114">Kattintson az **App Services** lehetőségre, majd kattintson az API-alkalmazás nevére.</span><span class="sxs-lookup"><span data-stu-id="49a28-114">Click **App Services**, and then click the name of your API app.</span></span>
   
    ![API-alkalmazás kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="49a28-116">A **Settings** (Beállítások) panelen (az **API app** (API-alkalmazás) paneltől jobbra) keresse meg az **API** szakaszt, majd kattintson a **CORS** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="49a28-116">In the **Settings** blade that opens to the right of the **API app** blade, find the **API** section, and then click **CORS**.</span></span>
   
   ![Válassza a CORS lehetőséget a Beállítások panelen](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="49a28-118">A szövegmezőbe írja be azokat az URL-címeket, amelyekről engedélyezni szeretné a JavaScript-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="49a28-118">In the text box enter the URL or URLs that you want to allow JavaScript calls to come from.</span></span>

    <span data-ttu-id="49a28-119">Ha például a JavaScript-alkalmazását egy todolistangular nevű webalkalmazásra telepítette, írja be a "https://todolistangular.azurewebsites.net" címet.</span><span class="sxs-lookup"><span data-stu-id="49a28-119">For example, if you deployed your JavaScript application to a web app named todolistangular, enter "https://todolistangular.azurewebsites.net".</span></span> <span data-ttu-id="49a28-120">Ha csillagot (*) ír be, azzal beállíthatja, hogy minden eredettartományból elfogadja a hívásokat.</span><span class="sxs-lookup"><span data-stu-id="49a28-120">As an alternative, you can enter an asterisk (*) to specify that all origin domains are accepted.</span></span>


1. <span data-ttu-id="49a28-121">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="49a28-121">Click **Save**.</span></span>
   
   ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="49a28-123">A **Save** (Mentés) gombra való kattintás után az API-alkalmazás fogadni fogja a megadott URL-címekről jövő hívásokat.</span><span class="sxs-lookup"><span data-stu-id="49a28-123">After you click **Save**, the API app will accept JavaScript calls from the specified URLs.</span></span>

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a><span data-ttu-id="49a28-124">A CORS konfigurálása az Azure Resource Manager eszközeinek használatával</span><span class="sxs-lookup"><span data-stu-id="49a28-124">Configure CORS by using Azure Resource Manager tools</span></span>
<span data-ttu-id="49a28-125">A CORS szolgáltatást úgy is beállíthatja egy API-alkalmazáshoz, hogy [Azure Resource Manager-sablonokat](../azure-resource-manager/resource-group-authoring-templates.md) használ a parancssori eszközökben, például az  [Azure PowerShell](/powershell/azureps-cmdlets-docs) vagy az [Azure CLI](../cli-install-nodejs.md) felületen.</span><span class="sxs-lookup"><span data-stu-id="49a28-125">You can also configure CORS for an API app by using [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) in command line tools such as [Azure PowerShell](/powershell/azureps-cmdlets-docs) and the [Azure CLI](../cli-install-nodejs.md).</span></span> 

<span data-ttu-id="49a28-126">Ha szeretne példát látni egy olyan Azure Resource Manager-sablonra, amely beállítja a CORS tulajdonságot, nyissa meg az oktatóanyag példaalkalmazását, amely a tárházban az [azuredeploy.json fájl](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="49a28-126">For an example of an Azure Resource Manager template that sets the CORS property, open the [azuredeploy.json file in the repository for this tutorial's sample application](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span></span> <span data-ttu-id="49a28-127">Keresse meg a sablonban az a részt, amely az alábbi példára hasonlít:</span><span class="sxs-lookup"><span data-stu-id="49a28-127">Find the section of the template that looks like the following example:</span></span>

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <span data-ttu-id="49a28-128"><a id="tutorialstart"></a> A .NET-bevezető oktatóanyag folytatása</span><span class="sxs-lookup"><span data-stu-id="49a28-128"><a id="tutorialstart"></a> Continuing the .NET getting-started tutorial</span></span>
<span data-ttu-id="49a28-129">Ha az API-alkalmazásokhoz készült Node.js vagy Java-bevezető sorozatot követi, akkor elvégezte a bevezetősorozatot.</span><span class="sxs-lookup"><span data-stu-id="49a28-129">If you are following the Node.js or Java getting-started series for API apps, you have completed the getting started series.</span></span> <span data-ttu-id="49a28-130">Ugorjon a [További lépések](#next-steps) című részre, ahol tanácsokat találhat az API-alkalmazások bővebb megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="49a28-130">Skip to the [Next steps](#next-steps) section to find suggestions for further learning about API Apps.</span></span>

<span data-ttu-id="49a28-131">A cikk hátralévő része a .NET-bevezető sorozat folytatása, és feltételezi, hogy Ön sikeresen elvégezte [az első oktatóanyagot](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="49a28-131">The remainder of this article is a continuation of the .NET getting-started series and assumes that you successfully completed [the first tutorial](app-service-api-dotnet-get-started.md).</span></span>

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a><span data-ttu-id="49a28-132">A ToDoListAngular projekt telepítése új webalkalmazásra</span><span class="sxs-lookup"><span data-stu-id="49a28-132">Deploy the ToDoListAngular project to a new web app</span></span>
<span data-ttu-id="49a28-133">[Az első oktatóprogramban](app-service-api-dotnet-get-started.md) létrehozott egy középső rétegbeli API-alkalmazást és egy adatrétegbeli API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49a28-133">In [the first tutorial](app-service-api-dotnet-get-started.md), you created a middle tier API app and a data tier API app.</span></span> <span data-ttu-id="49a28-134">Ebben az oktatóanyagban egy egyoldalas webalkalmazást (SPA) fogunk létrehozni, amely a középső rétegbeli API-alkalmazást hívja meg.</span><span class="sxs-lookup"><span data-stu-id="49a28-134">In this tutorial you create a single-page application (SPA) web app that calls the middle tier API app.</span></span> <span data-ttu-id="49a28-135">Az SPA működéséhez engedélyeznie kell a CORS szolgáltatást a középső rétegbeli API-alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="49a28-135">For the SPA to work you have to enable CORS on the middle tier API app.</span></span> 

<span data-ttu-id="49a28-136">A [ToDoList példaalkalmazásban](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) a ToDoListAngular projekt egy olyan, az AngularJS ügyfelet bemutató példaprogram, amely a középső rétegbeli ToDoListAPI webes API projektet hívja meg.</span><span class="sxs-lookup"><span data-stu-id="49a28-136">In the [ToDoList sample application](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), the ToDoListAngular project is a simple AngularJS client that calls the middle tier ToDoListAPI Web API project.</span></span> <span data-ttu-id="49a28-137">Az *app/scripts/todoListSvc.js* fájlban lévő JavaScript-kód az AngularJS HTTP-szolgáltató használatával hívja meg az API-t.</span><span class="sxs-lookup"><span data-stu-id="49a28-137">The JavaScript code in the *app/scripts/todoListSvc.js* file calls the API by using the AngularJS HTTP provider.</span></span> 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a><span data-ttu-id="49a28-138">Új webalkalmazás létrehozása a ToDoListAngular projekthez</span><span class="sxs-lookup"><span data-stu-id="49a28-138">Create a new web app for the ToDoListAngular project</span></span>
<span data-ttu-id="49a28-139">Az új App Service-webalkalmazások létrehozása és a hozzájuk tartozó projekt telepítése hasonló módon történik, mint az [API-alkalmazások létrehozása és telepítése, amit a sorozat első oktatóanyaga ismertetett](app-service-api-dotnet-get-started.md#createapiapp).</span><span class="sxs-lookup"><span data-stu-id="49a28-139">The procedure to create a new App Service web app and deploy a project to it is similar to what you saw for [creating and deploying an API app in the first tutorial in this series](app-service-api-dotnet-get-started.md#createapiapp).</span></span> <span data-ttu-id="49a28-140">Az egyetlen különbség, hogy az alkalmazás típusa **webalkalmazás**, nem pedig **API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49a28-140">The only difference is that the app type is **Web App** instead of **API App**.</span></span>  <span data-ttu-id="49a28-141">A párbeszédpanelek képernyőképeit az alábbi módon érheti el:</span><span class="sxs-lookup"><span data-stu-id="49a28-141">For screen shots of the dialogs, see</span></span> 

1. <span data-ttu-id="49a28-142">A **Solution Explorer** (Megoldáskezelő) területén kattintson a jobb gombbal a ToDoListAngular projektre, majd kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="49a28-142">In **Solution Explorer**, right-click the ToDoListAngular project, and then click **Publish**.</span></span>
2. <span data-ttu-id="49a28-143">A **Publish Web** (Weboldal közzététele) varázsló **Profile** (Profile) lapján kattintson a **Microsoft Azure App Service** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="49a28-143">In the **Profile** tab of the **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="49a28-144">Az **App Service** párbeszédpanelen kattintson a **New** (Új) gombra.</span><span class="sxs-lookup"><span data-stu-id="49a28-144">In the **App Service** dialog box, click **New**.</span></span>
4. <span data-ttu-id="49a28-145">A **Create App Service** (App Service létrehozása) párbeszédpanel **Hosting** (Üzemeltetés) lapján írjon be egy olyan nevet a **Web App Name** (Webalkalmazás neve) mezőbe, amely egyedi az *azurewebsites.net* tartományban.</span><span class="sxs-lookup"><span data-stu-id="49a28-145">In the **Hosting** tab of the **Create App Service** dialog box, enter a **Web App Name** that is unique in the *azurewebsites.net* domain.</span></span> 
5. <span data-ttu-id="49a28-146">Válassza ki a használni kívánt Azure **előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="49a28-146">Choose the Azure **Subscription** you want to work with.</span></span>
6. <span data-ttu-id="49a28-147">A **Resource Group** (Erőforráscsoport) legördülő listában válassza a korábban létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="49a28-147">In the **Resource Group** drop-down list, choose the same resource group you created earlier.</span></span>
7. <span data-ttu-id="49a28-148">Az **App Service Plan** (App Service-csomag) legördülő listában válassza a korábban létrehozott csomagot.</span><span class="sxs-lookup"><span data-stu-id="49a28-148">In the **App Service Plan** drop-down list, choose the same plan you created earlier.</span></span> 
8. <span data-ttu-id="49a28-149">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="49a28-149">Click **Create**.</span></span>
   
    <span data-ttu-id="49a28-150">A Visual Studio létrehozza a webalkalmazást és a hozzá tartozó közzétételi profilt, majd megjeleníti a **Publish Web** (Weboldal közzététele) varázsló **Connection** (Kapcsolat) lépését.</span><span class="sxs-lookup"><span data-stu-id="49a28-150">Visual Studio creates the web app, creates a publish profile for it, and displays the **Connection** step of the **Publish Web** wizard.</span></span>
   
    <span data-ttu-id="49a28-151">Még ne kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="49a28-151">Don't click **Publish** yet.</span></span> <span data-ttu-id="49a28-152">A következő szakaszban beállíthatja, hogy a webalkalmazás az App Service platformon futó, középső rétegbeli API-alkalmazást hívja.</span><span class="sxs-lookup"><span data-stu-id="49a28-152">In the following section, you configure the new web app to call the middle tier API app that is running in App Service.</span></span> 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a><span data-ttu-id="49a28-153">A középső réteg URL-címének beállítása a webalkalmazás beállításaiban</span><span class="sxs-lookup"><span data-stu-id="49a28-153">Set the middle tier URL in web app settings</span></span>
1. <span data-ttu-id="49a28-154">Nyissa meg az [Azure Portalt](https://portal.azure.com/), majd keresse meg a ToDoListAngular projekt (kezelőfelület) üzemeltetésére létrehozott webalkalmazáshoz tartozó **Web App** (Webalkalmazás) panelt.</span><span class="sxs-lookup"><span data-stu-id="49a28-154">Go to the [Azure portal](https://portal.azure.com/), and then navigate to the **Web App** blade for the web app that you created to host the TodoListAngular (front end) project.</span></span>
2. <span data-ttu-id="49a28-155">Kattintson a **Settings > Application Settings** (Beállítások > Alkalmazásbeállítások) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="49a28-155">Click **Settings > Application Settings**.</span></span>
3. <span data-ttu-id="49a28-156">Az **App settings** (Alkalmazás beállításai) szakaszban adja meg a következő kulcs-érték párt:</span><span class="sxs-lookup"><span data-stu-id="49a28-156">In the **App settings** section, add the following key and value:</span></span>
   
   | <span data-ttu-id="49a28-157">Kulcs</span><span class="sxs-lookup"><span data-stu-id="49a28-157">Key</span></span> | <span data-ttu-id="49a28-158">Érték</span><span class="sxs-lookup"><span data-stu-id="49a28-158">Value</span></span> | <span data-ttu-id="49a28-159">Példa</span><span class="sxs-lookup"><span data-stu-id="49a28-159">Example</span></span> |
   | --- | --- | --- |
   | <span data-ttu-id="49a28-160">toDoListAPIURL</span><span class="sxs-lookup"><span data-stu-id="49a28-160">toDoListAPIURL</span></span> |<span data-ttu-id="49a28-161">https://{a középső réteg API-alkalmazásának neve}.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="49a28-161">https://{your middle tier API app name}.azurewebsites.net</span></span> |<span data-ttu-id="49a28-162">https://todolistapi0121.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="49a28-162">https://todolistapi0121.azurewebsites.net</span></span> |
4. <span data-ttu-id="49a28-163">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="49a28-163">Click **Save**.</span></span>
   
    <span data-ttu-id="49a28-164">Amikor a kód lefut az Azure-ban, a rendszer ezzel az értékkel írja felül a *Web.config* fájlban található localhost URL-címet.</span><span class="sxs-lookup"><span data-stu-id="49a28-164">When the code runs in Azure, this value overrides the localhost URL that is in the *Web.config* file.</span></span> 
   
    <span data-ttu-id="49a28-165">A beállítás értékét lekérdező kód az *index.cshtml* fájlban található:</span><span class="sxs-lookup"><span data-stu-id="49a28-165">The code that gets the setting value is in *index.cshtml*:</span></span>
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    <span data-ttu-id="49a28-166">A *todoListSvc.js* fájlban lévő kód a következő beállítást használja:</span><span class="sxs-lookup"><span data-stu-id="49a28-166">The code in *todoListSvc.js* uses the setting:</span></span>
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a><span data-ttu-id="49a28-167">A ToDoListAngular webes projekt telepítése az új webalkalmazásra</span><span class="sxs-lookup"><span data-stu-id="49a28-167">Deploy the ToDoListAngular web project to the new web app</span></span>
* <span data-ttu-id="49a28-168">A Visual Studio **Publish Web** (Webes közzététel) varázslójának **Connection** (Kapcsolat) lépésénél kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="49a28-168">In Visual Studio, in the **Connection** step of the **Publish Web** wizard, click **Publish**.</span></span>
  
   <span data-ttu-id="49a28-169">A Visual Studio telepíti a ToDoListAPI projektet az új webalkalmazásba, és egy böngészőablakban megnyitja a webalkalmazás URL-címét.</span><span class="sxs-lookup"><span data-stu-id="49a28-169">Visual Studio deploys the ToDoListAngular project to the new web app and opens a browser to the URL of the web app.</span></span> 

### <a name="test-the-application-without-cors-enabled"></a><span data-ttu-id="49a28-170">Az alkalmazás tesztelése a CORS engedélyezése nélkül</span><span class="sxs-lookup"><span data-stu-id="49a28-170">Test the application without CORS enabled</span></span>
1. <span data-ttu-id="49a28-171">A böngésző Fejlesztői eszközök funkciójával nyissa meg a Konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="49a28-171">In your browser Developer Tools, open the Console window.</span></span>
2. <span data-ttu-id="49a28-172">Az AngularJS felhasználói felületet megjelenítő böngészőablakban kattintson a **To Do List** (Tennivalók) hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="49a28-172">In the browser window that displays the AngularJS UI, click the **To Do List** link.</span></span>
   
    <span data-ttu-id="49a28-173">A JavaScript-kód megpróbálja meghívni a középső rétegbeli API-alkalmazást, de a hívás sikertelen lesz, mivel a kezelőfelület egy másik tartományban fut, mint a háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="49a28-173">The JavaScript code tries to call the middle tier API app, but the call fails because the front end is running in a different domain than the back end.</span></span> <span data-ttu-id="49a28-174">A böngésző Fejlesztői eszközök funkciójával elérhető konzolablak hibaüzenetet jelenít meg az eltérő eredetről.</span><span class="sxs-lookup"><span data-stu-id="49a28-174">The browser's Developer Tools Console window shows a cross-origin error message.</span></span>
   
    ![Hibaüzenet az eltérő eredetről](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a><span data-ttu-id="49a28-176">A CORS konfigurálása középső rétegbeli API-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="49a28-176">Configure CORS for the middle tier API app</span></span>
<span data-ttu-id="49a28-177">Ebben a szakaszban a középső rétegbeli ToDoListAPI API-alkalmazás CORS beállítását konfiguráljuk az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="49a28-177">In this section, you configure the CORS setting in Azure for the middle tier ToDoListAPI API app.</span></span> <span data-ttu-id="49a28-178">Ez a beállítás lehetővé teszi, hogy a középső rétegbeli API-alkalmazás JavaScript-hívásokat fogadjon abból a webalkalmazásból, amelyet a ToDoListAngular projekthez létrehozott.</span><span class="sxs-lookup"><span data-stu-id="49a28-178">This setting will allow the middle tier API app to receive JavaScript calls from the web app that you created for the ToDoListAngular project.</span></span>

1. <span data-ttu-id="49a28-179">Nyissa meg böngészőben az [Azure Portalt](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="49a28-179">In a browser, go to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="49a28-180">Kattintson az **App Services** (Alkalmazásszolgáltatások) lehetőségre, majd a ToDoListAPI (középső réteg) API-alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="49a28-180">Click **App Services**, and then click the ToDoListAPI (middle tier) API app.</span></span>
   
    ![API-alkalmazás kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="49a28-182">A **Settings** (Beállítások) panelen (az **API app** (API-alkalmazás) paneltől jobbra) keresse meg az **API** szakaszt, majd kattintson a **CORS** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="49a28-182">In the **Settings** blade that opens to the right of the **API app** blade, find the **API** section, and then click **CORS**.</span></span>
   
   ![A CORS kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="49a28-184">A szövegmezőbe írja be a ToDoListAngular (kezelőfelület) webalkalmazás URL-címét.</span><span class="sxs-lookup"><span data-stu-id="49a28-184">In the text box, enter the URL for the ToDoListAngular (front end) web app.</span></span> <span data-ttu-id="49a28-185">Ha például a ToDoListAngular projektet egy todolistangular0121 nevű webalkalmazáshoz telepítette, engedélyezze a hívásokat a következő URL-címről: `https://todolistangular0121.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="49a28-185">For example, if you deployed the ToDoListAngular project to a web app named todolistangular0121, allow calls from the URL `https://todolistangular0121.azurewebsites.net`.</span></span>
   
   <span data-ttu-id="49a28-186">Ha csillagot (*) ír be, azzal beállíthatja, hogy minden eredettartományból elfogadja a hívásokat.</span><span class="sxs-lookup"><span data-stu-id="49a28-186">As an alternative, you can enter an asterisk (*) to specify that all origin domains are accepted.</span></span>
5. <span data-ttu-id="49a28-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="49a28-187">Click **Save**.</span></span>
   
   ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="49a28-189">A **Save** (Mentés) gombra való kattintás után az API-alkalmazás fogadni fogja a megadott URL-címről jövő hívásokat.</span><span class="sxs-lookup"><span data-stu-id="49a28-189">After you click **Save**, the API app will accept JavaScript calls from the specified URL.</span></span> <span data-ttu-id="49a28-190">Ezen a képernyőképen a ToDoListAPI0223 API-alkalmazás fogja fogadni a ToDoListAngular webalkalmazásból jövő JavaScript-ügyfélhívásokat.</span><span class="sxs-lookup"><span data-stu-id="49a28-190">In this screen shot, the ToDoListAPI0223 API app will accept JavaScript client calls from the ToDoListAngular web app.</span></span>

### <a name="test-the-application-with-cors-enabled"></a><span data-ttu-id="49a28-191">Az alkalmazás tesztelése a CORS engedélyezése mellett</span><span class="sxs-lookup"><span data-stu-id="49a28-191">Test the application with CORS enabled</span></span>
* <span data-ttu-id="49a28-192">Nyissa meg egy böngészőben a webalkalmazás HTTPS URL-címét.</span><span class="sxs-lookup"><span data-stu-id="49a28-192">Open a browser to the HTTPS URL of the web app.</span></span> 
  
    <span data-ttu-id="49a28-193">Ezúttal az alkalmazás lehetővé teszi tennivalók megtekintését, hozzáadását, szerkesztését és törlését.</span><span class="sxs-lookup"><span data-stu-id="49a28-193">This time the application lets you view, add, edit, and delete to-do items.</span></span> 
  
    ![A példaalkalmazás To Do List oldala](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a><span data-ttu-id="49a28-195">Az App Service CORS és a webes API CORS összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="49a28-195">App Service CORS versus Web API CORS</span></span>
<span data-ttu-id="49a28-196">Web API-projektekben a [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-csomag telepítésével állíthatja be, hogy milyen tartományokból fogadja az API a JavaScript-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="49a28-196">In a Web API project, you can install the [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package to specify in code which domains your API will accept JavaScript calls from.</span></span>

<span data-ttu-id="49a28-197">A Web API CORS-támogatása rugalmasabb, mint az App Service CORS-támogatása.</span><span class="sxs-lookup"><span data-stu-id="49a28-197">Web API CORS support is more flexible than App Service CORS support.</span></span> <span data-ttu-id="49a28-198">Például a kódban a különböző műveletekhez különböző elfogadott származási helyeket adhat meg, míg az App Service CORS esetében az API-alkalmazás összes függvényéhez csupán az elfogadott tartományok egyetlen halmazát állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="49a28-198">For example, in code you can specify different accepted origins for different action methods, while for App Service CORS you specify one set of accepted origins for all of an API app's methods.</span></span>

> [!NOTE]
> <span data-ttu-id="49a28-199">Egy API-alkalmazásban ne használja a Web API CORS-t és az App Service CORS-t is.</span><span class="sxs-lookup"><span data-stu-id="49a28-199">Don't try to use both Web API CORS and App Service CORS in one API app.</span></span> <span data-ttu-id="49a28-200">Az App Service CORS szolgáltatása elsőbbséget élvez, így a Web API CORS szolgáltatásának nem lesz hatása.</span><span class="sxs-lookup"><span data-stu-id="49a28-200">App Service CORS will take precedence and Web API CORS will have no effect.</span></span> <span data-ttu-id="49a28-201">Ha például az App Service-ben egyetlen eredettartományt engedélyez, a Web API-kódban pedig az összes tartományt engedélyezi, akkor az Azure API-alkalmazás csak az Azure-ban megadott tartományból fogja a hívásokat fogadni.</span><span class="sxs-lookup"><span data-stu-id="49a28-201">For example, if you enable one origin domain in App Service, and enable all origin domains in your Web API code, your Azure API app will only accept calls from the domain you specified in Azure.</span></span>
> 
> 

### <a name="how-to-enable-cors-in-web-api-code"></a><span data-ttu-id="49a28-202">A CORS engedélyezése Web API-kódban</span><span class="sxs-lookup"><span data-stu-id="49a28-202">How to enable CORS in Web API code</span></span>
<span data-ttu-id="49a28-203">A Web API CORS-támogatásának engedélyezése az alábbi lépésekkel foglalható össze.</span><span class="sxs-lookup"><span data-stu-id="49a28-203">The following steps summarize the process for enabling Web API CORS support.</span></span> <span data-ttu-id="49a28-204">További információ: [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api) (Az eltérő eredetű kérések engedélyezése az ASP.NET Web API 2-ben).</span><span class="sxs-lookup"><span data-stu-id="49a28-204">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).</span></span>

1. <span data-ttu-id="49a28-205">Egy Web API-projektben telepítse a [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="49a28-205">In a Web API project, install the [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package.</span></span>
2. <span data-ttu-id="49a28-206">Adja hozzá a `config.EnableCors()` kódsort a **WebApiConfig** osztály **Register** metódusához az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="49a28-206">Include a `config.EnableCors()` line of code in the **Register** method of the **WebApiConfig** class, as in the following example.</span></span> 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. <span data-ttu-id="49a28-207">A Web API-vezérlőben helyezzen el egy `using` utasítást a `System.Web.Http.Cors` névtérhez, és adja hozzá az `EnableCors` attribútumot a vezérlő osztályhoz vagy az egyes műveletmetódusokhoz.</span><span class="sxs-lookup"><span data-stu-id="49a28-207">In your Web API controller, add a `using` statement for the `System.Web.Http.Cors` namespace, and add the `EnableCors` attribute to the controller class or to individual action methods.</span></span> <span data-ttu-id="49a28-208">A következő példában a CORS-támogatás a teljes vezérlőre vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="49a28-208">In the following example, CORS support applies to the entire controller.</span></span>
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a><span data-ttu-id="49a28-209">Az Azure API Management használata API-alkalmazásokkal</span><span class="sxs-lookup"><span data-stu-id="49a28-209">Using Azure API Management with API Apps</span></span>
<span data-ttu-id="49a28-210">Ha az Azure API Management szolgáltatást egy API-alkalmazással használja, az API-alkalmazás helyett az API Management szolgáltatásban konfigurálja a CORS támogatást.</span><span class="sxs-lookup"><span data-stu-id="49a28-210">If you use Azure API Management with an API app, configure CORS in API Management instead of in the API app.</span></span> <span data-ttu-id="49a28-211">További információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="49a28-211">For more information, see the following resources:</span></span>

* [<span data-ttu-id="49a28-212">Az Azure API Management áttekintése (videó: a CORS-ról szóló rész 12:10-nél kezdődik)</span><span class="sxs-lookup"><span data-stu-id="49a28-212">Azure API Management Overview (video: CORS starts at 12:10)</span></span>](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [<span data-ttu-id="49a28-213">Az API Management tartományközi szabályzatai</span><span class="sxs-lookup"><span data-stu-id="49a28-213">API Management cross domain policies</span></span>](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a><span data-ttu-id="49a28-214">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="49a28-214">Troubleshooting</span></span>
<span data-ttu-id="49a28-215">Ha az oktatóanyag lépéseinek elvégzése közben hibákba ütközne, olvassa el az alábbi hibaelhárítási tippeket:</span><span class="sxs-lookup"><span data-stu-id="49a28-215">In case you run into a problem while going through this tutorial, here are some troubleshooting ideas.</span></span>

* <span data-ttu-id="49a28-216">Ellenőrizze, hogy az [Azure SDK for .NET for Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003) legfrissebb verzióját használja-e.</span><span class="sxs-lookup"><span data-stu-id="49a28-216">Make sure that you're using the latest version of the [Azure SDK for .NET for Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).</span></span>
* <span data-ttu-id="49a28-217">Győződjön meg arról, hogy `https`-t írt be a CORS beállításba, valamint arról, hogy `https`-t használ a webalkalmazás kezelőfelületének futtatásához.</span><span class="sxs-lookup"><span data-stu-id="49a28-217">Make sure that you entered `https` in the CORS setting, and make sure that you're using `https` to run the front-end web app.</span></span>
* <span data-ttu-id="49a28-218">Győződjön meg arról, hogy a CORS beállítást a középső rétegbeli API-alkalmazásba, és nem a kezelőfelületbe helyezte el.</span><span class="sxs-lookup"><span data-stu-id="49a28-218">Make sure that you entered the CORS setting in the middle tier API app, not in the front-end web app.</span></span>
* <span data-ttu-id="49a28-219">Ha az alkalmazás kódjában és az Azure App Service platformon is konfigurálja a CORS-támogatást, akkor ne feledje, hogy az App Service CORS-beállítása felülírja az alkalmazás kódjában lévő beállítást.</span><span class="sxs-lookup"><span data-stu-id="49a28-219">If you're configuring CORS in both application code and Azure App Service, note that the App Service CORS setting will override whatever you're doing in application code.</span></span> 

<span data-ttu-id="49a28-220">A Visual Studio hibaelhárítást egyszerűsítő szolgáltatásairól a [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) (Azure App Service-alkalmazások hibáinak elhárítása Visual Studióban) című szakaszban olvashat bővebben.</span><span class="sxs-lookup"><span data-stu-id="49a28-220">To learn more about Visual Studio features that simplify troubleshooting, see [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="49a28-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49a28-221">Next steps</span></span>
<span data-ttu-id="49a28-222">Ebből a cikkből megtudhatta, hogyan engedélyezheti az App Service CORS-támogatását úgy, hogy az ügyfélbeli JavaScript-kód meghívhasson egy másik tartományban lévő API-t.</span><span class="sxs-lookup"><span data-stu-id="49a28-222">In this article, you saw how to enable App Service CORS support so that client JavaScript code can call an API in a different domain.</span></span> <span data-ttu-id="49a28-223">Az API-alkalmazások részletesebb megismeréséhez olvassa el az [App Service-hitelesítésbe való bevezetést](../app-service/app-service-authentication-overview.md), majd nyissa meg az [API-alkalmazásokban való felhasználóhitelesítést](app-service-api-dotnet-user-principal-auth.md) bemutató oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="49a28-223">To learn more about API apps, read the [introduction to authentication in App Service](../app-service/app-service-authentication-overview.md), and then go to the [user authentication for API apps](app-service-api-dotnet-user-principal-auth.md) tutorial.</span></span>

