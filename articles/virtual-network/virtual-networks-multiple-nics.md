---
title: "PowerShell-lel több hálózati adapterrel rendelkező virtuális gép (klasszikus) aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és a PowerShell segítségével több hálózati adapterrel rendelkező virtuális gépek konfigurálása."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 8ef35bd4cfd7e6a527080f1cfc541275ca86f5e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a><span data-ttu-id="8536e-103">Több hálózati adapterrel rendelkező virtuális gép (klasszikus) létrehozása</span><span class="sxs-lookup"><span data-stu-id="8536e-103">Create a VM (Classic) with multiple NICs</span></span>
<span data-ttu-id="8536e-104">Virtuális gépek (VM) létrehozása az Azure-ban, és csatlakoztassa a virtuális gépek több hálózati adapterek (NIC) tooeach.</span><span class="sxs-lookup"><span data-stu-id="8536e-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="8536e-105">Több hálózati adapter áll számos hálózati virtuális készülékeket, például az alkalmazások biztosításán és WAN-optimalizálást megoldások követelményeit.</span><span class="sxs-lookup"><span data-stu-id="8536e-105">Multiple NICs are a requirement for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span> <span data-ttu-id="8536e-106">Több hálózati adapterrel is közötti hálózati forgalom elkülönítést biztosítani.</span><span class="sxs-lookup"><span data-stu-id="8536e-106">Multiple NICs also provide isolation of traffic between NICs.</span></span>

![Több hálózati adapter a virtuális gép](./media/virtual-networks-multiple-nics/IC757773.png)

<span data-ttu-id="8536e-108">hello. ábra azt mutatja be a három hálózati adapterrel rendelkező virtuális gépet, tooa eltérő alhálózathoz csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="8536e-108">hello figure shows a VM with three NICs, each connected tooa different subnet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8536e-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8536e-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="8536e-110">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="8536e-110">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="8536e-111">A Microsoft azt javasolja, hogy az új telepítések esetén használja a Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8536e-111">Microsoft recommends that most new deployments use Resource Manager.</span></span>

* <span data-ttu-id="8536e-112">Az Internet felé néző VIP (klasszikus üzembe helyezés) csak akkor támogatott a hello "alapértelmezett" hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-112">Internet-facing VIP (classic deployments) is only supported on hello "default" NIC.</span></span> <span data-ttu-id="8536e-113">Nincs a csak egy VIP toohello IP-címe hello alapértelmezett hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-113">There is only one VIP toohello IP of hello default NIC.</span></span>
* <span data-ttu-id="8536e-114">Ilyenkor (klasszikus üzembe helyezés) példány szint nyilvános IP (LPIP) címek nem támogatottak a több hálózati adapter virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="8536e-114">At this time, Instance Level Public IP (LPIP) addresses (classic deployments) are not supported for multi NIC VMs.</span></span>
* <span data-ttu-id="8536e-115">hello sorrendjének hello hálózati adaptert a virtuális gép véletlenszerű lesz, és különböző Azure-infrastruktúra frissítéseket is módosulhatnak hello belül.</span><span class="sxs-lookup"><span data-stu-id="8536e-115">hello order of hello NICs from inside hello VM will be random, and could also change across Azure infrastructure updates.</span></span> <span data-ttu-id="8536e-116">Azonban hello IP-címeket, és megfelelő ethernet MAC hello megmarad címek hello azonos.</span><span class="sxs-lookup"><span data-stu-id="8536e-116">However, hello IP addresses, and hello corresponding ethernet MAC addresses will remain hello same.</span></span> <span data-ttu-id="8536e-117">Tegyük fel például, **Eth1** ; 10.1.0.100 IP-cím és MAC-cím 00-0D-3A-B0-39-0D tartalmaz, az Azure infrastruktúra-frissítési és újraindítás után az módosítható túl**Eth2**, de hello az IP-cím és MAC párosítás lesz továbbra is hello azonos.</span><span class="sxs-lookup"><span data-stu-id="8536e-117">For example, assume **Eth1** has IP address 10.1.0.100 and MAC address 00-0D-3A-B0-39-0D; after an Azure infrastructure update and reboot, it could be changed too**Eth2**, but hello IP and MAC pairing will remain hello same.</span></span> <span data-ttu-id="8536e-118">Ha újraindításra az ügyfél által kezdeményezett, hello NIC rendelés marad hello azonos.</span><span class="sxs-lookup"><span data-stu-id="8536e-118">When a restart is customer-initiated, hello NIC order will remain hello same.</span></span>
* <span data-ttu-id="8536e-119">hello minden egyes virtuális Gépen lévő hálózati adapter címe kell lennie az alhálózat, több hálózati adaptert egy virtuális gép egyes hozzárendelhetők legyenek címek hello ugyanazon az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="8536e-119">hello address for each NIC on each VM must be located in a subnet, multiple NICs on a single VM can each be assigned addresses that are in hello same subnet.</span></span>
* <span data-ttu-id="8536e-120">Virtuálisgép-méretet hello határozza meg, hogy hello, létrehozhat egy virtuális hálózati Adapterrel.</span><span class="sxs-lookup"><span data-stu-id="8536e-120">hello VM size determines hello number of NICS that you can create for a VM.</span></span> <span data-ttu-id="8536e-121">Hivatkozás hello [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) virtuális gép méretének cikkek toodetermine hány hálózati ADAPTERT támogat minden egyes Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="8536e-121">Reference hello [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM sizes articles toodetermine how many NICS each VM size supports.</span></span> 

## <a name="network-security-groups-nsgs"></a><span data-ttu-id="8536e-122">Hálózati biztonsági csoportokkal (NSG-k)</span><span class="sxs-lookup"><span data-stu-id="8536e-122">Network Security Groups (NSGs)</span></span>
<span data-ttu-id="8536e-123">Egy erőforrás-kezelő telepítése esetén a a hálózati adapterek, a virtuális gép is lehet társítani, a hálózati biztonsági csoport (NSG), például minden hálózati adaptert egy virtuális gépen, amelyen engedélyezve van több hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="8536e-123">In a Resource Manager deployment, any NIC on a VM may be associated with a Network Security Group (NSG), including any NICs on a VM that has multiple NICs enabled.</span></span> <span data-ttu-id="8536e-124">Ha egy hálózati adapter hozzá van rendelve egy címet, ahol hello alhálózat társítva van egy NSG-t egy alhálózaton belül, majd hello hello alhálózati NSG szabályait is érvényesek toothat hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-124">If a NIC is assigned an address within a subnet where hello subnet is associated with an NSG, then hello rules in hello subnet’s NSG also apply toothat NIC.</span></span> <span data-ttu-id="8536e-125">Továbbá tooassociating alhálózatokon az NSG-ket is társíthat egy hálózati adapter egy NSG.</span><span class="sxs-lookup"><span data-stu-id="8536e-125">In addition tooassociating subnets with NSGs, you can also associate a NIC with an NSG.</span></span>

<span data-ttu-id="8536e-126">Ha egy alhálózat társítva egy NSG-t, és egy hálózati adapter belül alhálózaton külön-külön társítva van egy NSG-t, hello társított NSG-szabályok érvényesek a **flow-rendelési** toohello forgalom iránya a hello átadta-e be vagy ki szerint a hálózati adapter hello:</span><span class="sxs-lookup"><span data-stu-id="8536e-126">If a subnet is associated with an NSG, and a NIC within that subnet is individually associated with an NSG, hello associated NSG rules are applied in **flow order** according toohello direction of hello traffic being passed into or out of hello NIC:</span></span>

* <span data-ttu-id="8536e-127">**Bejövő forgalom** először áthaladó amelyek célja az adott hálózati hello hello alhálózati hello alhálózati NSG-szabályok előtt hello NIC történő továbbításához, majd hello NIC NSG-szabályok kiváltó váltanak.</span><span class="sxs-lookup"><span data-stu-id="8536e-127">**Incoming traffic** whose destination is hello NIC in question flows first through hello subnet, triggering hello subnet’s NSG rules, before passing into hello NIC, then triggering hello NIC’s NSG rules.</span></span>
* <span data-ttu-id="8536e-128">**Kimenő forgalom** hello NIC kiváltó hello NIC NSG-szabályok előtt hello alhálózati áthaladó, majd kiváltó hello alhálózati NSG-szabályok az első kimeneti amelynek forrása hello az adott hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="8536e-128">**Outgoing traffic** whose source is hello NIC in question flows first out from hello NIC, triggering hello NIC’s NSG rules, before passing through hello subnet, then triggering hello subnet’s NSG rules.</span></span>

<span data-ttu-id="8536e-129">További információ [hálózati biztonsági csoportok](virtual-networks-nsg.md) és társítások toosubnets, a virtuális gépek és a hálózati adapterek alkalmazásának módja alapján...</span><span class="sxs-lookup"><span data-stu-id="8536e-129">Learn more about [Network Security Groups](virtual-networks-nsg.md) and how they are applied based on associations toosubnets, VMs, and NICs..</span></span>

## <a name="how-tooconfigure-a-multi-nic-vm-in-a-classic-deployment"></a><span data-ttu-id="8536e-130">Hogyan tooConfigure egy hálózati adapter több virtuális Gépre kiterjedő a klasszikus telepítési</span><span class="sxs-lookup"><span data-stu-id="8536e-130">How tooConfigure a multi NIC VM in a classic deployment</span></span>
<span data-ttu-id="8536e-131">az alábbi utasítások hello segítségével hozhat létre egy több hálózati adapter virtuális gép 3 hálózati adaptert tartalmazó: az alapértelmezett hálózati adapter és két további hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="8536e-131">hello instructions below will help you create a multi NIC VM containing 3 NICs: a default NIC and two additional NICs.</span></span> <span data-ttu-id="8536e-132">hello konfigurációs lépéseket hozza létre a virtuális gépek toohello szolgáltatás konfigurációs fájl töredék alábbi megfelelően kell konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="8536e-132">hello configuration steps will create a VM that will be configured according toohello service configuration file fragment below:</span></span>

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over hello remainder section …
    </VirtualNetworkSite>


<span data-ttu-id="8536e-133">Előfeltételek toorun hello PowerShell-parancsok hello példa próbálkozás előtt a következő hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8536e-133">You need hello following prerequisites before trying toorun hello PowerShell commands in hello example.</span></span>

* <span data-ttu-id="8536e-134">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8536e-134">An Azure subscription.</span></span>
* <span data-ttu-id="8536e-135">A konfigurált virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="8536e-135">A configured virtual network.</span></span> <span data-ttu-id="8536e-136">Lásd: [virtuális hálózat áttekintése](virtual-networks-overview.md) Vnetek további információt.</span><span class="sxs-lookup"><span data-stu-id="8536e-136">See [Virtual Network Overview](virtual-networks-overview.md) for more information about VNets.</span></span>
* <span data-ttu-id="8536e-137">hello Azure PowerShell legújabb verziójának letöltése, és telepítve.</span><span class="sxs-lookup"><span data-stu-id="8536e-137">hello latest version of Azure PowerShell downloaded and installed.</span></span> <span data-ttu-id="8536e-138">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8536e-138">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="8536e-139">virtuális gép több hálózati adapter a következő lépéseket egy PowerShell-munkameneten belül minden parancs beírásával teljes hello és toocreate:</span><span class="sxs-lookup"><span data-stu-id="8536e-139">toocreate a VM with multiple NICs, complete hello following steps by entering each command within a single PowerShell session:</span></span>

1. <span data-ttu-id="8536e-140">Válassza ki a Virtuálisgép-lemezkép Azure VM-lemezkép gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="8536e-140">Select a VM image from Azure VM image gallery.</span></span> <span data-ttu-id="8536e-141">Vegye figyelembe, hogy a képek módosulnak, és régiónként elérhetők.</span><span class="sxs-lookup"><span data-stu-id="8536e-141">Note that images change frequently and are available by region.</span></span> <span data-ttu-id="8536e-142">hello hello az alábbi példában megadott lemezkép módosítása vagy előfordulhat, hogy nem a régióban kell, ezért mindenképpen toospecify hello lemezkép van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8536e-142">hello image specified in hello example below may change or might not be in your region, so be sure toospecify hello image you need.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. <span data-ttu-id="8536e-143">Hozzon létre egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8536e-143">Create a VM configuration.</span></span>

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. <span data-ttu-id="8536e-144">Hello alapértelmezett rendszergazdai bejelentkezés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8536e-144">Create hello default administrator login.</span></span>

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. <span data-ttu-id="8536e-145">Adja hozzá a további hálózati adapterek toohello Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8536e-145">Add additional NICs toohello VM configuration.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. <span data-ttu-id="8536e-146">Hello alhálózatot és IP-cím megadása hello alapértelmezett hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-146">Specify hello subnet and IP address for hello default NIC.</span></span>

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. <span data-ttu-id="8536e-147">Hozzon létre hello VM a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="8536e-147">Create hello VM in your virtual network.</span></span>

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > <span data-ttu-id="8536e-148">hello itt megadott virtuális hálózat már léteznie kell (ahogy azt hello Előfeltételek).</span><span class="sxs-lookup"><span data-stu-id="8536e-148">hello VNet that you specify here must already exist (as mentioned in hello prerequisites).</span></span> <span data-ttu-id="8536e-149">az alábbi példa hello megadja nevű virtuális hálózat **MultiNIC-VNet**.</span><span class="sxs-lookup"><span data-stu-id="8536e-149">hello example below specifies a virtual network named **MultiNIC-VNet**.</span></span>
    >

## <a name="limitations"></a><span data-ttu-id="8536e-150">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="8536e-150">Limitations</span></span>
<span data-ttu-id="8536e-151">hello a következő korlátozások vonatkoznak, több hálózati adapter használata esetén:</span><span class="sxs-lookup"><span data-stu-id="8536e-151">hello following limitations are applicable when using multiple NICs:</span></span>

* <span data-ttu-id="8536e-152">Több hálózati adapterrel rendelkező virtuális gépeket az Azure virtuális hálózatokról (Vnetekről) kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8536e-152">VMs with multiple NICs must be created in Azure virtual networks (VNets).</span></span> <span data-ttu-id="8536e-153">Nem-virtuális hálózat virtuális gépek több hálózati adapter nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="8536e-153">Non-VNet VMs cannot be configured with multiple NICs.</span></span>
* <span data-ttu-id="8536e-154">Rendelkezésre állási virtuális gépeinek kell toouse állítsa be, vagy több hálózati adapterrel, vagy egy egyetlen hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-154">All VMs in an availability set need toouse either multiple NICs or a single NIC.</span></span> <span data-ttu-id="8536e-155">Nem lehet több hálózati adapter virtuális gépek és egyetlen, a hálózati adapter virtuális gépek rendelkezésre állási csoportok belül.</span><span class="sxs-lookup"><span data-stu-id="8536e-155">There cannot be a mixture of multiple NIC VMs and single NIC VMs within an availability set.</span></span> <span data-ttu-id="8536e-156">Ugyanazok a szabályok vonatkoznak a virtuális gépek felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="8536e-156">Same rules apply for VMs in a cloud service.</span></span> <span data-ttu-id="8536e-157">Több hálózati adapter virtuális gépek esetén nem szükséges toohave hello azonos számú hálózati adaptert, mindaddig, amíg minden rendelkeznek legalább két.</span><span class="sxs-lookup"><span data-stu-id="8536e-157">For multiple NIC VMs, they aren't required toohave hello same number of NICs, as long as they each have at least two.</span></span>
* <span data-ttu-id="8536e-158">Virtuális gép egyetlen hálózati adapter és több hálózati adapter (és fordítva) nem állítható be, ha telepítve van, törlésével és ismételt létrehozása nélkül.</span><span class="sxs-lookup"><span data-stu-id="8536e-158">A VM with a single NIC cannot be configured with multi NICs (and vice-versa) once it is deployed, without deleting and re-creating it.</span></span>

## <a name="secondary-nics-access-tooother-subnets"></a><span data-ttu-id="8536e-159">Másodlagos hálózati adapterek hozzáférés tooother alhálózatok</span><span class="sxs-lookup"><span data-stu-id="8536e-159">Secondary NICs access tooother subnets</span></span>
<span data-ttu-id="8536e-160">Másodlagos hálózati adapter nem konfigurálható egy alapértelmezett átjáró alapértelmezés szerint miatt hello toowhich hello adatforgalmat másodlagos hálózati adapter lesz belül hello korlátozott toobe ugyanazon az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="8536e-160">By default secondary NICs will not be configured with a default gateway, due toowhich hello traffic flow on hello secondary NICs will be limited toobe within hello same subnet.</span></span> <span data-ttu-id="8536e-161">Hello tooenable másodlagos hálózati adapterek tootalk kívül a saját alhálózati szeretnék, azok bejegyzés hello útválasztási táblázat tooconfigure hello átjáró, az alább ismertetett tooadd kell.</span><span class="sxs-lookup"><span data-stu-id="8536e-161">If hello users want tooenable secondary NICs tootalk outside their own subnet, they will need tooadd an entry in hello routing table tooconfigure hello gateway as described below.</span></span>

> [!NOTE]
> <span data-ttu-id="8536e-162">A virtuális gépeket létrehozni, mielőtt a 2015. július lehet a hálózati adapterek összes beállított alapértelmezett átjárót.</span><span class="sxs-lookup"><span data-stu-id="8536e-162">VMs created before July 2015 may have a default gateway configured for all NICs.</span></span> <span data-ttu-id="8536e-163">hello alapértelmezett átjáró másodlagos hálózati adaptert a virtuális gépeken újraindítása nem lesznek eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="8536e-163">hello default gateway for secondary NICs will not be removed until these VMs are rebooted.</span></span> <span data-ttu-id="8536e-164">Hello gyenge állomás útválasztási modellt használó, például a Linux operációs rendszerben internetkapcsolat érvénytelenné Ha hello bemenő és kimenő forgalom használjon másik hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-164">In Operating systems that use hello weak host routing model, such as Linux, Internet connectivity can break if hello ingress and egress traffic use different NICs.</span></span>
> 

### <a name="configure-windows-vms"></a><span data-ttu-id="8536e-165">Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="8536e-165">Configure Windows VMs</span></span>
<span data-ttu-id="8536e-166">Tegyük fel, hogy két hálózati adapterrel rendelkező virtuális gép Windows az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8536e-166">Suppose that you have a Windows VM with two NICs as follows:</span></span>

* <span data-ttu-id="8536e-167">Elsődleges hálózati adapter IP-cím: 192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="8536e-167">Primary NIC IP address: 192.168.1.4</span></span>
* <span data-ttu-id="8536e-168">Másodlagos hálózati adapter IP-cím: 192.168.2.5</span><span class="sxs-lookup"><span data-stu-id="8536e-168">Secondary NIC IP address: 192.168.2.5</span></span>

<span data-ttu-id="8536e-169">Ez a virtuális gép hello IPv4 útvonaltábla néz ki:</span><span class="sxs-lookup"><span data-stu-id="8536e-169">hello IPv4 route table for this VM would look like this:</span></span>

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

<span data-ttu-id="8536e-170">Láthatja, hogy hello alapértelmezett útvonalat (0.0.0.0) csak elérhető toohello elsődleges hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-170">Notice that hello default route (0.0.0.0) is only available toohello primary NIC.</span></span> <span data-ttu-id="8536e-171">Csak akkor tudja tooaccess erőforrások kívül másodlagos hello hello IP-alhálózatot a hálózati adapter, az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="8536e-171">You will not be able tooaccess resources outside hello subnet for hello secondary NIC, as seen below:</span></span>

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

<span data-ttu-id="8536e-172">az alapértelmezett útvonal a tooadd hello másodlagos hálózati adapter, hajtsa végre hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8536e-172">tooadd a default route on hello secondary NIC, follow hello steps below:</span></span>

1. <span data-ttu-id="8536e-173">Egy parancssorból futtassa a tooidentify hello indexszámát hello hello parancsot másodlagos hálózati adapter:</span><span class="sxs-lookup"><span data-stu-id="8536e-173">From a command prompt, run hello command below tooidentify hello index number for hello secondary NIC:</span></span>
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. <span data-ttu-id="8536e-174">Figyelje meg a második bejegyzés hello hello a táblában (a példában) 27 az indexet.</span><span class="sxs-lookup"><span data-stu-id="8536e-174">Notice hello second entry in hello table, with an index of 27 (in this example).</span></span>
3. <span data-ttu-id="8536e-175">Hello parancssorból futtassa a hello **útvonal hozzáadása** parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="8536e-175">From hello command prompt, run hello **route add** command as shown below.</span></span> <span data-ttu-id="8536e-176">Ebben a példában meg 192.168.2.1 hello alapértelmezett átjáróként hello a másodlagos hálózati adapter:</span><span class="sxs-lookup"><span data-stu-id="8536e-176">In this example, you are specifying 192.168.2.1 as hello default gateway for hello secondary NIC:</span></span>
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. <span data-ttu-id="8536e-177">tootest kapcsolatot, lépjen vissza toohello parancssort, és próbálkozzon egy másik alhálózatból származó hello másodlagos hálózati adapter látható egész elemeként eh az alábbi példában tooping:</span><span class="sxs-lookup"><span data-stu-id="8536e-177">tootest connectivity, go back toohello command prompt and try tooping a different subnet from hello secondary NIC as shown int eh example below:</span></span>
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. <span data-ttu-id="8536e-178">Ellenőrizheti az útvonal tábla toocheck hello újonnan hozzáadott útvonal, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="8536e-178">You can also check your route table toocheck hello newly added route, as shown below:</span></span>
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a><span data-ttu-id="8536e-179">Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="8536e-179">Configure Linux VMs</span></span>
<span data-ttu-id="8536e-180">Linux virtuális gépekhez, mivel a hello alapértelmezett konfigurációját használja gyenge állomás útválasztási, azt javasoljuk, hogy hello másodlagos hálózati adapterek korlátozott tootraffic folyamatot csak belül hello ugyanazon az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="8536e-180">For Linux VMs, since hello default behavior uses weak host routing, we recommend that hello secondary NICs are restricted tootraffic flows only within hello same subnet.</span></span> <span data-ttu-id="8536e-181">Azonban bizonyos forgatókönyvek hello alhálózati kívüli kapcsolatot igényelnek, ha felhasználók lehetővé kell tennie a csoportházirend-alapú útválasztási tooensure érkező hello és kimenő forgalom által használt hello azonos hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8536e-181">However if certain scenarios demand connectivity outside hello subnet, users should enable policy based routing tooensure that hello ingress and egress traffic uses hello same NIC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8536e-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8536e-182">Next steps</span></span>
* <span data-ttu-id="8536e-183">Telepítése [MultiNIC virtuális gépeket a 2-rétegbeli alkalmazás esetén a Resource Manager-telepítés a](virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="8536e-183">Deploy [MultiNIC VMs in a 2-tier application scenario in a Resource Manager deployment](virtual-network-deploy-multinic-arm-template.md).</span></span>
* <span data-ttu-id="8536e-184">Telepítése [MultiNIC virtuális gépeket a 2-rétegbeli alkalmazás esetén a klasszikus üzembe helyezési a](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="8536e-184">Deploy [MultiNIC VMs in a 2-tier application scenario in a classic deployment](virtual-network-deploy-multinic-classic-ps.md).</span></span>

