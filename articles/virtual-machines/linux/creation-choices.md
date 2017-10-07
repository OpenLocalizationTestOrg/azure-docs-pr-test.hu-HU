---
title: "aaaDifferent módon toocreate egy Linux virtuális Gépet az Azure-ban |} A Microsoft Azure"
description: "Ismerje meg, hogy hello különböző módokon toocreate egy Linux virtuális gépet az Azure, beleértve a hivatkozások tootools és az egyes módszerek oktatóanyagok."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 250e92c063c87a8c1279097dc2264777d95478d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-linux-vm"></a><span data-ttu-id="b4ea5-103">Különböző módokon toocreate Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="b4ea5-103">Different ways toocreate a Linux VM</span></span>
<span data-ttu-id="b4ea5-104">Hello beleszólása van az Azure toocreate Linux virtuális gép (VM) eszközök és a munkafolyamatok kényelmes tooyou.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-104">You have hello flexibility in Azure toocreate a Linux virtual machine (VM) using tools and workflows comfortable tooyou.</span></span> <span data-ttu-id="b4ea5-105">Ez a cikk ezeket különbségek és a Linux virtuális gépeken, beleértve az Azure CLI 2.0 hello létrehozására vonatkozó példákat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-105">This article summarizes these differences and examples for creating your Linux VMs, including hello Azure CLI 2.0.</span></span> <span data-ttu-id="b4ea5-106">Megtekintheti többek között a hello létrehozási lehetőségek [Azure CLI 1.0](creation-choices-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b4ea5-106">You can also view creation choices including hello [Azure CLI 1.0](creation-choices-nodejs.md).</span></span>

<span data-ttu-id="b4ea5-107">Hello [Azure CLI 2.0](/cli/azure/install-az-cli2) érhető el több platformon keresztül az npm-csomagot, a megadott distro csomagok és a Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-107">hello [Azure CLI 2.0](/cli/azure/install-az-cli2) is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="b4ea5-108">Hello a környezetnek leginkább megfelelő build telepítése, és jelentkezzen be tooan Azure-fiók használatával [az bejelentkezés](/cli/azure/#login)</span><span class="sxs-lookup"><span data-stu-id="b4ea5-108">Install hello most appropriate build for your environment and log in tooan Azure account using [az login](/cli/azure/#login)</span></span>

* [<span data-ttu-id="b4ea5-109">Hozzon létre egy Linux virtuális gép hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b4ea5-109">Create a Linux VM with hello Azure CLI 2.0</span></span>](quick-create-cli.md)
  
  * <span data-ttu-id="b4ea5-110">Hozzon létre egy *myResourceGroup* nevű erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-110">Create a resource group with [az group create](/cli/azure/group#create) named *myResourceGroup*:</span></span> 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * <span data-ttu-id="b4ea5-111">A virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) nevű *myVM* legújabb hello segítségével *UbuntuLTS* rendszerképet, az SSH-kulcsok létrehozása, ha még nem léteznek a *~/.ssh*:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-111">Create a VM with [az vm create](/cli/azure/vm#create) named *myVM* using hello latest *UbuntuLTS* image and generate SSH keys if they do not already exist in *~/.ssh*:</span></span>

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [<span data-ttu-id="b4ea5-112">Linux virtuális gép létrehozása Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="b4ea5-112">Create a Linux VM with an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="b4ea5-113">hello alábbi példában [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) toocreate egy virtuális Gépet a Githubon tárolt sablonból:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-113">hello following example uses [az group deployment create](/cli/azure/group/deployment#create) toocreate a VM from a template stored on GitHub:</span></span>
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [<span data-ttu-id="b4ea5-114">Linux virtuális gép létrehozása és testreszabása a cloud-init paranccsal</span><span class="sxs-lookup"><span data-stu-id="b4ea5-114">Create a Linux VM and customize with cloud-init</span></span>](tutorial-automate-vm-deployment.md)

* [<span data-ttu-id="b4ea5-115">Elosztott terhelésű és magas rendelkezésre állású alkalmazás létrehozása több Linux virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="b4ea5-115">Create a load balanced and highly available application on multiple Linux VMs</span></span>](tutorial-load-balancer.md)


## <a name="azure-portal"></a><span data-ttu-id="b4ea5-116">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b4ea5-116">Azure portal</span></span>
<span data-ttu-id="b4ea5-117">Hello [Azure-portálon](https://portal.azure.com) lehetővé teszi a tooquickly virtuális gép létrehozása, mivel nincs szükség a rendszeren tooinstall.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-117">hello [Azure portal](https://portal.azure.com) allows you tooquickly create a VM since there is nothing tooinstall on your system.</span></span> <span data-ttu-id="b4ea5-118">Hello Azure portál toocreate hello VM használja:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-118">Use hello Azure portal toocreate hello VM:</span></span>

* [<span data-ttu-id="b4ea5-119">Linux virtuális gép létrehozása Azure-portálon hello</span><span class="sxs-lookup"><span data-stu-id="b4ea5-119">Create a Linux VM using hello Azure portal</span></span>](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a><span data-ttu-id="b4ea5-120">Választható operációs rendszerek és rendszerképek</span><span class="sxs-lookup"><span data-stu-id="b4ea5-120">Operating system and image choices</span></span>
<span data-ttu-id="b4ea5-121">Virtuális gép létrehozásakor ki kell választania egy operációs rendszer kívánt toorun hello alapuló rendszerképet.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-121">When creating a VM, you choose an image based on hello operating system you want toorun.</span></span> <span data-ttu-id="b4ea5-122">Az Azure és a partnerei számos rendszerképet kínálnak, amelyek némelyike előre telepített alkalmazásokat és eszközöket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-122">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="b4ea5-123">Vagy a feltöltött egyik saját rendszerképét (lásd: [szakasz következő hello](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="b4ea5-123">Or, upload one of your own images (see [hello following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="b4ea5-124">Azure-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="b4ea5-124">Azure images</span></span>
<span data-ttu-id="b4ea5-125">Használjon hello [az virtuálisgép-lemezkép](/cli/azure/vm/image) elérhető közzétevő, distro kiadás és buildek toosee parancsokat.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-125">Use hello [az vm image](/cli/azure/vm/image) commands toosee what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="b4ea5-126">Az elérhető közzétevők listázása:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-126">List available publishers:</span></span>

```azurecli
az vm image list-publishers --location eastus
```

<span data-ttu-id="b4ea5-127">Egy adott közzétevő elérhető termékeinek (ajánlatainak) listázása:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-127">List available products (offers) for a given publisher:</span></span>

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

<span data-ttu-id="b4ea5-128">Egy adott ajánlat elérhető termékváltozatainak (disztribúciós kiadásainak) listázása:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-128">List available SKUs (distro releases) of a given offer:</span></span>

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

<span data-ttu-id="b4ea5-129">Egy adott kiadás összes elérhető rendszerképének listázása:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-129">List all available images for a given release:</span></span>

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

<span data-ttu-id="b4ea5-130">Keresse meg, és elérhető rendszerkép használatával további példákért lásd [keresse meg és válassza ki azokat az Azure CLI hello Azure virtuális gép lemezképeket](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="b4ea5-130">For more examples on browsing and using available images, see [Navigate and select Azure virtual machine images with hello Azure CLI](cli-ps-findimage.md).</span></span>

<span data-ttu-id="b4ea5-131">Hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsában aliasok vannak használhatja tooquickly hozzáférés hello gyakori disztribúciókkal és a legújabb kiadást.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-131">hello [az vm create](/cli/azure/vm#create) command has aliases you can use tooquickly access hello more common distros and their latest releases.</span></span> <span data-ttu-id="b4ea5-132">Használjon olyan aliasneveket gyakran gyorsabb, mint hello közzétevő, az ajánlat, SKU és verzió megadása minden alkalommal, amikor a virtuális gép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-132">Using aliases is often quicker than specifying hello publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="b4ea5-133">Alias</span><span class="sxs-lookup"><span data-stu-id="b4ea5-133">Alias</span></span> | <span data-ttu-id="b4ea5-134">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="b4ea5-134">Publisher</span></span> | <span data-ttu-id="b4ea5-135">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="b4ea5-135">Offer</span></span> | <span data-ttu-id="b4ea5-136">SKU</span><span class="sxs-lookup"><span data-stu-id="b4ea5-136">SKU</span></span> | <span data-ttu-id="b4ea5-137">Verzió</span><span class="sxs-lookup"><span data-stu-id="b4ea5-137">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="b4ea5-138">CentOS</span><span class="sxs-lookup"><span data-stu-id="b4ea5-138">CentOS</span></span> |<span data-ttu-id="b4ea5-139">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="b4ea5-139">OpenLogic</span></span> |<span data-ttu-id="b4ea5-140">Centos</span><span class="sxs-lookup"><span data-stu-id="b4ea5-140">Centos</span></span> |<span data-ttu-id="b4ea5-141">7.2</span><span class="sxs-lookup"><span data-stu-id="b4ea5-141">7.2</span></span> |<span data-ttu-id="b4ea5-142">legújabb</span><span class="sxs-lookup"><span data-stu-id="b4ea5-142">latest</span></span> |
| <span data-ttu-id="b4ea5-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="b4ea5-143">CoreOS</span></span> |<span data-ttu-id="b4ea5-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="b4ea5-144">CoreOS</span></span> |<span data-ttu-id="b4ea5-145">CoreOS</span><span class="sxs-lookup"><span data-stu-id="b4ea5-145">CoreOS</span></span> |<span data-ttu-id="b4ea5-146">Stable</span><span class="sxs-lookup"><span data-stu-id="b4ea5-146">Stable</span></span> |<span data-ttu-id="b4ea5-147">legújabb</span><span class="sxs-lookup"><span data-stu-id="b4ea5-147">latest</span></span> |
| <span data-ttu-id="b4ea5-148">Debian</span><span class="sxs-lookup"><span data-stu-id="b4ea5-148">Debian</span></span> |<span data-ttu-id="b4ea5-149">credativ</span><span class="sxs-lookup"><span data-stu-id="b4ea5-149">credativ</span></span> |<span data-ttu-id="b4ea5-150">Debian</span><span class="sxs-lookup"><span data-stu-id="b4ea5-150">Debian</span></span> |<span data-ttu-id="b4ea5-151">8</span><span class="sxs-lookup"><span data-stu-id="b4ea5-151">8</span></span> |<span data-ttu-id="b4ea5-152">legújabb</span><span class="sxs-lookup"><span data-stu-id="b4ea5-152">latest</span></span> |
| <span data-ttu-id="b4ea5-153">openSUSE</span><span class="sxs-lookup"><span data-stu-id="b4ea5-153">openSUSE</span></span> |<span data-ttu-id="b4ea5-154">SUSE</span><span class="sxs-lookup"><span data-stu-id="b4ea5-154">SUSE</span></span> |<span data-ttu-id="b4ea5-155">openSUSE</span><span class="sxs-lookup"><span data-stu-id="b4ea5-155">openSUSE</span></span> |<span data-ttu-id="b4ea5-156">13.2</span><span class="sxs-lookup"><span data-stu-id="b4ea5-156">13.2</span></span> |<span data-ttu-id="b4ea5-157">legújabb</span><span class="sxs-lookup"><span data-stu-id="b4ea5-157">latest</span></span> |
| <span data-ttu-id="b4ea5-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="b4ea5-158">RHEL</span></span> |<span data-ttu-id="b4ea5-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="b4ea5-159">Redhat</span></span> |<span data-ttu-id="b4ea5-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="b4ea5-160">RHEL</span></span> |<span data-ttu-id="b4ea5-161">7.2</span><span class="sxs-lookup"><span data-stu-id="b4ea5-161">7.2</span></span> |<span data-ttu-id="b4ea5-162">legújabb</span><span class="sxs-lookup"><span data-stu-id="b4ea5-162">latest</span></span> |
| <span data-ttu-id="b4ea5-163">SLES</span><span class="sxs-lookup"><span data-stu-id="b4ea5-163">SLES</span></span> |<span data-ttu-id="b4ea5-164">SLES</span><span class="sxs-lookup"><span data-stu-id="b4ea5-164">SLES</span></span> |<span data-ttu-id="b4ea5-165">SLES</span><span class="sxs-lookup"><span data-stu-id="b4ea5-165">SLES</span></span> |<span data-ttu-id="b4ea5-166">12-SP1</span><span class="sxs-lookup"><span data-stu-id="b4ea5-166">12-SP1</span></span> |<span data-ttu-id="b4ea5-167">legújabb</span><span class="sxs-lookup"><span data-stu-id="b4ea5-167">latest</span></span> |
| <span data-ttu-id="b4ea5-168">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="b4ea5-168">UbuntuLTS</span></span> |<span data-ttu-id="b4ea5-169">Canonical</span><span class="sxs-lookup"><span data-stu-id="b4ea5-169">Canonical</span></span> |<span data-ttu-id="b4ea5-170">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="b4ea5-170">UbuntuServer</span></span> |<span data-ttu-id="b4ea5-171">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="b4ea5-171">14.04.4-LTS</span></span> |<span data-ttu-id="b4ea5-172">legújabb</span><span class="sxs-lookup"><span data-stu-id="b4ea5-172">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="b4ea5-173">Saját rendszerkép használata</span><span class="sxs-lookup"><span data-stu-id="b4ea5-173">Use your own image</span></span>
<span data-ttu-id="b4ea5-174">Ha speciális egyéni beállításokra van szüksége, használhat egy meglévő Azure virtuális gépen alapuló rendszerképet a virtuális gép rögzítésével.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-174">If you require specific customizations, you can use an image based on an existing Azure VM by capturing that VM.</span></span> <span data-ttu-id="b4ea5-175">Emellett feltölthet egy helyszínen létrehozott rendszerképet is.</span><span class="sxs-lookup"><span data-stu-id="b4ea5-175">You can also upload an image created on-premises.</span></span> <span data-ttu-id="b4ea5-176">További információ a támogatott disztribúciókkal és hogyan toouse a saját lemezképek: hello következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-176">For more information on supported distros and how toouse your own images, see hello following articles:</span></span>

* [<span data-ttu-id="b4ea5-177">Azure által támogatott disztribúciók</span><span class="sxs-lookup"><span data-stu-id="b4ea5-177">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="b4ea5-178">Nem támogatott disztribúciókkal kapcsolatos tudnivalók</span><span class="sxs-lookup"><span data-stu-id="b4ea5-178">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* <span data-ttu-id="b4ea5-179">[Hogyan toocreate az egy meglévő Azure virtuális gép lemezképének](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="b4ea5-179">[How toocreate an image from an existing Azure VM](tutorial-custom-images.md).</span></span>
  
  * <span data-ttu-id="b4ea5-180">Gyors üzembe helyezési példa parancsok toocreate az egy meglévő Azure virtuális gép lemezképét:</span><span class="sxs-lookup"><span data-stu-id="b4ea5-180">Quick-start example commands toocreate an image from an existing Azure VM:</span></span>
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a><span data-ttu-id="b4ea5-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4ea5-181">Next steps</span></span>
* <span data-ttu-id="b4ea5-182">Hozzon létre egy Linux virtuális gép hello [CLI](quick-create-cli.md), a hello [portal](quick-create-portal.md), vagy egy [Azure Resource Manager sablon](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b4ea5-182">Create a Linux VM with hello [CLI](quick-create-cli.md), from hello [portal](quick-create-portal.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="b4ea5-183">A Linux virtuális gép létrehozása után [ismerje meg az Azure lemezeket és tárolót](tutorial-manage-disks.md).</span><span class="sxs-lookup"><span data-stu-id="b4ea5-183">After creating a Linux VM, [learn about Azure disks and storage](tutorial-manage-disks.md).</span></span>
* <span data-ttu-id="b4ea5-184">Gyors lépések túl[jelszó vagy SSH-kulcsok alaphelyzetbe és felhasználók kezeléséhez](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="b4ea5-184">Quick steps too[reset a password or SSH keys and manage users](using-vmaccess-extension.md).</span></span>
