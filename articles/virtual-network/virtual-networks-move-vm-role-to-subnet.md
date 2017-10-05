---
title: "A virtuális gép (klasszikus) vagy a Cloud Services szerepkör-példány áthelyezése egy másik alhálózat - Azure PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan helyezi át a virtuális gépek (klasszikus) és a Felhőszolgáltatások szerepkörpéldányokat egy másik alhálózat PowerShell használatával."
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
ms.openlocfilehash: b094f8338394ef2e84cad3070936d715411326a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a><span data-ttu-id="e81e0-103">A virtuális gép (klasszikus) vagy a Cloud Services szerepkör példány áthelyezése egy másik alhálózat PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="e81e0-103">Move a VM (Classic) or Cloud Services role instance to a different subnet using PowerShell</span></span>
<span data-ttu-id="e81e0-104">PowerShell használatával helyezze át a virtuális gépek (klasszikus) egy alhálózatból másik ugyanazt a virtuális hálózatot (VNet).</span><span class="sxs-lookup"><span data-stu-id="e81e0-104">You can use PowerShell to move your VMs (Classic) from one subnet to another in the same virtual network (VNet).</span></span> <span data-ttu-id="e81e0-105">Szerepkörpéldányokat helyezheti át a szolgáltatáskonfigurációs SÉMA fájl szerkesztése, hanem a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="e81e0-105">Role instances can be moved by editing the CSCFG file, rather than using PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="e81e0-106">Ez a cikk ismerteti, hogyan kívánja áthelyezni a virtuális gépek telepítése csak a klasszikus üzembe helyezési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="e81e0-106">This article explains how to move VMs deployed through the classic deployment model only.</span></span>
> 
> 

<span data-ttu-id="e81e0-107">Miért virtuális gépek áthelyezése egy másik alhálózat?</span><span class="sxs-lookup"><span data-stu-id="e81e0-107">Why move VMs to another subnet?</span></span> <span data-ttu-id="e81e0-108">Alhálózati áttelepítés akkor hasznos, ha a régebbi alhálózati túl kicsi, és nem bonthatók ki meglévő futó virtuális gépeinek alhálózat miatt.</span><span class="sxs-lookup"><span data-stu-id="e81e0-108">Subnet migration is useful when the older subnet is too small and cannot be expanded due to existing running VMs in that subnet.</span></span> <span data-ttu-id="e81e0-109">Ebben az esetben hozzon létre egy új, nagyobb alhálózatot, és telepítse át a virtuális gépeket az új alhálózatot, majd áttelepítés befejezése után törölheti a régi üres alhálózat.</span><span class="sxs-lookup"><span data-stu-id="e81e0-109">In that case, you can create a new, larger subnet and migrate the VMs to the new subnet, then after migration is complete, you can delete the old empty subnet.</span></span>

## <a name="how-to-move-a-vm-to-another-subnet"></a><span data-ttu-id="e81e0-110">A virtuális gép áthelyezése egy másik alhálózat</span><span class="sxs-lookup"><span data-stu-id="e81e0-110">How to move a VM to another subnet</span></span>
<span data-ttu-id="e81e0-111">Helyezze át a virtuális Gépet, futtassa a Set-AzureSubnet PowerShell-parancsmag, az alábbi példában egy sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="e81e0-111">To move a VM, run the Set-AzureSubnet PowerShell cmdlet, using the example below as a template.</span></span> <span data-ttu-id="e81e0-112">Az alábbi példa azt áthelyezi TestVM a jelen alhálózatból az alhálózat-2.</span><span class="sxs-lookup"><span data-stu-id="e81e0-112">In the example below, we are moving TestVM from its present subnet, to Subnet-2.</span></span> <span data-ttu-id="e81e0-113">Ne felejtse el szerkeszteni a példa a környezetnek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e81e0-113">Be sure to edit the example to reflect your environment.</span></span> <span data-ttu-id="e81e0-114">Vegye figyelembe, hogy amikor a frissítés-AzureVM parancsmag egy eljárás részeként futtatja, mindig újra lesz indítva a virtuális Gépet a frissítési folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="e81e0-114">Note that whenever you run the Update-AzureVM cmdlet as part of a procedure, it will restart your VM as part of the update process.</span></span>

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

<span data-ttu-id="e81e0-115">Ha megadott egy statikus belső magánhálózati IP-címe a virtuális Gépet, törölje ezt a beállítást, mielőtt a virtuális gép áthelyezése egy új alhálózatot kell.</span><span class="sxs-lookup"><span data-stu-id="e81e0-115">If you specified a static internal private IP for your VM, you'll have to clear that setting before you can move the VM to a new subnet.</span></span> <span data-ttu-id="e81e0-116">Ebben az esetben az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="e81e0-116">In that case, use the following:</span></span>

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a><span data-ttu-id="e81e0-117">A szerepkör példánya áthelyezése egy másik alhálózat</span><span class="sxs-lookup"><span data-stu-id="e81e0-117">To move a role instance to another subnet</span></span>
<span data-ttu-id="e81e0-118">Helyezze át a szerepkör-példányt, szerkessze a CSCFG-fájl.</span><span class="sxs-lookup"><span data-stu-id="e81e0-118">To move a role instance, edit the CSCFG file.</span></span> <span data-ttu-id="e81e0-119">Az alábbi példa azt áthelyezi "Role0"-beli virtuális hálózaton *VNETName* a jelen alhálózatból az *alhálózat-2*.</span><span class="sxs-lookup"><span data-stu-id="e81e0-119">In the example below, we are moving "Role0" in virtual network *VNETName* from its present subnet to *Subnet-2*.</span></span> <span data-ttu-id="e81e0-120">A szerepkör példánya már telepítve lett, mert az alhálózat neve csak módosítani fogjuk alhálózat-2 =.</span><span class="sxs-lookup"><span data-stu-id="e81e0-120">Because the role instance was already deployed, you'll just change the Subnet name = Subnet-2.</span></span> <span data-ttu-id="e81e0-121">Ne felejtse el szerkeszteni a példa a környezetnek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e81e0-121">Be sure to edit the example to reflect your environment.</span></span>

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
