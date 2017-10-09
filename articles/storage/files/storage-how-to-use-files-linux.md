---
title: "a Linux és Azure File storage aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toomount egy Azure-fájl megosztása Linux SMB protokollon keresztül."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/8/2017
ms.author: renash
ms.openlocfilehash: eeaa24b7f9e646724c5d86ae1e80dfdadaff34fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-storage-with-linux"></a><span data-ttu-id="68d10-103">Azure File storage használata Linux</span><span class="sxs-lookup"><span data-stu-id="68d10-103">Use Azure File storage with Linux</span></span>
<span data-ttu-id="68d10-104">[Az Azure File storage](../storage-dotnet-how-to-use-files.md) a Microsoft easy toouse felhő fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="68d10-104">[Azure File storage](../storage-dotnet-how-to-use-files.md) is Microsoft's easy toouse cloud file system.</span></span> <span data-ttu-id="68d10-105">A Linux terjesztéseket hello segítségével lehet csatlakoztatni az Azure fájlmegosztások [cifs-utils csomag](https://wiki.samba.org/index.php/LinuxCIFS_utils) a hello [Samba projekt](https://www.samba.org/).</span><span class="sxs-lookup"><span data-stu-id="68d10-105">Azure File shares can be mounted in Linux distributions using hello [cifs-utils package](https://wiki.samba.org/index.php/LinuxCIFS_utils) from hello [Samba project](https://www.samba.org/).</span></span> <span data-ttu-id="68d10-106">Ez a cikk bemutatja két módon toomount egy Azure fájlmegosztás: az igény a hello `mount` parancs és a rendszerindítási bejegyzés létrehozásával `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="68d10-106">This article shows two ways toomount an Azure File share: on-demand with hello `mount` command and on-boot by creating an entry in `/etc/fstab`.</span></span>

> [!NOTE]  
> <span data-ttu-id="68d10-107">A sorrend toomount egy Azure fájlmegosztás hello kívül helyezkedik el, például a helyszínen vagy egy másik Azure régióban, az operációs rendszer hello Azure-régiót támogatnia kell hello titkosítás funkciót az SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="68d10-107">In order toomount an Azure File share outside of hello Azure region it is hosted in, such as on-premises or in a different Azure region, hello OS must support hello encryption functionality of SMB 3.0.</span></span> <span data-ttu-id="68d10-108">Az SMB 3.0 Linux titkosítási szolgáltatás 4.11 kernel jelent meg.</span><span class="sxs-lookup"><span data-stu-id="68d10-108">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="68d10-109">Ez a funkció lehetővé teszi, hogy az Azure fájlmegosztás regisztrációját a helyi vagy egy másik Azure-régiót csatlakoztatását.</span><span class="sxs-lookup"><span data-stu-id="68d10-109">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="68d10-110">A közzététel hello időpontjában ez a funkció backported tooUbuntu 16.04 és újabb verziók le lett.</span><span class="sxs-lookup"><span data-stu-id="68d10-110">At hello time of publishing, this functionality has been backported tooUbuntu from 16.04 and above.</span></span>


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-hello-cifs-utils-package"></a><span data-ttu-id="68d10-111">Egy Azure fájlmegosztás Linux és hello cifs-utils csomaggal csatlakoztatására Prerequisities</span><span class="sxs-lookup"><span data-stu-id="68d10-111">Prerequisities for mounting an Azure File share with Linux and hello cifs-utils package</span></span>
* <span data-ttu-id="68d10-112">**Válasszon egy Linux-disztribúció, amelyeken telepítve hello cifs-utils csomag**: a Microsoft azt javasolja, hogy a Linux terjesztéseket hello kép: Azure-katalógus következő hello:</span><span class="sxs-lookup"><span data-stu-id="68d10-112">**Pick a Linux distribution that can have hello cifs-utils package installed**: Microsoft recommends hello following Linux distributions in hello Azure image gallery:</span></span>

    * <span data-ttu-id="68d10-113">Ubuntu Server 14.04 +</span><span class="sxs-lookup"><span data-stu-id="68d10-113">Ubuntu Server 14.04+</span></span>
    * <span data-ttu-id="68d10-114">RHEL 7 +</span><span class="sxs-lookup"><span data-stu-id="68d10-114">RHEL 7+</span></span>
    * <span data-ttu-id="68d10-115">CentOS 7 +</span><span class="sxs-lookup"><span data-stu-id="68d10-115">CentOS 7+</span></span>
    * <span data-ttu-id="68d10-116">Debian 8</span><span class="sxs-lookup"><span data-stu-id="68d10-116">Debian 8</span></span>
    * <span data-ttu-id="68d10-117">openSUSE 13.2 +</span><span class="sxs-lookup"><span data-stu-id="68d10-117">openSUSE 13.2+</span></span>
    * <span data-ttu-id="68d10-118">SUSE Linux Enterprise Server 12</span><span class="sxs-lookup"><span data-stu-id="68d10-118">SUSE Linux Enterprise Server 12</span></span>

* <span data-ttu-id="68d10-119"><a id="install-cifs-utils"></a>**hello cifs-utils telepítve van-e**: hello cifs-utils hello a package manager segítségével az Ön által választott hello Linux terjesztési kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="68d10-119"><a id="install-cifs-utils"></a>**hello cifs-utils package is installed**: hello cifs-utils can be installed using hello package manager on hello Linux distribution of your choice.</span></span> 

    <span data-ttu-id="68d10-120">A **Ubuntu** és **Debian-alapú** azokat a terjesztéseket, használja a hello `apt-get` Csomagkezelő:</span><span class="sxs-lookup"><span data-stu-id="68d10-120">On **Ubuntu** and **Debian-based** distributions, use hello `apt-get` package manager:</span></span>

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    <span data-ttu-id="68d10-121">A **RHEL** és **CentOS**, használja a hello `yum` Csomagkezelő:</span><span class="sxs-lookup"><span data-stu-id="68d10-121">On **RHEL** and **CentOS**, use hello `yum` package manager:</span></span>

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    <span data-ttu-id="68d10-122">A **openSUSE**, használja a hello `zypper` Csomagkezelő:</span><span class="sxs-lookup"><span data-stu-id="68d10-122">On **openSUSE**, use hello `zypper` package manager:</span></span>

    ```
    sudo zypper install samba*
    ```

    <span data-ttu-id="68d10-123">A más azokat a terjesztéseket, használja a megfelelő Csomagkezelő hello vagy [fordítási forrás](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span><span class="sxs-lookup"><span data-stu-id="68d10-123">On other distributions, use hello appropriate package manager or [compile from source](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span></span>

* <span data-ttu-id="68d10-124">**Adja meg a könyvtár vagy fájl engedélyeit hello hello csatlakoztatott megosztást**: hello a következő példa használjuk 0777, toogive olvasási, írási, és végrehajtási engedélyek tooall felhasználók.</span><span class="sxs-lookup"><span data-stu-id="68d10-124">**Decide on hello directory/file permissions of hello mounted share**: In hello examples below, we use 0777, toogive read, write, and execute permissions tooall users.</span></span> <span data-ttu-id="68d10-125">Más lecserélheti [chmod engedélyek](https://en.wikipedia.org/wiki/Chmod) igény szerint.</span><span class="sxs-lookup"><span data-stu-id="68d10-125">You can replace it with other [chmod permissions](https://en.wikipedia.org/wiki/Chmod) as desired.</span></span> 

* <span data-ttu-id="68d10-126">**A tárfiók neve**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello hello tárfiókja nevére.</span><span class="sxs-lookup"><span data-stu-id="68d10-126">**Storage Account Name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="68d10-127">**Tárfiók kulcsa**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello elsődleges (vagy másodlagos) storage-kulcs.</span><span class="sxs-lookup"><span data-stu-id="68d10-127">**Storage Account Key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="68d10-128">Az SAS-kulcsokkal való csatlakoztatás jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="68d10-128">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="68d10-129">**Győződjön meg arról, 445-ös port meg nyitva**: SMB - 445-ös TCP-porton keresztül kommunikál ellenőrizze toosee, ha a tűzfal nem blokkolja-e a TCP portokat a 445-ös, az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="68d10-129">**Ensure port 445 is open**: SMB communicates over TCP port 445 - check toosee if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-hello-azure-file-share-on-demand-with-mount"></a><span data-ttu-id="68d10-130">Azure fájlmegosztás az igény a hello csatlakoztatása`mount`</span><span class="sxs-lookup"><span data-stu-id="68d10-130">Mount hello Azure File share on-demand with `mount`</span></span>
1. <span data-ttu-id="68d10-131">**[A Linux terjesztéshez hello cifs-utils telepítéséhez](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="68d10-131">**[Install hello cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="68d10-132">**Hozzon létre egy mappát hello csatlakoztatási pont**: ehhez bárhol hello fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="68d10-132">**Create a folder for hello mount point**: This can be done anywhere on hello file system.</span></span>

    ```
    mkdir mymountpoint
    ```

3. <span data-ttu-id="68d10-133">**Használjon hello csatlakoztatási parancs toomount hello Azure fájlmegosztás**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, és `<storage-account-key>` hello megfelelő információkkal.</span><span class="sxs-lookup"><span data-stu-id="68d10-133">**Use hello mount command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with hello proper information.</span></span>

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> <span data-ttu-id="68d10-134">Ha végzett használatával hello Azure fájlmegosztás, használhatja a `sudo umount ./mymountpoint` toounmount hello megosztást.</span><span class="sxs-lookup"><span data-stu-id="68d10-134">When you are done using hello Azure File share, you may use `sudo umount ./mymountpoint` toounmount hello share.</span></span>

## <a name="create-a-persistent-mount-point-for-hello-azure-file-share-with-etcfstab"></a><span data-ttu-id="68d10-135">Egy állandó csatlakoztatási pontot hello Azure fájlmegosztás létrehozása`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="68d10-135">Create a persistent mount point for hello Azure File share with `/etc/fstab`</span></span>
1. <span data-ttu-id="68d10-136">**[A Linux terjesztéshez hello cifs-utils telepítéséhez](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="68d10-136">**[Install hello cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="68d10-137">**Hozzon létre egy mappát hello csatlakoztatási pont**: ehhez bárhol hello fájlrendszerben, de toonote hello abszolút mappa elérési útja hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="68d10-137">**Create a folder for hello mount point**: This can be done anywhere on hello file system, but you need toonote hello absolute path of hello folder.</span></span> <span data-ttu-id="68d10-138">a következő példa hello egy legfelső szintű mappát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="68d10-138">hello following example creates a folder under root.</span></span>

    ```
    sudo mkdir /mymountpoint
    ```

3. <span data-ttu-id="68d10-139">**Használjon hello következő parancsot a sor túl a következő tooappend hello`/etc/fstab`**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, és `<storage-account-key>` hello megfelelő információkkal.</span><span class="sxs-lookup"><span data-stu-id="68d10-139">**Use hello following command tooappend hello following line too`/etc/fstab`**: Remember tooreplace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with hello proper information.</span></span>

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> <span data-ttu-id="68d10-140">Használhat `sudo mount -a` toomount hello Azure fájlmegosztás módosítása után `/etc/fstab` újraindítása helyett.</span><span class="sxs-lookup"><span data-stu-id="68d10-140">You can use `sudo mount -a` toomount hello Azure File share after editing `/etc/fstab` instead of rebooting.</span></span>

## <a name="feedback"></a><span data-ttu-id="68d10-141">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="68d10-141">Feedback</span></span>
<span data-ttu-id="68d10-142">Linux-felhasználók, azt szeretnénk, ha az Ön toohear!</span><span class="sxs-lookup"><span data-stu-id="68d10-142">Linux users, we want toohear from you!</span></span>

<span data-ttu-id="68d10-143">hello Azure File storage Linux-felhasználók csoport visszajelzést biztosítanak a fórumot az Ön tooshare értékelje ki és elfogadják a File storage Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="68d10-143">hello Azure File storage for Linux users' group provides a forum for you tooshare feedback as you evaluate and adopt File storage on Linux.</span></span> <span data-ttu-id="68d10-144">E-mailek [Azure File storage Linux-felhasználók](mailto:azurefileslinuxusers@microsoft.com) toojoin hello felhasználói csoportnak.</span><span class="sxs-lookup"><span data-stu-id="68d10-144">Email [Azure File storage Linux Users](mailto:azurefileslinuxusers@microsoft.com) toojoin hello users' group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68d10-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="68d10-145">Next steps</span></span>
<span data-ttu-id="68d10-146">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="68d10-146">See these links for more information about Azure File storage.</span></span>
* [<span data-ttu-id="68d10-147">Referencia a fájlszolgáltatás REST API-jához</span><span class="sxs-lookup"><span data-stu-id="68d10-147">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [<span data-ttu-id="68d10-148">Hogyan toouse AzCopy Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="68d10-148">How toouse AzCopy with Microsoft Azure storage</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [<span data-ttu-id="68d10-149">Az Azure storage hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="68d10-149">Using hello Azure CLI with Azure storage</span></span>](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [<span data-ttu-id="68d10-150">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="68d10-150">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="68d10-151">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="68d10-151">Troubleshooting</span></span>](storage-troubleshoot-linux-file-connection-problems.md)
