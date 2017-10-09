---
title: "virtuális gép több hálózati adapter - Azure CLI 2.0 és aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan használja több hálózati adapterrel rendelkező virtuális gép toocreate hello Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac0291a978e2c8682c69104915196cc6c4fcf8dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-20"></a>Azure CLI 2.0 hello segítségével több hálózati adapterrel rendelkező virtuális gép létrehozása

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](virtual-network-deploy-multinic-classic-cli.md).
>

## <a name="create"></a>Hello virtuális gép létrehozása

Hajthatja végre ezt a feladatot az Azure CLI 2.0 (Ez a cikk) hello vagy hello [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md). az értékek hello "" hello lépések hello változók erőforrások létre hello forgatókönyvet beállításokkal. Hello értékek, a környezetének megfelelő módosítására.

1. Telepítse a hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Ha még nincs telepítve.
2. Az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek hello lépések végrehajtásával a hello [az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. A bejelentkezési hello paranccsal parancshéj `az login`.
4. Hozzon létre hello VM hello parancsfájl, amely a következő Linux vagy a Mac számítógépen. hello parancsfájlt hoz létre egy erőforráscsoportot, egy virtuális hálózatot (VNet) két alhálózat, a két hálózati adapter és a hello két hálózati adapterrel rendelkező virtuális csatolni tooit. Hálózati adapter hello egyik csatlakoztatott tooone alhálózat, és hozzá van rendelve egy statikus nyilvános és magánhálózati IP-címet. hello más hálózati csatlakoztatott toohello más alhálózathoz, és hozzá statikus magánhálózati IP-cím és a nyilvános IP-cím van hozzárendelve. hálózati adapter, a nyilvános IP-cím, a virtuális hálózati hello, és a Virtuálisgép-erőforrások kell lenniük a hello azonos helyen és az előfizetés. Bár a hello erőforrások összes nincs tooexist hello ugyanabban az erőforráscsoportban, a parancsfájl tehetik a következő hello.

```bash
#!/bin/sh

RgName="Multi-NIC-VM"
Location="westus"

# Create a resource group.
az group create \
--name $RgName \
--location $Location

# hello address is assigned toohello resource from a pool of IP adresses unique tooeach Azure region. 
# Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists
# hello ranges for each region.

PipName="PIP-WEB"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static

# Create a virtual network with one subnet

VnetName="VNet1"
VnetPrefix="10.0.0.0/16"
VnetSubnet1Name="Front-End"
VnetSubnet1Prefix="10.0.0.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnet1Name \
--subnet-prefix $VnetSubnet1Prefix

# Create a second subnet within hello VNet

VnetSubnet2Name="Back-end"
VnetSubnet2Prefix="10.0.1.0/24"
az network vnet subnet create \
--vnet-name $VnetName \
--resource-group $RgName \
--name $VnetSubnet2Name \
--address-prefix $VnetSubnet2Prefix

# Create a network interface connected tooone of hello subnets. hello NIC is assigned a single dynamic private and
# public IP address by default, but you can instead, assign static addresses, or no public IP address at all.
# You can also assign multiple private or public IP addresses tooeach NIC. toolearn more about IP addressing
# options for NICs, enter hello az network nic create -h command.

Nic1Name="NIC-FE"
PrivateIpAddress1="10.0.0.5"

az network nic create \
--name $Nic1Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress1 \
--public-ip-address $PipName

# Create a second network interface and connect it toohello other subnet. Though multiple NICs attached toohello same
# VM can be connected toodifferent subnets, hello subnets must all be within hello same VNet. Add additional NICs as necessary.

Nic2Name="NIC-BE"
PrivateIpAddress2="10.0.1.5"

az network nic create \
--name $Nic2Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet2Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress2

# Create a VM and attach hello two NICs.

VmName="WEB"

# Replace hello value for hello following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. Not all VM sizes support
# more than one NIC, so be sure tooselect a VM size that supports hello number of NICs you want tooattach toohello VM.
# You must create hello VM with at least two NICs if you want tooadd more after VM creation. If you create a VM with
# only one NIC, you can't add additional NICs toohello VM after VM creation, regardless of how many NICs hello VM supports.
# hello VM size specified in hello following variable supports two NICs.

VmSize="Standard_DS2"

# Replace hello value for hello OsImage variable value with a value for *urn* from hello output returned by entering the
# az vm image list command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace hello following value with hello path tooyour public key file.

SshKeyValue="~/.ssh/id_rsa.pub"

# Before executing hello following command, add variable names of additional NICs you may have added toohello script that
# you want tooattach toohello VM. If creating a Windows VM, remove hello **ssh-key-value** line and you'll be prompted for
# hello password you want tooconfigure for hello VM.

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $Nic1Name $Nic2Name \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

Ezen kívül két hálózati adapterrel rendelkező virtuális toocreating, hello parancsfájl hoz létre:
- Egyszeri támogatás lemez felügyelt alapértelmezés szerint, de más beállítások érhetők el hello lemeztípus hozhat létre. Olvasási hello [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben alább.
- Két alhálózatokat és egyetlen nyilvános IP-címmel rendelkező virtuális hálózatban. Másik megoldásként használhatja *meglévő* virtuális hálózat, alhálózat, a hálózati adapter vagy nyilvános IP-cím erőforrás. Hogyan toouse meglévő hálózati erőforrások helyett adjon meg további erőforrások létrehozása toolearn `az vm create -h`.

## <a name = "validate"></a>Ellenőrizze a virtuális gép létrehozása és a hálózati adapterek

1. Adja meg a hello parancs `az resource list --resouce-group Multi-NIC-VM --output table` toosee hello parancsfájl által létrehozott hello erőforrások listáját. Kell lennie hat erőforrások adott vissza a kimeneti hello: két hálózati adapterrel, egy lemezt, egy nyilvános IP-cím, egy virtuális hálózat és egy virtuális gépet.
2. Adja meg a hello parancs `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`. Az adott vissza a kimeneti hello, vegye figyelembe a hello értékének **IP-cím** és, hogy hello értékének **PublicIpAllocationMethod** van *statikus*.
3. Hello a következő parancs futtatása, előtt távolítsa el a hello <>, cserélje le *felhasználónév* hello használt hello nevű **felhasználónév** hello parancsfájlt, és cserélje ki a változó *IP-cím* a hello **IP-cím** hello előző lépésben. Futtatási hello következő parancsot a virtuális gép tooconnect toohello: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 
4. Miután csatlakozott a virtuális gép toohello, futtassa a hello `sudo ifconfig` parancs toosee *eth0* és *eth1* felületek. Egyes hálózati adapterek hello statikus magánhálózati IP-címek hello Azure DHCP-kiszolgálók hello parancsfájlban megadott van rendelve. hello hozzárendelt toohello hálózati adapter IP- és MAC-címek ne változtassa meg amíg hello virtuális gép törlődik. Azt javasoljuk, hogy nem változtat IP-címzést, az operációs rendszerben, azt is le lehet tiltani kapcsolat toohello számítógép. Nyilvános IP-címek nem jelennek meg hello operációs rendszerben, mert azok hálózati cím lefordított tooand hello privát IP-címről hello Azure infrastruktúra által.

## <a name= "clean-up"></a>Távolítsa el a virtuális gép hello és a kapcsolódó erőforrások

Javasoljuk, hogy törölje-e ebben a gyakorlatban létrejött, ha nem használja őket éles környezetben hello erőforrásokat. Virtuális gép, a nyilvános IP-cím és a lemezerőforrásokat függő díj terheli, mindaddig, amíg azok van üzembe helyezve. Ebben a gyakorlatban teljes hello lépések során létrejött tooremove hello erőforrásokat:
1. tooview hello erőforrások hello erőforráscsoportban, futtassa a hello `az resource list --resource-group Multi-NIC-VM` parancsot.
2. Ellenőrizze, hogy nincsenek erőforrások hello erőforráscsoport nem ebben a cikkben hello parancsfájl által létrehozott hello erőforrások. 
3. Ebben a gyakorlatban, futtassa a hello létrehozott összes erőforrás toodelete `az group delete --name Multi-NIC-VM` parancsot. hello parancs hello erőforráscsoport és a benne található összes hello erőforrás törlése.

## <a name="next-steps"></a>Következő lépések

Hálózati forgalmat a virtuális gép létrehozása ebben a cikkben hello tooand áramolhasson. Bejövő és kimenő szabályok belül egy NSG-t, amely korlátozza, hogy mindegyik hálózati interfész vagy az egyes alhálózatokon tooand áramolhasson hello forgalmat adhat meg. További információk az NSG-k, olvassa el a hello toolearn [NSG áttekintése](virtual-networks-nsg.md) cikk.
