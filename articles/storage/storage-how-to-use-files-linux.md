---
title: "Azure File storage használata Linux |} Microsoft Docs"
description: "Ismerje meg, egy Azure fájlmegosztás csatlakoztatásáról SMB-n keresztül Linux rendszeren."
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
ms.openlocfilehash: 27b393a899c60a3a0393619f338a396dff659498
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-file-storage-with-linux"></a><span data-ttu-id="10ee3-103">Azure File storage használata Linux</span><span class="sxs-lookup"><span data-stu-id="10ee3-103">Use Azure File storage with Linux</span></span>
<span data-ttu-id="10ee3-104">Az [Azure-fájlmegosztás](storage-dotnet-how-to-use-files.md) a Microsoft könnyen használható felhőalapú fájlrendszere.</span><span class="sxs-lookup"><span data-stu-id="10ee3-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy to use cloud file system.</span></span> <span data-ttu-id="10ee3-105">Az Azure fájlmegosztások a Linux terjesztéseket segítségével lehet csatlakoztatni az [cifs-utils csomag](https://wiki.samba.org/index.php/LinuxCIFS_utils) a a [Samba projekt](https://www.samba.org/).</span><span class="sxs-lookup"><span data-stu-id="10ee3-105">Azure File shares can be mounted in Linux distributions using the [cifs-utils package](https://wiki.samba.org/index.php/LinuxCIFS_utils) from the [Samba project](https://www.samba.org/).</span></span> <span data-ttu-id="10ee3-106">Ez a cikk bemutatja egy Azure fájlmegosztás csatlakoztatásához kétféleképpen: az igény a `mount` parancs és a rendszerindítási bejegyzés létrehozásával `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="10ee3-106">This article shows two ways to mount an Azure File share: on-demand with the `mount` command and on-boot by creating an entry in `/etc/fstab`.</span></span>

> [!NOTE]  
> <span data-ttu-id="10ee3-107">Ahhoz, hogy csatlakoztassa egy Azure fájlmegosztás az Azure-on kívüli helyezkedik el, például a helyszínen vagy egy másik Azure régióban, az operációs rendszer régió támogatnia kell a titkosítás funkciót, az SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="10ee3-107">In order to mount an Azure File share outside of the Azure region it is hosted in, such as on-premises or in a different Azure region, the OS must support the encryption functionality of SMB 3.0.</span></span> <span data-ttu-id="10ee3-108">Az SMB 3.0 Linux titkosítási szolgáltatás 4.11 kernel jelent meg.</span><span class="sxs-lookup"><span data-stu-id="10ee3-108">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="10ee3-109">Ez a funkció lehetővé teszi, hogy az Azure fájlmegosztás regisztrációját a helyi vagy egy másik Azure-régiót csatlakoztatását.</span><span class="sxs-lookup"><span data-stu-id="10ee3-109">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="10ee3-110">Közzététel alkalmával Ez a funkció lett backported az Ubuntu 16.04 és fent.</span><span class="sxs-lookup"><span data-stu-id="10ee3-110">At the time of publishing, this functionality has been backported to Ubuntu from 16.04 and above.</span></span>


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a><span data-ttu-id="10ee3-111">Az Azure File csatlakoztatására Prerequisities megosztása a Linux és a cifs-utils csomag</span><span class="sxs-lookup"><span data-stu-id="10ee3-111">Prerequisities for mounting an Azure File share with Linux and the cifs-utils package</span></span>
* <span data-ttu-id="10ee3-112">**Válassza ki, amelyeken a cifs-utils csomag telepítése Linux-disztribúció**: a Microsoft azt javasolja, hogy a kép: Azure-katalógus a következő Linux terjesztéseket:</span><span class="sxs-lookup"><span data-stu-id="10ee3-112">**Pick a Linux distribution that can have the cifs-utils package installed**: Microsoft recommends the following Linux distributions in the Azure image gallery:</span></span>

    * <span data-ttu-id="10ee3-113">Ubuntu Server 14.04 +</span><span class="sxs-lookup"><span data-stu-id="10ee3-113">Ubuntu Server 14.04+</span></span>
    * <span data-ttu-id="10ee3-114">RHEL 7 +</span><span class="sxs-lookup"><span data-stu-id="10ee3-114">RHEL 7+</span></span>
    * <span data-ttu-id="10ee3-115">CentOS 7 +</span><span class="sxs-lookup"><span data-stu-id="10ee3-115">CentOS 7+</span></span>
    * <span data-ttu-id="10ee3-116">Debian 8</span><span class="sxs-lookup"><span data-stu-id="10ee3-116">Debian 8</span></span>
    * <span data-ttu-id="10ee3-117">openSUSE 13.2 +</span><span class="sxs-lookup"><span data-stu-id="10ee3-117">openSUSE 13.2+</span></span>
    * <span data-ttu-id="10ee3-118">SUSE Linux Enterprise Server 12</span><span class="sxs-lookup"><span data-stu-id="10ee3-118">SUSE Linux Enterprise Server 12</span></span>

* <span data-ttu-id="10ee3-119"><a id="install-cifs-utils"></a>**A cifs-utils csomag telepítve**: A cifs-utils a package manager segítségével az Ön által választott Linux terjesztési kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="10ee3-119"><a id="install-cifs-utils"></a>**The cifs-utils package is installed**: The cifs-utils can be installed using the package manager on the Linux distribution of your choice.</span></span> 

    <span data-ttu-id="10ee3-120">A **Ubuntu** és **Debian-alapú** azokat a terjesztéseket, használja a `apt-get` Csomagkezelő:</span><span class="sxs-lookup"><span data-stu-id="10ee3-120">On **Ubuntu** and **Debian-based** distributions, use the `apt-get` package manager:</span></span>

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    <span data-ttu-id="10ee3-121">A **RHEL** és **CentOS**, használja a `yum` Csomagkezelő:</span><span class="sxs-lookup"><span data-stu-id="10ee3-121">On **RHEL** and **CentOS**, use the `yum` package manager:</span></span>

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    <span data-ttu-id="10ee3-122">A **openSUSE**, használja a `zypper` Csomagkezelő:</span><span class="sxs-lookup"><span data-stu-id="10ee3-122">On **openSUSE**, use the `zypper` package manager:</span></span>

    ```
    sudo zypper install samba*
    ```

    <span data-ttu-id="10ee3-123">A más azokat a terjesztéseket, használja a megfelelő Csomagkezelő vagy [fordítási forrás](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span><span class="sxs-lookup"><span data-stu-id="10ee3-123">On other distributions, use the appropriate package manager or [compile from source](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span></span>

* <span data-ttu-id="10ee3-124">**Adja meg a csatlakoztatott megosztást a könyvtár-/ fájlengedélyek**: a következő példa 0777 használjuk, adjon olvasási, írási és végrehajtási engedélyek minden felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="10ee3-124">**Decide on the directory/file permissions of the mounted share**: In the examples below, we use 0777, to give read, write, and execute permissions to all users.</span></span> <span data-ttu-id="10ee3-125">Más lecserélheti [chmod engedélyek](https://en.wikipedia.org/wiki/Chmod) igény szerint.</span><span class="sxs-lookup"><span data-stu-id="10ee3-125">You can replace it with other [chmod permissions](https://en.wikipedia.org/wiki/Chmod) as desired.</span></span> 

* <span data-ttu-id="10ee3-126">**Tárfiók neve**: Az Azure-fájlmegosztások csatlakoztatásához szüksége lesz a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="10ee3-126">**Storage Account Name**: To mount an Azure File share, you will need the name of the storage account.</span></span>

* <span data-ttu-id="10ee3-127">**Tárfiók kulcsa**: Az Azure-fájlmegosztások csatlakoztatásához szüksége lesz az elsődleges (vagy másodlagos) tárkulcsra.</span><span class="sxs-lookup"><span data-stu-id="10ee3-127">**Storage Account Key**: To mount an Azure File share, you will need the primary (or secondary) storage key.</span></span> <span data-ttu-id="10ee3-128">Az SAS-kulcsokkal való csatlakoztatás jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="10ee3-128">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="10ee3-129">**Győződjön meg arról, 445-ös port meg nyitva**: SMB - 445-ös TCP-porton keresztül kommunikál ellenőrizze megjelenítéséhez, ha a tűzfal nem blokkolja-e a TCP portokat a 445-ös, az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="10ee3-129">**Ensure port 445 is open**: SMB communicates over TCP port 445 - check to see if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a><span data-ttu-id="10ee3-130">Az Azure File megosztást az igény a csatlakoztatása`mount`</span><span class="sxs-lookup"><span data-stu-id="10ee3-130">Mount the Azure File share on-demand with `mount`</span></span>
1. <span data-ttu-id="10ee3-131">**[A cifs-utils telepítéséhez a Linux-disztribúció](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="10ee3-131">**[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="10ee3-132">**Hozzon létre egy mappát a csatlakoztatási pont**: ehhez bárhol a fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="10ee3-132">**Create a folder for the mount point**: This can be done anywhere on the file system.</span></span>

    ```
    mkdir mymountpoint
    ```

3. <span data-ttu-id="10ee3-133">**A csatlakoztatási parancs használata az Azure fájlmegosztás csatlakoztatásához**: ne felejtse el lecserélni `<storage-account-name>`, `<share-name>`, és `<storage-account-key>` a megfelelő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="10ee3-133">**Use the mount command to mount the Azure File share**: Remember to replace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with the proper information.</span></span>

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> <span data-ttu-id="10ee3-134">Ha végzett az Azure-fájlmegosztást használja, használhatja a `sudo umount ./mymountpoint` leválasztása a megosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="10ee3-134">When you are done using the Azure File share, you may use `sudo umount ./mymountpoint` to unmount the share.</span></span>

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a><span data-ttu-id="10ee3-135">Az Azure fájlmegosztás állandó csatlakoztatási pont létrehozása`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="10ee3-135">Create a persistent mount point for the Azure File share with `/etc/fstab`</span></span>
1. <span data-ttu-id="10ee3-136">**[A cifs-utils telepítéséhez a Linux-disztribúció](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="10ee3-136">**[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="10ee3-137">**Hozzon létre egy mappát a csatlakoztatási pont**: ehhez bárhol a fájlrendszerben, de vegye figyelembe a abszolút mappa elérési útját kell.</span><span class="sxs-lookup"><span data-stu-id="10ee3-137">**Create a folder for the mount point**: This can be done anywhere on the file system, but you need to note the absolute path of the folder.</span></span> <span data-ttu-id="10ee3-138">Az alábbi példa létrehoz egy legfelső szintű mappát.</span><span class="sxs-lookup"><span data-stu-id="10ee3-138">The following example creates a folder under root.</span></span>

    ```
    sudo mkdir /mymountpoint
    ```

3. <span data-ttu-id="10ee3-139">**Az alábbi parancs segítségével az alábbi sor hozzáfűzése `/etc/fstab`** : ne felejtse el lecserélni `<storage-account-name>`, `<share-name>`, és `<storage-account-key>` a megfelelő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="10ee3-139">**Use the following command to append the following line to `/etc/fstab`**: Remember to replace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with the proper information.</span></span>

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> <span data-ttu-id="10ee3-140">Használhat `sudo mount -a` módosítása után az Azure fájlmegosztás csatlakoztatásához `/etc/fstab` újraindítása helyett.</span><span class="sxs-lookup"><span data-stu-id="10ee3-140">You can use `sudo mount -a` to mount the Azure File share after editing `/etc/fstab` instead of rebooting.</span></span>

## <a name="feedback"></a><span data-ttu-id="10ee3-141">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="10ee3-141">Feedback</span></span>
<span data-ttu-id="10ee3-142">Linux-felhasználók szeretnénk a véleményét!</span><span class="sxs-lookup"><span data-stu-id="10ee3-142">Linux users, we want to hear from you!</span></span>

<span data-ttu-id="10ee3-143">Linux-felhasználók csoport a Azure File storage közös visszajelzést, értékelje ki és elfogadják a File storage Linux rendszeren fórum biztosít.</span><span class="sxs-lookup"><span data-stu-id="10ee3-143">The Azure File storage for Linux users' group provides a forum for you to share feedback as you evaluate and adopt File storage on Linux.</span></span> <span data-ttu-id="10ee3-144">E-mailek [Azure File storage Linux-felhasználók](mailto:azurefileslinuxusers@microsoft.com) csatlakozni a felhasználói csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="10ee3-144">Email [Azure File storage Linux Users](mailto:azurefileslinuxusers@microsoft.com) to join the users' group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10ee3-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10ee3-145">Next steps</span></span>
<span data-ttu-id="10ee3-146">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="10ee3-146">See these links for more information about Azure File storage.</span></span>
* [<span data-ttu-id="10ee3-147">Referencia a fájlszolgáltatás REST API-jához</span><span class="sxs-lookup"><span data-stu-id="10ee3-147">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [<span data-ttu-id="10ee3-148">Az Azure storage Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="10ee3-148">Using Azure PowerShell with Azure storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="10ee3-149">AzCopy használata a Microsoft Azure storage</span><span class="sxs-lookup"><span data-stu-id="10ee3-149">How to use AzCopy with Microsoft Azure storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="10ee3-150">Az Azure storage az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="10ee3-150">Using the Azure CLI with Azure storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="10ee3-151">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="10ee3-151">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="10ee3-152">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="10ee3-152">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)
