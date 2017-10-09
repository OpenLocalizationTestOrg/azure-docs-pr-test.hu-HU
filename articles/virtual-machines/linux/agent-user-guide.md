---
title: "aaaAzure Linux virtuális gép ügynök áttekintése |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és a Linux-ügynök (waagent) toomanage konfigurálja a virtuális gép Azure Fabric Controller interakció."
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
ms.openlocfilehash: 4e08c84d9205f4db7aae6fd1568ec1f15fba395c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-and-using-hello-azure-linux-agent"></a><span data-ttu-id="58708-103">Megismeréséhez és használatához hello Azure Linux ügynök</span><span class="sxs-lookup"><span data-stu-id="58708-103">Understanding and using hello Azure Linux Agent</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a><span data-ttu-id="58708-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="58708-104">Introduction</span></span>
<span data-ttu-id="58708-105">hello Microsoft Azure Linux ügynök (waagent) a Linux és freebsd rendszerű kiépítés és hello Azure Fabric Controller interakcióba VM kezeli.</span><span class="sxs-lookup"><span data-stu-id="58708-105">hello Microsoft Azure Linux Agent (waagent) manages Linux & FreeBSD provisioning, and VM interaction with hello Azure Fabric Controller.</span></span> <span data-ttu-id="58708-106">A következő funkció a Linux és freebsd rendszerű infrastruktúra-szolgáltatási központi telepítések hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="58708-106">It provides hello following functionality for Linux and FreeBSD IaaS deployments:</span></span>

> [!NOTE]
> <span data-ttu-id="58708-107">Lásd: hello Azure Linux ügynök [információs](https://github.com/Azure/WALinuxAgent/blob/master/README.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="58708-107">See hello Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) for additional details.</span></span>
> 
> 

* <span data-ttu-id="58708-108">**Kép kiépítése**</span><span class="sxs-lookup"><span data-stu-id="58708-108">**Image Provisioning**</span></span>
  
  * <span data-ttu-id="58708-109">A felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="58708-109">Creation of a user account</span></span>
  * <span data-ttu-id="58708-110">SSH hitelesítési típus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58708-110">Configuring SSH authentication types</span></span>
  * <span data-ttu-id="58708-111">Az SSH nyilvános kulcsok és kulcspárok központi telepítését</span><span class="sxs-lookup"><span data-stu-id="58708-111">Deployment of SSH public keys and key pairs</span></span>
  * <span data-ttu-id="58708-112">Beállítás hello gazdagép neve</span><span class="sxs-lookup"><span data-stu-id="58708-112">Setting hello host name</span></span>
  * <span data-ttu-id="58708-113">Közzétételi hello állomás neve toohello platform DNS</span><span class="sxs-lookup"><span data-stu-id="58708-113">Publishing hello host name toohello platform DNS</span></span>
  * <span data-ttu-id="58708-114">Jelentéskészítési SSH állomás kulcs ujjlenyomat toohello platform</span><span class="sxs-lookup"><span data-stu-id="58708-114">Reporting SSH host key fingerprint toohello platform</span></span>
  * <span data-ttu-id="58708-115">Erőforrás Lemezkezelés</span><span class="sxs-lookup"><span data-stu-id="58708-115">Resource Disk Management</span></span>
  * <span data-ttu-id="58708-116">Formázás és hello erőforrás lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="58708-116">Formatting and mounting hello resource disk</span></span>
  * <span data-ttu-id="58708-117">Lapozófájl konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58708-117">Configuring swap space</span></span>
* <span data-ttu-id="58708-118">**Hálózat**</span><span class="sxs-lookup"><span data-stu-id="58708-118">**Networking**</span></span>
  
  * <span data-ttu-id="58708-119">Kezeli az útvonalak tooimprove kompatibilitási platform DHCP-kiszolgálókkal</span><span class="sxs-lookup"><span data-stu-id="58708-119">Manages routes tooimprove compatibility with platform DHCP servers</span></span>
  * <span data-ttu-id="58708-120">Biztosítja a hello stabilitásának hello a hálózati adapter neve</span><span class="sxs-lookup"><span data-stu-id="58708-120">Ensures hello stability of hello network interface name</span></span>
* <span data-ttu-id="58708-121">**Kernel**</span><span class="sxs-lookup"><span data-stu-id="58708-121">**Kernel**</span></span>
  
  * <span data-ttu-id="58708-122">Konfigurálja a virtuális NUMA (letiltja a kernel < 2.6.37)</span><span class="sxs-lookup"><span data-stu-id="58708-122">Configures virtual NUMA (disable for kernel <2.6.37)</span></span>
  * <span data-ttu-id="58708-123">Hyper-V entrópia /dev/random a felhasználva</span><span class="sxs-lookup"><span data-stu-id="58708-123">Consumes Hyper-V entropy for /dev/random</span></span>
  * <span data-ttu-id="58708-124">Konfigurálja az SCSI-időtúllépések hello legfelső szintű eszköz (amely lehet távoli)</span><span class="sxs-lookup"><span data-stu-id="58708-124">Configures SCSI timeouts for hello root device (which could be remote)</span></span>
* <span data-ttu-id="58708-125">**Diagnosztika**</span><span class="sxs-lookup"><span data-stu-id="58708-125">**Diagnostics**</span></span>
  
  * <span data-ttu-id="58708-126">Konzol átirányítási toohello soros port</span><span class="sxs-lookup"><span data-stu-id="58708-126">Console redirection toohello serial port</span></span>
* <span data-ttu-id="58708-127">**Az SCVMM központi telepítések**</span><span class="sxs-lookup"><span data-stu-id="58708-127">**SCVMM Deployments**</span></span>
  
  * <span data-ttu-id="58708-128">Észleli és hello VMM-ügynök Linux betöltéséhez, ha a System Center Virtual Machine Manager 2012 R2 környezetben futó</span><span class="sxs-lookup"><span data-stu-id="58708-128">Detects and bootstraps hello VMM agent for Linux when running in a System Center Virtual Machine Manager 2012 R2 environment</span></span>
* <span data-ttu-id="58708-129">**Virtuálisgép-bővítmény**</span><span class="sxs-lookup"><span data-stu-id="58708-129">**VM Extension**</span></span>
  
  * <span data-ttu-id="58708-130">A Linux virtuális gép (IaaS) tooenable szoftver- és konfigurációs automation Microsoft és a partnerei által készített összetevő beszúrása</span><span class="sxs-lookup"><span data-stu-id="58708-130">Inject component authored by Microsoft and Partners into Linux VM (IaaS) tooenable software and configuration automation</span></span>
  * <span data-ttu-id="58708-131">Virtuálisgép-bővítmény hivatkozási megvalósítása a [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span><span class="sxs-lookup"><span data-stu-id="58708-131">VM Extension reference implementation on [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span></span>

## <a name="communication"></a><span data-ttu-id="58708-132">Kommunikáció</span><span class="sxs-lookup"><span data-stu-id="58708-132">Communication</span></span>
<span data-ttu-id="58708-133">hello információáramlás hello platform toohello ügynöktől két csatornákon keresztül történnek:</span><span class="sxs-lookup"><span data-stu-id="58708-133">hello information flow from hello platform toohello agent occurs via two channels:</span></span>

* <span data-ttu-id="58708-134">A rendszerindítás DVD csatolt IaaS telepítésekhez.</span><span class="sxs-lookup"><span data-stu-id="58708-134">A boot-time attached DVD for IaaS deployments.</span></span> <span data-ttu-id="58708-135">A DVD-t egy OVF-kompatibilis hello tényleges SSH keypairs kivételével az összes kiépítési információkat tartalmazó konfigurációs fájlt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="58708-135">This DVD includes an OVF-compliant configuration file that includes all provisioning information other than hello actual SSH keypairs.</span></span>
* <span data-ttu-id="58708-136">A TCP-végpont teszi ki a REST API használt tooobtain központi telepítése és topológia konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="58708-136">A TCP endpoint exposing a REST API used tooobtain deployment and topology configuration.</span></span>

## <a name="requirements"></a><span data-ttu-id="58708-137">Követelmények</span><span class="sxs-lookup"><span data-stu-id="58708-137">Requirements</span></span>
<span data-ttu-id="58708-138">hello következő rendszerek lettek tesztelve, és a hello Azure Linux ügynök toowork ismert:</span><span class="sxs-lookup"><span data-stu-id="58708-138">hello following systems have been tested and are known toowork with hello Azure Linux Agent:</span></span>

> [!NOTE]
> <span data-ttu-id="58708-139">Ez a lista eltérhet a hello hivatalos a Microsoft Azure Platform hello támogatott rendszerek listáját itt leírt módon: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span><span class="sxs-lookup"><span data-stu-id="58708-139">This list may differ from hello official list of supported systems on hello Microsoft Azure Platform, as described here: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span></span>
> 
> 

* <span data-ttu-id="58708-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="58708-140">CoreOS</span></span>
* <span data-ttu-id="58708-141">CentOS 6.3 +</span><span class="sxs-lookup"><span data-stu-id="58708-141">CentOS 6.3+</span></span>
* <span data-ttu-id="58708-142">Red Hat Enterprise Linux 6.7 +</span><span class="sxs-lookup"><span data-stu-id="58708-142">Red Hat Enterprise Linux 6.7+</span></span>
* <span data-ttu-id="58708-143">Debian 7.0 +</span><span class="sxs-lookup"><span data-stu-id="58708-143">Debian 7.0+</span></span>
* <span data-ttu-id="58708-144">Ubuntu 12.04 +</span><span class="sxs-lookup"><span data-stu-id="58708-144">Ubuntu 12.04+</span></span>
* <span data-ttu-id="58708-145">openSUSE 12.3 +</span><span class="sxs-lookup"><span data-stu-id="58708-145">openSUSE 12.3+</span></span>
* <span data-ttu-id="58708-146">SLES 11 SP3 +</span><span class="sxs-lookup"><span data-stu-id="58708-146">SLES 11 SP3+</span></span>
* <span data-ttu-id="58708-147">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="58708-147">Oracle Linux 6.4+</span></span>

<span data-ttu-id="58708-148">Egyéb támogatott rendszerek:</span><span class="sxs-lookup"><span data-stu-id="58708-148">Other Supported Systems:</span></span>

* <span data-ttu-id="58708-149">Freebsd rendszerű 10 + (Azure Linux ügynök v2.0.10 +)</span><span class="sxs-lookup"><span data-stu-id="58708-149">FreeBSD 10+ (Azure Linux Agent v2.0.10+)</span></span>

<span data-ttu-id="58708-150">hello Linux-ügynök megfelelően egyes rendszer csomagok rendelés toofunction függ:</span><span class="sxs-lookup"><span data-stu-id="58708-150">hello Linux agent depends on some system packages in order toofunction properly:</span></span>

* <span data-ttu-id="58708-151">Python 2.6 +</span><span class="sxs-lookup"><span data-stu-id="58708-151">Python 2.6+</span></span>
* <span data-ttu-id="58708-152">OpenSSL 1.0 +</span><span class="sxs-lookup"><span data-stu-id="58708-152">OpenSSL 1.0+</span></span>
* <span data-ttu-id="58708-153">OpenSSH 5.3 +</span><span class="sxs-lookup"><span data-stu-id="58708-153">OpenSSH 5.3+</span></span>
* <span data-ttu-id="58708-154">Fájlrendszer segédprogramok: sfdisk, fdisk, mkfs, válogatottak</span><span class="sxs-lookup"><span data-stu-id="58708-154">Filesystem utilities: sfdisk, fdisk, mkfs, parted</span></span>
* <span data-ttu-id="58708-155">Jelszó-eszközök: chpasswd, sudo</span><span class="sxs-lookup"><span data-stu-id="58708-155">Password tools: chpasswd, sudo</span></span>
* <span data-ttu-id="58708-156">Eszközök feldolgozása szöveg: csökkentésének, grep</span><span class="sxs-lookup"><span data-stu-id="58708-156">Text processing tools: sed, grep</span></span>
* <span data-ttu-id="58708-157">A hálózati eszközök: ip-útvonal</span><span class="sxs-lookup"><span data-stu-id="58708-157">Network tools: ip-route</span></span>
* <span data-ttu-id="58708-158">Kernel támogatása UDF fájlrendszerek csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="58708-158">Kernel support for mounting UDF filesystems.</span></span>

## <a name="installation"></a><span data-ttu-id="58708-159">Telepítés</span><span class="sxs-lookup"><span data-stu-id="58708-159">Installation</span></span>
<span data-ttu-id="58708-160">A telepítési csomag tárházból egy RPM vagy DEB-csomag telepítését telepítése és hello Azure Linux ügynök frissítése hello előnyben részesített módja.</span><span class="sxs-lookup"><span data-stu-id="58708-160">Installation using an RPM or a DEB package from your distribution's package repository is hello preferred method of installing and upgrading hello Azure Linux Agent.</span></span> <span data-ttu-id="58708-161">Minden hello [terjesztési szolgáltatók által támogatott](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hello Azure Linux ügynök csomag integrálja a képeket és tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="58708-161">All hello [endorsed distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrate hello Azure Linux agent package into their images and repositories.</span></span>

<span data-ttu-id="58708-162">Tekintse meg a hello toohello dokumentációját [Azure Linux ügynök-tárház a Githubon](https://github.com/Azure/WALinuxAgent) a speciális telepítési lehetőségeket, például a forrás- vagy toocustom helyekről vagy az előtagok telepítése.</span><span class="sxs-lookup"><span data-stu-id="58708-162">Refer toohello documentation in hello [Azure Linux Agent repo on GitHub](https://github.com/Azure/WALinuxAgent) for advanced installation options, such as installing from source or toocustom locations or prefixes.</span></span>

## <a name="command-line-options"></a><span data-ttu-id="58708-163">Parancssori kapcsolók</span><span class="sxs-lookup"><span data-stu-id="58708-163">Command Line Options</span></span>
### <a name="flags"></a><span data-ttu-id="58708-164">Jelzők</span><span class="sxs-lookup"><span data-stu-id="58708-164">Flags</span></span>
* <span data-ttu-id="58708-165">részletes: növelése a megadott parancs</span><span class="sxs-lookup"><span data-stu-id="58708-165">verbose: Increase verbosity of specified command</span></span>
* <span data-ttu-id="58708-166">kényszerített: hagyja ki az egyes parancsok interaktív megerősítése</span><span class="sxs-lookup"><span data-stu-id="58708-166">force: Skip interactive confirmation for some commands</span></span>

### <a name="commands"></a><span data-ttu-id="58708-167">Parancsok</span><span class="sxs-lookup"><span data-stu-id="58708-167">Commands</span></span>
* <span data-ttu-id="58708-168">Súgó: hello támogatott parancsok és jelzők sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="58708-168">help: Lists hello supported commands and flags.</span></span>
* <span data-ttu-id="58708-169">deprovision: tooclean hello rendszer kísérletet, és teszi megfelelő ismételt üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="58708-169">deprovision: Attempt tooclean hello system and make it suitable for re-provisioning.</span></span> <span data-ttu-id="58708-170">Ez a művelet törli a hello következő:</span><span class="sxs-lookup"><span data-stu-id="58708-170">This operation deleted hello following:</span></span>
  
  * <span data-ttu-id="58708-171">Az összes SSH állomáskulcsai (ha Provisioning.RegenerateSshHostKeyPair "y" hello konfigurációs fájlban)</span><span class="sxs-lookup"><span data-stu-id="58708-171">All SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in hello configuration file)</span></span>
  * <span data-ttu-id="58708-172">A /etc/resolv.conf névkiszolgáló-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="58708-172">Nameserver configuration in /etc/resolv.conf</span></span>
  * <span data-ttu-id="58708-173">Gyökér szintű jelszavát a /etc/shadow (ha Provisioning.DeleteRootPassword "y" hello konfigurációs fájlban)</span><span class="sxs-lookup"><span data-stu-id="58708-173">Root password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in hello configuration file)</span></span>
  * <span data-ttu-id="58708-174">Gyorsítótárazott DHCP-ügyfél bérletek</span><span class="sxs-lookup"><span data-stu-id="58708-174">Cached DHCP client leases</span></span>
  * <span data-ttu-id="58708-175">Alaphelyzetbe állítását gazdagép neve toolocalhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="58708-175">Resets host name toolocalhost.localdomain</span></span>

> [!WARNING]
> <span data-ttu-id="58708-176">Megszüntetés nem garantálja hello lemezkép nincs bejelölve, az összes bizalmas adatokat, és a megfelelő terjesztési.</span><span class="sxs-lookup"><span data-stu-id="58708-176">Deprovisioning does not guarantee that hello image is cleared of all sensitive information and suitable for redistribution.</span></span>
> 
> 

* <span data-ttu-id="58708-177">deprovision + felhasználói: hajt végre, minden elemet - deprovision (fent) is törli a hello (/var/lib/waagent nyert) utolsó kiépített felhasználói fiókot, és az adatok.</span><span class="sxs-lookup"><span data-stu-id="58708-177">deprovision+user: Performs everything under -deprovision (above) and also deletes hello last provisioned user account (obtained from /var/lib/waagent) and associated data.</span></span> <span data-ttu-id="58708-178">Ez a paraméter esetén a megszüntetést egy olyanra, amely korábban az Azure-on kiépítés volt, előfordulhat, hogy lehet rögzíteni és újra felhasználni.</span><span class="sxs-lookup"><span data-stu-id="58708-178">This parameter is when de-provisioning an image that was previously provisioning on Azure so it may be captured and re-used.</span></span>
* <span data-ttu-id="58708-179">verzió: waagent hello verzióját jeleníti meg</span><span class="sxs-lookup"><span data-stu-id="58708-179">version: Displays hello version of waagent</span></span>
* <span data-ttu-id="58708-180">serialconsole: konfigurálja a LÁRVAJÁRAT toomark ttyS0 (hello első soros port) hello rendszerindító konzollal.</span><span class="sxs-lookup"><span data-stu-id="58708-180">serialconsole: Configures GRUB toomark ttyS0 (hello first serial port) as hello boot console.</span></span> <span data-ttu-id="58708-181">Ez biztosítja, hogy a kernel rendszerindítási naplóit toothe soros port küldött és elérhetővé tenni a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="58708-181">This ensures that kernel bootup logs are sent toothe serial port and made available for debugging.</span></span>
* <span data-ttu-id="58708-182">démon: waagent futtató hello platform démon toomanage interakció.</span><span class="sxs-lookup"><span data-stu-id="58708-182">daemon: Run waagent as a daemon toomanage interaction with hello platform.</span></span> <span data-ttu-id="58708-183">Az argumentum értéke a megadott toowaagent hello waagent init parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="58708-183">This argument is specified toowaagent in hello waagent init script.</span></span>
* <span data-ttu-id="58708-184">Start: háttérfolyamatként waagent futtatása</span><span class="sxs-lookup"><span data-stu-id="58708-184">start: Run waagent as a background process</span></span>

## <a name="configuration"></a><span data-ttu-id="58708-185">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="58708-185">Configuration</span></span>
<span data-ttu-id="58708-186">Egy konfigurációs fájl (/ etc/waagent.conf) vezérlők hello waagent műveleteket.</span><span class="sxs-lookup"><span data-stu-id="58708-186">A configuration file (/etc/waagent.conf) controls hello actions of waagent.</span></span> <span data-ttu-id="58708-187">Alább látható egy példa konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="58708-187">A sample configuration file is shown below:</span></span>

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

<span data-ttu-id="58708-188">hello részletesen ismerteti a különböző konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="58708-188">hello various configuration options are described in detail below.</span></span> <span data-ttu-id="58708-189">Beállítási lehetőségek állnak a három típusa létezik; Logikai érték, String vagy Integer.</span><span class="sxs-lookup"><span data-stu-id="58708-189">Configuration options are of three types; Boolean, String or Integer.</span></span> <span data-ttu-id="58708-190">"y" vagy "n" Hello logikai konfigurációs beállításokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="58708-190">hello Boolean configuration options can be specified as "y" or "n".</span></span> <span data-ttu-id="58708-191">hello különleges kulcsszó "None" néhány karakterlánc típusú konfigurációs elemet részletes, az alábbi használható.</span><span class="sxs-lookup"><span data-stu-id="58708-191">hello special keyword "None" may be used for some string type configuration entries as detailed below.</span></span>

<span data-ttu-id="58708-192">**Provisioning.Enabled:**</span><span class="sxs-lookup"><span data-stu-id="58708-192">**Provisioning.Enabled:**</span></span>  
<span data-ttu-id="58708-193">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-193">Type: Boolean</span></span>  
<span data-ttu-id="58708-194">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="58708-194">Default: y</span></span>

<span data-ttu-id="58708-195">Ez lehetővé teszi, hogy a felhasználó tooenable hello, vagy tiltsa le a hello kiépítés hello ügynök funkcióit.</span><span class="sxs-lookup"><span data-stu-id="58708-195">This allows hello user tooenable or disable hello provisioning functionality in hello agent.</span></span> <span data-ttu-id="58708-196">Érvényes értékek: "y" vagy "n".</span><span class="sxs-lookup"><span data-stu-id="58708-196">Valid values are "y" or "n".</span></span> <span data-ttu-id="58708-197">Kiépítés le van tiltva, ha hello kép SSH-állomás és a felhasználói kulcsok megmaradnak, és a hello Azure létesítési API megadott minden beállítás figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="58708-197">If provisioning is disabled, SSH host and user keys in hello image are preserved and any configuration specified in hello Azure provisioning API is ignored.</span></span>

> [!NOTE]
> <span data-ttu-id="58708-198">Hello `Provisioning.Enabled` túl "n" Ubuntu felhő lemezképeket inicializálás felhőben történő üzembe helyezéséhez használjon a paraméter alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="58708-198">hello `Provisioning.Enabled` parameter defaults too"n" on Ubuntu Cloud Images that use cloud-init for provisioning.</span></span>
> 
> 

<span data-ttu-id="58708-199">**Provisioning.DeleteRootPassword:**</span><span class="sxs-lookup"><span data-stu-id="58708-199">**Provisioning.DeleteRootPassword:**</span></span>  
<span data-ttu-id="58708-200">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-200">Type: Boolean</span></span>  
<span data-ttu-id="58708-201">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="58708-201">Default: n</span></span>

<span data-ttu-id="58708-202">Ha be van állítva, hello gyökér szintű jelszavát a hello/etc/árnyékmásolat fájl törlése során hello a telepítési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="58708-202">If set, hello root password in hello /etc/shadow file is erased during hello provisioning process.</span></span>

<span data-ttu-id="58708-203">**Provisioning.RegenerateSshHostKeyPair:**</span><span class="sxs-lookup"><span data-stu-id="58708-203">**Provisioning.RegenerateSshHostKeyPair:**</span></span>  
<span data-ttu-id="58708-204">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-204">Type: Boolean</span></span>  
<span data-ttu-id="58708-205">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="58708-205">Default: y</span></span>

<span data-ttu-id="58708-206">Ha be van állítva, az összes SSH állomás kulcspárok (ecdsa, dsa és rsa) során hello létesítésének folyamatát kell használnia az/etc/ssh/törlés.</span><span class="sxs-lookup"><span data-stu-id="58708-206">If set, all SSH host key pairs (ecdsa, dsa and rsa) are deleted during hello provisioning process from /etc/ssh/.</span></span> <span data-ttu-id="58708-207">És egy egyetlen új kulcspár.</span><span class="sxs-lookup"><span data-stu-id="58708-207">And a single fresh key pair is generated.</span></span>

<span data-ttu-id="58708-208">hello titkosítási hello friss kulcspár alaptípusa hello Provisioning.SshHostKeyPairType bejegyzés által konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="58708-208">hello encryption type for hello fresh key pair is configurable by hello Provisioning.SshHostKeyPairType entry.</span></span> <span data-ttu-id="58708-209">Vegye figyelembe, hogy néhány terjesztési hozza létre a hiányzó titkosítási típusok SSH-kulcspár, hello SSH démon (például biztonsági újraindításkor) újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="58708-209">Please note that some distributions will re-create SSH key pairs for any missing encryption types when hello SSH daemon is restarted (for example, upon a reboot).</span></span>

<span data-ttu-id="58708-210">**Provisioning.SshHostKeyPairType:**</span><span class="sxs-lookup"><span data-stu-id="58708-210">**Provisioning.SshHostKeyPairType:**</span></span>  
<span data-ttu-id="58708-211">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-211">Type: String</span></span>  
<span data-ttu-id="58708-212">Alapértelmezett: rsa</span><span class="sxs-lookup"><span data-stu-id="58708-212">Default: rsa</span></span>

<span data-ttu-id="58708-213">E beállítás tooan titkosítási algoritmus típus hello SSH-démon hello virtuális gép által támogatott.</span><span class="sxs-lookup"><span data-stu-id="58708-213">This can be set tooan encryption algorithm type that is supported by hello SSH daemon on hello virtual machine.</span></span> <span data-ttu-id="58708-214">hello általában a támogatott értékek a következők: "rsa", "dsa" és "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="58708-214">hello typically supported values are "rsa", "dsa" and "ecdsa".</span></span> <span data-ttu-id="58708-215">Vegye figyelembe, hogy a "putty.exe" a Windows nem támogatja "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="58708-215">Note that "putty.exe" on Windows does not support "ecdsa".</span></span> <span data-ttu-id="58708-216">Igen ha azt tervezi, a Windows tooconnect tooa Linux telepítési toouse putty.exe, használja az "rsa" vagy "dsa".</span><span class="sxs-lookup"><span data-stu-id="58708-216">So, if you intend toouse putty.exe on Windows tooconnect tooa Linux deployment, please use "rsa" or "dsa".</span></span>

<span data-ttu-id="58708-217">**Provisioning.MonitorHostName:**</span><span class="sxs-lookup"><span data-stu-id="58708-217">**Provisioning.MonitorHostName:**</span></span>  
<span data-ttu-id="58708-218">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-218">Type: Boolean</span></span>  
<span data-ttu-id="58708-219">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="58708-219">Default: y</span></span>

<span data-ttu-id="58708-220">Ha beállításához waagent ellenőrzi hello Linux virtuális gép állomásnevét módosítások (a hello "állomásnév" parancs által visszaadott) és automatikusan frissíti a hálózati konfiguráció hello hello kép tooreflect hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="58708-220">If set, waagent will monitor hello Linux virtual machine for hostname changes (as returned by hello "hostname" command) and automatically update hello networking configuration in hello image tooreflect hello change.</span></span> <span data-ttu-id="58708-221">A sorrend toopush hello név toohello DNS-kiszolgálók módosításához hálózatkezelés hello virtuális gép újraindul.</span><span class="sxs-lookup"><span data-stu-id="58708-221">In order toopush hello name change toohello DNS servers, networking will be restarted in hello virtual machine.</span></span> <span data-ttu-id="58708-222">Ekkor röviden a Internet kapcsolat megszakadása.</span><span class="sxs-lookup"><span data-stu-id="58708-222">This will result in brief loss of Internet connectivity.</span></span>

<span data-ttu-id="58708-223">**Provisioning.DecodeCustomData**</span><span class="sxs-lookup"><span data-stu-id="58708-223">**Provisioning.DecodeCustomData**</span></span>  
<span data-ttu-id="58708-224">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-224">Type: Boolean</span></span>  
<span data-ttu-id="58708-225">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="58708-225">Default: n</span></span>

<span data-ttu-id="58708-226">Ha beállításához waagent meg fog dekódolni a Base64 kódolású anyag CustomData.</span><span class="sxs-lookup"><span data-stu-id="58708-226">If set, waagent will decode CustomData from Base64.</span></span>

<span data-ttu-id="58708-227">**Provisioning.ExecuteCustomData**</span><span class="sxs-lookup"><span data-stu-id="58708-227">**Provisioning.ExecuteCustomData**</span></span>  
<span data-ttu-id="58708-228">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-228">Type: Boolean</span></span>  
<span data-ttu-id="58708-229">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="58708-229">Default: n</span></span>

<span data-ttu-id="58708-230">Ha beállításához waagent végrehajtja a CustomData kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="58708-230">If set, waagent will execute CustomData after provisioning.</span></span>

<span data-ttu-id="58708-231">**Provisioning.PasswordCryptId**</span><span class="sxs-lookup"><span data-stu-id="58708-231">**Provisioning.PasswordCryptId**</span></span>  
<span data-ttu-id="58708-232">Típus: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-232">Type:String</span></span>  
<span data-ttu-id="58708-233">Alapértelmezett: 6</span><span class="sxs-lookup"><span data-stu-id="58708-233">Default:6</span></span>

<span data-ttu-id="58708-234">Jelszókivonat létrehozásakor használt titkosítási algoritmus.</span><span class="sxs-lookup"><span data-stu-id="58708-234">Algorithm used by crypt when generating password hash.</span></span>  
 <span data-ttu-id="58708-235">1 - MD5</span><span class="sxs-lookup"><span data-stu-id="58708-235">1 - MD5</span></span>  
 <span data-ttu-id="58708-236">2a – Blowfish</span><span class="sxs-lookup"><span data-stu-id="58708-236">2a - Blowfish</span></span>  
 <span data-ttu-id="58708-237">5 - SHA-256</span><span class="sxs-lookup"><span data-stu-id="58708-237">5 - SHA-256</span></span>  
 <span data-ttu-id="58708-238">6 - SHA-512</span><span class="sxs-lookup"><span data-stu-id="58708-238">6 - SHA-512</span></span>  

<span data-ttu-id="58708-239">**Provisioning.PasswordCryptSaltLength**</span><span class="sxs-lookup"><span data-stu-id="58708-239">**Provisioning.PasswordCryptSaltLength**</span></span>  
<span data-ttu-id="58708-240">Típus: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-240">Type:String</span></span>  
<span data-ttu-id="58708-241">Alapértelmezett: 10</span><span class="sxs-lookup"><span data-stu-id="58708-241">Default:10</span></span>

<span data-ttu-id="58708-242">Jelszókivonat létrehozásához használt véletlenszerű védőérték hosszát.</span><span class="sxs-lookup"><span data-stu-id="58708-242">Length of random salt used when generating password hash.</span></span>

<span data-ttu-id="58708-243">**ResourceDisk.Format:**</span><span class="sxs-lookup"><span data-stu-id="58708-243">**ResourceDisk.Format:**</span></span>  
<span data-ttu-id="58708-244">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-244">Type: Boolean</span></span>  
<span data-ttu-id="58708-245">Alapértelmezett: y</span><span class="sxs-lookup"><span data-stu-id="58708-245">Default: y</span></span>

<span data-ttu-id="58708-246">Ha beállításához hello erőforrás lemez hello platform által biztosított fog formázható és hello filesystem "ResourceDisk.Filesystem" hello felhasználó által kért típus "ntfs" csakis által waagent csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="58708-246">If set, hello resource disk provided by hello platform will be formatted and mounted by waagent if hello filesystem type requested by hello user in "ResourceDisk.Filesystem" is anything other than "ntfs".</span></span> <span data-ttu-id="58708-247">Egy olyan partíciót, Linux (83) típusú hello lemezen rendelkezésre álló fog történni.</span><span class="sxs-lookup"><span data-stu-id="58708-247">A single partition of type Linux (83) will be made available on hello disk.</span></span> <span data-ttu-id="58708-248">Vegye figyelembe, hogy ez a partíció nem lesz formázva Ha sikeresen csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="58708-248">Note that this partition will not be formatted if it can be successfully mounted.</span></span>

<span data-ttu-id="58708-249">**ResourceDisk.Filesystem:**</span><span class="sxs-lookup"><span data-stu-id="58708-249">**ResourceDisk.Filesystem:**</span></span>  
<span data-ttu-id="58708-250">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-250">Type: String</span></span>  
<span data-ttu-id="58708-251">Alapértelmezett: ext4</span><span class="sxs-lookup"><span data-stu-id="58708-251">Default: ext4</span></span>

<span data-ttu-id="58708-252">Azt határozza meg a hello erőforrás lemez hello fájlrendszer típusát.</span><span class="sxs-lookup"><span data-stu-id="58708-252">This specifies hello filesystem type for hello resource disk.</span></span> <span data-ttu-id="58708-253">A Linux-disztribúció által támogatott értékek eltérők lehetnek.</span><span class="sxs-lookup"><span data-stu-id="58708-253">Supported values vary by Linux distribution.</span></span> <span data-ttu-id="58708-254">Ha X, majd mkfs hello karakterlánc. X hello Linux kép jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="58708-254">If hello string is X, then mkfs.X should be present on hello Linux image.</span></span> <span data-ttu-id="58708-255">SLES 11 lemezképeket általában használjon "ext3".</span><span class="sxs-lookup"><span data-stu-id="58708-255">SLES 11 images should typically use 'ext3'.</span></span> <span data-ttu-id="58708-256">Freebsd rendszerű lemezképek itt "ufs2" kell használni.</span><span class="sxs-lookup"><span data-stu-id="58708-256">FreeBSD images should use 'ufs2' here.</span></span>

<span data-ttu-id="58708-257">**ResourceDisk.MountPoint:**</span><span class="sxs-lookup"><span data-stu-id="58708-257">**ResourceDisk.MountPoint:**</span></span>  
<span data-ttu-id="58708-258">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-258">Type: String</span></span>  
<span data-ttu-id="58708-259">Alapértelmezett: / mnt és az erőforrások</span><span class="sxs-lookup"><span data-stu-id="58708-259">Default: /mnt/resource</span></span> 

<span data-ttu-id="58708-260">Azt határozza meg a hello elérési utat, amelyen hello erőforrás lemez csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="58708-260">This specifies hello path at which hello resource disk is mounted.</span></span> <span data-ttu-id="58708-261">Vegye figyelembe, hogy hello erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="58708-261">Note that hello resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span>

<span data-ttu-id="58708-262">**ResourceDisk.MountOptions**</span><span class="sxs-lookup"><span data-stu-id="58708-262">**ResourceDisk.MountOptions**</span></span>  
<span data-ttu-id="58708-263">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-263">Type: String</span></span>  
<span data-ttu-id="58708-264">Alapértelmezett: nincs</span><span class="sxs-lookup"><span data-stu-id="58708-264">Default: None</span></span>

<span data-ttu-id="58708-265">Lemez csatlakoztatási lehetőségek átadott toobe toohello csatlakoztatási -o parancsot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="58708-265">Specifies disk mount options toobe passed toohello mount -o command.</span></span> <span data-ttu-id="58708-266">Például ez az értékek, vesszővel elválasztott listája.</span><span class="sxs-lookup"><span data-stu-id="58708-266">This is a comma separated list of values, ex.</span></span> <span data-ttu-id="58708-267">"nodev, nosuid".</span><span class="sxs-lookup"><span data-stu-id="58708-267">'nodev,nosuid'.</span></span> <span data-ttu-id="58708-268">Tekintse meg a részletes mount(8).</span><span class="sxs-lookup"><span data-stu-id="58708-268">See mount(8) for details.</span></span>

<span data-ttu-id="58708-269">**ResourceDisk.EnableSwap:**</span><span class="sxs-lookup"><span data-stu-id="58708-269">**ResourceDisk.EnableSwap:**</span></span>  
<span data-ttu-id="58708-270">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-270">Type: Boolean</span></span>  
<span data-ttu-id="58708-271">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="58708-271">Default: n</span></span>

<span data-ttu-id="58708-272">Ha beállításához lapozófájl (/ swapfile) hello erőforrás lemezen jön létre, és toohello rendszer lapozóterület hozzá.</span><span class="sxs-lookup"><span data-stu-id="58708-272">If set, a swap file (/swapfile) is created on hello resource disk and added toohello system swap space.</span></span>

<span data-ttu-id="58708-273">**ResourceDisk.SwapSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="58708-273">**ResourceDisk.SwapSizeMB:**</span></span>  
<span data-ttu-id="58708-274">Típus: egész szám</span><span class="sxs-lookup"><span data-stu-id="58708-274">Type: Integer</span></span>  
<span data-ttu-id="58708-275">Alapértelmezett: 0</span><span class="sxs-lookup"><span data-stu-id="58708-275">Default: 0</span></span>

<span data-ttu-id="58708-276">hello mérete hello lapozófájl mérete (MB).</span><span class="sxs-lookup"><span data-stu-id="58708-276">hello size of hello swap file in megabytes.</span></span>

<span data-ttu-id="58708-277">**Logs.Verbose:**</span><span class="sxs-lookup"><span data-stu-id="58708-277">**Logs.Verbose:**</span></span>  
<span data-ttu-id="58708-278">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-278">Type: Boolean</span></span>  
<span data-ttu-id="58708-279">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="58708-279">Default: n</span></span>

<span data-ttu-id="58708-280">Ha be van állítva, napló részletességi súlyozott van.</span><span class="sxs-lookup"><span data-stu-id="58708-280">If set, log verbosity is boosted.</span></span> <span data-ttu-id="58708-281">Waagent too/var/log/waagent.log naplózza, és lehetővé teszi a hello logrotate funkció toorotate rendszernaplókat.</span><span class="sxs-lookup"><span data-stu-id="58708-281">Waagent logs too/var/log/waagent.log and leverages hello system logrotate functionality toorotate logs.</span></span>

<span data-ttu-id="58708-282">**AZ OPERÁCIÓS RENDSZER. EnableRDMA**</span><span class="sxs-lookup"><span data-stu-id="58708-282">**OS.EnableRDMA**</span></span>  
<span data-ttu-id="58708-283">Típus: logikai</span><span class="sxs-lookup"><span data-stu-id="58708-283">Type: Boolean</span></span>  
<span data-ttu-id="58708-284">Alapértelmezett: n</span><span class="sxs-lookup"><span data-stu-id="58708-284">Default: n</span></span>

<span data-ttu-id="58708-285">Amennyiben beállításához hello ügynök fog próbálja tooinstall és töltsön be egy RDMA kernel-illesztőprogram hello belső vezérlőprogramját az alapul szolgáló hardver hello hello verziójának megfelelő.</span><span class="sxs-lookup"><span data-stu-id="58708-285">If set, hello agent will attempt tooinstall and then load an RDMA kernel driver that matches hello version of hello firmware on hello underlying hardware.</span></span>

<span data-ttu-id="58708-286">**AZ OPERÁCIÓS RENDSZER. RootDeviceScsiTimeout:**</span><span class="sxs-lookup"><span data-stu-id="58708-286">**OS.RootDeviceScsiTimeout:**</span></span>  
<span data-ttu-id="58708-287">Típus: egész szám</span><span class="sxs-lookup"><span data-stu-id="58708-287">Type: Integer</span></span>  
<span data-ttu-id="58708-288">Alapértelmezett: 300</span><span class="sxs-lookup"><span data-stu-id="58708-288">Default: 300</span></span>

<span data-ttu-id="58708-289">Ez konfigurálja az hello SCSI időtúllépés másodpercben hello OS lemez- és meghajtókon.</span><span class="sxs-lookup"><span data-stu-id="58708-289">This configures hello SCSI timeout in seconds on hello OS disk and data drives.</span></span> <span data-ttu-id="58708-290">Ha nincs beállítva, hello rendszer alapértelmezett értékeket használják.</span><span class="sxs-lookup"><span data-stu-id="58708-290">If not set, hello system defaults are used.</span></span>

<span data-ttu-id="58708-291">**AZ OPERÁCIÓS RENDSZER. OpensslPath:**</span><span class="sxs-lookup"><span data-stu-id="58708-291">**OS.OpensslPath:**</span></span>  
<span data-ttu-id="58708-292">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-292">Type: String</span></span>  
<span data-ttu-id="58708-293">Alapértelmezett: nincs</span><span class="sxs-lookup"><span data-stu-id="58708-293">Default: None</span></span>

<span data-ttu-id="58708-294">Ez lehet használt toospecify egy alternatív elérési utat a hello openssl bináris toouse titkosítási műveletek.</span><span class="sxs-lookup"><span data-stu-id="58708-294">This can be used toospecify an alternate path for hello openssl binary toouse for cryptographic operations.</span></span>

<span data-ttu-id="58708-295">**HttpProxy.Host, HttpProxy.Port**</span><span class="sxs-lookup"><span data-stu-id="58708-295">**HttpProxy.Host, HttpProxy.Port**</span></span>  
<span data-ttu-id="58708-296">Típus: Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="58708-296">Type: String</span></span>  
<span data-ttu-id="58708-297">Alapértelmezett: nincs</span><span class="sxs-lookup"><span data-stu-id="58708-297">Default: None</span></span>

<span data-ttu-id="58708-298">Ha beállításához hello ügynök használni fog a proxy server tooaccess hello internet.</span><span class="sxs-lookup"><span data-stu-id="58708-298">If set, hello agent will use this proxy server tooaccess hello internet.</span></span> 

## <a name="ubuntu-cloud-images"></a><span data-ttu-id="58708-299">Ubuntu felhő lemezképek</span><span class="sxs-lookup"><span data-stu-id="58708-299">Ubuntu Cloud Images</span></span>
<span data-ttu-id="58708-300">Vegye figyelembe, hogy Ubuntu felhő lemezképek használata [felhő inicializálás](https://launchpad.net/ubuntu/+source/cloud-init) tooperform számos konfigurációs feladatok, akkor más módon kell kezelnie hello Azure Linux ügynök.</span><span class="sxs-lookup"><span data-stu-id="58708-300">Note that Ubuntu Cloud Images utilize [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) tooperform many configuration tasks that would otherwise be managed by hello Azure Linux Agent.</span></span>  <span data-ttu-id="58708-301">Vegye figyelembe a következő különbségek hello:</span><span class="sxs-lookup"><span data-stu-id="58708-301">Please note hello following differences:</span></span>

* <span data-ttu-id="58708-302">**Provisioning.Enabled** túl "n" Ubuntu felhő képek feladatok kiépítés felhő inicializálás tooperform használó alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="58708-302">**Provisioning.Enabled** defaults too"n" on Ubuntu Cloud Images that use cloud-init tooperform provisioning tasks.</span></span>
* <span data-ttu-id="58708-303">a következő konfigurációs paraméterek hello nincs hatással a felhő inicializálás toomanage hello erőforrás lemez és a lapozófájl-kapacitás helyet használó Ubuntu felhő képek rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="58708-303">hello following configuration parameters have no effect on Ubuntu Cloud Images that use cloud-init toomanage hello resource disk and swap space:</span></span>
  
  * <span data-ttu-id="58708-304">**ResourceDisk.Format**</span><span class="sxs-lookup"><span data-stu-id="58708-304">**ResourceDisk.Format**</span></span>
  * <span data-ttu-id="58708-305">**ResourceDisk.Filesystem**</span><span class="sxs-lookup"><span data-stu-id="58708-305">**ResourceDisk.Filesystem**</span></span>
  * <span data-ttu-id="58708-306">**ResourceDisk.MountPoint**</span><span class="sxs-lookup"><span data-stu-id="58708-306">**ResourceDisk.MountPoint**</span></span>
  * <span data-ttu-id="58708-307">**ResourceDisk.EnableSwap**</span><span class="sxs-lookup"><span data-stu-id="58708-307">**ResourceDisk.EnableSwap**</span></span>
  * <span data-ttu-id="58708-308">**ResourceDisk.SwapSizeMB**</span><span class="sxs-lookup"><span data-stu-id="58708-308">**ResourceDisk.SwapSizeMB**</span></span>
* <span data-ttu-id="58708-309">Tekintse meg a következő erőforrások tooconfigure hello erőforrás lemez csatlakoztatási pont hello, és a kiépítés során Ubuntu felhő képek a lapozófájl:</span><span class="sxs-lookup"><span data-stu-id="58708-309">Please see hello following resources tooconfigure hello resource disk mount point and swap space on Ubuntu Cloud Images during provisioning:</span></span>
  
  * [<span data-ttu-id="58708-310">Ubuntu Wiki: Lapozófájl-kapacitás-partíciók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58708-310">Ubuntu Wiki: Configure Swap Partitions</span></span>](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [<span data-ttu-id="58708-311">Egyéni adatok hogy Azure virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="58708-311">Injecting Custom Data into an Azure Virtual Machine</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

