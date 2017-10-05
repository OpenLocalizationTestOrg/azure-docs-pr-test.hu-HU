---
title: "Virtuális gép (klasszikus) létrehozása a PowerShell segítségével több hálózati adapterrel rendelkező |} Microsoft Docs"
description: "Megtudhatja, hogyan hozza létre és konfigurálja a virtuális gépek több hálózati adapterrel, PowerShell-lel rendelkező."
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
ms.openlocfilehash: 68ccc1cac22e593b099729fe68c6bee63df44d9b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a><span data-ttu-id="de121-103">Több hálózati adapterrel rendelkező virtuális gép (klasszikus) létrehozása</span><span class="sxs-lookup"><span data-stu-id="de121-103">Create a VM (Classic) with multiple NICs</span></span>
<span data-ttu-id="de121-104">Virtuális gépek (VM) létrehozása az Azure-ban, és csatlakoztassa a virtuális gépek mindegyikének több hálózati adapterek (NIC).</span><span class="sxs-lookup"><span data-stu-id="de121-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) to each of your VMs.</span></span> <span data-ttu-id="de121-105">Több hálózati adapter áll számos hálózati virtuális készülékeket, például az alkalmazások biztosításán és WAN-optimalizálást megoldások követelményeit.</span><span class="sxs-lookup"><span data-stu-id="de121-105">Multiple NICs are a requirement for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span> <span data-ttu-id="de121-106">Több hálózati adapterrel is közötti hálózati forgalom elkülönítést biztosítani.</span><span class="sxs-lookup"><span data-stu-id="de121-106">Multiple NICs also provide isolation of traffic between NICs.</span></span>

![Több hálózati adapter a virtuális gép](./media/virtual-networks-multiple-nics/IC757773.png)

<span data-ttu-id="de121-108">Az ábrán látható virtuális gép három hálózati adaptert, és csatlakoznak egy másik alhálózat.</span><span class="sxs-lookup"><span data-stu-id="de121-108">The figure shows a VM with three NICs, each connected to a different subnet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="de121-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="de121-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="de121-110">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="de121-110">This article covers using the classic deployment model.</span></span> <span data-ttu-id="de121-111">A Microsoft azt javasolja, hogy az új telepítések esetén használja a Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="de121-111">Microsoft recommends that most new deployments use Resource Manager.</span></span>

* <span data-ttu-id="de121-112">Internet felé néző VIP (klasszikus üzembe helyezés) csak akkor támogatott a "alapértelmezett" adapteren zajlik.</span><span class="sxs-lookup"><span data-stu-id="de121-112">Internet-facing VIP (classic deployments) is only supported on the "default" NIC.</span></span> <span data-ttu-id="de121-113">Nincs a csak egy VIP – IP-címét az alapértelmezett hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="de121-113">There is only one VIP to the IP of the default NIC.</span></span>
* <span data-ttu-id="de121-114">Ilyenkor (klasszikus üzembe helyezés) példány szint nyilvános IP (LPIP) címek nem támogatottak a több hálózati adapter virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="de121-114">At this time, Instance Level Public IP (LPIP) addresses (classic deployments) are not supported for multi NIC VMs.</span></span>
* <span data-ttu-id="de121-115">A hálózati adapterek sorrendje a virtuális gépen belül véletlenszerű, és az Azure-infrastruktúra frissítései során is változhat.</span><span class="sxs-lookup"><span data-stu-id="de121-115">The order of the NICs from inside the VM will be random, and could also change across Azure infrastructure updates.</span></span> <span data-ttu-id="de121-116">Azonban az IP-címeket, és a megfelelő ethernet MAC címek lesz változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="de121-116">However, the IP addresses, and the corresponding ethernet MAC addresses will remain the same.</span></span> <span data-ttu-id="de121-117">Tegyük fel például, **Eth1** ; 10.1.0.100 IP-cím és MAC-cím 00-0D-3A-B0-39-0D tartalmaz, az Azure infrastruktúra-frissítési és újraindítás után azt módosítani: **Eth2**, de IP- és MAC párosítás változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="de121-117">For example, assume **Eth1** has IP address 10.1.0.100 and MAC address 00-0D-3A-B0-39-0D; after an Azure infrastructure update and reboot, it could be changed to **Eth2**, but the IP and MAC pairing will remain the same.</span></span> <span data-ttu-id="de121-118">Ha újraindításra az ügyfél által kezdeményezett, a hálózati adapter rendelés változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="de121-118">When a restart is customer-initiated, the NIC order will remain the same.</span></span>
* <span data-ttu-id="de121-119">A cím az egyes hálózati adapterek minden egyes virtuális gépen kell lennie az alhálózat, a egyetlen virtuális gép több hálózati adapter egyes hozzárendelhetők legyenek címek, amelyek ugyanazon az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="de121-119">The address for each NIC on each VM must be located in a subnet, multiple NICs on a single VM can each be assigned addresses that are in the same subnet.</span></span>
* <span data-ttu-id="de121-120">A Virtuálisgép-méretet, amelyet létrehozhat egy virtuális gép hálózati adapterek számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="de121-120">The VM size determines the number of NICS that you can create for a VM.</span></span> <span data-ttu-id="de121-121">Hivatkozás a [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) virtuális gép méretének annak meghatározásához, hány hálózati ADAPTERT támogat minden egyes Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="de121-121">Reference the [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM sizes articles to determine how many NICS each VM size supports.</span></span> 

## <a name="network-security-groups-nsgs"></a><span data-ttu-id="de121-122">Hálózati biztonsági csoportokkal (NSG-k)</span><span class="sxs-lookup"><span data-stu-id="de121-122">Network Security Groups (NSGs)</span></span>
<span data-ttu-id="de121-123">Egy erőforrás-kezelő telepítése esetén a a hálózati adapterek, a virtuális gép is lehet társítani, a hálózati biztonsági csoport (NSG), például minden hálózati adaptert egy virtuális gépen, amelyen engedélyezve van több hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="de121-123">In a Resource Manager deployment, any NIC on a VM may be associated with a Network Security Group (NSG), including any NICs on a VM that has multiple NICs enabled.</span></span> <span data-ttu-id="de121-124">Ha egy hálózati adapter hozzá van rendelve egy címet, ahol az alhálózaton társítva van egy NSG-t egy alhálózaton belül, majd a az alhálózat NSG is alkalmazni kell a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="de121-124">If a NIC is assigned an address within a subnet where the subnet is associated with an NSG, then the rules in the subnet’s NSG also apply to that NIC.</span></span> <span data-ttu-id="de121-125">Alhálózatok társítása NSG-ket, mellett is társíthat egy hálózati adapter egy NSG.</span><span class="sxs-lookup"><span data-stu-id="de121-125">In addition to associating subnets with NSGs, you can also associate a NIC with an NSG.</span></span>

<span data-ttu-id="de121-126">Ha egy alhálózat társítva egy NSG-t, és egy hálózati adapter belül alhálózaton külön-külön társítva van egy NSG-t, a társított NSG-szabályok vonatkoznak a **flow-rendelési** a átadta-e virtuális gépbe vagy onnan a hálózati forgalom irányát megfelelően:</span><span class="sxs-lookup"><span data-stu-id="de121-126">If a subnet is associated with an NSG, and a NIC within that subnet is individually associated with an NSG, the associated NSG rules are applied in **flow order** according to the direction of the traffic being passed into or out of the NIC:</span></span>

* <span data-ttu-id="de121-127">**Bejövő forgalom** először áthaladó amelynek célja a szóban forgó hálózati adapter az alhálózat, időt. az alhálózat NSG-szabályok előtt a hálózati adapter történő továbbításához, majd a hálózati adapter NSG-szabályok váltanak.</span><span class="sxs-lookup"><span data-stu-id="de121-127">**Incoming traffic** whose destination is the NIC in question flows first through the subnet, triggering the subnet’s NSG rules, before passing into the NIC, then triggering the NIC’s NSG rules.</span></span>
* <span data-ttu-id="de121-128">**Kimenő forgalom** amelyek forrása a hálózati adapter a a hálózati időt. a hálózati adapter NSG-szabályok előtt áthaladó az alhálózatot, majd időt. az alhálózat NSG-szabályok első out zajlik.</span><span class="sxs-lookup"><span data-stu-id="de121-128">**Outgoing traffic** whose source is the NIC in question flows first out from the NIC, triggering the NIC’s NSG rules, before passing through the subnet, then triggering the subnet’s NSG rules.</span></span>

<span data-ttu-id="de121-129">További információ [hálózati biztonsági csoportok](virtual-networks-nsg.md) és alhálózatok, a virtuális gépek és a hálózati adapter társítását alkalmazásának módja alapján...</span><span class="sxs-lookup"><span data-stu-id="de121-129">Learn more about [Network Security Groups](virtual-networks-nsg.md) and how they are applied based on associations to subnets, VMs, and NICs..</span></span>

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a><span data-ttu-id="de121-130">A klasszikus üzembe helyezési egy több hálózati adapter virtuális Gépre kiterjedő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de121-130">How to Configure a multi NIC VM in a classic deployment</span></span>
<span data-ttu-id="de121-131">Az alábbi utasítások segítségével hozhat létre egy több hálózati adapter virtuális gép 3 hálózati adaptert tartalmazó: az alapértelmezett hálózati adapter és két további hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="de121-131">The instructions below will help you create a multi NIC VM containing 3 NICs: a default NIC and two additional NICs.</span></span> <span data-ttu-id="de121-132">A konfigurálás lépéseinek végrehajtásához hozza létre a virtuális gépek, a szolgáltatás konfigurációs fájl töredéke alábbi megfelelően kell konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="de121-132">The configuration steps will create a VM that will be configured according to the service configuration file fragment below:</span></span>

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
    … Skip over the remainder section …
    </VirtualNetworkSite>


<span data-ttu-id="de121-133">A példában a PowerShell-parancsok futtatása előtt kell a következő előfeltételek teljesülését.</span><span class="sxs-lookup"><span data-stu-id="de121-133">You need the following prerequisites before trying to run the PowerShell commands in the example.</span></span>

* <span data-ttu-id="de121-134">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="de121-134">An Azure subscription.</span></span>
* <span data-ttu-id="de121-135">A konfigurált virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="de121-135">A configured virtual network.</span></span> <span data-ttu-id="de121-136">Lásd: [virtuális hálózat áttekintése](virtual-networks-overview.md) Vnetek további információt.</span><span class="sxs-lookup"><span data-stu-id="de121-136">See [Virtual Network Overview](virtual-networks-overview.md) for more information about VNets.</span></span>
* <span data-ttu-id="de121-137">Az Azure PowerShell legújabb verziójának letöltése, és telepítve.</span><span class="sxs-lookup"><span data-stu-id="de121-137">The latest version of Azure PowerShell downloaded and installed.</span></span> <span data-ttu-id="de121-138">Lásd: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="de121-138">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="de121-139">Több hálózati adapterrel rendelkező virtuális gép létrehozása, az alábbi lépésekkel egy PowerShell-munkameneten belül minden parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="de121-139">To create a VM with multiple NICs, complete the following steps by entering each command within a single PowerShell session:</span></span>

1. <span data-ttu-id="de121-140">Válassza ki a Virtuálisgép-lemezkép Azure VM-lemezkép gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="de121-140">Select a VM image from Azure VM image gallery.</span></span> <span data-ttu-id="de121-141">Vegye figyelembe, hogy a képek módosulnak, és régiónként elérhetők.</span><span class="sxs-lookup"><span data-stu-id="de121-141">Note that images change frequently and are available by region.</span></span> <span data-ttu-id="de121-142">Az alábbi példában megadott lemezkép módosítása vagy előfordulhat, hogy nem a régióban kell, ezért ügyeljen arra, hogy adja meg a lemezképet kell.</span><span class="sxs-lookup"><span data-stu-id="de121-142">The image specified in the example below may change or might not be in your region, so be sure to specify the image you need.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. <span data-ttu-id="de121-143">Hozzon létre egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="de121-143">Create a VM configuration.</span></span>

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. <span data-ttu-id="de121-144">Az alapértelmezett rendszergazdai bejelentkezés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="de121-144">Create the default administrator login.</span></span>

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. <span data-ttu-id="de121-145">További hálózati adapterek hozzáadása a Virtuálisgép-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="de121-145">Add additional NICs to the VM configuration.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. <span data-ttu-id="de121-146">Adja meg az alhálózatot és IP-címet az alapértelmezett hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="de121-146">Specify the subnet and IP address for the default NIC.</span></span>

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. <span data-ttu-id="de121-147">Hozzon létre a virtuális Gépet a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="de121-147">Create the VM in your virtual network.</span></span>

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > <span data-ttu-id="de121-148">Az itt megadott virtuális hálózat már léteznie kell (ahogy azt az Előfeltételek).</span><span class="sxs-lookup"><span data-stu-id="de121-148">The VNet that you specify here must already exist (as mentioned in the prerequisites).</span></span> <span data-ttu-id="de121-149">Az alábbi példa meghatározza nevű virtuális hálózat **MultiNIC-VNet**.</span><span class="sxs-lookup"><span data-stu-id="de121-149">The example below specifies a virtual network named **MultiNIC-VNet**.</span></span>
    >

## <a name="limitations"></a><span data-ttu-id="de121-150">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="de121-150">Limitations</span></span>
<span data-ttu-id="de121-151">A következő korlátozások vonatkoznak, több hálózati adapter használata esetén:</span><span class="sxs-lookup"><span data-stu-id="de121-151">The following limitations are applicable when using multiple NICs:</span></span>

* <span data-ttu-id="de121-152">Több hálózati adapterrel rendelkező virtuális gépeket az Azure virtuális hálózatokról (Vnetekről) kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="de121-152">VMs with multiple NICs must be created in Azure virtual networks (VNets).</span></span> <span data-ttu-id="de121-153">Nem-virtuális hálózat virtuális gépek több hálózati adapter nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="de121-153">Non-VNet VMs cannot be configured with multiple NICs.</span></span>
* <span data-ttu-id="de121-154">Minden virtuális gép rendelkezésre állási csoportba szeretné használni, vagy több hálózati adapterrel, vagy egy egyetlen hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="de121-154">All VMs in an availability set need to use either multiple NICs or a single NIC.</span></span> <span data-ttu-id="de121-155">Nem lehet több hálózati adapter virtuális gépek és egyetlen, a hálózati adapter virtuális gépek rendelkezésre állási csoportok belül.</span><span class="sxs-lookup"><span data-stu-id="de121-155">There cannot be a mixture of multiple NIC VMs and single NIC VMs within an availability set.</span></span> <span data-ttu-id="de121-156">Ugyanazok a szabályok vonatkoznak a virtuális gépek felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="de121-156">Same rules apply for VMs in a cloud service.</span></span> <span data-ttu-id="de121-157">Több hálózati adapter virtuális gépekhez, azok megnyitásáig nem szükséges hálózati adapterek, azonos számú mindaddig, amíg minden rendelkeznek legalább két.</span><span class="sxs-lookup"><span data-stu-id="de121-157">For multiple NIC VMs, they aren't required to have the same number of NICs, as long as they each have at least two.</span></span>
* <span data-ttu-id="de121-158">Virtuális gép egyetlen hálózati adapter és több hálózati adapter (és fordítva) nem állítható be, ha telepítve van, törlésével és ismételt létrehozása nélkül.</span><span class="sxs-lookup"><span data-stu-id="de121-158">A VM with a single NIC cannot be configured with multi NICs (and vice-versa) once it is deployed, without deleting and re-creating it.</span></span>

## <a name="secondary-nics-access-to-other-subnets"></a><span data-ttu-id="de121-159">Másodlagos hálózati adapterek hozzáférést más alhálózatok</span><span class="sxs-lookup"><span data-stu-id="de121-159">Secondary NICs access to other subnets</span></span>
<span data-ttu-id="de121-160">Alapértelmezés szerint a egy alapértelmezett átjáró, amely miatt a forgalom áramlását az másodlagos hálózati adaptereken kell lennie ugyanazon az alhálózaton belül korlátozza nem másodlagos hálózati adapter lesz konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="de121-160">By default secondary NICs will not be configured with a default gateway, due to which the traffic flow on the secondary NICs will be limited to be within the same subnet.</span></span> <span data-ttu-id="de121-161">Ha a felhasználók a saját alhálózati kívül ügyfélcsatornája másodlagos hálózati adapterek engedélyezni kívánja, akkor kell bejegyzés hozzáadása az átjáró konfigurálásához, az alább ismertetett-útválasztási táblázatához.</span><span class="sxs-lookup"><span data-stu-id="de121-161">If the users want to enable secondary NICs to talk outside their own subnet, they will need to add an entry in the routing table to configure the gateway as described below.</span></span>

> [!NOTE]
> <span data-ttu-id="de121-162">A virtuális gépeket létrehozni, mielőtt a 2015. július lehet a hálózati adapterek összes beállított alapértelmezett átjárót.</span><span class="sxs-lookup"><span data-stu-id="de121-162">VMs created before July 2015 may have a default gateway configured for all NICs.</span></span> <span data-ttu-id="de121-163">Az alapértelmezett átjáró, a másodlagos hálózati adapter nem távolítja el, a virtuális gép újraindítása.</span><span class="sxs-lookup"><span data-stu-id="de121-163">The default gateway for secondary NICs will not be removed until these VMs are rebooted.</span></span> <span data-ttu-id="de121-164">Az operációs rendszerek, Linux, például a gyenge állomás útválasztási modellt használó internetkapcsolat különböző hálózati adapterein használatakor a bemenő és kimenő forgalom meghibásodásához vezethet.</span><span class="sxs-lookup"><span data-stu-id="de121-164">In Operating systems that use the weak host routing model, such as Linux, Internet connectivity can break if the ingress and egress traffic use different NICs.</span></span>
> 

### <a name="configure-windows-vms"></a><span data-ttu-id="de121-165">Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="de121-165">Configure Windows VMs</span></span>
<span data-ttu-id="de121-166">Tegyük fel, hogy két hálózati adapterrel rendelkező virtuális gép Windows az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="de121-166">Suppose that you have a Windows VM with two NICs as follows:</span></span>

* <span data-ttu-id="de121-167">Elsődleges hálózati adapter IP-cím: 192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="de121-167">Primary NIC IP address: 192.168.1.4</span></span>
* <span data-ttu-id="de121-168">Másodlagos hálózati adapter IP-cím: 192.168.2.5</span><span class="sxs-lookup"><span data-stu-id="de121-168">Secondary NIC IP address: 192.168.2.5</span></span>

<span data-ttu-id="de121-169">A virtuális gép az IPv4-alapú útválasztási táblázatot néz ki:</span><span class="sxs-lookup"><span data-stu-id="de121-169">The IPv4 route table for this VM would look like this:</span></span>

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

<span data-ttu-id="de121-170">Figyelje meg, hogy az alapértelmezett útvonalat (0.0.0.0) csak érhető el az elsődleges hálózati adapternek.</span><span class="sxs-lookup"><span data-stu-id="de121-170">Notice that the default route (0.0.0.0) is only available to the primary NIC.</span></span> <span data-ttu-id="de121-171">Csak akkor férhessenek hozzá az erőforrásokhoz kívül az alhálózat a másodlagos hálózati adapter, az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="de121-171">You will not be able to access resources outside the subnet for the secondary NIC, as seen below:</span></span>

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

<span data-ttu-id="de121-172">A másodlagos hálózati adapter által az alapértelmezett útvonal hozzáadásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="de121-172">To add a default route on the secondary NIC, follow the steps below:</span></span>

1. <span data-ttu-id="de121-173">Egy parancssorból futtassa a parancsot az alábbi indexszámát azonosítja a másodlagos hálózati adapter:</span><span class="sxs-lookup"><span data-stu-id="de121-173">From a command prompt, run the command below to identify the index number for the secondary NIC:</span></span>
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. <span data-ttu-id="de121-174">Figyelje meg, a második bejegyzés a táblában (a példában) 27 az indexet.</span><span class="sxs-lookup"><span data-stu-id="de121-174">Notice the second entry in the table, with an index of 27 (in this example).</span></span>
3. <span data-ttu-id="de121-175">A parancssorból futtassa a **útvonal hozzáadása** parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="de121-175">From the command prompt, run the **route add** command as shown below.</span></span> <span data-ttu-id="de121-176">Ebben a példában meg 192.168.2.1 alapértelmezett átjáróként a másodlagos hálózati adapter:</span><span class="sxs-lookup"><span data-stu-id="de121-176">In this example, you are specifying 192.168.2.1 as the default gateway for the secondary NIC:</span></span>
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. <span data-ttu-id="de121-177">A kapcsolat tesztelése, lépjen vissza a parancssort, és próbálja meg Pingelje meg egy másik alhálózat a másodlagos hálózati adapter látható egész elemeként eh az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="de121-177">To test connectivity, go back to the command prompt and try to ping a different subnet from the secondary NIC as shown int eh example below:</span></span>
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. <span data-ttu-id="de121-178">Alább látható módon is ellenőrizheti az útválasztási táblázatot az újonnan hozzáadott útvonal kereséséhez:</span><span class="sxs-lookup"><span data-stu-id="de121-178">You can also check your route table to check the newly added route, as shown below:</span></span>
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a><span data-ttu-id="de121-179">Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="de121-179">Configure Linux VMs</span></span>
<span data-ttu-id="de121-180">Linux virtuális gépekhez mivel az alapértelmezett viselkedés használ gyenge állomás útválasztási, azt javasoljuk, hogy a másodlagos hálózati adapterek forgalom csak az ugyanazon alhálózaton belüli korlátozódnak.</span><span class="sxs-lookup"><span data-stu-id="de121-180">For Linux VMs, since the default behavior uses weak host routing, we recommend that the secondary NICs are restricted to traffic flows only within the same subnet.</span></span> <span data-ttu-id="de121-181">Azonban ha bizonyos esetekben az alhálózat kívüli kapcsolatot igényelnek, felhasználók engedélyeznie kell a csoportházirend-alapú útválasztási annak érdekében, hogy a bemenő és kimenő forgalom használja-e az azonos hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="de121-181">However if certain scenarios demand connectivity outside the subnet, users should enable policy based routing to ensure that the ingress and egress traffic uses the same NIC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de121-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de121-182">Next steps</span></span>
* <span data-ttu-id="de121-183">Telepítése [MultiNIC virtuális gépeket a 2-rétegbeli alkalmazás esetén a Resource Manager-telepítés a](virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="de121-183">Deploy [MultiNIC VMs in a 2-tier application scenario in a Resource Manager deployment](virtual-network-deploy-multinic-arm-template.md).</span></span>
* <span data-ttu-id="de121-184">Telepítése [MultiNIC virtuális gépeket a 2-rétegbeli alkalmazás esetén a klasszikus üzembe helyezési a](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="de121-184">Deploy [MultiNIC VMs in a 2-tier application scenario in a classic deployment](virtual-network-deploy-multinic-classic-ps.md).</span></span>

