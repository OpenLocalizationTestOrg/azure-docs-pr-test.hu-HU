---
title: "aaaDeactivate, és törölje a Microsoft Azure StorSimple virtuális tömb |} Microsoft Docs"
description: "Ismerteti, hogyan tooremove StorSimple eszközt a szolgáltatás első inaktiválása és törlését is."
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
ms.openlocfilehash: b1f3ddb5822d19965739777e238af19b507df984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a><span data-ttu-id="b442a-103">A StorSimple virtuális tömb inaktiváljon</span><span class="sxs-lookup"><span data-stu-id="b442a-103">Deactivate and delete a StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="b442a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b442a-104">Overview</span></span>

<span data-ttu-id="b442a-105">Ha egy StorSimple virtuális tömb inaktiválja, megszakítja hello kapcsolat hello eszköz és hello megfelelő StorSimple Device Manager szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="b442a-105">When you deactivate a StorSimple Virtual Array, you break hello connection between hello device and hello corresponding StorSimple Device Manager service.</span></span> <span data-ttu-id="b442a-106">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="b442a-106">This tutorial explains how to:</span></span>

* <span data-ttu-id="b442a-107">Eszköz inaktiválása</span><span class="sxs-lookup"><span data-stu-id="b442a-107">Deactivate a device</span></span> 
* <span data-ttu-id="b442a-108">Deaktivált eszköz törlése</span><span class="sxs-lookup"><span data-stu-id="b442a-108">Delete a deactivated device</span></span>

<span data-ttu-id="b442a-109">a cikkben szereplő információkat hello virtuális tömbök tooStorSimple vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b442a-109">hello information in this article applies tooStorSimple Virtual Arrays only.</span></span> <span data-ttu-id="b442a-110">8000 Series információkért nyissa meg toohow túl[inaktiválja vagy törölje az eszköz](storsimple-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="b442a-110">For information on 8000 series, go toohow too[deactivate or delete a device](storsimple-deactivate-and-delete-device.md).</span></span>

## <a name="when-toodeactivate"></a><span data-ttu-id="b442a-111">Amikor toodeactivate?</span><span class="sxs-lookup"><span data-stu-id="b442a-111">When toodeactivate?</span></span>

<span data-ttu-id="b442a-112">Az inaktiválást állandó művelet, és nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="b442a-112">Deactivation is a PERMANENT operation and cannot be undone.</span></span> <span data-ttu-id="b442a-113">Deaktivált eszköz újra hello StorSimple Device Manager szolgáltatás nem regisztrált.</span><span class="sxs-lookup"><span data-stu-id="b442a-113">You cannot register a deactivated device with hello StorSimple Device Manager service again.</span></span> <span data-ttu-id="b442a-114">Előfordulhat, hogy toodeactivate kell, és törölje a következő forgatókönyvek hello egy StorSimple virtuális tömb:</span><span class="sxs-lookup"><span data-stu-id="b442a-114">You may need toodeactivate and delete a StorSimple Virtual Array in hello following scenarios:</span></span>

* <span data-ttu-id="b442a-115">**A tervezett feladatátvételt** : az eszköz online állapotban, és azt tervezi, toofail keresztül az eszközt.</span><span class="sxs-lookup"><span data-stu-id="b442a-115">**Planned failover** : Your device is online and you plan toofail over your device.</span></span> <span data-ttu-id="b442a-116">Ha azt tervezi, tooupgrade tooa nagyobb eszközre, szükség lehet toofail keresztül az eszközt.</span><span class="sxs-lookup"><span data-stu-id="b442a-116">If you are planning tooupgrade tooa larger device, you may need toofail over your device.</span></span> <span data-ttu-id="b442a-117">Miután hello adatok tulajdonjoga kerül, és hello feladatátvétel befejeződött, a rendszer automatikusan törli a hello forráseszközt.</span><span class="sxs-lookup"><span data-stu-id="b442a-117">After hello data ownership is transferred and hello failover is complete, hello source device is automatically deleted.</span></span>
* <span data-ttu-id="b442a-118">**Nem tervezett feladatátvétel** : az eszköz offline állapotban, és szüksége toofail hello eszközön keresztül.</span><span class="sxs-lookup"><span data-stu-id="b442a-118">**Unplanned failover** : Your device is offline and you need toofail over hello device.</span></span> <span data-ttu-id="b442a-119">Ez a forgatókönyv során egy olyan vészhelyzet esetén fordulhat, ha nincs kimaradás hello adatközpontban, és az elsődleges eszköz nem működik.</span><span class="sxs-lookup"><span data-stu-id="b442a-119">This scenario may occur during a disaster when there is an outage in hello datacenter and your primary device is down.</span></span> <span data-ttu-id="b442a-120">Toofail hello eszköz tooa másodlagos eszköz keresztül tervezi.</span><span class="sxs-lookup"><span data-stu-id="b442a-120">You plan toofail over hello device tooa secondary device.</span></span> <span data-ttu-id="b442a-121">Miután hello adatok tulajdonjoga kerül, és hello feladatátvétel befejeződött, a rendszer automatikusan törli a hello forráseszközt.</span><span class="sxs-lookup"><span data-stu-id="b442a-121">After hello data ownership is transferred and hello failover is complete, hello source device is automatically deleted.</span></span>
* <span data-ttu-id="b442a-122">**Szerelje le** : kívánt toodecommission hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="b442a-122">**Decommission** : You want toodecommission hello device.</span></span> <span data-ttu-id="b442a-123">Ehhez toofirst hello eszköz inaktiválása, és törölje azt.</span><span class="sxs-lookup"><span data-stu-id="b442a-123">This requires you toofirst deactivate hello device and then delete it.</span></span> <span data-ttu-id="b442a-124">Ha egy eszköz inaktiválja, már nem érheti el helyben tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="b442a-124">When you deactivate a device, you can no longer access any data that is stored locally.</span></span> <span data-ttu-id="b442a-125">Egyetlen hozzáférés és a helyreállítás hello felhőben tárolt adatok hello is.</span><span class="sxs-lookup"><span data-stu-id="b442a-125">You can only access and recover hello data stored in hello cloud.</span></span> <span data-ttu-id="b442a-126">Ha tookeep hello eszközadatok inaktiválás után, majd kell venni az adatok egy felhő-pillanatfelvételt eszköz inaktiválása előtt.</span><span class="sxs-lookup"><span data-stu-id="b442a-126">If you plan tookeep hello device data after deactivation, then you should take a cloud snapshot of all your data before you deactivate a device.</span></span> <span data-ttu-id="b442a-127">A felhő-pillanatfelvételt toorecover lehetővé teszi az összes adat hello egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="b442a-127">This cloud snapshot allows you toorecover all hello data at a later stage.</span></span>

## <a name="deactivate-a-device"></a><span data-ttu-id="b442a-128">Eszköz inaktiválása</span><span class="sxs-lookup"><span data-stu-id="b442a-128">Deactivate a device</span></span>

<span data-ttu-id="b442a-129">toodeactivate eszközét, hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="b442a-129">toodeactivate your device, perform hello following steps.</span></span>

#### <a name="toodeactivate-hello-device"></a><span data-ttu-id="b442a-130">toodeactivate hello eszköz</span><span class="sxs-lookup"><span data-stu-id="b442a-130">toodeactivate hello device</span></span>

1. <span data-ttu-id="b442a-131">A szolgáltatást, lépjen túl**felügyeleti > eszközök**.</span><span class="sxs-lookup"><span data-stu-id="b442a-131">In your service, go too**Management > Devices**.</span></span> <span data-ttu-id="b442a-132">A hello **eszközök** panelen, kattintson és, hogy kívánja-e toodeactivate válassza hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="b442a-132">In hello **Devices** blade, click and select hello device that you wish toodeactivate.</span></span>
   
    ![Válassza ki az eszköz toodeactivate](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. <span data-ttu-id="b442a-134">Az a **eszköz irányítópult** paneljén kattintson **... További** hello listájából válassza ki és **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="b442a-134">In your **Device dashboard** blade, click **… More** and from hello list, select **Deactivate**.</span></span>
   
    ![Kattintson az Inaktiválás gombra](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. <span data-ttu-id="b442a-136">A hello **Deactivate** paneljén, típusnév hello eszköz, és kattintson **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="b442a-136">In hello **Deactivate** blade, type hello device name and then click **Deactivate**.</span></span> 
   
    ![Erősítse meg inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    <span data-ttu-id="b442a-138">hello inaktiválása folyamat elindul, és néhány perc toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b442a-138">hello deactivate process starts and takes a few minutes toocomplete.</span></span>
   
    ![A folyamatban lévő inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. <span data-ttu-id="b442a-140">Az inaktiválást, után hello listán szereplő eszközök frissíti.</span><span class="sxs-lookup"><span data-stu-id="b442a-140">After deactivation, hello list of devices refreshes.</span></span>
   
    ![Teljes inaktiválása](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    <span data-ttu-id="b442a-142">Most törölheti ezt az eszközt.</span><span class="sxs-lookup"><span data-stu-id="b442a-142">You can now delete this device.</span></span>

## <a name="delete-hello-device"></a><span data-ttu-id="b442a-143">Hello eszköz törlése</span><span class="sxs-lookup"><span data-stu-id="b442a-143">Delete hello device</span></span>

<span data-ttu-id="b442a-144">Egy eszköz rendelkezik toobe első deaktivált toodelete azt.</span><span class="sxs-lookup"><span data-stu-id="b442a-144">A device has toobe first deactivated toodelete it.</span></span> <span data-ttu-id="b442a-145">Egy eszköz törlése eltávolítja a eszközök csatlakoztatott toohello szolgáltatás hello listájáról.</span><span class="sxs-lookup"><span data-stu-id="b442a-145">Deleting a device removes it from hello list of devices connected toohello service.</span></span> <span data-ttu-id="b442a-146">hello szolgáltatás már nem felügyelhető hello törlése eszközt.</span><span class="sxs-lookup"><span data-stu-id="b442a-146">hello service can then no longer manage hello deleted device.</span></span> <span data-ttu-id="b442a-147">hello adatok hello eszközhöz társított azonban hello felhőben marad.</span><span class="sxs-lookup"><span data-stu-id="b442a-147">hello data associated with hello device however remains in hello cloud.</span></span> <span data-ttu-id="b442a-148">Ezek az adatok a díjak majd fizetni.</span><span class="sxs-lookup"><span data-stu-id="b442a-148">This data then accrues charges.</span></span>

<span data-ttu-id="b442a-149">toodelete hello eszköz, hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="b442a-149">toodelete hello device, perform hello following steps.</span></span>

#### <a name="toodelete-hello-device"></a><span data-ttu-id="b442a-150">toodelete hello eszköz</span><span class="sxs-lookup"><span data-stu-id="b442a-150">toodelete hello device</span></span>

1. <span data-ttu-id="b442a-151">Túl nyissa meg a StorSimple Device Manager**felügyeleti > eszközök**.</span><span class="sxs-lookup"><span data-stu-id="b442a-151">In your StorSimple Device Manager, go too**Management > Devices**.</span></span> <span data-ttu-id="b442a-152">A hello **eszközök** panelen jelölje ki az, hogy kívánja-e toodelete deaktivált eszközt.</span><span class="sxs-lookup"><span data-stu-id="b442a-152">In hello **Devices** blade, select a deactivated device that you wish toodelete.</span></span>
2. <span data-ttu-id="b442a-153">A hello **eszköz irányítópult** paneljén kattintson **... További** majd **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b442a-153">In hello **Device dashboard** blade, click **… More** and then click **Delete**.</span></span>
   
   ![Válassza ki az eszköz toodelete](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. <span data-ttu-id="b442a-155">A hello **törlése** paneljén, típusnév hello az eszköz tooconfirm hello törlésre, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b442a-155">In hello **Delete** blade, type hello name of your device tooconfirm hello deletion and then click **Delete**.</span></span> <span data-ttu-id="b442a-156">Hello-eszköz törlése nem törli hello eszközhöz társított hello felhőbeli adatát.</span><span class="sxs-lookup"><span data-stu-id="b442a-156">Deleting hello device does not delete hello cloud data associated with hello device.</span></span> 
   
   ![Törlés megerősítése](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. <span data-ttu-id="b442a-158">hello törlése elindul, és néhány perc toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b442a-158">hello deletion starts and takes a few minutes toocomplete.</span></span>
   
   ![Törlése folyamatban](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    <span data-ttu-id="b442a-160">Hello eszköz törlését követően megtekintheti az eszközök listájának frissítése hello.</span><span class="sxs-lookup"><span data-stu-id="b442a-160">After hello device is deleted, you can view hello updated list of devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b442a-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b442a-161">Next steps</span></span>

* <span data-ttu-id="b442a-162">Információ toofail keresztül, nyissa meg túl[feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple virtuális tömb](storsimple-virtual-array-failover-dr.md).</span><span class="sxs-lookup"><span data-stu-id="b442a-162">For information on how toofail over, go too[Failover and disaster recovery of your StorSimple Virtual Array](storsimple-virtual-array-failover-dr.md).</span></span>

* <span data-ttu-id="b442a-163">További részletek toolearn, hogyan toouse hello StorSimple Device Manager szolgáltatást, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple virtuális tömb](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b442a-163">toolearn more about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple Virtual Array](storsimple-virtual-array-manager-service-administration.md).</span></span> 

