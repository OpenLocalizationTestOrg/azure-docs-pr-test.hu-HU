---
title: "Állítsa be a forrás-környezetet (VMware az Azure-bA) |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan állíthat be a helyszíni környezetben elindítani a VMware virtuális gépek replikálása Azure-bA."
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
ms.openlocfilehash: a2fabc56463c8cbf0b8a76b7a84369ed8e535486
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a><span data-ttu-id="90e1b-103">Állítsa be a forrás környezetet (az Azure-bA VMware)</span><span class="sxs-lookup"><span data-stu-id="90e1b-103">Set up the source environment (VMware to Azure)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="90e1b-104">VMware – Azure</span><span class="sxs-lookup"><span data-stu-id="90e1b-104">VMware to Azure</span></span>](./site-recovery-set-up-vmware-to-azure.md)
> * [<span data-ttu-id="90e1b-105">Fizikai az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="90e1b-105">Physical to Azure</span></span>](./site-recovery-set-up-physical-to-azure.md)

<span data-ttu-id="90e1b-106">A cikkből megtudhatja, hogyan állíthat be a helyszíni környezet az Azure-bA a VMware rendszerben futó virtuális gépek replikálást indítani.</span><span class="sxs-lookup"><span data-stu-id="90e1b-106">This article describes how to set up your on-premises environment to start replicating virtual machines running on VMware to Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90e1b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="90e1b-107">Prerequisites</span></span>

<span data-ttu-id="90e1b-108">A cikk feltételezi, hogy már létrehozta:</span><span class="sxs-lookup"><span data-stu-id="90e1b-108">The article assumes that you have already created:</span></span>
- <span data-ttu-id="90e1b-109">A Recovery Services-tároló a a [Azure-portálon](http://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="90e1b-109">A Recovery Services Vault in the [Azure portal](http://portal.azure.com "Azure portal").</span></span>
- <span data-ttu-id="90e1b-110">A VMware vcenter programban, amely nem használható egy külön fiókot [automatikus felderítés](./site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="90e1b-110">A dedicated account in your VMware vCenter that can be used for [automatic discovery](./site-recovery-vmware-to-azure.md).</span></span>
- <span data-ttu-id="90e1b-111">A virtuális gép telepítéséhez a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="90e1b-111">A virtual machine on which to install the configuration server.</span></span>

## <a name="configuration-server-minimum-requirements"></a><span data-ttu-id="90e1b-112">Konfigurációs kiszolgáló minimális követelményeknek</span><span class="sxs-lookup"><span data-stu-id="90e1b-112">Configuration server minimum requirements</span></span>
<span data-ttu-id="90e1b-113">A konfigurációs kiszolgáló szoftvert kell telepíteni a VMware magas rendelkezésre állású virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="90e1b-113">The configuration server software should be deployed on a highly available VMware virtual machine.</span></span> <span data-ttu-id="90e1b-114">A következő táblázat felsorolja a minimális hardver-, szoftver, és hálózati követelményei a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="90e1b-114">The following table lists the minimum hardware, software, and network requirements for a configuration server.</span></span>
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> <span data-ttu-id="90e1b-115">A konfigurációs kiszolgáló által a HTTPS-alapú proxykiszolgálókat nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="90e1b-115">HTTPS-based proxy servers are not supported by the configuration server.</span></span>

## <a name="choose-your-protection-goals"></a><span data-ttu-id="90e1b-116">Védelmi célok megválasztása</span><span class="sxs-lookup"><span data-stu-id="90e1b-116">Choose your protection goals</span></span>

1. <span data-ttu-id="90e1b-117">Az Azure-portálon lépjen a **Recovery Services** tároló panelt, és válassza ki a tároló.</span><span class="sxs-lookup"><span data-stu-id="90e1b-117">In the Azure portal, go to the **Recovery Services** vault blade and select your vault.</span></span>
2. <span data-ttu-id="90e1b-118">A tároló erőforrás menüben Ugrás **bevezetés** > **Site Recovery** > **1. lépés: infrastruktúra előkészítése**  >  **Védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="90e1b-118">On the resource menu of the vault, go to **Getting Started** > **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. <span data-ttu-id="90e1b-120">A **védelmi cél**, jelölje be **az Azure-bA**, és válassza a **Igen, amelyen a VMware vSphere Hipervizorra**.</span><span class="sxs-lookup"><span data-stu-id="90e1b-120">In **Protection goal**, select **To Azure**, and choose **Yes, with VMware vSphere Hypervisor**.</span></span> <span data-ttu-id="90e1b-121">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="90e1b-121">Then click **OK**.</span></span>

    ![Célok megválasztása](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a><span data-ttu-id="90e1b-123">A forráskörnyezet beállítása</span><span class="sxs-lookup"><span data-stu-id="90e1b-123">Set up the source environment</span></span>
<span data-ttu-id="90e1b-124">A forrás-környezet létrehozása két fő tevékenységet foglal magában:</span><span class="sxs-lookup"><span data-stu-id="90e1b-124">Setting up the source environment involves two main activities:</span></span>

- <span data-ttu-id="90e1b-125">Telepítse, és a konfigurációs kiszolgáló regisztrálása a Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="90e1b-125">Install and register a configuration server with Site Recovery.</span></span>
- <span data-ttu-id="90e1b-126">A helyszíni virtuális gépek észlelése csatlakozva a Site Recovery a helyszíni VMware vCenter vagy vSphere EXSi gazdagépek.</span><span class="sxs-lookup"><span data-stu-id="90e1b-126">Discover your on-premises virtual machines by connecting Site Recovery to your on-premises VMware vCenter or vSphere EXSi hosts.</span></span>

### <a name="step-1-install-and-register-a-configuration-server"></a><span data-ttu-id="90e1b-127">1. lépés: Telepítse és regisztrálja a konfigurációs kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="90e1b-127">Step 1: Install and register a configuration server</span></span>

1. <span data-ttu-id="90e1b-128">Kattintson a **1. lépés: infrastruktúra előkészítése** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="90e1b-128">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span> <span data-ttu-id="90e1b-129">A **forrás előkészítése**, ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló** kattintva felvehet egyet.</span><span class="sxs-lookup"><span data-stu-id="90e1b-129">In **Prepare source**, if you don’t have a configuration server, click **+Configuration server** to add one.</span></span>

    ![A forrás beállítása](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. <span data-ttu-id="90e1b-131">Az a **kiszolgáló hozzáadása** panelen ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.</span><span class="sxs-lookup"><span data-stu-id="90e1b-131">On the **Add Server** blade, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="90e1b-132">Töltse le a Site Recovery az egységes telepítő telepítőfájlját.</span><span class="sxs-lookup"><span data-stu-id="90e1b-132">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="90e1b-133">Töltse le a tárolóregisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="90e1b-133">Download the vault registration key.</span></span> <span data-ttu-id="90e1b-134">Az egységes telepítő futtatásakor a regisztrációs kulcsot kell.</span><span class="sxs-lookup"><span data-stu-id="90e1b-134">You need the registration key when you run Unified Setup.</span></span> <span data-ttu-id="90e1b-135">A kulcs a generálásától számított öt napig érvényes.</span><span class="sxs-lookup"><span data-stu-id="90e1b-135">The key is valid for five days after you generate it.</span></span>

    ![A forrás beállítása](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. <span data-ttu-id="90e1b-137">A számítógépen, mint a konfigurációs kiszolgálót használ, futtassa **Azure Site Recovery az egységes telepítő** a konfigurációs kiszolgáló, a folyamatkiszolgáló és a fő célkiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="90e1b-137">On the machine you’re using as the configuration server, run **Azure Site Recovery Unified Setup** to install the configuration server, the process server, and the master target server.</span></span>

#### <a name="run-azure-site-recovery-unified-setup"></a><span data-ttu-id="90e1b-138">Futtassa az Azure Site Recovery egyesített telepítő</span><span class="sxs-lookup"><span data-stu-id="90e1b-138">Run Azure Site Recovery Unified Setup</span></span>

> [!TIP]
> <span data-ttu-id="90e1b-139">Konfigurációs kiszolgáló regisztrálása sikertelen lesz, ha az a számítógép rendszerórája eltér a helyi idő több mint öt perc.</span><span class="sxs-lookup"><span data-stu-id="90e1b-139">Configuration server registration fails if the time on your computer's system clock differs from local time by more than five minutes.</span></span> <span data-ttu-id="90e1b-140">A rendszer szinkronizálást, és egy [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) a telepítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="90e1b-140">Synchronize your system clock with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) before starting the installation.</span></span>

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="90e1b-141">A konfigurációs kiszolgáló telepíthető a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="90e1b-141">The configuration server can be installed via command line.</span></span> <span data-ttu-id="90e1b-142">További információkért lásd: [a konfigurációs kiszolgáló, a parancssori eszközök telepítése](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="90e1b-142">For more information, see [Installing the configuration server using Command-line tools](http://aka.ms/installconfigsrv).</span></span>

#### <a name="add-the-vmware-account-for-automatic-discovery"></a><span data-ttu-id="90e1b-143">Adja hozzá a VMware-fiókot a automatikus felderítése</span><span class="sxs-lookup"><span data-stu-id="90e1b-143">Add the VMware account for automatic discovery</span></span>

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a><span data-ttu-id="90e1b-144">2. lépés: A vCenter hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90e1b-144">Step 2: Add a vCenter</span></span>
<span data-ttu-id="90e1b-145">Ahhoz, hogy az Azure Site Recovery számára a helyszíni környezetben futó virtuális gépek felderítése, meg kell kapcsolni a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek a Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="90e1b-145">To allow Azure Site Recovery to discover virtual machines running in your on-premises environment, you need to connect your VMware vCenter Server or vSphere ESXi hosts with Site Recovery.</span></span>

<span data-ttu-id="90e1b-146">Válassza ki **+ vCenter** elindítani a VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.</span><span class="sxs-lookup"><span data-stu-id="90e1b-146">Select **+vCenter** to start connecting a VMware vCenter server or a VMware vSphere ESXi host.</span></span>

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a><span data-ttu-id="90e1b-147">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="90e1b-147">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a><span data-ttu-id="90e1b-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90e1b-148">Next steps</span></span>
<span data-ttu-id="90e1b-149">[A célkörnyezet beállítása](./site-recovery-prepare-target-vmware-to-azure.md) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="90e1b-149">[Set up your target environment](./site-recovery-prepare-target-vmware-to-azure.md) in Azure.</span></span>
