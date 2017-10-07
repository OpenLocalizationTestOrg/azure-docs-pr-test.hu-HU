---
title: "az Azure Site Recovery segítségével VMware tooAzure replikációs aaaPrerequisites |} Microsoft Docs"
description: "VMware virtuális gépek tooAzure hello Azure Site Recovery szolgáltatással munkaterheléseinek replikálása hello előfeltételeit foglalja össze."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 14f8e9713b42a1d8da71223fbbb7a6b7c4303adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-vmware-tooazure-replication"></a><span data-ttu-id="20fd3-103">2. lépés: A VMware tooAzure replikációs hello előfeltételeinek áttekintése</span><span class="sxs-lookup"><span data-stu-id="20fd3-103">Step 2: Review hello prerequisites for VMware tooAzure replication</span></span>

<span data-ttu-id="20fd3-104">Olvassa el a hello Előfeltételek hello a következő táblázat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="20fd3-104">Read hello prerequisites summarized in hello following table.</span></span>

<span data-ttu-id="20fd3-105">**Előfeltétel**</span><span class="sxs-lookup"><span data-stu-id="20fd3-105">**Prerequisite**</span></span> | <span data-ttu-id="20fd3-106">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="20fd3-106">**Details**</span></span>
--- | ---
<span data-ttu-id="20fd3-107">**Azure**</span><span class="sxs-lookup"><span data-stu-id="20fd3-107">**Azure**</span></span> | <span data-ttu-id="20fd3-108">További tudnivalók [Azure-követelmények](site-recovery-prereq.md#azure-requirements)</span><span class="sxs-lookup"><span data-stu-id="20fd3-108">Learn about [Azure requirements](site-recovery-prereq.md#azure-requirements)</span></span>
<span data-ttu-id="20fd3-109">**Helyszíni konfigurációs kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="20fd3-109">**On-premises configuration server**</span></span> | <span data-ttu-id="20fd3-110">Windows Server 2012 R2 rendszerű VMware virtuális gép van szüksége, vagy később.</span><span class="sxs-lookup"><span data-stu-id="20fd3-110">You need a VMware VM running Windows Server 2012 R2 or later.</span></span> <span data-ttu-id="20fd3-111">Ez a kiszolgáló beállítása a Site Recovery üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="20fd3-111">You set up this server during Site Recovery deployment.</span></span><br/><br/> <span data-ttu-id="20fd3-112">Alapértelmezett hello folyamat kiszolgáló és a fő célkiszolgáló is települnek a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="20fd3-112">By default hello process server and master target server are also installed on this VM.</span></span> <span data-ttu-id="20fd3-113">Vertikális felskálázás, előfordulhat, hogy különálló folyamatkiszolgálót, és rendelkezik hello ugyanazok vonatkoznak, mint hello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="20fd3-113">When you scale up, you might need a separate process server, and it has hello same requirements as hello configuration server.</span></span><br/><br/> <span data-ttu-id="20fd3-114">További információ ezen összetevők [Itt](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)</span><span class="sxs-lookup"><span data-stu-id="20fd3-114">Learn more about these components [here](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)</span></span>
<span data-ttu-id="20fd3-115">**Helyszíni VMware-kiszolgálók**</span><span class="sxs-lookup"><span data-stu-id="20fd3-115">**On-premises VMware servers**</span></span> | <span data-ttu-id="20fd3-116">Egy vagy több VMware vSphere, futtató kiszolgálók 6.5, 6.0, 5.5, 5.1 legújabb frissítéseit.</span><span class="sxs-lookup"><span data-stu-id="20fd3-116">One or more VMware vSphere servers, running 6.5, 6.0, 5.5, 5.1 with latest updates.</span></span> <span data-ttu-id="20fd3-117">Kiszolgálók hello azonos hálózati hello konfigurációs kiszolgáló (vagy különálló folyamatkiszolgálót) kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="20fd3-117">Servers should be located in hello same network as hello configuration server (or separate process server).</span></span><br/><br/> <span data-ttu-id="20fd3-118">Ajánlott egy vCenter-kiszolgáló toomanage állomások 6.5, 6.0 vagy 5.5 rendszerű hello legújabb frissítéseit.</span><span class="sxs-lookup"><span data-stu-id="20fd3-118">We recommend a vCenter server toomanage hosts, running 6.5, 6.0 or 5.5 with hello latest updates.</span></span>
<span data-ttu-id="20fd3-119">**A helyszíni virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="20fd3-119">**On-premises VMs**</span></span> | <span data-ttu-id="20fd3-120">Virtuális gépek kívánt tooreplicate rendszerűnek kell lennie egy [támogatott operációs rendszer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), és adott a specifikációknak való [Azure-Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="20fd3-120">VMs you want tooreplicate should be running a [supported operating system](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), and conform with [Azure prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span> <span data-ttu-id="20fd3-121">Virtuális gép VMware-eszközök futó kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="20fd3-121">VM should have VMware tools running.</span></span>
<span data-ttu-id="20fd3-122">**URL-címek**</span><span class="sxs-lookup"><span data-stu-id="20fd3-122">**URLs**</span></span> | <span data-ttu-id="20fd3-123">meg kell hello konfigurációs kiszolgálót toothese URL hozzáférés:</span><span class="sxs-lookup"><span data-stu-id="20fd3-123">hello configuration server needs access toothese URLs:</span></span><br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> <span data-ttu-id="20fd3-124">Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="20fd3-124">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span><br/></br> <span data-ttu-id="20fd3-125">Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.</span><span class="sxs-lookup"><span data-stu-id="20fd3-125">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span><br/></br> <span data-ttu-id="20fd3-126">Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).</span><span class="sxs-lookup"><span data-stu-id="20fd3-126">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span><br/><br/> <span data-ttu-id="20fd3-127">Engedélyezi a hello MySQL letöltési URL-címe: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.</span><span class="sxs-lookup"><span data-stu-id="20fd3-127">Allow this URL for hello MySQL download: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.</span></span>
<span data-ttu-id="20fd3-128">**Mobilitási szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="20fd3-128">**Mobility service**</span></span> | <span data-ttu-id="20fd3-129">Minden replikált virtuális gép telepítve.</span><span class="sxs-lookup"><span data-stu-id="20fd3-129">Installed on every replicated VM.</span></span>




## <a name="limitations"></a><span data-ttu-id="20fd3-130">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="20fd3-130">Limitations</span></span>

<span data-ttu-id="20fd3-131">Győződjön meg arról, hogy hello korlátozások központi telepítése előtt hello táblázat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="20fd3-131">Make sure you understand hello limitations summarized in hello table before you deploy.</span></span>

<span data-ttu-id="20fd3-132">**A korlátozás**</span><span class="sxs-lookup"><span data-stu-id="20fd3-132">**Limitation**</span></span> | <span data-ttu-id="20fd3-133">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="20fd3-133">**Details**</span></span>
--- | ---
<span data-ttu-id="20fd3-134">**Azure**</span><span class="sxs-lookup"><span data-stu-id="20fd3-134">**Azure**</span></span> | <span data-ttu-id="20fd3-135">Tárolási és hálózati fiókoknak kell lenniük a hello és hello-tárolónak ugyanabban a régióban</span><span class="sxs-lookup"><span data-stu-id="20fd3-135">Storage and network accounts must be in hello same region as hello vault</span></span><br/><br/> <span data-ttu-id="20fd3-136">A prémium szintű storage-fiókok használatához is szükség van egy standard fiók toostore replikálási naplók tárolásához</span><span class="sxs-lookup"><span data-stu-id="20fd3-136">If you use a premium storage account, you also need a standard store account toostore replication logs</span></span><br/><br/> <span data-ttu-id="20fd3-137">Közép-és Dél-Indiában toopremium fiókok nem replikálja.</span><span class="sxs-lookup"><span data-stu-id="20fd3-137">You can't replicate toopremium accounts in Central and South India.</span></span>
<span data-ttu-id="20fd3-138">**Helyszíni konfigurációs kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="20fd3-138">**On-premises configuration server**</span></span> | <span data-ttu-id="20fd3-139">VMware virtuális hálózatiadapter-típus VMXNET3 kell lennie.</span><span class="sxs-lookup"><span data-stu-id="20fd3-139">VMware VM adapter type should be VMXNET3.</span></span> <span data-ttu-id="20fd3-140">Ha nem, [a frissítés telepítése](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)</span><span class="sxs-lookup"><span data-stu-id="20fd3-140">If it isn't, [install this update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)</span></span><br/><br/> <span data-ttu-id="20fd3-141">a vSphere PowerCLI 6.0 kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="20fd3-141">vSphere PowerCLI 6.0 should be installed.</span></span><br/><br> <span data-ttu-id="20fd3-142">hello gépet nem lehet tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="20fd3-142">hello machine shouldn't be a domain controller.</span></span> <span data-ttu-id="20fd3-143">hello gépnek rendelkeznie kell egy statikus IP-címet.</span><span class="sxs-lookup"><span data-stu-id="20fd3-143">hello machine should have a static IP address.</span></span><br/><br/> <span data-ttu-id="20fd3-144">hello állomásnév 15 karakter lehet ne legyen, és az operációs rendszer angol nyelven kell lennie.</span><span class="sxs-lookup"><span data-stu-id="20fd3-144">hello host name should be 15 characters or less, and operating system should be in English.</span></span>
<span data-ttu-id="20fd3-145">**VMware**</span><span class="sxs-lookup"><span data-stu-id="20fd3-145">**VMware**</span></span> | <span data-ttu-id="20fd3-146">A Site Recovery nem támogatja az új vCenter és vSphere 6.5-ös és 6.0 szolgáltatások például a kereszt-vCenter vMotion, a virtuális kötetek és a tárolási DRS.</span><span class="sxs-lookup"><span data-stu-id="20fd3-146">Site Recovery doesn't support new vCenter and vSphere 6.5 and 6.0 features such as cross vCenter vMotion, virtual volumes, and storage DRS.</span></span>
<span data-ttu-id="20fd3-147">**Virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="20fd3-147">**VMs**</span></span> | <span data-ttu-id="20fd3-148">Győződjön meg arról [Azure virtuális gép korlátozásai](site-recovery-prereq.md#azure-requirements)</span><span class="sxs-lookup"><span data-stu-id="20fd3-148">Verify [Azure VM limitations](site-recovery-prereq.md#azure-requirements)</span></span><br/><br/> <span data-ttu-id="20fd3-149">Titkosított lemezzel rendelkező virtuális gépeket, vagy az UEFI/EFI-rendszerbetöltés virtuális gépeken nem replikálja.</span><span class="sxs-lookup"><span data-stu-id="20fd3-149">You can't replicate VMs with encrypted disks, or VMs with UEFI/EFI boot.</span></span><br/><br> <span data-ttu-id="20fd3-150">A megosztott lemez nem használhatók.</span><span class="sxs-lookup"><span data-stu-id="20fd3-150">Shared disk clusters aren't supported.</span></span> <span data-ttu-id="20fd3-151">Ha hello forrás virtuális gép rendelkezik hálózati adapterek összevonása, azt konvertálja tooa egyetlen hálózati adapter a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="20fd3-151">If hello source VM has NIC teaming, it's converted tooa single NIC after failover.</span></span><br/><br/> <span data-ttu-id="20fd3-152">Ha a virtuális gép iSCSI-lemez van, a Site Recovery átalakítja tooa VHD-fájlt a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="20fd3-152">If VMs have an iSCSI disk, Site Recovery converts it tooa VHD file after failover.</span></span> <span data-ttu-id="20fd3-153">Hello iSCSI-tároló hello Azure virtuális gép elérhető, ha tooit csatlakozik, és azt és a virtuális merevlemez hello látja.</span><span class="sxs-lookup"><span data-stu-id="20fd3-153">If hello iSCSI target can be reached by hello Azure VM, it connects tooit, and sees both it and hello VHD.</span></span> <span data-ttu-id="20fd3-154">Ha ez történik, válassza le a hello iSCSI-tároló.</span><span class="sxs-lookup"><span data-stu-id="20fd3-154">If this happens, disconnect hello iSCSI target.</span></span><br/><br/> <span data-ttu-id="20fd3-155">Ha azt szeretné, hogy a virtuális Gépre kiterjedő konzisztencia tooenable, amely lehetővé teszi a munkaterhelés ugyanazon toobe helyre hello futó virtuális gépeket együtt tooa azonos adatokat mutasson, nyissa meg a virtuális gép hello 20004 portot.</span><span class="sxs-lookup"><span data-stu-id="20fd3-155">If you want tooenable multi-VM consistency, which enables VMs running hello same workload toobe recovered together tooa consistent data point, open port 20004 on hello VM.</span></span><br/><br/> <span data-ttu-id="20fd3-156">Windows hello C meghajtó telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="20fd3-156">Windows must be installed on hello C drive.</span></span> <span data-ttu-id="20fd3-157">az operációsrendszer-lemez hello alapvető, és nem dinamikus kell lennie.</span><span class="sxs-lookup"><span data-stu-id="20fd3-157">hello OS disk should be basic, and not dynamic.</span></span> <span data-ttu-id="20fd3-158">hello adatlemez dinamikus lehet.</span><span class="sxs-lookup"><span data-stu-id="20fd3-158">hello data disk can be dynamic.</span></span><br/><br/> <span data-ttu-id="20fd3-159">Linux/Etc/Hosts fájlt a virtuális gépek bejegyzéseit, amelyek hozzárendeléséről hello helyi állomás neve tooIP minden hálózati adapterekkel társított kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="20fd3-159">Linux /etc/hosts files on VMs should contain entries that map hello local host name tooIP addresses associated with all network adapters.</span></span> <span data-ttu-id="20fd3-160">állomásnév, csatlakoztatási pontokat, eszköz neve, elérési utak és fájlnevek hello (/ etc; / usr) kell lennie csak angol nyelven.</span><span class="sxs-lookup"><span data-stu-id="20fd3-160">hello host name, mount points, device name, system paths, and file names (/etc; /usr) should be in English only.</span></span><br/><br/> <span data-ttu-id="20fd3-161">Adott típusú [Linux tárolási](site-recovery-support-matrix-to-azure.md#support-for-storage) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="20fd3-161">Specific types of [Linux storage](site-recovery-support-matrix-to-azure.md#support-for-storage) are supported.</span></span><br/><br/><span data-ttu-id="20fd3-162">Hozzon létre, vagy állítsa be **disk.enableUUID=true** hello virtuális gép beállításai.</span><span class="sxs-lookup"><span data-stu-id="20fd3-162">Create or set **disk.enableUUID=true** in hello VM settings.</span></span> <span data-ttu-id="20fd3-163">Egységes UUID toohello VMDK, ez biztosítja, így a megfelelő csatlakoztatja, és biztosítja, hogy az csak a változási különbözeteket átvitt hátsó tooon helyszíni feladat-visszavétel, anélkül, hogy a teljes replikáció során.</span><span class="sxs-lookup"><span data-stu-id="20fd3-163">This provides a consistent UUID toohello VMDK, so that it mounts correctly, and ensures that only delta changes are transferred back tooon-premises during failback, without full replication.</span></span>


## <a name="next-steps"></a><span data-ttu-id="20fd3-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20fd3-164">Next steps</span></span>

- <span data-ttu-id="20fd3-165">Ha teljes körű telepítésére végzett, nyissa meg túl[3. lépés: kapacitástervezés](vmware-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="20fd3-165">If you're doing a full deployment, go too[Step 3: Plan capacity](vmware-walkthrough-capacity.md)</span></span>
- <span data-ttu-id="20fd3-166">Ha egy egyszerű próbatelepítés végzett, nyissa meg túl[4. lépés: hálózati terv](vmware-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="20fd3-166">If you're doing a simple test deployment, go too[Step 4: Plan networking](vmware-walkthrough-network.md).</span></span>