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
# <a name="manage-dns-records-and-record-sets-by-using-hello-azure-portal"></a>DNS-rekordok és a rekordhalmazok kezelése hello Azure-portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Ez a cikk bemutatja, hogyan toomanage rekordhalmazok és -rekordokat a DNS-zóna használatával hello Azure-portálon.

Fontos toounderstand hello különbség DNS-rekordhalmazok és egyedi DNS-rekordokat is. Rekordhalmaz gyűjteménye, amelyek hello azonos nevet, és azonos hello írja be a zóna rekordjait. További információkért lásd: [hozzon létre DNS-rekordhalmazok és rekordok használatával hello Azure-portálon](dns-getstarted-create-recordset-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Egy új rekordhalmaz és rekord létrehozása

egy rekordot hello Azure-portálon toocreate lásd [hozzon létre DNS-rekordok segítségével hello Azure-portálon](dns-getstarted-create-recordset-portal.md).

## <a name="view-a-record-set"></a>A rekordhalmaz megtekintése

1. A hello Azure-portálon, válassza a toohello **DNS-zóna** panelen.
2. Hello rekordhalmaz kereshet, és válassza ki azt. Ekkor megnyílik a hello rekordhalmaz tulajdonságait.

    ![A rekordhalmaz keresése](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-tooa-record-set"></a>Új rekord tooa rekordhalmaz hozzáadása

Másolatot too20 rekordok tooany rekordhalmaz adhat hozzá. A rekordhalmaz nem tartalmazhat két azonos rekordokat. (A nulla rekord) üres rekordhalmazok hozhatók létre, de nem szerepelnek a hello Azure DNS névkiszolgálóit. CNAME típusú rekordhalmazok legfeljebb egy bejegyzést tartalmazhat.

1. A hello **rekordhalmaz tulajdonságok** a DNS-zóna panelen kattintson a hello rekord beállítása, amelyet tooadd egy rekordot.

    ![Válassza ki a rekordhalmaz](./media/dns-operations-recordsets-portal/selectset500.png)

2. Adja meg a hello rekordhalmaz tulajdonságok hello mezők kitöltésével.

    ![Adjon hozzá egy rekordot](./media/dns-operations-recordsets-portal/addrecord500.png)

3. Kattintson a **mentése** : hello hello panel toosave tetején a beállításokat. Zárja be hello panelen.
4. Hello sarokban jelenik meg, hogy menti az hello rekord.

    ![Rekordhalmaz mentése](./media/dns-operations-recordsets-portal/saving150.png)

Hello rekord mentését követően hello hello értékét **DNS-zóna** panel hello új bejegyzést fogja tartalmazni.

## <a name="update-a-record"></a>Rekord frissítése

Amikor frissít egy meglévő rekordhalmaz rekord, frissítheti hello mezők függ hello típusú rekord dolgozunk.

1. A hello **rekordhalmaz tulajdonságok** a rekordhalmaz, keressen a hello rekord panelen.
2. Hello rekord módosítása. Amikor módosít egy rekordot, módosíthatja a hello hello rekord az elérhető beállítások. A következő példa hello, hello **IP-cím** mező be van jelölve, és hello IP-cím a módosított hello folyamatán.

    ![Egy rekord módosítása](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Kattintson a **mentése** : hello hello panel toosave tetején a beállításokat. Hello jobb felső sarkában található akkor megjelenik a hello rekord mentett hello értesítés.

    ![Rekordhalmaz mentése](./media/dns-operations-recordsets-portal/saved150.png)

Hello rekord mentését követően hello rekord hello értékét be hello **DNS-zóna** panel frissítése hello bejegyzést fogja tartalmazni.

## <a name="remove-a-record-from-a-record-set"></a>A rekordhalmaz bejegyzés eltávolítása

Használhatja a rekordhalmaz hello Azure portál tooremove bejegyzéseit. Vegye figyelembe, hogy a rekordhalmaz hello utolsó bejegyzés eltávolítása nem törli hello rekordhalmaz.

1. A hello **rekordhalmaz tulajdonságok** a rekordhalmaz, keressen a hello rekord panelen.
2. Kattintson a megjeleníteni kívánt tooremove hello rekord. Válassza ki **eltávolítása**.

    ![Bejegyzés eltávolítása](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Kattintson a **mentése** : hello hello panel toosave tetején a beállításokat.
4. Hello rekord eltávolítását követően hello hello hello rekord értékeit **DNS-zóna** panel hello eltávolító fogja tartalmazni.

## <a name="delete"></a>A rekordhalmaz törlése

1. A hello **rekordhalmaz tulajdonságok** a rekordhalmaz panelen kattintson a **törlése**.

    ![A rekordhalmaz törlése](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Egy üzenet jelenik meg azzal a kérdéssel, ha azt szeretné, hogy toodelete hello rekordhalmaz.
3. Ellenőrizze, hogy hello neve megegyezik hello rekord toodelete szeretne, és kattintson **Igen**.
4. A hello **DNS-zóna** panelen ellenőrizze, hogy hello rekordkészlet már nem látható.

## <a name="work-with-ns-and-soa-records"></a>Az NS és SOA rekordok kezelése

Automatikusan létrehozott NS és SOA rekordokat másképp kezeli az egyéb típusú bejegyzés.

### <a name="modify-soa-records"></a>Módosíthatja a SOA rekordokat

Nem adja hozzá vagy rekordok eltávolítása hello automatikusan létrehozott hello zóna felső pontja, SOA rekordot (név = "@"). Azonban módosíthatja hello SOA típusú rekordját (kivéve az "állomás") belül hello paraméterek egyikét, és hello rekordhalmaz TTL-t.

### <a name="modify-ns-records-at-hello-zone-apex"></a>Hello zóna felső pontja a Névkiszolgálói rekordok módosítása

Állítsa be megfelelően hello zóna felső pontja hello NS-rekord automatikusan hozza létre minden DNS-zóna. Hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello szerepel benne.

Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál. Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja. Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit.

Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja. Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) korlátozás nélkül lehet módosítani.

### <a name="delete-soa-or-ns-record-sets"></a>SOA vagy NS rekordhalmazok törlése

Nem lehet törölni hello SOA és NS rekordhalmazok hello zóna csúcsán (név = "@") létrehozott automatikusan hello zóna létrehozásakor. Akkor azok törlődnek automatikusan hello zóna törlésekor.

## <a name="next-steps"></a>Következő lépések

* Azure DNS szolgáltatással kapcsolatos további információkért lásd: hello [Azure DNS áttekintése](dns-overview.md).
* DNS automatizálásával kapcsolatos további információkért lásd: [létrehozása DNS-zónák és a rekordhalmazok használatával hello .NET SDK](dns-sdk.md).
* Névkeresési DNS-rekordok kapcsolatos további információkért lásd: [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).
