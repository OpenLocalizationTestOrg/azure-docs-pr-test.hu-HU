---
title: "aaaUse a virtuális gép belső DNS névfeloldás a hello Azure CLI 2.0 |} Microsoft Docs"
description: "Hogyan toocreate virtuális hálózati kártyák és a virtuális gép névfeloldáshoz Azure hello Azure CLI 2.0 a belső DNS használt"
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
ms.openlocfilehash: b3c4bfd3ab698f7b25d763ba9e60dd7984f6269d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a><span data-ttu-id="cfb13-103">Virtuális hálózati adapterek létrehozása és használata a belső DNS Azure VM-névfeloldás</span><span class="sxs-lookup"><span data-stu-id="cfb13-103">Create virtual network interface cards and use internal DNS for VM name resolution on Azure</span></span>
<span data-ttu-id="cfb13-104">Ez a cikk bemutatja, hogyan tooset statikus belső DNS-neveit Linux virtuális gépek virtuális hálózati kártyák (vNics) és a DNS-címke nevek a hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="cfb13-104">This article shows you how tooset static internal DNS names for Linux VMs using virtual network interface cards (vNics) and DNS label names with hello Azure CLI 2.0.</span></span> <span data-ttu-id="cfb13-105">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cfb13-105">You can also perform these steps with hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="cfb13-106">Statikus DNS-nevek használhatók állandó infrastruktúra-szolgáltatásokat, mint egy Jenkins build, ez a dokumentum használt, vagy egy Git kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="cfb13-106">Static DNS names are used for permanent infrastructure services like a Jenkins build server, which is used for this document, or a Git server.</span></span>

<span data-ttu-id="cfb13-107">hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="cfb13-107">hello requirements are:</span></span>

* [<span data-ttu-id="cfb13-108">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="cfb13-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* [<span data-ttu-id="cfb13-109">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="cfb13-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="cfb13-110">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="cfb13-110">Quick commands</span></span>
<span data-ttu-id="cfb13-111">Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="cfb13-111">If you need tooquickly accomplish hello task, hello following section details hello commands needed.</span></span> <span data-ttu-id="cfb13-112">Részletes információkat és a környezetben az egyes lépéseit itt található: hello dokumentum többi részén hello, [itt indítása](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="cfb13-112">More detailed information and context for each step can be found in hello rest of hello document, [starting here](#detailed-walkthrough).</span></span> <span data-ttu-id="cfb13-113">Ezek a lépések tooperform, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="cfb13-113">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="cfb13-114">Előfeltételek: Erőforráscsoport, virtuális hálózati és alhálózati, hálózati biztonsági csoport SSH bejövő.</span><span class="sxs-lookup"><span data-stu-id="cfb13-114">Pre-Requirements: Resource Group, virtual network and subnet, Network Security Group with SSH inbound.</span></span>

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a><span data-ttu-id="cfb13-115">Hozzon létre egy virtuális hálózati adaptert statikus belső DNS-név</span><span class="sxs-lookup"><span data-stu-id="cfb13-115">Create a virtual network interface card with a static internal DNS name</span></span>
<span data-ttu-id="cfb13-116">Hozzon létre a hello vNic [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="cfb13-116">Create hello vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="cfb13-117">Hello `--internal-dns-name` CLI jelző csak az beállítás hello DNS-címke, amely hello virtuális hálózati kártya (vNic) hello statikus DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="cfb13-117">hello `--internal-dns-name` CLI flag is for setting hello DNS label, which provides hello static DNS name for hello virtual network interface card (vNic).</span></span> <span data-ttu-id="cfb13-118">hello alábbi példa létrehoz egy virtuális hálózati nevű `myNic`, toohello csatlakoztató `myVnet` virtuális hálózaton, és létrehoz egy belső DNS-név rekordot nevű `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="cfb13-118">hello following example creates a vNic named `myNic`, connects it toohello `myVnet` virtual network, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-hello-vnic"></a><span data-ttu-id="cfb13-119">A virtuális gép üzembe helyezése, és csatlakozzon a hello vNic</span><span class="sxs-lookup"><span data-stu-id="cfb13-119">Deploy a VM and connect hello vNic</span></span>
<span data-ttu-id="cfb13-120">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="cfb13-120">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="cfb13-121">Hello `--nics` jelző hello vNic toohello Virtuálisgép kapcsolódik hello telepítési tooAzure során.</span><span class="sxs-lookup"><span data-stu-id="cfb13-121">hello `--nics` flag connects hello vNic toohello VM during hello deployment tooAzure.</span></span> <span data-ttu-id="cfb13-122">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` Azure felügyelt lemezek és rendeli hello vNic nevű `myNic` az előző lépésben hello:</span><span class="sxs-lookup"><span data-stu-id="cfb13-122">hello following example creates a VM named `myVM` with Azure Managed Disks and attaches hello vNic named `myNic` from hello preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="cfb13-123">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="cfb13-123">Detailed walkthrough</span></span>

<span data-ttu-id="cfb13-124">A teljes folyamatos integrációt és a folyamatos üzembe helyezés (CiCd) Azure-beli infrastruktúra egyes kiszolgálók toobe statikus vagy hosszú élettartamú kiszolgálókra van szükség.</span><span class="sxs-lookup"><span data-stu-id="cfb13-124">A full continuous integration and continuous deployment (CiCd) infrastructure on Azure requires certain servers toobe static or long-lived servers.</span></span> <span data-ttu-id="cfb13-125">Ajánlott, például a virtuális hálózatok és a hálózati biztonsági csoportok hello Azure eszközök statikus, és hosszabb élettartamú ritkán rendszerbe telepítendő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cfb13-125">It is recommended that Azure assets like hello virtual networks and Network Security Groups are static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="cfb13-126">Ha egy virtuális hálózathoz van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="cfb13-126">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="cfb13-127">Később hozzáadhat egy Git-tárház kiszolgáló, vagy egy Jenkins automation kiszolgáló kézbesíti CiCd toothis virtuális hálózat a fejlesztési vagy tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="cfb13-127">You can later add a Git repository server or a Jenkins automation server delivers CiCd toothis virtual network for your development or test environments.</span></span>  

<span data-ttu-id="cfb13-128">Belső DNS-nevek csak feloldható egy Azure virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="cfb13-128">Internal DNS names are only resolvable inside an Azure virtual network.</span></span> <span data-ttu-id="cfb13-129">Mivel hello DNS-nevek belső, azok nem feloldható toohello internet, így további biztonsági toohello infrastruktúra kívül.</span><span class="sxs-lookup"><span data-stu-id="cfb13-129">Because hello DNS names are internal, they are not resolvable toohello outside internet, providing additional security toohello infrastructure.</span></span>

<span data-ttu-id="cfb13-130">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="cfb13-130">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cfb13-131">Példa paraméter nevek a következők `myResourceGroup`, `myNic`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="cfb13-131">Example parameter names include `myResourceGroup`, `myNic`, and `myVM`.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="cfb13-132">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfb13-132">Create hello resource group</span></span>
<span data-ttu-id="cfb13-133">Először hozza létre az erőforráscsoport hello [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="cfb13-133">First, create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="cfb13-134">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helye:</span><span class="sxs-lookup"><span data-stu-id="cfb13-134">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-hello-virtual-network"></a><span data-ttu-id="cfb13-135">Hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfb13-135">Create hello virtual network</span></span>

<span data-ttu-id="cfb13-136">hello következő lépés egy virtuális hálózati toolaunch hello a virtuális gépek toobuild.</span><span class="sxs-lookup"><span data-stu-id="cfb13-136">hello next step is toobuild a virtual network toolaunch hello VMs into.</span></span> <span data-ttu-id="cfb13-137">hello virtuális hálózat Ez a forgatókönyv egy alhálózatot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cfb13-137">hello virtual network contains one subnet for this walkthrough.</span></span> <span data-ttu-id="cfb13-138">Egy Azure virtuális hálózatot további információkért lásd: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cfb13-138">For more information on Azure virtual networks, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="cfb13-139">Hozzon létre virtuális hálózat hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="cfb13-139">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="cfb13-140">hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` és nevű alhálózat `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="cfb13-140">hello following example creates a virtual network named `myVnet` and subnet named `mySubnet`:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="cfb13-141">Hello hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfb13-141">Create hello Network Security Group</span></span>
<span data-ttu-id="cfb13-142">Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben.</span><span class="sxs-lookup"><span data-stu-id="cfb13-142">Azure Network Security Groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="cfb13-143">Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hogyan toocreate NSG-ket a hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cfb13-143">For more information about Network Security Groups, see [How toocreate NSGs in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="cfb13-144">Hello hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="cfb13-144">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="cfb13-145">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="cfb13-145">hello following example creates a network security group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-tooallow-ssh"></a><span data-ttu-id="cfb13-146">Egy bejövő forgalomra vonatkozó szabály tooallow SSH hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cfb13-146">Add an inbound rule tooallow SSH</span></span>
<span data-ttu-id="cfb13-147">Egy bejövő szabályt az hello hálózati biztonsági csoport hozzáadása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="cfb13-147">Add an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="cfb13-148">hello alábbi példa létrehoz egy nevű szabályt `myRuleAllowSSH`:</span><span class="sxs-lookup"><span data-stu-id="cfb13-148">hello following example creates a rule named `myRuleAllowSSH`:</span></span>

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

## <a name="associate-hello-subnet-with-hello-network-security-group"></a><span data-ttu-id="cfb13-149">Hello alhálózatot hozzátársíthat hello hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="cfb13-149">Associate hello subnet with hello Network Security Group</span></span>
<span data-ttu-id="cfb13-150">tooassociate hello alhálózat hello hálózati biztonsági csoportot használjon [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update).</span><span class="sxs-lookup"><span data-stu-id="cfb13-150">tooassociate hello subnet with hello Network Security Group, use [az network vnet subnet update](/cli/azure/network/vnet/subnet#update).</span></span> <span data-ttu-id="cfb13-151">hello alábbi példa társítja hello alhálózati név `mySubnet` a hálózati biztonsági csoport nevű hello `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="cfb13-151">hello following example associates hello subnet name `mySubnet` with hello Network Security Group named `myNetworkSecurityGroup`:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-hello-virtual-network-interface-card-and-static-dns-names"></a><span data-ttu-id="cfb13-152">Hello virtuális hálózati kártya és a statikus DNS-nevek létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfb13-152">Create hello virtual network interface card and static DNS names</span></span>
<span data-ttu-id="cfb13-153">Azure a rendkívül rugalmas, de névfeloldás VM toouse DNS-neveit, kell toocreate virtuális hálózati kártyák (vNics), amely tartalmazza a DNS-címke.</span><span class="sxs-lookup"><span data-stu-id="cfb13-153">Azure is very flexible, but toouse DNS names for VM name resolution, you need toocreate virtual network interface cards (vNics) that include a DNS label.</span></span> <span data-ttu-id="cfb13-154">vNics fontosak, mivel újra felhasználhatja azokat csatlakoztatja őket toodifferent virtuális gépek hello infrastruktúra életciklusa alatt.</span><span class="sxs-lookup"><span data-stu-id="cfb13-154">vNics are important as you can reuse them by connecting them toodifferent VMs over hello infrastructure lifecycle.</span></span> <span data-ttu-id="cfb13-155">Ezt a módszert hello vNic statikus erőforrásként tartja a virtuális gépek hello ideiglenes lehetnek.</span><span class="sxs-lookup"><span data-stu-id="cfb13-155">This approach keeps hello vNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="cfb13-156">Hello virtuális címkézés DNS használatával képes tooenable egyszerű név feloldása más virtuális gépek a virtuális hálózat hello folyamatban.</span><span class="sxs-lookup"><span data-stu-id="cfb13-156">By using DNS labeling on hello vNic, we are able tooenable simple name resolution from other VMs in hello VNet.</span></span> <span data-ttu-id="cfb13-157">Feloldható nevek használata lehetővé teszi, hogy más virtuális gépek tooaccess hello fürtautomatizálási kiszolgáló hello DNS-neve alapján `Jenkins` vagy hello Git kiszolgálóra `gitrepo`.</span><span class="sxs-lookup"><span data-stu-id="cfb13-157">Using resolvable names enables other VMs tooaccess hello automation server by hello DNS name `Jenkins` or hello Git server as `gitrepo`.</span></span>  

<span data-ttu-id="cfb13-158">Hozzon létre a hello vNic [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="cfb13-158">Create hello vNic with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="cfb13-159">hello alábbi példa létrehoz egy virtuális hálózati nevű `myNic`, toohello csatlakoztató `myVnet` nevű virtuális hálózati `myVnet`, és létrehoz egy belső DNS-név rekordot nevű `jenkins`:</span><span class="sxs-lookup"><span data-stu-id="cfb13-159">hello following example creates a vNic named `myNic`, connects it toohello `myVnet` virtual network named `myVnet`, and creates an internal DNS name record called `jenkins`:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="cfb13-160">Hello VM be hello virtuális hálózati infrastruktúra telepítése</span><span class="sxs-lookup"><span data-stu-id="cfb13-160">Deploy hello VM into hello virtual network infrastructure</span></span>
<span data-ttu-id="cfb13-161">Most már rendelkezik egy virtuális hálózat és alhálózat, a hálózati biztonsági csoport működött egy tűzfal tooprotect az alhálózat által az SSH és a virtuális hálózati 22-es port kivételével minden bejövő forgalom blokkolása.</span><span class="sxs-lookup"><span data-stu-id="cfb13-161">We now have a virtual network and subnet, a Network Security Group acting as a firewall tooprotect our subnet by blocking all inbound traffic except port 22 for SSH, and a vNic.</span></span> <span data-ttu-id="cfb13-162">Most már telepítheti a virtuális gépek a meglévő hálózati infrastruktúra belül.</span><span class="sxs-lookup"><span data-stu-id="cfb13-162">You can now deploy a VM inside this existing network infrastructure.</span></span>

<span data-ttu-id="cfb13-163">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="cfb13-163">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="cfb13-164">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` Azure felügyelt lemezek és rendeli hello vNic nevű `myNic` az előző lépésben hello:</span><span class="sxs-lookup"><span data-stu-id="cfb13-164">hello following example creates a VM named `myVM` with Azure Managed Disks and attaches hello vNic named `myNic` from hello preceding step:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="cfb13-165">Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, azt kérje meg az Azure toodeploy hello VM hello meglévő hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="cfb13-165">By using hello CLI flags toocall out existing resources, we instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="cfb13-166">tooreiterate, miután egy VNet és alhálózat van telepítve, azokat maradhatnak, statikus és végleges erőforráshoz, az Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="cfb13-166">tooreiterate, once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="cfb13-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cfb13-167">Next steps</span></span>
* [<span data-ttu-id="cfb13-168">Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva</span><span class="sxs-lookup"><span data-stu-id="cfb13-168">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="cfb13-169">Linux virtuális gép létrehozása az Azure-ban sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="cfb13-169">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
