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
# <a name="move-a-vm-classic-or-cloud-services-role-instance-tooa-different-subnet-using-powershell"></a>Helyezze át a virtuális gép (klasszikus) vagy Felhőszolgáltatások szerepkör példány tooa másik alhálózat PowerShell használatával
Használható PowerShell toomove a virtuális gépek (klasszikus) az egyik alhálózat tooanother hello ugyanazt a virtuális hálózatot (VNet). Szerepkörpéldányokat áthelyezhető hello CSCFG-fájl szerkesztése, hanem a PowerShell használatával.

> [!NOTE]
> Ez a cikk ismerteti, hogyan toomove virtuális gépek telepítve hello klasszikus telepítési modell használatával.
> 
> 

Miért helyezze át a virtuális gépek tooanother alhálózati? Alhálózati áttelepítés akkor hasznos, ha hello régebbi alhálózati túl kicsi, és nem bonthatók ki lejáró futtató virtuális gépek alhálózat tooexisting. Ebben az esetben hozzon létre egy új, nagyobb alhálózatot, és át hello virtuális gépek toohello új alhálózatot, majd áttelepítés befejezése után törölheti a régi üres alhálózati hello.

## <a name="how-toomove-a-vm-tooanother-subnet"></a>Hogyan toomove tooanother alhálózatot
a virtuális gépek toomove futtassa az alábbi példában hello használata sablonként hello Set-AzureSubnet PowerShell-parancsmagot. Hello az alábbi példában szereplő azt áthelyezi TestVM a jelen alhálózatból tooSubnet-2. Lehet, hogy tooedit hello példa tooreflect a környezetben. Vegye figyelembe, hogy amikor hello frissítés-AzureVM parancsmag egy eljárás részeként futtatja, mindig újra lesz indítva a virtuális gép hello frissítési folyamat részeként.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Ha a virtuális Géphez megadott egy statikus belső magánhálózati IP-címe, akkor kell tooclear beállítás hello VM tooa új alhálózat áthelyezés előtt. Ebben az esetben használja a hello következő:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="toomove-a-role-instance-tooanother-subnet"></a>egy szerepkör-példány tooanother alhálózati toomove
egy szerepkör példánya toomove hello CSCFG-fájl szerkesztése. Hello az alábbi példában szereplő azt áthelyezi "Role0"-beli virtuális hálózaton *VNETName* a jelen alhálózatból túl*alhálózat-2*. Hello szerepkör példánya már telepítve lett, mert hello alhálózati név csak módosítani fogjuk alhálózat-2 =. Lehet, hogy tooedit hello példa tooreflect a környezetben.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
