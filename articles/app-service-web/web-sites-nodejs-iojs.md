---
title: az Azure App Service Web Apps aaaHow toouse io.js
description: "Megtudhatja, hogyan toouse egy webalkalmazást az io.js az Azure App Service-ben."
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
ms.openlocfilehash: 5dfdac37546b36bc91ab43d9e0a39c2126b4fa9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-iojs-with-azure-app-service-web-apps"></a><span data-ttu-id="44053-103">Hogyan toouse io.js az Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="44053-103">How toouse io.js with Azure App Service Web Apps</span></span>
<span data-ttu-id="44053-104">hello népszerű csomópont elágazás [io.js] különböző különbségek tooJoyent Node.js projekt, beleértve a modell több megnyitása cégirányítási, gyorsabb kiadás ciklust és gyorsabb elfogadását új és kísérleti JavaScript-szolgáltatások szolgáltatásait.</span><span class="sxs-lookup"><span data-stu-id="44053-104">hello popular Node fork [io.js] features various differences tooJoyent's Node.js project, including a more open governance model, a faster release cycle and a faster adoption of new and experimental JavaScript features.</span></span>

<span data-ttu-id="44053-105">Amíg [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások sok Node.js verzió előre telepítve van, lehetővé teszi egy felhasználó által megadott Node.js bináris fájljának.</span><span class="sxs-lookup"><span data-stu-id="44053-105">While [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps has many Node.js versions preinstalled, it also allows for an user-provided Node.js binary.</span></span> <span data-ttu-id="44053-106">Ez a cikk ismerteti, amelyek két módszer az App Service Web Apps io.js hello használatának engedélyezése: hello egy kiterjesztett telepítési parancsfájlt, amely automatikusan konfigurálja az Azure toouse hello legújabb elérhető io.js verzió használatát, valamint az io.js bináris hello manuális feltöltése.</span><span class="sxs-lookup"><span data-stu-id="44053-106">This article discusses two methods enabling hello use of io.js on App Service Web Apps: hello use of an extended deployment script, which automatically configures Azure toouse hello latest available io.js version, as well as hello manual upload of a io.js binary.</span></span> 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a><span data-ttu-id="44053-107">A telepítési parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="44053-107">Using a Deployment Script</span></span>
<span data-ttu-id="44053-108">A Node.js-alkalmazás központi telepítését, akkor App Service Web Apps megfelelően van-e konfigurálva, hogy a környezet hello tooensure kis parancsok számos futtatja.</span><span class="sxs-lookup"><span data-stu-id="44053-108">Upon deployment of a Node.js app, App Service Web Apps runs a number of small commands tooensure that hello environment is configured properly.</span></span> <span data-ttu-id="44053-109">A telepítési parancsfájl használatával, ez a folyamat csak testreszabott tooinclude hello letöltési és io.js konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="44053-109">Using a deployment script, this process can be customized tooinclude hello download and configuration of io.js.</span></span>

<span data-ttu-id="44053-110">Hello [io.js telepítési parancsfájl](https://github.com/felixrieseberg/iojs-azure) a Githubon érhető el.</span><span class="sxs-lookup"><span data-stu-id="44053-110">hello [io.js Deployment Script](https://github.com/felixrieseberg/iojs-azure) is available on GitHub.</span></span> <span data-ttu-id="44053-111">a webalkalmazásban tooenable io.js egyszerűen másolhatja **.deployment**, **Deploy.cmd fájl** és **iisnode.yml fájlt** toohello gyökérmappájában az alkalmazás mappájában és tooWeb alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="44053-111">tooenable io.js on your web app, simply copy **.deployment**, **deploy.cmd** and **IISNode.yml** toohello root of your application folder and deploy tooWeb Apps.</span></span>  

<span data-ttu-id="44053-112">hello első fájl, **.deployment**, arra utasítja a webes alkalmazások toorun **Deploy.cmd fájl** központi telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="44053-112">hello first file, **.deployment**, instructs Web Apps toorun **deploy.cmd** upon deployment.</span></span> <span data-ttu-id="44053-113">Ez a parancsfájl minden hello szokásos lépései a Node.js-alkalmazás fut, de is letölti az io.js hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="44053-113">This script runs all hello usual steps for a Node.js application, but also downloads hello latest version of io.js.</span></span> <span data-ttu-id="44053-114">Végezetül **iisnode.yml fájlt** webalkalmazások toouse letöltött csak hello io.js bináris helyett egy előre telepített Node.js bináris konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="44053-114">Finally, **IISNode.yml** configures Web Apps toouse just hello downloaded io.js binary instead of a pre-installed Node.js binary.</span></span>

> [!NOTE]
> <span data-ttu-id="44053-115">tooupdate hello használt io.js bináris, egyszerűen telepítse újra az alkalmazás - hello parancsfájl letölti azokat a io.js minden egy alkalommal hello alkalmazás központi telepítése egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="44053-115">tooupdate hello used io.js binary, just redeploy your application - hello script will download a new version of io.js every single time hello application is deployed.</span></span>
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a><span data-ttu-id="44053-116">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="44053-116">Using Manual Installation</span></span>
<span data-ttu-id="44053-117">manuális telepítés hello olyan egyéni io.js verzió csak két folyamatát foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="44053-117">hello manual installation of a custom io.js version includes only two steps.</span></span> <span data-ttu-id="44053-118">Első lépésként töltse le a hello **win-x64** bináris közvetlenül a hello [io.js terjesztési].</span><span class="sxs-lookup"><span data-stu-id="44053-118">First, download hello **win-x64** binary directly from hello [io.js distribution].</span></span> <span data-ttu-id="44053-119">Szükség van a két fájlt - **iojs.exe** és **iojs.lib**.</span><span class="sxs-lookup"><span data-stu-id="44053-119">Required are two files - **iojs.exe** and **iojs.lib**.</span></span> <span data-ttu-id="44053-120">Egyaránt mentés-fájlok tooa mappa a web app alkalmazásban például **bin/iojs**.</span><span class="sxs-lookup"><span data-stu-id="44053-120">Save both files tooa folder inside your web app, for example in **bin/iojs**.</span></span>

<span data-ttu-id="44053-121">tooconfigure webalkalmazások toouse **iojs.exe** helyett egy előre telepített csomópont, hozzon létre egy **iisnode.yml fájlt** hello gyökerében az alkalmazás fájlt, és adja hozzá a következő sor hello.</span><span class="sxs-lookup"><span data-stu-id="44053-121">tooconfigure Web Apps toouse **iojs.exe** instead of a pre-installed Node version, create a **IISNode.yml** file at hello root of your application and add hello following line.</span></span>

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="44053-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44053-122">Next Steps</span></span>
<span data-ttu-id="44053-123">Ebben a cikkben megtanulta, hogyan toouse io.js az App Service Web Apps, mind a megadott üzembe helyezési parancsfájlok, valamint a manuális telepítés.</span><span class="sxs-lookup"><span data-stu-id="44053-123">In this article you learned how toouse io.js with App Service Web Apps, using both provided deployment scripts as well as manual installation.</span></span> 

> [!NOTE]
> <span data-ttu-id="44053-124">IO.js nehéz fejlesztési, és Node.js gyakoribb.</span><span class="sxs-lookup"><span data-stu-id="44053-124">io.js is in heavy development and updated more frequently than Node.js.</span></span> <span data-ttu-id="44053-125">A Node.js modulok száma nem működik együtt io.js - adjon tájékoztatást [a Githubon io.js] hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="44053-125">A number of Node.js modules might not work with io.js - please consult [io.js on GitHub] for troubleshooting.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="44053-126">A változások</span><span class="sxs-lookup"><span data-stu-id="44053-126">What's changed</span></span>
* <span data-ttu-id="44053-127">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="44053-127">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="44053-128">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="44053-128">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="44053-129">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="44053-129">No credit cards required; no commitments.</span></span>
> 
> 

[io.js]: https://iojs.org
[io.js terjesztési]: https://iojs.org/dist/
[a Githubon io.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
