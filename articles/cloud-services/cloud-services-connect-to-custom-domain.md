---
title: "egy felhőalapú szolgáltatás tooa aaaConnect egyéni tartományvezérlő |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect webes vagy feldolgozói szerepkörök tooa egyéni AD-tartomány PowerShell és az AD-tartomány bővítmény használatával"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 9540190ccf17c03e55159c6c68429eee29e0a558
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-azure-cloud-services-roles-tooa-custom-ad-domain-controller-hosted-in-azure"></a>Csatlakozás Azure Cloud Services szerepkörök tooa egyéni Azure-ban üzemeltetett AD-tartományvezérlő
Első üzembe helyezünk egy virtuális hálózatot (VNet) az Azure-ban. Az Active Directory-tartományvezérlőhöz (az Azure virtuális gép üzemeltetett) toohello VNet majd adjuk hozzá. A következő azt szolgáltatás hozzáadása a meglévő felhőalapú szolgáltatás szerepkörök toohello előre létrehozott virtuális hálózatot, majd csatlakoztassa őket toohello tartományvezérlő.

Mielőtt azt először néhány dolgot tookeep szem előtt a:

1. Ez az oktatóanyag PowerShell használja, ezért győződjön meg arról, hogy Azure PowerShell telepítése és toogo kész. tooget segítséget beállítása az Azure PowerShell, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
2. Az AD-tartományvezérlő és a webes vagy feldolgozói szerepkör-példányok a hello VNet toobe kell.

Kövesse a cikk részletesen ismerteti, és ha probléma merül fel, hagyja meg velünk hello cikk végén hello megjegyzés. Valaki kap vissza tooyou (Igen, találtunk megjegyzések).

hello hálózati hello felhőalapú szolgáltatás által hivatkozott kell lennie egy **klasszikus virtuális hálózatot**.

## <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása
Azure hello Azure-portálon vagy a PowerShell használatával is létrehozhat egy virtuális hálózatot. Ebben az oktatóanyagban a PowerShell használjuk. a virtuális hálózat használatával toocreate hello Azure portál, lásd: [virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása
Hello virtuális hálózat beállításának befejezése után kell toocreate az AD tartományvezérlőt. Ebben az oktatóanyagban azt szeretné állítani egy AD-tartományvezérlő egy Azure virtuális gépen.

toodo, hozzon létre egy virtuális gépet a PowerShell segítségével a következő parancsok hello használata:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it toohello Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-tooa-domain-controller"></a>A virtuális gép tooa tartományvezérlő előléptetése
Virtuális gép tooconfigure hello AD-tartományvezérlő, mert először a virtuális gép toohello toolog kell, konfigurálja.

a virtuális gép toohello toolog, kaphat a hello RDP-fájlt a PowerShell segítségével, a következő parancsok használata hello:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Ha be van jelentkezve toohello VM, be a virtuális gép AD-tartományvezérlő által következő hello részletes útmutató a [hogyan tooset az ügyfélnek AD-tartományvezérlő](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-toohello-virtual-network"></a>A felhőalapú szolgáltatás toohello virtuális hálózat hozzáadása
A következő lépésben tooadd a felhőalapú szolgáltatás telepítési toohello új virtuális hálózat. toodo, módosítsa a felhőalapú szolgáltatás szolgáltatáskonfigurációs séma hello vonatkozó szakaszaihoz tooyour szolgáltatáskonfigurációs séma Visual Studio használatával hozzáadásával, vagy az Ön által választott szerkesztőben hello.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Ezután a felhőalapú szolgáltatások projekt buildjének elkészítéséhez, és tooAzure telepítheti. a cloud services csomag tooAzure üzembe helyezésével járó tooget súgó lásd: [hogyan tooCreate és egy felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md#how-to-deploy-a-cloud-service)

## <a name="connect-your-webworker-roles-toohello-domain"></a>A webes vagy feldolgozói szerepkörök toohello tartományhoz csatlakozzon.
Miután a felhőszolgáltatás-projekt Azure van telepítve, csatlakoztassa a szerepkör példányok toohello egyéni AD tartományi hello AD-tartomány bővítmény használatával. tooadd hello AD-tartomány bővítmény tooyour meglévő felhőalapú szolgáltatások telepítése és csatlakozás hello egyéni tartományhoz, hajtsa végre a következő PowerShell-parancsok hello:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension toohello cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

És azt.

A felhőalapú szolgáltatások csatlakoztatott tooyour egyéni tartományvezérlő kell lennie. Ha azt szeretné, hogy további információk a különböző lehetőségekről hello toolearn hogyan tooconfigure AD-tartomány-bővítmény használatát hello PowerShell segítségével. Néhány példa kövesse:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
