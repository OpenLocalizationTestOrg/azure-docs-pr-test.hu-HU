---
title: "Bevezetés az Azure freebsd rendszerű |} Microsoft Docs"
description: "További információ az Azure-on freebsd rendszerű virtuális gépek használatáról"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: d0fc5de34f7d9e5a607495eb97d9e35dc9eb21f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-freebsd-on-azure"></a><span data-ttu-id="034f0-103">Az Azure-on freebsd rendszerű bemutatása</span><span class="sxs-lookup"><span data-stu-id="034f0-103">Introduction to FreeBSD on Azure</span></span>
<span data-ttu-id="034f0-104">Ez a témakör az Azure-ban freebsd rendszerű virtuális gépet futtató áttekintését.</span><span class="sxs-lookup"><span data-stu-id="034f0-104">This topic provides an overview of running a FreeBSD virtual machine in Azure.</span></span>

## <a name="overview"></a><span data-ttu-id="034f0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="034f0-105">Overview</span></span>
<span data-ttu-id="034f0-106">Freebsd rendszerű, a Microsoft Azure egy speciális számítógép operációs rendszerének power modern kiszolgálók, asztali számítógépek, használatával, és a beágyazott platformokon.</span><span class="sxs-lookup"><span data-stu-id="034f0-106">FreeBSD for Microsoft Azure is an advanced computer operating system used to power modern servers, desktops, and embedded platforms.</span></span>

<span data-ttu-id="034f0-107">A Microsoft Corporation van elérhetővé képeket freebsd rendszerű az Azure szolgáltatásban az a [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) előre konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="034f0-107">Microsoft Corporation is making images of FreeBSD available on Azure with the [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) pre-configured.</span></span> <span data-ttu-id="034f0-108">Jelenleg a következő freebsd rendszerű verziók képként által felajánlott Microsoft:</span><span class="sxs-lookup"><span data-stu-id="034f0-108">Currently, the following FreeBSD versions are offered as images by Microsoft:</span></span>

- <span data-ttu-id="034f0-109">Freebsd rendszerű 10.3-kiadás</span><span class="sxs-lookup"><span data-stu-id="034f0-109">FreeBSD 10.3-RELEASE</span></span>
- <span data-ttu-id="034f0-110">Freebsd rendszerű 11.0-kiadás</span><span class="sxs-lookup"><span data-stu-id="034f0-110">FreeBSD 11.0-RELEASE</span></span>

<span data-ttu-id="034f0-111">Az ügynök nem felelős a freebsd rendszerű virtuális gép és a műveletek, például az első használatkor (felhasználónév, jelszó vagy SSH-kulcsot, állomásnév, stb.) a virtuális gép kiépítése, és szelektív Virtuálisgép-bővítmények a funkció engedélyezése az Azure-hálót közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="034f0-111">The agent is responsible for communication between the FreeBSD VM and the Azure fabric for operations such as provisioning the VM on first use (user name, password or SSH key, host name, etc.) and enabling functionality for selective VM extensions.</span></span>

<span data-ttu-id="034f0-112">Jövőbeli verzióiban freebsd rendszerű, mint a stratégia mindig naprakész, és lehetővé teszi a legfrissebb verziókban elérhető azok közzététele a freebsd rendszerű kiadás mérnöki csapat követően rövid időn belül.</span><span class="sxs-lookup"><span data-stu-id="034f0-112">As for future versions of FreeBSD, the strategy is to stay current and make the latest releases available shortly after they are published by the FreeBSD release engineering team.</span></span>

## <a name="deploying-a-freebsd-virtual-machine"></a><span data-ttu-id="034f0-113">Freebsd rendszerű virtuális gép telepítése</span><span class="sxs-lookup"><span data-stu-id="034f0-113">Deploying a FreeBSD virtual machine</span></span>
<span data-ttu-id="034f0-114">Egy egyszerű folyamatot a lemezkép használatával az Azure-portálon az Azure piactérről freebsd rendszerű virtuális gép telepítése:</span><span class="sxs-lookup"><span data-stu-id="034f0-114">Deploying a FreeBSD virtual machine is a straightforward process using an image from the Azure Marketplace from the Azure portal:</span></span>

- [<span data-ttu-id="034f0-115">Az Azure piactéren 10.3 freebsd rendszerű</span><span class="sxs-lookup"><span data-stu-id="034f0-115">FreeBSD 10.3 on the Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [<span data-ttu-id="034f0-116">Az Azure piactéren 11.0 freebsd rendszerű</span><span class="sxs-lookup"><span data-stu-id="034f0-116">FreeBSD 11.0 on the Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a><span data-ttu-id="034f0-117">Az Azure CLI 2.0 keresztül freebsd rendszerű virtuális gép létrehozása az freebsd rendszerű</span><span class="sxs-lookup"><span data-stu-id="034f0-117">Create a FreeBSD VM through Azure CLI 2.0 on FreeBSD</span></span>
<span data-ttu-id="034f0-118">Először telepítenie kell [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) , ha a következő parancsot egy freebsd rendszerű gépen.</span><span class="sxs-lookup"><span data-stu-id="034f0-118">First you need to install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) though following command on a FreeBSD machine.</span></span>

```bash 
    curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="034f0-119">Ha a bash nincs telepítve a freebsd rendszerű számítógépre, futtassa a következő parancsot a telepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="034f0-119">If bash is not installed on your FreeBSD machine, run following command before the installation.</span></span> 

```
    sudo pkg install bash
```

<span data-ttu-id="034f0-120">Ha a python nincs telepítve a freebsd rendszerű számítógépre, futtassa a következő parancsokat a telepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="034f0-120">If python is not installed on your FreeBSD machine, run following commands before the installation.</span></span> 

```
    sudo pkg install python35
    cd /usr/local/bin 
    sudo rm /usr/local/bin/python 
    sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

<span data-ttu-id="034f0-121">A telepítés során a rendszer felkéri `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`.</span><span class="sxs-lookup"><span data-stu-id="034f0-121">During the installation, you are asked `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`.</span></span> <span data-ttu-id="034f0-122">Válasz `y` , és írja be `/etc/rc.conf` , `a path to an rc file to update`, előfordulhat, hogy megfeleljen a probléma `ERROR: [Errno 13] Permission denied`.</span><span class="sxs-lookup"><span data-stu-id="034f0-122">If you answer `y` and enter `/etc/rc.conf` as `a path to an rc file to update`, you may meet the problem `ERROR: [Errno 13] Permission denied`.</span></span> <span data-ttu-id="034f0-123">A probléma megoldásához, akkor adja meg az írási jogosultság szemben a fájl aktuális felhasználó számára `etc/rc.conf`.</span><span class="sxs-lookup"><span data-stu-id="034f0-123">To resolve this problem, you should grant the write right to current user against the file `etc/rc.conf`.</span></span>

<span data-ttu-id="034f0-124">Most már Azure-e jelentkezni, és a freebsd rendszerű virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="034f0-124">Now you can log in Azure and create your FreeBSD VM.</span></span> <span data-ttu-id="034f0-125">Az alábbiakban példája 11.0 freebsd rendszerű virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="034f0-125">Below is an example to create a FreeBSD 11.0 VM.</span></span> <span data-ttu-id="034f0-126">Azt is megteheti a paraméter `--public-ip-address-dns-name` egy újonnan létrehozott nyilvános IP-cím globálisan egyedi DNS-név.</span><span class="sxs-lookup"><span data-stu-id="034f0-126">You can also add the parameter `--public-ip-address-dns-name` with a globally unique DNS name for a newly created Public IP.</span></span> 

```azurecli
    az login 
    az group create -n myResourceGroup -l westus az vm create -n myFreeBSD11 -g myResourceGroup --image MicrosoftOSTC:FreeBSD:11.0:latest --admin-username azureuser --ssh-key-value /etc/ssh/ssh_host_rsa_key.pub 
```

<span data-ttu-id="034f0-127">Ezután bejelentkezhet a freebsd rendszerű virtuális géphez az IP-cím, amelyet a központi telepítés fent kimenetében keresztül.</span><span class="sxs-lookup"><span data-stu-id="034f0-127">Then you can log in to your FreeBSD VM through the ip address that printed in the output of above deployment.</span></span> 

```bash
    ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a><span data-ttu-id="034f0-128">Freebsd rendszerű Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="034f0-128">VM extensions for FreeBSD</span></span>
<span data-ttu-id="034f0-129">Az alábbiakban freebsd rendszerű támogatott Virtuálisgép-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="034f0-129">Following are supported VM extensions in FreeBSD.</span></span>

### <a name="vmaccess"></a><span data-ttu-id="034f0-130">Vmaccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="034f0-130">VMAccess</span></span>
<span data-ttu-id="034f0-131">A [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) bővítmény is:</span><span class="sxs-lookup"><span data-stu-id="034f0-131">The [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extension can:</span></span>

* <span data-ttu-id="034f0-132">Az eredeti sudo felhasználó jelszavának alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="034f0-132">Reset the password of the original sudo user.</span></span>
* <span data-ttu-id="034f0-133">Hozzon létre egy új felhasználót sudo megadott jelszót.</span><span class="sxs-lookup"><span data-stu-id="034f0-133">Create a new sudo user with the password specified.</span></span>
* <span data-ttu-id="034f0-134">Állítsa be a nyilvános állomás kulcsát a megadott kulccsal.</span><span class="sxs-lookup"><span data-stu-id="034f0-134">Set the public host key with the key given.</span></span>
* <span data-ttu-id="034f0-135">Alaphelyzetbe állítja a nyilvános állomás kulcsát, ha a gazdagép kulcs nem áll rendelkezésre a virtuális gép kiépítése során megadott.</span><span class="sxs-lookup"><span data-stu-id="034f0-135">Reset the public host key provided during VM provisioning if the host key is not provided.</span></span>
* <span data-ttu-id="034f0-136">Nyissa meg az SSH-port (22-es), és állítsa vissza a sshd_config, ha reset_ssh értéke TRUE.</span><span class="sxs-lookup"><span data-stu-id="034f0-136">Open the SSH port (22) and restore the sshd_config if reset_ssh is set to true.</span></span>
* <span data-ttu-id="034f0-137">Távolítsa el a meglévő felhasználót.</span><span class="sxs-lookup"><span data-stu-id="034f0-137">Remove the existing user.</span></span>
* <span data-ttu-id="034f0-138">Ellenőrizze a lemezek.</span><span class="sxs-lookup"><span data-stu-id="034f0-138">Check disks.</span></span>
* <span data-ttu-id="034f0-139">Javítsa ki az felvett lemezekről.</span><span class="sxs-lookup"><span data-stu-id="034f0-139">Repair an added disk.</span></span>

### <a name="customscript"></a><span data-ttu-id="034f0-140">CustomScript</span><span class="sxs-lookup"><span data-stu-id="034f0-140">CustomScript</span></span>
<span data-ttu-id="034f0-141">A [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) bővítmény is:</span><span class="sxs-lookup"><span data-stu-id="034f0-141">The [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extension can:</span></span>

* <span data-ttu-id="034f0-142">Ha a megadott, le az egyéni parancsfájlok Azure Storage vagy a külső nyilvános tárhelyen (például GitHub).</span><span class="sxs-lookup"><span data-stu-id="034f0-142">If provided, download the customized scripts from Azure Storage or external public storage (for example, GitHub).</span></span>
* <span data-ttu-id="034f0-143">A belépési pont parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="034f0-143">Run the entry point script.</span></span>
* <span data-ttu-id="034f0-144">Támogatja a soron belüli parancsokat.</span><span class="sxs-lookup"><span data-stu-id="034f0-144">Support inline commands.</span></span>
* <span data-ttu-id="034f0-145">Windows-stílusú sortörés a rendszerhéj- és Python parancsfájlok automatikusan konvertálni.</span><span class="sxs-lookup"><span data-stu-id="034f0-145">Convert Windows-style newline in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="034f0-146">Távolítsa el azt AJ rendszerhéjat, és a Python parancsfájlok automatikusan.</span><span class="sxs-lookup"><span data-stu-id="034f0-146">Remove BOM in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="034f0-147">CommandToExecute bizalmas adatok védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="034f0-147">Protect sensitive data in CommandToExecute.</span></span>

> [!NOTE]
> <span data-ttu-id="034f0-148">Freebsd rendszerű virtuális gép csak támogatja CustomScript mostanra 1.x.</span><span class="sxs-lookup"><span data-stu-id="034f0-148">FreeBSD VM only supports CustomScript version 1.x by now.</span></span>  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a><span data-ttu-id="034f0-149">Hitelesítési: felhasználóneveket, jelszavakat és SSH-kulcsok</span><span class="sxs-lookup"><span data-stu-id="034f0-149">Authentication: user names, passwords, and SSH keys</span></span>
<span data-ttu-id="034f0-150">Freebsd rendszerű virtuális gép létrehozása az Azure portál használatával, meg kell adnia egy felhasználónevet, jelszót vagy nyilvános SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="034f0-150">When you're creating a FreeBSD virtual machine by using the Azure portal, you must provide a user name, password, or SSH public key.</span></span>
<span data-ttu-id="034f0-151">Felhasználónevek Azure freebsd rendszerű virtuális gép üzembe helyezéséhez nem felelhet meg rendszerfiókok nevét (UID < 100) már szerepel a virtuális gép ("Gyökér", például).</span><span class="sxs-lookup"><span data-stu-id="034f0-151">User names for deploying a FreeBSD virtual machine on Azure must not match names of system accounts (UID <100) already present in the virtual machine ("root", for example).</span></span>
<span data-ttu-id="034f0-152">Jelenleg csak az RSA SSH-kulcsának használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="034f0-152">Currently, only the RSA SSH key is supported.</span></span> <span data-ttu-id="034f0-153">A többsoros SSH-kulcs a következővel kell kezdődnie `---- BEGIN SSH2 PUBLIC KEY ----` és végződhet `---- END SSH2 PUBLIC KEY ----`.</span><span class="sxs-lookup"><span data-stu-id="034f0-153">A multiline SSH key must begin with `---- BEGIN SSH2 PUBLIC KEY ----` and end with `---- END SSH2 PUBLIC KEY ----`.</span></span>

## <a name="obtaining-superuser-privileges"></a><span data-ttu-id="034f0-154">Felügyelő jogosultságokat megszerzésére</span><span class="sxs-lookup"><span data-stu-id="034f0-154">Obtaining superuser privileges</span></span>
<span data-ttu-id="034f0-155">A felhasználói fiók az Azure virtuálisgép-példány telepítése során megadott egy rendszerjogosultságú fiókot.</span><span class="sxs-lookup"><span data-stu-id="034f0-155">The user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="034f0-156">A csomag a sudo a közzétett freebsd rendszerű kép telepítése.</span><span class="sxs-lookup"><span data-stu-id="034f0-156">The package of sudo was installed in the published FreeBSD image.</span></span>
<span data-ttu-id="034f0-157">Miután jelentkezett be a felhasználói fiókon keresztül, a parancsszintaxis segítségével parancsok futtathatja legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="034f0-157">After you're logged in through this user account, you can run commands as root by using the command syntax.</span></span>

```
    $ sudo <COMMAND>
```

<span data-ttu-id="034f0-158">Opcionálisan beszerezhet egy legfelső szintű rendszerhéj használatával `sudo -s`.</span><span class="sxs-lookup"><span data-stu-id="034f0-158">You can optionally obtain a root shell by using `sudo -s`.</span></span>

## <a name="known-issues"></a><span data-ttu-id="034f0-159">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="034f0-159">Known issues</span></span>
<span data-ttu-id="034f0-160">A [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) 2.2.2 rendelkezik [ismert probléma] verziója (https://github.com/Azure/WALinuxAgent/pull/517), ami a kiépítés sikertelen freebsd rendszerű virtuális gép az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="034f0-160">The [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 has a [known issue] (https://github.com/Azure/WALinuxAgent/pull/517) that causes the provision failure for FreeBSD VM on Azure.</span></span> <span data-ttu-id="034f0-161">A javítás által rögzített [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) 2.2.3 verziója és a későbbi kibocsátásokban megtörténik.</span><span class="sxs-lookup"><span data-stu-id="034f0-161">The fix was captured by [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 and later releases.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="034f0-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="034f0-162">Next steps</span></span>
* <span data-ttu-id="034f0-163">Ugrás a [Azure piactér](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) freebsd rendszerű virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="034f0-163">Go to [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) to create a FreeBSD VM.</span></span>
* <span data-ttu-id="034f0-164">Ha azt szeretné, hogy a saját freebsd rendszerű Azure, tekintse meg [létrehozása és feltöltése az Azure-bA freebsd rendszerű virtuális merevlemez](linux/classic/freebsd-create-upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="034f0-164">If you want to bring your own FreeBSD to Azure, refer to [Create and upload a FreeBSD VHD to Azure](linux/classic/freebsd-create-upload-vhd.md).</span></span>
