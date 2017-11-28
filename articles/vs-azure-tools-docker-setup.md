---
title: "Állítson be egy Docker-állomás VirtualBox |} Microsoft Docs"
description: "Részletes útmutatás a Docker gép és VirtualBox alapértelmezett Docker példányok konfigurálása"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: e9465afb560a73d74f853c19094b3ee75b8c470c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a><span data-ttu-id="e6e7d-103">VirtualBox egy Docker-állomás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6e7d-103">Configure a Docker Host with VirtualBox</span></span>
## <a name="overview"></a><span data-ttu-id="e6e7d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e6e7d-104">Overview</span></span>
<span data-ttu-id="e6e7d-105">Ez a cikk végigvezeti Önt egy Docker-számítógép és VirtualBox alapértelmezett Docker-példány konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-105">This article guides you through configuring a default Docker instance using Docker Machine and VirtualBox.</span></span> <span data-ttu-id="e6e7d-106">Használata esetén a [Docker for Windows beta](http://beta.docker.com/), ez a konfiguráció nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-106">If you’re using the [Docker for Windows beta](http://beta.docker.com/), this configuration is not necessary.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6e7d-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e6e7d-107">Prerequisites</span></span>
<span data-ttu-id="e6e7d-108">A következő eszközök telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-108">The following tools need to be installed.</span></span>

* [<span data-ttu-id="e6e7d-109">Docker eszközkészlet</span><span class="sxs-lookup"><span data-stu-id="e6e7d-109">Docker Toolbox</span></span>](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a><span data-ttu-id="e6e7d-110">A Docker-ügyfél konfigurálása a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="e6e7d-110">Configuring the Docker client with Windows PowerShell</span></span>
<span data-ttu-id="e6e7d-111">Egy Docker-ügyfél konfigurálásához, egyszerűen nyissa meg a Windows Powershellt, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e6e7d-111">To configure a Docker client, simply open Windows PowerShell, and perform the following steps:</span></span>

1. <span data-ttu-id="e6e7d-112">Hozzon létre alapértelmezett docker-állomás példányt.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-112">Create a default docker host instance.</span></span>
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. <span data-ttu-id="e6e7d-113">Ellenőrizze, hogy az alapértelmezett példány, konfigurálni és futtatni.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-113">Verify the default instance is configured and running.</span></span> <span data-ttu-id="e6e7d-114">(Látnia kell egy futtató a "default" nevű példány.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-114">(You should see an instance named \`default' running.</span></span>
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker-gép ls kimeneti][0]
3. <span data-ttu-id="e6e7d-116">Alapértelmezett állítja be az aktuális állomás, és konfigurálja a rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-116">Set default as the current host, and configure your shell.</span></span>
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. <span data-ttu-id="e6e7d-117">Megjeleníti az aktív Docker-tárolókat.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-117">Display the active Docker containers.</span></span> <span data-ttu-id="e6e7d-118">Lehet, hogy a lista üres.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-118">The list should be empty.</span></span>
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps kimeneti][1]

> [!NOTE]
> <span data-ttu-id="e6e7d-120">Minden alkalommal, amikor a fejlesztői számítógépén, indítsa újra lesz szüksége a helyi docker host újraindítására.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-120">Each time you reboot your development machine, you’ll need to restart your local docker host.</span></span>
> <span data-ttu-id="e6e7d-121">Ehhez adja ki a következő parancsot a parancssorba: `docker-machine start default`.</span><span class="sxs-lookup"><span data-stu-id="e6e7d-121">To do this, issue the following command at a command prompt: `docker-machine start default`.</span></span>
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
