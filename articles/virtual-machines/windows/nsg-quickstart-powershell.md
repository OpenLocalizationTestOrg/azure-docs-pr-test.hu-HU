---
title: "aaaOpen portok tooa virtuális gép Azure PowerShell használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour VM a Windows hello Azure resource manager rendszerbe állítási mód és az Azure PowerShell használatával"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: c1817a0c447ae4ce7a1ce2a1fc6927bedf2dacb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-and-endpoints-tooa-vm-in-azure-using-powershell"></a>Hogyan tooopen portokat és végpontot tooa PowerShell használata Azure-ban
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Gyors parancsok
toocreate egy hálózati biztonsági csoport és a hozzáférés-vezérlési lista szabályok kell [telepített Azure PowerShell legújabb verziójának hello](/powershell/azureps-cmdlets-docs). Emellett [hello Azure-portál használatával a következő lépésekkel](nsg-quickstart-portal.md).

Jelentkezzen be tooyour Azure-fiók:

```powershell
Login-AzureRmAccount
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.

Hozzon létre egy szabályt a [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* tooallow *tcp* porton forgalom *80*:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

Ezután hozzon létre a hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) és hozzárendelése hello HTTP szabály újonnan létrehozott az alábbiak szerint. hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Most tegyük rendelje hozzá a hálózati biztonsági csoport tooa alhálózat. hello alábbi példa hozzárendel egy meglévő virtuális hálózatot nevű *myVnet* toohello változó *$vnet* rendelkező [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

A hálózati biztonsági csoporthoz társítandó az alhálózat [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig). hello alábbi példa társítja nevű hello alhálózati *mySubnet* a hálózati biztonsági csoporthoz:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Végül frissítse a virtuális hálózaton található [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) ahhoz, hogy a módosítások tootake érvénybe:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>További információ a hálózati biztonsági csoportok
hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása. Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez. További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](tutorial-virtual-network.md#manage-internal-traffic).

Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött. hello terheléselosztó osztja el a forgalmat tooVMs forgalomszűrést végez biztosító hálózati biztonsági csoport. További információkért lásd: [hogyan tooload egyenleg Linux virtuális gépek az Azure toocreate a magas rendelkezésre állású alkalmazások](tutorial-load-balancer.md).

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat. További részletes környezetek létrehozásáról a következő cikkek hello információt talál:

* [Az Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)
* [Mi az a hálózati biztonsági csoport (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Terheléselosztók Azure Resource Manager áttekintése](../../load-balancer/load-balancer-arm.md)

