---
title: "hello forrás és cél VMware replikációs tooAzure az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Hello lépéseket tooset forrása és célja beállítások megadása a VMware virtuális gépek tooAzure tárolás az Azure Site Recovery replikációs összesíti"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ef33a44bc5da17afb0442be63f576925f5b9a8b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-vmware-replication-tooazure"></a><span data-ttu-id="2a11c-103">8. lépés: Hello forrása és célja a VMware-replikáció tooAzure beállítása</span><span class="sxs-lookup"><span data-stu-id="2a11c-103">Step 8: Set up hello source and target for VMware replication tooAzure</span></span>

<span data-ttu-id="2a11c-104">Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni VMware virtuális gépek tooAzure, használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2a11c-104">This article describes how tooconfigure source and target settings when replicating on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="2a11c-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="2a11c-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="2a11c-106">Hello forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="2a11c-106">Set up hello source environment</span></span>

<span data-ttu-id="2a11c-107">Hello konfigurációs kiszolgáló, regisztrálja azt hello tárolóban, és virtuális gépek felderítése.</span><span class="sxs-lookup"><span data-stu-id="2a11c-107">Set up hello configuration server, register it in hello vault, and discover VMs.</span></span>

1. <span data-ttu-id="2a11c-108">Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="2a11c-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="2a11c-109">Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="2a11c-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="2a11c-110">A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="2a11c-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="2a11c-111">Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="2a11c-111">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="2a11c-112">Hello tárolóbeli regisztrációs kulcs letöltése.</span><span class="sxs-lookup"><span data-stu-id="2a11c-112">Download hello vault registration key.</span></span> <span data-ttu-id="2a11c-113">Ez szükséges az egységes telepítő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="2a11c-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="2a11c-114">hello kulcs a generálásától öt napig esetén érvényes.</span><span class="sxs-lookup"><span data-stu-id="2a11c-114">hello key is valid for five days after you generate it.</span></span>

   ![A forrás beállítása](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a><span data-ttu-id="2a11c-116">Hello tároló hello konfigurációs kiszolgáló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="2a11c-116">Register hello configuration server in hello vault</span></span>

<span data-ttu-id="2a11c-117">Tegye hello következő előtt indítsa el, majd futtassa az egységes telepítő tooinstall hello konfigurációs kiszolgáló, hello folyamatkiszolgáló és fő célkiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="2a11c-117">Do hello following before you start, then run Unified Setup tooinstall hello configuration server, hello process server, and hello master target server.</span></span>
    - <span data-ttu-id="2a11c-118">Gyors áttekintő videó beolvasása</span><span class="sxs-lookup"><span data-stu-id="2a11c-118">Get a quick video overview</span></span>

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - <span data-ttu-id="2a11c-119">Hello konfigurációs kiszolgálón VM, győződjön meg arról, hogy hello rendszeróra szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="2a11c-119">On hello configuration server VM, make sure that hello system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="2a11c-120">Meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="2a11c-120">It should match.</span></span> <span data-ttu-id="2a11c-121">Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.</span><span class="sxs-lookup"><span data-stu-id="2a11c-121">If it's 15 minutes in front or behind, setup might fail.</span></span>
    - <span data-ttu-id="2a11c-122">Futtassa a telepítőt a helyi rendszergazdájaként hello virtuális gép konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2a11c-122">Run setup as a Local Administrator on hello configuration server VM.</span></span>
    - <span data-ttu-id="2a11c-123">Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="2a11c-123">Make sure TLS 1.0 is enabled on hello VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="2a11c-124">hello konfigurációs kiszolgálón is telepíthető [hello parancssorból](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="2a11c-124">hello configuration server can also be installed [from hello command line](http://aka.ms/installconfigsrv).</span></span>



## <a name="connect-toovmware-servers"></a><span data-ttu-id="2a11c-125">Csatlakozás tooVMware kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="2a11c-125">Connect tooVMware servers</span></span>

<span data-ttu-id="2a11c-126">tooallow Azure Site Recovery toodiscover futó virtuális gépek a helyszíni környezetben, kell tooconnect a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="2a11c-126">tooallow Azure Site Recovery toodiscover virtual machines running in your on-premises environment, you need tooconnect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span> <span data-ttu-id="2a11c-127">Vegye figyelembe a következő hello megkezdése előtt:</span><span class="sxs-lookup"><span data-stu-id="2a11c-127">Note hello following before you start:</span></span>

- <span data-ttu-id="2a11c-128">Ha hello vCenter-kiszolgáló vagy vSphere állomások tooSite helyreállítási rendszergazdai jogosultságok nélküli fiókkal hello kiszolgálón, a hello fiókot kell ezeket a jogokat engedélyezve:</span><span class="sxs-lookup"><span data-stu-id="2a11c-128">If you add hello vCenter server or vSphere hosts tooSite Recovery with an account without administrator privileges on hello server, hello account needs these privileges enabled:</span></span>
    - <span data-ttu-id="2a11c-129">Datacenter, Datastore, mappa, állomás, hálózati, erőforrás, a virtuális gép, vSphere elosztott kapcsoló.</span><span class="sxs-lookup"><span data-stu-id="2a11c-129">Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, vSphere Distributed Switch.</span></span>
    - <span data-ttu-id="2a11c-130">hello vCenter-kiszolgálót kell tárolási nézetek engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="2a11c-130">hello vCenter server needs Storage views permissions.</span></span>
- <span data-ttu-id="2a11c-131">VMware-kiszolgálók tooSite helyreállítási hozzáadásakor 15 percig is tarthat, vagy hosszabb ideig őket tooappear hello portálon.</span><span class="sxs-lookup"><span data-stu-id="2a11c-131">When you add VMware servers tooSite Recovery, it can take 15 minutes or longer for them tooappear in hello portal.</span></span>

### <a name="add-hello-account-for-automatic-discovery"></a><span data-ttu-id="2a11c-132">Automatikus felderítés hello fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a11c-132">Add hello account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a><span data-ttu-id="2a11c-133">Kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="2a11c-133">Set up a connection</span></span>

<span data-ttu-id="2a11c-134">Csatlakozás a tooservers az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2a11c-134">Connect tooservers as follows:</span></span>

1. <span data-ttu-id="2a11c-135">Válassza ki **+ vCenter** toostart VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.</span><span class="sxs-lookup"><span data-stu-id="2a11c-135">Select **+vCenter** toostart connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>
2. <span data-ttu-id="2a11c-136">A **vCenter hozzáadása**, adjon meg egy rövid nevet hello vSphere gazdagép vagy a vCenter-kiszolgálóhoz, és adja meg a hello IP-cím vagy hello kiszolgáló teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="2a11c-136">In **Add vCenter**, specify a friendly name for hello vSphere host or vCenter server, and then specify hello IP address or FQDN of hello server.</span></span>
3. <span data-ttu-id="2a11c-137">Hello port hagyja 443-as, kivéve, ha a VMware Server kérések egy másik portra beállított toolisten.</span><span class="sxs-lookup"><span data-stu-id="2a11c-137">Leave hello port as 443 unless your VMware servers are configured toolisten for requests on a different port.</span></span> <span data-ttu-id="2a11c-138">Válassza ki a hello fiók, amely tooconnect toohello VMware vCenter vagy vSphere ESXi-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2a11c-138">Select hello account that is tooconnect toohello VMware vCenter or vSphere ESXi server.</span></span> <span data-ttu-id="2a11c-139">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2a11c-139">Click **OK**.</span></span>
4. <span data-ttu-id="2a11c-140">A Site Recovery csatlakoztatja tooVMware kiszolgálókat hello segítségével megadott beállításokat, és felderíti a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2a11c-140">Site Recovery connects tooVMware servers using hello specified settings, and discovers VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="2a11c-141">Egy kiszolgáló vagy egy olyan fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal hello vCenter vagy a gazdagép-kiszolgálói gazdagép hozzáadása, győződjön meg arról, hogy hello fiókja rendelkezik-e ezek a jogosultságok engedélyezve: Datacenter, Datastore, mappa, Host, hálózati, erőforrás, a virtuális gép, és a vSphere elosztott kapcsoló.</span><span class="sxs-lookup"><span data-stu-id="2a11c-141">If you're adding a server or host with an account that doesn't have administrator privileges on hello vCenter or host server, make sure that hello account has these privileges enabled: Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, and vSphere Distributed Switch.</span></span> <span data-ttu-id="2a11c-142">Emellett hello VMware vCenter kiszolgálót kell hello tárolási nézetek jogosultság engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="2a11c-142">In addition, hello VMware vCenter server needs hello Storage Views privilege enabled.</span></span>


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="2a11c-143">Hello célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="2a11c-143">Set up hello target environment</span></span>

<span data-ttu-id="2a11c-144">Hello célkörnyezet beállítása előtt győződjön meg arról, hogy az Azure storage-fiók és a virtuális hálózat beállítása.</span><span class="sxs-lookup"><span data-stu-id="2a11c-144">Before you set up hello target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="2a11c-145">Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a kívánt toouse Azure-előfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="2a11c-145">Click **Prepare infrastructure** > **Target**, and select hello Azure subscription you want toouse.</span></span>
2. <span data-ttu-id="2a11c-146">Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.</span><span class="sxs-lookup"><span data-stu-id="2a11c-146">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="2a11c-147">A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.</span><span class="sxs-lookup"><span data-stu-id="2a11c-147">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cél](./media/vmware-walkthrough-source-target/gs-target.png)
4. <span data-ttu-id="2a11c-149">Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, egy erőforrás-kezelő fiók vagy a hálózati beágyazott toocreate.</span><span class="sxs-lookup"><span data-stu-id="2a11c-149">If you haven't created a storage account or network, click **+Storage account** or **+Network**, toocreate a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a11c-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a11c-150">Next steps</span></span>

<span data-ttu-id="2a11c-151">Nyissa meg túl[9. lépés: a replikációs házirend beállítása](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="2a11c-151">Go too[Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>
