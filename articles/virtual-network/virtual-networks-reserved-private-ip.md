---
title: "aaaStatic belső privát IP - Azure VM – klasszikus"
description: "Statikus belső IP-címek (immerzióban) ismertetése és hogyan toomanage őket"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 93444c6f-af1b-41f8-a035-77f5c0302bf0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.openlocfilehash: 5abe1c59f2f3ed19bcf56c269dfe57ac32d4f601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-a-static-internal-private-ip-address-using-powershell-classic"></a>Hogyan tooset egy belső statikus magánhálózati IP cím PowerShell (klasszikus) használata
A legtöbb esetben nincs szükség a virtuális gép toospecify statikus belső IP-címnek. A virtuális hálózat virtuális gépek automatikusan fog kapni a belső IP-címnek megadott tartomány. Azonban bizonyos esetekben egy statikus IP-címet ad meg egy adott virtuális gép teljesen logikus. Ha például a virtuális gép folyamatos toorun DNS vagy egy tartományvezérlő. A statikus belső IP-cím hello VM akár keresztül stop/deprovision állapotban marad. 

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén használjon hello [Resource Manager üzembe helyezési modellben](virtual-networks-static-private-ip-arm-ps.md).
> 
> 

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a>Hogyan tooverify, ha egy adott IP-cím áll rendelkezésre
Ha hello tooverify IP-cím *10.0.0.7* érhető el egy nevű vnetet *TestVnet*, futtassa a hello a következő PowerShell-parancsot, és ellenőrizze a hello értéke *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> Tootest hello parancs fent biztonságos környezetben kövesse hello irányelveinek [hozzon létre egy virtuális hálózatot (klasszikus)](virtual-networks-create-vnet-classic-pportal.md) toocreate egy nevű vnetet *TestVnet* , és ellenőrizze a hello használ  *10.0.0.0/8* címterének.
> 
> 

## <a name="how-toospecify-a-static-internal-ip-when-creating-a-vm"></a>Hogyan toospecify egy statikus belső IP-cím egy virtuális gép létrehozásakor
hello az alábbi PowerShell-parancsfájlt hoz létre egy új felhőalapú szolgáltatás nevű *TestService*, majd lemezkép lekéri az Azure-ból, majd létrehoz egy nevű virtuális gép *TestVM* hello új felhőszolgáltatásban beolvasott hello lemezkép használatával készletek hello VM toobe nevű alhálózat *alhálózat-1*, és beállítja a *10.0.0.7* , egy statikus belső IP-Címek hello VM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-tooretrieve-static-internal-ip-information-for-a-vm"></a>Hogyan tooretrieve statikus belső IP-információit a virtuális gépek
tooview hello statikus belső IP-információit hello hello parancsfájl a fenti létrehozott virtuális gép hello a következő PowerShell-parancs futtatása, és tekintse meg az értékeket hello *IP-cím*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-tooremove-a-static-internal-ip-from-a-vm"></a>Hogyan tooremove egy statikus belső IP-cím egy virtuális gépről
tooremove hello statikus belső IP-címet toohello VM hozzáadott hello parancsfájl fenti, futtassa a következő PowerShell-paranccsal hello:

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-tooadd-a-static-internal-ip-tooan-existing-vm"></a>Hogyan tooadd statikus belső IP-tooan meglévő virtuális gép
a statikus belső IP toohello runt a fenti hello parancsfájl a következő parancs használatával létrehozott virtuális gép tooadd:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a>Következő lépések
[Fenntartott IP-cím](virtual-networks-reserved-public-ip.md)

[Példányszintű nyilvános IP-cím (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Fenntartott IP-cím REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx)

