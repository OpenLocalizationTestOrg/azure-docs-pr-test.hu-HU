---
title: "a StorSimple aaaManage hozzáférés-vezérlési rekordokat |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hozzáférés-vezérlés rögzíti, mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszköz (ACRs) toodetermine."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a1e718c2679301b34221a233557a1eaae869a94f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a><span data-ttu-id="efdc8-103">Hello StorSimple Manager szolgáltatás toomanage access control rekordok használata</span><span class="sxs-lookup"><span data-stu-id="efdc8-103">Use hello StorSimple Manager service toomanage access control records</span></span>
## <a name="overview"></a><span data-ttu-id="efdc8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="efdc8-104">Overview</span></span>
<span data-ttu-id="efdc8-105">Hozzáférés-vezérlési rekordokat (ACRs) lehetővé teszik toospecify mely állomásokat kapcsolódhatnak tooa kötet hello StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="efdc8-105">Access control records (ACRs) allow you toospecify which hosts can connect tooa volume on hello StorSimple device.</span></span> <span data-ttu-id="efdc8-106">ACRs tooa adott kötet van beállítva, és tartalmazhat hello iSCSI minősített nevét (IQN-nevekre vonatkozóan) hello gazdagépek.</span><span class="sxs-lookup"><span data-stu-id="efdc8-106">ACRs are set tooa specific volume and contain hello iSCSI Qualified Names (IQNs) of hello hosts.</span></span> <span data-ttu-id="efdc8-107">Ha egy állomás megpróbál tooconnect tooa kötet, hello eszköz ellenőrzi hello ACR társított hello IQN nevét, és ha egyezést, majd hello kapcsolat jön létre a köteten.</span><span class="sxs-lookup"><span data-stu-id="efdc8-107">When a host tries tooconnect tooa volume, hello device checks hello ACR associated with that volume for hello IQN name and if there is a match, then hello connection is established.</span></span> <span data-ttu-id="efdc8-108">hozzáférés-vezérlés hello rögzíti a hello szakasz **konfigurálása** oldal megjelenik az összes hello hozzáférés-vezérlési rekordokat hello megfelelő hello állomás IQN-nevekre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="efdc8-108">hello access control records section on hello **Configure** page displays all hello access control records with hello corresponding IQNs of hello hosts.</span></span>

<span data-ttu-id="efdc8-109">Ez az oktatóanyag azt ismerteti, a következő ACR kapcsolatos általános feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="efdc8-109">This tutorial explains hello following common ACR-related tasks:</span></span>

* <span data-ttu-id="efdc8-110">Egy hozzáférés-vezérlési rekordot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="efdc8-110">Add an access control record</span></span> 
* <span data-ttu-id="efdc8-111">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="efdc8-111">Edit an access control record</span></span> 
* <span data-ttu-id="efdc8-112">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="efdc8-112">Delete an access control record</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="efdc8-113">Az ACR tooa kötet hozzárendelésekor gondoskodunk, hogy hello kötet nem egyidejűleg hozzáfér egynél több nem fürtözött gazdagép, mert ez hello kötet, megsérülhet.</span><span class="sxs-lookup"><span data-stu-id="efdc8-113">When assigning an ACR tooa volume, take care that hello volume is not concurrently accessed by more than one non-clustered host because this could corrupt hello volume.</span></span> 
> * <span data-ttu-id="efdc8-114">Ha töröl egy ACR a kötetről, ellenőrizze, hogy adott hello megfelelő gazdagép nem fér hozzá hello kötet mert hello törlését eredményezheti egy írható-olvasható megszakítása.</span><span class="sxs-lookup"><span data-stu-id="efdc8-114">When deleting an ACR from a volume, make sure that hello corresponding host is not accessing hello volume because hello deletion could result in a read-write disruption.</span></span>
> 
> 

## <a name="add-an-access-control-record"></a><span data-ttu-id="efdc8-115">Egy hozzáférés-vezérlési rekordot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="efdc8-115">Add an access control record</span></span>
<span data-ttu-id="efdc8-116">Hello StorSimple Manager szolgáltatás használ **konfigurálása** tooadd ACRs lapon.</span><span class="sxs-lookup"><span data-stu-id="efdc8-116">You use hello StorSimple Manager service **Configure** page tooadd ACRs.</span></span> <span data-ttu-id="efdc8-117">Általában egy ACR fog társítani egy kötetet.</span><span class="sxs-lookup"><span data-stu-id="efdc8-117">Typically, you will associate one ACR with one volume.</span></span>

<span data-ttu-id="efdc8-118">Hajtsa végre a következő lépéseket tooadd egy ACR hello.</span><span class="sxs-lookup"><span data-stu-id="efdc8-118">Perform hello following steps tooadd an ACR.</span></span>

#### <a name="tooadd-an-access-control-record"></a><span data-ttu-id="efdc8-119">egy hozzáférés-vezérlési rekordot tooadd</span><span class="sxs-lookup"><span data-stu-id="efdc8-119">tooadd an access control record</span></span>
1. <span data-ttu-id="efdc8-120">Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="efdc8-120">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="efdc8-121">A táblázatos szerint felsoroló hello **hozzáférés-vezérlési rekordokat**, szállítási egy **neve** az ACR.</span><span class="sxs-lookup"><span data-stu-id="efdc8-121">In hello tabular listing under **Access control records**, supply a **Name** for your ACR.</span></span>
3. <span data-ttu-id="efdc8-122">Adja meg a Windows-gazdagépen hello IQN nevét **iSCSI-kezdeményező neve**.</span><span class="sxs-lookup"><span data-stu-id="efdc8-122">Provide hello IQN name of your Windows host under **iSCSI Initiator Name**.</span></span> <span data-ttu-id="efdc8-123">a Windows Server-állomás IQN-Nevének tooget hello hello a következő:</span><span class="sxs-lookup"><span data-stu-id="efdc8-123">tooget hello IQN of your Windows Server host, do hello following:</span></span>
   
   * <span data-ttu-id="efdc8-124">Indítsa el a hello Microsoft iSCSI-kezdeményezőt a Windows-gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="efdc8-124">Start hello Microsoft iSCSI initiator on your Windows host.</span></span>
   * <span data-ttu-id="efdc8-125">A hello **iSCSI-kezdeményező tulajdonságai** ablakban a hello **konfigurációs** lapra, válassza ki, másolja hello karakterlánc hello **kezdeményező neve** mező.</span><span class="sxs-lookup"><span data-stu-id="efdc8-125">In hello **iSCSI Initiator Properties** window, on hello **Configuration** tab, select and copy hello string from hello **Initiator Name** field.</span></span>
   * <span data-ttu-id="efdc8-126">Ez a karakterlánc illessze be hello **iSCSI-kezdeményező neve** mezőjét a klasszikus Azure portálon hello hello ACRs táblán.</span><span class="sxs-lookup"><span data-stu-id="efdc8-126">Paste this string in hello **iSCSI Initiator Name** field on hello ACRs table in hello Azure classic portal.</span></span>
4. <span data-ttu-id="efdc8-127">Kattintson a **mentése** toosave hello ACR, újonnan létrehozott.</span><span class="sxs-lookup"><span data-stu-id="efdc8-127">Click **Save** toosave hello newly created ACR.</span></span> <span data-ttu-id="efdc8-128">hello táblázatos lesz listázása kell frissített tooreflect a hozzáadást.</span><span class="sxs-lookup"><span data-stu-id="efdc8-128">hello tabular listing will be updated tooreflect this addition.</span></span>

## <a name="edit-an-access-control-record"></a><span data-ttu-id="efdc8-129">Egy hozzáférés-vezérlési rekordot szerkesztése</span><span class="sxs-lookup"><span data-stu-id="efdc8-129">Edit an access control record</span></span>
<span data-ttu-id="efdc8-130">Hello használata **konfigurálása** hello Azure klasszikus portál tooedit ACRs lapján.</span><span class="sxs-lookup"><span data-stu-id="efdc8-130">You use hello **Configure** page in hello Azure classic portal tooedit ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="efdc8-131">Csak a jelenleg nem használható ACRs módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="efdc8-131">You can modify only those ACRs that are currently not in use.</span></span> <span data-ttu-id="efdc8-132">tooedit egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="efdc8-132">tooedit an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>
> 
> 

<span data-ttu-id="efdc8-133">Hajtsa végre a következő lépéseket tooedit egy ACR hello.</span><span class="sxs-lookup"><span data-stu-id="efdc8-133">Perform hello following steps tooedit an ACR.</span></span>

#### <a name="tooedit-an-access-control-record"></a><span data-ttu-id="efdc8-134">egy hozzáférés-vezérlési rekordot tooedit</span><span class="sxs-lookup"><span data-stu-id="efdc8-134">tooedit an access control record</span></span>
1. <span data-ttu-id="efdc8-135">Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="efdc8-135">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="efdc8-136">Hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat, a hello ACR, hogy kívánja-e toomodify mutasson.</span><span class="sxs-lookup"><span data-stu-id="efdc8-136">In hello tabular listing of hello access control records, hover over hello ACR that you wish toomodify.</span></span>
3. <span data-ttu-id="efdc8-137">Adjon meg egy új nevet és/vagy IQN a hello ACR.</span><span class="sxs-lookup"><span data-stu-id="efdc8-137">Supply a new name and/or IQN for hello ACR.</span></span>
4. <span data-ttu-id="efdc8-138">Kattintson a **mentése** toosave hello ACR módosítani.</span><span class="sxs-lookup"><span data-stu-id="efdc8-138">Click **Save** toosave hello modified ACR.</span></span> <span data-ttu-id="efdc8-139">hello táblázatos lesz listázása kell frissített tooreflect ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="efdc8-139">hello tabular listing will be updated tooreflect this change.</span></span>

## <a name="delete-an-access-control-record"></a><span data-ttu-id="efdc8-140">Egy hozzáférés-vezérlési rekordot törlése</span><span class="sxs-lookup"><span data-stu-id="efdc8-140">Delete an access control record</span></span>
<span data-ttu-id="efdc8-141">Hello használata **konfigurálása** hello Azure klasszikus portál toodelete ACRs lapján.</span><span class="sxs-lookup"><span data-stu-id="efdc8-141">You use hello **Configure** page in hello Azure classic portal toodelete ACRs.</span></span> 

> [!NOTE]
> <span data-ttu-id="efdc8-142">Csak azokat a ACRs, amelyek jelenleg nem törölhetők.</span><span class="sxs-lookup"><span data-stu-id="efdc8-142">You can delete only those ACRs that are currently not in use.</span></span> <span data-ttu-id="efdc8-143">toodelete egy ACR társított olyan kötet, amely jelenleg használatban van, először érdemes hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="efdc8-143">toodelete an ACR associated with a volume that is currently in use, you must first take hello volume offline.</span></span>
> 
> 

<span data-ttu-id="efdc8-144">Hajtsa végre a következő lépéseket toodelete egy hozzáférés-vezérlési rekordot hello.</span><span class="sxs-lookup"><span data-stu-id="efdc8-144">Perform hello following steps toodelete an access control record.</span></span>

#### <a name="toodelete-an-access-control-record"></a><span data-ttu-id="efdc8-145">egy hozzáférés-vezérlési rekordot toodelete</span><span class="sxs-lookup"><span data-stu-id="efdc8-145">toodelete an access control record</span></span>
1. <span data-ttu-id="efdc8-146">Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="efdc8-146">On hello service landing page, select your service, double-click hello service name, and then click hello **Configure** tab.</span></span>
2. <span data-ttu-id="efdc8-147">A hello táblázatos felsorolása hello hozzáférés-vezérlési rekordokat (ACRs), mutasson, hogy kívánja-e toodelete ACR hello.</span><span class="sxs-lookup"><span data-stu-id="efdc8-147">In hello tabular listing of hello access control records (ACRs), hover over hello ACR that you wish toodelete.</span></span>
3. <span data-ttu-id="efdc8-148">A Törlés ikonra (**x**) hello szélsőséges jobb oldali oszlopban hello kiválasztott ACR fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="efdc8-148">A delete icon (**x**) will appear in hello extreme right column for hello ACR that you select.</span></span> <span data-ttu-id="efdc8-149">Kattintson a hello **x** ikon toodelete hello ACR.</span><span class="sxs-lookup"><span data-stu-id="efdc8-149">Click hello **x** icon toodelete hello ACR.</span></span>
4. <span data-ttu-id="efdc8-150">Amikor felszólítja a megerősítésre, kattintson **Igen** toocontinue a hello törlését.</span><span class="sxs-lookup"><span data-stu-id="efdc8-150">When prompted for confirmation, click **YES** toocontinue with hello deletion.</span></span> <span data-ttu-id="efdc8-151">hello táblázatos felsorolása frissített tooreflect hello törlés lesz.</span><span class="sxs-lookup"><span data-stu-id="efdc8-151">hello tabular listing will be updated tooreflect hello deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="efdc8-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="efdc8-152">Next steps</span></span>
* <span data-ttu-id="efdc8-153">További információ [felügyelete a StorSimple-köteteket](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="efdc8-153">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="efdc8-154">További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="efdc8-154">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

