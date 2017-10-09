---
title: "a StorSimple aaaManage hozzáférés-vezérlési rekordokat |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hozzáférés-vezérlés rögzíti, mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszköz (ACRs) toodetermine."
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
ms.openlocfilehash: cf532206e2c0bc49da853663ba34ae993ec2981d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a><span data-ttu-id="e665d-103">Hello StorSimple Manager szolgáltatás toomanage access control rekordok használata</span><span class="sxs-lookup"><span data-stu-id="e665d-103">Use hello StorSimple Manager service toomanage access control records</span></span>

## <a name="overview"></a><span data-ttu-id="e665d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e665d-104">Overview</span></span>
<span data-ttu-id="e665d-105">Hozzáférés-vezérlési rekordokat (ACRs) lehetővé teszik toospecify mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="e665d-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple device.</span></span> <span data-ttu-id="e665d-106">ACRs tooa adott kötet van beállítva, és tartalmazhat hello iSCSI minősített nevét (IQN-nevekre vonatkozóan) hello gazdagépek.</span><span class="sxs-lookup"><span data-stu-id="e665d-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="e665d-107">Ha egy állomás megpróbál tooconnect tooa kötet, hello eszköz ellenőrzi hello ACR társított hello IQN nevét, és ha egyezést, majd hello kapcsolat jön létre a köteten.</span><span class="sxs-lookup"><span data-stu-id="e665d-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="e665d-108">hozzáférés-vezérlési rekordokat a hello hello **konfigurációs** a StorSimple Device Manager szolgáltatás panelre szakasza az összes hello hozzáférés-vezérlési rekordokat hello állomás IQN-nevekre vonatkozóan megfelelő hello együtt jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e665d-108">hello access control records in hello **Configuration** section of your StorSimple Device Manager service blade display all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

<span data-ttu-id="e665d-109">Ez az oktatóanyag azt ismerteti, a következő ACR kapcsolatos általános feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="e665d-109">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="e665d-110">Egy hozzáférés-vezérlési rekordot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e665d-110">Add an access control record</span></span>
* <span data-ttu-id="e665d-111">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="e665d-111">Edit an access control record</span></span>
* <span data-ttu-id="e665d-112">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="e665d-112">Delete an access control record</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="e665d-113">Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet.</span><span class="sxs-lookup"><span data-stu-id="e665d-113">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>
> * <span data-ttu-id="e665d-114">Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.</span><span class="sxs-lookup"><span data-stu-id="e665d-114">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>

## <a name="get-hello-iqn"></a><span data-ttu-id="e665d-115">Hello IQN-Nevének lekérése</span><span class="sxs-lookup"><span data-stu-id="e665d-115">Get hello IQN</span></span>

<span data-ttu-id="e665d-116">Hajtsa végre a következő lépéseket tooget hello egy Windows Server 2012 rendszert futtató Windows-állomás IQN-Nevének hello.</span><span class="sxs-lookup"><span data-stu-id="e665d-116">Perform hello following steps tooget hello IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a><span data-ttu-id="e665d-117">Egy hozzáférés-vezérlési rekordot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e665d-117">Add an access control record</span></span>
<span data-ttu-id="e665d-118">Hello használata **konfigurációs** hello StorSimple Device Manager szolgáltatás panel tooadd ACRs szakaszában.</span><span class="sxs-lookup"><span data-stu-id="e665d-118">You use hello **Configuration** section in hello StorSimple Device Manager service blade tooadd ACRs.</span></span> <span data-ttu-id="e665d-119">Általában egy ACR fog társítani egy kötetet.</span><span class="sxs-lookup"><span data-stu-id="e665d-119">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="e665d-120">Hajtsa végre a következő lépéseket tooadd egy ACR hello.</span><span class="sxs-lookup"><span data-stu-id="e665d-120">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-acr"></a><span data-ttu-id="e665d-121">egy ACR tooadd</span><span class="sxs-lookup"><span data-stu-id="e665d-121">tooadd an ACR</span></span>

1. <span data-ttu-id="e665d-122">Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="e665d-122">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="e665d-123">A hello **hozzáférés-vezérlési rekordokat** panelen kattintson a **+ Hozzáadás ACR**.</span><span class="sxs-lookup"><span data-stu-id="e665d-123">In hello **Access control records** blade, click **+ Add ACR**.</span></span>

    ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. <span data-ttu-id="e665d-125">A hello **hozzáadása ACR** panelen hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e665d-125">In hello **Add ACR** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="e665d-126">Adja meg az ACR nevét.</span><span class="sxs-lookup"><span data-stu-id="e665d-126">Supply a name for your ACR.</span></span>
    
    2. <span data-ttu-id="e665d-127">Adja meg a Windows Server-állomáson alatt hello IQN nevét **iSCSI kezdeményező nevét (IQN)**.</span><span class="sxs-lookup"><span data-stu-id="e665d-127">Provide hello IQN name of your Windows Server host under **iSCSI Initiator Name (IQN)**.</span></span>

    3. <span data-ttu-id="e665d-128">Kattintson a **Hozzáadás** toocreate hello ACR.</span><span class="sxs-lookup"><span data-stu-id="e665d-128">Click **Add** toocreate hello ACR.</span></span>

        ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  <span data-ttu-id="e665d-130">hello újonnan hozzáadott ACR hello ACRs táblázatos listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e665d-130">hello newly added ACR will display in hello tabular listing of ACRs.</span></span>

    ![Kattintson a Hozzáadás ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a><span data-ttu-id="e665d-132">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="e665d-132">Edit an access control record</span></span>
<span data-ttu-id="e665d-133">Hello használata **konfigurációs** hello StorSimple Device Manager szolgáltatás panel tooedit ACRs szakaszában.</span><span class="sxs-lookup"><span data-stu-id="e665d-133">You use hello **Configuration** section in hello StorSimple Device Manager service blade tooedit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="e665d-134">Javasoljuk, hogy csak a jelenleg nem használható ACRs módosítása.</span><span class="sxs-lookup"><span data-stu-id="e665d-134">It is recommended that you modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="e665d-135">tooedit egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="e665d-135">tooedit an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>

<span data-ttu-id="e665d-136">Hajtsa végre a következő lépéseket tooedit egy ACR hello.</span><span class="sxs-lookup"><span data-stu-id="e665d-136">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-access-control-record"></a><span data-ttu-id="e665d-137">egy hozzáférés-vezérlési rekordot tooedit</span><span class="sxs-lookup"><span data-stu-id="e665d-137">tooedit an access control record</span></span>
1.  <span data-ttu-id="e665d-138">Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="e665d-138">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="e665d-140">Kattintson a hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, és válassza ki, hogy kívánja-e toomodify ACR hello.</span><span class="sxs-lookup"><span data-stu-id="e665d-140">In hello tabular listing of hello access control records, click and select hello ACR that you wish toomodify.</span></span>

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr1.png)

3. <span data-ttu-id="e665d-142">A hello **Szerkesztés hozzáférés-vezérlési rekordot** panelen adjon meg egy másik IQN-Nevének megfelelő tooanother állomás.</span><span class="sxs-lookup"><span data-stu-id="e665d-142">In hello **Edit access control record** blade, provide a different IQN corresponding tooanother host.</span></span>

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr2.png)

4. <span data-ttu-id="e665d-144">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e665d-144">Click **Save**.</span></span> <span data-ttu-id="e665d-145">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="e665d-145">When prompted for confirmation, click **Yes**.</span></span> 

    ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-8000-manage-acrs/editacr3.png)

5. <span data-ttu-id="e665d-147">Értesítést kap hello ACR frissül.</span><span class="sxs-lookup"><span data-stu-id="e665d-147">You are notified when hello ACR is updated.</span></span> <span data-ttu-id="e665d-148">hello táblázatos felsorolása tooreflect hello változás is frissíti.</span><span class="sxs-lookup"><span data-stu-id="e665d-148">hello tabular listing also updates tooreflect hello change.</span></span>

   
## <a name="delete-an-access-control-record"></a><span data-ttu-id="e665d-149">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="e665d-149">Delete an access control record</span></span>
<span data-ttu-id="e665d-150">Hello használata **konfigurációs** hello StorSimple Device Manager szolgáltatás panel toodelete ACRs szakaszában.</span><span class="sxs-lookup"><span data-stu-id="e665d-150">You use hello **Configuration** section in hello StorSimple Device Manager service blade toodelete ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="e665d-151">Csak azokat a ACRs, amelyek jelenleg nem törölhetők.</span><span class="sxs-lookup"><span data-stu-id="e665d-151">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="e665d-152">toodelete egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="e665d-152">toodelete an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>

<span data-ttu-id="e665d-153">Hajtsa végre a következő lépéseket toodelete egy hozzáférés-vezérlési rekordot hello.</span><span class="sxs-lookup"><span data-stu-id="e665d-153">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="e665d-154">egy hozzáférés-vezérlési rekordot toodelete</span><span class="sxs-lookup"><span data-stu-id="e665d-154">toodelete an access control record</span></span>
1.  <span data-ttu-id="e665d-155">Nyissa meg tooyour StorSimple Device Manager szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="e665d-155">Go tooyour StorSimple Device Manager service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/createacr1.png)

2. <span data-ttu-id="e665d-157">Kattintson a hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, és válassza ki, hogy kívánja-e toodelete ACR hello.</span><span class="sxs-lookup"><span data-stu-id="e665d-157">In hello tabular listing of hello access control records, click and select hello ACR that you wish toodelete.</span></span>

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. <span data-ttu-id="e665d-159">Kattintson a jobb gombbal a tooinvoke hello helyi menüt, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e665d-159">Right-click tooinvoke hello context menu and select **Delete**.</span></span>

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. <span data-ttu-id="e665d-161">Megerősítést kér, amikor hello tekintse át, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e665d-161">When prompted for confirmation, review hello information and then click **Delete**.</span></span>

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. <span data-ttu-id="e665d-163">Hello törlés befejezéséről értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="e665d-163">You are notified when hello deletion completes.</span></span> <span data-ttu-id="e665d-164">hello táblázatos felsorolása frissített tooreflect hello törlésre.</span><span class="sxs-lookup"><span data-stu-id="e665d-164">hello tabular listing is updated tooreflect hello deletion.</span></span>

    ![Nyissa meg tooaccess vezérlésére rekordok](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a><span data-ttu-id="e665d-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e665d-166">Next steps</span></span>
* <span data-ttu-id="e665d-167">További információ [felügyelete a StorSimple-köteteket](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="e665d-167">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span>
* <span data-ttu-id="e665d-168">További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="e665d-168">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

