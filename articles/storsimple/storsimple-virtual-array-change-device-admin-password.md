---
title: "aaaChange StorSimple virtuális tömb eszköz rendszergazdai jelszava |} Microsoft Docs"
description: "Ismerteti, hogyan toouse vagy hello Azure-portálon vagy a StorSimple virtuális tömb webes felhasználói felület toochange hello eszköz rendszergazdai jelszava."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 531b395df7aeade0a909360797c6b0f0abd9fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a><span data-ttu-id="e2b6e-103">Hello StorSimple virtuális tömb eszköz rendszergazdai jelszava keresztül StorSimple Device Manager módosítása</span><span class="sxs-lookup"><span data-stu-id="e2b6e-103">Change hello StorSimple Virtual Array device administrator password via StorSimple Device Manager</span></span>

## <a name="overview"></a><span data-ttu-id="e2b6e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e2b6e-104">Overview</span></span>

<span data-ttu-id="e2b6e-105">Használatakor hello Windows PowerShell felületet tooaccess hello StorSimple virtuális tömb, akkor szükség tooenter egy eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-105">When you use hello Windows PowerShell interface tooaccess hello StorSimple Virtual Array, you are required tooenter a device administrator password.</span></span> <span data-ttu-id="e2b6e-106">Hello StorSimple eszköz először kiosztásakor és lépések, hello alapértelmezett jelszava *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-106">When hello StorSimple device is first provisioned and started, hello default password is *Password1*.</span></span> <span data-ttu-id="e2b6e-107">Hello biztonsági adatait, hello alapértelmezett jelszava lejár, és hello első alkalommal, amikor bejelentkezik a rendszer szükséges toochange ezt a jelszót.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-107">For hello security of your data, hello default password expires hello first time that you sign in and you are required toochange this password.</span></span>

<span data-ttu-id="e2b6e-108">Az éles környezetben hello eszköz telepítését követően bármikor használhatja bármelyik hello helyi webes felhasználói felületén vagy a hello Azure portál toochange hello eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-108">You can also use either hello local web UI or hello Azure portal toochange hello device administrator password at any time after hello device is deployed in your production environment.</span></span> <span data-ttu-id="e2b6e-109">Ezek az eljárások leírását ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-109">Each of these procedures is described in this article.</span></span>

 ![Eszközök panel](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-hello-azure-portal-toochange-hello-password"></a><span data-ttu-id="e2b6e-111">Hello Azure portál toochange hello jelszó használata</span><span class="sxs-lookup"><span data-stu-id="e2b6e-111">Use hello Azure portal toochange hello password</span></span>

<span data-ttu-id="e2b6e-112">Hajtsa végre a következő lépéseket toochange hello eszköz rendszergazdai jelszava hello Azure-portálon keresztül hello.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-112">Perform hello following steps toochange hello device administrator password through hello Azure portal.</span></span>

#### <a name="toochange-hello-device-administrator-password-via-hello-azure-portal"></a><span data-ttu-id="e2b6e-113">toochange hello eszköz rendszergazdai jelszava hello Azure-portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="e2b6e-113">toochange hello device administrator password via hello Azure portal</span></span>

1. <span data-ttu-id="e2b6e-114">Hello szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és ezután belül hello **felügyeleti** kattintson **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-114">On hello service landing page, select your service, double-click hello service name, and then within hello **Management** section, click **Devices**.</span></span> <span data-ttu-id="e2b6e-115">Ekkor megnyílik a hello **eszközök** panel, amelyen a StorSimple virtuális tömb-eszközeinek felsorolását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-115">This opens hello **Devices** blade that lists all your StorSimple Virtual Array devices.</span></span>

2. <span data-ttu-id="e2b6e-116">A hello **eszközök** panelen kattintson duplán a jelszó megváltoztatása igénylő hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-116">In hello **Devices** blade, double-click hello device that requires a change of password.</span></span>

3. <span data-ttu-id="e2b6e-117">A hello **beállítások** az eszköz paneljén kattintson **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-117">In hello **Settings** blade for your device, click **Security**.</span></span>

4. <span data-ttu-id="e2b6e-118">A hello **biztonsági beállítások** panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-118">In hello **Security Settings** blade, do hello following:</span></span>
   
   1. <span data-ttu-id="e2b6e-119">Görgessen lefelé toohello **eszköz rendszergazdai jelszavát** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-119">Scroll down toohello **Device Administrator Password** section.</span></span> <span data-ttu-id="e2b6e-120">Adjon meg egy rendszergazdai jelszót, amely a 8 too15 karaktereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-120">Provide an administrator password that contains from 8 too15 characters.</span></span>
   2. <span data-ttu-id="e2b6e-121">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-121">Confirm hello password.</span></span>
   3. <span data-ttu-id="e2b6e-122">Kattintson a **mentése** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-122">Click **Save** at hello top of hello blade.</span></span>

<span data-ttu-id="e2b6e-123">hello eszköz rendszergazdai jelszava most frissül.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-123">hello device administrator password is now updated.</span></span> <span data-ttu-id="e2b6e-124">A módosított jelszó tooaccess hello eszköz helyileg is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-124">You can use this modified password tooaccess hello device locally.</span></span>

![Biztonsági beállítások panel](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-hello-local-web-ui-toochange-hello-password"></a><span data-ttu-id="e2b6e-126">Hello helyi webes felhasználói felület toochange hello jelszó használata</span><span class="sxs-lookup"><span data-stu-id="e2b6e-126">Use hello local web UI toochange hello password</span></span>

<span data-ttu-id="e2b6e-127">Hajtsa végre a következő lépéseket toochange hello eszköz rendszergazdai jelszava hello helyi webes felhasználói felületen keresztül hello.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-127">Perform hello following steps toochange hello device administrator password through hello local web UI.</span></span>

#### <a name="toochange-hello-device-administrator-password-via-hello-local-web-ui"></a><span data-ttu-id="e2b6e-128">toochange hello eszköz rendszergazdai jelszava hello helyi webes felhasználói felületen keresztül</span><span class="sxs-lookup"><span data-stu-id="e2b6e-128">toochange hello device administrator password via hello local web UI</span></span>

1. <span data-ttu-id="e2b6e-129">Hello helyi webes felhasználói felület, kattintson **karbantartási** > **jelszómódosítás** az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-129">In hello local web UI, click **Maintenance** > **Password change** for your device.</span></span>
   
    ![jelszó1 módosítása](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. <span data-ttu-id="e2b6e-131">Adja meg a hello **jelenlegi jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-131">Enter hello **Current password**.</span></span>
3. <span data-ttu-id="e2b6e-132">Adjon meg egy **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-132">Provide a **New Password**.</span></span> <span data-ttu-id="e2b6e-133">hello jelszó legalább 8 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-133">hello password must be at least 8 characters long.</span></span> <span data-ttu-id="e2b6e-134">3, 4 hello következő tartalmaznia kell: nagybetűk, nagybetűk, numerikus és speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-134">It must contain 3 of 4 of hello following: uppercase, lowercase, numeric, and special characters.</span></span>
   
    <span data-ttu-id="e2b6e-135">Vegye figyelembe, hogy a jelszó nem lehet ugyanaz, mint a legutóbbi 24 jelszó hello hello.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-135">Note that your password cannot be hello same as hello last 24 passwords.</span></span>
4. <span data-ttu-id="e2b6e-136">Adja meg újból hello jelszó tooconfirm azt.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-136">Reenter hello password tooconfirm it.</span></span>
   
    ![jelszó2 módosítása](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. <span data-ttu-id="e2b6e-138">Hello a hello lap alján, kattintson **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-138">At hello bottom of hello page, click **Apply**.</span></span> <span data-ttu-id="e2b6e-139">Új jelszó hello most alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-139">hello new password is now applied.</span></span> <span data-ttu-id="e2b6e-140">Ha hello jelszó módosítása sikertelen, hiba történt a következő hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="e2b6e-140">If hello password change is not successful, you see hello following error:</span></span>
   
    ![jelszó hiba](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    <span data-ttu-id="e2b6e-142">Miután hello jelszó frissítése sikeres volt, értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-142">After hello password is successfully updated, you are notified.</span></span> <span data-ttu-id="e2b6e-143">A módosított jelszó tooaccess hello eszköz helyileg majd használni.</span><span class="sxs-lookup"><span data-stu-id="e2b6e-143">You can then use this modified password tooaccess hello device locally.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e2b6e-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2b6e-144">Next steps</span></span>
<span data-ttu-id="e2b6e-145">Ismerje meg, hogyan túl[felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="e2b6e-145">Learn how too[administer your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

