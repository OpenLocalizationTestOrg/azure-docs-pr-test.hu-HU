---
title: "Példa – aaaDMZ Build egy Szegélyhálózaton tooProtect hálózatokat és a tűzfalon, UDR és NSG |} Microsoft Docs"
description: "Build tűzfallal DMZ, felhasználó által definiált útválasztási (UDR) és a hálózati biztonsági csoportokkal (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc121f9cd5fe3c3e9ac2c70fbb7d982a80bb345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-3--build-a-dmz-tooprotect-networks-with-a-firewall-udr-and-nsg"></a>Build a DMZ tooProtect hálózatokat és a tűzfalon, UDR és NSG 3 – példa
[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]

Ebben a példában az tűzfal, négy windows Server kiszolgálókon, felhasználói definiált útválasztó, IP-továbbítást, és hálózati biztonsági csoportok hoz létre a Szegélyhálózaton. Azt is haladhat végig hello vonatkozó parancsok tooprovide bemutatják az egyes lépéseket. Szerepel továbbá egy forgalom forgatókönyv szakasz tooprovide egy részletesebb lépésenkénti hogyan forgalom halad keresztül hello szolgálhat a hello DMZ. Végezetül hello references szakaszában van hello teljes kód látható és utasítás toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet. 

![Kétirányú DMZ NVA, NSG és UDR][1]

## <a name="environment-setup"></a>Környezet beállítása
Ebben a példában nincs olyan előfizetést, amelyet hello következő tartalmazza:

* A felhőalapú szolgáltatások három: "SecSvc001", "FrontEnd001" és "BackEnd001"
* A virtuális hálózati "CorpNetwork", három alhálózatokon: "SecNet", "Előtér" és "Háttér"
* Ebben a példában a tűzfal, a hálózati virtuális készülék toohello SecNet alhálózati csatlakoztatva
* A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő
* Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás biztonsági end-kiszolgálók ("AppVM01", "AppVM02")
* A Windows server DNS-kiszolgáló ("DNS01") jelölő

Hello references szakaszában alatt van egy PowerShell-parancsfájlt, amely a fent leírt hello környezetben a legtöbb fog létrehozni. Épület hello virtuális gépek és virtuális hálózatok, bár hello a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen.

toobuild hello rendelkező környezetben:

1. Hello hálózati konfigurációs XML-fájl (nevét, helyét és IP címeket toomatch megadott hello forgatókönyv frissíti) hello references szakaszában szereplő mentése
2. Frissítés hello felhasználói változók hello parancsfájl toomatch hello környezet hello parancsfájlban toobe futtatni (előfizetések, service nevét stb.)
3. PowerShell hello parancsfájlok végrehajtását

**Megjegyzés:**: hello régió hello PowerShell-parancsfájl esetében meg kell egyeznie a hello hálózati konfigurációs XML-fájlban szereplő hello régióban.

Miután hello parancsprogram sikeresen lefut hello utáni parancsfájl lépések tehetők:

1. Állítson be hello tűzfalszabályokat, ezzel kapcsolatban lásd: hello című az alábbi: tűzfal szabály leírása.
2. Opcionálisan hello references szakaszában vannak két parancsfájlok tooset hello webkiszolgáló és egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése az alkalmazások kiszolgálói.

Miután hello parancsprogram sikeresen lefut hello tűzfalszabályokat kell toobe befejeződött, ez hello című kapcsolatban lásd: tűzfalszabályokat.

## <a name="user-defined-routing-udr"></a>Felhasználó által definiált útválasztási (UDR)
Alapértelmezés szerint a következő rendszerútvonalak hello is meg van adva:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

hello VNETLocal mindig definiálva hello cím előtag(ok) a VNet hello (azaz azt állapotúról attól függően, hogy hogyan definiálva van-e az egyes konkrét VNet hálózatok tooVNet) adott hálózat. hello fennmaradó rendszerútvonalak statikusak, és a fenti alapértelmezett.

Prioritás, mint útvonalak dolgoznak fel hello a leghosszabb előtag-megfeleltetés (LPM) módszerrel, így hello legjobban megfelelő útvonal hello tábla csak akkor vonatkozik célcím megadott tooa.

Ezért forgalom (például a toohello DNS01 server, 10.0.2.4) szánt hello helyi hálózati (10.0.0.0/16) keresztül hello VNet tooits cél miatt toohello 10.0.0.0/16 útvonal lesz átirányítva. Más szóval a 10.0.2.4, hello 10.0.0.0/16 útvonal út hello legjobban megfelel, annak ellenére, hogy hello 10.0.0.0/8 és 0.0.0.0/0 is beállíthat, de mivel kevesebb adott nincsenek hatással a forgalmat. Így hello forgalom too10.0.2.4 lenne a következő ugrás a virtuális helyi hálózat hello és egyszerűen útvonal-toohello cél.

Ha forgalom történő készült 10.1.1.1 például hello 10.0.0.0/16 útvonal nem érvényes, de hello 10.0.0.0/8 lenne hello legjobban megfelel, és hello forgalmat lenne eldobott ("fekete furatos"), mert hello következő ugrás értéke Null. 

Ha hello cél hello Null előtagok vagy hello VNETLocal előtagok tooany nem lett alkalmazva, akkor azt végrehajtania a legkevésbé egyedi hello továbbítani, 0.0.0.0/0 és átirányítás kimenő Internet hello következő ugrásként toohello, így Azure internet peremhálózati ki.

Ha két azonos előtagok szerepelnek hello útválasztási táblázatot, hello az alábbiakban olvashat hello útvonalak "forrás" attribútum alapján hello sorrendben:

1. "VirtualAppliance" egy felhasználó által definiált útvonaltábla manuálisan hozzáadott toohello =
2. "A(z)" (BGP hibrid hálózatok használata esetén), dinamikus útvonal = dinamikus hálózati protokoll által hozzáadott, ezeket az útvonalakat idővel megváltozhat, hello dinamikus protokoll automatikusan módosításoknak társviszonyban hálózat
3. "Alapértelmezett" = hello Rendszerútvonalak, helyi hálózatok és hello statikus bejegyzések hello hello útválasztási táblázatot a fenti ábrán.

> [!NOTE]
> Ezután már használhatja felhasználói definiált útválasztás (UDR) az ExpressRoute- és VPN-átjárók tooforce kimenő és bejövő létesítmények közötti forgalom toobe irányított tooa hálózati virtuális készülék (NVA).
> 
> 

#### <a name="creating-hello-local-routes"></a>Hello helyi útvonalak létrehozása
Ebben a példában két útvonaltáblát van szükség, egy minden hello előtér- és háttérszolgáltatások alhálózatok esetében. Minden tábla megfelelő a megadott alhálózati hello statikus útvonalakkal be van töltve. Ebben a példában a hello célból minden táblának három útvonalakat:

1. A következő ugrás a helyi alhálózat forgalmának definiált tooallow helyi alhálózati forgalom toobypass hello tűzfal
2. Virtuális hálózati forgalmat a következő ugrásaként definiálva, tűzfal, hello alapértelmezett szabály, amely lehetővé teszi a helyi VNet forgalmát tooroute közvetlenül a rendszer felülírja
3. Az összes többi forgalom (0/0) a következő ugrásaként megadva, a tűzfal hello

Ha létrehozta a hello útválasztási táblázataiba kötött tootheir alhálózatok. Hello előtér alhálózati útválasztási táblázat Miután létrehozott és a kötött toohello alhálózati kell kinéznie:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Ehhez a példához hello következő parancsok használt toobuild hello útvonaltábla, adjon hozzá egy felhasználó által megadott útvonalat, és majd kötési hello útvonal tábla tooa alhálózati (Megjegyzés; dollárjelet kezdődő alatti elemek (pl.: $BESubnet) hello parancsfájlból a felhasználó által definiált változók hello útmutató szakaszban a jelen dokumentum):

1. Első hello útválasztási alaptábla kell létrehozni. A kódrészletben láthatja a hello Backend alhálózathoz hello tábla hello létrehozását. Hello parancsfájlban egy megfelelő táblázatot is létrejön a hello Frontend alhálózathoz.
   
     Új AzureRouteTable-$BERouteTableName neve "
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. Hello útvonaltábla létrehozása után a megadott felhasználó által megadott útvonalakat is hozzáadhatók. Ezen parancsmaghoz minden forgalmat (0.0.0.0/0) hello virtuális készülék (egy változót, $VMIP [0], az a hello IP-cím hello virtuális készülék hello parancsfájl a korábbi létrehozásakor használt toopass) keresztül továbbítódik. Hello parancsfájl, a megfelelő létrejön egy szabály is hello előtér táblában.
   
     Get-AzureRouteTable $BERouteTableName |} `
   
         Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. hello fent útvonal bejegyzés felülírják hello alapértelmezett "0.0.0.0/0" útvonal, de továbbra is meglévő, amelyek lehetővé teszik a hello alapértelmezett 10.0.0.0/16 szabály forgalom belül hello VNet tooroute közvetlenül toohello a cél és a nem hálózati virtuális készülék toohello. Ez a viselkedés hello kövesse szabály hozzá kell adni toocorrect.
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. Ezen a ponton nincs a választott toobe végzett. A fenti két útvonalak hello minden forgalom átirányítja az toohello tűzfal értékelésére, még akkor is, forgalmat egy egyetlen alhálózaton belül. Ez kívánatos lehet, de tooallow forgalom belül egy alhálózat tooroute helyileg egy harmadik, olyan speciális szabályt is hozzáadhatók hello tűzfaltól használata nélkül. Ez az útvonal bármely cím destine hello helyi alhálózati is csak az állapotok közvetlenül útvonal létezik (NextHopType = VNETLocal).
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. Végezetül hello útvonaltábla létrehozása, és feltölti a felhasználó által megadott útvonalakat, hello tábla most kell a kötött tooa alhálózat. Hello parancsfájlban hello előtér útvonaltábla egyben kötött toohello Frontend alhálózathoz. Itt található hello kötés parancsfájl hello háttér-alhálózathoz.
   
     Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName "
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP-továbbítás
A kiegészítő funkció tooUDR, az IP-továbbítást. Ez egy beállítást a virtuális készülék, amely lehetővé teszi nem kifejezetten tooreceive forgalom címzett toohello készüléknek, majd továbbítja az adott forgalom tooits végső célja.

Tegyük fel AppVM01 forgalmát kiszolgálóvá egy kérelem toohello DNS01, ha UDR szeretné továbbítani a toohello tűzfal. Az IP-továbbítás engedélyezve van hello forgalom hello DNS01 cél (10.0.2.4) a rendszer elfogadta hello készülék (10.0.0.4), majd továbbítja a végső rendeltetési tooits (10.0.2.4). Anélkül, hogy az IP-továbbítás engedélyezve a tűzfal hello forgalom volna nem fogadja el hello készülék annak ellenére, hogy hello útvonaltábla legyen hello tűzfal hello következő ugrásként. 

> [!IMPORTANT]
> Kritikus tooremember tooenable IP-továbbítás felhasználó definiált útválasztási együtt.
> 
> 

Az IP-továbbítás beállítása egy parancs, és a virtuális gép létrehozás időpontjában végezhető. Hello folyamata a következő példa hello kódrészletet hello parancsfájl hello vége felé, és hello UDR parancsok csoportosítva:

1. Hívás, amely a virtuális készülékre hello Virtuálisgép-példány, hello ebben az esetben a tűzfal és az IP-továbbítás engedélyezése (Megjegyzés; valamely elemére dollárjelet piros kezdődő (pl.: $VMName[0]) egy felhasználó által definiált változó hello parancsfájlból hello hivatkozás szakaszban ebben a dokumentumban. hello nulla szögletes [0], jelöli hello hello tömb hello példa parancsfájl toowork módosítás nélkül a virtuális gépek, az első virtuális gép, első virtuális gép (VM 0) kell lennie hello hello tűzfal):
   
     Get-AzureVM-Name $VMName [0] - szolgáltatásnév $ServiceName [0] |} `
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Hálózati biztonsági csoportok (NSG)
Ebben a példában egy NSG-csoport összeállítása és kezelhető egyetlen szabállyal majd betölteni. Ez a csoport van, majd kötött csak toohello előtér- és háttérszolgáltatások alhálózatok (nem a SecNet hello). Deklaratív módon a következő szabály hello éppen készül:

1. Teljes virtuális hálózatot (az összes alhálózatot) megtagadva hello Internet toohello minden forgalmat (minden porthoz)

Bár ebben a példában NSG-ket használ, annak fő célja azoknak a manuális helytelen konfiguráció másodlagos rétegként. Azt szeretnénk, tooblock hello internet tooeither hello előtér minden bejövő forgalom vagy háttér alhálózatok felderítéséhez, a forgalom csak áramlása hello SecNet alhálózati toohello tűzfal (és toohello előtér- vagy háttéradatbázis alhálózatok majd ha szükséges). Plus hello UDR szabályokkal helyen, minden forgalom, amelyek adott hello az előtér- vagy háttéradatbázis alhálózatok volna átirányítja kimenő toohello tűzfal (Köszönjük tooUDR). hello tűzfal jelenik meg ez az aszimmetrikus folyamatként és hello kimenő forgalom volna eldobni. Így nincsenek biztonsági védelmet nyújtó hello előtér három réteg és a háttérkiszolgáló alhálózatok; 1.) a forgalom nincs nyitott végpontok hello FrontEnd001 és BackEnd001 felhőszolgáltatások, 2) az NSG-k megtagadása hello Internet, 3) hello tűzfal aszimmetrikus forgalom eldobását.

Ebben a példában a hálózati biztonsági csoport hello vonatkozó egy érdekes pont, hogy az tartalmazza csak egy szabály, az alábbi ábrán látható, amely toodeny internetes forgalom toohello teljes virtuális hálózat, amely magában foglalja a hello biztonság – alhálózati. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet `
        from hello Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Azonban mivel hello NSG csak van kötve, toohello előtér- és háttér alhálózatok, hello szabály nem feldolgozott forgalom toohello biztonság – alhálózati bejövő. Ennek eredményeképpen annak ellenére, hogy hello NSG-szabály nem internetes forgalom tooany címet szereplő ellenőrzőösszeg hello virtuális hálózatot, mert hello NSG soha nem volt kötve toohello biztonság – alhálózati a forgalom halad toohello biztonság – alhálózati.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Tűzfalszabályok
Az hello tűzfal szabályok továbbítás kell toobe létrehozni. Mivel a hello tűzfal blokkolja vagy az összes bejövő, kimenő továbbítási és intra-VNet forgalom sok tűzfalszabályok van szükség. Emellett minden bejövő forgalom elért hello biztonsági szolgáltatás nyilvános IP-cím (különböző portok), toobe feldolgozva hello tűzfal. Ajánlott toodiagram hello logikai adatfolyamok hello alhálózatok és a tűzfalon szabályok tooavoid átdolgozás később beállítása előtt. hello alábbi ábrán is látható az ebben a példában hello tűzfalszabályok logikai nézetében:

![Tűzfalszabályok hello logikai ábrázolása][2]

> [!NOTE]
> A virtuális hálózati berendezések használt hello alapján, hello kezelőportjai függ. Ebben a példában a Barracuda NextGen tűzfal hivatkozott használó 22, 801 és 807 portok. További részleteket hello készülék gyártói dokumentációjában toofind hello pontos használt portok használt hello eszköz kezelését.
> 
> 

### <a name="logical-rule-description"></a>Logikai szabály leírása
Hello logikai diagramon fenti hello biztonság – alhálózati nem látható, mivel hello tűzfal hello csak erőforrás adott alhálózaton, és ezen a diagramon látható hello tűzfal-szabályok és hogyan azok logikailag engedélyezi vagy megtagadja a forgalmat, és nem hello tényleges irányított elérési. Is, hello külső portok kiválasztott hello RDP-forgalmát magasabb címkiosztási portok (8014 – 8026), és volt kijelölt toosomewhat igazodni hello utolsó két oktetben hello helyi IP-cím könnyebb olvashatóság érdekében (pl. helyi kiszolgáló címe 10.0.1.4 társítva van külső port 8014), azonban bármely magasabb nem ütköző portok is használható.

Az ebben a példában 7 típusú szabályokat kell, ezek a szabályok típusai a következők ismertetett:

* Külső szabályainak (bejövő forgalom):
  1. Felügyeleti tűzfalszabály: Az alkalmazás átirányítási szabály lehetővé teszi a forgalom toopass toohello kezelőportjai hello hálózati virtuális készülék.
  2. RDP-szabályok (az egyes windows-kiszolgálók): A négy szabályok (kiszolgálónként egyet) lehetővé teszi hello kezelését az egyes kiszolgálók RDP-kapcsolaton keresztül. Ezt követően sikerült kell is kötegelt, attól függően, hogy hello hálózati használt virtuális készülék képességeinek hello egy szabályt a.
  3. Alkalmazás-forgalomra vonatkozó szabályok: Nincsenek két alkalmazás forgalomra vonatkozó szabályok, először a hello előtérbeli webes forgalomban hello és hello második adatforgalomhoz hello háttér (pl. web server toodata réteg). Ezek a szabályok hello konfigurálása hello hálózati architektúra (ahol a kiszolgálók kerülnek) és a forgalom (melyik irányba hello forgalmat, és mely portokat kell használni) függ.
     * hello első szabály lehetővé teszi a hello tényleges alkalmazás forgalom tooreach hello alkalmazáskiszolgáló. Hello más szabályok engedélyezik a biztonsági, felügyeleti stb, amíg az alkalmazás szabályok lettek, mi engedélyezése külső felhasználók vagy szolgáltatások tooaccess hello alkalmazás(ok). Ebben a példában egyetlen webkiszolgálón való üzemeltetésekor 80-as porton van, így egyetlen alkalmazás tűzfalszabály átirányítja a bejövő forgalom toohello külső IP-toohello webes kiszolgálók belső IP-címet. hello átirányítva forgalom munkamenet NAT szeretné venni toohello belső kiszolgálóhiba.
     * második alkalmazás forgalmi szabály hello hello háttér szabály tooallow hello webkiszolgáló tootalk toohello AppVM01 kiszolgáló (de nem AppVM02) bármely porton keresztül.
* Belső szabályainak (intra-VNet-forgalom)
  1. Kimenő tooInternet szabály: Ez a szabály lehetővé teszi a hálózati forgalom toopass kijelölt toohello hálózatok. Ez a szabály az általában egy alapértelmezett szabály már a hello tűzfalat, de a letiltott állapotú. Ez a szabály ehhez a példához engedélyezni kell.
  2. DNS-szabály: Ez a szabály lehetővé teszi, hogy csak a DNS (53-as port) forgalmat toopass toohello DNS-kiszolgáló. Az ebben a környezetben a legtöbb forgalmat hello előtér toohello háttér le van tiltva ez a szabály kifejezetten engedélyezi DNS a helyi alhálózaton.
  3. Alhálózati tooSubnet szabály: Ez a szabály tooallow bármely kiszolgáló hello vissza a záró alhálózat tooconnect tooany server hello előtér alhálózaton (de nem a fordított hello) szerepel.
* Hibamentes (forgalomra vonatkozó szabály, amely nem felel meg a fenti hello bármely):
  1. Minden forgalmi szabály megtagadása: Ez mindig legyen hello végső szabály (tekintetében prioritás), és használja, így ha a forgalom sikertelen lesz toomatch bármelyik hello megelőző szabályokat a rendszer eldobja a szabály által. Ez az alapértelmezett szabály, és általában aktív, módosítás nélkül általában szükség van.

> [!TIP]
> Hello második alkalmazás forgalmi szabályt minden port engedélyezett könnyen ebben a példában, egy valós forgatókönyv hello legjobban megfelelő portok és címtartományok használt tooreduce hello támadási felületét Ez a szabály kell lennie.
> 
> 

<br />

> [!IMPORTANT]
> Miután az összes fenti szabályok hello jönnek létre, fontos, minden egyes szabály tooensure forgalom tooreview hello rangsorának engedélyezett vagy a kívánt módon megtagadva. Ehhez a példához hello szabályok prioritási sorrendben szerepelnek. Egyszerű toobe hello tűzfal toomis rendelt szabályok miatt tévedéssel is. Minimális győződjön meg arról hello felügyeleti hello tűzfal maga mindig hello abszolút legmagasabb prioritású szabály.
> 
> 

### <a name="rule-prerequisites"></a>A szabály Előfeltételek
Valamelyik előfeltétel hello virtuális gép futó hello tűzfal olyan nyilvános végpontok. Hello tűzfal tooprocess forgalom hello megfelelő nyilvános végpontok nyitva kell lennie. Három típusú forgalom ebben a példában; 1.) felügyeleti forgalom toocontrol hello tűzfal és tűzfalszabályok, 2) RDP forgalom toocontrol hello windows-kiszolgálók és a 3.) alkalmazás-forgalmat. Ezek a hello három oszlopot a felső hello a forgalomtípusok fele hello tűzfalszabályok fent logikai nézetében.

> [!IMPORTANT]
> A kulcs takeway tooremember, amely **minden** forgalmat fog érkezni hello tűzfalon keresztül. Igen tooremote asztali toohello IIS01 kiszolgáló, akkor is, ha hello első End felhőalapú szolgáltatás, valamint hello előtér alhálózati tooaccess ehhez a kiszolgálóhoz a tooRDP toohello tűzfal fel kell port 8014, és majd belső az hello tűzfal tooroute hello RDP-kérés engedélyezése toohello IIS01 RDP-portjára. hello Azure portal "Csatlakozás" gomb nem fog működni, mert nincs közvetlen RDP-elérési út tooIIS01 (szerint hello portal látható). Ez azt jelenti, hogy az összes kapcsolat hello internet lesz toohello biztonsági szolgáltatás és a Port, pl. secscv001.cloudapp.net:xxxx (hello hivatkozás a fenti diagram hello megfeleltetése külső Port és a belső IP-cím és a Port).
> 
> 

A végpont használatával megnyitható vagy a Virtuálisgép-létrehozási hello időpontjában vagy post build hello a példaként megadott parancsfájlt végezheti el, és a következő kódrészletet az alább látható módon (Megjegyzés; bármely dollárjelet elem kezdődő (pl.: $VMName[$i]) hello parancsfájlból hello referen a felhasználó által definiált változó a dokumentum CE szakaszát. hello "$i" szögletes [$i] jelöli hello tömb számát egy adott virtuális gép virtuális gépek tömbben):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Bár nem jelenik meg világosan itt miatt vannak-e a változókat, de a végpontok toohello használatát **csak** megnyitnia azon hello biztonsági felhőalapú szolgáltatás. Ez az, hogy minden bejövő forgalom kezelése tooensure (továbbítani, NAT, csökkent) hello tűzfal.

Egy olyan kezelőügyfelet toobe PC toomanage hello tűzfal telepítve kell, és a szükséges hello-konfigurációk létrehozása. Hogyan toomanage hello eszköz hello dokumentáció a tűzfal (vagy más NVA) szállító talál. Ez a szakasz és hello következő szakaszt, tűzfal-szabályok létrehozását, hello maradéka hello konfigurációs hello tűzfal, hello szállító felügyeleti ügyfél (azaz nem hello Azure-portál vagy PowerShell) segítségével ismerteti.

A letölthető ügyfél és a kapcsolódó toohello ebben a példában használt Barracuda utasítások itt találhatók: [Barracuda NG rendszergazda](https://techlib.barracuda.com/NG61/NGAdmin)

Miután bejelentkezett hello tűzfal alakzatot, de a tűzfalszabályok létrehozása előtt, vannak-e két előfeltétel objektumosztályok megkönnyítheti létrehozása hello szabályok; Hálózati & szolgáltatás objektumok.

Ebben a példában három elnevezett hálózati objektumok meghatározott (egy hello Frontend alhálózathoz és hello Backend alhálózathoz is egy hálózati objektum hello DNS-kiszolgáló IP-címhez hello) kell elhelyezni. toocreate elnevezett hálózati; hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő, toohello konfiguráció lapon keresse meg, a hello működési konfigurációs szakaszban kattintson a Ruleset, majd kattintson "Hálózat" hello Tűzfalobjektumokat menüben, majd új hello hálózatok Szerkesztés menü. hello hálózati objektum most hozhatók létre, hello nevét és hello előtag hozzáadásával:

![Hozzon létre egy előtér-hálózati objektumot][3]

Ezzel létrehoz egy nevesített hálózati hello FrontEnd alhálózathoz, egy hasonló objektum hello BackEnd alhálózathoz is létre kell hozni. Most hello alhálózatok könnyebben hivatkozható hello tűzfalszabályok név szerint.

A DNS-kiszolgáló objektum hello:

![Hozzon létre egy DNS-kiszolgáló objektum][4]

Az egyetlen IP-címre való hivatkozás a DNS-szabályban hello dokumentum későbbi szakaszában fog történni.

hello második előfeltétel objektumok olyan szolgáltatások objektumok. Ezek képviselik hello RDP csatlakozási porttal kiszolgálónként. Mivel hello meglévő RDP szolgáltatás objektumhoz kötött toohello alapértelmezett RDP-portjára, 3389-es, az új szolgáltatásokat is létrehozható tooallow forgalom hello külső portot (8014-8026). hello új portok is vehető toohello meglévő RDP szolgáltatást, de a bemutató megkönnyítése érdekében, az egyes szabályok kiszolgálónként is létrehozható. kiszolgáló; új RDP szabály toocreate hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő toohello konfiguráció lapon lépjen, a hello működési konfigurációs szakasz szabálykészletben, kattintson, majd kattintson a "Szolgáltatások" hello Tűzfalobjektumokat menü, görgessen lefelé hello szolgáltatás listája alatt hello kiválasztása "RDP" szolgáltatás. Kattintson a jobb gombbal és válassza a másolás, majd kattintson a jobb gombbal, és válassza ki a beillesztés. Már létezik egy RDP-Copy1 szolgáltatás objektum, amely szerkeszthető. Kattintson a jobb gombbal az RDP-Copy1, és válassza a Szerkesztés, hello szerkesztése szolgáltatás objektum ablak jelenik meg, ahogy az itt látható:

![Alapértelmezett RDP-szabály másolása][5]

hello értékek majd lehet szerkesztett toorepresent hello RDP-szolgáltatás egy adott kiszolgáló számára. AppVM01 hello alapértelmezett feletti RDP szabály legyen módosított tooreflect egy új szolgáltatás nevét, leírását, és a 8. ábra diagram hello használt külső RDP-portjára (Megjegyzés: hello portok vannak változása hello RDP alapértelmezett 3389 toohello külső port ezt használja: adott kiszolgáló hello esetben AppVM01 hello külső port 8025) hello módosított szolgáltatás alább találja:

![AppVM01 szabály][6]

Ez a folyamat ismételt toocreate RDP NFS-kiszolgálók; fennmaradó hello kell lennie. AppVM02 DNS01 és IIS01. Ezek a szolgáltatások hello létrehozását teszi hello szabályok létrehozása egyszerűbb és viszonylag kézenfekvő hello a következő szakaszban.

> [!NOTE]
> Hello tűzfal RDP szolgáltatásnak nincs szükség két okból; 1.) első hello tűzfal virtuális gép egy Linux-alapú lemezkép, ezért SSH port 22-es VM Management helyett RDP és a 2.) 22-es portot használják, és más felügyeleti két portok engedélyezettek hello tooallow kezelési kapcsolatot az alább ismertetett első felügyeleti szabály.
> 
> 

### <a name="firewall-rules-creation"></a>Tűzfal-szabályok létrehozása
Ebben a példában használt tűzfal-szabályok három különböző, azok összes rendelkeznek, különböző ikonok:

hello alkalmazás átirányítási szabály: ![alkalmazás átirányítási ikonja][7]

hello cél NAT-szabály: ![cél NAT ikon][8]

hello hozzáférési szabály: ![átadni ikon][9]

Ezek a szabályok bővebben hello Barracuda webhelyen található.

toocreate szabályainak hello (vagy ellenőrizze a meglévő alapértelmezett szabályok), hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő, keresse meg a toohello konfiguráció lapon, a hello működési konfigurációs szakaszban kattintson a Ruleset. A rács nevű, "Main szabályok" jeleníti meg a tűzfalon az aktív és inaktív szabályok meglévő hello. A hello jobb felső sarkában a rácson a kisméretű, zöld "+" gombra, kattintson az új szabály toocreate (Megjegyzés: a tűzfal "zárolva" a változásokat, ha a gomb "Lock" jelölésű, és nem toocreate, vagy a szabályok szerkesztése, kattintson erre a gombra túl szabály se "feloldásához" hello t és szerkeszthető). Ha tooedit meglévő szabály, válassza ki, hogy a szabály, kattintson a jobb gombbal, és válassza ki a szabály szerkesztése.

Ha a szabályok létrehozása és/vagy módosítva, fel kell toohello tűzfal leküldött és majd aktiválni, ha ez nem történik meg a módosítások életbe hello szabály. hello leküldéses és az aktiválási folyamat alábbiakban hello részletek szabály leírását.

minden egyes szabály hello mintaadatokról szükséges toocomplete ebben a példában az alábbiak szerint:

* **Tűzfal-kezelési szabály**: az alkalmazás átirányítási szabály forgalmát toopass toohello kezelőportjai hello hálózati virtuális készülék, ebben a példában a Barracuda NextGen tűzfal engedélyezi. hello kezelőportjai meg 801, a 807 és opcionálisan 22. hello külső és belső portok vannak hello (azaz nem port fordítási) azonos. A szabály, a telepítő-MGMT-hozzáférés alapértelmezett szabály, amely alapértelmezés szerint engedélyezett (a Barracuda NextGen tűzfal 6.1-es verzió).
  
    ![Felügyeleti tűzfalszabály][10]

> [!TIP]
> hello Ez a szabály forrás-címtér ilyen, ha hello ismert felügyeleti IP-címtartományok, csökkenti a hatókör volna is csökkentse hello támadási felület toohello felügyeleti portokat.
> 
> 

* **RDP-szabályok**: ezek cél NAT-szabályok lehetővé teszi hello kezelését az egyes kiszolgálók RDP-kapcsolaton keresztül.
  Nincsenek négy szükséges kritikus mezők toocreate Ez a szabály:
  
  1. Forrás – tooallow RDP helyről való munkavégzéséből és hello hivatkozás "A" hello mezőjének használatban van.
  2. Szolgáltatás – használja a megfelelő objektumot korábban létrehozott ebben az esetben "AppVM01 RDP" hello, hello külső portok átirányítási toohello kiszolgálók helyi IP-cím és tooport 3386 (hello alapértelmezett RDP-portjára). Ez a szabály az RDP-hozzáférést tooAppVM01 van.
  3. Cél-kell hello *hello tűzfalon helyi port*, "DCHP 1 helyi IP-címe" vagy eth0 statikus IP-címek használata. hello sorszámmal (eth0, eth1 stb.) Ha a hálózati berendezések több helyi kapcsolatok lehetnek. Hello portja hello tűzfal küldi ki az (lehet hello ugyanaz, mint fogadó port hello), hello tényleges irányított célobjektuma hello Céllista mezőben.
  4. Átirányítás – Ez a szakasz azt ismerteti hello virtuális készülék ahol tooultimately a forgalom átirányítására. hello legegyszerűbb átirányítási tooplace hello IP-cím és Port (nem kötelező) hello Céllista mezőben. Ha nincs port használt hello célport hello bejövő kérés lesz (azaz nem fordítási), használható, ha a port van kijelölve hello port lesz NAT volna együtt hello IP cím.
     
     ![RDP tűzfalszabály][11]
     
     Összesen négy RDP-szabályok létrehozása toobe lesz szüksége: 
     
     | Szabály neve | Kiszolgáló | Szolgáltatás | Cél lista |
     | --- | --- | --- | --- |
     | RDP-IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
     | RDP-DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
     | RDP-AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
     | RDP-AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

> [!TIP]
> Hello körét hello forrás és a szolgáltatás mezőket szűkíteni csökkenti hello támadási felületét. a korlátozott hello hatókör, amely lehetővé teszi a funkció kell használni.
> 
> 

* **Alkalmazás forgalomra vonatkozó szabályok**: nincsenek a két alkalmazás forgalomra vonatkozó szabályok, először a hello előtérbeli webes forgalomban hello és hello második adatforgalomhoz hello háttér (pl. web server toodata réteg). Ezek a szabályok hello hálózati architektúra (ahol a kiszolgálók kerülnek) és a forgalom (melyik irányba hello forgalmat, és mely portokat kell használni) függ.
  
    Először ismertetjük, hogy webes forgalomban hello előtér szabály:
  
    ![Webes tűzfalszabály][12]
  
    A cél NAT-szabály engedélyezi, hogy hello tényleges alkalmazás forgalom tooreach hello alkalmazáskiszolgáló. Hello más szabályok engedélyezik a biztonsági, felügyeleti stb, amíg az alkalmazás szabályok lettek, mi engedélyezése külső felhasználók vagy szolgáltatások tooaccess hello alkalmazás(ok). Ebben a példában egyetlen webkiszolgálón való üzemeltetésekor 80-as porton van, így hello egyetlen alkalmazás tűzfalszabály átirányítja a bejövő forgalom toohello külső IP-toohello webes kiszolgálók belső IP-címet.
  
    **Megjegyzés:**:, hogy nincs hozzárendelve hello Céllista mezőben port, így hello bejövő port 80-as (vagy 443-as hello szolgáltatás kiválasztva) használja a hello átirányítási hello webkiszolgáló. Hello webalkalmazás-kiszolgáló egy másik portot figyeli, ha például a 8080-as portra, hello cél lista mező frissített too10.0.1.4:8080 tooallow hello Port átirányítási is lehet.
  
    Tovább alkalmazás forgalmi szabály hello hello háttér szabály tooallow hello webkiszolgáló tootalk toohello AppVM01 kiszolgáló (de nem AppVM02) bármely szolgáltatáson keresztül:
  
    ![Tűzfalszabály AppVM01][13]
  
    A hozzáférési szabály lehetővé teszi, hogy a bármely IIS-kiszolgálót a hello Frontend alhálózathoz tooreach hello AppVM01 (IP-cím 10.0.2.5) bármely porton hello webes alkalmazás által igényelt protokoll tooaccess adatokat használ.
  
    Ezen a képernyőn megtekinthet egy "\<explicit cél\>" hello cél mező toosignify 10.0.2.5 hello célként használatos. Ennek oka lehet, vagy explicit látható módon, vagy nevű hálózati objektum (ahogy úgy történt, a DNS-kiszolgáló hello hello előfeltételei). Ez működik-e hello tűzfal toohello rendszergazdája, toowhich metódus lesz használva. tooadd 10.0.2.5 egy Explict Desitnation, kattintson duplán a hello első üres sor alatt \<explicit cél\> és hello ablakban hello címet adjon meg.
  
    A átadni szabálynak nem NAT van szükség, mert ez belső forgalmat, így hello kapcsolódási módszert túl állítható be "No SNAT".
  
    **Megjegyzés:**: hello Forráshálózat ebben a szabályban hello FrontEnd alhálózaton bármilyen olyan erőforrás értéke, ha csak lesz egy, és a webkiszolgálók, a hálózati objektum erőforrás lehet ismert adott számú létrehozott toobe pontosabb toothose pontos IP-címek helyett hello teljes Frontend alhálózathoz.

> [!TIP]
> Ez a szabály "Bármely" toomake hello minta alkalmazás könnyebb toosetup, és használjon hello szolgáltatást használ, ez lehetővé teszi a is ICMPv4 (ping) lévő egyetlen szabályt. Ez azonban nem ajánlott. hello portok és protokollok ("szolgáltatások") szűkített toohello minimális előfordulhat, hogy lehetővé teszi, hogy az alkalmazás műveletet tooreduce hello támadási felületét ehhez a határhoz között kell lennie.
> 
> 

<br />

> [!TIP]
> Bár ez a szabály azt mutatja, hogy egy explicit cél hivatkozást használja, a konzisztens megközelítés hello tűzfal konfigurálása során minden kell használni. Javasoljuk, hogy hálózati objektum nevű hello használni a teljes könnyebb olvashatóság és támogatás. hello explicit-cél használt Itt csak tooshow egy alternatív módszert, és általában nem ajánlott (különösen a összetett konfigurációk).
> 
> 

* **Kimenő tooInternet szabály**: ehhez adja át a szabály lehetővé teszi a forrás hálózati toopass toohello kiválasztott cél hálózati forgalmat. Ez a szabály egy alapértelmezett szabály már általában hello Barracuda NextGen tűzfalon, de a letiltott állapotú. Kattintson a jobb gombbal a szabály a hello aktiválása szabály parancs férhetnek hozzá. Itt látható hello szabály lett módosított tooadd hello két helyi alhálózatok utalás, ez a szabály a dokumentum toohello forrásattribútumának hello Előfeltételek szakaszában létrehozott.
  
    ![Kimenő tűzfalszabály][14]
* **DNS-szabály**: ehhez adja át a szabály lehetővé teszi, hogy csak DNS (53-as port) forgalmat toopass toohello DNS-kiszolgáló. Ebben a környezetben a legtöbb forgalmat hello előtér toohello háttér le van tiltva ez a szabály kifejezetten lehetővé teszi a DNS.
  
    ![DNS tűzfalszabály][15]
  
    **Megjegyzés:**: Ez a képernyő hibaüzenetet hello kapcsolódási módszert szerepel. Mivel ez a szabály belső IP toointernal IP-cím forgalom, nem NATing szükség, a hello csatlakozási mód beállítása túl "No SNAT" a hozzáférési szabályhoz.
* **Alhálózati tooSubnet szabály**: szabály ez adjon át egy alapértelmezett szabály, amely aktiválva lett, és zárulnak alhálózati tooconnect tooany kiszolgáló bármely kiszolgáló hello vissza a módosított tooallow hello előtérben lévő alhálózat alhálózat. Ez a szabály az összes belső forgalom, így a csatlakozási módszerhez hello tooNo SNAT állítható be.
  
    ![Tűzfal Intra-VNet szabály][16]
  
    **Megjegyzés:**: hello kétirányú jelölőnégyzet nincs bejelölve (se nem be van jelölve, a legtöbb szabályok), ez a szabály jelentős abban, hogy ez a szabály az "egy irányított" teszi, a kapcsolatot a számítógép hello háttér alhálózati toohello előtérhálózat, a azonban nem hello névkeresési. Ha a jelölőnégyzet be lett jelölve, ez a szabály lehetővé tenné kétirányú forgalmat, amely az a logikai diagram nem kívánatos.
* **Minden forgalmi szabály megtagadása**: hello végső szabály (tekintetében prioritás) mindig legyen, és használja, így ha a forgalom sikertelen toomatch szabályok megelőző hello bármelyikét, a program eltávolítja a szabály által. Ez az alapértelmezett szabály, és általában aktív, módosítás nélkül általában szükség van. 
  
    ![Tűzfal megtagadási szabály][17]

> [!IMPORTANT]
> Miután az összes fenti szabályok hello jönnek létre, fontos, minden egyes szabály tooensure forgalom tooreview hello rangsorának engedélyezett vagy a kívánt módon megtagadva. Ehhez a példához hello szabályok hello hello Barracuda felügyeleti ügyfél szabályai továbbításának fő rács látható kell hello sorrendben szerepelnek.
> 
> 

## <a name="rule-activation"></a>A szabály aktiválás
Hello módosított szabálykészletben toohello megadását hello rajz, hello szabálykészletben kell toohello tűzfal feltöltött, és a majd aktiválja.

![Tűzfal szabály aktiválás][18]

Hello felügyeleti ügyfél hello jobb felső sarkában a fürtöket gombok. Hello "küldése módosítások" gomb toosend módosított hello szabályok toohello tűzfal elemre, majd kattintson a hello "Aktiválás" gombra.

A hello tűzfal ruleset hello aktiválásával e példa környezet build befejeződött.

## <a name="traffic-scenarios"></a>Forgalom forgatókönyvek
> [!IMPORTANT]
> A kulcs takeway tooremember, amely **minden** forgalmat fog érkezni hello tűzfalon keresztül. Igen tooremote asztali toohello IIS01 kiszolgáló, akkor is, ha hello első End felhőalapú szolgáltatás, valamint hello előtér alhálózati tooaccess ehhez a kiszolgálóhoz a tooRDP toohello tűzfal fel kell port 8014, és majd belső az hello tűzfal tooroute hello RDP-kérés engedélyezése toohello IIS01 RDP-portjára. hello Azure portal "Csatlakozás" gomb nem fog működni, mert nincs közvetlen RDP-elérési út tooIIS01 (szerint hello portal látható). Ez azt jelenti, hogy az összes kapcsolat hello internet lesz toohello biztonsági szolgáltatás és a Port, pl. secscv001.cloudapp.net:xxxx.
> 
> 

Ezek a forgatókönyvek a következő tűzfalszabályok hello helyen legyen:

1. Tűzfalkezelés
2. RDP tooIIS01
3. RDP tooDNS01
4. RDP tooAppVM01
5. RDP tooAppVM02
6. Webes alkalmazás forgalom toohello
7. Alkalmazás forgalom tooAppVM01
8. Kimenő toohello Internet
9. Előtérbeli tooDNS01
10. Helyen belüli forgalmat (csak háttér toofront záró)
11. Az összes megtagadása

hello tényleges tűzfal szabálykészletben valószínűleg lesz számos más szabályok hozzáadása toothese, bármely adott tűzfal hello szabályok is rendelkezik különböző prioritást mint hello azokat, az itt felsorolt. A lista és a hozzájuk társított számokat tooprovide relevanciájának között csak e 11 szabályok és a relatív prioritását hello közülük. Más szóval; hello tényleges tűzfalon hello "RDP tooIIS01" lehet, hogy lehet szabály szám 5, de mindaddig, amíg volna az ebben a listában hello szándéka igazodni hello "RDP tooDNS01" szabály felett és hello "Tűzfal kezelése" szabály alatt van. hello lista lesz is segítséget nyújtanak az alábbi forgatókönyvek kivonatosan mutatja; így hello például "FW szabály 9 (DNS)". Is kivonatosan mutatja négy RDP-szabályok együttesen lesznek hello nevű, "hello RDP szabályok" független tooRDP hello forgalom forgatókönyv esetén.

Visszahívása is, hogy a hálózati biztonsági csoportok helyben a bejövő forgalmat a hello előtér és háttér alhálózatok-e.

#### <a name="allowed-internet-tooweb-server"></a>(Engedélyezett) Internet tooWeb kiszolgáló
1. Internetes felhasználói kérelmek HTTP oldal SecSvc001.CloudApp.Net (Internet Facing Felhőszolgáltatás)
2. Felhőalapú szolgáltatás fázisok forgalom nyitott végpontok 10.0.0.4:80 a 80-as port toofirewall kapcsolaton keresztül
3. Nincs hozzárendelve tooSecurity alhálózat, a rendszer NSG-szabályok lehetővé teszik a forgalom toofirewall NSG
4. Forgalom találatok hello tűzfal (10.0.1.4) belső IP-címe
5. Tűzfal szabály feldolgozása kezdődik:
   1. Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.
   2. 2 – 5 (RDP szabályok) FW szabályok nem teljesül, a toonext szabály áthelyezése
   3. FW 6. szabály (App: webes) alkalmazásához forgalom engedélyezve van, tűzfal NAT azt too10.0.1.4 (IIS01)
6. hello Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG 1. szabály (elérésű internetes) nem vonatkozik (Ez az adatforgalom lett NAT hello tűzfal volna, így hello forráscím mostantól hello tűzfal, amelyek a biztonság – alhálózati hello és hello Frontend alhálózathoz NSG toobe "helyi" forgalom láthatók, és így engedélyezve van), toonext szabály áthelyezése
   2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
7. IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása
8. IIS01 megpróbál egy FTP-munkamenet tooAppVM01 tooinitiates a Backend alhálózathoz
9. hello UDR útvonal Frontend alhálózaton teszi hello tűzfal hello következő ugrás
10. Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van
11. Tűzfal szabály feldolgozása kezdődik:
    1. Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.
    2. Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.
    3. FW 6. szabály (App: webes) nem teljesül, a toonext szabály áthelyezése
    4. FW szabály 7 (App: háttér) alkalmazásához forgalom engedélyezve van, a tűzfal továbbítja a forgalom too10.0.2.5 (AppVM01)
12. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
    1. NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály
    2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
13. AppVM01 hello kérelmet kap, és kezdeményezi hello munkamenet, és válaszol
14. hello UDR útvonal a Backend alhálózathoz teszi hello tűzfal hello következő ugrás
15. Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Backend alhálózathoz hello válasz
16. Mivel a forgalom Ez vissza egy munkamenetet hello tűzfal továbbítja hello válasz hátsó toohello web server (IIS01)
17. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
    1. NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály
    2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
18. hello IIS server hello választ kap, AppVM01 hello tranzakció, és ha igen épület hello HTTP-válasz, a HTTP-válasz küldött toohello kérelmező
19. Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Frontend alhálózathoz hello válasz
20. hello HTTP válasz találatok hello tűzfal, és mivel ez hello válasz tooan létrehozott NAT munkamenet elfogadható hello tűzfal
21. hello tűzfal majd átirányítja a felhasználókat hello válasz hátsó toohello internetes felhasználó
22. Mivel nincsenek kimenő NSG-szabályok vagy UDR ugrások hello Frontend alhálózaton hello választ kap, és hello internetes felhasználó megkapja a kért weblap hello.

#### <a name="allowed-internet-rdp-toobackend"></a>(Engedélyezett) Internet RDP tooBackend
1. Server Admin interneten kérelmek RDP munkamenet tooAppVM01 keresztül SecSvc001.CloudApp.Net:8025, ahol 8025 az hello felhasználó lehet hozzárendelve hello "RDP tooAppVM01" tűzfalszabály portszáma
2. hello felhőszolgáltatás hello nyitott végpontok forgalmát továbbítja a port 8025 toofirewall felülete 10.0.0.4:8025
3. Nincs hozzárendelve tooSecurity alhálózat, a rendszer NSG-szabályok lehetővé teszik a forgalom toofirewall NSG
4. Tűzfal szabály feldolgozása kezdődik:
   1. Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.
   2. Toonext szabály FW 2. szabály (RDP IIS) nem vonatkozik, helyezze át.
   3. Toonext szabály FW 3. szabály (RDP DNS01) nem vonatkozik, helyezze át.
   4. FW 4. szabály (RDP AppVM01) teljesül, a forgalom engedélyezve van, tűzfal NAT azt too10.0.2.5:3386 (RDP-portjához AppVM01)
5. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG 1. szabály (elérésű internetes) nem vonatkozik (Ez az adatforgalom lett NAT hello tűzfal volna, így hello forráscím mostantól hello tűzfal, amelyek a biztonság – alhálózati hello és hello Backend alhálózathoz NSG toobe "helyi" forgalom láthatók, és így engedélyezve van), toonext szabály áthelyezése
   2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
6. AppVM01 RDP-forgalmát a figyeli és reagál
7. Nincsenek kimenő NSG-szabályok, az alapértelmezett szabályok vonatkoznak, és a forgalom engedélyezve van
8. UDR útvonalak kimenő forgalom toohello tűzfal hello következő ugrásként
9. Mivel ez adott vissza forgalom a egy munkamenetet hello tűzfal továbbítja hello válasz hátsó toohello internetes felhasználó
10. RDP-munkamenetbe engedélyezve van
11. Felhasználói nevének jelszavának megadását kéri az AppVM01

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Engedélyezett) Web Server DNS-címkeresés DNS-kiszolgálón
1. Webalkalmazás-kiszolgáló, IIS01, kell egy adatcsatorna www.data.gov, azonban kell tooresolve hello cím.
2. hello tartozó fürthálózat-konfiguráció hello hálózatok listája DNS01 (a hello Backend alhálózathoz 10.0.2.4) hello elsődleges DNS-kiszolgálóként, IIS01 küldi hello DNS kérelem tooDNS01
3. UDR útvonalak kimenő forgalom toohello tűzfal hello következő ugrásként
4. Nincs kimenő NSG-szabályok a következők kötött toohello Frontend alhálózathoz, forgalom engedélyezve van
5. Tűzfal szabály feldolgozása kezdődik:
   1. Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.
   2. Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.
   3. Nem alkalmazza a FW szabályok 6 és 7 (Alkalmazásszabályok), helyezze át a toonext szabály
   4. Toonext szabály FW szabály 8 (tooInternet) nem vonatkozik, helyezze át.
   5. FW szabály 9 (DNS) alkalmazza, a forgalom engedélyezve van, a tűzfal továbbítja a forgalom too10.0.2.4 (DNS01)
6. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály
   2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
7. DNS-kiszolgáló hello kérést kap.
8. DNS-kiszolgáló nem rendelkezik gyorsítótárazott hello címmel és egy legfelső szintű DNS-kiszolgáló kéri a hello internet
9. UDR útvonalak kimenő forgalom toohello tűzfal hello következő ugrásként
10. Nincs kimenő NSG-szabályok a Backend alhálózathoz, forgalom engedélyezve van
11. Tűzfal szabály feldolgozása kezdődik:
    1. Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.
    2. Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.
    3. Nem alkalmazza a FW szabályok 6 és 7 (Alkalmazásszabályok), helyezze át a toonext szabály
    4. FW szabály 8 (tooInternet) alkalmazni, forgalom engedélyezve van, munkamenet SNAT tooroot hello internetes DNS-kiszolgáló ki.
12. Internetes DNS-kiszolgáló válaszol, mivel a munkamenet hello tűzfal kezdeményezték, hello válasz elfogadható hello tűzfal
13. Ez ugyanis a munkamenetet, hello tűzfal továbbítja hello válasz toohello server DNS01 kezdeményezése
14. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
    1. NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály
    2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
15. hello DNS-kiszolgáló fogadja és gyorsítótárazza hello választ, és ezután válaszol toohello kezdeti kérés hátsó tooIIS01
16. hello UDR útvonal a backend alhálózathoz teszi hello tűzfal hello következő ugrás
17. Nincsenek kimenő NSG-szabályok szerepelnek a hello Backend alhálózathoz, forgalom engedélyezve van
18. Ez a hello tűzfal munkamenetet, hello válasz továbbítja hello tűzfal hátsó toohello IIS-kiszolgálón
19. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
    1. Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály
    2. hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van
20. IIS01 hello választ kap DNS01

#### <a name="allowed-backend-server-toofrontend-server"></a>(Engedélyezett) Kiszolgáló tooFrontend háttérkiszolgáló
1. RDP-kapcsolaton keresztül tooAppVM02 bejelentkezett rendszergazda olyan fájlt kér, közvetlenül a hello IIS01 kiszolgálón windows Fájlkezelőben-n keresztül
2. hello UDR útvonal a Backend alhálózathoz teszi hello tűzfal hello következő ugrás
3. Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Backend alhálózathoz hello válasz
4. Tűzfal szabály feldolgozása kezdődik:
   1. Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.
   2. Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.
   3. Nem alkalmazza a FW szabályok 6 és 7 (Alkalmazásszabályok), helyezze át a toonext szabály
   4. Toonext szabály FW szabály 8 (tooInternet) nem vonatkozik, helyezze át.
   5. Nem alkalmazható FW szabály 9 (DNS), helyezze át a toonext szabály
   6. FW szabály 10 (Intra-alhálózat) alkalmazza, a forgalom engedélyezve van, a tűzfal továbbítja a forgalom too10.0.1.4 (IIS01)
5. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály
   2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
6. Ha a megfelelő hitelesítési és engedélyezési, IIS01 hello kérést fogad és reagál
7. hello UDR útvonal Frontend alhálózaton teszi hello tűzfal hello következő ugrás
8. Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Frontend alhálózathoz hello válasz
9. Ez ugyanis egy meglévő munkamenetben hello tűzfalon ezt a választ kap, és hello tűzfal hello válasz tooAppVM02 adja vissza.
10. Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
    1. NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály
    2. Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása
11. AppVM02 hello válasz fogadása

#### <a name="denied-internet-direct-tooweb-server"></a>(Megtagadva) Az Internet közvetlen tooWeb kiszolgáló
1. Internetes felhasználó megpróbál tooaccess hello webkiszolgáló IIS01, hello FrontEnd001.CloudApp.Net szolgáltatás keresztül
2. Mivel nincsenek végpontok nyissa meg a HTTP-forgalom, ez nem lenne továbbítása hello felhőalapú szolgáltatás, és így elérni hello kiszolgálót
3. Ha valamilyen okból hello végpontok nyitott, hello NSG-t (Internet blokk) hello Frontend alhálózaton megakadályozza a forgalom
4. Végül hello Frontend alhálózathoz UDR útvonal minden kimenő forgalom elküldése a IIS01 toohello tűzfal hello következő ugrásként, és hello tűzfal ehhez megjelenik ez az aszimmetrikus forgalmat, és dobja el a hello kimenő válasz legalább három független réteg van így védekezés hello közötti internetes és az IIS01 keresztül a felhőalapú szolgáltatás nem engedélyezett/nem megfelelő elérésének megakadályozásához.

#### <a name="denied-internet-toobackend-server"></a>(Megtagadva) Internet tooBackend kiszolgáló
1. Internetes felhasználó megpróbál tooaccess keresztül hello BackEnd001.CloudApp.Net szolgáltatás AppVM01 fájlba
2. Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello kiszolgálót
3. Ha valamilyen okból hello végpontok nyitott, hello NSG-t (Internet blokk) megakadályozza a forgalom
4. Végül hello UDR útvonal minden kimenő forgalom elküldése a AppVM01 toohello tűzfal hello következő ugrásként, és hello tűzfal akkor megjelenik ez az aszimmetrikus adatforgalom és a drop hello kimenő válasz legalább három független rétegek közötti védelmi van így internetes és az AppVM01 hello segítségével a felhőalapú szolgáltatás nem engedélyezett/nem megfelelő elérésének megakadályozásához.

#### <a name="denied-frontend-server-toobackend-server"></a>(Megtagadva) Előtérbeli server tooBackend kiszolgáló
1. Tegyük fel, IIS01 biztonsági szempontból sérült és fut-e rosszindulatú kódot próbált tooscan hello alhálózati háttérkiszolgálókhoz.
2. hello Frontend alhálózathoz UDR útvonal elküldése minden kimenő forgalom IIS01 toohello tűzfaltól hello következő ugrásként. Ez nem lehet megváltoztatni egy hello VM sérült.
3. hello tűzfal szeretné feldolgozni hello forgalmat, ha hello kérelmező tooAppVM01 vagy toohello DNS-kiszolgáló DNS-keresések, amely a forgalom potenciálisan megengednek hello tűzfal (miatt tooFW szabályok 7 és 9). Az összes többi forgalom FW szabály 11 (az összes megtagadása) lenne blokkolja.
4. Ha speciális fenyegetésészlelés engedélyezve lett hello tűzfal (a jelen dokumentum nem ismerteti, az adott hálózati berendezések advanced threat képességek hello gyártói dokumentációjában), akkor is igaz, akkor engedélyezni által hello alapvető szabályok továbbítás forgalom Ezen tárgyalt dokumentum sikerült előzhető, ha hello forgalom ismert aláírások vagy mintáról olvashat, amelyek az advanced threat szabály jelzőt tartalmaz.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Megtagadva) A DNS-kiszolgáló internetes DNS-címkeresés
1. Internetes felhasználó megpróbál toolookup egy belső DNS-rekordot a DNS01 BackEnd001.CloudApp.Net szolgáltatáson keresztül 
2. Mivel nincsenek végpontok nyissa meg a DNS-forgalmat, ez nem lenne továbbítása hello felhőalapú szolgáltatás, és így elérni hello kiszolgálót
3. Ha valamilyen okból hello végpontok nyitott, hello NSG szabály (elérésű internetes) hello Frontend alhálózaton blokkolná-e a forgalom
4. Végül hello Backend alhálózathoz UDR útvonal minden kimenő forgalom elküldése a DNS01 toohello tűzfal hello következő ugrásként, és hello tűzfal ehhez megjelenik ez az aszimmetrikus forgalmat, és dobja el a hello kimenő válasz legalább három független réteg van így védekezés hello közötti internetes és az DNS01 keresztül a felhőalapú szolgáltatás nem engedélyezett/nem megfelelő elérésének megakadályozásához.

#### <a name="denied-internet-toosql-access-through-firewall"></a>(Megtagadva) Tűzfalon keresztüli internetkapcsolattal tooSQL
1. Internetes felhasználó SQL adatokat kér a SecSvc001.CloudApp.Net (Internet Facing Felhőszolgáltatás)
2. Mivel nincsenek végpontok nyissa meg az SQL, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello tűzfal
3. Ha valamilyen okból SQL végpontok nyitott, hello tűzfal szabály feldolgozása volna megkezdéséhez:
   1. Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.
   2. 2 – 5 (RDP szabályok) FW szabályok nem teljesül, a toonext szabály áthelyezése
   3. Toonext szabály nem alkalmazza a FW szabály 6 és 7 (alkalmazás szabályok), helyezze át.
   4. Toonext szabály FW szabály 8 (tooInternet) nem vonatkozik, helyezze át.
   5. Nem alkalmazható FW szabály 9 (DNS), helyezze át a toonext szabály
   6. Toonext szabály FW szabály 10 (Intra-alhálózat) nem vonatkozik, helyezze át.
   7. FW szabály 11 (az összes megtagadása) alkalmazza, a forgalom letiltott, befejezése szabály feldolgozása

## <a name="references"></a>Referencia
### <a name="main-script-and-network-config"></a>Fő parancsfájlt és a hálózati konfiguráció
Hello teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt. Hálózati konfiguráció hello "NetworkConf2.xml" fájlba mentése.
Szükség szerint módosítsa a hello felhasználó által definiált változókat. Hello parancsfájl futtatása, majd kövesse a hello tűzfal szabály telepítő utasítás fenti.

#### <a name="full-script"></a>Teljes körű parancsfájl
Fogja ezt a parancsfájlt, a felhasználó által definiált változóknak hello alapján:

1. Csatlakozás Azure-előfizetés tooan
2. Új tárfiók létrehozása
3. Hozzon létre egy új virtuális hálózat és a hello hálózati konfigurációs fájljában definiált három alhálózatok
4. Build öt virtuális gépek; 1 tűzfal és a 4 a windows server virtuális gépek
5. Adja meg UDR többek között:
   1. Két új útvonal-táblázatok létrehozása
   2. Útvonalak toohello táblák hozzáadása
   3. Kötési táblák tooappropriate alhálózatok
6. Az IP-továbbítás hello NVA engedélyezése
7. Adja meg, beleértve az NSG:
   1. Egy NSG létrehozása
   2. Szabály hozzáadása
   3. Kötési hello NSG toohello megfelelő alhálózatokat

A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.

> [!IMPORTANT]
> Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit. Piros csak hibaüzenetek aggodalomra.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet
       - Three Servers on hello BackEnd Subnet
       - IP Forwading from hello FireWall out toohello internet
       - User Defined Routing FrontEnd and BackEnd Subnets toohello NVA

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind hello appropriate
      layer(s) of protection. This script serves as an example of some of hello techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and hello appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes tooreflect your subscription and services
      # Invalid options will fail in hello validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be hello NVA.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - hello First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - hello Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - hello DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer tooNew Storage Account
        Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "hello SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation varible is correct and hello netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see hello above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all hello EndPoints we'll need once we're up and running
                # Note: All traffic goes through hello firewall, so we'll need tooset up all ports here.
                #       Also, hello firewall will be redirecting traffic tooa new IP and Port in a forwarding
                #       rule, so all of these endpoint have hello same public and local port and hello firewall
                #       will do hello mapping, NATing, and/or redirection as declared in hello firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create hello Route Tables
        Write-Host "Creating hello Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes tooRoute Tables
        Write-Host "Adding Routes toohello Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate hello Route Tables with hello Subnets
        Write-Host "Binding Route Tables toohello Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on hello Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign hello NSG tootwo Subnets
        # hello NSG is *not* bound toohello Security Subnet. hello result
        # is that internet traffic flows only toohello Security subnet
        # since hello NSG bound toohello Frontend and Backback subnets
        # will Deny internet traffic toothose subnets.
        Write-Host "Binding hello NSG tootwo subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on hello IIS Server)
      # Install Backend resource (Run Post-Build Script on hello AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Hálózati konfigurációs fájl
Az XML-fájl mentése frissített hellyel rendelkező, és adja hozzá a hello hivatkozás toothis fájl toohello $NetworkConfigFile változó a fenti hello parancsfájl.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Alkalmazás mintaparancsfájlok
Ha a tooinstall mintaalkalmazás ezzel és más DMZ példák, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Kétirányú DMZ NVA, NSG és UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Tűzfalszabályok hello logikai ábrázolása"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Hozzon létre egy előtér-hálózati objektumot"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Hozzon létre egy DNS-kiszolgáló objektum"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Alapértelmezett RDP-szabály másolása"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 szabály"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Alkalmazás átirányítási ikonja"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Cél NAT ikon"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pass ikon"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Felügyeleti tűzfalszabály"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP tűzfalszabály"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Webes tűzfalszabály"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Tűzfalszabály AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Kimenő tűzfalszabály"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS tűzfalszabály"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Tűzfal Intra-VNet szabály"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Tűzfal megtagadási szabály"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Tűzfal szabály aktiválás"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
