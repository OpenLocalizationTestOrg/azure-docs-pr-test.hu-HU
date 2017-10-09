---
title: "a StorSimple virtuális tömb aaaManage kötetek |} Microsoft Docs"
description: "StorSimple Device Manager hello ismerteti és bemutatja hogyan toouse azt a StorSimple virtuális tömb toomanage köteteket."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 46aa6d7508b3e62f75a3b78ed73302b88320a0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-service-toomanage-volumes-on-hello-storsimple-virtual-array"></a><span data-ttu-id="b67b0-103">StorSimple az Eszközkezelő szolgáltatás toomanage kötetet a StorSimple virtuális tömb hello</span><span class="sxs-lookup"><span data-stu-id="b67b0-103">Use StorSimple Device Manager service toomanage volumes on hello StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="b67b0-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b67b0-104">Overview</span></span>

<span data-ttu-id="b67b0-105">Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás toocreate, és a StorSimple virtuális tömb kötetek kezelése.</span><span class="sxs-lookup"><span data-stu-id="b67b0-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage volumes on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="b67b0-106">StorSimple Device Manager szolgáltatás hello egy bővítmény hello Azure-portál, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését.</span><span class="sxs-lookup"><span data-stu-id="b67b0-106">hello StorSimple Device Manager service is an extension in hello Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="b67b0-107">Továbbá toomanaging megosztásokat és -kötetek, a is hello StorSimple Device Manager szolgáltatás tooview használja és eszközök kezeléséhez, megtekintheti a riasztásokat, és megtekinthető és kezelhető biztonsági mentési házirendek és hello biztonságimásolat-katalógus.</span><span class="sxs-lookup"><span data-stu-id="b67b0-107">In addition toomanaging shares and volumes, you can use hello StorSimple Device Manager service tooview and manage devices, view alerts, and view and manage backup policies and hello backup catalog.</span></span>

## <a name="volume-types"></a><span data-ttu-id="b67b0-108">Kötet típusok</span><span class="sxs-lookup"><span data-stu-id="b67b0-108">Volume Types</span></span>

<span data-ttu-id="b67b0-109">StorSimple-köteteket lehet:</span><span class="sxs-lookup"><span data-stu-id="b67b0-109">StorSimple volumes can be:</span></span>

* <span data-ttu-id="b67b0-110">**Helyileg rögzített**: ezek a kötetek adatainak mindig hello tömb marad, és nem kerülnek toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="b67b0-110">**Locally pinned**: Data in these volumes stays on hello array at all times and does not spill toohello cloud.</span></span>
* <span data-ttu-id="b67b0-111">**Rétegzett**: ezek a kötetek adatainak toohello felhő is kerülnek.</span><span class="sxs-lookup"><span data-stu-id="b67b0-111">**Tiered**: Data in these volumes can spill toohello cloud.</span></span> <span data-ttu-id="b67b0-112">Ha rétegzett kötetet hoz létre, körülbelül 10 %-a hello terület hello helyi rétegen van kiépítve és 90 % hello terület hello felhőben regisztráltak-e.</span><span class="sxs-lookup"><span data-stu-id="b67b0-112">When you create a tiered volume, approximately 10 % of hello space is provisioned on hello local tier and 90 % of hello space is provisioned in hello cloud.</span></span> <span data-ttu-id="b67b0-113">Például ha 1 TB-os kötet létesített, 100 GB hello helyi terület kellene lennie, és 900 GB lesz felhasználva a következőben hello felhő amikor hello adat szintet.</span><span class="sxs-lookup"><span data-stu-id="b67b0-113">For example, if you provisioned a 1 TB volume, 100 GB would reside in hello local space and 900 GB would be used in hello cloud when hello data tiers.</span></span> <span data-ttu-id="b67b0-114">Ez viszont azt jelenti, hogy minden hello helyi tárhely fogyjon hello eszközön, ha nem használhatók a rétegzett kötetek (mert hello 10 % szükséges, a helyi hello réteg nem lesz elérhető).</span><span class="sxs-lookup"><span data-stu-id="b67b0-114">This in turn implies that if you run out of all hello local space on hello device, you cannot provision a tiered volume (because hello 10 % required on hello local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="b67b0-115">Kiosztott kapacitást</span><span class="sxs-lookup"><span data-stu-id="b67b0-115">Provisioned capacity</span></span>
<span data-ttu-id="b67b0-116">Tekintse meg a következő táblázat az egyes mennyiségi maximális kiosztott kapacitást toohello.</span><span class="sxs-lookup"><span data-stu-id="b67b0-116">Refer toohello following table for maximum provisioned capacity for each volume type.</span></span>

| <span data-ttu-id="b67b0-117">**Korlát azonosítója**</span><span class="sxs-lookup"><span data-stu-id="b67b0-117">**Limit identifier**</span></span>                                       | <span data-ttu-id="b67b0-118">**Korlát**</span><span class="sxs-lookup"><span data-stu-id="b67b0-118">**Limit**</span></span>     |
|------------------------------------------------------------|---------------|
| <span data-ttu-id="b67b0-119">A rétegzett kötetek minimális mérete</span><span class="sxs-lookup"><span data-stu-id="b67b0-119">Minimum size of a tiered volume</span></span>                            | <span data-ttu-id="b67b0-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="b67b0-120">500 GB</span></span>        |
| <span data-ttu-id="b67b0-121">A rétegzett kötetek maximális mérete</span><span class="sxs-lookup"><span data-stu-id="b67b0-121">Maximum size of a tiered volume</span></span>                            | <span data-ttu-id="b67b0-122">5 TB</span><span class="sxs-lookup"><span data-stu-id="b67b0-122">5 TB</span></span>          |
| <span data-ttu-id="b67b0-123">Egy helyileg rögzített kötet minimális mérete</span><span class="sxs-lookup"><span data-stu-id="b67b0-123">Minimum size of a locally pinned volume</span></span>                    | <span data-ttu-id="b67b0-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="b67b0-124">50 GB</span></span>         |
| <span data-ttu-id="b67b0-125">Egy helyileg rögzített kötet maximális mérete</span><span class="sxs-lookup"><span data-stu-id="b67b0-125">Maximum size of a locally pinned volume</span></span>                    | <span data-ttu-id="b67b0-126">500 GB</span><span class="sxs-lookup"><span data-stu-id="b67b0-126">500 GB</span></span>        |

## <a name="hello-volumes-blade"></a><span data-ttu-id="b67b0-127">hello kötetek panel</span><span class="sxs-lookup"><span data-stu-id="b67b0-127">hello Volumes blade</span></span>
<span data-ttu-id="b67b0-128">Hello **kötetek** menü a StorSimple szolgáltatás összefoglaló panelen megjeleníti egy adott StorSimple tömb tárolási köteteket hello listát, és lehetővé teszi a toomanage őket.</span><span class="sxs-lookup"><span data-stu-id="b67b0-128">hello **Volumes** menu on your StorSimple service summary blade displays hello list of storage volumes on a given StorSimple array and allows you toomanage them.</span></span>

![Kötetek panel](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

<span data-ttu-id="b67b0-130">A kötet attribútumainak állnak:</span><span class="sxs-lookup"><span data-stu-id="b67b0-130">A volume consists of a series of attributes:</span></span>

* <span data-ttu-id="b67b0-131">**Kötet neve** – egy leíró nevet, amely egyedinek kell lennie, és azonosíthatja a hello kötet.</span><span class="sxs-lookup"><span data-stu-id="b67b0-131">**Volume Name** – A descriptive name that must be unique and helps identify hello volume.</span></span>
* <span data-ttu-id="b67b0-132">**Állapot** – lehet online vagy offline állapotú.</span><span class="sxs-lookup"><span data-stu-id="b67b0-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="b67b0-133">Ha egy kötet offline állapotban, nincs látható tooinitiators (kiszolgálók), amelyek számára engedélyezett a hozzáférés toouse hello kötet.</span><span class="sxs-lookup"><span data-stu-id="b67b0-133">If a volume is offline, it is not visible tooinitiators (servers) that are allowed access toouse hello volume.</span></span>
* <span data-ttu-id="b67b0-134">**Típus** – azt jelzi, hogy hello kötet **rétegzett** (alapértelmezett hello) vagy **helyileg rögzített**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-134">**Type** – Indicates whether hello volume is **Tiered** (hello default) or **Locally pinned**.</span></span>
* <span data-ttu-id="b67b0-135">**Kapacitás** – hello kezdeményező (kiszolgáló) tárolt adatok teljes mennyiségének összehasonlított toohello használt hello adatmennyiség határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b67b0-135">**Capacity** – specifies hello amount of data used as compared toohello total amount of data that can be stored by hello initiator (server).</span></span>
* <span data-ttu-id="b67b0-136">**Biztonsági mentési** – abban az esetben a StorSimple virtuális tömb hello, az összes kötegről automatikusan engedélyezve vannak a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="b67b0-136">**Backup** – In case of hello StorSimple Virtual Array, all volumes are automatically enabled for backup.</span></span>
* <span data-ttu-id="b67b0-137">**Kapcsolódó állomások** – Itt adhatja meg, amelyek számára engedélyezett a hozzáférés toothis kötet hello kezdeményezőktől (kiszolgálóktól).</span><span class="sxs-lookup"><span data-stu-id="b67b0-137">**Connected hosts** – Specifies hello initiators (servers) that are allowed access toothis volume.</span></span>

![Kötetek részletei](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

<span data-ttu-id="b67b0-139">Ez a következő feladatok oktatóanyag tooperform hello használata hello utasításait:</span><span class="sxs-lookup"><span data-stu-id="b67b0-139">Use hello instructions in this tutorial tooperform hello following tasks:</span></span>

* <span data-ttu-id="b67b0-140">Kötet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b67b0-140">Add a volume</span></span>
* <span data-ttu-id="b67b0-141">A kötet módosítása</span><span class="sxs-lookup"><span data-stu-id="b67b0-141">Modify a volume</span></span>
* <span data-ttu-id="b67b0-142">A kötet offline állapotba helyezése</span><span class="sxs-lookup"><span data-stu-id="b67b0-142">Take a volume offline</span></span>
* <span data-ttu-id="b67b0-143">Kötet törlése</span><span class="sxs-lookup"><span data-stu-id="b67b0-143">Delete a volume</span></span>

## <a name="add-a-volume"></a><span data-ttu-id="b67b0-144">Kötet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b67b0-144">Add a volume</span></span>

1. <span data-ttu-id="b67b0-145">Hello StorSimple szolgáltatás összefoglaló panelen, kattintson a **+ kötet hozzáadása** hello parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="b67b0-145">From hello StorSimple service summary blade, click **+ Add volume** from hello command bar.</span></span> <span data-ttu-id="b67b0-146">Ezzel megnyílik hello **kötet hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="b67b0-146">This opens up hello **Add volume** blade.</span></span>
   
    ![Kötet hozzáadása](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. <span data-ttu-id="b67b0-148">A hello **kötet hozzáadása** panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b67b0-148">In hello **Add volume** blade, do hello following:</span></span>
   
   * <span data-ttu-id="b67b0-149">A hello **kötetneve** mezőben adjon meg egy egyedi nevet a kötethez.</span><span class="sxs-lookup"><span data-stu-id="b67b0-149">In hello **Volume name** field, enter a unique name for your volume.</span></span> <span data-ttu-id="b67b0-150">hello neve 3 too127 karaktert tartalmazó karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b67b0-150">hello name must be a string that contains 3 too127 characters.</span></span>
   * <span data-ttu-id="b67b0-151">A hello **típus** legördülő listában, adja meg, hogy toocreate egy **rétegzett** vagy **helyileg rögzített** kötet.</span><span class="sxs-lookup"><span data-stu-id="b67b0-151">In hello **Type** dropdown list, specify whether toocreate a **Tiered** or **Locally pinned** volume.</span></span> <span data-ttu-id="b67b0-152">Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **helyileg rögzített kötet**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-152">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned volume**.</span></span> <span data-ttu-id="b67b0-153">Minden más adathoz válasszon **rétegzett** kötet.</span><span class="sxs-lookup"><span data-stu-id="b67b0-153">For all other data, select **Tiered** volume.</span></span>
   * <span data-ttu-id="b67b0-154">A hello **kapacitás** mezőben adja meg a hello hello kötet méretét.</span><span class="sxs-lookup"><span data-stu-id="b67b0-154">In hello **Capacity** field, specify hello size of hello volume.</span></span> <span data-ttu-id="b67b0-155">A rétegzett kötetek 500 GB és 5 TB között kell lennie, és egy helyileg rögzített kötet 50 GB-os és 500 GB között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b67b0-155">A tiered volume must be between 500 GB and 5 TB and a locally pinned volume must be between 50 GB and 500 GB.</span></span>
   * * <span data-ttu-id="b67b0-156">Kattintson a **csatlakozó állomások**, válassza ki a hozzáférési vezérlési rekordot (ACR) megfelelő toohello iSCSI-kezdeményező szeretné, hogy tooconnect toothis kötet, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-156">Click **Connected hosts**, select an access control record (ACR) corresponding toohello iSCSI initiator that you want tooconnect toothis volume, and then click **Select**.</span></span>
3. <span data-ttu-id="b67b0-157">új csatlakoztatott gazdagépek esetén tooadd kattintson **új hozzáadása**, adjon meg egy nevet a hello állomás és az iSCSI minősített nevét (IQN), és kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-157">tooadd a new connected host, click **Add new**, enter a name for hello host and its iSCSI Qualified Name (IQN), and then click **Add**.</span></span>
   
    ![Kötet hozzáadása](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. <span data-ttu-id="b67b0-159">Amikor elkészült, a kötet konfigurálása, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-159">When you've finished configuring your volume, click **Create**.</span></span> <span data-ttu-id="b67b0-160">A kötet jön létre a megadott hello beállításait, és megjelenik egy értesítés a hello sikeres létrehozása, hello azonos.</span><span class="sxs-lookup"><span data-stu-id="b67b0-160">A volume will be created with hello specified settings and you will see a notification on hello successful creation of hello same.</span></span> <span data-ttu-id="b67b0-161">Alapértelmezés szerint biztonsági mentés hello kötet engedélyezve lesz.</span><span class="sxs-lookup"><span data-stu-id="b67b0-161">By default backup will be enabled for hello volume.</span></span>
5. <span data-ttu-id="b67b0-162">amely a kötet hello tooconfirm lett sikeresen létrehozva, lépjen toohello **kötetek** panelen.</span><span class="sxs-lookup"><span data-stu-id="b67b0-162">tooconfirm that hello volume was successfully created, go toohello **Volumes** blade.</span></span> <span data-ttu-id="b67b0-163">Meg kell jelenniük a felsorolt hello kötetet.</span><span class="sxs-lookup"><span data-stu-id="b67b0-163">You should see hello volume listed.</span></span>
   
    ![Kötet létrehozása sikeres](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a><span data-ttu-id="b67b0-165">A kötet módosítása</span><span class="sxs-lookup"><span data-stu-id="b67b0-165">Modify a volume</span></span>

<span data-ttu-id="b67b0-166">A kötet módosításával toochange hello állomások hello kötet elérő van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b67b0-166">Modify a volume when you need toochange hello hosts that access hello volume.</span></span> <span data-ttu-id="b67b0-167">hello egyéb attribútumai a kötet nem módosítható hello kötet létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="b67b0-167">hello other attributes of a volume cannot be modified once hello volume has been created.</span></span>

#### <a name="toomodify-a-volume"></a><span data-ttu-id="b67b0-168">a kötet toomodify</span><span class="sxs-lookup"><span data-stu-id="b67b0-168">toomodify a volume</span></span>

1. <span data-ttu-id="b67b0-169">A hello **kötetek** hello StorSimple szolgáltatás összefoglaló panelen beállításnál válassza hello virtuális tömb mely hello toomodify kívánja meg kötet található.</span><span class="sxs-lookup"><span data-stu-id="b67b0-169">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you toomodify resides.</span></span>
2. <span data-ttu-id="b67b0-170">**Válassza ki** hello kötet, és kattintson a **csatlakozó állomások** tooview hello a jelenleg csatlakozó állomás, és módosítsa azt tooa másik kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="b67b0-170">**Select** hello volume and click **Connected hosts** tooview hello currently connected host and modify it tooa different server.</span></span>
   
    ![Kötet szerkesztése](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. <span data-ttu-id="b67b0-172">A módosítások mentéséhez kattintson a hello **mentése** parancssávon.</span><span class="sxs-lookup"><span data-stu-id="b67b0-172">Save your changes by clicking hello **Save** command bar.</span></span> <span data-ttu-id="b67b0-173">A megadott beállítások lesznek alkalmazva, és megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="b67b0-173">Your specified settings will be applied and you will see a notification.</span></span>

## <a name="take-a-volume-offline"></a><span data-ttu-id="b67b0-174">A kötet offline állapotba helyezése</span><span class="sxs-lookup"><span data-stu-id="b67b0-174">Take a volume offline</span></span>

<span data-ttu-id="b67b0-175">Szükség lehet egy kötet offline tootake tervezésekor toomodify vagy törölje azt.</span><span class="sxs-lookup"><span data-stu-id="b67b0-175">You may need tootake a volume offline when you are planning toomodify it or delete it.</span></span> <span data-ttu-id="b67b0-176">Ha egy kötet offline állapotban, nincs olvasási és írási hozzáférése érhető el.</span><span class="sxs-lookup"><span data-stu-id="b67b0-176">When a volume is offline, it is not available for read-write access.</span></span> <span data-ttu-id="b67b0-177">Szüksége lesz tootake hello kötet offline hello állomáson, valamint hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="b67b0-177">You will need tootake hello volume offline on hello host as well as on hello device.</span></span>

#### <a name="tootake-a-volume-offline"></a><span data-ttu-id="b67b0-178">a kötet offline tootake</span><span class="sxs-lookup"><span data-stu-id="b67b0-178">tootake a volume offline</span></span>

1. <span data-ttu-id="b67b0-179">Győződjön meg arról, hogy a szóban forgó hello kötet nem offline állapotba helyezése előtt használatban van.</span><span class="sxs-lookup"><span data-stu-id="b67b0-179">Make sure that hello volume in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="b67b0-180">Igénybe hello kötet offline hello állomás első.</span><span class="sxs-lookup"><span data-stu-id="b67b0-180">Take hello volume offline on hello host first.</span></span> <span data-ttu-id="b67b0-181">Ezzel a megoldással adatsérülés hello köteten lehetséges kockázatát.</span><span class="sxs-lookup"><span data-stu-id="b67b0-181">This eliminates any potential risk of data corruption on hello volume.</span></span> <span data-ttu-id="b67b0-182">Lépéseit tekintse meg a gazda operációs rendszer toohello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="b67b0-182">For specific steps, refer toohello instructions for your host operating system.</span></span>
3. <span data-ttu-id="b67b0-183">Miután hello gazdagépen hello kötet offline állapotban, igénybe hello kötet offline hello tömb hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="b67b0-183">After hello volume on hello host is offline, take hello volume on hello array  offline by performing hello following steps:</span></span>
   
   * <span data-ttu-id="b67b0-184">A hello **kötetek** hello StorSimple szolgáltatás összefoglaló panelen beállításnál válassza hello virtuális tömb mely hello tootake kapcsolat nélkül kívánja azt kötet található.</span><span class="sxs-lookup"><span data-stu-id="b67b0-184">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you tootake offline resides.</span></span>
   * <span data-ttu-id="b67b0-185">**Válassza ki** hello kötet, és kattintson a **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **offline állapotba**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-185">**Select** hello volume and click **...** (alternately right-click in this row) and from hello context menu, select **Take offline**.</span></span>
     
        ![Kapcsolat nélküli kötet](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * <span data-ttu-id="b67b0-187">Tekintse át a hello hello információkat **offline állapotba** panel és elfogadta hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="b67b0-187">Review hello information in hello **Take offline** blade and confirm your acceptance of hello operation.</span></span> <span data-ttu-id="b67b0-188">Kattintson a **offline állapotba** tootake hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="b67b0-188">Click **Take offline** tootake hello volume offline.</span></span> <span data-ttu-id="b67b0-189">Megjelenik egy értesítés, amely hello művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="b67b0-189">You will see a notification of hello operation in progress.</span></span>
   * <span data-ttu-id="b67b0-190">tooconfirm hello kötet sikeresen készült offline, nyissa meg toohello **kötetek** panelen.</span><span class="sxs-lookup"><span data-stu-id="b67b0-190">tooconfirm that hello volume was successfully taken offline, go toohello **Volumes** blade.</span></span> <span data-ttu-id="b67b0-191">Kapcsolat nélküli, meg kell jelennie hello hello kötetre vonatkozó állapotának.</span><span class="sxs-lookup"><span data-stu-id="b67b0-191">You should see hello status of hello volume as offline.</span></span>
     
       ![Kapcsolat nélküli kötet megerősítése](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a><span data-ttu-id="b67b0-193">Kötet törlése</span><span class="sxs-lookup"><span data-stu-id="b67b0-193">Delete a volume</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b67b0-194">Törölheti a kötet csak akkor, ha offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="b67b0-194">You can delete a volume only if it is offline.</span></span>
> 
> 

<span data-ttu-id="b67b0-195">Hajtsa végre a következő lépéseket toodelete kötet hello.</span><span class="sxs-lookup"><span data-stu-id="b67b0-195">Complete hello following steps toodelete a volume.</span></span>

#### <a name="toodelete-a-volume"></a><span data-ttu-id="b67b0-196">a kötet toodelete</span><span class="sxs-lookup"><span data-stu-id="b67b0-196">toodelete a volume</span></span>

1. <span data-ttu-id="b67b0-197">A hello **kötetek** hello StorSimple szolgáltatás összefoglaló panelen beállításnál válassza hello virtuális tömb mely hello toodelete kívánja meg kötet található.</span><span class="sxs-lookup"><span data-stu-id="b67b0-197">From hello **Volumes** setting on hello StorSimple service summary blade, select hello virtual array on which hello volume you wish you toodelete resides.</span></span>
2. <span data-ttu-id="b67b0-198">**Válassza ki** hello kötet, és kattintson a **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-198">**Select** hello volume and click **...** (alternately right-click in this row) and from hello context menu, select **Delete**.</span></span>
   
    ![Kötet törlése](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. <span data-ttu-id="b67b0-200">Hello állapotának hello kötet toodelete szeretné.</span><span class="sxs-lookup"><span data-stu-id="b67b0-200">Check hello status of hello volume you want toodelete.</span></span> <span data-ttu-id="b67b0-201">Ha azt szeretné, hogy toodelete hello kötet nem offline állapotban, végezze el az offline első, a következő hello lépéseket szereplő [kötet offline állapotba](#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="b67b0-201">If hello volume you want toodelete is not offline, take it offline first, following hello steps in [Take a volume offline](#take-a-volume-offline).</span></span>
4. <span data-ttu-id="b67b0-202">Ha a hello megerősítést kér **törlése** panelen fogadja el a hello megerősítő, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b67b0-202">When prompted for confirmation in hello **Delete** blade, accept hello confirmation and click **Delete**.</span></span> <span data-ttu-id="b67b0-203">a program törli hello kötet, és hello **kötetek** panelen megjelenik a kötetek hello virtuális tömbön belüli hello frissített listáját.</span><span class="sxs-lookup"><span data-stu-id="b67b0-203">hello volume will now be deleted and hello **Volumes** blade will show hello updated list of volumes within hello virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b67b0-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b67b0-204">Next steps</span></span>

<span data-ttu-id="b67b0-205">Ismerje meg, hogyan túl[klónozza a StorSimple-kötet](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="b67b0-205">Learn how too[clone a StorSimple volume](storsimple-virtual-array-clone.md).</span></span>

