---
title: "aaaCORS támogatja az App Service szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatják a toouse CORS Azure Azure App Service-ben."
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
ms.openlocfilehash: c229378b75840bc0f7b2eefc3df3031233f9b494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-api-app-from-javascript-using-cors"></a><span data-ttu-id="9c7b3-103">API-alkalmazások felhasználása JavaScriptből a CORS használatával</span><span class="sxs-lookup"><span data-stu-id="9c7b3-103">Consume an API app from JavaScript using CORS</span></span>
<span data-ttu-id="9c7b3-104">App Service beépített támogatást nyújt a [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), lehetővé teszi a JavaScript ügyfelek toomake tartományközi meghívja az API-alkalmazások tárolt tooAPIs.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-104">App Service offers built-in support for [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), which enables JavaScript clients toomake cross-domain calls tooAPIs that are hosted in API apps.</span></span> <span data-ttu-id="9c7b3-105">App Service segítségével úgy konfigurálja a CORS hozzáférés tooyour API a API programozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-105">App Service lets you configure CORS access tooyour API without writing any code in your API.</span></span>

<span data-ttu-id="9c7b3-106">Ez a cikk két részből áll:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-106">This article contains two sections:</span></span>

* <span data-ttu-id="9c7b3-107">Hello [hogyan tooconfigure CORS](#corsconfig) a szakasz ismerteti a általános hogyan tooconfigure CORS API-alkalmazás, webalkalmazás, és mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-107">hello [How tooconfigure CORS](#corsconfig) section explains in general how tooconfigure CORS for any API app, web app, or mobile app.</span></span> <span data-ttu-id="9c7b3-108">Ez egyaránt érvényes tooall keretrendszerek, amelyet az App Service, beleértve a .NET, Node.js és Java támogatott.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-108">It applies equally tooall frameworks that are supported by App Service, including .NET, Node.js, and Java.</span></span> 
* <span data-ttu-id="9c7b3-109">Hello kezdve [hello .NET-bevezető oktatóanyagok folytatása](#tutorialstart) szakaszban hello cikk mutatja be a CORS-támogatás oktató építve [hello első API-alkalmazások alapszintű bemutató ](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-109">Starting with hello [Continuing hello .NET getting-started tutorials](#tutorialstart) section, hello article is a tutorial that demonstrates CORS support by building on what you did in [hello first API Apps getting started tutorial](app-service-api-dotnet-get-started.md).</span></span> 

## <span data-ttu-id="9c7b3-110"><a id="corsconfig"></a>Hogyan tooconfigure az Azure App Service CORS</span><span class="sxs-lookup"><span data-stu-id="9c7b3-110"><a id="corsconfig"></a> How tooconfigure CORS in Azure App Service</span></span>
<span data-ttu-id="9c7b3-111">Cors szolgáltatást konfigurálhatja a hello Azure-portálon vagy [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) eszközök.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-111">You can configure CORS in hello Azure portal or by using [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tools.</span></span>

#### <a name="configure-cors-in-hello-azure-portal"></a><span data-ttu-id="9c7b3-112">A CORS konfigurálása hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9c7b3-112">Configure CORS in hello Azure portal</span></span>
1. <span data-ttu-id="9c7b3-113">Egy böngészőben nyissa meg toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-113">In a browser go toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9c7b3-114">Kattintson a **alkalmazásszolgáltatások**, majd kattintson az API-alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-114">Click **App Services**, and then click hello name of your API app.</span></span>
   
    ![API-alkalmazás kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="9c7b3-116">A hello **beállítások** panelen megjelenő toohello jobb oldalán hello **API-alkalmazás** panelen, a keresés hello **API** szakaszt, és kattintson a **CORS**.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-116">In hello **Settings** blade that opens toohello right of hello **API app** blade, find hello **API** section, and then click **CORS**.</span></span>
   
   ![Válassza a CORS lehetőséget a Beállítások panelen](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="9c7b3-118">Hello mezőbe írjon be hello URL-címe vagy tooallow JavaScript-hívásokat toocome a használni kívánt URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-118">In hello text box enter hello URL or URLs that you want tooallow JavaScript calls toocome from.</span></span>

    <span data-ttu-id="9c7b3-119">Például ha telepítette a JavaScript alkalmazás tooa webalkalmazás todolistangular nevű webalkalmazásra telepítette, akkor adja meg az "https://todolistangular.azurewebsites.net" értéket.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-119">For example, if you deployed your JavaScript application tooa web app named todolistangular, enter "https://todolistangular.azurewebsites.net".</span></span> <span data-ttu-id="9c7b3-120">Alternatív megoldásként megadhat egy csillag (*) toospecify, hogy minden eredettartományból elfogadja a hívásokat.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-120">As an alternative, you can enter an asterisk (*) toospecify that all origin domains are accepted.</span></span>


1. <span data-ttu-id="9c7b3-121">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-121">Click **Save**.</span></span>
   
   ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="9c7b3-123">Miután rákattintott **mentése**, hello API-alkalmazás fogadni fogja a JavaScript hello hívásait megadott URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-123">After you click **Save**, hello API app will accept JavaScript calls from hello specified URLs.</span></span>

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a><span data-ttu-id="9c7b3-124">A CORS konfigurálása az Azure Resource Manager eszközeinek használatával</span><span class="sxs-lookup"><span data-stu-id="9c7b3-124">Configure CORS by using Azure Resource Manager tools</span></span>
<span data-ttu-id="9c7b3-125">Beállíthatja úgy is a CORS API-alkalmazás használatával [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-authoring-templates.md) parancssori eszközökben, például a [Azure PowerShell](/powershell/azureps-cmdlets-docs) és hello [Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-125">You can also configure CORS for an API app by using [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) in command line tools such as [Azure PowerShell](/powershell/azureps-cmdlets-docs) and hello [Azure CLI](../cli-install-nodejs.md).</span></span> 

<span data-ttu-id="9c7b3-126">Például egy olyan Azure Resource Manager sablon, amely beállítja hello CORS tulajdonságot, nyissa meg a hello [azuredeploy.json fájlt az oktatóanyag példaalkalmazását hello tárház](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-126">For an example of an Azure Resource Manager template that sets hello CORS property, open hello [azuredeploy.json file in hello repository for this tutorial's sample application](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span></span> <span data-ttu-id="9c7b3-127">A következő példa hello néz hello sablon hello szakasz keresése:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-127">Find hello section of hello template that looks like hello following example:</span></span>

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <span data-ttu-id="9c7b3-128"><a id="tutorialstart"></a>Hello .NET-bevezető oktatóanyag folytatása</span><span class="sxs-lookup"><span data-stu-id="9c7b3-128"><a id="tutorialstart"></a> Continuing hello .NET getting-started tutorial</span></span>
<span data-ttu-id="9c7b3-129">Ha hello Node.js vagy Java-bevezető sorozat API-alkalmazások, első lépések adatsorozat befejezett hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-129">If you are following hello Node.js or Java getting-started series for API apps, you have completed hello getting started series.</span></span> <span data-ttu-id="9c7b3-130">Hagyja ki a toohello [további lépések](#next-steps) szakasz toofind javaslatok API-alkalmazások bővebb megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-130">Skip toohello [Next steps](#next-steps) section toofind suggestions for further learning about API Apps.</span></span>

<span data-ttu-id="9c7b3-131">hello a cikk hátralévő része hello .NET-bevezető sorozat folytatása, és feltételezi, hogy Ön sikeresen elvégezte [hello első oktatóanyaga, amely](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-131">hello remainder of this article is a continuation of hello .NET getting-started series and assumes that you successfully completed [hello first tutorial](app-service-api-dotnet-get-started.md).</span></span>

## <a name="deploy-hello-todolistangular-project-tooa-new-web-app"></a><span data-ttu-id="9c7b3-132">Hello ToDoListAngular projekt tooa új webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="9c7b3-132">Deploy hello ToDoListAngular project tooa new web app</span></span>
<span data-ttu-id="9c7b3-133">A [hello első oktatóanyaga, amely](app-service-api-dotnet-get-started.md), létrehozott egy középső rétegbeli API-alkalmazást és egy adatrétegbeli API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-133">In [hello first tutorial](app-service-api-dotnet-get-started.md), you created a middle tier API app and a data tier API app.</span></span> <span data-ttu-id="9c7b3-134">Ebben az oktatóanyagban létrehoz egy egyoldalas alkalmazások (SPA) webalkalmazás adott hívások hello középső réteg API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-134">In this tutorial you create a single-page application (SPA) web app that calls hello middle tier API app.</span></span> <span data-ttu-id="9c7b3-135">Hello SPA toowork a hello középső rétegbeli API-alkalmazás CORS tooenable rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-135">For hello SPA toowork you have tooenable CORS on hello middle tier API app.</span></span> 

<span data-ttu-id="9c7b3-136">A hello [ToDoList példaalkalmazásban](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), hello ToDoListAngular projekt egy egyszerű AngularJS ügyfél hello középső rétegbeli ToDoListAPI webes API projektet hívja.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-136">In hello [ToDoList sample application](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), hello ToDoListAngular project is a simple AngularJS client that calls hello middle tier ToDoListAPI Web API project.</span></span> <span data-ttu-id="9c7b3-137">JavaScript-kód hello hello *app/scripts/todoListSvc.js* fájl hívja hello API-t hello AngularJS HTTP-szolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-137">hello JavaScript code in hello *app/scripts/todoListSvc.js* file calls hello API by using hello AngularJS HTTP provider.</span></span> 

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

### <a name="create-a-new-web-app-for-hello-todolistangular-project"></a><span data-ttu-id="9c7b3-138">Hozzon létre egy új webalkalmazást hello ToDoListAngular projekthez</span><span class="sxs-lookup"><span data-stu-id="9c7b3-138">Create a new web app for hello ToDoListAngular project</span></span>
<span data-ttu-id="9c7b3-139">hello eljárás toocreate egy új App Service web app és projekt telepítése tooit a látott hasonló toowhat [létrehozása és telepítése az API-alkalmazás az a sorozat első oktatóanyaga hello](app-service-api-dotnet-get-started.md#createapiapp).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-139">hello procedure toocreate a new App Service web app and deploy a project tooit is similar toowhat you saw for [creating and deploying an API app in hello first tutorial in this series](app-service-api-dotnet-get-started.md#createapiapp).</span></span> <span data-ttu-id="9c7b3-140">hello egyetlen különbség az, hogy hello alkalmazás típusa van **webalkalmazás** helyett **API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-140">hello only difference is that hello app type is **Web App** instead of **API App**.</span></span>  <span data-ttu-id="9c7b3-141">Hello párbeszédpanelek képernyőképeit lásd:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-141">For screen shots of hello dialogs, see</span></span> 

1. <span data-ttu-id="9c7b3-142">A **Megoldáskezelőben**, kattintson a jobb gombbal a ToDoListAngular projekt hello, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-142">In **Solution Explorer**, right-click hello ToDoListAngular project, and then click **Publish**.</span></span>
2. <span data-ttu-id="9c7b3-143">A hello **profil** hello lapján **webhely közzététele** varázsló, kattintson a **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-143">In hello **Profile** tab of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="9c7b3-144">A hello **App Service** párbeszédpanel, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-144">In hello **App Service** dialog box, click **New**.</span></span>
4. <span data-ttu-id="9c7b3-145">A hello **üzemeltetési** hello lapján **App Service létrehozása** párbeszédpanelen adja meg egy **webalkalmazás neve** hello az egyedi *azurewebsites.net* tartomány.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-145">In hello **Hosting** tab of hello **Create App Service** dialog box, enter a **Web App Name** that is unique in hello *azurewebsites.net* domain.</span></span> 
5. <span data-ttu-id="9c7b3-146">Válassza ki a hello Azure **előfizetés** azt szeretné, hogy a toowork.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-146">Choose hello Azure **Subscription** you want toowork with.</span></span>
6. <span data-ttu-id="9c7b3-147">A hello **erőforráscsoport** legördülő menüben válassza ki a hello ugyanazt az a korábban létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-147">In hello **Resource Group** drop-down list, choose hello same resource group you created earlier.</span></span>
7. <span data-ttu-id="9c7b3-148">A hello **App Service-csomag** legördülő menüben válassza ki a hello korábban létrehozott csomagot.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-148">In hello **App Service Plan** drop-down list, choose hello same plan you created earlier.</span></span> 
8. <span data-ttu-id="9c7b3-149">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-149">Click **Create**.</span></span>
   
    <span data-ttu-id="9c7b3-150">A Visual Studio létrehozza hello webalkalmazást, a közzétételi profilt hozza létre és hello megjeleníti **kapcsolat** hello lépését **webhely közzététele** varázsló.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-150">Visual Studio creates hello web app, creates a publish profile for it, and displays hello **Connection** step of hello **Publish Web** wizard.</span></span>
   
    <span data-ttu-id="9c7b3-151">Még ne kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-151">Don't click **Publish** yet.</span></span> <span data-ttu-id="9c7b3-152">A következő szakasz hello hello új alkalmazás toocall hello középső réteg API webalkalmazást az App Service-ben futtató konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-152">In hello following section, you configure hello new web app toocall hello middle tier API app that is running in App Service.</span></span> 

### <a name="set-hello-middle-tier-url-in-web-app-settings"></a><span data-ttu-id="9c7b3-153">Hello középső réteg URL-Címének beállítása a webalkalmazás beállításaiban</span><span class="sxs-lookup"><span data-stu-id="9c7b3-153">Set hello middle tier URL in web app settings</span></span>
1. <span data-ttu-id="9c7b3-154">Toohello lépjen [Azure-portálon](https://portal.azure.com/), és navigáljon a toohello **webalkalmazás** toohost hello TodoListAngular (kezelőfelület) projekt létrehozott hello webalkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-154">Go toohello [Azure portal](https://portal.azure.com/), and then navigate toohello **Web App** blade for hello web app that you created toohost hello TodoListAngular (front end) project.</span></span>
2. <span data-ttu-id="9c7b3-155">Kattintson a **Settings > Application Settings** (Beállítások > Alkalmazásbeállítások) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-155">Click **Settings > Application Settings**.</span></span>
3. <span data-ttu-id="9c7b3-156">A hello **Alkalmazásbeállítások** területen írja be a következő hello kulcs-érték:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-156">In hello **App settings** section, add hello following key and value:</span></span>
   
   | <span data-ttu-id="9c7b3-157">Kulcs</span><span class="sxs-lookup"><span data-stu-id="9c7b3-157">Key</span></span> | <span data-ttu-id="9c7b3-158">Érték</span><span class="sxs-lookup"><span data-stu-id="9c7b3-158">Value</span></span> | <span data-ttu-id="9c7b3-159">Példa</span><span class="sxs-lookup"><span data-stu-id="9c7b3-159">Example</span></span> |
   | --- | --- | --- |
   | <span data-ttu-id="9c7b3-160">toDoListAPIURL</span><span class="sxs-lookup"><span data-stu-id="9c7b3-160">toDoListAPIURL</span></span> |<span data-ttu-id="9c7b3-161">https://{a középső réteg API-alkalmazásának neve}.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="9c7b3-161">https://{your middle tier API app name}.azurewebsites.net</span></span> |<span data-ttu-id="9c7b3-162">https://todolistapi0121.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="9c7b3-162">https://todolistapi0121.azurewebsites.net</span></span> |
4. <span data-ttu-id="9c7b3-163">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-163">Click **Save**.</span></span>
   
    <span data-ttu-id="9c7b3-164">Amikor hello kód lefut az Azure-ban, ez az érték felülbírálja hello localhost URL-címet a hello *Web.config* fájlt.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-164">When hello code runs in Azure, this value overrides hello localhost URL that is in hello *Web.config* file.</span></span> 
   
    <span data-ttu-id="9c7b3-165">hello beállításérték hello kód van *index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-165">hello code that gets hello setting value is in *index.cshtml*:</span></span>
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    <span data-ttu-id="9c7b3-166">a kód hello *todoListSvc.js* hello beállítást használja:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-166">hello code in *todoListSvc.js* uses hello setting:</span></span>
   
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

### <a name="deploy-hello-todolistangular-web-project-toohello-new-web-app"></a><span data-ttu-id="9c7b3-167">Hello ToDoListAngular webes projekt toohello új webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="9c7b3-167">Deploy hello ToDoListAngular web project toohello new web app</span></span>
* <span data-ttu-id="9c7b3-168">A Visual Studio, a hello **kapcsolat** hello lépését **webhely közzététele** varázsló, kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-168">In Visual Studio, in hello **Connection** step of hello **Publish Web** wizard, click **Publish**.</span></span>
  
   <span data-ttu-id="9c7b3-169">A Visual Studio hello ToDoListAngular projekt toohello webalkalmazása telepíti, és megnyitja a böngésző toohello webalkalmazás URL-címe hello.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-169">Visual Studio deploys hello ToDoListAngular project toohello new web app and opens a browser toohello URL of hello web app.</span></span> 

### <a name="test-hello-application-without-cors-enabled"></a><span data-ttu-id="9c7b3-170">Hello alkalmazás tesztelése a CORS engedélyezése nélkül</span><span class="sxs-lookup"><span data-stu-id="9c7b3-170">Test hello application without CORS enabled</span></span>
1. <span data-ttu-id="9c7b3-171">Nyissa meg a böngésző fejlesztői eszközök, hello Console ablakban.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-171">In your browser Developer Tools, open hello Console window.</span></span>
2. <span data-ttu-id="9c7b3-172">Megjeleníti az AngularJS felhasználói felület hello hello böngészőablakban, kattintson a hello **tooDo lista** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-172">In hello browser window that displays hello AngularJS UI, click hello **tooDo List** link.</span></span>
   
    <span data-ttu-id="9c7b3-173">hello JavaScript-kód megpróbál toocall hello középső rétegbeli API-alkalmazás, de hello hívás sikertelen lesz, mivel hello előtér mint hello vissza egy másik tartományban fut célból.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-173">hello JavaScript code tries toocall hello middle tier API app, but hello call fails because hello front end is running in a different domain than hello back end.</span></span> <span data-ttu-id="9c7b3-174">hello böngésző fejlesztői eszközök konzol ablakának egy eltérő eredetű hibaüzenetet jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-174">hello browser's Developer Tools Console window shows a cross-origin error message.</span></span>
   
    ![Hibaüzenet az eltérő eredetről](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-hello-middle-tier-api-app"></a><span data-ttu-id="9c7b3-176">Hello középső rétegbeli API-alkalmazás CORS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c7b3-176">Configure CORS for hello middle tier API app</span></span>
<span data-ttu-id="9c7b3-177">Ebben a szakaszban konfigurálni hello CORS beállítása az Azure-ban hello középső rétegbeli ToDoListAPI API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-177">In this section, you configure hello CORS setting in Azure for hello middle tier ToDoListAPI API app.</span></span> <span data-ttu-id="9c7b3-178">Ez a beállítás lehetővé teszi a hello középső rétegbeli API app tooreceive JavaScript-hívásokat webalkalmazásból hello hello ToDoListAngular projekthez létrehozott.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-178">This setting will allow hello middle tier API app tooreceive JavaScript calls from hello web app that you created for hello ToDoListAngular project.</span></span>

1. <span data-ttu-id="9c7b3-179">Egy böngészőben nyissa meg toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-179">In a browser, go toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9c7b3-180">Kattintson a **alkalmazásszolgáltatások**, majd kattintson a hello ToDoListAPI (középső réteg) API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-180">Click **App Services**, and then click hello ToDoListAPI (middle tier) API app.</span></span>
   
    ![API-alkalmazás kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="9c7b3-182">A hello **beállítások** panelen megjelenő toohello jobb oldalán hello **API-alkalmazás** panelen, a keresés hello **API** szakaszt, és kattintson a **CORS**.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-182">In hello **Settings** blade that opens toohello right of hello **API app** blade, find hello **API** section, and then click **CORS**.</span></span>
   
   ![A CORS kiválasztása a portálon](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="9c7b3-184">Hello szövegmezőben hello ToDoListAngular (kezelőfelület) webalkalmazás hello URL-cím megadása.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-184">In hello text box, enter hello URL for hello ToDoListAngular (front end) web app.</span></span> <span data-ttu-id="9c7b3-185">Például ha hello ToDoListAngular projekt tooa webalkalmazás todolistangular0121 nevű webalkalmazáshoz telepítette, engedélyezze a hívásokat a hello URL-cím `https://todolistangular0121.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-185">For example, if you deployed hello ToDoListAngular project tooa web app named todolistangular0121, allow calls from hello URL `https://todolistangular0121.azurewebsites.net`.</span></span>
   
   <span data-ttu-id="9c7b3-186">Alternatív megoldásként megadhat egy csillag (*) toospecify, hogy minden eredettartományból elfogadja a hívásokat.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-186">As an alternative, you can enter an asterisk (*) toospecify that all origin domains are accepted.</span></span>
5. <span data-ttu-id="9c7b3-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-187">Click **Save**.</span></span>
   
   ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="9c7b3-189">Miután rákattintott **mentése**, hello API-alkalmazás fogadni fogja a JavaScript hello hívásait megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-189">After you click **Save**, hello API app will accept JavaScript calls from hello specified URL.</span></span> <span data-ttu-id="9c7b3-190">Ezen a képernyőfelvételen a hello ToDoListAPI0223 API-alkalmazás fogja fogadni a ToDoListAngular webalkalmazásból hello JavaScript-ügyfélhívásokat.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-190">In this screen shot, hello ToDoListAPI0223 API app will accept JavaScript client calls from hello ToDoListAngular web app.</span></span>

### <a name="test-hello-application-with-cors-enabled"></a><span data-ttu-id="9c7b3-191">Hello alkalmazás tesztelése a CORS engedélyezése mellett</span><span class="sxs-lookup"><span data-stu-id="9c7b3-191">Test hello application with CORS enabled</span></span>
* <span data-ttu-id="9c7b3-192">Nyissa meg a böngésző toohello hello webalkalmazás HTTPS URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-192">Open a browser toohello HTTPS URL of hello web app.</span></span> 
  
    <span data-ttu-id="9c7b3-193">Az idő hello alkalmazás lehetővé teszi megtekintése, hozzáadása, szerkesztése és törlése a Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-193">This time hello application lets you view, add, edit, and delete to-do items.</span></span> 
  
    ![a mintaalkalmazás tooDo lista lap](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a><span data-ttu-id="9c7b3-195">Az App Service CORS és a webes API CORS összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="9c7b3-195">App Service CORS versus Web API CORS</span></span>
<span data-ttu-id="9c7b3-196">A Web API-projektet telepíthet hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet csomag toospecify a kódban, mely tartományok tartományokból fogadja az API a JavaScript-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-196">In a Web API project, you can install hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package toospecify in code which domains your API will accept JavaScript calls from.</span></span>

<span data-ttu-id="9c7b3-197">A Web API CORS-támogatása rugalmasabb, mint az App Service CORS-támogatása.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-197">Web API CORS support is more flexible than App Service CORS support.</span></span> <span data-ttu-id="9c7b3-198">Például a kódban a különböző műveletekhez különböző elfogadott származási helyeket adhat meg, míg az App Service CORS esetében az API-alkalmazás összes függvényéhez csupán az elfogadott tartományok egyetlen halmazát állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-198">For example, in code you can specify different accepted origins for different action methods, while for App Service CORS you specify one set of accepted origins for all of an API app's methods.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7b3-199">Ne próbáljon toouse Web API CORS és az App Service CORS egy API-alkalmazás is.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-199">Don't try toouse both Web API CORS and App Service CORS in one API app.</span></span> <span data-ttu-id="9c7b3-200">Az App Service CORS szolgáltatása elsőbbséget élvez, így a Web API CORS szolgáltatásának nem lesz hatása.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-200">App Service CORS will take precedence and Web API CORS will have no effect.</span></span> <span data-ttu-id="9c7b3-201">Például ha engedélyezi az App Service egy forrástartományt, és minden eredettartományból engedélyezéséhez a Web API-kódban, az Azure API-alkalmazás fogja csak hívásokat fogadni hello Azure-ban megadott tartományból.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-201">For example, if you enable one origin domain in App Service, and enable all origin domains in your Web API code, your Azure API app will only accept calls from hello domain you specified in Azure.</span></span>
> 
> 

### <a name="how-tooenable-cors-in-web-api-code"></a><span data-ttu-id="9c7b3-202">Hogyan tooenable CORS webes API-kódban</span><span class="sxs-lookup"><span data-stu-id="9c7b3-202">How tooenable CORS in Web API code</span></span>
<span data-ttu-id="9c7b3-203">a lépéseket követve hello hello folyamat Web API CORS-támogatás engedélyezésének foglalják össze.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-203">hello following steps summarize hello process for enabling Web API CORS support.</span></span> <span data-ttu-id="9c7b3-204">További információ: [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api) (Az eltérő eredetű kérések engedélyezése az ASP.NET Web API 2-ben).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-204">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).</span></span>

1. <span data-ttu-id="9c7b3-205">A Web API-projektet, telepítse a hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-205">In a Web API project, install hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package.</span></span>
2. <span data-ttu-id="9c7b3-206">Tartalmaznak egy `config.EnableCors()` hello kódsort **regisztrálása** hello metódusában **register** osztály, mint például a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-206">Include a `config.EnableCors()` line of code in hello **Register** method of hello **WebApiConfig** class, as in hello following example.</span></span> 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // hello following line enables you toocontrol CORS by using Web API code
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
3. <span data-ttu-id="9c7b3-207">Adja hozzá a Web API-vezérlőben egy `using` hello nyilatkozata `System.Web.Http.Cors` névteret, és adja hozzá a hello `EnableCors` toohello-vezérlő osztályhoz vagy tooindividual műveletmetódusokhoz attribútum.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-207">In your Web API controller, add a `using` statement for hello `System.Web.Http.Cors` namespace, and add hello `EnableCors` attribute toohello controller class or tooindividual action methods.</span></span> <span data-ttu-id="9c7b3-208">A következő példa hello, a CORS-támogatás toohello teljes vezérlőre vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-208">In hello following example, CORS support applies toohello entire controller.</span></span>
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a><span data-ttu-id="9c7b3-209">Az Azure API Management használata API-alkalmazásokkal</span><span class="sxs-lookup"><span data-stu-id="9c7b3-209">Using Azure API Management with API Apps</span></span>
<span data-ttu-id="9c7b3-210">Ha Azure API Management használata API-alkalmazást, adja meg a CORS API felügyelete hello API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-210">If you use Azure API Management with an API app, configure CORS in API Management instead of in hello API app.</span></span> <span data-ttu-id="9c7b3-211">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-211">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="9c7b3-212">Az Azure API Management áttekintése (videó: a CORS-ról szóló rész 12:10-nél kezdődik)</span><span class="sxs-lookup"><span data-stu-id="9c7b3-212">Azure API Management Overview (video: CORS starts at 12:10)</span></span>](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [<span data-ttu-id="9c7b3-213">Az API Management tartományközi szabályzatai</span><span class="sxs-lookup"><span data-stu-id="9c7b3-213">API Management cross domain policies</span></span>](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a><span data-ttu-id="9c7b3-214">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9c7b3-214">Troubleshooting</span></span>
<span data-ttu-id="9c7b3-215">Ha az oktatóanyag lépéseinek elvégzése közben hibákba ütközne, olvassa el az alábbi hibaelhárítási tippeket:</span><span class="sxs-lookup"><span data-stu-id="9c7b3-215">In case you run into a problem while going through this tutorial, here are some troubleshooting ideas.</span></span>

* <span data-ttu-id="9c7b3-216">Győződjön meg arról, hogy hello hello legújabb verzióját használja [Azure SDK for .NET for Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-216">Make sure that you're using hello latest version of hello [Azure SDK for .NET for Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).</span></span>
* <span data-ttu-id="9c7b3-217">Győződjön meg arról, hogy a megadott `https` hello CORS beállítását, és győződjön meg arról, hogy használata `https` toorun hello előtér-webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-217">Make sure that you entered `https` in hello CORS setting, and make sure that you're using `https` toorun hello front-end web app.</span></span>
* <span data-ttu-id="9c7b3-218">Győződjön meg arról, hogy a megadott hello CORS beállításba hello középső rétegbeli API-alkalmazás, és nem hello előtér-webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-218">Make sure that you entered hello CORS setting in hello middle tier API app, not in hello front-end web app.</span></span>
* <span data-ttu-id="9c7b3-219">Ha az alkalmazás kódjában és az Azure App Service CORS konfigurálja, vegye figyelembe, hogy hello App Service CORS-beállítása felülírja alkalmazáskód végzett függetlenül.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-219">If you're configuring CORS in both application code and Azure App Service, note that hello App Service CORS setting will override whatever you're doing in application code.</span></span> 

<span data-ttu-id="9c7b3-220">További információ a Visual Studio funkcióit, hibaelhárítást egyszerűsítő toolearn lásd [hibaelhárítási Azure App Service apps szolgáltatásban a Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="9c7b3-220">toolearn more about Visual Studio features that simplify troubleshooting, see [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c7b3-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9c7b3-221">Next steps</span></span>
<span data-ttu-id="9c7b3-222">Ebből a cikkből megtudhatta, hogyan támogatják a tooenable App Service CORS, hogy az ügyfélbeli JavaScript-kód meghívhatja az API-k egy másik tartományban.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-222">In this article, you saw how tooenable App Service CORS support so that client JavaScript code can call an API in a different domain.</span></span> <span data-ttu-id="9c7b3-223">További API-alkalmazások, olvassa el a hello toolearn [bemutatása tooauthentication az App Service](../app-service/app-service-authentication-overview.md), és folytassa a toohello [API-alkalmazások felhasználói hitelesítésének](app-service-api-dotnet-user-principal-auth.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="9c7b3-223">toolearn more about API apps, read hello [introduction tooauthentication in App Service](../app-service/app-service-authentication-overview.md), and then go toohello [user authentication for API apps](app-service-api-dotnet-user-principal-auth.md) tutorial.</span></span>

