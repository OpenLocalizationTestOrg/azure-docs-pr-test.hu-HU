---
title: "az App Service Environment-környezet v1 aaaHow tooConfigure"
description: "Konfigurációs felügyeleti és hello App Service Environment-környezet v1 monitorozása"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: f9539a72517276d8a1e340a408841561e8b8f56d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-an-app-service-environment-v1"></a>Alkalmazás konfigurálása szolgáltatás környezet 1-es verzió

> [!NOTE]
> Ez a cikk hello App Service Environment-környezet v1 tárgya.  App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve. több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Áttekintés
Magas szinten az Azure App Service-környezetek több fő részből áll:

* Az App Service Environment-környezet hello futó számítási erőforrásokat üzemeltetett szolgáltatás
* Storage
* Egy adatbázis
* Egy Classic(V1) vagy erőforrás Manager(V2) Azure virtuális hálózatot (VNet) 
* Hello App Service-környezetben üzemeltetett szolgáltatást futtató azt olyan alhálózatra

### <a name="compute-resources"></a>Számítási erőforrásokat
A négy erőforráskészletek hello számítási erőforrásokat használ.  Minden egyes App Service környezetben (ASE) rendelkezik egy előtér-webkiszolgálóinak és három lehetséges feldolgozókészletek. Nem kell toouse összes három feldolgozókészletek – Ha azt szeretné, használhatja egy vagy két.

hello gazdagépek hello erőforráskészletek (előtér-webkiszolgálóinak és alkalmazottak) nem érhető el közvetlenül tootenants. Nem használja a távoli asztal protokoll (RDP) tooconnect toothem, módosítsa a kiépítés vagy működjön, és a rendszergazda az őket.

Beállíthatja az erőforrás-készlet mennyiségétől és méretétől. Service-környezetben lehetősége van négy mérete, amelyek tartalma P1 P4 keresztül. Ezen méretét és tarifacsomagját kapcsolatos részletekért lásd: [App Service díjszabás](../app-service/app-service-value-prop-what-is.md).
Hello mennyiség vagy méretének módosítása a méretezési művelet neve.  Egyszerre csak egy skálázási művelet folyamatban lehet.

**Előtérkiszolgáló-végpontok**: hello előtér-webkiszolgálóinak olyan hello HTTP/HTTPS-végpontok az alkalmazások, amelyek a ASE vannak használatban. Nem lehet futtatni munkaterhelések hello az előtér-webkiszolgálóinak.

* Egy ASE kezdődik-e két P2s, amely is elegendő fejlesztési és tesztelési célú alkalmazásokat és az alacsony szintű termelési számítási feladatokhoz. Erősen ajánlott P3s mérsékelt tooheavy a termelési számítási feladatokhoz.
* Mérsékelt tooheavy a termelési számítási feladatokhoz azt javasoljuk, hogy rendelkezik-e legalább négy P3s tooensure futó, ütemezett karbantartás esetén elegendő előtér-webkiszolgálóinak van. Ütemezett karbantartás tevékenységek egyszerre egy előtér le fogja hozni. Ez csökkenti a teljes elérhető előtér-kapacitás karbantartási tevékenységek során.
* Előtér-webkiszolgálóinak tooan óra tooprovision is eltarthat. 
* A további méretezési pontosabb beállításra, célszerű figyelemmel kísérni hello Processzor, memória százalékos- és aktív kérelmek metrikák hello előtér-készlet. Ha hello CPU és memória százalékos 70 százalék felett P3s futtatásakor, vegyen fel több előtér-webkiszolgálóinak. Kimenő too15, 000 too20, 000 kérelmek előtér / hello aktív kérelmek érték átlaga szerepel, ha több előtér-webkiszolgálóinak is fel kell. hello általános célja tookeep CPU és memória százalékos alatt 70 % és átlagolási kimenő toobelow 15 000 kérelmek előtér / jelenik meg P3s aktív igénylések.  

**Feldolgozók**: hello munkavállalók, az alkalmazások ténylegesen futtatják. Ha vertikális felskálázás az App Service-csomagok, hogy másolatot hello dolgozók által használt társított munkavégző készletét.

* Azonnal munkavállalók vehető fel. Ezek tooan óra tooprovision igénybe vehet.
* A készlet számítási erőforrás mérete hello skálázás lépnek frissítés tartományonként < 1 óra. Nincsenek 20 frissítési tartományok Service-környezetben. Ha kiterjesztett hello számítási mérete 10 osztályt a feldolgozókészleten, eltarthat too10 óra toocomplete.
* A feldolgozókészleten használt hello számítási erőforrásokat hello méretének módosításához okoz az adott munkavégző készletét futó alkalmazások hello hidegindítások.

hello leggyorsabb toochange hello számítási erőforrás mérete a feldolgozókészletek nem minden alkalmazás futó módja:

* Csökkentheti a munkavállalók too2 hello mennyiségét.  hello minimális mérete hello portálon vertikális: 2. A példányok eltarthat néhány percig toodeallocate. 
* Válassza ki a hello új számítási méretét és példányok számát. Itt tart too2 óra toocomplete fel.

Ha az alkalmazások egy nagyobb számítási erőforrás mérete van szükség, akkor tudják kihasználni hello előző útmutatást. Hello munkavégző készletét, amelyen ezek az alkalmazások hello méretének módosítása, helyett egy másik munkavégző készletét. a szükséges hello méretű munkavállalók feltöltéséhez, és az alkalmazások átvitele toothat készlet.

* Hozzon létre több példányt hello hello szükséges számítási egy másik munkavégző készlet mérete. A rendszer tooan óra toocomplete tarthat.
* Az App Service-csomagokról, a nagyobb méretű újonnan konfigurált toohello feldolgozókészletek szükséges hello alkalmazások tároló rendelni. Ez az egy gyors műveletet hajtson végre kisebb, mint egy perc toocomplete.  
* Hello első feldolgozókészletek csökkentheti, ha azokat nem használt példányait többé nem szükséges. Ez a művelet néhány percet toocomplete vesz igénybe.

**Automatikus skálázás**: egy hello eszközök, melyek segíthetnek toomanage a számítási erőforrás-felhasználás az automatikus skálázást. Használhatja az automatikus skálázás előtér- vagy feldolgozókészletek. Végezhet például növelje az összes alkalmazáskészlet típusú példányok hello reggel a, és csökkenteni a hello este. Vagy esetleg adhat hozzá példányok, amikor a meghatározott küszöbérték alá csökken, a feldolgozókészleten az elérhető hello száma.

Ha szeretné, hogy tooset automatikus skálázási szabályok számítási erőforrás készlet metrikák körül, majd tartsa szem előtt tartva hello ideje, hogy a létesítési igényel. App Service Environment-környezetek automatikus skálázás kapcsolatos további tudnivalókért lásd: [hogyan tooconfigure automatikus skálázás egy App Service Environment-környezetben][ASEAutoscale].

### <a name="storage"></a>Storage
Minden egyes ASE 500 GB tárhelyet van konfigurálva. Ezen a helyen használt hello ASE az összes hello alkalmazások között. Ezt a tárolóhelyet hello ASE része, és jelenleg nem lehet bekapcsolva toouse a tárhelyre. Módosításának tooyour virtuális hálózati útválasztási vagy biztonsági hajt végre, ha szüksége van-e toostill hozzáférés tooAzure tárolási--engedélyezése vagy hello ASE nem működik.

### <a name="database"></a>Adatbázis
hello adatbázis hello információt, amely meghatározza a hello környezet, valamint belül futó alkalmazások hello hello adatait tárolja. Ez túl része egy hello Azure tárolt előfizetés. Nincs valamit, hogy rendelkezik-e a közvetlen képességét toomanipulate. Módosításának tooyour virtuális hálózati útválasztási vagy biztonsági hajt végre, ha szüksége van-e toostill hozzáférés tooSQL Azure--engedélyezése vagy hello ASE nem működik.

### <a name="network"></a>Network (Hálózat)
hello a mértékéig használt hálózatok egyike lehet hello ASE létrehozása után végrehajtott egy időben végrehajtott. Hello alhálózati ASE létrehozásakor létrehozásakor kényszerül hello ASE toobe a hello hello virtuális hálózatnak ugyanahhoz az erőforráscsoporthoz tartozik. Ha a ASE toobe eltér a virtuális hálózat által használt hello erőforráscsoport van szüksége, akkor szüksége toocreate a ASE resource manager-sablon használatával.

Nincsenek hello virtuális hálózat egy ASE használt bizonyos korlátozások vonatkoznak:

* hello virtuális hálózat regionális virtuális hálózatot kell lennie.
* Szükség van a toobe egy alhálózat 8 vagy több címekkel hello ASE telepítési helyét.
* Miután egy alhálózat használt toohost egy ASE, hello címtartomány hello alhálózat nem módosítható. Ezért azt javasoljuk, hogy hello alhálózat tartalmaz legalább 64 címek tooaccommodate ASE jövőbeli növekedésének.
* Lehet nincs más hello alhálózati, de hello ASE.

Hello üzemeltetett szolgáltatás, amely tartalmazza a hello ASE, eltérően hello [virtuális hálózati] [ virtualnetwork] és alhálózati felhasználói felügyelete alatt áll.  A virtuális hálózat hello virtuális hálózati felhasználói felületén vagy a Powershellen keresztül felügyelheti.  Egy ASE klasszikus és Resource Manager virtuális hálózat is telepíthető.  hello portál és API-élmény némileg eltérő klasszikus és Resource Manager Vnetek között, de hello ASE élmény van hello azonos.

virtuális hálózat, amely egy ASE használt toohost hello vagy saját RFC1918 IP-címek használhatók, vagy telepítheti a nyilvános IP-címeket.  Ha kívánja toouse egy IP-címtartományt, amely nem fedi RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) kell használnia toocreate a ASE ASE létrehozása előtt használja a VNet és alhálózat toobe.

Ez a funkció a virtuális hálózat az Azure App Service hello helyezi, mert az azt jelenti, hogy az alkalmazások, amelyek a ASE érhetők el, amelyeket közzétettek expressroute-on vagy a pont-pont virtuális magánhálózatok (VPN) keresztül közvetlenül erőforrások. az App Service-környezet belüli hello-alkalmazásokhoz nincs szükség további hálózati funkciókat tooaccess erőforrások elérhető toohello virtuális hálózat, amelyen az App Service-környezet. Ez azt jelenti, hogy nem kell toouse virtuális integráció vagy hibrid kapcsolatok tooget tooresources a vagy csatlakoztatott tooyour virtuális hálózat. Továbbra is használhatja ezeket a szolgáltatásokat is, ha tooaccess erőforrások hálózatokban, amelyek nem tooyour virtuális hálózathoz csatlakozik.

Például az előfizetésben, de nem csatlakoztatott toohello virtuális hálózatot, amely a ASE a virtuális hálózat virtuális integráció toointegrate is használhatja. Hibrid kapcsolatok tooaccess-erőforrást más hálózatokhoz, ahogy normális esetben is továbbra is használhatja.  

Ha a virtuális hálózatot az ExpressRoute VPN rendelkezik, néhány hello útválasztási igényeinek, amely rendelkezik egy ASE tudomást kell lennie. Néhány felhasználó által megadott útvonal (UDR) konfigurációkat, amelyek nem kompatibilisek egy mértékéig van. A virtuális hálózatban az ExpressRoute egy ASE futtatásával kapcsolatos további részletekért lásd: [ExpressRoute rendelkező virtuális hálózatban egy App Service Environment-környezet futó][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Bejövő adatforgalom biztonságossá tétele
Nincsenek a két elsődleges módszere toocontrol bejövő forgalom tooyour ASE.  Használhatja hálózati biztonsági Groups(NSGs) toocontrol milyen IP-címek hozzáférhet a ASE Itt [hogyan toocontrol bejövő forgalmat egy App Service Environment-környezetben](app-service-app-service-environment-control-inbound-traffic.md) és is konfigurálható a ASE belső Balancer(ILB).  Ezek a funkciók is együtt is használható, ha azt szeretné, hogy az NSG-k tooyour ILB ASE toorestrict-hozzáférés.

Amikor létrehoz egy ASE, VIP hoz létre a Vneten belül.  Két VIP típusa, a külső és belső van.  Amikor létrehoz egy ASE egy külső virtuális IP-címre a ASE alkalmazások internet irányítható IP-cím elérhetők lesznek. Ha bejelöli a ASE belső egy ILB van konfigurálva, és nem lesz közvetlenül az interneten érhető el.  Egy ILB ASE továbbra is igényel egy külső virtuális IP, de csak az Azure felügyeleti és a karbantartás használja.  

ILB ASE létrehozásakor adja meg a hello altartomány hello ILB ASE által használt, és lesz toomanage hello altartomány megadhatja a saját DNS.  Mivel hello altartománynév beállított is a HTTPS-hozzáféréshez használt toomanage hello tanúsítványra van szükség.  ASE után az Ön létrehozását kéri a tooprovide hello tanúsítvány.  További információ az létrehozása és használata egy ILB ASE toolearn olvassa el [egy belső terheléselosztó használata az App Service-környezetek][ILBASE]. 

## <a name="portal"></a>Portál
Ön felügyelheti és figyelheti az App Service-környezet hello felhasználói felület a hello Azure-portál használatával. Ha egy ASE, esetén a rendszer valószínűleg toosee hello App Service szimbólum az oldalsávon. Ez a szimbólum hello Azure-portálon használt toorepresent App Service-környezetek:

![App Service Environment-környezet szimbólum][1]

tooopen hello felhasználói felület, amely felsorolja az App Service Environment-környezetek, hello ikon vagy select hello sávnyíl (">" jel) hello oldalsávon tooselect App Service Environment-környezetek hello alján. Felsorolt hello ASEs közül válassza ki, nyissa meg a felhasználói felület, amely használt toomonitor hello, és kezelheti.

![Felhasználói felülete figyelése és az App Service-környezet kezelése][2]

Az első panel a ASE néhány tulajdonságát együtt erőforrás készletenként metrika diagram jeleníti meg. Hello tulajdonságok hello látható **Essentials** blokk megtalálhatók hiperhivatkozásokat, ekkor megnyílik a hello panel, amely társítva hozzá. Például kiválaszthatja a hello **virtuális hálózati** neve tooopen be felhasználói felület hello társított hello virtuális hálózat, amely a ASE futtatja. **App Service-csomagok** és **alkalmazások** panelt, amely ezeket az elemeket, amelyek a ASE a listában minden egyes nyissa meg.  

### <a name="monitoring"></a>Figyelés
hello diagramok lehetővé teszik a számos különböző Teljesítményelemzési mutatón minden erőforráskészlet toosee. Hello előtér-készlet figyelheti a hello átlagos CPU és memória. A feldolgozókészletek figyelheti hello használt és hello mennyiséggel elérhető.

Több App Service csomagokban teheti a feldolgozókészleten hello munkavállalók használja. hello munkaterhelés nem elosztott hello azonos módon, mint a hello előtér-kiszolgálók, így hello Processzor- és memóriahasználatról nem ad meg, sok hasznos információk hello módon. Fontosabb tootrack hány munkavállalók használt és elérhető – különösen akkor, ha a rendszer Ön által felügyelt mások toouse.  

Használhatja az összes hello metrikák hello diagramok tooset figyelmeztetéseket követhető nyomon. Beállítása itt riasztások works hello ugyanaz, mint máshol az App Service-ben. Vagy hello között állítható be egy riasztás **riasztások** része, vagy állapotkategóriák vizsgálatát az összes metrikák felhasználói felületén, majd válassza a felhasználói felület **hozzáadása riasztás**.

![Felhasználói felület metrikák][3]

csak tárgyalt hello metrikák hello App Service Environment-környezet mérőszámok. Van még ítélt hello App Service-csomag szint érhető el. Ez azért, ahol figyelési CPU és memória lehetővé teszi a nagy mennyiségű logika.

-Környezetben hello App Service-csomagok összes dedikált App Service-csomagokról. Amely azt jelenti, hogy hello csak alkalmazások hello lefoglalt állomások toothat App Service-csomag a futó alkalmazások hello az adott App Service-csomag. az App Service-csomag toosee részleteinek bármelyik hello listák hello ASE felhasználói felületén vagy a elindítani az App Service-csomag **Tallózás App Service-csomagok** (amely tartalmazza az összes).   

### <a name="settings"></a>Beállítások
Hello ASE panelen belül egy **beállítások** szakasz néhány fontos képességeket tartalmazza:

**Beállítások** > **tulajdonságok**: hello **beállítások** panel automatikusan nyitja meg elindítani a ASE panelen. Hello felső van **tulajdonságok**. Itt, amelyek redundáns toowhat látható elemek több **Essentials**, de mi nagyon hasznos lehet az **virtuális IP-cím**, valamint **kimenő IP-címek**.

![Beállítások panel és tulajdonságai][4]

**Beállítások** > **IP-címek**: a ASE IP Secure Sockets Layer (SSL) alkalmazást hoz létre, ha szüksége van-e az IP SSL-címet. Rendelés tooobtain egy a ASE IP SSL címek saját felosztható van szüksége. Egy ASE jön létre, amikor erre a célra egy IP SSL-címmel rendelkezik, de több adhat hozzá. Nincs díj további IP-SSL-címek, ahogy az [App Service díjszabás] [ AppServicePricing] (az SSL-kapcsolatok a hello szakaszban). hello további ár hello IP SSL ár.

**Beállítások** > **első készlet** / **Feldolgozókészletek**: hello képességét toosee adatokat csak az adott erőforráskészletben kínál a erőforrás készlet paneleken mindegyikének , továbbá tooproviding szabályozza toofully méretezési adott erőforráskészlethez.  

hello alap panel minden erőforráskészlet metrikákat a diagramot biztosít adott erőforráskészlethez. Csakúgy, mint hello diagramok hello ASE paneljéről megnyithatja hello diagramba, és állítsa be a kívánt riasztásokat. Riasztás beállítása adott erőforráskészlet hello ASE paneljéről hello ugyanaz, mint hello erőforráskészlet végezze el. A hello feldolgozókészletek **beállítások** panelen hozzáférés tooall hello alkalmazások vannak, vagy az App Service-csomagok, amelyek a feldolgozókészleten.

![Munkavégző Készletbeállítások felhasználói felület][5]

### <a name="portal-scale-capabilities"></a>Portál méretezési képességek
Nincsenek három a skálázási műveletek:

* IP-címek hello ASE elérhető IP-SSL használatának hello számának módosítása.
* Hello számítási erőforrás használja erőforráskészlet a hello méretének módosítása.
* Az erőforráskészlet, manuális vagy automatikus skálázás használt számítási erőforrásokat hello számának módosítása.

Hello portálon van három módon toocontrol kiszolgálók számát, amely szerepel az erőforrás-készletek:

* A skálázási művelet hello fő ASE paneljén hello tetején. Biztosíthatja, hogy több skálázási konfiguráció toohello előtér- és feldolgozókészletek módosul. Az összes lépésük egyetlen műveletként.
* A manuális skálázási művelet hello egyedi erőforráskészlet **méretezési** panel, amely alatt **beállítások**.
* Egyes erőforráskészlet hello beállítása automatikus skálázás **méretezési** panelen.

toouse hello skálázási művelet hello ASE paneljén húzza hello csúszkát toohello mennyiség szeretne, és mentse. Ez a felhasználói felület is támogatja a változó hello méretét.  

![Skála felhasználói felület][6]

toouse hello manuális vagy automatikus skálázás képességei adott erőforráskészlethez, nyissa meg túl**beállítások** > **első készlet** / **Feldolgozókészletek** , a megfelelő. Majd nyissa meg hello készlet, amelyet az toochange. Nyissa meg túl**beállítások** > **horizontális Felskálázás** vagy **beállítások** > **vertikális Felskálázás**. Hello **horizontális Felskálázás** panel lehetővé teszi a toocontrol példány mennyiség. **Vertikális Felskálázás** lehetővé teszi a toocontrol erőforrás mérete.  

![Felhasználói felület skálázási beállításokat][7]

## <a name="fault-tolerance-considerations"></a>Hibatűrési szempontok
Az App Service Environment-környezet toouse too55 teljes számítási erőforrások konfigurálhatja. 55 számítási erőforrások csak 50 lehet használt toohost munkaterhelések. hello ennek oka kétszeres. Nincs legalább 2 előtér-számítási erőforrásokat.  Amely hagyja, mentése too53 toosupport hello worker-verem lefoglalásának kéréséhez. Rendelés tooprovide hibatűrést kell toohave egy további számítási erőforrás lefoglalt toohello a következő szabályok szerint:

* Minden egyes feldolgozókészletek legalább 1 további számítási erőforrás, amely nincs hozzárendelve a munkaterhelés elérhető toobe kell.
* A feldolgozókészleten a számítási erőforrásokat hello mennyisége a megadott érték fölé megy, majd egy másik számítási erőforrás szükség a hibatűrés érdekében. Ez helyzet nem hello hello előtér-készletben.

Bármely egyetlen feldolgozókészletek belül hello hibatűrést követelményei, hogy egy adott értéket az x erőforrások tooa feldolgozókészletek hozzárendelve:

* Ha X 2 és 20 közötti, hello használható számítási erőforrásokat, amelyek a munkaterhelések mérete X-1.
* Ha X 21 és 40 közötti, hello használható számítási erőforrásokat, amelyek a munkaterhelések mérete X-2.
* Ha X 41-es és 53 között, hello használható számítási erőforrásokat, amelyek a munkaterhelések mérete X-3.

hello minimális erőforrásigényét 2 előtér- és 2 feldolgozónak van.  A fenti utasítások majd hello Íme néhány példa tooclarify:  

* Ha 30 munkavállalók egyetlen készletben, azok 28 használt toohost munkaterhelések lehet.
* Ha egyetlen készletben 2 feldolgozónak, 1 használt toohost munkaterhelések lehet.
* Ha 20 munkavállalók egyetlen készletben, 19 használt toohost munkaterhelések lehet.  
* Ha 21 munkavállalók egyetlen tárolókészlet, majd még csak az 19 használt toohost munkaterhelések.  

hello hibatűrést aspektus fontos, de van szüksége, vegye figyelembe, hogy bizonyos küszöbérték feletti méretezése tookeep. Ha azt szeretné, tooadd kapacitás 20 példányokat, és lépjen too22 át, vagy magasabb mert 21 nem adja hozzá a további kapacitást. hello is igaz, 40, fent is, ahol hello következő kapacitás hozzáadó értéke 42.  

## <a name="deleting-an-app-service-environment"></a>App Service-környezet törlése
Ha azt szeretné, hogy az App Service-környezetek toodelete, egyszerűen használja hello **törlése** művelet hello App Service Environment-környezet panel hello tetején. Ha így tesz, az App Service Environment-környezet tooconfirm, hogy valóban szeretné toodo ez felszólító tooenter hello neve lesz. Vegye figyelembe, hogy ha töröl egy App Service Environment-környezet, akkor törölje az összes hello tartalmakra azt is.  

![Felhasználói felület App Service-környezet törlése][9]  

## <a name="getting-started"></a>Bevezetés
Lásd az App Service-környezetek lépései tooget [hogyan toocreate az App Service-környezetek](app-service-web-how-to-create-an-app-service-environment.md).

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service](../app-service/app-service-value-prop-what-is.md).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
