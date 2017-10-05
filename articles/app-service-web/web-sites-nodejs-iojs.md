---
title: "Az io.js használata az Azure App Service Web Apps szolgáltatással"
description: "Ismerje meg, hogyan használható egy webalkalmazást az Azure App Service io.js."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 4800504e1939a46842d15e8c9d4279a4b9cae787
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a><span data-ttu-id="6788f-103">Az io.js használata az Azure App Service Web Apps szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="6788f-103">How to use io.js with Azure App Service Web Apps</span></span>
<span data-ttu-id="6788f-104">A népszerű csomópont elágazás [io.js] Joyent a Node.js projekthez, beleértve a modell több megnyitása cégirányítási, gyorsabb kiadás ciklust és gyorsabb elfogadását új és kísérleti JavaScript-szolgáltatások különböző különbségek szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="6788f-104">The popular Node fork [io.js] features various differences to Joyent's Node.js project, including a more open governance model, a faster release cycle and a faster adoption of new and experimental JavaScript features.</span></span>

<span data-ttu-id="6788f-105">Amíg [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások sok Node.js verzió előre telepítve van, lehetővé teszi egy felhasználó által megadott Node.js bináris fájljának.</span><span class="sxs-lookup"><span data-stu-id="6788f-105">While [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps has many Node.js versions preinstalled, it also allows for an user-provided Node.js binary.</span></span> <span data-ttu-id="6788f-106">Ez a cikk ismerteti, amelyek két módszer io.js az App Service Web Apps használatának engedélyezése: egy kiterjesztett telepítési parancsfájlt, amely automatikusan konfigurálja az Azure használata a legújabb elérhető io.js, valamint egy bináris io.js manuális feltöltésének használatát.</span><span class="sxs-lookup"><span data-stu-id="6788f-106">This article discusses two methods enabling the use of io.js on App Service Web Apps: The use of an extended deployment script, which automatically configures Azure to use the latest available io.js version, as well as the manual upload of a io.js binary.</span></span> 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a><span data-ttu-id="6788f-107">A telepítési parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="6788f-107">Using a Deployment Script</span></span>
<span data-ttu-id="6788f-108">A Node.js-alkalmazás központi telepítését, akkor App Service Web Apps kis parancsok futtatásával győződjön meg arról, hogy megfelelően van-e konfigurálva a környezetben több futtatja.</span><span class="sxs-lookup"><span data-stu-id="6788f-108">Upon deployment of a Node.js app, App Service Web Apps runs a number of small commands to ensure that the environment is configured properly.</span></span> <span data-ttu-id="6788f-109">A telepítési parancsfájl használatával, ez a folyamat testre szabható letöltését és io.js konfigurációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6788f-109">Using a deployment script, this process can be customized to include the download and configuration of io.js.</span></span>

<span data-ttu-id="6788f-110">A [io.js telepítési parancsfájl](https://github.com/felixrieseberg/iojs-azure) a Githubon érhető el.</span><span class="sxs-lookup"><span data-stu-id="6788f-110">The [io.js Deployment Script](https://github.com/felixrieseberg/iojs-azure) is available on GitHub.</span></span> <span data-ttu-id="6788f-111">Ahhoz, hogy a webalkalmazás az io.js, egyszerűen másolhatja **.deployment**, **Deploy.cmd fájl** és **iisnode.yml fájlt** a gyökérben, az alkalmazás mappájában, és a webalkalmazások telepítését.</span><span class="sxs-lookup"><span data-stu-id="6788f-111">To enable io.js on your web app, simply copy **.deployment**, **deploy.cmd** and **IISNode.yml** to the root of your application folder and deploy to Web Apps.</span></span>  

<span data-ttu-id="6788f-112">Az első ilyen fájlt, **.deployment**, arra utasítja a webes alkalmazások futtatásához **Deploy.cmd fájl** központi telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="6788f-112">The first file, **.deployment**, instructs Web Apps to run **deploy.cmd** upon deployment.</span></span> <span data-ttu-id="6788f-113">Ezt a parancsfájlt a szokásos lépéseket egy Node.js-alkalmazás fut, de is letölti az io.js legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="6788f-113">This script runs all the usual steps for a Node.js application, but also downloads the latest version of io.js.</span></span> <span data-ttu-id="6788f-114">Végezetül **iisnode.yml fájlt** csak a letöltött io.js használata bináris helyett egy előre telepített Node.js bináris webalkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6788f-114">Finally, **IISNode.yml** configures Web Apps to use just the downloaded io.js binary instead of a pre-installed Node.js binary.</span></span>

> [!NOTE]
> <span data-ttu-id="6788f-115">A parancsfájl és a használt io.js bináris, egyszerűen telepítse újra az alkalmazás - letölti egy új verziója io.js minden telepítik az alkalmazást egy alkalommal.</span><span class="sxs-lookup"><span data-stu-id="6788f-115">To update the used io.js binary, just redeploy your application - the script will download a new version of io.js every single time the application is deployed.</span></span>
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a><span data-ttu-id="6788f-116">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="6788f-116">Using Manual Installation</span></span>
<span data-ttu-id="6788f-117">A manuális telepítés olyan egyéni io.js verzió csak két folyamatát foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="6788f-117">The manual installation of a custom io.js version includes only two steps.</span></span> <span data-ttu-id="6788f-118">Első lépésként töltse le a **win-x64** bináris közvetlenül a [io.js terjesztési].</span><span class="sxs-lookup"><span data-stu-id="6788f-118">First, download the **win-x64** binary directly from the [io.js distribution].</span></span> <span data-ttu-id="6788f-119">Szükség van a két fájlt - **iojs.exe** és **iojs.lib**.</span><span class="sxs-lookup"><span data-stu-id="6788f-119">Required are two files - **iojs.exe** and **iojs.lib**.</span></span> <span data-ttu-id="6788f-120">Mentse mindkét fájlt, hogy egy mappát a web app alkalmazásban például **bin/iojs**.</span><span class="sxs-lookup"><span data-stu-id="6788f-120">Save both files to a folder inside your web app, for example in **bin/iojs**.</span></span>

<span data-ttu-id="6788f-121">Webalkalmazások használandó konfigurálása **iojs.exe** helyett egy előre telepített csomópont, hozzon létre egy **iisnode.yml fájlt** fájlt az alkalmazás gyökérkönyvtárában, és adja hozzá a következő sort.</span><span class="sxs-lookup"><span data-stu-id="6788f-121">To configure Web Apps to use **iojs.exe** instead of a pre-installed Node version, create a **IISNode.yml** file at the root of your application and add the following line.</span></span>

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="6788f-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6788f-122">Next Steps</span></span>
<span data-ttu-id="6788f-123">Ebben a cikkben megtanulta, hogyan használhatja az App Service Web Apps, mind io.js, valamint a manuális telepítés megadott üzembe helyezési parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="6788f-123">In this article you learned how to use io.js with App Service Web Apps, using both provided deployment scripts as well as manual installation.</span></span> 

> [!NOTE]
> <span data-ttu-id="6788f-124">IO.js nehéz fejlesztési, és Node.js gyakoribb.</span><span class="sxs-lookup"><span data-stu-id="6788f-124">io.js is in heavy development and updated more frequently than Node.js.</span></span> <span data-ttu-id="6788f-125">A Node.js modulok száma nem működik együtt io.js - adjon tájékoztatást [a Githubon io.js] hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="6788f-125">A number of Node.js modules might not work with io.js - please consult [io.js on GitHub] for troubleshooting.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="6788f-126">A változások</span><span class="sxs-lookup"><span data-stu-id="6788f-126">What's changed</span></span>
* <span data-ttu-id="6788f-127">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6788f-127">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="6788f-128">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6788f-128">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6788f-129">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="6788f-129">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="6788f-130">[io.js]: https://iojs.org</span><span class="sxs-lookup"><span data-stu-id="6788f-130">[io.js]: https://iojs.org</span></span>
<span data-ttu-id="6788f-131">[io.js terjesztési]: https://iojs.org/dist/</span><span class="sxs-lookup"><span data-stu-id="6788f-131">[io.js distribution]: https://iojs.org/dist/</span></span>
<span data-ttu-id="6788f-132">[a Githubon io.js]: https://github.com/iojs/io.js</span><span class="sxs-lookup"><span data-stu-id="6788f-132">[io.js on GitHub]: https://github.com/iojs/io.js</span></span>
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
