---
title: "Az Azure CLI Windows minták |} Microsoft Docs"
description: "Az Azure CLI Windows – minták"
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
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f4b2e8a5583855df7472af3fbef01ac641caf6bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a><span data-ttu-id="67828-103">Az Azure CLI minták a Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="67828-103">Azure CLI Samples for Windows virtual machines</span></span>

<span data-ttu-id="67828-104">A következő táblázat az Azure parancssori felület használatával készített olyan parancsfájlok, amelyek Windows virtuális gépek telepítése bash mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="67828-104">The following table includes links to bash scripts built using the Azure CLI that deploy Windows virtual machines.</span></span>

| | |
|---|---|
|<span data-ttu-id="67828-105">**Virtuális gépek létrehozása**</span><span class="sxs-lookup"><span data-stu-id="67828-105">**Create virtual machines**</span></span>||
| [<span data-ttu-id="67828-106">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="67828-106">Create a virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-107">A Windows rendszerű virtuális gép minimális konfigurációs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="67828-107">Creates a Windows virtual machine with minimal configuration.</span></span> |
| [<span data-ttu-id="67828-108">A teljesen konfigurált virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="67828-108">Create a fully configured virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-109">Létrehoz egy erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="67828-109">Creates a resource group, virtual machine, and all related resources.</span></span>|
| [<span data-ttu-id="67828-110">Magas rendelkezésre állású virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="67828-110">Create highly available virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-111">Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="67828-111">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="67828-112">Hozzon létre egy virtuális Gépet, és futtassa a konfigurációs parancsfájl</span><span class="sxs-lookup"><span data-stu-id="67828-112">Create a VM and run configuration script</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-113">Létrehoz egy virtuális gépet, és az Azure egyéni parancsprogramok futtatására szolgáló bővítmény segítségével telepítse az IIS szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="67828-113">Creates a virtual machine and uses the Azure Custom Script extension to install IIS.</span></span> |
| [<span data-ttu-id="67828-114">Hozzon létre egy virtuális Gépet, és futtassa a DSC-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="67828-114">Create a VM and run DSC configuration</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-115">Létrehoz egy virtuális gépet, és az Azure kívánt állapot konfigurációs szolgáltatása (DSC) bővítmény segítségével telepítse az IIS.</span><span class="sxs-lookup"><span data-stu-id="67828-115">Creates a virtual machine and uses the Azure Desired State Configuration (DSC) extension to install IIS.</span></span> |
|<span data-ttu-id="67828-116">**Virtuális gépek hálózati**</span><span class="sxs-lookup"><span data-stu-id="67828-116">**Network virtual machines**</span></span>||
| [<span data-ttu-id="67828-117">Virtuális gépek közötti hálózati forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="67828-117">Secure network traffic between virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-118">Két virtuális gép minden kapcsolódó erőforrások és egy belső és külső hálózati biztonsági csoportokkal (NSG) hoz létre.</span><span class="sxs-lookup"><span data-stu-id="67828-118">Creates two virtual machines, all related resources, and an internal and external network security groups (NSG).</span></span> |
|<span data-ttu-id="67828-119">**Virtuális gépek védelme**</span><span class="sxs-lookup"><span data-stu-id="67828-119">**Secure virtual machines**</span></span>||
| [<span data-ttu-id="67828-120">A Virtuálisgép- és adatlemezek titkosítása</span><span class="sxs-lookup"><span data-stu-id="67828-120">Encrypt a VM and data disks</span></span>](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-121">Létrehoz egy Azure Key Vault, a titkosítási kulcs és a szolgáltatás egyszerű, majd a virtuális gépek titkosítja.</span><span class="sxs-lookup"><span data-stu-id="67828-121">Creates an Azure Key Vault, encryption key, and service principal, then encrypts a VM.</span></span> |
|<span data-ttu-id="67828-122">**Virtuális gépek figyelése**</span><span class="sxs-lookup"><span data-stu-id="67828-122">**Monitor virtual machines**</span></span>||
| [<span data-ttu-id="67828-123">Virtuális gép és az Operations Management Suite figyelése</span><span class="sxs-lookup"><span data-stu-id="67828-123">Monitor a VM with Operations Management Suite</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="67828-124">Létrehoz egy virtuális gépet, telepíti az Operations Management Suite-ügynököt, és regisztrálja az OMS-munkaterület virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="67828-124">Creates a virtual machine, installs the Operations Management Suite agent, and enrolls the VM in an OMS Workspace.</span></span>  |
| | |
