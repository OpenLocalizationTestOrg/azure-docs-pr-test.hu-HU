---
title: "aaaUsing belső DNS VM a névfeloldás az Azure-on |} Microsoft Docs"
description: "Belső DNS-sel Azure VM névfeloldáshoz."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: 94fd6577aa51ce5db4dc26649b415ddeeb410eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="08860-103">Névfeloldás VM Azure belső DNS-sel</span><span class="sxs-lookup"><span data-stu-id="08860-103">Using internal DNS for VM name resolution on Azure</span></span>

<span data-ttu-id="08860-104">Ez a cikk bemutatja hogyan tooset statikus belső DNS-nevek, Linux virtuális gépek virtuális hálózati kártyák (VNic) és a DNS-címke nevek használatával.</span><span class="sxs-lookup"><span data-stu-id="08860-104">This article shows how tooset static internal DNS names for Linux VMs using Virtual NIC cards (VNic) and DNS label names.</span></span> <span data-ttu-id="08860-105">Statikus DNS-nevek használhatók állandó infrastruktúra-szolgáltatásokat, mint egy Jenkins build, ez a dokumentum használt, vagy egy Git kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="08860-105">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="08860-106">hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="08860-106">hello requirements are:</span></span>

* [<span data-ttu-id="08860-107">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="08860-107">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="08860-108">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="08860-108">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="08860-109">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="08860-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="08860-110">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="08860-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="08860-111">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="08860-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="08860-112">[Az Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="08860-112">[Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="08860-113">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="08860-113">Quick commands</span></span>

<span data-ttu-id="08860-114">Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="08860-114">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="08860-115">Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="08860-115">More detailed information and context for each step can be found hello rest of hello document, [starting here](#detailed-walkthrough).</span></span>  

<span data-ttu-id="08860-116">Előfeltételek: A SSH erőforráscsoportot, a virtuális hálózat, NSG bejövő, alhálózati.</span><span class="sxs-lookup"><span data-stu-id="08860-116">Pre-Requirements: Resource Group, VNet, NSG with SSH inbound, Subnet.</span></span>

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a><span data-ttu-id="08860-117">Hozzon létre egy virtuális hálózati statikus belső DNS-név</span><span class="sxs-lookup"><span data-stu-id="08860-117">Create a VNic with a static internal DNS name</span></span>

<span data-ttu-id="08860-118">Hello `-r` cli jelző csak az beállítás hello DNS-címke, amely biztosít a virtuális hálózati hello hello statikus DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="08860-118">hello `-r` cli flag is for setting hello DNS label, which provides hello static DNS name for hello VNic.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-hello-vm-into-hello-vnet-nsg-and-connect-hello-vnic"></a><span data-ttu-id="08860-119">Hello VM hello VNet NSG történő telepítéséhez, és csatlakozzon a virtuális hálózati hello</span><span class="sxs-lookup"><span data-stu-id="08860-119">Deploy hello VM into hello VNet, NSG and, connect hello VNic</span></span>

<span data-ttu-id="08860-120">Hello `-N` hello VNic toohello kapcsolódó új virtuális gép hello telepítési tooAzure során.</span><span class="sxs-lookup"><span data-stu-id="08860-120">hello `-N` connects hello VNic toohello new VM during hello deployment tooAzure.</span></span>

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="08860-121">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="08860-121">Detailed walkthrough</span></span>

<span data-ttu-id="08860-122">A teljes folyamatos integrációt és a folyamatos üzembe helyezés (CiCd) Azure-beli infrastruktúra egyes kiszolgálók toobe statikus vagy hosszú élettartamú kiszolgálókra van szükség.</span><span class="sxs-lookup"><span data-stu-id="08860-122">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers toobe static or long-lived servers.</span></span>  <span data-ttu-id="08860-123">Javasoljuk, hogy hello virtuális hálózatokról (Vnetekről) és a hálózati biztonsági csoportokkal (NSG-k), például az Azure eszközök statikusnak kell lennie, és hosszabb élettartamú ritkán rendszerbe telepítendő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="08860-123">It is recommended that Azure assets like hello Virtual Networks (VNets) and Network Security Groups (NSGs), should be static and long lived resources that are rarely deployed.</span></span>  <span data-ttu-id="08860-124">Miután a virtuális hálózaton van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="08860-124">Once a VNet has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span>  <span data-ttu-id="08860-125">Toothis statikus hálózat Git hozzáadása tárház és Jenkins automation kiszolgáló nyújt CiCd tooyour fejlesztési vagy tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="08860-125">Adding toothis static network a Git repository server and a Jenkins automation server delivers CiCd tooyour development or test environments.</span></span>  

<span data-ttu-id="08860-126">Belső DNS-nevek csak feloldható egy Azure virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="08860-126">Internal DNS names are only resolvable inside an Azure virtual network.</span></span>  <span data-ttu-id="08860-127">Mivel hello DNS-nevek belső, azok nem feloldható toohello internet, így további biztonsági toohello infrastruktúra kívül.</span><span class="sxs-lookup"><span data-stu-id="08860-127">Because hello DNS names are internal, they are not resolvable toohello outside internet, providing additional security toohello infrastructure.</span></span>

<span data-ttu-id="08860-128">_Olyan cserélje le a saját elnevezési._</span><span class="sxs-lookup"><span data-stu-id="08860-128">_Replace any examples with your own naming._</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="08860-129">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="08860-129">Create hello Resource group</span></span>

<span data-ttu-id="08860-130">Erőforráscsoport minden ebben a bemutatóban létrehozhatunk szükséges tooorganize van.</span><span class="sxs-lookup"><span data-stu-id="08860-130">A Resource Group is needed tooorganize everything we create in this walkthrough.</span></span>  <span data-ttu-id="08860-131">További információ az Azure erőforráscsoportokkal: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="08860-131">For more information on Azure Resource Groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-hello-vnet"></a><span data-ttu-id="08860-132">Hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="08860-132">Create hello VNet</span></span>

<span data-ttu-id="08860-133">hello első lépés egy VNet toolaunch hello a virtuális gépek toobuild.</span><span class="sxs-lookup"><span data-stu-id="08860-133">hello first step is toobuild a VNet toolaunch hello VMs into.</span></span>  <span data-ttu-id="08860-134">virtuális hálózat hello külön alhálózatokon üzemeltetni az Ez az útmutató tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="08860-134">hello VNet contains one subnet for this walkthrough.</span></span>  <span data-ttu-id="08860-135">Az Azure Vnetekhez további információkért lásd: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="08860-135">For more information on Azure VNets, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-hello-nsg"></a><span data-ttu-id="08860-136">Hello NSG-t létrehozni</span><span class="sxs-lookup"><span data-stu-id="08860-136">Create hello NSG</span></span>

<span data-ttu-id="08860-137">hello alhálózati mögött egy meglévő hálózati biztonsági csoport épül, így azt build hello NSG hello alhálózati előtt.</span><span class="sxs-lookup"><span data-stu-id="08860-137">hello Subnet is built behind an existing Network Security Group so we build hello NSG before hello Subnet.</span></span>  <span data-ttu-id="08860-138">Az Azure NSG-k olyan egyenértékű tooa tűzfal hello hálózati rétegben.</span><span class="sxs-lookup"><span data-stu-id="08860-138">Azure NSGs are equivalent tooa firewall at hello network layer.</span></span>  <span data-ttu-id="08860-139">Az NSG-k Azure további információkért lásd: [hogyan toocreate NSG-ket a hello Azure parancssori felület](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="08860-139">For more information on Azure NSGs, see [How toocreate NSGs in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="08860-140">Egy bejövő SSH engedélyezési szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="08860-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="08860-141">Linux virtuális gép hello hello internet, egy szabályt, amely lehetővé teszi a bejövő portot 22 forgalom toobe továbbítja hello hálózati tooport 22 hello Linux virtuális gép van szükség a engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="08860-141">hello Linux VM needs access from hello internet so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello Linux VM is needed.</span></span>

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a><span data-ttu-id="08860-142">Egy alhálózat toohello virtuális hálózat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="08860-142">Add a subnet toohello VNet</span></span>

<span data-ttu-id="08860-143">Virtuális gépek hello virtuális hálózaton kell lennie egy alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="08860-143">VMs within hello VNet must be located in a subnet.</span></span>  <span data-ttu-id="08860-144">Minden egyes virtuális hálózat több alhálózattal is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="08860-144">Each VNet can have multiple subnets.</span></span>  <span data-ttu-id="08860-145">Hozzon létre hello alhálózatot, és hello alhálózatot hozzátársíthat hello NSG tooadd tűzfal toohello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="08860-145">Create hello subnet and associate hello subnet with hello NSG tooadd a firewall toohello subnet.</span></span>

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

<span data-ttu-id="08860-146">hello alhálózati most már hozzáadott hello virtuális hálózaton belül, és társított hello NSG és hello NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="08860-146">hello Subnet is now added inside hello VNet and associated with hello NSG and hello NSG rule.</span></span>

## <a name="creating-static-dns-names"></a><span data-ttu-id="08860-147">Statikus DNS-nevek létrehozása</span><span class="sxs-lookup"><span data-stu-id="08860-147">Creating static DNS names</span></span>

<span data-ttu-id="08860-148">Azure a rendkívül rugalmas, de virtuális gépek névfeloldás toouse DNS-neveit, kell őket, mint a virtuális hálózati kártyák (VNics), a címkézés DNS toocreate.</span><span class="sxs-lookup"><span data-stu-id="08860-148">Azure is very flexible, but toouse DNS names for VMs name resolution, you need toocreate them as Virtual network cards (VNics) using DNS labeling.</span></span>  <span data-ttu-id="08860-149">VNics fontosak, mivel újra felhasználhatja azokat csatlakoztatja őket toodifferent virtuális gépek, így hello VNic statikus erőforrásként hello virtuális gépek lehetnek ideiglenes.</span><span class="sxs-lookup"><span data-stu-id="08860-149">VNics are important as you can reuse them by connecting them toodifferent VMs, which keeps hello VNic as a static resource while hello VMs can be temporary.</span></span>  <span data-ttu-id="08860-150">A virtuális hálózati hello címkézés DNS használatával képes tooenable egyszerű név feloldása más virtuális gépek a virtuális hálózat hello folyamatban.</span><span class="sxs-lookup"><span data-stu-id="08860-150">By using DNS labeling on hello VNic, we are able tooenable simple name resolution from other VMs in hello VNet.</span></span>  <span data-ttu-id="08860-151">Feloldható nevek használata lehetővé teszi, hogy más virtuális gépek tooaccess hello fürtautomatizálási kiszolgáló hello DNS-neve alapján `Jenkins` vagy hello Git kiszolgálóra `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="08860-151">Using resolvable names enables other VMs tooaccess hello automation server by hello DNS name `Jenkins` or hello Git server as `gitrepo`.</span></span>  <span data-ttu-id="08860-152">Hozzon létre egy virtuális hálózati, és társítsa azt az alhálózati hello előző lépésben létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="08860-152">Create a VNic and associate it with hello Subnet created in hello previous step.</span></span>

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="08860-153">Hello virtuális hálózatot a virtuális gép hello és NSG központi telepítése</span><span class="sxs-lookup"><span data-stu-id="08860-153">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="08860-154">Most már van egy Vnetet, egy alhálózaton belül, hogy a virtuális hálózat, és működött egy tűzfal tooprotect az alhálózati 22-es port kivételével az összes bejövő forgalmát blokkolja az SSH egy NSG.</span><span class="sxs-lookup"><span data-stu-id="08860-154">We now have a VNet, a subnet inside that VNet, and an NSG acting as a firewall tooprotect our subnet by blocking all inbound traffic except port 22 for SSH.</span></span>  <span data-ttu-id="08860-155">hello VM mostantól a meglévő hálózati infrastruktúra belül telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="08860-155">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="08860-156">Hello Azure parancssori felület használatával, és hello `azure vm create` parancs hello Linux virtuális gép meglévő Azure-erőforráscsoportot, virtuális hálózatot, alhálózatot és virtuális hálózati telepített toohello.</span><span class="sxs-lookup"><span data-stu-id="08860-156">Using hello Azure CLI, and hello `azure vm create` command, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>  <span data-ttu-id="08860-157">További információ az hello CLI toodeploy a teljes virtuális gépek használatával, lásd: [hozzon létre egy teljes Linux környezetet hello Azure parancssori felület használatával](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="08860-157">For more information on using hello CLI toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

<span data-ttu-id="08860-158">Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, azt kérje meg az Azure toodeploy hello VM hello meglévő hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="08860-158">By using hello CLI flags toocall out existing resources, we instruct Azure toodeploy hello VM inside hello existing network.</span></span>  <span data-ttu-id="08860-159">tooreiterate, miután egy VNet és alhálózat van telepítve, azokat maradhatnak, statikus és végleges erőforráshoz, az Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="08860-159">tooreiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="08860-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08860-160">Next steps</span></span>
* [<span data-ttu-id="08860-161">Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva</span><span class="sxs-lookup"><span data-stu-id="08860-161">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="08860-162">Linux virtuális gép létrehozása az Azure-ban sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="08860-162">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
