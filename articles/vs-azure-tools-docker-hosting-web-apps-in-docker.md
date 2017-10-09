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
# <a name="deploy-an-aspnet-container-tooa-remote-docker-host"></a>Az ASP.NET tároló tooa távoli Docker gazdagép telepítése
## <a name="overview"></a>Áttekintés
Docker olyan egyszerűsített tárolóban, az egyes módszereket tooa virtuális gépet, mely akkor is hasonló toohost alkalmazások és szolgáltatások használatát.
Ez az oktatóanyag bemutatja, hogyan hello segítségével [Docker Visual Studio eszközök](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) bővítmény toodeploy egy ASP.NET Core app tooa Docker-állomás az Azure PowerShell használatával.

## <a name="prerequisites"></a>Előfeltételek
hello következő van szükség toocomplete Ez az oktatóanyag:

* Hozzon létre egy Azure Docker állomás virtuális gép leírtak [hogyan toouse docker-gép az Azure-ral](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Telepítse a legújabb változatát hello [Visual Studio](https://www.visualstudio.com/downloads/)
* Töltse le a hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
* Telepítés [Windows Docker](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Az ASP.NET Core webalkalmazás létrehozása
hello következő lépések végigvezetik egy egyszerű ASP.NET Core alkalmazást ebben az oktatóanyagban használt létrehozása.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Adja hozzá a Docker-támogatás
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-hello-dockertaskps1-powershell-script"></a>3. Hello DockerTask.ps1 PowerShell-parancsfájl használata
1. Nyissa meg a projekt PowerShell Rákérdezés toohello gyökérkönyvtár. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Érvényesítése hello távoli gazdagépen fut-e. Megtekintheti az állapot fut = 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Build hello alkalmazás használatával hello - Build paraméter
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Hello alkalmazás használatával hello - Futtatás paraméter
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Miután befejeződött a docker, eredmények hasonló toohello következő kell megjelennie:
   
   ![Az alkalmazás megtekintéséhez][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
