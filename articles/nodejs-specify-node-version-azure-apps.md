---
title: "a Node.js verzió aaaSpecifying"
description: "Ismerje meg, hogyan toospecify hello Node.js Azure-webhelyek és Felhőszolgáltatások által használt verziója"
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 09c27bfc43c132b6d66f9a2943179e06ee75bedc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a><span data-ttu-id="fabfe-103">A Node.js verzió megadása az Azure alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="fabfe-103">Specifying a Node.js version in an Azure application</span></span>
<span data-ttu-id="fabfe-104">Amikor egy Node.js-alkalmazást üzemeltet, érdemes lehet az alkalmazás által egy adott verziójához Node.js tooensure.</span><span class="sxs-lookup"><span data-stu-id="fabfe-104">When hosting a Node.js application, you may want tooensure that your application uses a specific version of Node.js.</span></span> <span data-ttu-id="fabfe-105">Nincsenek számos módon tooaccomplish Ez az Azure-platformon futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="fabfe-105">There are several ways tooaccomplish this for applications hosted on Azure.</span></span>

## <a name="default-versions"></a><span data-ttu-id="fabfe-106">Alapértelmezett verziók</span><span class="sxs-lookup"><span data-stu-id="fabfe-106">Default versions</span></span>
<span data-ttu-id="fabfe-107">Azure által biztosított hello Node.js verzió folyamatosan frissülnek.</span><span class="sxs-lookup"><span data-stu-id="fabfe-107">hello Node.js versions provided by Azure are constantly updated.</span></span> <span data-ttu-id="fabfe-108">Hacsak másként nincs megadva, alapértelmezett verzió hello megadott hello `WEBSITE_NODE_DEFAULT_VERSION` környezeti változó fogja használni.</span><span class="sxs-lookup"><span data-stu-id="fabfe-108">Unless otherwise specified, hello default version that is specified in hello `WEBSITE_NODE_DEFAULT_VERSION` environment variable will be used.</span></span> <span data-ttu-id="fabfe-109">toooverride Ez az alapértelmezett érték, ez a cikk a következő szakaszokban kövesse hello lépéseket</span><span class="sxs-lookup"><span data-stu-id="fabfe-109">toooverride this default value, follow hello steps in following sections of this article</span></span>

> [!NOTE]
> <span data-ttu-id="fabfe-110">Ha az alkalmazást egy Azure-Felhőszolgáltatásban (webes vagy feldolgozói szerepkör) üzemeltet, és hello először hello alkalmazást telepített, Azure kísérli meg toouse hello Node.js ugyanazon verzióját telepítette a fejlesztési környezet Ha, akkor megfelelő hello alapértelmezett verziójú, amely az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="fabfe-110">If you are hosting your application in an Azure Cloud Service (web or worker role,) and it is hello first time you have deployed hello application, Azure will attempt toouse hello same version of Node.js as you have installed on your development environment if it matches one of hello default versions available on Azure.</span></span>
>
>

## <a name="versioning-with-packagejson"></a><span data-ttu-id="fabfe-111">A package.json Versioning</span><span class="sxs-lookup"><span data-stu-id="fabfe-111">Versioning with package.json</span></span>
<span data-ttu-id="fabfe-112">Adja hozzá a következő tooyour hello használt Node.js toobe hello verzióját is megadhat **package.json** fájlt:</span><span class="sxs-lookup"><span data-stu-id="fabfe-112">You can specify hello version of Node.js toobe used by adding hello following tooyour **package.json** file:</span></span>

    "engines":{"node":version}

<span data-ttu-id="fabfe-113">Ha *verzió* hello adott verziójához számú toouse van.</span><span class="sxs-lookup"><span data-stu-id="fabfe-113">Where *version* is hello specific version number toouse.</span></span> <span data-ttu-id="fabfe-114">Például megadhatja verziójához, összetettebb feltételek:</span><span class="sxs-lookup"><span data-stu-id="fabfe-114">You can specify more complex conditions for version, such as:</span></span>

    "engines":{"node": "0.6.22 || 0.8.x"}

<span data-ttu-id="fabfe-115">Mivel 0.6.22 nem érhető el a gazdakörnyezet hello hello-verziók valamelyikét, hello hello 0,8 adatsorozat elérhető lesz a legújabb verziót használja - 0.8.4.</span><span class="sxs-lookup"><span data-stu-id="fabfe-115">Since 0.6.22 is not one of hello versions available in hello hosting environment, hello highest version of hello 0.8 series that is available will be used instead - 0.8.4.</span></span>

## <a name="versioning-websites-with-app-settings"></a><span data-ttu-id="fabfe-116">Az alkalmazásbeállítások Versioning webhelyek</span><span class="sxs-lookup"><span data-stu-id="fabfe-116">Versioning Websites with App Settings</span></span>
<span data-ttu-id="fabfe-117">Ha egy webhelyen lévő hello alkalmazást üzemeltet, beállíthatja azt a hello környezeti változó **WEBSITE_NODE_DEFAULT_VERSION** toohello kívánt verziójával.</span><span class="sxs-lookup"><span data-stu-id="fabfe-117">If you are hosting hello application in a Website, you can set hello environment variable **WEBSITE_NODE_DEFAULT_VERSION** toohello desired version.</span></span>

## <a name="versioning-cloud-services-with-powershell"></a><span data-ttu-id="fabfe-118">A PowerShell-lel Versioning Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="fabfe-118">Versioning Cloud Services with PowerShell</span></span>
<span data-ttu-id="fabfe-119">Ha egy felhőalapú szolgáltatás hello alkalmazást üzemeltető, és az Azure PowerShell hello alkalmazást telepít, felülbírálhatja hello segítségével hello alapértelmezett Node.js verziót **Set-AzureServiceProjectRole** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="fabfe-119">If you are hosting hello application in a Cloud Service, and are deploying hello application using Azure PowerShell, you can override hello default Node.js version by using hello **Set-AzureServiceProjectRole** PowerShell cmdlet.</span></span> <span data-ttu-id="fabfe-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="fabfe-120">For example:</span></span>

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

<span data-ttu-id="fabfe-121">Megjegyzés: hello fent utasítás hello paraméterei kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="fabfe-121">Note hello parameters in hello above statement are case-sensitive.</span></span>  <span data-ttu-id="fabfe-122">Ellenőrizheti a Node.js hello verziójának kijelölt hello ellenőrzésével **motorok** tulajdonság a szerepkör **package.json**.</span><span class="sxs-lookup"><span data-stu-id="fabfe-122">You can verify hello correct version of Node.js has been selected by checking hello **engines** property in your role's **package.json**.</span></span>

<span data-ttu-id="fabfe-123">Is használhatja a hello **Get-AzureServiceProjectRoleRuntime** tooretrieve az Node.js verzió érhető el a felhőalapú szolgáltatásként üzemeltetett alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="fabfe-123">You can also use hello **Get-AzureServiceProjectRoleRuntime** tooretrieve a list of Node.js versions available for applications hosted as a Cloud Service.</span></span>  <span data-ttu-id="fabfe-124">Mindig győződjön meg arról a projekt függ Node.js hello verzióját ezen a listán.</span><span class="sxs-lookup"><span data-stu-id="fabfe-124">Always verify hello version of Node.js your project depends on is in this list.</span></span>

## <a name="using-a-custom-version-with-azure-websites"></a><span data-ttu-id="fabfe-125">Az Azure Websitesra egyéni verziójával</span><span class="sxs-lookup"><span data-stu-id="fabfe-125">Using a custom version with Azure Websites</span></span>
<span data-ttu-id="fabfe-126">Amíg az Azure biztosít a Node.js több alapértelmezett verzióját, érdemes lehet toouse olyan verzióra, amely alapértelmezés szerint nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="fabfe-126">While Azure provides several default versions of Node.js, you may want toouse a version that is not provided by default.</span></span> <span data-ttu-id="fabfe-127">Ha az alkalmazás üzemel, egy Azure-webhelyre, ez elvégezhető hello segítségével **iisnode.yml fájlt** fájlt.</span><span class="sxs-lookup"><span data-stu-id="fabfe-127">If your application is hosted as an Azure Website, you can accomplish this by using hello **iisnode.yml** file.</span></span> <span data-ttu-id="fabfe-128">hello következő lépéseit ismerteti hello folyamatot, amely az Azure-webhely Node.Js egyéni verzióját használja:</span><span class="sxs-lookup"><span data-stu-id="fabfe-128">hello following steps walk through hello process of using a custom version of Node.Js with an Azure Website:</span></span>

1. <span data-ttu-id="fabfe-129">Hozzon létre egy új könyvtárat, és hozzon létre egy **server.js** hello könyvtárban lévő fájlt.</span><span class="sxs-lookup"><span data-stu-id="fabfe-129">Create a new directory, and then create a **server.js** file within hello directory.</span></span> <span data-ttu-id="fabfe-130">Hello **server.js** fájl hello következőket kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="fabfe-130">hello **server.js** file should contain hello following:</span></span>

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    <span data-ttu-id="fabfe-131">Ez megjeleníti hello Node.js verziót használják, ha a felhasználó hello webhelyet.</span><span class="sxs-lookup"><span data-stu-id="fabfe-131">This will display hello Node.js version being used when you browse hello website.</span></span>
2. <span data-ttu-id="fabfe-132">Hozzon létre egy új webhelyet és megjegyzés hello hello hely nevét.</span><span class="sxs-lookup"><span data-stu-id="fabfe-132">Create a new Website and note hello name of hello site.</span></span> <span data-ttu-id="fabfe-133">Például a következő hello használ hello [Azure parancssori eszközök] toocreate nevű új Azure webhely **segítségével**, és engedélyez egy Git-tárház hello webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="fabfe-133">For example, hello following uses hello [Azure Command-line tools] toocreate a new Azure Website named **mywebsite**, and then enable a Git repository for hello website.</span></span>

        azure site create mywebsite --git
3. <span data-ttu-id="fabfe-134">Hozzon létre egy új könyvtárat nevű **bin** hello tartalmazó hello könyvtár gyermekeként **server.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="fabfe-134">Create a new directory named **bin** as a child of hello directory containing hello **server.js** file.</span></span>
4. <span data-ttu-id="fabfe-135">Töltse le az adott verziójához hello **node.exe** (hello Windows-verzió) toouse kíván az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="fabfe-135">Download hello specific version of **node.exe** (hello Windows version) that you wish toouse with your application.</span></span> <span data-ttu-id="fabfe-136">Például a következő felhasználási hello **curl** toodownload verzió 0.8.1:</span><span class="sxs-lookup"><span data-stu-id="fabfe-136">For example, hello following uses **curl** toodownload version 0.8.1:</span></span>

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    <span data-ttu-id="fabfe-137">Mentse a hello **node.exe** hello fájlból **bin** korábban létrehozott mappába.</span><span class="sxs-lookup"><span data-stu-id="fabfe-137">Save hello **node.exe** file into hello **bin** folder created previously.</span></span>
5. <span data-ttu-id="fabfe-138">Hozzon létre egy **iisnode.yml fájlt** hello fájlt azonos könyvtárhoz, mint a hello **server.js** fájlt, és adja hozzá a következő tartalom toohello hello **iisnode.yml fájlt** fájlt:</span><span class="sxs-lookup"><span data-stu-id="fabfe-138">Create an **iisnode.yml** file in hello same directory as hello **server.js** file, and then add hello following content toohello **iisnode.yml** file:</span></span>

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    <span data-ttu-id="fabfe-139">Ez az elérési út, ahol hello **node.exe** fájlt a projektben található után az alkalmazás toohello Azure webhelyen közzétett.</span><span class="sxs-lookup"><span data-stu-id="fabfe-139">This path is where hello **node.exe** file within your project will be located once you have published your application toohello Azure Website.</span></span>
6. <span data-ttu-id="fabfe-140">Az alkalmazás közzétételére.</span><span class="sxs-lookup"><span data-stu-id="fabfe-140">Publish your application.</span></span> <span data-ttu-id="fabfe-141">Például egy új webhely korábbi hello--git paraméterrel létrehozott, mert hello következő parancsok fog hello alkalmazás fájlok toomy helyi Git-tárház hozzáadása, és majd küldje le őket toohello webhely tárház:</span><span class="sxs-lookup"><span data-stu-id="fabfe-141">For example, since I created a new website with hello --git parameter earlier, hello following commands will add hello application files toomy local Git repository, and then push them toohello website repository:</span></span>

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    <span data-ttu-id="fabfe-142">Miután közzétette hello alkalmazás, hello webhely megnyitása a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="fabfe-142">After hello application has published, open hello website in a browser.</span></span> <span data-ttu-id="fabfe-143">Megjelenik egy üzenet "Hello Azure futó csomópont verziójáról: v0.8.1".</span><span class="sxs-lookup"><span data-stu-id="fabfe-143">You should see a message stating "Hello from Azure running node version: v0.8.1".</span></span>

## <a name="next-steps"></a><span data-ttu-id="fabfe-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fabfe-144">Next Steps</span></span>
<span data-ttu-id="fabfe-145">Most, hogy megértse, hogyan Node.js toospecify hello verzióját használja-e az alkalmazás, megtudhatja, hogyan túl[a modulok], [és üzembe egy Node.js webhely](app-service-web/app-service-web-get-started-nodejs.md), és [hogyan toouse hello Azure A Mac és Linux parancssori eszközök].</span><span class="sxs-lookup"><span data-stu-id="fabfe-145">Now that you understand how toospecify hello version of Node.js used by your application, learn how too[work with modules], [build and deploy a Node.js Web Site](app-service-web/app-service-web-get-started-nodejs.md), and [How toouse hello Azure Command-Line Tools for Mac and Linux].</span></span>

<span data-ttu-id="fabfe-146">További információkért lásd: hello [Node.js fejlesztői központ](https://azure.microsoft.com/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="fabfe-146">For more information, see hello [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/).</span></span>

[hogyan toouse hello Azure A Mac és Linux parancssori eszközök]:cli-install-nodejs.md
[Azure parancssori eszközök]:cli-install-nodejs.md
[a modulok]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service-web/app-service-web-get-started-nodejs.md
