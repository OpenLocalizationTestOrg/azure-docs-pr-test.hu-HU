---
title: "A StorSimple eszköz inaktiváljon |} Microsoft Docs"
description: "Ismerteti a StorSimple eszköz eltávolítása a szolgáltatás első inaktiválása és törlését is."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c000a642aa088ac80cc7077453b87e9a47f96900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a><span data-ttu-id="9a4d8-103">Inaktiválása és a StorSimple Manager szolgáltatással a StorSimple 8000 series eszköz törlése</span><span class="sxs-lookup"><span data-stu-id="9a4d8-103">Deactivate and delete a StorSimple 8000 series device via StorSimple Manager service</span></span>
## <a name="overview"></a><span data-ttu-id="9a4d8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9a4d8-104">Overview</span></span>
<span data-ttu-id="9a4d8-105">Kezdésként érdemes lehet, hogy nem működik a StorSimple eszköz (például akkor, ha cseréje vagy az eszköz frissítése, vagy ha már nem használja a StorSimple).</span><span class="sxs-lookup"><span data-stu-id="9a4d8-105">You may wish to take a StorSimple device out of service (for example, if you are replacing or upgrading your device or if you are no longer using StorSimple).</span></span> <span data-ttu-id="9a4d8-106">Ha ez a helyzet, szüksége lesz az eszköz inaktiválása, mielőtt törölné.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-106">If this is the case, you will need to deactivate the device before you can delete it.</span></span> <span data-ttu-id="9a4d8-107">Az eszköz és a megfelelő StorSimple Manager szolgáltatás közötti kapcsolat inaktiválása kiszolgálója.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-107">Deactivating severs the connection between the device and the corresponding StorSimple Manager service.</span></span> <span data-ttu-id="9a4d8-108">Ez az oktatóanyag ismerteti a StorSimple eszköz eltávolítása a szolgáltatás első inaktiválása és törlését is.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-108">This tutorial explains how to remove a StorSimple device from service by first deactivating it and then deleting it.</span></span> 

<span data-ttu-id="9a4d8-109">Ha az eszköz inaktiválása az eszközön helyileg tárolt adatokat már nem lesznek elérhetők.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-109">When you deactivate a device, any data that was stored locally on the device will no longer be accessible.</span></span> <span data-ttu-id="9a4d8-110">Csak a felhőben tárolt az eszközzel kapcsolatos adatok állíthatók helyre.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-110">Only the data associated with the device that was stored in the cloud can be recovered.</span></span>  

> [!WARNING]
> <span data-ttu-id="9a4d8-111">Az inaktiválást állandó művelet, és nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-111">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="9a4d8-112">Deaktivált eszköz nem lehet regisztrálni a StorSimple Manager szolgáltatásban, kivéve, ha először visszaáll az alapértelmezett gyári beállításokat.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-112">A deactivated device cannot be registered with the StorSimple Manager service unless it is first reset to the default factory settings.</span></span> 
> 
> <span data-ttu-id="9a4d8-113">A gyári folyamat törli az eszközön helyileg tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-113">The factory reset process deletes all the data that was stored locally on your device.</span></span> <span data-ttu-id="9a4d8-114">Ezért fontos, hogy pillanatképet egy felhőalapú, az adatok egy eszköz inaktiválása előtt.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-114">Therefore, it is essential that you take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="9a4d8-115">Ez lehetővé teszi az adatok helyreállítása egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-115">This will allow you to recover all the data at a later stage.</span></span>
> 
> 

<span data-ttu-id="9a4d8-116">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="9a4d8-116">This tutorial explains how to:</span></span>

* <span data-ttu-id="9a4d8-117">Eszköz inaktiválása és az adatok törlése</span><span class="sxs-lookup"><span data-stu-id="9a4d8-117">Deactivate a device and delete the data</span></span>
* <span data-ttu-id="9a4d8-118">Eszköz inaktiválása és az adatok megőrzése mellett</span><span class="sxs-lookup"><span data-stu-id="9a4d8-118">Deactivate a device and retain the data</span></span>

<span data-ttu-id="9a4d8-119">Azt is bemutatja, hogyan inaktiválása és törlési működik a StorSimple virtuális eszköz.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-119">It also explains how deactivation and deletion works on a StorSimple virtual device.</span></span>

> [!NOTE]
> <span data-ttu-id="9a4d8-120">A StorSimple fizikai vagy virtuális eszköz inaktiválása előtt győződjön meg arról, állítsa le vagy törölje az ügyfelek és kiszolgálók azokon az eszközökön.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-120">Before you deactivate a StorSimple physical or virtual device, make sure to stop or delete clients and hosts that depend on that device.</span></span>
> 
> 

## <a name="deactivate-and-delete-data"></a><span data-ttu-id="9a4d8-121">Inaktiválása és adatok törlése</span><span class="sxs-lookup"><span data-stu-id="9a4d8-121">Deactivate and delete data</span></span>
<span data-ttu-id="9a4d8-122">Ha szeretné az eszköz teljes törlése, és nem szeretné, hogy az eszközön lévő adatok megőrzése mellett, majd hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-122">If you are interested in deleting the device completely and do not want to retain the data on the device, then complete the following steps.</span></span>

#### <a name="to-deactivate-the-device-and-delete-the-data"></a><span data-ttu-id="9a4d8-123">Az eszköz inaktiválásához és az adatok törlése</span><span class="sxs-lookup"><span data-stu-id="9a4d8-123">To deactivate the device and delete the data</span></span>
1. <span data-ttu-id="9a4d8-124">A eszköz inaktiválása előtt törölnie kell az összes kötet az eszközhöz társított tárolók (és a kötetek).</span><span class="sxs-lookup"><span data-stu-id="9a4d8-124">Prior to deactivating a device, you must delete all the volume containers (and the volumes) associated with the device.</span></span> <span data-ttu-id="9a4d8-125">Kötettárolók törölheti csak a társított biztonsági másolatok törlését követően.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-125">You can delete volume containers only after you have deleted the associated backups.</span></span>
2. <span data-ttu-id="9a4d8-126">Az eszköz az alábbiak szerint inaktiválása:</span><span class="sxs-lookup"><span data-stu-id="9a4d8-126">Deactivate the device as follows:</span></span>
   
   1. <span data-ttu-id="9a4d8-127">A StorSimple Manager szolgáltatás **eszközök** lapon, válassza ki az eszköz inaktiválása, és a lap alján kattintson a kívánt **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-127">On the StorSimple Manager service **Devices** page, select the device that you wish to deactivate and, at the bottom of the page, click **Deactivate**.</span></span>
   2. <span data-ttu-id="9a4d8-128">Ekkor megjelenik egy megerősítő üzenet.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-128">A confirmation message will appear.</span></span> <span data-ttu-id="9a4d8-129">Kattintson a **Igen** folytatja.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-129">Click **Yes** to continue.</span></span> <span data-ttu-id="9a4d8-130">Az inaktiválás folyamat elindítja és néhány percet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-130">The deactivate process will start and take a few minutes to complete.</span></span>
3. <span data-ttu-id="9a4d8-131">Az inaktiválást, után törölheti az eszköz teljesen.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-131">After deactivation, you can delete the device completely.</span></span> <span data-ttu-id="9a4d8-132">Egy eszköz törlése eltávolítja a listán szereplő eszközök kapcsolódik a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-132">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="9a4d8-133">A szolgáltatás már nem kezelhetők a törölt eszköz.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-133">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="9a4d8-134">Az alábbi lépések segítségével az eszköz törlése:</span><span class="sxs-lookup"><span data-stu-id="9a4d8-134">Use the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="9a4d8-135">A StorSimple Manager szolgáltatás **eszközök** lapon, válassza ki a törölni kívánt inaktív eszközök.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-135">On the StorSimple Manager service **Devices** page, select a deactivated device that you wish to delete.</span></span>
   2. <span data-ttu-id="9a4d8-136">Az oldalon alján kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-136">On the bottom on the page, click **Delete**.</span></span>
   3. <span data-ttu-id="9a4d8-137">Megerősítést kér bekéri.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-137">You will be prompted for confirmation.</span></span> <span data-ttu-id="9a4d8-138">Kattintson a **Igen** folytatja.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-138">Click **Yes** to continue.</span></span>
      
      <span data-ttu-id="9a4d8-139">Törli az eszköz néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-139">It may take a few minutes for the device to be deleted.</span></span>

## <a name="deactivate-and-retain-data"></a><span data-ttu-id="9a4d8-140">Inaktiválása és adatainak megőrzése</span><span class="sxs-lookup"><span data-stu-id="9a4d8-140">Deactivate and retain data</span></span>
<span data-ttu-id="9a4d8-141">Ha az eszköz törlése érdekelt, de szeretné megőrizni az adatokat, majd hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-141">If you are interested in deleting the device but want to retain the data, then complete the following steps.</span></span>

#### <a name="to-deactivate-a-device-and-retain-the-data"></a><span data-ttu-id="9a4d8-142">Eszköz inaktiválása és az adatok megőrzése mellett</span><span class="sxs-lookup"><span data-stu-id="9a4d8-142">To deactivate a device and retain the data</span></span>
1. <span data-ttu-id="9a4d8-143">Az eszköz inaktiválásához.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-143">Deactivate the device.</span></span> <span data-ttu-id="9a4d8-144">A kötet tárolók és az eszköz a pillanatképek megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-144">All the volume containers and the snapshots of the device will remain.</span></span>
   
   1. <span data-ttu-id="9a4d8-145">A StorSimple Manager szolgáltatás **eszközök** lapon, válassza ki az eszköz inaktiválása, és a lap alján kattintson a kívánt **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-145">On the StorSimple Manager service **Devices** page, select the device that you wish to deactivate and, at the bottom of the page, click **Deactivate**.</span></span>
   2. <span data-ttu-id="9a4d8-146">Ekkor megjelenik egy megerősítő üzenet.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-146">A confirmation message will appear.</span></span> <span data-ttu-id="9a4d8-147">Kattintson a **Igen** folytatja.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-147">Click **Yes** to continue.</span></span> <span data-ttu-id="9a4d8-148">Az inaktiválás folyamat elindítja és néhány percet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-148">The deactivate process will start and take a few minutes to complete.</span></span>
2. <span data-ttu-id="9a4d8-149">Most átveheti a kötet-tárolók és a társított pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-149">You can now fail over the volume containers and the associated snapshots.</span></span> <span data-ttu-id="9a4d8-150">Az eljárások, nyissa meg a [a StorSimple eszköz feladatátvétel és a katasztrófa utáni helyreállítás](storsimple-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="9a4d8-150">For procedures, go to [Failover and disaster recovery for your StorSimple device](storsimple-device-failover-disaster-recovery.md).</span></span>
3. <span data-ttu-id="9a4d8-151">Az inaktiválást és a feladatátvétel után teljesen törölheti az eszközön.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-151">After deactivation and failover, you can delete the device completely.</span></span> <span data-ttu-id="9a4d8-152">Egy eszköz törlése eltávolítja a listán szereplő eszközök kapcsolódik a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-152">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="9a4d8-153">A szolgáltatás már nem kezelhetők a törölt eszköz.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-153">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="9a4d8-154">Hajtsa végre az alábbi lépéseket, hogy törli az eszközt:</span><span class="sxs-lookup"><span data-stu-id="9a4d8-154">Complete the following steps to delete the device:</span></span>
   
   1. <span data-ttu-id="9a4d8-155">A StorSimple Manager szolgáltatás **eszközök** lapon, válassza ki a törölni kívánt inaktív eszközök.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-155">On the StorSimple Manager service **Devices** page, select a deactivated device that you wish to delete.</span></span>
   2. <span data-ttu-id="9a4d8-156">Az oldalon alján kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-156">On the bottom on the page, click **Delete**.</span></span>
   3. <span data-ttu-id="9a4d8-157">Megerősítést kér bekéri.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-157">You will be prompted for confirmation.</span></span> <span data-ttu-id="9a4d8-158">Kattintson a **Igen** folytatja.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-158">Click **Yes** to continue.</span></span>
      
      <span data-ttu-id="9a4d8-159">Törli az eszköz néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-159">It may take a few minutes for the device to be deleted.</span></span>

## <a name="deactivate-and-delete-a-virtual-device"></a><span data-ttu-id="9a4d8-160">A virtuális eszköz inaktiváljon</span><span class="sxs-lookup"><span data-stu-id="9a4d8-160">Deactivate and delete a virtual device</span></span>
<span data-ttu-id="9a4d8-161">A StorSimple virtuális eszköz inaktiválása felszabadítja a a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-161">For a StorSimple virtual device, deactivation deallocates the virtual machine.</span></span> <span data-ttu-id="9a4d8-162">Törölheti a virtuális gép és a kiépítésekor létrejött erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-162">You can then delete the virtual machine and the resources created when it was provisioned.</span></span> <span data-ttu-id="9a4d8-163">A virtuális eszközt az inaktiválás után már nem lehet a korábbi állapotára visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-163">After the virtual device is deactivated, it cannot be restored to its previous state.</span></span> 

<span data-ttu-id="9a4d8-164">Az inaktiválást eredményezi, a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="9a4d8-164">Deactivation results in the following actions:</span></span>

* <span data-ttu-id="9a4d8-165">A StorSimple virtuális eszközt a rendszer eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-165">The StorSimple virtual device is removed.</span></span>
* <span data-ttu-id="9a4d8-166">Eltávolítja az OSDisk és az adatlemezek a StorSimple virtuális eszköz létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-166">The OSDisk and Data Disks created for the StorSimple virtual device are removed.</span></span>
* <span data-ttu-id="9a4d8-167">Az üzemeltetett szolgáltatás és a kiépítés során létrehozott virtuális hálózat megmarad.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-167">The Hosted Service and Virtual Network that were created during provisioning are retained.</span></span> <span data-ttu-id="9a4d8-168">Ha nem használ ezeket az entitásokat, akkor manuálisan kell törölnie őket.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-168">If you are not using these entities, you should delete them manually.</span></span>
* <span data-ttu-id="9a4d8-169">A StorSimple virtuális eszköz által létrehozott felhőalapú pillanatfelvételek megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="9a4d8-169">Cloud snapshots created by the StorSimple virtual device are retained.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a4d8-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a4d8-170">Next steps</span></span>
* <span data-ttu-id="9a4d8-171">A deaktivált eszköz visszaállítása a gyári beállításokat, nyissa meg a [visszaállítani az eszközt a gyári alapértelmezett beállításokra](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="9a4d8-171">To restore the deactivated device to factory defaults, go to [Reset the device to factory default settings](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
* <span data-ttu-id="9a4d8-172">Technikai támogatásért [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="9a4d8-172">For technical assistance, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="9a4d8-173">A StorSimple Manager szolgáltatással kapcsolatos további tudnivalókért keresse fel [felügyelete a StorSimple eszközt a StorSimple Manager szolgáltatás segítségével](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="9a4d8-173">To learn more about how to use the StorSimple Manager service, go to [Use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span> 

