---
title: "A Microsoft Azure StorSimple virtuális tömb inaktiváljon |} Microsoft Docs"
description: "Ismerteti a StorSimple eszköz eltávolítása a szolgáltatás első inaktiválása és törlését is."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 8dea36f92b034f8c6cdb6875634848d37f4c6606
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a><span data-ttu-id="58cad-103">A StorSimple virtuális tömb inaktiváljon</span><span class="sxs-lookup"><span data-stu-id="58cad-103">Deactivate and delete a StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="58cad-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="58cad-104">Overview</span></span>

<span data-ttu-id="58cad-105">A StorSimple virtuális tömb inaktiválásakor megszakítja a kapcsolatot az eszköz és a megfelelő StorSimple Device Manager szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="58cad-105">When you deactivate a StorSimple Virtual Array, you break the connection between the device and the corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="58cad-106">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="58cad-106">This tutorial explains how to:</span></span>

* <span data-ttu-id="58cad-107">Eszköz inaktiválása</span><span class="sxs-lookup"><span data-stu-id="58cad-107">Deactivate a device</span></span> 
* <span data-ttu-id="58cad-108">Deaktivált eszköz törlése</span><span class="sxs-lookup"><span data-stu-id="58cad-108">Delete a deactivated device</span></span>

<span data-ttu-id="58cad-109">A cikkben szereplő információk kizárólag a StorSimple virtuális tömbök.</span><span class="sxs-lookup"><span data-stu-id="58cad-109">The information in this article applies to StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="58cad-110">8000 Series információkért nyissa meg a hogyan [inaktiválja vagy törölje az eszköz](storsimple-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="58cad-110">For information on 8000 series, go to how to [deactivate or delete a device](storsimple-deactivate-and-delete-device.md).</span></span>

## <a name="when-to-deactivate"></a><span data-ttu-id="58cad-111">Ha inaktiválni?</span><span class="sxs-lookup"><span data-stu-id="58cad-111">When to deactivate?</span></span>

<span data-ttu-id="58cad-112">Az inaktiválást állandó művelet, és nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="58cad-112">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="58cad-113">Deaktivált eszköz újra nem regisztrálja a StorSimple eszköz Manager szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="58cad-113">You cannot register a deactivated device with the StorSimple Device Manager service again.</span></span> <span data-ttu-id="58cad-114">Szükség lehet inaktiváljon egy StorSimple virtuális tömb a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="58cad-114">You may need to deactivate and delete a StorSimple Virtual Array in the following scenarios:</span></span>

* <span data-ttu-id="58cad-115">**A tervezett feladatátvételt** : az eszköz online állapotban, és azt tervezi, hogy áthelyezze a feladatokat az eszközt.</span><span class="sxs-lookup"><span data-stu-id="58cad-115">**Planned failover** : Your device is online and you plan to fail over your device.</span></span> <span data-ttu-id="58cad-116">Ha azt tervezi, és egy nagyobb eszközt frissítse, szükség lehet az eszköz a feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="58cad-116">If you are planning to upgrade to a larger device, you may need to fail over your device.</span></span> <span data-ttu-id="58cad-117">Miután az adatok tulajdonjoga, és a feladatátvétel befejeződött, a rendszer automatikusan törli a forráseszközt.</span><span class="sxs-lookup"><span data-stu-id="58cad-117">After the data ownership is transferred and the failover is complete, the source device is automatically deleted.</span></span>
* <span data-ttu-id="58cad-118">**Nem tervezett feladatátvétel** : az eszköz offline állapotban, és meg kell az eszközt a feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="58cad-118">**Unplanned failover** : Your device is offline and you need to fail over the device.</span></span> <span data-ttu-id="58cad-119">Ez a forgatókönyv során egy olyan vészhelyzet esetén fordulhat, ha nincs kimaradás az adatközpontban, és az elsődleges eszköz nem működik.</span><span class="sxs-lookup"><span data-stu-id="58cad-119">This scenario may occur during a disaster when there is an outage in the datacenter and your primary device is down.</span></span> <span data-ttu-id="58cad-120">Tervezi az eszközt, hogy egy másodlagos eszköz feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="58cad-120">You plan to fail over the device to a secondary device.</span></span> <span data-ttu-id="58cad-121">Miután az adatok tulajdonjoga, és a feladatátvétel befejeződött, a rendszer automatikusan törli a forráseszközt.</span><span class="sxs-lookup"><span data-stu-id="58cad-121">After the data ownership is transferred and the failover is complete, the source device is automatically deleted.</span></span>
* <span data-ttu-id="58cad-122">**Szerelje le** : az eszköz leszerelése szeretné.</span><span class="sxs-lookup"><span data-stu-id="58cad-122">**Decommission** : You want to decommission the device.</span></span> <span data-ttu-id="58cad-123">Ehhez telepítenie kell az eszköz először inaktiválja és törölje azt.</span><span class="sxs-lookup"><span data-stu-id="58cad-123">This requires you to first deactivate the device and then delete it.</span></span> <span data-ttu-id="58cad-124">Ha egy eszköz inaktiválja, már nem érheti el helyben tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="58cad-124">When you deactivate a device, you can no longer access any data that is stored locally.</span></span> <span data-ttu-id="58cad-125">Csak is elérheti, és a felhőben tárolt adatok helyreállításához.</span><span class="sxs-lookup"><span data-stu-id="58cad-125">You can only access and recover the data stored in the cloud.</span></span> <span data-ttu-id="58cad-126">Ha az eszköz adatok megőrzéséhez inaktiválás után szeretne, majd kell pillanatképet készít felhő az összes adat eszköz inaktiválása előtt.</span><span class="sxs-lookup"><span data-stu-id="58cad-126">If you plan to keep the device data after deactivation, then you should take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="58cad-127">A felhő-pillanatfelvételt segítségével visszaállíthatja az adatokat egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="58cad-127">This cloud snapshot allows you to recover all the data at a later stage.</span></span>

## <a name="deactivate-a-device"></a><span data-ttu-id="58cad-128">Eszköz inaktiválása</span><span class="sxs-lookup"><span data-stu-id="58cad-128">Deactivate a device</span></span>

<span data-ttu-id="58cad-129">Az eszköz inaktiválásához hajtsa végre a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="58cad-129">To deactivate your device, perform the following steps.</span></span>

#### <a name="to-deactivate-the-device"></a><span data-ttu-id="58cad-130">Az eszköz inaktiválása</span><span class="sxs-lookup"><span data-stu-id="58cad-130">To deactivate the device</span></span>

1. <span data-ttu-id="58cad-131">A szolgáltatást, lépjen **felügyeleti > eszközök**.</span><span class="sxs-lookup"><span data-stu-id="58cad-131">In your service, go to **Management > Devices**.</span></span> <span data-ttu-id="58cad-132">Az a **eszközök** paneljén kattintson, és válassza ki az eszközt, amelyet meg kíván inaktiválása.</span><span class="sxs-lookup"><span data-stu-id="58cad-132">In the **Devices** blade, click and select the device that you wish to deactivate.</span></span>
   
    ![Válassza ki az eszköz inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. <span data-ttu-id="58cad-134">Az a **eszköz irányítópult** paneljén kattintson **... További** , és válassza ki a listáról, **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="58cad-134">In your **Device dashboard** blade, click **… More** and from the list, select **Deactivate**.</span></span>
   
    ![Kattintson az Inaktiválás gombra](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. <span data-ttu-id="58cad-136">Az a **Deactivate** panelen írja be az eszköz nevét, és kattintson a **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="58cad-136">In the **Deactivate** blade, type the device name and then click **Deactivate**.</span></span> 
   
    ![Erősítse meg inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    <span data-ttu-id="58cad-138">Az inaktiválás folyamat elindul, és néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="58cad-138">The deactivate process starts and takes a few minutes to complete.</span></span>
   
    ![A folyamatban lévő inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. <span data-ttu-id="58cad-140">Deaktiválás, után az eszközök listájának frissítése.</span><span class="sxs-lookup"><span data-stu-id="58cad-140">After deactivation, the list of devices refreshes.</span></span>
   
    ![Teljes inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    <span data-ttu-id="58cad-142">Most törölheti ezt az eszközt.</span><span class="sxs-lookup"><span data-stu-id="58cad-142">You can now delete this device.</span></span>

## <a name="delete-the-device"></a><span data-ttu-id="58cad-143">Az eszköz törlése</span><span class="sxs-lookup"><span data-stu-id="58cad-143">Delete the device</span></span>

<span data-ttu-id="58cad-144">Egy eszköz regisztrációját törölni először inaktiválja.</span><span class="sxs-lookup"><span data-stu-id="58cad-144">A device has to be first deactivated to delete it.</span></span> <span data-ttu-id="58cad-145">Egy eszköz törlése eltávolítja a listán szereplő eszközök kapcsolódik a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="58cad-145">Deleting a device removes it from the list of devices connected to the service.</span></span> <span data-ttu-id="58cad-146">A szolgáltatás már nem kezelhetők a törölt eszköz.</span><span class="sxs-lookup"><span data-stu-id="58cad-146">The service can then no longer manage the deleted device.</span></span> <span data-ttu-id="58cad-147">Az eszközzel kapcsolatos adatok azonban továbbra is a felhőben.</span><span class="sxs-lookup"><span data-stu-id="58cad-147">The data associated with the device however remains in the cloud.</span></span> <span data-ttu-id="58cad-148">Ezek az adatok a díjak majd fizetni.</span><span class="sxs-lookup"><span data-stu-id="58cad-148">This data then accrues charges.</span></span>

<span data-ttu-id="58cad-149">Az eszköz törlése, hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="58cad-149">To delete the device, perform the following steps.</span></span>

#### <a name="to-delete-the-device"></a><span data-ttu-id="58cad-150">Az eszköz törlése</span><span class="sxs-lookup"><span data-stu-id="58cad-150">To delete the device</span></span>

1. <span data-ttu-id="58cad-151">Ugrás a StorSimple Device Manager **felügyeleti > eszközök**.</span><span class="sxs-lookup"><span data-stu-id="58cad-151">In your StorSimple Device Manager, go to **Management > Devices**.</span></span> <span data-ttu-id="58cad-152">Az a **eszközök** panelen válassza ki a törölni kívánt inaktív eszközök.</span><span class="sxs-lookup"><span data-stu-id="58cad-152">In the **Devices** blade, select a deactivated device that you wish to delete.</span></span>
2. <span data-ttu-id="58cad-153">Az a **eszköz irányítópult** paneljén kattintson **... További** majd **törlése**.</span><span class="sxs-lookup"><span data-stu-id="58cad-153">In the **Device dashboard** blade, click **… More** and then click **Delete**.</span></span>
   
   ![Válassza ki az eszköz törlése](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. <span data-ttu-id="58cad-155">Az a **törlése** panelen írja be a törlés jóváhagyásához, és kattintson az eszköz nevét **törlése**.</span><span class="sxs-lookup"><span data-stu-id="58cad-155">In the **Delete** blade, type the name of your device to confirm the deletion and then click **Delete**.</span></span> <span data-ttu-id="58cad-156">Az eszköz törlése nem érinti az eszközhöz társított felhő adatokat.</span><span class="sxs-lookup"><span data-stu-id="58cad-156">Deleting the device does not delete the cloud data associated with the device.</span></span> 
   
   ![Törlés megerősítése](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. <span data-ttu-id="58cad-158">A törlés indul, és néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="58cad-158">The deletion starts and takes a few minutes to complete.</span></span>
   
   ![Törlése folyamatban](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    <span data-ttu-id="58cad-160">Az eszköz törlését követően megtekintheti a eszközök frissített listáját.</span><span class="sxs-lookup"><span data-stu-id="58cad-160">After the device is deleted, you can view the updated list of devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="58cad-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58cad-161">Next steps</span></span>

* <span data-ttu-id="58cad-162">Feladatátvételt információkért látogasson el [feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple virtuális tömb](storsimple-virtual-array-failover-dr.md).</span><span class="sxs-lookup"><span data-stu-id="58cad-162">For information on how to fail over, go to [Failover and disaster recovery of your StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md).</span></span>

* <span data-ttu-id="58cad-163">A StorSimple eszköz Manager szolgáltatással kapcsolatos további tudnivalókért keresse fel [a StorSimple Device Manager szolgáltatás segítségével felügyelheti a StorSimple virtuális tömb](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="58cad-163">To learn more about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple Virtual Array](storsimple-virtual-array-manager-service-administration.md).</span></span> 

