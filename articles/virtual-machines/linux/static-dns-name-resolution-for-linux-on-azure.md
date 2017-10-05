---
title: "Belső DNS használata az Azure CLI 2.0-s VM névfeloldás |} Microsoft Docs"
description: "Hozzon létre a virtuális hálózati adapterek, és a belső DNS használata az Azure CLI 2.0 Azure VM-névfeloldás"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: 992920adb1ae3736d43cc5f0bbb2081a20a1674d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="f8450-103">Virtuális hálózati adapterek létrehozása és használata a belső DNS Azure VM-névfeloldás</span><span class="sxs-lookup"><span data-stu-id="f8450-103">Create virtual network interface cards and use internal DNS for VM name resolution on Azure</span></span>
<span data-ttu-id="f8450-104">Ez a cikk bemutatja, hogyan állítsa be a statikus belső DNS-nevek Linux virtuális gépek virtuális hálózati kártyák (vNics) és a DNS-címke nevek használata az Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="f8450-104">This article shows you how to set static internal DNS names for Linux VMs using virtual network interface cards (vNics) and DNS label names with the Azure CLI 2.0.</span></span> <span data-ttu-id="f8450-105">Az [Azure CLI 1.0-s](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f8450-105">You can also perform these steps with the [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f8450-106">Statikus DNS-nevek használhatók állandó infrastruktúra-szolgáltatásokat, mint egy Jenkins build, ez a dokumentum használt, vagy egy Git kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="f8450-106">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="f8450-107">Követelmények:</span><span class="sxs-lookup"><span data-stu-id="f8450-107">The requirements are:</span></span>

* [<span data-ttu-id="f8450-108">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="f8450-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="f8450-109">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="f8450-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="f8450-110">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="f8450-110">Quick commands</span></span>
<span data-ttu-id="f8450-111">Ha gyorsan feladatnak van szüksége, az alábbi szakasz részletesen a szükséges parancsokat.</span><span class="sxs-lookup"><span data-stu-id="f8450-111">If you need to quickly accomplish the task, the following section details the commands needed.</span></span> <span data-ttu-id="f8450-112">Részletes információkat és a környezetben az egyes lépések a dokumentum többi részén található [itt indítása](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="f8450-112">More detailed information and context for each step can be found in the rest of the document, [starting here](#detailed-walkthrough).</span></span> <span data-ttu-id="f8450-113">A következő lépésekkel lesz szüksége a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="f8450-113">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="f8450-114">Előfeltételek: Erőforráscsoport, virtuális hálózati és alhálózati, hálózati biztonsági csoport SSH bejövő.</span><span class="sxs-lookup"><span data-stu-id="f8450-114">Pre-Requirements: Resource Group, virtual network and subnet, Network Security Group with SSH inbound.</span></span>

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a><span data-ttu-id="f8450-115">Hozzon létre egy virtuális hálózati adaptert statikus belső DNS-név</span><span class="sxs-lookup"><span data-stu-id="f8450-115">Create a virtual network interface card with a static internal DNS name</span></span>
<span data-ttu-id="f8450-116">Hozzon létre a virtuális hálózati rendelkező [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="f8450-116">Create the vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="f8450-117">A `--internal-dns-name` CLI jelző van szeretné beállítani a DNS-címke, amely a virtuális hálózati kártya (vNic) statikus DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="f8450-117">The `--internal-dns-name` CLI flag is for setting the DNS label, which provides the static DNS name for the virtual network interface card (vNic).</span></span> <span data-ttu-id="f8450-118">Az alábbi példakód létrehozza a virtuális hálózati nevű `myNic`, úgy, hogy csatlakozik a `myVnet` virtuális hálózaton, és létrehoz egy belső DNS-név rekordot nevű `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="f8450-118">The following example creates a vNic named `myNic`, connects it to the `myVnet` virtual network, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a><span data-ttu-id="f8450-119">A virtuális gép üzembe helyezése, és csatlakozzon a virtuális hálózati</span><span class="sxs-lookup"><span data-stu-id="f8450-119">Deploy a VM and connect the vNic</span></span>
<span data-ttu-id="f8450-120">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="f8450-120">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="f8450-121">A `--nics` jelző kapcsolódik a virtuális hálózati a virtuális Gépet az Azure-bA a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="f8450-121">The `--nics` flag connects the vNic to the VM during the deployment to Azure.</span></span> <span data-ttu-id="f8450-122">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM` Azure felügyelt lemezek és a virtuális hálózati nevű rendeli `myNic` az előző lépésben:</span><span class="sxs-lookup"><span data-stu-id="f8450-122">The following example creates a VM named `myVM` with Azure Managed Disks and attaches the vNic named `myNic` from the preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="f8450-123">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="f8450-123">Detailed walkthrough</span></span>

<span data-ttu-id="f8450-124">A teljes folyamatos integrációt és a folyamatos üzembe helyezés (CiCd) Azure infrastruktúra megköveteli az egyes kiszolgálók statikus vagy hosszú élettartamú kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="f8450-124">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers to be static or long-lived servers.</span></span> <span data-ttu-id="f8450-125">Ajánlott, például a virtuális hálózatok és a hálózati biztonsági csoportok Azure eszközök statikus, és hosszabb élettartamú ritkán rendszerbe telepítendő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f8450-125">It is recommended that Azure assets like the virtual networks and Network Security Groups are static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="f8450-126">Ha egy virtuális hálózathoz van telepítve, azt bármely kedvezőtlen hatással van az infrastruktúra nélkül új központi telepítések során felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="f8450-126">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects to the infrastructure.</span></span> <span data-ttu-id="f8450-127">Később hozzáadhat egy Git-tárház kiszolgáló, vagy egy Jenkins fürtautomatizálási kiszolgáló CiCd továbbítja a fejlesztési vagy tesztelési környezetben a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="f8450-127">You can later add a Git repository server or a Jenkins automation server delivers CiCd to this virtual network for your development or test environments.</span></span>  

<span data-ttu-id="f8450-128">Belső DNS-nevek csak feloldható egy Azure virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="f8450-128">Internal DNS names are only resolvable inside an Azure virtual network.</span></span> <span data-ttu-id="f8450-129">Mivel a DNS-nevek belső, azok nem oldható fel, az internetet, az infrastruktúra egyéb biztonsági lépések.</span><span class="sxs-lookup"><span data-stu-id="f8450-129">Because the DNS names are internal, they are not resolvable to the outside internet, providing additional security to the infrastructure.</span></span>

<span data-ttu-id="f8450-130">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="f8450-130">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="f8450-131">Példa paraméter nevek a következők `myResourceGroup`, `myNic`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="f8450-131">Example parameter names include `myResourceGroup`, `myNic`, and `myVM`.</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="f8450-132">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8450-132">Create the resource group</span></span>
<span data-ttu-id="f8450-133">Először hozza létre az erőforráscsoport [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="f8450-133">First, create the resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="f8450-134">Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `westus` helye:</span><span class="sxs-lookup"><span data-stu-id="f8450-134">The following example creates a resource group named `myResourceGroup` in the `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a><span data-ttu-id="f8450-135">A virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8450-135">Create the virtual network</span></span>

<span data-ttu-id="f8450-136">A következő lépésre elindíthatja a virtuális gépeket a virtuális hálózat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f8450-136">The next step is to build a virtual network to launch the VMs into.</span></span> <span data-ttu-id="f8450-137">A virtuális hálózat külön alhálózatokon üzemeltetni az Ez az útmutató tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f8450-137">The virtual network contains one subnet for this walkthrough.</span></span> <span data-ttu-id="f8450-138">Egy Azure virtuális hálózatot további információkért lásd: [virtuális hálózat létrehozása az Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8450-138">For more information on Azure virtual networks, see [Create a virtual network by using the Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="f8450-139">A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="f8450-139">Create the virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="f8450-140">Az alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` és nevű alhálózat `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="f8450-140">The following example creates a virtual network named `myVnet` and subnet named `mySubnet`:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a><span data-ttu-id="f8450-141">A hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8450-141">Create the Network Security Group</span></span>
<span data-ttu-id="f8450-142">Azure hálózati biztonsági csoportokat a hálózati rétegben tűzfal egyenértékűek.</span><span class="sxs-lookup"><span data-stu-id="f8450-142">Azure Network Security Groups are equivalent to a firewall at the network layer.</span></span> <span data-ttu-id="f8450-143">Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [NSG-k létrehozása az Azure parancssori felület a](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8450-143">For more information about Network Security Groups, see [How to create NSGs in the Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="f8450-144">A hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="f8450-144">Create the network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="f8450-145">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="f8450-145">The following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a><span data-ttu-id="f8450-146">Egy bejövő szabályt, amely engedélyezi az SSH hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f8450-146">Add an inbound rule to allow SSH</span></span>
<span data-ttu-id="f8450-147">A hálózati biztonsági csoport a bejövő szabály felvétele [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="f8450-147">Add an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="f8450-148">Az alábbi példa létrehoz egy nevű szabályt `myRuleAllowSSH`:</span><span class="sxs-lookup"><span data-stu-id="f8450-148">The following example creates a rule named `myRuleAllowSSH`:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-the-subnet-with-the-network-security-group"></a><span data-ttu-id="f8450-149">Az alhálózatot a hálózati biztonsági csoporthoz társítandó</span><span class="sxs-lookup"><span data-stu-id="f8450-149">Associate the subnet with the Network Security Group</span></span>
<span data-ttu-id="f8450-150">Az alhálózatot a hálózati biztonsági csoporthoz társítandó, használja a [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update).</span><span class="sxs-lookup"><span data-stu-id="f8450-150">To associate the subnet with the Network Security Group, use [az network vnet subnet update](/cli/azure/network/vnet/subnet#update).</span></span> <span data-ttu-id="f8450-151">Az alábbi példa társítja az alhálózati név `mySubnet` együtt a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="f8450-151">The following example associates the subnet name `mySubnet` with the Network Security Group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a><span data-ttu-id="f8450-152">A virtuális hálózati kártya és a statikus DNS-nevek létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8450-152">Create the virtual network interface card and static DNS names</span></span>
<span data-ttu-id="f8450-153">Azure a rendkívül rugalmas, de szeretné használni a Virtuálisgép-név feloldása DNS-neveit, hozzon létre a virtuális hálózati kártyák (vNics), amely tartalmazza a DNS-címke kell.</span><span class="sxs-lookup"><span data-stu-id="f8450-153">Azure is very flexible, but to use DNS names for VM name resolution, you need to create virtual network interface cards (vNics) that include a DNS label.</span></span> <span data-ttu-id="f8450-154">vNics fontosak, újra felhasználhatja azokat keresztül az infrastruktúra-életciklus különböző virtuális gépek csatlakoztatja őket.</span><span class="sxs-lookup"><span data-stu-id="f8450-154">vNics are important as you can reuse them by connecting them to different VMs over the infrastructure lifecycle.</span></span> <span data-ttu-id="f8450-155">Ezt a módszert használja a virtuális hálózati tartja a statikus erőforrásként a virtuális gépek ideiglenes lehetnek.</span><span class="sxs-lookup"><span data-stu-id="f8450-155">This approach keeps the vNic as a static resource while the VMs can be temporary.</span></span> <span data-ttu-id="f8450-156">A virtuális címkézés DNS használatával azt is más virtuális gépek virtuális egyszerű névfeloldás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f8450-156">By using DNS labeling on the vNic, we are able to enable simple name resolution from other VMs in the VNet.</span></span> <span data-ttu-id="f8450-157">Feloldható nevek használata lehetővé teszi, hogy a DNS-neve alapján az automation-kiszolgáló eléréséhez más virtuális gépek `Jenkins` vagy a Git kiszolgálóra `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="f8450-157">Using resolvable names enables other VMs to access the automation server by the DNS name `Jenkins` or the Git server as `gitrepo`.</span></span>  

<span data-ttu-id="f8450-158">Hozzon létre a virtuális hálózati rendelkező [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="f8450-158">Create the vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="f8450-159">Az alábbi példakód létrehozza a virtuális hálózati nevű `myNic`, úgy, hogy csatlakozik a `myVnet` nevű virtuális hálózati `myVnet`, és létrehoz egy belső DNS-név rekordot nevű `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="f8450-159">The following example creates a vNic named `myNic`, connects it to the `myVnet` virtual network named `myVnet`, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a><span data-ttu-id="f8450-160">A virtuális gép üzembe helyezés a virtuális hálózati infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="f8450-160">Deploy the VM into the virtual network infrastructure</span></span>
<span data-ttu-id="f8450-161">Most már rendelkezik egy virtuális hálózat és a hálózati biztonsági csoport működött a tűzfal védi az alhálózati 22-es port kivételével minden bejövő forgalom blokkolása az SSH és a virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="f8450-161">We now have a virtual network and subnet, a Network Security Group acting as a firewall to protect our subnet by blocking all inbound traffic except port 22 for SSH, and a vNic.</span></span> <span data-ttu-id="f8450-162">Most már telepítheti a virtuális gépek a meglévő hálózati infrastruktúra belül.</span><span class="sxs-lookup"><span data-stu-id="f8450-162">You can now deploy a VM inside this existing network infrastructure.</span></span>

<span data-ttu-id="f8450-163">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="f8450-163">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="f8450-164">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM` Azure felügyelt lemezek és a virtuális hálózati nevű rendeli `myNic` az előző lépésben:</span><span class="sxs-lookup"><span data-stu-id="f8450-164">The following example creates a VM named `myVM` with Azure Managed Disks and attaches the vNic named `myNic` from the preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="f8450-165">A parancssori felület jelzők használatával hívásához a meglévő erőforrásokat, azt kérje meg a virtuális Gépet a meglévő hálózaton telepítendő Azure.</span><span class="sxs-lookup"><span data-stu-id="f8450-165">By using the CLI flags to call out existing resources, we instruct Azure to deploy the VM inside the existing network.</span></span> <span data-ttu-id="f8450-166">Megújítja, miután egy VNet és alhálózat van telepítve, hogy azok maradhatnak, statikus vagy állandó erőforrásként belül az Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="f8450-166">To reiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="f8450-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8450-167">Next steps</span></span>
* [<span data-ttu-id="f8450-168">Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva</span><span class="sxs-lookup"><span data-stu-id="f8450-168">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="f8450-169">Linux virtuális gép létrehozása az Azure-ban sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="f8450-169">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
