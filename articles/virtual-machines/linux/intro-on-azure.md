---
title: az Azure-ban aaaIntroduction tooLinux |} Microsoft Docs
description: "Tudnivalók Azure Linux virtuális gépek használatával."
services: virtual-machines-linux
documentationcenter: python
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: b13bf305-87bf-4df3-815e-e8f6337aa6ea
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: szark
ms.openlocfilehash: 3a931447ee23ce7000174ca314c3e10abc6b8e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toolinux-on-azure"></a><span data-ttu-id="2614a-103">Bevezetés tooLinux az Azure-on</span><span class="sxs-lookup"><span data-stu-id="2614a-103">Introduction tooLinux on Azure</span></span>
<span data-ttu-id="2614a-104">Ez a témakör áttekintést a Linux virtuális gépek használatát hello Azure felhőben egyes funkcióit.</span><span class="sxs-lookup"><span data-stu-id="2614a-104">This topic provides an overview of some aspects of using Linux virtual machines in hello Azure cloud.</span></span> <span data-ttu-id="2614a-105">A Linux virtuális gép telepítését egy egyszerű folyamatot a lemezkép használatával hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="2614a-105">Deploying a Linux virtual machine is a straightforward process using an image from hello gallery.</span></span>

## <a name="authentication-usernames-passwords-and-ssh-keys"></a><span data-ttu-id="2614a-106">Hitelesítési: Felhasználónevek, jelszavak és SSH-kulcsok</span><span class="sxs-lookup"><span data-stu-id="2614a-106">Authentication: Usernames, Passwords and SSH Keys</span></span>
<span data-ttu-id="2614a-107">Történő létrehozásakor a Linux virtuális gépek hello Azure-portálon, a rendszer felkéri tooprovide vagy felhasználónév és jelszó vagy nyilvános SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2614a-107">When creating a Linux virtual machine using hello Azure portal, you are asked tooprovide a either username and password or an SSH public key.</span></span> <span data-ttu-id="2614a-108">hello egy felhasználónévvel, egy Linux virtuális gépet az Azure telepítéséhez választott nem a következő megkötés tulajdonos toohello: rendszer fiókok nevét (UID < 100) hello már szerepel a virtuális gép nem engedélyezettek, "Gyökér" például.</span><span class="sxs-lookup"><span data-stu-id="2614a-108">hello choice of a username for deploying a Linux virtual machine on Azure is subject toohello following constraint: names of system accounts (UID <100) already present in hello virtual machine are not allowed, 'root' for example.</span></span>

* <span data-ttu-id="2614a-109">Lásd: [Linuxot futtató virtuális gép létrehozása](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2614a-109">See [Create a Virtual Machine Running Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="2614a-110">Lásd: [hogyan tooUse Azure Linux SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2614a-110">See [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="obtaining-superuser-privileges-using-sudo"></a><span data-ttu-id="2614a-111">Nem sikerült beolvasni felügyelő jogosultságok használata`sudo`</span><span class="sxs-lookup"><span data-stu-id="2614a-111">Obtaining Superuser Privileges Using `sudo`</span></span>
<span data-ttu-id="2614a-112">hello Azure virtuálisgép-példány telepítése során megadott felhasználói fiók egy rendszerjogosultságú fiókot.</span><span class="sxs-lookup"><span data-stu-id="2614a-112">hello user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="2614a-113">Ez a fiók konfigurálható hello Azure Linux ügynök toobe képes tooelevate jogosultságokkal tooroot (felügyelő fiók) használatával hello `sudo` segédprogram.</span><span class="sxs-lookup"><span data-stu-id="2614a-113">This account is configured by hello Azure Linux Agent toobe able tooelevate privileges tooroot (superuser account) using hello `sudo` utility.</span></span> <span data-ttu-id="2614a-114">Miután bejelentkezett a felhasználói fiókkal, legfelső szintű hello parancsszintaxis segítségével fogja képes toorun parancsokat:</span><span class="sxs-lookup"><span data-stu-id="2614a-114">Once logged in using this user account, you will be able toorun commands as root using hello command syntax:</span></span>

    # sudo <COMMAND>

<span data-ttu-id="2614a-115">Opcionálisan beszerezhet egy legfelső szintű rendszerhéj használatával **sudo -s**.</span><span class="sxs-lookup"><span data-stu-id="2614a-115">You can optionally obtain a root shell using **sudo -s**.</span></span>

* <span data-ttu-id="2614a-116">Lásd: [root jogosultságot használata a Linux virtuális gépek Azure-ban](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2614a-116">See [Using root privileges on Linux virtual machines in Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="firewall-configuration"></a><span data-ttu-id="2614a-117">Tűzfal-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="2614a-117">Firewall Configuration</span></span>
<span data-ttu-id="2614a-118">Azure biztosít egy bejövő csomag szűrőt, amely korlátozza a kapcsolat tooports megadott hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2614a-118">Azure provides an inbound packet filter that restricts connectivity tooports specified in hello Azure portal.</span></span> <span data-ttu-id="2614a-119">Alapértelmezés szerint hello csak engedélyezett port az SSH.</span><span class="sxs-lookup"><span data-stu-id="2614a-119">By default, hello only allowed port is SSH.</span></span> <span data-ttu-id="2614a-120">Megnyitható másolatot hozzáférési tooadditional portok a Linux virtuális gép végpontok konfigurálásával hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="2614a-120">You may open up access tooadditional ports on your Linux virtual machine by configuring endpoints in hello Azure portal:</span></span>

* <span data-ttu-id="2614a-121">Lásd: [hogyan tooSet be végpontok tooa virtuális gép](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2614a-121">See: [How tooSet Up Endpoints tooa Virtual Machine](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="2614a-122">hello Linux képek hello Azure-katalógus nem engedélyezi a hello *iptables* tűzfal alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="2614a-122">hello Linux images in hello Azure Gallery do not enable hello *iptables* firewall by default.</span></span> <span data-ttu-id="2614a-123">Igény szerint konfigurálható hello tűzfal tooprovide további szűrést.</span><span class="sxs-lookup"><span data-stu-id="2614a-123">If desired, hello firewall may be configured tooprovide additional filtering.</span></span>

## <a name="hostname-changes"></a><span data-ttu-id="2614a-124">Állomásnév változások</span><span class="sxs-lookup"><span data-stu-id="2614a-124">Hostname Changes</span></span>
<span data-ttu-id="2614a-125">A Linux-lemezkép egy példányát telepít elkülönítésével szükséges tooprovide egy hello virtuálisgép-nevet.</span><span class="sxs-lookup"><span data-stu-id="2614a-125">When you initially deploy an instance of a Linux image, you are required tooprovide a host name for hello virtual machine.</span></span> <span data-ttu-id="2614a-126">Ha hello virtuális gép fut, ez az állomásnév közzétett toohello platform DNS-kiszolgálók több csatlakozó virtuális gépek tooeach más IP-cím keresések segítségével hostnames hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="2614a-126">Once hello virtual machine is running, this hostname is published toohello platform DNS servers so that multiple virtual machines connected tooeach other can perform IP address lookups using hostnames.</span></span>

<span data-ttu-id="2614a-127">Igény állomásnév módosítások vannak egy virtuális gép telepítését követően, használjon hello parancs</span><span class="sxs-lookup"><span data-stu-id="2614a-127">If hostname changes are desired after a virtual machine has been deployed, please use hello command</span></span>

    # sudo hostname <newname>

<span data-ttu-id="2614a-128">hello Azure Linux ügynök funkcióit tartalmazza: tooautomatically észleli a név megváltoztatása és megfelelő konfigurálása hello virtuális gép toopersist ezt a változtatást és közzététele a módosítás toohello platform DNS-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2614a-128">hello Azure Linux Agent includes functionality tooautomatically detect this name change and appropriately configure hello virtual machine toopersist this change and publish this change toohello platform DNS servers.</span></span>

* [<span data-ttu-id="2614a-129">Azure Linux ügynök felhasználói útmutatója</span><span class="sxs-lookup"><span data-stu-id="2614a-129">Azure Linux Agent User Guide</span></span>](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a><span data-ttu-id="2614a-130">Felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="2614a-130">Cloud-Init</span></span>
<span data-ttu-id="2614a-131">**Ubuntu** és **CoreOS** képek felhő inicializálás Azure, amely további képességeket biztosít a virtuális gépek rendszerindítása használják.</span><span class="sxs-lookup"><span data-stu-id="2614a-131">**Ubuntu** and **CoreOS** images utilize cloud-init on Azure, which provides additional capabilities for bootstrapping a virtual machine.</span></span>

* [<span data-ttu-id="2614a-132">Hogyan tooInject egyéni adatok</span><span class="sxs-lookup"><span data-stu-id="2614a-132">How tooInject Custom Data</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="2614a-133">Egyéni adatok és a Microsoft Azure felhőbe inicializálás</span><span class="sxs-lookup"><span data-stu-id="2614a-133">Custom Data and Cloud-Init on Microsoft Azure</span></span>](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [<span data-ttu-id="2614a-134">Partíciók létrehozása a Azure Swap használatával felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="2614a-134">Create Azure Swap Partitions Using Cloud-Init</span></span>](https://wiki.ubuntu.com/AzureSwapPartitions)
* [<span data-ttu-id="2614a-135">Hogyan tooUse CoreOS az Azure-on</span><span class="sxs-lookup"><span data-stu-id="2614a-135">How tooUse CoreOS on Azure</span></span>](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a><span data-ttu-id="2614a-136">Virtuális gép lemezképének rögzítése</span><span class="sxs-lookup"><span data-stu-id="2614a-136">Virtual Machine Image Capture</span></span>
<span data-ttu-id="2614a-137">Azure hello képességét toocapture hello egy meglévő virtuális gép állapotát, ezt követően lehet használt toodeploy további virtuálisgép-példányok lemezképekbe biztosít.</span><span class="sxs-lookup"><span data-stu-id="2614a-137">Azure provides hello ability toocapture hello state of an existing virtual machine into an image that can subsequently be used toodeploy additional virtual machine instances.</span></span> <span data-ttu-id="2614a-138">hello Azure Linux ügynök lehet néhány hello hello kiépítési folyamat során végrehajtott testreszabási használt toorollback.</span><span class="sxs-lookup"><span data-stu-id="2614a-138">hello Azure Linux Agent may be used toorollback some of hello customization that was performed during hello provisioning process.</span></span> <span data-ttu-id="2614a-139">Előfordulhat, hogy kövesse az alábbi toocapture egy virtuális gép hello lépéseket képként:</span><span class="sxs-lookup"><span data-stu-id="2614a-139">You may follow hello steps below toocapture a virtual machine as an image:</span></span>

1. <span data-ttu-id="2614a-140">Futtatás **waagent-deprovision** testreszabási kiépítés tooundo.</span><span class="sxs-lookup"><span data-stu-id="2614a-140">Run **waagent -deprovision** tooundo provisioning customization.</span></span> <span data-ttu-id="2614a-141">Vagy **waagent-deprovision + felhasználói** toooptionally hello felhasználói fiók megadva során üzembe helyezési és minden kapcsolódó adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="2614a-141">Or **waagent -deprovision+user** toooptionally delete hello user account specified during provisioning and all associated data.</span></span>
2. <span data-ttu-id="2614a-142">Állítsa le vagy kapcsolja ki hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="2614a-142">Shut down/power off hello virtual machine.</span></span>
3. <span data-ttu-id="2614a-143">Kattintson a **rögzítése** hello Azure portál vagy használata hello PowerShell vagy a parancssori eszközök toocapture hello virtuális gép képként.</span><span class="sxs-lookup"><span data-stu-id="2614a-143">Click **Capture** in hello Azure portal or use hello PowerShell or CLI tools toocapture hello virtual machine as an image.</span></span>
   
   * <span data-ttu-id="2614a-144">Lásd: [hogyan tooCapture egy Linux virtuális gép tooUse sablonként](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2614a-144">See: [How tooCapture a Linux Virtual Machine tooUse as a Template](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="attaching-disks"></a><span data-ttu-id="2614a-145">Lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="2614a-145">Attaching Disks</span></span>
<span data-ttu-id="2614a-146">Minden virtuális gép van egy ideiglenes, helyi *erőforrás lemez* csatolva.</span><span class="sxs-lookup"><span data-stu-id="2614a-146">Each virtual machine has a temporary, local *resource disk* attached.</span></span> <span data-ttu-id="2614a-147">Egy erőforrás tárolt adatokat nem lehet tartós újraindítások, mert gyakran használják az alkalmazások és az átmeneti hello virtuális gépen futó folyamatok és **ideiglenes** adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="2614a-147">Because data on a resource disk may not be durable across reboots, it is often used by applications and processes running in hello virtual machine for transient and **temporary** storage of data.</span></span> <span data-ttu-id="2614a-148">Akkor is használt toostore hello lap vagy swap hello operációs rendszer fájljait.</span><span class="sxs-lookup"><span data-stu-id="2614a-148">It is also used toostore hello page or swap files for hello operating system.</span></span>

<span data-ttu-id="2614a-149">Linux, hello erőforrás lemez általában hello Azure Linux ügynök által felügyelt, és automatikusan csatlakoztatott túl**/mnt és az erőforrások** (vagy **/mnt** Ubuntu képek).</span><span class="sxs-lookup"><span data-stu-id="2614a-149">On Linux, hello resource disk is typically managed by hello Azure Linux Agent and automatically mounted too**/mnt/resource** (or **/mnt** on Ubuntu images).</span></span>

> [!NOTE]
> <span data-ttu-id="2614a-150">Vegye figyelembe, hogy hello erőforrás lemez egy **ideiglenes** lemez, és előfordulhat, hogy törli, majd amikor hello virtuális gép újraindítása után formázni.</span><span class="sxs-lookup"><span data-stu-id="2614a-150">Note that hello resource disk is a **temporary** disk, and might be deleted and reformatted when hello VM is rebooted.</span></span>
> 
> 

<span data-ttu-id="2614a-151">Linux hello adatlemez, hello kernel neve lehet `/dev/sdc`, és toopartition, a felhasználóknak kell formázni, és csatlakoztassa az adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2614a-151">On Linux hello data disk might be named by hello kernel as `/dev/sdc`, and users will need toopartition, format and mount that resource.</span></span> <span data-ttu-id="2614a-152">Ez vonatkozik hello oktatóprogram lépésről lépésre: [hogyan tooAttach egy virtuális gép adatlemez tooa](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2614a-152">This is covered step-by-step in hello tutorial: [How tooAttach a Data Disk tooa Virtual Machine](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

* <span data-ttu-id="2614a-153">**További információ:** [szoftveres RAID Linux konfigurálása](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [LVM konfigurálása a Linux virtuális gép az Azure-ban](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2614a-153">**See also:** [Configure Software RAID on Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [Configure LVM on a Linux VM in Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

