---
title: "Hello forrás környezetet (VMware tooAzure) |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart replikálása VMware virtuális gépek tooAzure."
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
ms.openlocfilehash: cbeeffc1061b11223e12ff3e237fa7e55b999aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-vmware-tooazure"></a><span data-ttu-id="d23ce-103">Hello forrás környezetet (VMware tooAzure)</span><span class="sxs-lookup"><span data-stu-id="d23ce-103">Set up hello source environment (VMware tooAzure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d23ce-104">VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="d23ce-104">VMware tooAzure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="d23ce-105">Fizikai tooAzure</span><span class="sxs-lookup"><span data-stu-id="d23ce-105">Physical tooAzure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="d23ce-106">Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart replikáló virtuális gépek futnak a VMware tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d23ce-106">This article describes how tooset up your on-premises environment toostart replicating virtual machines running on VMware tooAzure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d23ce-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d23ce-107">Prerequisites</span></span>

<span data-ttu-id="d23ce-108">hello cikk feltételezi, hogy már létrehozta:</span><span class="sxs-lookup"><span data-stu-id="d23ce-108">hello article assumes that you have already created:</span></span>
- <span data-ttu-id="d23ce-109">A Recovery Services-tároló a hello [Azure-portálon](http://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="d23ce-109">A Recovery Services Vault in hello [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="d23ce-110">A VMware vcenter programban, amely nem használható egy külön fiókot [automatikus felderítés](./site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="d23ce-110">A dedicated account in your VMware vCenter that can be used for [automatic discovery](./site-recovery-vmware-to-azure.md).</span></span>
- <span data-ttu-id="d23ce-111">A virtuális gép mely tooinstall hello konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d23ce-111">A virtual machine on which tooinstall hello configuration server.</span></span>

## <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="d23ce-112">Konfigurációs kiszolgáló minimális követelményeknek</span><span class="sxs-lookup"><span data-stu-id="d23ce-112">Configuration server minimum requirements</span></span>
<span data-ttu-id="d23ce-113">hello konfigurációs server szoftverre a következő VMware magas rendelkezésre állású virtuális gépen kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="d23ce-113">hello configuration server software should be deployed on a highly available VMware virtual machine.</span></span> <span data-ttu-id="d23ce-114">hello a következő táblázat felsorolja a hello minimális hardver, szoftverek és hálózati követelményei a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d23ce-114">hello following table lists hello minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="d23ce-115">Hello konfigurációs kiszolgáló által a HTTPS-alapú proxykiszolgálókat nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="d23ce-115">HTTPS-based proxy servers are not supported by hello configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="d23ce-116">Védelmi célok megválasztása</span><span class="sxs-lookup"><span data-stu-id="d23ce-116">Choose your protection goals</span></span>

1. <span data-ttu-id="d23ce-117">A hello Azure-portálon, válassza a toohello **Recovery Services** tároló panelt, és válassza ki a tároló.</span><span class="sxs-lookup"><span data-stu-id="d23ce-117">In hello Azure portal, go toohello **Recovery Services** vault blade and select your vault.</span></span>
2. <span data-ttu-id="d23ce-118">A hello tároló hello erőforrás menüben nyissa meg túl**bevezetés** > **Site Recovery** > **1. lépés: infrastruktúra előkészítése**  >  **Védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="d23ce-118">On hello resource menu of hello vault, go too**Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. <span data-ttu-id="d23ce-120">A **védelmi cél**, jelölje be **tooAzure**, és válassza a **Igen, amelyen a VMware vSphere Hipervizorra**.</span><span class="sxs-lookup"><span data-stu-id="d23ce-120">In **Protection goal**, select **tooAzure**, and choose **Yes, with VMware vSphere Hypervisor**.</span></span> <span data-ttu-id="d23ce-121">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d23ce-121">Then click **OK**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a><span data-ttu-id="d23ce-123">Hello forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="d23ce-123">Set up hello source environment</span></span>
<span data-ttu-id="d23ce-124">Hello forráskörnyezet beállítása magában foglalja a két fő tevékenység:</span><span class="sxs-lookup"><span data-stu-id="d23ce-124">Setting up hello source environment involves two main activities:</span></span>

- <span data-ttu-id="d23ce-125">Telepítse, és a konfigurációs kiszolgáló regisztrálása a Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d23ce-125">Install and register a configuration server with Site Recovery.</span></span>
- <span data-ttu-id="d23ce-126">A helyszíni virtuális gépek észlelése csatlakozzon a Site Recovery tooyour helyszíni VMware vCenter vagy vSphere EXSi gazdagépek.</span><span class="sxs-lookup"><span data-stu-id="d23ce-126">Discover your on-premises virtual machines by connecting Site Recovery tooyour on-premises VMware vCenter or vSphere EXSi hosts.</span></span>

### <a name="step-1-install-and-register-a-configuration-server"></a><span data-ttu-id="d23ce-127">1. lépés: Telepítse és regisztrálja a konfigurációs kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="d23ce-127">Step 1: Install and register a configuration server</span></span>

1. <span data-ttu-id="d23ce-128">Kattintson a **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="d23ce-128">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span> <span data-ttu-id="d23ce-129">A **forrás előkészítése**, ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló** tooadd egyet.</span><span class="sxs-lookup"><span data-stu-id="d23ce-129">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** tooadd one.</span></span>

    ![A forrás beállítása](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. <span data-ttu-id="d23ce-131">A hello **kiszolgáló hozzáadása** panelen ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="d23ce-131">On hello **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="d23ce-132">Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="d23ce-132">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="d23ce-133">Hello tárolóbeli regisztrációs kulcs letöltése.</span><span class="sxs-lookup"><span data-stu-id="d23ce-133">Download hello vault registration key.</span></span> <span data-ttu-id="d23ce-134">Az egységes telepítő szüksége hello regisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d23ce-134">You need hello registration key when you run Unified Setup.</span></span> <span data-ttu-id="d23ce-135">hello kulcs a generálásától öt napig esetén érvényes.</span><span class="sxs-lookup"><span data-stu-id="d23ce-135">hello key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. <span data-ttu-id="d23ce-137">Hello gépen hello kiszolgálóként használ, futtassa **Azure Site Recovery az egységes telepítő** tooinstall hello konfigurációs kiszolgáló, a hello folyamatkiszolgáló és a hello fő célkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d23ce-137">On hello machine you’re using as hello configuration server, run **Azure Site Recovery Unified Setup** tooinstall hello configuration server, hello process server, and hello master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="d23ce-138">Futtassa az Azure Site Recovery egyesített telepítő</span><span class="sxs-lookup"><span data-stu-id="d23ce-138">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="d23ce-139">Konfigurációs kiszolgáló regisztrálása sikertelen lesz, ha hello a számítógép rendszerórája eltér a helyi idő több mint öt perc.</span><span class="sxs-lookup"><span data-stu-id="d23ce-139">Configuration server registration fails if hello time on your computer's system clock differs from local time by more than five minutes.</span></span> <span data-ttu-id="d23ce-140">A rendszer szinkronizálást, és egy [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) hello telepítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="d23ce-140">Synchronize your system clock with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting hello installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="d23ce-141">hello konfigurációs kiszolgálón a parancssorból is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="d23ce-141">hello configuration server can be installed via command line.</span></span> <span data-ttu-id="d23ce-142">További információkért lásd: [telepítése hello konfigurációs kiszolgáló parancssori eszközökkel](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="d23ce-142">For more information, see [Installing hello configuration server using Command-line tools](http://aka.ms/installconfigsrv).</span></span>

#### <a name="add-hello-vmware-account-for-automatic-discovery"></a><span data-ttu-id="d23ce-143">Automatikus felderítés hello VMware fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d23ce-143">Add hello VMware account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a><span data-ttu-id="d23ce-144">2. lépés: A vCenter hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d23ce-144">Step 2: Add a vCenter</span></span>
<span data-ttu-id="d23ce-145">tooallow Azure Site Recovery toodiscover futó virtuális gépek a helyszíni környezetben, kell tooconnect a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d23ce-145">tooallow Azure Site Recovery toodiscover virtual machines running in your on-premises environment, you need tooconnect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span>

<span data-ttu-id="d23ce-146">Válassza ki **+ vCenter** toostart VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.</span><span class="sxs-lookup"><span data-stu-id="d23ce-146">Select **+vCenter** toostart connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a><span data-ttu-id="d23ce-147">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="d23ce-147">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="d23ce-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d23ce-148">Next steps</span></span>
<span data-ttu-id="d23ce-149">[A célkörnyezet beállítása](./site-recovery-prepare-target-vmware-to-azure.md) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d23ce-149">[Set up your target environment](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.</span></span>
