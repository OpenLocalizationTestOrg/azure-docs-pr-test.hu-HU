---
title: "egy Azure virtuális hálózatra alkalmazás aaaIntegrate"
description: "Bemutatja, hogyan tooconnect egy alkalmazást az Azure App Service tooa új vagy meglévő Azure virtuális hálózat"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ccompy
ms.openlocfilehash: a93c504481400245b02220b541a008a7c874d10a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Az alkalmazás integrálása az Azure virtuális hálózat
Ez a dokumentum bemutatja hello Azure App Service virtuális hálózati integráció szolgáltatást, és bemutatja, hogyan tooset azt másolatot az alkalmazásokkal [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Ha ismeri az Azure virtuális hálózatokról (Vnetekről), ez az a képesség, amely lehetővé teszi az Azure-erőforrások, amelyek nem internetes routeable hálózatban számos elérhető tooplace. Ezek a hálózatok majd lehet csatlakoztatott tooyour a helyszíni hálózatokban VPN technológiáin számos használatát. További Azure virtuális hálózatokról, toolearn hello információkat itt kezdődik: [Azure virtuális hálózat áttekintése][VNETOverview]. 

hello Azure App Service rendelkezik két űrlap. 

1. hello teljes körű díjszabások támogató hello több-bérlős rendszerek
2. hello App Service-környezet (ASE) premium szolgáltatás, amely telepíti azokat a virtuális hálózat. 

Ez a dokumentum végighalad a virtuális integráció és nem App Service Environment-környezet. Ha azt szeretné, hogy hello ASE funkcióval kapcsolatban további toolearn, indítsa el a hello információkat itt: [App Service Environment bemutatása][ASEintro].

Virtuális integráció a webes alkalmazás hozzáférés tooresources biztosít a virtuális hálózat, de nem biztosít a privát hozzáférést tooyour webalkalmazás hello virtuális hálózatról. Személyes hozzáférés hivatkozik toomaking az alkalmazás csak egy privát hálózatról, mint az Azure virtuális hálózat belül érhető el. Titkos hozzáférés csak érhető el egy konfigurált egy belső Load Balancer (ILB) rendelkező mértékéig. Egy ILB ASE használata részletesen, kezdje itt hello cikk: [létrehozása és használata egy ILB ASE][ILBASE]. 

Egy gyakori forgatókönyv, ahol a VNet integrációs használna a webes alkalmazás tooa adatbázis vagy az Azure virtuális hálózat a virtuális gépen futó webes szolgáltatás hozzáférést tesz lehetővé a rendszer. A virtuális hálózat integrációja nem kell az alkalmazások tooexpose egy nyilvános végpontot a virtuális gépen, de hello nem internetes irányítható Magáncímeket is használja. 

hello VNet integrációs szolgáltatás számára:

* a Standard, Premium vagy terv árképzési elszigetelt igényel 
* Klasszikus vagy erőforrás-kezelő virtuális hálózat együttműködik 
* támogatja a TCP és UDP
* Webes, mobil és API-alkalmazások esetében működik
* lehetővé teszi, hogy egy alkalmazás tooconnect tooonly 1 VNet egyszerre
* lehetővé teszi a toofive Vnetek toobe integrálva van a egy App Service-csomag 
* lehetővé teszi, hogy egy App Service-csomag több alkalmazások által használt azonos virtuális hálózaton toobe hello
* támogatja az egy 99,9 %-os SLA miatt toohello SLA hello hálózatok átjáró

Néhány dolgot virtuális integráció nem támogatja, többek között:

* a meghajtó csatlakoztatása
* AD-integrációs 
* NetBios
* személyes hozzáférés

### <a name="getting-started"></a>Bevezetés
Az alábbiakban néhány dolgot tookeep szem előtt a webes alkalmazás tooa virtuális hálózati csatlakozás előtt:

* Virtuális integráció csak olyan alkalmazások működik egy **szabványos**, **prémium**, vagy **elszigetelt** árképzési terv. Ha hello funkció engedélyezéséhez, majd méretezni az App Service-csomag tooan nem támogatott az alkalmazások terv árképzési elveszítheti a kapcsolatok toohello Vnetek használják. 
* Ha a cél virtuális hálózat már létezik, pont-pont VPN engedélyezve van egy dinamikus útválasztási átjáró, mielőtt azok csatlakoztatott tooan app kell rendelkeznie. Ha az átjáró statikus útválasztással van konfigurálva, nem engedélyezhető a pont-pont virtuális magánhálózati (VPN).
* hello virtuális hálózaton kell lennie a hello az App Service Plan(ASP) tárolóként ugyanazt az előfizetést. 
* hello alkalmazásokat, amelyekbe beépül az egy Vnetet hello megadott, hogy a vnet DNS használja.
* Alapértelmezés szerint az integrációs alkalmazások csak útvonal-forgalmat a virtuális hálózat a virtuális hálózat definiált hello útvonalak alapján történő. 

## <a name="enabling-vnet-integration"></a>Virtuális integráció engedélyezése
Ez a dokumentum elsősorban a hello Azure-portál használatával virtuális integráció kialakításával foglalkozik. tooenable virtuális integráció saját alkalmazással powershellel, kövesse az itt hello utasításait: [PowerShell használatával kapcsolódnak a app tooyour virtuális hálózat][IntPowershell].

Hello beállítás tooconnect rendelkezik az app tooa új vagy meglévő virtuális hálózat. Ha létrehoz egy új hálózati integráció részeként, majd továbbá toojust létrehozása hello VNet dinamikus útválasztó átjáró meg előre beállított, amely pont tooSite VPN engedélyezve van. 

> [!NOTE]
> Egy új virtuális hálózati integráció konfigurálásához több percet is igénybe vehet. 
> 
> 

tooenable virtuális integráció, nyissa meg az alkalmazás beállításait, és válassza a hálózat. felhasználói felület, a megnyitott hello három hálózatkezelési lehetőségeket kínál. Ez az útmutató csak érintetlen virtuális integráció be, ha hibrid kapcsolatok és az App Service Environment-környezetek ismertetése a dokumentum későbbi szakaszában. 

Ha az alkalmazás nem a megfelelő terv árképzési hello, hello felhasználói felület lehetővé teszi tooscale a terv tooa magasabb árképzési terv az Ön által választott.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Egy már meglévő virtuális hálózaton VNet-integráció engedélyezése
hello virtuális integráció felhasználói felület lehetővé teszi a Vnetek listájából tooselect. a hagyományos Vneteket hello jelezve, hogy ilyen hello Word "Klasszikus" zárójelek következő toohello VNet neve. hello lista rendezett úgy, hogy a hello erőforrás-kezelő Vnetek először. A hello kép alábbi értékekre, akkor látható, hogy csak egy virtuális hálózat választható ki. Több oka, hogy a virtuális hálózat is jelenítette meg többek között:

* hello virtuális hálózat egy másik előfizetésben található, amely a fiókjának van hozzáférési joga van
* hello virtuális hálózat nem rendelkezik pont tooSite engedélyezve
* hello virtuális hálózat nem rendelkezik egy dinamikus útválasztási átjáró

![][2]

tooenable integrációs egyszerűen kattintson a hello kívánja toointegrate a virtuális hálózat. Miután kiválasztotta a virtuális hálózat hello, az alkalmazás hello módosítások tootake hatás automatikusan újraindul. 

##### <a name="enable-point-toosite-in-a-classic-vnet"></a>A klasszikus virtuális pont tooSite engedélyezése
Ha a virtuális hálózat nem kell egy átjárót, és pont tooSite rendelkezik, akkor rendelkezik tooset, hogy naprakész első. toodo ezt a klasszikus virtuális hálózatot, nyissa meg toohello [Azure-portálon] [ AzurePortal] és elindítani a virtuális Networks(classic) hello listája. Itt kattintson a toointegrate ki, majd kattintson a hello nagy jelölését a VPN-kapcsolatok nevű Essentials hello hálózaton. Itt hozhat létre a pont toosite VPN és van arra is, akkor hozzon létre. Az átjáró létrehozása révén hello pont toosite lépéseinek után körülbelül 30 percet, mielőtt kész is. 

![][8]

##### <a name="enabling-point-toosite-in-a-resource-manager-vnet"></a>Egy erőforrás-kezelő virtuális pont tooSite engedélyezése
egy erőforrás-kezelő virtuális hálózatot az átjáró és pont tooSite tooconfigure, vagy a PowerShell segítségével dokumentált módon Itt [pont – hely kapcsolat tooa virtuális hálózat konfigurálása PowerShell-lel] [ V2VNETP2S] vagy használjon hello Azure-portálon dokumentált Itt [konfigurálása egy pont – hely kapcsolat tooa virtuális hálózat használatával hello Azure-portálon][V2VNETPortal]. hello felhasználói felület tooperform Ez a funkció még nem érhető el. Ne feledje, hogy toocreate tanúsítványok hello pont tooSite konfigurációhoz. Ez automatikusan konfigurálja a webalkalmazás toohello VNet csatlakoztatásakor. 

### <a name="creating-a-pre-configured-vnet"></a>Egy előre konfigurált virtuális hálózat létrehozása
Ha azt szeretné, toocreate egy új Vnetet, amelynek része egy átjáró és a pont-hely, majd hello felhasználói felület hálózati App Service rendelkezik hello funkció toodo adott, de csak egy erőforrás-kezelő virtuális hálózat számára. Ha toocreate egy klasszikus virtuális hálózatot egy átjáró és a pont-pont, akkor szüksége toodo ez manuálisan hello hálózatkezelés felhasználói felületen keresztül. 

toocreate egy erőforrás-kezelő virtuális hálózat hello virtuális integráció felhasználói felületén keresztül egyszerűen válassza **új virtuális hálózat létrehozása** , és adja meg a:

* Virtuális hálózat neve
* Virtuális hálózat címterülete
* Alhálózat neve
* Alhálózati címterülete
* Átjáró címterülete
* Pont-pont címterülete

Ha a virtuális hálózat tooconnect tooany más hálózatokkal, majd ne válassza háttérszínnek IP-címtartomány átfedésben az ezekhez a hálózatokhoz. 

> [!NOTE]
> Erőforrás-kezelő VNet létrehozása egy átjáró körülbelül 30 percet vesz igénybe, és jelenleg nem hello virtuális hálózat és az alkalmazás integrációjához. Hello átjáróval a virtuális hálózat létrejötte után kell toocome hátsó tooyour app virtuális integráció felhasználói felület, és válassza ki az új Vnetet.
> 
> 

![][3]

Az Azure Vnetekhez általában jönnek létre magánhálózati címek. Alapértelmezett hello virtuális integráció által a szolgáltatás a virtuális hálózat be ezeket az IP-címtartományok bármely adatforgalmat irányítja. hello privát IP-címtartományok a következők:

* -Ez van hello azonos 10.0.0.0 típusúként - 10.0.0.0/8 10.255.255.255
* -Ez van hello ugyanaz, mint 172.16.0.0 - 172.16.0.0/12 172.31.255.255 
* -Ez van hello ugyanaz, mint 192.168.0.0 - 192.168.0.0/16 192.168.255.255

hello VNet címterület CIDR-formátumban megadott toobe kell. Ha nincs tisztában a CIDR-formátumban, a rendszer a címterületet IP-címet és egy egész számot jelölő hello hálózati maszk megadásának módszer. Egy rövid összefoglaló vegye figyelembe, hogy 10.1.0.0/24 256 címet és 10.1.0.0/25 lenne 128 címek. Egy IPv4-cím egy /32 csak 1 cím lenne. 

Ha az itt beállított hello DNS-kiszolgáló adatait, majd, hogy be van állítva a virtuális hálózat. VNet létrehozása után szerkesztheti ezt az információt hello VNet felhasználói felületéről. Ha megváltoztatja hello a hello virtuális hálózat DNS-kiszolgálón, akkor szüksége tooperform egy szinkronizálási hálózati művelet.

Amikor létrehoz egy klasszikus virtuális hálózat hello virtuális integráció felhasználói felület használatával, olyan hoz létre egy Vnetet a hello ugyanabban az erőforráscsoportban az alkalmazáshoz. 

## <a name="how-hello-system-works"></a>Hello rendszer működése
Hello színfalak Ez a szolgáltatás épít, pont-hely típusú VPN-technológia tooconnect felett az alkalmazás tooyour virtuális hálózat. Az Azure App Service alkalmazások is rendelkeznek egy több-bérlős rendszer-architektúra, amely kizárja a Vneten belül közvetlenül egy alkalmazás kiépítés módon történik a virtuális gépek. Pont-pont technológia építve jelenleg korlátozza a hálózati hozzáférés toojust hello virtuálisgép üzemeltetési hello app. Hozzáférés toohello hálózati tovább korlátozza ilyen alkalmazás csomópontokon, hogy az alkalmazások csak férjenek hozzá, hogy konfigurálja azokat tooaccess hello hálózatok. 

![][4]

Ha virtuális hálózati DNS-kiszolgáló még nincs konfigurálva, az alkalmazás toouse IP-címek tooreach erőforrás a virtuális hálózat hello kell. IP-címet használja, ne feledje, hogy hello fő előnye, hogy ezt a szolgáltatást, hogy lehetővé teszi a magánhálózaton belül toouse hello Magáncímeket. Ha beállított toouse be az alkalmazás nyilvános IP-címek a virtuális gépek egyik ezután hello VNet integrációs szolgáltatás számára nem használt és keresztül kommunikáló hello az interneten.

## <a name="managing-hello-vnet-integrations"></a>Hello virtuális integráció kezelése
képes tooconnect hello, vagy válassza le a virtuális hálózat van az alkalmazás szintjén tooa. Hatással lehet a virtuális integráció hello több alkalmazás között olyan ASP szinten. Hello hello alkalmazási szintű látható felhasználói felület, a részletek kaphat, ha a virtuális hálózaton. A legtöbb hello ugyanazokat az információkat is látható: hello ASP szint. 

![][5]

Hello hálózati funkció állapota lapon látható, ha az alkalmazás-e a csatlakoztatott tooyour virtuális hálózat. Ha a virtuális hálózat átjáró bármilyen oknál fogva nem működik, majd ez látható, nincs csatlakoztatva. 

hello információk szintű virtuális integráció felhasználói felület hello alkalmazásban most már elérhető tooyou hello azonos hello részletes információk hello ASP kap. Az alábbiakban azokat a cikkeket:

* Virtuális hálózat neve – Ez a hivatkozás megnyitja hello Azure-beli virtuális hálózat felhasználói felület
* Hely – a virtuális hálózat helyét hello tükrözi. A virtuális hálózaton, egy másik helyen lehetséges toointegrate.
* Tanúsítványállapot - használt tanúsítványok toosecure hello VPN-kapcsolat hello alkalmazás és hello VNet között van. Ez egy teszt tooensure szinkronban a tükrözi.
* Az átjáró állapotának - az átjárók kell le valamilyen okból majd az alkalmazás nem fér hozzá hello virtuális hálózaton lévő erőforrások. 
* Virtuális címterület - Ez az IP-címterület hello a Vnethez tartozó. 
* Pont tooSite címterület - hello pont toosite IP-címtér a Vnethez tartozó azt. Az alkalmazás látható hello a címterületen belüli IP-címek egyikét érkező kommunikációt. 
* Hely toosite címterület - hely tooSite VPN tooconnect használhatja a VNet tooyour helyszíni erőforrásokhoz vagy tooother virtuális hálózat. Kell majd hello IP-címtartományok meghatározott VPN-kapcsolat itt látható konfigurált.
* DNS-kiszolgálók – Ha a virtuális hálózaton konfigurált DNS-kiszolgálók és az itt felsorolt.
* IP-címek irányított toohello VNet – a virtuális hálózat van definiálva a következő útválasztási IP-címek listáját, és ezekre a címekre találat. 

hello egyetlen művelet elvégezhető hello app virtuális integráció nézetében toodisconnect az alkalmazását hello virtuális hálózat jelenleg csatlakoztatva van. Ez egyszerűen kattintson leválasztása hello felső toodo. Ez a művelet nem módosítja a virtuális hálózat. hello virtuális hálózat és a konfiguráció, beleértve az hello átjárók változatlan marad. Majd toodelete a VNet, akkor kell toofirst delete hello-erőforrásokat hello átjárók beleértve. 

App Service-csomag nézet hello számos további műveletek. Is hozzáférés másképp mint hello alkalmazásból. tooreach hello ASP hálózat felhasználói felületén nyissa meg az ASP felhasználói felületi és görgetve. Van egy felhasználói felületi elem nevű hálózati szolgáltatás állapotát. Egyes kisebb adatok körül virtuális integráció biztosít. Ez a felhasználói felület a kattintva megnyithatja a hálózati szolgáltatás állapot felhasználói felületi hello. Ha, majd kattintson a "kattintson ide toomanage", hello felhasználói felület, amely tartalmazza az ASP a VNet Integrációk megnyitja hello.

![][6]

hello hello ASP helye jó tooremember kezelőkonzoljából nézve hello integrációhoz Vnetek hello helyét. Virtuális hálózat hello van egy másik helyen elkülönítésével sokkal nagyobb eséllyel toosee késési problémák szempontjából. 

hello Vnetek integrálva a hány Vnetek egy emlékeztető, az alkalmazások integrálva vannak az ASP és hány, akkor is. 

toosee részleteit hozzá minden egyes virtuális hálózaton, a virtuális hálózat érdekli hello parancsot. Továbbá, amely korábban feljegyzett toohello részletek megtekintheti egy alkalmazáslistájából hello az ASP által használt, hogy a virtuális hálózat. 

Illetően tooactions hiba a két elsődleges műveletek. hello először hello képességét tooadd útvonalakat, amelyek az alkalmazás azokat a virtuális hálózat elhagyó forgalomra meghajtó. hello második művelete hello képességét toosync tanúsítványokat és a hálózat adatait.

![][7]

**Útválasztás** korábban feljegyzett határozzák meg a virtuális hálózat hello útvonalakat a rendszer irányítja a forgalmat a virtuális hálózat az alkalmazásból történő alkalmazott. Abban az esetben, ha az ügyfelek, ahová az alkalmazásokból kimenő forgalom további toosend hello virtuális hálózat és a számukra megadott ezt a képességet, nincsenek néhány felhasználását. Mi történik a toohello forgalom követően, hogy működik-e toohow hello ügyfél konfigurálja a virtuális hálózat. 

**Tanúsítványok** hello tanúsítvány állapotát tükrözi egy App Service toovalidate, hogy a VPN-kapcsolat hello használunk hello tanúsítványok továbbra is megfelelőek hello által végzett ellenőrzést. Ha virtuális integráció engedélyezve van, majd ha hello első integrációs toothat VNet az ASP, az alkalmazások a nincs tanúsítványok tooensure hello biztonsági hello kapcsolat szükséges exchange. Hello tanúsítványok mellett azt lekérése hello DNS-konfiguráció, útvonalakat és más hasonló dolgot hello hálózati leíró.
Ha ezeket a tanúsítványokat vagy hálózati adatok módosul, akkor szüksége tooclick "Sync hálózat". **Megjegyzés:**: "Sync hálózat" kattintva, majd a rövid kimaradásokat okozhat az alkalmazás és a virtuális hálózat közötti kapcsolat. Az alkalmazás nem indítják újra, amíg a kapcsolat megszakadása hello okozhat a hely toonot függvény megfelelően. 

## <a name="accessing-on-premises-resources"></a>Hozzáférés a helyszíni erőforrások
Hello VNet integrációs szolgáltatás előnyei hello egyike, hogy ha a virtuális hálózat csatlakozik tooyour helyszíni egy hely tooSite VPN hálózat és az alkalmazások is rendelkeznek tooyour az alkalmazásból a helyszíni erőforrások eléréséhez. A toowork a tooupdate szükség lehet, ha a helyszíni VPN átjáró hello irányítja a pont tooSite IP-címtartomány. Hello hely tooSite VPN először beállítását hello parancsfájlok használja a rendszer tooconfigure azt útvonalakat, beleértve a pont tooSite VPN-érdemes beállítania. Ha a hely tooSite VPN létrehozása után hozzáadhat hello pont tooSite VPN, akkor szüksége tooupdate hello útvonalak manuálisan. Hogyan toodo átjárón eltérők lehetnek, és nem az itt ismertetett az adatokat. 

> [!NOTE]
> hello VNet integrációs szolgáltatás számára nem integrálható egy Vnetet, amely rendelkezik egy ExpressRoute-átjárót egy alkalmazást. Akkor is, ha hello ExpressRoute-átjárót úgy van konfigurálva, a [párhuzamos módban] [ VPNERCoex] hello virtuális integráció nem működik. Ha tooaccess erőforrásoknak ExpressRoute kapcsolat van szüksége, akkor is használhat egy [App Service Environment-környezet][ASE], amelyen fut a virtuális hálózat.
> 
> 

## <a name="pricing-details"></a>Díjszabás részletei
Van néhány apró kell ügyelnie, ha a hello VNet integrációs szolgáltatás díjszabása. Nincsenek 3 kapcsolódó díjakat toohello használatára:

* Az ASP árképzési szint követelmények
* Adatátviteli költségek
* VPN-átjáró költségeket.

Az alkalmazások toobe képes toouse az ezzel a funkcióval szükségük toobe Standard vagy prémium szintű App Service-csomag. További információt itt azok a költségek is olvashat: [App Service szolgáltatás díjszabása][ASPricing]. 

Lejáró toohello módon pont tooSite kezeli, VPN, mindig van járnak a kimenő adatok a virtuális integráció kapcsolaton keresztül akkor is, ha hello VNet a hello azonos adatközpontba. toosee mi ezeket a költségeket, itt tekintse meg: [adatok átvitele Díjszabásának részleteit][DataPricing]. 

utolsó hello elem-hello VNet átjárók hello költségét. Ha nincs szüksége hello átjárók valami mást, például a webhely tooSite VPN, majd fizet átjárók toosupport hello VNet integrációs szolgáltatás számára. Részletek érhetők el a költségekre itt: [VPN-átjáró árképzési][VNETPricing]. 

## <a name="troubleshooting"></a>Hibaelhárítás
Hello szolgáltatás egyszerű tooset működik-e, amíg, hogy nem jelenti azt, hogy a felhasználói élmény lesz-e szabad probléma. A kívánt végpont elérésével kapcsolatos problémák kell észlel néhány tootest kapcsolat hello app konzol használható eszközöket. Nincsenek két konzol feladatait is használhatja. Egyik hello Kudu konzolról, más hello hello konzolt, amely az Azure-portálon hello érhető el. tooget toohello Kudu konzol az alkalmazásból lépjen a tooTools Kudu ->. Ez van hello ugyanaz, mint túl fog [sitename]. scm.azurewebsites.net. Amennyiben az adott megnyílik egyszerűen lépjen toohello hibakeresési konzol külön-külön tooget toohello majd az alkalmazásból az Azure portál központi konzolon nyissa meg a tooTools konzol ->. 

#### <a name="tools"></a>Eszközök
hello eszköz pingelést, nslookup és tracert hello konzollal toosecurity korlátozások miatt nem fognak működni. Nincs toofill hello void már hozzáadott két külön eszközök. Rendelés tootest DNS funkcióinak hozzáadott egy nameresolver.exe nevű eszköz. hello szintaxisa:

    nameresolver.exe hostname [optional: DNS Server]

Az alkalmazás függ nameresolver toocheck hello állomásnevek is használhatja. Ezzel a módszerrel tesztelheti Ha semmit a DNS-beli rosszul konfigurálva van, vagy esetleg nem rendelkezik hozzáféréssel tooyour DNS-kiszolgáló.

hello következő eszköz lehetővé teszi a TCP-kapcsolat tooa állomás és port kombinációja tootest. Ez az eszköz tcpping.exe nevezik, és hello szintaxisa:

    tcpping.exe hostname [optional: port]

hello tcpping segédprogram jelzi, hogy ha érhető el egy adott állomás és port. Csak megmutatja sikeres ha: egy alkalmazás a következőnél: hello állomás és port kombinációja, és a hálózati hozzáférés az alkalmazás toohello megadott állomás és port.

#### <a name="debugging-access-toovnet-hosted-resources"></a>Hibakeresési hozzáférés tooVNet üzemeltetett erőforrásokhoz
Számos dolgot, amely megakadályozhatja, hogy az alkalmazás egy adott állomás és port elérését. A legtöbb hello idő három lehetőség közül:

* **Van egy tűzfal hello módon** Ha tűzfal hello módon, hogy elért hello TCP időtúllépési. Ebben az esetben ez 21 másodperc. Hello tcpping eszköz tootest kapcsolatot használjon. TCP-időtúllépések lehet, hogy túl tűzfalak toomany dolog, de kezdeni. 
* **Nem érhető el DNS** hello DNS időtúllépési érték egy DNS-kiszolgálón három másodperc. Ha két DNS-kiszolgálók, a hello időtúllépési érték 6 másodperc. Nameresolver toosee használja, ha a DNS nem működik. Ne felejtse el, nem használhatja az nslookup nem használó hello DNS, a virtuális hálózat van konfigurálva.
* **Érvénytelen P2S IP-címtartomány** hello pont toosite IP-címtartomány van szüksége az RFC 1918 hello privát IP-címtartományok toobe (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Ha hello tartományon kívül, amely IP-címeket használ, dolgot nem fognak működni. 

Ha ezen elemekre nem válaszolja meg a problémát, keresse meg hello egyszerű többek között az első: 

* Nem hello átjáró megjelenítése, hogy másolatot hello portálon?
* Tanúsítványok, hogy szinkronban jelennek?
* Birtokában bárki változtatta hello hálózati konfigurációt a "Sync hálózat" az érintett hello ASP nélkül? 

Ha az átjáró nem működik, majd hálózatra kell kapcsolni, készítsen biztonsági másolatot. Ha a tanúsítványok nincsenek szinkronban, majd nyissa meg a virtuális hálózat integráció toohello ASP nézetében, és kattintson a "Sync hálózat". Ha azt gyanítja, hogy történt a módosítások tooyour VNet konfigurációját, és azt nem történt szinkronizálás üzembe helyezése az ASP, majd nyissa meg a virtuális hálózat integráció toohello ASP nézetében, és kattintson a "Sync hálózat" csak ne feledje, a rövid kimaradásokat ennek hatására az alkalmazások és a VNet-kapcsolatot . 

Ha, amely minden rendben, akkor szüksége a mélyebb bit toodig:

* Rendszer nincs bármely más alkalmazások virtuális integráció tooreach erőforrások hello ugyanazt a virtuális hálózatot? 
* Akkor megnyithatja toohello app konzol és a Vneten belül az összes erőforrását tcpping tooreach használja? 

Ha a fenti hello bármelyike igaz, majd virtuális integráció finom pedig hello probléma valahol máshol. Ez azért, ahol nyer toobe több kérést, mert nincs egyszerű módszer toosee, miért nem éri el a gazdagép és a portszám. Hello okok a következők:

* a számítógépen egy tűzfal hozzáférési toohello alkalmazás port az a pont toosite IP-tartomány megakadályozza a gazdagépen. Alhálózatok gyakran meghaladó nyilvános hozzáférés szükséges.
* a célállomás nem működik
* az alkalmazás nem működik
* hello megfelelő IP-cím vagy állomásnév volt
* az alkalmazás a várttól mint egy másik portot figyeli. Ellenőrizheti ezt állapotra vált, hogy a gazdagép-kiszolgálóra, és használja a "netstat – aon" hello cmd parancssorból. Ez azt jelenti, melyik folyamat azonosítója melyik portot figyeli. 
* a hálózati biztonsági csoportok állíthatók be oly módon, hogy megakadályozzák, hogy a hozzáférés tooyour alkalmazás állomás és port az a pont toosite IP-címtartomány

Ne feledje, hogy milyen IP-nem ismeri a az alkalmazás által használt, ezért meg kell tooallow a hozzáférést a teljes tartomány hello pont tooSite IP-címtartományban. 

További hibakeresési lépések az alábbiak:

* Jelentkezzen be másik virtuális gép a virtuális hálózaton, és megpróbál tooreach található az erőforrás-állomás: port onnan. Nincsenek az egyes TCP ping segédprogramokat használhatja erre a célra, vagy még akkor is használhatja a telnet, ha szükséges. hello itt célja egyszerűen toodetermine kapcsolat nincs az adott virtuális Gépről. 
* egy másik virtuális Gépen lévő alkalmazás elindítani és hozzáférési toothat állomás és port hello konzolról a alkalmazás tesztelése

#### <a name="on-premises-resources"></a>A helyszíni erőforrások ####
Ha az alkalmazás nem érhető el a helyszíni erőforrások, hello először is ellenőrizni kell, ha érhető el erőforrás a Vneten belül. Ha biztosan működik, próbálja tooreach hello a helyszíni alkalmazások hello virtuális hálózatot a virtuális gép alapján. A telnet vagy a TCP ping segédprogramot használhatja. A virtuális Gépet a helyi erőforrás nem érhető el, majd győződjön meg arról, hogy működik-e a hely tooSite VPN-kapcsolatot. Ha működik, akkor ellenőrizze a hello, valamint hello helyszíni átjáró konfigurációs és állapotadatainak korábban feljegyzett ugyanazokat a műveleteket. 

Ha a virtuális hálózat most méretű képes elérni a helyszíni rendszer, de az alkalmazás nem tudja majd hello oka valószínűleg egy olyan hello következő:

* az útvonalak nincs konfigurálva vannak a pont toosite IP-címtartományai a helyszíni átjáró
* a hálózati biztonsági csoportok blokkolják a hozzáférést a pont tooSite IP-címtartomány
* a helyszíni tűzfalak blokkolják a forgalmat a pont tooSite IP-címtartomány
* a felhasználó definiált Route(UDR) vannak a virtuális hálózat, amely megakadályozza az a pont tooSite alapú forgalom elérje a helyszíni hálózatot

## <a name="hybrid-connections-and-app-service-environments"></a>Hibrid kapcsolatok és az App Service-környezetek
Nincsenek három szolgáltatást, amelyek lehetővé teszik az üzemeltetett tooVNet erőforrások eléréséhez. Ezek a következők:

* Virtuális integráció
* Hibrid kapcsolatok
* App Service-környezetek

Hibrid kapcsolatok tooinstall nevű hibrid kapcsolat Manager(HCM) hello a hálózaton lévő továbbító ügynök szükséges. hello HCM kell toobe képes tooconnect tooAzure, és tooyour alkalmazás. Ez a megoldás különösen nagy, például a helyi hálózat egy távoli hálózatról, vagy akár egy másik felhőben tárolt hálózati, mert nem szükséges az interneten elérhető végponton. hello HCM csak akkor fut, a Windows, illetve azt, hogy toofive példány tooprovide magas rendelkezésre állású fut be. Hibrid kapcsolatok csak az, ha TCP támogatja, és minden egyes HC végpont toomatch tooa adott állomás: port kombinációja. 

hello App Service Environment-környezet funkcióval toorun hello Azure App Service egy példánya a Vneten belül. Ez lehetővé teszi, hogy az alkalmazások az Azure-erőforrások bármely további lépések nélkül a virtuális hálózat. Néhány hello az App Service-környezetek egyéb előnyeit rendszer használható alapú Dv2 dolgozók mentése too14 GB RAM memóriával. Egy másik előnye, hogy méretezheti hello rendszer toomeet igényeinek. Hello több-bérlős környezetekben, ahol az ASP korlátozott too20 példányok, eltérően-környezetben költenie too100 ASP példányok. Egy mértékéig, amely a virtuális hálózat integrációja nem kap hello dolog, hogy az App Service-környezetek együttműködik az ExpressRoute VPN. 

Míg néhány eset átfedés használja, a szolgáltatás egyik lecserélheti hello bármelyikét mások. Milyen szolgáltatás toouse ismerete kapcsolt tooyour igényeinek. Példa:

* Ha a fejlesztők és egyszerűen szeretné, hogy toorun egy helyet az Azure-ban, és rendelkezik az elérni hello adatbázist hello munkaállomáson a saját számítógépéről, akkor hello legegyszerűbb dolog toouse hibrid kapcsolatok. 
* Ha olyan nagy szervezethez, amely a tooput szeretne nagyszámú webtulajdonaiban hello nyilvános felhőben, és kezelheti őket a saját hálózati, majd szeretné az App Service Environment-környezet hello toogo. 
* Ha az App Service számos üzemeltetett alkalmazások és egyszerűen szeretné, hogy a virtuális hálózat tooaccess erőforrásokat, majd virtuális integráció hello módon toogo. 

Hello használat esetén túl van néhány egyszerűség kapcsolatos szempontjai. Ha a virtuális hálózat már csatlakoztatva van tooyour a helyszíni hálózat, majd a virtuális integráció használatával, vagy az App Service-környezetek egyszerűen tooconsume a helyszíni erőforrások. A hello ugyanakkor, ha a virtuális hálózat nem tooyour a helyszíni hálózat csatlakozik, akkor célszerű további általános tooset be egy webhely toosite a virtuális hálózaton a VPN és a telepítés hello HCM összehasonlítása nagy. 

Túl hello funkcionális eltérések, nincs vannak is árképzési eltéréseket. hello App Service Environment-környezet szolgáltatása a prémium szintű szolgáltatásajánlat ajánlatok hello azonban a legtöbb hálózati konfigurációs lehetőségek továbbá tooother különleges szolgáltatásait. Virtuális integráció Standard vagy prémium ASP használható, és biztonságosan fogyassza az erőforrásokat a hello több-bérlős App Service a Vnetben tökéletes megoldás. Hibrid kapcsolatok jelenleg egy fiókot, amely start szabad, majd fokozatosan drágább beolvasása szintek alapján kell hello összeg BizTalk függ. Tooworking, ha sok hálózatokon keresztül származik, amikor nincs más szolgáltatás például a hibrid kapcsolatok, amelyek lehetővé teszik a tooaccess erőforrásokat is több mint 100, különálló hálózatokból. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://docs.microsoft.com/azure/app-service/app-service-environment/intro
[ILBASE]: http://docs.microsoft.com/azure/app-service/app-service-environment/create-ilb-ase
[V2VNETPortal]: https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal
[VPNERCoex]: http://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-coexist-resource-manager
[ASE]: http://docs.microsoft.com/azure/app-service/app-service-environment/intro
