---
title: "aaaMultiple IP-címek az Azure virtuális gépek - portál |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gép használt hello Azure portálon |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
ms.openlocfilehash: 34075766ac68c8de38c258a4d70e35881f28bb0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-hello-azure-portal"></a>Rendelje hozzá több IP címek toovirtual gép hello Azure-portál használatával

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
Ez a cikk azt ismerteti, hogyan toocreate keresztül hello Azure Resource Manager deployment model használatával egy virtuális gép (VM) hello Azure-portálon. Több IP-cím hello klasszikus telepítési modell használatával létrehozott tooresources nem lehet hozzárendelni. További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása

Ha azt szeretné, hogy a virtuális gép több IP-címet, vagy hozzá statikus magánhálózati IP-cím és toocreate, hozza létre azt a PowerShell vagy Azure CLI hello. Hello PowerShell vagy a CLI-beállítások ezen cikk toolearn hello tetején kattintson a módját. Létrehozhat egy virtuális Gépet egy egyetlen dinamikus magánhálózati IP-cím és (opcionálisan) hello hello utasításait követve hello portál használatával egyetlen nyilvános IP-cím [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md) vagy [hozzon létre egy Linux virtuális Gépet](../virtual-machines/linux/quick-create-portal.md) cikkek. Hello virtuális gép létrehozása után módosítsa dinamikus toostatic hello IP-cím típusa, és kövesse a hello hello portál használatával további IP-címek hozzáadása [hozzáadása IP-címek tooa VM](#add) című szakaszát.

## <a name="add"></a>Adja hozzá az IP-címek tooa méretű VM

Magán- és IP címeket tooa NIC hello következő lépések végrehajtásával adhat hozzá. hello hello a következő szakaszokban szereplő példák azt feltételezik, hogy már van egy virtuális gép hello három IP-konfigurációk hello ismertetett [forgatókönyv](#Scenario) ezen cikk, de nem szükséges, hogy végezzen.

### <a name="coreadd"></a>Alapvető lépéseket

1. Keresse meg a toohello https://portal.azure.com és bejelentkezési Azure-portálon is, ha szükséges.
2. Hello portálon kattintson **további szolgáltatások** > típus *virtuális gépek* hello a Szűrő mezőbe, és kattintson a **virtuális gépek**.
3. A hello **virtuális gépek** panelen kattintson a virtuális gép kívánt tooadd IP-címek hello. Kattintson a **hálózati illesztőt** hello virtuális gépek paneljét, amely akkor jelenik meg, és jelölje ki hello hálózati kapcsolat tooadd hello IP-címek. Hello látható hello példát a következő képen, hello nevű hálózati *myNIC* hello nevű virtuális gép a *myVM* van kiválasztva:

    ![Hálózati illesztő](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. Hello paneljén megjelenő hello kijelölt hálózati kattintson **IP-konfigurációk**.

Teljes hello valamelyik hello szakaszokban szereplő lépések, hello típusú IP-címek alapján kívánja a tooadd.

### <a name="add-a-private-ip-address"></a>**Adja hozzá a magánhálózati IP-cím**

Hajtsa végre a következő lépéseket tooadd egy új magánhálózati IP-cím hello:

1. Teljes hello szükséges lépések hello [alapvető lépéseket](#coreadd) című szakaszát.
2. Kattintson az **Add** (Hozzáadás) parancsra. A hello **hozzáadása IP-konfiguráció** panel, amely akkor jelenik meg, egy IP-konfiguráció létrehozásának *IPConfig-4* rendelkező *10.0.0.7* , egy *statikus* magánhálózati IP-címe címet, majd kattintson az **OK**.

    > [!NOTE]
    > Ad hozzá egy statikus IP-címet, ha egy nem használt, érvényes címet kell megadnia hello alhálózati hello hálózati adapter csatlakozik. Ha bejelöli hello cím nem érhető el, hello portálon jelennek meg az X hello IP-cím, és tooselect egy másik lesz szüksége.

3. Miután az OK gombra kattint, hello panel bezárul, és látni fogja, hello felsorolt új IP-konfigurációja. Kattintson a **OK** tooclose hello **hozzáadása IP-konfiguráció** panelen.
4. Kattinthat **Hozzáadás** tooadd további IP-konfiguráció, vagy zárjon be minden nyitott paneleken toofinish hozzáadását IP-címeket.
5. Hozzáadás hello privát IP-címek toohello virtuális gép operációs rendszer hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.

### <a name="add-a-public-ip-address"></a>A nyilvános IP-cím hozzáadása

A nyilvános IP-cím jelenik meg egy új IP-konfigurációt vagy egy meglévő IP-konfigurációja nyilvános IP-cím erőforrás tooeither társításával.

> [!NOTE]
> Nyilvános IP-címek névleges díjat kell. tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.
> 

### <a name="create-public-ip"></a>Hozzon létre egy nyilvános IP-cím erőforrás

Egy nyilvános IP-címet az egyik beállítása egy nyilvános IP-cím erőforrás. Ha egy nyilvános IP-cím erőforrás, amelyet jelenleg társított tooan IP-konfigurációja érvénytelen tooassociate tooan IP-konfiguráció, hagyja ki a következő lépéseket hello, és végrehajtásához hello, az alábbi szakaszokban hello összes kívánt beállítást. Ha nincs elérhető nyilvános IP-cím erőforrás, hajtsa végre a következő lépéseket toocreate egy hello:

1. Keresse meg a toohello https://portal.azure.com és bejelentkezési Azure-portálon is, ha szükséges.
3. Hello portálon kattintson **új** > **hálózati** > **nyilvános IP-cím**.
4. A hello **nyilvános IP-cím létrehozása** a megjelenő panelen adjon meg egy **neve**, jelölje be egy **IP-cím hozzárendelése** típus, egy **előfizetés**, egy **Erőforráscsoport**, és egy **hely**, majd kattintson a **létrehozása**, ahogy az alábbi képen hello:

    ![Hozzon létre egy nyilvános IP-cím erőforrás](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Teljes hello tooassociate hello nyilvános IP-cím erőforrás tooan IP konfigurációja az alábbi szakaszokban hello szükséges lépések.

#### <a name="associate-hello-public-ip-address-resource-tooa-new-ip-configuration"></a>Rendeljen hozzá hello nyilvános IP-cím erőforrás tooa új IP konfigurációja

1. Teljes hello szükséges lépések hello [alapvető lépéseket](#coreadd) című szakaszát.
2. Kattintson az **Add** (Hozzáadás) parancsra. A hello **hozzáadása IP-konfiguráció** panel, amely akkor jelenik meg, egy IP-konfiguráció létrehozásának *IPConfig-4*. Hello engedélyezése **nyilvános IP-cím** jelöljön ki egy meglévő, elérhető nyilvános IP-cím erőforrás hello **nyilvános IP-cím kiválasztása** panel, amely akkor jelenik meg.

    Hello nyilvános IP-cím erőforrás kijelölése után kattintson **OK** és hello panel bezárul. Ha egy meglévő nyilvános IP-cím nincs, létrehozhat egy hello lépések végrehajtásával a hello [hozzon létre egy nyilvános IP-cím erőforrás](#create-public-ip) című szakaszát. 

3. Felülvizsgálati hello új IP-konfigurációja. Annak ellenére, hogy a magánhálózati IP-cím nincs explicit módon hozzárendelt, egy automatikusan kapott toohello IP-konfigurációja, mivel az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie.
4. Kattinthat **Hozzáadás** tooadd további IP-konfiguráció, vagy zárjon be minden nyitott paneleken toofinish hozzáadását IP-címeket.
5. Adja hozzá a hello titkos IP-cím toohello virtuális gép operációs rendszere hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-cím toohello operációs rendszere nem adja hozzá.

#### <a name="associate-hello-public-ip-address-resource-tooan-existing-ip-configuration"></a>Rendeljen hozzá hello nyilvános IP-cím erőforrás tooan meglévő IP konfigurációja

1. Teljes hello szükséges lépések hello [alapvető lépéseket](#coreadd) című szakaszát.
2. Kattintson a kívánt tooadd hello nyilvános IP-cím erőforrás a hello IP-konfigurációt.
3. Hello IPConfig panelen megjelenő kattintson **IP-cím**.
4. A hello **nyilvános IP-cím kiválasztása** panel, amelyen megjelenik, válassza ki a nyilvános IP-cím.
5. Kattintson a **mentése** és hello paneleken bezárul. Ha egy meglévő nyilvános IP-cím nincs, létrehozhat egy hello lépések végrehajtásával a hello [hozzon létre egy nyilvános IP-cím erőforrás](#create-public-ip) című szakaszát.
3. Felülvizsgálati hello új IP-konfigurációja.
4. Kattinthat **Hozzáadás** tooadd további IP-konfiguráció, vagy zárjon be minden nyitott paneleken toofinish hozzáadását IP-címeket. Hello nyilvános IP-cím toohello operációs rendszere nem adja hozzá.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
