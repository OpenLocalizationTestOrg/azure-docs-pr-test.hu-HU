---
title: a Windows Azure CLI aaaUsing hello |} Microsoft Docs
description: "A Windows hello Azure parancssori felület használatával"
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
ms.openlocfilehash: edca4a2701a7dfc0fc54df5649e8a5e7afc95490
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-on-windows"></a><span data-ttu-id="b88e3-103">A Windows hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="b88e3-103">Using hello Azure CLI on Windows</span></span>

<span data-ttu-id="b88e3-104">hello Azure parancssori felület (CLI) biztosít egy parancssor, és a parancsfájl-kezelési környezet létrehozására és Azure-erőforrások kezelésére.</span><span class="sxs-lookup"><span data-stu-id="b88e3-104">hello Azure Command Line Interface (CLI) provides a command line and scripting environment for creating and managing Azure resources.</span></span> <span data-ttu-id="b88e3-105">hello Azure CLI macOS, a Linux és a Windows operációs rendszereken érhető el.</span><span class="sxs-lookup"><span data-stu-id="b88e3-105">hello Azure CLI is available for macOS, Linux, and Windows operating systems.</span></span> <span data-ttu-id="b88e3-106">Ezek az operációs rendszerek között hello parancssori felület parancsai azonosak, azonban az operációs rendszer adott parancsfájlkezelő szintaxis eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="b88e3-106">Across these operating systems, hello CLI commands are identical, however operating system specific scripting syntax can differ.</span></span>

<span data-ttu-id="b88e3-107">Ez a dokumentum részletek hello módon hello Azure parancssori felület telepítve, és az egyes Windows és a részletek szintaktikai szempontok futtatnak.</span><span class="sxs-lookup"><span data-stu-id="b88e3-107">This document details hello ways that hello Azure CLI can be installed and run on Windows and details syntactical considerations for each.</span></span> <span data-ttu-id="b88e3-108">A részletes Azure CLI dokumentációját lásd [Azure CLI dokumentáció]( https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b88e3-108">For in-depth Azure CLI documentation see, [Azure CLI documentation]( https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="windows-subsystem-for-linux"></a><span data-ttu-id="b88e3-109">Linux rendszerhez készült Windows alrendszer</span><span class="sxs-lookup"><span data-stu-id="b88e3-109">Windows Subsystem for Linux</span></span>

<span data-ttu-id="b88e3-110">hello Windows alrendszer Linux (WSL) Ubuntu Linux környezetet biztosít a Windows 10 évforduló és a későbbi kiadások.</span><span class="sxs-lookup"><span data-stu-id="b88e3-110">hello Windows Subsystem for Linux (WSL) provides an Ubuntu Linux environment on Windows 10 Anniversary and later editions.</span></span> <span data-ttu-id="b88e3-111">Ha engedélyezve van, WSL natív Bash élményt, létrehozásának és az Azure CLI-parancsfájlok futtatásakor, amellyel nyújt.</span><span class="sxs-lookup"><span data-stu-id="b88e3-111">When enabled, WSL provides a native Bash experience, which can be used for creating and running Azure CLI scripts.</span></span> <span data-ttu-id="b88e3-112">WSL natív Bash élményt biztosít, mert az Azure parancssori felület parancsfájlok megoszthatók macOS, a Linux és a Windows között módosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="b88e3-112">Because WSL provides a native Bash experience, Azure CLI scripts can be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="b88e3-113">toouse hello WSL, az Azure CLI hello következő végezze el.</span><span class="sxs-lookup"><span data-stu-id="b88e3-113">toouse hello Azure CLI in WSL, complete hello following.</span></span>

|<span data-ttu-id="b88e3-114">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="b88e3-114">Task</span></span> | <span data-ttu-id="b88e3-115">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="b88e3-115">Instructions</span></span> |
|---|---|
| <span data-ttu-id="b88e3-116">WSL engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b88e3-116">Enable WSL</span></span> | [<span data-ttu-id="b88e3-117">Telepítse a WSL dokumentáció</span><span class="sxs-lookup"><span data-stu-id="b88e3-117">Install WSL documentation </span></span>](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| <span data-ttu-id="b88e3-118">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="b88e3-118">Install hello Azure CLI</span></span> |[<span data-ttu-id="b88e3-119">WSL/Ubuntu 14.04 hello parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="b88e3-119">Install hello CLI on WSL/Ubuntu 14.04</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a><span data-ttu-id="b88e3-120">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b88e3-120">PowerShell</span></span>

<span data-ttu-id="b88e3-121">hello Azure CLI Windows natív módon futtatható.</span><span class="sxs-lookup"><span data-stu-id="b88e3-121">hello Azure CLI can be run natively in Windows.</span></span> <span data-ttu-id="b88e3-122">Ebben a konfigurációban hello Azure CLI csomag hello Windows operációs rendszerre van telepítve, és PowerShell parancsok futtatását.</span><span class="sxs-lookup"><span data-stu-id="b88e3-122">In this configuration, hello Azure CLI package is installed on hello Windows operating system, and commands can be run from PowerShell.</span></span> <span data-ttu-id="b88e3-123">Ebben a konfigurációban az Azure parancssori felület parancsaiban és parancsfájljaiban futtatható bármely, a Windows támogatott verzióját azonban platform adott parancsfájlkezelő szintaxis kötelező.</span><span class="sxs-lookup"><span data-stu-id="b88e3-123">In this configuration, Azure CLI commands and scripts can be run on any supported version of Windows, however platform specific scripting syntax is required.</span></span> <span data-ttu-id="b88e3-124">Ebből kifolyólag parancsfájlok nem feltétlenül lehet megosztani macOS, a Linux és a Windows között módosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="b88e3-124">Because of this, scripts cannot necessarily be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="b88e3-125">toouse hello Azure CLI Windows, a jelen útmutatást hello csomag telepítésének [telepítés hello CLI Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span><span class="sxs-lookup"><span data-stu-id="b88e3-125">toouse hello Azure CLI on Windows, install hello package using these instructions, [Install hello CLI on Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span></span>

## <a name="docker-image"></a><span data-ttu-id="b88e3-126">Docker kép</span><span class="sxs-lookup"><span data-stu-id="b88e3-126">Docker Image</span></span>

<span data-ttu-id="b88e3-127">Docker Windows használatakor a Docker-lemezkép indíthatók el, amely tartalmazza az Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="b88e3-127">When using Docker for Windows, a Docker image can be started that includes hello Azure CLI.</span></span> <span data-ttu-id="b88e3-128">Ez a rendszerkép kijelentkezés Linux alapul, és egy natív Bash élmény tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b88e3-128">This image is based off of Linux, and includes a native Bash experience.</span></span>  <span data-ttu-id="b88e3-129">Docker a Windows és a hello Azure CLI képet, a parancsfájlok toobe macOS, a Linux és a Windows között megosztott használatakor.</span><span class="sxs-lookup"><span data-stu-id="b88e3-129">When using Docker for Windows and hello Azure CLI image, scripts toobe shared between macOS, Linux, and Windows.</span></span> 

<span data-ttu-id="b88e3-130">toouse hello Azure parancssori felület a Docker Windows, győződjön meg arról, hogy Docker for Windows fut, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="b88e3-130">toouse hello Azure CLI on Docker for Windows, ensure that Docker for Windows is running and run hello following command.</span></span>

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

<span data-ttu-id="b88e3-131">Ezt követően a Bash munkamenet indul el, amely előre feltölti hello Azure CLI-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="b88e3-131">Once completed, a Bash session will start that is preloaded with hello Azure CLI tools.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b88e3-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b88e3-132">Next Steps</span></span>

[<span data-ttu-id="b88e3-133">Az Azure virtuális gépek CLI minta</span><span class="sxs-lookup"><span data-stu-id="b88e3-133">CLI sample for Azure virtual machines</span></span>](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="b88e3-134">Az Azure Web Apps CLI-minták</span><span class="sxs-lookup"><span data-stu-id="b88e3-134">CLI samples for Azure Web Apps</span></span>](../../app-service-web/app-service-cli-samples.md)

[<span data-ttu-id="b88e3-135">Az Azure SQL CLI minták</span><span class="sxs-lookup"><span data-stu-id="b88e3-135">CLI samples for Azure SQL</span></span>](../../sql-database/sql-database-cli-samples.md)
