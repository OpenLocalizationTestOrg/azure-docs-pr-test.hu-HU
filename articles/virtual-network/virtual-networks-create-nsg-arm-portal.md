---
title: "aaaManage hálózati biztonsági csoport – az Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage hálózati biztonsági csoportok használatával hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: faee5ac8-f4c4-4f97-ade5-197a37aad496
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53fb29e60cbc2a535f6cf03e430d9e703e97b216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-portal"></a>Hello Azure-portál használatával a hálózati biztonsági csoportok kezelése

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [NSG-k létrehozása hello klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

hello minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell fenti hello forgatókönyv alapján. Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon. hello használata alatt lépések **RG-NSG** , hello tanúsítványsablon-nevet, hello erőforrás csoport hello állított be.

## <a name="create-hello-nsg-frontend-nsg"></a>NSG-előtérbeli NSG hello létrehozása
toocreate hello **NSG-előtérbeli** NSG hello forgatókönyv a fenti ábrán kövesse az alábbi hello lépéseket.

1. Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.
2. Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. A hello **hálózati biztonsági csoportok** panelen kattintson a **Hozzáadás**.
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. A hello **hálózati biztonsági csoport létrehozása** panelen, hozzon létre egy NSG nevű *NSG-előtér* a hello *RG-NSG* erőforráscsoportban, és kattintson **létrehozása**.
   
    ![Azure portál – NSG-k](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Szabályok létrehozása egy létező NSG-ben
egy meglévő NSG hello Azure-portálon a toocreate szabályait kövesse hello lépéseket.

1. Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.
2. Az NSG-k hello listájában kattintson **NSG-előtérbeli** > **bejövő biztonsági szabályok**
   
    ![Azure portál – NSG-előtér](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. Hello listájában **bejövő biztonsági szabályok**, kattintson a **Hozzáadás**.
   
    ![Azure portál – szabály hozzáadása](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. A hello **Hozzáadás bejövő biztonsági szabály** panelen nevű szabályt létrehozni *web-szabály* prioritását *200* keresztül hozzáférést *TCP* tooport *80* tooany VM bármelyik forrás-, és kattintson a **OK**. Figyelje meg, hogy ezek a beállítások a legtöbb alapértelmezett értékei lesznek már.
   
    ![Azure portál – szabálybeállításai](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Néhány másodpercen belül megjelenik hello NSG hello új szabályt.
   
    ![Azure portál – új szabály](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Ismételje meg a too6 toocreate egy bejövő forgalomra vonatkozó szabály nevű *rdp-szabály* prioritással *250* keresztül hozzáférést *TCP* tooport *3389-es* a forrás virtuális gép tooany.

## <a name="associate-hello-nsg-toohello-frontend-subnet"></a>Társítsa a hello NSG toohello FrontEnd alhálózathoz
1. Kattintson a **Tallózás >** > **erőforráscsoportok** > **RG-NSG**.
2. A hello **RG-NSG** paneljén kattintson **...**   >  **TestVNet**.
   
    ![Azure portál – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. A hello **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport**  >  **NSG-előtérbeli**.
   
    ![Azure portál – az alhálózati beállítások](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. A hello **előtér** panelen kattintson a **mentése**.
   
    ![Azure portál – az alhálózati beállítások](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-hello-nsg-backend-nsg"></a>Hello NSG NSG-háttéralkalmazás létrehozása
toocreate hello **NSG-háttérrendszer** NSG és társítsa azt toohello **háttér** alhálózati, hajtsa végre hello alábbi lépéseket.

1. Ismétlődő hello szükséges lépések [létrehozás hello NSG-előtérbeli NSG](#Create-the-NSG-FrontEnd-NSG) toocreate egy NSG nevű *NSG-háttérrendszer*
2. Ismétlődő hello szükséges lépések [egy meglévő NSG-szabályok létrehozására](#Create-rules-in-an-existing-NSG) toocreate hello **bejövő** hello az alábbi táblázatban a szabályok.
   
   | Bejövő forgalomra vonatkozó szabály | Kimenő forgalomra vonatkozó szabály |
   | --- | --- |
   | ![Azure portál – bejövő forgalomra vonatkozó szabály](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portál – kimenő forgalomra vonatkozó szabály](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Ismétlődő hello szükséges lépések [hello NSG toohello FrontEnd alhálózathoz társítása](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG-háttérrendszer** NSG toohello **háttér** alhálózat.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[meglévő NSG-k kezelése](virtual-network-manage-nsg-arm-portal.md)
* [Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.

