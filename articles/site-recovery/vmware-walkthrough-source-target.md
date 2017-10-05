---
title: "A forrás és cél a VMware-replikáció az Azure szolgáltatásban az Azure Site Recovery beállítása |} Microsoft Docs"
description: "A forrás és cél beállítások megadása a VMware virtuális gépek replikálása az Azure storage az Azure Site Recovery ismertetett lépéseket foglalja"
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
ms.openlocfilehash: 94b629a62c3a54eee69ee397b2f27e3f20b753d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-vmware-replication-to-azure"></a><span data-ttu-id="d141a-103">8. lépés: A forrás- és a cél a VMware-replikáció az Azure beállítása</span><span class="sxs-lookup"><span data-stu-id="d141a-103">Step 8: Set up the source and target for VMware replication to Azure</span></span>

<span data-ttu-id="d141a-104">A cikk ismerteti a forrás és cél beállítások konfigurálása, ha a helyszíni VMware virtuális gépek replikálása Azure-ba, használja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="d141a-104">This article describes how to configure source and target settings when replicating on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="d141a-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="d141a-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="d141a-106">A forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="d141a-106">Set up the source environment</span></span>

<span data-ttu-id="d141a-107">Állítsa be a konfigurációs kiszolgáló, és regisztrálja őket a tárolóban lévő virtuális gépek felderítése.</span><span class="sxs-lookup"><span data-stu-id="d141a-107">Set up the configuration server, register it in the vault, and discover VMs.</span></span>

1. <span data-ttu-id="d141a-108">Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="d141a-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="d141a-109">Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="d141a-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="d141a-110">A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="d141a-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="d141a-111">Töltse le a Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="d141a-111">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="d141a-112">Töltse le a tárolóregisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d141a-112">Download the vault registration key.</span></span> <span data-ttu-id="d141a-113">Ez szükséges az egységes telepítő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="d141a-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="d141a-114">A kulcs a generálásától számított öt napig érvényes.</span><span class="sxs-lookup"><span data-stu-id="d141a-114">The key is valid for five days after you generate it.</span></span>

   ![A forrás beállítása](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a><span data-ttu-id="d141a-116">Regisztrálja a konfigurációs kiszolgálót a tárolóban</span><span class="sxs-lookup"><span data-stu-id="d141a-116">Register the configuration server in the vault</span></span>

<span data-ttu-id="d141a-117">Tegye a következőket előtt indítsa el, majd az egységes telepítőjének futtatásával telepítse a konfigurációs kiszolgáló, a folyamatkiszolgáló és a fő célkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d141a-117">Do the following before you start, then run Unified Setup to install the configuration server, the process server, and the master target server.</span></span>
    - <span data-ttu-id="d141a-118">Gyors áttekintő videó beolvasása</span><span class="sxs-lookup"><span data-stu-id="d141a-118">Get a quick video overview</span></span>

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - <span data-ttu-id="d141a-119">A konfigurációs kiszolgálón VM, győződjön meg arról, hogy a rendszer órája szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="d141a-119">On the configuration server VM, make sure that the system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="d141a-120">Meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="d141a-120">It should match.</span></span> <span data-ttu-id="d141a-121">Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.</span><span class="sxs-lookup"><span data-stu-id="d141a-121">If it's 15 minutes in front or behind, setup might fail.</span></span>
    - <span data-ttu-id="d141a-122">Futtassa a telepítőt a helyi rendszergazda, a virtuális gép konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d141a-122">Run setup as a Local Administrator on the configuration server VM.</span></span>
    - <span data-ttu-id="d141a-123">Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="d141a-123">Make sure TLS 1.0 is enabled on the VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="d141a-124">A konfigurációs kiszolgáló is telepíthető [a parancssorból](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="d141a-124">The configuration server can also be installed [from the command line](http://aka.ms/installconfigsrv).</span></span>



## <a name="connect-to-vmware-servers"></a><span data-ttu-id="d141a-125">VMware-kiszolgálókkal</span><span class="sxs-lookup"><span data-stu-id="d141a-125">Connect to VMware servers</span></span>

<span data-ttu-id="d141a-126">Ahhoz, hogy az Azure Site Recovery számára a helyszíni környezetben futó virtuális gépek felderítése, meg kell kapcsolni a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek a Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="d141a-126">To allow Azure Site Recovery to discover virtual machines running in your on-premises environment, you need to connect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span> <span data-ttu-id="d141a-127">Mielőtt elkezdené, vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="d141a-127">Note the following before you start:</span></span>

- <span data-ttu-id="d141a-128">Ha a vCenter-kiszolgáló vagy vSphere-gazdagép Site Recovery rendszergazdai jogosultságok nélküli fiókkal ad hozzá a kiszolgálón, a fiók ezeket a jogokat engedélyezett van szüksége:</span><span class="sxs-lookup"><span data-stu-id="d141a-128">If you add the vCenter server or vSphere hosts to Site Recovery with an account without administrator privileges on the server, the account needs these privileges enabled:</span></span>
    - <span data-ttu-id="d141a-129">Datacenter, Datastore, mappa, állomás, hálózati, erőforrás, a virtuális gép, vSphere elosztott kapcsoló.</span><span class="sxs-lookup"><span data-stu-id="d141a-129">Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, vSphere Distributed Switch.</span></span>
    - <span data-ttu-id="d141a-130">A vCenter-kiszolgálót kell tárolási nézetek engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="d141a-130">The vCenter server needs Storage views permissions.</span></span>
- <span data-ttu-id="d141a-131">Amikor VMware-kiszolgálók ad hozzá a Site Recovery, 15 percbe is telhet, vagy hosszabb, hogy megjelenjenek a portálon.</span><span class="sxs-lookup"><span data-stu-id="d141a-131">When you add VMware servers to Site Recovery, it can take 15 minutes or longer for them to appear in the portal.</span></span>

### <a name="add-the-account-for-automatic-discovery"></a><span data-ttu-id="d141a-132">Adja hozzá a fiókot a automatikus felderítése</span><span class="sxs-lookup"><span data-stu-id="d141a-132">Add the account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a><span data-ttu-id="d141a-133">Kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="d141a-133">Set up a connection</span></span>

<span data-ttu-id="d141a-134">Kapcsolódás a kiszolgálók az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d141a-134">Connect to servers as follows:</span></span>

1. <span data-ttu-id="d141a-135">Válassza ki **+ vCenter** elindítani a VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.</span><span class="sxs-lookup"><span data-stu-id="d141a-135">Select **+vCenter** to start connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>
2. <span data-ttu-id="d141a-136">A **vCenter hozzáadása** területen adjon meg egy rövid nevet a vSphere-gazdagép vagy a vCenter-kiszolgáló számára, majd adja meg a kiszolgáló IP-címét vagy teljes tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="d141a-136">In **Add vCenter**, specify a friendly name for the vSphere host or vCenter server, and then specify the IP address or FQDN of the server.</span></span>
3. <span data-ttu-id="d141a-137">A 443-as portot csak akkor módosítsa, ha a VMware-kiszolgálók úgy vannak konfigurálva, hogy más porton figyeljék a kéréseket.</span><span class="sxs-lookup"><span data-stu-id="d141a-137">Leave the port as 443 unless your VMware servers are configured to listen for requests on a different port.</span></span> <span data-ttu-id="d141a-138">Válassza ki a VMware vCenter- vagy vSphere ESXi-kiszolgálóhoz csatlakoztatni kívánt fiókot.</span><span class="sxs-lookup"><span data-stu-id="d141a-138">Select the account that is to connect to the VMware vCenter or vSphere ESXi server.</span></span> <span data-ttu-id="d141a-139">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d141a-139">Click **OK**.</span></span>
4. <span data-ttu-id="d141a-140">A Site Recovery VMware-kiszolgálók, a megadott beállítások csatlakozik, és felderíti a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="d141a-140">Site Recovery connects to VMware servers using the specified settings, and discovers VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="d141a-141">Ha a kiszolgáló vagy a gazdagép egy olyan fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal a vCenter vagy a gazdagép-kiszolgálón, győződjön meg arról, hogy a fiók rendelkezik-e ezek a jogosultságok engedélyezve: Datacenter, Datastore, mappa, Host, hálózati, erőforrás, a virtuális gép, és a vSphere elosztott kapcsoló.</span><span class="sxs-lookup"><span data-stu-id="d141a-141">If you're adding a server or host with an account that doesn't have administrator privileges on the vCenter or host server, make sure that the account has these privileges enabled: Datacenter, Datastore, Folder, Host, Network, Resource, Virtual machine, and vSphere Distributed Switch.</span></span> <span data-ttu-id="d141a-142">Emellett a VMware vCenter-kiszolgálót kell a tárolási nézetek jogosultság engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d141a-142">In addition, the VMware vCenter server needs the Storage Views privilege enabled.</span></span>


## <a name="set-up-the-target-environment"></a><span data-ttu-id="d141a-143">A célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="d141a-143">Set up the target environment</span></span>

<span data-ttu-id="d141a-144">A célkörnyezet beállítása előtt győződjön meg arról, hogy az Azure storage-fiók és a virtuális hálózat beállítása.</span><span class="sxs-lookup"><span data-stu-id="d141a-144">Before you set up the target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="d141a-145">Kattintson az **Infrastruktúra előkészítése** > **Cél** elemre, majd válassza ki a használni kívánt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="d141a-145">Click **Prepare infrastructure** > **Target**, and select the Azure subscription you want to use.</span></span>
2. <span data-ttu-id="d141a-146">Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.</span><span class="sxs-lookup"><span data-stu-id="d141a-146">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="d141a-147">A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.</span><span class="sxs-lookup"><span data-stu-id="d141a-147">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cél](./media/vmware-walkthrough-source-target/gs-target.png)
4. <span data-ttu-id="d141a-149">Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, az erőforrás-kezelő fiók létrehozása vagy szövegközi hálózati.</span><span class="sxs-lookup"><span data-stu-id="d141a-149">If you haven't created a storage account or network, click **+Storage account** or **+Network**, to create a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d141a-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d141a-150">Next steps</span></span>

<span data-ttu-id="d141a-151">Ugrás a [9. lépés: a replikációs házirend beállítása](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="d141a-151">Go to [Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>
