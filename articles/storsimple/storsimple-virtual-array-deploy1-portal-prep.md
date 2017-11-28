---
title: "a StorSimple virtuális tömb aaaPortal előkészítő |} Microsoft Docs"
description: "Első oktatóanyagot toodeploy StorSimple virtuális tömb magában foglalja a hello Azure-portálon előkészítése"
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
ms.openlocfilehash: 5332b235e7296a9274f2e7dafcdf72f4b9cdadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-hello-azure-portal"></a><span data-ttu-id="29f93-103">A StorSimple virtuális tömb telepítése – hello Azure-portálon előkészítése</span><span class="sxs-lookup"><span data-stu-id="29f93-103">Deploy StorSimple Virtual Array - Prepare hello Azure portal</span></span>

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a><span data-ttu-id="29f93-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="29f93-104">Overview</span></span>

<span data-ttu-id="29f93-105">Ez az hello első cikkében hello sorozat szükséges az üzembehelyezési oktatóanyagok toocompletely egy fájl vagy hello Resource Manager modellt használja az iSCSI-kiszolgáló telepítése a virtuális tömbjét.</span><span class="sxs-lookup"><span data-stu-id="29f93-105">This is hello first article in hello series of deployment tutorials required toocompletely deploy your virtual array as a file server or an iSCSI server using hello Resource Manager model.</span></span> <span data-ttu-id="29f93-106">Ez a cikk ismerteti a hello előkészületek toocreate, és konfigurálása a StorSimple Device Manager szolgáltatás előzetes tooprovisioning virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="29f93-106">This article describes hello preparation required toocreate and configure your StorSimple Device Manager service prior tooprovisioning a virtual array.</span></span> <span data-ttu-id="29f93-107">Ez a cikk is ki tooa üzembehelyezési konfigurációs ellenőrzőlista és konfigurációs Előfeltételek hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="29f93-107">This article also links out tooa deployment configuration checklist and configuration prerequisites.</span></span>

<span data-ttu-id="29f93-108">Rendszergazdai jogosultságok toocomplete hello beállítása és konfigurációja folyamat van szüksége.</span><span class="sxs-lookup"><span data-stu-id="29f93-108">You need administrator privileges toocomplete hello setup and configuration process.</span></span> <span data-ttu-id="29f93-109">Ajánlott áttekinteni hello üzembehelyezési konfigurációs ellenőrzőlista megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="29f93-109">We recommend that you review hello deployment configuration checklist before you begin.</span></span> <span data-ttu-id="29f93-110">hello portál előkészítése kisebb, mint 10 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="29f93-110">hello portal preparation takes less than 10 minutes.</span></span>

<span data-ttu-id="29f93-111">Ebben a cikkben közzétett hello információ toohello és központi telepítése a StorSimple virtuális tömbök hello Azure-portál a Microsoft Azure Government felhő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="29f93-111">hello information published in this article applies toohello deployment of StorSimple Virtual Arrays in hello Azure portal and Microsoft Azure Government Cloud.</span></span>

### <a name="get-started"></a><span data-ttu-id="29f93-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="29f93-112">Get started</span></span>
<span data-ttu-id="29f93-113">hello telepítési munkafolyamat hello portal előkészítése, a virtualizált környezetben egy virtuális tömb kiépítés és hello telepítést áll.</span><span class="sxs-lookup"><span data-stu-id="29f93-113">hello deployment workflow consists of preparing hello portal, provisioning a virtual array in your virtualized environment, and completing hello setup.</span></span> <span data-ttu-id="29f93-114">tooget hello StorSimple virtuális tömb központi telepítésben is egy fájl vagy iSCSI-kiszolgáló használatába, a következő táblázatos erőforrások toorefer toohello kell.</span><span class="sxs-lookup"><span data-stu-id="29f93-114">tooget started with hello StorSimple Virtual Array deployment as a file server or an iSCSI server, you need toorefer toohello following tabulated resources.</span></span>

#### <a name="deployment-articles"></a><span data-ttu-id="29f93-115">Telepítési cikkek</span><span class="sxs-lookup"><span data-stu-id="29f93-115">Deployment articles</span></span>

<span data-ttu-id="29f93-116">toodeploy a StorSimple virtuális tömb, tekintse meg a következő cikkek feladatütemezési előírt hello toohello.</span><span class="sxs-lookup"><span data-stu-id="29f93-116">toodeploy your StorSimple Virtual Array, refer toohello following articles in hello prescribed sequence.</span></span>

| **#** | <span data-ttu-id="29f93-117">**Ebben a lépésben**</span><span class="sxs-lookup"><span data-stu-id="29f93-117">**In this step**</span></span> | <span data-ttu-id="29f93-118">**Ezt megteheti...**</span><span class="sxs-lookup"><span data-stu-id="29f93-118">**You do this …**</span></span> | <span data-ttu-id="29f93-119">**És használja ezeket a dokumentumokat.**</span><span class="sxs-lookup"><span data-stu-id="29f93-119">**And use these documents.**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="29f93-120">1.</span><span class="sxs-lookup"><span data-stu-id="29f93-120">1.</span></span> |<span data-ttu-id="29f93-121">**Azure-portálon hello beállítása**</span><span class="sxs-lookup"><span data-stu-id="29f93-121">**Set up hello Azure portal**</span></span> |<span data-ttu-id="29f93-122">Létrehozása és konfigurálása a StorSimple Device Manager szolgáltatás előzetes tooprovisioning egy StorSimple virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="29f93-122">Create and configure your StorSimple Device Manager service prior tooprovisioning a StorSimple Virtual Array.</span></span> |[<span data-ttu-id="29f93-123">Hello portal előkészítése</span><span class="sxs-lookup"><span data-stu-id="29f93-123">Prepare hello portal</span></span>](storsimple-virtual-array-deploy1-portal-prep.md) |
| <span data-ttu-id="29f93-124">2.</span><span class="sxs-lookup"><span data-stu-id="29f93-124">2.</span></span> |<span data-ttu-id="29f93-125">**Kiépítés hello virtuális tömb**</span><span class="sxs-lookup"><span data-stu-id="29f93-125">**Provision hello Virtual Array**</span></span> |<span data-ttu-id="29f93-126">A Hyper-V kiépíteni, és csatlakozzon a StorSimple virtuális tömb tooa állomás operációs rendszert futtató Hyper-V a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszeren.</span><span class="sxs-lookup"><span data-stu-id="29f93-126">For Hyper-V, provision and connect tooa StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <br></br> <br></br> <span data-ttu-id="29f93-127">A VMware kiépíteni, és csatlakozzon a tooa StorSimple virtuális tömb a gazdagéphez, VMware ESXi 5.5 rendszerű vagy újabb rendszeren.</span><span class="sxs-lookup"><span data-stu-id="29f93-127">For VMware, provision and connect tooa StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span><br></br> |[<span data-ttu-id="29f93-128">A Hyper-V egy virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="29f93-128">Provision a virtual array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [<span data-ttu-id="29f93-129">VMware-ben a virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="29f93-129">Provision a virtual array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md) |
| <span data-ttu-id="29f93-130">3.</span><span class="sxs-lookup"><span data-stu-id="29f93-130">3.</span></span> |<span data-ttu-id="29f93-131">**Virtuális tömb hello beállítása**</span><span class="sxs-lookup"><span data-stu-id="29f93-131">**Set up hello Virtual Array**</span></span> |<span data-ttu-id="29f93-132">A fájlkiszolgáló hajtsa végre a kezdeti beállítás regisztrálása a StorSimple fájlkiszolgáló és hello eszköz beállításának befejezése.</span><span class="sxs-lookup"><span data-stu-id="29f93-132">For your file server, perform initial setup, register your StorSimple file server, and complete hello device setup.</span></span> <span data-ttu-id="29f93-133">SMB-megosztások majd oszthat.</span><span class="sxs-lookup"><span data-stu-id="29f93-133">You can then provision SMB shares.</span></span> <br></br> <br></br> <span data-ttu-id="29f93-134">Az iSCSI-kiszolgáló hajtsa végre a kezdeti beállítás, a StorSimple iSCSI kiszolgáló regisztrálásához és hello eszköz beállításának befejezése.</span><span class="sxs-lookup"><span data-stu-id="29f93-134">For your iSCSI server, perform initial setup, register your StorSimple iSCSI server, and complete hello device setup.</span></span> <span data-ttu-id="29f93-135">Az iSCSI-kötetek majd oszthat.</span><span class="sxs-lookup"><span data-stu-id="29f93-135">You can then provision iSCSI volumes.</span></span> |[<span data-ttu-id="29f93-136">Fájlkiszolgáló virtuális tömb beállítása</span><span class="sxs-lookup"><span data-stu-id="29f93-136">Set up virtual array as file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[<span data-ttu-id="29f93-137">Állítsa be a tömb virtuális iSCSI-kiszolgálóként</span><span class="sxs-lookup"><span data-stu-id="29f93-137">Set up virtual array as iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md) |

<span data-ttu-id="29f93-138">Most már megkezdheti a tooset mentése hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="29f93-138">You can now begin tooset up hello Azure portal.</span></span>

## <a name="configuration-checklist"></a><span data-ttu-id="29f93-139">Konfigurációs ellenőrzőlista</span><span class="sxs-lookup"><span data-stu-id="29f93-139">Configuration checklist</span></span>

<span data-ttu-id="29f93-140">hello konfigurációs ellenőrzőlista leírja, hogy a StorSimple virtuális tömb hello szoftver konfigurálása előtt szükséges toocollect hello információkat.</span><span class="sxs-lookup"><span data-stu-id="29f93-140">hello configuration checklist describes hello information that you need toocollect before you configure hello software on your StorSimple Virtual Array.</span></span> <span data-ttu-id="29f93-141">Ezek az információk idő segítségével előre érdekében hello folyamat hello StorSimple eszközt az adott környezetben történő telepítésének előkészítése.</span><span class="sxs-lookup"><span data-stu-id="29f93-141">Preparing this information ahead of time helps streamline hello process of deploying hello StorSimple device in your environment.</span></span> <span data-ttu-id="29f93-142">Attól függően, hogy a StorSimple virtuális tömb van telepítve egy fájl vagy iSCSI-kiszolgáló, egyike szükséges a következő ellenőrzőlisták hello.</span><span class="sxs-lookup"><span data-stu-id="29f93-142">Depending upon whether your StorSimple Virtual Array is deployed as a file server or an iSCSI server, you need one of hello following checklists.</span></span>

* <span data-ttu-id="29f93-143">Töltse le a hello [StorSimple virtuális tömb fájl kiszolgáló konfigurációs ellenőrzőlista](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="29f93-143">Download hello [StorSimple Virtual Array File Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span></span>
* <span data-ttu-id="29f93-144">Töltse le a hello [StorSimple virtuális tömb iSCSI kiszolgáló konfigurációs ellenőrzőlista](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="29f93-144">Download hello [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29f93-145">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="29f93-145">Prerequisites</span></span>

<span data-ttu-id="29f93-146">Itt megtalálja az Ön hello konfigurációs előfeltételeit a StorSimple eszköz kezelő szolgáltatás, a StorSimple virtuális tömb és hello adatközponti hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="29f93-146">Here you find hello configuration prerequisites for your StorSimple Device Manager service, your StorSimple Virtual Array, and hello datacenter network.</span></span>

### <a name="for-hello-storsimple-device-manager-service"></a><span data-ttu-id="29f93-147">A StorSimple eszköz Manager szolgáltatás hello</span><span class="sxs-lookup"><span data-stu-id="29f93-147">For hello StorSimple Device Manager service</span></span>

<span data-ttu-id="29f93-148">Mielőtt hozzákezd, győződjön meg az alábbiakról:</span><span class="sxs-lookup"><span data-stu-id="29f93-148">Before you begin, make sure that:</span></span>

* <span data-ttu-id="29f93-149">Rendelkezik Microsoft-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="29f93-149">You have your Microsoft account with access credentials.</span></span>
* <span data-ttu-id="29f93-150">Rendelkezik Microsoft Azure Storage-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="29f93-150">You have your Microsoft Azure storage account with access credentials.</span></span>
* <span data-ttu-id="29f93-151">A Microsoft Azure-előfizetés engedélyezni kell a StorSimple eszköz Manager szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="29f93-151">Your Microsoft Azure subscription should be enabled for StorSimple Device Manager service.</span></span>

### <a name="for-hello-storsimple-virtual-array"></a><span data-ttu-id="29f93-152">A StorSimple virtuális tömb hello</span><span class="sxs-lookup"><span data-stu-id="29f93-152">For hello StorSimple Virtual Array</span></span>

<span data-ttu-id="29f93-153">Egy virtuális tömb telepítése előtt győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="29f93-153">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="29f93-154">Hozzáférés tooa állomás rendszer fut a Windows Server 2008 R2 vagy újabb Hyper-V vagy VMware (ESXi 5.5-ös vagy újabb), amely használt tooa eszköz kiépítése.</span><span class="sxs-lookup"><span data-stu-id="29f93-154">You have access tooa host system running Hyper-V on Windows Server 2008 R2 or later or VMware (ESXi 5.5 or later) that can be used tooa provision a device.</span></span>
* <span data-ttu-id="29f93-155">hello gazdarendszer képes toodedicate a virtuális tömb a következő erőforrások tooprovision hello:</span><span class="sxs-lookup"><span data-stu-id="29f93-155">hello host system is able toodedicate hello following resources tooprovision your virtual array:</span></span>
  
  * <span data-ttu-id="29f93-156">Legalább 4 mag.</span><span class="sxs-lookup"><span data-stu-id="29f93-156">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="29f93-157">Legalább 8 GB RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="29f93-157">At least 8 GB of RAM.</span></span> <span data-ttu-id="29f93-158">Ha azt tervezi, tooconfigure hello virtuális tömb fájlkiszolgálóként, a 8 GB 2 millió fájlokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="29f93-158">If you plan tooconfigure hello virtual array as file server, 8 GB supports 2 million files.</span></span> <span data-ttu-id="29f93-159">16 GB RAM toosupport 2 – 4 millió fájlok van szüksége.</span><span class="sxs-lookup"><span data-stu-id="29f93-159">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
  * <span data-ttu-id="29f93-160">Egy hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="29f93-160">One network interface.</span></span>
  * <span data-ttu-id="29f93-161">500 GB-os virtuális lemez Rendszeradatok.</span><span class="sxs-lookup"><span data-stu-id="29f93-161">A 500 GB virtual disk for system data.</span></span>

### <a name="for-hello-datacenter-network"></a><span data-ttu-id="29f93-162">Hello adatközpont-hálózat</span><span class="sxs-lookup"><span data-stu-id="29f93-162">For hello datacenter network</span></span>

<span data-ttu-id="29f93-163">Mielőtt hozzákezd, győződjön meg az alábbiakról:</span><span class="sxs-lookup"><span data-stu-id="29f93-163">Before you begin, make sure that:</span></span>

* <span data-ttu-id="29f93-164">Az Adatközpont hálózatának hello hello a StorSimple eszköz hálózatkezelési követelményei szerint van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="29f93-164">hello network in your datacenter is configured as per hello networking requirements for your StorSimple device.</span></span> <span data-ttu-id="29f93-165">További információkért lásd: hello [StorSimple virtuális tömb rendszerkövetelmények](storsimple-ova-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="29f93-165">For more information, see hello [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).</span></span>
* <span data-ttu-id="29f93-166">A StorSimple virtuális tömb nem rendelkezik egy dedikált 5 MB/s internetes sávszélességet (vagy több) mindenkor.</span><span class="sxs-lookup"><span data-stu-id="29f93-166">Your StorSimple Virtual Array has a dedicated 5 Mbps Internet bandwidth (or more) available at all times.</span></span> <span data-ttu-id="29f93-167">A sávszélesség nem lehet megosztva egyéb alkalmazásokkal.</span><span class="sxs-lookup"><span data-stu-id="29f93-167">This bandwidth should not be shared with any other applications.</span></span>

## <a name="step-by-step-preparation"></a><span data-ttu-id="29f93-168">Részletes előkészítése</span><span class="sxs-lookup"><span data-stu-id="29f93-168">Step-by-step preparation</span></span>

<span data-ttu-id="29f93-169">A következő lépésről tooprepare hello a portál használni hello StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="29f93-169">Use hello following step-by-step instructions tooprepare your portal for hello StorSimple Device Manager service.</span></span>

## <a name="step-1-create-a-new-service"></a><span data-ttu-id="29f93-170">1. lépés: Új szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="29f93-170">Step 1: Create a new service</span></span>

<span data-ttu-id="29f93-171">Egyetlen példány futhat hello StorSimple Device Manager szolgáltatás több StorSimple virtuális tömbök kezelheti.</span><span class="sxs-lookup"><span data-stu-id="29f93-171">A single instance of hello StorSimple Device Manager service can manage multiple StorSimple Virtual Arrays.</span></span> <span data-ttu-id="29f93-172">Hajtsa végre a következő lépéseket toocreate hello StorSimple Device Manager szolgáltatás egy példánya hello.</span><span class="sxs-lookup"><span data-stu-id="29f93-172">Perform hello following steps toocreate an instance of hello StorSimple Device Manager service.</span></span> <span data-ttu-id="29f93-173">Ha egy meglévő StorSimple Device Manager szolgáltatás toomanage a virtuális tömbök, kihagyhatja ezt a lépést, és nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="29f93-173">If you have an existing StorSimple Device Manager service toomanage your virtual arrays, skip this step and go too[Step 2: Get hello service registration key](#step-2-get-the-service-registration-key).</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> <span data-ttu-id="29f93-174">Ha nem engedélyezte a tárfiók automatikus létrehozását hello a szolgáltatással, szüksége lesz a toocreate legalább egy tárfiókot, miután sikeresen létrehozott egy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="29f93-174">If you did not enable hello automatic creation of a storage account with your service, you will need toocreate at least one storage account after you have successfully created a service.</span></span>
> 
> * <span data-ttu-id="29f93-175">Ha nem hozott létre egy tárfiókot automatikusan, nyissa meg túl[hello szolgáltatást egy új tárfiók konfigurálása](#optional-step-configure-a-new-storage-account-for-the-service) részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="29f93-175">If you did not create a storage account automatically, go too[Configure a new storage account for hello service](#optional-step-configure-a-new-storage-account-for-the-service) for detailed instructions.</span></span>
> * <span data-ttu-id="29f93-176">Ha engedélyezte a tárfiók automatikus létrehozását hello, nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="29f93-176">If you enabled hello automatic creation of a storage account, go too[Step 2: Get hello service registration key](#step-2-get-the-service-registration-key).</span></span>
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a><span data-ttu-id="29f93-177">2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése</span><span class="sxs-lookup"><span data-stu-id="29f93-177">Step 2: Get hello service registration key</span></span>

<span data-ttu-id="29f93-178">Miután hello StorSimple Device Manager szolgáltatás fut, akkor tooget hello Szolgáltatásregisztrációs kulcs.</span><span class="sxs-lookup"><span data-stu-id="29f93-178">After hello StorSimple Device Manager service is up and running, you will need tooget hello service registration key.</span></span> <span data-ttu-id="29f93-179">Ez a kulcs használt tooregister, és csatlakoztathatja StorSimple eszközét hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="29f93-179">This key is used tooregister and connect your StorSimple device with hello service.</span></span>

<span data-ttu-id="29f93-180">Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29f93-180">Perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> <span data-ttu-id="29f93-181">hello Szolgáltatásregisztrációs kulcs használt tooregister összes hello StorSimple Device Manager eszközök, amelyeket a StorSimple Device Manager szolgáltatással tooregister.</span><span class="sxs-lookup"><span data-stu-id="29f93-181">hello service registration key is used tooregister all hello StorSimple Device Manager devices that need tooregister with your StorSimple Device Manager service.</span></span>
> 
> 

## <a name="step-3-download-hello-virtual-array-image"></a><span data-ttu-id="29f93-182">3. lépés: Hello virtuális tömb lemezkép letöltése</span><span class="sxs-lookup"><span data-stu-id="29f93-182">Step 3: Download hello virtual array image</span></span>

<span data-ttu-id="29f93-183">Miután hello szolgáltatás regisztrációs kulcsának, a gazdarendszer toodownload hello megfelelő virtuális tömb kép tooprovision virtuális tömb lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="29f93-183">After you have hello service registration key, you will need toodownload hello appropriate virtual array image tooprovision a virtual array on your host system.</span></span> <span data-ttu-id="29f93-184">hello virtuális tömb lemezképek operációs rendszer adott és tölthető le: első lépések oldaláról hello hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="29f93-184">hello virtual array images are operating system specific and can be downloaded from hello Quick Start page in hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29f93-185">a StorSimple virtuális tömb hello futó hello szoftver csak hello StorSimple Device Manager szolgáltatás is használhatók.</span><span class="sxs-lookup"><span data-stu-id="29f93-185">hello software running on hello StorSimple Virtual Array may only be used with hello StorSimple Device Manager service.</span></span>
> 
> 

<span data-ttu-id="29f93-186">Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29f93-186">Perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="tooget-hello-virtual-array-image"></a><span data-ttu-id="29f93-187">tooget hello virtuális tömb kép</span><span class="sxs-lookup"><span data-stu-id="29f93-187">tooget hello virtual array image</span></span>

1. <span data-ttu-id="29f93-188">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29f93-188">Sign into hello [Azure portal](https://portal.azure.com/).</span></span> 
2. <span data-ttu-id="29f93-189">Hello Azure-portálon, kattintson **Tallózás > StorSimple eszköz kezelői**.</span><span class="sxs-lookup"><span data-stu-id="29f93-189">In hello Azure portal, click **Browse > StorSimple Device Managers**.</span></span>
3. <span data-ttu-id="29f93-190">Válasszon ki egy meglévő StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="29f93-190">Select an existing StorSimple Device Manager service.</span></span> <span data-ttu-id="29f93-191">A hello **StorSimple Device Manager** panelen kattintson a **gyors üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="29f93-191">In hello **StorSimple Device Manager** blade, click **Quick Start**.</span></span> 
4. <span data-ttu-id="29f93-192">Kattintson a hello hivatkozás megfelelő toohello képe, amelyet a Microsoft Download Center hello toodownload.</span><span class="sxs-lookup"><span data-stu-id="29f93-192">Click hello link corresponding toohello image that you want toodownload from hello Microsoft Download Center.</span></span> <span data-ttu-id="29f93-193">hello kép fájlok körülbelül 4.8 GB.</span><span class="sxs-lookup"><span data-stu-id="29f93-193">hello image files are approximately 4.8 GB.</span></span>
   
   * <span data-ttu-id="29f93-194">A Hyper-V a Windows Server 2012 és újabb verziók VHDX</span><span class="sxs-lookup"><span data-stu-id="29f93-194">VHDX for Hyper-V on Windows Server 2012 and later</span></span>
   * <span data-ttu-id="29f93-195">Virtuális merevlemez Hyper-v a Windows Server 2008 R2 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="29f93-195">VHD for Hyper-V on Windows Server 2008 R2 and later</span></span>
   * <span data-ttu-id="29f93-196">Vmdk-fájl, a VMWare ESXi 5.5 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="29f93-196">VMDK for VMWare ESXi 5.5 and later</span></span>
5. <span data-ttu-id="29f93-197">Töltse le és csomagolja ki a hello fájl tooa helyi meghajtó, ahol hello tömörítetlen fájl megtalálható-e a Megjegyzés elvégzése.</span><span class="sxs-lookup"><span data-stu-id="29f93-197">Download and unzip hello file tooa local drive, making a note of where hello unzipped file is located.</span></span>

## <a name="optional-step-configure-a-new-storage-account-for-hello-service"></a><span data-ttu-id="29f93-198">Nem kötelező lépés: hello szolgáltatást egy új tárfiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="29f93-198">Optional step: Configure a new storage account for hello service</span></span>

<span data-ttu-id="29f93-199">Ez a lépés nem kötelező, és csak akkor, ha a szolgáltatással nem engedélyezte a tárfiók automatikus létrehozását hello kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="29f93-199">This step is optional and should be performed only if you did not enable hello automatic creation of a storage account with your service.</span></span>

<span data-ttu-id="29f93-200">Ha Azure-tárfiók más régióban toocreate van szüksége, tekintse meg [hogyan toocreate tárfiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) részletes útmutatásait.</span><span class="sxs-lookup"><span data-stu-id="29f93-200">If you need toocreate an Azure storage account in a different region, see [How toocreate a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for step-by-step instructions.</span></span>

<span data-ttu-id="29f93-201">Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://ms.portal.azure.com/) a hello StorSimple Device Manager szolgáltatás lapján tooadd egy meglévő Microsoft Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="29f93-201">Perform hello following steps in hello [Azure portal](https://ms.portal.azure.com/) on hello StorSimple Device Manager service page tooadd an existing Microsoft Azure storage account.</span></span>

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a><span data-ttu-id="29f93-202">a tárolási fiók hitelesítő adatait, amelynek tooadd hello hello Eszközkezelő szolgáltatásként azonos Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="29f93-202">tooadd a storage account credential that has hello same Azure subscription as hello Device Manager service</span></span>

1. <span data-ttu-id="29f93-203">Keresse meg a tooyour Eszközkezelő szolgáltatás, válassza ki, és kattintson rá duplán.</span><span class="sxs-lookup"><span data-stu-id="29f93-203">Navigate tooyour Device Manager service, select and double-click it.</span></span> <span data-ttu-id="29f93-204">Ekkor megnyílik a hello **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="29f93-204">This opens hello **Overview** blade.</span></span>
2. <span data-ttu-id="29f93-205">Válassza ki **Tárfiók hitelesítő adatainak** belül hello **konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="29f93-205">Select **Storage account credentials** within hello **Configuration** section.</span></span>
3. <span data-ttu-id="29f93-206">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="29f93-206">Click **Add**.</span></span>
4. <span data-ttu-id="29f93-207">A hello **tárfiók hozzáadása** panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="29f93-207">In hello **Add a storage account** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="29f93-208">A **előfizetés**, jelölje be **aktuális**.</span><span class="sxs-lookup"><span data-stu-id="29f93-208">For **Subscription**, select **Current**.</span></span>
   
    2. <span data-ttu-id="29f93-209">Adja meg az Azure storage-fiók hello nevét.</span><span class="sxs-lookup"><span data-stu-id="29f93-209">Provide hello name of your Azure storage account.</span></span>
   
    3. <span data-ttu-id="29f93-210">Válassza ki **engedélyezése** toocreate biztonságos csatornát a StorSimple eszköz és hello felhő közötti hálózati kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="29f93-210">Select **Enable** toocreate a secure channel for network communication between your StorSimple device and hello cloud.</span></span> <span data-ttu-id="29f93-211">Válassza ki **letiltása** csak akkor, ha magánfelhőben tevékenykedik.</span><span class="sxs-lookup"><span data-stu-id="29f93-211">Select **Disable** only if you are operating within a private cloud.</span></span>
   
    4. <span data-ttu-id="29f93-212">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="29f93-212">Click **Add**.</span></span> <span data-ttu-id="29f93-213">Értesítés jelenik meg a tárfiók hello sikeres létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="29f93-213">You are notified after hello storage account is successfully created.</span></span><br></br>
   
     ![Adja hozzá egy meglévő storage-fiók hitelesítő adatait](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a><span data-ttu-id="29f93-215">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="29f93-215">Next step</span></span>

<span data-ttu-id="29f93-216">következő lépés hello tooprovision a StorSimple virtuális tömb virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="29f93-216">hello next step is tooprovision a virtual machine for your StorSimple Virtual Array.</span></span> <span data-ttu-id="29f93-217">Attól függően, hogy a gazda operációs rendszer, lásd: hello részletes utasításait:</span><span class="sxs-lookup"><span data-stu-id="29f93-217">Depending on your host operating system, see hello detailed instructions in:</span></span>

* [<span data-ttu-id="29f93-218">A Hyper-V egy StorSimple virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="29f93-218">Provision a StorSimple Virtual Array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [<span data-ttu-id="29f93-219">Kiépíteni a StorSimple virtuális tömb VMware-ben</span><span class="sxs-lookup"><span data-stu-id="29f93-219">Provision a StorSimple Virtual Array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md)

