---
title: "virtuális gépek - Azure-portálon aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 474161303cdf8cb98e16ffd7cef6b74debdbc49a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-portal"></a>Hello Azure-portál használatával virtuális gépek magánhálózati IP-címek konfigurálása

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
> * [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
> * [Azure CLI](virtual-networks-static-private-ip-arm-cli.md)
> * [(Klasszikus) Azure-portálon](virtual-networks-static-private-ip-classic-pportal.md)
> * [PowerShell (klasszikus)](virtual-networks-static-private-ip-classic-ps.md)
> * [Az Azure CLI (klasszikus)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [statikus magánhálózati IP-cím hello klasszikus üzembe helyezési modellel kezelése](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

hello minta lépéseket már létrehozott egy egyszerű környezetben várható. Ha ebben a dokumentumban megjelenített szeretné toorun hello lépéseket, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-toocreate-a-vm-for-testing-static-private-ip-addresses"></a>Hogyan toocreate statikus magánhálózati IP-címe tesztelési virtuális gép címek
Nem be hozzá statikus magánhálózati IP-cím hello létrehozásakor a virtuális gépek erőforrás-kezelő a telepítési módban hello hello Azure-portál használatával. Először létre kell hoznia a virtuális gép hello, illeszti be a magánhálózati IP-toobe statikus.

toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet*, kövesse az alábbi hello lépéseket.

1. Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.
2. Kattintson a **új** > **számítási** > **Windows Server 2012 R2 Datacenter**, figyelje meg, hogy hello **telepítésimodellkiválasztása** lista már bemutatja **erőforrás-kezelő**, és kattintson a **létrehozása**, hello az alábbi ábrán látható módon.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. A hello **alapjai** panelen adja meg a létrehozott hello VM toobe hello nevét (*DNS01* a mi esetünkben), hello helyi rendszergazda fiókot és jelszót, hello az alábbi ábrán látható módon.
   
    ![Alapvető beállítások panel](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Győződjön meg arról, hogy hello **hely** kiválasztott *USA középső RÉGIÓJA*, kattintson a **válasszon meglévő** alatt **erőforráscsoport**, majd kattintson az **Erőforráscsoport** újra, majd kattintson a *TestRG*, és kattintson a **OK**.
   
    ![Alapvető beállítások panel](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. A hello **méret kiválasztása** panelen válassza **A1 szabványos**, és kattintson a **válasszon**.
   
    ![Válassza ki a méret panelen](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. Az a **beállítások** panelen, győződjön meg arról, hogy hello következő tulajdonságai vannak beállítva az alábbi hello értékekkel van beállítva, és kattintson a **OK**.
   
    -**A tárfiók**: *vnetstorage*
   
   * **Hálózati**: *TestVNet*
   * **Alhálózati**: *előtér*
     
     ![Válassza ki a méret panelen](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. A hello **összegzés** panelen kattintson a **OK**. Értesítés hello alábbi megjelenített mozaik az irányítópulton.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek
tooview hello statikus magánhálózati IP-címadatok hello hello lépéseket, a létrehozott virtuális gép hello lépésekkel hajtható végre.

1. Hello Azure Azure portálon kattintson **összes TALLÓZÁSA** > **virtuális gépek** > **DNS01** > **összes beállítások** > **hálózati illesztőt** hello felsorolt egyetlen hálózati adapteren majd.
   
    ![Virtuális gép csempe telepítése](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. A hello **hálózati illesztő** panelen kattintson a **összes beállítás** > **IP-címek** és a hirdetmény hello **hozzárendelés** és **IP-cím** értékeket.
   
    ![Virtuális gép csempe telepítése](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a>Hogyan tooadd statikus magánhálózati IP-címe kezelje a meglévő virtuális gép tooan
a statikus magánhálózati IP cím toohello hello fenti lépések használatával létrehozott virtuális gép tooadd kövesse hello alábbi lépéseket:

1. A hello **IP-címek** kattintson a panel látható a fenti **statikus** alatt **hozzárendelés**.
2. Típus *192.168.1.101* a **IP-cím**, és kattintson a **mentése**.
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Ha kattintás után **mentése** bizonyára észrevette, hogy hello hozzárendelés továbbra is túl beállítása**dinamikus**, az azt jelenti, hogy a beírt hello IP-cím már használatban van. Próbálja meg egy másik IP-címet.
> 
> 

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Hogyan tooremove egy statikus magánhálózati IP-VM
tooremove hello statikus magánhálózati IP-cím a virtuális gép, a fenti létrehozott hello hajtsa végre a következő lépés hello:

A hello **IP-címek** panelen látható a fenti kattintson **dinamikus** alatt **hozzárendelés**, és kattintson a **mentése**.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.
* További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.
* Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).

