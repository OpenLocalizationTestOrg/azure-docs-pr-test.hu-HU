---
title: "az Azure Site Recovery replikációs beállítások aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan toodeploy Site Recovery tooorchestrate replikációs, feladatátvételével és helyreállításával Hyper-V virtuális gépek a VMM-felhők tooAzure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 618e92e42411732a2a1bb75c5e5ea8a433cd7d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-replication-policy-for-vmware-tooazure"></a><span data-ttu-id="383cf-103">A VMware tooAzure replikációs házirend kezelése</span><span class="sxs-lookup"><span data-stu-id="383cf-103">Manage replication policy for VMware tooAzure</span></span>


## <a name="create-a-replication-policy"></a><span data-ttu-id="383cf-104">Replikációs házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="383cf-104">Create a replication policy</span></span>

1. <span data-ttu-id="383cf-105">Válassza a **Kezelés** > **Site Recovery-infrastruktúra** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="383cf-105">Select **Manage** > **Site Recovery Infrastructure**.</span></span>
2. <span data-ttu-id="383cf-106">Válassza a **Replikációs házirendek** lehetőséget a **VMware- és fizikai gépek számára** területen.</span><span class="sxs-lookup"><span data-stu-id="383cf-106">Select **Replication policies** under **For VMware and Physical machines**.</span></span>
3. <span data-ttu-id="383cf-107">Válassza a **+Replikációs házirend** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="383cf-107">Select **+Replication policy**.</span></span>

    ![Replikációs házirend létrehozása](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. <span data-ttu-id="383cf-109">Adja meg a hello házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="383cf-109">Enter hello policy name.</span></span>

5. <span data-ttu-id="383cf-110">A **helyreállítási Időkorlát küszöbértéke**, adja meg a hello RPO korlátot.</span><span class="sxs-lookup"><span data-stu-id="383cf-110">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="383cf-111">A rendszer riasztásokat küld, ha a folyamatos replikáció meghaladja ezt a korlátot.</span><span class="sxs-lookup"><span data-stu-id="383cf-111">Alerts will be generated when continuous replication exceeds this limit.</span></span>
6. <span data-ttu-id="383cf-112">A **helyreállításipont-megőrzést**, adja meg (órában) hello hello megőrzési időszak az egyes helyreállítási pontok időtartama.</span><span class="sxs-lookup"><span data-stu-id="383cf-112">In **Recovery point retention**, specify (in hours) hello duration of hello retention window for each recovery point.</span></span> <span data-ttu-id="383cf-113">Védett gépek lehet helyreállítani egy adatmegőrzési időtartam tooany pontot.</span><span class="sxs-lookup"><span data-stu-id="383cf-113">Protected machines can be recovered tooany point within a retention window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="383cf-114">Adatmegőrzési too24 órában gépek replikált toopremium tárolási támogatott.</span><span class="sxs-lookup"><span data-stu-id="383cf-114">Up too24 hours of retention is supported for machines replicated toopremium storage.</span></span> <span data-ttu-id="383cf-115">Adatmegőrzési too72 órában gépek replikált toostandard tárolási támogatott.</span><span class="sxs-lookup"><span data-stu-id="383cf-115">Up too72 hours of retention is supported for machines replicated toostandard storage.</span></span>

    > [!NOTE]
    > <span data-ttu-id="383cf-116">A rendszer automatikusan hoz létre replikációs házirendet a feladat-visszavételhez.</span><span class="sxs-lookup"><span data-stu-id="383cf-116">A replication policy for failback is automatically created.</span></span>

7. <span data-ttu-id="383cf-117">Az **Alkalmazáskonzisztens pillanatkép gyakorisága** beállítás azt határozza meg, hogy milyen gyakran hozzon létre a rendszer alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat (a beállítás értéke percekben adható meg).</span><span class="sxs-lookup"><span data-stu-id="383cf-117">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points that contain application-consistent snapshots will be created.</span></span>

8. <span data-ttu-id="383cf-118">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="383cf-118">Click **OK**.</span></span> <span data-ttu-id="383cf-119">hello házirend létre kell hozni too60 30 másodperc.</span><span class="sxs-lookup"><span data-stu-id="383cf-119">hello policy should be created in 30 too60 seconds.</span></span>

![Replikációs házirend létrehozása](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a><span data-ttu-id="383cf-121">Konfigurációs kiszolgáló társítása egy replikációs házirenddel</span><span class="sxs-lookup"><span data-stu-id="383cf-121">Associate a configuration server with a replication policy</span></span>
1. <span data-ttu-id="383cf-122">Válassza ki a hello replikációs házirend toowhich tooassociate hello konfigurációs kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="383cf-122">Choose hello replication policy toowhich you want tooassociate hello configuration server.</span></span>
2. <span data-ttu-id="383cf-123">Kattintson a **Társítás** gombra.</span><span class="sxs-lookup"><span data-stu-id="383cf-123">Click **Associate**.</span></span>
<span data-ttu-id="383cf-124">![Konfigurációs kiszolgáló társítása](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)</span><span class="sxs-lookup"><span data-stu-id="383cf-124">![Associate configuration server](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)</span></span>

3. <span data-ttu-id="383cf-125">Válassza ki a hello konfigurációs kiszolgáló hello azon kiszolgálók listájáról.</span><span class="sxs-lookup"><span data-stu-id="383cf-125">Select hello configuration server from hello list of servers.</span></span>
4. <span data-ttu-id="383cf-126">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="383cf-126">Click **OK**.</span></span> <span data-ttu-id="383cf-127">hello konfigurációs kiszolgáló egy tootwo percben kell rendelhetők hozzá.</span><span class="sxs-lookup"><span data-stu-id="383cf-127">hello configuration server should be associated in one tootwo minutes.</span></span>

![A konfigurációs kiszolgáló társítása](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a><span data-ttu-id="383cf-129">Replikációs házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="383cf-129">Edit a replication policy</span></span>
1. <span data-ttu-id="383cf-130">Válassza ki a kívánt tooedit replikációs beállítások hello replikációs házirend.</span><span class="sxs-lookup"><span data-stu-id="383cf-130">Choose hello replication policy for which you want tooedit replication settings.</span></span>
<span data-ttu-id="383cf-131">![Replikációs házirend szerkesztése](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)</span><span class="sxs-lookup"><span data-stu-id="383cf-131">![Edit replication policy](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)</span></span>

2. <span data-ttu-id="383cf-132">Kattintson a **Beállítások szerkesztése** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="383cf-132">Click **Edit Settings**.</span></span>
<span data-ttu-id="383cf-133">![Replikációs házirend beállításainak szerkesztése](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)</span><span class="sxs-lookup"><span data-stu-id="383cf-133">![Edit replication policy settings](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)</span></span>

3. <span data-ttu-id="383cf-134">Módosítsa a igények alapján hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="383cf-134">Change hello settings based on your need.</span></span>
4. <span data-ttu-id="383cf-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="383cf-135">Click **Save**.</span></span> <span data-ttu-id="383cf-136">hello házirend mentésére két toofive percben, attól függően, hogy hány virtuális gépeket használ, hogy a replikációs házirend.</span><span class="sxs-lookup"><span data-stu-id="383cf-136">hello policy should be saved in two toofive minutes, depending on how many VMs are using that replication policy.</span></span>

![Replikációs házirend mentése](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a><span data-ttu-id="383cf-138">A konfigurációs kiszolgáló és a replikációs házirend társításának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="383cf-138">Dissociate a configuration server from a replication policy</span></span>
1. <span data-ttu-id="383cf-139">Válassza ki a hello replikációs házirend toowhich tooassociate hello konfigurációs kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="383cf-139">Choose hello replication policy toowhich you want tooassociate hello configuration server.</span></span>
2. <span data-ttu-id="383cf-140">Kattintson a **Társítás megszüntetése** gombra.</span><span class="sxs-lookup"><span data-stu-id="383cf-140">Click **Dissociate**.</span></span>
3. <span data-ttu-id="383cf-141">Válassza ki a hello konfigurációs kiszolgáló hello azon kiszolgálók listájáról.</span><span class="sxs-lookup"><span data-stu-id="383cf-141">Select hello configuration server from hello list of servers.</span></span>
4. <span data-ttu-id="383cf-142">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="383cf-142">Click **OK**.</span></span> <span data-ttu-id="383cf-143">hello konfigurációs kiszolgáló egy tootwo percben kell különíthetők el.</span><span class="sxs-lookup"><span data-stu-id="383cf-143">hello configuration server should be dissociated in one tootwo minutes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="383cf-144">A konfigurációs kiszolgáló nem lehet leválasztani, ha legalább egy replikált elem hello házirenddel.</span><span class="sxs-lookup"><span data-stu-id="383cf-144">You cannot dissociate a configuration server if there is at least one replicated item using hello policy.</span></span> <span data-ttu-id="383cf-145">Ellenőrizze, hogy nincsenek hello házirend használata előtt hello konfigurációs kiszolgáló megszünteti replikált elemek.</span><span class="sxs-lookup"><span data-stu-id="383cf-145">Make sure there are no replicated items using hello policy before you dissociate hello configuration server.</span></span>

## <a name="delete-a-replication-policy"></a><span data-ttu-id="383cf-146">Replikációs házirend törlése</span><span class="sxs-lookup"><span data-stu-id="383cf-146">Delete a replication policy</span></span>

1. <span data-ttu-id="383cf-147">Válassza ki a hello replikációs házirendet, amelyet az toodelete.</span><span class="sxs-lookup"><span data-stu-id="383cf-147">Choose hello replication policy that you want toodelete.</span></span>
2. <span data-ttu-id="383cf-148">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="383cf-148">Click **Delete**.</span></span> <span data-ttu-id="383cf-149">hello házirend törölni kell a too60 30 másodperc.</span><span class="sxs-lookup"><span data-stu-id="383cf-149">hello policy should be deleted in 30 too60 seconds.</span></span>

    > [!NOTE]
    > <span data-ttu-id="383cf-150">Replikációs házirend nem törölhető, ha legalább egy konfigurációs kiszolgálóhoz kapcsolódó tooit van.</span><span class="sxs-lookup"><span data-stu-id="383cf-150">You cannot delete a replication policy if it has at least one configuration server associated tooit.</span></span> <span data-ttu-id="383cf-151">Ellenőrizze, hogy nincsenek hello házirenddel replikált elemek, és törölje az összes hello kapcsolódó konfigurációs kiszolgálók hello házirend törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="383cf-151">Make sure there are no replicated items using hello policy and delete all hello associated configuration servers before you delete hello policy.</span></span>
