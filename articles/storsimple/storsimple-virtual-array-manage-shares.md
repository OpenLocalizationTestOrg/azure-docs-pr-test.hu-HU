---
title: "aaaManage StorSimple virtuális tömb felhőszolgáltatásaival |} Microsoft Docs"
description: "StorSimple Device Manager hello ismerteti és bemutatja hogyan toouse azt a StorSimple virtuális tömb toomanage megosztásokat."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9b57d7ec7c0b7de5a22e1b816daa8852d0f32a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-shares-on-hello-storsimple-virtual-array"></a><span data-ttu-id="615b5-103">A StorSimple virtuális tömb hello hello StorSimple Device Manager szolgáltatás toomanage megosztásokat használata</span><span class="sxs-lookup"><span data-stu-id="615b5-103">Use hello StorSimple Device Manager service toomanage shares on hello StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="615b5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="615b5-104">Overview</span></span>

<span data-ttu-id="615b5-105">Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás toocreate, és a StorSimple virtuális tömb-megosztások kezelése.</span><span class="sxs-lookup"><span data-stu-id="615b5-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage shares on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="615b5-106">StorSimple Device Manager szolgáltatás hello egy bővítmény hello Azure-portál, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését.</span><span class="sxs-lookup"><span data-stu-id="615b5-106">hello StorSimple Device Manager service is an extension in hello Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="615b5-107">Toomanaging megosztások hozzáadását és a kötetek akkor is hello StorSimple Device Manager szolgáltatás tooview használja és eszközök kezeléséhez, megtekintheti a riasztásokat, biztonsági mentési házirendek, valamint kezelheti és hello biztonságimásolat-katalógus.</span><span class="sxs-lookup"><span data-stu-id="615b5-107">In addition toomanaging shares and volumes, you can use hello StorSimple Device Manager service tooview and manage devices, view alerts, manage backup policies, and manage hello backup catalog.</span></span>

## <a name="share-types"></a><span data-ttu-id="615b5-108">Megosztás típusok</span><span class="sxs-lookup"><span data-stu-id="615b5-108">Share Types</span></span>

<span data-ttu-id="615b5-109">StorSimple megosztások lehet:</span><span class="sxs-lookup"><span data-stu-id="615b5-109">StorSimple shares can be:</span></span>

* <span data-ttu-id="615b5-110">**Helyileg rögzített**: ezek az adatok mindig hello tömb marad, és nem kerülnek toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="615b5-110">**Locally pinned**: Data in these shares stays on hello array at all times and does not spill toohello cloud.</span></span>
* <span data-ttu-id="615b5-111">**Rétegzett**: ezek az adatok toohello felhő is kerülnek.</span><span class="sxs-lookup"><span data-stu-id="615b5-111">**Tiered**: Data in these shares can spill toohello cloud.</span></span> <span data-ttu-id="615b5-112">Egy rétegzett megosztás létrehozásakor körülbelül 10 %-a hello terület hello helyi rétegen ki van építve, és 90 % hello terület hello felhőben lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="615b5-112">When you create a tiered share, approximately 10 % of hello space is provisioned on hello local tier and 90 % of hello space is provisioned in hello cloud.</span></span> <span data-ttu-id="615b5-113">Például ha egy 1 TB-os megosztás létesített, 100 GB hello helyi terület kellene lennie, és 900 GB lesz felhasználva a következőben hello felhő amikor hello adat szintet.</span><span class="sxs-lookup"><span data-stu-id="615b5-113">For example, if you provisioned a 1 TB share, 100 GB would reside in hello local space and 900 GB would be used in hello cloud when hello data tiers.</span></span> <span data-ttu-id="615b5-114">Ez viszont azt jelenti, hogy minden hello helyi tárhely fogyjon hello eszközön, ha nem használhatók a rétegzett megosztás (mert hello 10 % szükséges, a helyi hello réteg nem lesz elérhető).</span><span class="sxs-lookup"><span data-stu-id="615b5-114">This in turn implies that if you run out of all hello local space on hello device, you cannot provision a tiered share (because hello 10 % required on hello local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="615b5-115">Kiosztott kapacitást</span><span class="sxs-lookup"><span data-stu-id="615b5-115">Provisioned capacity</span></span>

<span data-ttu-id="615b5-116">Tekintse meg a következő táblázat az egyes megosztás maximális kiosztott kapacitást toohello.</span><span class="sxs-lookup"><span data-stu-id="615b5-116">Refer toohello following table for maximum provisioned capacity for each share type.</span></span>

| <span data-ttu-id="615b5-117">**Korlát azonosítója**</span><span class="sxs-lookup"><span data-stu-id="615b5-117">**Limit identifier**</span></span> | <span data-ttu-id="615b5-118">**Korlát**</span><span class="sxs-lookup"><span data-stu-id="615b5-118">**Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="615b5-119">A rétegzett megosztás minimális mérete</span><span class="sxs-lookup"><span data-stu-id="615b5-119">Minimum size of a tiered share</span></span> |<span data-ttu-id="615b5-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="615b5-120">500 GB</span></span> |
| <span data-ttu-id="615b5-121">A rétegzett megosztás maximális mérete</span><span class="sxs-lookup"><span data-stu-id="615b5-121">Maximum size of a tiered share</span></span> |<span data-ttu-id="615b5-122">20 TB</span><span class="sxs-lookup"><span data-stu-id="615b5-122">20 TB</span></span> |
| <span data-ttu-id="615b5-123">Egy helyileg rögzített megosztás minimális mérete</span><span class="sxs-lookup"><span data-stu-id="615b5-123">Minimum size of a locally pinned share</span></span> |<span data-ttu-id="615b5-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="615b5-124">50 GB</span></span> |
| <span data-ttu-id="615b5-125">Egy helyileg rögzített megosztás maximális mérete</span><span class="sxs-lookup"><span data-stu-id="615b5-125">Maximum size of a locally pinned share</span></span> |<span data-ttu-id="615b5-126">2 TB</span><span class="sxs-lookup"><span data-stu-id="615b5-126">2 TB</span></span> |

## <a name="hello-shares-blade"></a><span data-ttu-id="615b5-127">hello megosztások panel</span><span class="sxs-lookup"><span data-stu-id="615b5-127">hello Shares blade</span></span>

<span data-ttu-id="615b5-128">Hello **megosztások** menü a StorSimple szolgáltatás összefoglaló panelen megjeleníti egy adott StorSimple tömb storage-megosztásokat hello listát, és lehetővé teszi a toomanage őket.</span><span class="sxs-lookup"><span data-stu-id="615b5-128">hello **Shares** menu on your StorSimple service summary blade displays hello list of storage shares on a given StorSimple array and allows you toomanage them.</span></span>

![Megosztások panel](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

<span data-ttu-id="615b5-130">A megosztás sorozatából attribútumok:</span><span class="sxs-lookup"><span data-stu-id="615b5-130">A share consists of a series of attributes:</span></span>

* <span data-ttu-id="615b5-131">**Megosztási név** – egy leíró nevet, amely egyedinek kell lennie, és azonosíthatja a hello megosztást.</span><span class="sxs-lookup"><span data-stu-id="615b5-131">**Share Name** – A descriptive name that must be unique and helps identify hello share.</span></span>
* <span data-ttu-id="615b5-132">**Állapot** – lehet online vagy offline állapotú.</span><span class="sxs-lookup"><span data-stu-id="615b5-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="615b5-133">Ha egy megosztást offline állapotban, hello megosztás felhasználóira nem lesz képes tooaccess azt.</span><span class="sxs-lookup"><span data-stu-id="615b5-133">If a share is offline, users of hello share will not be able tooaccess it.</span></span>
* <span data-ttu-id="615b5-134">**Típus** – azt jelzi, hogy hello megosztás **rétegzett** (alapértelmezett hello) vagy **helyileg rögzített**.</span><span class="sxs-lookup"><span data-stu-id="615b5-134">**Type** – Indicates whether hello share is **Tiered** (hello default) or **Locally pinned**.</span></span>
* <span data-ttu-id="615b5-135">**Kapacitás** – hello megosztáson tárolt adatok teljes mennyiségének összehasonlított toohello használt hello adatmennyiség határozza meg.</span><span class="sxs-lookup"><span data-stu-id="615b5-135">**Capacity** – specifies hello amount of data used as compared toohello total amount of data that can be stored on hello share.</span></span>
* <span data-ttu-id="615b5-136">**Leírás** – választható beállítás, amely segít a hello megosztás leírása.</span><span class="sxs-lookup"><span data-stu-id="615b5-136">**Description** – An optional setting that helps describe hello share.</span></span>
* <span data-ttu-id="615b5-137">**Engedélyek** -hello NTFS engedélyek toohello megosztást, amelyet a Windows Explorer használatával kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="615b5-137">**Permissions** - hello NTFS permissions toohello share that can be managed through Windows Explorer.</span></span>
* <span data-ttu-id="615b5-138">**Biztonsági mentési** – abban az esetben a StorSimple virtuális tömb hello, minden megosztás automatikusan engedélyezve vannak a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="615b5-138">**Backup** – In case of hello StorSimple Virtual Array, all shares are automatically enabled for backup.</span></span>

![Megosztások részletei](./media/storsimple-virtual-array-manage-shares/share-details.png)

<span data-ttu-id="615b5-140">Ez a következő feladatok oktatóanyag tooperform hello használata hello utasításait:</span><span class="sxs-lookup"><span data-stu-id="615b5-140">Use hello instructions in this tutorial tooperform hello following tasks:</span></span>

* <span data-ttu-id="615b5-141">Megosztás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="615b5-141">Add a share</span></span>
* <span data-ttu-id="615b5-142">Egy megosztás módosítása</span><span class="sxs-lookup"><span data-stu-id="615b5-142">Modify a share</span></span>
* <span data-ttu-id="615b5-143">Egy kapcsolat nélküli üzemmódra állítása</span><span class="sxs-lookup"><span data-stu-id="615b5-143">Take a share offline</span></span>
* <span data-ttu-id="615b5-144">Megosztás törlése</span><span class="sxs-lookup"><span data-stu-id="615b5-144">Delete a share</span></span>

## <a name="add-a-share"></a><span data-ttu-id="615b5-145">Megosztás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="615b5-145">Add a share</span></span>

1. <span data-ttu-id="615b5-146">Hello StorSimple szolgáltatás összefoglaló panelen, kattintson a **+ Hozzáadás megosztás** hello parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="615b5-146">From hello StorSimple service summary blade, click **+ Add share** from hello command bar.</span></span> <span data-ttu-id="615b5-147">Ezzel megnyílik hello **Hozzáadás megosztás** panelen.</span><span class="sxs-lookup"><span data-stu-id="615b5-147">This opens up hello **Add share** blade.</span></span>

    ![Adja hozzá a megosztás](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. <span data-ttu-id="615b5-149">A hello **Hozzáadás megosztás** panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="615b5-149">In hello **Add share** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="615b5-150">A hello **megosztásnevet** mezőben adjon meg egy egyedi nevet a megosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="615b5-150">In hello **Share name** field, enter a unique name for your share.</span></span> <span data-ttu-id="615b5-151">hello neve 3 too127 karaktert tartalmazó karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="615b5-151">hello name must be a string that contains 3 too127 characters.</span></span>

    2. <span data-ttu-id="615b5-152">Egy nem kötelező **leírás** hello megosztás.</span><span class="sxs-lookup"><span data-stu-id="615b5-152">An optional **Description** for hello share.</span></span> <span data-ttu-id="615b5-153">hello leírás azonosításához hello fájlmegosztás-tulajdonosok.</span><span class="sxs-lookup"><span data-stu-id="615b5-153">hello description will help identify hello share owners.</span></span>

    3. <span data-ttu-id="615b5-154">A hello **típus** legördülő listában, adja meg, hogy toocreate egy **rétegzett** vagy **helyileg rögzített** megosztani.</span><span class="sxs-lookup"><span data-stu-id="615b5-154">In hello **Type** dropdown list, specify whether toocreate a **Tiered** or **Locally pinned** share.</span></span> <span data-ttu-id="615b5-155">Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **helyileg rögzített megosztás**.</span><span class="sxs-lookup"><span data-stu-id="615b5-155">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned share**.</span></span> <span data-ttu-id="615b5-156">Minden más adathoz válasszon **rétegzett** megosztani.</span><span class="sxs-lookup"><span data-stu-id="615b5-156">For all other data, select **Tiered** share.</span></span>

    4. <span data-ttu-id="615b5-157">A hello **kapacitás** mezőben adja meg a hello megosztás hello méretét.</span><span class="sxs-lookup"><span data-stu-id="615b5-157">In hello **Capacity** field, specify hello size of hello share.</span></span> <span data-ttu-id="615b5-158">A rétegzett megosztás 500 GB és 20 TB között kell lennie, és egy helyileg rögzített megosztás 50 GB-os és a 2 TB közé kell esnie.</span><span class="sxs-lookup"><span data-stu-id="615b5-158">A tiered share must be between 500 GB and 20 TB and a locally pinned share must be between 50 GB and 2 TB.</span></span>

    5. <span data-ttu-id="615b5-159">A hello **értékre alapértelmezett teljes körű engedélyekkel** mezőbe hello engedélyek toohello felhasználói vagy fér hozzá a megosztás hello csoport hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="615b5-159">In hello **Set default full permissions to** field, assign hello permissions toohello user, or hello group that is accessing this share.</span></span> <span data-ttu-id="615b5-160">Adja meg a hello felhasználó vagy felhasználói csoport hello hello nevét  _john@contoso.com_  formátumban.</span><span class="sxs-lookup"><span data-stu-id="615b5-160">Specify hello name of hello user or hello user group in _john@contoso.com_ format.</span></span> <span data-ttu-id="615b5-161">Javasoljuk, hogy használjon egy felhasználói csoport (helyett egy-egy felhasználóhoz) tooallow rendszergazdai jogosultságokkal tooaccess megosztást.</span><span class="sxs-lookup"><span data-stu-id="615b5-161">We recommend that you use a user group (instead of a single user) tooallow admin privileges tooaccess these shares.</span></span> <span data-ttu-id="615b5-162">Miután itt hello engedélyek hozzárendelt, használhatja a Fájlkezelőben toomodify ezeket az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="615b5-162">After you have assigned hello permissions here, you can then use File Explorer toomodify these permissions.</span></span>
3. <span data-ttu-id="615b5-163">Amikor elkészült, a megosztás konfigurálására, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="615b5-163">When you've finished configuring your share, click **Create**.</span></span> <span data-ttu-id="615b5-164">A megosztás jön létre a megadott hello beállításait, és megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="615b5-164">A share will be created with hello specified settings and you will see a notification.</span></span> <span data-ttu-id="615b5-165">Alapértelmezés szerint biztonsági mentés hello megosztás engedélyezve lesz.</span><span class="sxs-lookup"><span data-stu-id="615b5-165">By default, backup will be enabled for hello share.</span></span>
4. <span data-ttu-id="615b5-166">tooconfirm, hogy a megosztás hello lett sikeresen létrehozva, lépjen toohello **megosztások** panelen.</span><span class="sxs-lookup"><span data-stu-id="615b5-166">tooconfirm that hello share was successfully created, go toohello **Shares** blade.</span></span> <span data-ttu-id="615b5-167">Megosztás felsorolt hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="615b5-167">You should see hello share listed.</span></span>
   
    ![Fájlmegosztás létrehozása sikerült](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a><span data-ttu-id="615b5-169">Egy megosztás módosítása</span><span class="sxs-lookup"><span data-stu-id="615b5-169">Modify a share</span></span>

<span data-ttu-id="615b5-170">A megosztás módosításával hello megosztás toochange hello leírása van szüksége.</span><span class="sxs-lookup"><span data-stu-id="615b5-170">Modify a share when you need toochange hello description of hello share.</span></span> <span data-ttu-id="615b5-171">Nincs más tulajdonságok hello megosztás létrehozása után módosítható.</span><span class="sxs-lookup"><span data-stu-id="615b5-171">No other share properties can be modified once hello share is created.</span></span>

#### <a name="toomodify-a-share"></a><span data-ttu-id="615b5-172">a megosztás toomodify</span><span class="sxs-lookup"><span data-stu-id="615b5-172">toomodify a share</span></span>

1. <span data-ttu-id="615b5-173">A hello **megosztások** válasszon hello beállítása hello StorSimple szolgáltatás összefoglaló panelre, mely hello toomodify kívánja meg megosztás található virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="615b5-173">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish you toomodify resides.</span></span>
2. <span data-ttu-id="615b5-174">**Válassza ki** hello megosztás tooview hello aktuális leírása, és módosítsa azt.</span><span class="sxs-lookup"><span data-stu-id="615b5-174">**Select** hello share tooview hello current description and modify it.</span></span>
3. <span data-ttu-id="615b5-175">A módosítások mentéséhez kattintson a hello **mentése** parancssávon.</span><span class="sxs-lookup"><span data-stu-id="615b5-175">Save your changes by clicking hello **Save** command bar.</span></span> <span data-ttu-id="615b5-176">A megadott beállítások lesznek alkalmazva, és megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="615b5-176">Your specified settings will be applied and you will see a notification.</span></span>
   
    ![ <span data-ttu-id="615b5-177">Megosztás módosítása</span><span class="sxs-lookup"><span data-stu-id="615b5-177">Edit share</span></span>](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a><span data-ttu-id="615b5-178">Egy kapcsolat nélküli üzemmódra állítása</span><span class="sxs-lookup"><span data-stu-id="615b5-178">Take a share offline</span></span>

<span data-ttu-id="615b5-179">Szükség lehet a megosztás nélküli tootake tervezésekor toomodify vagy törölje azt.</span><span class="sxs-lookup"><span data-stu-id="615b5-179">You may need tootake a share offline when you are planning toomodify it or delete it.</span></span> <span data-ttu-id="615b5-180">Ha egy megosztást offline állapotban, nincs olvasási és írási hozzáférése érhető el.</span><span class="sxs-lookup"><span data-stu-id="615b5-180">When a share is offline, it is not available for read-write access.</span></span> <span data-ttu-id="615b5-181">Szüksége lesz tootake hello megosztás nélküli hello állomáson, valamint hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="615b5-181">You will need tootake hello share offline on hello host as well as on hello device.</span></span>

#### <a name="tootake-a-share-offline"></a><span data-ttu-id="615b5-182">a megosztás nélküli tootake</span><span class="sxs-lookup"><span data-stu-id="615b5-182">tootake a share offline</span></span>

1. <span data-ttu-id="615b5-183">Győződjön meg arról, hogy a szóban forgó hello megosztás nem offline állapotba helyezése előtt használatban van.</span><span class="sxs-lookup"><span data-stu-id="615b5-183">Make sure that hello share in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="615b5-184">Hello megosztás végrehajtása a kapcsolat nélküli hello tömb hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="615b5-184">Take hello share on hello array offline by performing hello following steps:</span></span>
   
    1. <span data-ttu-id="615b5-185">A hello **megosztások** válasszon hello beállítása hello StorSimple szolgáltatás összefoglaló panelre, mely hello tootake kapcsolat nélkül kívánja azt megosztás található virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="615b5-185">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish you tootake offline resides.</span></span>

    2. <span data-ttu-id="615b5-186">**Válassza ki** hello megosztást, és kattintson **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **offline állapotba**.</span><span class="sxs-lookup"><span data-stu-id="615b5-186">**Select** hello share and Click **...** (alternately right-click in this row) and from hello context menu, select **Take offline**.</span></span>
     
        ![Kapcsolat nélküli megosztás](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. <span data-ttu-id="615b5-188">Tekintse át a hello hello információkat **offline állapotba** panel és elfogadta hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="615b5-188">Review hello information in hello **Take offline** blade and confirm your acceptance of hello operation.</span></span> <span data-ttu-id="615b5-189">Kattintson a **offline állapotba** tootake hello offline megosztást.</span><span class="sxs-lookup"><span data-stu-id="615b5-189">Click **Take offline** tootake hello share offline.</span></span> <span data-ttu-id="615b5-190">Megjelenik egy értesítés, amely hello művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="615b5-190">You will see a notification of hello operation in progress.</span></span>

    4. <span data-ttu-id="615b5-191">tooconfirm, hogy a megosztás hello sikeresen készült offline, lépjen toohello **megosztások** panelen.</span><span class="sxs-lookup"><span data-stu-id="615b5-191">tooconfirm that hello share was successfully taken offline, go toohello **Shares** blade.</span></span> <span data-ttu-id="615b5-192">Hello állapotának hello megosztás offline állapotúként kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="615b5-192">You should see hello status of hello share as offline.</span></span>

## <a name="delete-a-share"></a><span data-ttu-id="615b5-193">Megosztás törlése</span><span class="sxs-lookup"><span data-stu-id="615b5-193">Delete a share</span></span>

> [!IMPORTANT]
> <span data-ttu-id="615b5-194">Törölheti a megosztás csak akkor, ha offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="615b5-194">You can delete a share only if it is offline.</span></span>


<span data-ttu-id="615b5-195">Hajtsa végre a következő lépéseket toodelete megosztás hello.</span><span class="sxs-lookup"><span data-stu-id="615b5-195">Complete hello following steps toodelete a share.</span></span>

#### <a name="toodelete-a-share"></a><span data-ttu-id="615b5-196">a megosztás toodelete</span><span class="sxs-lookup"><span data-stu-id="615b5-196">toodelete a share</span></span>

1. <span data-ttu-id="615b5-197">A hello **megosztások** válasszon hello beállítása hello StorSimple szolgáltatás összefoglaló panelre, mely hello megosztáson kívánja toodelete található virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="615b5-197">From hello **Shares** setting on hello StorSimple service summary blade, select hello virtual array on which hello share you wish toodelete resides.</span></span>
2. <span data-ttu-id="615b5-198">**Válassza ki** hello megosztást, és kattintson **...**  (felváltva kattintson a jobb gombbal a sorhoz) hello helyi menüből válassza ki a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="615b5-198">**Select** hello share and Click **...** (alternately right-click in this row) and from hello context menu, select **Delete**.</span></span>
   
    ![Megosztás törlése](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. <span data-ttu-id="615b5-200">Hello állapotának hello megosztás toodelete szeretné.</span><span class="sxs-lookup"><span data-stu-id="615b5-200">Check hello status of hello share you want toodelete.</span></span> <span data-ttu-id="615b5-201">Ha azt szeretné, hogy toodelete hello megosztás nem offline állapotban, offline állapotba először.</span><span class="sxs-lookup"><span data-stu-id="615b5-201">If hello share you want toodelete is not offline, take it offline first.</span></span> <span data-ttu-id="615b5-202">Hello kövesse [offline állapotba a megosztás](#take-a-share-offline).</span><span class="sxs-lookup"><span data-stu-id="615b5-202">Follow hello steps in [Take a share offline](#take-a-share-offline).</span></span>
4. <span data-ttu-id="615b5-203">Ha a hello megerősítést kér **törlése** panelen fogadja el a hello megerősítő, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="615b5-203">When prompted for confirmation in hello **Delete** blade, accept hello confirmation and click **Delete**.</span></span> <span data-ttu-id="615b5-204">a program törli hello megosztás- és hello **megosztások** panel hello virtuális tömbön belüli megosztások hello frissített listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="615b5-204">hello share will now be deleted and hello **Shares** blade shows hello updated list of shares within hello virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="615b5-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="615b5-205">Next steps</span></span>
<span data-ttu-id="615b5-206">Ismerje meg, hogyan túl[klónozza a StorSimple megosztás](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="615b5-206">Learn how too[clone a StorSimple share](storsimple-virtual-array-clone.md).</span></span>

