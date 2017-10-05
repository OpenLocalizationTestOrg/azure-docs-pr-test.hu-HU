---
title: "A forrás (fizikai kiszolgálók Azure-bA) környezet beállítása |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan állíthat be a helyszíni környezet az Azure Windows vagy Linux operációs rendszert futtató fizikai kiszolgálók replikálást indítani."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 49b9d2e21dbcb612828a25f21ed4382327d6f64c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a><span data-ttu-id="e92e0-103">Állítsa be a forrás-környezetet (az Azure-bA a fizikai kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="e92e0-103">Set up the source environment (physical server to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e92e0-104">VMware – Azure</span><span class="sxs-lookup"><span data-stu-id="e92e0-104">VMware to Azure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="e92e0-105">Fizikai az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="e92e0-105">Physical to Azure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="e92e0-106">A cikkből megtudhatja, hogyan állíthat be a helyszíni környezet az Azure Windows vagy Linux operációs rendszert futtató fizikai kiszolgálók replikálást indítani.</span><span class="sxs-lookup"><span data-stu-id="e92e0-106">This article describes how to set up your on-premises environment to start replicating physical servers running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e92e0-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e92e0-107">Prerequisites</span></span>

<span data-ttu-id="e92e0-108">A cikk feltételezi, hogy már rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e92e0-108">The article assumes that you already have:</span></span>
1. <span data-ttu-id="e92e0-109">A tároló a Recovery Services a [Azure-portálon](http://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="e92e0-109">A Recovery Services vault in the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
3. <span data-ttu-id="e92e0-110">Egy fizikai számítógépen, amelyre a konfigurációs kiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="e92e0-110">A physical computer on which to install the configuration server.</span></span>

### <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="e92e0-111">Konfigurációs kiszolgáló minimális követelményeknek</span><span class="sxs-lookup"><span data-stu-id="e92e0-111">Configuration server minimum requirements</span></span>
<span data-ttu-id="e92e0-112">A következő táblázat felsorolja a minimális hardver-, szoftver, és hálózati követelményei a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e92e0-112">The following table lists the minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="e92e0-113">A konfigurációs kiszolgáló által a HTTPS-alapú proxykiszolgálókat nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="e92e0-113">HTTPS-based proxy servers are not supported by the configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="e92e0-114">Védelmi célok megválasztása</span><span class="sxs-lookup"><span data-stu-id="e92e0-114">Choose your protection goals</span></span>

1. <span data-ttu-id="e92e0-115">Az Azure-portálon lépjen a **Recovery Services** -tárolók panelt, és válassza ki a tároló.</span><span class="sxs-lookup"><span data-stu-id="e92e0-115">In the Azure portal, go to the **Recovery Services** vaults blade and select your vault.</span></span>
2. <span data-ttu-id="e92e0-116">Az a **erőforrás** a tároló menüjében kattintson **bevezetés** > **Site Recovery** > **1. lépés: infrastruktúra előkészítése**   >  **Védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="e92e0-116">In the **Resource** menu of the vault, click **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. <span data-ttu-id="e92e0-118">A **védelmi cél**, jelölje be **az Azure-bA** és **nem virtualizált vagy egyéb**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e92e0-118">In **Protection goal**, select **To Azure** and **Not virtualized/Other**, and then click **OK**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-the-source-environment"></a><span data-ttu-id="e92e0-120">A forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="e92e0-120">Set up the source environment</span></span>

1. <span data-ttu-id="e92e0-121">A **forrás előkészítése**, ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló** kattintva felvehet egyet.</span><span class="sxs-lookup"><span data-stu-id="e92e0-121">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** to add one.</span></span>

  ![A forrás beállítása](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. <span data-ttu-id="e92e0-123">Az a **kiszolgáló hozzáadása** panelen ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="e92e0-123">In the **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="e92e0-124">Töltse le a Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="e92e0-124">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="e92e0-125">Töltse le a tárolóregisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e92e0-125">Download the vault registration key.</span></span> <span data-ttu-id="e92e0-126">Az egységes telepítő futtatásakor a regisztrációs kulcsot kell.</span><span class="sxs-lookup"><span data-stu-id="e92e0-126">You need the registration key when you run Unified Setup.</span></span> <span data-ttu-id="e92e0-127">A kulcs a generálásától számított öt napig érvényes.</span><span class="sxs-lookup"><span data-stu-id="e92e0-127">The key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. <span data-ttu-id="e92e0-129">A számítógépen, mint a konfigurációs kiszolgálót használ, futtassa **Azure Site Recovery az egységes telepítő** a konfigurációs kiszolgáló, a folyamatkiszolgáló és a fő célkiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="e92e0-129">On the machine you’re using as the configuration server, run **Azure Site Recovery Unified Setup** to install the configuration server, the process server, and the master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="e92e0-130">Futtassa az Azure Site Recovery egyesített telepítő</span><span class="sxs-lookup"><span data-stu-id="e92e0-130">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="e92e0-131">Konfigurációs kiszolgáló regisztrálása sikertelen lesz, ha a számítógép rendszer órája a ideje ki a helyi idő több mint öt perc.</span><span class="sxs-lookup"><span data-stu-id="e92e0-131">Configuration server registration fails if the time on your computer's system clock is more than five minutes off of local time.</span></span> <span data-ttu-id="e92e0-132">A rendszer szinkronizálást, és egy [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) a telepítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="e92e0-132">Synchronize your system clock with a [time server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting the installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="e92e0-133">A konfigurációs kiszolgáló telepíthető a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="e92e0-133">The configuration server can be installed via a command line.</span></span> <span data-ttu-id="e92e0-134">További információkért lásd: [telepítése konfigurációs kiszolgáló parancssori eszközökkel](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="e92e0-134">For more information, see [Installing configuration server using command-line tools](http://aka.ms/installconfigsrv).</span></span>


## <a name="common-issues"></a><span data-ttu-id="e92e0-135">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="e92e0-135">Common issues</span></span>

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="e92e0-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e92e0-136">Next steps</span></span>

<span data-ttu-id="e92e0-137">Magában foglalja a következő lépés [a célkörnyezet beállítása](./site-recovery-prepare-target-physical-to-azure.md) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e92e0-137">Next step involves [setting up your target environment](./site-recovery-prepare-target-physical-to-azure.md) in Azure.</span></span>
