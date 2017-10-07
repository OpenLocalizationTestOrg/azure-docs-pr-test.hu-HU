---
title: "hello Azure CLI 2.0 használatával több IP-címmel aaaVM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gép használt hello Azure CLI 2.0 |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 15efd853cc7c31bacb64ed052dabedd3fe4d3079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-cli-20"></a>Több IP-cím hozzárendelése toovirtual gépek hello Azure CLI 2.0 használatával

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Ez a cikk azt ismerteti, hogyan toocreate keresztül hello Azure Resource Manager deployment model használatával egy virtuális gép (VM) hello Azure CLI 2.0. Több IP-cím hello klasszikus telepítési modell használatával létrehozott tooresources nem lehet hozzárendelni. További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása

Hajthatja végre ezt a feladatot az Azure CLI 2.0 (Ez a cikk) hello vagy hello [Azure CLI 1.0](virtual-network-multiple-ip-addresses-cli-nodejs.md). Hello értékek, a környezetének megfelelő módosítására. hello következő lépések azt ismertetik, hogyan toocreate például VM több IP-címek, hello forgatókönyvben leírtak szerint. A változók értékeinek módosítása "" és az IP-cím típusok pedig ez szükséges lenne a megvalósítás. 

1. Telepítse a hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Ha még nincs telepítve.
2. Az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek hello lépések végrehajtásával a hello [az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. A bejelentkezési hello paranccsal parancshéj `az login` válassza ki a hello előfizetés használata.
4. Hozzon létre hello VM hello parancsfájl, amely a következő Linux vagy a Mac számítógépen. hello parancsfájlt hoz létre egy erőforráscsoportot, egy virtuális hálózathoz (VNet), egyetlen hálózati Adapterrel rendelkező három IP-konfigurációk és a virtuális gépek hello két hálózati adapter csatlakoztatva tooit. hálózati adapter, a nyilvános IP-cím, a virtuális hálózati hello, és a Virtuálisgép-erőforrások kell lenniük a hello azonos helyen és az előfizetés. Bár a hello erőforrások összes nincs tooexist hello ugyanabban az erőforráscsoportban, a parancsfájl tehetik a következő hello.

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using hello `--allocation-method Static` option. If you
# do not specify this option, hello address is allocated dynamically. hello address is assigned toohello resource from a pool
# of IP adresses unique tooeach Azure region. Download and view hello file from
# https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists hello ranges for each region.

PipName="myPublicIP"

# This name must be unique within an Azure location.
DnsName="myDNSName"

az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--dns-name $DnsName\
--allocation-method Static

# Create a virtual network with one subnet

VnetName="myVnet"
VnetPrefix="10.0.0.0/16"
VnetSubnetName="mySubnet"
VnetSubnetPrefix="10.0.0.0/24"

az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnetName \
--subnet-prefix $VnetSubnetPrefix

# Create a network interface connected toohello subnet and associate hello public IP address tooit. Azure will create the
# first IP configuration with a static private IP address and will associate hello public IP address resource tooit.

NicName="MyNic1"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--private-ip-address 10.0.0.4
--vnet-name $VnetName \
--public-ip-address $PipName
    
# Create a second public IP address, a second IP configuration, and associate it toohello NIC. This configuration has a
# static public IP address and a static private IP address.

az network public-ip create \
--resource-group $RgName \
--location $Location \
--name myPublicIP2 \
--dns-name mypublicdns2 \
--allocation-method Static

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--name IPConfig-2 \
--private-ip-address 10.0.0.5 \
--public-ip-name myPublicIP2

# Create a third IP configuration, and associate it toohello NIC. This configuration has  static private IP address and # no public IP address.

azure network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--private-ip-address 10.0.0.6 \
--name IPConfig-3

# Note: Though this article assigns all IP configurations tooa single NIC, you can also assign multiple IP configurations
# tooany NIC in a VM. toolearn how toocreate a VM with multiple NICs, read hello Create a VM with multiple NICs 
# article: https://docs.microsoft.com/azure/virtual-network/virtual-network-deploy-multinic-arm-cli.

# Create a VM and attach hello NIC.

VmName="myVm"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes rticle. hello script fails if hello VM size
# is not supported in hello location you select. Run hello `azure vm sizes --location estcentralus` command tooget a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello utput returned by entering the
# `az vm image list` command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file. If you're creating a Windows VM, remove hello following
# line and you'll be prompted for hello password you want tooconfigure for hello VM.

SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

Ezenkívül toocreating virtuális gép és egy hálózati Adaptert a 3 IP-konfigurációk, hello parancsfájlt hoz létre:

- Egyszeri támogatás lemez felügyelt alapértelmezés szerint, de más beállítások érhetők el hello lemeztípus hozhat létre. Olvasási hello [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben alább.
- Egy virtuális hálózatot egyetlen alhálózattal és két nyilvános IP-címet. Másik megoldásként használhatja *meglévő* virtuális hálózat, alhálózat, a hálózati adapter vagy nyilvános IP-cím erőforrás. Hogyan toouse meglévő hálózati erőforrások helyett adjon meg további erőforrások létrehozása toolearn `az vm create -h`.

Nyilvános IP-címek névleges díjat kell. tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.

Hello virtuális gép létrehozása után adja meg a hello `az network nic show --name MyNic1 --resource-group myResourceGroup` parancs tooview hello hálózati Adapteres konfiguráció. Adja meg a hello `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` tooview hello IP-konfigurációk listáját társított toohello hálózati adaptert.

Hozzáadás hello privát IP-címek toohello virtuális gép operációs rendszer hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.

## <a name="add"></a>Adja hozzá az IP-címek tooa méretű VM

Hozzáadhat további magán- és IP címeket tooan meglévő hálózati hello következő lépések végrehajtásával. hello példák épül hello [forgatókönyv](#Scenario) a cikkben.

1. Nyisson meg egy parancssori rendszerhéj és a teljes hello hátralévő lépéseket ebben a szakaszban egy munkameneten belül. Ha még nem rendelkezik Azure CLI telepítése és konfigurálása, teljes hello hello szükséges lépések [Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben és a bejelentkezési tooyour hello Azure fiók `az-login` parancs.

2. Végezze el a következő szakaszok a követelmények alapján hello egyikében hello lépéseket:

    **Adja hozzá a magánhálózati IP-cím**
    
    tooadd egy privát IP-cím tooa hálózati adapter, létre kell hoznia egy IP-konfiguráció, amely a következő hello parancs használatával. hello statikus IP-cím hello alhálózat nem használt címnek kell lennie.

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    Tetszőleges számú konfigurációk összes kívánt beállítást, egyedi konfigurációs nevek és privát IP-címek használata (a statikus IP-címekkel rendelkező konfigurációk) létrehozása.

    **A nyilvános IP-cím hozzáadása**
    
    A nyilvános IP-cím való társítás tooeither által hozzáadott új IP-konfiguráció vagy a meglévő IP-konfigurációt. Hajtsa végre hello egyik hello szakaszokban szereplő, az összes kívánt beállítást.

    Nyilvános IP-címek névleges díjat kell. tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.

    - **Hello erőforrás tooa új IP-konfiguráció hozzárendelése**
    
        Egy nyilvános IP-címet ad hozzá egy új IP-konfiguráció, amikor is hozzá kell magánhálózati IP-cím, mert az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie. Egy meglévő nyilvános IP-cím erőforrás hozzáadása, vagy hozzon létre egy újat. toocreate egy új, adja meg a következő parancs hello:
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        egy új IP-beállítását egy statikus magánhálózati IP-cím és a kapcsolódó hello toocreate *myPublicIP3* nyilvános IP-cím erőforrás címet, adja meg a következő parancs hello:

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - **Rendeljen hozzá hello erőforrás tooan meglévő IP-konfiguráció** egy nyilvános IP-cím erőforrás csak lehet társított tooan IP-konfigurációja, amely még nincs társítva. Segítségével meghatározhatja, hogy rendelkezik-e IP-konfigurációt tartozó nyilvános IP-cím hello a következő parancs beírásával:

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        Kimeneti adott vissza:
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        Hello óta **PublicIpAddressId** oszlopában *IpConfig-3* van a hello üres kimeneti, nem nyilvános IP-cím erőforrás jelenleg társított tooit. Adja hozzá egy meglévő nyilvános IP cím erőforrás tooIpConfig-3, vagy adja meg a következő parancs toocreate egy hello:

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        Adja meg a következő parancs tooassociate hello nyilvános IP-cím erőforrás toohello meglévő IP-konfiguráció hello *IPConfig-3*:
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. Nézet hello magánhálózati IP-címek és hello nyilvános IP-cím erőforrás-azonosítók hozzárendelt toohello megadásával NIC hello a következő parancsot:

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    Kimeneti adott vissza: <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. Hello toohello NIC toohello virtuális gép operációs rendszer hello hello utasításait követve hozzáadott magánhálózati IP-címek hozzáadása [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
