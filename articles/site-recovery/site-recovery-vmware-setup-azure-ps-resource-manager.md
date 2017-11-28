---
title: " Az Azure (erőforrás-kezelő) folyamat kiszolgáló kezeléséhez |} Microsoft Docs"
description: "Ez a cikk ismerteti a hogyan tooset be egy feladat-visszavétel feldolgozni a kiszolgáló (Resource Manager) az Azure-ban."
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
ms.openlocfilehash: 70426b96095cc42befff6c4114fb56536284a667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a><span data-ttu-id="66ac3-103">(Erőforrás-kezelő) Azure-ban futó folyamat kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="66ac3-103">Manage a process server running in Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="66ac3-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="66ac3-104">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [<span data-ttu-id="66ac3-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="66ac3-105">Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)

<span data-ttu-id="66ac3-106">A feladat-visszavétel során nagy késleltetésű hello Azure virtuális hálózat és a helyszíni hálózat között esetén ajánlott toodeploy folyamatkiszolgáló az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="66ac3-106">During failback, it is recommended toodeploy process server in Azure if there is high latency between hello Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="66ac3-107">Ez a cikk ismerteti, hogyan beállításához, konfigurálásához és hello folyamat kiszolgálók Azure-beli kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="66ac3-107">This article describes how you can set up, configure, and manage hello process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="66ac3-108">Ez a cikk használható, ha a használt toobe **erőforrás-kezelő** hello telepítési modell hello virtuális gépek a feladatátvétel során.</span><span class="sxs-lookup"><span data-stu-id="66ac3-108">This article is toobe used if you used **Resource Manager** as hello deployment model for hello virtual machines during failover.</span></span> <span data-ttu-id="66ac3-109">Ha követte **klasszikus** hello telepítési modell, kövesse a lépéseket hello [hogyan tooset akár & a feladat-visszavételi folyamatkiszolgáló (klasszikus) konfigurálása](./site-recovery-vmware-setup-azure-ps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="66ac3-109">If you used **Classic** as hello deployment model, follow hello steps in [How tooset up & configure a Failback process server (Classic)](./site-recovery-vmware-setup-azure-ps-classic.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66ac3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="66ac3-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="66ac3-111">Az Azure-on folyamat-kiszolgáló központi telepítése</span><span class="sxs-lookup"><span data-stu-id="66ac3-111">Deploy a process server on Azure</span></span>
1. <span data-ttu-id="66ac3-112">A tároló hello > **Site Recovery-infrastruktúra** (a hello "Kezelése" fejléc) > **konfigurációs kiszolgálók** (a "A VMware és fizikai gépek" fejléc), válassza a hello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="66ac3-112">In hello Vault > **Site Recovery Infrastructure** (under hello "Manage" heading) > **Configuration Servers** (under "For VMware and Physical Machines" heading), select hello configuration server.</span></span>
2. <span data-ttu-id="66ac3-113">Hello konfigurációs kiszolgáló részleteit megjelenítő oldalon megnyíló kattintson "+ feldolgozni a kiszolgáló"</span><span class="sxs-lookup"><span data-stu-id="66ac3-113">In hello Configuration Server details page that opens click "+ Process server"</span></span>

  ![Folyamat server gyűjtemény hozzáadása](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  <span data-ttu-id="66ac3-115">A hello **Hozzáadás folyamatkiszolgáló** lapon, jelölje be a következő értékek hello</span><span class="sxs-lookup"><span data-stu-id="66ac3-115">On hello **Add process server** page, select hello following values</span></span>

  ![Adja hozzá a folyamatkiszolgáló katalóguseleméhez](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|<span data-ttu-id="66ac3-117">**Mező neve**</span><span class="sxs-lookup"><span data-stu-id="66ac3-117">**Field Name**</span></span>|<span data-ttu-id="66ac3-118">**Érték**</span><span class="sxs-lookup"><span data-stu-id="66ac3-118">**Value**</span></span>|
|-|-|
|<span data-ttu-id="66ac3-119">Adja meg, ahol toodeploy a folyamatkiszolgálót</span><span class="sxs-lookup"><span data-stu-id="66ac3-119">Choose where you want toodeploy your process server</span></span>|<span data-ttu-id="66ac3-120">Adja meg a hello értéket **az Azure-ban feladat-visszavételi folyamatkiszolgáló telepítése**</span><span class="sxs-lookup"><span data-stu-id="66ac3-120">Select hello value **Deploy a failback process server in Azure**</span></span> |
|<span data-ttu-id="66ac3-121">Előfizetés</span><span class="sxs-lookup"><span data-stu-id="66ac3-121">Subscription</span></span>|<span data-ttu-id="66ac3-122">Ha feladatátvételt hello virtuális gépek Azure-előfizetés hello kiválasztása</span><span class="sxs-lookup"><span data-stu-id="66ac3-122">Select hello Azure Subscription where you failed over hello virtual machines</span></span>|
|<span data-ttu-id="66ac3-123">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="66ac3-123">Resource Group</span></span>|<span data-ttu-id="66ac3-124">Hozzon létre egy erőforráscsoportot toodeploy a folyamatkiszolgáló, vagy válasszon egy meglévő erőforráscsoportot toodeploy hello folyamatkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="66ac3-124">You can create a Resource Group toodeploy this process server or choose toodeploy hello process server in an existing Resource Group</span></span>|
|<span data-ttu-id="66ac3-125">Hely</span><span class="sxs-lookup"><span data-stu-id="66ac3-125">Location</span></span>|<span data-ttu-id="66ac3-126">Válassza ki a hello Azure-adatközpont hello virtuális gépek az adott történő feladatátvételt</span><span class="sxs-lookup"><span data-stu-id="66ac3-126">Select hello Azure Data Center into which hello virtual machines where failed over into</span></span>|
|<span data-ttu-id="66ac3-127">Azure Network-hálózat</span><span class="sxs-lookup"><span data-stu-id="66ac3-127">Azure Network</span></span>|<span data-ttu-id="66ac3-128">Jelölje be hello Azure virtuális gépek hello virtuális Network(VNet) Ha a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="66ac3-128">Select hello Azure Virtual Network(VNet) that hello virtual machines where failed over into.</span></span> <span data-ttu-id="66ac3-129">Ha több Azure Vnetekhez történő sikertelen a virtuális gépeket, majd van szüksége a folyamatkiszolgáló telepítése egy virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="66ac3-129">If you failed over virtual machines into multiple Azure VNets, then you need a process server deployed per VNet</span></span>|

4. <span data-ttu-id="66ac3-130">Töltse ki hello többi hello folyamatkiszolgáló hello tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="66ac3-130">Fill in hello rest of hello properties for hello process server</span></span>

  ![Összegzési folyamat-kiszolgáló hozzáadása](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|<span data-ttu-id="66ac3-132">**Mező neve**</span><span class="sxs-lookup"><span data-stu-id="66ac3-132">**Field Name**</span></span>|<span data-ttu-id="66ac3-133">**Érték**</span><span class="sxs-lookup"><span data-stu-id="66ac3-133">**Value**</span></span>|
|-|-|
|<span data-ttu-id="66ac3-134">Kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="66ac3-134">Server Name</span></span>|<span data-ttu-id="66ac3-135">Megjelenített név és a folyamat kiszolgáló virtuális gép állomásnevét</span><span class="sxs-lookup"><span data-stu-id="66ac3-135">Display name & Host name for your process server virtual machine</span></span>|
| <span data-ttu-id="66ac3-136">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="66ac3-136">User Name</span></span>|<span data-ttu-id="66ac3-137">A felhasználónevet, amely a virtuális gépen a rendszergazda válik</span><span class="sxs-lookup"><span data-stu-id="66ac3-137">A user name that becomes an Administrator on that virtual machine</span></span>|
|<span data-ttu-id="66ac3-138">Tárfiók</span><span class="sxs-lookup"><span data-stu-id="66ac3-138">Storage Account</span></span>|<span data-ttu-id="66ac3-139">Hello Tárfiókot, ahol kerülnek, hello virtuális gép virtuális lemez neve</span><span class="sxs-lookup"><span data-stu-id="66ac3-139">Name of hello Storage Account where hello virtual machine's virtual disk's are placed</span></span>|
|<span data-ttu-id="66ac3-140">Alhálózat</span><span class="sxs-lookup"><span data-stu-id="66ac3-140">Subnet</span></span>|<span data-ttu-id="66ac3-141">hello alhálózati hello Azure VNet toowhich hello virtuális gép csatlakozik</span><span class="sxs-lookup"><span data-stu-id="66ac3-141">hello subnet of hello Azure VNet toowhich hello virtual machine is connected</span></span>|
| <span data-ttu-id="66ac3-142">IP-cím</span><span class="sxs-lookup"><span data-stu-id="66ac3-142">IP Address</span></span>|<span data-ttu-id="66ac3-143">IP-címet, hogy milyen hello folyamat server tooassume után elinduló</span><span class="sxs-lookup"><span data-stu-id="66ac3-143">IP Address that you would like hello process server tooassume once it boots up</span></span>|
5. <span data-ttu-id="66ac3-144">Kattintson a hello OK gomb toostart hello folyamat kiszolgáló virtuális gép telepítését.</span><span class="sxs-lookup"><span data-stu-id="66ac3-144">Click hello OK button toostart deploying hello process server virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="66ac3-145">toobe képes toouse a folyamatkiszolgálót a feladat-visszavétel tooregister kell azt hello helyszíni konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="66ac3-145">toobe able toouse this process server for failback, you need tooregister it with hello on-premises configuration server.</span></span>

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a><span data-ttu-id="66ac3-146">Hello folyamat (az Azure-ban futtató) tooa konfigurációs kiszolgáló (a helyszínen fut) regisztrálása</span><span class="sxs-lookup"><span data-stu-id="66ac3-146">Registering hello process server (running in Azure) tooa Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a><span data-ttu-id="66ac3-147">Hello folyamat server toolatest verziójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="66ac3-147">Upgrading hello process server toolatest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="66ac3-148">Beállításjegyzékből való törlésekor (Azure-beli) hello folyamatkiszolgáló a konfigurációs kiszolgálóról (a helyszínen fut)</span><span class="sxs-lookup"><span data-stu-id="66ac3-148">Unregistering hello process server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
