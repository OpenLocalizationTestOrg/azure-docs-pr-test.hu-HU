---
title: "Hello forrás környezetet (fizikai kiszolgálók tooAzure) |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart az Azure Windows vagy Linux operációs rendszert futtató fizikai kiszolgálók replikálása."
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
ms.openlocfilehash: d4702265bf36910015685d2bba99d6e577531bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-physical-server-tooazure"></a><span data-ttu-id="67898-103">Hello forrás környezetet (fizikai kiszolgáló tooAzure)</span><span class="sxs-lookup"><span data-stu-id="67898-103">Set up hello source environment (physical server tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="67898-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="67898-104">VMware tooAzure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="67898-105">Fizikai tooAzure</span><span class="sxs-lookup"><span data-stu-id="67898-105">Physical tooAzure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="67898-106">Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart az Azure Windows vagy Linux operációs rendszert futtató fizikai kiszolgálók replikálása.</span><span class="sxs-lookup"><span data-stu-id="67898-106">This article describes how tooset up your on-premises environment toostart replicating physical servers running Windows or Linux into Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67898-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="67898-107">Prerequisites</span></span>

<span data-ttu-id="67898-108">hello cikk feltételezi, hogy már rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="67898-108">hello article assumes that you already have:</span></span>
1. <span data-ttu-id="67898-109">A Recovery Services-tárolónak a hello [Azure-portálon](http://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="67898-109">A Recovery Services vault in hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
3. <span data-ttu-id="67898-110">A fizikai számítógép melyik tooinstall hello konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="67898-110">A physical computer on which tooinstall hello configuration server.</span></span>

### <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="67898-111">Konfigurációs kiszolgáló minimális követelményeknek</span><span class="sxs-lookup"><span data-stu-id="67898-111">Configuration server minimum requirements</span></span>
<span data-ttu-id="67898-112">hello a következő táblázat felsorolja a hello minimális hardver, szoftverek és hálózati követelményei a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="67898-112">hello following table lists hello minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="67898-113">Hello konfigurációs kiszolgáló által a HTTPS-alapú proxykiszolgálókat nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="67898-113">HTTPS-based proxy servers are not supported by hello configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="67898-114">Védelmi célok megválasztása</span><span class="sxs-lookup"><span data-stu-id="67898-114">Choose your protection goals</span></span>

1. <span data-ttu-id="67898-115">A hello Azure-portálon, válassza a toohello **Recovery Services** -tárolók panelt, és válassza ki a tároló.</span><span class="sxs-lookup"><span data-stu-id="67898-115">In hello Azure portal, go toohello **Recovery Services** vaults blade and select your vault.</span></span>
2. <span data-ttu-id="67898-116">A hello **erőforrás** hello tároló menüjében kattintson **bevezetés** > **Site Recovery** > **1. lépés: előkészítése Infrastruktúra** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="67898-116">In hello **Resource** menu of hello vault, click **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. <span data-ttu-id="67898-118">A **védelmi cél**, jelölje be **tooAzure** és **nem virtualizált vagy egyéb**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="67898-118">In **Protection goal**, select **tooAzure** and **Not virtualized/Other**, and then click **OK**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-hello-source-environment"></a><span data-ttu-id="67898-120">Hello forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="67898-120">Set up hello source environment</span></span>

1. <span data-ttu-id="67898-121">A **forrás előkészítése**, ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló** tooadd egyet.</span><span class="sxs-lookup"><span data-stu-id="67898-121">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** tooadd one.</span></span>

  ![A forrás beállítása](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. <span data-ttu-id="67898-123">A hello **kiszolgáló hozzáadása** panelen ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="67898-123">In hello **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="67898-124">Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="67898-124">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="67898-125">Hello tárolóbeli regisztrációs kulcs letöltése.</span><span class="sxs-lookup"><span data-stu-id="67898-125">Download hello vault registration key.</span></span> <span data-ttu-id="67898-126">Az egységes telepítő szüksége hello regisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="67898-126">You need hello registration key when you run Unified Setup.</span></span> <span data-ttu-id="67898-127">hello kulcs a generálásától öt napig esetén érvényes.</span><span class="sxs-lookup"><span data-stu-id="67898-127">hello key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. <span data-ttu-id="67898-129">Hello gépen hello kiszolgálóként használ, futtassa **Azure Site Recovery az egységes telepítő** tooinstall hello konfigurációs kiszolgáló, a hello folyamatkiszolgáló és a hello fő célkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="67898-129">On hello machine you’re using as hello configuration server, run **Azure Site Recovery Unified Setup** tooinstall hello configuration server, hello process server, and hello master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="67898-130">Futtassa az Azure Site Recovery egyesített telepítő</span><span class="sxs-lookup"><span data-stu-id="67898-130">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="67898-131">Konfigurációs kiszolgáló regisztrálása meghiúsul, ha a számítógép rendszerórája hello idő ki a helyi idő több mint öt perc.</span><span class="sxs-lookup"><span data-stu-id="67898-131">Configuration server registration fails if hello time on your computer's system clock is more than five minutes off of local time.</span></span> <span data-ttu-id="67898-132">A rendszer szinkronizálást, és egy [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) hello telepítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="67898-132">Synchronize your system clock with a [time server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting hello installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="67898-133">hello konfigurációs kiszolgálón a parancssorból is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="67898-133">hello configuration server can be installed via a command line.</span></span> <span data-ttu-id="67898-134">További információkért lásd: [telepítése konfigurációs kiszolgáló parancssori eszközökkel](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="67898-134">For more information, see [Installing configuration server using command-line tools](http://aka.ms/installconfigsrv).</span></span>


## <a name="common-issues"></a><span data-ttu-id="67898-135">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="67898-135">Common issues</span></span>

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="67898-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67898-136">Next steps</span></span>

<span data-ttu-id="67898-137">Magában foglalja a következő lépés [a célkörnyezet beállítása](./site-recovery-prepare-target-physical-to-azure.md) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="67898-137">Next step involves [setting up your target environment](./site-recovery-prepare-target-physical-to-azure.md) in Azure.</span></span>
