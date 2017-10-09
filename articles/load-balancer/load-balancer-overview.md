---
title: "aaaAzure Load Balancer áttekintése |} Microsoft Docs"
description: "Azure Load Balancer funkciók, architektúrájának és megvalósítási áttekintése. Ismerje meg, hogyan működik a hello terheléselosztó és hello felhőben kihasználja azt."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 0f313dc0-f007-4cee-b2b9-f542357925a3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 2376a02f7cbbbed6a90f216419c0c3d30f594272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-load-balancer-overview"></a>Az Azure Load Balancer áttekintése

Az Azure Load Balancer tooyour alkalmazások magas rendelkezésre állás és a hálózati teljesítményt nyújt. A réteg 4 (TCP, UDP) terheléselosztó bejövő forgalmat egy elosztott terhelésű készlet definiált kifogástalan szolgáltatáspéldányok között ellátó.

Az Azure Load Balancer beállítható úgy, hogy:

* Gépek terhelést elosztani bejövő internetes forgalom toovirtual. Ez a konfiguráció az úgynevezett [internetre terheléselosztási](load-balancer-internet-overview.md).
* Betöltési oszthatja el a forgalmat egy virtuális hálózatban lévő virtuális gépek, virtuális gépek felhőszolgáltatások közötti vagy a helyszíni számítógépek és a létesítmények közötti virtuális hálózatban lévő virtuális gépek között. Ez a konfiguráció az úgynevezett [belső terheléselosztás](load-balancer-internal-overview.md).
* Külső forgalom tooa adott virtuális gép.

Hello felhőben található összes erőforrást kell egy nyilvános IP-cím toobe hello Internet elérhető. hello felhő-infrastruktúra az Azure-ban nem elérhető IP-címet használ az erőforrások. Azure nyilvános IP címek toocommunicate toohello Internet hálózati címfordítás (NAT) használ.

## <a name="azure-deployment-models"></a>Az Azure-telepítés modellek

Fontos fontos toounderstand hello különbségei hello Azure klasszikus és Resource Manager [üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md). Az Azure Load Balancer minden modellben másként van konfigurálva.

### <a name="azure-classic-deployment-model"></a>Az Azure klasszikus telepítési modell

Egy felhőalapú szolgáltatás határon belül telepített virtuális gépek csoportosított toouse egy terheléselosztó lehet. Ez a minta egy nyilvános IP-címet, és egy teljesen minősített tartománynevét, (FQDN) rendelt tooa felhőalapú szolgáltatás. hello terheléselosztó port fordítása, és egyenlegének hello hálózati forgalmi betöltése hello felhőszolgáltatás hello nyilvános IP-cím használatával.

Elosztott terhelésű forgalom végpontok határozzák meg. Port fordítási végpontok rendelkezik hello nyilvános által hozzárendelt port hello nyilvános IP-cím és helyi port hello hozzárendelt toohello szolgáltatást egy adott virtuális gép közötti kapcsolatot. Betöltési terheléselosztási végpontok hello virtuális gépek hello a felhőszolgáltatásban található egy-a-többhöz hello nyilvános IP-cím és hello hozzárendelt helyi portok toohello szolgáltatások rendelkezik.

![Az Azure Load Balancer hello klasszikus üzembe helyezési modellel](./media/load-balancer-overview/asm-lb.png)

1. ábra – hello klasszikus üzembe helyezési modellel Azure Load Balancer

hello tartomány címke hello nyilvános IP-címhez, amely a központi telepítési modell betöltése terheléselosztó által használt hello \<felhőszolgáltatás neve\>. cloudapp.net. hello alábbi ábrán látható hello Azure Load Balancer ebben a modellben.

### <a name="azure-resource-manager-deployment-model"></a>Az Azure Resource Manager telepítési modell

Hello Resource Manager üzembe helyezési modellben van nincs szükség toocreate egy felhőalapú szolgáltatás. hello terheléselosztó tooexplicitly irányíthatja a forgalmat több virtuális gép között jön létre.

A nyilvános IP-cím egy egyéni erőforrást, amely rendelkezik egy tartomány címke (DNS-név). hello nyilvános IP-cím társítva hello terheléselosztó erőforrást. Load balancer szabályok és bejövő NAT-szabályok használata hello nyilvános IP-cím szerint hello Internet végpont fordulnak elő a hálózati forgalmat az elosztott terhelésű hello erőforrások.

A magán- vagy nyilvános IP-cím hozzá van rendelve toohello hálózati illesztő csatolt erőforrás tooa virtuális gépet. Egy adott hálózati csatoló tooa terheléselosztójának háttér IP-címkészlet hozzáadása után hello terheléselosztó képes toosend terhelésű a hálózati adatforgalom egy hello terhelésű szabályok alapján jönnek létre.

hello alábbi ábrán látható hello Azure Load Balancer ebben a modellben:

![Az Azure terheléselosztó a erőforrás-kezelő](./media/load-balancer-overview/arm-lb.png)

2. ábra – az Azure terheléselosztó a erőforrás-kezelő

hello terheléselosztó sablonok Resource Manager-alapú API-k és eszközök is kezelhetőek. toolearn több kapcsolatos erőforrás-kezelő, lásd: hello [Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).

## <a name="load-balancer-features"></a>A terheléselosztó funkció betöltése

* Kivonat-alapú terjesztési

    Az Azure Load Balancer kivonat-alapú elosztási algoritmust használ. Alapértelmezés szerint 5 rekordos kivonatot forrás IP-cím, a forrásport, a cél IP-cím, a célport és a protokoll típus toomap forgalom tooavailable kiszolgálók álló használ. Csak tölcsérútvonalak biztosít *belül* egy átviteli munkamenet. Az azonos TCP vagy UDP-munkamenet hello csomagok azonos példány mögött hello elosztott terhelésű végpont toohello irányítja. Hello ügyfél bezárja és újra megnyitja a hello kapcsolat vagy az új munkamenet indítása hello azonos forrás IP-cím, az adatforrás-port módosításokkal hello. Emiatt a hello forgalom toogo tooa másik végpont egy ugyanabban az adatközpontban.

    További részletekért lásd: [terheléselosztó terheléselosztási mód](load-balancer-distribution-mode.md). hello alábbi ábrán látható hello kivonat-alapú terjesztési:

    ![Kivonat-alapú terjesztési](./media/load-balancer-overview/load-balancer-distribution.png)

    3. ábra - kivonat-alapú terjesztési

* Port átirányítása

    Az Azure Load Balancer állítható be, hogyan bejövő kommunikáció alatt áll. Ez a kommunikáció internetes gazdagépek, virtuális gépek más felhőalapú szolgáltatások, vagy a virtuális hálózatok kezdeményezett forgalmat foglalja magában. Ez a vezérlő végpont (más néven bemeneti végpontja) jelöl.

    Bemeneti végpontja egy nyilvános portot figyeli, és továbbítja a forgalmat tooan belső port. Képezi le azonos portok belső vagy külső végpont hello, vagy egy másik portot használ. Például lehet egy webes konfigurált kiszolgáló toolisten tooport 81-es hello nyilvános végpontot leképezési pedig 80-as porton. egy nyilvános végpontot hello létrehozását a terhelés terheléselosztó példány létrehozásához hello váltja ki.

    Azure-portálon létrehozott használatával hello, hello portál automatikusan létrehoz végpontok toohello virtuális gép hello Remote Desktop Protocol (RDP) és a Windows PowerShell távoli munkamenet-forgalmat. Ezeket a végpontokat is használhat tooremotely hello virtuális gép felügyeletét hello interneten keresztül.

* Automatikus újrakonfigurálása

    Az Azure Load Balancer azonnal újrakonfigurálása akkor példányok felfelé vagy lefelé. Például az újrakonfigurálás történik, amikor növeli a példányok száma a felhőszolgáltatásban a webes/munkavégző szerepkörök hello, vagy további virtuális gépek hello történő hozzáadásakor ugyanaz az elosztott terhelésű állítsa be.

* Szolgáltatásfigyelés

    Az Azure Load Balancer különböző kiszolgálópéldányok is mintavételi hello hello állapotát. Egy mintavételt toorespond sikertelensége esetén hello terheléselosztó leállítja az új kapcsolatok toohello sérült példányok küldése. A meglévő kapcsolatok nem érintettek.

    Három típusú mintavételt támogatja:

    + **Vendég ügynök mintavétele (platformon, a szolgáltatási célú virtuális gépek csak):** hello terheléselosztót használja hello vendégügynök hello virtuális gépen belül. hello vendégügynök figyeli és reagál HTTP 200 OK válasz csak akkor, ha hello példány hello kész állapotban van (azaz hello példány nem állapotban van egy foglalt, például újrahasznosítási, vagy leállítása). Egy HTTP 200 OK toorespond hello ügynök meghibásodásakor hello load balancer jelek hello példányához nem válaszol, és a forgalom toothat példány küldése leáll. hello terheléselosztó tooping hello példány továbbra is. Ha hello vendégügynök válaszol egy HTTP 200, hello terheléselosztó küldi a forgalom toothat példány újra. Ha a webes szerepkör használata esetén a webhely kódot általában a w3wp.exe, nem figyelt hello Azure-hálót vagy vendégügynök fut. Ez azt jelenti, hogy hibák w3wp.exe (pl. HTTP 500 válaszok) nem lesz jelentett toohello vendégügynök, hello terheléselosztó nem fogja tudni tootake kívül Elforgatás példányhoz.
    + **Egyéni HTTP-vizsgálatot:** Ez a Hálózatfigyelő felülbírálások hello alapértelmezett (vendégügynök) mintavétel. Használat toocreate hello szerepkörpéldányt saját egyéni logika toodetermine hello állapotát. hello terheléselosztó rendszeresen fog mintavételi a végpont (alapértelmezés szerint minden 15 másodperc). hello példány akkor tekinthető toobe az elforgatás, ha a 200-as HTTP vagy TCP Nyugtázási hello határidőn (alapértelmezett 31 másodperc) belül válaszol. Ez akkor hasznos, a saját logikai tooremove példányok hello terheléselosztójának Elforgatás megvalósításához. Például hello példány tooreturn nem 200 állapot konfigurálása, ha hello példány CPU 90 % feletti. A webes szerepkörök w3wp.exe használó, is kap automatikus figyelési a webhely, mivel a webhely kódban hibák visszaadása nem 200 állapot toohello vizsgálatok.
    + **Egyéni TCP-Hálózatfigyelővel:** Ez a Hálózatfigyelő sikeres TCP-munkamenet létrehozása definiált tooa mintavételi portot támaszkodik.

    További információkért lásd: hello [LoadBalancerProbe séma](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Forrás NAT

    A szolgáltatásból származó Internet megy keresztül minden kimenő forgalom toohello forrás NAT (SNAT) használatával hello ugyanazon virtuális IP-cím szerint hello bejövő forgalmat. SNAT fontos előnyökkel jár:

    + Egyszerű frissítés és katasztrófa-helyreállítás szolgáltatások, lehetővé teszi óta hello VIP lehet dinamikusan hozzárendelt hello szolgáltatás tooanother példánya.
    + Ez egyszerűbbé teszi a hozzáférés-vezérlési lista (ACL) felügyeletet. Virtuális IP-címek kifejezett hozzáférés-vezérlési listák nem módosítása szolgáltatások felskálázott, mint lefelé vagy újratelepítése beolvasása.

    hello terheléselosztó-konfiguráció teljes amerikai NAT támogatja az UDP-hez. Teljes amerikai NAT NAT, ahol hello port lehetővé teszi a bejövő kapcsolatok bármely külső állomásról (a válasz tooan kimenő kérelem) típusú.

    Egy kimenő porton új kimenő kapcsolatok a virtuális gépek kezdeményező, is lefoglalta hello terheléselosztóhoz. hello külső gazdagép látja a forgalom virtuális IP-Címek VIP-hozzárendelt portszámmal együtt. Kimenő kapcsolatok nagy számú igénylő forgatókönyvek esetén ajánlott toouse [példányszintű nyilvános IP-cím](../virtual-network/virtual-networks-instance-level-public-ip.md) szünteti meg, hogy hello virtuális gépek SNAT egy dedikált kimenő IP-címet beállítani. Ez csökkenti a port Erőforrásfogyás hello kockázatát.

    Ellenőrizze a [kimenő kapcsolatok](load-balancer-outbound-connections.md) cikket ebben a témakörben olvashat.

### <a name="support-for-multiple-load-balanced-ip-addresses-for-virtual-machines"></a>Több-az elosztott terhelésű IP-címet a virtuális gépek támogatása
Egynél több terhelésű nyilvános IP cím tooa virtuális gépek halmaza rendelhet hozzá. Ez a lehetőség a gazdagép több SSL-webhely és/vagy ugyanaz a virtuális gépek készletét hello figyelőinek több SQL Server AlwaysOn rendelkezésre állási csoport. További információkért lásd: [több virtuális IP-címek egy felhőalapú szolgáltatás](load-balancer-multivip.md).

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="limitations"></a>Korlátozások

Load Balancer háttérkészletek bármely virtuális gép SKU, kivéve az alapszintű csomag is tartalmazhat.

## <a name="next-steps"></a>Következő lépések

- További információ [internetre terheléselosztó](load-balancer-internet-overview.md)

- További információ [belső load balancer áttekintése](load-balancer-internal-overview.md)

- Hozzon létre egy [internetre terheléselosztó](load-balancer-get-started-internet-portal.md)

- Megismerhet néhány hello más kulcs [hálózati lehetőségeket](../networking/networking-overview.md) Azure

