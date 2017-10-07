---
title: "parancssori felület minták Windows aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 1eef61a24d14897dd0a88a3f467854cc21b1938d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a><span data-ttu-id="c010b-103">Az Azure CLI minták a Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="c010b-103">Azure CLI Samples for Windows virtual machines</span></span>

<span data-ttu-id="c010b-104">hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure parancssori felület használatával készített Windows virtuális gépek telepítése.</span><span class="sxs-lookup"><span data-stu-id="c010b-104">hello following table includes links toobash scripts built using hello Azure CLI that deploy Windows virtual machines.</span></span>

| | |
|---|---|
|<span data-ttu-id="c010b-105">**Virtuális gépek létrehozása**</span><span class="sxs-lookup"><span data-stu-id="c010b-105">**Create virtual machines**</span></span>||
| [<span data-ttu-id="c010b-106">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c010b-106">Create a virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-107">A Windows rendszerű virtuális gép minimális konfigurációs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c010b-107">Creates a Windows virtual machine with minimal configuration.</span></span> |
| [<span data-ttu-id="c010b-108">A teljesen konfigurált virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c010b-108">Create a fully configured virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-109">Létrehoz egy erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="c010b-109">Creates a resource group, virtual machine, and all related resources.</span></span>|
| [<span data-ttu-id="c010b-110">Magas rendelkezésre állású virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c010b-110">Create highly available virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-111">Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c010b-111">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="c010b-112">Hozzon létre egy virtuális Gépet, és futtassa a konfigurációs parancsfájl</span><span class="sxs-lookup"><span data-stu-id="c010b-112">Create a VM and run configuration script</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-113">Létrehoz egy virtuális gépet, és hello Azure Custom Script bővítmény tooinstall IIS használ.</span><span class="sxs-lookup"><span data-stu-id="c010b-113">Creates a virtual machine and uses hello Azure Custom Script extension tooinstall IIS.</span></span> |
| [<span data-ttu-id="c010b-114">Hozzon létre egy virtuális Gépet, és futtassa a DSC-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="c010b-114">Create a VM and run DSC configuration</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-115">Létrehoz egy virtuális gépet, és hello Azure kívánt állapot konfigurációs szolgáltatása (DSC) bővítmény tooinstall IIS használ.</span><span class="sxs-lookup"><span data-stu-id="c010b-115">Creates a virtual machine and uses hello Azure Desired State Configuration (DSC) extension tooinstall IIS.</span></span> |
|<span data-ttu-id="c010b-116">**Virtuális gépek hálózati**</span><span class="sxs-lookup"><span data-stu-id="c010b-116">**Network virtual machines**</span></span>||
| [<span data-ttu-id="c010b-117">Virtuális gépek közötti hálózati forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="c010b-117">Secure network traffic between virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-118">Két virtuális gép minden kapcsolódó erőforrások és egy belső és külső hálózati biztonsági csoportokkal (NSG) hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c010b-118">Creates two virtual machines, all related resources, and an internal and external network security groups (NSG).</span></span> |
|<span data-ttu-id="c010b-119">**Virtuális gépek védelme**</span><span class="sxs-lookup"><span data-stu-id="c010b-119">**Secure virtual machines**</span></span>||
| [<span data-ttu-id="c010b-120">A Virtuálisgép- és adatlemezek titkosítása</span><span class="sxs-lookup"><span data-stu-id="c010b-120">Encrypt a VM and data disks</span></span>](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-121">Létrehoz egy Azure Key Vault, a titkosítási kulcs és a szolgáltatás egyszerű, majd a virtuális gépek titkosítja.</span><span class="sxs-lookup"><span data-stu-id="c010b-121">Creates an Azure Key Vault, encryption key, and service principal, then encrypts a VM.</span></span> |
|<span data-ttu-id="c010b-122">**Virtuális gépek figyelése**</span><span class="sxs-lookup"><span data-stu-id="c010b-122">**Monitor virtual machines**</span></span>||
| [<span data-ttu-id="c010b-123">Virtuális gép és az Operations Management Suite figyelése</span><span class="sxs-lookup"><span data-stu-id="c010b-123">Monitor a VM with Operations Management Suite</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="c010b-124">Létrehoz egy virtuális gépet, hello Operations Management Suite ügynököt telepíti és regisztrálja az OMS-munkaterület VM hello.</span><span class="sxs-lookup"><span data-stu-id="c010b-124">Creates a virtual machine, installs hello Operations Management Suite agent, and enrolls hello VM in an OMS Workspace.</span></span>  |
| | |
