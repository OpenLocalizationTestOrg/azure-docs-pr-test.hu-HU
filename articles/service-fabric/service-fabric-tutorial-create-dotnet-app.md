---
title: ".NET-alkalmazás létrehozása a Service Fabric |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy alkalmazást az ASP.NET Core előtér- és egy megbízható szolgáltatás állapot-nyilvántartó háttér-alkalmazás és központi telepítését a fürthöz."
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
ms.openlocfilehash: ef50adf3af19bce494c3256308b443c8eaccdcea
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a><span data-ttu-id="4f823-103">Hozzon létre és telepítsen egy alkalmazást az ASP.NET Core Web API előtér- és egy állapotalapú háttér-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4f823-103">Create and deploy an application with an ASP.NET Core Web API front-end service and a stateful back-end service</span></span>
<span data-ttu-id="4f823-104">Ez az oktatóanyag egy sorozat része.</span><span class="sxs-lookup"><span data-stu-id="4f823-104">This tutorial is part one of a series.</span></span>  <span data-ttu-id="4f823-105">Megtudhatja, hogyan egy Azure Service Fabric-alkalmazás létrehozása az ASP.NET Core Web API előtér és állapot-nyilvántartó háttér-szolgáltatás az adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="4f823-105">You will learn how to create an Azure Service Fabric application with an ASP.NET Core Web API front end and a stateful back-end service to store your data.</span></span> <span data-ttu-id="4f823-106">Amikor végzett, hogy a szavazóalkalmazást az ASP.NET Core webes előtér-állapot-nyilvántartó háttér-szolgáltatás a fürt szavazó eredmények takarít meg, amely a.</span><span class="sxs-lookup"><span data-stu-id="4f823-106">When you're finished, you have a voting application with an ASP.NET Core web front-end that saves voting results in a stateful back-end service in the cluster.</span></span> <span data-ttu-id="4f823-107">Ha nem szeretné manuálisan létrehozni a szavazóalkalmazást, akkor [töltse le a forráskód](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) a kész alkalmazás, és ugorjon előre [végezze el a szavazó mintaalkalmazás](#walkthrough_anchor).</span><span class="sxs-lookup"><span data-stu-id="4f823-107">If you don't want to manually create the voting application, you can [download the source code](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) for the completed application and skip ahead to [Walk through the voting sample application](#walkthrough_anchor).</span></span>

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="4f823-109">A rész az adatsorozatok megismerheti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="4f823-109">In part one of the series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4f823-110">Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f823-110">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="4f823-111">Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f823-111">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="4f823-112">A fordított proxy segítségével kommunikál az állapotalapú szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="4f823-112">Use the reverse proxy to communicate with the stateful service</span></span>

<span data-ttu-id="4f823-113">Az oktatóanyag adatsorozat elsajátíthatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="4f823-113">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="4f823-114">A .NET Service Fabric-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f823-114">Build a .NET Service Fabric application</span></span>
> * [<span data-ttu-id="4f823-115">Telepítse központilag az alkalmazást egy távoli fürthöz</span><span class="sxs-lookup"><span data-stu-id="4f823-115">Deploy the application to a remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [<span data-ttu-id="4f823-116">Konfigurálja a CI/CD Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="4f823-116">Configure CI/CD using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a><span data-ttu-id="4f823-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4f823-117">Prerequisites</span></span>
<span data-ttu-id="4f823-118">Ez az oktatóanyag elkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="4f823-118">Before you begin this tutorial:</span></span>
- <span data-ttu-id="4f823-119">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="4f823-119">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="4f823-120">[Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és telepítse a **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="4f823-120">[Install Visual Studio 2017](https://www.visualstudio.com/) and install the **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="4f823-121">A Service Fabric SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="4f823-121">Install the Service Fabric SDK</span></span>](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a><span data-ttu-id="4f823-122">Megbízható szolgáltatásként ASP.NET Web API-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f823-122">Create an ASP.NET Web API service as a reliable service</span></span>
<span data-ttu-id="4f823-123">Először hozza létre a webes előtér-, az ASP.NET Core segítségével szavazóalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f823-123">First, create the web front-end of the voting application using ASP.NET Core.</span></span> <span data-ttu-id="4f823-124">Az ASP.NET Core egy egyszerűsített, platformfüggetlen webes fejlesztési keretrendszer, amely a webes API-k és modern webes felhasználói felület létrehozása segítségével.</span><span class="sxs-lookup"><span data-stu-id="4f823-124">ASP.NET Core is a lightweight, cross-platform web development framework that you can use to create modern web UI and web APIs.</span></span> <span data-ttu-id="4f823-125">Ahhoz, hogy a teljes ismertetése, hogy az ASP.NET Core hogyan integrálható a Service Fabric, mindenképpen olvassa a [ASP.NET Core a Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="4f823-125">To get a complete understanding of how ASP.NET Core integrates with Service Fabric, we strongly recommend reading through the [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) article.</span></span> <span data-ttu-id="4f823-126">Most hajtsa végre ezt az oktatóanyagot gyorsan.</span><span class="sxs-lookup"><span data-stu-id="4f823-126">For now, you can follow this tutorial to get started quickly.</span></span> <span data-ttu-id="4f823-127">Az ASP.NET Core kapcsolatos további tudnivalókért tekintse meg a [ASP.NET Core dokumentációja](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="4f823-127">To learn more about ASP.NET Core, see the [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

> [!NOTE]
> <span data-ttu-id="4f823-128">Ez az oktatóanyag alapul a [ASP.NET Core eszközei Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="4f823-128">This tutorial is based on the [ASP.NET Core tools for Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc).</span></span> <span data-ttu-id="4f823-129">A .NET Core eszközöket a Visual Studio 2015 már nem frissítés alatt álló.</span><span class="sxs-lookup"><span data-stu-id="4f823-129">The .NET Core tools for Visual Studio 2015 are no longer being updated.</span></span>

1. <span data-ttu-id="4f823-130">Indítsa el a Visual Studiót **rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="4f823-130">Launch Visual Studio as an **administrator**.</span></span>

2. <span data-ttu-id="4f823-131">A projekt létrehozása **fájl**->**új**->**projekt**</span><span class="sxs-lookup"><span data-stu-id="4f823-131">Create a project with **File**->**New**->**Project**</span></span>

3. <span data-ttu-id="4f823-132">Az **Új projekt** párbeszédpanelen válassza a **Felhő > Service Fabric-alkalmazás** elemet.</span><span class="sxs-lookup"><span data-stu-id="4f823-132">In the **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

4. <span data-ttu-id="4f823-133">Adjon nevet az alkalmazásnak **Voting** nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f823-133">Name the application **Voting** and press **OK**.</span></span>

   ![A Visual Studio Új projekt párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. <span data-ttu-id="4f823-135">Az a **új Service Fabric-szolgáltatás** lapon, válassza ki **állapotmentes ASP.NET Core**, és a szolgáltatás **VotingWeb**.</span><span class="sxs-lookup"><span data-stu-id="4f823-135">On the **New Service Fabric Service** page, choose **Stateless ASP.NET Core**, and name your service **VotingWeb**.</span></span>
   
   ![ASP.NET webszolgáltatás kiválasztása az új service párbeszédpanelen](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. <span data-ttu-id="4f823-137">A következő oldalon biztosít az ASP.NET Core projektsablonjai.</span><span class="sxs-lookup"><span data-stu-id="4f823-137">The next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="4f823-138">A jelen oktatóanyag esetében válassza ki a **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4f823-138">For this tutorial, choose **Web Application**.</span></span> 
   
   ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   <span data-ttu-id="4f823-140">Visual Studio létrehoz egy alkalmazás és szolgáltatás projekt, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="4f823-140">Visual Studio creates an application and a service project and displays them in Solution Explorer.</span></span>

   ![A megoldáskezelő ASP.NET alapvető szolgáltatás webes API-alkalmazás létrehozása]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a><span data-ttu-id="4f823-142">A VotingWeb szolgáltatás AngularJS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4f823-142">Add AngularJS to the VotingWeb service</span></span>
<span data-ttu-id="4f823-143">Adja hozzá [AngularJS](http://angularjs.org/) a szolgáltatáshoz a beépített [Bower támogatási](/aspnet/core/client-side/bower).</span><span class="sxs-lookup"><span data-stu-id="4f823-143">Add [AngularJS](http://angularjs.org/) to your service using the built-in [Bower support](/aspnet/core/client-side/bower).</span></span> <span data-ttu-id="4f823-144">Nyissa meg *bower.json* szögben kifejezett és szögben kifejezett rendszerindítási bejegyzés hozzáadása, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="4f823-144">Open *bower.json* and add entries for angular and angular-bootstrap, then save your changes.</span></span>

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
<span data-ttu-id="4f823-145">Mentéskor a *bower.json* fájl, Angular telepítve van a projekt *wwwroot/lib* mappát.</span><span class="sxs-lookup"><span data-stu-id="4f823-145">Upon saving the *bower.json* file, Angular is installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="4f823-146">Ezenkívül szerepel a listán belül a *függőségek/Bower* mappa.</span><span class="sxs-lookup"><span data-stu-id="4f823-146">Additionally, it is listed within the *Dependencies/Bower* folder.</span></span>

### <a name="update-the-sitejs-file"></a><span data-ttu-id="4f823-147">A site.js fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="4f823-147">Update the site.js file</span></span>
<span data-ttu-id="4f823-148">Nyissa meg a *wwwroot/js/site.js* fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f823-148">Open the *wwwroot/js/site.js* file.</span></span>  <span data-ttu-id="4f823-149">Cserélje ki annak tartalmát a JavaScript a kezdőlap nézetek által használt:</span><span class="sxs-lookup"><span data-stu-id="4f823-149">Replace its contents with the JavaScript used by the Home views:</span></span>

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

### <a name="update-the-indexcshtml-file"></a><span data-ttu-id="4f823-150">A Index.cshtml fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="4f823-150">Update the Index.cshtml file</span></span>
<span data-ttu-id="4f823-151">Nyissa meg a *Views/Home/Index.cshtml* fájl, a nézet a kezdőlap vezérlő jellemző.</span><span class="sxs-lookup"><span data-stu-id="4f823-151">Open the *Views/Home/Index.cshtml* file, the view specific to the Home controller.</span></span>  <span data-ttu-id="4f823-152">Cserélje ki annak tartalmát a következőt, majd a változtatások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f823-152">Replace its contents with the following, then save your changes.</span></span>

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
                        Click to vote
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

### <a name="update-the-layoutcshtml-file"></a><span data-ttu-id="4f823-153">A _Layout.cshtml fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="4f823-153">Update the _Layout.cshtml file</span></span>
<span data-ttu-id="4f823-154">Nyissa meg a *Views/Shared/_Layout.cshtml* fájlt, az ASP.NET-alkalmazás alapértelmezett elrendezését.</span><span class="sxs-lookup"><span data-stu-id="4f823-154">Open the *Views/Shared/_Layout.cshtml* file, the default layout for the ASP.NET app.</span></span>  <span data-ttu-id="4f823-155">Cserélje ki annak tartalmát a következőt, majd a változtatások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f823-155">Replace its contents with the following, then save your changes.</span></span>

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

### <a name="update-the-votingwebcs-file"></a><span data-ttu-id="4f823-156">A VotingWeb.cs fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="4f823-156">Update the VotingWeb.cs file</span></span>
<span data-ttu-id="4f823-157">Nyissa meg a *VotingWeb.cs* fájlt, amely létrehozza az ASP.NET Core WebHost az állapot nélküli-szolgáltatást a WebListener webkiszolgáló belül.</span><span class="sxs-lookup"><span data-stu-id="4f823-157">Open the *VotingWeb.cs* file, which creates the ASP.NET Core WebHost inside the stateless service using the WebListener web server.</span></span>  <span data-ttu-id="4f823-158">Adja hozzá a `using System.Net.Http;` irányelv a fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="4f823-158">Add the `using System.Net.Http;` directive to the top of the file.</span></span>  <span data-ttu-id="4f823-159">Cserélje le a `CreateServiceInstanceListeners()` és a következő működni, akkor a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f823-159">Replace the `CreateServiceInstanceListeners()` function with the following, then save your changes.</span></span>

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

### <a name="add-the-votescontrollercs-file"></a><span data-ttu-id="4f823-160">Adja hozzá a VotesController.cs</span><span class="sxs-lookup"><span data-stu-id="4f823-160">Add the VotesController.cs file</span></span>
<span data-ttu-id="4f823-161">Vegyen fel egy vezérlőt, amely meghatározza a szavazó műveletek.</span><span class="sxs-lookup"><span data-stu-id="4f823-161">Add a controller which defines voting actions.</span></span> <span data-ttu-id="4f823-162">Kattintson a jobb gombbal a a **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**.</span><span class="sxs-lookup"><span data-stu-id="4f823-162">Right-click on the **Controllers** folder, then select **Add->New item->Class**.</span></span>  <span data-ttu-id="4f823-163">A fájl neve "VotesController.cs", és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4f823-163">Name the file "VotesController.cs" and click **Add**.</span></span>  <span data-ttu-id="4f823-164">Cserélje ki a fájl tartalmát a következőt, majd a változtatások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f823-164">Replace the file contents with the following, then save your changes.</span></span>  <span data-ttu-id="4f823-165">A későbbi [VotesController.cs fájl frissíthető](#updatevotecontroller_anchor), ezt a fájlt úgy módosítjuk, hogy olvasási és írási szavazó adatok a háttér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4f823-165">Later, in [Update the VotesController.cs file](#updatevotecontroller_anchor), this file will be modified to read and write voting data from the back-end service.</span></span>  <span data-ttu-id="4f823-166">A lépést a tartományvezérlő nézetbe statikus karakterlánc adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4f823-166">For now, the controller returns static string data to the view.</span></span>

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



### <a name="deploy-and-run-the-application-locally"></a><span data-ttu-id="4f823-167">Telepítheti és futtathatja az alkalmazást helyileg</span><span class="sxs-lookup"><span data-stu-id="4f823-167">Deploy and run the application locally</span></span>
<span data-ttu-id="4f823-168">Most lépjen tovább, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f823-168">You can now go ahead and run the application.</span></span> <span data-ttu-id="4f823-169">Nyomja le az `F5` billentyűt a Visual Studióban, hogy üzembe helyezze az alkalmazást a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="4f823-169">In Visual Studio, press `F5` to deploy the application for debugging.</span></span> <span data-ttu-id="4f823-170">`F5`sikertelen lesz, ha nem korábban nyissa meg a Visual Studióban **rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="4f823-170">`F5` fails if you didn't previously open Visual Studio as **administrator**.</span></span>

> [!NOTE]
> <span data-ttu-id="4f823-171">Az alkalmazás első helyi történő üzembe helyezésekor a Visual Studio létrehoz egy helyi hibakeresési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="4f823-171">The first time you run and deploy the application locally, Visual Studio creates a local cluster for debugging.</span></span>  <span data-ttu-id="4f823-172">A fürt létrehozása eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="4f823-172">Cluster creation may take some time.</span></span> <span data-ttu-id="4f823-173">A fürt létrehozási állapota a Visual Studio kimeneti ablakában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4f823-173">The cluster creation status is displayed in the Visual Studio output window.</span></span>

<span data-ttu-id="4f823-174">Ezen a ponton a webalkalmazás kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="4f823-174">At this point, your web app should look like this:</span></span>

![Az ASP.NET Core előtér-](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

<span data-ttu-id="4f823-176">Az alkalmazás hibakeresését végzi leállításához vissza a Visual Studio, és nyomja le az **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="4f823-176">To stop debugging the application, go back to Visual Studio and press **Shift+F5**.</span></span>

## <a name="add-a-stateful-back-end-service-to-your-application"></a><span data-ttu-id="4f823-177">Állapot-nyilvántartó háttér-szolgáltatás hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="4f823-177">Add a stateful back-end service to your application</span></span>
<span data-ttu-id="4f823-178">Most, hogy az ASP.NET Web API-szolgáltatás fut az alkalmazás, lépjen tovább, és adja hozzá egy állapotalapú megbízható szolgáltatást, hogy néhány adat tárolása az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4f823-178">Now that we have an ASP.NET Web API service running in our application, let's go ahead and add a stateful reliable service to store some data in our application.</span></span>

<span data-ttu-id="4f823-179">A Service Fabric következetesen és megbízhatóan tárolja az adatok jobb belül a szolgáltatás megbízható gyűjtemények segítségével teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="4f823-179">Service Fabric allows you to consistently and reliably store your data right inside your service by using reliable collections.</span></span> <span data-ttu-id="4f823-180">Megbízható gyűjtemények olyan magas rendelkezésre állású és megbízható gyűjteményosztály, amelyek bárki, aki használta a C# gyűjtemények számára.</span><span class="sxs-lookup"><span data-stu-id="4f823-180">Reliable collections are a set of highly available and reliable collection classes that are familiar to anyone who has used C# collections.</span></span>

<span data-ttu-id="4f823-181">Ebben az oktatóanyagban létrehoz egy szolgáltatás, amely egy megbízható gyűjtemény egy számláló értékét tárolja.</span><span class="sxs-lookup"><span data-stu-id="4f823-181">In this tutorial, you create a service which stores a counter value in a reliable collection.</span></span>

1. <span data-ttu-id="4f823-182">A Megoldáskezelőben kattintson a jobb gombbal **szolgáltatások** az alkalmazásban le, és válassza a **Hozzáadás > új Service Fabric-szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="4f823-182">In Solution Explorer, right-click **Services** within the application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Egy új szolgáltatás hozzáadása meglévő alkalmazáshoz](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. <span data-ttu-id="4f823-184">Az a **új Service Fabric-szolgáltatás** párbeszédpanelen válasszon **állapotalapú alkalmazások és szolgáltatások ASP.NET Core**, és a szolgáltatás **VotingData** nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f823-184">In the **New Service Fabric Service** dialog, choose **Stateful ASP.NET Core**, and name the service **VotingData** and press **OK**.</span></span>

    ![A Visual Studio Új szolgáltatás párbeszédpanelje](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    <span data-ttu-id="4f823-186">A service-projekt létrehozása után a felhasználó két szolgáltatást az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4f823-186">Once your service project is created, you have two services in your application.</span></span> <span data-ttu-id="4f823-187">Továbbra is építenie az alkalmazást, mert a szolgáltatás ugyanúgy is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="4f823-187">As you continue to build your application, you can add more services in the same way.</span></span> <span data-ttu-id="4f823-188">Minden egyes lehet függetlenül rendszerverzióval ellátott és frissített.</span><span class="sxs-lookup"><span data-stu-id="4f823-188">Each can be independently versioned and upgraded.</span></span>

3. <span data-ttu-id="4f823-189">A következő oldalon biztosít az ASP.NET Core projektsablonjai.</span><span class="sxs-lookup"><span data-stu-id="4f823-189">The next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="4f823-190">A jelen oktatóanyag esetében válassza ki a **Web API**.</span><span class="sxs-lookup"><span data-stu-id="4f823-190">For this tutorial, choose **Web API**.</span></span>

    ![ASP.NET-projekt típusának kiválasztása](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    <span data-ttu-id="4f823-192">A Visual Studio létrehoz egy szolgáltatási projektet, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="4f823-192">Visual Studio creates a service project and displays them in Solution Explorer.</span></span>

    ![Megoldáskezelő](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-the-votedatacontrollercs-file"></a><span data-ttu-id="4f823-194">Adja hozzá a VoteDataController.cs</span><span class="sxs-lookup"><span data-stu-id="4f823-194">Add the VoteDataController.cs file</span></span>

<span data-ttu-id="4f823-195">A a **VotingData** projekt kattintson a jobb gombbal a **tartományvezérlők** mappát, majd válassza ki **Hozzáadás -> Új elem -> osztály**.</span><span class="sxs-lookup"><span data-stu-id="4f823-195">In the **VotingData** project right-click on the **Controllers** folder, then select **Add->New item->Class**.</span></span> <span data-ttu-id="4f823-196">A fájl neve "VoteDataController.cs", és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4f823-196">Name the file "VoteDataController.cs" and click **Add**.</span></span> <span data-ttu-id="4f823-197">Cserélje ki a fájl tartalmát a következőt, majd a változtatások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f823-197">Replace the file contents with the following, then save your changes.</span></span>

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


## <a name="connect-the-services"></a><span data-ttu-id="4f823-198">Csatlakozás a szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="4f823-198">Connect the services</span></span>
<span data-ttu-id="4f823-199">A következő lépésben rendszer összekapcsolja a két szolgáltatásokat és ellenőrizze az előtér-webkiszolgálók alkalmazás beolvasása és beállítása a szavazás a háttér-szolgáltatás adatait.</span><span class="sxs-lookup"><span data-stu-id="4f823-199">In this next step, we will connect the two services and make the front-end Web application get and set voting information from the back-end service.</span></span>

<span data-ttu-id="4f823-200">A Service Fabric hogyan kommunikáljanak megbízható szolgáltatások teljes rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="4f823-200">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="4f823-201">Egyetlen alkalmazásban lehetséges, hogy TCP-n keresztül elérhető szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="4f823-201">Within a single application, you might have services that are accessible via TCP.</span></span> <span data-ttu-id="4f823-202">Elképzelhető, hogy egy HTTP REST API-n keresztül érhető el, hogy más szolgáltatások és egyéb szolgáltatások továbbra is lehet webes szoftvercsatornák keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="4f823-202">Other services that might be accessible via an HTTP REST API and still other services could be accessible via web sockets.</span></span> <span data-ttu-id="4f823-203">A rendelkezésre álló lehetőségeket, és a mellékhatásokkal jár a háttérben, lásd: [szolgáltatások folytatott kommunikáció](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="4f823-203">For background on the options available and the tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span>

<span data-ttu-id="4f823-204">Ebben az oktatóanyagban használjuk [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="4f823-204">In this tutorial, we are using [ASP.NET Core Web API](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a><span data-ttu-id="4f823-205">A VotesController.cs fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="4f823-205">Update the VotesController.cs file</span></span>
<span data-ttu-id="4f823-206">Az a **VotingWeb** projektben nyissa meg a *Controllers/VotesController.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f823-206">In the **VotingWeb** project, open the *Controllers/VotesController.cs* file.</span></span>  <span data-ttu-id="4f823-207">Cserélje le a `VotesController` definíciójának tartalma a következő osztályt, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="4f823-207">Replace the `VotesController` class definition contents with the following, then save your changes.</span></span>

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

## <a name="walk-through-the-voting-sample-application"></a><span data-ttu-id="4f823-208">Végezze el a szavazó mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="4f823-208">Walk through the voting sample application</span></span>
<span data-ttu-id="4f823-209">A szavazó alkalmazás két szolgáltatásból áll:</span><span class="sxs-lookup"><span data-stu-id="4f823-209">The voting application consists of two services:</span></span>
- <span data-ttu-id="4f823-210">Webes előtér-szolgáltatás (VotingWeb) – az ASP.NET Core webes előtér-szolgáltatás, a weblap szolgál, és tesz elérhetővé webes API-khoz a háttér-szolgáltatással való kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="4f823-210">Web front-end service (VotingWeb)- An ASP.NET Core web front-end service, which serves the web page and exposes web APIs to communicate with the backend service.</span></span>
- <span data-ttu-id="4f823-211">Háttér-szolgáltatás (VotingData)-az ASP.NET Core webszolgáltatáshoz, amely közzétesz egy API-t, a megbízható szótárban az szavazattal eredmények tárolásához a lemezen maradnak.</span><span class="sxs-lookup"><span data-stu-id="4f823-211">Back-end service (VotingData)- An ASP.NET Core web service, which exposes an API to store the vote results in a reliable dictionary persisted on disk.</span></span>

![Alkalmazásdiagram](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

<span data-ttu-id="4f823-213">Amikor az alkalmazás szavaz a következők történnek:</span><span class="sxs-lookup"><span data-stu-id="4f823-213">When you vote in the application the following events occur:</span></span>
1. <span data-ttu-id="4f823-214">A JavaScript a szavazás kérést küld egy HTTP PUT-kérelmet, a a webes API-t a webes előtér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4f823-214">A JavaScript sends the vote request to the web API in the web front-end service as an HTTP PUT request.</span></span>

2. <span data-ttu-id="4f823-215">A webes előtér-szolgáltatás egy proxy használatával keresse meg és a háttér-szolgáltatás egy HTTP PUT-kérelmet továbbítja.</span><span class="sxs-lookup"><span data-stu-id="4f823-215">The web front-end service uses a proxy to locate and forward an HTTP PUT request to the back-end service.</span></span>

3. <span data-ttu-id="4f823-216">A háttér-szolgáltatás időt vesz igénybe a bejövő kérelem, és tárolja a frissített eredmény megbízható szótár, amely lekérdezi a fürt több csomópontja replikálása, és a lemezen maradnak.</span><span class="sxs-lookup"><span data-stu-id="4f823-216">The back-end service takes the incoming request, and stores the updated result in a reliable dictionary, which gets replicated to multiple nodes within the cluster and persisted on disk.</span></span> <span data-ttu-id="4f823-217">Az alkalmazás adatokat tárolja a fürt így adatbázis szükséges.</span><span class="sxs-lookup"><span data-stu-id="4f823-217">All the application's data is stored in the cluster, so no database is needed.</span></span>

## <a name="debug-in-visual-studio"></a><span data-ttu-id="4f823-218">A Visual Studio hibakeresési</span><span class="sxs-lookup"><span data-stu-id="4f823-218">Debug in Visual Studio</span></span>
<span data-ttu-id="4f823-219">A Visual Studio alkalmazás nyomkövetésére használ egy helyi Service Fabric-fejlesztési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="4f823-219">When debugging application in Visual Studio, you are using a local Service Fabric development cluster.</span></span> <span data-ttu-id="4f823-220">Lehetősége van úgy, hogy adott esetben a hibakeresési felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="4f823-220">You have the option to adjust your debugging experience to your scenario.</span></span> <span data-ttu-id="4f823-221">Ebben az alkalmazásban tároljuk adatok a háttér-szolgáltatásban egy megbízható szótár használatával.</span><span class="sxs-lookup"><span data-stu-id="4f823-221">In this application, we store data in our back-end service, using a reliable dictionary.</span></span> <span data-ttu-id="4f823-222">A Visual Studio eltávolítja az alkalmazás alapértelmezés szerint a hibakereső leállítása.</span><span class="sxs-lookup"><span data-stu-id="4f823-222">Visual Studio removes the application per default when you stop the debugger.</span></span> <span data-ttu-id="4f823-223">Az alkalmazás eltávolítása azt eredményezi, az adatok a háttér-szolgáltatás is el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="4f823-223">Removing the application causes the data in the back-end service to also be removed.</span></span> <span data-ttu-id="4f823-224">Az adatok között munkamenetek hibakeresés megőrizni, módosíthatja a **alkalmazás hibakeresési módban** meg tulajdonságként a **Voting** projektre a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f823-224">To persist the data between debugging sessions, you can change the **Application Debug Mode** as a property on the **Voting** project in Visual Studio.</span></span>

<span data-ttu-id="4f823-225">Nézze meg mi történik, a kódban, végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4f823-225">To look at what happens in the code, complete the following steps:</span></span>
1. <span data-ttu-id="4f823-226">Nyissa meg a **VotesController.cs** fájlt, és állítson be egy töréspontot a webes API **Put** metódus (sor: 47) – a fájlt a Visual studióban a Solution Explorer kereshet.</span><span class="sxs-lookup"><span data-stu-id="4f823-226">Open the **VotesController.cs** file and set a breakpoint in the web API's **Put** method (line 47) - You can search for the file in the Solution Explorer in Visual Studio.</span></span>

2. <span data-ttu-id="4f823-227">Nyissa meg a **VoteDataController.cs** fájlt, és állítson be egy töréspontot ezen webes API **Put** metódus (sor 50).</span><span class="sxs-lookup"><span data-stu-id="4f823-227">Open the **VoteDataController.cs** file and set a breakpoint in this web API's **Put** method (line 50).</span></span>

3. <span data-ttu-id="4f823-228">Lépjen vissza a böngésző és szavazó lehetőségre, vagy adja hozzá egy új szavazó lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4f823-228">Go back to the browser and click a voting option or add a new voting option.</span></span> <span data-ttu-id="4f823-229">A webalkalmazás első-a befejezési tartozó api-vezérlőben első töréspont kattint.</span><span class="sxs-lookup"><span data-stu-id="4f823-229">You hit the first breakpoint in the web front-end's api controller.</span></span>
    
    1. <span data-ttu-id="4f823-230">Ez azért, ahol a JavaScript a böngészőben egy kérést küld a webes API-vezérlőben az előtér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4f823-230">This is where the JavaScript in the browser sends a request to the web API controller in the front-end service.</span></span>
    
    ![Szavazás előtér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. <span data-ttu-id="4f823-232">Először a háttér-szolgáltatás URL-CÍMÉT a ReverseProxy azt összeállításához **(1)**.</span><span class="sxs-lookup"><span data-stu-id="4f823-232">First we construct the URL to the ReverseProxy for our back-end service **(1)**.</span></span>
    3. <span data-ttu-id="4f823-233">Ezt követően nem küldeni a HTTP PUT kérés a ReverseProxy **(2)**.</span><span class="sxs-lookup"><span data-stu-id="4f823-233">Then we send the HTTP PUT Request to the ReverseProxy **(2)**.</span></span>
    4. <span data-ttu-id="4f823-234">Végül a rendszer visszaadja a már a válasz a háttér-szolgáltatásból az ügyfélnek **(3)**.</span><span class="sxs-lookup"><span data-stu-id="4f823-234">Finally the we return the response from the back-end service to the client **(3)**.</span></span>

4. <span data-ttu-id="4f823-235">Nyomja le az **F5** folytatja</span><span class="sxs-lookup"><span data-stu-id="4f823-235">Press **F5** to continue</span></span>
    1. <span data-ttu-id="4f823-236">Ekkor a számítógép a töréspontot a háttér-szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="4f823-236">You are now at the break point in the back-end service.</span></span>
    
    ![Szavazás háttér-szolgáltatás hozzáadása](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. <span data-ttu-id="4f823-238">A metódus első sorában **(1)** használjuk a `StateManager` nevű megbízható szótár felvétele, illetve `counts`.</span><span class="sxs-lookup"><span data-stu-id="4f823-238">In the first line in the method **(1)** we are using the `StateManager` to get or add a reliable dictionary called `counts`.</span></span>
    3. <span data-ttu-id="4f823-239">A megbízható szótárban értékek minden interakció tranzakció, a használatával szükséges utasítás **(2)** hoz létre, hogy a tranzakció.</span><span class="sxs-lookup"><span data-stu-id="4f823-239">All interactions with values in a reliable dictionary require a transaction, this using statement **(2)** creates that transaction.</span></span>
    4. <span data-ttu-id="4f823-240">A tranzakció frissítjük majd a szavazó beállítás értékének a megfelelő kulcs értékét és véglegesíti a művelet **(3)**.</span><span class="sxs-lookup"><span data-stu-id="4f823-240">In the transaction, we then update the value of the relevant key for the voting option and commits the operation **(3)**.</span></span> <span data-ttu-id="4f823-241">Amikor a véglegesítési mód értéket ad vissza, az adatok frissítése a szótárban és replikálja a fürt többi tagján.</span><span class="sxs-lookup"><span data-stu-id="4f823-241">Once the commit method returns, the data is updated in the dictionary and replicated to other nodes in the cluster.</span></span> <span data-ttu-id="4f823-242">Az adatok immár biztonságosan tárolja a fürt, és a háttér-szolgáltatás átveheti más csomópontokat, a rendelkezésre álló adatok továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="4f823-242">The data is now safely stored in the cluster, and the back-end service can fail over to other nodes, still having the data available.</span></span>
5. <span data-ttu-id="4f823-243">Nyomja le az **F5** folytatja</span><span class="sxs-lookup"><span data-stu-id="4f823-243">Press **F5** to continue</span></span>

<span data-ttu-id="4f823-244">A hibakeresési munkamenetben leállításához nyomja le az **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="4f823-244">To stop the debugging session, press **Shift+F5**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4f823-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f823-245">Next steps</span></span>
<span data-ttu-id="4f823-246">Az oktatóanyag ezen része megtanulta, hogyan:</span><span class="sxs-lookup"><span data-stu-id="4f823-246">In this part of the tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4f823-247">Állapot-nyilvántartó megbízható szolgáltatásként az ASP.NET Core webes API-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f823-247">Create an ASP.NET Core Web API service as a stateful reliable service</span></span>
> * <span data-ttu-id="4f823-248">Állapot nélküli webszolgáltatásként ASP.NET Core webalkalmazás-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f823-248">Create an ASP.NET Core Web Application service as a stateless web service</span></span>
> * <span data-ttu-id="4f823-249">A fordított proxy segítségével kommunikál az állapotalapú szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="4f823-249">Use the reverse proxy to communicate with the stateful service</span></span>

<span data-ttu-id="4f823-250">Előzetes következő oktatóanyagot:</span><span class="sxs-lookup"><span data-stu-id="4f823-250">Advance to the next tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4f823-251">Telepítse központilag az alkalmazást az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="4f823-251">Deploy the application to Azure</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)