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
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-10"></a><span data-ttu-id="77239-103">A virtuális gépek hello Azure CLI 1.0 saját IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="77239-103">Configure private IP addresses for a virtual machine using hello Azure CLI 1.0</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="77239-104">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="77239-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="77239-105">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="77239-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="77239-106">[Az Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="77239-106">[Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="77239-107">[Az Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="77239-107">[Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) - our next-generation CLI for hello resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="77239-108">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="77239-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="77239-109">Emellett [statikus magánhálózati IP-cím hello klasszikus üzembe helyezési modellel kezelése](virtual-networks-static-private-ip-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="77239-109">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="77239-110">minta hello Azure CLI-t az alábbi parancsok már létrehozott egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="77239-110">hello sample Azure CLI commands below expect a simple environment already created.</span></span> <span data-ttu-id="77239-111">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="77239-111">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-cli.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="77239-112">Hogyan toospecify egy statikus magánhálózati IP-virtuális gép létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="77239-112">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="77239-113">toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet* statikus magánhálózati IP-címe a *192.168.1.101*, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="77239-113">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="77239-114">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="77239-114">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="77239-115">Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="77239-115">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="77239-116">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-116">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="77239-117">Futtassa a hello **létrehozása azure-hálózat nyilvános ip-** toocreate hello virtuális gép IP-Címek.</span><span class="sxs-lookup"><span data-stu-id="77239-117">Run hello **azure network public-ip create** toocreate a public IP for hello VM.</span></span> <span data-ttu-id="77239-118">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="77239-118">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure network public-ip create -g TestRG -n TestPIP -l centralus
   
    <span data-ttu-id="77239-119">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-119">Expected output:</span></span>
   
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
   
   * <span data-ttu-id="77239-120">**-g (vagy --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="77239-120">**-g (or --resource-group)**.</span></span> <span data-ttu-id="77239-121">Hello erőforrás csoport hello nyilvános IP-cím neve létrejön.</span><span class="sxs-lookup"><span data-stu-id="77239-121">Name of hello resource group hello public IP will be created in.</span></span>
   * <span data-ttu-id="77239-122">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="77239-122">**-n (or --name)**.</span></span> <span data-ttu-id="77239-123">Hello nyilvános IP-cím neve.</span><span class="sxs-lookup"><span data-stu-id="77239-123">Name of hello public IP.</span></span>
   * <span data-ttu-id="77239-124">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="77239-124">**-l (or --location)**.</span></span> <span data-ttu-id="77239-125">Azure-régió, ahol létrejön hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="77239-125">Azure region where hello public IP will be created.</span></span> <span data-ttu-id="77239-126">A mi esetünkben *centralus*.</span><span class="sxs-lookup"><span data-stu-id="77239-126">For our scenario, *centralus*.</span></span>
4. <span data-ttu-id="77239-127">Futtatás hello **azure-hálózat hálózati adapter létrehozása** parancs toocreate statikus magánhálózati IP-címe a hálózati Adapterhez.</span><span class="sxs-lookup"><span data-stu-id="77239-127">Run hello **azure network nic create** command toocreate a NIC with a static private IP.</span></span> <span data-ttu-id="77239-128">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="77239-128">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd
   
    <span data-ttu-id="77239-129">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-129">Expected output:</span></span>
   
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
   
   * <span data-ttu-id="77239-130">**-a (vagy--magánfelhő-ip-cím)**.</span><span class="sxs-lookup"><span data-stu-id="77239-130">**-a (or --private-ip-address)**.</span></span> <span data-ttu-id="77239-131">Statikus magánhálózati IP-cím a hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="77239-131">Static private IP address for hello NIC.</span></span>
   * <span data-ttu-id="77239-132">**-m (vagy--vnet-alhálózatneve)**.</span><span class="sxs-lookup"><span data-stu-id="77239-132">**-m (or --subnet-vnet-name)**.</span></span> <span data-ttu-id="77239-133">Hello ahol hello NIC létrejön a VNet neve.</span><span class="sxs-lookup"><span data-stu-id="77239-133">Name of hello VNet where hello NIC will be created.</span></span>
   * <span data-ttu-id="77239-134">**-k (vagy--alhálózati-név)**.</span><span class="sxs-lookup"><span data-stu-id="77239-134">**-k (or --subnet-name)**.</span></span> <span data-ttu-id="77239-135">Ahol létrejön-e a hálózati adapter hello hello alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="77239-135">Name of hello subnet where hello NIC will be created.</span></span>
5. <span data-ttu-id="77239-136">Futtassa a hello **azure virtuális gép létrehozása** parancs toocreate hello hello nyilvános IP-cím és a hálózati adapter használó virtuális gépek a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="77239-136">Run hello **azure vm create** command toocreate hello VM using hello public IP and NIC created above.</span></span> <span data-ttu-id="77239-137">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="77239-137">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd
   
    <span data-ttu-id="77239-138">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-138">Expected output:</span></span>
   
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
   
   * <span data-ttu-id="77239-139">**-y (vagy--operációsrendszer-típust)**.</span><span class="sxs-lookup"><span data-stu-id="77239-139">**-y (or --os-type)**.</span></span> <span data-ttu-id="77239-140">Vagy operációs rendszer a virtuális gép, hello típusú *Windows* vagy *Linux*.</span><span class="sxs-lookup"><span data-stu-id="77239-140">Type of operating system for hello VM, either *Windows* or *Linux*.</span></span>
   * <span data-ttu-id="77239-141">**-f (vagy--nic-név)**.</span><span class="sxs-lookup"><span data-stu-id="77239-141">**-f (or --nic-name)**.</span></span> <span data-ttu-id="77239-142">Hello NIC hello virtuális gép nevét fogja használni.</span><span class="sxs-lookup"><span data-stu-id="77239-142">Name of hello NIC hello VM will use.</span></span>
   * <span data-ttu-id="77239-143">**-i (vagy--nyilvános-ip-név)**.</span><span class="sxs-lookup"><span data-stu-id="77239-143">**-i (or --public-ip-name)**.</span></span> <span data-ttu-id="77239-144">Hello nyilvános IP-hello virtuális gép nevét fogja használni.</span><span class="sxs-lookup"><span data-stu-id="77239-144">Name of hello public IP hello VM will use.</span></span>
   * <span data-ttu-id="77239-145">**-F (vagy--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="77239-145">**-F (or --vnet-name)**.</span></span> <span data-ttu-id="77239-146">Hello ahol létrejön a virtuális gép hello VNet neve.</span><span class="sxs-lookup"><span data-stu-id="77239-146">Name of hello VNet where hello VM will be created.</span></span>
   * <span data-ttu-id="77239-147">**-j (vagy--vnet-alhálózat-név)**.</span><span class="sxs-lookup"><span data-stu-id="77239-147">**-j (or --vnet-subnet-name)**.</span></span> <span data-ttu-id="77239-148">Ahol létrejön a virtuális gép hello hello alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="77239-148">Name of hello subnet where hello VM will be created.</span></span>

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="77239-149">Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="77239-149">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="77239-150">tooview hello statikus magánhálózati IP-címe cím VM létre hello parancsfájl fenti, Futtatás a következő parancs az Azure parancssori felület hello hello adatait, és tekintse meg az hello értékeinek *privát IP-foglalási-metódus* és *magánhálózatiIP-cím*:</span><span class="sxs-lookup"><span data-stu-id="77239-150">tooview hello static private IP address information for hello VM created with hello script above, run hello following Azure CLI command and observe hello values for *Private IP alloc-method* and *Private IP address*:</span></span>

    azure vm show -g TestRG -n DNS01

<span data-ttu-id="77239-151">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-151">Expected output:</span></span>

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

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="77239-152">Hogyan tooremove egy statikus magánhálózati IP-VM</span><span class="sxs-lookup"><span data-stu-id="77239-152">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="77239-153">Az Azure CLI az erőforrás-kezelő egy hálózati adapter nem távolítható el a statikus magánhálózati IP-cím.</span><span class="sxs-lookup"><span data-stu-id="77239-153">You cannot remove a static private IP address from a NIC in Azure CLI for Resource Manager.</span></span> <span data-ttu-id="77239-154">Akkor hozzon létre egy új hálózati Adaptert, amely egy dinamikus IP-cím használ, távolítsa el a virtuális gép hello előző NIC hello, és adja hozzá a hello új hálózati toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="77239-154">You must create a new NIC that uses a dynamic IP, remove hello previous NIC from hello VM, and then add hello new NIC toohello VM.</span></span> <span data-ttu-id="77239-155">toochange NIC hello hello használt virtuális gép int eh fenti parancsokat, kövesse az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="77239-155">toochange hello NIC for hello VM used int eh commands above, follow hello steps below.</span></span>

1. <span data-ttu-id="77239-156">Futtassa a hello **azure-hálózat hálózati adapter létrehozása** toocreate parancsot egy új hálózati Adaptert dinamikus IP-lefoglalást használatával.</span><span class="sxs-lookup"><span data-stu-id="77239-156">Run hello **azure network nic create** command toocreate a new NIC using dynamic IP allocation.</span></span> <span data-ttu-id="77239-157">Figyelje meg, hogyan nem kell toospecify hello IP-cím most.</span><span class="sxs-lookup"><span data-stu-id="77239-157">Notice how you do not need toospecify hello IP address this time.</span></span>
   
        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd
   
    <span data-ttu-id="77239-158">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-158">Expected output:</span></span>
   
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
2. <span data-ttu-id="77239-159">Futtassa a hello **azure virtuálisgép-készlet** parancs toochange hello hello virtuális gép által használt hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="77239-159">Run hello **azure vm set** command toochange hello NIC used by hello VM.</span></span>
   
        azure vm set -g TestRG -n DNS01 -N TestNIC2
   
    <span data-ttu-id="77239-160">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-160">Expected output:</span></span>
   
        info:    Executing command vm set
        + Looking up hello VM "DNS01"
        + Looking up hello NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK
3. <span data-ttu-id="77239-161">Ha szeretett volna, futtassa a hello **azure-hálózat hálózati törlése** parancs toodelete hello régi hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="77239-161">If wanted, run hello **azure network nic delete** command toodelete hello old NIC.</span></span>
   
        azure network nic delete -g TestRG -n TestNIC --quiet
   
    <span data-ttu-id="77239-162">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-162">Expected output:</span></span>
   
        info:    Executing command network nic delete
        + Looking up hello network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="77239-163">Hogyan tooadd statikus magánhálózati IP-címe kezelje a meglévő virtuális gép tooan</span><span class="sxs-lookup"><span data-stu-id="77239-163">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="77239-164">tooadd egy statikus magánhálózati IP cím toohello a fentiek hello parancsfájl használatával létrehozott virtuális gép által használt hálózati hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="77239-164">tooadd a static private IP address toohello NIC used by teh VM created using hello script above, run hello following command:</span></span>

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

<span data-ttu-id="77239-165">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="77239-165">Expected output:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="77239-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77239-166">Next steps</span></span>
* <span data-ttu-id="77239-167">További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="77239-167">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="77239-168">További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="77239-168">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="77239-169">Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="77239-169">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

