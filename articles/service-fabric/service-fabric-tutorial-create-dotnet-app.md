---
title: "a Service Fabric .NET-alkalmazás aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan a toocreate az alkalmazás az ASP.NET Core előtér- és egy megbízható állapot-nyilvántartó háttér-szolgáltatás, és hello alkalmazás tooa fürt központi telepítése."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: bab331b9f8616c50a2794b6c048aace15579c8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a><span data-ttu-id="0eb1d-103">Hozzon létre és telepítsen egy alkalmazást az ASP.NET Core Web API előtér- és egy állapotalapú háttér-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="0eb1d-103">Create and deploy an application with an ASP.NET Core Web API front-end service and a stateful back-end service</span></span>
<span data-ttu-id="0eb1d-104">Ez az oktatóanyag egy sorozat része.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-104">This tutorial is part one of a series.</span></span>  <span data-ttu-id="0eb1d-105">Megtudhatja, hogyan toocreate az ASP.NET Core Web API első Azure Service Fabric alkalmazás befejezését, majd egy állapotalapú háttérszolgáltatásnak toostore adatait.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-105">You will learn how toocreate an Azure Service Fabric application with an ASP.NET Core Web API front end and a stateful back-end service toostore your data.</span></span> <span data-ttu-id="0eb1d-106">Amikor végzett, hogy a szavazóalkalmazást az ASP.NET Core webes előtér-egy állapotalapú háttérszolgáltatásnak hello fürt szavazó eredmények takarít meg, amely a.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-106">When you're finished, you have a voting application with an ASP.NET Core web front-end that saves voting results in a stateful back-end service in hello cluster.</span></span> <span data-ttu-id="0eb1d-107">Ha nem szeretné, hogy toomanually hello szavazás alkalmazás létrehozása, érdemes [hello forráskód letöltése](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) hello befejeződött a alkalmazást, és hagyja ki azokat, amelyek túl[mintaalkalmazás szavazás hello bízná](#walkthrough_anchor).</span><span class="sxs-lookup"><span data-stu-id="0eb1d-107">If you don't want toomanually create hello voting application, you can [download hello source code](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) for hello completed application and skip ahead too[Walk through hello voting sample application](#walkthrough_anchor).</span></span>

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="0eb1d-109">Egyik hello sorozat része, a megismerheti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-109">In part one of hello series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0eb1d-110">Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-110">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="0eb1d-111">Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-111">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="0eb1d-112">Fordított proxy toocommunicate hello használata hello állapotalapú szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="0eb1d-112">Use hello reverse proxy toocommunicate with hello stateful service</span></span>

<span data-ttu-id="0eb1d-113">Az oktatóanyag adatsorozat elsajátíthatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-113">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="0eb1d-114">A .NET Service Fabric-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-114">Build a .NET Service Fabric application</span></span>
> * [<span data-ttu-id="0eb1d-115">Hello alkalmazás tooa távoli fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-115">Deploy hello application tooa remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [<span data-ttu-id="0eb1d-116">Konfigurálja a CI/CD Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="0eb1d-116">Configure CI/CD using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a><span data-ttu-id="0eb1d-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0eb1d-117">Prerequisites</span></span>
<span data-ttu-id="0eb1d-118">Ez az oktatóanyag elkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-118">Before you begin this tutorial:</span></span>
- <span data-ttu-id="0eb1d-119">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="0eb1d-119">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="0eb1d-120">[Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és hello telepítése **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-120">[Install Visual Studio 2017](https://www.visualstudio.com/) and install hello **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="0eb1d-121">Hello Service Fabric SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-121">Install hello Service Fabric SDK</span></span>](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a><span data-ttu-id="0eb1d-122">Megbízható szolgáltatásként ASP.NET Web API-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-122">Create an ASP.NET Web API service as a reliable service</span></span>
<span data-ttu-id="0eb1d-123">Először hozzon létre hello webes előtér-, az ASP.NET Core segítségével alkalmazás szavazás hello.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-123">First, create hello web front-end of hello voting application using ASP.NET Core.</span></span> <span data-ttu-id="0eb1d-124">Az ASP.NET Core egy egyszerűsített, platformfüggetlen webes fejlesztési keretrendszer, hogy toocreate modern webes felhasználói Felületét használja, és webes API-khoz.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-124">ASP.NET Core is a lightweight, cross-platform web development framework that you can use toocreate modern web UI and web APIs.</span></span> <span data-ttu-id="0eb1d-125">a teljes ismertetése, hogy az ASP.NET Core hogyan integrálható a Service Fabric tooget, mindenképpen olvassa keresztül hello [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-125">tooget a complete understanding of how ASP.NET Core integrates with Service Fabric, we strongly recommend reading through hello [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) article.</span></span> <span data-ttu-id="0eb1d-126">Most hajtsa végre az oktatóanyag tooget gyorsan.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-126">For now, you can follow this tutorial tooget started quickly.</span></span> <span data-ttu-id="0eb1d-127">További információ az ASP.NET Core toolearn lásd: hello [ASP.NET Core dokumentációja](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="0eb1d-127">toolearn more about ASP.NET Core, see hello [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

> [!NOTE]
> <span data-ttu-id="0eb1d-128">Ez az oktatóanyag hello alapuló [ASP.NET Core eszközei Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="0eb1d-128">This tutorial is based on hello [ASP.NET Core tools for Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span></span> <span data-ttu-id="0eb1d-129">a Visual Studio 2015 hello .NET Core eszközök már nem frissítés alatt álló.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-129">hello .NET Core tools for Visual Studio 2015 are no longer being updated.</span></span>

1. <span data-ttu-id="0eb1d-130">Indítsa el a Visual Studiót **rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-130">Launch Visual Studio as an **administrator**.</span></span>

2. <span data-ttu-id="0eb1d-131">A projekt létrehozása **fájl**->**új**->**projekt**</span><span class="sxs-lookup"><span data-stu-id="0eb1d-131">Create a project with **File**->**New**->**Project**</span></span>

3. <span data-ttu-id="0eb1d-132">A hello **új projekt** párbeszédpanelen válasszon **felhő > Service Fabric-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-132">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

4. <span data-ttu-id="0eb1d-133">Hello alkalmazás neve **Voting** nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-133">Name hello application **Voting** and press **OK**.</span></span>

   ![A Visual Studio Új projekt párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. <span data-ttu-id="0eb1d-135">A hello **új Service Fabric-szolgáltatás** lapon, válassza ki **állapotmentes ASP.NET Core**, és a szolgáltatás **VotingWeb**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-135">On hello **New Service Fabric Service** page, choose **Stateless ASP.NET Core**, and name your service **VotingWeb**.</span></span>
   
   ![ASP.NET webszolgáltatás kiválasztásával hello új szolgáltatás párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. <span data-ttu-id="0eb1d-137">hello következő oldal biztosít az ASP.NET Core projektsablonjai.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-137">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="0eb1d-138">A jelen oktatóanyag esetében válassza ki a **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-138">For this tutorial, choose **Web Application**.</span></span> 
   
   ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   <span data-ttu-id="0eb1d-140">Visual Studio létrehoz egy alkalmazás és szolgáltatás projekt, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-140">Visual Studio creates an application and a service project and displays them in Solution Explorer.</span></span>

   ![A megoldáskezelő ASP.NET alapvető szolgáltatás webes API-alkalmazás létrehozása]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-toohello-votingweb-service"></a><span data-ttu-id="0eb1d-142">AngularJS toohello VotingWeb szolgáltatás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-142">Add AngularJS toohello VotingWeb service</span></span>
<span data-ttu-id="0eb1d-143">Adja hozzá [AngularJS](http://angularjs.org/) tooyour szolgáltatást hello beépített [Bower támogatási](/aspnet/core/client-side/bower).</span><span class="sxs-lookup"><span data-stu-id="0eb1d-143">Add [AngularJS](http://angularjs.org/) tooyour service using hello built-in [Bower support](/aspnet/core/client-side/bower).</span></span> <span data-ttu-id="0eb1d-144">Nyissa meg *bower.json* szögben kifejezett és szögben kifejezett rendszerindítási bejegyzés hozzáadása, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-144">Open *bower.json* and add entries for angular and angular-bootstrap, then save your changes.</span></span>

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.5",
    "angular-bootstrap": "v1.1.0"
  }
}
```
<span data-ttu-id="0eb1d-145">Hello mentése után *bower.json* fájl, Angular telepítve van a projekt *wwwroot/lib* mappát.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-145">Upon saving hello *bower.json* file, Angular is installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="0eb1d-146">Ezenkívül szerepel a listán belül hello *függőségek/Bower* mappa.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-146">Additionally, it is listed within hello *Dependencies/Bower* folder.</span></span>

### <a name="update-hello-sitejs-file"></a><span data-ttu-id="0eb1d-147">Hello site.js fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-147">Update hello site.js file</span></span>
<span data-ttu-id="0eb1d-148">Nyissa meg hello *wwwroot/js/site.js* fájlt.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-148">Open hello *wwwroot/js/site.js* file.</span></span>  <span data-ttu-id="0eb1d-149">Cserélje ki annak tartalmát hello hello otthoni nézetek által használt JavaScript:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-149">Replace its contents with hello JavaScript used by hello Home views:</span></span>

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-hello-indexcshtml-file"></a><span data-ttu-id="0eb1d-150">Hello Index.cshtml fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-150">Update hello Index.cshtml file</span></span>
<span data-ttu-id="0eb1d-151">Nyissa meg hello *Views/Home/Index.cshtml* fájl, hello adott toohello otthoni nézetvezérlő.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-151">Open hello *Views/Home/Index.cshtml* file, hello view specific toohello Home controller.</span></span>  <span data-ttu-id="0eb1d-152">Cserélje ki annak tartalmát hello következőre, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-152">Replace its contents with hello following, then save your changes.</span></span>

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click toovote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-hello-layoutcshtml-file"></a><span data-ttu-id="0eb1d-153">Hello _Layout.cshtml fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-153">Update hello _Layout.cshtml file</span></span>
<span data-ttu-id="0eb1d-154">Nyissa meg hello *Views/Shared/_Layout.cshtml* fájl, hello alapértelmezett elrendezési hello ASP.NET alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-154">Open hello *Views/Shared/_Layout.cshtml* file, hello default layout for hello ASP.NET app.</span></span>  <span data-ttu-id="0eb1d-155">Cserélje ki annak tartalmát hello következőre, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-155">Replace its contents with hello following, then save your changes.</span></span>

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
    <link href="~/css/site.css" rel="stylesheet" />

</head>
<body>
    <div class="container body-content">
        @RenderBody()
    </div>

    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/angular/angular.js"></script>
    <script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="~/js/site.js"></script>

    @RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-hello-votingwebcs-file"></a><span data-ttu-id="0eb1d-156">Hello VotingWeb.cs fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-156">Update hello VotingWeb.cs file</span></span>
<span data-ttu-id="0eb1d-157">Nyissa meg hello *VotingWeb.cs* fájlt, amely az ASP.NET Core WebHost hello belül hello állapotmentes szolgáltatások hello WebListener webkiszolgáló használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-157">Open hello *VotingWeb.cs* file, which creates hello ASP.NET Core WebHost inside hello stateless service using hello WebListener web server.</span></span>  <span data-ttu-id="0eb1d-158">Adja hozzá a hello `using System.Net.Http;` irányelv toohello hello fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-158">Add hello `using System.Net.Http;` directive toohello top of hello file.</span></span>  <span data-ttu-id="0eb1d-159">Cserélje le a hello `CreateServiceInstanceListeners()` hello következő működik, akkor a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-159">Replace hello `CreateServiceInstanceListeners()` function with hello following, then save your changes.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
            {
                ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting WebListener on {url}");

                return new WebHostBuilder().UseWebListener()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<StatelessServiceContext>(serviceContext)
                                    .AddSingleton<HttpClient>())
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseApplicationInsights()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
            }))
    };
}
```

### <a name="add-hello-votescontrollercs-file"></a><span data-ttu-id="0eb1d-160">Hello VotesController.cs fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-160">Add hello VotesController.cs file</span></span>
<span data-ttu-id="0eb1d-161">Vegyen fel egy vezérlőt, amely meghatározza a szavazó műveletek.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-161">Add a controller which defines voting actions.</span></span> <span data-ttu-id="0eb1d-162">Kattintson a jobb gombbal a hello **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-162">Right-click on hello **Controllers** folder, then select **Add->New item->Class**.</span></span>  <span data-ttu-id="0eb1d-163">Hello fájl "VotesController.cs" nevet, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-163">Name hello file "VotesController.cs" and click **Add**.</span></span>  <span data-ttu-id="0eb1d-164">Cserélje ki hello következő hello fájl tartalmát, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-164">Replace hello file contents with hello following, then save your changes.</span></span>  <span data-ttu-id="0eb1d-165">A későbbi [hello VotesController.cs frissítésfájl](#updatevotecontroller_anchor), ezt a fájlt a rendszer módosított tooread, majd szavazó adatírás hello háttér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-165">Later, in [Update hello VotesController.cs file](#updatevotecontroller_anchor), this file will be modified tooread and write voting data from hello back-end service.</span></span>  <span data-ttu-id="0eb1d-166">Most hello vezérlő statikus karakterlánc toohello adatnézet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-166">For now, hello controller returns static string data toohello view.</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

namespace VotingWeb.Controllers
{
    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```



### <a name="deploy-and-run-hello-application-locally"></a><span data-ttu-id="0eb1d-167">Központi telepítése és hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-167">Deploy and run hello application locally</span></span>
<span data-ttu-id="0eb1d-168">Ezután lépjen tovább és hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-168">You can now go ahead and run hello application.</span></span> <span data-ttu-id="0eb1d-169">A Visual Studio, nyomja le a `F5` toodeploy hello alkalmazást a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-169">In Visual Studio, press `F5` toodeploy hello application for debugging.</span></span> <span data-ttu-id="0eb1d-170">`F5`sikertelen lesz, ha nem korábban nyissa meg a Visual Studióban **rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-170">`F5` fails if you didn't previously open Visual Studio as **administrator**.</span></span>

> [!NOTE]
> <span data-ttu-id="0eb1d-171">hello első alkalommal futtatja, és hello-alkalmazás központi telepítése helyileg, a Visual Studio hibakeresési egy helyi fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-171">hello first time you run and deploy hello application locally, Visual Studio creates a local cluster for debugging.</span></span>  <span data-ttu-id="0eb1d-172">A fürt létrehozása eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-172">Cluster creation may take some time.</span></span> <span data-ttu-id="0eb1d-173">hello fürt létrehozási állapota hello Visual Studio kimeneti ablakában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-173">hello cluster creation status is displayed in hello Visual Studio output window.</span></span>

<span data-ttu-id="0eb1d-174">Ezen a ponton a webalkalmazás kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-174">At this point, your web app should look like this:</span></span>

![Az ASP.NET Core előtér-](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

<span data-ttu-id="0eb1d-176">toostop hello alkalmazásban, lépjen vissza a tooVisual Studio, és nyomja le az **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-176">toostop debugging hello application, go back tooVisual Studio and press **Shift+F5**.</span></span>

## <a name="add-a-stateful-back-end-service-tooyour-application"></a><span data-ttu-id="0eb1d-177">Állapot-nyilvántartó háttérszolgáltatásnak tooyour alkalmazás felvétele</span><span class="sxs-lookup"><span data-stu-id="0eb1d-177">Add a stateful back-end service tooyour application</span></span>
<span data-ttu-id="0eb1d-178">Most, hogy az ASP.NET Web API-szolgáltatás fut az alkalmazás, lépjen tovább, és adja hozzá egy állapotalapú szolgáltatás toostore egyes adatokat az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-178">Now that we have an ASP.NET Web API service running in our application, let's go ahead and add a stateful reliable service toostore some data in our application.</span></span>

<span data-ttu-id="0eb1d-179">A Service Fabric tooconsistently lehetővé teszi, és megbízhatóan tárolja az adatok jobb belül a szolgáltatás megbízható gyűjtemények használatával.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-179">Service Fabric allows you tooconsistently and reliably store your data right inside your service by using reliable collections.</span></span> <span data-ttu-id="0eb1d-180">Megbízható gyűjtemények olyan magas rendelkezésre állású és megbízható gyűjteményosztály, amelyek a megszokott tooanyone, akik C# gyűjtemények használatban van.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-180">Reliable collections are a set of highly available and reliable collection classes that are familiar tooanyone who has used C# collections.</span></span>

<span data-ttu-id="0eb1d-181">Ebben az oktatóanyagban létrehoz egy szolgáltatás, amely egy megbízható gyűjtemény egy számláló értékét tárolja.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-181">In this tutorial, you create a service which stores a counter value in a reliable collection.</span></span>

1. <span data-ttu-id="0eb1d-182">A Megoldáskezelőben kattintson a jobb gombbal **szolgáltatások** belül hello projektet, és válassza a **Hozzáadás > új Service Fabric-szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-182">In Solution Explorer, right-click **Services** within hello application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Új szolgáltatás tooan meglévő alkalmazás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. <span data-ttu-id="0eb1d-184">A hello **új Service Fabric-szolgáltatás** párbeszédpanelen válasszon **állapotalapú alkalmazások és szolgáltatások ASP.NET Core**, és hello szolgáltatást **VotingData** nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-184">In hello **New Service Fabric Service** dialog, choose **Stateful ASP.NET Core**, and name hello service **VotingData** and press **OK**.</span></span>

    ![A Visual Studio Új szolgáltatás párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    <span data-ttu-id="0eb1d-186">A service-projekt létrehozása után a felhasználó két szolgáltatást az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-186">Once your service project is created, you have two services in your application.</span></span> <span data-ttu-id="0eb1d-187">Az alkalmazás továbbra toobuild, adhat hozzá további szolgáltatások hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-187">As you continue toobuild your application, you can add more services in hello same way.</span></span> <span data-ttu-id="0eb1d-188">Minden egyes lehet függetlenül rendszerverzióval ellátott és frissített.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-188">Each can be independently versioned and upgraded.</span></span>

3. <span data-ttu-id="0eb1d-189">hello következő oldal biztosít az ASP.NET Core projektsablonjai.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-189">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="0eb1d-190">A jelen oktatóanyag esetében válassza ki a **Web API**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-190">For this tutorial, choose **Web API**.</span></span>

    ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    <span data-ttu-id="0eb1d-192">A Visual Studio létrehoz egy szolgáltatási projektet, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-192">Visual Studio creates a service project and displays them in Solution Explorer.</span></span>

    ![Megoldáskezelő](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-hello-votedatacontrollercs-file"></a><span data-ttu-id="0eb1d-194">Hello VoteDataController.cs fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-194">Add hello VoteDataController.cs file</span></span>

<span data-ttu-id="0eb1d-195">A hello **VotingData** projektben kattintson jobb gombbal a hello **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-195">In hello **VotingData** project right-click on hello **Controllers** folder, then select **Add->New item->Class**.</span></span> <span data-ttu-id="0eb1d-196">Hello fájl "VoteDataController.cs" nevet, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-196">Name hello file "VoteDataController.cs" and click **Add**.</span></span> <span data-ttu-id="0eb1d-197">Cserélje ki hello következő hello fájl tartalmát, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-197">Replace hello file contents with hello following, then save your changes.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.ServiceFabric.Data;
using System.Threading;
using Microsoft.ServiceFabric.Data.Collections;

namespace VotingData.Controllers
{
    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var ct = new CancellationToken();

            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                var list = await votesDictionary.CreateEnumerableAsync(tx);

                var enumerator = list.GetAsyncEnumerator();

                var result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-hello-services"></a><span data-ttu-id="0eb1d-198">Connect hello szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="0eb1d-198">Connect hello services</span></span>
<span data-ttu-id="0eb1d-199">A következő lépésben rendszer összekapcsolja a hello két szolgáltatásokat és ellenőrizze a hello előtér-webkiszolgáló alkalmazás beolvasása és beállítása a szavazás hello háttér-szolgáltatás adatait.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-199">In this next step, we will connect hello two services and make hello front-end Web application get and set voting information from hello back-end service.</span></span>

<span data-ttu-id="0eb1d-200">A Service Fabric hogyan kommunikáljanak megbízható szolgáltatások teljes rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-200">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="0eb1d-201">Egyetlen alkalmazásban lehetséges, hogy TCP-n keresztül elérhető szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-201">Within a single application, you might have services that are accessible via TCP.</span></span> <span data-ttu-id="0eb1d-202">Elképzelhető, hogy egy HTTP REST API-n keresztül érhető el, hogy más szolgáltatások és egyéb szolgáltatások továbbra is lehet webes szoftvercsatornák keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-202">Other services that might be accessible via an HTTP REST API and still other services could be accessible via web sockets.</span></span> <span data-ttu-id="0eb1d-203">Háttér hello lehetőségeit és hello mellékhatásokkal jár, lásd: [szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="0eb1d-203">For background on hello options available and hello tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

<span data-ttu-id="0eb1d-204">Ebben az oktatóanyagban használjuk [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="0eb1d-204">In this tutorial, we are using [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-hello-votescontrollercs-file"></a><span data-ttu-id="0eb1d-205">Hello VotesController.cs fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-205">Update hello VotesController.cs file</span></span>
<span data-ttu-id="0eb1d-206">A hello **VotingWeb** projektet, nyissa meg hello *Controllers/VotesController.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-206">In hello **VotingWeb** project, open hello *Controllers/VotesController.cs* file.</span></span>  <span data-ttu-id="0eb1d-207">Cserélje le a hello `VotesController` osztály hello következőre definíciójának tartalma, akkor a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-207">Replace hello `VotesController` class definition contents with hello following, then save your changes.</span></span>

```csharp
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;
        string serviceProxyUrl = "http://localhost:19081/Voting/VotingData/api/VoteData";
        string partitionKind = "Int64Range";
        string partitionKey = "0";

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            IEnumerable<KeyValuePair<string, int>> votes;

            HttpResponseMessage response = await this.httpClient.GetAsync($"{serviceProxyUrl}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            votes = JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync());

            return Json(votes);
        }

        // PUT: api/Votes/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            string payload = $"{{ 'name' : '{name}' }}";
            StringContent putContent = new StringContent(payload, Encoding.UTF8, "application/json");
            putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            string proxyUrl = $"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}";

            HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent);

            return new ContentResult()
            {
                StatusCode = (int)response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }

        // DELETE: api/Votes/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            HttpResponseMessage response = await this.httpClient.DeleteAsync($"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            return new OkResult();

        }
    }
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-hello-voting-sample-application"></a><span data-ttu-id="0eb1d-208">Szavazás mintaalkalmazás hello bízná</span><span class="sxs-lookup"><span data-stu-id="0eb1d-208">Walk through hello voting sample application</span></span>
<span data-ttu-id="0eb1d-209">hello szavazás alkalmazás két szolgáltatásból áll:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-209">hello voting application consists of two services:</span></span>
- <span data-ttu-id="0eb1d-210">Webes előtér-szolgáltatás (VotingWeb) – az ASP.NET Core webes előtér-szolgáltatás, hello weblap szolgál, és tesz elérhetővé webes API-k toocommunicate a hello háttérszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-210">Web front-end service (VotingWeb)- An ASP.NET Core web front-end service, which serves hello web page and exposes web APIs toocommunicate with hello backend service.</span></span>
- <span data-ttu-id="0eb1d-211">Háttér-szolgáltatás (VotingData)-az ASP.NET Core a webszolgáltatást tesz elérhetővé, az API toostore hello szavazattal megbízható dictionary eredményezi a lemezen maradnak.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-211">Back-end service (VotingData)- An ASP.NET Core web service, which exposes an API toostore hello vote results in a reliable dictionary persisted on disk.</span></span>

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="0eb1d-213">Amikor szavaz a hello alkalmazás hello a következő események következnek be:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-213">When you vote in hello application hello following events occur:</span></span>
1. <span data-ttu-id="0eb1d-214">JavaScript hello webes előtér-szolgáltatás hello szavazattal kérelem toohello webes API-t, egy HTTP PUT-kérelmet küld.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-214">A JavaScript sends hello vote request toohello web API in hello web front-end service as an HTTP PUT request.</span></span>

2. <span data-ttu-id="0eb1d-215">hello webes előtér-szolgáltatás egy proxy toolocate használ, és továbbítja a HTTP PUT kérés toohello háttér-szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-215">hello web front-end service uses a proxy toolocate and forward an HTTP PUT request toohello back-end service.</span></span>

3. <span data-ttu-id="0eb1d-216">hello háttérszolgáltatásnak hello bejövő kérelem vesz igénybe, és a tárolók hello frissítve megbízható szótár, amely hello fürt csomópontja replikált toomultiple kap, és a lemezen tárolt eredményt.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-216">hello back-end service takes hello incoming request, and stores hello updated result in a reliable dictionary, which gets replicated toomultiple nodes within hello cluster and persisted on disk.</span></span> <span data-ttu-id="0eb1d-217">Minden hello alkalmazásadatok hello fürt tárolja, így nincs adatbázis szükséges.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-217">All hello application's data is stored in hello cluster, so no database is needed.</span></span>

## <a name="debug-in-visual-studio"></a><span data-ttu-id="0eb1d-218">A Visual Studio hibakeresési</span><span class="sxs-lookup"><span data-stu-id="0eb1d-218">Debug in Visual Studio</span></span>
<span data-ttu-id="0eb1d-219">A Visual Studio alkalmazás nyomkövetésére használ egy helyi Service Fabric-fejlesztési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-219">When debugging application in Visual Studio, you are using a local Service Fabric development cluster.</span></span> <span data-ttu-id="0eb1d-220">A hibakeresési élmény tooyour környezettel rendelkezik hello beállítás tooadjust.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-220">You have hello option tooadjust your debugging experience tooyour scenario.</span></span> <span data-ttu-id="0eb1d-221">Ebben az alkalmazásban tároljuk adatok a háttér-szolgáltatásban egy megbízható szótár használatával.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-221">In this application, we store data in our back-end service, using a reliable dictionary.</span></span> <span data-ttu-id="0eb1d-222">A Visual Studio hello alkalmazás alapértelmezés szerint eltávolítja a hello hibakereső leállítása.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-222">Visual Studio removes hello application per default when you stop hello debugger.</span></span> <span data-ttu-id="0eb1d-223">Hello adatok hello alkalmazás eltávolítása hatására a hello háttér-szolgáltatás tooalso el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-223">Removing hello application causes hello data in hello back-end service tooalso be removed.</span></span> <span data-ttu-id="0eb1d-224">toopersist hello adatok hibakeresés a munkamenetek között, módosíthatja hello **alkalmazás hibakeresési módban** hello meg tulajdonságként **Voting** projektre a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-224">toopersist hello data between debugging sessions, you can change hello **Application Debug Mode** as a property on hello **Voting** project in Visual Studio.</span></span>

<span data-ttu-id="0eb1d-225">toolook, mi történik, a lépéseket követve teljes hello hello kódban:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-225">toolook at what happens in hello code, complete hello following steps:</span></span>
1. <span data-ttu-id="0eb1d-226">Nyissa meg hello **VotesController.cs** fájlt, és állítson be egy töréspontot hello webes API **Put** metódus (sor: 47) – hello fájlra a Visual Studio Solution Explorer hello kereshet.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-226">Open hello **VotesController.cs** file and set a breakpoint in hello web API's **Put** method (line 47) - You can search for hello file in hello Solution Explorer in Visual Studio.</span></span>

2. <span data-ttu-id="0eb1d-227">Nyissa meg hello **VoteDataController.cs** fájlt, és állítson be egy töréspontot ezen webes API **Put** metódus (sor 50).</span><span class="sxs-lookup"><span data-stu-id="0eb1d-227">Open hello **VoteDataController.cs** file and set a breakpoint in this web API's **Put** method (line 50).</span></span>

3. <span data-ttu-id="0eb1d-228">Lépjen vissza toohello böngésző és szavazó lehetőségre, vagy adja hozzá egy új szavazó beállítás.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-228">Go back toohello browser and click a voting option or add a new voting option.</span></span> <span data-ttu-id="0eb1d-229">Kattintson az első töréspont hello hello webes első-end api-vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-229">You hit hello first breakpoint in hello web front-end's api controller.</span></span>
    
    1. <span data-ttu-id="0eb1d-230">Ez azért, ahol hello böngészőjében JavaScript hello küldi hello előtér-szolgáltatás egy kérelem toohello webes API-vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-230">This is where hello JavaScript in hello browser sends a request toohello web API controller in hello front-end service.</span></span>
    
    ![Szavazás előtér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. <span data-ttu-id="0eb1d-232">Először a háttér-szolgáltatás hello URL-cím toohello ReverseProxy azt összeállításához **(1)**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-232">First we construct hello URL toohello ReverseProxy for our back-end service **(1)**.</span></span>
    3. <span data-ttu-id="0eb1d-233">Ezt követően kapni hello HTTP PUT kérés toohello ReverseProxy **(2)**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-233">Then we send hello HTTP PUT Request toohello ReverseProxy **(2)**.</span></span>
    4. <span data-ttu-id="0eb1d-234">Végül hello a rendszer visszaadja a hello válasz hello háttérszolgáltatásnak toohello ügyfélről **(3)**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-234">Finally hello we return hello response from hello back-end service toohello client **(3)**.</span></span>

4. <span data-ttu-id="0eb1d-235">Nyomja le az **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="0eb1d-235">Press **F5** toocontinue</span></span>
    1. <span data-ttu-id="0eb1d-236">Ekkor a számítógép hello töréspontot hello háttér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-236">You are now at hello break point in hello back-end service.</span></span>
    
    ![Szavazás háttér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. <span data-ttu-id="0eb1d-238">Hello metódus első sorában hello **(1)** hello használjuk `StateManager` tooget nevű megbízható szótár felvétele vagy `counts`.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-238">In hello first line in hello method **(1)** we are using hello `StateManager` tooget or add a reliable dictionary called `counts`.</span></span>
    3. <span data-ttu-id="0eb1d-239">A megbízható szótárban értékek minden interakció tranzakció, a használatával szükséges utasítás **(2)** hoz létre, hogy a tranzakció.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-239">All interactions with values in a reliable dictionary require a transaction, this using statement **(2)** creates that transaction.</span></span>
    4. <span data-ttu-id="0eb1d-240">Hello tranzakcióban, majd frissítjük hello megfelelő kulcs a szavazás beállítás hello hello értékét, és véglegesíti hello művelet **(3)**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-240">In hello transaction, we then update hello value of hello relevant key for hello voting option and commits hello operation **(3)**.</span></span> <span data-ttu-id="0eb1d-241">Hello véglegesítése után metódus ad vissza, hello adatok hello szótárban frissül, és a replikált tooother hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-241">Once hello commit method returns, hello data is updated in hello dictionary and replicated tooother nodes in hello cluster.</span></span> <span data-ttu-id="0eb1d-242">hello adatok immár biztonságosan tárolja hello fürtben, és hello háttérszolgáltatásnak átveheti tooother csomópontok, adatok hello továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-242">hello data is now safely stored in hello cluster, and hello back-end service can fail over tooother nodes, still having hello data available.</span></span>
5. <span data-ttu-id="0eb1d-243">Nyomja le az **F5** toocontinue</span><span class="sxs-lookup"><span data-stu-id="0eb1d-243">Press **F5** toocontinue</span></span>

<span data-ttu-id="0eb1d-244">hibakeresési munkamenetben, nyomja meg az toostop hello **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="0eb1d-244">toostop hello debugging session, press **Shift+F5**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0eb1d-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0eb1d-245">Next steps</span></span>
<span data-ttu-id="0eb1d-246">Hello oktatóanyag ezen része megtanulta, hogyan:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-246">In this part of hello tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0eb1d-247">Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-247">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="0eb1d-248">Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb1d-248">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="0eb1d-249">Fordított proxy toocommunicate hello használata hello állapotalapú szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="0eb1d-249">Use hello reverse proxy toocommunicate with hello stateful service</span></span>

<span data-ttu-id="0eb1d-250">Előzetes toohello következő oktatóanyaga:</span><span class="sxs-lookup"><span data-stu-id="0eb1d-250">Advance toohello next tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0eb1d-251">Hello alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="0eb1d-251">Deploy hello application tooAzure</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
