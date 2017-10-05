---
title: "Telepítse az ASP.NET Core Linux Docker-tároló egy távoli Docker gazdagépen |} Microsoft Docs"
description: "Útmutató: az ASP.NET Core webalkalmazás telepítése az az Azure Docker fogadó Linux virtuális gép futó Docker-tároló Docker Visual Studio eszközök segítségével"
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 4a87ee69f23779bf4f6f5db40bc05edbcfc7668d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a><span data-ttu-id="84431-103">Telepítse az ASP.NET-tároló egy távoli Docker gazdagépen</span><span class="sxs-lookup"><span data-stu-id="84431-103">Deploy an ASP.NET container to a remote Docker host</span></span>
## <a name="overview"></a><span data-ttu-id="84431-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="84431-104">Overview</span></span>
<span data-ttu-id="84431-105">Docker olyan egyszerűsített tároló, virtuális géphez, amely állomás alkalmazások és szolgáltatások segítségével bizonyos értelemben hasonló.</span><span class="sxs-lookup"><span data-stu-id="84431-105">Docker is a lightweight container engine, similar in some ways to a virtual machine, which you can use to host applications and services.</span></span>
<span data-ttu-id="84431-106">Ez az oktatóanyag bemutatja, hogyan használja a [Docker Visual Studio eszközök](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) bővítmény ASP.NET Core alkalmazás egy Docker-állomáshoz az Azure PowerShell használatával történő telepítése.</span><span class="sxs-lookup"><span data-stu-id="84431-106">This tutorial walks you through using the [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84431-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84431-107">Prerequisites</span></span>
<span data-ttu-id="84431-108">A következő szükséges az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="84431-108">The following is required to complete this tutorial:</span></span>

* <span data-ttu-id="84431-109">Hozzon létre egy Azure Docker állomás virtuális gép leírtak [docker-gép használata az Azure-ral](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="84431-109">Create an Azure Docker Host VM as described in [How to use docker-machine with Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="84431-110">Telepítse a legújabb verzióját [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="84431-110">Install the latest version of [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
* <span data-ttu-id="84431-111">Töltse le a [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span><span class="sxs-lookup"><span data-stu-id="84431-111">Download the [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span></span>
* <span data-ttu-id="84431-112">Telepítés [Windows Docker](https://docs.docker.com/docker-for-windows/install/)</span><span class="sxs-lookup"><span data-stu-id="84431-112">Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)</span></span>

## <a name="1-create-an-aspnet-core-web-app"></a><span data-ttu-id="84431-113">1. Az ASP.NET Core webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="84431-113">1. Create an ASP.NET Core web app</span></span>
<span data-ttu-id="84431-114">A következő lépések végigvezetik egy egyszerű ASP.NET Core alkalmazást ebben az oktatóanyagban használt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="84431-114">The following steps guide you through creating a basic ASP.NET Core app that will be used in this tutorial.</span></span>

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="84431-115">2. Adja hozzá a Docker-támogatás</span><span class="sxs-lookup"><span data-stu-id="84431-115">2. Add Docker support</span></span>
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a><span data-ttu-id="84431-116">3. A DockerTask.ps1 PowerShell-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="84431-116">3. Use the DockerTask.ps1 PowerShell Script</span></span>
1. <span data-ttu-id="84431-117">Nyisson meg egy PowerShell-parancssorba, hogy a projekt gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="84431-117">Open a PowerShell prompt to the root directory of your project.</span></span> 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. <span data-ttu-id="84431-118">Ellenőrizze a távoli gazdagépen fut-e.</span><span class="sxs-lookup"><span data-stu-id="84431-118">Validate the remote host is running.</span></span> <span data-ttu-id="84431-119">Megtekintheti az állapot fut =</span><span class="sxs-lookup"><span data-stu-id="84431-119">You should see state = Running</span></span> 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. <span data-ttu-id="84431-120">Létrehozására az alkalmazás - Build paraméter</span><span class="sxs-lookup"><span data-stu-id="84431-120">Build the app using the -Build parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. <span data-ttu-id="84431-121">Futtassa az alkalmazást, használja a - Futtatás paraméter</span><span class="sxs-lookup"><span data-stu-id="84431-121">Run the app, using the -Run parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   <span data-ttu-id="84431-122">Miután befejeződött a docker, az alábbihoz hasonló eredményeket kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="84431-122">Once docker completes, you should see results similar to the following:</span></span>
   
   ![Az alkalmazás megtekintéséhez][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
