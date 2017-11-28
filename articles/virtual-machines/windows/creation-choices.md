---
title: "aaaDifferent módon toocreate egy Windows Azure-ban |} Microsoft Docs"
description: "Hello különböző módokon toocreate egy Windows rendszerű virtuális gép a Resource Manager sorolja fel."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 809ba8f4-b54e-43c5-bbe3-8e710c49971f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2928d4daa9b44c4d3a5083092a82c9a7f7c69fae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-windows-virtual-machine"></a><span data-ttu-id="21c73-103">A Windows rendszerű virtuális gép különböző módokon toocreate</span><span class="sxs-lookup"><span data-stu-id="21c73-103">Different ways toocreate a Windows virtual machine</span></span>

<span data-ttu-id="21c73-104">Az Azure kínál különböző módokon toocreate egy virtuális gép, mert a virtuális gépek különböző felhasználóknak és céloknak vannak kialakítva.</span><span class="sxs-lookup"><span data-stu-id="21c73-104">Azure offers different ways toocreate a virtual machine because virtual machines are suited for different users and purposes.</span></span> <span data-ttu-id="21c73-105">Ez azt jelenti, hogy szüksége toomake néhány hello virtuális géppel kapcsolatos döntések és hogyan toocreate azt.</span><span class="sxs-lookup"><span data-stu-id="21c73-105">This means that you need toomake some choices about hello virtual machine and how toocreate it.</span></span> <span data-ttu-id="21c73-106">Ez a cikk lehetővé teszi az ezeket a döntéseket összegzését, és hivatkozásokat tartalmaz tooinstructions.</span><span class="sxs-lookup"><span data-stu-id="21c73-106">This article gives you a summary of these choices and links tooinstructions.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="21c73-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="21c73-107">Azure portal</span></span>
<span data-ttu-id="21c73-108">Egy egyszerű módon tootry ki egy virtuális gépet hello Azure-portálon a használata, különösen akkor, ha nemrég kezdte az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="21c73-108">Using hello Azure portal is a simple way tootry out a virtual machine, especially if you're just starting out with Azure.</span></span> 

[<span data-ttu-id="21c73-109">Hozzon létre egy olyan virtuális géphez a Windows hello-portál használatával</span><span class="sxs-lookup"><span data-stu-id="21c73-109">Create a virtual machine running Windows using hello portal</span></span>](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a><span data-ttu-id="21c73-110">Sablon</span><span class="sxs-lookup"><span data-stu-id="21c73-110">Template</span></span>
<span data-ttu-id="21c73-111">Virtuális gépeknél szükség van a több erőforrás kombinációját (például egy rendelkezésre állási készletek és a storage-fiókok).</span><span class="sxs-lookup"><span data-stu-id="21c73-111">Virtual machines require a combination of resources (such as a availability sets and storage accounts).</span></span> <span data-ttu-id="21c73-112">Ahelyett, hogy telepíti, és az egyes erőforrások kezelése külön-külön, létrehozhat egy Azure Resource Manager-sablon, amely üzembe és tesz elérhetővé minden hello erőforrást egyetlen, koordinált műveletben.</span><span class="sxs-lookup"><span data-stu-id="21c73-112">Rather than deploying and managing each resource separately, you can create an Azure Resource Manager template that deploys and provisions all of hello resources in a single, coordinated operation.</span></span>

* [<span data-ttu-id="21c73-113">Windows rendszerű virtuális gép létrehozása egy Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="21c73-113">Create a Windows virtual machine with a Resource Manager template</span></span>](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a><span data-ttu-id="21c73-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="21c73-114">Azure PowerShell</span></span>
<span data-ttu-id="21c73-115">Ha szeretné, hogy működik-e a parancs-rendszerhéj, használhatja az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21c73-115">If you prefer working in a command shell, you can use Azure PowerShell.</span></span>

* [<span data-ttu-id="21c73-116">Windows rendszerű virtuális gép létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="21c73-116">Create a Windows VM using PowerShell</span></span>](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a><span data-ttu-id="21c73-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21c73-117">Visual Studio</span></span>
<span data-ttu-id="21c73-118">Használja a Visual Studio toobuild, kezeléséhez, és hello Azure eszközök rendelkező virtuális gépek telepítése a Visual Studio és hello Azure SDK-t.</span><span class="sxs-lookup"><span data-stu-id="21c73-118">Use Visual Studio toobuild, manage, and deploy VMs with hello Azure Tools for Visual Studio and hello Azure SDK.</span></span>

[<span data-ttu-id="21c73-119">Az Azure Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21c73-119">Azure Tools for Visual Studio</span></span>](https://www.visualstudio.com/features/azure-tools-vs)

