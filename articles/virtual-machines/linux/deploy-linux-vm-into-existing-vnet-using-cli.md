---
title: "az Azure CLI 2.0 meglévő hálózati Linux virtuális gépek aaaDeploy |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy egy Linux virtuális gép egy meglévő virtuális hálózat használatával történő hello Azure CLI 2.0"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0df44b3437002df050db56f3b3899083fb49d803
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli"></a><span data-ttu-id="b826f-103">Hogyan toodeploy hello Azure CLI-t a meglévő Azure virtuális hálózat a Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b826f-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure CLI</span></span>

<span data-ttu-id="b826f-104">Ez a cikk bemutatja, hogyan toouse hello Azure CLI 2.0 toodeploy egy virtuális gép (VM) be egy meglévő virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="b826f-104">This article shows you how toouse hello Azure CLI 2.0 toodeploy a virtual machine (VM) into an existing virtual network.</span></span> <span data-ttu-id="b826f-105">hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="b826f-105">hello requirements are:</span></span>

- [<span data-ttu-id="b826f-106">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="b826f-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="b826f-107">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="b826f-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

<span data-ttu-id="b826f-108">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b826f-108">You can also perform these steps with hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="b826f-109">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="b826f-109">Quick Commands</span></span>
<span data-ttu-id="b826f-110">Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="b826f-110">If you need tooquickly accomplish hello task, hello following section details hello  commands needed.</span></span> <span data-ttu-id="b826f-111">Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="b826f-111">More detailed information and context for each step can be found hello rest of hello document, [starting here](#detailed-walkthrough).</span></span>

<span data-ttu-id="b826f-112">toocreate egyéni ebben a környezetben, hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="b826f-112">toocreate this custom environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="b826f-113">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="b826f-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="b826f-114">Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="b826f-114">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

<span data-ttu-id="b826f-115">**Előfeltételek:** Azure-erőforráscsoportot, a virtuális hálózat és az alhálózat, a hálózati biztonsági csoport SSH bejövő, és a virtuális hálózati kártya.</span><span class="sxs-lookup"><span data-stu-id="b826f-115">**Pre-requirements:** Azure resource group, virtual network and subnet, network security group with SSH inbound, and a virtual network interface card.</span></span>

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="b826f-116">Hello VM be hello virtuális hálózati infrastruktúra telepítése</span><span class="sxs-lookup"><span data-stu-id="b826f-116">Deploy hello VM into hello virtual network infrastructure</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="b826f-117">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="b826f-117">Detailed walkthrough</span></span>

<span data-ttu-id="b826f-118">Az Azure eszközök, például a virtuális hálózatok és a hálózati biztonsági csoportok statikusnak kell lennie, és hosszabb élettartamú ritkán telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="b826f-118">Azure assets like virtual networks and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="b826f-119">Ha egy virtuális hálózathoz van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="b826f-119">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="b826f-120">Gondolja át, hogy egy hagyományos hardver hálózati kapcsoló a virtuális hálózat – nem kell minden egyes központi telepítés kapcsoló egy új hardver tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="b826f-120">Think about a virtual network as being a traditional hardware network switch - you would not need tooconfigure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="b826f-121">Egy megfelelően konfigurált virtuális hálózattal, folytathatja a toodeploy új virtuális gépekre történő kevés és újra a virtuális hálózatban, bármilyen szükséges módosítását hello virtuális hálózati hello élettartama során.</span><span class="sxs-lookup"><span data-stu-id="b826f-121">With a correctly configured virtual network, you can continue toodeploy new VMs into that virtual network over and over with few, if any, changes required over hello life of hello virtual network.</span></span>

<span data-ttu-id="b826f-122">toocreate egyéni ebben a környezetben, hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="b826f-122">toocreate this custom environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="b826f-123">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="b826f-123">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="b826f-124">Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="b826f-124">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="b826f-125">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b826f-125">Create hello resource group</span></span>

<span data-ttu-id="b826f-126">Először hozzon létre egy Azure-erőforrás csoport tooorganize mindent hoz létre ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="b826f-126">First, create an Azure resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="b826f-127">További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b826f-127">For more information on resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="b826f-128">Hello erőforráscsoport létrehozása [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b826f-128">Create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="b826f-129">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="b826f-129">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-hello-virtual-network"></a><span data-ttu-id="b826f-130">Hello virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b826f-130">Create hello virtual network</span></span>

<span data-ttu-id="b826f-131">Lehetővé teszi, hogy az Azure-beli virtuális hálózat toolaunch hello a virtuális gépek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b826f-131">Lets build an Azure virtual network toolaunch hello VMs into.</span></span> <span data-ttu-id="b826f-132">További információ a virtuális hálózatok: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b826f-132">For more information on virtual networks, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span></span> <span data-ttu-id="b826f-133">Hozzon létre virtuális hálózat hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="b826f-133">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="b826f-134">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="b826f-134">hello following example creates a virtual network named *myVnet* and subnet named *mySubnet*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="b826f-135">Hello hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b826f-135">Create hello network security group</span></span>

<span data-ttu-id="b826f-136">Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben.</span><span class="sxs-lookup"><span data-stu-id="b826f-136">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="b826f-137">Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hogyan toocreate hálózati biztonsági csoportok, az Azure CLI hello](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b826f-137">For more information on network security groups, see [How toocreate network security groups in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span> <span data-ttu-id="b826f-138">Hello hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="b826f-138">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="b826f-139">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="b826f-139">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="b826f-140">Egy bejövő SSH engedélyezési szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b826f-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="b826f-141">virtuális gép hello kell elérnie az interneten, így lehetővé téve a 22-es portot a bejövő forgalom toobe szabály továbbítja hello hálózati tooport 22-es hello VM szükséges hello.</span><span class="sxs-lookup"><span data-stu-id="b826f-141">hello VM needs access from hello internet, so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is needed.</span></span> <span data-ttu-id="b826f-142">Egy bejövő szabályt az hello hálózati biztonsági csoport hozzáadása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="b826f-142">Add an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="b826f-143">hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="b826f-143">hello following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-hello-subnet-toohello-network-security-group"></a><span data-ttu-id="b826f-144">Hello alhálózati toohello hálózati biztonsági csoport csatolása</span><span class="sxs-lookup"><span data-stu-id="b826f-144">Attach hello subnet toohello network security group</span></span>

<span data-ttu-id="b826f-145">hálózati biztonsági csoportszabályok hello lehet alkalmazott tooa alhálózatot vagy egy adott virtuális hálózati adapteren.</span><span class="sxs-lookup"><span data-stu-id="b826f-145">hello network security group rules can be applied tooa subnet or a specific virtual network interface.</span></span> <span data-ttu-id="b826f-146">Lehetővé teszi, hogy hello hálózati biztonsági csoport tooour alhálózati csatolni.</span><span class="sxs-lookup"><span data-stu-id="b826f-146">Lets attach hello network security group tooour subnet.</span></span> <span data-ttu-id="b826f-147">Csatlakoztassa a alhálózati toohello hálózati biztonsági csoport [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update):</span><span class="sxs-lookup"><span data-stu-id="b826f-147">Attach your subnet toohello network security group with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update):</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-toohello-subnet"></a><span data-ttu-id="b826f-148">Adjon hozzá egy virtuális hálózati illesztő kártya toohello alhálózatot</span><span class="sxs-lookup"><span data-stu-id="b826f-148">Add a virtual network interface card toohello subnet</span></span>

<span data-ttu-id="b826f-149">Virtuális hálózati kártyák (VNics) fontosak, újra felhasználhatja azokat toodifferent virtuális gépek csatlakoztatja őket.</span><span class="sxs-lookup"><span data-stu-id="b826f-149">Virtual network interface cards (VNics) are important as you can reuse them by connecting them toodifferent VMs.</span></span> <span data-ttu-id="b826f-150">Az ismételt lehetővé teszi a tookeep hello VNic statikus erőforrásként hello virtuális gépek ideiglenes lehetnek.</span><span class="sxs-lookup"><span data-stu-id="b826f-150">This reuse allows you tookeep hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="b826f-151">Hozzon létre egy virtuális hálózati, és társítsa azt az alhálózat hello [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="b826f-151">Create a VNic and associate it with hello subnet with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="b826f-152">hello alábbi példa létrehoz egy virtuális hálózati nevű *myNic*:</span><span class="sxs-lookup"><span data-stu-id="b826f-152">hello following example creates a VNic named *myNic*:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="b826f-153">Hello VM be hello virtuális hálózati infrastruktúra telepítése</span><span class="sxs-lookup"><span data-stu-id="b826f-153">Deploy hello VM into hello virtual network infrastructure</span></span>

<span data-ttu-id="b826f-154">Most már rendelkezik egy virtuális hálózat és alhálózat és a hálózati biztonsági csoport tooprotect hello alhálózati 22-es port kivételével az összes bejövő forgalmát blokkolja az SSH.</span><span class="sxs-lookup"><span data-stu-id="b826f-154">You now have a virtual network and subnet, and a network security group tooprotect hello subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="b826f-155">hello VM mostantól a meglévő hálózati infrastruktúra belül telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="b826f-155">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="b826f-156">A virtuális gép a létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="b826f-156">Create your VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="b826f-157">Hello bővebben jelzők a hello Azure CLI 2.0 toodeploy toouse egy teljes virtuális Gépet, a következő témakörben: [hozzon létre egy teljes Linux környezetet hello Azure parancssori felület használatával](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="b826f-157">For more information on hello flags toouse with hello Azure CLI 2.0 toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md).</span></span>

<span data-ttu-id="b826f-158">a következő példa hello hoz létre a virtuális gépek Azure felügyelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="b826f-158">hello following example creates a VM using Azure Managed Disks.</span></span> <span data-ttu-id="b826f-159">Ezeknek a lemezeknek hello Azure platform kezeli, és nem igényelnek bármely előkészítő vagy a hely toostore őket.</span><span class="sxs-lookup"><span data-stu-id="b826f-159">These disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="b826f-160">További információ a felügyelt lemezekről: [Azure Managed Disks – áttekintés](../../storage/storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b826f-160">For more information about managed disks, see [Azure Managed Disks overview](../../storage/storage-managed-disks-overview.md).</span></span> <span data-ttu-id="b826f-161">Ha a nem felügyelt toouse lemezek, lásd az alábbi hello további megjegyzést.</span><span class="sxs-lookup"><span data-stu-id="b826f-161">If you wish toouse unmanaged disks, see hello additional note below.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

<span data-ttu-id="b826f-162">Ha felügyelt lemezek használja, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="b826f-162">If you use managed disks, skip this step.</span></span> <span data-ttu-id="b826f-163">Ha a nem felügyelt toouse lemezek, a következő további paraméterek nem felügyelt toocreate toohello eljárás parancs lemezek nevű hello tárfiók tooadd hello kell `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="b826f-163">If you wish toouse unmanaged disks, you need tooadd hello following additional parameters toohello proceeding command toocreate unmanaged disks in hello storage account named `mystorageaccount`:</span></span> 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

<span data-ttu-id="b826f-164">Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, az Azure toodeploy hello VM hello meglévő hálózaton belüli utasította.</span><span class="sxs-lookup"><span data-stu-id="b826f-164">By using hello CLI flags toocall out existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="b826f-165">Ha egy virtuális hálózat és alhálózat van telepítve, azokat statikus vagy állandó erőforráshoz, az Azure-régió, mint maradhatnak.</span><span class="sxs-lookup"><span data-stu-id="b826f-165">Once a virtual network and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span> <span data-ttu-id="b826f-166">Ebben a példában nem volt létrehozni és hozzárendelni egy nyilvános IP-cím toohello VNic, ezért a virtuális gép nincs nyilvánosan elérhető internetes hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="b826f-166">In this example, you did not create and assign a public IP address toohello VNic, so this VM is not publicly accessible over hello Internet.</span></span> <span data-ttu-id="b826f-167">További információkért lásd: [hozzon létre egy virtuális Gépet egy statikus nyilvános IP-cím hello Azure parancssori felület használatával](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b826f-167">For more information, see [Create a VM with a static public IP using hello Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b826f-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b826f-168">Next steps</span></span>
<span data-ttu-id="b826f-169">Többféleképpen toocreate virtuális gépek Azure-ban kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="b826f-169">For more information about ways toocreate virtual machines in Azure, see hello following resources:</span></span>

* [<span data-ttu-id="b826f-170">Az Azure Resource Manager sablon toocreate egy adott központi telepítés használata</span><span class="sxs-lookup"><span data-stu-id="b826f-170">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* [<span data-ttu-id="b826f-171">Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva</span><span class="sxs-lookup"><span data-stu-id="b826f-171">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md)
* [<span data-ttu-id="b826f-172">Linux virtuális gép létrehozása az Azure-ban sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="b826f-172">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
