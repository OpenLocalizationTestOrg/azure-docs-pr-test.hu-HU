---
title: "aaaTroubleshoot útvonalak - portál |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot útvonalak hello Azure Resource Manager telepítési modell használatával hello Azure portálon."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bdd8b6dc-02fb-4057-bb23-8289caa9de89
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 579bae91ef3200852032b3953d3cc5d26deada86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-hello-azure-portal"></a>Útvonalak hello Azure portál használatával hibaelhárítása
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
>
>

Ha a hálózati csatlakozási problémák tooor az az Azure virtuális gép (VM) tapasztal, útvonalak előfordulhat, hogy mely negatív hatással lehet a VM forgalmat. Ez a cikk áttekintést diagnosztikai képességek útvonalak toohelp további hibaelhárítást kell végeznie.

Az útvonaltáblák alhálózatok tartoznak, és minden hálózati interfészen (NIC) alhálózat érvényben. a következő típusú útvonalak hello lehet alkalmazott tooeach hálózati adapter:

* **Rendszerútvonalak:** alapértelmezés szerint az Azure Virtual Network (VNet) létrehozott összes alhálózatot rendelkezik rendszer útvonaltábláit, amelyek lehetővé teszik a helyi VNet forgalmát, a helyszíni forgalom keresztül VPN-átjárók és internetes forgalmat. Rendszerútvonalak társviszonyban Vnetek esetében is létezik.
* **BGP-útvonalakat:** propagálása toonetwork felületek expressroute-on vagy VPN-kapcsolatok pont-pont keresztül. Tudjon meg többet a BGP-útválasztás hello olvasásával [BGP a VPN-átjárók](../vpn-gateway/vpn-gateway-bgp-overview.md) és [ExpressRoute – áttekintés](../expressroute/expressroute-introduction.md) cikkeket.
* **Felhasználó által definiált útvonalak (UDR):** Ha hálózati virtuális készülékek használata, vagy a rendszer kényszerített bújtatás forgalom tooan a helyi hálózaton keresztül a telephelyek közötti VPN, előfordulhat, hogy felhasználói (udr-EK) rendelt útvonalakat a alhálózati útválasztási táblázatot. Ha még nem ismeri a udr-EK, olvassa el a hello [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md#user-defined-routes) cikk.

Hello alkalmazott tooa hálózati kapcsolat lehet különböző útvonalakat után lehet nehéz toodetermine összesített útvonalakat érvényben. toohelp hibáinak elhárítása a virtuális gép hálózati kapcsolatot, akkor az összes hello hatékony útvonal egy adott hálózati csatoló hello Azure Resource Manager üzembe helyezési modellben.

## <a name="using-effective-routes-tootroubleshoot-vm-traffic-flow"></a>Hatékony útvonalak tootroubleshoot VM adatforgalmat használatával
Ez a cikk hello forgatókönyv szerint egy példa tooillustrate hogyan tootroubleshoot hello hatályos irányítja a hálózati illesztő a következő használja:

A virtuális gép (*VM1*) toohello VNet csatlakoztatva (*VNet1*, előtag: 10.9.0.0/16) tooconnect tooa újonnan peered vnet VM(VM3) sikertelen (*VNet3*, 10.10.0.0/16 előtag). Nincsenek nem udr-EK vagy a BGP irányítja a alkalmazott tooVM1-NIC1 hálózathoz csatlakozó adapter toohello VM, csak a rendszerútvonalak érvényesek.

Ez a cikk azt ismerteti, hogyan toodetermine hello oka hello csatlakozási hiba, hatékony útvonalak funkció használata Azure Resource Manager üzembe helyezési modellben.
Amíg hello példában csak a rendszerútvonalak, ugyanezek a lépések hello lehet használt toodetermine bejövő és kimenő kapcsolódási hibák útvonal bármilyen keresztül.

> [!NOTE]
> Ha a virtuális Géphez kapcsolt egynél több hálózati Adaptert, ellenőrizze a hatékony útvonalak az egyes hálózati adapterek toodiagnose hálózati csatlakozási problémák tooand VM hello.
>
>

### <a name="view-effective-routes-for-a-virtual-machine"></a>A virtuális gépek hatékony útvonalak megtekintése
toosee hello összesített útvonalakat, amelyek alkalmazott tooa VM, teljes hello a következő lépéseket:

1. Bejelentkezés az Azure portálon, a https://portal.azure.com toohello.
2. Kattintson a **további szolgáltatások**, majd kattintson a **virtuális gépek** hello listában megjelenő.
3. Jelölje ki a virtuális gép tootroubleshoot hello listában megjelenő, és a beállítások egy virtuális gép panel jelenik meg.
4. Kattintson a **derítse & felmerülő problémák megoldásához** , és válassza a gyakori probléma. Ehhez a példához **toomy Windows virtuális gép nem lehet csatlakozni az** van kiválasztva.

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)
5. Az alábbi képen hello módon lépéseket hello probléma alatt jelenik meg:

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Kattintson a *hatékony útvonalak* a lépések hello listáját.
6. Hello **hatékony útvonalak** panel jelenik meg, ahogy az alábbi képen hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Ha a virtuális gép egyetlen hálózati Adapterrel rendelkezik, az alapértelmezés szerint van kiválasztva. Ha egynél több hálózati adapter, jelölje ki a kívánt tooview hello hatékony útvonalak hello NIC.

   > [!NOTE]
   > Ha hello VM társított hello hálózati adapter nem fut, nem jelenik meg a hatékony útvonalak. Csak hello első 200 hatékony útvonalak láthatók hello portálon. Hello teljes listáját, kattintson a **letöltése**. További végezhet hello eredmények hello letöltött .csv fájlból.
   >
   >

    Figyelje meg a következő hello hello kimeneti:

   * **Forrás**: útvonal hello típusát jelzi. Rendszerútvonalak egyaránt megjelennek az helyeként *alapértelmezett*, udr-EK egyaránt megjelennek az helyeként *felhasználói* és átjáró útvonalak (statikus vagy BGP) jelennek meg *A(z)*.
   * **Állapot**: hello hatékony útvonal állapotát jelzi. A lehetséges értékek: *aktív* vagy *érvénytelen*.
   * **AddressPrefixes**: hello hatékony útvonal hello címelőtagot CIDR-formátumban határozza meg.
   * **nextHopType**: hello következő ugrást az adott útvonal hello jelzi. A lehetséges értékek: *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, vagy *Null*. Érték *Null* a **nextHopType** egy UDR jelezhetik érvénytelen útvonal. Például ha **nextHopType** van *VirtualAppliance* és hello hálózati virtuális készülék virtuális gép nem létesített/futó állapotban van. Ha **nextHopType** van *A(z)* és nincs átjáró kiépítése/fut a virtuális hálózat megadott hello, hello útvonal érvénytelen válhat.
7. Nincs a nem felsorolt útvonal toohello *WestUS-VNET3* VNet (10.10.0.0/16 előtag) a hello *WestUS-VNet1* (10.9.0.0/16 előtag) a hello kép hello előző lépésben. Az alábbi képen hello, hello társviszony-létesítési csatolás van hello *Disconnected* állapota:

    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    hello kétirányú hivatkozás a hello társviszony megszakad, amely ismerteti, miért VM1 nem tudott csatlakozni a hello tooVM3 *WestUS-VNet3* virtuális hálózat.
8. hello alábbi képen látható hello útvonalak hello kétirányú társviszony-létesítési hivatkozás létrehozása után:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

További hibaelhárítási forgatókönyveket a kényszerített bújtatás és útvonal értékelésre, olvassa el a hello [szempontok](virtual-network-routes-troubleshoot-portal.md#considerations) című szakaszát.

### <a name="view-effective-routes-for-a-network-interface"></a>Egy adott hálózati csatoló hatékony útvonalak megtekintése
Ha egy adott hálózati csatoló (NIC) kihatással van a hálózati adatforgalmat, megtekintheti hatékony útvonalak teljes listáját a hálózati adapter közvetlenül. toosee hello összesített útvonalakat, amelyek alkalmazott tooa hálózati adapter, teljes hello a következő lépéseket:

1. Bejelentkezés az Azure portálon, a https://portal.azure.com toohello.
2. Kattintson a **további szolgáltatások**, majd kattintson a **hálózati illesztői**
3. Keresési hello hello nevét egy hálózati Adaptert a listából, vagy megjelenő hello listából válassza ki. Ebben a példában **VM1-NIC1** van kiválasztva.
4. Válassza ki **hatékony útvonalak** a hello **hálózati illesztő** panelen, ahogy az alábbi képen hello:

       ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Hello **hatókör** kijelölt alapértelmezett toohello hálózati illesztőt.

      ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)

### <a name="view-effective-routes-for-a-route-table"></a>Egy útválasztási táblázatot hatékony útvonalak megtekintése
Ha módosítja a felhasználó által definiált útvonalak (udr-EK) a egy útválasztási táblázatot, érdemes lehet egy adott virtuális Gépet felvenni hello útvonalak tooreview hello hatását. Lehet, hogy egy útválasztási táblázatot alhálózatok tetszőleges számú társítani. Most már megtekintheti az összes hello hatékony útvonal az összes hálózati adapter egy adott útválasztási táblázatot alkalmazott, hello anélkül, hogy a megadott útvonal tábla panel hello tooswitch környezetben.

Az ebben a példában egy UDR (*UDRoute*) van megadva az útvonaltábla (*UDRouteTable*). Ez az útvonal elküldi az összes internetes forgalom *Alhalozat_1* a hello *WestUS-VNet1* virtuális hálózaton keresztül a hálózati virtuális készülék (NVA), a *Alhalozat_2* a hello ugyanazt a virtuális hálózatot. hello útvonal hello az alábbi képen látható:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

toosee hello összesített útvonalakat egy útválasztási táblázatot lépések teljes hello:

1. Bejelentkezés az Azure portálon, a https://portal.azure.com toohello.
2. Kattintson a **további szolgáltatások**, majd kattintson a **útvonaltáblát**
3. Keresési hello listája hello útválasztási táblázatot, azt szeretné, hogy a toosee összesített útvonalait, és jelölje ki. Ebben a példában **UDRouteTable** van kiválasztva. A kiválasztott hello útválasztási táblázatot egy panel jelenik meg, ahogy az alábbi képen hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)
4. Válassza ki **hatékony útvonalak** a hello **útvonaltábla** panelen. Hello **hatókör** kiválasztott toohello útvonaltábla van beállítva.
5. Egy útválasztási táblázatot lehet alkalmazott toomultiple alhálózatokat. Jelölje be hello **alhálózati** kívánt tooreview hello listából. Ebben a példában **Alhalozat_1** van kiválasztva.
6. Válassza ki a **hálózati adapter**. Az összes hálózati adapter kijelölt csatlakoztatott toohello alhálózati vannak felsorolva. Ebben a példában **VM1-NIC1** van kiválasztva.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

   > [!NOTE]
   > Ha nincs társítva van egy virtuális gép hálózati hello, nem hatékony útvonalak jelennek meg.
   >
   >

## <a name="considerations"></a>Megfontolandó szempontok
Útvonalak listája hello felülvizsgálata során szem előtt néhány dolgot tookeep adott vissza:

* Útválasztás alapján történik a leghosszabb előtag-megfeleltetés (LPM) között udr-EK, útvonalakat a BGP és a rendszer. Ha egynél több útvonal rendelkezik hello azonos LPM egyezik, majd egy útvonal kiválasztása sorrend hello Kiindulás alapján történik:

  * Felhasználó által megadott útvonal
  * BGP-útvonal
  * Rendszerútvonal (alapértelmezett)

    Hatékony útvonalakat hatékony útvonalakat, amelyek alapján az összes hello mavenen útvonal az LPM megfeleltetéssel csak kaphat. Jelenít meg, hogyan hello útvonalak ténylegesen kiértékeli a megadott hálózati adapter, így sokkal könnyebb tootroubleshoot adott útvonalak, előfordulhat, hogy mely negatív hatással lehet a virtuális gép és a kapcsolat.
* Ha udr-EK és a forgalom tooa hálózati virtuális készülék (NVA) üzenetet küld *VirtualAppliance* , **nextHopType**, győződjön meg arról, hogy az IP-továbbítás engedélyezve van-e a hello NVA fogadó hello forgalom vagy Eldobott csomagok.
* Ha kényszerített bújtatás engedélyezve van, minden kimenő internetforgalom nem útválasztásos tooon helyszíni. A virtuális gép nem működik ez a beállítás attól függően, hogy hogyan hello helyszíni Internet tooyour RDP/SSH a forgalmat kezeli.
  A kényszerített bújtatás engedélyezhető:
  * Ha használja a telephelyek közötti VPN, úgy, hogy egy felhasználó által megadott útvonal (UDR) a VPN-átjáróként nextHopType
  * Ha az alapértelmezett útvonal BGP keresztül lett hirdetve.
* Vnet-társviszony létesítése – forgalom toowork megfelelően, a rendszer útvonal **nextHopType** *VNetPeering* léteznie kell a hello társviszonyban virtuális hálózat előtag tartományon. Ha ilyen útvonal nem létezik, majd a Vnetben társviszony-létesítés hello hivatkozás OK néz ki:
  * Várjon néhány másodpercet, amíg, és próbálja meg újra, ha az újonnan létrehozott társviszony-létesítési hivatkozást. Alkalmanként tart hosszabb toopropagate útvonalak tooall hello hálózati illesztők alhálózat.
  * Hálózati biztonsági csoport (NSG) szabályok hello forgalom lehet, hogy érinti. További információkért lásd: hello [hibaelhárítása hálózati biztonsági csoportok](virtual-network-nsg-troubleshoot-portal.md) cikk.
