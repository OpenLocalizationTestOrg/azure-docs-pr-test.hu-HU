---
title: "aaaPrepare helyszíni VMware erőforrásokat az Azure Site Recovery replikációs tooAzure |} Microsoft Docs"
description: "Összefoglalja azokat a VMware virtuális gépek tooAzure tárolási munkaterheléseinek replikálásához kell hello lépéseket"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 09d81f15f6ee764135a62f5555e458c55fa30048
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-tooazure"></a><span data-ttu-id="a9ea3-103">6. lépés: Felkészülés a helyszíni VMware replikációs tooAzure</span><span class="sxs-lookup"><span data-stu-id="a9ea3-103">Step 6: Prepare on-premises VMware replication tooAzure</span></span>

<span data-ttu-id="a9ea3-104">Ez a cikk tooprepare helyszíni VMware kiszolgálók toointeract az Azure Site Recovery használata hello utasításokat, és készítse elő a VMWare virtuális gépek hello mobilitási szolgáltatás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-104">Use hello instructions in this article tooprepare on-premises VMware servers toointeract with Azure Site Recovery, and prepare VMWare VMs for installation of hello Mobility service.</span></span> <span data-ttu-id="a9ea3-105">hello mobilitási szolgáltatás ügynököt telepíteni kell az összes helyszíni virtuális gépeken, amelyet az tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-105">hello Mobility service agent must be installed on all on-premises VMs that you want tooreplicate tooAzure.</span></span>

## <a name="prepare-for-automatic-discovery"></a><span data-ttu-id="a9ea3-106">Automatikus felderítés előkészítése</span><span class="sxs-lookup"><span data-stu-id="a9ea3-106">Prepare for automatic discovery</span></span>

<span data-ttu-id="a9ea3-107">A Site Recovery automatikusan észleli a vSphere ESXi-gazdagépek (függetlenül a vCenter-kiszolgáló) futó virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-107">Site Recovery automatically discovers VMs running on vSphere ESXi hosts (with or without a vCenter server).</span></span> <span data-ttu-id="a9ea3-108">Az automatikus felderítést, a Site Recovery kell egy fiók tooaccess gazdagépek és a kiszolgálók:</span><span class="sxs-lookup"><span data-stu-id="a9ea3-108">For automatic discovery, Site Recovery needs an account tooaccess hosts and servers:</span></span>

1. <span data-ttu-id="a9ea3-109">egy külön fiókot toouse (szinten hello vCenter hello az alábbi táblázatban ismertetett hello engedélyeivel szerepkör létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-109">toouse a dedicated account, create a role (at hello vCenter level, with hello permissions described in hello table below.</span></span> <span data-ttu-id="a9ea3-110">Nevezze el, például a **Azure_Site_Recovery**.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-110">Give it a name such as **Azure_Site_Recovery**.</span></span>
2. <span data-ttu-id="a9ea3-111">Ezután hello vSphere gazdagép/vCenter-kiszolgáló egy felhasználó létrehozása, és rendelje hozzá hello szerepkör toohello felhasználót.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-111">Then, create a user on hello vSphere host/vCenter server, and assign hello role toohello user.</span></span> <span data-ttu-id="a9ea3-112">Ez a felhasználói fiók a Site Recovery üzembe helyezése során adja meg.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-112">You specify this user account during Site Recovery deployment.</span></span>


### <a name="vmware-account-permissions"></a><span data-ttu-id="a9ea3-113">VMware fiókjához hozzárendelt jogosultságoktól</span><span class="sxs-lookup"><span data-stu-id="a9ea3-113">VMware account permissions</span></span>

<span data-ttu-id="a9ea3-114">A Site Recovery hozzáférés tooVMware van szüksége, hello folyamat server tooautomatically virtuális gépek felderítése, valamint a feladatátvétel és a feladat-visszavételt a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-114">Site Recovery needs access tooVMware for hello process server tooautomatically discover VMs, and for failover and failback of VMs.</span></span>

- <span data-ttu-id="a9ea3-115">**Telepítse át**: csak toomigrate VMware virtuális gépek tooAzure, nélkül szeretné legalább egyszer sikertelen vissza őket, ha használható a VMware-fiók egy csak olvasható szerepkör.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-115">**Migrate**: If you only want toomigrate VMware VMs tooAzure, without ever failing them back, you can use a VMware account with a read-only role.</span></span> <span data-ttu-id="a9ea3-116">Ilyen szerepkör feladatátvételi futtatható, de nem lehet leállítani a védett forrásgépek.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-116">Such a role can run failover, but can't shut down protected source machines.</span></span> <span data-ttu-id="a9ea3-117">Ez nem szükséges az áttelepítéshez.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-117">This isn't necessary for migration.</span></span>
- <span data-ttu-id="a9ea3-118">**Replikálás/Recover**: Ha toodeploy (replikálja, feladatátvétel, feladat-visszavétel) teljes replikáció hello fióknak kell lennie a képes toorun műveletek, például a létrehozásával és lemezek eltávolításával, bekapcsolása a virtuális gépek stb.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-118">**Replicate/Recover**: If you want toodeploy full replication (replicate, failover, failback) hello account must be able toorun operations such as creating and removing disks, powering on VMs etc.</span></span>
- <span data-ttu-id="a9ea3-119">**Automatikus felderítés**: legalább egy csak olvasható fiókot kell megadni.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-119">**Automatic discovery**: At least a read-only account is required.</span></span>


<span data-ttu-id="a9ea3-120">**Tevékenység**</span><span class="sxs-lookup"><span data-stu-id="a9ea3-120">**Task**</span></span> | <span data-ttu-id="a9ea3-121">**Szükséges fiók/szerepkör**</span><span class="sxs-lookup"><span data-stu-id="a9ea3-121">**Required account/role**</span></span> | <span data-ttu-id="a9ea3-122">**Engedélyek**</span><span class="sxs-lookup"><span data-stu-id="a9ea3-122">**Permissions**</span></span> | <span data-ttu-id="a9ea3-123">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="a9ea3-123">**Details**</span></span>
--- | --- | --- | ---
<span data-ttu-id="a9ea3-124">**A folyamatkiszolgáló automatikusan felderíti a VMware virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="a9ea3-124">**Process server automatically discovers VMware VMs**</span></span> | <span data-ttu-id="a9ea3-125">Legalább egy írásvédett felhasználó van szüksége</span><span class="sxs-lookup"><span data-stu-id="a9ea3-125">You need at least a read-only user</span></span> | <span data-ttu-id="a9ea3-126">Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható</span><span class="sxs-lookup"><span data-stu-id="a9ea3-126">Data Center object –> Propagate tooChild Object, role=Read-only</span></span> | <span data-ttu-id="a9ea3-127">Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-127">User assigned at datacenter level, and has access tooall hello objects in hello datacenter.</span></span><br/><br/> <span data-ttu-id="a9ea3-128">toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).</span><span class="sxs-lookup"><span data-stu-id="a9ea3-128">toorestrict access, assign hello **No access** role with hello **Propagate toochild** object, toohello child objects (vSphere hosts, datastores, VMs and networks).</span></span>
<span data-ttu-id="a9ea3-129">**Feladatátvétel**</span><span class="sxs-lookup"><span data-stu-id="a9ea3-129">**Failover**</span></span> | <span data-ttu-id="a9ea3-130">Legalább egy írásvédett felhasználó van szüksége</span><span class="sxs-lookup"><span data-stu-id="a9ea3-130">You need at least a read-only user</span></span> | <span data-ttu-id="a9ea3-131">Adatközpont objektum –> propagálása tooChild objektum szerepkör = csak olvasható</span><span class="sxs-lookup"><span data-stu-id="a9ea3-131">Data Center object –> Propagate tooChild Object, role=Read-only</span></span> | <span data-ttu-id="a9ea3-132">Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-132">User assigned at datacenter level, and has access tooall hello objects in hello datacenter.</span></span><br/><br/> <span data-ttu-id="a9ea3-133">toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok) objektum.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-133">toorestrict access, assign hello **No access** role with hello **Propagate toochild** object toohello child objects (vSphere hosts, datastores, VMs and networks).</span></span><br/><br/> <span data-ttu-id="a9ea3-134">Akkor hasznos, ha az áttelepítési célokból, de nem a teljes replikáció, feladatátvétel, feladat-visszavétel.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-134">Useful for migration purposes, but not full replication, failover, failback.</span></span>
<span data-ttu-id="a9ea3-135">**A feladatátvételi és a feladat-visszavétel**</span><span class="sxs-lookup"><span data-stu-id="a9ea3-135">**Failover and failback**</span></span> | <span data-ttu-id="a9ea3-136">Javasoljuk, hogy (Azure_Site_Recovery) szerepkör létrehozása hello szükséges engedélyekkel, és hozzárendelheti a hello szerepkör tooa VMware felhasználó vagy csoport</span><span class="sxs-lookup"><span data-stu-id="a9ea3-136">We suggest you create a role (Azure_Site_Recovery) with hello required permissions, and then assign hello role tooa VMware user or group</span></span> | <span data-ttu-id="a9ea3-137">Adatközpont objektum –> propagálása tooChild objektum szerepkör = Azure_Site_Recovery</span><span class="sxs-lookup"><span data-stu-id="a9ea3-137">Data Center object –> Propagate tooChild Object, role=Azure_Site_Recovery</span></span><br/><br/> <span data-ttu-id="a9ea3-138">Datastore foglaljon le terület ->, keresse meg az adattároló, alacsony szintű fájlműveletek, távolítsa el a fájlt, frissítse a virtuális géphez tartozó fájlokat</span><span class="sxs-lookup"><span data-stu-id="a9ea3-138">Datastore -> Allocate space, browse datastore, low-level file operations, remove file, update virtual machine files</span></span><br/><br/> <span data-ttu-id="a9ea3-139">Hálózat -> hálózat hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a9ea3-139">Network -> Network assign</span></span><br/><br/> <span data-ttu-id="a9ea3-140">Erőforrás -> rendelje hozzá a virtuális gép tooresource készlet, energiaforrással rendelkező ki a virtuális gép áttelepítése, energiaforrással rendelkező, a virtuális gép áttelepítése</span><span class="sxs-lookup"><span data-stu-id="a9ea3-140">Resource -> Assign VM tooresource pool, migrate powered off VM, migrate powered on VM</span></span><br/><br/> <span data-ttu-id="a9ea3-141">Feladatok -> hozzon létre feladat, a frissítési feladat</span><span class="sxs-lookup"><span data-stu-id="a9ea3-141">Tasks -> Create task, update task</span></span><br/><br/> <span data-ttu-id="a9ea3-142">Virtuális gép konfigurációja -></span><span class="sxs-lookup"><span data-stu-id="a9ea3-142">Virtual machine -> Configuration</span></span><br/><br/> <span data-ttu-id="a9ea3-143">Virtuálisgép -> kommunikáció -> válasz kérdést, az eszköz kapcsolat, a CD media konfigurálásához, a konfigurálásához a hajlékonylemezes adathordozót, kapcsolja ki, a bekapcsolás, VMware-eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="a9ea3-143">Virtual machine -> Interact -> answer question, device connection, configure CD media, configure floppy media, power off, power on, VMware tools install</span></span><br/><br/> <span data-ttu-id="a9ea3-144">Virtuálisgép -> leltár -> létrehozási, regisztrálása, unregister</span><span class="sxs-lookup"><span data-stu-id="a9ea3-144">Virtual machine -> Inventory -> Create, register, unregister</span></span><br/><br/> <span data-ttu-id="a9ea3-145">Virtuálisgép -> kiépítés engedélyezése virtuális gép letöltési ->, hogy a virtuálisgép-fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="a9ea3-145">Virtual machine -> Provisioning -> Allow virtual machine download, allow virtual machine files upload</span></span><br/><br/> <span data-ttu-id="a9ea3-146">Virtuális gép pillanatkép ->-Remove pillanatképek ></span><span class="sxs-lookup"><span data-stu-id="a9ea3-146">Virtual machine -> Snapshots -> Remove snapshots</span></span> | <span data-ttu-id="a9ea3-147">Felhasználó datacenter szinten hozzárendelt, és hozzáférést tooall hello objektumok hello adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-147">User assigned at datacenter level, and has access tooall hello objects in hello datacenter.</span></span><br/><br/> <span data-ttu-id="a9ea3-148">toorestrict hozzáférés hozzárendelése hello **nem lehet hozzáférni** hello szerepkört **toochild propagálása** objektum toohello gyermekobjektum (vSphere-gazdagép, datastores, virtuális gépek és hálózatok).</span><span class="sxs-lookup"><span data-stu-id="a9ea3-148">toorestrict access, assign hello **No access** role with hello **Propagate toochild** object, toohello child objects (vSphere hosts, datastores, VMs and networks).</span></span>


## <a name="prepare-for-push-installation-of-hello-mobility-service"></a><span data-ttu-id="a9ea3-149">Készítse elő a hello mobilitási szolgáltatás leküldéses telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="a9ea3-149">Prepare for push installation of hello Mobility service</span></span>

<span data-ttu-id="a9ea3-150">kell telepíteni a mobilitási szolgáltatás hello tooreplicate kívánt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-150">hello Mobility service must be installed on all VMs you want tooreplicate.</span></span> <span data-ttu-id="a9ea3-151">Számos módon tooinstall hello szolgáltatás, így az ügyfélleküldéses telepítés hello Site Recovery folyamatkiszolgáló, és a telepítési módszerrel, például a System Center Configuration Manager manuális telepítésére.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-151">There are a number of ways tooinstall hello service, including manual installation, push installation from hello Site Recovery process server, and installation using methods such as System Center Configuration Manager.</span></span>

<span data-ttu-id="a9ea3-152">Ha azt szeretné, hogy toouse ügyfélleküldéses telepítést, egy fiókot, hogy a Site Recovery használhatja-e a virtuális gép tooaccess hello tooprepare kell.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-152">If you want toouse push installation, you need tooprepare an account that Site Recovery can use tooaccess hello VM.</span></span>

- <span data-ttu-id="a9ea3-153">A tartományi vagy helyi fiók használható</span><span class="sxs-lookup"><span data-stu-id="a9ea3-153">You can use a domain or local account</span></span>
- <span data-ttu-id="a9ea3-154">A Windows Ha nem használ egy olyan tartományi fiók kell toodisable távelérési felhasználói vezérlő hello helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-154">For Windows, if you're not using a domain account, you need toodisable Remote User Access control on hello local machine.</span></span> <span data-ttu-id="a9ea3-155">Ez, hello regisztrálni a toodo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, hello DWORD bejegyzés hozzáadása **LocalAccountTokenFilterPolicy**, 1 értékű.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-155">toodo this, in hello register under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, add hello DWORD entry **LocalAccountTokenFilterPolicy**, with a value of 1.</span></span>
- <span data-ttu-id="a9ea3-156">Ha a parancssori felület a Windows hello bejegyzés tooadd szeretne, írja be:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``</span><span class="sxs-lookup"><span data-stu-id="a9ea3-156">If you want tooadd hello registry entry for Windows from a CLI, type:       ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``</span></span>
- <span data-ttu-id="a9ea3-157">Linux hello fióknak root forráskiszolgálón hello Linux kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a9ea3-157">For Linux, hello account should be root on hello source Linux server.</span></span>



## <a name="next-steps"></a><span data-ttu-id="a9ea3-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9ea3-158">Next steps</span></span>

<span data-ttu-id="a9ea3-159">Nyissa meg túl[7. lépés: a tároló létrehozása](vmware-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="a9ea3-159">Go too[Step 7: Create a vault](vmware-walkthrough-create-vault.md)</span></span>