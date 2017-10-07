---
title: "aaaManage DNS rögzítése és az Azure DNS-rekordok |} Microsoft Docs"
description: "Az Azure DNS hello funkció toomanage DNS-rekord állítja be, és rögzíti, amikor a tartomány biztosít."
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
ms.openlocfilehash: 2e62d017341589eaf8d1f8df2fe5db4b973381d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-hello-azure-portal"></a><span data-ttu-id="64990-103">DNS-rekordok és a rekordhalmazok kezelése hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="64990-103">Manage DNS records and record sets by using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="64990-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="64990-104">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="64990-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="64990-105">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="64990-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="64990-106">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="64990-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="64990-107">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="64990-108">Ez a cikk bemutatja, hogyan toomanage rekordhalmazok és -rekordokat a DNS-zóna használatával hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="64990-108">This article shows you how toomanage record sets and records for your DNS zone by using hello Azure portal.</span></span>

<span data-ttu-id="64990-109">Fontos toounderstand hello különbség DNS-rekordhalmazok és egyedi DNS-rekordokat is.</span><span class="sxs-lookup"><span data-stu-id="64990-109">It's important toounderstand hello difference between DNS record sets and individual DNS records.</span></span> <span data-ttu-id="64990-110">Rekordhalmaz gyűjteménye, amelyek hello azonos nevet, és azonos hello írja be a zóna rekordjait.</span><span class="sxs-lookup"><span data-stu-id="64990-110">A record set is a collection of records in a zone that have hello same name and are hello same type.</span></span> <span data-ttu-id="64990-111">További információkért lásd: [hozzon létre DNS-rekordhalmazok és rekordok használatával hello Azure-portálon](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="64990-111">For more information, see [Create DNS record sets and records by using hello Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="create-a-new-record-set-and-record"></a><span data-ttu-id="64990-112">Egy új rekordhalmaz és rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="64990-112">Create a new record set and record</span></span>

<span data-ttu-id="64990-113">egy rekordot hello Azure-portálon toocreate lásd [hozzon létre DNS-rekordok segítségével hello Azure-portálon](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="64990-113">toocreate a record set in hello Azure portal, see [Create DNS records by using hello Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="view-a-record-set"></a><span data-ttu-id="64990-114">A rekordhalmaz megtekintése</span><span class="sxs-lookup"><span data-stu-id="64990-114">View a record set</span></span>

1. <span data-ttu-id="64990-115">A hello Azure-portálon, válassza a toohello **DNS-zóna** panelen.</span><span class="sxs-lookup"><span data-stu-id="64990-115">In hello Azure portal, go toohello **DNS zone** blade.</span></span>
2. <span data-ttu-id="64990-116">Hello rekordhalmaz kereshet, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="64990-116">Search for hello record set and select it.</span></span> <span data-ttu-id="64990-117">Ekkor megnyílik a hello rekordhalmaz tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="64990-117">This opens hello record set properties.</span></span>

    ![A rekordhalmaz keresése](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-tooa-record-set"></a><span data-ttu-id="64990-119">Új rekord tooa rekordhalmaz hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64990-119">Add a new record tooa record set</span></span>

<span data-ttu-id="64990-120">Másolatot too20 rekordok tooany rekordhalmaz adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="64990-120">You can add up too20 records tooany record set.</span></span> <span data-ttu-id="64990-121">A rekordhalmaz nem tartalmazhat két azonos rekordokat.</span><span class="sxs-lookup"><span data-stu-id="64990-121">A record set cannot contain two identical records.</span></span> <span data-ttu-id="64990-122">(A nulla rekord) üres rekordhalmazok hozhatók létre, de nem szerepelnek a hello Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="64990-122">Empty record sets (with zero records) can be created, but do not appear on hello Azure DNS name servers.</span></span> <span data-ttu-id="64990-123">CNAME típusú rekordhalmazok legfeljebb egy bejegyzést tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="64990-123">Record sets of type CNAME can contain one record at most.</span></span>

1. <span data-ttu-id="64990-124">A hello **rekordhalmaz tulajdonságok** a DNS-zóna panelen kattintson a hello rekord beállítása, amelyet tooadd egy rekordot.</span><span class="sxs-lookup"><span data-stu-id="64990-124">On hello **Record set properties** blade for your DNS zone, click hello record set that you want tooadd a record to.</span></span>

    ![Válassza ki a rekordhalmaz](./media/dns-operations-recordsets-portal/selectset500.png)

2. <span data-ttu-id="64990-126">Adja meg a hello rekordhalmaz tulajdonságok hello mezők kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="64990-126">Specify hello record set properties by filling in hello fields.</span></span>

    ![Adjon hozzá egy rekordot](./media/dns-operations-recordsets-portal/addrecord500.png)

3. <span data-ttu-id="64990-128">Kattintson a **mentése** : hello hello panel toosave tetején a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="64990-128">Click **Save** at hello top of hello blade toosave your settings.</span></span> <span data-ttu-id="64990-129">Zárja be hello panelen.</span><span class="sxs-lookup"><span data-stu-id="64990-129">Then close hello blade.</span></span>
4. <span data-ttu-id="64990-130">Hello sarokban jelenik meg, hogy menti az hello rekord.</span><span class="sxs-lookup"><span data-stu-id="64990-130">In hello corner, you will see that hello record is saving.</span></span>

    ![Rekordhalmaz mentése](./media/dns-operations-recordsets-portal/saving150.png)

<span data-ttu-id="64990-132">Hello rekord mentését követően hello hello értékét **DNS-zóna** panel hello új bejegyzést fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="64990-132">After hello record has been saved, hello values on hello **DNS zone** blade will reflect hello new record.</span></span>

## <a name="update-a-record"></a><span data-ttu-id="64990-133">Rekord frissítése</span><span class="sxs-lookup"><span data-stu-id="64990-133">Update a record</span></span>

<span data-ttu-id="64990-134">Amikor frissít egy meglévő rekordhalmaz rekord, frissítheti hello mezők függ hello típusú rekord dolgozunk.</span><span class="sxs-lookup"><span data-stu-id="64990-134">When you update a record in an existing record set, hello fields you can update depend on hello type of record you're working with.</span></span>

1. <span data-ttu-id="64990-135">A hello **rekordhalmaz tulajdonságok** a rekordhalmaz, keressen a hello rekord panelen.</span><span class="sxs-lookup"><span data-stu-id="64990-135">On hello **Record set properties** blade for your record set, search for hello record.</span></span>
2. <span data-ttu-id="64990-136">Hello rekord módosítása.</span><span class="sxs-lookup"><span data-stu-id="64990-136">Modify hello record.</span></span> <span data-ttu-id="64990-137">Amikor módosít egy rekordot, módosíthatja a hello hello rekord az elérhető beállítások.</span><span class="sxs-lookup"><span data-stu-id="64990-137">When you modify a record, you can change hello available settings for hello record.</span></span> <span data-ttu-id="64990-138">A következő példa hello, hello **IP-cím** mező be van jelölve, és hello IP-cím a módosított hello folyamatán.</span><span class="sxs-lookup"><span data-stu-id="64990-138">In hello following example, hello **IP address** field is selected, and hello IP address is in hello process of being modified.</span></span>

    ![Egy rekord módosítása](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. <span data-ttu-id="64990-140">Kattintson a **mentése** : hello hello panel toosave tetején a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="64990-140">Click **Save** at hello top of hello blade toosave your settings.</span></span> <span data-ttu-id="64990-141">Hello jobb felső sarkában található akkor megjelenik a hello rekord mentett hello értesítés.</span><span class="sxs-lookup"><span data-stu-id="64990-141">In hello upper right corner, you'll see hello notification that hello record has been saved.</span></span>

    ![Rekordhalmaz mentése](./media/dns-operations-recordsets-portal/saved150.png)

<span data-ttu-id="64990-143">Hello rekord mentését követően hello rekord hello értékét be hello **DNS-zóna** panel frissítése hello bejegyzést fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="64990-143">After hello record has been saved, hello values for hello record set on hello **DNS zone** blade will reflect hello updated record.</span></span>

## <a name="remove-a-record-from-a-record-set"></a><span data-ttu-id="64990-144">A rekordhalmaz bejegyzés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="64990-144">Remove a record from a record set</span></span>

<span data-ttu-id="64990-145">Használhatja a rekordhalmaz hello Azure portál tooremove bejegyzéseit.</span><span class="sxs-lookup"><span data-stu-id="64990-145">You can use hello Azure portal tooremove records from a record set.</span></span> <span data-ttu-id="64990-146">Vegye figyelembe, hogy a rekordhalmaz hello utolsó bejegyzés eltávolítása nem törli hello rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="64990-146">Note that removing hello last record from a record set does not delete hello record set.</span></span>

1. <span data-ttu-id="64990-147">A hello **rekordhalmaz tulajdonságok** a rekordhalmaz, keressen a hello rekord panelen.</span><span class="sxs-lookup"><span data-stu-id="64990-147">On hello **Record set properties** blade for your record set, search for hello record.</span></span>
2. <span data-ttu-id="64990-148">Kattintson a megjeleníteni kívánt tooremove hello rekord.</span><span class="sxs-lookup"><span data-stu-id="64990-148">Click hello record that you want tooremove.</span></span> <span data-ttu-id="64990-149">Válassza ki **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="64990-149">Then select **Remove**.</span></span>

    ![Bejegyzés eltávolítása](./media/dns-operations-recordsets-portal/removerecord500.png)

3. <span data-ttu-id="64990-151">Kattintson a **mentése** : hello hello panel toosave tetején a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="64990-151">Click **Save** at hello top of hello blade toosave your settings.</span></span>
4. <span data-ttu-id="64990-152">Hello rekord eltávolítását követően hello hello hello rekord értékeit **DNS-zóna** panel hello eltávolító fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="64990-152">After hello record has been removed, hello values for hello record on hello **DNS zone** blade will reflect hello removal.</span></span>

## <span data-ttu-id="64990-153"><a name="delete"></a>A rekordhalmaz törlése</span><span class="sxs-lookup"><span data-stu-id="64990-153"><a name="delete"></a>Delete a record set</span></span>

1. <span data-ttu-id="64990-154">A hello **rekordhalmaz tulajdonságok** a rekordhalmaz panelen kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="64990-154">On hello **Record set properties** blade for your record set, click **Delete**.</span></span>

    ![A rekordhalmaz törlése](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. <span data-ttu-id="64990-156">Egy üzenet jelenik meg azzal a kérdéssel, ha azt szeretné, hogy toodelete hello rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="64990-156">A message appears asking if you want toodelete hello record set.</span></span>
3. <span data-ttu-id="64990-157">Ellenőrizze, hogy hello neve megegyezik hello rekord toodelete szeretne, és kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="64990-157">Verify that hello name matches hello record set that you want toodelete, and then click **Yes**.</span></span>
4. <span data-ttu-id="64990-158">A hello **DNS-zóna** panelen ellenőrizze, hogy hello rekordkészlet már nem látható.</span><span class="sxs-lookup"><span data-stu-id="64990-158">On hello **DNS zone** blade, verify that hello record set is no longer visible.</span></span>

## <a name="work-with-ns-and-soa-records"></a><span data-ttu-id="64990-159">Az NS és SOA rekordok kezelése</span><span class="sxs-lookup"><span data-stu-id="64990-159">Work with NS and SOA records</span></span>

<span data-ttu-id="64990-160">Automatikusan létrehozott NS és SOA rekordokat másképp kezeli az egyéb típusú bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="64990-160">NS and SOA records that are automatically created are managed differently from other record types.</span></span>

### <a name="modify-soa-records"></a><span data-ttu-id="64990-161">Módosíthatja a SOA rekordokat</span><span class="sxs-lookup"><span data-stu-id="64990-161">Modify SOA records</span></span>

<span data-ttu-id="64990-162">Nem adja hozzá vagy rekordok eltávolítása hello automatikusan létrehozott hello zóna felső pontja, SOA rekordot (név = "@").</span><span class="sxs-lookup"><span data-stu-id="64990-162">You cannot add or remove records from hello automatically created SOA record set at hello zone apex (name = "@").</span></span> <span data-ttu-id="64990-163">Azonban módosíthatja hello SOA típusú rekordját (kivéve az "állomás") belül hello paraméterek egyikét, és hello rekordhalmaz TTL-t.</span><span class="sxs-lookup"><span data-stu-id="64990-163">However, you can modify any of hello parameters within hello SOA record (except "Host") and hello record set TTL.</span></span>

### <a name="modify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="64990-164">Hello zóna felső pontja a Névkiszolgálói rekordok módosítása</span><span class="sxs-lookup"><span data-stu-id="64990-164">Modify NS records at hello zone apex</span></span>

<span data-ttu-id="64990-165">Állítsa be megfelelően hello zóna felső pontja hello NS-rekord automatikusan hozza létre minden DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="64990-165">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="64990-166">Hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello szerepel benne.</span><span class="sxs-lookup"><span data-stu-id="64990-166">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="64990-167">Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="64990-167">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="64990-168">Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="64990-168">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="64990-169">Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="64990-169">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="64990-170">Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja.</span><span class="sxs-lookup"><span data-stu-id="64990-170">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="64990-171">Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) korlátozás nélkül lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="64990-171">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

### <a name="delete-soa-or-ns-record-sets"></a><span data-ttu-id="64990-172">SOA vagy NS rekordhalmazok törlése</span><span class="sxs-lookup"><span data-stu-id="64990-172">Delete SOA or NS record sets</span></span>

<span data-ttu-id="64990-173">Nem lehet törölni hello SOA és NS rekordhalmazok hello zóna csúcsán (név = "@") létrehozott automatikusan hello zóna létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="64990-173">You cannot delete hello SOA and NS record sets at hello zone apex (name = "@") that are created automatically when hello zone is created.</span></span> <span data-ttu-id="64990-174">Akkor azok törlődnek automatikusan hello zóna törlésekor.</span><span class="sxs-lookup"><span data-stu-id="64990-174">They are deleted automatically when you delete hello zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64990-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64990-175">Next steps</span></span>

* <span data-ttu-id="64990-176">Azure DNS szolgáltatással kapcsolatos további információkért lásd: hello [Azure DNS áttekintése](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="64990-176">For more information about Azure DNS, see hello [Azure DNS overview](dns-overview.md).</span></span>
* <span data-ttu-id="64990-177">DNS automatizálásával kapcsolatos további információkért lásd: [létrehozása DNS-zónák és a rekordhalmazok használatával hello .NET SDK](dns-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="64990-177">For more information about automating DNS, see [Creating DNS zones and record sets using hello .NET SDK](dns-sdk.md).</span></span>
* <span data-ttu-id="64990-178">Névkeresési DNS-rekordok kapcsolatos további információkért lásd: [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="64990-178">For more information about reverse DNS records, see [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>
