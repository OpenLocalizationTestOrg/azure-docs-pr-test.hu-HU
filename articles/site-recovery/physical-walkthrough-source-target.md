---
title: "hello forrás és cél a fizikai kiszolgáló replikációs tooAzure az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket tooset hello Azure Site Recovery szolgáltatás a fizikai kiszolgálók tooAzure tárhely replikálás forrás- és beállításainak mentése"
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
ms.openlocfilehash: e75ef23174d77509bf54380eba8f251a7337a45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-hello-source-and-target-for-physical-server-replication-tooazure"></a><span data-ttu-id="bdaf4-103">7. lépés: Hello forrása és célja a fizikai kiszolgáló replikációs tooAzure beállítása</span><span class="sxs-lookup"><span data-stu-id="bdaf4-103">Step 7: Set up hello source and target for physical server replication tooAzure</span></span>

<span data-ttu-id="bdaf4-104">Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni fizikai kiszolgálók tooAzure használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-104">This article describes how tooconfigure source and target settings when replicating on-premises physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="bdaf4-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="bdaf4-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="bdaf4-106">Hello forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="bdaf4-106">Set up hello source environment</span></span>

<span data-ttu-id="bdaf4-107">Hello konfigurációs kiszolgáló, és regisztrálja azt hello tárolóban gépek észlelése.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-107">Set up hello configuration server, register it in hello vault, and discover machines.</span></span>

1. <span data-ttu-id="bdaf4-108">Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="bdaf4-109">Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="bdaf4-110">A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="bdaf4-111">Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-111">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="bdaf4-112">Hello tárolóbeli regisztrációs kulcs letöltése.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-112">Download hello vault registration key.</span></span> <span data-ttu-id="bdaf4-113">Ez szükséges az egységes telepítő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="bdaf4-114">hello kulcs a generálásától öt napig esetén érvényes.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-114">hello key is valid for five days after you generate it.</span></span>

   ![A forrás beállítása](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a><span data-ttu-id="bdaf4-116">Hello tároló hello konfigurációs kiszolgáló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="bdaf4-116">Register hello configuration server in hello vault</span></span>

<span data-ttu-id="bdaf4-117">Tegye hello következő előtt indítsa el, majd futtassa az egységes telepítő tooinstall hello konfigurációs kiszolgáló, hello folyamatkiszolgáló és fő célkiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-117">Do hello following before you start, then run Unified Setup tooinstall hello configuration server, hello process server, and hello master target server.</span></span> <span data-ttu-id="bdaf4-118">videó hello VMware virtuális gép tooAzure replikációs hello összetevőket beállításának a módját ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-118">hello video describes setting up hello components for VMware VM tooAzure replication.</span></span> <span data-ttu-id="bdaf4-119">Azonban hello ugyanazt a folyamatot nem érvényes a fizikai kiszolgáló tooAzure replikációs.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-119">However, hello same process is valid for physical server tooAzure replication.</span></span>

- <span data-ttu-id="bdaf4-120">Hello konfigurációs kiszolgálón VM, győződjön meg arról, hogy hello rendszeróra szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="bdaf4-120">On hello configuration server VM, make sure that hello system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="bdaf4-121">Meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-121">It should match.</span></span> <span data-ttu-id="bdaf4-122">Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-122">If it's 15 minutes in front or behind, setup might fail.</span></span>
- <span data-ttu-id="bdaf4-123">Futtassa a telepítőt a helyi rendszergazda, a hello konfigurációs kiszolgáló számítógépén.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-123">Run setup as a Local Administrator on hello configuration server machine.</span></span>
- <span data-ttu-id="bdaf4-124">Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-124">Make sure TLS 1.0 is enabled on hello VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="bdaf4-125">hello konfigurációs kiszolgálón is telepíthető [hello parancssorból](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="bdaf4-125">hello configuration server can also be installed [from hello command line](http://aka.ms/installconfigsrv).</span></span>




## <a name="set-up-hello-target-environment"></a><span data-ttu-id="bdaf4-126">Hello célkörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="bdaf4-126">Set up hello target environment</span></span>

<span data-ttu-id="bdaf4-127">Hello célkörnyezet beállítása előtt győződjön meg arról, hogy az Azure storage-fiók és a virtuális hálózat beállítása.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-127">Before you set up hello target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="bdaf4-128">Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a kívánt toouse Azure-előfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-128">Click **Prepare infrastructure** > **Target**, and select hello Azure subscription you want toouse.</span></span>
2. <span data-ttu-id="bdaf4-129">Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-129">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="bdaf4-130">A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-130">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cél](./media/physical-walkthrough-source-target/gs-target.png)

4. <span data-ttu-id="bdaf4-132">Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, egy erőforrás-kezelő fiók vagy a hálózati beágyazott toocreate.</span><span class="sxs-lookup"><span data-stu-id="bdaf4-132">If you haven't created a storage account or network, click **+Storage account** or **+Network**, toocreate a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bdaf4-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdaf4-133">Next steps</span></span>

<span data-ttu-id="bdaf4-134">Nyissa meg túl[8. lépés: a replikációs házirend beállítása](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="bdaf4-134">Go too[Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>
