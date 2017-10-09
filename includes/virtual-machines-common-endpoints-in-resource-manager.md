hello megközelítés tooAzure végpontok működik egy kicsit másképp hello klasszikus és Resource Manager üzembe helyezési modellek között. Hello Resource Manager üzembe helyezési modellel hello rugalmasságot toocreate hálózati szűrők forgalmat a virtuális gépek mindkét hello áramló szabályozó most már rendelkezik. Ezek a szűrők lehetővé teszik egy egyszerű végpont hasonlóan hello klasszikus telepítési modell túl hálózati környezetek összetett toocreate. Ez a cikk áttekintése hálózati biztonsági csoportok és hogyan Miben különböznek a klasszikus végpontok, ezek a szűrési szabályok létrehozása használatával és telepítési forgatókönyvek mintát.

## <a name="overview-of-resource-manager-deployments"></a>A Resource Manager üzembe helyezések áttekintése
Hello klasszikus üzembe helyezési modellel végpontok helyébe a hálózati biztonsági csoportok, és hozzáférés-vezérlési lista (ACL) szabályok. Gyorsműveletek megvalósításához a hálózati biztonsági csoport ACL-szabályok a következők:

* Hálózati biztonsági csoport létrehozása.
* tooallow vagy letilthatja a forgalmat, a hálózati biztonsági csoport ACL-szabályok meghatározása.
* Rendelje hozzá a hálózati biztonsági csoport tooa hálózati adapter vagy a virtuális hálózati alhálózat.

Ha vannak, aki a tooalso port-továbbítást, tooplace terheléselosztó kell előtt a virtuális Gépet, és a NAT-szabályokat használ. Gyorsműveletek a terheléselosztó és a NAT-szabályok a következőképpen nézne ki:

* Hozzon létre egy terheléselosztót.
* Hozzon létre egy háttér címkészletet, és adja hozzá a virtuális gépek toohello készlet.
* Adja meg a szükséges hello port továbbítási NAT-szabályokat.
* Rendelje hozzá a NAT-szabályok tooyour virtuális gépeket.

## <a name="network-security-group-overview"></a>Hálózati biztonsági csoport – áttekintés
Hálózati biztonsági csoportok adjon meg tooallow adott portok biztonsági réteget és alhálózatok tooaccess a virtuális gépek. Általában mindig van egy hálózati biztonsági csoportot ezen a virtuális gépek között a hello world kívül biztonsági réteget biztosít. Hálózati biztonsági csoportok alkalmazott tooa virtuális hálózati alhálózat vagy egy adott hálózati illesztőn a virtuális gépek lehetnek. Ahelyett, hogy a végpont ACL-szabályok létrehozása, hogy most létrehozza a hálózati biztonsági csoport ACL-szabályok. Ezek az ACL-szabályok sokkal hatékonyabb vezérlését a mint egyszerűen létrehozása egy végpont tooforward egy adott portot adja meg. További információkért lásd: [Mi az a hálózati biztonsági csoport?](../articles/virtual-network/virtual-networks-nsg.md)

> [!TIP]
> Hálózati biztonsági csoportok toomultiple alhálózatok vagy a hálózati adapterek is hozzárendelhető. Nincs 1:1 megfeleltetés. Hálózati biztonsági csoport létrehozása közös ACL-szabályok vannak beállítva, és toomultiple alhálózatok és a hálózati adapterek alkalmazni. További, a hálózati biztonsági csoport lehet alkalmazott tooresources között az előfizetés (alapján [szerepkör alapú hozzáférés-vezérlést](../articles/active-directory/role-based-access-control-what-is.md).

## <a name="load-balancers-overview"></a>Load Balancer Terheléselosztók áttekintése
Hello klasszikus üzembe helyezési modellel Azure Ehhez hajtsa végre az összes hello hálózati címfordítás (NAT) és port átirányítása egy felhőalapú szolgáltatás meg. A végpont létrehozásakor meg kellene adni hello külső port tooexpose hello belső port toodirect forgalmat együtt. Hálózati biztonsági csoportok önmagában ne hajtsa végre a azonos NAT és port továbbító. 

tooallow toocreate NAT-szabályok ilyen port továbbítást, létrehozhat egy Azure terheléselosztó az erőforráscsoportban. Ebben az esetben hello terheléselosztó részletes elegendő tooonly toospecific virtuális gépek alkalmazni, ha szükséges. hello Azure terheléselosztó NAT-szabályok munkahelyi együtt a hálózati biztonsági csoport ACL szabályok tooprovide betölteni, sokkal nagyobb rugalmasságot és vezérlés, mint a korábban elérhető használatával a felhőalapú szolgáltatás végpontok. További információkért lásd: hello [terheléselosztó áttekintés betöltése](../articles/load-balancer/load-balancer-overview.md).

## <a name="network-security-group-acl-rules"></a>Hálózati biztonsági csoport ACL-szabályok
ACL-szabályok lehetővé teszik, hogy a forgalmat a virtuális gép alapján adott portok, porttartományok vagy protokollok mindkét áramolhasson meghatározása. Szabályok tooindividual virtuális gépek vagy tooa alhálózathoz van hozzárendelve. a következő képernyőkép hello példája egy gyakori webkiszolgáló ACL-szabályok:

![A hálózati biztonsági csoportja ACL-szabályok](./media/virtual-machines-common-endpoints-in-resource-manager/example-acl-rules.png)

ACL-szabályok érvényesek prioritás metrika megadott - hello nagyobb hello érték, hello alacsonyabb hello prioritás alapján. Minden hálózati biztonsági csoport rendelkezik három alapértelmezett szabályokat készült Azure hálózati forgalmat, explicit toohandle hello áramló `DenyAllInbound` hello végső szabály. Alapértelmezett ACL-szabályok vannak egy kis prioritású virtuális gép toonot megadott zavarják a létrehozott szabályokat.

## <a name="assigning-network-security-groups"></a>Hálózati biztonsági csoportok hozzárendelése
Hozzárendelt hálózati biztonsági csoport tooa alhálózatot vagy egy adott hálózati csatoló. Ez a megközelítés lehetővé teszi toobe ennek részletes, ha alkalmazása az ACL szabályok tooonly egy adott virtuális gép szükség szerint, vagy közös biztosítása ACL-szabályok a következők alkalmazott tooall virtuális gépek része az alhálózatnak:

![Alkalmazza az NSG-k toonetwork felületek vagy alhálózatok](./media/virtual-machines-common-endpoints-in-resource-manager/apply-nsg-to-resources.png)

Hálózati biztonsági csoport hello hello viselkedése attól függően, hogy folyik a hozzárendelése tooa alhálózatot vagy egy adott hálózati csatoló nem változik. Központi telepítési forgatókönyve a hálózati biztonsági csoporthoz hozzárendelt tooa alhálózati tooensure megfelelőségi minden virtuális gépek csatolt toothat alhálózat hello rendelkezik. További információkért lásd: [alkalmazásakor a hálózati biztonsági csoportok tooresources](../articles/virtual-network/virtual-networks-nsg.md#associating-nsgs).

## <a name="default-behavior-of-network-security-groups"></a>Hálózati biztonsági csoportok alapértelmezett viselkedése
Attól függően, hogy hogyan és mikor hoz létre a hálózati biztonsági csoport az alapértelmezett szabályok eléréshez Windows virtuális gépek toopermit RDP a 3389-es TCP-port hozható létre. Linux virtuális gépek alapértelmezett szabályok lehetővé teszik az SSH-elérést a 22-es TCP-portot. Ezek automatikus ACL-szabályok alapján a következő feltételek hello készült:

* Ha hello portálon keresztül Windows virtuális gép létrehozása, és fogadja el a hello alapértelmezett művelet toocreate hálózati biztonsági csoport, akkor egy hozzáférés-vezérlési lista szabály tooallow TCP 3389-es port (RDP) jön létre.
* Ha hozzon létre egy Linux virtuális Gépet hello portálon keresztül, és fogadja el a hello alapértelmezett művelet toocreate hálózati biztonsági csoport, akkor egy hozzáférés-vezérlési lista szabály tooallow TCP-port a 22-(SSH) jön létre.

Minden egyéb körülmények ezek alapértelmezett ACL-szabályok nem jönnek létre. Virtuális gép tooyour hello megfelelő ACL-szabályok létrehozása nélkül nem lehet csatlakoztatni. Ilyen feltétel a következő gyakori műveletei hello:

* Mint egy külön művelet toocreating hello VM hello portálon keresztül hálózati biztonsági csoport létrehozása.
* Programozott módon keresztül PowerShell, az Azure parancssori felület, a Rest API-k, a stb hálózati biztonsági csoport létrehozása.
* Virtuális gép létrehozása és a meglévő hálózati biztonsági csoporthoz, amely még nincs hello megfelelő ACL-szabályához megadott tooan hozzárendelésével.

Minden hello esetekben megelőző kell toocreate ACL-szabályok a virtuális gép tooallow hello megfelelő távoli felügyeleti kapcsolatokhoz.

## <a name="default-behavior-of-a-vm-without-a-network-security-group"></a>Alapértelmezett viselkedés a virtuális gépek hálózati biztonsági csoport nélkül
Létrehozhat egy virtuális gép hálózati biztonsági csoport létrehozása nélkül. Ezekben a helyzetekben kapcsolódhatnak tooyour virtuális gép RDP és az SSH használatával ACL-szabályok létrehozása nélkül. Hasonlóképpen ha egy webszolgáltatás-bővítmény telepítése 80-as porton, a service érhető el automatikusan távolról. virtuális gép hello nyissa meg az összes port tartozik.

> [!NOTE]
> Szüksége van egy nyilvános IP cím tooa VM ahhoz, hogy a távoli kapcsolatokat toohave. Hello alhálózati vagy hálózati illesztő hálózati biztonsági csoport nem rendelkezik hello VM tooany külső forgalom nem fedi fel. hello alapértelmezett művelet hello portálon keresztül virtuális gép létrehozásakor toocreate egy új nyilvános IP-cím. Egyéb kulcstárolást például PowerShell, az Azure parancssori felület vagy a Resource Manager-sablon virtuális gép létrehozása egy nyilvános IP-cím nincs automatikusan létrehozva kivéve, ha explicit módon kért. hello alapértelmezett művelet hello portálon keresztül akkor is toocreate hálózati biztonsági csoport. Ez az alapértelmezett művelet, az azt jelenti, végül nem szabad a egy kitett virtuális Géphez, amelynek nincs hálózati helyen szűrés helyzetben.

## <a name="understanding-load-balancers-and-nat-rules"></a>Egy terheléselosztó és a NAT-szabályok ismertetése
Hello klasszikus üzembe helyezési modellel létrehozhat is sor port továbbítási végpontok. A virtuális gépek létrehozásakor hello klasszikus üzembe helyezési modellel ACL-szabályok az RDP- vagy SSH automatikusan létrejön lenne. Ezek a szabályok nem vezet 3389-es TCP-port vagy a 22-es TCP-portot rendre toohello globális kívül. Ehelyett a nagy értékű TCP-port volna elérhetővé tehető, amely leképezhető toohello megfelelő belső port. Hasonló módon is létrehozhat saját ACL-szabályok, például elérhetővé tétele egy webkiszolgáló, az TCP port 4280 toohello globális kívül. Ezeket a ACL-szabályok és a következő képernyőkép hello klasszikus portálon hello porthozzárendelései látható:

![Klasszikus végpontokon port-továbbítás](./media/virtual-machines-common-endpoints-in-resource-manager/classic-endpoints-port-forwarding.png)

A hálózati biztonsági csoportokkal a port-továbbítási funkció terheléselosztó kezeli. További információkért lásd: [terheléselosztók az Azure-ban](../articles/load-balancer/load-balancer-overview.md). hello alábbi példa bemutatja a terheléselosztó NAT szabály tooperform port-továbbítási TCP port 4222 toohello belső TCP porton 22 egy virtuális Gépet:

![Terheléselosztó NAT-szabályok port-továbbítási betöltése](./media/virtual-machines-common-endpoints-in-resource-manager/load-balancer-nat-rules.png)

> [!NOTE]
> Ha egy terhelés-kiegyenlítő, általában nem rendel hozzá virtuális gépért hello egy nyilvános IP-címet. Ehelyett a hello terheléselosztó a nyilvános IP-cím tooit rendelkezik. Szüksége van az toocreate a hálózati biztonsági csoport és az ACL szabályok toodefine hello áramló forgalom mindkét a virtuális Gépet. hello load balancer NAT-szabályok egyszerűen toodefine milyen portok hello terheléselosztóhoz, és hogyan azok beolvasása pontjain hello háttér virtuális gépek használata engedélyezett. Szükség van egy NAT-szabály toocreate forgalom tooflow hello terheléselosztó keresztül. tooallow hello forgalom tooreach hello virtuális gép, egy hálózati biztonsági csoport ACL-szabályának létrehozása.
