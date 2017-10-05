---
title: "Egy Azure virtuális gép átalakítása egy méretezési |} Microsoft Docs"
description: "Létrehozhat és telepíthet a Linux Azure virtuálisgép-méretezési beállítása az Azure parancssori felület segítségével."
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
ms.openlocfilehash: 8d3376d2791b1349298db618d475ce5573083702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="convert-an-existing-azure-virtual-machine-to-a-scale-set"></a><span data-ttu-id="b540d-103">Meglévő Azure virtuális gép átalakítása egy méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="b540d-103">Convert an existing Azure virtual machine to a scale set</span></span>

<span data-ttu-id="b540d-104">Az oktatóanyag bemutatja, hogyan használható az Azure CLI 2.0 egy virtuálisgép-méretezési csoport egy virtuális gép átalakítása.</span><span class="sxs-lookup"><span data-stu-id="b540d-104">This tutorial shows you how to use Azure CLI 2.0 to convert a virtual machine to a virtual machine scale set.</span></span> <span data-ttu-id="b540d-105">Is megismerheti, hogyan automatizálható a méretezési csoportban lévő virtuális gépek konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="b540d-105">You also learn how to automate the configuration of the virtual machines in the scale set.</span></span> <span data-ttu-id="b540d-106">Azure CLI 2.0 telepítéséről további információkért lásd: [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b540d-106">For more information on how to install Azure CLI 2.0, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span> <span data-ttu-id="b540d-107">Méretezési csoportok kapcsolatos további információkért lásd: [virtuálisgép-skálázási készletekben](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b540d-107">For more information about scale sets, see [Virtual Machine Scale Sets](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span></span>

## <a name="step-1---deprovision-the-vm"></a><span data-ttu-id="b540d-108">1. lépés – a virtuális gép kiosztásának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="b540d-108">Step 1 - Deprovision the VM</span></span>

<span data-ttu-id="b540d-109">Az SSH használata a virtuális Géphez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="b540d-109">Use SSH to connect to the VM.</span></span>

<span data-ttu-id="b540d-110">Kiosztásának megszüntetése a virtuális Gépet az Azure Virtuálisgép-ügynök használatával törli a fájlokat és adatokat.</span><span class="sxs-lookup"><span data-stu-id="b540d-110">Deprovision the VM using the Azure VM agent to delete files and data.</span></span> <span data-ttu-id="b540d-111">A megszüntetés részletes áttekintése, lásd: [Linux virtuális gép rögzítése](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="b540d-111">For a detailed overview of deprovisioning, see [Capture a Linux virtual machine](capture-image.md).</span></span>

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-the-vm"></a><span data-ttu-id="b540d-112">2. lépés – a virtuális gép lemezképének rögzítése</span><span class="sxs-lookup"><span data-stu-id="b540d-112">Step 2 - Capture an image of the VM</span></span>

<span data-ttu-id="b540d-113">Részletes megtudhatja, hogy a, [Linux virtuális gép rögzítése](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="b540d-113">For a detailed overview of capturing, see [Capture a Linux virtual machine](capture-image.md).</span></span>

<span data-ttu-id="b540d-114">A virtuális Géphez a felszabadítani [az virtuális gép felszabadítása](/cli/azure/vm#deallocate):</span><span class="sxs-lookup"><span data-stu-id="b540d-114">Deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate):</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b540d-115">A virtuális Géphez a generalize [az vm generalize](/cli/azure/vm#generalize):</span><span class="sxs-lookup"><span data-stu-id="b540d-115">Generalize the VM with [az vm generalize](/cli/azure/vm#generalize):</span></span>

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b540d-116">Kép készítése a VM erőforrás [az lemezkép létrehozása](/cli/azure/image#create):</span><span class="sxs-lookup"><span data-stu-id="b540d-116">Create an image from the VM resource with [az image create](/cli/azure/image#create):</span></span>

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-the-scale-set"></a><span data-ttu-id="b540d-117">3. lépés - a méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="b540d-117">Step 3 - Create the scale set</span></span>

<span data-ttu-id="b540d-118">Beolvasása a **azonosító** kép.</span><span class="sxs-lookup"><span data-stu-id="b540d-118">Get the **id** of the image.</span></span>

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

<span data-ttu-id="b540d-119">A kép erőforrás a virtuális gép létrehozása [az vmss létrehozása](/cli/azure/vmss#create):</span><span class="sxs-lookup"><span data-stu-id="b540d-119">Create a VM from your image resource with [az vmss create](/cli/azure/vmss#create):</span></span>

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

<span data-ttu-id="b540d-120">Ez a parancs is 10 GB-os adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="b540d-120">This command also attached a 10gb data disk.</span></span> <span data-ttu-id="b540d-121">Ne feledje, hogy attól függően, hogy a virtuális Géphez kiválasztott méret (használtuk **Standard_DS1_v2**), az adatlemezek száma az engedélyezett különböző.</span><span class="sxs-lookup"><span data-stu-id="b540d-121">Keep in mind that depending on the VM size chosen (we used **Standard_DS1_v2**), the number of data disks allowed is different.</span></span> <span data-ttu-id="b540d-122">További információkért tekintse át a [virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="b540d-122">For more information, review the [virtual machine sizes](sizes.md).</span></span>

<span data-ttu-id="b540d-123">Ha a skála befejeződik, kapcsolódni hozzá.</span><span class="sxs-lookup"><span data-stu-id="b540d-123">Once the scale set finishes, connect to it.</span></span> <span data-ttu-id="b540d-124">A példányok IP-címek listájának lekérése az SSH [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info):</span><span class="sxs-lookup"><span data-stu-id="b540d-124">Get a list of IP addresses for the instances for SSH with [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info):</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

<span data-ttu-id="b540d-125">Most csatlakozhat a virtuálisgép-példányt a adatlemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="b540d-125">Now you can connect to the virtual machine instance to initialize the data disk</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-the-data-disk"></a><span data-ttu-id="b540d-126">4. lépés - az adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="b540d-126">Step 4 - Initialize the data disk</span></span>

<span data-ttu-id="b540d-127">Ha a virtuális gép csatlakozik, a lemez partícióazonosító `fdisk`:</span><span class="sxs-lookup"><span data-stu-id="b540d-127">While connected to the virtual machine, partition the disk with `fdisk`:</span></span>

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="b540d-128">A fájlrendszer írni a partíció a `mkfs` parancs:</span><span class="sxs-lookup"><span data-stu-id="b540d-128">Write a file system to the partition with the `mkfs` command:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="b540d-129">Csatlakoztassa az új lemezt, úgy, hogy az operációs rendszerben érhető el:</span><span class="sxs-lookup"><span data-stu-id="b540d-129">Mount the new disk so that it is accessible in the operating system:</span></span>

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="b540d-130">A lemez is lehet a datadrive csatlakozási pont, amely ellenőrizhető keresztül fér hozzá `ls /datadrive/`.</span><span class="sxs-lookup"><span data-stu-id="b540d-130">The disk can now be accesses through the datadrive mountpoint, which can be verified with `ls /datadrive/`.</span></span>

<span data-ttu-id="b540d-131">Az SSH-munkamenet befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b540d-131">End the SSH session.</span></span>


## <a name="step-5---configure-firewall"></a><span data-ttu-id="b540d-132">5. lépés - a tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b540d-132">Step 5 - Configure firewall</span></span>

<span data-ttu-id="b540d-133">Lyukasztás lyuk a webkiszolgálónak, a méretezési készlet által üzemeltetett a tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="b540d-133">Punch a hole through the firewall to the webserver hosted by the scale set.</span></span> <span data-ttu-id="b540d-134">A méretezési csoportban hozták létre, egy terhelés-kiegyenlítő is hozták létre, és használta **SSH** az egyes virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="b540d-134">When the scale set was created, a load balancer was also created, and you used it **SSH** to the individual virtual machines.</span></span> <span data-ttu-id="b540d-135">Nyisson meg egy portot, kétféle információt, amely akkor kell Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="b540d-135">To open a port, you need two pieces of information, which you can get using Azure CLI.</span></span>

* <span data-ttu-id="b540d-136">**Előtérbeli IP-címkészlet**</span><span class="sxs-lookup"><span data-stu-id="b540d-136">**Frontend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* <span data-ttu-id="b540d-137">**Háttérbeli IP-címkészlet**</span><span class="sxs-lookup"><span data-stu-id="b540d-137">**Backend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

<span data-ttu-id="b540d-138">E két nevekkel, nyissa meg port **80**.</span><span class="sxs-lookup"><span data-stu-id="b540d-138">With those two names, you can open port **80**.</span></span>

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a><span data-ttu-id="b540d-139">6. lépés - automatizálásához</span><span class="sxs-lookup"><span data-stu-id="b540d-139">Step 6 - Automate configuration</span></span>

<span data-ttu-id="b540d-140">Az adatok lemezre kell megadni minden egyes virtuálisgép-példányon.</span><span class="sxs-lookup"><span data-stu-id="b540d-140">The data disk needs to be configured on each virtual machine instance.</span></span> <span data-ttu-id="b540d-141">Azt automatizálhatja a virtuális gép konfigurációját a **CustomScript** bővítmény.</span><span class="sxs-lookup"><span data-stu-id="b540d-141">We can automate the configuration of the virtual machine with the **CustomScript** extension.</span></span>

<span data-ttu-id="b540d-142">Először hozzon létre egy *.sh* parancsfájlt, ami a lemez formátum parancsokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b540d-142">First, create a *.sh* script that includes the disk format commands.</span></span>

```sh
#!/bin/bash

# Setup the data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

<span data-ttu-id="b540d-143">Ezt követően töltse fel, hogy a parancsfájl arra, ahol a **CustomScript** bővítmény-e hozzáférési engedélye.</span><span class="sxs-lookup"><span data-stu-id="b540d-143">Next, upload that script file to where the **CustomScript** extension can access it.</span></span> <span data-ttu-id="b540d-144">Egy példány érhető [Itt](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span><span class="sxs-lookup"><span data-stu-id="b540d-144">A copy is available [here](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span></span>

<span data-ttu-id="b540d-145">Hozzon létre egy helyi fájlt **settings.json** és a következő JSON-blokk be.</span><span class="sxs-lookup"><span data-stu-id="b540d-145">Create a local file named **settings.json** and put the following JSON block in it.</span></span> <span data-ttu-id="b540d-146">A `flieUris` tulajdonságot kell beállítani, ahol a parancsfájl feltöltöttnek való.</span><span class="sxs-lookup"><span data-stu-id="b540d-146">The `flieUris` property should be set to where your script file was uploaded to.</span></span>

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

<span data-ttu-id="b540d-147">Telepítse ezt a parancsot a skála állítható be a **CustomScript** bővítmény hivatkozik a **settings.json** imént létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="b540d-147">Deploy this command to your scale set with the **CustomScript** extension, referencing the **settings.json** file we just created.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

<span data-ttu-id="b540d-148">Aktuális példányainak, valamint a később létrehozott skálázással példányok automatikusan futtatja ezt a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="b540d-148">This extension automatically runs on all current instances, and any instances later created by scaling.</span></span>

## <a name="step-7---configure-autoscale-rules"></a><span data-ttu-id="b540d-149">7. lépés – az automatikus skálázási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b540d-149">Step 7 - Configure autoscale rules</span></span>

<span data-ttu-id="b540d-150">Automatikus skálázási szabályok jelenleg az Azure parancssori felület nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="b540d-150">Currently, autoscale rules cannot be set in Azure CLI.</span></span> <span data-ttu-id="b540d-151">Használja a [Azure-portálon](https://portal.azure.com) automatikus skálázás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b540d-151">Use the [Azure portal](https://portal.azure.com) to configure autoscale.</span></span>

## <a name="step-8---management-tasks"></a><span data-ttu-id="b540d-152">8 - felügyeleti feladatokat. lépés</span><span class="sxs-lookup"><span data-stu-id="b540d-152">Step 8 - Management tasks</span></span>

<span data-ttu-id="b540d-153">A méretezési életciklusa során szükség lehet egy vagy több felügyeleti feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b540d-153">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="b540d-154">Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat hozhatnak létre, és az Azure parancssori felület e feladatok elvégzéséhez gyors lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="b540d-154">Additionally, you may want to create scripts that automate various lifecycle-tasks, and the Azure CLI provides a quick way to do those tasks.</span></span> <span data-ttu-id="b540d-155">Az alábbiakban néhány gyakori feladatot.</span><span class="sxs-lookup"><span data-stu-id="b540d-155">Here are a few common tasks.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="b540d-156">Kapcsolat-adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="b540d-156">Get connection info</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a><span data-ttu-id="b540d-157">Állítsa be a példányszám (manuális méretezési)</span><span class="sxs-lookup"><span data-stu-id="b540d-157">Set instance count (manual scale)</span></span>

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a><span data-ttu-id="b540d-158">Erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="b540d-158">Delete resource group</span></span>

<span data-ttu-id="b540d-159">Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="b540d-159">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="b540d-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b540d-160">Next steps</span></span>
<span data-ttu-id="b540d-161">Ebben az oktatóanyagban bevezetett virtuálisgép-méretezési készlet szolgáltatások némelyike kapcsolatos további tudnivalókért tekintse meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b540d-161">To learn more about some of the virtual machine scale set features introduced in this tutorial, see the following information:</span></span>

- [<span data-ttu-id="b540d-162">Az Azure virtuálisgép-méretezési csoportok áttekintése</span><span class="sxs-lookup"><span data-stu-id="b540d-162">Azure Virtual Machine Scale Sets overview</span></span>](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [<span data-ttu-id="b540d-163">Az Azure Load Balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="b540d-163">Azure Load Balancer overview</span></span>](../../load-balancer/load-balancer-overview.md)
- [<span data-ttu-id="b540d-164">A hálózati biztonsági csoportokkal hálózati adatforgalom szabályozásához</span><span class="sxs-lookup"><span data-stu-id="b540d-164">Control network traffic flow with network security groups</span></span>](../../virtual-network/virtual-networks-nsg.md)