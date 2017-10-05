---
title: "DNS-rekordhalmazok és az Azure DNS-rekordok kezelése |} Microsoft Docs"
description: "Az Azure DNS lehetővé teszi a DNS-rekordhalmazok és rekordok kezelése, amikor a tartomány."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ed44a1-7bfe-454f-964e-922ad978264a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 001b80ccba43beab44f6a598f820df65a85a345f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a><span data-ttu-id="a78d0-103">DNS-rekordok kezelése és a rekordhalmazok az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="a78d0-103">Manage DNS records and record sets by using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a78d0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a78d0-104">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="a78d0-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a78d0-105">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="a78d0-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a78d0-106">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="a78d0-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a78d0-107">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="a78d0-108">Ez a cikk bemutatja, hogyan rekordhalmazok és rekordokat a DNS-zóna kezelése az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="a78d0-108">This article shows you how to manage record sets and records for your DNS zone by using the Azure portal.</span></span>

<span data-ttu-id="a78d0-109">Fontos a DNS-rekordhalmazok és egyedi DNS-rekordok közötti különbségek megértése.</span><span class="sxs-lookup"><span data-stu-id="a78d0-109">It's important to understand the difference between DNS record sets and individual DNS records.</span></span> <span data-ttu-id="a78d0-110">A rekordhalmaz zónában azt jelzi, hogy az azonos nevű és azonos típusú gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="a78d0-110">A record set is a collection of records in a zone that have the same name and are the same type.</span></span> <span data-ttu-id="a78d0-111">További információkért lásd: [hozzon létre DNS-rekordhalmazok és az Azure portál használatával rekordok](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a78d0-111">For more information, see [Create DNS record sets and records by using the Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="create-a-new-record-set-and-record"></a><span data-ttu-id="a78d0-112">Egy új rekordhalmaz és rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="a78d0-112">Create a new record set and record</span></span>

<span data-ttu-id="a78d0-113">Az Azure-portálon a rekordhalmaz létrehozásához lásd: [hozzon létre DNS-rekordok az Azure portál használatával](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a78d0-113">To create a record set in the Azure portal, see [Create DNS records by using the Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="view-a-record-set"></a><span data-ttu-id="a78d0-114">A rekordhalmaz megtekintése</span><span class="sxs-lookup"><span data-stu-id="a78d0-114">View a record set</span></span>

1. <span data-ttu-id="a78d0-115">Az Azure-portálon lépjen a **DNS-zóna** panelen.</span><span class="sxs-lookup"><span data-stu-id="a78d0-115">In the Azure portal, go to the **DNS zone** blade.</span></span>
2. <span data-ttu-id="a78d0-116">Keresse meg a rekordhalmaz, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="a78d0-116">Search for the record set and select it.</span></span> <span data-ttu-id="a78d0-117">Ekkor megnyílik a rekordhalmaz tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a78d0-117">This opens the record set properties.</span></span>

    ![A rekordhalmaz keresése](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-to-a-record-set"></a><span data-ttu-id="a78d0-119">Adjon hozzá egy új rekordot a rekordhalmaz</span><span class="sxs-lookup"><span data-stu-id="a78d0-119">Add a new record to a record set</span></span>

<span data-ttu-id="a78d0-120">Minden rekordhalmaz legfeljebb 20 adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="a78d0-120">You can add up to 20 records to any record set.</span></span> <span data-ttu-id="a78d0-121">A rekordhalmaz nem tartalmazhat két azonos rekordokat.</span><span class="sxs-lookup"><span data-stu-id="a78d0-121">A record set cannot contain two identical records.</span></span> <span data-ttu-id="a78d0-122">(A nulla rekord) üres rekordhalmazok hozhatók létre, de nem jelennek meg az Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="a78d0-122">Empty record sets (with zero records) can be created, but do not appear on the Azure DNS name servers.</span></span> <span data-ttu-id="a78d0-123">CNAME típusú rekordhalmazok legfeljebb egy bejegyzést tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="a78d0-123">Record sets of type CNAME can contain one record at most.</span></span>

1. <span data-ttu-id="a78d0-124">Az a **rekordhalmaz tulajdonságok** a DNS-zóna panelen kattintson a bővíteni kívánt rekordkészlet.</span><span class="sxs-lookup"><span data-stu-id="a78d0-124">On the **Record set properties** blade for your DNS zone, click the record set that you want to add a record to.</span></span>

    ![Válassza ki a rekordhalmaz](./media/dns-operations-recordsets-portal/selectset500.png)

2. <span data-ttu-id="a78d0-126">Adja meg, a rekordhalmaz tulajdonságok a mezők kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="a78d0-126">Specify the record set properties by filling in the fields.</span></span>

    ![Adjon hozzá egy rekordot](./media/dns-operations-recordsets-portal/addrecord500.png)

3. <span data-ttu-id="a78d0-128">Kattintson a **mentése** mentse a beállításokat a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="a78d0-128">Click **Save** at the top of the blade to save your settings.</span></span> <span data-ttu-id="a78d0-129">Zárja be a panelt.</span><span class="sxs-lookup"><span data-stu-id="a78d0-129">Then close the blade.</span></span>
4. <span data-ttu-id="a78d0-130">Sarokban jelenik meg, hogy a rekord menti.</span><span class="sxs-lookup"><span data-stu-id="a78d0-130">In the corner, you will see that the record is saving.</span></span>

    ![Rekordhalmaz mentése](./media/dns-operations-recordsets-portal/saving150.png)

<span data-ttu-id="a78d0-132">A rekord mentését követően, az értékek a a **DNS-zóna** panel fogja tartalmazni az új rekordban.</span><span class="sxs-lookup"><span data-stu-id="a78d0-132">After the record has been saved, the values on the **DNS zone** blade will reflect the new record.</span></span>

## <a name="update-a-record"></a><span data-ttu-id="a78d0-133">Rekord frissítése</span><span class="sxs-lookup"><span data-stu-id="a78d0-133">Update a record</span></span>

<span data-ttu-id="a78d0-134">Amikor frissít egy meglévő rekordhalmaz rekord, a mezők frissítheti a típusú rekord dolgozunk függ.</span><span class="sxs-lookup"><span data-stu-id="a78d0-134">When you update a record in an existing record set, the fields you can update depend on the type of record you're working with.</span></span>

1. <span data-ttu-id="a78d0-135">Az a **rekordhalmaz tulajdonságok** panel a rekordhalmaz, keressen rá a rekordot.</span><span class="sxs-lookup"><span data-stu-id="a78d0-135">On the **Record set properties** blade for your record set, search for the record.</span></span>
2. <span data-ttu-id="a78d0-136">Módosítsa a bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="a78d0-136">Modify the record.</span></span> <span data-ttu-id="a78d0-137">Amikor módosít egy rekordot, módosíthatja a rendelkezésre álló beállítások a rekordhoz.</span><span class="sxs-lookup"><span data-stu-id="a78d0-137">When you modify a record, you can change the available settings for the record.</span></span> <span data-ttu-id="a78d0-138">A következő példában a **IP-cím** mező be van jelölve, és az IP-cím módosított folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a78d0-138">In the following example, the **IP address** field is selected, and the IP address is in the process of being modified.</span></span>

    ![Egy rekord módosítása](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. <span data-ttu-id="a78d0-140">Kattintson a **mentése** mentse a beállításokat a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="a78d0-140">Click **Save** at the top of the blade to save your settings.</span></span> <span data-ttu-id="a78d0-141">A jobb felső sarkában található megjelenik az értesítés a rekord mentett.</span><span class="sxs-lookup"><span data-stu-id="a78d0-141">In the upper right corner, you'll see the notification that the record has been saved.</span></span>

    ![Rekordhalmaz mentése](./media/dns-operations-recordsets-portal/saved150.png)

<span data-ttu-id="a78d0-143">A rekord mentését követően a rekord értékeit beállítani a **DNS-zóna** panel a frissített rekord fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="a78d0-143">After the record has been saved, the values for the record set on the **DNS zone** blade will reflect the updated record.</span></span>

## <a name="remove-a-record-from-a-record-set"></a><span data-ttu-id="a78d0-144">A rekordhalmaz bejegyzés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a78d0-144">Remove a record from a record set</span></span>

<span data-ttu-id="a78d0-145">Az Azure portál segítségével rekordok eltávolítása rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="a78d0-145">You can use the Azure portal to remove records from a record set.</span></span> <span data-ttu-id="a78d0-146">Vegye figyelembe, hogy a rekordhalmaz utolsó rekordját eltávolítása nem törli a rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="a78d0-146">Note that removing the last record from a record set does not delete the record set.</span></span>

1. <span data-ttu-id="a78d0-147">Az a **rekordhalmaz tulajdonságok** panel a rekordhalmaz, keressen rá a rekordot.</span><span class="sxs-lookup"><span data-stu-id="a78d0-147">On the **Record set properties** blade for your record set, search for the record.</span></span>
2. <span data-ttu-id="a78d0-148">Kattintson a bejegyzésre, hogy el szeretné távolítani.</span><span class="sxs-lookup"><span data-stu-id="a78d0-148">Click the record that you want to remove.</span></span> <span data-ttu-id="a78d0-149">Válassza ki **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="a78d0-149">Then select **Remove**.</span></span>

    ![Bejegyzés eltávolítása](./media/dns-operations-recordsets-portal/removerecord500.png)

3. <span data-ttu-id="a78d0-151">Kattintson a **mentése** mentse a beállításokat a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="a78d0-151">Click **Save** at the top of the blade to save your settings.</span></span>
4. <span data-ttu-id="a78d0-152">A rekord eltávolítását követően a rekord értékeit a **DNS-zóna** panel eltávolításának fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="a78d0-152">After the record has been removed, the values for the record on the **DNS zone** blade will reflect the removal.</span></span>

## <span data-ttu-id="a78d0-153"><a name="delete"></a>A rekordhalmaz törlése</span><span class="sxs-lookup"><span data-stu-id="a78d0-153"><a name="delete"></a>Delete a record set</span></span>

1. <span data-ttu-id="a78d0-154">Az a **rekordhalmaz tulajdonságok** a rekordhalmaz panelen kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a78d0-154">On the **Record set properties** blade for your record set, click **Delete**.</span></span>

    ![A rekordhalmaz törlése](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. <span data-ttu-id="a78d0-156">Egy üzenet jelenik meg azzal a kérdéssel, ha azt szeretné, a rekordhalmaz törlése.</span><span class="sxs-lookup"><span data-stu-id="a78d0-156">A message appears asking if you want to delete the record set.</span></span>
3. <span data-ttu-id="a78d0-157">Győződjön meg arról, hogy a neve egyezik-e a rekordhalmaz törlése, és kattintson a kívánt **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a78d0-157">Verify that the name matches the record set that you want to delete, and then click **Yes**.</span></span>
4. <span data-ttu-id="a78d0-158">Az a **DNS-zóna** panelen ellenőrizze, hogy a rekordhalmaz már nem látható.</span><span class="sxs-lookup"><span data-stu-id="a78d0-158">On the **DNS zone** blade, verify that the record set is no longer visible.</span></span>

## <a name="work-with-ns-and-soa-records"></a><span data-ttu-id="a78d0-159">Az NS és SOA rekordok kezelése</span><span class="sxs-lookup"><span data-stu-id="a78d0-159">Work with NS and SOA records</span></span>

<span data-ttu-id="a78d0-160">Automatikusan létrehozott NS és SOA rekordokat másképp kezeli az egyéb típusú bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="a78d0-160">NS and SOA records that are automatically created are managed differently from other record types.</span></span>

### <a name="modify-soa-records"></a><span data-ttu-id="a78d0-161">Módosíthatja a SOA rekordokat</span><span class="sxs-lookup"><span data-stu-id="a78d0-161">Modify SOA records</span></span>

<span data-ttu-id="a78d0-162">Nem adja hozzá vagy rekordok eltávolítása az automatikusan létrehozott SOA típusú rekordját, állítsa be a zóna felső pontja (név = "@").</span><span class="sxs-lookup"><span data-stu-id="a78d0-162">You cannot add or remove records from the automatically created SOA record set at the zone apex (name = "@").</span></span> <span data-ttu-id="a78d0-163">Azonban módosíthatja a SOA típusú rekordját (kivéve az "állomás") belül paraméterek egyikét, és a rekordhalmaz TTL-t.</span><span class="sxs-lookup"><span data-stu-id="a78d0-163">However, you can modify any of the parameters within the SOA record (except "Host") and the record set TTL.</span></span>

### <a name="modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="a78d0-164">A zóna felső pontja a Névkiszolgálói rekordok módosítása</span><span class="sxs-lookup"><span data-stu-id="a78d0-164">Modify NS records at the zone apex</span></span>

<span data-ttu-id="a78d0-165">Állítsa be a zóna felső pontja NS-rekord automatikusan létrejön minden DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="a78d0-165">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="a78d0-166">Az Azure DNS névkiszolgálóit, a zóna nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a78d0-166">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="a78d0-167">Hozzáadhat további névhez a kiszolgálók e NS-rekord, támogatja a párhuzamos üzemeltetési tartományok egynél több DNS-szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="a78d0-167">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="a78d0-168">A TTL-t és a metaadatok a rekordhalmaz is módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="a78d0-168">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="a78d0-169">Azonban nem lehet eltávolítani, vagy módosítsa az előre megadott Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="a78d0-169">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="a78d0-170">Vegye figyelembe, hogy csak vonatkozik az NS típusú rekordhalmaz zóna tetején.</span><span class="sxs-lookup"><span data-stu-id="a78d0-170">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="a78d0-171">A zónában (a használt gyermekzónákhoz delegálása) más NS-rekordhalmazok lehet módosítani, korlátozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="a78d0-171">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

### <a name="delete-soa-or-ns-record-sets"></a><span data-ttu-id="a78d0-172">SOA vagy NS rekordhalmazok törlése</span><span class="sxs-lookup"><span data-stu-id="a78d0-172">Delete SOA or NS record sets</span></span>

<span data-ttu-id="a78d0-173">Nem lehet törölni a SOA és NS-rekord beállítja a zóna felső pontja (név = "@"), amely automatikusan jönnek létre a zóna létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a78d0-173">You cannot delete the SOA and NS record sets at the zone apex (name = "@") that are created automatically when the zone is created.</span></span> <span data-ttu-id="a78d0-174">Ezek automatikusan törlődnek a zóna törlésekor.</span><span class="sxs-lookup"><span data-stu-id="a78d0-174">They are deleted automatically when you delete the zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a78d0-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a78d0-175">Next steps</span></span>

* <span data-ttu-id="a78d0-176">Azure DNS szolgáltatással kapcsolatos további információkért lásd: a [Azure DNS áttekintése](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a78d0-176">For more information about Azure DNS, see the [Azure DNS overview](dns-overview.md).</span></span>
* <span data-ttu-id="a78d0-177">DNS automatizálásával kapcsolatos további információkért lásd: [létrehozása DNS-zónák és a .NET SDK használatával rekordhalmazok](dns-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="a78d0-177">For more information about automating DNS, see [Creating DNS zones and record sets using the .NET SDK](dns-sdk.md).</span></span>
* <span data-ttu-id="a78d0-178">Névkeresési DNS-rekordok kapcsolatos további információkért lásd: [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a78d0-178">For more information about reverse DNS records, see [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>
