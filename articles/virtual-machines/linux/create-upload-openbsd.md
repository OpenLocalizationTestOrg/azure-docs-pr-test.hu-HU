---
title: "aaaCreate és feltöltése az OpenBSD virtuális gép kép tooAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate és feltöltése egy virtuális merevlemez (VHD) tartalmazó hello OpenBSD operációs rendszer toocreate egy Azure virtuális gépet az Azure parancssori felület használatával"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: kyliel
ms.openlocfilehash: 0524f45ea1ecec37e55cbe9d54438a571a831de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-tooazure"></a><span data-ttu-id="0c382-103">Hozzon létre, és töltse fel az OpenBSD lemez kép tooAzure</span><span class="sxs-lookup"><span data-stu-id="0c382-103">Create and Upload an OpenBSD disk image tooAzure</span></span>
<span data-ttu-id="0c382-104">Ez a cikk bemutatja, hogyan toocreate és feltöltése egy virtuális merevlemez (VHD) tartalmazó hello OpenBSD operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="0c382-104">This article shows you how toocreate and upload a virtual hard disk (VHD) that contains hello OpenBSD operating system.</span></span> <span data-ttu-id="0c382-105">Után a feltöltés, mint a saját kép toocreate egy virtuális gép (VM) az Azure parancssori felületen keresztül Azure-ban használhatja.</span><span class="sxs-lookup"><span data-stu-id="0c382-105">After you upload it, you can use it as your own image toocreate a virtual machine (VM) in Azure through Azure CLI.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0c382-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0c382-106">Prerequisites</span></span>
<span data-ttu-id="0c382-107">Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0c382-107">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="0c382-108">**Azure-előfizetés** – Ha nincs fiókja, létrehozhat egyet, néhány perc alatt.</span><span class="sxs-lookup"><span data-stu-id="0c382-108">**An Azure subscription** - If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="0c382-109">Ha MSDN-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="0c382-109">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="0c382-110">Ellenkező esetben megtudhatja, hogyan túl[hozzon létre egy próbafiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0c382-110">Otherwise, learn how too[create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="0c382-111">**Az Azure CLI 2.0** -legyen hello legújabb [Azure CLI 2.0](/cli/azure/install-azure-cli) telepítve, és az Azure-fiók tooyour bejelentkezve [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="0c382-111">**Azure CLI 2.0** - Make sure you have hello latest [Azure CLI 2.0](/cli/azure/install-azure-cli) installed and logged in tooyour Azure account with [az login](/cli/azure/#login).</span></span>
* <span data-ttu-id="0c382-112">**OpenBSD operációs rendszer van telepítve, a .vhd-fájllá** -OpenBSD támogatott operációs rendszer (6.1-es verzió) telepített tooa virtuális merevlemezt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0c382-112">**OpenBSD operating system installed in a .vhd file** - A supported OpenBSD operating system (6.1 version) must be installed tooa virtual hard disk.</span></span> <span data-ttu-id="0c382-113">Több különféle eszköz toocreate .vhd fájlok léteznek.</span><span class="sxs-lookup"><span data-stu-id="0c382-113">Multiple tools exist toocreate .vhd files.</span></span> <span data-ttu-id="0c382-114">Például egy virtualizálási megoldást használja például a Hyper-V toocreate hello .vhd fájl, és hello operációs rendszer telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0c382-114">For example, you can use a virtualization solution such as Hyper-V toocreate hello .vhd file and install hello operating system.</span></span> <span data-ttu-id="0c382-115">Leírja, hogyan tooinstall és használja a Hyper-V, tekintse meg a [telepítése Hyper-V virtuális gép létrehozása és](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c382-115">For instructions about how tooinstall and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>


## <a name="prepare-openbsd-image-for-azure"></a><span data-ttu-id="0c382-116">Az Azure-OpenBSD lemezkép előkészítése</span><span class="sxs-lookup"><span data-stu-id="0c382-116">Prepare OpenBSD image for Azure</span></span>
<span data-ttu-id="0c382-117">Hello VM hello OpenBSD operációs rendszer 6.1, amely a Hyper-V támogatása, amelyen a hajtsa végre a következő eljárások hello:</span><span class="sxs-lookup"><span data-stu-id="0c382-117">On hello VM where you installed hello OpenBSD operating system 6.1, which added Hyper-V support, complete hello following procedures:</span></span>

1. <span data-ttu-id="0c382-118">Ha a DHCP telepítése során nem engedélyezett, engedélyezze a hello szolgáltatást az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-118">If DHCP is not enabled during installation, enable hello service as follows:</span></span>

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. <span data-ttu-id="0c382-119">Az alábbiak szerint állíthatja be a soros konzol:</span><span class="sxs-lookup"><span data-stu-id="0c382-119">Set up a serial console as follows:</span></span>

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. <span data-ttu-id="0c382-120">Konfigurálja a csomag telepítése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-120">Configure Package installation as follows:</span></span>

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. <span data-ttu-id="0c382-121">Alapértelmezés szerint hello `root` felhasználó le van tiltva, az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="0c382-121">By default, hello `root` user is disabled on virtual machines in Azure.</span></span> <span data-ttu-id="0c382-122">Felhasználók futtathatják a parancsokat emelt szintű jogosultságokkal hello segítségével `doas` OpenBSD VM parancsot.</span><span class="sxs-lookup"><span data-stu-id="0c382-122">Users can run commands with elevated privileges by using hello `doas` command on OpenBSD VM.</span></span> <span data-ttu-id="0c382-123">Doas alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="0c382-123">Doas is enabled by default.</span></span> <span data-ttu-id="0c382-124">További információkért lásd: [doas.conf](http://man.openbsd.org/doas.conf.5).</span><span class="sxs-lookup"><span data-stu-id="0c382-124">For more information, see [doas.conf](http://man.openbsd.org/doas.conf.5).</span></span> 

5. <span data-ttu-id="0c382-125">Telepítse és konfigurálja az alábbiak szerint hello Azure ügynök előfeltételei:</span><span class="sxs-lookup"><span data-stu-id="0c382-125">Install and configure prerequisites for hello Azure Agent as follows:</span></span>

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. <span data-ttu-id="0c382-126">hello Azure ügynök legújabb kiadása hello mindig található [Github](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="0c382-126">hello latest release of hello Azure agent can always be found on [Github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="0c382-127">Hello ügynök telepítése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-127">Install hello agent as follows:</span></span>

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="0c382-128">Azure-ügynök telepítése után egy jó ötlet tooverify, hogy fut-e az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-128">After you install Azure Agent, it's a good idea tooverify that it's running as follows:</span></span>
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. <span data-ttu-id="0c382-129">Deprovision hello rendszer tooclean, és ellenőrizze, megfelelő-e reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="0c382-129">Deprovision hello system tooclean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="0c382-130">hello következő parancs is törli hello utolsó kiosztott felhasználói fiókkal és a kapcsolódó hello adatokat:</span><span class="sxs-lookup"><span data-stu-id="0c382-130">hello following command also deletes hello last provisioned user account and hello associated data:</span></span>

    ```sh
    waagent -deprovision+user -force
    ```

<span data-ttu-id="0c382-131">Most, állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="0c382-131">Now you can shut down your VM.</span></span>


## <a name="prepare-hello-vhd"></a><span data-ttu-id="0c382-132">Hello virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="0c382-132">Prepare hello VHD</span></span>
<span data-ttu-id="0c382-133">hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.</span><span class="sxs-lookup"><span data-stu-id="0c382-133">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span> <span data-ttu-id="0c382-134">Átalakítás hello toofixed VHD formátummal Hyper-V kezelőjével vagy a hello Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0c382-134">You can convert hello disk toofixed VHD format using Hyper-V Manager or hello Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet.</span></span> <span data-ttu-id="0c382-135">Példa van, mint következő.</span><span class="sxs-lookup"><span data-stu-id="0c382-135">An example is as following.</span></span>

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a><span data-ttu-id="0c382-136">Tároló-erőforrások létrehozása és feltöltése</span><span class="sxs-lookup"><span data-stu-id="0c382-136">Create storage resources and upload</span></span>
<span data-ttu-id="0c382-137">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="0c382-137">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="0c382-138">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="0c382-138">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="0c382-139">tooupload a VHD-t, hozzon létre egy tárfiókot a [az storage-fiók létrehozása](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="0c382-139">tooupload your VHD, create a storage account with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="0c382-140">Tárfiókneveket kell egyedinek lennie, ezért adja meg saját nevét.</span><span class="sxs-lookup"><span data-stu-id="0c382-140">Storage account names must be unique, so provide your own name.</span></span> <span data-ttu-id="0c382-141">hello alábbi példa létrehoz egy nevű tárfiók *mystorageaccount*:</span><span class="sxs-lookup"><span data-stu-id="0c382-141">hello following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

<span data-ttu-id="0c382-142">toocontrol toohello tárfiók eléréséhez, hello biztonságitár-kulcs az beszerzése [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-142">toocontrol access toohello storage account, obtain hello storage key with [az storage account keys list](/cli/azure/storage/account/keys#list) as follows:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

<span data-ttu-id="0c382-143">toologically külön hello tölt fel, virtuális merevlemezek létrehozni egy tárolót a hello tárfiókon belül [az tároló létrehozása](/cli/azure/storage/container#create):</span><span class="sxs-lookup"><span data-stu-id="0c382-143">toologically separate hello VHDs you upload, create a container within hello storage account with [az storage container create](/cli/azure/storage/container#create):</span></span>

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

<span data-ttu-id="0c382-144">Végül, töltse fel a virtuális merevlemez [az tárolási blob feltöltése](/cli/azure/storage/blob#upload) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-144">Finally, upload your VHD with [az storage blob upload](/cli/azure/storage/blob#upload) as follows:</span></span>

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a><span data-ttu-id="0c382-145">A virtuális Merevlemezt a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0c382-145">Create VM from your VHD</span></span>
<span data-ttu-id="0c382-146">Virtuális gép és hozhat létre egy [mintaparancsfájl](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) vagy közvetlenül az [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="0c382-146">You can create a VM with a [sample script](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) or directly with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="0c382-147">toospecify hello OpenBSD VHD feltöltött, használjon hello `--image` paraméter az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-147">toospecify hello OpenBSD VHD you uploaded, use hello `--image` parameter as follows:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="0c382-148">Hello IP-cím beszerzése a OpenBSD tulajdonsággal rendelkező virtuális gépet [az vm-ip-címeinek listáját](/cli/azure/vm#list-ip-addresses) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0c382-148">Obtain hello IP address for your OpenBSD VM with [az vm list-ip-addresses](/cli/azure/vm#list-ip-addresses) as follows:</span></span>

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

<span data-ttu-id="0c382-149">Most SSH tooyour OpenBSD VM szokásos módon teheti:</span><span class="sxs-lookup"><span data-stu-id="0c382-149">Now you can SSH tooyour OpenBSD VM as normal:</span></span>
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a><span data-ttu-id="0c382-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c382-150">Next steps</span></span>
<span data-ttu-id="0c382-151">Ha azt szeretné, hogy további információk a Hyper-V támogatja a OpenBSD6.1 tooknow, olvassa el [OpenBSD 6.1](https://www.openbsd.org/61.html) és [hyperv.4](http://man.openbsd.org/hyperv.4).</span><span class="sxs-lookup"><span data-stu-id="0c382-151">If you want tooknow more about Hyper-V support on OpenBSD6.1, read [OpenBSD 6.1](https://www.openbsd.org/61.html) and [hyperv.4](http://man.openbsd.org/hyperv.4).</span></span>

<span data-ttu-id="0c382-152">Ha azt szeretné, hogy a felügyelt lemezes virtuális gép toocreate, olvassa el [az lemez](/cli/azure/disk).</span><span class="sxs-lookup"><span data-stu-id="0c382-152">If you want toocreate a VM from managed disk, read [az disk](/cli/azure/disk).</span></span> 
