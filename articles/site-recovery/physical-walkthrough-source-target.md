---
title: "A forrás és cél a fizikai kiszolgáló replikáció az Azure szolgáltatásban az Azure Site Recovery beállítása |} Microsoft Docs"
description: "A forrás és cél beállítások megadása az Azure Site Recovery szolgáltatásban az Azure storage fizikai kiszolgálók replikálása ismertetett lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e89bbf5a2c1d71852e49da43d3106a05ebfc28a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-the-source-and-target-for-physical-server-replication-to-azure"></a><span data-ttu-id="e4e2e-103">7. lépés: A forrás- és a cél a fizikai kiszolgáló replikáció az Azure beállítása</span><span class="sxs-lookup"><span data-stu-id="e4e2e-103">Step 7: Set up the source and target for physical server replication to Azure</span></span>

<span data-ttu-id="e4e2e-104">A cikk ismerteti a forrás és cél beállítások konfigurálása, ha a helyszíni fizikai kiszolgálók replikálása Azure-ba, használja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-104">This article describes how to configure source and target settings when replicating on-premises physical servers to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="e4e2e-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e4e2e-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="e4e2e-106">A forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="e4e2e-106">Set up the source environment</span></span>

<span data-ttu-id="e4e2e-107">Állítsa be a konfigurációs kiszolgáló, regisztrálja őket a tárolóban, és a gépek észlelése.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-107">Set up the configuration server, register it in the vault, and discover machines.</span></span>

1. <span data-ttu-id="e4e2e-108">Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="e4e2e-109">Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="e4e2e-110">A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="e4e2e-111">Töltse le a Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-111">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="e4e2e-112">Töltse le a tárolóregisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-112">Download the vault registration key.</span></span> <span data-ttu-id="e4e2e-113">Ez szükséges az egységes telepítő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="e4e2e-114">A kulcs a generálásától számított öt napig érvényes.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-114">The key is valid for five days after you generate it.</span></span>

   ![A forrás beállítása](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a><span data-ttu-id="e4e2e-116">Regisztrálja a konfigurációs kiszolgálót a tárolóban</span><span class="sxs-lookup"><span data-stu-id="e4e2e-116">Register the configuration server in the vault</span></span>

<span data-ttu-id="e4e2e-117">Tegye a következőket előtt indítsa el, majd az egységes telepítőjének futtatásával telepítse a konfigurációs kiszolgáló, a folyamatkiszolgáló és a fő célkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-117">Do the following before you start, then run Unified Setup to install the configuration server, the process server, and the master target server.</span></span> <span data-ttu-id="e4e2e-118">A videó VMware virtuális gép Azure replikációs összetevők beállításának a módját ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-118">The video describes setting up the components for VMware VM to Azure replication.</span></span> <span data-ttu-id="e4e2e-119">Azonban ugyanazt a folyamatot nem érvényes Azure-replikációs szolgáltatására való fizikai kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-119">However, the same process is valid for physical server to Azure replication.</span></span>

- <span data-ttu-id="e4e2e-120">A konfigurációs kiszolgálón VM, győződjön meg arról, hogy a rendszer órája szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="e4e2e-120">On the configuration server VM, make sure that the system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="e4e2e-121">Meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-121">It should match.</span></span> <span data-ttu-id="e4e2e-122">Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-122">If it's 15 minutes in front or behind, setup might fail.</span></span>
- <span data-ttu-id="e4e2e-123">Futtassa a telepítőt a helyi rendszergazda, a konfigurációs kiszolgáló gépen.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-123">Run setup as a Local Administrator on the configuration server machine.</span></span>
- <span data-ttu-id="e4e2e-124">Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-124">Make sure TLS 1.0 is enabled on the VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="e4e2e-125">A konfigurációs kiszolgáló is telepíthető [a parancssorból](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="e4e2e-125">The configuration server can also be installed [from the command line](http://aka.ms/installconfigsrv).</span></span>




## <a name="set-up-the-target-environment"></a><span data-ttu-id="e4e2e-126">A célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="e4e2e-126">Set up the target environment</span></span>

<span data-ttu-id="e4e2e-127">A célkörnyezet beállítása előtt győződjön meg arról, hogy az Azure storage-fiók és a virtuális hálózat beállítása.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-127">Before you set up the target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="e4e2e-128">Kattintson az **Infrastruktúra előkészítése** > **Cél** elemre, majd válassza ki a használni kívánt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-128">Click **Prepare infrastructure** > **Target**, and select the Azure subscription you want to use.</span></span>
2. <span data-ttu-id="e4e2e-129">Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-129">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="e4e2e-130">A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-130">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cél](./media/physical-walkthrough-source-target/gs-target.png)

4. <span data-ttu-id="e4e2e-132">Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, az erőforrás-kezelő fiók létrehozása vagy szövegközi hálózati.</span><span class="sxs-lookup"><span data-stu-id="e4e2e-132">If you haven't created a storage account or network, click **+Storage account** or **+Network**, to create a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4e2e-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4e2e-133">Next steps</span></span>

<span data-ttu-id="e4e2e-134">Ugrás a [8. lépés: a replikációs házirend beállítása](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="e4e2e-134">Go to [Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>
