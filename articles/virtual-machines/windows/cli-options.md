---
title: "Az Azure parancssori felület használata Windows rendszeren |} Microsoft Docs"
description: "A Windows az Azure parancssori felület használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: be02ad0d7752cb08f092deeb5a86dcd126403237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-azure-cli-on-windows"></a><span data-ttu-id="4609d-103">A Windows az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="4609d-103">Using the Azure CLI on Windows</span></span>

<span data-ttu-id="4609d-104">Az Azure parancssori felület (CLI) biztosít egy parancssor, és a parancsfájl-kezelési környezet létrehozására és Azure-erőforrások kezelésére.</span><span class="sxs-lookup"><span data-stu-id="4609d-104">The Azure Command Line Interface (CLI) provides a command line and scripting environment for creating and managing Azure resources.</span></span> <span data-ttu-id="4609d-105">Az Azure parancssori felület macOS, a Linux és a Windows operációs rendszereken érhető el.</span><span class="sxs-lookup"><span data-stu-id="4609d-105">The Azure CLI is available for macOS, Linux, and Windows operating systems.</span></span> <span data-ttu-id="4609d-106">Ezek az operációs rendszerek között a parancssori felület parancsai azonosak, azonban az operációs rendszer adott parancsfájlkezelő szintaxis eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="4609d-106">Across these operating systems, the CLI commands are identical, however operating system specific scripting syntax can differ.</span></span>

<span data-ttu-id="4609d-107">Ez a dokumentum részletesen, hogy az Azure parancssori felület telepítve-e, és hogy az egyes Windows és a részletek szintaktikai szempontok futtatnak.</span><span class="sxs-lookup"><span data-stu-id="4609d-107">This document details the ways that the Azure CLI can be installed and run on Windows and details syntactical considerations for each.</span></span> <span data-ttu-id="4609d-108">A részletes Azure CLI dokumentációját lásd [Azure CLI dokumentáció]( https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4609d-108">For in-depth Azure CLI documentation see, [Azure CLI documentation]( https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="windows-subsystem-for-linux"></a><span data-ttu-id="4609d-109">Linux rendszerhez készült Windows alrendszer</span><span class="sxs-lookup"><span data-stu-id="4609d-109">Windows Subsystem for Linux</span></span>

<span data-ttu-id="4609d-110">A Windows alrendszer Linux (WSL) Ubuntu Linux környezetet biztosít a Windows 10 évforduló és a későbbi kiadások.</span><span class="sxs-lookup"><span data-stu-id="4609d-110">The Windows Subsystem for Linux (WSL) provides an Ubuntu Linux environment on Windows 10 Anniversary and later editions.</span></span> <span data-ttu-id="4609d-111">Ha engedélyezve van, WSL natív Bash élményt, létrehozásának és az Azure CLI-parancsfájlok futtatásakor, amellyel nyújt.</span><span class="sxs-lookup"><span data-stu-id="4609d-111">When enabled, WSL provides a native Bash experience, which can be used for creating and running Azure CLI scripts.</span></span> <span data-ttu-id="4609d-112">WSL natív Bash élményt biztosít, mert az Azure parancssori felület parancsfájlok megoszthatók macOS, a Linux és a Windows között módosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="4609d-112">Because WSL provides a native Bash experience, Azure CLI scripts can be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="4609d-113">Ha szeretne használni az Azure parancssori felület WSL, az alábbi lépések elvégzésével.</span><span class="sxs-lookup"><span data-stu-id="4609d-113">To use the Azure CLI in WSL, complete the following.</span></span>

|<span data-ttu-id="4609d-114">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="4609d-114">Task</span></span> | <span data-ttu-id="4609d-115">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="4609d-115">Instructions</span></span> |
|---|---|
| <span data-ttu-id="4609d-116">WSL engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4609d-116">Enable WSL</span></span> | [<span data-ttu-id="4609d-117">Telepítse a WSL dokumentáció</span><span class="sxs-lookup"><span data-stu-id="4609d-117">Install WSL documentation </span></span>](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| <span data-ttu-id="4609d-118">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="4609d-118">Install the Azure CLI</span></span> |[<span data-ttu-id="4609d-119">A parancssori felület telepítése WSL/Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="4609d-119">Install the CLI on WSL/Ubuntu 14.04</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a><span data-ttu-id="4609d-120">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4609d-120">PowerShell</span></span>

<span data-ttu-id="4609d-121">Az Azure parancssori felület a Windows natív módon futtatható.</span><span class="sxs-lookup"><span data-stu-id="4609d-121">The Azure CLI can be run natively in Windows.</span></span> <span data-ttu-id="4609d-122">Ebben a konfigurációban az Azure CLI csomag telepítve van a Windows operációs rendszer, és PowerShell parancsok futtatását.</span><span class="sxs-lookup"><span data-stu-id="4609d-122">In this configuration, the Azure CLI package is installed on the Windows operating system, and commands can be run from PowerShell.</span></span> <span data-ttu-id="4609d-123">Ebben a konfigurációban az Azure parancssori felület parancsaiban és parancsfájljaiban futtatható bármely, a Windows támogatott verzióját azonban platform adott parancsfájlkezelő szintaxis kötelező.</span><span class="sxs-lookup"><span data-stu-id="4609d-123">In this configuration, Azure CLI commands and scripts can be run on any supported version of Windows, however platform specific scripting syntax is required.</span></span> <span data-ttu-id="4609d-124">Ebből kifolyólag parancsfájlok nem feltétlenül lehet megosztani macOS, a Linux és a Windows között módosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="4609d-124">Because of this, scripts cannot necessarily be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="4609d-125">Szeretne használni az Azure CLI Windows, telepítse a csomagot ezeket az utasításokat a [a parancssori felület telepítése Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span><span class="sxs-lookup"><span data-stu-id="4609d-125">To use the Azure CLI on Windows, install the package using these instructions, [Install the CLI on Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span></span>

## <a name="docker-image"></a><span data-ttu-id="4609d-126">Docker kép</span><span class="sxs-lookup"><span data-stu-id="4609d-126">Docker Image</span></span>

<span data-ttu-id="4609d-127">Docker Windows használatakor a Docker-lemezkép indíthatók el, amely tartalmazza az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="4609d-127">When using Docker for Windows, a Docker image can be started that includes the Azure CLI.</span></span> <span data-ttu-id="4609d-128">Ez a rendszerkép kijelentkezés Linux alapul, és egy natív Bash élmény tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4609d-128">This image is based off of Linux, and includes a native Bash experience.</span></span>  <span data-ttu-id="4609d-129">Docker a Windows és az Azure CLI lemezképének macOS, a Linux és a Windows között megosztani kívánt parancsfájlok használata.</span><span class="sxs-lookup"><span data-stu-id="4609d-129">When using Docker for Windows and the Azure CLI image, scripts to be shared between macOS, Linux, and Windows.</span></span> 

<span data-ttu-id="4609d-130">A Docker a Windows Azure CLI használata, ellenőrizze, hogy fut-e a Windows Docker, és futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="4609d-130">To use the Azure CLI on Docker for Windows, ensure that Docker for Windows is running and run the following command.</span></span>

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

<span data-ttu-id="4609d-131">Ezt követően a Bash munkamenet indul el, amely előre feltölti az Azure CLI-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="4609d-131">Once completed, a Bash session will start that is preloaded with the Azure CLI tools.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4609d-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4609d-132">Next Steps</span></span>

[<span data-ttu-id="4609d-133">Az Azure virtuális gépek CLI minta</span><span class="sxs-lookup"><span data-stu-id="4609d-133">CLI sample for Azure virtual machines</span></span>](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="4609d-134">Az Azure Web Apps CLI-minták</span><span class="sxs-lookup"><span data-stu-id="4609d-134">CLI samples for Azure Web Apps</span></span>](../../app-service-web/app-service-cli-samples.md)

[<span data-ttu-id="4609d-135">Az Azure SQL CLI minták</span><span class="sxs-lookup"><span data-stu-id="4609d-135">CLI samples for Azure SQL</span></span>](../../sql-database/sql-database-cli-samples.md)
