---
title: "a klasszikus Linux virtuális gép használatával aaaCreate hello Azure CLI 1.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate hello Azure CLI 1.0 használata Linux virtuális gépek hello klasszikus telepítési modell"
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
ms.openlocfilehash: 5c3c54e5d1444a79e8e609c76d04a3a621c8d03f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-classic-linux-vm-with-hello-azure-cli-10"></a><span data-ttu-id="f425a-103">Hogyan tooCreate rendelkező klasszikus Linux virtuális gépet a hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f425a-103">How tooCreate a Classic Linux VM with hello Azure CLI 1.0</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="f425a-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f425a-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f425a-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="f425a-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="f425a-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="f425a-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="f425a-107">Hello Resource Manager-verziójáért lásd: [Itt](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f425a-107">For hello Resource Manager version, see [here](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f425a-108">Ez a témakör ismerteti, hogyan toocreate egy Linux virtuális gép (VM) hello Azure CLI 1.0 használatával hello klasszikus telepítési modellt.</span><span class="sxs-lookup"><span data-stu-id="f425a-108">This topic describes how toocreate a Linux virtual machine (VM) with hello Azure CLI 1.0 using hello Classic deployment model.</span></span> <span data-ttu-id="f425a-109">A rendelkezésre álló hello Linux lemezkép használjuk **képek** az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="f425a-109">We use a Linux image from hello available **IMAGES** on Azure.</span></span> <span data-ttu-id="f425a-110">hello Azure CLI 1.0 parancsok konfigurációs beállításokkal, többek között a következő hello biztosítják:</span><span class="sxs-lookup"><span data-stu-id="f425a-110">hello Azure CLI 1.0 commands give hello following configuration choices, among others:</span></span>

* <span data-ttu-id="f425a-111">Hello VM tooa virtuális hálózati csatlakozás</span><span class="sxs-lookup"><span data-stu-id="f425a-111">Connecting hello VM tooa virtual network</span></span>
* <span data-ttu-id="f425a-112">Hello VM tooan meglévő felhőalapú szolgáltatás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f425a-112">Adding hello VM tooan existing cloud service</span></span>
* <span data-ttu-id="f425a-113">Hello VM tooan meglévő tárfiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f425a-113">Adding hello VM tooan existing storage account</span></span>
* <span data-ttu-id="f425a-114">Hello VM tooan rendelkezésre állási csoport vagy a hely hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f425a-114">Adding hello VM tooan availability set or location</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f425a-115">Ha azt szeretné, hogy a virtuális hálózat tooit állomásnév vagy állítsa be a létesítmények közötti kapcsolatok által közvetlenül is elérheti, ellenőrizze, hogy virtuális gép toouse hello virtuális hálózati meg hello virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f425a-115">If you want your VM toouse a virtual network so you can connect tooit directly by hostname or set up cross-premises connections, make sure you specify hello virtual network when you create hello VM.</span></span> <span data-ttu-id="f425a-116">A virtuális gép lehet konfigurált toojoin virtuális hálózat csak hello virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f425a-116">A VM can be configured toojoin a virtual network only when you create hello VM.</span></span> <span data-ttu-id="f425a-117">A virtuális hálózatokon, lásd: [Azure virtuális hálózat áttekintése](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span><span class="sxs-lookup"><span data-stu-id="f425a-117">For details on virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span></span>
> 
> 

## <a name="how-toocreate-a-linux-vm-using-hello-classic-deployment-model"></a><span data-ttu-id="f425a-118">Hogyan toocreate a Linux virtuális gép hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="f425a-118">How toocreate a Linux VM using hello Classic deployment model</span></span>
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

