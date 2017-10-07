---
title: "virtuális gépek - Azure CLI 1.0 aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek hello Azure parancssori felület (CLI) 1.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6caae79c98c7bc5f246b7bb3bb8bd8f032eb2d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-10"></a>A virtuális gépek hello Azure CLI 1.0 saját IP-címek konfigurálása


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat 

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre: 

- [Az Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell 

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [statikus magánhálózati IP-cím hello klasszikus üzembe helyezési modellel kezelése](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

minta hello Azure CLI-t az alábbi parancsok már létrehozott egy egyszerű környezetben várható. Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Hogyan toospecify egy statikus magánhálózati IP-virtuális gép létrehozásakor
toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet* statikus magánhálózati IP-címe a *192.168.1.101*, kövesse az alábbi hello lépéseket:

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.
   
        azure config mode arm
   
    Várt kimenet:
   
        info:    New mode is arm
3. Futtassa a hello **létrehozása azure-hálózat nyilvános ip-** toocreate hello virtuális gép IP-Címek. hello kimenet után látható hello lista hello paramétereket ismerteti.
   
        azure network public-ip create -g TestRG -n TestPIP -l centralus
   
    Várt kimenet:
   
        info:    Executing command network public-ip create
        + Looking up hello public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up hello public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK
   
   * **-g (vagy --resource-group)**. Hello erőforrás csoport hello nyilvános IP-cím neve létrejön.
   * **-n (vagy --name)**. Hello nyilvános IP-cím neve.
   * **-l (vagy --location)**. Azure-régió, ahol létrejön hello nyilvános IP-cím. A mi esetünkben *centralus*.
4. Futtatás hello **azure-hálózat hálózati adapter létrehozása** parancs toocreate statikus magánhálózati IP-címe a hálózati Adapterhez. hello kimenet után látható hello lista hello paramétereket ismerteti.
   
        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd
   
    Várt kimenet:
   
        + Looking up hello network interface "TestNIC"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up hello network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
   
   * **-a (vagy--magánfelhő-ip-cím)**. Statikus magánhálózati IP-cím a hello hálózati adaptert.
   * **-m (vagy--vnet-alhálózatneve)**. Hello ahol hello NIC létrejön a VNet neve.
   * **-k (vagy--alhálózati-név)**. Ahol létrejön-e a hálózati adapter hello hello alhálózat neve.
5. Futtassa a hello **azure virtuális gép létrehozása** parancs toocreate hello hello nyilvános IP-cím és a hálózati adapter használó virtuális gépek a fenti létrehozott. hello kimenet után látható hello lista hello paramétereket ismerteti.
   
        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd
   
    Várt kimenet:
   
        info:    Executing command vm create
        + Looking up hello VM "DNS01"
        info:    Using hello VM Size "Standard_A1"
        warn:    hello image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account vnetstorage
        + Looking up hello NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in hello NIC "TestNIC"
        info:    Found public ip parameters, trying toosetup PublicIP profile
        + Looking up hello public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up hello NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK
   
   * **-y (vagy--operációsrendszer-típust)**. Vagy operációs rendszer a virtuális gép, hello típusú *Windows* vagy *Linux*.
   * **-f (vagy--nic-név)**. Hello NIC hello virtuális gép nevét fogja használni.
   * **-i (vagy--nyilvános-ip-név)**. Hello nyilvános IP-hello virtuális gép nevét fogja használni.
   * **-F (vagy--vnet-name)**. Hello ahol létrejön a virtuális gép hello VNet neve.
   * **-j (vagy--vnet-alhálózat-név)**. Ahol létrejön a virtuális gép hello hello alhálózat neve.

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek
tooview hello statikus magánhálózati IP-címe cím VM létre hello parancsfájl fenti, Futtatás a következő parancs az Azure parancssori felület hello hello adatait, és tekintse meg az hello értékeinek *privát IP-foglalási-metódus* és *magánhálózatiIP-cím*:

    azure vm show -g TestRG -n DNS01

Várt kimenet:

    info:    Executing command vm show
    + Looking up hello VM "DNS01"
    + Looking up hello NIC "TestNIC"
    + Looking up hello public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Hogyan tooremove egy statikus magánhálózati IP-VM
Az Azure CLI az erőforrás-kezelő egy hálózati adapter nem távolítható el a statikus magánhálózati IP-cím. Akkor hozzon létre egy új hálózati Adaptert, amely egy dinamikus IP-cím használ, távolítsa el a virtuális gép hello előző NIC hello, és adja hozzá a hello új hálózati toohello virtuális gép. toochange NIC hello hello használt virtuális gép int eh fenti parancsokat, kövesse az alábbi hello lépéseket.

1. Futtassa a hello **azure-hálózat hálózati adapter létrehozása** toocreate parancsot egy új hálózati Adaptert dinamikus IP-lefoglalást használatával. Figyelje meg, hogyan nem kell toospecify hello IP-cím most.
   
        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd
   
    Várt kimenet:
   
        info:    Executing command network nic create
        + Looking up hello network interface "TestNIC2"
        + Looking up hello subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up hello network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
2. Futtassa a hello **azure virtuálisgép-készlet** parancs toochange hello hello virtuális gép által használt hálózati adapter.
   
        azure vm set -g TestRG -n DNS01 -N TestNIC2
   
    Várt kimenet:
   
        info:    Executing command vm set
        + Looking up hello VM "DNS01"
        + Looking up hello NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK
3. Ha szeretett volna, futtassa a hello **azure-hálózat hálózati törlése** parancs toodelete hello régi hálózati adaptert.
   
        azure network nic delete -g TestRG -n TestNIC --quiet
   
    Várt kimenet:
   
        info:    Executing command network nic delete
        + Looking up hello network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Hogyan tooadd statikus magánhálózati IP-címe kezelje a meglévő virtuális gép tooan
tooadd egy statikus magánhálózati IP cím toohello a fentiek hello parancsfájl használatával létrehozott virtuális gép által használt hálózati hello a következő parancs futtatásával:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Várt kimenet:

    info:    Executing command network nic set
    + Looking up hello network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up hello network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.
* További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.
* Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).

