---
title: "aaaConvert egy Azure virtuális gép tooa méretezési |} Microsoft Docs"
description: "Létrehozhat és telepíthet a Linux Azure virtuálisgép-méretezési hello Azure CLI állítható be."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/05/2017
ms.author: adegeo
ms.openlocfilehash: e228282ac4855cef589b8500e74e9d461f9aed84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-an-existing-azure-virtual-machine-tooa-scale-set"></a><span data-ttu-id="5e427-103">Alakítsa át egy meglévő Azure-beli virtuális gép tooa méretezési</span><span class="sxs-lookup"><span data-stu-id="5e427-103">Convert an existing Azure virtual machine tooa scale set</span></span>

<span data-ttu-id="5e427-104">Az oktatóanyag bemutatja, hogyan toouse Azure CLI 2.0 tooconvert egy virtuális gép tooa virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="5e427-104">This tutorial shows you how toouse Azure CLI 2.0 tooconvert a virtual machine tooa virtual machine scale set.</span></span> <span data-ttu-id="5e427-105">Azt is megtudhatja, hogyan tooautomate hello konfigurációs hello virtuális gépek méretezési hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="5e427-105">You also learn how tooautomate hello configuration of hello virtual machines in hello scale set.</span></span> <span data-ttu-id="5e427-106">További információ a hogyan tooinstall 2.0, az Azure CLI: [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5e427-106">For more information on how tooinstall Azure CLI 2.0, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span> <span data-ttu-id="5e427-107">Méretezési csoportok kapcsolatos további információkért lásd: [virtuálisgép-skálázási készletekben](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5e427-107">For more information about scale sets, see [Virtual Machine Scale Sets](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span></span>

## <a name="step-1---deprovision-hello-vm"></a><span data-ttu-id="5e427-108">1. lépés – hello VM kiosztásának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="5e427-108">Step 1 - Deprovision hello VM</span></span>

<span data-ttu-id="5e427-109">SSH tooconnect toohello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="5e427-109">Use SSH tooconnect toohello VM.</span></span>

<span data-ttu-id="5e427-110">Virtuális gép deprovision hello segítségével hello Azure virtuális gép ügynök toodelete fájlokat és adatokat.</span><span class="sxs-lookup"><span data-stu-id="5e427-110">Deprovision hello VM using hello Azure VM agent toodelete files and data.</span></span> <span data-ttu-id="5e427-111">A megszüntetés részletes áttekintése, lásd: [Linux virtuális gép rögzítése](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="5e427-111">For a detailed overview of deprovisioning, see [Capture a Linux virtual machine](capture-image.md).</span></span>

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-hello-vm"></a><span data-ttu-id="5e427-112">2. lépés – hello virtuális gép képének rögzítése</span><span class="sxs-lookup"><span data-stu-id="5e427-112">Step 2 - Capture an image of hello VM</span></span>

<span data-ttu-id="5e427-113">Részletes megtudhatja, hogy a, [Linux virtuális gép rögzítése](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="5e427-113">For a detailed overview of capturing, see [Capture a Linux virtual machine](capture-image.md).</span></span>

<span data-ttu-id="5e427-114">Felszabadítani a virtuális gép és hello [az virtuális gép felszabadítása](/cli/azure/vm#deallocate):</span><span class="sxs-lookup"><span data-stu-id="5e427-114">Deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate):</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5e427-115">Generalize tulajdonsággal rendelkező virtuális gépet hello [az vm generalize](/cli/azure/vm#generalize):</span><span class="sxs-lookup"><span data-stu-id="5e427-115">Generalize hello VM with [az vm generalize](/cli/azure/vm#generalize):</span></span>

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5e427-116">Kép készítése hello VM erőforrás [az lemezkép létrehozása](/cli/azure/image#create):</span><span class="sxs-lookup"><span data-stu-id="5e427-116">Create an image from hello VM resource with [az image create](/cli/azure/image#create):</span></span>

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-hello-scale-set"></a><span data-ttu-id="5e427-117">3. lépés – hello méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e427-117">Step 3 - Create hello scale set</span></span>

<span data-ttu-id="5e427-118">Első hello **azonosító** hello kép.</span><span class="sxs-lookup"><span data-stu-id="5e427-118">Get hello **id** of hello image.</span></span>

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

<span data-ttu-id="5e427-119">A kép erőforrás a virtuális gép létrehozása [az vmss létrehozása](/cli/azure/vmss#create):</span><span class="sxs-lookup"><span data-stu-id="5e427-119">Create a VM from your image resource with [az vmss create](/cli/azure/vmss#create):</span></span>

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

<span data-ttu-id="5e427-120">Ez a parancs is 10 GB-os adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="5e427-120">This command also attached a 10gb data disk.</span></span> <span data-ttu-id="5e427-121">Ne feledje, hogy attól függően, hogy a virtuális gép hello kiválasztott méret (használtuk **Standard_DS1_v2**), az adatlemezek megengedett hello száma nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="5e427-121">Keep in mind that depending on hello VM size chosen (we used **Standard_DS1_v2**), hello number of data disks allowed is different.</span></span> <span data-ttu-id="5e427-122">További információkért tekintse át a hello [virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="5e427-122">For more information, review hello [virtual machine sizes](sizes.md).</span></span>

<span data-ttu-id="5e427-123">Miután hello méretezési befejeződik, csatlakoztassa a tooit.</span><span class="sxs-lookup"><span data-stu-id="5e427-123">Once hello scale set finishes, connect tooit.</span></span> <span data-ttu-id="5e427-124">IP-címek listájának lekérése az SSH hello példányok [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info):</span><span class="sxs-lookup"><span data-stu-id="5e427-124">Get a list of IP addresses for hello instances for SSH with [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info):</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

<span data-ttu-id="5e427-125">Most toohello virtuális gép példány tooinitialize hello adatlemezt csatlakoztathat</span><span class="sxs-lookup"><span data-stu-id="5e427-125">Now you can connect toohello virtual machine instance tooinitialize hello data disk</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-hello-data-disk"></a><span data-ttu-id="5e427-126">4. lépés: hello adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="5e427-126">Step 4 - Initialize hello data disk</span></span>

<span data-ttu-id="5e427-127">Csatlakoztatott toohello virtuális gépet, miközben hello lemez particionálása `fdisk`:</span><span class="sxs-lookup"><span data-stu-id="5e427-127">While connected toohello virtual machine, partition hello disk with `fdisk`:</span></span>

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="5e427-128">A fájl toohello rendszerpartíción hello írási `mkfs` parancs:</span><span class="sxs-lookup"><span data-stu-id="5e427-128">Write a file system toohello partition with hello `mkfs` command:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="5e427-129">Csatlakoztassa a hello új lemezt, így hello operációs rendszerben érhető el:</span><span class="sxs-lookup"><span data-stu-id="5e427-129">Mount hello new disk so that it is accessible in hello operating system:</span></span>

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="5e427-130">hello lemez mostantól lehet ellenőrizni a hello datadrive csatlakozásipont keresztül fér hozzá `ls /datadrive/`.</span><span class="sxs-lookup"><span data-stu-id="5e427-130">hello disk can now be accesses through hello datadrive mountpoint, which can be verified with `ls /datadrive/`.</span></span>

<span data-ttu-id="5e427-131">Záró hello SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="5e427-131">End hello SSH session.</span></span>


## <a name="step-5---configure-firewall"></a><span data-ttu-id="5e427-132">5. lépés - a tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e427-132">Step 5 - Configure firewall</span></span>

<span data-ttu-id="5e427-133">Lyukasztás lyuk keresztül hello tűzfal toohello webkiszolgálón üzemeltetett hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="5e427-133">Punch a hole through hello firewall toohello webserver hosted by hello scale set.</span></span> <span data-ttu-id="5e427-134">Hello méretezési csoportban hozták létre, egy terhelés-kiegyenlítő is hozták létre, és használta **SSH** toohello egyedi virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="5e427-134">When hello scale set was created, a load balancer was also created, and you used it **SSH** toohello individual virtual machines.</span></span> <span data-ttu-id="5e427-135">kétféle információt, amely akkor kell tooopen egy portot, az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="5e427-135">tooopen a port, you need two pieces of information, which you can get using Azure CLI.</span></span>

* <span data-ttu-id="5e427-136">**Előtérbeli IP-címkészlet**</span><span class="sxs-lookup"><span data-stu-id="5e427-136">**Frontend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* <span data-ttu-id="5e427-137">**Háttérbeli IP-címkészlet**</span><span class="sxs-lookup"><span data-stu-id="5e427-137">**Backend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

<span data-ttu-id="5e427-138">E két nevekkel, nyissa meg port **80**.</span><span class="sxs-lookup"><span data-stu-id="5e427-138">With those two names, you can open port **80**.</span></span>

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a><span data-ttu-id="5e427-139">6. lépés - automatizálásához</span><span class="sxs-lookup"><span data-stu-id="5e427-139">Step 6 - Automate configuration</span></span>

<span data-ttu-id="5e427-140">hello adatlemez toobe minden egyes virtuálisgép-példányt konfigurálni kell.</span><span class="sxs-lookup"><span data-stu-id="5e427-140">hello data disk needs toobe configured on each virtual machine instance.</span></span> <span data-ttu-id="5e427-141">A Microsoft automatizálhatja a virtuális gép hello hello hello konfigurálását **CustomScript** bővítmény.</span><span class="sxs-lookup"><span data-stu-id="5e427-141">We can automate hello configuration of hello virtual machine with hello **CustomScript** extension.</span></span>

<span data-ttu-id="5e427-142">Először hozzon létre egy *.sh* parancsfájlt, ami hello lemez formátum parancsokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5e427-142">First, create a *.sh* script that includes hello disk format commands.</span></span>

```sh
#!/bin/bash

# Setup hello data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

<span data-ttu-id="5e427-143">Ezt követően töltse fel a parancsfájl fájl toowhere hello **CustomScript** bővítmény-e hozzáférési engedélye.</span><span class="sxs-lookup"><span data-stu-id="5e427-143">Next, upload that script file toowhere hello **CustomScript** extension can access it.</span></span> <span data-ttu-id="5e427-144">Egy példány érhető [Itt](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span><span class="sxs-lookup"><span data-stu-id="5e427-144">A copy is available [here](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span></span>

<span data-ttu-id="5e427-145">Hozzon létre egy helyi fájlt **settings.json** és hello JSON blokk azt követően.</span><span class="sxs-lookup"><span data-stu-id="5e427-145">Create a local file named **settings.json** and put hello following JSON block in it.</span></span> <span data-ttu-id="5e427-146">Hello `flieUris` tulajdonságot kell beállítani, a parancsfájl feltöltöttnek toowhere.</span><span class="sxs-lookup"><span data-stu-id="5e427-146">hello `flieUris` property should be set toowhere your script file was uploaded to.</span></span>

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

<span data-ttu-id="5e427-147">A parancs tooyour skála hello beállított telepítése **CustomScript** bővítmény hello hivatkozó **settings.json** imént létrehozott fájl.</span><span class="sxs-lookup"><span data-stu-id="5e427-147">Deploy this command tooyour scale set with hello **CustomScript** extension, referencing hello **settings.json** file we just created.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

<span data-ttu-id="5e427-148">Aktuális példányainak, valamint a később létrehozott skálázással példányok automatikusan futtatja ezt a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="5e427-148">This extension automatically runs on all current instances, and any instances later created by scaling.</span></span>

## <a name="step-7---configure-autoscale-rules"></a><span data-ttu-id="5e427-149">7. lépés – az automatikus skálázási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e427-149">Step 7 - Configure autoscale rules</span></span>

<span data-ttu-id="5e427-150">Automatikus skálázási szabályok jelenleg az Azure parancssori felület nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="5e427-150">Currently, autoscale rules cannot be set in Azure CLI.</span></span> <span data-ttu-id="5e427-151">Használjon hello [Azure-portálon](https://portal.azure.com) tooconfigure automatikus skálázási.</span><span class="sxs-lookup"><span data-stu-id="5e427-151">Use hello [Azure portal](https://portal.azure.com) tooconfigure autoscale.</span></span>

## <a name="step-8---management-tasks"></a><span data-ttu-id="5e427-152">8 - felügyeleti feladatokat. lépés</span><span class="sxs-lookup"><span data-stu-id="5e427-152">Step 8 - Management tasks</span></span>

<span data-ttu-id="5e427-153">Hello méretezési hello életciklusa során szükség lehet a toorun egy vagy több felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="5e427-153">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="5e427-154">Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat toocreate, és hello Azure parancssori Felületet biztosít egy gyorsan toodo ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="5e427-154">Additionally, you may want toocreate scripts that automate various lifecycle-tasks, and hello Azure CLI provides a quick way toodo those tasks.</span></span> <span data-ttu-id="5e427-155">Az alábbiakban néhány gyakori feladatot.</span><span class="sxs-lookup"><span data-stu-id="5e427-155">Here are a few common tasks.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="5e427-156">Kapcsolat-adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="5e427-156">Get connection info</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a><span data-ttu-id="5e427-157">Állítsa be a példányszám (manuális méretezési)</span><span class="sxs-lookup"><span data-stu-id="5e427-157">Set instance count (manual scale)</span></span>

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a><span data-ttu-id="5e427-158">Erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="5e427-158">Delete resource group</span></span>

<span data-ttu-id="5e427-159">Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="5e427-159">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="5e427-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e427-160">Next steps</span></span>
<span data-ttu-id="5e427-161">További információ az egyes virtuálisgép-méretezési hello beállítása ebben az oktatóanyagban bevezetett szolgáltatások toolearn hello a következő információkat lásd:</span><span class="sxs-lookup"><span data-stu-id="5e427-161">toolearn more about some of hello virtual machine scale set features introduced in this tutorial, see hello following information:</span></span>

- [<span data-ttu-id="5e427-162">Az Azure virtuálisgép-méretezési csoportok áttekintése</span><span class="sxs-lookup"><span data-stu-id="5e427-162">Azure Virtual Machine Scale Sets overview</span></span>](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [<span data-ttu-id="5e427-163">Az Azure Load Balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="5e427-163">Azure Load Balancer overview</span></span>](../../load-balancer/load-balancer-overview.md)
- [<span data-ttu-id="5e427-164">A hálózati biztonsági csoportokkal hálózati adatforgalom szabályozásához</span><span class="sxs-lookup"><span data-stu-id="5e427-164">Control network traffic flow with network security groups</span></span>](../../virtual-network/virtual-networks-nsg.md)