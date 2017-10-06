---
title: "a Windows és Linux IaaS virtuális gépeket lemeztitkosítás aaaAzure |} Microsoft Docs"
description: "Ez a cikk áttekintést nyújt a Microsoft Azure lemez titkosítása a Windows és Linux IaaS virtuális gépeket."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: kakhan
ms.openlocfilehash: b685abdcc908e66d2352ec5ac2d9996aa75af1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a><span data-ttu-id="5feb2-103">Windows és Linux IaaS virtuális gépeket az Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="5feb2-103">Azure Disk Encryption for Windows and Linux IaaS VMs</span></span>
<span data-ttu-id="5feb2-104">A Microsoft Azure erősen véglegesített tooensuring az adatvédelem, az adatok közös joghatóság alá és teszi lehetővé az Azure-t toocontrol speciális technológiák tooencrypt számos adatokat vezérlik, és a titkosítási kulcsok kezeléséhez az adatok vezérlő & naplózási hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="5feb2-104">Microsoft Azure is strongly committed tooensuring your data privacy, data sovereignty and enables you toocontrol your Azure hosted data through a range of advanced technologies tooencrypt, control and manage encryption keys, control & audit access of data.</span></span> <span data-ttu-id="5feb2-105">Ez az Azure ügyfelek hello rugalmasságot toochoose hello megoldást nyújt, amely az üzleti igényeinek leginkább megfelelő.</span><span class="sxs-lookup"><span data-stu-id="5feb2-105">This provides Azure customers hello flexibility toochoose hello solution that best meets their business needs.</span></span> <span data-ttu-id="5feb2-106">A dokumentum azt kódelemeit tooa új technológia megoldás "A Windows és Linux rendszerű infrastruktúra-szolgáltatási virtuális Azure Disk Encryption" toohelp és megvédeni az adatok toomeet a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="5feb2-106">In this paper, we will introduce you tooa new technology solution “Azure Disk Encryption for Windows and Linux IaaS VM’s” toohelp protect and safeguard your data toomeet your organizational security and compliance commitments.</span></span> <span data-ttu-id="5feb2-107">hello papír hogyan toouse hello Azure lemez titkosítási szolgáltatások többek között a hello támogatott forgatókönyvek és hello felhasználói élményt nyújt részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="5feb2-107">hello paper provides detailed guidance on how toouse hello Azure disk encryption features including hello supported scenarios and hello user experiences.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-108">Bizonyos ajánlások növelheti az adatok, hálózati vagy számítási erőforrás-használat, ami azt eredményezi, hogy további licencek vagy előfizetések költségeit.</span><span class="sxs-lookup"><span data-stu-id="5feb2-108">Certain recommendations might increase data, network, or compute resource usage, resulting in additional license or subscription costs.</span></span>

## <a name="overview"></a><span data-ttu-id="5feb2-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5feb2-109">Overview</span></span>
<span data-ttu-id="5feb2-110">Az Azure Disk Encryption egy új képesség, amely segít a Windows és Linux IaaS virtuális gépek lemezeit titkosításához.</span><span class="sxs-lookup"><span data-stu-id="5feb2-110">Azure Disk Encryption is a new capability that helps you encrypt your Windows and Linux IaaS virtual machine disks.</span></span> <span data-ttu-id="5feb2-111">Az Azure Disk Encryption kihasználja hello iparági szabvány [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) és a Windows hello szolgáltatás [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux tooprovide kötet titkosítási hello az operációs rendszer és adatlemezek hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5feb2-111">Azure Disk Encryption leverages hello industry standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) feature of Windows and hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) feature of Linux tooprovide volume encryption for hello OS and hello data disks.</span></span> <span data-ttu-id="5feb2-112">hello megoldás integrálva van [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp vezérlik, és hello lemez-titkosítási kulcsokat és titkos kódokat a kulcstároló-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="5feb2-112">hello solution is integrated with [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp you control and manage hello disk-encryption keys and secrets in your key vault subscription.</span></span> <span data-ttu-id="5feb2-113">hello megoldás biztosítja azt is, hogy a virtuálisgép-lemezeket hello lévő összes adat titkosítva legyenek az Azure tárolás közben.</span><span class="sxs-lookup"><span data-stu-id="5feb2-113">hello solution also ensures that all data on hello virtual machine disks are encrypted at rest in your Azure storage.</span></span>

<span data-ttu-id="5feb2-114">A Windows és Linux IaaS virtuális gépeket Azure lemeztitkosítás jelenleg **általánosan rendelkezésre álló** minden nyilvános Azure-régiók és AzureGov régióban a prémium szintű Storage szabványos virtuális gépek és virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="5feb2-114">Azure disk encryption for Windows and Linux IaaS VMs is now in **General Availability** in all Azure public regions and AzureGov regions for Standard  VMs and VMs with premium storage.</span></span>

### <a name="encryption-scenarios"></a><span data-ttu-id="5feb2-115">Titkosítási forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="5feb2-115">Encryption scenarios</span></span>
<span data-ttu-id="5feb2-116">hello Azure Disk Encryption megoldás támogatja a következő forgatókönyvet hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-116">hello Azure Disk Encryption solution supports hello following customer scenarios:</span></span>

* <span data-ttu-id="5feb2-117">Engedélyezze a titkosítást a előzetes titkosítással VHD- és titkosítási kulcsok alapján létrehozott új IaaS virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5feb2-117">Enable encryption on new IaaS VMs created from pre-encrypted VHD and encryption keys</span></span>
* <span data-ttu-id="5feb2-118">Engedélyezze a titkosítást a hello támogatott Azure-gyűjtemény lemezképei alapján létrehozott új IaaS virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5feb2-118">Enable encryption on new IaaS VMs created from hello supported Azure Gallery images</span></span>
* <span data-ttu-id="5feb2-119">Az Azure-ban futó meglévő infrastruktúra-szolgáltatási virtuális gépeket a titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5feb2-119">Enable encryption on existing IaaS VMs running in Azure</span></span>
* <span data-ttu-id="5feb2-120">Tiltsa le a titkosítást a Windows IaaS virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5feb2-120">Disable encryption on Windows IaaS VMs</span></span>
* <span data-ttu-id="5feb2-121">Tiltsa le a titkosítást a adatmeghajtókon Linux IaaS virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5feb2-121">Disable encryption on data drives for Linux IaaS VMs</span></span>
* <span data-ttu-id="5feb2-122">Felügyelt lemezes virtuális gépek titkosításának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5feb2-122">Enable encryption of managed disk VMs</span></span>
* <span data-ttu-id="5feb2-123">Egy meglévő titkosított nem prémium szintű storage VM titkosítási beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-123">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="5feb2-124">Biztonsági mentési és helyreállítási kulcs titkosítási kulccsal titkosított titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5feb2-124">Backup and restore of encrypted VMs, encrypted with key encryption key</span></span>

<span data-ttu-id="5feb2-125">hello megoldás támogatja a következő forgatókönyvek IaaS virtuális gépek esetén, ha engedélyezve van a Microsoft Azure-ban hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-125">hello solution supports hello following scenarios for IaaS VMs when they are enabled in Microsoft Azure:</span></span>

* <span data-ttu-id="5feb2-126">Az Azure Key Vault integrációja</span><span class="sxs-lookup"><span data-stu-id="5feb2-126">Integration with Azure Key Vault</span></span>
* <span data-ttu-id="5feb2-127">Standard szint virtuális gépek: [A, D, DS, G, GS, F és stb adatsorozat IaaS virtuális gépeket](https://azure.microsoft.com/pricing/details/virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="5feb2-127">Standard tier VMs: [A, D, DS, G, GS, F, and so forth series IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)</span></span>
* <span data-ttu-id="5feb2-128">A Windows és Linux IaaS virtuális gépeket és a felügyelt lemezes virtuális gépek a hello engedélyezése titkosítása támogatott Azure-katalógus lemezképek</span><span class="sxs-lookup"><span data-stu-id="5feb2-128">Enable encryption on Windows and Linux IaaS VMs and managed disk VMs from hello supported Azure Gallery images</span></span>
* <span data-ttu-id="5feb2-129">Tiltsa le a titkosítást az operációs rendszer és az adatok meghajtókon Windows IaaS virtuális gépeket és a felügyelt lemezes virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5feb2-129">Disable encryption on OS and data drives for Windows IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="5feb2-130">A Linux IaaS virtuális gépeket és a felügyelt lemezes virtuális gépek adatmeghajtók titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="5feb2-130">Disable encryption on data drives for Linux IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="5feb2-131">Engedélyezheti a titkosítást IaaS virtuális gépeket futtató Windows ügyfél operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="5feb2-131">Enable encryption on IaaS VMs running Windows Client OS</span></span>
* <span data-ttu-id="5feb2-132">Engedélyezze a titkosítást a csatlakoztatási elérési utak köteteken</span><span class="sxs-lookup"><span data-stu-id="5feb2-132">Enable encryption on volumes with mount paths</span></span>
* <span data-ttu-id="5feb2-133">Engedélyezze a titkosítást a Linux virtuális gépeken lemezzel konfigurált csíkozást (RAID) mdadm használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-133">Enable encryption on Linux VMs configured with disk striping (RAID) using mdadm</span></span>
* <span data-ttu-id="5feb2-134">Linux virtuális gépek LVM adatlemezek titkosításának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5feb2-134">Enable encryption on Linux VMs using LVM for data disks</span></span>
* <span data-ttu-id="5feb2-135">Engedélyezze a titkosítást a tárolóhelyekkel konfigurált Windows virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5feb2-135">Enable encryption on Windows VMs configured with Storage Spaces</span></span>
* <span data-ttu-id="5feb2-136">Egy meglévő titkosított nem prémium szintű storage VM titkosítási beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-136">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="5feb2-137">Minden Azure nyilvános és AzureGov régiók támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5feb2-137">All Azure Public and AzureGov regions are supported</span></span>

<span data-ttu-id="5feb2-138">hello megoldás nem támogatja a következő forgatókönyvek, funkcióit és technológia hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-138">hello solution does not support hello following scenarios, features, and technology:</span></span>

* <span data-ttu-id="5feb2-139">Az alapszintű csomag IaaS virtuális gépeket</span><span class="sxs-lookup"><span data-stu-id="5feb2-139">Basic tier IaaS VMs</span></span>
* <span data-ttu-id="5feb2-140">Linux IaaS virtuális gépeket egy operációs rendszer meghajtóján titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="5feb2-140">Disabling encryption on an OS drive for Linux IaaS VMs</span></span>
* <span data-ttu-id="5feb2-141">Egy adatmeghajtó titkosítás letiltása, ha az operációs rendszer meghajtó hello titkosított Linux Iaas virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5feb2-141">Disabling encryption on a data drive if hello OS drive is encrypted for Linux Iaas VMs</span></span>
* <span data-ttu-id="5feb2-142">Infrastruktúra-szolgáltatási virtuális gépeket hello klasszikus virtuális gép létrehozási módszerének használatával létrehozott</span><span class="sxs-lookup"><span data-stu-id="5feb2-142">IaaS VMs that are created by using hello classic VM creation method</span></span>
* <span data-ttu-id="5feb2-143">Engedélyezze a titkosítást a Windows és Linux IaaS virtuális gépeket a felhasználói egyéni lemezképek nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-143">Enable encryption on Windows and Linux IaaS VMs customer custom images is NOT supported.</span></span> <span data-ttu-id="5feb2-144">Engedélyezze a enccryption Linux LVM operációs rendszer lemezek használata nem támogatott jelenleg.</span><span class="sxs-lookup"><span data-stu-id="5feb2-144">Enable enccryption on Linux LVM OS disk is not supported currently.</span></span> <span data-ttu-id="5feb2-145">Ez a támogatás hamarosan határozza meg.</span><span class="sxs-lookup"><span data-stu-id="5feb2-145">This support will come soon.</span></span>
* <span data-ttu-id="5feb2-146">Integráció a helyszíni kulcskezelő szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5feb2-146">Integration with your on-premises Key Management Service</span></span>
* <span data-ttu-id="5feb2-147">Az Azure Files (megosztott fájlrendszer), a hálózati fájlrendszer (NFS), a dinamikus köteteket és a Windows alapú szoftveres RAID-rendszerek használatára konfigurált virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5feb2-147">Azure Files (shared file system), Network File System (NFS), dynamic volumes, and Windows VMs that are configured with software-based RAID systems</span></span>
* <span data-ttu-id="5feb2-148">Biztonsági mentési és visszaállítási titkosított virtuális gépek, titkosított kulcs titkosítási kulcs nélkül.</span><span class="sxs-lookup"><span data-stu-id="5feb2-148">Backup and restore of encrypted VMs, encrypted without key encryption key.</span></span>
* <span data-ttu-id="5feb2-149">Egy meglévő titkosított prémium szintű storage VM titkosítási beállításainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="5feb2-149">Update encryption settings of an existing encrypted premium storage VM.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-150">Biztonsági mentés és visszaállítás titkosított virtuális gépek csak támogatott virtuális gépek Titkosított hello KEK konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5feb2-150">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="5feb2-151">Nélkül KEK titkosított virtuális gépeken nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-151">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="5feb2-152">KEK nem kötelező paraméter, amely lehetővé teszi a virtuális gép titkosítása.</span><span class="sxs-lookup"><span data-stu-id="5feb2-152">KEK is an optional parameter that enables VM encryption.</span></span> <span data-ttu-id="5feb2-153">Ez a támogatás hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="5feb2-153">This support is coming soon.</span></span>
> <span data-ttu-id="5feb2-154">Egy meglévő titkosított prémium szintű storage, virtuális gép nem támogatott titkosítási beállításainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="5feb2-154">Update encryption settings of an existing encrypted premium storage VM are not supported.</span></span> <span data-ttu-id="5feb2-155">Ez a támogatás hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="5feb2-155">This support is coming soon.</span></span>

### <a name="encryption-features"></a><span data-ttu-id="5feb2-156">Titkosítási funkciók</span><span class="sxs-lookup"><span data-stu-id="5feb2-156">Encryption features</span></span>
<span data-ttu-id="5feb2-157">Engedélyezése és központi telepítésekor Azure Disk Encryption Azure IaaS virtuális gépekhez, hello alábbi képességeket engedélyezve vannak, a megadott hello konfigurációjától függően:</span><span class="sxs-lookup"><span data-stu-id="5feb2-157">When you enable and deploy Azure Disk Encryption for Azure IaaS VMs, hello following capabilities are enabled, depending on hello configuration provided:</span></span>

* <span data-ttu-id="5feb2-158">Az operációs rendszer hello kötet tooprotect hello rendszerindító kötet a tárolás közben titkosítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-158">Encryption of hello OS volume tooprotect hello boot volume at rest in your storage</span></span>
* <span data-ttu-id="5feb2-159">Az adatok kötetek tooprotect hello az adatkötetek a tárolás közben titkosítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-159">Encryption of data volumes tooprotect hello data volumes at rest in your storage</span></span>
* <span data-ttu-id="5feb2-160">Windows IaaS virtuális meghajtók hello az operációs rendszer és az adatok titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="5feb2-160">Disabling encryption on hello OS and data drives for Windows IaaS VMs</span></span>
* <span data-ttu-id="5feb2-161">Hello adatok titkosításának letiltása meghajtók Linux IaaS virtuális gépek (csak akkor, ha az operációs rendszer a nem titkosított meghajtó)</span><span class="sxs-lookup"><span data-stu-id="5feb2-161">Disabling encryption on hello data drives for Linux IaaS VMs (only if OS drive IS NOT encrypted)</span></span>
* <span data-ttu-id="5feb2-162">Hello titkosítási kulcsok és titkos kulcsokat a kulcstartót előfizetésben védelme</span><span class="sxs-lookup"><span data-stu-id="5feb2-162">Safeguarding hello encryption keys and secrets in your key vault subscription</span></span>
* <span data-ttu-id="5feb2-163">Infrastruktúra-szolgáltatási virtuális gép hello titkosítási állapotát hello jelentéskészítés titkosított</span><span class="sxs-lookup"><span data-stu-id="5feb2-163">Reporting hello encryption status of hello encrypted IaaS VM</span></span>
* <span data-ttu-id="5feb2-164">Lemeztitkosítás konfigurációs beállítások hello infrastruktúra-szolgáltatási virtuális gép eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-164">Removal of disk-encryption configuration settings from hello IaaS virtual machine</span></span>
* <span data-ttu-id="5feb2-165">Biztonsági mentés és visszaállítás titkosított virtuális gépek hello Azure Backup szolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-165">Backup and restore of encrypted VMs by using hello Azure Backup service</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-166">Biztonsági mentés és visszaállítás titkosított virtuális gépek csak támogatott virtuális gépek Titkosított hello KEK konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5feb2-166">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="5feb2-167">Nélkül KEK titkosított virtuális gépeken nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-167">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="5feb2-168">KEK nem kötelező paraméter, amely lehetővé teszi a virtuális gép titkosítása.</span><span class="sxs-lookup"><span data-stu-id="5feb2-168">KEK is an optional parameter that enables VM encryption.</span></span>

<span data-ttu-id="5feb2-169">Infrastruktúra-szolgáltatási virtuális gépek Windows és Linux-megoldás az Azure Disk Encryption tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5feb2-169">Azure Disk Encryption for IaaS VMS for Windows and Linux solution includes:</span></span>

* <span data-ttu-id="5feb2-170">a Windows hello-lemeztitkosítás kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="5feb2-170">hello disk-encryption extension for Windows.</span></span>
* <span data-ttu-id="5feb2-171">Linux hello lemeztitkosítás kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="5feb2-171">hello disk-encryption extension for Linux.</span></span>
* <span data-ttu-id="5feb2-172">hello lemeztitkosítás PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="5feb2-172">hello disk-encryption PowerShell cmdlets.</span></span>
* <span data-ttu-id="5feb2-173">lemeztitkosítás Azure parancssori felület (CLI) parancsmagok hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-173">hello disk-encryption Azure command-line interface (CLI) cmdlets.</span></span>
* <span data-ttu-id="5feb2-174">hello lemeztitkosítás Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="5feb2-174">hello disk-encryption Azure Resource Manager templates.</span></span>

<span data-ttu-id="5feb2-175">hello Azure Disk Encryption megoldás Windows vagy Linux operációs rendszert futtató IaaS virtuális gépek esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-175">hello Azure Disk Encryption solution is supported on IaaS VMs that are running Windows or Linux OS.</span></span> <span data-ttu-id="5feb2-176">Hello támogatott operációs rendszerekkel kapcsolatos további információkért lásd a "Hello"Előfeltételek"szakaszában.</span><span class="sxs-lookup"><span data-stu-id="5feb2-176">For more information about hello supported operating systems, see hello "Prerequisites" section.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-177">Nem kell külön fizetni virtuális gépek lemezei az Azure Disk Encryption titkosítási van.</span><span class="sxs-lookup"><span data-stu-id="5feb2-177">There is no additional charge for encrypting VM disks with Azure Disk Encryption.</span></span>

### <a name="value-proposition"></a><span data-ttu-id="5feb2-178">Értékajánlat</span><span class="sxs-lookup"><span data-stu-id="5feb2-178">Value proposition</span></span>
<span data-ttu-id="5feb2-179">Ha hello Azure lemez titkosítási-kezelési megoldást alkalmaz, felel meg a következő üzleti igények hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-179">When you apply hello Azure Disk Encryption-management solution, you can satisfy hello following business needs:</span></span>

* <span data-ttu-id="5feb2-180">IaaS virtuális gépek védett aktívan, mert használhatja a szabványos titkosítási technológia tooaddress szervezeti biztonsági és megfelelőségi követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="5feb2-180">IaaS VMs are secured at rest, because you can use industry-standard encryption technology tooaddress organizational security and compliance requirements.</span></span>
* <span data-ttu-id="5feb2-181">Az ügyfél által felügyelt kulcsok és a házirendeket, és IaaS virtuális gépeket rendszerindító naplózhatja a key vaultban lévő használatát.</span><span class="sxs-lookup"><span data-stu-id="5feb2-181">IaaS VMs boot under customer-controlled keys and policies, and you can audit their usage in your key vault.</span></span>

### <a name="encryption-workflow"></a><span data-ttu-id="5feb2-182">Titkosítási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="5feb2-182">Encryption workflow</span></span>
<span data-ttu-id="5feb2-183">Windows és Linux virtuális gépek, tooenable lemeztitkosítás hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-183">tooenable disk encryption for Windows and Linux VMs, do hello following:</span></span>

1. <span data-ttu-id="5feb2-184">Válasszon egy titkosítási forgatókönyv hello megelőző titkosítási forgatókönyvek közül.</span><span class="sxs-lookup"><span data-stu-id="5feb2-184">Choose an encryption scenario from among hello preceding encryption scenarios.</span></span>
2. <span data-ttu-id="5feb2-185">Részt vevő tooenabling lemeztitkosítás hello Azure lemez titkosítási Resource Manager-sablon, a PowerShell-parancsmagokkal vagy a CLI parancsot, és hello titkosítási konfiguráció megadásához.</span><span class="sxs-lookup"><span data-stu-id="5feb2-185">Opt in tooenabling disk encryption via hello Azure Disk Encryption Resource Manager template, PowerShell cmdlets, or CLI command, and specify hello encryption configuration.</span></span>

   * <span data-ttu-id="5feb2-186">Hello ügyfél titkosított virtuális Merevlemezt a forgatókönyvben a Titkosított hello VHD tooyour tárfiók és hello titkosítási kulcs jelentős tooyour kulcstároló feltöltése.</span><span class="sxs-lookup"><span data-stu-id="5feb2-186">For hello customer-encrypted VHD scenario, upload hello encrypted VHD tooyour storage account and hello encryption key material tooyour key vault.</span></span> <span data-ttu-id="5feb2-187">Ezt követően hello titkosítási konfigurációs tooenable titkosítást biztosíthat, egy új IaaS virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5feb2-187">Then, provide hello encryption configuration tooenable encryption on a new IaaS VM.</span></span>
   * <span data-ttu-id="5feb2-188">A piactér hello létrehozott új virtuális gépek és a meglévő virtuális gépek, amelyeken már az Azure-ban adja meg hello titkosítás konfigurációs tooenable funkciót a hello infrastruktúra-szolgáltatási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5feb2-188">For new VMs that are created from hello Marketplace and existing VMs that are already running in Azure, provide hello encryption configuration tooenable encryption on hello IaaS VM.</span></span>

3. <span data-ttu-id="5feb2-189">Adjon hozzáférést toohello Azure platformon tooread hello titkosítási kulcs anyagok (a BitLocker titkosítási kulcsok Windows rendszerekhez) és a Linux jelszava a kulcstartót tooenable titkosítás hello infrastruktúra-szolgáltatási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5feb2-189">Grant access toohello Azure platform tooread hello encryption-key material (BitLocker encryption keys for Windows systems and Passphrase for Linux) from your key vault tooenable encryption on hello IaaS VM.</span></span>

4. <span data-ttu-id="5feb2-190">Adja meg a hello Azure Active Directory (Azure AD) alkalmazás identitását toowrite hello titkosítási kulcs jelentős tooyour kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="5feb2-190">Provide hello Azure Active Directory (Azure AD) application identity toowrite hello encryption key material tooyour key vault.</span></span> <span data-ttu-id="5feb2-191">Ezzel engedélyezi a titkosítás hello infrastruktúra-szolgáltatási virtuális gép 2. lépésben említett hello forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="5feb2-191">Doing so enables encryption on hello IaaS VM for hello scenarios mentioned in step 2.</span></span>

5. <span data-ttu-id="5feb2-192">Azure hello VM modell frissíti a titkosítás és hello kulcstároló konfigurációval, majd állít be a titkosított virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="5feb2-192">Azure updates hello VM service model with encryption and hello key vault configuration, and sets up your encrypted VM.</span></span>

 ![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a><span data-ttu-id="5feb2-194">Visszafejtési munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="5feb2-194">Decryption workflow</span></span>
<span data-ttu-id="5feb2-195">infrastruktúra-szolgáltatási virtuális gépeket, a következő magas szintű lépései teljes hello toodisable lemeztitkosítás:</span><span class="sxs-lookup"><span data-stu-id="5feb2-195">toodisable disk encryption for IaaS VMs, complete hello following high-level steps:</span></span>

1. <span data-ttu-id="5feb2-196">Toodisable titkosítás (visszafejtési) válasszon egy infrastruktúra-szolgáltatási virtuális gép az Azure-ban a hello Azure lemez titkosítási Resource Manager-sablon vagy a PowerShell-parancsmagok használatával, majd hello visszafejtési konfiguráció megadásához.</span><span class="sxs-lookup"><span data-stu-id="5feb2-196">Choose toodisable encryption (decryption) on a running IaaS VM in Azure via hello Azure Disk Encryption Resource Manager template or PowerShell cmdlets, and specify hello decryption configuration.</span></span>

 <span data-ttu-id="5feb2-197">Ezzel a lépéssel letiltja a titkosítást hello az operációs rendszer vagy hello adatmennyiség vagy mindkét szolgáltatás fut a Windows alapú infrastruktúra-szolgáltatási virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-197">This step disables encryption of hello OS or hello data volume or both on hello running Windows IaaS VM.</span></span> <span data-ttu-id="5feb2-198">Azonban mivel hello előző szakaszban említett, Linux operációs rendszer lemez titkosításának letiltása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-198">However, as mentioned in hello previous section, disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="5feb2-199">hello visszafejtési lépés csak engedélyezett adatmeghajtók Linux virtuális gépeken, amíg az operációsrendszer-lemez hello nincs titkosítva.</span><span class="sxs-lookup"><span data-stu-id="5feb2-199">hello decryption step is allowed only for data drives on Linux VMs as long as hello OS disk is not encrypted.</span></span>
2. <span data-ttu-id="5feb2-200">Azure-hírt hello VM modell, és infrastruktúra-szolgáltatási virtuális gép hello visszafejtett van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="5feb2-200">Azure updates hello VM service model, and hello IaaS VM is marked decrypted.</span></span> <span data-ttu-id="5feb2-201">hello tartalmát hello virtuális gép már nem titkosított aktívan.</span><span class="sxs-lookup"><span data-stu-id="5feb2-201">hello contents of hello VM are no longer encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-202">hello disable-titkosítási művelet nem törli a kulcs tárolóban és hello titkosítási kulcsokat tárol (a Windows rendszereken a BitLocker titkosítási kulcsok) vagy a Linux jelszava.</span><span class="sxs-lookup"><span data-stu-id="5feb2-202">hello disable-encryption operation does not delete your key vault and hello encryption key material (BitLocker encryption keys for Windows systems or Passphrase for Linux).</span></span>
 > <span data-ttu-id="5feb2-203">Nem támogatott Linux operációs rendszer lemez titkosításának letiltása.</span><span class="sxs-lookup"><span data-stu-id="5feb2-203">Disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="5feb2-204">hello visszafejtési lépés csak a Linux virtuális gépeken adatmeghajtók engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5feb2-204">hello decryption step is allowed only for data drives on Linux VMs.</span></span>
<span data-ttu-id="5feb2-205">Lemez adattitkosítás a Linux letiltása nem támogatott, ha hello operációsrendszer-meghajtó van titkosítva.</span><span class="sxs-lookup"><span data-stu-id="5feb2-205">Disabling data disk encryption for Linux is not supported if hello OS drive is encrypted.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5feb2-206">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5feb2-206">Prerequisites</span></span>
<span data-ttu-id="5feb2-207">Mielőtt engedélyezné a Azure Disk Encryption Azure IaaS virtuális gépeken hello támogatott forgatókönyvek hello "Overview" szakaszban tárgyalt, lásd: hello a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="5feb2-207">Before you enable Azure Disk Encryption on Azure IaaS VMs for hello supported scenarios that were discussed in hello "Overview" section, see hello following prerequisites:</span></span>

* <span data-ttu-id="5feb2-208">Rendelkeznie kell egy érvényes aktív Azure-előfizetéssel toocreate erőforrások az Azure-régiókban hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-208">You must have a valid active Azure subscription toocreate resources in Azure in hello supported regions.</span></span>
* <span data-ttu-id="5feb2-209">A következő Windows Server-verziók hello támogatott Azure Disk Encryption: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 és Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="5feb2-209">Azure Disk Encryption is supported on hello following Windows Server versions: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016.</span></span>
* <span data-ttu-id="5feb2-210">A következő Windows-ügyfélverziókat hello támogatott Azure Disk Encryption: Windows 8 ügyfél és a Windows 10-ügyfeleknek.</span><span class="sxs-lookup"><span data-stu-id="5feb2-210">Azure Disk Encryption is supported on hello following Windows client versions: Windows 8 client and Windows 10 client.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-211">Windows Server 2008 R2 rendelkeznie kell az Azure-titkosítás engedélyezése előtt telepített .NET-keretrendszer 4.5.</span><span class="sxs-lookup"><span data-stu-id="5feb2-211">For Windows Server 2008 R2, you must have .NET Framework 4.5 installed before you enable encryption in Azure.</span></span> <span data-ttu-id="5feb2-212">Telepítheti a Windows Update telepítésével hello opcionális frissítést a Microsoft .NET-keretrendszer 4.5.2-es Windows Server 2008 R2 x64-alapú rendszerekhez ([KB2901983](https://support.microsoft.com/kb/2901983)).</span><span class="sxs-lookup"><span data-stu-id="5feb2-212">You can install it from Windows Update by installing hello optional update Microsoft .NET Framework 4.5.2 for Windows Server 2008 R2 x64-based systems ([KB2901983](https://support.microsoft.com/kb/2901983)).</span></span>

* <span data-ttu-id="5feb2-213">Az Azure Disk Encryption támogatott Azure-katalógus következő hello Linux server disztribúciók és verziók alapul:</span><span class="sxs-lookup"><span data-stu-id="5feb2-213">Azure Disk Encryption is supported on hello following Azure Gallery based Linux server distributions and versions:</span></span>

| <span data-ttu-id="5feb2-214">A Linux-Disztribúció</span><span class="sxs-lookup"><span data-stu-id="5feb2-214">Linux Distribution</span></span> | <span data-ttu-id="5feb2-215">Verzió</span><span class="sxs-lookup"><span data-stu-id="5feb2-215">Version</span></span> | <span data-ttu-id="5feb2-216">Támogatott titkosítási a kötet típusa</span><span class="sxs-lookup"><span data-stu-id="5feb2-216">Volume Type Supported for Encryption</span></span>|
| --- | --- |--- |
| <span data-ttu-id="5feb2-217">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5feb2-217">Ubuntu</span></span> | <span data-ttu-id="5feb2-218">16.04-NAPI-ES LTS VERZIÓ</span><span class="sxs-lookup"><span data-stu-id="5feb2-218">16.04-DAILY-LTS</span></span> | <span data-ttu-id="5feb2-219">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-219">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-220">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5feb2-220">Ubuntu</span></span> | <span data-ttu-id="5feb2-221">14.04.5-DAILY-LTS</span><span class="sxs-lookup"><span data-stu-id="5feb2-221">14.04.5-DAILY-LTS</span></span> | <span data-ttu-id="5feb2-222">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-222">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-223">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5feb2-223">Ubuntu</span></span> | <span data-ttu-id="5feb2-224">12.10</span><span class="sxs-lookup"><span data-stu-id="5feb2-224">12.10</span></span> | <span data-ttu-id="5feb2-225">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-225">Data disk</span></span> |
| <span data-ttu-id="5feb2-226">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5feb2-226">Ubuntu</span></span> | <span data-ttu-id="5feb2-227">12.04</span><span class="sxs-lookup"><span data-stu-id="5feb2-227">12.04</span></span> | <span data-ttu-id="5feb2-228">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-228">Data disk</span></span> |
| <span data-ttu-id="5feb2-229">RHEL</span><span class="sxs-lookup"><span data-stu-id="5feb2-229">RHEL</span></span> | <span data-ttu-id="5feb2-230">7.3</span><span class="sxs-lookup"><span data-stu-id="5feb2-230">7.3</span></span> | <span data-ttu-id="5feb2-231">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-231">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-232">RHEL</span><span class="sxs-lookup"><span data-stu-id="5feb2-232">RHEL</span></span> | <span data-ttu-id="5feb2-233">7.2</span><span class="sxs-lookup"><span data-stu-id="5feb2-233">7.2</span></span> | <span data-ttu-id="5feb2-234">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-234">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-235">RHEL</span><span class="sxs-lookup"><span data-stu-id="5feb2-235">RHEL</span></span> | <span data-ttu-id="5feb2-236">6.8</span><span class="sxs-lookup"><span data-stu-id="5feb2-236">6.8</span></span> | <span data-ttu-id="5feb2-237">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-237">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-238">RHEL</span><span class="sxs-lookup"><span data-stu-id="5feb2-238">RHEL</span></span> | <span data-ttu-id="5feb2-239">6.7</span><span class="sxs-lookup"><span data-stu-id="5feb2-239">6.7</span></span> | <span data-ttu-id="5feb2-240">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-240">Data disk</span></span> |
| <span data-ttu-id="5feb2-241">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-241">CentOS</span></span> | <span data-ttu-id="5feb2-242">7.3</span><span class="sxs-lookup"><span data-stu-id="5feb2-242">7.3</span></span> | <span data-ttu-id="5feb2-243">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-243">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-244">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-244">CentOS</span></span> | <span data-ttu-id="5feb2-245">7.2n</span><span class="sxs-lookup"><span data-stu-id="5feb2-245">7.2n</span></span> | <span data-ttu-id="5feb2-246">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-246">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-247">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-247">CentOS</span></span> | <span data-ttu-id="5feb2-248">6.8</span><span class="sxs-lookup"><span data-stu-id="5feb2-248">6.8</span></span> | <span data-ttu-id="5feb2-249">Operációs rendszer és az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="5feb2-249">OS and Data disk</span></span> |
| <span data-ttu-id="5feb2-250">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-250">CentOS</span></span> | <span data-ttu-id="5feb2-251">7.1</span><span class="sxs-lookup"><span data-stu-id="5feb2-251">7.1</span></span> | <span data-ttu-id="5feb2-252">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-252">Data disk</span></span> |
| <span data-ttu-id="5feb2-253">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-253">CentOS</span></span> | <span data-ttu-id="5feb2-254">7.0</span><span class="sxs-lookup"><span data-stu-id="5feb2-254">7.0</span></span> | <span data-ttu-id="5feb2-255">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-255">Data disk</span></span> |
| <span data-ttu-id="5feb2-256">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-256">CentOS</span></span> | <span data-ttu-id="5feb2-257">6.7</span><span class="sxs-lookup"><span data-stu-id="5feb2-257">6.7</span></span> | <span data-ttu-id="5feb2-258">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-258">Data disk</span></span> |
| <span data-ttu-id="5feb2-259">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-259">CentOS</span></span> | <span data-ttu-id="5feb2-260">6.6</span><span class="sxs-lookup"><span data-stu-id="5feb2-260">6.6</span></span> | <span data-ttu-id="5feb2-261">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-261">Data disk</span></span> |
| <span data-ttu-id="5feb2-262">CentOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-262">CentOS</span></span> | <span data-ttu-id="5feb2-263">6.5</span><span class="sxs-lookup"><span data-stu-id="5feb2-263">6.5</span></span> | <span data-ttu-id="5feb2-264">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-264">Data disk</span></span> |
| <span data-ttu-id="5feb2-265">openSUSE</span><span class="sxs-lookup"><span data-stu-id="5feb2-265">openSUSE</span></span> | <span data-ttu-id="5feb2-266">13.2</span><span class="sxs-lookup"><span data-stu-id="5feb2-266">13.2</span></span> | <span data-ttu-id="5feb2-267">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-267">Data disk</span></span> |
| <span data-ttu-id="5feb2-268">SLES</span><span class="sxs-lookup"><span data-stu-id="5feb2-268">SLES</span></span> | <span data-ttu-id="5feb2-269">12 SP1</span><span class="sxs-lookup"><span data-stu-id="5feb2-269">12 SP1</span></span> | <span data-ttu-id="5feb2-270">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-270">Data disk</span></span> |
| <span data-ttu-id="5feb2-271">SLES</span><span class="sxs-lookup"><span data-stu-id="5feb2-271">SLES</span></span> | <span data-ttu-id="5feb2-272">12-SP1 (prémium)</span><span class="sxs-lookup"><span data-stu-id="5feb2-272">12-SP1 (Premium)</span></span> | <span data-ttu-id="5feb2-273">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-273">Data disk</span></span> |
| <span data-ttu-id="5feb2-274">SLES</span><span class="sxs-lookup"><span data-stu-id="5feb2-274">SLES</span></span> | <span data-ttu-id="5feb2-275">HPC 12</span><span class="sxs-lookup"><span data-stu-id="5feb2-275">HPC 12</span></span> | <span data-ttu-id="5feb2-276">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-276">Data disk</span></span> |
| <span data-ttu-id="5feb2-277">SLES</span><span class="sxs-lookup"><span data-stu-id="5feb2-277">SLES</span></span> | <span data-ttu-id="5feb2-278">11-SP4 (prémium)</span><span class="sxs-lookup"><span data-stu-id="5feb2-278">11-SP4 (Premium)</span></span> | <span data-ttu-id="5feb2-279">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-279">Data disk</span></span> |
| <span data-ttu-id="5feb2-280">SLES</span><span class="sxs-lookup"><span data-stu-id="5feb2-280">SLES</span></span> | <span data-ttu-id="5feb2-281">11 SP4</span><span class="sxs-lookup"><span data-stu-id="5feb2-281">11 SP4</span></span> | <span data-ttu-id="5feb2-282">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-282">Data disk</span></span> |

* <span data-ttu-id="5feb2-283">Az Azure Disk Encryption megköveteli, hogy a kulcstartót és virtuális gépek található hello azonos Azure-régió, és az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5feb2-283">Azure Disk Encryption requires that your key vault and VMs reside in hello same Azure region and subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-284">Hello erőforrások konfigurálása az külön régióban hibát okoz a hello Azure Disk Encryption funkció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5feb2-284">Configuring hello resources in separate regions causes a failure in enabling hello Azure Disk Encryption feature.</span></span>

* <span data-ttu-id="5feb2-285">tooset össze és a kulcstartót konfigurálása az Azure Disk Encryption, című rész **állítsa össze, és a kulcstartót konfigurálása az Azure Disk Encryption** a hello *Előfeltételek* című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="5feb2-285">tooset up and configure your key vault for Azure Disk Encryption, see section **Set up and configure your key vault for Azure Disk Encryption** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="5feb2-286">tooset össze és az Azure AD-alkalmazást az Azure Active directory konfigurálása Azure Disk Encryption, című **hello Azure AD alkalmazás az Azure Active Directory beállítása** a hello *Előfeltételek* című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="5feb2-286">tooset up and configure Azure AD application in Azure Active directory for Azure Disk Encryption, see section **Set up hello Azure AD application in Azure Active Directory** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="5feb2-287">tooset össze és hello kulcstároló hozzáférési házirend hello Azure AD alkalmazás konfigurálása, című **hello kulcstároló hozzáférési házirend hello Azure AD-alkalmazás beállítása** a hello *Előfeltételek* szakasza Ez a cikk.</span><span class="sxs-lookup"><span data-stu-id="5feb2-287">tooset up and configure hello key vault access policy for hello Azure AD application, see section **Set up hello key vault access policy for hello Azure AD application** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="5feb2-288">című rész tooprepare előzetes titkosítással Windows virtuális merevlemez, **előzetes titkosítással Windows virtuális merevlemez előkészítése** a hello *függelék*.</span><span class="sxs-lookup"><span data-stu-id="5feb2-288">tooprepare a pre-encrypted Windows VHD, see section **Prepare a pre-encrypted Windows VHD** in hello *Appendix*.</span></span>
* <span data-ttu-id="5feb2-289">című rész tooprepare előzetes titkosítással Linux virtuális merevlemez, **előzetes titkosítással Linux virtuális merevlemez előkészítése** a hello *függelék*.</span><span class="sxs-lookup"><span data-stu-id="5feb2-289">tooprepare a pre-encrypted Linux VHD, see section **Prepare a pre-encrypted Linux VHD** in hello *Appendix*.</span></span>
* <span data-ttu-id="5feb2-290">hello Azure platformon kell hozzáférést toohello titkosítási kulcsok vagy titkos kulcsainak a kulcstartót toomake őket elérhető toohello virtuális gép indul el, és hello virtuális gép operációs rendszer mennyiségi visszafejti esetén.</span><span class="sxs-lookup"><span data-stu-id="5feb2-290">hello Azure platform needs access toohello encryption keys or secrets in your key vault toomake them available toohello virtual machine when it boots and decrypts hello virtual machine OS volume.</span></span> <span data-ttu-id="5feb2-291">toogrant engedélyek tooAzure platform, set hello **EnabledForDiskEncryption** hello kulcstartó tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="5feb2-291">toogrant permissions tooAzure platform, set hello **EnabledForDiskEncryption** property in hello key vault.</span></span> <span data-ttu-id="5feb2-292">További információkért lásd: **állítsa össze, és a kulcstartót konfigurálása az Azure Disk Encryption** a hello függelék.</span><span class="sxs-lookup"><span data-stu-id="5feb2-292">For more information, see **Set up and configure your key vault for Azure Disk Encryption** in hello Appendix.</span></span>
* <span data-ttu-id="5feb2-293">A kulcstároló titkos kulcs és a KEK URL-címek rendszerverzióval ellátott kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5feb2-293">Your key vault secret and KEK URLs must be versioned.</span></span> <span data-ttu-id="5feb2-294">Azure érvénybe lépteti az e versioning korlátozása.</span><span class="sxs-lookup"><span data-stu-id="5feb2-294">Azure enforces this restriction of versioning.</span></span> <span data-ttu-id="5feb2-295">Érvényes titkos kulcs és KEK URL-címek tekintse meg a következő példák hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-295">For valid secret and KEK URLs, see hello following examples:</span></span>

  * <span data-ttu-id="5feb2-296">Érvényes titkos URL-címet: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="5feb2-296">Example of a valid secret URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="5feb2-297">Érvényes KEK URL-címet: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="5feb2-297">Example of a valid KEK URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="5feb2-298">Az Azure Disk Encryption nem támogatja a kulcstároló titkok és KEK URL-címek részeként megadását portszámokat.</span><span class="sxs-lookup"><span data-stu-id="5feb2-298">Azure Disk Encryption does not support specifying port numbers as part of key vault secrets and KEK URLs.</span></span> <span data-ttu-id="5feb2-299">Nem támogatott és a támogatott kulcstároló URL-címek, tekintse meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-299">For examples of non-supported and supported key vault URLs, see hello following:</span></span>

  * <span data-ttu-id="5feb2-300">Nem fogadható el kulcstároló URL-cím *xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx https://contosovault.vault.azure.net:443/titkos kulcsok/contososecret*</span><span class="sxs-lookup"><span data-stu-id="5feb2-300">Unacceptable key vault URL  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="5feb2-301">Elfogadható kulcstároló URL-cím: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="5feb2-301">Acceptable key vault URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="5feb2-302">tooenable hello Azure Disk Encryption szolgáltatás hello IaaS virtuális gépeket hello hálózati végpont-konfigurációs követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="5feb2-302">tooenable hello Azure Disk Encryption feature, hello IaaS VMs must meet hello following network endpoint configuration requirements:</span></span>
  * <span data-ttu-id="5feb2-303">tooget token tooconnect tooyour kulcstároló, hello infrastruktúra-szolgáltatási virtuális gép lehet képes tooconnect tooan Azure Active Directory végpontján \[login.microsoftonline.com\].</span><span class="sxs-lookup"><span data-stu-id="5feb2-303">tooget a token tooconnect tooyour key vault, hello IaaS VM must be able tooconnect tooan Azure Active Directory endpoint, \[login.microsoftonline.com\].</span></span>
  * <span data-ttu-id="5feb2-304">toowrite hello titkosítási kulcsok tooyour kulcstároló, hello infrastruktúra-szolgáltatási virtuális gép képes tooconnect toohello kulcstároló végpont kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5feb2-304">toowrite hello encryption keys tooyour key vault, hello IaaS VM must be able tooconnect toohello key vault endpoint.</span></span>
  * <span data-ttu-id="5feb2-305">infrastruktúra-szolgáltatási virtuális gép hello képes tooconnect tooan az Azure storage végpont, hogy a gazdagépek hello Azure bővítménytárban és az Azure-tárfiók, hogy a gazdagépek hello VHD-fájlokat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5feb2-305">hello IaaS VM must be able tooconnect tooan Azure storage endpoint that hosts hello Azure extension repository and an Azure storage account that hosts hello VHD files.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5feb2-306">Ha a biztonsági házirend korlátozza a hozzáférést a Azure virtuális gépek toohello Internet, URI megelőző hello megoldásához, és egy adott szabály tooallow kimenő kapcsolat toohello IP-címek konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="5feb2-306">If your security policy limits access from Azure VMs toohello Internet, you can resolve hello preceding URI and configure a specific rule tooallow outbound connectivity toohello IPs.</span></span>
  >
  ><span data-ttu-id="5feb2-307">tooconfigure és hozzáférés az Azure Key Vault (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall) tűzfal mögött</span><span class="sxs-lookup"><span data-stu-id="5feb2-307">tooconfigure and access Azure Key Vault behind a firewall(https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span></span>

* <span data-ttu-id="5feb2-308">Az Azure PowerShell SDK verzió tooconfigure Azure Disk Encryption hello legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="5feb2-308">Use hello latest version of Azure PowerShell SDK version tooconfigure Azure Disk Encryption.</span></span> <span data-ttu-id="5feb2-309">Hello legújabb verziójának letöltése [Azure PowerShell kiadás](https://github.com/Azure/azure-powershell/releases)</span><span class="sxs-lookup"><span data-stu-id="5feb2-309">Download hello latest version of [Azure PowerShell release](https://github.com/Azure/azure-powershell/releases)</span></span>

 > [!NOTE]
  > <span data-ttu-id="5feb2-310">Nem támogatott az Azure Disk Encryption [Azure PowerShell SDK verzió 1.1.0-ás](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span><span class="sxs-lookup"><span data-stu-id="5feb2-310">Azure Disk Encryption is not supported on [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span></span> <span data-ttu-id="5feb2-311">Ha hibaüzenetet kap kapcsolódó toousing Azure PowerShell 1.1.0-ás, lásd: [Azure lemez titkosítási hiba kapcsolódó tooAzure PowerShell 1.1.0-ás](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span><span class="sxs-lookup"><span data-stu-id="5feb2-311">If you are receiving an error related toousing Azure PowerShell 1.1.0, see [Azure Disk Encryption Error Related tooAzure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span></span>

* <span data-ttu-id="5feb2-312">toorun bármely Azure CLI parancsot, és társítsa azt az Azure-előfizetése, először telepítenie kell az Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="5feb2-312">toorun any Azure CLI command and associate it with your Azure subscription, you must first install Azure CLI:</span></span>
  * <span data-ttu-id="5feb2-313">az Azure parancssori felület tooinstall és rendelje hozzá azt az Azure-előfizetéssel, lásd: [hogyan tooinstall és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5feb2-313">tooinstall Azure CLI and associate it with your Azure subscription, see [How tooinstall and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="5feb2-314">Lásd az Azure parancssori felület Mac, Linux és a Windows az Azure Resource Manager toouse [Azure parancssori felület parancsait erőforrás-kezelő módban](../virtual-machines/azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="5feb2-314">toouse Azure CLI for Mac, Linux, and Windows with Azure Resource Manager, see [Azure CLI commands in Resource Manager mode](../virtual-machines/azure-cli-arm-commands.md).</span></span>

* <span data-ttu-id="5feb2-315">Egy kezelt lemez titkosítása esetén kötelező előfeltétel tootake felügyelt hello lemez pillanatképe vagy hello lemezen kívül Azure Disk Encryption előzetes tooenabling titkosítási biztonsági mentést.</span><span class="sxs-lookup"><span data-stu-id="5feb2-315">When encrypting a managed disk, it is mandatory prerequisite tootake a snapshot of hello managed disk or a backup of hello disk outside of Azure Disk Encryption prior tooenabling encryption.</span></span>  <span data-ttu-id="5feb2-316">A hely biztonsági másolat nélkül minden titkosítás során váratlan hiba okozhatja hello lemez és virtuális gép nem érhető el egy helyreállítási beállítás megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="5feb2-316">Without a backup in place, any unexpected failure during encryption may render hello disk and VM inaccessible without a recovery option.</span></span>  <span data-ttu-id="5feb2-317">Set-AzureRmVMDiskEncryptionExtension felügyelt lemezek biztonsági mentése, és jelenleg nem lesz a hiba, ha használni egy felügyelt lemezes szemben, kivéve, ha hello - skipVmBackup paraméter van megadva.</span><span class="sxs-lookup"><span data-stu-id="5feb2-317">Set-AzureRmVMDiskEncryptionExtension does not currently back up managed disks and will error if used against a managed disk unless hello -skipVmBackup parameter has been specified.</span></span>  <span data-ttu-id="5feb2-318">A paraméter nem biztonságos toouse, kivéve, ha a biztonsági mentés kívül Azure Disk Encryption már megtörtént.</span><span class="sxs-lookup"><span data-stu-id="5feb2-318">This parameter is unsafe toouse unless a backup has already been made outside of Azure Disk Encryption.</span></span>   <span data-ttu-id="5feb2-319">Hello - skipVmBackup paraméter megadása esetén a hello parancsmag nem készítsen felügyelt hello lemez előzetes tooencryption biztonsági mentést.</span><span class="sxs-lookup"><span data-stu-id="5feb2-319">When hello -skipVmBackup parameter is specified, hello cmdlet will not make a backup of hello managed disk prior tooencryption.</span></span>  <span data-ttu-id="5feb2-320">Emiatt egy kötelező meg arról, hogy a virtuális gép helye előzetes tooenabling Azure Disk Encryption van, abban az esetben, ha újabb hello felügyelt lemezes biztonsági másolatának szükséges előfeltételek toomake számít.</span><span class="sxs-lookup"><span data-stu-id="5feb2-320">For this reason, it is considered a mandatory prerequisite toomake sure a backup of hello managed disk VM is in place prior tooenabling Azure Disk Encryption in case recovery is later needed.</span></span>  
> [!NOTE]
 > <span data-ttu-id="5feb2-321">hello - skipVmBackup paraméter soha nem kell használni, kivéve, ha egy pillanatkép vagy a biztonsági mentés már megtörtént Azure Disk Encryption kívül.</span><span class="sxs-lookup"><span data-stu-id="5feb2-321">hello -skipVmBackup parameter should never be used unless a snapshot or backup has already been made outside of Azure Disk Encryption.</span></span> 

* <span data-ttu-id="5feb2-322">hello Azure Disk Encryption megoldás hello BitLocker külső kulcsvédő Windows IaaS virtuális gépeket használ.</span><span class="sxs-lookup"><span data-stu-id="5feb2-322">hello Azure Disk Encryption solution uses hello BitLocker external key protector for Windows IaaS VMs.</span></span> <span data-ttu-id="5feb2-323">Tartományhoz csatlakoztatott virtuális gépeket, a nem leküldéses, amelyeket TPM védelmi csoport házirendekben.</span><span class="sxs-lookup"><span data-stu-id="5feb2-323">For domain joined VMs, DO NOT push any group policies that enforce TPM protectors.</span></span> <span data-ttu-id="5feb2-324">"A BitLocker engedélyezése kompatibilis TPM nélküli" hello a csoportházirenddel kapcsolatos információkért lásd: [a BitLocker csoportházirend-referenciája](https://technet.microsoft.com/library/ee706521).</span><span class="sxs-lookup"><span data-stu-id="5feb2-324">For information about hello group policy for “Allow BitLocker without a compatible TPM,” see [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521).</span></span>
* <span data-ttu-id="5feb2-325">Egyéni csoportházirenddel tartományhoz csatlakoztatott virtuális gépeken a BitLocker házirendnek tartalmaznia kell a következő beállítás hello: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption sikertelenek lesznek, amikor egyéni csoportházirend-beállítások a BitLocker nem kompatibilisek.</span><span class="sxs-lookup"><span data-stu-id="5feb2-325">Bitlocker policy on domain joined virtual machines with custom group policy must include hello following setting: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key`  Azure Disk Encryption will fail when custom group policy settings for Bitlocker are incompatible.</span></span> <span data-ttu-id="5feb2-326">Gépek, amelyeknek nincs hello szükség lehet a megfelelő házirend-beállítás alkalmazása hello új házirend, újraindítás hello új házirend tooupdate (gpupdate.exe Force), és indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="5feb2-326">On machines that did not have hello correct policy setting, applying hello new policy, forcing hello new policy tooupdate (gpupdate.exe /force), and then restarting may be required.</span></span>  
* <span data-ttu-id="5feb2-327">toocreate egy Azure AD-alkalmazást, hozzon létre egy kulcstartót, vagy állítson be egy meglévő kulcstároló és engedélyezheti a titkosítást, lásd: hello [Azure Disk Encryption előfeltétel PowerShell-parancsfájl](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span><span class="sxs-lookup"><span data-stu-id="5feb2-327">toocreate an Azure AD application, create a key vault, or set up an existing key vault and enable encryption, see hello [Azure Disk Encryption prerequisite PowerShell script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span></span>
* <span data-ttu-id="5feb2-328">tooconfigure lemeztitkosítás Előfeltételek hello Azure parancssori felület használatával lásd: [a Bash parancsfájlok](https://github.com/ejarvi/ade-cli-getting-started).</span><span class="sxs-lookup"><span data-stu-id="5feb2-328">tooconfigure disk-encryption prerequisites using hello Azure CLI, see [this Bash script](https://github.com/ejarvi/ade-cli-getting-started).</span></span>
* <span data-ttu-id="5feb2-329">toouse hello Azure Backup szolgáltatás tooback mentése és visszaállítása titkosított virtuális gépeken, ha titkosítás engedélyezve van az Azure Disk Encryption használatát, a virtuális gépek titkosítására hello Azure Disk Encryption konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="5feb2-329">toouse hello Azure Backup service tooback up and restore encrypted VMs, when encryption is enabled with Azure Disk Encryption, encrypt your VMs by using hello Azure Disk Encryption key configuration.</span></span> <span data-ttu-id="5feb2-330">hello biztonsági mentési szolgáltatás támogatja a virtuális gépek csak KEK-beállítás használatával titkosított.</span><span class="sxs-lookup"><span data-stu-id="5feb2-330">hello Backup service supports VMs that are encrypted using KEK configuration only.</span></span> <span data-ttu-id="5feb2-331">Lásd: [hogyan tooback mentése és visszaállítása titkosított virtuális gépek az Azure biztonsági másolatok titkosításának](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span><span class="sxs-lookup"><span data-stu-id="5feb2-331">See [How tooback up and restore encrypted virtual machines with Azure Backup  encryption](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span></span>

* <span data-ttu-id="5feb2-332">A Linux operációs rendszer kötetének titkosításához, vegye figyelembe, hogy a virtuális gép újraindítása jelenleg szükséges hello hello folyamat végén.</span><span class="sxs-lookup"><span data-stu-id="5feb2-332">When encrypting a Linux OS volume, note that a VM restart is currently required at hello end of hello process.</span></span> <span data-ttu-id="5feb2-333">Ezt megteheti hello portálon, a powershell vagy a parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="5feb2-333">This can be done via hello portal, powershell, or CLI.</span></span>   <span data-ttu-id="5feb2-334">tootrack hello folyamatban lévő titkosítási, rendszeres időközönként lekérdezi a Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus által visszaadott hello állapotüzenetet.</span><span class="sxs-lookup"><span data-stu-id="5feb2-334">tootrack hello progress of encryption, periodically poll hello status message returned by Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.</span></span>  <span data-ttu-id="5feb2-335">Titkosítási végrehajtása után hello hello Ez a parancs által visszaadott állapotüzenet jelzi ezt.</span><span class="sxs-lookup"><span data-stu-id="5feb2-335">Once encryption is complete, hello hello status message returned by this command will indicate this.</span></span>  <span data-ttu-id="5feb2-336">Például "Feladatnézetben: operációsrendszer-lemez titkosítása sikeres volt, indítsa újra a virtuális gép hello", a pont hello virtuális gép újraindul, és használja.</span><span class="sxs-lookup"><span data-stu-id="5feb2-336">For example, "ProgressMessage: OS disk successfully encrypted, please reboot hello VM"  At this point hello VM can be restarted and used.</span></span>  

* <span data-ttu-id="5feb2-337">Az Azure Disk Encryption Linux szükséges adatok lemezek toohave lévő Linux előzetes tooencryption csatlakoztatott fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="5feb2-337">Azure Disk Encryption for Linux requires data disks toohave a mounted file system in Linux prior tooencryption</span></span>

* <span data-ttu-id="5feb2-338">A rekurzív módon csatlakoztatott adatlemezek nem támogatottak az Azure Disk Encryption hello Linux.</span><span class="sxs-lookup"><span data-stu-id="5feb2-338">Recursively mounted data disks are not supported by hello Azure Disk Encryption for Linux.</span></span> <span data-ttu-id="5feb2-339">Például ha hello célrendszer rendelkezik csatlakoztatott lemez /foo/bar és majd egy másik /foo/bar/baz, /foo/bar/baz hello titkosítását a sikeres lesz, de/PEL/sáv titkosítása sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5feb2-339">For example, if hello target system has mounted a disk on /foo/bar and then another on /foo/bar/baz, hello encryption of /foo/bar/baz will succeed, but encryption of /foo/bar will fail.</span></span> 

* <span data-ttu-id="5feb2-340">Az Azure Disk Encryption csak támogatott Azure katalógusában lemezképeket, amelyek megfelelnek a fentebb említett Előfeltételek hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-340">Azure Disk Encryption is only supported on Azure gallery supported images that meet hello aforementioned prerequisites.</span></span> <span data-ttu-id="5feb2-341">Ügyfél egyéni lemezképek használata nem támogatott fizetési toocustom partíciós séma és a folyamat viselkedéshez, amely ezeket a lemezképeket is létezik.</span><span class="sxs-lookup"><span data-stu-id="5feb2-341">Customer custom images are not supported due toocustom partition schemes and process behaviors that may exist on these images.</span></span> <span data-ttu-id="5feb2-342">További még akkor is gyűjteménye a Képalapú a virtuális gép kezdetben előfeltételek teljesülnek, de lehet, hogy nem kompatibilis létrehozása után módosított.</span><span class="sxs-lookup"><span data-stu-id="5feb2-342">Further, even gallery image based VM's that initially met prerequisites but have been modified after creation may be incompatible.</span></span>  <span data-ttu-id="5feb2-343">Az, hogy emiatt hello javasolt eljárás a Linux virtuális gépek titkosításához toostart tiszta gyűjtemény lemezképről, hello virtuális gép titkosítása, és hozzáadja az egyéni szoftverfrissítések vagy adatok toohello virtuális gép szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="5feb2-343">For that reason, hello suggested procedure for encrypting a Linux VM is toostart from a clean gallery image, encrypt hello VM, and then add custom software or data toohello VM as needed.</span></span>  

> [!NOTE]
> <span data-ttu-id="5feb2-344">Biztonsági mentés és visszaállítás titkosított virtuális gépek csak támogatott virtuális gépek Titkosított hello KEK konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5feb2-344">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="5feb2-345">Nélkül KEK titkosított virtuális gépeken nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-345">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="5feb2-346">KEK nem kötelező paraméter, amely lehetővé teszi a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5feb2-346">KEK is an optional parameter that enables VM.</span></span>

#### <a name="set-up-hello-azure-ad-application-in-azure-active-directory"></a><span data-ttu-id="5feb2-347">Beállítása hello Azure AD-alkalmazást az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="5feb2-347">Set up hello Azure AD application in Azure Active Directory</span></span>
<span data-ttu-id="5feb2-348">Titkosítási toobe egy Azure-ban futó virtuális gépen engedélyezve van szüksége, amikor az Azure Disk Encryption hoz létre, és írja a hello titkosítási kulcsok tooyour kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="5feb2-348">When you need encryption toobe enabled on a running VM in Azure, Azure Disk Encryption generates and writes hello encryption keys tooyour key vault.</span></span> <span data-ttu-id="5feb2-349">A kulcstartót a titkosítási kulcsok kezelése az Azure AD-hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="5feb2-349">Managing encryption keys in your key vault requires Azure AD authentication.</span></span>

<span data-ttu-id="5feb2-350">Erre a célra hozzon létre egy Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5feb2-350">For this purpose, create an Azure AD application.</span></span> <span data-ttu-id="5feb2-351">Az alkalmazás regisztrálásához hello blogbejegyzés hello "Identitás beolvasása az alkalmazás hello" szakaszában található részletes lépéseket [Azure Key Vault - részletes](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="5feb2-351">You can find detailed steps for registering an application in hello “Get an Identity for hello Application” section of hello blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="5feb2-352">A feladás egy vagy több beállítása és konfigurálása a kulcstartót hasznos példák számos is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5feb2-352">This post also contains a number of helpful examples for setting up and configuring your key vault.</span></span> <span data-ttu-id="5feb2-353">Hitelesítési célokra is használhatja, vagy titkos kulcs-alapú ügyfélhitelesítés, vagy az Azure AD-alapú ügyfél-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="5feb2-353">For authentication purposes, you can use either client secret-based authentication or client certificate-based Azure AD authentication.</span></span>

#### <a name="client-secret-based-authentication-for-azure-ad"></a><span data-ttu-id="5feb2-354">Az Azure AD ügyfél titkos kulcs alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="5feb2-354">Client secret-based authentication for Azure AD</span></span>
<span data-ttu-id="5feb2-355">hello szakaszok segíthetnek az Azure AD egy ügyfél titkos kulcs alapú hitelesítés beállítása.</span><span class="sxs-lookup"><span data-stu-id="5feb2-355">hello sections that follow can help you configure a client secret-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a><span data-ttu-id="5feb2-356">Az Azure AD alkalmazás létrehozása az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-356">Create an Azure AD application by using Azure PowerShell</span></span>
<span data-ttu-id="5feb2-357">A következő PowerShell-parancsmag toocreate egy Azure AD-alkalmazást hello használata:</span><span class="sxs-lookup"><span data-stu-id="5feb2-357">Use hello following PowerShell cmdlet toocreate an Azure AD application:</span></span>

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> <span data-ttu-id="5feb2-358">$azureAdApplication.ApplicationId hello Azure AD ClientID és $aadClientSecret hello ügyfélkulcs újabb tooenable Azure Disk Encryption kell használni.</span><span class="sxs-lookup"><span data-stu-id="5feb2-358">$azureAdApplication.ApplicationId is hello Azure AD ClientID and $aadClientSecret is hello client secret that you should use later tooenable Azure Disk Encryption.</span></span> <span data-ttu-id="5feb2-359">Az Azure AD hello ügyfélkulcs megfelelően védelme.</span><span class="sxs-lookup"><span data-stu-id="5feb2-359">Safeguard hello Azure AD client secret appropriately.</span></span>

##### <a name="setting-up-hello-azure-ad-client-id-and-secret-from-hello-azure-classic-portal"></a><span data-ttu-id="5feb2-360">A klasszikus Azure portálon hello hello Azure AD ügyfél-azonosító és a titkos kulcs beállítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-360">Setting up hello Azure AD client ID and secret from hello Azure classic portal</span></span>
<span data-ttu-id="5feb2-361">Is állíthatja be az ügyfél-azonosító az Azure AD és a titkos kulcs hello segítségével [a klasszikus Azure portálon]( https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5feb2-361">You can also set up your Azure AD client ID and secret by using hello [Azure classic portal]( https://manage.windowsazure.com).</span></span> <span data-ttu-id="5feb2-362">tooperform ennek a feladatnak a, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-362">tooperform this task, do hello following:</span></span>

1. <span data-ttu-id="5feb2-363">Kattintson a hello **Active Directory** fülre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-363">Click hello **Active Directory** tab.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. <span data-ttu-id="5feb2-365">Kattintson a **alkalmazás hozzáadása**, és ezután hello alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="5feb2-365">Click **Add Application**, and then type hello application name.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. <span data-ttu-id="5feb2-367">Hello nyíl gombra, és adja meg hello alkalmazás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="5feb2-367">Click hello arrow button, and then configure hello application properties.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. <span data-ttu-id="5feb2-369">Kattintson a hello alacsonyabb bal sarok toofinish hello van jelölve.</span><span class="sxs-lookup"><span data-stu-id="5feb2-369">Click hello check mark in hello lower left corner toofinish.</span></span> <span data-ttu-id="5feb2-370">hello alkalmazás konfigurációs lap jelenik meg, és hello Azure AD ügyfél-azonosító hello aljához hello oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5feb2-370">hello application configuration page appears, and hello Azure AD client ID is displayed at hello bottom of hello page.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. <span data-ttu-id="5feb2-372">Mentse az Azure AD hello ügyfélkulcs hello kattintva **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5feb2-372">Save hello Azure AD client secret by clicking hello **Save** button.</span></span> <span data-ttu-id="5feb2-373">Megjegyzés: az Azure AD hello ügyfélkulcs hello kulcsok szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="5feb2-373">Note hello Azure AD client secret in hello keys text box.</span></span> <span data-ttu-id="5feb2-374">Annak megfelelően védelme.</span><span class="sxs-lookup"><span data-stu-id="5feb2-374">Safeguard it appropriately.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > <span data-ttu-id="5feb2-376">Attribútumfolyam megelőző hello hello a klasszikus Azure portálon nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-376">hello preceding flow is not supported on hello Azure classic portal.</span></span>

##### <a name="use-an-existing-application"></a><span data-ttu-id="5feb2-377">Használjon egy meglévő alkalmazást</span><span class="sxs-lookup"><span data-stu-id="5feb2-377">Use an existing application</span></span>
<span data-ttu-id="5feb2-378">tooexecute hello a következő parancsokat, beszerzése, és hello használata [Azure AD PowerShell modult](https://technet.microsoft.com/library/jj151815.aspx).</span><span class="sxs-lookup"><span data-stu-id="5feb2-378">tooexecute hello following commands, obtain and use hello [Azure AD PowerShell module](https://technet.microsoft.com/library/jj151815.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-379">hello következő parancsok kell végrehajtani egy új PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="5feb2-379">hello following commands must be executed from a new PowerShell window.</span></span> <span data-ttu-id="5feb2-380">Ne használja az Azure PowerShell vagy hello Azure Resource Manager ablak tooexecute hello parancsok.</span><span class="sxs-lookup"><span data-stu-id="5feb2-380">Do not use Azure PowerShell or hello Azure Resource Manager window tooexecute hello commands.</span></span> <span data-ttu-id="5feb2-381">Ez a megközelítés azt javasoljuk, mivel ezek a parancsmagok hello MSOnline modul- vagy Azure AD PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="5feb2-381">We recommend this approach because these cmdlets are in hello MSOnline module or Azure AD PowerShell.</span></span>

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a><span data-ttu-id="5feb2-382">Az Azure AD-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="5feb2-382">Certificate-based authentication for Azure AD</span></span>
> [!NOTE]
> <span data-ttu-id="5feb2-383">Az Azure AD-alapú hitelesítés jelenleg nem támogatott Linux virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="5feb2-383">Azure AD certificate-based authentication is currently not supported on Linux VMs.</span></span>

<span data-ttu-id="5feb2-384">hello a következő szakaszok megjelenítése hogyan tooconfigure egy az Azure AD-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="5feb2-384">hello sections that follow show how tooconfigure a certificate-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application"></a><span data-ttu-id="5feb2-385">Az Azure AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5feb2-385">Create an Azure AD application</span></span>
<span data-ttu-id="5feb2-386">toocreate egy Azure AD-alkalmazást, a következő PowerShell-parancsmagok hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="5feb2-386">toocreate an Azure AD application, execute hello following PowerShell cmdlets:</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-387">Cserélje le a következő hello `yourpassword` a biztonságos jelszó és a biztonságos működés érdekében hello jelszót karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="5feb2-387">Replace hello following `yourpassword` string with your secure password, and safeguard hello password.</span></span>

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

<span data-ttu-id="5feb2-388">Ez a lépés befejezése után PFX fájl tooyour kulcstároló feltöltése és hello hozzáférési házirend toodeploy adott tanúsítvány tooa VM engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5feb2-388">After you finish this step, upload a PFX file tooyour key vault and enable hello access policy needed toodeploy that certificate tooa VM.</span></span>

##### <a name="use-an-existing-azure-ad-application"></a><span data-ttu-id="5feb2-389">Egy Azure AD-alkalmazást használja</span><span class="sxs-lookup"><span data-stu-id="5feb2-389">Use an existing Azure AD application</span></span>
<span data-ttu-id="5feb2-390">Ha konfigurál egy meglévő alkalmazás tanúsítvány alapú hitelesítést, használja az itt látható hello PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="5feb2-390">If you are configuring certificate-based authentication for an existing application, use hello PowerShell cmdlets shown here.</span></span> <span data-ttu-id="5feb2-391">Lehet, hogy tooexecute őket a egy új PowerShell-ablakot.</span><span class="sxs-lookup"><span data-stu-id="5feb2-391">Be sure tooexecute them from a new PowerShell window.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

<span data-ttu-id="5feb2-392">Ez a lépés befejezése után PFX fájl tooyour kulcstároló feltöltése és toodeploy hello tanúsítvány tooa VM szükséges hello hozzáférési szabályzatának engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5feb2-392">After you finish this step, upload a PFX file tooyour key vault and enable hello access policy that's needed toodeploy hello certificate tooa VM.</span></span>

##### <a name="upload-a-pfx-file-tooyour-key-vault"></a><span data-ttu-id="5feb2-393">PFX fájl tooyour kulcstároló feltöltése</span><span class="sxs-lookup"><span data-stu-id="5feb2-393">Upload a PFX file tooyour key vault</span></span>
<span data-ttu-id="5feb2-394">Ez az eljárás részletes leírását, lásd: [hivatalos Azure Key Vault fejlesztői Blog hello](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span><span class="sxs-lookup"><span data-stu-id="5feb2-394">For a detailed explanation of this process, see [hello Official Azure Key Vault Team Blog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span></span> <span data-ttu-id="5feb2-395">Azonban a következő PowerShell-parancsmagok hello-e minden hello tevékenység van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5feb2-395">However, hello following PowerShell cmdlets are all you need for hello task.</span></span> <span data-ttu-id="5feb2-396">Lehet, hogy tooexecute őket az Azure PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="5feb2-396">Be sure tooexecute them from Azure PowerShell console.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-397">Cserélje le a következő hello `yourpassword` a biztonságos jelszó és a biztonságos működés érdekében hello jelszót karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="5feb2-397">Replace hello following `yourpassword` string with your secure password, and safeguard hello password.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-tooan-existing-vm"></a><span data-ttu-id="5feb2-398">A virtuális gép meglévő kulcstároló tooan a tanúsítvány központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5feb2-398">Deploy a certificate in your key vault tooan existing VM</span></span>
<span data-ttu-id="5feb2-399">Hello PFX feltöltése után a virtuális gép meglévő hello következőre hello kulcstároló tooan tanúsítvány központi telepítése:</span><span class="sxs-lookup"><span data-stu-id="5feb2-399">After you finish uploading hello PFX, deploy a certificate in hello key vault tooan existing VM with hello following:</span></span>
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-hello-key-vault-access-policy-for-hello-azure-ad-application"></a><span data-ttu-id="5feb2-400">Hello kulcstároló hozzáférési házirend hello Azure AD-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-400">Set up hello key vault access policy for hello Azure AD application</span></span>
<span data-ttu-id="5feb2-401">Az Azure AD-alkalmazást kell jogok tooaccess hello kulcsok vagy titkos hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="5feb2-401">Your Azure AD application needs rights tooaccess hello keys or secrets in hello vault.</span></span> <span data-ttu-id="5feb2-402">Használjon hello [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) parancsmag toogrant engedélyek toohello alkalmazás, hello ügyfél-azonosító (amely készítésének hello alkalmazás regisztrálásakor) használja, mint hello _– ServicePrincipalName_ a paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="5feb2-402">Use hello [`Set-AzureKeyVaultAccessPolicy`](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet toogrant permissions toohello application, using hello client ID (which was generated when hello application was registered) as hello _–ServicePrincipalName_ parameter value.</span></span> <span data-ttu-id="5feb2-403">toolearn több, tekintse meg a hello blogbejegyzésben [Azure Key Vault - részletes](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="5feb2-403">toolearn more, see hello blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="5feb2-404">Íme egy példa hogyan tooperform ennek a feladatnak a keresztül PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5feb2-404">Here is an example of how tooperform this task via PowerShell:</span></span>

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> <span data-ttu-id="5feb2-405">Az Azure Disk Encryption igényel a következő hozzáférési házirendek az Azure AD tooyour ügyfélalkalmazás tooconfigure hello: _WrapKey_ és _beállítása_ engedélyek.</span><span class="sxs-lookup"><span data-stu-id="5feb2-405">Azure Disk Encryption requires you tooconfigure hello following access policies tooyour Azure AD client application: _WrapKey_ and _Set_ permissions.</span></span>

## <a name="terminology"></a><span data-ttu-id="5feb2-406">Terminológia</span><span class="sxs-lookup"><span data-stu-id="5feb2-406">Terminology</span></span>
<span data-ttu-id="5feb2-407">néhány gyakori használati hello használják ezt a technológiát használja hello következő terminológia táblázat toounderstand:</span><span class="sxs-lookup"><span data-stu-id="5feb2-407">toounderstand some of hello common terms used by this technology, use hello following terminology table:</span></span>

| <span data-ttu-id="5feb2-408">Terminológia</span><span class="sxs-lookup"><span data-stu-id="5feb2-408">Terminology</span></span> | <span data-ttu-id="5feb2-409">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="5feb2-409">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="5feb2-410">Azure AD</span><span class="sxs-lookup"><span data-stu-id="5feb2-410">Azure AD</span></span> | <span data-ttu-id="5feb2-411">Azure ad [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="5feb2-411">Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span></span> <span data-ttu-id="5feb2-412">Az Azure AD-fiókot hitelesítéséhez, tárolásának és titkos kulcsok beolvasása a kulcstároló előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="5feb2-412">An Azure AD account is a prerequisite for authenticating, storing, and retrieving secrets from a key vault.</span></span> |
| <span data-ttu-id="5feb2-413">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5feb2-413">Azure Key Vault</span></span> | <span data-ttu-id="5feb2-414">Key Vault egy kriptográfiai, kulcs szolgáltatást a Federal Information Processing szabványok FIPS érvényesített hardveres biztonsági modulok, amely segítségével megakadályozhatja a kriptográfiai kulcsokat és a bizalmas titkos alapul.</span><span class="sxs-lookup"><span data-stu-id="5feb2-414">Key Vault is a cryptographic, key management service that's based on Federal Information Processing Standards (FIPS)-validated hardware security modules, which help safeguard your cryptographic keys and sensitive secrets.</span></span> <span data-ttu-id="5feb2-415">További információkért lásd: [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5feb2-415">For more information, see [Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="5feb2-416">ARM</span><span class="sxs-lookup"><span data-stu-id="5feb2-416">ARM</span></span> | <span data-ttu-id="5feb2-417">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5feb2-417">Azure Resource Manager</span></span> |
| <span data-ttu-id="5feb2-418">A BitLocker</span><span class="sxs-lookup"><span data-stu-id="5feb2-418">BitLocker</span></span> |<span data-ttu-id="5feb2-419">[A BitLocker](https://technet.microsoft.com/library/hh831713.aspx) egy iparági felismerhető Windows kötet titkosítási technológia, amely Windows IaaS virtuális gépeken tooenable lemeztitkosítás használatban van.</span><span class="sxs-lookup"><span data-stu-id="5feb2-419">[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is an industry-recognized Windows volume encryption technology that's used tooenable disk encryption on Windows IaaS VMs.</span></span> |
| <span data-ttu-id="5feb2-420">BEK</span><span class="sxs-lookup"><span data-stu-id="5feb2-420">BEK</span></span> | <span data-ttu-id="5feb2-421">A BitLocker titkosítási kulcsai használt tooencrypt hello az operációs rendszer rendszerindító kötet és az adatköteteket.</span><span class="sxs-lookup"><span data-stu-id="5feb2-421">BitLocker encryption keys are used tooencrypt hello OS boot volume and data volumes.</span></span> <span data-ttu-id="5feb2-422">hello BitLocker-kulcsok elő a kulcstároló, a titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="5feb2-422">hello BitLocker keys are safeguarded in a key vault as secrets.</span></span> |
| <span data-ttu-id="5feb2-423">parancssori felület</span><span class="sxs-lookup"><span data-stu-id="5feb2-423">CLI</span></span> | <span data-ttu-id="5feb2-424">Lásd: [Azure parancssori felület](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5feb2-424">See [Azure command-line interface](../cli-install-nodejs.md).</span></span> |
| <span data-ttu-id="5feb2-425">DM-crypt program segítségével</span><span class="sxs-lookup"><span data-stu-id="5feb2-425">DM-Crypt</span></span> |<span data-ttu-id="5feb2-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) hello Linux-alapú, átlátszó lemeztitkosítás alrendszer tooenable lemeztitkosítás által használt Linux IaaS virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="5feb2-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is hello Linux-based, transparent disk-encryption subsystem that's used tooenable disk encryption on Linux IaaS VMs.</span></span> |
| <span data-ttu-id="5feb2-427">KEK</span><span class="sxs-lookup"><span data-stu-id="5feb2-427">KEK</span></span> | <span data-ttu-id="5feb2-428">Kulcstitkosítás hello aszimmetrikus kulcs (RSA 2048), hogy tooprotect használhatja, vagy burkolása hello titkos kulcsa.</span><span class="sxs-lookup"><span data-stu-id="5feb2-428">Key encryption key is hello asymmetric key (RSA 2048) that you can use tooprotect or wrap hello secret.</span></span> <span data-ttu-id="5feb2-429">Megadhat egy hardveres biztonsági modulok (HSM)-kulcs vagy kulcs szoftveres védelemmel ellátott védett.</span><span class="sxs-lookup"><span data-stu-id="5feb2-429">You can provide a hardware security modules (HSM)-protected key or software-protected key.</span></span> <span data-ttu-id="5feb2-430">További részletekért lásd: [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5feb2-430">For more details, see [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="5feb2-431">PS parancsmagok</span><span class="sxs-lookup"><span data-stu-id="5feb2-431">PS cmdlets</span></span> | <span data-ttu-id="5feb2-432">Lásd: [Azure PowerShell-parancsmagok](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5feb2-432">See [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span> |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a><span data-ttu-id="5feb2-433">Állítson be és az Azure Disk Encryption key vaultban konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5feb2-433">Set up and configure your key vault for Azure Disk Encryption</span></span>
<span data-ttu-id="5feb2-434">Az Azure Disk Encryption segíti biztonságos működés érdekében hello lemez-titkosítási kulcsok és titkos key vaultban lévő.</span><span class="sxs-lookup"><span data-stu-id="5feb2-434">Azure Disk Encryption helps safeguard hello disk-encryption keys and secrets in your key vault.</span></span> <span data-ttu-id="5feb2-435">tooset mentése az Azure Disk Encryption key vaultban, teljes hello az egyes szakaszok a következő hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="5feb2-435">tooset up your key vault for Azure Disk Encryption, complete hello steps in each of hello following sections.</span></span>

#### <a name="create-a-key-vault"></a><span data-ttu-id="5feb2-436">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5feb2-436">Create a key vault</span></span>
<span data-ttu-id="5feb2-437">Kulcstároló, toocreate hello alábbi beállítások egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="5feb2-437">toocreate a key vault, use one of hello following options:</span></span>

* [<span data-ttu-id="5feb2-438">"101-kulcs-tároló-" Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="5feb2-438">"101-Key-Vault-Create" Resource Manager template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [<span data-ttu-id="5feb2-439">Az Azure PowerShell kulcstároló-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="5feb2-439">Azure PowerShell key vault cmdlets</span></span>](/powershell/module/azurerm.keyvault/#key_vault)
* <span data-ttu-id="5feb2-440">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5feb2-440">Azure Resource Manager</span></span>
* <span data-ttu-id="5feb2-441">Hogyan túl[a kulcstartót biztonságos](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span><span class="sxs-lookup"><span data-stu-id="5feb2-441">How too[Secure your key vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-442">Ha már állított be kulcstároló az előfizetéséhez, hagyja ki toohello következő szakaszt.</span><span class="sxs-lookup"><span data-stu-id="5feb2-442">If you have already set up a key vault for your subscription, skip toohello next section.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a><span data-ttu-id="5feb2-444">Állítsa be a titkosítás kulcsot (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="5feb2-444">Set up a key encryption key (optional)</span></span>
<span data-ttu-id="5feb2-445">Ha egy KEK toouse hello BitLocker titkosítási kulcsok biztonsági szint, vegye fel a KEK tooyour kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="5feb2-445">If you want toouse a KEK for an additional layer of security for hello BitLocker encryption keys, add a KEK tooyour key vault.</span></span> <span data-ttu-id="5feb2-446">Használjon hello [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) parancsmag toocreate hello a key vaultban egy kulcs titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="5feb2-446">Use hello [`Add-AzureKeyVaultKey`](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet toocreate a key encryption key in hello key vault.</span></span> <span data-ttu-id="5feb2-447">A helyszíni kulcskezelő hardveres biztonsági MODULT is importálhat egy KEK.</span><span class="sxs-lookup"><span data-stu-id="5feb2-447">You can also import a KEK from your on-premises key management HSM.</span></span> <span data-ttu-id="5feb2-448">További részletekért lásd: [Key Vault dokumentáció](https://azure.microsoft.com/documentation/services/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="5feb2-448">For more details, see [Key Vault Documentation](https://azure.microsoft.com/documentation/services/key-vault/).</span></span>

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

<span data-ttu-id="5feb2-449">Hello KEK tooAzure erőforrás-kezelő címen, vagy a kulcstartót felhasználói felületének használatával adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="5feb2-449">You can add hello KEK by going tooAzure Resource Manager or by using your key vault interface.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a><span data-ttu-id="5feb2-451">Kulcstároló engedélyek beállítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-451">Set key vault permissions</span></span>
<span data-ttu-id="5feb2-452">hello Azure platformon kell hozzáférést toohello titkosítási kulcsok vagy titkos kulcsainak a kulcstartót toomake őket elérhető toohello virtuális Gépet a rendszerindítás és hello kötetek visszafejtése.</span><span class="sxs-lookup"><span data-stu-id="5feb2-452">hello Azure platform needs access toohello encryption keys or secrets in your key vault toomake them available toohello VM for booting and decrypting hello volumes.</span></span> <span data-ttu-id="5feb2-453">toogrant engedélyek toohello Azure platformon, set hello **EnabledForDiskEncryption** tulajdonság hello kulcs tároló hello kulcstároló PowerShell parancsmag segítségével:</span><span class="sxs-lookup"><span data-stu-id="5feb2-453">toogrant permissions toohello Azure platform, set hello **EnabledForDiskEncryption** property in hello key vault by using hello key vault PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

<span data-ttu-id="5feb2-454">Azt is beállíthatja hello **EnabledForDiskEncryption** hello felkeresésével tulajdonság [Azure erőforrás-kezelő](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5feb2-454">You can also set hello **EnabledForDiskEncryption** property by visiting hello [Azure Resource Explorer](https://resources.azure.com).</span></span>

<span data-ttu-id="5feb2-455">Amint azt korábban említettük, meg kell adni a hello **EnabledForDiskEncryption** a kulcstartót tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="5feb2-455">As mentioned earlier, you must set hello **EnabledForDiskEncryption** property on your key vault.</span></span> <span data-ttu-id="5feb2-456">Ellenkező esetben hello központi telepítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5feb2-456">Otherwise, hello deployment will fail.</span></span>

<span data-ttu-id="5feb2-457">Beállíthat hozzáférési házirendek az Azure AD alkalmazás hello kulcstároló felületről itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="5feb2-457">You can set up access policies for your Azure AD application from hello key vault interface, as shown here:</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

<span data-ttu-id="5feb2-460">A hello **speciális hozzáférési házirendek** lapra, győződjön meg arról, hogy a kulcstartót engedélyezve van-e az Azure Disk Encryption:</span><span class="sxs-lookup"><span data-stu-id="5feb2-460">On hello **Advanced access policies** tab, make sure that your key vault is enabled for Azure Disk Encryption:</span></span>

![Az Azure key vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a><span data-ttu-id="5feb2-462">Lemeztitkosítás üzembe helyezési forgatókönyvek és a felhasználói élmény</span><span class="sxs-lookup"><span data-stu-id="5feb2-462">Disk-encryption deployment scenarios and user experiences</span></span>
<span data-ttu-id="5feb2-463">Sok lemeztitkosítás lehetőségeket engedélyezhet, és hello lépések eltérhetnek függően toohello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="5feb2-463">You can enable many disk-encryption scenarios, and hello steps may vary according toohello scenario.</span></span> <span data-ttu-id="5feb2-464">a következő szakaszok hello forgatókönyveket is lefedi nagyobb részletességgel hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-464">hello following sections cover hello scenarios in greater detail.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-hello-marketplace"></a><span data-ttu-id="5feb2-465">Új IaaS virtuális gépeket a piactér hello létrehozott titkosításának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5feb2-465">Enable encryption on new IaaS VMs that are created from hello Marketplace</span></span>
<span data-ttu-id="5feb2-466">Hello segítségével engedélyezheti az új IaaS Windows virtuális gép hello Azure Piactérről származó lemeztitkosítás [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span><span class="sxs-lookup"><span data-stu-id="5feb2-466">You can enable disk encryption on new IaaS Windows VM from hello Marketplace in Azure by using hello  [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span></span>

1. <span data-ttu-id="5feb2-467">Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello titkosítási konfigurációs hello **paraméterek** panelt, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-467">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5feb2-468">Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** tooenable titkosítási egy új IaaS virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5feb2-468">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on a new IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-469">Ez a sablon létrehoz egy új titkosított Windows virtuális Gépet, amely a Windows Server 2012 hello gyűjtemény lemezképet használja.</span><span class="sxs-lookup"><span data-stu-id="5feb2-469">This template creates a new encrypted Windows VM that uses hello Windows Server 2012 gallery image.</span></span>

<span data-ttu-id="5feb2-470">Segítségével engedélyezheti az adatok titkosítása a új IaaS RedHat Linux 7.2 rendszerű virtuális gép egy 200 GB-os RAID-0 tömbbel rendelkező [Resource Manager-sablon](https://aka.ms/fde-rhel).</span><span class="sxs-lookup"><span data-stu-id="5feb2-470">You can enable disk encryption on a new IaaS RedHat Linux 7.2 VM with a 200-GB RAID-0 array by using this [Resource Manager template](https://aka.ms/fde-rhel).</span></span> <span data-ttu-id="5feb2-471">Hello sablon telepítése után ellenőrizze a hello VM titkosítási állapotát hello segítségével `Get-AzureRmVmDiskEncryptionStatus` parancsmag, a [az operációs rendszer titkosított meghajtó a futó Linux virtuális gép](#encrypting-os-drive-on-a-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="5feb2-471">After you deploy hello template, verify hello VM encryption status by using hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet, as described in [Encrypting OS drive on a running Linux VM](#encrypting-os-drive-on-a-running-linux-vm).</span></span> <span data-ttu-id="5feb2-472">Ha hello gép állapotot ad vissza _VMRestartPending_, indítsa újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-472">When hello machine returns a status of _VMRestartPending_, restart hello VM.</span></span>

<span data-ttu-id="5feb2-473">hello alábbi táblázat hello erőforrás-kezelő Sablonparaméterek hello piactér forgatókönyvet az Azure AD ügyfél-azonosító segítségével új virtuális gépek esetén:</span><span class="sxs-lookup"><span data-stu-id="5feb2-473">hello following table lists hello Resource Manager template parameters for new VMs from hello Marketplace scenario using Azure AD client ID:</span></span>

| <span data-ttu-id="5feb2-474">Paraméter</span><span class="sxs-lookup"><span data-stu-id="5feb2-474">Parameter</span></span> | <span data-ttu-id="5feb2-475">Leírás</span><span class="sxs-lookup"><span data-stu-id="5feb2-475">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5feb2-476">adminUserName</span><span class="sxs-lookup"><span data-stu-id="5feb2-476">adminUserName</span></span> | <span data-ttu-id="5feb2-477">Rendszergazda felhasználó hello virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="5feb2-477">Admin user name for hello virtual machine.</span></span> |
| <span data-ttu-id="5feb2-478">adminPassword</span><span class="sxs-lookup"><span data-stu-id="5feb2-478">adminPassword</span></span> | <span data-ttu-id="5feb2-479">Rendszergazdai jelszóval hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="5feb2-479">Admin user password for hello virtual machine.</span></span> |
| <span data-ttu-id="5feb2-480">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="5feb2-480">newStorageAccountName</span></span> | <span data-ttu-id="5feb2-481">Hello tárolási fiók toostore az operációs rendszer és a VHD-k adatokat neve.</span><span class="sxs-lookup"><span data-stu-id="5feb2-481">Name of hello storage account toostore OS and data VHDs.</span></span> |
| <span data-ttu-id="5feb2-482">vmSize</span><span class="sxs-lookup"><span data-stu-id="5feb2-482">vmSize</span></span> | <span data-ttu-id="5feb2-483">Hello virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="5feb2-483">Size of hello VM.</span></span> <span data-ttu-id="5feb2-484">Jelenleg csak a Standard A, a D és G adatsorozat támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5feb2-484">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="5feb2-485">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="5feb2-485">virtualNetworkName</span></span> | <span data-ttu-id="5feb2-486">Virtuális hálózat a virtuális gép hálózati hello hello nevét kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="5feb2-486">Name of hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="5feb2-487">subnetName</span><span class="sxs-lookup"><span data-stu-id="5feb2-487">subnetName</span></span> | <span data-ttu-id="5feb2-488">A virtuális hálózat a virtuális gép hálózati hello hello hello alhálózat nevét kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="5feb2-488">Name of hello subnet in hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="5feb2-489">AADClientID</span><span class="sxs-lookup"><span data-stu-id="5feb2-489">AADClientID</span></span> | <span data-ttu-id="5feb2-490">Engedélyek toowrite titkok tooyour kulcstároló hello Azure AD alkalmazás ügyfél-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5feb2-490">Client ID of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="5feb2-491">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="5feb2-491">AADClientSecret</span></span> | <span data-ttu-id="5feb2-492">Hello Azure AD-alkalmazás, amely rendelkezik engedélyekkel toowrite titkok tooyour kulcstároló titkos ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="5feb2-492">Client secret of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="5feb2-493">keyVaultURL</span><span class="sxs-lookup"><span data-stu-id="5feb2-493">keyVaultURL</span></span> | <span data-ttu-id="5feb2-494">Hello kulcs URL-tároló adott hello BitLocker kulcs fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="5feb2-494">URL of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="5feb2-495">Hello parancsmaggal kaphat `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-495">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span></span> |
| <span data-ttu-id="5feb2-496">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="5feb2-496">keyEncryptionKeyURL</span></span> | <span data-ttu-id="5feb2-497">Hello fő titkosítási kulcsát használt tooencrypt hello URL-címe jönnek létre, a BitLocker-kulcsot (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="5feb2-497">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key (optional).</span></span> |
| <span data-ttu-id="5feb2-498">keyVaultResourceGroup</span><span class="sxs-lookup"><span data-stu-id="5feb2-498">keyVaultResourceGroup</span></span> | <span data-ttu-id="5feb2-499">Hello kulcstároló erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5feb2-499">Resource group of hello key vault.</span></span> |
| <span data-ttu-id="5feb2-500">vmName</span><span class="sxs-lookup"><span data-stu-id="5feb2-500">vmName</span></span> | <span data-ttu-id="5feb2-501">Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-501">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="5feb2-502">_KeyEncryptionKeyURL_ egy nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="5feb2-502">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="5feb2-503">A key vaultban helyezheti a saját KEK toofurther védelmi hello adattitkosítási kulcsot (jelszó titkos).</span><span class="sxs-lookup"><span data-stu-id="5feb2-503">You can bring your own KEK toofurther safeguard hello data encryption key (Passphrase secret) in your key vault.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a><span data-ttu-id="5feb2-504">Új IaaS virtuális gépeket ügyfél titkosított virtuális merevlemez és a titkosítási kulcsokat a létrehozott titkosításának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5feb2-504">Enable encryption on new IaaS VMs that are created from customer-encrypted VHD and encryption keys</span></span>
<span data-ttu-id="5feb2-505">Ebben a forgatókönyvben engedélyezheti a hello Resource Manager-sablon, a PowerShell-parancsmagokkal vagy a parancssori felület parancsait használatával titkosított.</span><span class="sxs-lookup"><span data-stu-id="5feb2-505">In this scenario, you can enable encrypting by using hello Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="5feb2-506">hello alábbi szakaszok ismertetik a nagyobb részletességgel hello Resource Manager-sablon és a parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="5feb2-506">hello following sections explain in greater detail hello Resource Manager template and CLI commands.</span></span>

<span data-ttu-id="5feb2-507">Hello követésével egy ezekben a szakaszokban való felkészülés előzetes titkosítással képek használható az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5feb2-507">Follow hello instructions from one of these sections for preparing pre-encrypted images that can be used in Azure.</span></span> <span data-ttu-id="5feb2-508">Hello lemezkép létrehozása után hello következő szakasz toocreate egy titkosított Azure virtuális gép hello lépéseket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5feb2-508">After hello image is created, you can use hello steps in hello next section toocreate an encrypted Azure VM.</span></span>

* [<span data-ttu-id="5feb2-509">Előzetes titkosítással Windows virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="5feb2-509">Prepare a pre-encrypted Windows VHD</span></span>](#preparing-a-pre-encrypted-windows-vhd)
* [<span data-ttu-id="5feb2-510">Előzetes titkosítással Linux virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="5feb2-510">Prepare a pre-encrypted Linux VHD</span></span>](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-hello-resource-manager-template"></a><span data-ttu-id="5feb2-511">Hello Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-511">Using hello Resource Manager template</span></span>
<span data-ttu-id="5feb2-512">Hello használatával a titkosított virtuális merevlemezen lemeztitkosítás is engedélyezheti [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span><span class="sxs-lookup"><span data-stu-id="5feb2-512">You can enable disk encryption on your encrypted VHD by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span></span>

1. <span data-ttu-id="5feb2-513">Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello titkosítási konfigurációs hello **paraméterek** panelt, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-513">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5feb2-514">Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** tooenable titkosítás hello új IaaS virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="5feb2-514">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello new IaaS VM.</span></span>

<span data-ttu-id="5feb2-515">hello alábbi táblázat hello Resource Manager sablon paramétereinek a titkosított virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="5feb2-515">hello following table lists hello Resource Manager template parameters for your encrypted VHD:</span></span>

| <span data-ttu-id="5feb2-516">Paraméter</span><span class="sxs-lookup"><span data-stu-id="5feb2-516">Parameter</span></span> | <span data-ttu-id="5feb2-517">Leírás</span><span class="sxs-lookup"><span data-stu-id="5feb2-517">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5feb2-518">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="5feb2-518">newStorageAccountName</span></span> | <span data-ttu-id="5feb2-519">Hello tárolási fiók toostore hello neve titkosítja az operációs rendszer virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="5feb2-519">Name of hello storage account toostore hello encrypted OS VHD.</span></span> <span data-ttu-id="5feb2-520">Ezt a tárfiókot kell már létrehozott hello ugyanazt az erőforráscsoportot és hello VM ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="5feb2-520">This storage account should already have been created in hello same resource group and same location as hello VM.</span></span> |
| <span data-ttu-id="5feb2-521">osVhdUri</span><span class="sxs-lookup"><span data-stu-id="5feb2-521">osVhdUri</span></span> | <span data-ttu-id="5feb2-522">Az operációs rendszer virtuális merevlemez hello hello tárfiókból URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="5feb2-522">URI of hello OS VHD from hello storage account.</span></span> |
| <span data-ttu-id="5feb2-523">osType</span><span class="sxs-lookup"><span data-stu-id="5feb2-523">osType</span></span> | <span data-ttu-id="5feb2-524">Az operációs rendszer terméktípusának (Windows/Linux).</span><span class="sxs-lookup"><span data-stu-id="5feb2-524">OS product type (Windows/Linux).</span></span> |
| <span data-ttu-id="5feb2-525">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="5feb2-525">virtualNetworkName</span></span> | <span data-ttu-id="5feb2-526">Virtuális hálózat a virtuális gép hálózati hello hello nevét kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="5feb2-526">Name of hello VNet that hello VM NIC should belong to.</span></span> <span data-ttu-id="5feb2-527">hello nevét kell már létrehozott hello ugyanazt az erőforráscsoportot és hello VM ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="5feb2-527">hello name should already have been created in hello same resource group and same location as hello VM.</span></span> |
| <span data-ttu-id="5feb2-528">subnetName</span><span class="sxs-lookup"><span data-stu-id="5feb2-528">subnetName</span></span> | <span data-ttu-id="5feb2-529">Hello alhálózatot a virtuális hálózat a virtuális gép hálózati hello hello nevét kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="5feb2-529">Name of hello subnet on hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="5feb2-530">vmSize</span><span class="sxs-lookup"><span data-stu-id="5feb2-530">vmSize</span></span> | <span data-ttu-id="5feb2-531">Hello virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="5feb2-531">Size of hello VM.</span></span> <span data-ttu-id="5feb2-532">Jelenleg csak a Standard A, a D és G adatsorozat támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5feb2-532">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="5feb2-533">keyVaultResourceID</span><span class="sxs-lookup"><span data-stu-id="5feb2-533">keyVaultResourceID</span></span> | <span data-ttu-id="5feb2-534">hello ResourceID hello kulcstároló erőforrásához az Azure Resource Manager azonosító.</span><span class="sxs-lookup"><span data-stu-id="5feb2-534">hello ResourceID that identifies hello key vault resource in Azure Resource Manager.</span></span> <span data-ttu-id="5feb2-535">Hello PowerShell parancsmag használatával kaphat `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-535">You can get it by using hello PowerShell cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span></span> |
| <span data-ttu-id="5feb2-536">keyVaultSecretUrl</span><span class="sxs-lookup"><span data-stu-id="5feb2-536">keyVaultSecretUrl</span></span> | <span data-ttu-id="5feb2-537">Hello lemez-titkosítási kulcs, amely be van állítva a hello kulcstároló URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="5feb2-537">URL of hello disk-encryption key that's set up in hello key vault.</span></span> |
| <span data-ttu-id="5feb2-538">keyVaultKekUrl</span><span class="sxs-lookup"><span data-stu-id="5feb2-538">keyVaultKekUrl</span></span> | <span data-ttu-id="5feb2-539">Hello kulcs titkosítási kulcs jön létre hello lemez-titkosítási kulcs titkosításához URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="5feb2-539">URL of hello key encryption key for encrypting hello generated disk-encryption key.</span></span> |
| <span data-ttu-id="5feb2-540">vmName</span><span class="sxs-lookup"><span data-stu-id="5feb2-540">vmName</span></span> | <span data-ttu-id="5feb2-541">Hello infrastruktúra-szolgáltatási virtuális gép neve.</span><span class="sxs-lookup"><span data-stu-id="5feb2-541">Name of hello IaaS VM.</span></span> |

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="5feb2-542">PowerShell-parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-542">Using PowerShell cmdlets</span></span>
<span data-ttu-id="5feb2-543">Engedélyezheti lemeztitkosítás a titkosított virtuális merevlemezen hello PowerShell-parancsmaggal [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="5feb2-543">You can enable disk encryption on your encrypted VHD by using hello PowerShell cmdlet [`Set-AzureRmVMOSDisk`](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span>  

#### <a name="using-cli-commands"></a><span data-ttu-id="5feb2-544">Parancssori felület parancsai használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-544">Using CLI commands</span></span>
<span data-ttu-id="5feb2-545">Ebben a forgatókönyvben a parancssori felület parancsait használva tooenable lemeztitkosítás hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-545">tooenable disk encryption for this scenario by using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="5feb2-546">A kulcstároló hozzáférési házirendek meg:</span><span class="sxs-lookup"><span data-stu-id="5feb2-546">Set access policies in your key vault:</span></span>

   * <span data-ttu-id="5feb2-547">Set hello **EnabledForDiskEncryption** jelző:</span><span class="sxs-lookup"><span data-stu-id="5feb2-547">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="5feb2-548">Állítsa be az engedélyeket tooAzure AD alkalmazás toowrite titkok tooyour kulcstároló:</span><span class="sxs-lookup"><span data-stu-id="5feb2-548">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="5feb2-549">a titkosítás tooenable egy meglévő vagy a futó virtuális Gépre, típus:</span><span class="sxs-lookup"><span data-stu-id="5feb2-549">tooenable encryption on an existing or running VM, type:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="5feb2-550">Titkosítási állapot beolvasása:</span><span class="sxs-lookup"><span data-stu-id="5feb2-550">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="5feb2-551">egy új virtuális Gépet, a titkosított virtuális merevlemezről, a következő paramétereket hello használata hello tooenable titkosítás `azure vm create` parancs:</span><span class="sxs-lookup"><span data-stu-id="5feb2-551">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a><span data-ttu-id="5feb2-552">Meglévő vagy az Azure-beli infrastruktúra-szolgáltatási Windows virtuális gép titkosításának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5feb2-552">Enable encryption on existing or running IaaS Windows VM in Azure</span></span>
<span data-ttu-id="5feb2-553">Ebben a forgatókönyvben engedélyezheti a hello Resource Manager-sablon, a PowerShell-parancsmagokkal vagy a parancssori felület parancsait használatával titkosított.</span><span class="sxs-lookup"><span data-stu-id="5feb2-553">In this scenario, you can enable encrypting by using hello Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="5feb2-554">hello alábbi szakaszok ismertetik részletesebben hogyan azt a Resource Manager-sablon és a parancssori felület parancsait hello tooenable.</span><span class="sxs-lookup"><span data-stu-id="5feb2-554">hello following sections explain in greater detail how tooenable it by using hello Resource Manager template and CLI commands.</span></span>

#### <a name="using-hello-resource-manager-template"></a><span data-ttu-id="5feb2-555">Hello Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-555">Using hello Resource Manager template</span></span>
<span data-ttu-id="5feb2-556">Engedélyezheti a meglévő vagy az Azure-beli infrastruktúra-szolgáltatási Windows virtuális gépek hello segítségével lemeztitkosítás [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="5feb2-556">You can enable disk encryption on existing or running IaaS Windows VMs in Azure by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="5feb2-557">Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello titkosítási konfigurációs hello **paraméterek** panelt, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-557">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5feb2-558">Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** hello meglévő vagy az infrastruktúra-szolgáltatási virtuális gép fut tooenable titkosítás.</span><span class="sxs-lookup"><span data-stu-id="5feb2-558">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello existing or running IaaS VM.</span></span>

<span data-ttu-id="5feb2-559">hello alábbi táblázat hello Resource Manager sablon paramétereinek meglévő vagy az Azure AD ügyfél-Azonosítót használó virtuális gépeken futó:</span><span class="sxs-lookup"><span data-stu-id="5feb2-559">hello following table lists hello Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="5feb2-560">Paraméter</span><span class="sxs-lookup"><span data-stu-id="5feb2-560">Parameter</span></span> | <span data-ttu-id="5feb2-561">Leírás</span><span class="sxs-lookup"><span data-stu-id="5feb2-561">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5feb2-562">AADClientID</span><span class="sxs-lookup"><span data-stu-id="5feb2-562">AADClientID</span></span> | <span data-ttu-id="5feb2-563">Engedélyek toowrite titkok toohello kulcstároló hello Azure AD alkalmazás ügyfél-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5feb2-563">Client ID of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="5feb2-564">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="5feb2-564">AADClientSecret</span></span> | <span data-ttu-id="5feb2-565">Hello Azure AD-alkalmazás, amely rendelkezik engedélyekkel toowrite titkok toohello kulcstároló titkos ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="5feb2-565">Client secret of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="5feb2-566">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="5feb2-566">keyVaultName</span></span> | <span data-ttu-id="5feb2-567">Hello kulcs neve tároló adott hello BitLocker kulcs fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="5feb2-567">Name of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="5feb2-568">Hello parancsmaggal kaphat `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-568">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="5feb2-569">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="5feb2-569">keyEncryptionKeyURL</span></span> | <span data-ttu-id="5feb2-570">Hello fő titkosítási kulcsát használt tooencrypt hello URL-CÍMÉT a BitLocker kulcs jön létre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-570">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key.</span></span> <span data-ttu-id="5feb2-571">Ez a paraméter nem kötelező, ha **nokek** hello UseExistingKek legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="5feb2-571">This parameter is optional if you select **nokek** in hello UseExistingKek drop-down list.</span></span> <span data-ttu-id="5feb2-572">Ha **kek** hello UseExistingKek legördülő listában, meg kell adnia hello _keyEncryptionKeyURL_ érték.</span><span class="sxs-lookup"><span data-stu-id="5feb2-572">If you select **kek** in hello UseExistingKek drop-down list, you must enter hello _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="5feb2-573">volumeType</span><span class="sxs-lookup"><span data-stu-id="5feb2-573">volumeType</span></span> | <span data-ttu-id="5feb2-574">Hello titkosítási műveletet végzi el a kötet típusa.</span><span class="sxs-lookup"><span data-stu-id="5feb2-574">Type of volume that hello encryption operation is performed on.</span></span> <span data-ttu-id="5feb2-575">Érvényes értékek a következők _OS_, _adatok_, és _összes_.</span><span class="sxs-lookup"><span data-stu-id="5feb2-575">Valid values are _OS_, _Data_, and _All_.</span></span> |
| <span data-ttu-id="5feb2-576">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="5feb2-576">sequenceVersion</span></span> | <span data-ttu-id="5feb2-577">A BitLocker művelet hello feladatütemezési verzióját.</span><span class="sxs-lookup"><span data-stu-id="5feb2-577">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="5feb2-578">Ez a verziószám növelése minden alkalommal, amikor egy lemezt-titkosítási művelet történik hello azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5feb2-578">Increment this version number every time a disk-encryption operation is performed on hello same VM.</span></span> |
| <span data-ttu-id="5feb2-579">vmName</span><span class="sxs-lookup"><span data-stu-id="5feb2-579">vmName</span></span> | <span data-ttu-id="5feb2-580">Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-580">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="5feb2-581">_KeyEncryptionKeyURL_ egy nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="5feb2-581">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="5feb2-582">A key vaultban hello helyezheti a saját KEK toofurther védelmi hello adattitkosítási kulcsot (a BitLocker titkosítási titkos).</span><span class="sxs-lookup"><span data-stu-id="5feb2-582">You can bring your own KEK toofurther safeguard hello data encryption key (BitLocker encryption secret) in hello key vault.</span></span>

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="5feb2-583">PowerShell-parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-583">Using PowerShell cmdlets</span></span>
<span data-ttu-id="5feb2-584">Az Azure Disk Encryption titkosítás engedélyezése a PowerShell-parancsmagok használatával kapcsolatos információkért lásd: hello blogbejegyzések [Azure Disk Encryption megismerkedhet az Azure PowerShell - 1. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) és [Azure Disk Encryption felfedezés az Azure PowerShell - 2. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="5feb2-584">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see hello blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

#### <a name="using-cli-commands"></a><span data-ttu-id="5feb2-585">Parancssori felület parancsai használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-585">Using CLI commands</span></span>
<span data-ttu-id="5feb2-586">a meglévő vagy infrastruktúra-szolgáltatási Windows virtuális gép fut az Azure-ban a parancssori felület parancsait tooenable titkosítási hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-586">tooenable encryption on existing or running IaaS Windows VM in Azure using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="5feb2-587">a key vaultban hello tooset a hozzáférési házirendek:</span><span class="sxs-lookup"><span data-stu-id="5feb2-587">tooset access policies in hello key vault:</span></span>
   * <span data-ttu-id="5feb2-588">Set hello **EnabledForDiskEncryption** jelző:</span><span class="sxs-lookup"><span data-stu-id="5feb2-588">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="5feb2-589">Állítsa be az engedélyeket tooAzure AD alkalmazás toowrite titkok tooyour kulcstároló:</span><span class="sxs-lookup"><span data-stu-id="5feb2-589">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. <span data-ttu-id="5feb2-590">egy meglévő vagy a futó virtuális gép tooenable titkosítás:</span><span class="sxs-lookup"><span data-stu-id="5feb2-590">tooenable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. <span data-ttu-id="5feb2-591">tooget titkosítási állapotát:</span><span class="sxs-lookup"><span data-stu-id="5feb2-591">tooget encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. <span data-ttu-id="5feb2-592">egy új virtuális Gépet, a titkosított virtuális merevlemezről, a következő paramétereket hello használata hello tooenable titkosítás `azure vm create` parancs:</span><span class="sxs-lookup"><span data-stu-id="5feb2-592">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a><span data-ttu-id="5feb2-593">Engedélyezheti a titkosítást egy már létező vagy nem futnak IaaS Linux virtuális Gépre az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="5feb2-593">Enable encryption on an existing or running IaaS Linux VM in Azure</span></span>
<span data-ttu-id="5feb2-594">Egy már létező vagy nem futnak IaaS Linux virtuális gépre az Azure-ban lemeztitkosítás hello használatával engedélyezheti [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="5feb2-594">You can enable disk encryption on an existing or running IaaS Linux VM in Azure by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span></span>

1. <span data-ttu-id="5feb2-595">Kattintson a **tooAzure telepítése** hello Azure gyors üzembe helyezési sablon, adja meg a hello hello titkosítási konfigurációs **paraméterek** panelt, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-595">Click **Deploy tooAzure** on hello Azure quick-start template, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5feb2-596">Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** hello meglévő vagy az infrastruktúra-szolgáltatási virtuális gép fut tooenable titkosítás.</span><span class="sxs-lookup"><span data-stu-id="5feb2-596">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello existing or running IaaS VM.</span></span>

<span data-ttu-id="5feb2-597">hello a következő táblázat felsorolja a Resource Manager sablon paramétereinek meglévő vagy az Azure AD ügyfél-Azonosítót használó virtuális gépeken futó:</span><span class="sxs-lookup"><span data-stu-id="5feb2-597">hello following table lists Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="5feb2-598">Paraméter</span><span class="sxs-lookup"><span data-stu-id="5feb2-598">Parameter</span></span> | <span data-ttu-id="5feb2-599">Leírás</span><span class="sxs-lookup"><span data-stu-id="5feb2-599">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5feb2-600">AADClientID</span><span class="sxs-lookup"><span data-stu-id="5feb2-600">AADClientID</span></span> | <span data-ttu-id="5feb2-601">Engedélyek toowrite titkok toohello kulcstároló hello Azure AD alkalmazás ügyfél-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5feb2-601">Client ID of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="5feb2-602">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="5feb2-602">AADClientSecret</span></span> | <span data-ttu-id="5feb2-603">Hello Azure AD-alkalmazás, amely rendelkezik engedélyekkel toowrite titkok tooyour kulcstároló titkos ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="5feb2-603">Client secret of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="5feb2-604">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="5feb2-604">keyVaultName</span></span> | <span data-ttu-id="5feb2-605">Hello kulcs neve tároló adott hello BitLocker kulcs fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="5feb2-605">Name of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="5feb2-606">Hello parancsmaggal kaphat `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-606">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="5feb2-607">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="5feb2-607">keyEncryptionKeyURL</span></span> | <span data-ttu-id="5feb2-608">Hello fő titkosítási kulcsát használt tooencrypt hello URL-CÍMÉT a BitLocker kulcs jön létre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-608">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key.</span></span> <span data-ttu-id="5feb2-609">Ez a paraméter nem kötelező, ha **nokek** hello UseExistingKek legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="5feb2-609">This parameter is optional if you select **nokek** in hello UseExistingKek drop-down list.</span></span> <span data-ttu-id="5feb2-610">Ha **kek** hello UseExistingKek legördülő listában, meg kell adnia hello _keyEncryptionKeyURL_ érték.</span><span class="sxs-lookup"><span data-stu-id="5feb2-610">If you select **kek** in hello UseExistingKek drop-down list, you must enter hello _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="5feb2-611">volumeType</span><span class="sxs-lookup"><span data-stu-id="5feb2-611">volumeType</span></span> | <span data-ttu-id="5feb2-612">Hello titkosítási műveletet végzi el a kötet típusa.</span><span class="sxs-lookup"><span data-stu-id="5feb2-612">Type of volume that hello encryption operation is performed on.</span></span> <span data-ttu-id="5feb2-613">Érvényes támogatott értékek a következők _OS_ vagy _összes_ (az RHEL 7.2, CentOS 7.2 és Ubuntu 16.04), és _adatok_ (az összes többi terjesztéseket).</span><span class="sxs-lookup"><span data-stu-id="5feb2-613">Valid supported values are _OS_ or _All_ (for RHEL 7.2, CentOS 7.2, and Ubuntu 16.04), and _Data_ (for all other distributions).</span></span> |
| <span data-ttu-id="5feb2-614">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="5feb2-614">sequenceVersion</span></span> | <span data-ttu-id="5feb2-615">A BitLocker művelet hello feladatütemezési verzióját.</span><span class="sxs-lookup"><span data-stu-id="5feb2-615">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="5feb2-616">Ez a verziószám növelése minden alkalommal, amikor egy lemezt-titkosítási művelet történik hello azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5feb2-616">Increment this version number every time a disk-encryption operation is performed on hello same VM.</span></span> |
| <span data-ttu-id="5feb2-617">vmName</span><span class="sxs-lookup"><span data-stu-id="5feb2-617">vmName</span></span> | <span data-ttu-id="5feb2-618">Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-618">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |
| <span data-ttu-id="5feb2-619">hozzáférési kód</span><span class="sxs-lookup"><span data-stu-id="5feb2-619">passPhrase</span></span> | <span data-ttu-id="5feb2-620">Írjon be egy erős jelszót hello adattitkosítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="5feb2-620">Type a strong passphrase as hello data encryption key.</span></span> |

> [!NOTE]
> <span data-ttu-id="5feb2-621">_KeyEncryptionKeyURL_ egy nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="5feb2-621">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="5feb2-622">A key vaultban helyezheti a saját KEK toofurther védelmi hello adattitkosítási kulcsot (jelszó titkos).</span><span class="sxs-lookup"><span data-stu-id="5feb2-622">You can bring your own KEK toofurther safeguard hello data encryption key (passphrase secret) in your key vault.</span></span>

#### <a name="cli-commands"></a><span data-ttu-id="5feb2-623">Parancssori felület parancsai</span><span class="sxs-lookup"><span data-stu-id="5feb2-623">CLI commands</span></span>
<span data-ttu-id="5feb2-624">Engedélyezheti lemeztitkosítás a titkosított virtuális merevlemezen történő telepítéssel és használattal hello [CLI parancs](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5feb2-624">You can enable disk encryption on your encrypted VHD by installing and using hello [CLI command](../cli-install-nodejs.md).</span></span> <span data-ttu-id="5feb2-625">meglévő vagy az Azure-beli infrastruktúra-szolgáltatási Linux virtuális gépek CLI-parancsok segítségével tooenable titkosítás hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-625">tooenable encryption on existing or running IaaS Linux VMs in Azure by using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="5feb2-626">Állítsa be a hozzáférési házirendek hello a key vaultban:</span><span class="sxs-lookup"><span data-stu-id="5feb2-626">Set access policies in hello key vault:</span></span>

 * <span data-ttu-id="5feb2-627">Set hello **EnabledForDiskEncryption** jelző:</span><span class="sxs-lookup"><span data-stu-id="5feb2-627">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * <span data-ttu-id="5feb2-628">Állítsa be az engedélyeket tooAzure AD alkalmazás toowrite titkok tooyour kulcstároló:</span><span class="sxs-lookup"><span data-stu-id="5feb2-628">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="5feb2-629">egy meglévő vagy a futó virtuális gép tooenable titkosítás:</span><span class="sxs-lookup"><span data-stu-id="5feb2-629">tooenable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="5feb2-630">Titkosítási állapot beolvasása:</span><span class="sxs-lookup"><span data-stu-id="5feb2-630">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="5feb2-631">egy új virtuális Gépet, a titkosított virtuális merevlemezről, a következő paramétereket hello használata hello tooenable titkosítás `azure vm create` parancs:</span><span class="sxs-lookup"><span data-stu-id="5feb2-631">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-hello-encryption-status-of-an-encrypted-iaas-vm"></a><span data-ttu-id="5feb2-632">Hello titkosítási állapotát egy titkosított infrastruktúra-szolgáltatási virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="5feb2-632">Get hello encryption status of an encrypted IaaS VM</span></span>
<span data-ttu-id="5feb2-633">Azure Resource Manager használatával kaphat hello titkosítási állapot [PowerShell-parancsmagok](/powershell/azure/overview), vagy a parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="5feb2-633">You can get hello encryption status by using Azure Resource Manager, [PowerShell cmdlets](/powershell/azure/overview), or CLI commands.</span></span> <span data-ttu-id="5feb2-634">hello a következő szakaszok azt ismertetik, hogyan toouse hello a klasszikus Azure portálon és parancssori felület parancsai tooget hello titkosítási állapottal.</span><span class="sxs-lookup"><span data-stu-id="5feb2-634">hello following sections explain how toouse hello Azure classic portal and CLI commands tooget hello encryption status.</span></span>

#### <a name="get-hello-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a><span data-ttu-id="5feb2-635">Azure Resource Manager segítségével könnyebben nyerhet hello titkosítási állapotát egy titkosított Windows virtuális Gépre</span><span class="sxs-lookup"><span data-stu-id="5feb2-635">Get hello encryption status of an encrypted Windows VM by using Azure Resource Manager</span></span>
<span data-ttu-id="5feb2-636">Kaphat hello titkosítási hello infrastruktúra-szolgáltatási virtuális gép állapotát az Azure Resource Manager hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="5feb2-636">You can get hello encryption status of hello IaaS VM from Azure Resource Manager by doing hello following:</span></span>

1. <span data-ttu-id="5feb2-637">Jelentkezzen be toohello [a klasszikus Azure portálon](https://portal.azure.com/), és kattintson a **virtuális gépek** a hello bal oldali ablaktáblán toosee hello virtuális gépek az előfizetéshez összefoglaló áttekintést.</span><span class="sxs-lookup"><span data-stu-id="5feb2-637">Sign in toohello [Azure classic portal](https://portal.azure.com/), and then click **Virtual machines** in hello left pane toosee a summary view of hello virtual machines in your subscription.</span></span> <span data-ttu-id="5feb2-638">Hello virtuális gépek nézet szűrheti hello hello előfizetés neve kiválasztásával **előfizetés** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="5feb2-638">You can filter hello virtual machines view by selecting hello subscription name in hello **Subscription** drop-down list.</span></span>

2. <span data-ttu-id="5feb2-639">Hello hello tetején **virtuális gépek** kattintson **oszlopok**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-639">At hello top of hello **Virtual machines** page, click **Columns**.</span></span>

3. <span data-ttu-id="5feb2-640">A hello **válasszon oszlop** panelen válassza **lemeztitkosítás**, és kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-640">On hello **Choose column** blade, select **Disk Encryption**, and then click **Update**.</span></span> <span data-ttu-id="5feb2-641">Megtekintheti az hello lemeztitkosítás oszlop ábrázoló hello titkosítási állapot _engedélyezve_ vagy _nincs engedélyezve a_ az egyes virtuális gépek, ahogy az ábra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-641">You should see hello disk-encryption column showing hello encryption state _Enabled_ or _Not Enabled_ for each VM, as shown in hello following figure:</span></span>

 ![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-hello-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-hello-disk-encryption-powershell-cmdlet"></a><span data-ttu-id="5feb2-643">Egy titkosított (Windows/Linux) infrastruktúra-szolgáltatási virtuális gép hello titkosítási állapotát hello lemeztitkosítás PowerShell-parancsmag segítségével könnyebben nyerhet</span><span class="sxs-lookup"><span data-stu-id="5feb2-643">Get hello encryption status of an encrypted (Windows/Linux) IaaS VM by using hello disk-encryption PowerShell cmdlet</span></span>
<span data-ttu-id="5feb2-644">Infrastruktúra-szolgáltatási virtuális gép hello hello titkosítási állapotát letölthető hello lemeztitkosítás PowerShell-parancsmag `Get-AzureRmVMDiskEncryptionStatus`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-644">You can get hello encryption status of hello IaaS VM from hello disk-encryption PowerShell cmdlet `Get-AzureRmVMDiskEncryptionStatus`.</span></span> <span data-ttu-id="5feb2-645">tooget hello fájltitkosítási beállításainak megadása a virtuális Gépet, írja be a hello következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-645">tooget hello encryption settings for your VM, enter hello following:</span></span>

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

<span data-ttu-id="5feb2-646">Hello kimenete vizsgálhatja _Get-AzureRmVMDiskEncryptionStatus_ titkosítási kulcs az URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="5feb2-646">You can inspect hello output of _Get-AzureRmVMDiskEncryptionStatus_ for encryption key URLs.</span></span>

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

<span data-ttu-id="5feb2-647">hello OSVolumeEncrypted és a DataVolumesEncrypted beállítási értékei too_Encrypted_, amely mutatja, hogy mindkét kötet titkosítása Azure Disk Encryption használatával van beállítva.</span><span class="sxs-lookup"><span data-stu-id="5feb2-647">hello OSVolumeEncrypted and DataVolumesEncrypted settings values are set too_Encrypted_, which shows that both volumes are encrypted using Azure Disk Encryption.</span></span> <span data-ttu-id="5feb2-648">Az Azure Disk Encryption titkosítás engedélyezése a PowerShell-parancsmagok használatával kapcsolatos információkért lásd: hello blogbejegyzések [Azure Disk Encryption megismerkedhet az Azure PowerShell - 1. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) és [Azure Disk Encryption felfedezés az Azure PowerShell - 2. rész](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="5feb2-648">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see hello blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-649">A Linux virtuális gépeken, hello három toofour percet vesz igénybe `Get-AzureRmVMDiskEncryptionStatus` parancsmag tooreport hello titkosítási állapottal.</span><span class="sxs-lookup"><span data-stu-id="5feb2-649">On Linux VMs, it takes three toofour minutes for hello `Get-AzureRmVMDiskEncryptionStatus` cmdlet tooreport hello encryption status.</span></span>

#### <a name="get-hello-encryption-status-of-hello-iaas-vm-from-hello-disk-encryption-cli-command"></a><span data-ttu-id="5feb2-650">Hello lemeztitkosítás CLI parancs hello titkosítási állapotát hello infrastruktúra-szolgáltatási virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="5feb2-650">Get hello encryption status of hello IaaS VM from hello disk-encryption CLI command</span></span>
<span data-ttu-id="5feb2-651">Hello titkosítási állapotát hello infrastruktúra-szolgáltatási virtuális gép hello lemeztitkosítás CLI parancs használatával beszerezheti `azure vm show-disk-encryption-status`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-651">You can get hello encryption status of hello IaaS VM by using hello disk-encryption CLI command `azure vm show-disk-encryption-status`.</span></span> <span data-ttu-id="5feb2-652">tooget hello fájltitkosítási beállításainak megadása a virtuális Gépet, adja meg a Azure CLI-munkamenethez:</span><span class="sxs-lookup"><span data-stu-id="5feb2-652">tooget hello encryption settings for your VM, enter your Azure CLI session:</span></span>

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a><span data-ttu-id="5feb2-653">Tiltsa le a titkosítást a windowsos infrastruktúra-szolgáltatási virtuális gép futó</span><span class="sxs-lookup"><span data-stu-id="5feb2-653">Disable encryption on running Windows IaaS VM</span></span>
<span data-ttu-id="5feb2-654">Tiltsa le a titkosítást egy futó Windows vagy Linux rendszerű infrastruktúra-szolgáltatási virtuális hello Azure lemez titkosítási Resource Manager-sablon vagy a PowerShell-parancsmagok használatával, és hello visszafejtési konfiguráció megadásához.</span><span class="sxs-lookup"><span data-stu-id="5feb2-654">You can disable encryption on a running Windows or Linux IaaS VM via hello Azure Disk Encryption Resource Manager template or PowerShell cmdlets and specify hello decryption configuration.</span></span>

##### <a name="windows-vm"></a><span data-ttu-id="5feb2-655">Windowsos VM</span><span class="sxs-lookup"><span data-stu-id="5feb2-655">Windows VM</span></span>
<span data-ttu-id="5feb2-656">hello disable-titkosítás lépés letiltja a titkosítást hello az operációs rendszer, hello adatmennyiség vagy mindkét szolgáltatás fut a Windows alapú infrastruktúra-szolgáltatási virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-656">hello disable-encryption step disables encryption of hello OS, hello data volume, or both on hello running Windows IaaS VM.</span></span> <span data-ttu-id="5feb2-657">Nem hello operációs rendszer kötetén tiltása, és nem titkosított hello adatmennyiség hagyja.</span><span class="sxs-lookup"><span data-stu-id="5feb2-657">You cannot disable hello OS volume and leave hello data volume encrypted.</span></span> <span data-ttu-id="5feb2-658">Hello disable-titkosítás a lépést, ha hello Azure klasszikus üzembe helyezési modellel frissíti hello VM modell, és IaaS-VM Windows hello visszafejtett van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="5feb2-658">When hello disable-encryption step is performed, hello Azure classic deployment model updates hello VM service model, and hello Windows IaaS VM is marked decrypted.</span></span> <span data-ttu-id="5feb2-659">hello tartalmát hello virtuális gép már nem titkosított aktívan.</span><span class="sxs-lookup"><span data-stu-id="5feb2-659">hello contents of hello VM are no longer encrypted at rest.</span></span> <span data-ttu-id="5feb2-660">hello visszafejtési nem törli a kulcs tárolóban és hello titkosítási kulcsokat tárol (a BitLocker titkosítási kulcsokat a Windows és Linux jelszava).</span><span class="sxs-lookup"><span data-stu-id="5feb2-660">hello decryption does not delete your key vault and hello encryption key material (BitLocker encryption keys for Windows and Passphrase for Linux).</span></span>

##### <a name="linux-vm"></a><span data-ttu-id="5feb2-661">Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="5feb2-661">Linux VM</span></span>
<span data-ttu-id="5feb2-662">hello disable-titkosítás lépés letiltja a titkosítást hello adatok kötetének hello futó Linux infrastruktúra-szolgáltatási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5feb2-662">hello disable-encryption step disables encryption of hello data volume on hello running Linux IaaS VM.</span></span> <span data-ttu-id="5feb2-663">Ez a lépés csak akkor működik, ha hello operációsrendszer-lemez nem titkosított.</span><span class="sxs-lookup"><span data-stu-id="5feb2-663">This step only works if hello OS disk is not encrypted.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-664">Hello operációsrendszer-lemez titkosításának letiltása a Linux virtuális gépeken nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5feb2-664">Disabling encryption on hello OS disk is not allowed on Linux VMs.</span></span>

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="5feb2-665">Tiltsa le a titkosítást egy már létező vagy nem futnak IaaS virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="5feb2-665">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="5feb2-666">Letilthatja a Windows IaaS virtuális gépeken futó hello segítségével lemeztitkosítás [Resource Manager-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="5feb2-666">You can disable disk encryption on running Windows IaaS VMs by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="5feb2-667">Hello Azure gyors üzembe helyezési-sablont, kattintson a **tooAzure telepítése**, írjon be hello visszafejtési konfigurációs hello **paraméterek** panelt, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-667">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello decryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5feb2-668">Válassza ki a hello előfizetés, a erőforráscsoport, a erőforráscsoport helye, a jogi feltételeket és a szerződést, és kattintson a **létrehozása** tooenable titkosítási egy új IaaS virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5feb2-668">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on a new IaaS VM.</span></span>

<span data-ttu-id="5feb2-669">Linux virtuális gépekhez, letilthatja titkosítási hello segítségével [tiltsa le a titkosítást a futó Linux virtuális gép](https://aka.ms/decrypt-linuxvm) sablont.</span><span class="sxs-lookup"><span data-stu-id="5feb2-669">For Linux VMs, you can disable encryption by using hello [Disable encryption on a running Linux VM](https://aka.ms/decrypt-linuxvm) template.</span></span>

<span data-ttu-id="5feb2-670">hello alábbi táblázat Resource Manager sablon paramétereinek egy infrastruktúra-szolgáltatási virtuális gép titkosításának letiltása:</span><span class="sxs-lookup"><span data-stu-id="5feb2-670">hello following table lists Resource Manager template parameters for disabling encryption on a running IaaS VM:</span></span>

| <span data-ttu-id="5feb2-671">Paraméter</span><span class="sxs-lookup"><span data-stu-id="5feb2-671">Parameter</span></span> | <span data-ttu-id="5feb2-672">Leírás</span><span class="sxs-lookup"><span data-stu-id="5feb2-672">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5feb2-673">vmName</span><span class="sxs-lookup"><span data-stu-id="5feb2-673">vmName</span></span> | <span data-ttu-id="5feb2-674">Hello virtuális Gépet, amely a titkosítási művelet hello értéke toobe végre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-674">Name of hello VM that hello encryption operation is toobe performed on.</span></span>
| <span data-ttu-id="5feb2-675">volumeType</span><span class="sxs-lookup"><span data-stu-id="5feb2-675">volumeType</span></span> | <span data-ttu-id="5feb2-676">A visszafejtési művelet végrehajtott kötet típusa.</span><span class="sxs-lookup"><span data-stu-id="5feb2-676">Type of volume that a decryption operation is performed on.</span></span> <span data-ttu-id="5feb2-677">Érvényes értékek a következők _OS_, _adatok_, és _összes_.</span><span class="sxs-lookup"><span data-stu-id="5feb2-677">Valid values are _OS_, _Data_, and _All_.</span></span> <span data-ttu-id="5feb2-678">A windowsos infrastruktúra-szolgáltatási virtuális gép operációs rendszer vagy rendszerindító kötet futó letiltása hello titkosítás nélküli titkosítás nem tiltható le _adatok_ kötet.</span><span class="sxs-lookup"><span data-stu-id="5feb2-678">You cannot disable encryption on running Windows IaaS VM OS/boot volume without disabling encryption on hello _Data_ volume.</span></span> <span data-ttu-id="5feb2-679">Ne feledje, hogy az operációsrendszer-lemez hello titkosításának letiltása nem engedélyezett a Linux virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="5feb2-679">Also note that disabling encryption on hello OS disk is not allowed on Linux VMs.</span></span> |
| <span data-ttu-id="5feb2-680">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="5feb2-680">sequenceVersion</span></span> | <span data-ttu-id="5feb2-681">A BitLocker művelet hello feladatütemezési verzióját.</span><span class="sxs-lookup"><span data-stu-id="5feb2-681">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="5feb2-682">Ez a verziószám növelése minden alkalommal, amikor a lemez visszafejtési művelet történik hello azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5feb2-682">Increment this version number every time a disk decryption operation is performed on hello same VM.</span></span> |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="5feb2-683">Tiltsa le a titkosítást egy már létező vagy nem futnak IaaS virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="5feb2-683">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="5feb2-684">egy már létező vagy nem futnak infrastruktúra-szolgáltatási virtuális gép hello PowerShell-parancsmag segítségével toodisable titkosítás lásd [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span><span class="sxs-lookup"><span data-stu-id="5feb2-684">toodisable encryption on an existing or running IaaS VM by using hello PowerShell cmdlet, see [`Disable-AzureRmVMDiskEncryption`](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span></span> <span data-ttu-id="5feb2-685">Ez a parancsmag a Windows és Linux virtuális gépeket is támogatja.</span><span class="sxs-lookup"><span data-stu-id="5feb2-685">This cmdlet supports both Windows and Linux VMs.</span></span> <span data-ttu-id="5feb2-686">toodisable titkosítási telepíti egy bővítmény hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="5feb2-686">toodisable encryption, it installs an extension on hello virtual machine.</span></span> <span data-ttu-id="5feb2-687">Ha hello _neve_ paraméter nincs megadva, hello alapértelmezett nevű bővítménye _AzureDiskEncryption a Windows virtuális gépek_ jön létre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-687">If hello _Name_ parameter is not specified, an extension with hello default name _AzureDiskEncryption for Windows VMs_ is created.</span></span>

<span data-ttu-id="5feb2-688">A Linux virtuális gépeken hello AzureDiskEncryptionForLinux bővítmény szolgál.</span><span class="sxs-lookup"><span data-stu-id="5feb2-688">On Linux VMs, hello AzureDiskEncryptionForLinux extension is used.</span></span>

> [!NOTE]
> <span data-ttu-id="5feb2-689">Ez a parancsmag hello virtuális gép újraindul.</span><span class="sxs-lookup"><span data-stu-id="5feb2-689">This cmdlet reboots hello virtual machine.</span></span>

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="5feb2-690">Engedélyezze a titkosítást a Azure felügyelt lemezes előzetes titkosítással IaaS virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="5feb2-690">Enable encryption on pre-encrypted IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="5feb2-691">Használja a hello Azure kezelt lemez ARM sablon toocreate hello ARM-sablon használatával előre titkosított virtuális Merevlemezt egy titkosított virtuális gép helye:</span><span class="sxs-lookup"><span data-stu-id="5feb2-691">Use hello Azure Managed Disk ARM template toocreate a encrypted VM from a pre-encrypted VHD using hello ARM template located at</span></span>   
<span data-ttu-id="5feb2-692">[Előzetes titkosítással virtuális merevlemez tárolási blob hozhat létre új titkosított felügyelt lemezt] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span><span class="sxs-lookup"><span data-stu-id="5feb2-692">[Create a new encrypted managed disk from a pre-encrypted VHD/storage blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span></span>

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="5feb2-693">Engedélyezheti a titkosítást egy új Linux IaaS virtuális Gépet az Azure Managed lemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-693">Enable encryption on a new Linux IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="5feb2-694">Hello Azure kezelt lemez ARM sablon toocreate új Linux infrastruktúra-szolgáltatási virtuális gép helye: hello ARM-sablon használatával titkosítva használja</span><span class="sxs-lookup"><span data-stu-id="5feb2-694">Use hello Azure Managed Disk ARM template toocreate a new encrypted Linux IaaS VM using hello ARM template located at</span></span>   
<span data-ttu-id="5feb2-695">[A teljes lemez titkosítása RHEL 7.2 telepítését] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span><span class="sxs-lookup"><span data-stu-id="5feb2-695">[Deployment of RHEL 7.2 with full disk encryption] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span></span>

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="5feb2-696">Engedélyezheti a titkosítást egy új Windows IaaS virtuális Gépet az Azure Managed lemez</span><span class="sxs-lookup"><span data-stu-id="5feb2-696">Enable encryption on a new Windows IaaS VM with Azure Managed Disk</span></span>
 <span data-ttu-id="5feb2-697">Hello Azure kezelt lemez ARM sablon toocreate új Linux infrastruktúra-szolgáltatási virtuális gép helye: hello ARM-sablon használatával titkosítva használja</span><span class="sxs-lookup"><span data-stu-id="5feb2-697">Use hello Azure Managed Disk ARM template toocreate a new encrypted Linux IaaS VM using hello ARM template located at</span></span>   
 <span data-ttu-id="5feb2-698">[Új titkosított Windows IaaS kezelt lemez virtuális gép létrehozása gyűjtemény lemezképből] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span><span class="sxs-lookup"><span data-stu-id="5feb2-698">[Create a new encrypted Windows IaaS Managed Disk VM from gallery image] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span></span>

  > [!NOTE]
  ><span data-ttu-id="5feb2-699">Kötelező toosnapshot, és/vagy biztonsági mentési felügyelt lemezes alapú kívül a Virtuálisgép-példány és a korábbi tooenabling Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5feb2-699">It is mandatory toosnapshot and/or backup a managed disk based VM instance outside of and prior tooenabling Azure Disk Encryption.</span></span>  <span data-ttu-id="5feb2-700">Hello felügyelt lemezes pillanatképet átvihető hello portálról, vagy az Azure Backup használható.</span><span class="sxs-lookup"><span data-stu-id="5feb2-700">A snapshot of hello managed disk can be taken from hello portal, or Azure Backup can be used.</span></span>  <span data-ttu-id="5feb2-701">Biztonsági mentések győződjön meg arról, hogy egy helyreállítási lehetőséget a hello esetben bármely váratlan hiba lehetséges titkosítás során.</span><span class="sxs-lookup"><span data-stu-id="5feb2-701">Backups ensure that a recovery option is possible in hello case of any unexpected failure during encryption.</span></span>  <span data-ttu-id="5feb2-702">Biztonsági másolat elkészítése után a Set-AzureRmVMDiskEncryptionExtension hello parancsmag lehet használt tooencrypt kezelt lemezek hello - skipVmBackup paraméter megadásával.</span><span class="sxs-lookup"><span data-stu-id="5feb2-702">Once a backup is made, hello Set-AzureRmVMDiskEncryptionExtension cmdlet can be used tooencrypt managed disks by specifying hello -skipVmBackup parameter.</span></span>  <span data-ttu-id="5feb2-703">Ez a parancs elleni felügyelt lemezes virtuális gép sikertelen lesz, amíg a biztonsági mentést készített, és ez a paraméter van megadva.</span><span class="sxs-lookup"><span data-stu-id="5feb2-703">This command will fail against managed disk based VM's until a backup has been made and this parameter has been specified.</span></span>    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a><span data-ttu-id="5feb2-704">Meglévő titkosított nem prémium szintű virtuális titkosítási beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-704">Update encryption settings of an existing encrypted non-premium VM</span></span>
  <span data-ttu-id="5feb2-705">Használjon hello meglévő Azure lemez támogatott titkosítási felületek a futó virtuális gép [PS parancsmagok, CLI és ARM-sablonok] tooupdate hello titkosítási beállításait, például AAD ügyfél azonosítója/titkos, fő titkosítási kulcs [KEK], a BitLocker titkosítási kulcs Windows virtuális gép vagy a jelszót Linux virtuális gép stb hello frissítés titkosítási beállítás csak a nem prémium szintű storage által támogatott virtuális gépek esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="5feb2-705">Use hello existing Azure disk encryption supported interfaces for running VM [PS cmdlets, CLI or ARM templates] tooupdate hello encryption settings like AAD client ID/secret, Key encryption key [KEK], BitLocker encryption key for Windows VM or Passphrase for Linux VM etc. hello update encryption setting is supported only for VMs backed by non-premium storage.</span></span> <span data-ttu-id="5feb2-706">Prémium szintű storage által támogatott virtuális gépek esetén támogatott NNOT.</span><span class="sxs-lookup"><span data-stu-id="5feb2-706">It is NNOT supported for VMs backed by premium storage.</span></span>

## <a name="appendix"></a><span data-ttu-id="5feb2-707">Függelék:</span><span class="sxs-lookup"><span data-stu-id="5feb2-707">Appendix</span></span>
### <a name="connect-tooyour-subscription"></a><span data-ttu-id="5feb2-708">Csatlakozás tooyour előfizetés</span><span class="sxs-lookup"><span data-stu-id="5feb2-708">Connect tooyour subscription</span></span>
<span data-ttu-id="5feb2-709">Mielőtt folytatná, tekintse át a hello *Előfeltételek* című részben.</span><span class="sxs-lookup"><span data-stu-id="5feb2-709">Before you proceed, review hello *Prerequisites* section in this article.</span></span> <span data-ttu-id="5feb2-710">Miután meggyőződött arról, hogy a szükséges előfeltételek maradéktalanul teljesülnek, csatlakozás tooyour előfizetés hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="5feb2-710">After you ensure that all prerequisites have been met, connect tooyour subscription by doing hello following:</span></span>

1. <span data-ttu-id="5feb2-711">Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be Azure-fiók tooyour hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5feb2-711">Start an Azure PowerShell session, and sign in tooyour Azure account with hello following command:</span></span>

    `Login-AzureRmAccount`

2. <span data-ttu-id="5feb2-712">Ha több előfizetéssel rendelkezik, és szeretné, hogy egy toouse toospecify, írja be a fiókhoz toosee hello előfizetések a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-712">If you have multiple subscriptions and want toospecify one toouse, type hello following toosee hello subscriptions for your account:</span></span>

    `Get-AzureRmSubscription`

3. <span data-ttu-id="5feb2-713">azt szeretné, hogy toouse, toospecify hello előfizetés típusa:</span><span class="sxs-lookup"><span data-stu-id="5feb2-713">toospecify hello subscription you want toouse, type:</span></span>

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. <span data-ttu-id="5feb2-714">hogy helyesek-e a konfigurált hello előfizetés tooverify, írja be:</span><span class="sxs-lookup"><span data-stu-id="5feb2-714">tooverify that hello subscription configured is correct, type:</span></span>

    `Get-AzureRmSubscription`

5. <span data-ttu-id="5feb2-715">tooconfirm hello Azure Disk Encryption parancsmagjai telepítve vannak, típus:</span><span class="sxs-lookup"><span data-stu-id="5feb2-715">tooconfirm hello Azure Disk Encryption cmdlets are installed, type:</span></span>

    `Get-command *diskencryption*`

6. <span data-ttu-id="5feb2-716">a következő kimeneti hello megerősíti, hogy hello Azure lemez titkosítási PowerShell telepítési:</span><span class="sxs-lookup"><span data-stu-id="5feb2-716">hello following output confirms hello Azure Disk Encryption PowerShell installation:</span></span>

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a><span data-ttu-id="5feb2-717">Előzetes titkosítással Windows virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="5feb2-717">Prepare a pre-encrypted Windows VHD</span></span>
<span data-ttu-id="5feb2-718">hello szakaszok szükséges tooprepare előzetes titkosítással Windows virtuális központi telepítésben egy Azure IaaS titkosított VHD-t is.</span><span class="sxs-lookup"><span data-stu-id="5feb2-718">hello sections that follow are necessary tooprepare a pre-encrypted Windows VHD for deployment as an encrypted VHD in Azure IaaS.</span></span> <span data-ttu-id="5feb2-719">Hello információk tooprepare használja és az Azure Site Recovery vagy Azure friss Windows virtuális gép (VHD).</span><span class="sxs-lookup"><span data-stu-id="5feb2-719">Use hello information tooprepare and boot a fresh Windows VM (VHD) on Azure Site Recovery or Azure.</span></span>

#### <a name="update-group-policy-tooallow-non-tpm-for-os-protection"></a><span data-ttu-id="5feb2-720">Csoport házirend tooallow nem TPM-OS Protection frissítése</span><span class="sxs-lookup"><span data-stu-id="5feb2-720">Update group policy tooallow non-TPM for OS protection</span></span>
<span data-ttu-id="5feb2-721">Hello a BitLocker csoportházirend-beállítás konfigurálása **a BitLocker meghajtótitkosítás**, amely alatt található **helyi számítógép-házirend** > **számítógép konfigurációja**  >  **Felügyeleti sablonok** > **Windows-összetevők**.</span><span class="sxs-lookup"><span data-stu-id="5feb2-721">Configure hello BitLocker Group Policy setting **BitLocker Drive Encryption**, which you'll find under **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components**.</span></span> <span data-ttu-id="5feb2-722">Ez a beállítás túl**operációsrendszer-meghajtók** > **indításkor további hitelesítést** > **kompatibilisTPMnélküliBitLockerengedélyezése**, ahogy az ábra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-722">Change this setting too**Operating System Drives** > **Require additional authentication at startup** > **Allow BitLocker without a compatible TPM**, as shown in hello following figure:</span></span>

![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a><span data-ttu-id="5feb2-724">A BitLocker-szolgáltatás összetevőinek telepítése</span><span class="sxs-lookup"><span data-stu-id="5feb2-724">Install BitLocker feature components</span></span>
<span data-ttu-id="5feb2-725">A Windows Server 2012 és újabb verziók használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-725">For Windows Server 2012 and later, use hello following command:</span></span>

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

<span data-ttu-id="5feb2-726">A Windows Server 2008 R2 a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="5feb2-726">For Windows Server 2008 R2, use hello following command:</span></span>

    ServerManagerCmd -install BitLockers

#### <a name="prepare-hello-os-volume-for-bitlocker-by-using-bdehdcfg"></a><span data-ttu-id="5feb2-727">A BitLocker használatával hello rendszerkötet előkészítése`bdehdcfg`</span><span class="sxs-lookup"><span data-stu-id="5feb2-727">Prepare hello OS volume for BitLocker by using `bdehdcfg`</span></span>
<span data-ttu-id="5feb2-728">toocompress hello OS partíció, és hello gép előkészítése a BitLocker, hajtsa végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-728">toocompress hello OS partition and prepare hello machine for BitLocker, execute hello following command:</span></span>

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-hello-os-volume-by-using-bitlocker"></a><span data-ttu-id="5feb2-729">A BitLocker használatával hello operációsrendszer-kötet védelme</span><span class="sxs-lookup"><span data-stu-id="5feb2-729">Protect hello OS volume by using BitLocker</span></span>
<span data-ttu-id="5feb2-730">Használjon hello [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) parancs tooenable titkosítást használ egy külső kulcsvédő hello rendszerindító köteten.</span><span class="sxs-lookup"><span data-stu-id="5feb2-730">Use hello [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) command tooenable encryption on hello boot volume using an external key protector.</span></span> <span data-ttu-id="5feb2-731">Hello külső meghajtók vagy kötetek is elhelyezhető hello külső kulcs (.bek fájl).</span><span class="sxs-lookup"><span data-stu-id="5feb2-731">Also place hello external key (.bek file) on hello external drive or volume.</span></span> <span data-ttu-id="5feb2-732">Engedélyezve van hello rendszerlemez vagy rendszerindító köteten hello tovább a rendszer újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="5feb2-732">Encryption is enabled on hello system/boot volume after hello next reboot.</span></span>

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> <span data-ttu-id="5feb2-733">Készítsen elő egy külön adatok és az erőforrások virtuális Merevlemezt a virtuális gép hello hello külső kulcs kapcsolódnak a BitLocker használatával.</span><span class="sxs-lookup"><span data-stu-id="5feb2-733">Prepare hello VM with a separate data/resource VHD for getting hello external key by using BitLocker.</span></span>

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a><span data-ttu-id="5feb2-734">Az operációs rendszer meghajtóján futó Linux virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="5feb2-734">Encrypting an OS drive on a running Linux VM</span></span>
<span data-ttu-id="5feb2-735">A Linux virtuális gép egy operációs rendszer meghajtójának titkosítás funkciót támogatja, a következő terjesztéseket hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-735">Encryption of an OS drive on a running Linux VM is supported on hello following distributions:</span></span>

* <span data-ttu-id="5feb2-736">7.2 RHEL</span><span class="sxs-lookup"><span data-stu-id="5feb2-736">RHEL 7.2</span></span>
* <span data-ttu-id="5feb2-737">7.2 centOS</span><span class="sxs-lookup"><span data-stu-id="5feb2-737">CentOS 7.2</span></span>
* <span data-ttu-id="5feb2-738">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="5feb2-738">Ubuntu 16.04</span></span>

##### <a name="prerequisites-for-os-disk-encryption"></a><span data-ttu-id="5feb2-739">Az operációs rendszer lemeztitkosítás előfeltételei</span><span class="sxs-lookup"><span data-stu-id="5feb2-739">Prerequisites for OS disk encryption</span></span>

* <span data-ttu-id="5feb2-740">virtuális gép hello hello Piactéri lemezkép az Azure Resource Manager kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5feb2-740">hello VM must be created from hello Marketplace image in Azure Resource Manager.</span></span>
* <span data-ttu-id="5feb2-741">Az Azure virtuális gép, és legalább 4 GB RAM (mérete 7 GB ajánlott).</span><span class="sxs-lookup"><span data-stu-id="5feb2-741">Azure VM with at least 4 GB of RAM (recommended size is 7 GB).</span></span>
* <span data-ttu-id="5feb2-742">(Az RHEL és CentOS) Tiltsa le a SELinux.</span><span class="sxs-lookup"><span data-stu-id="5feb2-742">(For RHEL and CentOS) Disable SELinux.</span></span> <span data-ttu-id="5feb2-743">toodisable SELinux, tekintse meg a "4.4.2.</span><span class="sxs-lookup"><span data-stu-id="5feb2-743">toodisable SELinux, see "4.4.2.</span></span> <span data-ttu-id="5feb2-744">Letiltása SELinux"hello a [SELinux felhasználói és rendszergazdai útmutató](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-744">Disabling SELinux" in hello [SELinux User's and Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) on hello VM.</span></span>
* <span data-ttu-id="5feb2-745">Letiltja a SELinux, újraindítása után hello virtuális gép legalább egy alkalommal.</span><span class="sxs-lookup"><span data-stu-id="5feb2-745">After you disable SELinux, reboot hello VM at least once.</span></span>

##### <a name="steps"></a><span data-ttu-id="5feb2-746">Lépések</span><span class="sxs-lookup"><span data-stu-id="5feb2-746">Steps</span></span>
1. <span data-ttu-id="5feb2-747">Hozzon létre egy virtuális Gépet egy korábban megadott hello terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="5feb2-747">Create a VM by using one of hello distributions specified previously.</span></span>

 <span data-ttu-id="5feb2-748">7.2 – CentOS operációsrendszer-lemez titkosítása támogatott keresztül egy különös lemezképet.</span><span class="sxs-lookup"><span data-stu-id="5feb2-748">For CentOS 7.2, OS disk encryption is supported via a special image.</span></span> <span data-ttu-id="5feb2-749">toouse a lemezképet, adja meg a "7.2n", SKU hello hello virtuális gép létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="5feb2-749">toouse this image, specify "7.2n" as hello SKU when you create hello VM:</span></span>
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. <span data-ttu-id="5feb2-750">Hello VM függően tooyour kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="5feb2-750">Configure hello VM according tooyour needs.</span></span> <span data-ttu-id="5feb2-751">Tooencrypt összes hello (az operációs rendszer + adatok) meghajtókat is, ha hello adatmeghajtókon a megadott és a /etc/fstab csatlakoztatható toobe van szükség.</span><span class="sxs-lookup"><span data-stu-id="5feb2-751">If you are going tooencrypt all hello (OS + data) drives, hello data drives need toobe specified and mountable from /etc/fstab.</span></span>

 > [!NOTE]
 > <span data-ttu-id="5feb2-752">Használja az UUID =... toospecify adatmeghajtókon a/etc/fstab hello blokk eszköz neve (például /dev/sdb1) megadása helyett.</span><span class="sxs-lookup"><span data-stu-id="5feb2-752">Use UUID=... toospecify data drives in /etc/fstab instead of specifying hello block device name (for example, /dev/sdb1).</span></span> <span data-ttu-id="5feb2-753">Titkosítás során meghajtók hello sorrendjének módosítja, a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-753">During encryption, hello order of drives changes on hello VM.</span></span> <span data-ttu-id="5feb2-754">Ha a virtuális Gépet a megadott sorrendben blokk eszközök támaszkodik, meghiúsul toomount titkosítás után őket.</span><span class="sxs-lookup"><span data-stu-id="5feb2-754">If your VM relies on a specific order of block devices, it will fail toomount them after encryption.</span></span>

3. <span data-ttu-id="5feb2-755">Jelentkezzen ki hello SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="5feb2-755">Sign out of hello SSH sessions.</span></span>

4. <span data-ttu-id="5feb2-756">tooencrypt hello az operációs rendszer, adja meg, mint volumeType **összes** vagy **az operációs rendszer** amikor Ön [engedélyezheti a titkosítást](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span><span class="sxs-lookup"><span data-stu-id="5feb2-756">tooencrypt hello OS, specify volumeType as **All** or **OS** when you [enable encryption](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span></span>

 > [!NOTE]
 > <span data-ttu-id="5feb2-757">Minden felhasználói térben futó folyamatok, nem mint `systemd` rendelkező szolgáltatások el kell egy `SIGKILL`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-757">All user-space processes that are not running as `systemd` services should be killed with a `SIGKILL`.</span></span> <span data-ttu-id="5feb2-758">Indítsa újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-758">Reboot hello VM.</span></span> <span data-ttu-id="5feb2-759">Ha engedélyezi az operációs rendszer lemeztitkosítás egy futó virtuális gépen, tervezze meg a virtuális gép állásidő.</span><span class="sxs-lookup"><span data-stu-id="5feb2-759">When you enable OS disk encryption on a running VM, plan on VM downtime.</span></span>

5. <span data-ttu-id="5feb2-760">Rendszeres időközönként előrehaladást hello titkosítási hello vonatkozó utasításokat használva a hello [következő szakasz](#monitoring-os-encryption-progress).</span><span class="sxs-lookup"><span data-stu-id="5feb2-760">Periodically monitor hello progress of encryption by using hello instructions in hello [next section](#monitoring-os-encryption-progress).</span></span>

6. <span data-ttu-id="5feb2-761">Get-AzureRmVmDiskEncryptionStatus "VMRestartPending" jeleníti meg, miután tooit bejelentkezés vagy hello portálon, a PowerShell vagy a parancssori felület használatával indítsa újra a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="5feb2-761">After Get-AzureRmVmDiskEncryptionStatus shows "VMRestartPending," restart your VM either by signing in tooit or by using hello portal, PowerShell, or CLI.</span></span>
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot hello VM
    ```
<span data-ttu-id="5feb2-762">Ahhoz, hogy újraindította, azt javasoljuk, hogy menti [rendszerindítási diagnosztika](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="5feb2-762">Before you reboot, we recommend that you save [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) of hello VM.</span></span>

#### <a name="monitoring-os-encryption-progress"></a><span data-ttu-id="5feb2-763">Az operációs rendszer titkosítási folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="5feb2-763">Monitoring OS encryption progress</span></span>
<span data-ttu-id="5feb2-764">Figyelheti, hogy az operációs rendszer titkosítási folyamat három módon:</span><span class="sxs-lookup"><span data-stu-id="5feb2-764">You can monitor OS encryption progress in three ways:</span></span>

* <span data-ttu-id="5feb2-765">Használjon hello `Get-AzureRmVmDiskEncryptionStatus` parancsmag és hello Feladatnézetben mező vizsgálata:</span><span class="sxs-lookup"><span data-stu-id="5feb2-765">Use hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet and inspect hello ProgressMessage field:</span></span>
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 <span data-ttu-id="5feb2-766">Miután hello VM eléri "Az operációs rendszer lemezén lépések titkosítási", körülbelül 40 vesz igénybe egy prémium szintű storage too50 percek biztonsági VM.</span><span class="sxs-lookup"><span data-stu-id="5feb2-766">After hello VM reaches "OS disk encryption started," it takes about 40 too50 minutes on a Premium-storage backed VM.</span></span>

 <span data-ttu-id="5feb2-767">Mert [#388 ki](https://github.com/Azure/WALinuxAgent/issues/388) WALinuxAgent, a `OsVolumeEncrypted` és `DataVolumesEncrypted` megjelennek, `Unknown` az egyes terjesztési.</span><span class="sxs-lookup"><span data-stu-id="5feb2-767">Because of [issue #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` and `DataVolumesEncrypted` show up as `Unknown` in some distributions.</span></span> <span data-ttu-id="5feb2-768">A WALinuxAgent verzió 2.1.5 és újabb verziók, ez a probléma fennáll automatikusan.</span><span class="sxs-lookup"><span data-stu-id="5feb2-768">With WALinuxAgent version 2.1.5 and later, this issue is fixed automatically.</span></span> <span data-ttu-id="5feb2-769">Ha látja `Unknown` hello kimeneti ellenőrizheti lemeztitkosítás állapot hello Azure erőforrás-kezelő használatával.</span><span class="sxs-lookup"><span data-stu-id="5feb2-769">If you see `Unknown` in hello output, you can verify disk-encryption status by using hello Azure Resource Explorer.</span></span>

 <span data-ttu-id="5feb2-770">Nyissa meg túl[Azure erőforrás-kezelő](https://resources.azure.com/), majd bontsa ki a bal oldali panelen a hello kiválasztása ebben a hierarchiában:</span><span class="sxs-lookup"><span data-stu-id="5feb2-770">Go too[Azure Resource Explorer](https://resources.azure.com/), and then expand this hierarchy in hello selection panel on left:</span></span>

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 <span data-ttu-id="5feb2-771">Hello InstanceView görgessen le a meghajtók toosee hello titkosítási állapotát.</span><span class="sxs-lookup"><span data-stu-id="5feb2-771">In hello InstanceView, scroll down toosee hello encryption status of your drives.</span></span>

 ![Virtuális gép példányait tartalmazó nézet](./media/azure-security-disk-encryption/vm-instanceview.png)

* <span data-ttu-id="5feb2-773">Nézze meg [rendszerindítási diagnosztika](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span><span class="sxs-lookup"><span data-stu-id="5feb2-773">Look at [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span></span> <span data-ttu-id="5feb2-774">Hello ADE bővítmény üzeneteit kell előtagként `[AzureDiskEncryption]`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-774">Messages from hello ADE extension should be prefixed with `[AzureDiskEncryption]`.</span></span>

* <span data-ttu-id="5feb2-775">Jelentkezzen be a virtuális gép SSH-kapcsolaton keresztül toohello, és hello bővítmény napló az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="5feb2-775">Sign in toohello VM via SSH, and get hello extension log from:</span></span>

    <span data-ttu-id="5feb2-776">/var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span><span class="sxs-lookup"><span data-stu-id="5feb2-776">/var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span></span>

 <span data-ttu-id="5feb2-777">Azt javasoljuk, hogy nem bejelentkezik toohello VM közben az operációs rendszer titkosítás folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="5feb2-777">We recommend that you do not sign in toohello VM while OS encryption is in progress.</span></span> <span data-ttu-id="5feb2-778">Hello naplók másolása, csak ha hello más két módszer sikertelen.</span><span class="sxs-lookup"><span data-stu-id="5feb2-778">Copy hello logs only when hello other two methods have failed.</span></span>

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a><span data-ttu-id="5feb2-779">Előzetes titkosítással Linux virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="5feb2-779">Prepare a pre-encrypted Linux VHD</span></span>
##### <a name="ubuntu-16"></a><span data-ttu-id="5feb2-780">Ubuntu 16</span><span class="sxs-lookup"><span data-stu-id="5feb2-780">Ubuntu 16</span></span>
<span data-ttu-id="5feb2-781">Hello terjesztési telepítése közben titkosítás konfigurálása hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="5feb2-781">Configure encryption during hello distribution installation by doing hello following:</span></span>

1. <span data-ttu-id="5feb2-782">Válassza ki **titkosított kötetek konfigurálása** amikor hello lemezek particionálása-e.</span><span class="sxs-lookup"><span data-stu-id="5feb2-782">Select **Configure encrypted volumes** when you partition hello disks.</span></span>

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. <span data-ttu-id="5feb2-784">Hozzon létre egy külön rendszerindítási meghajtót, amely nem lesznek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="5feb2-784">Create a separate boot drive, which must not be encrypted.</span></span> <span data-ttu-id="5feb2-785">A legfelső szintű meghajtó titkosítására.</span><span class="sxs-lookup"><span data-stu-id="5feb2-785">Encrypt your root drive.</span></span>

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. <span data-ttu-id="5feb2-787">Adjon meg egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-787">Provide a passphrase.</span></span> <span data-ttu-id="5feb2-788">Ez az, hogy töltsön toohello kulcstároló hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-788">This is hello passphrase that you upload toohello key vault.</span></span>

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. <span data-ttu-id="5feb2-790">Fejezze be a particionálást.</span><span class="sxs-lookup"><span data-stu-id="5feb2-790">Finish partitioning.</span></span>

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. <span data-ttu-id="5feb2-792">Hello virtuális gép rendszerindítási és egy jelszót a rendszer felkéri, használja a 3. lépésben megadott hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-792">When you boot hello VM and are asked for a passphrase, use hello passphrase you provided in step 3.</span></span>

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. <span data-ttu-id="5feb2-794">Hello virtuális gép előkészítése az Azure használatával történő feltöltése [ezeket az utasításokat](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="5feb2-794">Prepare hello VM for uploading into Azure using [these instructions](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span></span> <span data-ttu-id="5feb2-795">Ne futtassa a még hello utolsó lépése (megszüntetési hello VM).</span><span class="sxs-lookup"><span data-stu-id="5feb2-795">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

<span data-ttu-id="5feb2-796">Titkosítási toowork konfigurálása az Azure-ral hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="5feb2-796">Configure encryption toowork with Azure by doing hello following:</span></span>

1. <span data-ttu-id="5feb2-797">Hozzon létre egy fájlt, a /usr/local/sbin/azure_crypt_key.sh, és a következő parancsfájl hello hello tartalom.</span><span class="sxs-lookup"><span data-stu-id="5feb2-797">Create a file under /usr/local/sbin/azure_crypt_key.sh, with hello content in hello following script.</span></span> <span data-ttu-id="5feb2-798">Nagy figyelmet toohello KeyFileName, mert hello Azure által használt jelszót fájlnév.</span><span class="sxs-lookup"><span data-stu-id="5feb2-798">Pay attention toohello KeyFileName, because it is hello passphrase file name used by Azure.</span></span>

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED toofind suitable passphrase file ..." >&2
        echo -n "Try tooenter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. <span data-ttu-id="5feb2-799">A hello a titkosítási config módosítása */etc/crypttab*.</span><span class="sxs-lookup"><span data-stu-id="5feb2-799">Change hello crypt config in */etc/crypttab*.</span></span> <span data-ttu-id="5feb2-800">A listának így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="5feb2-800">It should look like this:</span></span>
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. <span data-ttu-id="5feb2-801">Ha az éppen szerkesztett *azure_crypt_key.sh* Windows, és másolja át tooLinux futtatása `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-801">If you are editing *azure_crypt_key.sh* in Windows and you copied it tooLinux, run `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span></span>

4. <span data-ttu-id="5feb2-802">Végrehajtható engedélyek toohello parancsfájl hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="5feb2-802">Add executable permissions toohello script:</span></span>
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. <span data-ttu-id="5feb2-803">Szerkesztés */etc/initramfs-tools/modules* sorok hozzáfűzésével:</span><span class="sxs-lookup"><span data-stu-id="5feb2-803">Edit */etc/initramfs-tools/modules* by appending lines:</span></span>
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. <span data-ttu-id="5feb2-804">Futtatás `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` érvénybe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="5feb2-804">Run `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` take effect.</span></span>

7. <span data-ttu-id="5feb2-805">Most hello méretű képes kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="5feb2-805">Now you can deprovision hello VM.</span></span>

 ![Ubuntu 16.04 beállítása](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. <span data-ttu-id="5feb2-807">Továbbra is a következő lépés toohello és [a virtuális merevlemez feltöltéséhez](#upload-encrypted-vhd-to-an-azure-storage-account) az Azure.</span><span class="sxs-lookup"><span data-stu-id="5feb2-807">Continue toohello next step and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="opensuse-132"></a><span data-ttu-id="5feb2-808">openSUSE, 13.2</span><span class="sxs-lookup"><span data-stu-id="5feb2-808">openSUSE 13.2</span></span>
<span data-ttu-id="5feb2-809">hello terjesztési a telepítés során tooconfigure titkosítási hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-809">tooconfigure encryption during hello distribution installation, do hello following:</span></span>
1. <span data-ttu-id="5feb2-810">Partícióazonosító hello lemezek, válassza ki **titkosítása kötet csoport**, majd írja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-810">When you partition hello disks, select **Encrypt Volume Group**, and then enter a password.</span></span> <span data-ttu-id="5feb2-811">Ez az, hogy fel kell töltenie tooyour kulcstároló hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-811">This is hello password that you will upload tooyour key vault.</span></span>

 ![openSUSE, 13.2 beállítása](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. <span data-ttu-id="5feb2-813">Rendszerindító hello VM az Ön jelszavát.</span><span class="sxs-lookup"><span data-stu-id="5feb2-813">Boot hello VM using your password.</span></span>

 ![openSUSE, 13.2 beállítása](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. <span data-ttu-id="5feb2-815">Prepare VM hello tooAzure feltöltése hello utasításait követve [SLES vagy openSUSE virtuális gép előkészítése Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span><span class="sxs-lookup"><span data-stu-id="5feb2-815">Prepare hello VM for uploading tooAzure by following hello instructions in [Prepare a SLES or openSUSE virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span></span> <span data-ttu-id="5feb2-816">Ne futtassa a még hello utolsó lépése (megszüntetési hello VM).</span><span class="sxs-lookup"><span data-stu-id="5feb2-816">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

<span data-ttu-id="5feb2-817">az Azure-tooconfigure titkosítási toowork hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-817">tooconfigure encryption toowork with Azure, do hello following:</span></span>
1. <span data-ttu-id="5feb2-818">Hello /etc/dracut.conf szerkesztése, és adja hozzá a következő sor hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-818">Edit hello /etc/dracut.conf, and add hello following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. <span data-ttu-id="5feb2-819">Ezek a sorok hello hello végéig megjegyzésbe /usr/lib/dracut/modules.d/90crypt/module-setup.sh fájl:</span><span class="sxs-lookup"><span data-stu-id="5feb2-819">Comment out these lines by hello end of hello file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. <span data-ttu-id="5feb2-820">A következő sor hello fájl /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello elején hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="5feb2-820">Append hello following line at hello beginning of hello file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
 ```
    DRACUT_SYSTEMD=0
 ```
<span data-ttu-id="5feb2-821">És összes előfordulását módosítsa:</span><span class="sxs-lookup"><span data-stu-id="5feb2-821">And change all occurrences of:</span></span>
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
<span data-ttu-id="5feb2-822">erre:</span><span class="sxs-lookup"><span data-stu-id="5feb2-822">to:</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="5feb2-823">/Usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh szerkesztése és fűznünk túl "# nyitott LUKS eszköz":</span><span class="sxs-lookup"><span data-stu-id="5feb2-823">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append it too“# Open LUKS device”:</span></span>

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. <span data-ttu-id="5feb2-824">Futtatás `/usr/sbin/dracut -f -v` tooupdate hello initrd.</span><span class="sxs-lookup"><span data-stu-id="5feb2-824">Run `/usr/sbin/dracut -f -v` tooupdate hello initrd.</span></span>

6. <span data-ttu-id="5feb2-825">Most azt is kiosztásának megszüntetése hello VM és [a virtuális merevlemez feltöltéséhez](#upload-encrypted-vhd-to-an-azure-storage-account) az Azure.</span><span class="sxs-lookup"><span data-stu-id="5feb2-825">Now you can deprovision hello VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="centos-7"></a><span data-ttu-id="5feb2-826">CentOS 7</span><span class="sxs-lookup"><span data-stu-id="5feb2-826">CentOS 7</span></span>
<span data-ttu-id="5feb2-827">hello terjesztési a telepítés során tooconfigure titkosítási hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-827">tooconfigure encryption during hello distribution installation, do hello following:</span></span>
1. <span data-ttu-id="5feb2-828">Válassza ki **az adatok titkosítása** amikor particionálni a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="5feb2-828">Select **Encrypt my data** when you partition disks.</span></span>

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. <span data-ttu-id="5feb2-830">Győződjön meg arról, hogy **titkosítása** van kiválasztva a legfelső szintű partíció.</span><span class="sxs-lookup"><span data-stu-id="5feb2-830">Make sure **Encrypt** is selected for root partition.</span></span>

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. <span data-ttu-id="5feb2-832">Adjon meg egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-832">Provide a passphrase.</span></span> <span data-ttu-id="5feb2-833">Ez az, hogy fel kell töltenie tooyour kulcstároló hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-833">This is hello passphrase that you will upload tooyour key vault.</span></span>

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. <span data-ttu-id="5feb2-835">Hello virtuális gép rendszerindítási és egy jelszót a rendszer felkéri, használja a 3. lépésben megadott hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5feb2-835">When you boot hello VM and are asked for a passphrase, use hello passphrase you provided in step 3.</span></span>

 ![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. <span data-ttu-id="5feb2-837">Prepare hello VM feltöltése az Azure hello "CentOS 7.0 +" utasításait használatával [CentOS-alapú virtuális gép előkészítése Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span><span class="sxs-lookup"><span data-stu-id="5feb2-837">Prepare hello VM for uploading into Azure by using hello "CentOS 7.0+" instructions in [Prepare a CentOS-based virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span></span> <span data-ttu-id="5feb2-838">Ne futtassa a még hello utolsó lépése (megszüntetési hello VM).</span><span class="sxs-lookup"><span data-stu-id="5feb2-838">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

6. <span data-ttu-id="5feb2-839">Most azt is kiosztásának megszüntetése hello VM és [a virtuális merevlemez feltöltéséhez](#upload-encrypted-vhd-to-an-azure-storage-account) az Azure.</span><span class="sxs-lookup"><span data-stu-id="5feb2-839">Now you can deprovision hello VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

<span data-ttu-id="5feb2-840">az Azure-tooconfigure titkosítási toowork hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5feb2-840">tooconfigure encryption toowork with Azure, do hello following:</span></span>

1. <span data-ttu-id="5feb2-841">Hello /etc/dracut.conf szerkesztése, és adja hozzá a következő sor hello:</span><span class="sxs-lookup"><span data-stu-id="5feb2-841">Edit hello /etc/dracut.conf, and add hello following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. <span data-ttu-id="5feb2-842">Ezek a sorok hello hello végéig megjegyzésbe /usr/lib/dracut/modules.d/90crypt/module-setup.sh fájl:</span><span class="sxs-lookup"><span data-stu-id="5feb2-842">Comment out these lines by hello end of hello file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. <span data-ttu-id="5feb2-843">A következő sor hello fájl /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello elején hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="5feb2-843">Append hello following line at hello beginning of hello file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
```
    DRACUT_SYSTEMD=0
```
<span data-ttu-id="5feb2-844">És összes előfordulását módosítsa:</span><span class="sxs-lookup"><span data-stu-id="5feb2-844">And change all occurrences of:</span></span>
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
<span data-ttu-id="5feb2-845">erre:</span><span class="sxs-lookup"><span data-stu-id="5feb2-845">to</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="5feb2-846">/Usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh szerkesztése és hozzáfűzni a "# nyitott LUKS eszköz" hello után:</span><span class="sxs-lookup"><span data-stu-id="5feb2-846">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append this after hello “# Open LUKS device”:</span></span>
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. <span data-ttu-id="5feb2-847">Futtassa a hello "/ usr/sbin/dracut - f - v" tooupdate hello initrd.</span><span class="sxs-lookup"><span data-stu-id="5feb2-847">Run hello “/usr/sbin/dracut -f -v” tooupdate hello initrd.</span></span>

![CentOS 7 beállítása](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-tooan-azure-storage-account"></a><span data-ttu-id="5feb2-849">Töltse fel a titkosított virtuális merevlemez tooan Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="5feb2-849">Upload encrypted VHD tooan Azure storage account</span></span>
<span data-ttu-id="5feb2-850">Miután a BitLocker-titkosítást vagy a DM-Crypt titkosítás engedélyezve van, hello helyi titkosított VHD igényeinek feltöltött toobe tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5feb2-850">After BitLocker encryption or DM-Crypt encryption is enabled, hello local encrypted VHD needs toobe uploaded tooyour storage account.</span></span>

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-hello-disk-encryption-secret-for-hello-pre-encrypted-vm-tooyour-key-vault"></a><span data-ttu-id="5feb2-851">Lemeztitkosítás titkos kulcsát hello hello előzetes titkosítással VM tooyour kulcstároló feltöltése</span><span class="sxs-lookup"><span data-stu-id="5feb2-851">Upload hello disk-encryption secret for hello pre-encrypted VM tooyour key vault</span></span>
<span data-ttu-id="5feb2-852">hello lemeztitkosítás secret beszerzett korábban fel kell tölteni, mint a key vaultban lévő titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="5feb2-852">hello disk-encryption secret that you obtained previously must be uploaded as a secret in your key vault.</span></span> <span data-ttu-id="5feb2-853">hello kulcstároló kell toohave lemeztitkosítás és az Azure AD-ügyfél engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="5feb2-853">hello key vault needs toohave disk encryption and permissions enabled for your Azure AD client.</span></span>

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a><span data-ttu-id="5feb2-854">Lemez titkosítási titkos kulcs nem titkosított a KEK</span><span class="sxs-lookup"><span data-stu-id="5feb2-854">Disk encryption secret not encrypted with a KEK</span></span>
<span data-ttu-id="5feb2-855">a kulcstároló, használja a hello titkos másolatot tooset [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="5feb2-855">tooset up hello secret in your key vault, use [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span></span> <span data-ttu-id="5feb2-856">Ha egy Windows rendszerű virtuális gép, hello bek fájl base64 karakterláncként van kódolva, és majd feltöltött tooyour kulcstároló hello segítségével `Set-AzureKeyVaultSecret` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5feb2-856">If you have a Windows virtual machine, hello bek file is encoded as a base64 string and then uploaded tooyour key vault using hello `Set-AzureKeyVaultSecret` cmdlet.</span></span> <span data-ttu-id="5feb2-857">Linux hello jelszó base64 karakterláncként van kódolva, és majd feltöltött toohello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="5feb2-857">For Linux, hello passphrase is encoded as a base64 string and then uploaded toohello key vault.</span></span> <span data-ttu-id="5feb2-858">Ezenkívül győződjön meg arról, hogy a következő címkék hello vannak beállítva hello kulcstároló hello titkos kulcs létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="5feb2-858">In addition, make sure that hello following tags are set when you create hello secret in hello key vault.</span></span>

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

<span data-ttu-id="5feb2-859">Használjon hello `$secretUrl` hello következő lépésben a [hello OS lemez csatolása KEK használata nélkül](#without-using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="5feb2-859">Use hello `$secretUrl` in hello next step for [attaching hello OS disk without using KEK](#without-using-a-kek).</span></span>

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a><span data-ttu-id="5feb2-860">Lemez titkosítási titkos kulcsot egy KEK titkosítva</span><span class="sxs-lookup"><span data-stu-id="5feb2-860">Disk encryption secret encrypted with a KEK</span></span>
<span data-ttu-id="5feb2-861">Mielőtt feltölti hello titkos toohello kulcstároló, opcionálisan titkosításhoz kulcs titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="5feb2-861">Before you upload hello secret toohello key vault, you can optionally encrypt it by using a key encryption key.</span></span> <span data-ttu-id="5feb2-862">Használjon hello sortörés [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst hello kulcs titkosítási kulcs használatával hello titkos kulcs titkosításához.</span><span class="sxs-lookup"><span data-stu-id="5feb2-862">Use hello wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst encrypt hello secret using hello key encryption key.</span></span> <span data-ttu-id="5feb2-863">hello sortörés művelet eredménye a base64 kódolású URL karakterláncot, majd feltöltheti egy titkos kulcsként hello segítségével [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5feb2-863">hello output of this wrap operation is a base64 URL encoded string, which you can then upload as a secret by using hello [`Set-AzureKeyVaultSecret`](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.</span></span>

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

<span data-ttu-id="5feb2-864">Használjon `$KeyEncryptionKey` és `$secretUrl` hello következő lépésben a [hello operációsrendszer-lemez csatolása KEK használatával](#using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="5feb2-864">Use `$KeyEncryptionKey` and `$secretUrl` in hello next step for [attaching hello OS disk using KEK](#using-a-kek).</span></span>

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a><span data-ttu-id="5feb2-865">Adjon meg egy titkos URL-cím, egy operációsrendszer-lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="5feb2-865">Specify a secret URL when you attach an OS disk</span></span>
#### <a name="without-using-a-kek"></a><span data-ttu-id="5feb2-866">Egy KEK használata nélkül</span><span class="sxs-lookup"><span data-stu-id="5feb2-866">Without using a KEK</span></span>
<span data-ttu-id="5feb2-867">Hello operációsrendszer-lemez konfigurálhatóak, amíg toopass kell `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-867">While you are attaching hello OS disk, you need toopass `$secretUrl`.</span></span> <span data-ttu-id="5feb2-868">hello URL-cím hello "lemez-titkosítás a KEK nem titkosított titkos" szakaszban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-868">hello URL was generated in hello "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a><span data-ttu-id="5feb2-869">Egy KEK használatával</span><span class="sxs-lookup"><span data-stu-id="5feb2-869">Using a KEK</span></span>
<span data-ttu-id="5feb2-870">Hello operációsrendszer-lemez csatolása átadni `$KeyEncryptionKey` és `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="5feb2-870">When you attach hello OS disk, pass `$KeyEncryptionKey` and `$secretUrl`.</span></span> <span data-ttu-id="5feb2-871">hello URL-cím hello "lemez-titkosítás a KEK nem titkosított titkos" szakaszban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="5feb2-871">hello URL was generated in hello "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a><span data-ttu-id="5feb2-872">Ez az útmutató letöltése</span><span class="sxs-lookup"><span data-stu-id="5feb2-872">Download this guide</span></span>
<span data-ttu-id="5feb2-873">Ez az útmutató letölthető hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span><span class="sxs-lookup"><span data-stu-id="5feb2-873">You can download this guide from hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span></span>

## <a name="for-more-information"></a><span data-ttu-id="5feb2-874">További információ</span><span class="sxs-lookup"><span data-stu-id="5feb2-874">For more information</span></span>
[<span data-ttu-id="5feb2-875">Fedezze fel az Azure Disk Encryption Azure PowerShell - 1. rész</span><span class="sxs-lookup"><span data-stu-id="5feb2-875">Explore Azure Disk Encryption with Azure PowerShell - Part 1</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[<span data-ttu-id="5feb2-876">Fedezze fel az Azure Disk Encryption Azure PowerShell - 2. rész</span><span class="sxs-lookup"><span data-stu-id="5feb2-876">Explore Azure Disk Encryption with Azure PowerShell - Part 2</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
