---
title: "végpontok a klasszikus windowsos virtuális gép mentése aaaSet |} Microsoft Docs"
description: "Ismerje meg, tooset végpont mentése a Windows virtuális gépek a hello Azure klasszikus portál tooallow kommunikáció a Windows Azure virtuális gép."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: e817076f16d3a245a8d19add7b2f2cf5e3baa17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a><span data-ttu-id="5b3eb-103">Hogyan tooset be a klasszikus Windows rendszerű virtuális gép az Azure-végpontok</span><span class="sxs-lookup"><span data-stu-id="5b3eb-103">How tooset up endpoints on a classic Windows virtual machine in Azure</span></span>
<span data-ttu-id="5b3eb-104">Az Azure-ban hello klasszikus üzembe helyezési modellel létrehozott virtuális gépek automatikusan kommunikálhatnak más virtuális gépekkel a magánhálózat-csatornán keresztül az összes Windows hello ugyanazt a felhőalapú szolgáltatás, vagy a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-104">All Windows virtual machines that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="5b3eb-105">Hello Internet vagy egyéb virtuális hálózaton lévő számítógépek azonban végpontok toodirect hello bejövő hálózati forgalom tooa virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-105">However, computers on hello Internet or other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="5b3eb-106">Ez a cikk érhető el is [Linux virtuális gépek](../../linux/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="5b3eb-106">This article is also available for [Linux virtual machines](../../linux/classic/setup-endpoints.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b3eb-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5b3eb-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5b3eb-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="5b3eb-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="5b3eb-110">A hello **erőforrás-kezelő** üzembe helyezési modelljével végpontok használatával konfigurálhatók **hálózati biztonsági csoportokkal (NSG-k)**.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-110">In hello **Resource Manager** deployment model, endpoints are configured using **Network Security Groups (NSGs)**.</span></span> <span data-ttu-id="5b3eb-111">További információkért lásd: [engedélyezése külső hozzáférés tooyour VM használatával hello Azure-portálon](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5b3eb-111">For more information, see [Allow external access tooyour VM using hello Azure portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="5b3eb-112">Hello Azure-portálon, a Windows rendszerű virtuális gép létrehozásakor közös végpontok, például a távoli asztal és a Windows PowerShell távoli eljáráshívás általában meg automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-112">When you create a Windows virtual machine in hello Azure portal, common endpoints like those for Remote Desktop and Windows PowerShell Remoting are typically created for you automatically.</span></span> <span data-ttu-id="5b3eb-113">További végpontok hello virtuális gép létrehozása során vagy azt követően szükség szerint konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-113">You can configure additional endpoints while creating hello virtual machine or afterwards as needed.</span></span>

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a><span data-ttu-id="5b3eb-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b3eb-114">Next steps</span></span>
* <span data-ttu-id="5b3eb-115">Lásd az Azure PowerShell-parancsmag tooset be egy virtuális gép végpontjának toouse [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b3eb-115">toouse an Azure PowerShell cmdlet tooset up a VM endpoint, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).</span></span>
* <span data-ttu-id="5b3eb-116">Lásd az Azure PowerShell-parancsmag toomanage egy végponti ACL toouse [kezelése hozzáférés listák (ACL) végpontok PowerShell használatával](../../../virtual-network/virtual-networks-acl-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5b3eb-116">toouse an Azure PowerShell cmdlet toomanage an ACL on an endpoint, see [Managing access control lists (ACLs) for endpoints by using PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).</span></span>
* <span data-ttu-id="5b3eb-117">Ha egy virtuális gép hello Resource Manager üzembe helyezési modellel létrehozott, használhatja az Azure PowerShell túl[hálózati biztonsági csoportok létrehozása](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toocontrol forgalom toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5b3eb-117">If you created a virtual machine in hello Resource Manager deployment model, you can use Azure PowerShell too[create network security groups](../../../virtual-network/virtual-networks-create-nsg-arm-ps.md) toocontrol traffic toohello VM.</span></span>
