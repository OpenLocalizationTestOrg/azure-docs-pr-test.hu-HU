---
title: "Klasszikus Linux virtuális gép létrehozása az Azure CLI 1.0 |} Microsoft Docs"
description: "Útmutató: az Azure CLI 1.0 klasszikus telepítési modellel rendelkező Linux virtuális gép létrehozása"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 8ddbacbbb70c0cf1a2537fab4d981a316610a4d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a><span data-ttu-id="59eb9-103">Klasszikus Linux virtuális gép létrehozása az Azure parancssori felület 1.0</span><span class="sxs-lookup"><span data-stu-id="59eb9-103">How to Create a Classic Linux VM with the Azure CLI 1.0</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="59eb9-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="59eb9-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="59eb9-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="59eb9-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="59eb9-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="59eb9-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="59eb9-107">A Resource Manager-verziójáért lásd: [Itt](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59eb9-107">For the Resource Manager version, see [here](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="59eb9-108">Ez a témakör a Linux virtuális gép (VM) létrehozása az Azure CLI 1.0 klasszikus telepítési modellel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="59eb9-108">This topic describes how to create a Linux virtual machine (VM) with the Azure CLI 1.0 using the Classic deployment model.</span></span> <span data-ttu-id="59eb9-109">A Linux-lemezkép az elérhető használjuk **képek** az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="59eb9-109">We use a Linux image from the available **IMAGES** on Azure.</span></span> <span data-ttu-id="59eb9-110">Az Azure CLI 1.0 parancsok adja meg a következő konfigurációs beállításokkal, többek között:</span><span class="sxs-lookup"><span data-stu-id="59eb9-110">The Azure CLI 1.0 commands give the following configuration choices, among others:</span></span>

* <span data-ttu-id="59eb9-111">A virtuális gép csatlakoztatása a virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="59eb9-111">Connecting the VM to a virtual network</span></span>
* <span data-ttu-id="59eb9-112">A virtuális gép hozzáadása egy meglévő felhőszolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="59eb9-112">Adding the VM to an existing cloud service</span></span>
* <span data-ttu-id="59eb9-113">A virtuális gép hozzáadása egy meglévő tárfiók</span><span class="sxs-lookup"><span data-stu-id="59eb9-113">Adding the VM to an existing storage account</span></span>
* <span data-ttu-id="59eb9-114">Adja hozzá a virtuális gép egy rendelkezésre állási csoport vagy a hely</span><span class="sxs-lookup"><span data-stu-id="59eb9-114">Adding the VM to an availability set or location</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59eb9-115">Ha azt szeretné, hogy egy virtuális hálózatot használ, így kapcsolódhat az állomásnév vagy állítsa be a létesítmények közötti kapcsolatok által közvetlenül a virtuális Gépet, győződjön meg arról, hogy a virtuális gép létrehozásakor adja meg a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="59eb9-115">If you want your VM to use a virtual network so you can connect to it directly by hostname or set up cross-premises connections, make sure you specify the virtual network when you create the VM.</span></span> <span data-ttu-id="59eb9-116">A virtuális gépek beállítható úgy, hogy csatlakozzon a virtuális hálózat csak a virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="59eb9-116">A VM can be configured to join a virtual network only when you create the VM.</span></span> <span data-ttu-id="59eb9-117">A virtuális hálózatokon, lásd: [Azure virtuális hálózat áttekintése](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span><span class="sxs-lookup"><span data-stu-id="59eb9-117">For details on virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span></span>
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a><span data-ttu-id="59eb9-118">Linux virtuális gépet a klasszikus telepítési modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="59eb9-118">How to create a Linux VM using the Classic deployment model</span></span>
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

