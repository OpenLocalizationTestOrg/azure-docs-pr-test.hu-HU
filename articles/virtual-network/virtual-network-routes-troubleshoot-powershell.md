---
title: "aaaTroubleshoot útvonalak - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot irányítja a hello Azure Resource Manager üzembe helyezési modellel Azure PowerShell használatával."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 7a07806df5c1d0caee921187e6ad29f6755ab535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-azure-powershell"></a>Az Azure PowerShell útvonalak hibaelhárítása
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

### <a name="view-effective-routes-for-a-network-interface"></a>Egy adott hálózati csatoló hatékony útvonalak megtekintése
toosee hello összesített útvonalakat, amelyek alkalmazott tooa hálózati kapcsolat teljes hello a következő lépéseket:

1. Indítsa el az Azure PowerShell-munkamenet és bejelentkezési tooAzure. Ha nem ismeri az Azure PowerShell, olvassa el a hello [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.
2. hello parancs hatására az eredményobjektumoknak minden alkalmazott útvonalak tooa hálózati illesztő nevű *VM1-NIC1* hello erőforráscsoportban *RG1*.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Ha egy adott hálózati csatoló hello neve nem tudja, írja be a következő parancs tooretrieve hello nevét az összes hálózati adapter van egy erőforrás group.* hello
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   hello következő kimeneti keresi hasonló toohello kimeneti minden alkalmazott útvonal toohello alhálózati hello hálózati adapter csatlakozik:
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   Figyelje meg a következő hello hello kimeneti:
   
   * **Név**: hello hatékony útvonal lehet neve üres, kivéve, ha explicit módon megadott, felhasználó által definiált útvonalak. 
   * **Állapot**: hello hatékony útvonal állapotát jelzi. Lehetséges értékek: "Active" vagy "Érvénytelen"
   * **AddressPrefixes**: hello hatékony útvonal hello címelőtagot CIDR-formátumban határozza meg. 
   * **nextHopType**: hello következő ugrást az adott útvonal hello jelzi. A lehetséges értékek: *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, vagy *Null*. Érték *Null* a **nextHopType** egy UDR jelezhetik érvénytelen útvonal. Például ha **nextHopType** van *VirtualAppliance* és hello hálózati virtuális készülék virtuális gép nem létesített/futó állapotban van. Ha **nextHopType** van *A(z)* és nincs átjáró kiépítése/fut a virtuális hálózat megadott hello, hello útvonal érvénytelen válhat.
   * **NextHopIpAddress**: hello következő ugrás hello hatékony útvonal hello IP-címet adja meg.
   
   hello parancs hatására az eredményobjektumoknak hello útvonalak egy egyszerűbb tooview táblázatban:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   hello következő kimenete hello kimeneti hello forgatókönyvhöz a fentiekben ismertetett kapott egy részénél:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. Nincs a nem felsorolt útvonal toohello *WestUS-VNet3* VNet (a 10.10.0.0/16)** előtag *WestUS-VNet1* (előtag 10.9.0.0/16) hello kimenet hello előző lépésben. Ahogy az alábbi képen hello, hello hello a Vnetben társviszony-létesítési hivatkozás *WestUS-VNet3* VNet van hello *Disconnected* állapotát.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    hello kétirányú hivatkozás a hello társviszony megszakad, amely ismerteti, miért VM1 nem tudott csatlakozni a hello tooVM3 *WestUS-VNet3* virtuális hálózat. A telepítő egy kétirányú VNet társviszony-létesítési hivatkozást újra a *WestUS-VNet1* és *WestUS-VNet3* Vnetek. hello kimeneti visszaadott hello Vnetben társviszony-létesítési hivatkozás megfelelően létrejötte után a következőképpen:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    Miután hello probléma megállapításához is hozzáadhat, eltávolításához, vagy módosítsa a útvonalakat, és útvonaltáblát. Írja be a következő parancs toosee hello használt parancsok toodo listája úgy hello:
   
        Get-Help *-AzureRmRouteConfig

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
  * Hálózati biztonsági csoport (NSG) szabályok hello forgalom lehet, hogy érinti. További információkért lásd: hello [hibaelhárítása hálózati biztonsági csoportok](virtual-network-nsg-troubleshoot-powershell.md) cikk.

