---
title: "a virtuális gép (klasszikus) aaaMove vagy Felhőszolgáltatások szerepkör példány tooa másik alhálózat - Azure PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove (klasszikus) virtuális gépek és Felhőszolgáltatások szerepkör-példányok tooa másik alhálózat PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8d2de56f42a91be4a665414ea9641ffd46588fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-tooa-different-subnet-using-powershell"></a><span data-ttu-id="96fd0-103">Helyezze át a virtuális gép (klasszikus) vagy Felhőszolgáltatások szerepkör példány tooa másik alhálózat PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="96fd0-103">Move a VM (Classic) or Cloud Services role instance tooa different subnet using PowerShell</span></span>
<span data-ttu-id="96fd0-104">Használható PowerShell toomove a virtuális gépek (klasszikus) az egyik alhálózat tooanother hello ugyanazt a virtuális hálózatot (VNet).</span><span class="sxs-lookup"><span data-stu-id="96fd0-104">You can use PowerShell toomove your VMs (Classic) from one subnet tooanother in hello same virtual network (VNet).</span></span> <span data-ttu-id="96fd0-105">Szerepkörpéldányokat áthelyezhető hello CSCFG-fájl szerkesztése, hanem a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="96fd0-105">Role instances can be moved by editing hello CSCFG file, rather than using PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="96fd0-106">Ez a cikk ismerteti, hogyan toomove virtuális gépek telepítve hello klasszikus telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="96fd0-106">This article explains how toomove VMs deployed through hello classic deployment model only.</span></span>
> 
> 

<span data-ttu-id="96fd0-107">Miért helyezze át a virtuális gépek tooanother alhálózati?</span><span class="sxs-lookup"><span data-stu-id="96fd0-107">Why move VMs tooanother subnet?</span></span> <span data-ttu-id="96fd0-108">Alhálózati áttelepítés akkor hasznos, ha hello régebbi alhálózati túl kicsi, és nem bonthatók ki lejáró futtató virtuális gépek alhálózat tooexisting.</span><span class="sxs-lookup"><span data-stu-id="96fd0-108">Subnet migration is useful when hello older subnet is too small and cannot be expanded due tooexisting running VMs in that subnet.</span></span> <span data-ttu-id="96fd0-109">Ebben az esetben hozzon létre egy új, nagyobb alhálózatot, és át hello virtuális gépek toohello új alhálózatot, majd áttelepítés befejezése után törölheti a régi üres alhálózati hello.</span><span class="sxs-lookup"><span data-stu-id="96fd0-109">In that case, you can create a new, larger subnet and migrate hello VMs toohello new subnet, then after migration is complete, you can delete hello old empty subnet.</span></span>

## <a name="how-toomove-a-vm-tooanother-subnet"></a><span data-ttu-id="96fd0-110">Hogyan toomove tooanother alhálózatot</span><span class="sxs-lookup"><span data-stu-id="96fd0-110">How toomove a VM tooanother subnet</span></span>
<span data-ttu-id="96fd0-111">a virtuális gépek toomove futtassa az alábbi példában hello használata sablonként hello Set-AzureSubnet PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="96fd0-111">toomove a VM, run hello Set-AzureSubnet PowerShell cmdlet, using hello example below as a template.</span></span> <span data-ttu-id="96fd0-112">Hello az alábbi példában szereplő azt áthelyezi TestVM a jelen alhálózatból tooSubnet-2.</span><span class="sxs-lookup"><span data-stu-id="96fd0-112">In hello example below, we are moving TestVM from its present subnet, tooSubnet-2.</span></span> <span data-ttu-id="96fd0-113">Lehet, hogy tooedit hello példa tooreflect a környezetben.</span><span class="sxs-lookup"><span data-stu-id="96fd0-113">Be sure tooedit hello example tooreflect your environment.</span></span> <span data-ttu-id="96fd0-114">Vegye figyelembe, hogy amikor hello frissítés-AzureVM parancsmag egy eljárás részeként futtatja, mindig újra lesz indítva a virtuális gép hello frissítési folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="96fd0-114">Note that whenever you run hello Update-AzureVM cmdlet as part of a procedure, it will restart your VM as part of hello update process.</span></span>

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

<span data-ttu-id="96fd0-115">Ha a virtuális Géphez megadott egy statikus belső magánhálózati IP-címe, akkor kell tooclear beállítás hello VM tooa új alhálózat áthelyezés előtt.</span><span class="sxs-lookup"><span data-stu-id="96fd0-115">If you specified a static internal private IP for your VM, you'll have tooclear that setting before you can move hello VM tooa new subnet.</span></span> <span data-ttu-id="96fd0-116">Ebben az esetben használja a hello következő:</span><span class="sxs-lookup"><span data-stu-id="96fd0-116">In that case, use hello following:</span></span>

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="toomove-a-role-instance-tooanother-subnet"></a><span data-ttu-id="96fd0-117">egy szerepkör-példány tooanother alhálózati toomove</span><span class="sxs-lookup"><span data-stu-id="96fd0-117">toomove a role instance tooanother subnet</span></span>
<span data-ttu-id="96fd0-118">egy szerepkör példánya toomove hello CSCFG-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="96fd0-118">toomove a role instance, edit hello CSCFG file.</span></span> <span data-ttu-id="96fd0-119">Hello az alábbi példában szereplő azt áthelyezi "Role0"-beli virtuális hálózaton *VNETName* a jelen alhálózatból túl*alhálózat-2*.</span><span class="sxs-lookup"><span data-stu-id="96fd0-119">In hello example below, we are moving "Role0" in virtual network *VNETName* from its present subnet too*Subnet-2*.</span></span> <span data-ttu-id="96fd0-120">Hello szerepkör példánya már telepítve lett, mert hello alhálózati név csak módosítani fogjuk alhálózat-2 =.</span><span class="sxs-lookup"><span data-stu-id="96fd0-120">Because hello role instance was already deployed, you'll just change hello Subnet name = Subnet-2.</span></span> <span data-ttu-id="96fd0-121">Lehet, hogy tooedit hello példa tooreflect a környezetben.</span><span class="sxs-lookup"><span data-stu-id="96fd0-121">Be sure tooedit hello example tooreflect your environment.</span></span>

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
