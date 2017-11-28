---
title: "Létrehozása és kezelése a Linux virtuális gépek az Azure parancssori Felülettel |} Microsoft Docs"
description: "Az oktatóanyag - létrehozása és kezelése a Linux virtuális gépek az Azure parancssori felület"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c163c715eb1438a0d6b0ab53cbb43816ca8dbbb4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-manage-linux-vms-with-the-azure-cli"></a><span data-ttu-id="14616-103">Létrehozása és kezelése a Linux virtuális gépek az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="14616-103">Create and Manage Linux VMs with the Azure CLI</span></span>

<span data-ttu-id="14616-104">Az Azure virtuális gépek adjon meg egy teljes mértékben konfigurálhatók és rugalmas számítási környezet.</span><span class="sxs-lookup"><span data-stu-id="14616-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="14616-105">Ez az oktatóanyag ismerteti alapszintű Azure-beli virtuális gép telepítési elemek, például Virtuálisgép-méret kiválasztása, Virtuálisgép-lemezkép kiválasztása és a virtuális gépek telepítése.</span><span class="sxs-lookup"><span data-stu-id="14616-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="14616-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="14616-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="14616-107">Hozzon létre, és csatlakoztassa a virtuális Gépet</span><span class="sxs-lookup"><span data-stu-id="14616-107">Create and connect to a VM</span></span>
> * <span data-ttu-id="14616-108">Válassza ki és használja a Virtuálisgép-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="14616-108">Select and use VM images</span></span>
> * <span data-ttu-id="14616-109">Megtekintése és használata a megadott Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="14616-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="14616-110">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="14616-110">Resize a VM</span></span>
> * <span data-ttu-id="14616-111">Megtekinteni és megérteni a virtuális gép állapota</span><span class="sxs-lookup"><span data-stu-id="14616-111">View and understand VM state</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="14616-112">Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="14616-112">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="14616-113">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="14616-113">Run `az --version` to find the version.</span></span> <span data-ttu-id="14616-114">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="14616-114">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-resource-group"></a><span data-ttu-id="14616-115">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="14616-115">Create resource group</span></span>

<span data-ttu-id="14616-116">Hozzon létre egy erőforráscsoportot az [az group create](https://docs.microsoft.com/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="14616-116">Create a resource group with the [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="14616-117">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="14616-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="14616-118">Egy erőforráscsoportot a virtuális gépek előtt létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="14616-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="14616-119">Ebben a példában az erőforráscsoport neve *myResourceGroupVM* jön létre a *eastus* régióban.</span><span class="sxs-lookup"><span data-stu-id="14616-119">In this example, a resource group named *myResourceGroupVM* is created in the *eastus* region.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

<span data-ttu-id="14616-120">Az erőforráscsoport létrehozása vagy módosítása egy virtuális Gépet, amelynek ez az oktatóanyag teljes látható során van megadva.</span><span class="sxs-lookup"><span data-stu-id="14616-120">The resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="14616-121">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="14616-121">Create virtual machine</span></span>

<span data-ttu-id="14616-122">A virtuális gép létrehozása a [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="14616-122">Create a virtual machine with the [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="14616-123">A virtuális gép létrehozásakor több lehetőség is elérhető, például az operációs rendszer lemezképét, a lemez méretezés és a felügyeleti hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="14616-123">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="14616-124">Ebben a példában egy virtuális gép létrehozása nevű *myVM* Ubuntu Server operációs rendszert futtató.</span><span class="sxs-lookup"><span data-stu-id="14616-124">In this example, a virtual machine is created with a name of *myVM* running Ubuntu Server.</span></span> 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="14616-125">A virtuális gép létrehozása után az Azure parancssori felület exportálja a virtuális gép kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="14616-125">Once the VM has been created, the Azure CLI outputs information about the VM.</span></span> <span data-ttu-id="14616-126">Vegye figyelembe a `publicIpAddress`, ez a cím segítségével fér hozzá a virtuális géphez...</span><span class="sxs-lookup"><span data-stu-id="14616-126">Take note of the `publicIpAddress`, this address can be used to access the virtual machine..</span></span> 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-to-vm"></a><span data-ttu-id="14616-127">Csatlakozás virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="14616-127">Connect to VM</span></span>

<span data-ttu-id="14616-128">A virtuális gép SSH használatával is csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="14616-128">You can now connect to the VM using SSH.</span></span> <span data-ttu-id="14616-129">Cserélje le a példa IP-cím a `publicIpAddress` az előző lépésben feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="14616-129">Replace the example IP address with the `publicIpAddress` noted in the previous step.</span></span>

```bash
ssh 52.174.34.95
```

<span data-ttu-id="14616-130">Miután befejezte a virtuális gép, zárja be az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="14616-130">Once finished with the VM, close the SSH session.</span></span> 

```bash
exit
```

## <a name="understand-vm-images"></a><span data-ttu-id="14616-131">Virtuálisgép-rendszerképek ismertetése</span><span class="sxs-lookup"><span data-stu-id="14616-131">Understand VM images</span></span>

<span data-ttu-id="14616-132">Az Azure piactéren tartalmaz számos rendszerképet virtuális gépek létrehozásához használható.</span><span class="sxs-lookup"><span data-stu-id="14616-132">The Azure marketplace includes many images that can be used to create VMs.</span></span> <span data-ttu-id="14616-133">Az előző lépésben a virtuális gép létrehozása egy Ubuntu lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="14616-133">In the previous steps, a virtual machine was created using an Ubuntu image.</span></span> <span data-ttu-id="14616-134">Ebben a lépésben az Azure CLI-vel keresése a piactéren CentOS kép, amit a második virtuális gép telepítése.</span><span class="sxs-lookup"><span data-stu-id="14616-134">In this step, the Azure CLI is used to search the marketplace for a CentOS image, which is then used to deploy a second virtual machine.</span></span>  

<span data-ttu-id="14616-135">A legtöbb listája általánosan használt képek megjelenítéséhez használja a [az vm képlistában](/cli/azure/vm/image#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="14616-135">To see a list of the most commonly used images, use the [az vm image list](/cli/azure/vm/image#list) command.</span></span>

```azurecli-interactive 
az vm image list --output table
```

<span data-ttu-id="14616-136">A parancs kimenetét a legnépszerűbb VM-lemezképekkel az Azure ad vissza.</span><span class="sxs-lookup"><span data-stu-id="14616-136">The command output returns the most popular VM images on Azure.</span></span>

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

<span data-ttu-id="14616-137">Teljes listája látható hozzáadásával a `--all` argumentum.</span><span class="sxs-lookup"><span data-stu-id="14616-137">A full list can be seen by adding the `--all` argument.</span></span> <span data-ttu-id="14616-138">A képlistában is alapján szűrhetők `--publisher` vagy `–-offer`.</span><span class="sxs-lookup"><span data-stu-id="14616-138">The image list can also be filtered by `--publisher` or `–-offer`.</span></span> <span data-ttu-id="14616-139">Ebben a példában a listában az ajánlat, amely megfelel az összes lemezkép vannak *CentOS*.</span><span class="sxs-lookup"><span data-stu-id="14616-139">In this example, the list is filtered for all images with an offer that matches *CentOS*.</span></span> 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

<span data-ttu-id="14616-140">Részleges kimenete:</span><span class="sxs-lookup"><span data-stu-id="14616-140">Partial output:</span></span>

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

<span data-ttu-id="14616-141">A virtuális gépek azokba telepítéséhez jegyezze fel az értékét a *Urn* oszlop.</span><span class="sxs-lookup"><span data-stu-id="14616-141">To deploy a VM using a specific image, take note of the value in the *Urn* column.</span></span> <span data-ttu-id="14616-142">A kép megadásakor a lemezkép verziószámát lehet cserélni "legújabb", amely kiválasztja a terjesztési legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="14616-142">When specifying the image, the image version number can be replaced with “latest”, which selects the latest version of the distribution.</span></span> <span data-ttu-id="14616-143">Ebben a példában a `--image` argumentum használatával adja meg a CentOS 6.5-lemezkép legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="14616-143">In this example, the `--image` argument is used to specify the latest version of a CentOS 6.5 image.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="14616-144">Virtuálisgép-méretek ismertetése</span><span class="sxs-lookup"><span data-stu-id="14616-144">Understand VM sizes</span></span>

<span data-ttu-id="14616-145">A virtuális gép méretét határozza meg a számítási erőforrásokat, a virtuális gép számára elérhető például a CPU, grafikus Processzor és memória mennyisége.</span><span class="sxs-lookup"><span data-stu-id="14616-145">A virtual machine size determines the amount of compute resources such as CPU, GPU, and memory that are made available to the virtual machine.</span></span> <span data-ttu-id="14616-146">Virtuális gépek a várt munkahelyi terhelés méretének kell.</span><span class="sxs-lookup"><span data-stu-id="14616-146">Virtual machines need to be sized appropriately for the expected work load.</span></span> <span data-ttu-id="14616-147">Ha növeli a munkaterhelés, egy meglévő virtuális gépet is átméretezhetők.</span><span class="sxs-lookup"><span data-stu-id="14616-147">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="14616-148">Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="14616-148">VM Sizes</span></span>

<span data-ttu-id="14616-149">A következő táblázat azokat a használati esetek méretek kategorizálja.</span><span class="sxs-lookup"><span data-stu-id="14616-149">The following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="14616-150">Típus</span><span class="sxs-lookup"><span data-stu-id="14616-150">Type</span></span>                     | <span data-ttu-id="14616-151">Méretek</span><span class="sxs-lookup"><span data-stu-id="14616-151">Sizes</span></span>           |    <span data-ttu-id="14616-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="14616-152">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="14616-153">Általános célú</span><span class="sxs-lookup"><span data-stu-id="14616-153">General purpose</span></span>](sizes-general.md)         |<span data-ttu-id="14616-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span><span class="sxs-lookup"><span data-stu-id="14616-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="14616-155">Elosztott terhelésű CPU-memória.</span><span class="sxs-lookup"><span data-stu-id="14616-155">Balanced CPU-to-memory.</span></span> <span data-ttu-id="14616-156">Épp ezért tökéletes választás a fejlesztői / tesztelői és kis, közepes méretű alkalmazásokhoz és adatokhoz megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="14616-156">Ideal for dev / test and small to medium applications and data solutions.</span></span>  |
| [<span data-ttu-id="14616-157">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="14616-157">Compute optimized</span></span>](sizes-compute.md)   | <span data-ttu-id="14616-158">FS, F</span><span class="sxs-lookup"><span data-stu-id="14616-158">Fs, F</span></span>             | <span data-ttu-id="14616-159">Magas CPU-memória.</span><span class="sxs-lookup"><span data-stu-id="14616-159">High CPU-to-memory.</span></span> <span data-ttu-id="14616-160">Jó közepes forgalom alkalmazásokat, hálózati berendezéseket és kötegelt folyamatok.</span><span class="sxs-lookup"><span data-stu-id="14616-160">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| [<span data-ttu-id="14616-161">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="14616-161">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)    | <span data-ttu-id="14616-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="14616-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="14616-163">Magas memória-core.</span><span class="sxs-lookup"><span data-stu-id="14616-163">High memory-to-core.</span></span> <span data-ttu-id="14616-164">A relációs adatbázisok, közepes vagy nagyméretű gyorsítótárak és memórián belüli analytics nagy.</span><span class="sxs-lookup"><span data-stu-id="14616-164">Great for relational databases, medium to large caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="14616-165">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="14616-165">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)      | <span data-ttu-id="14616-166">Ls</span><span class="sxs-lookup"><span data-stu-id="14616-166">Ls</span></span>                | <span data-ttu-id="14616-167">Magas lemez-adatátviteli és I/O-műveleti jellemzők.</span><span class="sxs-lookup"><span data-stu-id="14616-167">High disk throughput and IO.</span></span> <span data-ttu-id="14616-168">Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.</span><span class="sxs-lookup"><span data-stu-id="14616-168">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="14616-169">GPU</span><span class="sxs-lookup"><span data-stu-id="14616-169">GPU</span></span>](sizes-gpu.md)          | <span data-ttu-id="14616-170">PORTOK HV, NC</span><span class="sxs-lookup"><span data-stu-id="14616-170">NV, NC</span></span>            | <span data-ttu-id="14616-171">Nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt speciális virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="14616-171">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| [<span data-ttu-id="14616-172">Magas teljesítmény</span><span class="sxs-lookup"><span data-stu-id="14616-172">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="14616-173">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="14616-173">H, A8-11</span></span>          | <span data-ttu-id="14616-174">A leghatékonyabb CPU rendelkező virtuális gépek nagy átviteli további hálózati adapterek (RDMA).</span><span class="sxs-lookup"><span data-stu-id="14616-174">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="14616-175">Elérhető Virtuálisgép-méretek keresése</span><span class="sxs-lookup"><span data-stu-id="14616-175">Find available VM sizes</span></span>

<span data-ttu-id="14616-176">Virtuálisgép-méretek érhető el az adott listájának megtekintéséhez használja a [az vm-méretek listázása](/cli/azure/vm#list-sizes) parancsot.</span><span class="sxs-lookup"><span data-stu-id="14616-176">To see a list of VM sizes available in a particular region, use the [az vm list-sizes](/cli/azure/vm#list-sizes) command.</span></span> 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

<span data-ttu-id="14616-177">Részleges kimenete:</span><span class="sxs-lookup"><span data-stu-id="14616-177">Partial output:</span></span>

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a><span data-ttu-id="14616-178">Meghatározott méretű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="14616-178">Create VM with specific size</span></span>

<span data-ttu-id="14616-179">Az előző virtuális gép létrehozása a példában a mérete nem adta meg, melyik eredmények alapértelmezett mérete.</span><span class="sxs-lookup"><span data-stu-id="14616-179">In the previous VM creation example, a size was not provided, which results in a default size.</span></span> <span data-ttu-id="14616-180">A Virtuálisgép-méret választható létrehozási ideje az [az virtuális gép létrehozása](/cli/azure/vm#create) és a `--size` argumentum.</span><span class="sxs-lookup"><span data-stu-id="14616-180">A VM size can be selected at creation time using [az vm create](/cli/azure/vm#create) and the `--size` argument.</span></span> 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a><span data-ttu-id="14616-181">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="14616-181">Resize a VM</span></span>

<span data-ttu-id="14616-182">A virtuális gépek telepítése után azt növeléséhez vagy csökkentéséhez az erőforrás-elosztás is átméretezhetők.</span><span class="sxs-lookup"><span data-stu-id="14616-182">After a VM has been deployed, it can be resized to increase or decrease resource allocation.</span></span>

<span data-ttu-id="14616-183">A virtuális gépek átméretezésével előtt ellenőrizze, hogy a kívánt méretet cím az aktuális Azure fürtön.</span><span class="sxs-lookup"><span data-stu-id="14616-183">Before resizing a VM, check if the desired size is available on the current Azure cluster.</span></span> <span data-ttu-id="14616-184">A [az vm-vm-átméretezési-beállításai](/cli/azure/vm#list-vm-resize-options) parancs méretek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="14616-184">The [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options) command returns the list of sizes.</span></span> 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
<span data-ttu-id="14616-185">Ha érhető el a kívánt méretet, a virtuális gép átméretezhető az alkalmazás bekapcsolja a állapotból, azonban a művelet során újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="14616-185">If the desired size is available, the VM can be resized from a powered-on state, however it is rebooted during the operation.</span></span> <span data-ttu-id="14616-186">Használja a [az vm átméretezése]( /cli/azure/vm#resize) az átméretezés végrehajtandó parancs.</span><span class="sxs-lookup"><span data-stu-id="14616-186">Use the [az vm resize]( /cli/azure/vm#resize) command to perform the resize.</span></span>

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

<span data-ttu-id="14616-187">Ha a kívánt méretet nincs a jelenlegi fürthöz, a virtuális gép kell felszabadítása előtt az átméretezés akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="14616-187">If the desired size is not on the current cluster, the VM needs to be deallocated before the resize operation can occur.</span></span> <span data-ttu-id="14616-188">Használja a [az virtuális gép felszabadítása]( /cli/azure/vm#deallocate) parancs használatával állítsa le, és a virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="14616-188">Use the [az vm deallocate]( /cli/azure/vm#deallocate) command to stop and deallocate the VM.</span></span> <span data-ttu-id="14616-189">Vegye figyelembe, ha a virtuális gép be van kapcsolva vissza, az ideiglenes lemezen lévő adatokhoz eltávolíthat.</span><span class="sxs-lookup"><span data-stu-id="14616-189">Note, when the VM is powered back on, any data on the temp disk may be removed.</span></span> <span data-ttu-id="14616-190">A nyilvános IP-cím is megváltozik, kivéve, ha a statikus IP-cím használatban van.</span><span class="sxs-lookup"><span data-stu-id="14616-190">The public IP address also changes unless a static IP address is being used.</span></span> 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

<span data-ttu-id="14616-191">Az átméretezés akkor fordulhat elő, ha felszabadítása. lehetséges.</span><span class="sxs-lookup"><span data-stu-id="14616-191">Once deallocated, the resize can occur.</span></span> 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

<span data-ttu-id="14616-192">Az átméretezés után a virtuális gép elindítható.</span><span class="sxs-lookup"><span data-stu-id="14616-192">After the resize, the VM can be started.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a><span data-ttu-id="14616-193">Virtuális gép energiaállapotokat</span><span class="sxs-lookup"><span data-stu-id="14616-193">VM power states</span></span>

<span data-ttu-id="14616-194">Egy Azure virtuális gép sok energiaállapotát egyike lehet.</span><span class="sxs-lookup"><span data-stu-id="14616-194">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="14616-195">Ebben az állapotban lévő virtuális gép aktuális állapotát jeleníti meg a hipervizor szempontjából.</span><span class="sxs-lookup"><span data-stu-id="14616-195">This state represents the current state of the VM from the standpoint of the hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="14616-196">Energiaállapotokat</span><span class="sxs-lookup"><span data-stu-id="14616-196">Power states</span></span>

| <span data-ttu-id="14616-197">Energiaállapot</span><span class="sxs-lookup"><span data-stu-id="14616-197">Power State</span></span> | <span data-ttu-id="14616-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="14616-198">Description</span></span>
|----|----|
| <span data-ttu-id="14616-199">Indulás alatt</span><span class="sxs-lookup"><span data-stu-id="14616-199">Starting</span></span> | <span data-ttu-id="14616-200">Azt jelzi, hogy a virtuális gép elindul.</span><span class="sxs-lookup"><span data-stu-id="14616-200">Indicates the virtual machine is being started.</span></span> |
| <span data-ttu-id="14616-201">Fut</span><span class="sxs-lookup"><span data-stu-id="14616-201">Running</span></span> | <span data-ttu-id="14616-202">Azt jelzi, hogy fut-e a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="14616-202">Indicates that the virtual machine is running.</span></span> |
| <span data-ttu-id="14616-203">Leállítás</span><span class="sxs-lookup"><span data-stu-id="14616-203">Stopping</span></span> | <span data-ttu-id="14616-204">Azt jelzi, hogy a virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="14616-204">Indicates that the virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="14616-205">Leállítva</span><span class="sxs-lookup"><span data-stu-id="14616-205">Stopped</span></span> | <span data-ttu-id="14616-206">Azt jelzi, hogy a virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="14616-206">Indicates that the virtual machine is stopped.</span></span> <span data-ttu-id="14616-207">A leállított állapotban lévő virtuális gépek továbbra is számítási díjakat fel Önnek.</span><span class="sxs-lookup"><span data-stu-id="14616-207">Virtual machines in the stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="14616-208">Felszabadítása</span><span class="sxs-lookup"><span data-stu-id="14616-208">Deallocating</span></span> | <span data-ttu-id="14616-209">Azt jelzi, hogy a virtuális gép felszabadítása alatt.</span><span class="sxs-lookup"><span data-stu-id="14616-209">Indicates that the virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="14616-210">Felszabadítása</span><span class="sxs-lookup"><span data-stu-id="14616-210">Deallocated</span></span> | <span data-ttu-id="14616-211">Azt jelzi, hogy a virtuális gép-e a hipervizor eltávolították, de a vezérlő vezérlősík továbbra is elérhető.</span><span class="sxs-lookup"><span data-stu-id="14616-211">Indicates that the virtual machine is removed from the hypervisor but still available in the control plane.</span></span> <span data-ttu-id="14616-212">Virtuális gépek Deallocated állapotban nem számítunk költségeket.</span><span class="sxs-lookup"><span data-stu-id="14616-212">Virtual machines in the Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="14616-213">Azt jelzi, hogy a teljesítmény a virtuális gép állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="14616-213">Indicates that the power state of the virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="14616-214">Energiaállapot keresése</span><span class="sxs-lookup"><span data-stu-id="14616-214">Find power state</span></span>

<span data-ttu-id="14616-215">Egy adott virtuális gép állapotának lekéréséhez használja a [az virtuálisgép-példányokat tartalmazó nézet beolvasása](/cli/azure/vm#get-instance-view) parancsot.</span><span class="sxs-lookup"><span data-stu-id="14616-215">To retrieve the state of a particular VM, use the [az vm get instance-view](/cli/azure/vm#get-instance-view) command.</span></span> <span data-ttu-id="14616-216">Ne adjon meg egy érvényes nevet a virtuális gép és erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="14616-216">Be sure to specify a valid name for a virtual machine and resource group.</span></span> 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

<span data-ttu-id="14616-217">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="14616-217">Output:</span></span>

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a><span data-ttu-id="14616-218">Felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="14616-218">Management tasks</span></span>

<span data-ttu-id="14616-219">Életciklusa során a virtuális gépek érdemes lehet indítása, leállítása vagy törlése a virtuális gépek például a felügyeleti feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="14616-219">During the life-cycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="14616-220">Emellett érdemes lehet ismétlődő vagy összetett feladatok automatizálása parancsfájlok létrehozására.</span><span class="sxs-lookup"><span data-stu-id="14616-220">Additionally, you may want to create scripts to automate repetitive or complex tasks.</span></span> <span data-ttu-id="14616-221">Az Azure parancssori felület használatával, számos gyakori felügyeleti feladatokat is futtatható a parancssorból vagy parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="14616-221">Using the Azure CLI, many common management tasks can be run from the command line or in scripts.</span></span> 

### <a name="get-ip-address"></a><span data-ttu-id="14616-222">IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="14616-222">Get IP address</span></span>

<span data-ttu-id="14616-223">Ez a parancs visszaadja a privát és nyilvános IP-címek egy virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="14616-223">This command returns the private and public IP addresses of a virtual machine.</span></span>  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a><span data-ttu-id="14616-224">Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="14616-224">Stop virtual machine</span></span>

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a><span data-ttu-id="14616-225">Virtuális gép elindítása</span><span class="sxs-lookup"><span data-stu-id="14616-225">Start virtual machine</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="14616-226">Erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="14616-226">Delete resource group</span></span>

<span data-ttu-id="14616-227">Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="14616-227">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a><span data-ttu-id="14616-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14616-228">Next steps</span></span>

<span data-ttu-id="14616-229">Ebben az oktatóprogramban megismerte alapvető virtuális gép létrehozása és kezelése például hogyan:</span><span class="sxs-lookup"><span data-stu-id="14616-229">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="14616-230">Hozzon létre, és csatlakoztassa a virtuális Gépet</span><span class="sxs-lookup"><span data-stu-id="14616-230">Create and connect to a VM</span></span>
> * <span data-ttu-id="14616-231">Válassza ki és használja a Virtuálisgép-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="14616-231">Select and use VM images</span></span>
> * <span data-ttu-id="14616-232">Megtekintése és használata a megadott Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="14616-232">View and use specific VM sizes</span></span>
> * <span data-ttu-id="14616-233">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="14616-233">Resize a VM</span></span>
> * <span data-ttu-id="14616-234">Megtekinteni és megérteni a virtuális gép állapota</span><span class="sxs-lookup"><span data-stu-id="14616-234">View and understand VM state</span></span>

<span data-ttu-id="14616-235">A következő oktatóanyag további információt a virtuális gépek lemezei továbblépés.</span><span class="sxs-lookup"><span data-stu-id="14616-235">Advance to the next tutorial to learn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="14616-236">Hozzon létre és kezelheti a virtuális gépek lemezei</span><span class="sxs-lookup"><span data-stu-id="14616-236">Create and Manage VM disks</span></span>](./tutorial-manage-disks.md)
