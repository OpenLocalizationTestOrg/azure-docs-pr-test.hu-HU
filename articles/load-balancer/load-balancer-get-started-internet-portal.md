---
title: "az internetre irányuló aaaCreate terheléselosztó - Azure-portál |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre irányuló terheléselosztót a Resource Manager használatával hello Azure-portálon"
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: annahar
ms.openlocfilehash: 390ba8ec1474c54cf2c0022c4a3c219d21b1a659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-hello-azure-portal"></a>Az Internet felé néző terheléselosztói hello Azure-portál használatával

> [!div class="op_single_selector"]
> * [Portál](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre irányuló terheléselosztót használja a klasszikus üzembe helyezési](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Ez magában foglalja a hello sorozata, amelyek végzett toobe toocreate olyan terheléselosztóhoz, és részletesen bemutatják, mi történik-e tooaccomplish hello cél egyedi feladatok.

## <a name="what-is-required-toocreate-an-internet-facing-load-balancer"></a>Mi az a szükséges toocreate egy internetre irányuló terheléselosztót?

Toocreate kell, és a következő objektumok toodeploy terheléselosztó hello konfigurálása.

* Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.
* Háttér-címkészlet - hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC) tartalmazza.
* Terheléselosztási szabályok - hello load balancer tooport hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Bejövő NAT-szabályok – hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Mintavétel - állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.

További információkat szerezhet a terheléselosztónak az Azure Resource Managerben használt összetevőiről [Az Azure Resource Manager által nyújtott támogatás a terheléselosztó számára](load-balancer-arm.md) című részben.

## <a name="set-up-a-load-balancer-in-azure-portal"></a>Terheléselosztó beállítása az Azure Portalon

> [!IMPORTANT]
> A példa feltételezi, hogy Ön rendelkezik egy **myVNet** nevű virtuális hálózattal. Tekintse meg a túl[hozzon létre virtuális hálózatot](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) toodo ez. Azt is feltételezi, hogy van egy alhálózaton belül **myVNet** nevű **LB-alhálózatot kell** és két virtuális gépek nevű **web1** és **web2** rendre belül egyazon rendelkezésre állási csoportban hívott hello **myAvailSet** a **myVNet**. Tekintse meg a túl[Ez a hivatkozás](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toocreate virtuális gépeket.

1. Egy böngészőből keresse meg a toohello Azure-portálon: [http://portal.azure.com](http://portal.azure.com) és az Azure-fiókkal történő bejelentkezés.
2. Hello felső bal oldalán hello képernyőn válassza ki **új** > **hálózati** > **terheléselosztóhoz.**
3. A hello **létrehozás terheléselosztó** panelen írja be a terheléselosztó nevét. Most **myLoadBalancer** a neve.
4. A **Típus** alatt válassza ki a **Nyilvános** elemet.
5. A **Nyilvános IP-cím** alatt hozzon létre egy új nyilvános IP-címet **myPublicIP** néven.
6. Az Erőforráscsoport alatt válassza ki a **myRG** elemet. Ezután válassza ki a megfelelő **helyet**, majd kattintson az **OK** gombra. hello terheléselosztó toodeploy majd elindul, és toosuccessfully teljes telepítési eltarthat néhány percig.

    ![Terheléselosztó erőforráscsoportjának frissítése](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)

## <a name="create-a-back-end-address-pool"></a>Háttércímkészlet létrehozása

1. A terheléselosztó sikeres üzembe helyezését követően válassza ki azt az erőforrások közül. A Beállítások alatt válassza ki a Háttérkészletek elemet. Írja be a háttérkészlet nevét. Kattintson a hello **Hozzáadás** gomb hello felső részén megjelenő hello panel felé.
2. Kattintson a **adja hozzá a virtuális gépek** a hello **háttérkészlet hozzáadása** panelen.  A **Rendelkezésre állási készlet** alatt válassza ki a **Rendelkezésre állási készlet kiválasztása** lehetőséget, majd válassza ki a **myAvailSet** elemet. Válassza ki, **válassza ki a virtuális gépek hello** a hello virtuális gépek szakaszában hello panelen, majd kattintson a **web1** és **web2**, hello két virtuális gépeket, a terheléselosztás létrehozása. Győződjön meg arról, hogy mindkét balra, ahogy az alábbi képen hello kék jelölését toohello. Kattintson a **válassza** az adott panelhez hello OK után **válassza a virtuális gépek** panelen, majd **OK** a hello **háttérkészlet hozzáadása** panelen.

    ![Toohello háttércímkészletet - hozzáadása ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Ellenőrizze, hogy az értesítések legördülő lista toomake frissítés van a virtuális gépek hello mindkét hello terheléselosztó háttérkészletének a hozzáadása tooupdating hello hálózati illesztő mentése vonatkozó **web1** és **web2**.

## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Mintavétel, LB-szabály és NAT-szabályok létrehozása

1. Hozzon létre egy állapotmintát.

    A terheléselosztó Beállítások menüjéből válassza ki a Mintavételek elemet. Kattintson a **Hozzáadás** hello panel hello tetején található.

    A mintavétel két módon tooconfigure nincsenek: HTTP vagy TCP. Ez a példa a HTTP módszert mutatja be, de a TCP is hasonló módon konfigurálható.
    Hello szükséges információ frissítése Amint már említettük, a **myLoadBalancer** betölti a 80-as portra az elosztott terhelésű forgalmat. a kiválasztott hello elérési út HealthProbe.aspx, időköz: 15 másodperc, és sérült küszöbérték 2. Amikor elkészült, kattintson a **OK** toocreate hello mintavétel.

    Vigye az egérmutatót hello "i" ikon toolearn további ezek egyedi, valamint hogyan el módosított toocater tooyour követelményeinek.

    ![Mintavétel hozzáadása](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Hozzon létre egy terheléselosztó-szabályt.

    Kattintson a terheléselosztási szabályok a hello a terheléselosztó beállítások területén. Hello új panel, kattintson a **Hozzáadás**. Adjon nevet a szabálynak. Itt a neve HTTP. Válassza ki a hello elülső rétegbeli portot és a háttérportnak. Ebben a példában mindkettőnek 80-as port van kiválasztva. Válasszon **LB-háttérrendszer** a háttérkészlet és a korábban létrehozott hello **HealthProbe** , hello mintavétel. Más konfigurációk tooyour követelmények szerint állíthat be. Majd kattintson az OK toosave hello terheléselosztási szabály.

    ![Terheléselosztási szabály hozzáadása](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Bejövő NAT-szabályok létrehozása

    Kattintson a hello beállítások szakaszban a terheléselosztó bejövő forgalmat kezelő NAT-szabályok. Hello új panel, amelyen, kattintson a **Hozzáadás**. Ezután adjon nevet a bejövő NAT-szabálynak. Itt **inboundNATrule1** a neve. hello cél nyilvános IP-címet korábban létrehozott hello kell lennie. Válassza az egyéni szolgáltatásban, és válassza ki szeretné toouse hello protokoll. Itt a TCP van kiválasztva. Adja meg a hello port 3441, és a célportnak, ebben az esetben hello 3389-es. Ez a szabály majd kattintson az OK toosave.

    Hello első szabály létrehozása után ismételje meg ezt a lépést hello második bejövő NAT-szabály inboundNATrule2 hívása port 3442 tooTarget 3389-es portot.

    ![Bejövő NAT-szabály hozzáadása](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Terheléselosztó eltávolítása

azt szeretné, hogy tooremove toodelete olyan terheléselosztóhoz, jelölje be hello terheléselosztó. A hello *terheléselosztó* panelen kattintson a **törlése** hello panel hello tetején található. Ezután válassza ki az **Igen** lehetőséget, ha a rendszer megkérdezi.

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-cli.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
