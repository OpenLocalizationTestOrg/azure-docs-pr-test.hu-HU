---
title: az Azure-on aaaIntroduction tooFreeBSD |} Microsoft Docs
description: "További információ az Azure-on freebsd rendszerű virtuális gépek használatáról"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: 43ba7a70ed21e7fb8b331f4a26db0426e098c4aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toofreebsd-on-azure"></a><span data-ttu-id="139a0-103">Bevezetés tooFreeBSD az Azure-on</span><span class="sxs-lookup"><span data-stu-id="139a0-103">Introduction tooFreeBSD on Azure</span></span>
<span data-ttu-id="139a0-104">Ez a témakör az Azure-ban freebsd rendszerű virtuális gépet futtató áttekintését.</span><span class="sxs-lookup"><span data-stu-id="139a0-104">This topic provides an overview of running a FreeBSD virtual machine in Azure.</span></span>

## <a name="overview"></a><span data-ttu-id="139a0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="139a0-105">Overview</span></span>
<span data-ttu-id="139a0-106">A Microsoft Azure freebsd rendszerű egy speciális operációs rendszerhez használt toopower modern kiszolgálók, asztali számítógépek és beágyazott platformok.</span><span class="sxs-lookup"><span data-stu-id="139a0-106">FreeBSD for Microsoft Azure is an advanced computer operating system used toopower modern servers, desktops, and embedded platforms.</span></span>

<span data-ttu-id="139a0-107">A Microsoft Corporation van elérhetővé képeket freebsd rendszerű az Azure a hello [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) előre konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="139a0-107">Microsoft Corporation is making images of FreeBSD available on Azure with hello [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) pre-configured.</span></span> <span data-ttu-id="139a0-108">Jelenleg hello következő freebsd rendszerű verziók képként által felajánlott Microsoft:</span><span class="sxs-lookup"><span data-stu-id="139a0-108">Currently, hello following FreeBSD versions are offered as images by Microsoft:</span></span>

- <span data-ttu-id="139a0-109">Freebsd rendszerű 10.3-kiadás</span><span class="sxs-lookup"><span data-stu-id="139a0-109">FreeBSD 10.3-RELEASE</span></span>
- <span data-ttu-id="139a0-110">Freebsd rendszerű 11.0-kiadás</span><span class="sxs-lookup"><span data-stu-id="139a0-110">FreeBSD 11.0-RELEASE</span></span>

<span data-ttu-id="139a0-111">hello ügynök hello freebsd rendszerű virtuális gép közötti kommunikáció felelős és hello Azure-hálót műveletek, például a kiépítés hello virtuális gép első használat (felhasználónév, jelszó vagy SSH-kulcsot, állomásnév, stb.) és a szelektív Virtuálisgép-bővítmények lehetővé teszi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="139a0-111">hello agent is responsible for communication between hello FreeBSD VM and hello Azure fabric for operations such as provisioning hello VM on first use (user name, password or SSH key, host name, etc.) and enabling functionality for selective VM extensions.</span></span>

<span data-ttu-id="139a0-112">Mint freebsd rendszerű jövőbeli verzióiban hello stratégia aktuális toostay, és elérhetővé hello legfrissebb verziókban azok közzététele hello freebsd rendszerű kiadás mérnöki csapat követően rövid időn belül.</span><span class="sxs-lookup"><span data-stu-id="139a0-112">As for future versions of FreeBSD, hello strategy is toostay current and make hello latest releases available shortly after they are published by hello FreeBSD release engineering team.</span></span>

## <a name="deploying-a-freebsd-virtual-machine"></a><span data-ttu-id="139a0-113">Freebsd rendszerű virtuális gép telepítése</span><span class="sxs-lookup"><span data-stu-id="139a0-113">Deploying a FreeBSD virtual machine</span></span>
<span data-ttu-id="139a0-114">Egy egyszerű folyamatot hello Azure Piactérről származó hello Azure-portálon a lemezkép használatával freebsd rendszerű virtuális gép telepítése:</span><span class="sxs-lookup"><span data-stu-id="139a0-114">Deploying a FreeBSD virtual machine is a straightforward process using an image from hello Azure Marketplace from hello Azure portal:</span></span>

- [<span data-ttu-id="139a0-115">Az Azure piactér hello freebsd rendszerű 10.3</span><span class="sxs-lookup"><span data-stu-id="139a0-115">FreeBSD 10.3 on hello Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [<span data-ttu-id="139a0-116">Az Azure piactér hello freebsd rendszerű 11.0</span><span class="sxs-lookup"><span data-stu-id="139a0-116">FreeBSD 11.0 on hello Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a><span data-ttu-id="139a0-117">Az Azure CLI 2.0 keresztül freebsd rendszerű virtuális gép létrehozása az freebsd rendszerű</span><span class="sxs-lookup"><span data-stu-id="139a0-117">Create a FreeBSD VM through Azure CLI 2.0 on FreeBSD</span></span>
<span data-ttu-id="139a0-118">Először meg kell tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) , ha a következő parancsot egy freebsd rendszerű gépen.</span><span class="sxs-lookup"><span data-stu-id="139a0-118">First you need tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) though following command on a FreeBSD machine.</span></span>

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="139a0-119">Ha a bash nincs telepítve a freebsd rendszerű számítógépre, futtassa a következő parancs hello telepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="139a0-119">If bash is not installed on your FreeBSD machine, run following command before hello installation.</span></span> 

```bash
sudo pkg install bash
```

<span data-ttu-id="139a0-120">Ha a python nincs telepítve a freebsd rendszerű számítógépre, futtassa a következő parancsok hello telepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="139a0-120">If python is not installed on your FreeBSD machine, run following commands before hello installation.</span></span> 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

<span data-ttu-id="139a0-121">Hello a telepítés során a rendszer felkéri `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`.</span><span class="sxs-lookup"><span data-stu-id="139a0-121">During hello installation, you are asked `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`.</span></span> <span data-ttu-id="139a0-122">Válasz `y` , és írja be `/etc/rc.conf` , `a path tooan rc file tooupdate`, előfordulhat, hogy megfelel a hello probléma `ERROR: [Errno 13] Permission denied`.</span><span class="sxs-lookup"><span data-stu-id="139a0-122">If you answer `y` and enter `/etc/rc.conf` as `a path tooan rc file tooupdate`, you may meet hello problem `ERROR: [Errno 13] Permission denied`.</span></span> <span data-ttu-id="139a0-123">tooresolve a probléma, akkor adja meg az hello írási jobb toocurrent felhasználót hello fájl `etc/rc.conf`.</span><span class="sxs-lookup"><span data-stu-id="139a0-123">tooresolve this problem, you should grant hello write right toocurrent user against hello file `etc/rc.conf`.</span></span>

<span data-ttu-id="139a0-124">Most már Azure-e jelentkezni, és a freebsd rendszerű virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="139a0-124">Now you can log in Azure and create your FreeBSD VM.</span></span> <span data-ttu-id="139a0-125">Az alábbiakban van egy példa toocreate 11.0 freebsd rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="139a0-125">Below is an example toocreate a FreeBSD 11.0 VM.</span></span> <span data-ttu-id="139a0-126">Azt is megteheti hello paraméter `--public-ip-address-dns-name` egy újonnan létrehozott nyilvános IP-cím globálisan egyedi DNS-név.</span><span class="sxs-lookup"><span data-stu-id="139a0-126">You can also add hello parameter `--public-ip-address-dns-name` with a globally unique DNS name for a newly created Public IP.</span></span> 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="139a0-127">Ezután bejelentkezhet tooyour freebsd rendszerű virtuális gép hello IP-címet, amelyet a központi telepítési fent hello kimenetét a keresztül.</span><span class="sxs-lookup"><span data-stu-id="139a0-127">Then you can log in tooyour FreeBSD VM through hello ip address that printed in hello output of above deployment.</span></span> 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a><span data-ttu-id="139a0-128">Freebsd rendszerű Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="139a0-128">VM extensions for FreeBSD</span></span>
<span data-ttu-id="139a0-129">Az alábbiakban freebsd rendszerű támogatott Virtuálisgép-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="139a0-129">Following are supported VM extensions in FreeBSD.</span></span>

### <a name="vmaccess"></a><span data-ttu-id="139a0-130">Vmaccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="139a0-130">VMAccess</span></span>
<span data-ttu-id="139a0-131">Hello [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) bővítmény is:</span><span class="sxs-lookup"><span data-stu-id="139a0-131">hello [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extension can:</span></span>

* <span data-ttu-id="139a0-132">Hello jelszó-átállítási hello eredeti sudo felhasználó.</span><span class="sxs-lookup"><span data-stu-id="139a0-132">Reset hello password of hello original sudo user.</span></span>
* <span data-ttu-id="139a0-133">Hozzon létre egy új felhasználót sudo hello jelszót adott meg.</span><span class="sxs-lookup"><span data-stu-id="139a0-133">Create a new sudo user with hello password specified.</span></span>
* <span data-ttu-id="139a0-134">Állítsa be a hello nyilvános állomáskulcsa megadott hello kulccsal.</span><span class="sxs-lookup"><span data-stu-id="139a0-134">Set hello public host key with hello key given.</span></span>
* <span data-ttu-id="139a0-135">Alaphelyzetbe állítja a hello nyilvános állomáskulcsa során Virtuálisgép-létrehozásnál Ha hello állomáskulcsa nem áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="139a0-135">Reset hello public host key provided during VM provisioning if hello host key is not provided.</span></span>
* <span data-ttu-id="139a0-136">Nyissa meg a hello SSH-port (22-es), és visszaállítását hello sshd_config reset_ssh tootrue van beállítva.</span><span class="sxs-lookup"><span data-stu-id="139a0-136">Open hello SSH port (22) and restore hello sshd_config if reset_ssh is set tootrue.</span></span>
* <span data-ttu-id="139a0-137">Távolítsa el a meglévő felhasználói hello.</span><span class="sxs-lookup"><span data-stu-id="139a0-137">Remove hello existing user.</span></span>
* <span data-ttu-id="139a0-138">Ellenőrizze a lemezek.</span><span class="sxs-lookup"><span data-stu-id="139a0-138">Check disks.</span></span>
* <span data-ttu-id="139a0-139">Javítsa ki az felvett lemezekről.</span><span class="sxs-lookup"><span data-stu-id="139a0-139">Repair an added disk.</span></span>

### <a name="customscript"></a><span data-ttu-id="139a0-140">CustomScript</span><span class="sxs-lookup"><span data-stu-id="139a0-140">CustomScript</span></span>
<span data-ttu-id="139a0-141">Hello [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) bővítmény is:</span><span class="sxs-lookup"><span data-stu-id="139a0-141">hello [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extension can:</span></span>

* <span data-ttu-id="139a0-142">Ha a megadott, le hello egyéni parancsfájlok Azure Storage vagy a külső nyilvános tárhelyen (például GitHub).</span><span class="sxs-lookup"><span data-stu-id="139a0-142">If provided, download hello customized scripts from Azure Storage or external public storage (for example, GitHub).</span></span>
* <span data-ttu-id="139a0-143">Hello belépési pont parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="139a0-143">Run hello entry point script.</span></span>
* <span data-ttu-id="139a0-144">Támogatja a soron belüli parancsokat.</span><span class="sxs-lookup"><span data-stu-id="139a0-144">Support inline commands.</span></span>
* <span data-ttu-id="139a0-145">Windows-stílusú sortörés a rendszerhéj- és Python parancsfájlok automatikusan konvertálni.</span><span class="sxs-lookup"><span data-stu-id="139a0-145">Convert Windows-style newline in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="139a0-146">Távolítsa el azt AJ rendszerhéjat, és a Python parancsfájlok automatikusan.</span><span class="sxs-lookup"><span data-stu-id="139a0-146">Remove BOM in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="139a0-147">CommandToExecute bizalmas adatok védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="139a0-147">Protect sensitive data in CommandToExecute.</span></span>

> [!NOTE]
> <span data-ttu-id="139a0-148">Freebsd rendszerű virtuális gép csak támogatja CustomScript mostanra 1.x.</span><span class="sxs-lookup"><span data-stu-id="139a0-148">FreeBSD VM only supports CustomScript version 1.x by now.</span></span>  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a><span data-ttu-id="139a0-149">Hitelesítési: felhasználóneveket, jelszavakat és SSH-kulcsok</span><span class="sxs-lookup"><span data-stu-id="139a0-149">Authentication: user names, passwords, and SSH keys</span></span>
<span data-ttu-id="139a0-150">Freebsd rendszerű virtuális gép létrehozásakor hello Azure portál segítségével meg kell adnia egy felhasználónevet, jelszót vagy nyilvános SSH-kulcs.</span><span class="sxs-lookup"><span data-stu-id="139a0-150">When you're creating a FreeBSD virtual machine by using hello Azure portal, you must provide a user name, password, or SSH public key.</span></span>
<span data-ttu-id="139a0-151">Felhasználónevek Azure freebsd rendszerű virtuális gép üzembe helyezéséhez nem felelhet meg rendszerfiókok nevét (UID < 100) már szerepel a hello virtuális gép ("Gyökér", például).</span><span class="sxs-lookup"><span data-stu-id="139a0-151">User names for deploying a FreeBSD virtual machine on Azure must not match names of system accounts (UID <100) already present in hello virtual machine ("root", for example).</span></span>
<span data-ttu-id="139a0-152">Jelenleg csak hello RSA SSH-kulcsának használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="139a0-152">Currently, only hello RSA SSH key is supported.</span></span> <span data-ttu-id="139a0-153">A többsoros SSH-kulcs a következővel kell kezdődnie `---- BEGIN SSH2 PUBLIC KEY ----` és végződhet `---- END SSH2 PUBLIC KEY ----`.</span><span class="sxs-lookup"><span data-stu-id="139a0-153">A multiline SSH key must begin with `---- BEGIN SSH2 PUBLIC KEY ----` and end with `---- END SSH2 PUBLIC KEY ----`.</span></span>

## <a name="obtaining-superuser-privileges"></a><span data-ttu-id="139a0-154">Felügyelő jogosultságokat megszerzésére</span><span class="sxs-lookup"><span data-stu-id="139a0-154">Obtaining superuser privileges</span></span>
<span data-ttu-id="139a0-155">hello Azure virtuálisgép-példány telepítése során megadott felhasználói fiók egy rendszerjogosultságú fiókot.</span><span class="sxs-lookup"><span data-stu-id="139a0-155">hello user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="139a0-156">hello csomagot a sudo telepítette a hello közzétett freebsd rendszerű kép.</span><span class="sxs-lookup"><span data-stu-id="139a0-156">hello package of sudo was installed in hello published FreeBSD image.</span></span>
<span data-ttu-id="139a0-157">Miután jelentkezett be a felhasználói fiókon keresztül, futtathatja parancsok legfelső szintű hello parancsszintaxis segítségével.</span><span class="sxs-lookup"><span data-stu-id="139a0-157">After you're logged in through this user account, you can run commands as root by using hello command syntax.</span></span>

```
$ sudo <COMMAND>
```

<span data-ttu-id="139a0-158">Opcionálisan beszerezhet egy legfelső szintű rendszerhéj használatával `sudo -s`.</span><span class="sxs-lookup"><span data-stu-id="139a0-158">You can optionally obtain a root shell by using `sudo -s`.</span></span>

## <a name="known-issues"></a><span data-ttu-id="139a0-159">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="139a0-159">Known issues</span></span>
<span data-ttu-id="139a0-160">Hello [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) 2.2.2 rendelkezik [ismert probléma] verziója (https://github.com/Azure/WALinuxAgent/pull/517), ami hello kiépítése sikertelen freebsd rendszerű virtuális gép az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="139a0-160">hello [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 has a [known issue] (https://github.com/Azure/WALinuxAgent/pull/517) that causes hello provision failure for FreeBSD VM on Azure.</span></span> <span data-ttu-id="139a0-161">hello javítás által rögzített [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) 2.2.3 verziója és a későbbi kibocsátásokban megtörténik.</span><span class="sxs-lookup"><span data-stu-id="139a0-161">hello fix was captured by [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 and later releases.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="139a0-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="139a0-162">Next steps</span></span>
* <span data-ttu-id="139a0-163">Nyissa meg túl[Azure piactér](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate freebsd rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="139a0-163">Go too[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate a FreeBSD VM.</span></span>
* <span data-ttu-id="139a0-164">Ha a toobring saját freebsd rendszerű tooAzure, olvassa el túl[létrehozása és feltöltése egy freebsd rendszerű virtuális merevlemez tooAzure](classic/freebsd-create-upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="139a0-164">If you want toobring your own FreeBSD tooAzure, refer too[Create and upload a FreeBSD VHD tooAzure](classic/freebsd-create-upload-vhd.md).</span></span>
