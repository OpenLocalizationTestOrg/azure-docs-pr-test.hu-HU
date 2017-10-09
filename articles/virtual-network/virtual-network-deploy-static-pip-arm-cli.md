---
title: "a virtuális gép statikus nyilvános IP-címmel – Azure CLI 2.0 aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate virtuális gép és a statikus nyilvános IP cím használatával hello Azure parancssori felület (CLI) 2.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486060463486462dd8336734a7ad23c4a2cba452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-cli-20"></a>Virtuális gép létrehozása az Azure CLI 2.0 hello segítségével statikus nyilvános IP-cím

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md)
> * [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [Sablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klasszikus)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>Hello virtuális gép létrehozása

Hajthatja végre ezt a feladatot az Azure CLI 2.0 (Ez a cikk) hello vagy hello [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md). az értékek hello "" hello lépések hello változók erőforrások létre hello forgatókönyvet beállításokkal. Hello értékek, a környezetének megfelelő módosítására.

1. Telepítse a hello [Azure CLI 2.0](/cli/azure/install-az-cli2) Ha még nincs telepítve.
2. Az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek hello lépések végrehajtásával a hello [az SSH nyilvános és titkos kulcsból álló kulcspárt létrehozása Linux virtuális gépek](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. A bejelentkezési hello paranccsal parancshéj `az login`.
4. Hozzon létre hello VM hello parancsfájl, amely a következő Linux vagy a Mac számítógépen. az Azure nyilvános IP-cím, a virtuális hálózat, a hálózati illesztő hello, és a Virtuálisgép-erőforrások kell lenniük a hello azonos helyen. Bár a hello erőforrások összes nincs tooexist hello ugyanabban az erőforráscsoportban, a parancsfájl tehetik a következő hello.

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using hello --allocation-method Static option.
# If you do not specify this option, hello address is allocated dynamically. hello address is assigned toothe
# resource from a pool of IP adresses unique tooeach Azure region. hello DnsName must be unique within the
# Azure location it's created in. Download and view hello file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists hello ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected toohello VNet with a static private IP address and associate hello public IP address
# resource toohello NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with hello NIC

VmName="WEB1"

# Replace hello value for hello VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace hello value for hello OsImage variable with a value for *urn* from hello output returned by entering
# hello `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace hello following value with hello path tooyour public key file.
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
# If creating a Windows VM, remove hello previous line and you'll be prompted for hello password you want tooconfigure for hello VM.
```

Továbbá a virtuális gépek toocreating, hello parancsfájl hoz létre:
- Egyszeri támogatás lemez felügyelt alapértelmezés szerint, de más beállítások érhetők el hello lemeztípus hozhat létre. Olvasási hello [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikkben alább.
- Virtuális hálózati alhálózat, a hálózati adapter és nyilvános IP-cím erőforrás. Másik megoldásként használhatja *meglévő* virtuális hálózat, alhálózat, a hálózati adapter vagy nyilvános IP-cím erőforrás. Hogyan toouse meglévő hálózati erőforrások helyett adjon meg további erőforrások létrehozása toolearn `az vm create -h`.

## <a name = "validate"></a>Virtuális gép létrehozása és a nyilvános IP-cím ellenőrzése

1. Adja meg a hello parancs `az resource list --resouce-group IaaSStory --output table` toosee hello parancsfájl által létrehozott hello erőforrások listáját. Kell lennie öt erőforrások adott vissza a kimeneti hello: hálózati kapcsolat, lemez, nyilvános IP-cím, virtuális hálózati és a virtuális gép.
2. Adja meg a hello parancs `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. Az adott vissza a kimeneti hello, vegye figyelembe a hello értékének **IP-cím** és, hogy hello értékének **PublicIpAllocationMethod** van *statikus*.
3. Hello a következő parancs végrehajtásakor, előtt távolítsa el a hello <>, cserélje le *felhasználónév* hello használt hello nevű **felhasználónév** hello parancsfájlt, és cserélje ki a változó *IP-cím* a hello **IP-cím** hello előző lépésben. Futtatási hello következő parancsot a virtuális gép tooconnect toohello: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>Távolítsa el a virtuális gép hello és a kapcsolódó erőforrások

Javasoljuk, hogy törölje-e ebben a gyakorlatban létrejött, ha nem használja őket éles környezetben hello erőforrásokat. Virtuális gép, a nyilvános IP-cím és a lemezerőforrásokat függő díj terheli, mindaddig, amíg azok van üzembe helyezve. Ebben a gyakorlatban teljes hello lépések során létrejött tooremove hello erőforrásokat:

1. tooview hello erőforrások hello erőforráscsoportban, futtassa a hello `az resource list --resource-group IaaSStory` parancsot.
2. Ellenőrizze, hogy nincsenek erőforrások hello erőforráscsoport nem ebben a cikkben hello parancsfájl által létrehozott hello erőforrások. 
3. Ebben a gyakorlatban, futtassa a hello létrehozott összes erőforrás toodelete `az group delete -n IaaSStory` parancsot. hello parancs hello erőforráscsoport és a benne található összes hello erőforrás törlése.

## <a name="next-steps"></a>Következő lépések

Hálózati forgalmat a virtuális gép létrehozása ebben a cikkben hello tooand áramolhasson. Bejövő és kimenő szabályok belül egy NSG áramolhasson hello hálózati adapter vagy hello alhálózati tooand hello forgalom korlátozó adhat meg. További információk az NSG-k, olvassa el a hello toolearn [NSG áttekintése](virtual-networks-nsg.md) cikk.
