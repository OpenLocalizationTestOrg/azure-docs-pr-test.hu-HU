---
title: "A StorSimple virtuális tömb megosztások kezelése |} Microsoft Docs"
description: "A StorSimple Device Manager és ismerteti a StorSimple virtuális tömb-megosztások kezelése segítségével."
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
ms.openlocfilehash: e5c62689de36baa175001f5f4f70d87568876ef0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-shares-on-the-storsimple-virtual-array"></a><span data-ttu-id="e63a0-103">A StorSimple Device Manager szolgáltatással a StorSimple virtuális tömb-megosztások kezelése</span><span class="sxs-lookup"><span data-stu-id="e63a0-103">Use the StorSimple Device Manager service to manage shares on the StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="e63a0-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e63a0-104">Overview</span></span>

<span data-ttu-id="e63a0-105">Ez az oktatóanyag ismerteti, hogyan a StorSimple Device Manager szolgáltatás létrehozását és kezelését a StorSimple virtuális tömb megosztásokat.</span><span class="sxs-lookup"><span data-stu-id="e63a0-105">This tutorial explains how to use the StorSimple Device Manager service to create and manage shares on your StorSimple Virtual Array.</span></span>

<span data-ttu-id="e63a0-106">A StorSimple Device Manager szolgáltatás nem egy bővítmény, az Azure portálon, amely lehetővé teszi a StorSimple megoldásban egy egyetlen webes felhasználói felületen keresztüli kezelését.</span><span class="sxs-lookup"><span data-stu-id="e63a0-106">The StorSimple Device Manager service is an extension in the Azure portal that lets you manage your StorSimple solution from a single web interface.</span></span> <span data-ttu-id="e63a0-107">Megosztások és a kötetek kezelése, mellett a StorSimple Device Manager szolgáltatás segítségével megtekintheti és eszközök kezeléséhez, megtekintheti a riasztásokat, biztonsági mentési házirendek kezelése és a biztonságimásolat-katalógus kezelése.</span><span class="sxs-lookup"><span data-stu-id="e63a0-107">In addition to managing shares and volumes, you can use the StorSimple Device Manager service to view and manage devices, view alerts, manage backup policies, and manage the backup catalog.</span></span>

## <a name="share-types"></a><span data-ttu-id="e63a0-108">Megosztás típusok</span><span class="sxs-lookup"><span data-stu-id="e63a0-108">Share Types</span></span>

<span data-ttu-id="e63a0-109">StorSimple megosztások lehet:</span><span class="sxs-lookup"><span data-stu-id="e63a0-109">StorSimple shares can be:</span></span>

* <span data-ttu-id="e63a0-110">**Helyileg rögzített**: ezek az adatok mindig a tömbben marad, és nem kerülnek a felhőbe.</span><span class="sxs-lookup"><span data-stu-id="e63a0-110">**Locally pinned**: Data in these shares stays on the array at all times and does not spill to the cloud.</span></span>
* <span data-ttu-id="e63a0-111">**Rétegzett**: ezek az adatok kerülnek a felhőbe is.</span><span class="sxs-lookup"><span data-stu-id="e63a0-111">**Tiered**: Data in these shares can spill to the cloud.</span></span> <span data-ttu-id="e63a0-112">Amikor létrehoz egy rétegzett megosztást, körülbelül 10 %-a a terület ki van építve a helyi rétegen, és 90 %-tér ki van építve a felhőben.</span><span class="sxs-lookup"><span data-stu-id="e63a0-112">When you create a tiered share, approximately 10 % of the space is provisioned on the local tier and 90 % of the space is provisioned in the cloud.</span></span> <span data-ttu-id="e63a0-113">Például ha kiépített egy 1 TB-os megosztást, 100 GB kellene lennie, a helyi terület és 900 GB használni a felhőben során az adatok szinteket.</span><span class="sxs-lookup"><span data-stu-id="e63a0-113">For example, if you provisioned a 1 TB share, 100 GB would reside in the local space and 900 GB would be used in the cloud when the data tiers.</span></span> <span data-ttu-id="e63a0-114">Ez viszont azt jelenti, hogy ha elfogy a helyi terület az eszközön, nem használhatók a rétegzett megosztás (mert a helyi rétegen szükséges 10 % csak akkor érhető el).</span><span class="sxs-lookup"><span data-stu-id="e63a0-114">This in turn implies that if you run out of all the local space on the device, you cannot provision a tiered share (because the 10 % required on the local tier will not be available).</span></span>

### <a name="provisioned-capacity"></a><span data-ttu-id="e63a0-115">Kiosztott kapacitást</span><span class="sxs-lookup"><span data-stu-id="e63a0-115">Provisioned capacity</span></span>

<span data-ttu-id="e63a0-116">Tekintse meg a következő táblázat az egyes megosztás maximális kiosztott kapacitást.</span><span class="sxs-lookup"><span data-stu-id="e63a0-116">Refer to the following table for maximum provisioned capacity for each share type.</span></span>

| <span data-ttu-id="e63a0-117">**Korlát azonosítója**</span><span class="sxs-lookup"><span data-stu-id="e63a0-117">**Limit identifier**</span></span> | <span data-ttu-id="e63a0-118">**Korlát**</span><span class="sxs-lookup"><span data-stu-id="e63a0-118">**Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="e63a0-119">A rétegzett megosztás minimális mérete</span><span class="sxs-lookup"><span data-stu-id="e63a0-119">Minimum size of a tiered share</span></span> |<span data-ttu-id="e63a0-120">500 GB</span><span class="sxs-lookup"><span data-stu-id="e63a0-120">500 GB</span></span> |
| <span data-ttu-id="e63a0-121">A rétegzett megosztás maximális mérete</span><span class="sxs-lookup"><span data-stu-id="e63a0-121">Maximum size of a tiered share</span></span> |<span data-ttu-id="e63a0-122">20 TB</span><span class="sxs-lookup"><span data-stu-id="e63a0-122">20 TB</span></span> |
| <span data-ttu-id="e63a0-123">Egy helyileg rögzített megosztás minimális mérete</span><span class="sxs-lookup"><span data-stu-id="e63a0-123">Minimum size of a locally pinned share</span></span> |<span data-ttu-id="e63a0-124">50 GB</span><span class="sxs-lookup"><span data-stu-id="e63a0-124">50 GB</span></span> |
| <span data-ttu-id="e63a0-125">Egy helyileg rögzített megosztás maximális mérete</span><span class="sxs-lookup"><span data-stu-id="e63a0-125">Maximum size of a locally pinned share</span></span> |<span data-ttu-id="e63a0-126">2 TB</span><span class="sxs-lookup"><span data-stu-id="e63a0-126">2 TB</span></span> |

## <a name="the-shares-blade"></a><span data-ttu-id="e63a0-127">A megosztások panel</span><span class="sxs-lookup"><span data-stu-id="e63a0-127">The Shares blade</span></span>

<span data-ttu-id="e63a0-128">A **megosztások** menü a StorSimple szolgáltatás összefoglaló panelen a tároló megosztások megjeleníti egy adott StorSimple-tömb, és kezelheti azokat.</span><span class="sxs-lookup"><span data-stu-id="e63a0-128">The **Shares** menu on your StorSimple service summary blade displays the list of storage shares on a given StorSimple array and allows you to manage them.</span></span>

![Megosztások panel](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

<span data-ttu-id="e63a0-130">A megosztás sorozatából attribútumok:</span><span class="sxs-lookup"><span data-stu-id="e63a0-130">A share consists of a series of attributes:</span></span>

* <span data-ttu-id="e63a0-131">**Megosztási név** – egy leíró nevet, amely egyedinek kell lennie, és segít azonosítani a megosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="e63a0-131">**Share Name** – A descriptive name that must be unique and helps identify the share.</span></span>
* <span data-ttu-id="e63a0-132">**Állapot** – lehet online vagy offline állapotú.</span><span class="sxs-lookup"><span data-stu-id="e63a0-132">**Status** – Can be online or offline.</span></span> <span data-ttu-id="e63a0-133">Ha egy megosztást offline állapotban, a megosztás felhasználók nem fognak tudni azt elérni.</span><span class="sxs-lookup"><span data-stu-id="e63a0-133">If a share is offline, users of the share will not be able to access it.</span></span>
* <span data-ttu-id="e63a0-134">**Típus** – azt jelzi, hogy a megosztás **rétegzett** (alapértelmezés) vagy **helyileg rögzített**.</span><span class="sxs-lookup"><span data-stu-id="e63a0-134">**Type** – Indicates whether the share is **Tiered** (the default) or **Locally pinned**.</span></span>
* <span data-ttu-id="e63a0-135">**Kapacitás** – Megadja azt az időt a megosztáson tárolt adatok teljes mennyisége képest használt adatokat.</span><span class="sxs-lookup"><span data-stu-id="e63a0-135">**Capacity** – specifies the amount of data used as compared to the total amount of data that can be stored on the share.</span></span>
* <span data-ttu-id="e63a0-136">**Leírás** – választható beállítás, amely segít a megosztás leírása.</span><span class="sxs-lookup"><span data-stu-id="e63a0-136">**Description** – An optional setting that helps describe the share.</span></span>
* <span data-ttu-id="e63a0-137">**Engedélyek** -a megosztásra, a Windows Explorer használatával kezelheti az NTFS-engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="e63a0-137">**Permissions** - The NTFS permissions to the share that can be managed through Windows Explorer.</span></span>
* <span data-ttu-id="e63a0-138">**Biztonsági mentési** – abban az esetben a StorSimple virtuális tömb minden megosztás automatikusan engedélyezve vannak a biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="e63a0-138">**Backup** – In case of the StorSimple Virtual Array, all shares are automatically enabled for backup.</span></span>

![Megosztások részletei](./media/storsimple-virtual-array-manage-shares/share-details.png)

<span data-ttu-id="e63a0-140">Kövesse az utasításokat ebben az oktatóanyagban a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="e63a0-140">Use the instructions in this tutorial to perform the following tasks:</span></span>

* <span data-ttu-id="e63a0-141">Megosztás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e63a0-141">Add a share</span></span>
* <span data-ttu-id="e63a0-142">Egy megosztás módosítása</span><span class="sxs-lookup"><span data-stu-id="e63a0-142">Modify a share</span></span>
* <span data-ttu-id="e63a0-143">Egy kapcsolat nélküli üzemmódra állítása</span><span class="sxs-lookup"><span data-stu-id="e63a0-143">Take a share offline</span></span>
* <span data-ttu-id="e63a0-144">Megosztás törlése</span><span class="sxs-lookup"><span data-stu-id="e63a0-144">Delete a share</span></span>

## <a name="add-a-share"></a><span data-ttu-id="e63a0-145">Megosztás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e63a0-145">Add a share</span></span>

1. <span data-ttu-id="e63a0-146">A StorSimple szolgáltatás összefoglaló paneljén kattintson **+ Hozzáadás megosztás** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="e63a0-146">From the StorSimple service summary blade, click **+ Add share** from the command bar.</span></span> <span data-ttu-id="e63a0-147">Ezzel megnyílik a **Hozzáadás megosztás** panelen.</span><span class="sxs-lookup"><span data-stu-id="e63a0-147">This opens up the **Add share** blade.</span></span>

    ![Adja hozzá a megosztás](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. <span data-ttu-id="e63a0-149">Az a **Hozzáadás megosztás** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e63a0-149">In the **Add share** blade, do the following:</span></span>
   
    1. <span data-ttu-id="e63a0-150">Az a **megosztásnevet** mezőben adjon meg egy egyedi nevet a megosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="e63a0-150">In the **Share name** field, enter a unique name for your share.</span></span> <span data-ttu-id="e63a0-151">A név 3 – 127 karaktert tartalmazó karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e63a0-151">The name must be a string that contains 3 to 127 characters.</span></span>

    2. <span data-ttu-id="e63a0-152">Egy nem kötelező **leírás** a megosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="e63a0-152">An optional **Description** for the share.</span></span> <span data-ttu-id="e63a0-153">A leírás segít azonosítani a fájlmegosztás-tulajdonosok.</span><span class="sxs-lookup"><span data-stu-id="e63a0-153">The description will help identify the share owners.</span></span>

    3. <span data-ttu-id="e63a0-154">Az a **típus** legördülő listában, adja meg, hogy hozzon létre egy **rétegzett** vagy **helyileg rögzített** megosztani.</span><span class="sxs-lookup"><span data-stu-id="e63a0-154">In the **Type** dropdown list, specify whether to create a **Tiered** or **Locally pinned** share.</span></span> <span data-ttu-id="e63a0-155">Helyi garanciákat, kis késleltetést és magasabb teljesítményt igénylő munkaterheléseknél válasszon **helyileg rögzített megosztás**.</span><span class="sxs-lookup"><span data-stu-id="e63a0-155">For workloads that require local guarantees, low latencies, and higher performance, select **Locally pinned share**.</span></span> <span data-ttu-id="e63a0-156">Minden más adathoz válasszon **rétegzett** megosztani.</span><span class="sxs-lookup"><span data-stu-id="e63a0-156">For all other data, select **Tiered** share.</span></span>

    4. <span data-ttu-id="e63a0-157">Az a **kapacitás** mezőben adja meg a fájlmegosztás méretét.</span><span class="sxs-lookup"><span data-stu-id="e63a0-157">In the **Capacity** field, specify the size of the share.</span></span> <span data-ttu-id="e63a0-158">A rétegzett megosztás 500 GB és 20 TB között kell lennie, és egy helyileg rögzített megosztás 50 GB-os és a 2 TB közé kell esnie.</span><span class="sxs-lookup"><span data-stu-id="e63a0-158">A tiered share must be between 500 GB and 20 TB and a locally pinned share must be between 50 GB and 2 TB.</span></span>

    5. <span data-ttu-id="e63a0-159">Az a **értékre alapértelmezett teljes körű engedélyekkel** mezőben, az engedélyek hozzárendelése a felhasználó vagy a csoportot, amelyhez ez a megosztás fér hozzá.</span><span class="sxs-lookup"><span data-stu-id="e63a0-159">In the **Set default full permissions to** field, assign the permissions to the user, or the group that is accessing this share.</span></span> <span data-ttu-id="e63a0-160">Adja meg a felhasználó vagy a felhasználói csoport nevét  _john@contoso.com_  formátumban.</span><span class="sxs-lookup"><span data-stu-id="e63a0-160">Specify the name of the user or the user group in _john@contoso.com_ format.</span></span> <span data-ttu-id="e63a0-161">Azt javasoljuk, hogy egy felhasználói csoport (helyett egy-egy felhasználóhoz) használatával biztosíthatja a rendszergazdai jogosultsággal a megosztást.</span><span class="sxs-lookup"><span data-stu-id="e63a0-161">We recommend that you use a user group (instead of a single user) to allow admin privileges to access these shares.</span></span> <span data-ttu-id="e63a0-162">Miután hozzárendelt itt az engedélyeket, majd segítségével Fájlkezelőben módosítani ezeket az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="e63a0-162">After you have assigned the permissions here, you can then use File Explorer to modify these permissions.</span></span>
3. <span data-ttu-id="e63a0-163">Amikor elkészült, a megosztás konfigurálására, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e63a0-163">When you've finished configuring your share, click **Create**.</span></span> <span data-ttu-id="e63a0-164">A megosztás jön létre a megadott beállításokat, és megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="e63a0-164">A share will be created with the specified settings and you will see a notification.</span></span> <span data-ttu-id="e63a0-165">Alapértelmezés szerint biztonsági mentés a megosztás engedélyezve lesz.</span><span class="sxs-lookup"><span data-stu-id="e63a0-165">By default, backup will be enabled for the share.</span></span>
4. <span data-ttu-id="e63a0-166">Győződjön meg arról, hogy a fájlmegosztás sikeresen létrejött, keresse fel a **megosztások** panelen.</span><span class="sxs-lookup"><span data-stu-id="e63a0-166">To confirm that the share was successfully created, go to the **Shares** blade.</span></span> <span data-ttu-id="e63a0-167">Meg kell jelenniük a felsorolt megosztást.</span><span class="sxs-lookup"><span data-stu-id="e63a0-167">You should see the share listed.</span></span>
   
    ![Fájlmegosztás létrehozása sikerült](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a><span data-ttu-id="e63a0-169">Egy megosztás módosítása</span><span class="sxs-lookup"><span data-stu-id="e63a0-169">Modify a share</span></span>

<span data-ttu-id="e63a0-170">Egy megosztás módosítása, ha módosítania kell a megosztás leírása.</span><span class="sxs-lookup"><span data-stu-id="e63a0-170">Modify a share when you need to change the description of the share.</span></span> <span data-ttu-id="e63a0-171">Nincs más tulajdonságok a megosztás létrehozása után módosítható.</span><span class="sxs-lookup"><span data-stu-id="e63a0-171">No other share properties can be modified once the share is created.</span></span>

#### <a name="to-modify-a-share"></a><span data-ttu-id="e63a0-172">Megosztás módosítása</span><span class="sxs-lookup"><span data-stu-id="e63a0-172">To modify a share</span></span>

1. <span data-ttu-id="e63a0-173">Az a **megosztások** a StorSimple szolgáltatás összefoglaló panelen beállításnál válassza a virtuális tömb kívánja módosíthatja a megosztás helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="e63a0-173">From the **Shares** setting on the StorSimple service summary blade, select the virtual array on which the share you wish you to modify resides.</span></span>
2. <span data-ttu-id="e63a0-174">**Válassza ki** aktuális leírás megtekintése és módosítása a megosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="e63a0-174">**Select** the share to view the current description and modify it.</span></span>
3. <span data-ttu-id="e63a0-175">A módosítások mentéséhez kattintson a **mentése** parancssávon.</span><span class="sxs-lookup"><span data-stu-id="e63a0-175">Save your changes by clicking the **Save** command bar.</span></span> <span data-ttu-id="e63a0-176">A megadott beállítások lesznek alkalmazva, és megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="e63a0-176">Your specified settings will be applied and you will see a notification.</span></span>
   
    ![ <span data-ttu-id="e63a0-177">Megosztás módosítása</span><span class="sxs-lookup"><span data-stu-id="e63a0-177">Edit share</span></span>](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a><span data-ttu-id="e63a0-178">Egy kapcsolat nélküli üzemmódra állítása</span><span class="sxs-lookup"><span data-stu-id="e63a0-178">Take a share offline</span></span>

<span data-ttu-id="e63a0-179">Szükség lehet egy offline állapotba, ha azt tervezi, hogy módosítsa vagy törölje azt.</span><span class="sxs-lookup"><span data-stu-id="e63a0-179">You may need to take a share offline when you are planning to modify it or delete it.</span></span> <span data-ttu-id="e63a0-180">Ha egy megosztást offline állapotban, nincs olvasási és írási hozzáférése érhető el.</span><span class="sxs-lookup"><span data-stu-id="e63a0-180">When a share is offline, it is not available for read-write access.</span></span> <span data-ttu-id="e63a0-181">Szüksége lesz a megosztás nélküli érvénybe az állomáson, valamint az eszközön.</span><span class="sxs-lookup"><span data-stu-id="e63a0-181">You will need to take the share offline on the host as well as on the device.</span></span>

#### <a name="to-take-a-share-offline"></a><span data-ttu-id="e63a0-182">A megosztás offline állapotba</span><span class="sxs-lookup"><span data-stu-id="e63a0-182">To take a share offline</span></span>

1. <span data-ttu-id="e63a0-183">Győződjön meg arról, hogy a szóban forgó megosztás nem offline állapotba helyezése előtt használatban van.</span><span class="sxs-lookup"><span data-stu-id="e63a0-183">Make sure that the share in question is not in use before taking it offline.</span></span>
2. <span data-ttu-id="e63a0-184">A megosztás végrehajtása a hálózatról a tömb az alábbi lépések elvégzésével:</span><span class="sxs-lookup"><span data-stu-id="e63a0-184">Take the share on the array offline by performing the following steps:</span></span>
   
    1. <span data-ttu-id="e63a0-185">Az a **megosztások** beállítása a StorSimple szolgáltatás összefoglaló panelen, jelölje ki a virtuális tömb, amelyen a megosztás kívánja, hogy a kapcsolat nélküli üzemmódra található.</span><span class="sxs-lookup"><span data-stu-id="e63a0-185">From the **Shares** setting on the StorSimple service summary blade, select the virtual array on which the share you wish you to take offline resides.</span></span>

    2. <span data-ttu-id="e63a0-186">**Válassza ki** a megosztást, majd kattintson **...**  (felváltva kattintson a jobb gombbal a sorhoz), és válassza ki a helyi menü **offline állapotba**.</span><span class="sxs-lookup"><span data-stu-id="e63a0-186">**Select** the share and Click **...** (alternately right-click in this row) and from the context menu, select **Take offline**.</span></span>
     
        ![Kapcsolat nélküli megosztás](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. <span data-ttu-id="e63a0-188">Tekintse át az adatokat a **offline állapotba** panel és elfogadta a műveletet.</span><span class="sxs-lookup"><span data-stu-id="e63a0-188">Review the information in the **Take offline** blade and confirm your acceptance of the operation.</span></span> <span data-ttu-id="e63a0-189">Kattintson a **offline állapotba** offline állapotba a megosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="e63a0-189">Click **Take offline** to take the share offline.</span></span> <span data-ttu-id="e63a0-190">Megjelenik egy értesítés, amely a folyamatban lévő művelet.</span><span class="sxs-lookup"><span data-stu-id="e63a0-190">You will see a notification of the operation in progress.</span></span>

    4. <span data-ttu-id="e63a0-191">Győződjön meg arról, hogy a megosztás sikeresen offline állapotba kerül, keresse fel a **megosztások** panelen.</span><span class="sxs-lookup"><span data-stu-id="e63a0-191">To confirm that the share was successfully taken offline, go to the **Shares** blade.</span></span> <span data-ttu-id="e63a0-192">A megosztás állapotát, a kapcsolat nélküli kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="e63a0-192">You should see the status of the share as offline.</span></span>

## <a name="delete-a-share"></a><span data-ttu-id="e63a0-193">Megosztás törlése</span><span class="sxs-lookup"><span data-stu-id="e63a0-193">Delete a share</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e63a0-194">Törölheti a megosztás csak akkor, ha offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="e63a0-194">You can delete a share only if it is offline.</span></span>


<span data-ttu-id="e63a0-195">Az alábbi lépésekkel törli a megosztást.</span><span class="sxs-lookup"><span data-stu-id="e63a0-195">Complete the following steps to delete a share.</span></span>

#### <a name="to-delete-a-share"></a><span data-ttu-id="e63a0-196">A megosztás törlése</span><span class="sxs-lookup"><span data-stu-id="e63a0-196">To delete a share</span></span>

1. <span data-ttu-id="e63a0-197">Az a **megosztások** a StorSimple szolgáltatás összefoglaló panelen beállításnál válassza a virtuális tömb törli a megosztást helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="e63a0-197">From the **Shares** setting on the StorSimple service summary blade, select the virtual array on which the share you wish to delete resides.</span></span>
2. <span data-ttu-id="e63a0-198">**Válassza ki** a megosztást, majd kattintson **...**  (felváltva kattintson a jobb gombbal a sorhoz), és válassza ki a helyi menü **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e63a0-198">**Select** the share and Click **...** (alternately right-click in this row) and from the context menu, select **Delete**.</span></span>
   
    ![Megosztás törlése](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. <span data-ttu-id="e63a0-200">Törli a megosztást állapotának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="e63a0-200">Check the status of the share you want to delete.</span></span> <span data-ttu-id="e63a0-201">Ha törli a megosztást nem offline állapotban, offline állapotba először.</span><span class="sxs-lookup"><span data-stu-id="e63a0-201">If the share you want to delete is not offline, take it offline first.</span></span> <span data-ttu-id="e63a0-202">Kövesse a [offline állapotba a megosztás](#take-a-share-offline).</span><span class="sxs-lookup"><span data-stu-id="e63a0-202">Follow the steps in [Take a share offline](#take-a-share-offline).</span></span>
4. <span data-ttu-id="e63a0-203">Ha a megerősítést kér a **törlése** panelen fogadja el a megerősítési, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e63a0-203">When prompted for confirmation in the **Delete** blade, accept the confirmation and click **Delete**.</span></span> <span data-ttu-id="e63a0-204">Program törli a megosztást és a **megosztások** panel megosztások a virtuális tömbön belüli frissített listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e63a0-204">The share will now be deleted and the **Shares** blade shows the updated list of shares within the virtual array.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e63a0-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e63a0-205">Next steps</span></span>
<span data-ttu-id="e63a0-206">Megtudhatja, hogyan [klónozza a StorSimple megosztás](storsimple-virtual-array-clone.md).</span><span class="sxs-lookup"><span data-stu-id="e63a0-206">Learn how to [clone a StorSimple share](storsimple-virtual-array-clone.md).</span></span>

