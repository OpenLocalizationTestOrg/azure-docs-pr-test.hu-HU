---
title: "A Linux virtuális gépeken, az SMB-csatlakoztatási Azure File storage |} Microsoft Docs"
description: "A Linux virtuális gépeken SMB használata az Azure CLI 2.0 Azure File storage csatlakoztatásáról"
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
ms.openlocfilehash: 9eae17b304f8a987b44ebed8906dabd8ff3a36a8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a><span data-ttu-id="9695a-103">A Linux virtuális gépeken, az SMB-csatlakoztatási Azure fájltároló</span><span class="sxs-lookup"><span data-stu-id="9695a-103">Mount Azure File storage on Linux VMs using SMB</span></span>

<span data-ttu-id="9695a-104">Ez a cikk bemutatja, hogyan használják a Azure storage szolgáltatás a Linux virtuális gép az SMB-csatlakoztatási használata az Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="9695a-104">This article shows you how to utilize the Azure File storage service on a Linux VM using an SMB mount with the Azure CLI 2.0.</span></span> <span data-ttu-id="9695a-105">Az Azure File storage kínál a felhőben, szabványos SMB protokollt használó fájlmegosztások.</span><span class="sxs-lookup"><span data-stu-id="9695a-105">Azure File storage offers file shares in the cloud using the standard SMB protocol.</span></span> <span data-ttu-id="9695a-106">Az [Azure CLI 1.0-s](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9695a-106">You can also perform these steps with the [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="9695a-107">Követelmények:</span><span class="sxs-lookup"><span data-stu-id="9695a-107">The requirements are:</span></span>

- [<span data-ttu-id="9695a-108">egy Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="9695a-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="9695a-109">SSH nyilvános- és titkoskulcs-fájlok</span><span class="sxs-lookup"><span data-stu-id="9695a-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="quick-commands"></a><span data-ttu-id="9695a-110">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="9695a-110">Quick Commands</span></span>

* <span data-ttu-id="9695a-111">Egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="9695a-111">A resource group</span></span>
* <span data-ttu-id="9695a-112">Egy Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="9695a-112">An Azure virtual network</span></span>
* <span data-ttu-id="9695a-113">Az SSH a hálózati biztonsági csoportok bejövő</span><span class="sxs-lookup"><span data-stu-id="9695a-113">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="9695a-114">Egy alhálózaton</span><span class="sxs-lookup"><span data-stu-id="9695a-114">A subnet</span></span>
* <span data-ttu-id="9695a-115">Egy Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="9695a-115">An Azure storage account</span></span>
* <span data-ttu-id="9695a-116">Az Azure storage-fiók kulcsok</span><span class="sxs-lookup"><span data-stu-id="9695a-116">Azure storage account keys</span></span>
* <span data-ttu-id="9695a-117">Az Azure File storage-megosztás</span><span class="sxs-lookup"><span data-stu-id="9695a-117">An Azure File storage share</span></span>
* <span data-ttu-id="9695a-118">Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="9695a-118">A Linux VM</span></span>

<span data-ttu-id="9695a-119">Cserélje le olyan saját beállításaival.</span><span class="sxs-lookup"><span data-stu-id="9695a-119">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-the-local-mount"></a><span data-ttu-id="9695a-120">Hozzon létre egy könyvtárat a helyi csatlakoztatási</span><span class="sxs-lookup"><span data-stu-id="9695a-120">Create a directory for the local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a><span data-ttu-id="9695a-121">A fájltároló SMB-megosztás a csatlakoztatási pont csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="9695a-121">Mount the File storage SMB share to the mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a><span data-ttu-id="9695a-122">Újraindítás után is megmaradjanak a csatlakoztatási</span><span class="sxs-lookup"><span data-stu-id="9695a-122">Persist the mount after a reboot</span></span>
<span data-ttu-id="9695a-123">Ehhez adja hozzá a következő sort a `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="9695a-123">To do so, add the following line to the `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="9695a-124">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="9695a-124">Detailed walkthrough</span></span>

<span data-ttu-id="9695a-125">A File storage fájlmegosztások a felhőben, amelyek a szabványos SMB-protokollt biztosít.</span><span class="sxs-lookup"><span data-stu-id="9695a-125">File storage offers file shares in the cloud that use the standard SMB protocol.</span></span> <span data-ttu-id="9695a-126">A legújabb kiadásra frissüljön a File storage is minden os, amely támogatja az SMB 3.0 lehet csatlakoztatni egy fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="9695a-126">With the latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="9695a-127">Az SMB-csatlakoztatási Linux rendszeren való használatakor egy robusztus, állandó archiválási tárolóhelyre szolgáltatásiszint-szerződésben garantált által támogatott kap egyszerű biztonsági mentést.</span><span class="sxs-lookup"><span data-stu-id="9695a-127">When you use an SMB mount on Linux, you get easy backups to a robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="9695a-128">Helyezze át a fájlokat egy virtuális gépről a File storage egy üzemeltetett SMB-csatlakoztatási kiváló módja annak, hogy hibakeresési naplókat.</span><span class="sxs-lookup"><span data-stu-id="9695a-128">Moving files from a VM to an SMB mount that's hosted on File storage is a great way to debug logs.</span></span> <span data-ttu-id="9695a-129">Ennek oka az, az SMB-megosztáshoz lehet csatlakoztatni helyileg a Mac, Linux vagy a Windows munkaállomás felé áramlik.</span><span class="sxs-lookup"><span data-stu-id="9695a-129">That's because the same SMB share can be mounted locally to your Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="9695a-130">Az SMB nem a legjobb megoldás az adatfolyam-Linux vagy alkalmazásnaplók valós időben, mert az SMB protokoll nem lett elkészítve ilyen gyakori naplózási feladatok kezelésére.</span><span class="sxs-lookup"><span data-stu-id="9695a-130">SMB isn't the best solution for streaming Linux or application logs in real time, because the SMB protocol is not built to handle such heavy logging duties.</span></span> <span data-ttu-id="9695a-131">Dedikált, egységes naplózási réteg eszközhöz hasonló eszközökkel Fluentd jobb megoldás, mint az SMB lenne a Linux és a naplózás a kimeneti alkalmazás gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="9695a-131">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="9695a-132">A részletes forgatókönyv azt létrehozásához először létre kell hoznia a File storage-megosztás szükséges előfeltételeket, és csatlakoztathatja SMB-protokollal a Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="9695a-132">For this detailed walkthrough, we create the prerequisites needed to first create the File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="9695a-133">Hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create) ahhoz, hogy a fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="9695a-133">Create a resource group with [az group create](/cli/azure/group#create) to hold the file share.</span></span>

    <span data-ttu-id="9695a-134">Nevű erőforráscsoport létrehozásához `myResourceGroup` az "USA nyugati régiója" helyen, használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="9695a-134">To create a resource group named `myResourceGroup` in the "West US" location, use the following example:</span></span>

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. <span data-ttu-id="9695a-135">Az Azure storage-fiók létrehozása [az storage-fiók létrehozása](/cli/azure/storage/account#create) a tényleges fájlok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="9695a-135">Create an Azure storage account with [az storage account create](/cli/azure/storage/account#create) to store the actual files.</span></span>

    <span data-ttu-id="9695a-136">A Standard_LRS tárolási SKU használatával mystorageaccount nevű tárfiók létrehozásához használja a következő példát:</span><span class="sxs-lookup"><span data-stu-id="9695a-136">To create a storage account named mystorageaccount by using the Standard_LRS storage SKU, use the following example:</span></span>

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. <span data-ttu-id="9695a-137">A tárfiók kulcsait megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="9695a-137">Show the storage account keys.</span></span>

    <span data-ttu-id="9695a-138">Amikor létrehoz egy tárfiókot, a kulcsait jönnek létre párok, hogy a szolgáltatás megszakítás nélkül forgatható el.</span><span class="sxs-lookup"><span data-stu-id="9695a-138">When you create a storage account, the account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="9695a-139">Amikor a második kulcsot a párok, hozzon létre új kulcspár.</span><span class="sxs-lookup"><span data-stu-id="9695a-139">When you switch to the second key in the pair, you create a new key pair.</span></span> <span data-ttu-id="9695a-140">Új tárfiókkulcsok mindig párosával győződjön meg arról, hogy mindig legyen legalább egy nem használt tárfiók kulcsa készen áll a Váltás jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="9695a-140">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage account key ready to switch to.</span></span>

    <span data-ttu-id="9695a-141">A tárfiók kulcsait a megtekintése a [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="9695a-141">View the storage account keys with the [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="9695a-142">A tárfiók a kulcsok az a megnevezett `mystorageaccount` a következő példában látható:</span><span class="sxs-lookup"><span data-stu-id="9695a-142">The storage account keys for the named `mystorageaccount` are listed in the following example:</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    <span data-ttu-id="9695a-143">Egy kulcs kibontásához használja a `--query` jelzőt.</span><span class="sxs-lookup"><span data-stu-id="9695a-143">To extract a single key, use the `--query` flag.</span></span> <span data-ttu-id="9695a-144">Az alábbi példa kibontja az első kulcsát (`[0]`):</span><span class="sxs-lookup"><span data-stu-id="9695a-144">The following example extracts the first key (`[0]`):</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. <span data-ttu-id="9695a-145">A tárolási fájlmegosztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9695a-145">Create the File storage share.</span></span>

    <span data-ttu-id="9695a-146">A File storage-megosztás az SMB-megosztás tartalmaz [az storage-megosztás létrehozása](/cli/azure/storage/share#create).</span><span class="sxs-lookup"><span data-stu-id="9695a-146">The File storage share contains the SMB share with [az storage share create](/cli/azure/storage/share#create).</span></span> <span data-ttu-id="9695a-147">A kvóta mindig gigabájt (GB) van megadva.</span><span class="sxs-lookup"><span data-stu-id="9695a-147">The quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="9695a-148">Az egyik kulcsának fázisra az előző `az storage account keys list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9695a-148">Pass in one of the keys from the preceding `az storage account keys list` command.</span></span> <span data-ttu-id="9695a-149">Hozzon létre egy megosztást, 10 GB-os kvótával mystorageshare nevű az alábbi példa szerint:</span><span class="sxs-lookup"><span data-stu-id="9695a-149">Create a share named mystorageshare with a 10-GB quota by using the following example:</span></span>

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. <span data-ttu-id="9695a-150">Hozzon létre egy csatlakoztatásipont-könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="9695a-150">Create a mount-point directory.</span></span>

    <span data-ttu-id="9695a-151">Hozzon létre egy helyi könyvtárat a Linux-fájlrendszer, az SMB-megosztáson való csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="9695a-151">Create a local directory in the Linux file system to mount the SMB share to.</span></span> <span data-ttu-id="9695a-152">Bármi írt, vagy olvassa el a helyi csatlakoztatási könyvtárból továbbíthatja a rendszer az SMB-fájlmegosztást, amely a File storage számára.</span><span class="sxs-lookup"><span data-stu-id="9695a-152">Anything written or read from the local mount directory is forwarded to the SMB share that's hosted on File storage.</span></span> <span data-ttu-id="9695a-153">Hozzon létre egy helyi könyvtárat, /mnt/mymountdirectory, használja az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="9695a-153">To create a local directory at /mnt/mymountdirectory, use the following example:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. <span data-ttu-id="9695a-154">Csatlakoztassa az SMB-megosztáshoz, a helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="9695a-154">Mount the SMB share to the local directory.</span></span>

    <span data-ttu-id="9695a-155">Az alábbiak szerint adja meg a saját tárolási fiók felhasználónevét és a tárfiók kulcsa a csatlakoztatási hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="9695a-155">Provide your own storage account username and storage account key for the mount credentials as follows:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. <span data-ttu-id="9695a-156">Az SMB-csatlakoztatási újraindítások keresztül megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="9695a-156">Persist the SMB mount through reboots.</span></span>

    <span data-ttu-id="9695a-157">A Linux virtuális gép újraindításakor a csatlakoztatott SMB-megosztáson fürtköteteiről leállítás során.</span><span class="sxs-lookup"><span data-stu-id="9695a-157">When you reboot the Linux VM, the mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="9695a-158">A Linux /etc/fstab vonal felvétele az SMB-megosztáshoz, a rendszerindító csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="9695a-158">To remount the SMB share on boot, add a line to the Linux /etc/fstab.</span></span> <span data-ttu-id="9695a-159">Linux fstab fájlt használja, amely a rendszerindítás során csatlakoztatni kell a fájlrendszerek listázásához.</span><span class="sxs-lookup"><span data-stu-id="9695a-159">Linux uses the fstab file to list the file systems that it needs to mount during the boot process.</span></span> <span data-ttu-id="9695a-160">Az SMB-megosztás hozzáadása biztosítja, hogy a File storage-megosztást véglegesen csatlakoztatott fájlrendszer a Linux virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="9695a-160">Adding the SMB share ensures that the File storage share is a permanently mounted file system for the Linux VM.</span></span> <span data-ttu-id="9695a-161">A fájltároló SMB-megosztási ad hozzá egy új virtuális gép felhőalapú inicializálás használata esetén lehetséges.</span><span class="sxs-lookup"><span data-stu-id="9695a-161">Adding the File storage SMB share to a new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="9695a-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9695a-162">Next steps</span></span>

- [<span data-ttu-id="9695a-163">Felhő inicializálás segítségével testre szabhatja a Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="9695a-163">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="9695a-164">Add a disk to a Linux VM (Lemez hozzáadása Linux rendszerű virtuális géphez)</span><span class="sxs-lookup"><span data-stu-id="9695a-164">Add a disk to a Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="9695a-165">Linux virtuális gép lemezeinek titkosítani az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="9695a-165">Encrypt disks on a Linux VM by using the Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
