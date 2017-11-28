---
title: "aaaManage hozzáférés-vezérlési rekordokat a StorSimple virtuális tömb |} Microsoft Docs"
description: "Ismerteti, hogyan toomanage hozzáférés-vezérlés rögzíti, mely állomásokat csatlakozni tud a StorSimple virtuális tömb hello tooa köteten (ACRs) toodetermine."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 608fdf72413761ce3c9c4bf297a748489c415685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-access-control-records-for-storsimple-virtual-array"></a><span data-ttu-id="41fd0-103">StorSimple az Eszközkezelő toomanage hozzáférés-vezérlési rekordokat a StorSimple virtuális tömb</span><span class="sxs-lookup"><span data-stu-id="41fd0-103">Use StorSimple Device Manager toomanage access control records for StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="41fd0-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="41fd0-104">Overview</span></span>

<span data-ttu-id="41fd0-105">Hozzáférés-vezérlési rekordokat (ACRs) lehetővé teszik toospecify mely állomásokat kapcsolódhatnak tooa köteten hello StorSimple virtuális tömb (más néven hello StorSimple a helyszíni virtuális eszközt használ).</span><span class="sxs-lookup"><span data-stu-id="41fd0-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple Virtual Array (also known as hello StorSimple on-premises virtual device).</span></span> <span data-ttu-id="41fd0-106">ACRs tooa adott kötet van beállítva, és tartalmazhat hello iSCSI minősített nevét (IQN-nevekre vonatkozóan) hello gazdagépek.</span><span class="sxs-lookup"><span data-stu-id="41fd0-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="41fd0-107">Ha egy állomás megpróbál tooconnect tooa kötet, hello eszköz ellenőrzi, hogy a köteten hello IQN nevének társított ACR hello, és ha van egyezés, majd hello kapcsolat létrejön.</span><span class="sxs-lookup"><span data-stu-id="41fd0-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name, and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="41fd0-108">Hello **hozzáférés-vezérlési rekordokat** belül hello panel **konfigurációs** szakasz az Eszközkezelő szolgáltatás az összes hello hozzáférés-vezérlési rekordokat rendelkező hello megfelelő hello állomás IQN-nevekre vonatkozóan jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="41fd0-108">hello **Access control records** blade within hello **Configuration** section of your Device Manager service displays all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

![Hozzáférés-vezérlési rekordokat kezelése](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

<span data-ttu-id="41fd0-110">Ez az oktatóanyag azt ismerteti, a következő ACR kapcsolatos általános feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="41fd0-110">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="41fd0-111">Hello IQN-Nevének lekérése</span><span class="sxs-lookup"><span data-stu-id="41fd0-111">Get hello IQN</span></span>
* <span data-ttu-id="41fd0-112">Egy hozzáférés-vezérlési rekordot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="41fd0-112">Add an access control record</span></span>
* <span data-ttu-id="41fd0-113">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="41fd0-113">Edit an access control record</span></span>
* <span data-ttu-id="41fd0-114">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="41fd0-114">Delete an access control record</span></span>

> [!IMPORTANT]
> 
> * <span data-ttu-id="41fd0-115">Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet.</span><span class="sxs-lookup"><span data-stu-id="41fd0-115">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>
> * <span data-ttu-id="41fd0-116">Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.</span><span class="sxs-lookup"><span data-stu-id="41fd0-116">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>


## <a name="get-hello-iqn"></a><span data-ttu-id="41fd0-117">Hello IQN-Nevének lekérése</span><span class="sxs-lookup"><span data-stu-id="41fd0-117">Get hello IQN</span></span>

<span data-ttu-id="41fd0-118">Hajtsa végre a következő lépéseket tooget hello egy Windows Server 2012 rendszert futtató Windows-állomás IQN-Nevének hello.</span><span class="sxs-lookup"><span data-stu-id="41fd0-118">Perform hello following steps tooget hello IQN of a Windows host that is running Windows Server 2012.</span></span>

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a><span data-ttu-id="41fd0-119">Egy ACR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="41fd0-119">Add an ACR</span></span>

<span data-ttu-id="41fd0-120">Használhat **hozzáférés-vezérlési rekordokat** belül hello panel **konfigurációs** a StorSimple Device Manager szolgáltatás tooadd ACRs szakasza.</span><span class="sxs-lookup"><span data-stu-id="41fd0-120">You use **Access control records** blade within hello **Configuration** section of your StorSimple Device Manager service tooadd ACRs.</span></span> <span data-ttu-id="41fd0-121">Általában egy ACR hozzárendel egy kötetet.</span><span class="sxs-lookup"><span data-stu-id="41fd0-121">Typically, you associate one ACR with one volume.</span></span>

<span data-ttu-id="41fd0-122">Egy mennyiségi egy ACR társításával kapcsolatos információkért nyissa meg túl[kötet hozzáadása](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="41fd0-122">For information about associating an ACR with a volume, go too[add a volume](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41fd0-123">Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet.</span><span class="sxs-lookup"><span data-stu-id="41fd0-123">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span>


<span data-ttu-id="41fd0-124">Hajtsa végre a következő lépéseket tooadd egy ACR hello.</span><span class="sxs-lookup"><span data-stu-id="41fd0-124">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-acr"></a><span data-ttu-id="41fd0-125">egy ACR tooadd</span><span class="sxs-lookup"><span data-stu-id="41fd0-125">tooadd an ACR</span></span>

1. <span data-ttu-id="41fd0-126">Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** kattintson **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="41fd0-126">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, click **Access control records**.</span></span>
2. <span data-ttu-id="41fd0-127">A hello **hozzáférés-vezérlési rekordokat** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="41fd0-127">In hello **Access control records** blade, click **Add**.</span></span>
3. <span data-ttu-id="41fd0-128">A hello **hozzáadása ACR** panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="41fd0-128">In hello **Add ACR** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="41fd0-129">Adja meg az ACR **nevét**.</span><span class="sxs-lookup"><span data-stu-id="41fd0-129">Supply a **Name** for your ACR.</span></span>
    
    2. <span data-ttu-id="41fd0-130">A **iSCSI-kezdeményező neve**, adja meg a Windows-állomás IQN hello nevét.</span><span class="sxs-lookup"><span data-stu-id="41fd0-130">Under **iSCSI Initiator Name**, provide hello IQN name of your Windows host.</span></span> <span data-ttu-id="41fd0-131">a Windows Server-állomás IQN-Nevének tooget hello hello a következő:</span><span class="sxs-lookup"><span data-stu-id="41fd0-131">tooget hello IQN of your Windows Server host, do hello following:</span></span>
   
    3. <span data-ttu-id="41fd0-132">Indítsa el a hello Microsoft iSCSI-kezdeményezőt a Windows-gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="41fd0-132">Start hello Microsoft iSCSI initiator on your Windows host.</span></span> <span data-ttu-id="41fd0-133">Hello iSCSI kezdeményező tulajdonságai ablakban a hello **konfigurációs** lapra, válassza ki, másolja hello karakterlánc hello **kezdeményező neve** mező.</span><span class="sxs-lookup"><span data-stu-id="41fd0-133">In hello iSCSI Initiator Properties window, on hello **Configuration** tab, select and copy hello string from hello **Initiator Name** field.</span></span>
    <span data-ttu-id="41fd0-134">Illessze be ezt a karakterláncot a hello **IQN** hello mezőjét **hozzáadása ACR** panelen.</span><span class="sxs-lookup"><span data-stu-id="41fd0-134">Paste this string in hello **IQN** field in hello **Add ACR** blade.</span></span>
   
    6. <span data-ttu-id="41fd0-135">Kattintson a **Hozzáadás** tooadd hello ACR.</span><span class="sxs-lookup"><span data-stu-id="41fd0-135">Click **Add** tooadd hello ACR.</span></span>  
   
        ![Hozzáférés-vezérlési rekordokat hozzáadása](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. <span data-ttu-id="41fd0-137">táblázatos felsorolása hello van frissített tooreflect a hozzáadást.</span><span class="sxs-lookup"><span data-stu-id="41fd0-137">hello tabular listing is updated tooreflect this addition.</span></span>

## <a name="edit-an-acr"></a><span data-ttu-id="41fd0-138">Egy ACR szerkesztése</span><span class="sxs-lookup"><span data-stu-id="41fd0-138">Edit an ACR</span></span>

<span data-ttu-id="41fd0-139">Hello használata **hozzáférés-vezérlési rekordokat** belül hello panel **konfigurációs** az Eszközkezelő szolgáltatás az Azure portál tooedit ACRs hello szakasza.</span><span class="sxs-lookup"><span data-stu-id="41fd0-139">You use hello **Access control records** blade within hello **Configuration** section of your Device Manager service in hello Azure portal tooedit ACRs.</span></span>

> [!NOTE]
> <span data-ttu-id="41fd0-140">Ne módosítsa az ACR, amely jelenleg használatban van.</span><span class="sxs-lookup"><span data-stu-id="41fd0-140">You should not modify an ACR that is currently in use.</span></span> <span data-ttu-id="41fd0-141">tooedit egy ACR társított olyan kötet, amely jelenleg használatban van, először hajtson hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="41fd0-141">tooedit an ACR associated with a volume that is currently in use, you should first take hello volume offline.</span></span>


<span data-ttu-id="41fd0-142">Hajtsa végre a következő lépéseket tooedit egy ACR hello.</span><span class="sxs-lookup"><span data-stu-id="41fd0-142">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-acr"></a><span data-ttu-id="41fd0-143">egy ACR tooedit</span><span class="sxs-lookup"><span data-stu-id="41fd0-143">tooedit an ACR</span></span>

1. <span data-ttu-id="41fd0-144">Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** szakaszban **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="41fd0-144">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, **Access control records**.</span></span>
2. <span data-ttu-id="41fd0-145">A hello **hozzáférés-vezérlési rekordokat** paneljén hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, kattintson duplán a hello ACR, hogy kívánja-e toomodify.</span><span class="sxs-lookup"><span data-stu-id="41fd0-145">In hello **Access control records** blade, from hello tabular listing of hello access control records, double-click hello ACR that you wish toomodify.</span></span>
3. <span data-ttu-id="41fd0-146">A hello **Szerkesztés hozzáférés-vezérlési rekordokat** panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="41fd0-146">In hello **Edit access control records** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="41fd0-147">Adja meg az hello ACR IQN hello.</span><span class="sxs-lookup"><span data-stu-id="41fd0-147">Supply hello IQN for hello ACR.</span></span>
   
    2. <span data-ttu-id="41fd0-148">Kattintson a **mentése** hello panel toosave hello tetején hello ACR módosítani.</span><span class="sxs-lookup"><span data-stu-id="41fd0-148">Click **Save** at hello top of hello blade toosave hello modified ACR.</span></span> <span data-ttu-id="41fd0-149">A következő megerősítő üzenetet hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="41fd0-149">You see hello following confirmation message:</span></span>
   
        ![Hozzáférés-vezérlési rekordokat szerkesztése](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a><span data-ttu-id="41fd0-151">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="41fd0-151">Delete an access control record</span></span>

<span data-ttu-id="41fd0-152">Hello használata **konfigurációs** hello Azure portál toodelete ACRs lapján.</span><span class="sxs-lookup"><span data-stu-id="41fd0-152">You use hello **Configuration** page in hello Azure portal toodelete ACRs.</span></span>

> [!NOTE]
> 
> * <span data-ttu-id="41fd0-153">Ne törölje az ACR, amely jelenleg használatban van.</span><span class="sxs-lookup"><span data-stu-id="41fd0-153">You should not delete an ACR that is currently in use.</span></span> <span data-ttu-id="41fd0-154">toodelete egy ACR társított olyan kötet, amely jelenleg használatban van, először hajtson hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="41fd0-154">toodelete an ACR associated with a volume that is currently in use, you should first take hello volume offline.</span></span>
> * <span data-ttu-id="41fd0-155">Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.</span><span class="sxs-lookup"><span data-stu-id="41fd0-155">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>


<span data-ttu-id="41fd0-156">Hajtsa végre a következő lépéseket toodelete egy hozzáférés-vezérlési rekordot hello.</span><span class="sxs-lookup"><span data-stu-id="41fd0-156">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="41fd0-157">egy hozzáférés-vezérlési rekordot toodelete</span><span class="sxs-lookup"><span data-stu-id="41fd0-157">toodelete an access control record</span></span>

1. <span data-ttu-id="41fd0-158">Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **konfigurációs** szakaszban **hozzáférés-vezérlési rekordokat**.</span><span class="sxs-lookup"><span data-stu-id="41fd0-158">On hello service landing page, select your service, double-click hello service name, and then within hello **Configuration** section, **Access control records**.</span></span>

2. <span data-ttu-id="41fd0-159">A hello **hozzáférés-vezérlési rekordokat** paneljén hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, kattintson duplán a hello ACR, hogy kívánja-e toodelete.</span><span class="sxs-lookup"><span data-stu-id="41fd0-159">In hello **Access control records** blade, from hello tabular listing of hello access control records, double-click hello ACR that you wish toodelete.</span></span>

3. <span data-ttu-id="41fd0-160">Hello Szerkesztés hozzáférést vezérlő rekordok paneljén kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="41fd0-160">In hello Edit access control records blade, click **Delete**.</span></span>
   
    ![ACRS törlése](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. <span data-ttu-id="41fd0-162">Amikor felszólítja a megerősítésre, kattintson **törlése** toocontinue a hello törlését.</span><span class="sxs-lookup"><span data-stu-id="41fd0-162">When prompted for confirmation, click **Delete** toocontinue with hello deletion.</span></span> <span data-ttu-id="41fd0-163">hello táblázatos felsorolása frissített tooreflect hello törlésre.</span><span class="sxs-lookup"><span data-stu-id="41fd0-163">hello tabular listing is updated tooreflect hello deletion.</span></span>
   
   ![Figyelmeztető üzenet](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a><span data-ttu-id="41fd0-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41fd0-165">Next steps</span></span>

* <span data-ttu-id="41fd0-166">További információ [kötetek hozzáadásával és konfigurálásával ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span><span class="sxs-lookup"><span data-stu-id="41fd0-166">Learn more about [adding volumes and configuring ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).</span></span>

