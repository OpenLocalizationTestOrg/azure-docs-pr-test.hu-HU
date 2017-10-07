---
title: "aaaManage NSG-ket hello Azure-portál használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage meglévő NSG-ket hello Azure-portál használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: ad9a4060bd81bae4597ad5a4f59622e10cd214cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-nsgs-using-hello-portal"></a>Hello portálon NSG-k kezelése

> [!div class="op_single_selector"]
> * [Portál](virtual-network-manage-nsg-arm-portal.md)
> * [PowerShell](virtual-network-manage-nsg-arm-ps.md)
> * [Azure CLI](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Információk beolvasása
Megtekintheti a meglévő NSG-ket, szabályok lekérdezni egy meglévő NSG-t, és megtudhatja, milyen erőforrásokat egy NSG társítva.

### <a name="view-existing-nsgs"></a>Meglévő NSG-k megtekintése

tooview minden NSG-k létezik az előfizetés, a következő lépéseket teljes hello:

1. Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.

2. Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Ellenőrizze a hello hello az NSG-k listája **hálózati biztonsági csoportok** panelen.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a>Az erőforráscsoport nézet NSG-k

tooview hello listája NSG-ket a hello **RG-NSG** erőforráscsoport, a teljes hello a következő lépéseket:

1. Kattintson a **erőforráscsoportok >** > **RG-NSG** > **...** .

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. A hello az erőforrások listájához, keresse meg elemek megjelenítése hello NSG ikonra, ahogy az hello **erőforrások** az alábbi panelen.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a>A szabályok egy NSG listázása

az NSG nevű tooview hello szabályainak **NSG-előtér**, teljes hello a következő lépéseket:

1. A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.

2. A hello **beállítások** lapra, majd **bejövő biztonsági szabályok**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Hello **bejövő biztonsági szabályok** panel alább látható módon jelenik meg.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. A hello **beállítások** lapra, majd **kimenő biztonsági szabályok** toosee hello kimenő szabályok.

    > [!NOTE]
    > alapértelmezett szabályok tooview, kattintson a hello **alapértelmezett szabályok** ikon hello szabályok megjelenítő panelen hello hello tetején.
    >

### <a name="view-nsgs-associations"></a>Az NSG-társításának megtekintéséhez

tooview milyen erőforrásokat hello **NSG-előtérbeli** NSG a következő lépéseket a társítása, teljes hello:

1. A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.

2. A hello **beállítások** lapra, majd **alhálózatok** tooview milyen alhálózat társított toohello NSG.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. A hello **beállítások** lapra, majd **hálózati illesztőt** tooview hálózati adapterek Mik társított toohello NSG.

## <a name="manage-rules"></a>Szabályok kezelése
Adja hozzá a meglévő NSG szabályok tooan, szerkesztheti a meglévő szabályokat, és törölje a szabályokat.

### <a name="add-a-rule"></a>Szabály hozzáadása
egy szabály, amely lehetővé teszi tooadd **bejövő** forgalom tooport **443-as** bármely gépen toohello a **NSG-előtér** NSG-t, a következő lépéseket teljes hello:

1. A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.
2. A hello **beállítások** lapra, majd **bejövő biztonsági szabályok**.
3. A hello **bejövő biztonsági szabályok** panelen kattintson a **Hozzáadás**. Ezt követően a hello **Hozzáadás bejövő biztonsági szabály** panelen, töltse ki a hello értékek alább látható módon, és kattintson **OK**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    Néhány másodperc múlva figyelje meg az új szabályt hello hello **bejövő biztonsági szabályok** panelen.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Szabály módosítása
a fenti tooallow létrehozott toochange hello szabály hello érkező bejövő adatforgalmat **Internet** lépések csak, teljes hello:

1. A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.
2. A hello **beállítások** fülre, kattintson a fenti létrehozott hello szabály.
3. A hello **engedélyezése https** panelen, a módosítás hello **forrás** tulajdonság alább látható módon, és kattintson **mentése**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Szabály törlése

toodelete hello szabály létrehozott fent, teljes hello a következő lépéseket:

1. A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.
2. A hello **beállítások** fülre, kattintson a fenti létrehozott hello szabály.
3. A hello **engedélyezése https** panelen kattintson a **törlése**, és kattintson a **Igen**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Társítások kezelése
Az NSG toosubnets és a hálózati adapterek is hozzárendelhető. Is is leválasztja az összes erőforrásból, amelyekhez társítva vannak a NSG.

### <a name="associate-an-nsg-tooa-nic"></a>Társítson egy NSG tooa hálózati adapter
tooassociate hello **NSG-előtérbeli** NSG toohello **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:

1. A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.
2. A hello **beállítások** lapra, majd **hálózati illesztőt** > **társítása** > **TestNICWeb1**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>A társítást egy NSG-t a hálózati Adapterhez

toodissociate hello **NSG-előtérbeli** hello az NSG **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:

1. A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestNICWeb1**.

2. A hello **TestNICWeb1** paneljén kattintson **biztonsági módosítása...**   >  **Nincs**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> A panel tooassociate hello NIC tooany meglévő NSG-t is használhatja.
>

### <a name="dissociate-an-nsg-from-a-subnet"></a>Az NSG alhálózatból származó leválasztani

toodissociate hello **NSG-előtérbeli** hello az NSG **előtér** alhálózat, a teljes hello a következő lépéseket:

1. A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestVNet**.

2. A hello **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport**  >  **Nincs**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. A hello **előtér** panelen kattintson a **mentése**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-tooa-subnet"></a>Társítsa az NSG-tooa alhálózatot.

tooassociate hello **NSG-előtérbeli** NSG toohello **FronEnd** alhálózati újra, a teljes hello a következő lépéseket:

1. A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestVNet**.
2. A hello **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport**  >  **NSG-előtérbeli**.
3. A hello **előtér** panelen kattintson a **mentése**.

> [!NOTE]
> Egy NSG tooa alhálózathoz a thh NSG-t is rendelhet **beállítások** panelen.
>

## <a name="delete-an-nsg"></a>Az NSG törlése
Ha nem kapcsolódnak hozzá erőforrás tooany csak törlése egy NSG. az NSG-t, a lépéseket követve teljes hello toodelete:.

1. A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **NSG-előtérbeli**.
2. A hello **beállítások** panelen kattintson a **hálózati illesztőt**.
3. Ha egyetlen hálózati adapterrel felsorolt, hello NIC kattintson, és hajtsa végre a 2. lépés [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC).
4. Ismételje meg a 3. lépés az egyes hálózati adapterhez.
5. A hello **beállítások** panelen kattintson a **alhálózatok**.
6. Ha nincsenek felsorolva alhálózatok, kattintson a hello alhálózatot, és kövesse a 2. és 3 [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet).
7. Bal oldali toohello görget **NSG-előtér** panelen, majd kattintson a **törlése** > **Igen**.

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Következő lépések
* [Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.
