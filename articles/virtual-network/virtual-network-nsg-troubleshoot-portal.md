---
title: "aaaTroubleshoot hálózati biztonsági csoportok - portál |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot hálózati biztonsági csoportok hello Azure Resource Manager telepítési modell hello Azure portál használatával."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 0d3d2110fe1507f36e3b933de924a0876db2747a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-hello-azure-portal"></a>Hálózati biztonsági csoportok hello Azure portál használatával hibaelhárítása
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Ha konfigurálva a hálózati biztonsági csoportokkal (NSG-k) a virtuális gépen (VM), és virtuális gép kapcsolódási problémákat tapasztal, ez a cikk áttekintést diagnosztikai képességek az NSG-k toohelp további hibaelhárítást kell végeznie.

Az NSG-k lehetővé teszik a toocontrol hello típusú forgalom, hogy a virtuális gépek (VM) mindkét folyamata. NSG-ket lehet alkalmazott toosubnets egy Azure virtuális hálózatot (VNet), hálózati adapterek (NIC) vagy mindkettőt. hello hatékony szabályokat tooa hálózati adapter létező hello alkalmazott NSG-k tooa hálózati adapter és hello alhálózaton van csatlakoztatva hello szabály összesítést. Ezek az NSG-k között szabályok néha ütköznek egymással, és hatással lehet a virtuális gépek hálózati kapcsolattal.  

Az NSG-k, az összes hello hatékony biztonsági szabály tekintheti meg a virtuális gép hálózati adapterek alkalmazott. Ez a cikk bemutatja, hogyan tootroubleshoot VM ezeket szabályok használata a problémák hello Azure Resource Manager üzembe helyezési modellben. Ha még nem ismeri a virtuális hálózat és NSG fogalmakat, olvassa el a hello [virtuális hálózati](virtual-networks-overview.md) és [hálózati biztonsági csoportok](virtual-networks-nsg.md) áttekintése cikkeket.

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a>Hatékony biztonsági szabályok tootroubleshoot VM adatforgalmat használatával
hello-forgatókönyvekben, amelyek a következő gyakori kapcsolati probléma példája:

A virtuális gépek nevű *VM1* nevű alhálózat része *Alhalozat_1* nevű egy Vneten belül *WestUS-VNet1*. Sikertelen a virtuális gépet RDP a 3389-es TCP-porton keresztül egy kísérlet tooconnect toohello. Az NSG-k mindkét hello NIC szintjén alkalmazhatóak *VM1-NIC1* és alhálózati hello *Alhalozat_1*. Forgalom tooTCP 3389-es port használata engedélyezett hello hello hálózati interfészhez társított NSG *VM1-NIC1*, azonban a TCP Pingelje meg tooVM1 port 3389 sikertelen.

Ebben a példában a 3389-es portot használja, amíg hello lépéseket követve lehet használt toodetermine bejövő és kimenő kapcsolódási hibák bármely porton keresztül.

### <a name="vm"></a>A virtuális gép hatékony biztonsági szabályok megtekintése
Hajtsa végre a következő lépések tootroubleshoot NSG-ket a virtuális gépek hello:

Egy hálózati Adaptert a virtuális gépért hello hello hatékony biztonsági szabályok teljes listáját megtekintheti. Azt is megteheti, módosítására és a hálózati adapter és az alhálózati NSG-szabályok hello hatékony szabályokat panelen, törölje, ha rendelkezik engedélyekkel tooperform ezeket a műveleteket.

1. Bejelentkezés az Azure portálon, a https://portal.azure.com toohello.
2. Kattintson a **további szolgáltatások**, majd kattintson a **virtuális gépek** hello listában megjelenő.
3. Jelölje ki a virtuális gép tootroubleshoot hello listában megjelenő, és a beállítások egy virtuális gép panel jelenik meg.
4. Kattintson a **derítse & felmerülő problémák megoldásához** , és válassza a gyakori probléma. Ehhez a példához **toomy Windows virtuális gép nem lehet csatlakozni az** van kiválasztva. 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)
5. Az alábbi képen hello módon lépéseket hello probléma alatt jelenik meg: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)
   
    Kattintson a *hatékony biztonsági csoportszabályok* a lépések hello listáját.
6. Hello **lekérni a hatékony biztonsági szabályokat** panel jelenik meg, ahogy az alábbi képen hello:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)
   
    Figyelje meg a következő részekben hello kép hello:
   
   * **Hatókör:** túl beállítása*VM1*, a 3. lépésben kiválasztott VM hello.
   * **Hálózati adapter:** *VM1-NIC1* van kiválasztva. A virtuális gépek több hálózati adapterek (NIC) lehet. Egyes hálózati adapterek egyedi hatékony biztonsági szabályokat is rendelkezhetnek. Hibaelhárításához, szükség lehet tooview hello hatékony biztonsági szabályokat az egyes hálózati adapterhez.
   * **Társított NSG-ket:** NSG-ket lehet alkalmazott tooboth hello hálózati adapter és hello alhálózati hello hálózati adapter csatlakozik. A hello kép egy NSG új alkalmazott tooboth hello hálózati adapter és hello alhálózathoz csatlakozik. Kattintson a hello NSG neve a toodirectly hello NSG-ket a szabályok módosítása.
   * **VM1-nsg lap:** hello megjelenített szabályok listáján a hello van hello NSG alkalmazott toohello hálózati adaptert. Több alapértelmezett szabályok jönnek létre az Azure-ban, amikor létrejön egy NSG. Hello alapértelmezett szabályokat nem távolítható el, de a nagyobb prioritású szabályokkal felülbírálhatja. További információk az alapértelmezett szabályokat, olvassa el a hello toolearn [NSG áttekintése](virtual-networks-nsg.md#default-rules) cikk.
   * **Céloszlop:** egyes hello szabályok olyan szöveg hello oszlopban, míg mások a cím előtagokat. hello szöveg alapértelmezett alkalmazott címkék toohello biztonsági szabály neve hello esetén hozták létre. hello címkék olyan rendszer által biztosított azonosítók, amelyek megfelelnek a több előtagok. A szabály címkével ellátott, például kiválasztásával *AllowInternetOutBound*, hello előtag szerepel a hello **cím előtagokat** panelen.
   * **Letöltés:** szabályok listáján a hello hosszú lehet. Egy CSV-fájlt hello szabályok kapcsolat nélküli elemzéshez kattintva letöltheti **letöltése** és hello fájl mentése.
   * **AllowRDP** bejövő forgalomra vonatkozó szabály: Ez a szabály lehetővé teszi, hogy az RDP-kapcsolatok toohello virtuális gép.
7. Kattintson a hello **Alhalozat_1-NSG** lapon tooview hello hatékony szabályokat hello NSG alkalmazott toohello alhálózati, ahogy az alábbi képen hello: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)
   
    Értesítés hello *denyRDP* **bejövő** szabály. Bejövő szabályok hello alhálózati alkalmazása előtt hello hálózati kapcsolódási szabálya értékeli ki a rendszer. Hello megtagadási szabály alkalmazásakor: hello alhálózati, mivel hello kérelem tooconnect tooTCP 3389-es sikertelen lesz, mert hello szabály engedélyezi, a rendszer soha nem értékeli ki, a hálózati adapter hello. 
   
    Hello *denyRDP* szabály oka hello miért hello RDP-kapcsolat nem működik. Akkor kell feloldhatónak lennie hello probléma.
   
   > [!NOTE]
   > Ha hello VM tartozó hello a hálózati adapter nem fut, vagy NSG-ket a hálózati adapter vagy az alhálózat alkalmazott toohello nem, szabályait nem jelennek meg.
   > 
   > 
8. NSG-szabályok tooedit, kattintson a *Alhalozat_1-NSG* a hello **társított NSG-k** szakasz.
   Ekkor megnyílik a hello **Alhalozat_1-NSG** panelen. Közvetlenül szerkesztheti hello szabályok kattintva **bejövő biztonsági szabályok**.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)
9. Hello eltávolítása után *denyRDP* hello a bejövő forgalomra vonatkozó szabály **Alhalozat_1-NSG** és hozzáadása egy *allowRDP* szabály, a következőképpen néz hello hatékony szabályok listája a következő kép hello:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)
   
    Győződjön meg arról, hogy 3389-es TCP-port meg nyitva egy RDP-kapcsolat toohello VM megnyitásával, vagy hello PsPing eszköz használata. Terhelésekről további információt a PsPing által olvasása hello [PsPing letöltési oldala](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="nic"></a>Egy adott hálózati csatoló hatékony biztonsági szabályok megtekintése
Ha a virtuális gép forgalom áramlását kihatással van a megadott hálózati adapter, hello hatékony szabályokat hello NIC hello hálózati illesztők környezetből teljes listáját megtekintheti a hello lépések végrehajtásával:

1. Bejelentkezés az Azure portálon, a https://portal.azure.com toohello.
2. Kattintson a **további szolgáltatások**, majd kattintson a **hálózati illesztőt** hello listában megjelenő.
3. Válasszon hálózati interfészt. Az alábbi képen hello, egy hálózati adapter nevű *VM1-NIC1* van kiválasztva.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)
   
    Figyelje meg, hogy hello **hatókör** kijelölt toohello hálózati adapter van beállítva. További részletek toolearn hello további információkat is látható, olvassa el a hello 6. lépés **hibáinak elhárítása az NSG-k a virtuális gépek** című szakaszát.
   
   > [!NOTE]
   > Ha egy NSG-t egy adott hálózati csatoló törlődik, hello alhálózati NSG továbbra is lép az hello megadott hálózati adaptert. Ebben az esetben hello kimeneti volna csak jelenít meg szabályokat a hello alhálózatból NSG. Szabályok csak akkor jelenik meg, ha hálózati hello csatolt tooa virtuális gép.
   > 
   > 
4. Közvetlenül szerkesztheti szabályok egy hálózati adapter és alhálózat társított NSG-ket. toolearn, hogy olvassa a hello 8. lépés **tekintse meg a virtuális gép hatékony biztonsági szabályokat** című szakaszát.

## <a name="nsg"></a>A hálózati biztonsági csoport (NSG) hatékony biztonsági szabályok megtekintése
Ha módosítja az NSG-szabályok, érdemes lehet hello szabályok egy adott virtuális Gépet felvenni a tooreview hello hatását. Hello hatékony biztonsági szabályok teljes listáját megtekintheti az összes hálózati adapter egy adott NSG alkalmazott, hello anélkül, hogy a megadott NSG panel hello tooswitch környezetben. tootroubleshoot hatékony szabályokat egy NSG-t, a következő lépéseket teljes hello belül:

1. Bejelentkezés az Azure portálon, a https://portal.azure.com toohello.
2. Kattintson a **további szolgáltatások**, majd kattintson a **hálózati biztonsági csoportok** hello listában megjelenő.
3. Válasszon egy NSG. Az alábbi képen hello egy NSG nevű VM1-NSG-t választotta ki.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)
   
    Figyelje meg a következő részekben hello előző kép hello:
   
   * **Hatókör:** toohello kijelölt NSG beállítása.
   * **Virtuális gép:** amikor egy NSG alkalmazott tooa alhálózati, alkalmazott tooall hálózati illesztők csatolt tooall virtuális gépek csatlakoztatott toohello alhálózat. Ez a lista tartalmazza az NSG vonatkozik az összes virtuális gépet. Kiválaszthatja a virtuális gép hello listából.
     
     > [!NOTE]
     > Ha egy NSG alkalmazott tooonly egy üres alhálózathoz, virtuális gépek nem jelenik meg. Ha egy NSG alkalmazott tooa hálózati adapter, amely nincs társítva van egy virtuális Gépet, ezeket a hálózati adapterek is nem jelenik meg. 
     > 
     > 
   * **Hálózati adapter:** egy virtuális Gépet is több hálózati illesztőre van szükség. Kiválaszthatja, hogy egy adott hálózati csatoló csatolt toohello kijelölt virtuális gép.
   * **AssociatedNSGs:** bármikor, egy hálózati adapter rendelkezhet mentése tootwo hatékony NSG-ket, egy hálózati adapter toohello alkalmazva, és más toohello alhálózati hello. Bár a hello hatókör van kiválasztva VM1-NSG-t, az NSG-hatékony alhálózatot hello NIC-e, hello kimenet megjeleníti mindkét NSG-ket.
4. Közvetlenül szerkesztheti szabályok egy hálózati adapter vagy az alhálózat társított NSG-ket. toolearn, hogy olvassa a hello 8. lépés **tekintse meg a virtuális gép hatékony biztonsági szabályokat** című szakaszát.

További részletek toolearn hello további információkat is látható, olvassa el a hello 6. lépés **tekintse meg a virtuális gép hatékony biztonsági szabályokat** című szakaszát.

> [!NOTE]
> Abban az esetben, ha egy alhálózat és a hálózati adapter is csak egy alkalmazott NSG toothem rendelkező, egy NSG társított toomultiple hálózati adapter és több alhálózatra lehet.
> 
> 

## <a name="considerations"></a>Megfontolandó szempontok
Vegye figyelembe a következő pontok csatlakozási problémák elhárításakor hello:

* Alapértelmezett NSG-szabályok blokkolja a bejövő hello hozzáférést internet és a virtuális hálózat engedély csak a bejövő forgalom. Szabályok kell adható hozzá közvetlen módon tooallow bejövő Internet, a szükséges hozzáférést.
* Ha nincsenek, amely a virtuális gép hálózati kapcsolat toofail NSG biztonsági szabályok, hello a probléma oka a következő lehet:
  * Hello virtuális gép operációs rendszerben futó tűzfal szoftver
  * A virtuális készülékek vagy a helyszíni forgalmi útvonalak. Internetes forgalom átirányított tooon helyszíni kényszerített bújtatás keresztül lehet. Az RDP/SSH-kapcsolat a hello Internet tooyour VM nem feltétlenül ezt a beállítást, attól függően, hogy miként hello a helyszíni hálózati hardver kezeli a forgalmat. Olvasási hello [hibaelhárítási útvonalak](virtual-network-routes-troubleshoot-powershell.md) cikk toolearn hogyan toodiagnose útvonal is lehet akadályozó problémákat hello és bezárja a forgalom hello virtuális gép. 
* Vnetek, alapértelmezés szerint rendelkezik társviszonyban, ha a hello VIRTUAL_NETWORK címke automatikusan tooinclude előtagok bontsa a társviszonyban Vnetek esetében. Ezeket az előtagokat megtekintheti a hello **ExpandedAddressPrefix** listájában, tootroubleshoot bármely társviszony-létesítés kapcsolódási problémák kapcsolódó tooVNet. 
* Hatékony biztonsági szabályok csak jelennek-e hello VM hálózati adapter és, vagy alhálózat társított egy NSG. 
* Ha nincs hálózati hello társított NSG-ket, vagy alhálózat és rendelkezik a nyilvános IP-cím hozzárendelése a virtuális gép tooyour, minden port nyissa meg a bejövő és kimenő hozzáférést lesz. Ha hello VM egy nyilvános IP-címmel rendelkezik, alkalmazza az NSG-k toohello hálózati adapter vagy az alhálózat erősen ajánlott.

