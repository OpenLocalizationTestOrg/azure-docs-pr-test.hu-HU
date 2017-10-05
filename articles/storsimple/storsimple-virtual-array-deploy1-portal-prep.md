---
title: "A StorSimple virtuális tömb portál prep |} Microsoft Docs"
description: "A StorSimple virtuális tömb telepítendő első oktatóanyaga, amely magában foglalja a előkészítése az Azure-portálon"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d0801053721f98ce7a2b0fcbe3c65da8dbdd8d3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a><span data-ttu-id="e35fb-103">A StorSimple virtuális tömb telepítése – előkészítése az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e35fb-103">Deploy StorSimple Virtual Array - Prepare the Azure portal</span></span>

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a><span data-ttu-id="e35fb-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e35fb-104">Overview</span></span>

<span data-ttu-id="e35fb-105">Ez a teljesen telepítsen az adott virtuális tömb, egy fájl vagy a Resource Manager modellt használja az iSCSI-kiszolgáló szükséges az üzembehelyezési oktatóanyagok a sorozat első cikkében.</span><span class="sxs-lookup"><span data-stu-id="e35fb-105">This is the first article in the series of deployment tutorials required to completely deploy your virtual array as a file server or an iSCSI server using the Resource Manager model.</span></span> <span data-ttu-id="e35fb-106">Ez a cikk ismerteti a előkészületek létrehozása és konfigurálása a StorSimple eszköz Manager szolgáltatás virtuális tömb kiépítése előtt.</span><span class="sxs-lookup"><span data-stu-id="e35fb-106">This article describes the preparation required to create and configure your StorSimple Device Manager service prior to provisioning a virtual array.</span></span> <span data-ttu-id="e35fb-107">Ez a cikk is kapcsolja ki egy üzembehelyezési konfigurációs ellenőrzőlista és konfigurációs előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="e35fb-107">This article also links out to a deployment configuration checklist and configuration prerequisites.</span></span>

<span data-ttu-id="e35fb-108">A beállítási és konfigurációs folyamat befejezéséhez rendszergazdai jogosultságok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="e35fb-108">You need administrator privileges to complete the setup and configuration process.</span></span> <span data-ttu-id="e35fb-109">Ajánlott áttekinteni a üzembehelyezési konfigurációs ellenőrzőlista megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="e35fb-109">We recommend that you review the deployment configuration checklist before you begin.</span></span> <span data-ttu-id="e35fb-110">A portál előkészítése kisebb, mint 10 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e35fb-110">The portal preparation takes less than 10 minutes.</span></span>

<span data-ttu-id="e35fb-111">Ez a cikk a közzétett adatokat a központi telepítést a StorSimple virtuális tömbök, az Azure portál és a Microsoft Azure Government felhő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e35fb-111">The information published in this article applies to the deployment of StorSimple Virtual Arrays in the Azure portal and Microsoft Azure Government Cloud.</span></span>

### <a name="get-started"></a><span data-ttu-id="e35fb-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e35fb-112">Get started</span></span>
<span data-ttu-id="e35fb-113">A telepítési munkafolyamat előkészítése a portálon, a virtualizált környezetben egy virtuális tömb kiépítés, és a telepítés befejezése áll.</span><span class="sxs-lookup"><span data-stu-id="e35fb-113">The deployment workflow consists of preparing the portal, provisioning a virtual array in your virtualized environment, and completing the setup.</span></span> <span data-ttu-id="e35fb-114">Az egy fájl vagy iSCSI-kiszolgáló, a StorSimple virtuális tömb telepítésének első lépései, tekintse meg az alábbi táblázatos erőforrások kell.</span><span class="sxs-lookup"><span data-stu-id="e35fb-114">To get started with the StorSimple Virtual Array deployment as a file server or an iSCSI server, you need to refer to the following tabulated resources.</span></span>

#### <a name="deployment-articles"></a><span data-ttu-id="e35fb-115">Telepítési cikkek</span><span class="sxs-lookup"><span data-stu-id="e35fb-115">Deployment articles</span></span>

<span data-ttu-id="e35fb-116">A StorSimple virtuális tömb telepítéséhez tekintse meg a következő cikkekben által előírt sorrendben.</span><span class="sxs-lookup"><span data-stu-id="e35fb-116">To deploy your StorSimple Virtual Array, refer to the following articles in the prescribed sequence.</span></span>

| **#** | <span data-ttu-id="e35fb-117">**Ebben a lépésben**</span><span class="sxs-lookup"><span data-stu-id="e35fb-117">**In this step**</span></span> | <span data-ttu-id="e35fb-118">**Ezt megteheti...**</span><span class="sxs-lookup"><span data-stu-id="e35fb-118">**You do this …**</span></span> | <span data-ttu-id="e35fb-119">**És használja ezeket a dokumentumokat.**</span><span class="sxs-lookup"><span data-stu-id="e35fb-119">**And use these documents.**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e35fb-120">1.</span><span class="sxs-lookup"><span data-stu-id="e35fb-120">1.</span></span> |<span data-ttu-id="e35fb-121">**Állítsa be az Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="e35fb-121">**Set up the Azure portal**</span></span> |<span data-ttu-id="e35fb-122">Létrehozhat és konfigurálhat egy StorSimple virtuális tömb kiépítése előtt a StorSimple Device Manager szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="e35fb-122">Create and configure your StorSimple Device Manager service prior to provisioning a StorSimple Virtual Array.</span></span> |[<span data-ttu-id="e35fb-123">Készítse elő a portálon</span><span class="sxs-lookup"><span data-stu-id="e35fb-123">Prepare the portal</span></span>](storsimple-virtual-array-deploy1-portal-prep.md) |
| <span data-ttu-id="e35fb-124">2.</span><span class="sxs-lookup"><span data-stu-id="e35fb-124">2.</span></span> |<span data-ttu-id="e35fb-125">**A virtuális tömb kiépítése**</span><span class="sxs-lookup"><span data-stu-id="e35fb-125">**Provision the Virtual Array**</span></span> |<span data-ttu-id="e35fb-126">A Hyper-V kiépíteni, és kapcsolódjon a StorSimple virtuális tömb állomás operációs rendszert futtató Hyper-V a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszeren.</span><span class="sxs-lookup"><span data-stu-id="e35fb-126">For Hyper-V, provision and connect to a StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <br></br> <br></br> <span data-ttu-id="e35fb-127">A VMware kiépíteni, és kapcsolódjon egy StorSimple virtuális tömb a gazdagéphez, VMware ESXi 5.5 rendszerű vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="e35fb-127">For VMware, provision and connect to a StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span><br></br> |[<span data-ttu-id="e35fb-128">A Hyper-V egy virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="e35fb-128">Provision a virtual array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [<span data-ttu-id="e35fb-129">VMware-ben a virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="e35fb-129">Provision a virtual array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md) |
| <span data-ttu-id="e35fb-130">3.</span><span class="sxs-lookup"><span data-stu-id="e35fb-130">3.</span></span> |<span data-ttu-id="e35fb-131">**A virtuális tömb beállítása**</span><span class="sxs-lookup"><span data-stu-id="e35fb-131">**Set up the Virtual Array**</span></span> |<span data-ttu-id="e35fb-132">A fájlkiszolgáló hajtsa végre a kezdeti beállítás regisztrálása a StorSimple fájlkiszolgáló és az eszköz beállításának befejezése.</span><span class="sxs-lookup"><span data-stu-id="e35fb-132">For your file server, perform initial setup, register your StorSimple file server, and complete the device setup.</span></span> <span data-ttu-id="e35fb-133">SMB-megosztások majd oszthat.</span><span class="sxs-lookup"><span data-stu-id="e35fb-133">You can then provision SMB shares.</span></span> <br></br> <br></br> <span data-ttu-id="e35fb-134">Az iSCSI-kiszolgáló hajtsa végre a kezdeti beállítás, a StorSimple iSCSI-kiszolgáló regisztrálása és az eszköz beállításának befejezése.</span><span class="sxs-lookup"><span data-stu-id="e35fb-134">For your iSCSI server, perform initial setup, register your StorSimple iSCSI server, and complete the device setup.</span></span> <span data-ttu-id="e35fb-135">Az iSCSI-kötetek majd oszthat.</span><span class="sxs-lookup"><span data-stu-id="e35fb-135">You can then provision iSCSI volumes.</span></span> |[<span data-ttu-id="e35fb-136">Fájlkiszolgáló virtuális tömb beállítása</span><span class="sxs-lookup"><span data-stu-id="e35fb-136">Set up virtual array as file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[<span data-ttu-id="e35fb-137">Állítsa be a tömb virtuális iSCSI-kiszolgálóként</span><span class="sxs-lookup"><span data-stu-id="e35fb-137">Set up virtual array as iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md) |

<span data-ttu-id="e35fb-138">Most már megkezdheti beállítása az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e35fb-138">You can now begin to set up the Azure portal.</span></span>

## <a name="configuration-checklist"></a><span data-ttu-id="e35fb-139">Konfigurációs ellenőrzőlista</span><span class="sxs-lookup"><span data-stu-id="e35fb-139">Configuration checklist</span></span>

<span data-ttu-id="e35fb-140">A konfigurációs ellenőrzőlistát a kell összegyűjtenie, mielőtt konfigurálná a szoftver a StorSimple virtuális tömb a információkat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e35fb-140">The configuration checklist describes the information that you need to collect before you configure the software on your StorSimple Virtual Array.</span></span> <span data-ttu-id="e35fb-141">Az információk időben előkészítése segít felgyorsítják az adott környezetben a StorSimple eszköz üzembe.</span><span class="sxs-lookup"><span data-stu-id="e35fb-141">Preparing this information ahead of time helps streamline the process of deploying the StorSimple device in your environment.</span></span> <span data-ttu-id="e35fb-142">Attól függően, hogy a StorSimple virtuális tömb van telepítve egy fájl vagy iSCSI-kiszolgáló, egyike szükséges a következő ellenőrzőlista.</span><span class="sxs-lookup"><span data-stu-id="e35fb-142">Depending upon whether your StorSimple Virtual Array is deployed as a file server or an iSCSI server, you need one of the following checklists.</span></span>

* <span data-ttu-id="e35fb-143">Töltse le a [StorSimple virtuális tömb fájl kiszolgáló konfigurációs ellenőrzőlista](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="e35fb-143">Download the [StorSimple Virtual Array File Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span></span>
* <span data-ttu-id="e35fb-144">Töltse le a [StorSimple virtuális tömb iSCSI kiszolgáló konfigurációs ellenőrzőlista](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="e35fb-144">Download the [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e35fb-145">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e35fb-145">Prerequisites</span></span>

<span data-ttu-id="e35fb-146">Itt megtalálja az Ön a StorSimple eszköz kezelő szolgáltatás, a StorSimple virtuális tömb és az Adatközpont hálózati konfigurációs előfeltételeit.</span><span class="sxs-lookup"><span data-stu-id="e35fb-146">Here you find the configuration prerequisites for your StorSimple Device Manager service, your StorSimple Virtual Array, and the datacenter network.</span></span>

### <a name="for-the-storsimple-device-manager-service"></a><span data-ttu-id="e35fb-147">A StorSimple-eszközkezelő szolgáltatás esetén</span><span class="sxs-lookup"><span data-stu-id="e35fb-147">For the StorSimple Device Manager service</span></span>

<span data-ttu-id="e35fb-148">Mielőtt hozzákezd, győződjön meg az alábbiakról:</span><span class="sxs-lookup"><span data-stu-id="e35fb-148">Before you begin, make sure that:</span></span>

* <span data-ttu-id="e35fb-149">Rendelkezik Microsoft-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="e35fb-149">You have your Microsoft account with access credentials.</span></span>
* <span data-ttu-id="e35fb-150">Rendelkezik Microsoft Azure Storage-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="e35fb-150">You have your Microsoft Azure storage account with access credentials.</span></span>
* <span data-ttu-id="e35fb-151">A Microsoft Azure-előfizetés engedélyezni kell a StorSimple eszköz Manager szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="e35fb-151">Your Microsoft Azure subscription should be enabled for StorSimple Device Manager service.</span></span>

### <a name="for-the-storsimple-virtual-array"></a><span data-ttu-id="e35fb-152">A StorSimple virtuális tömb</span><span class="sxs-lookup"><span data-stu-id="e35fb-152">For the StorSimple Virtual Array</span></span>

<span data-ttu-id="e35fb-153">Egy virtuális tömb telepítése előtt győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="e35fb-153">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="e35fb-154">Hozzáférést egy adott gazdarendszeren futó Windows Server 2008 R2 vagy újabb Hyper-V vagy VMware (ESXi 5.5-ös vagy újabb), amely rendelkezik egy kiépítéséhez használt eszköz.</span><span class="sxs-lookup"><span data-stu-id="e35fb-154">You have access to a host system running Hyper-V on Windows Server 2008 R2 or later or VMware (ESXi 5.5 or later) that can be used to a provision a device.</span></span>
* <span data-ttu-id="e35fb-155">A gazdagép rendszere tudni jelölt ki a virtuális tömb telepítéséhez a következőket:</span><span class="sxs-lookup"><span data-stu-id="e35fb-155">The host system is able to dedicate the following resources to provision your virtual array:</span></span>
  
  * <span data-ttu-id="e35fb-156">Legalább 4 mag.</span><span class="sxs-lookup"><span data-stu-id="e35fb-156">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="e35fb-157">Legalább 8 GB RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="e35fb-157">At least 8 GB of RAM.</span></span> <span data-ttu-id="e35fb-158">Ha szeretné konfigurálni a virtuális tömb fájlkiszolgálóként, a 8 GB 2 millió fájlokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="e35fb-158">If you plan to configure the virtual array as file server, 8 GB supports 2 million files.</span></span> <span data-ttu-id="e35fb-159">16 GB RAM, 2 – 4 millió fájlok támogatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="e35fb-159">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="e35fb-160">Egy hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="e35fb-160">One network interface.</span></span>
  * <span data-ttu-id="e35fb-161">500 GB-os virtuális lemez Rendszeradatok.</span><span class="sxs-lookup"><span data-stu-id="e35fb-161">A 500 GB virtual disk for system data.</span></span>

### <a name="for-the-datacenter-network"></a><span data-ttu-id="e35fb-162">Az Adatközpont-hálózat</span><span class="sxs-lookup"><span data-stu-id="e35fb-162">For the datacenter network</span></span>

<span data-ttu-id="e35fb-163">Mielőtt hozzákezd, győződjön meg az alábbiakról:</span><span class="sxs-lookup"><span data-stu-id="e35fb-163">Before you begin, make sure that:</span></span>

* <span data-ttu-id="e35fb-164">Az Adatközpont hálózati van konfigurálva, a StorSimple eszköz hálózatkezelési követelményei szerint.</span><span class="sxs-lookup"><span data-stu-id="e35fb-164">The network in your datacenter is configured as per the networking requirements for your StorSimple device.</span></span> <span data-ttu-id="e35fb-165">További információkért lásd: a [StorSimple virtuális tömb rendszerkövetelmények](storsimple-ova-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="e35fb-165">For more information, see the [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).</span></span>
* <span data-ttu-id="e35fb-166">A StorSimple virtuális tömb nem rendelkezik egy dedikált 5 MB/s internetes sávszélességet (vagy több) mindenkor.</span><span class="sxs-lookup"><span data-stu-id="e35fb-166">Your StorSimple Virtual Array has a dedicated 5 Mbps Internet bandwidth (or more) available at all times.</span></span> <span data-ttu-id="e35fb-167">A sávszélesség nem lehet megosztva egyéb alkalmazásokkal.</span><span class="sxs-lookup"><span data-stu-id="e35fb-167">This bandwidth should not be shared with any other applications.</span></span>

## <a name="step-by-step-preparation"></a><span data-ttu-id="e35fb-168">Részletes előkészítése</span><span class="sxs-lookup"><span data-stu-id="e35fb-168">Step-by-step preparation</span></span>

<span data-ttu-id="e35fb-169">Az alábbi részletes útmutatás segítségével készítse elő a portál a StorSimple eszköz Manager szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="e35fb-169">Use the following step-by-step instructions to prepare your portal for the StorSimple Device Manager service.</span></span>

## <a name="step-1-create-a-new-service"></a><span data-ttu-id="e35fb-170">1. lépés: Új szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e35fb-170">Step 1: Create a new service</span></span>

<span data-ttu-id="e35fb-171">Egyetlen példány futhat a StorSimple Device Manager szolgáltatás több StorSimple virtuális tömbök kezelheti.</span><span class="sxs-lookup"><span data-stu-id="e35fb-171">A single instance of the StorSimple Device Manager service can manage multiple StorSimple Virtual Arrays.</span></span> <span data-ttu-id="e35fb-172">Az alábbi lépések végrehajtásával hozza létre a StorSimple-eszközkezelő szolgáltatás egy példányát.</span><span class="sxs-lookup"><span data-stu-id="e35fb-172">Perform the following steps to create an instance of the StorSimple Device Manager service.</span></span> <span data-ttu-id="e35fb-173">Ha egy meglévő StorSimple Device Manager szolgáltatást a virtuális tömbök kezeléséhez, a lépés kihagyása és Ugrás [2. lépés: Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="e35fb-173">If you have an existing StorSimple Device Manager service to manage your virtual arrays, skip this step and go to [Step 2: Get the service registration key](#step-2-get-the-service-registration-key).</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> <span data-ttu-id="e35fb-174">Ha nem engedélyezte a tárfiók automatikus létrehozását a szolgáltatással, akkor legalább egy tárfiókot létre kell hoznia, miután sikeresen létrehozott egy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e35fb-174">If you did not enable the automatic creation of a storage account with your service, you will need to create at least one storage account after you have successfully created a service.</span></span>
> 
> * <span data-ttu-id="e35fb-175">Ha nem hozott létre automatikusan egy tárfiókot, a részletes utasításokat az [Új tárfiók konfigurálása a szolgáltatáshoz](#optional-step-configure-a-new-storage-account-for-the-service) című szakaszban tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e35fb-175">If you did not create a storage account automatically, go to [Configure a new storage account for the service](#optional-step-configure-a-new-storage-account-for-the-service) for detailed instructions.</span></span>
> * <span data-ttu-id="e35fb-176">Ha engedélyezte a tárfiók automatikus létrehozását, folytassa a [2. lépés: Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key) című szakasszal.</span><span class="sxs-lookup"><span data-stu-id="e35fb-176">If you enabled the automatic creation of a storage account, go to [Step 2: Get the service registration key](#step-2-get-the-service-registration-key).</span></span>
> 
> 

## <a name="step-2-get-the-service-registration-key"></a><span data-ttu-id="e35fb-177">2. lépés: Szolgáltatásregisztrációs kulcs lekérése</span><span class="sxs-lookup"><span data-stu-id="e35fb-177">Step 2: Get the service registration key</span></span>

<span data-ttu-id="e35fb-178">Ha a StorSimple-eszközkezelő szolgáltatás működik és elérhető, le kell kérnie a szolgáltatásregisztrációs kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e35fb-178">After the StorSimple Device Manager service is up and running, you will need to get the service registration key.</span></span> <span data-ttu-id="e35fb-179">Ezzel a kulccsal regisztrálhatja és csatlakoztathatja StorSimple eszközét a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="e35fb-179">This key is used to register and connect your StorSimple device with the service.</span></span>

<span data-ttu-id="e35fb-180">Hajtsa végre a következő lépéseket a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e35fb-180">Perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> <span data-ttu-id="e35fb-181">A szolgáltatás regisztrációs kulcsának használt összes a StorSimple Device Manager igénylő eszközök regisztrálása a StorSimple Device Manager szolgáltatásban való regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="e35fb-181">The service registration key is used to register all the StorSimple Device Manager devices that need to register with your StorSimple Device Manager service.</span></span>
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a><span data-ttu-id="e35fb-182">3. lépés: Töltse le a virtuális tömb kép</span><span class="sxs-lookup"><span data-stu-id="e35fb-182">Step 3: Download the virtual array image</span></span>

<span data-ttu-id="e35fb-183">Miután a szolgáltatás regisztrációs kulcsának, szüksége lesz letölti a megfelelő virtuális tömb lemezképet, a rendszer a gazdagép egy virtuális tömb kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e35fb-183">After you have the service registration key, you will need to download the appropriate virtual array image to provision a virtual array on your host system.</span></span> <span data-ttu-id="e35fb-184">A virtuális tömb lemezképek operációs rendszer adott és tölthető le: az Azure-portálon az első lépések lap.</span><span class="sxs-lookup"><span data-stu-id="e35fb-184">The virtual array images are operating system specific and can be downloaded from the Quick Start page in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e35fb-185">A StorSimple virtuális tömb futó szoftver csak a StorSimple Device Manager szolgáltatás is használhatók.</span><span class="sxs-lookup"><span data-stu-id="e35fb-185">The software running on the StorSimple Virtual Array may only be used with the StorSimple Device Manager service.</span></span>
> 
> 

<span data-ttu-id="e35fb-186">Hajtsa végre a következő lépéseket a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e35fb-186">Perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="to-get-the-virtual-array-image"></a><span data-ttu-id="e35fb-187">Virtuális tömb képlista</span><span class="sxs-lookup"><span data-stu-id="e35fb-187">To get the virtual array image</span></span>

1. <span data-ttu-id="e35fb-188">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e35fb-188">Sign into the [Azure portal](https://portal.azure.com/).</span></span> 
2. <span data-ttu-id="e35fb-189">Az Azure portálon kattintson **Tallózás > StorSimple eszköz kezelői**.</span><span class="sxs-lookup"><span data-stu-id="e35fb-189">In the Azure portal, click **Browse > StorSimple Device Managers**.</span></span>
3. <span data-ttu-id="e35fb-190">Válasszon ki egy meglévő StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e35fb-190">Select an existing StorSimple Device Manager service.</span></span> <span data-ttu-id="e35fb-191">Az a **StorSimple Device Manager** panelen kattintson a **gyors üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="e35fb-191">In the **StorSimple Device Manager** blade, click **Quick Start**.</span></span> 
4. <span data-ttu-id="e35fb-192">A Microsoft Download Center a letölteni kívánt kép megfelelő hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="e35fb-192">Click the link corresponding to the image that you want to download from the Microsoft Download Center.</span></span> <span data-ttu-id="e35fb-193">A kép mérete körülbelül 4.8 GB.</span><span class="sxs-lookup"><span data-stu-id="e35fb-193">The image files are approximately 4.8 GB.</span></span>
   
   * <span data-ttu-id="e35fb-194">A Hyper-V a Windows Server 2012 és újabb verziók VHDX</span><span class="sxs-lookup"><span data-stu-id="e35fb-194">VHDX for Hyper-V on Windows Server 2012 and later</span></span>
   * <span data-ttu-id="e35fb-195">Virtuális merevlemez Hyper-v a Windows Server 2008 R2 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="e35fb-195">VHD for Hyper-V on Windows Server 2008 R2 and later</span></span>
   * <span data-ttu-id="e35fb-196">Vmdk-fájl, a VMWare ESXi 5.5 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="e35fb-196">VMDK for VMWare ESXi 5.5 and later</span></span>
5. <span data-ttu-id="e35fb-197">Töltse le és csomagolja ki a fájlt egy helyi meghajtóra, a jegyezze fel, ahol a tömörítetlen fájl, így.</span><span class="sxs-lookup"><span data-stu-id="e35fb-197">Download and unzip the file to a local drive, making a note of where the unzipped file is located.</span></span>

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a><span data-ttu-id="e35fb-198">Nem kötelező lépés: a szolgáltatás egy új tárfiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e35fb-198">Optional step: Configure a new storage account for the service</span></span>

<span data-ttu-id="e35fb-199">Ez a lépés nem kötelező, és csak akkor, ha nem engedélyezte a tárfiók automatikus létrehozását a szolgáltatással történjen.</span><span class="sxs-lookup"><span data-stu-id="e35fb-199">This step is optional and should be performed only if you did not enable the automatic creation of a storage account with your service.</span></span>

<span data-ttu-id="e35fb-200">Ha az Azure storage-fiók létrehozása egy másik régióban van szüksége, tekintse meg [a storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account) részletes útmutatásait.</span><span class="sxs-lookup"><span data-stu-id="e35fb-200">If you need to create an Azure storage account in a different region, see [How to create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for step-by-step instructions.</span></span>

<span data-ttu-id="e35fb-201">Hajtsa végre a következő lépéseket a [Azure-portálon](https://ms.portal.azure.com/) egy meglévő Microsoft Azure storage-fiók hozzáadása a StorSimple Device Manager szolgáltatás oldalon.</span><span class="sxs-lookup"><span data-stu-id="e35fb-201">Perform the following steps in the [Azure portal](https://ms.portal.azure.com/) on the StorSimple Device Manager service page to add an existing Microsoft Azure storage account.</span></span>

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a><span data-ttu-id="e35fb-202">A tárolási fiók hitelesítő adatait, amely rendelkezik az Eszközkezelő szolgáltatásként az azonos Azure-előfizetés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e35fb-202">To add a storage account credential that has the same Azure subscription as the Device Manager service</span></span>

1. <span data-ttu-id="e35fb-203">Keresse meg az Eszközkezelő szolgáltatás, válassza ki, és kattintson rá duplán.</span><span class="sxs-lookup"><span data-stu-id="e35fb-203">Navigate to your Device Manager service, select and double-click it.</span></span> <span data-ttu-id="e35fb-204">Ekkor megnyílik a **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="e35fb-204">This opens the **Overview** blade.</span></span>
2. <span data-ttu-id="e35fb-205">Válassza ki **Tárfiók hitelesítő adatainak** belül a **konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e35fb-205">Select **Storage account credentials** within the **Configuration** section.</span></span>
3. <span data-ttu-id="e35fb-206">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="e35fb-206">Click **Add**.</span></span>
4. <span data-ttu-id="e35fb-207">Az a **tárfiók hozzáadása** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e35fb-207">In the **Add a storage account** blade, do the following:</span></span>
   
    1. <span data-ttu-id="e35fb-208">A **előfizetés**, jelölje be **aktuális**.</span><span class="sxs-lookup"><span data-stu-id="e35fb-208">For **Subscription**, select **Current**.</span></span>
   
    2. <span data-ttu-id="e35fb-209">Adja meg az Azure storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="e35fb-209">Provide the name of your Azure storage account.</span></span>
   
    3. <span data-ttu-id="e35fb-210">Válassza ki **engedélyezése** a StorSimple eszköz és a felhő közötti hálózati kommunikációhoz biztonságos csatornát létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e35fb-210">Select **Enable** to create a secure channel for network communication between your StorSimple device and the cloud.</span></span> <span data-ttu-id="e35fb-211">Válassza ki **letiltása** csak akkor, ha magánfelhőben tevékenykedik.</span><span class="sxs-lookup"><span data-stu-id="e35fb-211">Select **Disable** only if you are operating within a private cloud.</span></span>
   
    4. <span data-ttu-id="e35fb-212">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="e35fb-212">Click **Add**.</span></span> <span data-ttu-id="e35fb-213">Értesítés jelenik meg a tárfiók sikeres létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="e35fb-213">You are notified after the storage account is successfully created.</span></span><br></br>
   
     ![Adja hozzá egy meglévő storage-fiók hitelesítő adatait](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a><span data-ttu-id="e35fb-215">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="e35fb-215">Next step</span></span>

<span data-ttu-id="e35fb-216">A következő lépés, hogy osszon ki a virtuális gépek a StorSimple virtuális tömbhöz.</span><span class="sxs-lookup"><span data-stu-id="e35fb-216">The next step is to provision a virtual machine for your StorSimple Virtual Array.</span></span> <span data-ttu-id="e35fb-217">Attól függően, hogy a gazda operációs rendszer a részletes ismertetését lásd:</span><span class="sxs-lookup"><span data-stu-id="e35fb-217">Depending on your host operating system, see the detailed instructions in:</span></span>

* [<span data-ttu-id="e35fb-218">A Hyper-V egy StorSimple virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="e35fb-218">Provision a StorSimple Virtual Array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [<span data-ttu-id="e35fb-219">Kiépíteni a StorSimple virtuális tömb VMware-ben</span><span class="sxs-lookup"><span data-stu-id="e35fb-219">Provision a StorSimple Virtual Array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md)

