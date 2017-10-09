---
title: "az Azure CLI 1.0 a meglévő hálózati Linux virtuális gépek aaaDeploy |} Microsoft Docs"
description: "Hogyan toodeploy Linux virtuális gép egy meglévő virtuális hálózat használatával történő hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: e660f1563d386efc7788bd236f8b067145ea09bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli-10"></a><span data-ttu-id="c8976-103">Hogyan toodeploy be egy meglévő Azure virtuális hálózatot az Azure CLI 1.0 hello Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="c8976-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure CLI 1.0</span></span>

<span data-ttu-id="c8976-104">Ez a cikk bemutatja, hogyan toouse Azure CLI 1.0 toodeploy egy virtuális gép (VM) be egy meglévő virtuális hálózatot (VNet).</span><span class="sxs-lookup"><span data-stu-id="c8976-104">This article shows you how toouse Azure CLI 1.0 toodeploy a virtual machine (VM) into an existing Virtual Network (VNet).</span></span> <span data-ttu-id="c8976-105">hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="c8976-105">hello requirements are:</span></span>

- [<span data-ttu-id="c8976-106">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="c8976-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="c8976-107">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="c8976-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="c8976-108">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="c8976-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="c8976-109">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="c8976-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="c8976-110">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="c8976-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="c8976-111">[Az Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="c8976-111">[Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="c8976-112">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="c8976-112">Quick Commands</span></span>

<span data-ttu-id="c8976-113">Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="c8976-113">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="c8976-114">Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="c8976-114">More detailed information and context for each step can be found hello rest of hello document, [starting here](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).</span></span>

<span data-ttu-id="c8976-115">Előfeltételek: A SSH erőforráscsoportot, a virtuális hálózat, NSG bejövő, alhálózati.</span><span class="sxs-lookup"><span data-stu-id="c8976-115">Pre-requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span> <span data-ttu-id="c8976-116">Cserélje le olyan saját beállításaival.</span><span class="sxs-lookup"><span data-stu-id="c8976-116">Replace any examples with your own settings.</span></span>

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="c8976-117">Hello VM be hello virtuális hálózati infrastruktúra telepítése</span><span class="sxs-lookup"><span data-stu-id="c8976-117">Deploy hello VM into hello virtual network infrastructure</span></span>

```azurecli
azure vm create myVM \
    -g myResourceGroup \
    -l eastus \
    -y linux \
    -Q Debian \
    -o mystorageaccount \
    -u myAdminUser \
    -M ~/.ssh/id_rsa.pub \
    -n myVM \
    -F myVNet \
    -j mySubnet \
    -N myVNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="c8976-118">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="c8976-118">Detailed walkthrough</span></span>

<span data-ttu-id="c8976-119">Az Azure eszközök, például hello Vnetek és hálózati biztonsági csoportok statikusnak kell lennie, és hosszabb élettartamú ritkán telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="c8976-119">Azure assets like hello VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="c8976-120">Miután a virtuális hálózaton van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="c8976-120">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="c8976-121">Gondolja át, hogy egy hagyományos hardver hálózati kapcsoló a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="c8976-121">Think about a VNet as being a traditional hardware network switch.</span></span> <span data-ttu-id="c8976-122">Nem kell minden egyes központi telepítés kapcsoló egy új hardver tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="c8976-122">You would not need tooconfigure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="c8976-123">Egy megfelelően konfigurált virtuális hálózaton folytathatja toodeploy új kiszolgálók be, hogy a VNet és újra hello VNet hello élettartama során szükséges néhány, az esetleges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="c8976-123">With a correctly configured VNet, you can continue toodeploy new servers into that VNet over and over with few, if any, changes required over hello life of hello VNet.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="c8976-124">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8976-124">Create hello resource group</span></span>

<span data-ttu-id="c8976-125">Először hozzon létre egy erőforrás csoport tooorganize mindent hoz létre ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="c8976-125">First, create a resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="c8976-126">Erőforráscsoportok kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c8976-126">For more information about resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-hello-vnet"></a><span data-ttu-id="c8976-127">Hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8976-127">Create hello VNet</span></span>

<span data-ttu-id="c8976-128">hello első lépés egy VNet toolaunch hello a virtuális gépek toobuild.</span><span class="sxs-lookup"><span data-stu-id="c8976-128">hello first step is toobuild a VNet toolaunch hello VMs into.</span></span> <span data-ttu-id="c8976-129">virtuális hálózat hello külön alhálózatokon üzemeltetni az Ez az útmutató tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c8976-129">hello VNet contains one subnet for this walkthrough.</span></span> <span data-ttu-id="c8976-130">Az Azure Vnetekhez további információkért lásd: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="c8976-130">For more information on Azure VNets, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)</span></span>

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="c8976-131">Hello hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8976-131">Create hello network security group</span></span>

<span data-ttu-id="c8976-132">hello alhálózati mögött egy meglévő hálózati biztonsági csoport beépített így build hello hálózati biztonsági csoport hello alhálózati előtt.</span><span class="sxs-lookup"><span data-stu-id="c8976-132">hello subnet is built behind an existing network security group so build hello network security group before hello subnet.</span></span> <span data-ttu-id="c8976-133">Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben.</span><span class="sxs-lookup"><span data-stu-id="c8976-133">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="c8976-134">Az Azure hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hogyan toocreate hálózati biztonsági csoportja hello Azure parancssori felület](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span><span class="sxs-lookup"><span data-stu-id="c8976-134">For more information on Azure network security groups, see [How toocreate network security groups in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)</span></span>

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="c8976-135">Egy bejövő SSH engedélyezési szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8976-135">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="c8976-136">virtuális gép hello hello, így lehetővé téve a 22-es portot a bejövő forgalom toobe szabály továbbítja hello hálózati tooport 22 hello VM internet van szükség a engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="c8976-136">hello VM needs access from hello internet so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is needed.</span></span>

```azurecli
azure network nsg rule create inboundSSH \
    --resource-group myResourceGroup \
    --nsg-name myNSG \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range 22 \
    --destination-address-prefix 10.10.0.0/24 \
    --destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a><span data-ttu-id="c8976-137">Egy alhálózat toohello virtuális hálózat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8976-137">Add a subnet toohello VNet</span></span>

<span data-ttu-id="c8976-138">Virtuális gépek hello virtuális hálózaton kell lennie egy alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="c8976-138">VMs within hello VNet must be located in a subnet.</span></span> <span data-ttu-id="c8976-139">Minden egyes virtuális hálózat több alhálózattal is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c8976-139">Each VNet can have multiple subnets.</span></span> <span data-ttu-id="c8976-140">Hozzon létre hello alhálózatot, és hello hálózati biztonsági csoporthoz társítandó.</span><span class="sxs-lookup"><span data-stu-id="c8976-140">Create hello subnet and associate with hello network security group.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

<span data-ttu-id="c8976-141">most már hozzáadott hello virtuális hálózaton belül és hello hálózati biztonsági csoport és a szabály társított hello alhálózati.</span><span class="sxs-lookup"><span data-stu-id="c8976-141">hello Subnet is now added inside hello VNet and associated with hello network security group and rule.</span></span>


## <a name="add-a-vnic-toohello-subnet"></a><span data-ttu-id="c8976-142">Adjon hozzá egy virtuális hálózati toohello alhálózatot</span><span class="sxs-lookup"><span data-stu-id="c8976-142">Add a VNic toohello subnet</span></span>

<span data-ttu-id="c8976-143">Virtuális hálózati adapterek (VNics) fontosak, újra felhasználhatja azokat toodifferent virtuális gépek csatlakoztatja őket.</span><span class="sxs-lookup"><span data-stu-id="c8976-143">Virtual network cards (VNics) are important as you can reuse them by connecting them toodifferent VMs.</span></span> <span data-ttu-id="c8976-144">Ez a megközelítés hello virtuális hálózati tartja a statikus erőforrásként hello virtuális gépek ideiglenes lehetnek.</span><span class="sxs-lookup"><span data-stu-id="c8976-144">This approach keeps hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="c8976-145">Hozzon létre egy virtuális hálózati, és társítsa azt az hello alhálózati hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="c8976-145">Create a VNic and associate it with hello subnet created in hello previous step.</span></span>

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="c8976-146">Hello virtuális hálózatot a virtuális gép hello és NSG központi telepítése</span><span class="sxs-lookup"><span data-stu-id="c8976-146">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="c8976-147">Most már rendelkezik egy VNet és alhálózat belül, hogy a virtuális hálózat, és a 22-es port kivételével az összes bejövő forgalmát blokkolja az SSH tooprotect hello alhálózati működő hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="c8976-147">You now have a VNet and subnet inside that VNet, and a network security group acting tooprotect hello subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="c8976-148">hello VM mostantól a meglévő hálózati infrastruktúra belül telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="c8976-148">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="c8976-149">Hello Azure parancssori felület használatával, és hello `azure vm create` parancs hello Linux virtuális gép meglévő Azure-erőforráscsoportot, virtuális hálózatot, alhálózatot és virtuális hálózati telepített toohello.</span><span class="sxs-lookup"><span data-stu-id="c8976-149">Using hello Azure CLI, and hello `azure vm create` command, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span> <span data-ttu-id="c8976-150">További információ az hello CLI toodeploy a teljes virtuális gépek használatával, lásd: [hozzon létre egy teljes Linux környezetet hello Azure parancssori felület használatával](create-cli-complete.md)</span><span class="sxs-lookup"><span data-stu-id="c8976-150">For more information on using hello CLI toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md)</span></span>

```azurecli
azure vm create myVM \
    --resource-group myResourceGroup \
    --location eastus \
    --os-type linux \
    --image-urn Debian \
    --storage-account-name mystorageaccount \
    --admin-username myAdminUser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --vnet-name myVNet \
    --vnet-subnet-name mySubnet \
    --nic-name myVNic
```

<span data-ttu-id="c8976-151">Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, az Azure toodeploy hello VM hello meglévő hálózaton belüli utasította.</span><span class="sxs-lookup"><span data-stu-id="c8976-151">By using hello CLI flags toocall out existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="c8976-152">Egy VNet és alhálózat van telepítve, ha azok statikus vagy állandó erőforráshoz, az Azure-régió, mint maradhatnak.</span><span class="sxs-lookup"><span data-stu-id="c8976-152">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="c8976-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8976-153">Next steps</span></span>

* [<span data-ttu-id="c8976-154">Az Azure Resource Manager sablon toocreate egy adott központi telepítés használata</span><span class="sxs-lookup"><span data-stu-id="c8976-154">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* [<span data-ttu-id="c8976-155">Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva</span><span class="sxs-lookup"><span data-stu-id="c8976-155">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md)
* [<span data-ttu-id="c8976-156">Linux virtuális gép létrehozása az Azure-ban sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="c8976-156">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
