---
title: "Hozzáférés-vezérlési rekordokat a StorSimple kezelése |} Microsoft Docs"
description: "Hozzáférés-vezérlési rekordokat (ACRs) használja annak meghatározásához, hogy mely gazdagépek csatlakozhat egy kötetet a StorSimple eszköz ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: 9173e34f889ce1c082b20bb382cb6ca9a03dd797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a><span data-ttu-id="fd555-103">A StorSimple Manager szolgáltatással hozzáférés-vezérlési rekordokat kezelése</span><span class="sxs-lookup"><span data-stu-id="fd555-103">Use the StorSimple Manager service to manage access control records</span></span>

## <a name="overview"></a><span data-ttu-id="fd555-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fd555-104">Overview</span></span>
<span data-ttu-id="fd555-105">Hozzáférés-vezérlési rekordokat (ACRs) lehetővé teszik annak megadását, hogy mely gazdagépek csatlakozhat a StorSimple eszközön levő kötet.</span><span class="sxs-lookup"><span data-stu-id="fd555-105">Access control records (ACRs) allow you to specify which hosts can connect to a volume on the StorSimple device.</span></span> <span data-ttu-id="fd555-106">ACRs vannak beállítva, hogy egy adott kötet, és az iSCSI minősített nevét (IQN-nevekre vonatkozóan), a gazdagépek tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fd555-106">ACRs are set to a specific volume and contain the iSCSI Qualified Names (IQNs) of the hosts.</span></span> <span data-ttu-id="fd555-107">Állomás megpróbál egy kötet csatlakozni, amikor az eszköz ellenőrzi, hogy a köteten a IQN-nevének társított az ACR, és ha van egyezés, majd a kapcsolat létrejön.</span><span class="sxs-lookup"><span data-stu-id="fd555-107">When a host tries to connect to a volume, the device checks the ACR associated with that volume for the IQN name and if there is a match, then the connection is established.</span></span> <span data-ttu-id="fd555-108">A hozzáférés-vezérlés rögzít a **konfigurációs** a StorSimple Device Manager szolgáltatás panelre szakasza access control rekordok a megfelelő IQN-nevekre vonatkozóan a gazdagépek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fd555-108">The access control records in the **Configuration** section of your StorSimple Device Manager service blade display all the access control records with the corresponding IQNs of the hosts.</span></span>

<span data-ttu-id="fd555-109">Ez az oktatóanyag azt ismerteti, hogy a következő általános ACR kapcsolatos feladatok:</span><span class="sxs-lookup"><span data-stu-id="fd555-109">This tutorial explains the following common ACR-related tasks:</span></span>

* <span data-ttu-id="fd555-110">Egy hozzáférés-vezérlési rekordot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fd555-110">Add an access control record</span></span>
* <span data-ttu-id="fd555-111">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="fd555-111">Edit an access control record</span></span>
* <span data-ttu-id="fd555-112">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="fd555-112">Delete an access control record</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="fd555-113">Amikor egy ACR rendel egy olyan kötetre, gondoskodunk, hogy a kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagépen, mert ez a kötet, megsérülhet.</span><span class="sxs-lookup"><span data-stu-id="fd555-113">When assigning an ACR to a volume, take care that the volume is not concurrently accessed by more than one non-clustered host because this could corrupt the volume.</span></span>
> * <span data-ttu-id="fd555-114">Ha töröl egy ACR a kötetről, győződjön meg arról, hogy a megfelelő gazdagép nem fér hozzá a kötetet, mert a Törlés a írható-olvasható szüneteltetése eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="fd555-114">When deleting an ACR from a volume, make sure that the corresponding host is not accessing the volume because the deletion could result in a read-write disruption.</span></span>

## <a name="get-the-iqn"></a><span data-ttu-id="fd555-115">IQN-Nevének lekérése</span><span class="sxs-lookup"><span data-stu-id="fd555-115">Get the IQN</span></span>

<span data-ttu-id="fd555-116">A következő lépésekkel egy Windows Server 2012 rendszert futtató Windows-állomás IQN-Nevének lekérése.</span><span class="sxs-lookup"><span data-stu-id="fd555-116">Perform the following steps to get the IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a><span data-ttu-id="fd555-117">Egy hozzáférés-vezérlési rekordot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fd555-117">Add an access control record</span></span>
<span data-ttu-id="fd555-118">Használja a **konfigurációs** szakasz ACRs hozzáadása a StorSimple Device Manager szolgáltatás a panelen.</span><span class="sxs-lookup"><span data-stu-id="fd555-118">You use the **Configuration** section in the StorSimple Device Manager service blade to add ACRs.</span></span> <span data-ttu-id="fd555-119">Általában egy ACR fog társítani egy kötetet.</span><span class="sxs-lookup"><span data-stu-id="fd555-119">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="fd555-120">A következő lépésekkel adja hozzá az ACR.</span><span class="sxs-lookup"><span data-stu-id="fd555-120">Perform the following steps to add an ACR.</span></span>

#### <a name="to-add-an-acr"></a><span data-ttu-id="fd555-121">Egy ACR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fd555-121">To add an ACR</span></span>

1. <span data-ttu-id="fd555-122">Nyissa meg a StorSimple eszköz Manager szolgáltatáshoz, kattintson duplán a szolgáltatás nevére, majd belül a **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="fd555-122">Go to your StorSimple Device Manager service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="fd555-123">Az a **hozzáférés-vezérlési rekordokat** panelen kattintson a **+ Hozzáadás ACR**.</span><span class="sxs-lookup"><span data-stu-id="fd555-123">In the **Access control records** blade, click **+ Add ACR**.</span></span>

    ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. <span data-ttu-id="fd555-125">Az a **hozzáadása ACR** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fd555-125">In the **Add ACR** blade, do the following steps:</span></span>

    1. <span data-ttu-id="fd555-126">Adja meg az ACR nevét.</span><span class="sxs-lookup"><span data-stu-id="fd555-126">Supply a name for your ACR.</span></span>
    
    2. <span data-ttu-id="fd555-127">Adja meg az a Windows Server-állomás IQN nevét **iSCSI kezdeményező nevét (IQN)**.</span><span class="sxs-lookup"><span data-stu-id="fd555-127">Provide the IQN name of your Windows Server host under **iSCSI Initiator Name (IQN)**.</span></span>

    3. <span data-ttu-id="fd555-128">Kattintson a **Hozzáadás** a ACR létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="fd555-128">Click **Add** to create the ACR.</span></span>

        ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  <span data-ttu-id="fd555-130">Az újonnan hozzáadott ACR ACRs táblázatos listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fd555-130">The newly added ACR will display in the tabular listing of ACRs.</span></span>

    ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a><span data-ttu-id="fd555-132">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="fd555-132">Edit an access control record</span></span>
<span data-ttu-id="fd555-133">Használja a **konfigurációs** szakasz ACRs szerkesztése a StorSimple Device Manager szolgáltatás a panelen.</span><span class="sxs-lookup"><span data-stu-id="fd555-133">You use the **Configuration** section in the StorSimple Device Manager service blade to edit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="fd555-134">Javasoljuk, hogy csak a jelenleg nem használható ACRs módosítása.</span><span class="sxs-lookup"><span data-stu-id="fd555-134">It is recommended that you modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="fd555-135">Egy olyan kötet, amely jelenleg használatban van társított ACR szerkesztéséhez először érdemes a kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="fd555-135">To edit an ACR associated with a volume that is currently in use, you must first take the volume offline.</span></span>

<span data-ttu-id="fd555-136">A következő lépésekkel egy ACR szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="fd555-136">Perform the following steps to edit an ACR.</span></span>

#### <a name="to-edit-an-access-control-record"></a><span data-ttu-id="fd555-137">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="fd555-137">To edit an access control record</span></span>
1.  <span data-ttu-id="fd555-138">Nyissa meg a StorSimple eszköz Manager szolgáltatáshoz, kattintson duplán a szolgáltatás nevére, majd belül a **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="fd555-138">Go to your StorSimple Device Manager service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>

    ![Ugrás a hozzáférés-vezérlési rekordokat](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="fd555-140">Kattintson a hozzáférés-vezérlési rekordokat táblázatos listája, és válassza ki a módosítani kívánt ACR.</span><span class="sxs-lookup"><span data-stu-id="fd555-140">In the tabular listing of the access control records, click and select the ACR that you wish to modify.</span></span>

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr1.png)

3. <span data-ttu-id="fd555-142">Az a **Szerkesztés hozzáférés-vezérlési rekordot** panelen adjon meg egy másik IQN-Nevének megfelelő egy másik gazdagépre.</span><span class="sxs-lookup"><span data-stu-id="fd555-142">In the **Edit access control record** blade, provide a different IQN corresponding to another host.</span></span>

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr2.png)

4. <span data-ttu-id="fd555-144">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fd555-144">Click **Save**.</span></span> <span data-ttu-id="fd555-145">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="fd555-145">When prompted for confirmation, click **Yes**.</span></span> 

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr3.png)

5. <span data-ttu-id="fd555-147">Értesítést kap a ACR frissül.</span><span class="sxs-lookup"><span data-stu-id="fd555-147">You are notified when the ACR is updated.</span></span> <span data-ttu-id="fd555-148">A táblázatos listázása is frissíti a változásnak.</span><span class="sxs-lookup"><span data-stu-id="fd555-148">The tabular listing also updates to reflect the change.</span></span>

   
## <a name="delete-an-access-control-record"></a><span data-ttu-id="fd555-149">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="fd555-149">Delete an access control record</span></span>
<span data-ttu-id="fd555-150">Használja a **konfigurációs** szakasz ACRs törli a StorSimple Device Manager szolgáltatás a panelen.</span><span class="sxs-lookup"><span data-stu-id="fd555-150">You use the **Configuration** section in the StorSimple Device Manager service blade to delete ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="fd555-151">Csak azokat a ACRs, amelyek jelenleg nem törölhetők.</span><span class="sxs-lookup"><span data-stu-id="fd555-151">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="fd555-152">Egy olyan kötet, amely jelenleg használatban van társított ACR törléséhez először érdemes a kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="fd555-152">To delete an ACR associated with a volume that is currently in use, you must first take the volume offline.</span></span>

<span data-ttu-id="fd555-153">A következő lépésekkel egy hozzáférés-vezérlési rekordot törlése.</span><span class="sxs-lookup"><span data-stu-id="fd555-153">Perform the following steps to delete an access control record.</span></span>

#### <a name="to-delete-an-access-control-record"></a><span data-ttu-id="fd555-154">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="fd555-154">To delete an access control record</span></span>
1.  <span data-ttu-id="fd555-155">Nyissa meg a StorSimple eszköz Manager szolgáltatáshoz, kattintson duplán a szolgáltatás nevére, majd belül a **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="fd555-155">Go to your StorSimple Device Manager service, double-click the service name, and then within the **Configuration** section, click **Access control records**.</span></span>

    ![Ugrás a hozzáférés-vezérlési rekordokat](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="fd555-157">Kattintson a hozzáférés-vezérlési rekordokat táblázatos listája, és válassza ki a törölni kívánt ACR.</span><span class="sxs-lookup"><span data-stu-id="fd555-157">In the tabular listing of the access control records, click and select the ACR that you wish to delete.</span></span>

    ![Ugrás a hozzáférés-vezérlési rekordokat](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. <span data-ttu-id="fd555-159">Kattintson a jobb gombbal a helyi menü el, és válassza ki a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="fd555-159">Right-click to invoke the context menu and select **Delete**.</span></span>

    ![Ugrás a hozzáférés-vezérlési rekordokat](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. <span data-ttu-id="fd555-161">Amikor felszólítja a megerősítésre, tekintse át az adatokat, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="fd555-161">When prompted for confirmation, review the information and then click **Delete**.</span></span>

    ![Ugrás a hozzáférés-vezérlési rekordokat](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. <span data-ttu-id="fd555-163">Értesítést kap a törlés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="fd555-163">You are notified when the deletion completes.</span></span> <span data-ttu-id="fd555-164">A táblázatos felsorolása a törlés frissül.</span><span class="sxs-lookup"><span data-stu-id="fd555-164">The tabular listing is updated to reflect the deletion.</span></span>

    ![Ugrás a hozzáférés-vezérlési rekordokat](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a><span data-ttu-id="fd555-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd555-166">Next steps</span></span>
* <span data-ttu-id="fd555-167">További információ [felügyelete a StorSimple-köteteket](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="fd555-167">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span>
* <span data-ttu-id="fd555-168">További információ [a StorSimple Manager szolgáltatás használata a StorSimple eszköz felügyeletéhez](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="fd555-168">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

