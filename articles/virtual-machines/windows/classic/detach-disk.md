---
title: "A lemezt leválasztani a virtuális gép Windows |} Microsoft Docs"
description: "Ismerje meg a lemezt leválasztani a virtuális gép az Azure-ban a klasszikus üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b6406768-1726-41bb-9451-1fda0905cc24
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: cynthn
ms.openlocfilehash: 650c7e10150b95a6ad7cd455746f7c1d77b9b34c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-detach-a-disk-from-a-windows-virtual-machine"></a><span data-ttu-id="a2c1f-103">Lemez leválasztása windowsos virtuális gépről</span><span class="sxs-lookup"><span data-stu-id="a2c1f-103">How to detach a disk from a Windows virtual machine</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a2c1f-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a2c1f-104">Azure has two distinct deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a2c1f-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="a2c1f-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="a2c1f-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="a2c1f-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="a2c1f-107">A Resource Manager modellt használó lemez leválasztása kapcsolatos információkért lásd: [Itt](../../virtual-machines-windows-detach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a2c1f-107">For information about how to detach a disk using the Resource Manager model, see [here](../../virtual-machines-windows-detach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

[!INCLUDE [howto-detach-disk-windows-linux](../../../../includes/howto-detach-disk-windows-linux.md)]

## <a name="additional-resources"></a><span data-ttu-id="a2c1f-108">További források</span><span class="sxs-lookup"><span data-stu-id="a2c1f-108">Additional resources</span></span>
[<span data-ttu-id="a2c1f-109">Lemez és virtuális merevlemezek a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="a2c1f-109">About disks and VHDs for virtual machines</span></span>](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="a2c1f-110">Hogyan lehet adatlemezt csatolni egy Windows rendszerű virtuális gép</span><span class="sxs-lookup"><span data-stu-id="a2c1f-110">How to attach a data disk to a Windows virtual machine</span></span>](attach-disk.md)
