---
title: "az ASP.NET Core Linux Docker tároló tooa távoli Docker-gazdagépen aaaDeploy |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Visual Studio eszközök Docker toodeploy az ASP.NET Core web app tooa Docker-tároló egy Azure Docker fogadó Linux virtuális gépen"
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
ms.openlocfilehash: 27b0c6420628c73220200bc071b47a4cd89fff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-container-tooa-remote-docker-host"></a><span data-ttu-id="1ddb3-103">Az ASP.NET tároló tooa távoli Docker gazdagép telepítése</span><span class="sxs-lookup"><span data-stu-id="1ddb3-103">Deploy an ASP.NET container tooa remote Docker host</span></span>
## <a name="overview"></a><span data-ttu-id="1ddb3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1ddb3-104">Overview</span></span>
<span data-ttu-id="1ddb3-105">Docker olyan egyszerűsített tárolóban, az egyes módszereket tooa virtuális gépet, mely akkor is hasonló toohost alkalmazások és szolgáltatások használatát.</span><span class="sxs-lookup"><span data-stu-id="1ddb3-105">Docker is a lightweight container engine, similar in some ways tooa virtual machine, which you can use toohost applications and services.</span></span>
<span data-ttu-id="1ddb3-106">Ez az oktatóanyag bemutatja, hogyan hello segítségével [Docker Visual Studio eszközök](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) bővítmény toodeploy egy ASP.NET Core app tooa Docker-állomás az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="1ddb3-106">This tutorial walks you through using hello [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) extension toodeploy an ASP.NET Core app tooa Docker host on Azure using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ddb3-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1ddb3-107">Prerequisites</span></span>
<span data-ttu-id="1ddb3-108">hello következő van szükség toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="1ddb3-108">hello following is required toocomplete this tutorial:</span></span>

* <span data-ttu-id="1ddb3-109">Hozzon létre egy Azure Docker állomás virtuális gép leírtak [hogyan toouse docker-gép az Azure-ral](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="1ddb3-109">Create an Azure Docker Host VM as described in [How toouse docker-machine with Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="1ddb3-110">Telepítse a legújabb változatát hello [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="1ddb3-110">Install hello latest version of [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
* <span data-ttu-id="1ddb3-111">Töltse le a hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span><span class="sxs-lookup"><span data-stu-id="1ddb3-111">Download hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span></span>
* <span data-ttu-id="1ddb3-112">Telepítés [Windows Docker](https://docs.docker.com/docker-for-windows/install/)</span><span class="sxs-lookup"><span data-stu-id="1ddb3-112">Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)</span></span>

## <a name="1-create-an-aspnet-core-web-app"></a><span data-ttu-id="1ddb3-113">1. Az ASP.NET Core webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ddb3-113">1. Create an ASP.NET Core web app</span></span>
<span data-ttu-id="1ddb3-114">hello következő lépések végigvezetik egy egyszerű ASP.NET Core alkalmazást ebben az oktatóanyagban használt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1ddb3-114">hello following steps guide you through creating a basic ASP.NET Core app that will be used in this tutorial.</span></span>

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="1ddb3-115">2. Adja hozzá a Docker-támogatás</span><span class="sxs-lookup"><span data-stu-id="1ddb3-115">2. Add Docker support</span></span>
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-hello-dockertaskps1-powershell-script"></a><span data-ttu-id="1ddb3-116">3. Hello DockerTask.ps1 PowerShell-parancsfájl használata</span><span class="sxs-lookup"><span data-stu-id="1ddb3-116">3. Use hello DockerTask.ps1 PowerShell Script</span></span>
1. <span data-ttu-id="1ddb3-117">Nyissa meg a projekt PowerShell Rákérdezés toohello gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="1ddb3-117">Open a PowerShell prompt toohello root directory of your project.</span></span> 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. <span data-ttu-id="1ddb3-118">Érvényesítése hello távoli gazdagépen fut-e.</span><span class="sxs-lookup"><span data-stu-id="1ddb3-118">Validate hello remote host is running.</span></span> <span data-ttu-id="1ddb3-119">Megtekintheti az állapot fut =</span><span class="sxs-lookup"><span data-stu-id="1ddb3-119">You should see state = Running</span></span> 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. <span data-ttu-id="1ddb3-120">Build hello alkalmazás használatával hello - Build paraméter</span><span class="sxs-lookup"><span data-stu-id="1ddb3-120">Build hello app using hello -Build parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. <span data-ttu-id="1ddb3-121">Hello alkalmazás használatával hello - Futtatás paraméter</span><span class="sxs-lookup"><span data-stu-id="1ddb3-121">Run hello app, using hello -Run parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   <span data-ttu-id="1ddb3-122">Miután befejeződött a docker, eredmények hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="1ddb3-122">Once docker completes, you should see results similar toohello following:</span></span>
   
   ![Az alkalmazás megtekintéséhez][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
