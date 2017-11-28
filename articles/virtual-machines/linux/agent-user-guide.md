---
title: "Azure Linux virtuális gép ügynök áttekintése |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse és konfigurálja a Linux-ügynök (waagent) a virtuális gép az Azure Fabric Controller kezeléséhez."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486ad6bb148583a957fb82b7954ff94f853b12cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-and-using-the-azure-linux-agent"></a><span data-ttu-id="96e5f-103">Megismeréséhez és használatához az Azure Linux ügynök</span><span class="sxs-lookup"><span data-stu-id="96e5f-103">Understanding and using the Azure Linux Agent</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a><span data-ttu-id="96e5f-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="96e5f-104">Introduction</span></span>
<span data-ttu-id="96e5f-105">A Microsoft Azure Linux-ügynök (waagent) a Linux és freebsd rendszerű kiépítés és az Azure Fabric Controller interakcióba VM kezeli.</span><span class="sxs-lookup"><span data-stu-id="96e5f-105">The Microsoft Azure Linux Agent (waagent) manages Linux & FreeBSD provisioning, and VM interaction with the Azure Fabric Controller.</span></span> <span data-ttu-id="96e5f-106">A következő funkciókat biztosít a Linux és freebsd rendszerű infrastruktúra-szolgáltatási központi telepítések:</span><span class="sxs-lookup"><span data-stu-id="96e5f-106">It provides the following functionality for Linux and FreeBSD IaaS deployments:</span></span>

> [!NOTE]
> <span data-ttu-id="96e5f-107">Tekintse meg az Azure Linux ügynök [információs](https://github.com/Azure/WALinuxAgent/blob/master/README.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="96e5f-107">See the Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) for additional details.</span></span>
> 
> 

* <span data-ttu-id="96e5f-108">**Kép kiépítése**</span><span class="sxs-lookup"><span data-stu-id="96e5f-108">**Image Provisioning**</span></span>
  
  * <span data-ttu-id="96e5f-109">A felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="96e5f-109">Creation of a user account</span></span>
  * <span data-ttu-id="96e5f-110">SSH hitelesítési típus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96e5f-110">Configuring SSH authentication types</span></span>
  * <span data-ttu-id="96e5f-111">Az SSH nyilvános kulcsok és kulcspárok központi telepítését</span><span class="sxs-lookup"><span data-stu-id="96e5f-111">Deployment of SSH public keys and key pairs</span></span>
  * <span data-ttu-id="96e5f-112">Beállítás a gazdagép neve</span><span class="sxs-lookup"><span data-stu-id="96e5f-112">Setting the host name</span></span>
  * <span data-ttu-id="96e5f-113">Az állomásnév közzététele a DNS platform</span><span class="sxs-lookup"><span data-stu-id="96e5f-113">Publishing the host name to the platform DNS</span></span>
  * <span data-ttu-id="96e5f-114">SSH-kulcs ujjlenyomat állomás Reporting a platformra</span><span class="sxs-lookup"><span data-stu-id="96e5f-114">Reporting SSH host key fingerprint to the platform</span></span>
  * <span data-ttu-id="96e5f-115">Erőforrás Lemezkezelés</span><span class="sxs-lookup"><span data-stu-id="96e5f-115">Resource Disk Management</span></span>
  * <span data-ttu-id="96e5f-116">Formázás és az erőforrás-lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="96e5f-116">Formatting and mounting the resource disk</span></span>
  * <span data-ttu-id="96e5f-117">Lapozófájl konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96e5f-117">Configuring swap space</span></span>
* <span data-ttu-id="96e5f-118">**Hálózat**</span><span class="sxs-lookup"><span data-stu-id="96e5f-118">**Networking**</span></span>
  
  * <span data-ttu-id="96e5f-119">Útvonalak való kompatibilitás érdekében platform DHCP-kiszolgálók kezelése</span><span class="sxs-lookup"><span data-stu-id="96e5f-119">Manages routes to improve compatibility with platform DHCP servers</span></span>
  * <span data-ttu-id="96e5f-120">Biztosítja a stabilitását a hálózati kapcsolat neve</span><span class="sxs-lookup"><span data-stu-id="96e5f-120">Ensures the stability of the network interface name</span></span>
* <span data-ttu-id="96e5f-121">**Kernel**</span><span class="sxs-lookup"><span data-stu-id="96e5f-121">**Kernel**</span></span>
  
  * <span data-ttu-id="96e5f-122">Konfigurálja a virtuális NUMA (letiltja a kernel < 2.6.37)</span><span class="sxs-lookup"><span data-stu-id="96e5f-122">Configures virtual NUMA (disable for kernel <2.6.37)</span></span>
  * <span data-ttu-id="96e5f-123">Hyper-V entrópia /dev/random a felhasználva</span><span class="sxs-lookup"><span data-stu-id="96e5f-123">Consumes Hyper-V entropy for /dev/random</span></span>
  * <span data-ttu-id="96e5f-124">Konfigurálja az SCSI-időtúllépések a legfelső szintű eszköz (amely lehet távoli)</span><span class="sxs-lookup"><span data-stu-id="96e5f-124">Configures SCSI timeouts for the root device (which could be remote)</span></span>
* <span data-ttu-id="96e5f-125">**Diagnosztika**</span><span class="sxs-lookup"><span data-stu-id="96e5f-125">**Diagnostics**</span></span>
  
  * <span data-ttu-id="96e5f-126">A soros portjára segítségével</span><span class="sxs-lookup"><span data-stu-id="96e5f-126">Console redirection to the serial port</span></span>
* <span data-ttu-id="96e5f-127">**Az SCVMM központi telepítések**</span><span class="sxs-lookup"><span data-stu-id="96e5f-127">**SCVMM Deployments**</span></span>
  
  * <span data-ttu-id="96e5f-128">Észleli és a VMM-ügynök Linux betöltéséhez, ha a System Center Virtual Machine Manager 2012 R2 környezetben futó</span><span class="sxs-lookup"><span data-stu-id="96e5f-128">Detects and bootstraps the VMM agent for Linux when running in a System Center Virtual Machine Manager 2012 R2 environment</span></span>
* <span data-ttu-id="96e5f-129">**Virtuálisgép-bővítmény**</span><span class="sxs-lookup"><span data-stu-id="96e5f-129">**VM Extension**</span></span>
  
  * <span data-ttu-id="96e5f-130">Linux virtuális gép (IaaS) szoftver engedélyezéséhez és a konfigurációs automation Microsoft és a partnerei által készített összetevő beszúrása</span><span class="sxs-lookup"><span data-stu-id="96e5f-130">Inject component authored by Microsoft and Partners into Linux VM (IaaS) to enable software and configuration automation</span></span>
  * <span data-ttu-id="96e5f-131">Virtuálisgép-bővítmény hivatkozási megvalósítása a [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span><span class="sxs-lookup"><span data-stu-id="96e5f-131">VM Extension reference implementation on [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span></span>

## <a name="communication"></a><span data-ttu-id="96e5f-132">Kommunikáció</span><span class="sxs-lookup"><span data-stu-id="96e5f-132">Communication</span></span>
<span data-ttu-id="96e5f-133">Az ügynöknek a platformról információáramlás két csatornákon keresztül történnek:</span><span class="sxs-lookup"><span data-stu-id="96e5f-133">The information flow from the platform to the agent occurs via two channels:</span></span>

* <span data-ttu-id="96e5f-134">A rendszerindítás DVD csatolt IaaS telepítésekhez.</span><span class="sxs-lookup"><span data-stu-id="96e5f-134">A boot-time attached DVD for IaaS deployments.</span></span> <span data-ttu-id="96e5f-135">A DVD-t egy OVF-kompatibilis konfigurációs fájl, amely tartalmazza a tényleges SSH keypairs kivételével az összes kiépítési információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="96e5f-135">This DVD includes an OVF-compliant configuration file that includes all provisioning information other than the actual SSH keypairs.</span></span>
* <span data-ttu-id="96e5f-136">A TCP-végpont teszi ki a REST API-t használja a központi telepítés és a topológia konfigurációjával.</span><span class="sxs-lookup"><span data-stu-id="96e5f-136">A TCP endpoint exposing a REST API used to obtain deployment and topology configuration.</span></span>

## <a name="requirements"></a><span data-ttu-id="96e5f-137">Követelmények</span><span class="sxs-lookup"><span data-stu-id="96e5f-137">Requirements</span></span>
<span data-ttu-id="96e5f-138">A következő rendszerek lettek tesztelve, és ismert, hogy az Azure Linux ügynök használata:</span><span class="sxs-lookup"><span data-stu-id="96e5f-138">The following systems have been tested and are known to work with the Azure Linux Agent:</span></span>

> [!NOTE]
> <span data-ttu-id="96e5f-139">Ebben a listában eltérhet a Microsoft Azure platformon támogatott rendszerek hivatalos listáját itt: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span><span class="sxs-lookup"><span data-stu-id="96e5f-139">This list may differ from the official list of supported systems on the Microsoft Azure Platform, as described here: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span></span>
> 
> 

* <span data-ttu-id="96e5f-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="96e5f-140">CoreOS</span></span>
* <span data-ttu-id="96e5f-141">CentOS 6.3 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-141">CentOS 6.3+</span></span>
* <span data-ttu-id="96e5f-142">Red Hat Enterprise Linux 6.7 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-142">Red Hat Enterprise Linux 6.7+</span></span>
* <span data-ttu-id="96e5f-143">Debian 7.0 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-143">Debian 7.0+</span></span>
* <span data-ttu-id="96e5f-144">Ubuntu 12.04 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-144">Ubuntu 12.04+</span></span>
* <span data-ttu-id="96e5f-145">openSUSE 12.3 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-145">openSUSE 12.3+</span></span>
* <span data-ttu-id="96e5f-146">SLES 11 SP3 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-146">SLES 11 SP3+</span></span>
* <span data-ttu-id="96e5f-147">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-147">Oracle Linux 6.4+</span></span>

<span data-ttu-id="96e5f-148">Egyéb támogatott rendszerek:</span><span class="sxs-lookup"><span data-stu-id="96e5f-148">Other Supported Systems:</span></span>

* <span data-ttu-id="96e5f-149">Freebsd rendszerű 10 + (Azure Linux ügynök v2.0.10 +)</span><span class="sxs-lookup"><span data-stu-id="96e5f-149">FreeBSD 10+ (Azure Linux Agent v2.0.10+)</span></span>

<span data-ttu-id="96e5f-150">A Linux-ügynök megfelelő működéséhez néhány rendszer csomag függ:</span><span class="sxs-lookup"><span data-stu-id="96e5f-150">The Linux agent depends on some system packages in order to function properly:</span></span>

* <span data-ttu-id="96e5f-151">Python 2.6 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-151">Python 2.6+</span></span>
* <span data-ttu-id="96e5f-152">OpenSSL 1.0 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-152">OpenSSL 1.0+</span></span>
* <span data-ttu-id="96e5f-153">OpenSSH 5.3 +</span><span class="sxs-lookup"><span data-stu-id="96e5f-153">OpenSSH 5.3+</span></span>
* <span data-ttu-id="96e5f-154">Fájlrendszer segédprogramok: sfdisk, fdisk, mkfs, válogatottak</span><span class="sxs-lookup"><span data-stu-id="96e5f-154">Filesystem utilities: sfdisk, fdisk, mkfs, parted</span></span>
* <span data-ttu-id="96e5f-155">Jelszó-eszközök: chpasswd, sudo</span><span class="sxs-lookup"><span data-stu-id="96e5f-155">Password tools: chpasswd, sudo</span></span>
* <span data-ttu-id="96e5f-156">Eszközök feldolgozása szöveg: csökkentésének, grep</span><span class="sxs-lookup"><span data-stu-id="96e5f-156">Text processing tools: sed, grep</span></span>
* <span data-ttu-id="96e5f-157">A hálózati eszközök: ip-útvonal</span><span class="sxs-lookup"><span data-stu-id="96e5f-157">Network tools: ip-route</span></span>
* <span data-ttu-id="96e5f-158">Kernel támogatása UDF fájlrendszerek csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="96e5f-158">Kernel support for mounting UDF filesystems.</span></span>

## <a name="installation"></a><span data-ttu-id="96e5f-159">Telepítés</span><span class="sxs-lookup"><span data-stu-id="96e5f-159">Installation</span></span>
<span data-ttu-id="96e5f-160">A telepítési csomag tárházból egy RPM vagy DEB-csomag telepítését a előnyben részesített lehetőség, telepítése és az Azure Linux ügynök frissítése.</span><span class="sxs-lookup"><span data-stu-id="96e5f-160">Installation using an RPM or a DEB package from your distribution's package repository is the preferred method of installing and upgrading the Azure Linux Agent.</span></span> <span data-ttu-id="96e5f-161">Minden a [terjesztési szolgáltatók által támogatott](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) az Azure Linux-ügynök csomagja integrálja a képeket és tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="96e5f-161">All the [endorsed distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrate the Azure Linux agent package into their images and repositories.</span></span>

<span data-ttu-id="96e5f-162">A dokumentációban találja a [Azure Linux ügynök-tárház a Githubon](https://github.com/Azure/WALinuxAgent) a speciális telepítési lehetőségeket, például a forrás vagy egyéni helyek vagy az előtagok telepítése.</span><span class="sxs-lookup"><span data-stu-id="96e5f-162">Refer to the documentation in the [Azure Linux Agent repo on GitHub](https://github.com/Azure/WALinuxAgent) for advanced installation options, such as installing from source or to custom locations or prefixes.</span></span>

## <a name="command-line-options"></a><span data-ttu-id="96e5f-163">Parancssori kapcsolók</span><span class="sxs-lookup"><span data-stu-id="96e5f-163">Command Line Options</span></span>
### <a name="flags"></a><span data-ttu-id="96e5f-164">Jelzők</span><span class="sxs-lookup"><span data-stu-id="96e5f-164">Flags</span></span>
* <span data-ttu-id="96e5f-165">részletes: növelése a megadott parancs</span><span class="sxs-lookup"><span data-stu-id="96e5f-165">verbose: Increase verbosity of specified command</span></span>
* <span data-ttu-id="96e5f-166">kényszerített: hagyja ki az egyes parancsok interaktív megerősítése</span><span class="sxs-lookup"><span data-stu-id="96e5f-166">force: Skip interactive confirmation for some commands</span></span>

### <a name="commands"></a><span data-ttu-id="96e5f-167">Parancsok</span><span class="sxs-lookup"><span data-stu-id="96e5f-167">Commands</span></span>
* <span data-ttu-id="96e5f-168">Súgó: a támogatott parancsok és jelzők sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="96e5f-168">help: Lists the supported commands and flags.</span></span>
* <span data-ttu-id="96e5f-169">deprovision: próbálja meg törölni a rendszer, és lehetővé teszi a megfelelő ismételt üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="96e5f-169">deprovision: Attempt to clean the system and make it suitable for re-provisioning.</span></span> <span data-ttu-id="96e5f-170">Ez a művelet törli a következő:</span><span class="sxs-lookup"><span data-stu-id="96e5f-170">This operation deleted the following:</span></span>
  
  * <span data-ttu-id="96e5f-171">Az összes SSH állomáskulcsai (ha Provisioning.RegenerateSshHostKeyPair "y", a konfigurációs fájlban)</span><span class="sxs-lookup"><span data-stu-id="96e5f-171">All SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in the configuration file)</span></span>
  * <span data-ttu-id="96e5f-172">A /etc/resolv.conf névkiszolgáló-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="96e5f-172">Nameserver configuration in /etc/resolv.conf</span></span>
  * <span data-ttu-id="96e5f-173">Gyökér szintű jelszavát a /etc/shadow (ha Provisioning.DeleteRootPassword "y", a konfigurációs fájlban)</span><span class="sxs-lookup"><span data-stu-id="96e5f-173">Root password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in the configuration file)</span></span>
  * <span data-ttu-id="96e5f-174">Gyorsítótárazott DHCP-ügyfél bérletek</span><span class="sxs-lookup"><span data-stu-id="96e5f-174">Cached DHCP client leases</span></span>
  * <span data-ttu-id="96e5f-175">A localhost.localdomain állomásnév visszaállítása</span><span class="sxs-lookup"><span data-stu-id="96e5f-175">Resets host name to localhost.localdomain</span></span>

> [!WARNING]
> <span data-ttu-id="96e5f-176">Megszüntetés nem garantálható, hogy a lemezkép minden a bizalmas adatok törlődik, és a megfelelő terjesztési.</span><span class="sxs-lookup"><span data-stu-id="96e5f-176">Deprovisioning does not guarantee that the image is cleared of all sensitive information and suitable for redistribution.</span></span>
> 
> 

* <span data-ttu-id="96e5f-177">deprovision + felhasználói: hajt végre, minden elemet - deprovision (fent) is törli a legutóbbi kiépített felhasználói fiókot (/var/lib/waagent nyert) és az adatok.</span><span class="sxs-lookup"><span data-stu-id="96e5f-177">deprovision+user: Performs everything under -deprovision (above) and also deletes the last provisioned user account (obtained from /var/lib/waagent) and associated data.</span></span> <span data-ttu-id="96e5f-178">Ez a paraméter esetén a megszüntetést egy olyanra, amely korábban az Azure-on kiépítés volt, előfordulhat, hogy lehet rögzíteni és újra felhasználni.</span><span class="sxs-lookup"><span data-stu-id="96e5f-178">This parameter is when de-provisioning an image that was previously provisioning on Azure so it may be captured and re-used.</span></span>
* <span data-ttu-id="96e5f-179">verzió: waagent-verzió</span><span class="sxs-lookup"><span data-stu-id="96e5f-179">version: Displays the version of waagent</span></span>
* <span data-ttu-id="96e5f-180">serialconsole: ttyS0 megjelölni LÁRVAJÁRAT konfigurálása (az első soros port) a rendszerindító konzollal.</span><span class="sxs-lookup"><span data-stu-id="96e5f-180">serialconsole: Configures GRUB to mark ttyS0 (the first serial port) as the boot console.</span></span> <span data-ttu-id="96e5f-181">Ez biztosítja, hogy kernel rendszerindítási naplókat a soros port küldött és elérhetővé tenni a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="96e5f-181">This ensures that kernel bootup logs are sent to the serial port and made available for debugging.</span></span>
* <span data-ttu-id="96e5f-182">démon: waagent futtató démon kezeléséhez a platformon.</span><span class="sxs-lookup"><span data-stu-id="96e5f-182">daemon: Run waagent as a daemon to manage interaction with the platform.</span></span> <span data-ttu-id="96e5f-183">Ennek az argumentumnak a waagent a waagent init parancsfájl van megadva.</span><span class="sxs-lookup"><span data-stu-id="96e5f-183">This argument is specified to waagent in the waagent init script.</span></span>
* <span data-ttu-id="96e5f-184">Start: háttérfolyamatként waagent futtatása</span><span class="sxs-lookup"><span data-stu-id="96e5f-184">start: Run waagent as a background process</span></span>

## <a name="configuration"></a><span data-ttu-id="96e5f-185">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="96e5f-185">Configuration</span></span>
<span data-ttu-id="96e5f-186">Egy konfigurációs fájl (/ etc/waagent.conf) waagent műveleteit szabályozza.</span><span class="sxs-lookup"><span data-stu-id="96e5f-186">A configuration file (/etc/waagent.conf) controls the actions of waagent.</span></span> <span data-ttu-id="96e5f-187">Alább látható egy példa konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="96e5f-187">A sample configuration file is shown below:</span></span>

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

<span data-ttu-id="96e5f-188">A különböző konfigurációs beállításait ismerteti részletesen.</span><span class="sxs-lookup"><span data-stu-id="96e5f-188">The various configuration options are described in detail below.</span></span> <span data-ttu-id="96e5f-189">Beállítási lehetőségek állnak a három típusa létezik; Logikai érték, String vagy Integer.</span><span class="sxs-lookup"><span data-stu-id="96e5f-189">Configuration options are of three types; Boolean, String or Integer.</span></span> <span data-ttu-id="96e5f-190">"Y" vagy "n" logikai konfigurációs beállításokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="96e5f-190">The Boolean configuration options can be specified as "y" or "n".</span></span> <span data-ttu-id="96e5f-191">A speciális kulcsszó "None" néhány karakterlánc típusú konfigurációs elemet részletes, az alábbi használható.</span><span class="sxs-lookup"><span data-stu-id="96e5f-191">The special keyword "None" may be used for some string type configuration entries as detailed below.</span></span>

<span data-ttu-id="96e5f-192">**Provisioning.Enabled:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-192">**Provisioning.Enabled:**</span></span>  
<span data-ttu-id="96e5f-193">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-193">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-194">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="96e5f-194">Default: y</span></span>

<span data-ttu-id="96e5f-195">A felhasználó engedélyezhető vagy tiltható le az ügynököt a kiépítési funkciói.</span><span class="sxs-lookup"><span data-stu-id="96e5f-195">This allows the user to enable or disable the provisioning functionality in the agent.</span></span> <span data-ttu-id="96e5f-196">Érvényes értékek: "y" vagy "n".</span><span class="sxs-lookup"><span data-stu-id="96e5f-196">Valid values are "y" or "n".</span></span> <span data-ttu-id="96e5f-197">Kiépítés le van tiltva, ha a kép SSH-állomás és a felhasználói kulcsok megmaradnak, és a kiépítés API Azure-ban megadott minden beállítás figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="96e5f-197">If provisioning is disabled, SSH host and user keys in the image are preserved and any configuration specified in the Azure provisioning API is ignored.</span></span>

> [!NOTE]
> <span data-ttu-id="96e5f-198">A `Provisioning.Enabled` "n" Ubuntu felhő lemezképeket inicializálás felhőben történő üzembe helyezéséhez használjon a paraméter alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="96e5f-198">The `Provisioning.Enabled` parameter defaults to "n" on Ubuntu Cloud Images that use cloud-init for provisioning.</span></span>
> 
> 

<span data-ttu-id="96e5f-199">**Provisioning.DeleteRootPassword:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-199">**Provisioning.DeleteRootPassword:**</span></span>  
<span data-ttu-id="96e5f-200">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-200">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-201">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="96e5f-201">Default: n</span></span>

<span data-ttu-id="96e5f-202">Ha be van állítva, a/etc/árnyékmásolat fájlban gyökér szintű jelszavát a telepítési folyamat során elvész.</span><span class="sxs-lookup"><span data-stu-id="96e5f-202">If set, the root password in the /etc/shadow file is erased during the provisioning process.</span></span>

<span data-ttu-id="96e5f-203">**Provisioning.RegenerateSshHostKeyPair:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-203">**Provisioning.RegenerateSshHostKeyPair:**</span></span>  
<span data-ttu-id="96e5f-204">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-204">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-205">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="96e5f-205">Default: y</span></span>

<span data-ttu-id="96e5f-206">Ha be van állítva, az összes SSH állomás kulcspárok (ecdsa, dsa és rsa) a rendszer törli az/etc/ssh/a telepítési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="96e5f-206">If set, all SSH host key pairs (ecdsa, dsa and rsa) are deleted during the provisioning process from /etc/ssh/.</span></span> <span data-ttu-id="96e5f-207">És egy egyetlen új kulcspár.</span><span class="sxs-lookup"><span data-stu-id="96e5f-207">And a single fresh key pair is generated.</span></span>

<span data-ttu-id="96e5f-208">A friss kulcspár titkosítási típus a Provisioning.SshHostKeyPairType bejegyzés által konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="96e5f-208">The encryption type for the fresh key pair is configurable by the Provisioning.SshHostKeyPairType entry.</span></span> <span data-ttu-id="96e5f-209">Vegye figyelembe, hogy néhány terjesztési hozza létre SSH-kulcspár bármely hiányzó titkosítási típusok, az SSH démon (például biztonsági újraindításkor) újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="96e5f-209">Please note that some distributions will re-create SSH key pairs for any missing encryption types when the SSH daemon is restarted (for example, upon a reboot).</span></span>

<span data-ttu-id="96e5f-210">**Provisioning.SshHostKeyPairType:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-210">**Provisioning.SshHostKeyPairType:**</span></span>  
<span data-ttu-id="96e5f-211">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-211">Type: String</span></span>  
<span data-ttu-id="96e5f-212">Alapértelmezett: rsa</span><span class="sxs-lookup"><span data-stu-id="96e5f-212">Default: rsa</span></span>

<span data-ttu-id="96e5f-213">Ez a virtuális gépen az SSH démon által támogatott titkosítási algoritmus típust állítható be.</span><span class="sxs-lookup"><span data-stu-id="96e5f-213">This can be set to an encryption algorithm type that is supported by the SSH daemon on the virtual machine.</span></span> <span data-ttu-id="96e5f-214">A általában támogatott értékek: "rsa", "dsa" és "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="96e5f-214">The typically supported values are "rsa", "dsa" and "ecdsa".</span></span> <span data-ttu-id="96e5f-215">Vegye figyelembe, hogy a "putty.exe" a Windows nem támogatja "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="96e5f-215">Note that "putty.exe" on Windows does not support "ecdsa".</span></span> <span data-ttu-id="96e5f-216">Úgy Ha azt tervezi, a Windows putty.exe segítségével csatlakozzon a Linux-környezethez, használja az "rsa" vagy "dsa".</span><span class="sxs-lookup"><span data-stu-id="96e5f-216">So, if you intend to use putty.exe on Windows to connect to a Linux deployment, please use "rsa" or "dsa".</span></span>

<span data-ttu-id="96e5f-217">**Provisioning.MonitorHostName:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-217">**Provisioning.MonitorHostName:**</span></span>  
<span data-ttu-id="96e5f-218">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-218">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-219">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="96e5f-219">Default: y</span></span>

<span data-ttu-id="96e5f-220">Ha beállításához waagent ellenőrzi a Linux virtuális gép állomásnevét módosítások (mivel a parancs által visszaadott "állomásnév") és automatikus frissítése a kép, hogy tükrözze a hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="96e5f-220">If set, waagent will monitor the Linux virtual machine for hostname changes (as returned by the "hostname" command) and automatically update the networking configuration in the image to reflect the change.</span></span> <span data-ttu-id="96e5f-221">Ahhoz, hogy a kiszolgálónév-változás leküldése a DNS-kiszolgálók, hálózat a virtuális gép újraindul.</span><span class="sxs-lookup"><span data-stu-id="96e5f-221">In order to push the name change to the DNS servers, networking will be restarted in the virtual machine.</span></span> <span data-ttu-id="96e5f-222">Ekkor röviden a Internet kapcsolat megszakadása.</span><span class="sxs-lookup"><span data-stu-id="96e5f-222">This will result in brief loss of Internet connectivity.</span></span>

<span data-ttu-id="96e5f-223">**Provisioning.DecodeCustomData**</span><span class="sxs-lookup"><span data-stu-id="96e5f-223">**Provisioning.DecodeCustomData**</span></span>  
<span data-ttu-id="96e5f-224">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-224">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-225">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="96e5f-225">Default: n</span></span>

<span data-ttu-id="96e5f-226">Ha beállításához waagent meg fog dekódolni a Base64 kódolású anyag CustomData.</span><span class="sxs-lookup"><span data-stu-id="96e5f-226">If set, waagent will decode CustomData from Base64.</span></span>

<span data-ttu-id="96e5f-227">**Provisioning.ExecuteCustomData**</span><span class="sxs-lookup"><span data-stu-id="96e5f-227">**Provisioning.ExecuteCustomData**</span></span>  
<span data-ttu-id="96e5f-228">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-228">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-229">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="96e5f-229">Default: n</span></span>

<span data-ttu-id="96e5f-230">Ha beállításához waagent végrehajtja a CustomData kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="96e5f-230">If set, waagent will execute CustomData after provisioning.</span></span>

<span data-ttu-id="96e5f-231">**Provisioning.PasswordCryptId**</span><span class="sxs-lookup"><span data-stu-id="96e5f-231">**Provisioning.PasswordCryptId**</span></span>  
<span data-ttu-id="96e5f-232">Típus: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-232">Type:String</span></span>  
<span data-ttu-id="96e5f-233">Alapértelmezett: 6</span><span class="sxs-lookup"><span data-stu-id="96e5f-233">Default:6</span></span>

<span data-ttu-id="96e5f-234">Jelszókivonat létrehozásakor használt titkosítási algoritmus.</span><span class="sxs-lookup"><span data-stu-id="96e5f-234">Algorithm used by crypt when generating password hash.</span></span>  
 <span data-ttu-id="96e5f-235">1 - MD5</span><span class="sxs-lookup"><span data-stu-id="96e5f-235">1 - MD5</span></span>  
 <span data-ttu-id="96e5f-236">2a – Blowfish</span><span class="sxs-lookup"><span data-stu-id="96e5f-236">2a - Blowfish</span></span>  
 <span data-ttu-id="96e5f-237">5 - SHA-256</span><span class="sxs-lookup"><span data-stu-id="96e5f-237">5 - SHA-256</span></span>  
 <span data-ttu-id="96e5f-238">6 - SHA-512</span><span class="sxs-lookup"><span data-stu-id="96e5f-238">6 - SHA-512</span></span>  

<span data-ttu-id="96e5f-239">**Provisioning.PasswordCryptSaltLength**</span><span class="sxs-lookup"><span data-stu-id="96e5f-239">**Provisioning.PasswordCryptSaltLength**</span></span>  
<span data-ttu-id="96e5f-240">Típus: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-240">Type:String</span></span>  
<span data-ttu-id="96e5f-241">Alapértelmezett: 10</span><span class="sxs-lookup"><span data-stu-id="96e5f-241">Default:10</span></span>

<span data-ttu-id="96e5f-242">Jelszókivonat létrehozásához használt véletlenszerű védőérték hosszát.</span><span class="sxs-lookup"><span data-stu-id="96e5f-242">Length of random salt used when generating password hash.</span></span>

<span data-ttu-id="96e5f-243">**ResourceDisk.Format:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-243">**ResourceDisk.Format:**</span></span>  
<span data-ttu-id="96e5f-244">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-244">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-245">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="96e5f-245">Default: y</span></span>

<span data-ttu-id="96e5f-246">Ha beállítása, a erőforrás lemez a platform által biztosított fog kell formázni, waagent által csatlakoztatott, ha a fájlrendszer típusát kérte a felhasználónak a "ResourceDisk.Filesystem" nem "ntfs".</span><span class="sxs-lookup"><span data-stu-id="96e5f-246">If set, the resource disk provided by the platform will be formatted and mounted by waagent if the filesystem type requested by the user in "ResourceDisk.Filesystem" is anything other than "ntfs".</span></span> <span data-ttu-id="96e5f-247">Egy olyan partíciót, Linux (83) típusú lesz elérhető a lemezen.</span><span class="sxs-lookup"><span data-stu-id="96e5f-247">A single partition of type Linux (83) will be made available on the disk.</span></span> <span data-ttu-id="96e5f-248">Vegye figyelembe, hogy ez a partíció nem lesz formázva Ha sikeresen csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="96e5f-248">Note that this partition will not be formatted if it can be successfully mounted.</span></span>

<span data-ttu-id="96e5f-249">**ResourceDisk.Filesystem:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-249">**ResourceDisk.Filesystem:**</span></span>  
<span data-ttu-id="96e5f-250">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-250">Type: String</span></span>  
<span data-ttu-id="96e5f-251">Alapértelmezett: ext4</span><span class="sxs-lookup"><span data-stu-id="96e5f-251">Default: ext4</span></span>

<span data-ttu-id="96e5f-252">Azt határozza meg az erőforrás-lemez a fájlrendszer típusát.</span><span class="sxs-lookup"><span data-stu-id="96e5f-252">This specifies the filesystem type for the resource disk.</span></span> <span data-ttu-id="96e5f-253">A Linux-disztribúció által támogatott értékek eltérők lehetnek.</span><span class="sxs-lookup"><span data-stu-id="96e5f-253">Supported values vary by Linux distribution.</span></span> <span data-ttu-id="96e5f-254">Ha a karakterlánc X, majd mkfs. X a Linux-lemezkép jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="96e5f-254">If the string is X, then mkfs.X should be present on the Linux image.</span></span> <span data-ttu-id="96e5f-255">SLES 11 lemezképeket általában használjon "ext3".</span><span class="sxs-lookup"><span data-stu-id="96e5f-255">SLES 11 images should typically use 'ext3'.</span></span> <span data-ttu-id="96e5f-256">Freebsd rendszerű lemezképek itt "ufs2" kell használni.</span><span class="sxs-lookup"><span data-stu-id="96e5f-256">FreeBSD images should use 'ufs2' here.</span></span>

<span data-ttu-id="96e5f-257">**ResourceDisk.MountPoint:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-257">**ResourceDisk.MountPoint:**</span></span>  
<span data-ttu-id="96e5f-258">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-258">Type: String</span></span>  
<span data-ttu-id="96e5f-259">Alapértelmezett: / mnt és az erőforrások</span><span class="sxs-lookup"><span data-stu-id="96e5f-259">Default: /mnt/resource</span></span> 

<span data-ttu-id="96e5f-260">Azt határozza meg az elérési utat, amelyen az erőforrás-lemez csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="96e5f-260">This specifies the path at which the resource disk is mounted.</span></span> <span data-ttu-id="96e5f-261">Vegye figyelembe, hogy az erőforrás-lemez egy *ideiglenes* lemezre, és előfordulhat, hogy szerepelnek, ha a virtuális gép van platformelőfizetés.</span><span class="sxs-lookup"><span data-stu-id="96e5f-261">Note that the resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span>

<span data-ttu-id="96e5f-262">**ResourceDisk.MountOptions**</span><span class="sxs-lookup"><span data-stu-id="96e5f-262">**ResourceDisk.MountOptions**</span></span>  
<span data-ttu-id="96e5f-263">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-263">Type: String</span></span>  
<span data-ttu-id="96e5f-264">Alapértelmezett: nincs</span><span class="sxs-lookup"><span data-stu-id="96e5f-264">Default: None</span></span>

<span data-ttu-id="96e5f-265">A mount -o parancs átadandó lemez csatlakoztatási beállításait adja meg.</span><span class="sxs-lookup"><span data-stu-id="96e5f-265">Specifies disk mount options to be passed to the mount -o command.</span></span> <span data-ttu-id="96e5f-266">Például ez az értékek, vesszővel elválasztott listája.</span><span class="sxs-lookup"><span data-stu-id="96e5f-266">This is a comma separated list of values, ex.</span></span> <span data-ttu-id="96e5f-267">"nodev, nosuid".</span><span class="sxs-lookup"><span data-stu-id="96e5f-267">'nodev,nosuid'.</span></span> <span data-ttu-id="96e5f-268">Tekintse meg a részletes mount(8).</span><span class="sxs-lookup"><span data-stu-id="96e5f-268">See mount(8) for details.</span></span>

<span data-ttu-id="96e5f-269">**ResourceDisk.EnableSwap:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-269">**ResourceDisk.EnableSwap:**</span></span>  
<span data-ttu-id="96e5f-270">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-270">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-271">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="96e5f-271">Default: n</span></span>

<span data-ttu-id="96e5f-272">Ha beállításához lapozófájl (/ swapfile), az erőforrás lemezen létrehozni, és a rendszer lapozóterület hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="96e5f-272">If set, a swap file (/swapfile) is created on the resource disk and added to the system swap space.</span></span>

<span data-ttu-id="96e5f-273">**ResourceDisk.SwapSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-273">**ResourceDisk.SwapSizeMB:**</span></span>  
<span data-ttu-id="96e5f-274">Típus: egész szám</span><span class="sxs-lookup"><span data-stu-id="96e5f-274">Type: Integer</span></span>  
<span data-ttu-id="96e5f-275">Alapértelmezett: 0</span><span class="sxs-lookup"><span data-stu-id="96e5f-275">Default: 0</span></span>

<span data-ttu-id="96e5f-276">A lapozófájl mérete (MB) mérete.</span><span class="sxs-lookup"><span data-stu-id="96e5f-276">The size of the swap file in megabytes.</span></span>

<span data-ttu-id="96e5f-277">**Logs.Verbose:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-277">**Logs.Verbose:**</span></span>  
<span data-ttu-id="96e5f-278">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-278">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-279">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="96e5f-279">Default: n</span></span>

<span data-ttu-id="96e5f-280">Ha be van állítva, napló részletességi súlyozott van.</span><span class="sxs-lookup"><span data-stu-id="96e5f-280">If set, log verbosity is boosted.</span></span> <span data-ttu-id="96e5f-281">Waagent /var/log/waagent.log jelentkezik, és kihasználja a rendszer logrotate elforgatása naplókat.</span><span class="sxs-lookup"><span data-stu-id="96e5f-281">Waagent logs to /var/log/waagent.log and leverages the system logrotate functionality to rotate logs.</span></span>

<span data-ttu-id="96e5f-282">**AZ OPERÁCIÓS RENDSZER. EnableRDMA**</span><span class="sxs-lookup"><span data-stu-id="96e5f-282">**OS.EnableRDMA**</span></span>  
<span data-ttu-id="96e5f-283">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="96e5f-283">Type: Boolean</span></span>  
<span data-ttu-id="96e5f-284">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="96e5f-284">Default: n</span></span>

<span data-ttu-id="96e5f-285">Ha állítsa be, az ügynök megpróbálja telepíteni, és töltsön be egy RDMA egy rendszermag-illesztőprogramot, amely ugyanolyan verziójúak, mint az alapul szolgáló hardverben belső vezérlőprogramját.</span><span class="sxs-lookup"><span data-stu-id="96e5f-285">If set, the agent will attempt to install and then load an RDMA kernel driver that matches the version of the firmware on the underlying hardware.</span></span>

<span data-ttu-id="96e5f-286">**AZ OPERÁCIÓS RENDSZER. RootDeviceScsiTimeout:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-286">**OS.RootDeviceScsiTimeout:**</span></span>  
<span data-ttu-id="96e5f-287">Típus: egész szám</span><span class="sxs-lookup"><span data-stu-id="96e5f-287">Type: Integer</span></span>  
<span data-ttu-id="96e5f-288">Alapértelmezett: 300</span><span class="sxs-lookup"><span data-stu-id="96e5f-288">Default: 300</span></span>

<span data-ttu-id="96e5f-289">Ez konfigurálja az SCSI-időtúllépés másodpercben az operációs rendszer lemez- és meghajtókon.</span><span class="sxs-lookup"><span data-stu-id="96e5f-289">This configures the SCSI timeout in seconds on the OS disk and data drives.</span></span> <span data-ttu-id="96e5f-290">Ha nincs beállítva, a rendszer alapértelmezett értékeket használják.</span><span class="sxs-lookup"><span data-stu-id="96e5f-290">If not set, the system defaults are used.</span></span>

<span data-ttu-id="96e5f-291">**AZ OPERÁCIÓS RENDSZER. OpensslPath:**</span><span class="sxs-lookup"><span data-stu-id="96e5f-291">**OS.OpensslPath:**</span></span>  
<span data-ttu-id="96e5f-292">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-292">Type: String</span></span>  
<span data-ttu-id="96e5f-293">Alapértelmezett: nincs</span><span class="sxs-lookup"><span data-stu-id="96e5f-293">Default: None</span></span>

<span data-ttu-id="96e5f-294">Ez egy alternatív elérési utat a titkosítási műveletek használandó bináris openssl megadását is használható.</span><span class="sxs-lookup"><span data-stu-id="96e5f-294">This can be used to specify an alternate path for the openssl binary to use for cryptographic operations.</span></span>

<span data-ttu-id="96e5f-295">**HttpProxy.Host, HttpProxy.Port**</span><span class="sxs-lookup"><span data-stu-id="96e5f-295">**HttpProxy.Host, HttpProxy.Port**</span></span>  
<span data-ttu-id="96e5f-296">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="96e5f-296">Type: String</span></span>  
<span data-ttu-id="96e5f-297">Alapértelmezett: nincs</span><span class="sxs-lookup"><span data-stu-id="96e5f-297">Default: None</span></span>

<span data-ttu-id="96e5f-298">Ha állítsa be, az ügynök fogja használni a proxykiszolgálót az internet eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="96e5f-298">If set, the agent will use this proxy server to access the internet.</span></span> 

## <a name="ubuntu-cloud-images"></a><span data-ttu-id="96e5f-299">Ubuntu felhő lemezképek</span><span class="sxs-lookup"><span data-stu-id="96e5f-299">Ubuntu Cloud Images</span></span>
<span data-ttu-id="96e5f-300">Vegye figyelembe, hogy Ubuntu felhő lemezképek használata [felhő inicializálás](https://launchpad.net/ubuntu/+source/cloud-init) számos konfigurációs feladatok végrehajtását, amelyek egyébként volna kezeli az Azure Linux ügynök.</span><span class="sxs-lookup"><span data-stu-id="96e5f-300">Note that Ubuntu Cloud Images utilize [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) to perform many configuration tasks that would otherwise be managed by the Azure Linux Agent.</span></span>  <span data-ttu-id="96e5f-301">Vegye figyelembe a következő eltérésekkel:</span><span class="sxs-lookup"><span data-stu-id="96e5f-301">Please note the following differences:</span></span>

* <span data-ttu-id="96e5f-302">**Provisioning.Enabled** az alapértelmezett érték "n" a telepítési feladatok végrehajtásához használja a felhő inicializálás Ubuntu felhő lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="96e5f-302">**Provisioning.Enabled** defaults to "n" on Ubuntu Cloud Images that use cloud-init to perform provisioning tasks.</span></span>
* <span data-ttu-id="96e5f-303">A következő konfigurációs paraméterek nem befolyásolják a felhő inicializálás segítségével kezelheti az erőforrás-lemez, és a lapozófájl Ubuntu felhő lemezképek:</span><span class="sxs-lookup"><span data-stu-id="96e5f-303">The following configuration parameters have no effect on Ubuntu Cloud Images that use cloud-init to manage the resource disk and swap space:</span></span>
  
  * <span data-ttu-id="96e5f-304">**ResourceDisk.Format**</span><span class="sxs-lookup"><span data-stu-id="96e5f-304">**ResourceDisk.Format**</span></span>
  * <span data-ttu-id="96e5f-305">**ResourceDisk.Filesystem**</span><span class="sxs-lookup"><span data-stu-id="96e5f-305">**ResourceDisk.Filesystem**</span></span>
  * <span data-ttu-id="96e5f-306">**ResourceDisk.MountPoint**</span><span class="sxs-lookup"><span data-stu-id="96e5f-306">**ResourceDisk.MountPoint**</span></span>
  * <span data-ttu-id="96e5f-307">**ResourceDisk.EnableSwap**</span><span class="sxs-lookup"><span data-stu-id="96e5f-307">**ResourceDisk.EnableSwap**</span></span>
  * <span data-ttu-id="96e5f-308">**ResourceDisk.SwapSizeMB**</span><span class="sxs-lookup"><span data-stu-id="96e5f-308">**ResourceDisk.SwapSizeMB**</span></span>
* <span data-ttu-id="96e5f-309">Ellenőrizze a lemez erőforrás csatlakoztatási pontjának konfigurálásával és a lapozófájl Ubuntu felhő képek a kiépítés során a következőket:</span><span class="sxs-lookup"><span data-stu-id="96e5f-309">Please see the following resources to configure the resource disk mount point and swap space on Ubuntu Cloud Images during provisioning:</span></span>
  
  * [<span data-ttu-id="96e5f-310">Ubuntu Wiki: Lapozófájl-kapacitás-partíciók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96e5f-310">Ubuntu Wiki: Configure Swap Partitions</span></span>](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [<span data-ttu-id="96e5f-311">Egyéni adatok hogy Azure virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="96e5f-311">Injecting Custom Data into an Azure Virtual Machine</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

