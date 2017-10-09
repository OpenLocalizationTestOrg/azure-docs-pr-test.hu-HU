---
title: "aaaConfigure VirtualBox Docker gazdagépet |} Microsoft Docs"
description: "Részletes útmutatás tooconfigure Docker alapértelmezett példány Docker gép és VirtualBox használatával"
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
ms.openlocfilehash: 1df2da4482444a803d05e413e019edcc57269062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a><span data-ttu-id="6556e-103">VirtualBox egy Docker-állomás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6556e-103">Configure a Docker Host with VirtualBox</span></span>
## <a name="overview"></a><span data-ttu-id="6556e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6556e-104">Overview</span></span>
<span data-ttu-id="6556e-105">Ez a cikk végigvezeti Önt egy Docker-számítógép és VirtualBox alapértelmezett Docker-példány konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6556e-105">This article guides you through configuring a default Docker instance using Docker Machine and VirtualBox.</span></span> <span data-ttu-id="6556e-106">Hello használata [Docker for Windows beta](http://beta.docker.com/), ez a konfiguráció nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="6556e-106">If you’re using hello [Docker for Windows beta](http://beta.docker.com/), this configuration is not necessary.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6556e-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6556e-107">Prerequisites</span></span>
<span data-ttu-id="6556e-108">hello következő eszközök kell toobe telepítve.</span><span class="sxs-lookup"><span data-stu-id="6556e-108">hello following tools need toobe installed.</span></span>

* [<span data-ttu-id="6556e-109">Docker eszközkészlet</span><span class="sxs-lookup"><span data-stu-id="6556e-109">Docker Toolbox</span></span>](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-hello-docker-client-with-windows-powershell"></a><span data-ttu-id="6556e-110">Hello Docker-ügyfél konfigurálása a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="6556e-110">Configuring hello Docker client with Windows PowerShell</span></span>
<span data-ttu-id="6556e-111">tooconfigure egy Docker-ügyfelet, egyszerűen nyissa meg a Windows Powershellt, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6556e-111">tooconfigure a Docker client, simply open Windows PowerShell, and perform hello following steps:</span></span>

1. <span data-ttu-id="6556e-112">Hozzon létre alapértelmezett docker-állomás példányt.</span><span class="sxs-lookup"><span data-stu-id="6556e-112">Create a default docker host instance.</span></span>
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. <span data-ttu-id="6556e-113">Győződjön meg arról hello alapértelmezett példány, konfigurálni és futtatni.</span><span class="sxs-lookup"><span data-stu-id="6556e-113">Verify hello default instance is configured and running.</span></span> <span data-ttu-id="6556e-114">(Látnia kell egy futtató a "default" nevű példány.</span><span class="sxs-lookup"><span data-stu-id="6556e-114">(You should see an instance named \`default' running.</span></span>
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker-gép ls kimeneti][0]
3. <span data-ttu-id="6556e-116">Alapértelmezett hello aktuális gazdagépként, és a rendszerhéj konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6556e-116">Set default as hello current host, and configure your shell.</span></span>
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. <span data-ttu-id="6556e-117">Hello active Docker-tárolók megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6556e-117">Display hello active Docker containers.</span></span> <span data-ttu-id="6556e-118">hello lista üresnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6556e-118">hello list should be empty.</span></span>
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps kimeneti][1]

> [!NOTE]
> <span data-ttu-id="6556e-120">Minden alkalommal, amikor a fejlesztői számítógépén, indítsa újra kell toorestart az helyi docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="6556e-120">Each time you reboot your development machine, you’ll need toorestart your local docker host.</span></span>
> <span data-ttu-id="6556e-121">toodo, ez a probléma hello a következő parancsot a parancssorba: `docker-machine start default`.</span><span class="sxs-lookup"><span data-stu-id="6556e-121">toodo this, issue hello following command at a command prompt: `docker-machine start default`.</span></span>
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
