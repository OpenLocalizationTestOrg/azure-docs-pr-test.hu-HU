---
title: "a Linux virtuális gépeken, az SMB-Azure File storage aaaMount |} Microsoft Docs"
description: "Hogyan toomount Azure File storage a Linux virtuális gépeken, az SMB protokoll segítségével hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 7b34c81e602748b78c924f44a54b876744c28f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a><span data-ttu-id="a29b3-103">A Linux virtuális gépeken, az SMB-csatlakoztatási Azure fájltároló</span><span class="sxs-lookup"><span data-stu-id="a29b3-103">Mount Azure File storage on Linux VMs using SMB</span></span>

<span data-ttu-id="a29b3-104">Ez a cikk bemutatja, hogyan tooutilize hello Azure File storage szolgáltatást a Linux virtuális gépet egy SMB csatlakoztatása az Azure CLI 2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="a29b3-104">This article shows you how tooutilize hello Azure File storage service on a Linux VM using an SMB mount with hello Azure CLI 2.0.</span></span> <span data-ttu-id="a29b3-105">Az Azure File storage hello szabványos SMB protokollt használó hello felhőben fájlmegosztásokat kínál.</span><span class="sxs-lookup"><span data-stu-id="a29b3-105">Azure File storage offers file shares in hello cloud using hello standard SMB protocol.</span></span> <span data-ttu-id="a29b3-106">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a29b3-106">You can also perform these steps with hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="a29b3-107">hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="a29b3-107">hello requirements are:</span></span>

- [<span data-ttu-id="a29b3-108">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="a29b3-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="a29b3-109">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="a29b3-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="quick-commands"></a><span data-ttu-id="a29b3-110">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="a29b3-110">Quick Commands</span></span>

* <span data-ttu-id="a29b3-111">Egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="a29b3-111">A resource group</span></span>
* <span data-ttu-id="a29b3-112">Egy Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="a29b3-112">An Azure virtual network</span></span>
* <span data-ttu-id="a29b3-113">Az SSH a hálózati biztonsági csoportok bejövő</span><span class="sxs-lookup"><span data-stu-id="a29b3-113">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="a29b3-114">Egy alhálózaton</span><span class="sxs-lookup"><span data-stu-id="a29b3-114">A subnet</span></span>
* <span data-ttu-id="a29b3-115">Egy Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="a29b3-115">An Azure storage account</span></span>
* <span data-ttu-id="a29b3-116">Az Azure storage-fiók kulcsok</span><span class="sxs-lookup"><span data-stu-id="a29b3-116">Azure storage account keys</span></span>
* <span data-ttu-id="a29b3-117">Az Azure File storage-megosztás</span><span class="sxs-lookup"><span data-stu-id="a29b3-117">An Azure File storage share</span></span>
* <span data-ttu-id="a29b3-118">Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="a29b3-118">A Linux VM</span></span>

<span data-ttu-id="a29b3-119">Cserélje le olyan saját beállításaival.</span><span class="sxs-lookup"><span data-stu-id="a29b3-119">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-hello-local-mount"></a><span data-ttu-id="a29b3-120">Hozzon létre egy könyvtárat hello helyi csatlakoztatási</span><span class="sxs-lookup"><span data-stu-id="a29b3-120">Create a directory for hello local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a><span data-ttu-id="a29b3-121">Hello fájl tárolási SMB megosztás toohello csatlakoztatási pont csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="a29b3-121">Mount hello File storage SMB share toohello mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a><span data-ttu-id="a29b3-122">Újraindítás után is megmaradjanak hello csatlakoztatási</span><span class="sxs-lookup"><span data-stu-id="a29b3-122">Persist hello mount after a reboot</span></span>
<span data-ttu-id="a29b3-123">toodo Igen, vegye fel a következő sor toohello hello `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="a29b3-123">toodo so, add hello following line toohello `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="a29b3-124">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="a29b3-124">Detailed walkthrough</span></span>

<span data-ttu-id="a29b3-125">A File storage hello felhőben használó fájlmegosztásokról hello szabványos SMB-protokollt biztosít.</span><span class="sxs-lookup"><span data-stu-id="a29b3-125">File storage offers file shares in hello cloud that use hello standard SMB protocol.</span></span> <span data-ttu-id="a29b3-126">A File storage kiadása legújabb hello is minden os, amely támogatja az SMB 3.0 lehet csatlakoztatni fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="a29b3-126">With hello latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="a29b3-127">Az SMB-csatlakoztatási Linux rendszeren való használatakor kapott egyszerű biztonsági mentést tooa robusztus, állandó archiválási tárolóhely szolgáltatásiszint-szerződésben garantált által támogatott.</span><span class="sxs-lookup"><span data-stu-id="a29b3-127">When you use an SMB mount on Linux, you get easy backups tooa robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="a29b3-128">Helyezze át fájlokat a virtuális gép tooan SMB csatlakoztatási tárolt fájlok tárolására egy remek mód toodebug naplózza.</span><span class="sxs-lookup"><span data-stu-id="a29b3-128">Moving files from a VM tooan SMB mount that's hosted on File storage is a great way toodebug logs.</span></span> <span data-ttu-id="a29b3-129">Ennek oka azonos SMB-megosztási hello helyileg lehet csatlakoztatni tooyour Mac, Linux vagy a Windows munkaállomás.</span><span class="sxs-lookup"><span data-stu-id="a29b3-129">That's because hello same SMB share can be mounted locally tooyour Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="a29b3-130">Az SMB nem hello legjobb megoldás az adatfolyamként történő Linux vagy alkalmazásnaplók valós időben, mert hello SMB protokoll nem épített toohandle ilyen nehéz naplózási feladatokat.</span><span class="sxs-lookup"><span data-stu-id="a29b3-130">SMB isn't hello best solution for streaming Linux or application logs in real time, because hello SMB protocol is not built toohandle such heavy logging duties.</span></span> <span data-ttu-id="a29b3-131">Dedikált, egységes naplózási réteg eszközhöz hasonló eszközökkel Fluentd jobb megoldás, mint az SMB lenne a Linux és a naplózás a kimeneti alkalmazás gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="a29b3-131">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="a29b3-132">A részletes forgatókönyv létrehozhatunk hello Előfeltételek szükség toofirst hello tárolási fájlmegosztás létrehozása, és csatlakoztathatja SMB-protokollal a Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a29b3-132">For this detailed walkthrough, we create hello prerequisites needed toofirst create hello File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="a29b3-133">Hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create) toohold hello fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="a29b3-133">Create a resource group with [az group create](/cli/azure/group#create) toohold hello file share.</span></span>

    <span data-ttu-id="a29b3-134">Erőforráscsoport nevű toocreate `myResourceGroup` hello "USA nyugati régiója" helyre, használja a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="a29b3-134">toocreate a resource group named `myResourceGroup` in hello "West US" location, use hello following example:</span></span>

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. <span data-ttu-id="a29b3-135">Az Azure storage-fiók létrehozása [az storage-fiók létrehozása](/cli/azure/storage/account#create) toostore hello tényleges fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="a29b3-135">Create an Azure storage account with [az storage account create](/cli/azure/storage/account#create) toostore hello actual files.</span></span>

    <span data-ttu-id="a29b3-136">a tárfiók toocreate mystorageaccount nevű hello Standard_LRS tárolási Termékváltozat, a következő példa használata hello segítségével:</span><span class="sxs-lookup"><span data-stu-id="a29b3-136">toocreate a storage account named mystorageaccount by using hello Standard_LRS storage SKU, use hello following example:</span></span>

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. <span data-ttu-id="a29b3-137">Hello tárfiókkulcsok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a29b3-137">Show hello storage account keys.</span></span>

    <span data-ttu-id="a29b3-138">Amikor létrehoz egy tárfiókot, hello kulcsait jönnek létre párok, hogy a szolgáltatás megszakítás nélkül forgatható el.</span><span class="sxs-lookup"><span data-stu-id="a29b3-138">When you create a storage account, hello account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="a29b3-139">Amikor hello párokban szereplő toohello második kulcsot, hozzon létre egy új kulcspárt.</span><span class="sxs-lookup"><span data-stu-id="a29b3-139">When you switch toohello second key in hello pair, you create a new key pair.</span></span> <span data-ttu-id="a29b3-140">Új tárfiókkulcsok mindig párosával győződjön meg arról, hogy mindig legyen legalább egy nem használt tárolási fiók kulcs készen tooswitch való jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="a29b3-140">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage account key ready tooswitch to.</span></span>

    <span data-ttu-id="a29b3-141">Hello tárfiókkulcsok a hello megtekintése [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="a29b3-141">View hello storage account keys with hello [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="a29b3-142">hello nevű hello tárfiókkulcsainak `mystorageaccount` jelennek meg a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="a29b3-142">hello storage account keys for hello named `mystorageaccount` are listed in hello following example:</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    <span data-ttu-id="a29b3-143">tooextract egy kulcs, használja a hello `--query` jelzőt.</span><span class="sxs-lookup"><span data-stu-id="a29b3-143">tooextract a single key, use hello `--query` flag.</span></span> <span data-ttu-id="a29b3-144">hello alábbi példa kibontja hello első kulcsát (`[0]`):</span><span class="sxs-lookup"><span data-stu-id="a29b3-144">hello following example extracts hello first key (`[0]`):</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. <span data-ttu-id="a29b3-145">Hello File storage-megosztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a29b3-145">Create hello File storage share.</span></span>

    <span data-ttu-id="a29b3-146">hello File storage-megosztás az SMB-megosztási hello tartalmaz [az storage-megosztás létrehozása](/cli/azure/storage/share#create).</span><span class="sxs-lookup"><span data-stu-id="a29b3-146">hello File storage share contains hello SMB share with [az storage share create](/cli/azure/storage/share#create).</span></span> <span data-ttu-id="a29b3-147">hello kvóta mindig gigabájt (GB) van megadva.</span><span class="sxs-lookup"><span data-stu-id="a29b3-147">hello quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="a29b3-148">Előző hello hello kulcsok egyikével átadása `az storage account keys list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a29b3-148">Pass in one of hello keys from hello preceding `az storage account keys list` command.</span></span> <span data-ttu-id="a29b3-149">10 GB-os kvótával mystorageshare nevű hello a következő példa a megosztás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="a29b3-149">Create a share named mystorageshare with a 10-GB quota by using hello following example:</span></span>

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. <span data-ttu-id="a29b3-150">Hozzon létre egy csatlakoztatásipont-könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="a29b3-150">Create a mount-point directory.</span></span>

    <span data-ttu-id="a29b3-151">Hozzon létre egy helyi könyvtárba hello Linux fájl rendszer toomount hello SMB-megosztás.</span><span class="sxs-lookup"><span data-stu-id="a29b3-151">Create a local directory in hello Linux file system toomount hello SMB share to.</span></span> <span data-ttu-id="a29b3-152">SMB-megosztási toohello, amely a File storage semmit írt vagy hello helyi csatlakoztatási könyvtárból olvassa továbbítja.</span><span class="sxs-lookup"><span data-stu-id="a29b3-152">Anything written or read from hello local mount directory is forwarded toohello SMB share that's hosted on File storage.</span></span> <span data-ttu-id="a29b3-153">egy helyi könyvtár: / mnt/mymountdirectory, például a következő használatát hello toocreate:</span><span class="sxs-lookup"><span data-stu-id="a29b3-153">toocreate a local directory at /mnt/mymountdirectory, use hello following example:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. <span data-ttu-id="a29b3-154">Csatlakoztatási hello SMB megosztás toohello helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="a29b3-154">Mount hello SMB share toohello local directory.</span></span>

    <span data-ttu-id="a29b3-155">Az alábbiak szerint adja meg a saját tárolási fiók felhasználónevét és a tárfiók kulcsa hello csatlakoztatási hitelesítő adatokat:</span><span class="sxs-lookup"><span data-stu-id="a29b3-155">Provide your own storage account username and storage account key for hello mount credentials as follows:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. <span data-ttu-id="a29b3-156">SMB csatlakoztatási keresztül újraindítások hello megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="a29b3-156">Persist hello SMB mount through reboots.</span></span>

    <span data-ttu-id="a29b3-157">Linux virtuális gép hello újraindításakor hello csatlakoztatott SMB-megosztáson fürtköteteiről leállítás során.</span><span class="sxs-lookup"><span data-stu-id="a29b3-157">When you reboot hello Linux VM, hello mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="a29b3-158">SMB-megosztási az rendszerindításkor tooremount hello hozzáadása egy sor toohello Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="a29b3-158">tooremount hello SMB share on boot, add a line toohello Linux /etc/fstab.</span></span> <span data-ttu-id="a29b3-159">Linux hello fstab fájl toolist hello fájlrendszerek toomount jogcímadatokat hello rendszerindítási folyamat során használ.</span><span class="sxs-lookup"><span data-stu-id="a29b3-159">Linux uses hello fstab file toolist hello file systems that it needs toomount during hello boot process.</span></span> <span data-ttu-id="a29b3-160">Hozzáadását hello SMB-megosztás biztosítja, hogy hello File storage-megosztás véglegesen csatlakoztatott fájlrendszer a hello Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a29b3-160">Adding hello SMB share ensures that hello File storage share is a permanently mounted file system for hello Linux VM.</span></span> <span data-ttu-id="a29b3-161">Hello fájl tárolási SMB megosztás tooa hozzáadása új virtuális gép esetén lehetséges felhő inicializálás.</span><span class="sxs-lookup"><span data-stu-id="a29b3-161">Adding hello File storage SMB share tooa new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="a29b3-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a29b3-162">Next steps</span></span>

- [<span data-ttu-id="a29b3-163">Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="a29b3-163">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="a29b3-164">A lemez tooa Linux virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a29b3-164">Add a disk tooa Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="a29b3-165">Linux virtuális gép lemezeinek titkosítása hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a29b3-165">Encrypt disks on a Linux VM by using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
