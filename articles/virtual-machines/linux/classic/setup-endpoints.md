---
title: "végpontok a klasszikus Linux virtuális gép mentése aaaSet |} Microsoft Docs"
description: "Ismerje meg a végpontok mentése tooset rendelkező Linux virtuális gépek Azure-ban az Azure klasszikus portál tooallow kommunikációs hello Linux virtuális"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 1c959d10dd1e20228fa4a20e1cc0205c1d12f185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a><span data-ttu-id="82682-103">Hogyan tooset be Linux klasszikus virtuális gépre az Azure-végpontok</span><span class="sxs-lookup"><span data-stu-id="82682-103">How tooset up endpoints on a Linux classic virtual machine in Azure</span></span>
<span data-ttu-id="82682-104">Minden Linux virtuális gépek az Azure-ban hello klasszikus üzembe helyezési modellel létrehozott automatikusan kommunikálhatnak más virtuális gépekkel hello a magánhálózat-csatornán keresztül ugyanaz a felhőalapú szolgáltatás, vagy a virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="82682-104">All Linux virtual machines that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="82682-105">Hello Internet vagy egyéb virtuális hálózaton lévő számítógépek azonban végpontok toodirect hello bejövő hálózati forgalom tooa virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="82682-105">However, computers on hello Internet or other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="82682-106">Ez a cikk érhető el is [Windows virtuális gépek](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="82682-106">This article is also available for [Windows virtual machines](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82682-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="82682-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="82682-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="82682-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="82682-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="82682-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="82682-110">A hello **erőforrás-kezelő** üzembe helyezési modelljével végpontok használatával konfigurálhatók **hálózati biztonsági csoportokkal (NSG-k)**.</span><span class="sxs-lookup"><span data-stu-id="82682-110">In hello **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="82682-111">További információkért lásd: [portok, valamint a végpontok](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="82682-111">For more information, see [Opening ports and endpoints](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="82682-112">Hello Azure-portálon, a Linux virtuális gép létrehozásakor a végpont a Secure Shell (SSH) általában, automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="82682-112">When you create a Linux virtual machine in hello Azure portal, an endpoint for Secure Shell (SSH) is typically created for you automatically.</span></span> <span data-ttu-id="82682-113">További végpontok hello virtuális gép létrehozása során vagy azt követően szükség szerint konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="82682-113">You can configure additional endpoints while creating hello virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="82682-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82682-114">Next steps</span></span>
* <span data-ttu-id="82682-115">Hello segítségével is létrehozhat egy virtuális gép végpontjának [Azure parancssori felület](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="82682-115">You can also create a VM endpoint by using hello [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="82682-116">Futtassa a hello **azure virtuálisgép-végpont létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="82682-116">Run hello **azure vm endpoint create** command.</span></span>
* <span data-ttu-id="82682-117">Ha létrehozott egy virtuális gép hello Resource Manager üzembe helyezési modellel, használhatja az Azure parancssori felület hello erőforrás-kezelő módban túl[hálózati biztonsági csoportok létrehozása](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toocontrol forgalom toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="82682-117">If you created a virtual machine in hello Resource Manager deployment model, you can use hello Azure CLI in Resource Manager mode too[create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toocontrol traffic toohello VM.</span></span>
