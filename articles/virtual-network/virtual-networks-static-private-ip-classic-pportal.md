---
title: "magánhálózati IP-címek aaaConfigure virtuális gépek (klasszikus) – az Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek (klasszikus) hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df4bfa6768fc9e66db89785b633ffdb0274dbc46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-portal"></a>Egy virtuális gép (klasszikus) Azure-portálon hello saját IP-címek konfigurálása

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [hello Resource Manager üzembe helyezési modellel statikus magánhálózati IP-cím kezelése](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

hello minta lépéseket már létrehozott egy egyszerű környezetben várható. Ha ebben a dokumentumban megjelenített szeretné toorun hello lépéseket, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Hogyan toospecify egy statikus magánhálózati IP-virtuális gép létrehozásakor
toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet* statikus magánhálózati IP-címe a *192.168.1.101*, kövesse az alábbi hello lépéseket:

1. Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.
2. Kattintson a **új** > **számítási** > **Windows Server 2012 R2 Datacenter**, figyelje meg, hogy hello **telepítésimodellkiválasztása** lista már bemutatja **klasszikus**, és kattintson a **létrehozása**.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. A hello **hozzon létre virtuális gép** panelen adja meg a létrehozott hello VM toobe hello nevét (*DNS01* a mi esetünkben), hello helyi rendszergazda fiókot és jelszót.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Kattintson a **opcionális konfigurációs** > **hálózati** > **virtuális hálózati**, és kattintson a **TestVNet**. Ha **TestVNet** nem érhető el, ellenőrizze, hogy azok be hello *USA középső RÉGIÓJA* helyét, és ez a cikk hello elején leírt tesztkörnyezet hello hozott létre.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. A hello **hálózati** panelen ellenőrizze kijelölt meg arról, hogy hello alhálózat van *előtér*, majd kattintson a **IP-címek**, a **IP-cím hozzárendelése** kattintson **statikus**, majd adja meg *192.168.1.101* a **IP-cím** az alább látható módon.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Kattintson a **OK** a hello **IP-címek** panelen kattintson a **OK** a hello **hálózati** panelen, majd kattintson **OK**a hello **opcionális konfigurációs** panelen.
7. A hello **hozzon létre virtuális gép** panelen kattintson a **létrehozása**. Értesítés hello alábbi megjelenített mozaik az irányítópulton.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek
tooview hello statikus magánhálózati IP-címadatok hello hello lépéseket, a létrehozott virtuális gép hello lépésekkel hajtható végre.

1. Hello Azure Azure portálon kattintson **összes TALLÓZÁSA** > **virtuális gépek (klasszikus)** > **DNS01**  >   **Minden beállítás** > **IP-címek** , és figyelje meg, hello IP-cím hozzárendelése és IP-cím, az alább látható módon.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Hogyan tooremove egy statikus magánhálózati IP-VM
tooremove hello statikus magánhálózati IP-cím a virtuális gép, a fenti létrehozott hello hajtsa végre az alábbi hello lépéseket.

1. A hello **IP-címek** a fent látható panelen kattintson **dinamikus** toohello jobbra **IP-cím hozzárendelése**, majd kattintson **mentése**, majd Kattintson a **Igen**.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Hogyan tooadd statikus magánhálózati IP-címe kezelje a meglévő virtuális gép tooan
a statikus magánhálózati IP cím toohello hello fenti lépések használatával létrehozott virtuális gép tooadd kövesse hello alábbi lépéseket:

1. A hello **IP-címek** panelen látható a fenti kattintson **statikus** jobbra toohello **IP-cím hozzárendelése**.
2. Típus *192.168.1.101* a **IP-cím**, majd kattintson a **mentése**, és kattintson a **Igen**.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.
* További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.
* Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).

