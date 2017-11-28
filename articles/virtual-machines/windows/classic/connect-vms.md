---
title: "Csatlakozás a Windows virtuális gépek felhőszolgáltatásban |} Microsoft Docs"
description: "Csatlakozás a Windows virtuális gépek a klasszikus üzembe helyezési modellt az Azure-felhőszolgáltatásban vagy a virtuális hálózat létrehozva."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c1cbc802-4352-4d2e-9e49-4ccbd955324b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: 0c7a21461e5bb111c4359df8e949d48382b591c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a><span data-ttu-id="5e80d-103">Csatlakozás a Windows virtuális gépek létrehozása a klasszikus üzembe helyezési modellel egy virtuális hálózat vagy a felhőalapú szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="5e80d-103">Connect Windows virtual machines created with the classic deployment model with a virtual network or cloud service</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5e80d-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5e80d-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5e80d-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="5e80d-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="5e80d-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="5e80d-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="5e80d-107">A klasszikus üzembe helyezési modellel létrehozott Windows virtuális gépek felhőszolgáltatásban kerülnek.</span><span class="sxs-lookup"><span data-stu-id="5e80d-107">Windows virtual machines created with the classic deployment model are always placed in a cloud service.</span></span> <span data-ttu-id="5e80d-108">A felhőalapú szolgáltatás úgy működik, mint egy tárolót, és egy egyedi nyilvános DNS-nevet, egy nyilvános IP-cím és az interneten keresztül a virtuális gép eléréséhez végpontok készlete.</span><span class="sxs-lookup"><span data-stu-id="5e80d-108">The cloud service acts as a container and provides a unique public DNS name, a public IP address, and a set of endpoints to access the virtual machine over the Internet.</span></span> <span data-ttu-id="5e80d-109">A felhőszolgáltatás lehet egy virtuális hálózatban, de ez nem követelmény.</span><span class="sxs-lookup"><span data-stu-id="5e80d-109">The cloud service can be in a virtual network, but that's not a requirement.</span></span> <span data-ttu-id="5e80d-110">Emellett [csatlakozás Linux virtuális gépek virtuális hálózati vagy felhőalapú szolgáltatásként](../../linux/classic/connect-vms.md).</span><span class="sxs-lookup"><span data-stu-id="5e80d-110">You can also [connect Linux virtual machines with a virtual network or cloud service](../../linux/classic/connect-vms.md).</span></span>

<span data-ttu-id="5e80d-111">Ha egy felhőalapú szolgáltatás nem része virtuális hálózatnak, hívott egy *önálló* felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5e80d-111">If a cloud service isn't in a virtual network, it's called a *standalone* cloud service.</span></span> <span data-ttu-id="5e80d-112">A virtuális gépek önálló felhőalapú szolgáltatásként kommunikálnak más virtuális gépekkel más virtuális gépeket nyilvános DNS-nevek használatával, és a forgalom halad az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="5e80d-112">The virtual machines in a standalone cloud service communicate with other virtual machines by using the other virtual machines’ public DNS names, and the traffic travels over the Internet.</span></span> <span data-ttu-id="5e80d-113">Ha egy virtuális hálózatban a virtuális gépek egy felhőalapú szolgáltatás abban, hogy a felhőalapú szolgáltatás képes kommunikálni a virtuális hálózat más virtuális gépek összes forgalom küldése az interneten keresztül nélkül.</span><span class="sxs-lookup"><span data-stu-id="5e80d-113">If a cloud service is in a virtual network, the virtual machines in that cloud service can communicate with all other virtual machines in the virtual network without sending any traffic over the Internet.</span></span>

<span data-ttu-id="5e80d-114">Ha a virtuális gépeket helyez az azonos önálló felhőalapú szolgáltatás, terheléselosztás és a rendelkezésre állási csoportok továbbra is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5e80d-114">If you place your virtual machines in the same standalone cloud service, you can still use load balancing and availability sets.</span></span> <span data-ttu-id="5e80d-115">További információkért lásd: [terheléselosztási virtuális gépek](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [virtuális gépek rendelkezésre állásának kezelése](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5e80d-115">For details, see [Load balancing virtual machines](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Manage the availability of virtual machines](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="5e80d-116">Azonban nem a virtuális gépek alhálózatok rendszerezése vagy önálló felhőalapú szolgáltatásként csatlakozni a helyi hálózatra.</span><span class="sxs-lookup"><span data-stu-id="5e80d-116">However, you can't organize the virtual machines on subnets or connect a standalone cloud service to your on-premises network.</span></span> <span data-ttu-id="5e80d-117">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="5e80d-117">Here's an example:</span></span>

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a><span data-ttu-id="5e80d-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e80d-118">Next steps</span></span>
<span data-ttu-id="5e80d-119">A virtuális gép létrehozása után már jó ötlet [hozzá adatlemezt](attach-disk.md) , a szolgáltatások és a munkafolyamatok jogosult az adatok tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="5e80d-119">After you create a virtual machine, it's a good idea to [add a data disk](attach-disk.md) so your services and workloads have a location to store data.</span></span>
