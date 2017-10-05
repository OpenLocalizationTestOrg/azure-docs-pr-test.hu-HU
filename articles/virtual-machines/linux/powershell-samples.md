---
title: "Virtuális gép az Azure PowerShell-példák |} Microsoft Docs"
description: "Virtuális gép az Azure PowerShell-példák"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.openlocfilehash: 799a017a241ed3d37bb95344de7d50e1be7d559c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-virtual-machine-powershell-samples"></a><span data-ttu-id="ed361-103">Azure virtuális gép PowerShell-minták</span><span class="sxs-lookup"><span data-stu-id="ed361-103">Azure Virtual Machine PowerShell samples</span></span>

<span data-ttu-id="ed361-104">A következő táblázat a PowerShell parancsfájlok mintának, hogy a Linux virtuális gépek létrehozására és kezelésére mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ed361-104">The following table includes links to PowerShell scripts samples that create and manage Linux virtual machines.</span></span>

| | |
|---|---|
|<span data-ttu-id="ed361-105">**Virtuális gépek létrehozása**</span><span class="sxs-lookup"><span data-stu-id="ed361-105">**Create virtual machines**</span></span>||
| [<span data-ttu-id="ed361-106">A teljesen konfigurált virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed361-106">Create a fully configured virtual machine</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ed361-107">Létrehoz egy erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ed361-107">Creates a resource group, virtual machine, and all related resources.</span></span>|
| [<span data-ttu-id="ed361-108">A Docker engedélyezve van a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed361-108">Create a VM with Docker enabled</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ed361-109">Létrehoz egy virtuális gépet, és konfigurálja a virtuális gép egy Docker-gazdagépként egy NGINX tároló futtatja.</span><span class="sxs-lookup"><span data-stu-id="ed361-109">Creates a virtual machine, configures this VM as a Docker host, and runs an NGINX container.</span></span> |
| [<span data-ttu-id="ed361-110">Hozzon létre egy virtuális Gépet, és futtassa a konfigurációs parancsfájl</span><span class="sxs-lookup"><span data-stu-id="ed361-110">Create a VM and run configuration script</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ed361-111">Létrehoz egy virtuális gépet, és használja az Azure egyéni parancsprogramok futtatására szolgáló bővítmény NGINX telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ed361-111">Creates a virtual machine and uses the Azure Custom Script extension to install NGINX.</span></span> |
| [<span data-ttu-id="ed361-112">Hozzon létre egy virtuális gép WordPress telepítése</span><span class="sxs-lookup"><span data-stu-id="ed361-112">Create a VM with WordPress installed</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ed361-113">Létrehoz egy virtuális gépet, és az Azure egyéni parancsprogramok futtatására szolgáló bővítmény segítségével WordPress telepítése.</span><span class="sxs-lookup"><span data-stu-id="ed361-113">Creates a virtual machine and uses the Azure Custom Script extension to install WordPress.</span></span> |
|<span data-ttu-id="ed361-114">**Virtuális gépek figyelése**</span><span class="sxs-lookup"><span data-stu-id="ed361-114">**Monitor virtual machines**</span></span>||
| [<span data-ttu-id="ed361-115">Virtuális gép és az Operations Management Suite figyelése</span><span class="sxs-lookup"><span data-stu-id="ed361-115">Monitor a VM with Operations Management Suite</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ed361-116">Létrehoz egy virtuális gépet, telepíti az Operations Management Suite-ügynököt, és regisztrálja az OMS-munkaterület virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ed361-116">Creates a virtual machine, installs the Operations Management Suite agent, and enrolls the VM in an OMS Workspace.</span></span>  |
| | |
