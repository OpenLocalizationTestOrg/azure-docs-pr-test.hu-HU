---
title: "Azure File storage a Linux virtuális gépek az Azure CLI 1.0-s SMB használatával aaaMount |} Microsoft Docs"
description: "Hogyan toomount Azure File storage a Linux virtuális gépeken SMB használatával"
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
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 14a4224228cadb0ae2f05e8e5c8022ee84f138a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a><span data-ttu-id="74fcb-103">Csatlakoztatási Azure File storage a Linux virtuális gépek az Azure CLI 1.0-s SMB használatával</span><span class="sxs-lookup"><span data-stu-id="74fcb-103">Mount Azure File storage on Linux VMs by using SMB with Azure CLI 1.0</span></span>

<span data-ttu-id="74fcb-104">Ez a cikk bemutatja, hogyan toomount Azure File storage a Linux virtuális gép használatával hello Server Message Block (SMB) protokoll.</span><span class="sxs-lookup"><span data-stu-id="74fcb-104">This article shows how toomount Azure File storage on a Linux VM by using hello Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="74fcb-105">A File storage fájlmegosztások hello felhőben hello szabványos SMB protokollon keresztül biztosít.</span><span class="sxs-lookup"><span data-stu-id="74fcb-105">File storage offers file shares in hello cloud via hello standard SMB protocol.</span></span> <span data-ttu-id="74fcb-106">hello követelményei a következők:</span><span class="sxs-lookup"><span data-stu-id="74fcb-106">hello requirements are:</span></span>

* <span data-ttu-id="74fcb-107">Egy [Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="74fcb-107">An [Azure account](https://azure.microsoft.com/pricing/free-trial/)</span></span>
* [<span data-ttu-id="74fcb-108">Secure Shell (SSH) nyilvános és titkos kulcs fájlok</span><span class="sxs-lookup"><span data-stu-id="74fcb-108">Secure Shell (SSH) public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="cli-versions-toouse"></a><span data-ttu-id="74fcb-109">Parancssori felület verziók toouse</span><span class="sxs-lookup"><span data-stu-id="74fcb-109">CLI versions toouse</span></span>
<span data-ttu-id="74fcb-110">Hello feladat a következő parancssori felület (CLI) verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="74fcb-110">You can complete hello task by using one of hello following command-line interface (CLI) versions:</span></span>

- <span data-ttu-id="74fcb-111">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="74fcb-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="74fcb-112">[Az Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="74fcb-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)- our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="74fcb-113">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="74fcb-113">Quick commands</span></span>
<span data-ttu-id="74fcb-114">tooaccomplish hello feladat gyorsan, kövesse az ebben a szakaszban hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="74fcb-114">tooaccomplish hello task quickly, follow hello steps in this section.</span></span> <span data-ttu-id="74fcb-115">Részletes információkat és a környezetben, kezdődjenek hello ["Részletes forgatókönyv"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) szakasz.</span><span class="sxs-lookup"><span data-stu-id="74fcb-115">For more detailed information and context, begin at hello ["Detailed walkthrough"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) section.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="74fcb-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="74fcb-116">Prerequisites</span></span>
* <span data-ttu-id="74fcb-117">Egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="74fcb-117">A resource group</span></span>
* <span data-ttu-id="74fcb-118">Egy Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="74fcb-118">An Azure virtual network</span></span>
* <span data-ttu-id="74fcb-119">Az SSH a hálózati biztonsági csoportok bejövő</span><span class="sxs-lookup"><span data-stu-id="74fcb-119">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="74fcb-120">Egy alhálózaton</span><span class="sxs-lookup"><span data-stu-id="74fcb-120">A subnet</span></span>
* <span data-ttu-id="74fcb-121">Egy Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="74fcb-121">An Azure storage account</span></span>
* <span data-ttu-id="74fcb-122">Az Azure storage-fiók kulcsok</span><span class="sxs-lookup"><span data-stu-id="74fcb-122">Azure storage account keys</span></span>
* <span data-ttu-id="74fcb-123">Az Azure File storage-megosztás</span><span class="sxs-lookup"><span data-stu-id="74fcb-123">An Azure File storage share</span></span>
* <span data-ttu-id="74fcb-124">Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="74fcb-124">A Linux VM</span></span>

<span data-ttu-id="74fcb-125">Cserélje le olyan saját beállításaival.</span><span class="sxs-lookup"><span data-stu-id="74fcb-125">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-hello-local-mount"></a><span data-ttu-id="74fcb-126">Hozzon létre egy könyvtárat hello helyi csatlakoztatási</span><span class="sxs-lookup"><span data-stu-id="74fcb-126">Create a directory for hello local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a><span data-ttu-id="74fcb-127">Hello fájl tárolási SMB megosztás toohello csatlakoztatási pont csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="74fcb-127">Mount hello File storage SMB share toohello mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a><span data-ttu-id="74fcb-128">Újraindítás után is megmaradjanak hello csatlakoztatási</span><span class="sxs-lookup"><span data-stu-id="74fcb-128">Persist hello mount after a reboot</span></span>
<span data-ttu-id="74fcb-129">Adja hozzá a következő sor túl hello`/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="74fcb-129">Add hello following line too`/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="74fcb-130">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="74fcb-130">Detailed walkthrough</span></span>

<span data-ttu-id="74fcb-131">A File storage hello felhőben használó fájlmegosztásokról hello szabványos SMB-protokollt biztosít.</span><span class="sxs-lookup"><span data-stu-id="74fcb-131">File storage offers file shares in hello cloud that use hello standard SMB protocol.</span></span> <span data-ttu-id="74fcb-132">A File storage kiadása legújabb hello is minden os, amely támogatja az SMB 3.0 lehet csatlakoztatni fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="74fcb-132">With hello latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="74fcb-133">Az SMB-csatlakoztatási Linux rendszeren való használatakor kapott egyszerű biztonsági mentést tooa robusztus, állandó archiválási tárolóhely szolgáltatásiszint-szerződésben garantált által támogatott.</span><span class="sxs-lookup"><span data-stu-id="74fcb-133">When you use an SMB mount on Linux, you get easy backups tooa robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="74fcb-134">Helyezze át fájlokat a virtuális gép tooan SMB csatlakoztatási tárolt fájlok tárolására egy remek mód toodebug naplózza.</span><span class="sxs-lookup"><span data-stu-id="74fcb-134">Moving files from a VM tooan SMB mount that's hosted on File storage is a great way toodebug logs.</span></span> <span data-ttu-id="74fcb-135">Ennek oka azonos SMB-megosztási hello helyileg lehet csatlakoztatni tooyour Mac, Linux vagy a Windows munkaállomás.</span><span class="sxs-lookup"><span data-stu-id="74fcb-135">That's because hello same SMB share can be mounted locally tooyour Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="74fcb-136">Az SMB nem hello legjobb megoldás az adatfolyamként történő Linux vagy alkalmazásnaplók valós időben, mert hello SMB protokoll nem épített toohandle ilyen nehéz naplózási feladatokat.</span><span class="sxs-lookup"><span data-stu-id="74fcb-136">SMB isn't hello best solution for streaming Linux or application logs in real time, because hello SMB protocol is not built toohandle such heavy logging duties.</span></span> <span data-ttu-id="74fcb-137">Dedikált, egységes naplózási réteg eszközhöz hasonló eszközökkel Fluentd jobb megoldás, mint az SMB lenne a Linux és a naplózás a kimeneti alkalmazás gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="74fcb-137">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="74fcb-138">A részletes forgatókönyv létrehozhatunk hello Előfeltételek szükség toofirst hello tárolási fájlmegosztás létrehozása, és csatlakoztathatja SMB-protokollal a Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="74fcb-138">For this detailed walkthrough, we create hello prerequisites needed toofirst create hello File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="74fcb-139">Egy Azure storage-fiók létrehozása a következő kód hello használatával:</span><span class="sxs-lookup"><span data-stu-id="74fcb-139">Create an Azure storage account by using hello following code:</span></span>

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. <span data-ttu-id="74fcb-140">Hello tárfiókkulcsok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="74fcb-140">Show hello storage account keys.</span></span>

    <span data-ttu-id="74fcb-141">Amikor létrehoz egy tárfiókot, hello kulcsait jönnek létre párok, hogy a szolgáltatás megszakítás nélkül forgatható el.</span><span class="sxs-lookup"><span data-stu-id="74fcb-141">When you create a storage account, hello account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="74fcb-142">Amikor hello párokban szereplő toohello második kulcsot, hozzon létre egy új kulcspárt.</span><span class="sxs-lookup"><span data-stu-id="74fcb-142">When you switch toohello second key in hello pair, you create a new key pair.</span></span> <span data-ttu-id="74fcb-143">Új tárfiókkulcsok mindig párosával győződjön meg arról, hogy mindig legyen legalább egy nem használt tárolási kulcs készen tooswitch való jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="74fcb-143">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage key ready tooswitch to.</span></span> <span data-ttu-id="74fcb-144">tooshow hello tárfiókok kulcsait, a következő kód hello használata:</span><span class="sxs-lookup"><span data-stu-id="74fcb-144">tooshow hello storage account keys, use hello following code:</span></span>

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. <span data-ttu-id="74fcb-145">Hello File storage-megosztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="74fcb-145">Create hello File storage share.</span></span>

    <span data-ttu-id="74fcb-146">hello File storage-megosztás hello SMB-megosztás tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="74fcb-146">hello File storage share contains hello SMB share.</span></span> <span data-ttu-id="74fcb-147">hello kvóta mindig gigabájt (GB) van megadva.</span><span class="sxs-lookup"><span data-stu-id="74fcb-147">hello quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="74fcb-148">toocreate hello File storage-megosztást, a következő kód hello használata:</span><span class="sxs-lookup"><span data-stu-id="74fcb-148">toocreate hello File storage share, use hello following code:</span></span>

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. <span data-ttu-id="74fcb-149">Hozzon létre hello csatlakoztatásipont-könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="74fcb-149">Create hello mount-point directory.</span></span>

    <span data-ttu-id="74fcb-150">Hozzon létre egy helyi könyvtárat hello Linux fájl rendszer toomount hello SMB-megosztás.</span><span class="sxs-lookup"><span data-stu-id="74fcb-150">You must create a local directory in hello Linux file system toomount hello SMB share to.</span></span> <span data-ttu-id="74fcb-151">SMB-megosztási toohello, amely a File storage semmit írt vagy hello helyi csatlakoztatási könyvtárból olvassa továbbítja.</span><span class="sxs-lookup"><span data-stu-id="74fcb-151">Anything written or read from hello local mount directory is forwarded toohello SMB share that's hosted on File storage.</span></span> <span data-ttu-id="74fcb-152">toocreate hello directory, a következő kód használható hello:</span><span class="sxs-lookup"><span data-stu-id="74fcb-152">toocreate hello directory, use hello following code:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. <span data-ttu-id="74fcb-153">Csatlakoztatás SMB-megosztás a következő kód hello segítségével hello:</span><span class="sxs-lookup"><span data-stu-id="74fcb-153">Mount hello SMB share by using hello following code:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. <span data-ttu-id="74fcb-154">SMB csatlakoztatási keresztül újraindítások hello megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="74fcb-154">Persist hello SMB mount through reboots.</span></span>

    <span data-ttu-id="74fcb-155">Linux virtuális gép hello újraindításakor hello csatlakoztatott SMB-megosztáson fürtköteteiről leállítás során.</span><span class="sxs-lookup"><span data-stu-id="74fcb-155">When you reboot hello Linux VM, hello mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="74fcb-156">tooremount hello SMB-megosztás a rendszerindítás, hozzá kell adni egy sor toohello Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="74fcb-156">tooremount hello SMB share on boot, you must add a line toohello Linux /etc/fstab.</span></span> <span data-ttu-id="74fcb-157">Linux hello fstab fájl toolist hello fájlrendszerek toomount jogcímadatokat hello rendszerindítási folyamat során használ.</span><span class="sxs-lookup"><span data-stu-id="74fcb-157">Linux uses hello fstab file toolist hello file systems that it needs toomount during hello boot process.</span></span> <span data-ttu-id="74fcb-158">Hozzáadását hello SMB-megosztás biztosítja, hogy hello File storage-megosztás véglegesen csatlakoztatott fájlrendszer a hello Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="74fcb-158">Adding hello SMB share ensures that hello File storage share is a permanently mounted file system for hello Linux VM.</span></span> <span data-ttu-id="74fcb-159">Hello fájl tárolási SMB megosztás tooa hozzáadása új virtuális gép esetén lehetséges felhő inicializálás.</span><span class="sxs-lookup"><span data-stu-id="74fcb-159">Adding hello File storage SMB share tooa new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="74fcb-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="74fcb-160">Next steps</span></span>

- [<span data-ttu-id="74fcb-161">Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="74fcb-161">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="74fcb-162">A lemez tooa Linux virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="74fcb-162">Add a disk tooa Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="74fcb-163">Linux virtuális gép lemezeinek titkosítása hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="74fcb-163">Encrypt disks on a Linux VM by using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
