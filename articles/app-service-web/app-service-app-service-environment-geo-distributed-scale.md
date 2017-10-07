---
title: "az App Service Environment-környezetek elosztott méretezési aaaGeo"
description: "Ismerje meg, hogyan toohorizontally skálázása a Traffic Manager és az App Service Environment-környezetek földrajzi terjesztési használó alkalmazásokat."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.openlocfilehash: 9b441f637d8b7f679b3d83240baf99b8ee57e8f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>Földrajzi alapú méretezés App Service-környezetekkel
## <a name="overview"></a>Áttekintés
Nagyon nagy méretű igénylő alkalmazás-forgatókönyvekre azért lépheti túl a hello számítási erőforrások kapacitás elérhető tooa egyetlen telepítése egy alkalmazás.  Szavazás alkalmazások, ilyen események és közvetített Szórakozás események példák minden forgatókönyv esetén van szükség a rendkívül nagy méretű. Nagy méretű követelmények által vízszintesen kiterjesztése alkalmazások, egyetlen régión belül, valamint a különböző régiókban, toohandle szélsőséges Adatbetöltési követelményeinek végrehajtott több alkalmazás központi telepítéssel rendelkező érheti el.

App Service-környezetek vízszintes kibővítési ideális platform.  Amennyiben az App Service Environment-környezet konfigurálása során választott ki, amely támogathatja az egy ismert kérelmek aránya, fejlesztők telepíthet további App Service Environment-környezetek a "cookie-k vágó" módon tooattain kívánt csúcs terhelés kapacitású.

Tegyük fel például egy alkalmazást az App Service Environment-környezet konfigurációjának futó lett tesztelt toohandle 20K kérelmek / másodperc (RPS).  Ha hello kívánt csúcs betöltése kapacitás 100K RPS, majd öt (5) App Service-környezetek hozhatók létre és konfigurált tooensure hello alkalmazás kezeli a hello maximális tervezett terhelés.

Mivel az ügyfelek általában egy egyéni (vagy személyes) tartomány, a fejlesztők számára szükséges alkalmazások hozzáférhetnek a módon toodistribute alkalmazások keresztül hello App Service Environment-környezet példányok mindegyikének kéri.  Egy ez remek mód tooaccomplish tooresolve hello egyéni tartományt használ egy [Azure Traffic Manager-profil][AzureTrafficManagerProfile].  a Traffic Manager-profil lehet az összes konfigurált toopoint hello hello egyedi App Service Environment-környezetek.  A TRAFFIC Manager automatikusan osztja el az ügyfelek között összes hello App Service Environment-környezetek alapján hello terheléselosztási hello Traffic Manager-profil beállításait fogja kezelni.  Ez a megközelítés működik, függetlenül attól, hogy összes hello App Service Environment-környezetek telepítése egy Azure-régió, vagy több Azure-régiók közötti világszerte telepített.

Ezenkívül ügyfelek alkalmazások hozzáférhetnek a hello kreatív tartomány keresztül, mert az ügyfelek nem tudnak a futó alkalmazások App Service-környezetek hello száma.  Ennek eredményeképpen a fejlesztők is gyorsan és egyszerűen hozzáadása és eltávolítása, App Service Environment-környezetek megfigyelt adatforgalom jelentette teher alapján.

hello fogalmi az alábbi ábrán az alkalmazások között három App Service Environment-környezetek egyetlen régión belül vízszintesen horizontálisan ábrázol.

![Fogalmi architektúra][ConceptualArchitecture] 

Ez a témakör további része hello végigvezeti hello lépéseit egy elosztott topológia több App Service Environment-környezetek használatával hello mintaalkalmazás beállításához.

## <a name="planning-hello-topology"></a>Hello topológia tervezése
Egy elosztott alkalmazás erőforrásigényét létrehozására, mielőtt segít toohave néhány adatra információk időben.

* **Hello app tartozó egyéni tartomány:** Újdonságok hello egyéni tartománynevet, hogy az ügyfelek tooaccess hello alkalmazás fogja használni?  Hello sample app hello egyéni tartomány nevét az *www.scalableasedemo.com*
* **Traffic Manager-tartományra:** egy tartománynevet kell létrehozásakor kiválasztott toobe egy [Azure Traffic Manager-profil][AzureTrafficManagerProfile].  Ez a név a rendszer kombinálja hello *trafficmanager.net* utótag tooregister egy Traffic Manager által kezelt tartomány bejegyzést.  Hello mintaalkalmazást, hello neve választott pedig *méretezhető ase bemutató*.  Ennek eredményeképpen a Traffic Manager által kezelt hello teljes tartománynév rendszer *méretezhető ase demo.trafficmanager.net*.
* **Hello app erőforrásigényét méretezéshez stratégia:** hello alkalmazás erőforrásigényét sor kerül egy régió több App Service környezetek között?  Több régióba?  Vegyes-és-egyezés a mindkét megközelítés?  hello döntéshez az azzal kapcsolatos elvárások, ahol felhasználói forgalom állapottesztjei származnak, valamint, hogy egy alkalmazást támogató háttér-infrastruktúra jól hello részeinek méretezheti.  Például a 100 %-os állapot nélküli alkalmazások, az alkalmazások is nagymértékben méretezhető több App Service Environment-környezetek kombinációját használva az Azure-régió, és a több Azure-régiók telepített App Service-környezetek /.  15 + nyilvános Azure-régiók elérhető toochoose származó, az ügyfelek valóban egy világszerte kapacitású alkalmazás erőforrásigényét hozhat létre.  Az ebben a cikkben használt hello mintaalkalmazás három App Service Environment-környezetek (déli középső Régiójában) egy Azure régió jöttek létre.
* **App Service Environment-környezetek hello elnevezési konvenció:** minden App Service Environment-környezet megköveteli egy egyedi nevet.  Egy vagy két App Service Environment-környezetek túl hasznos toohave elnevezési toohelp azonosítása minden App Service Environment-környezet.  Hello mintaalkalmazás egyszerű elnevezési lett megadva.  hello hello három App Service Environment-környezetek nevei vannak *fe1ase*, *fe2ase*, és *fe3ase*.
* **Hello alkalmazások elnevezési konvenció:** hello alkalmazás több példányát telepíti, mert a neve hello telepített alkalmazás egyes példányainak szükséges.  App Service Environment-környezetek egy kis ismert azonban nagyon hasznos funkció, hogy hello azonos alkalmazásnév használható több App Service Environment-környezetek között.  Mivel minden egyes App Service Environment-környezet csak egy egyedi tartományutótagot, a felhasználók kiválaszthatják toore használható hello pontos azonos alkalmazásnév minden környezetben.  Egy fejlesztő például rendelkezhetnek elnevezése a következő alkalmazásokat: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*stb.  Hello mintaalkalmazást, ha minden app-példány is rendelkezik egy egyedi nevet.  hello használt alkalmazás példánynevek vannak *webfrontend1*, *webfrontend2*, és *webfrontend3*.

## <a name="setting-up-hello-traffic-manager-profile"></a>Hello Traffic Manager-profil beállítása
Ha egy alkalmazás több példánya több App Service Environment-környezetek vannak telepítve, a hello az egyes alkalmazásokra példányok a Traffic Managerrel regisztrálható.  Hello mintaalkalmazást a Traffic Manager profil van szükség a *méretezhető ase demo.trafficmanager.net* irányíthatja a ügyfelek telepítése a következő hello tooany app-példányok:

* **webfrontend1.fe1ase.p.azurewebsites.NET:** hello mintaalkalmazás telepített példánya hello első App Service Environment-környezet.
* **webfrontend2.fe2ase.p.azurewebsites.NET:** hello mintaalkalmazás telepített példánya hello második App Service Environment-környezet.
* **webfrontend3.fe3ase.p.azurewebsites.NET:** hello mintaalkalmazás telepített példánya hello harmadik App Service Environment-környezet.

hello legegyszerűbb módja tooregister több Azure App Service végpontot, hello az összes futó **azonos** Azure-régió, az a hello Powershell [Azure Resource Manager Traffic Manager-támogatás] [ ARMTrafficManager].  

első lépés hello toocreate az Azure Traffic Manager-profil.  hello az alábbi kód bemutatja, hogyan hello mintaalkalmazás hello-profil lett létrehozva:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Figyelje meg, hogyan hello *RelativeDnsName* paraméter túl lett beállítva*méretezhető ase bemutató*.  A rendszer hogyan hello tartománynév *méretezhető ase demo.trafficmanager.net* létrejön, és a Traffic Manager-profil társított.

Hello *TrafficRoutingMethod* paraméter határozza meg a hello terheléselosztási házirend Traffic Manager hogyan toospread ügyfél betöltése hello elérhető végpontok közötti toodetermine fogja használni.  Az ebben a példában hello *Weighted* módszer választása.  Ennek eredményeképpen az ügyfelek kéréseire alatt elosztva hello regisztrált alkalmazás végpontok hello minden végponthoz társított relatív súlyok alapján. 

A létrehozott hello-profil minden alkalmazáspéldányban meg van adva toohello profil natív Azure-végpont.  hello kódot kér le hivatkozást tooeach előtérbeli webes alkalmazás, és hozzáadja minden alkalmazás hello vállalja a Traffic Manager-végpontként *targetresourceid azonosítója* paraméter.

    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

Figyelje meg, hogyan van egy hívás túl*Add-AzureTrafficManagerEndpointConfig* minden egyes alkalmazás-példányhoz.  Hello *targetresourceid azonosítója* paraméter minden Powershell-parancsot a hello három telepített alkalmazás példányok egyik hivatkozik.  hello Traffic Manager-profil betöltése elosztva három végpontjai hello-profil regisztrálva.

Az összes hello három végpontok használja ugyanazt az értéket (10) hello hello *súly* paraméter.  Az eredmény elterjedésének Traffic Manager-ügyfelek kéréseire minden három app példányára viszonylag egyenletes. 

## <a name="pointing-hello-apps-custom-domain-at-hello-traffic-manager-domain"></a>Hello App egyéni tartomány: hello Traffic Manager-tartományra mutat
hello utolsó lépés szükséges toopoint hello egyéni tartomány hello alkalmazás hello Traffic Manager tartományban.  Hello mintaalkalmazás esetében ez azt jelenti, mutató *www.scalableasedemo.com* : *méretezhető ase demo.trafficmanager.net*.  Ez a lépés befejeződött, de kezeli az egyéni tartomány hello hello tartományregisztráló toobe kell.  

A regisztráló tartományi felügyeleti eszközökkel, egy CNAME rekordot kell toobe mely pontok hello az egyéni tartomány létrehozása: hello Traffic Manager-tartományra.  az alábbi képen hello a CNAME-konfiguráció néz példáját mutatja be:

![Egyéni tartomány CNAME][CNAMEforCustomDomain] 

Bár nem ebben a témakörben ismertetett, ne feledje, hogy minden egyes alkalmazás ezen példányát kell toohave hello egyéni tartomány regisztrál vele, valamint.  Ellenkező esetben ha kérelmet teszi tooan app-példány, és hello alkalmazás nem rendelkezik hello hello app regisztrált egyéni tartományt, hello kérelme sikertelen lesz.  

Ez a példa hello az egyéni tartomány van *www.scalableasedemo.com*, és minden alkalmazáspéldány hello egyéni tartomány társítva van.

![Egyéni tartomány][CustomDomain] 

Egy összefoglalása az Azure App Service-alkalmazásokhoz történő regisztrálásának egyéni tartományt, tekintse meg következő cikket hello [egyéni tartományok regisztrálása][RegisterCustomDomain].

## <a name="trying-out-hello-distributed-topology"></a>Próbálhatja ki hello Elosztott topológia
hello end hello Traffic Manager és a DNS-konfiguráció eredménye, hogy vonatkozó kérések *www.scalableasedemo.com* halad keresztül a következő feladatütemezési hello:

1. Egy böngésző vagy eszköz DNS-címkeresést tesz *www.scalableasedemo.com*
2. hello hello tartományregisztráló CNAME bejegyzése hello DNS keresési átirányítva toobe tooAzure Traffic Manager okoz.
3. DNS-címkeresést történik *méretezhető ase demo.trafficmanager.net* elleni hello Azure Traffic Manager DNS-kiszolgálók egyikét.
4. Hello terheléselosztási házirend alapján (hello *TrafficRoutingMethod* hello Traffic Manager-profil létrehozásakor korábban használt paraméter), jelölje ki a hello végpontok konfigurálva, és térjen vissza a Traffic Manager lesz hello, amelyek teljes Tartományneve végpont toohello böngésző vagy eszköz.
5. Mivel hello hello végpont teljes Tartományneve az App Service-környezetek futó alkalmazás példánya hello URL-címét, hello böngésző vagy eszköz felhasználói jóváhagyást kér a Microsoft Azure DNS-kiszolgáló tooresolve hello FQDN tooan IP-címet. 
6. hello böngésző vagy eszköz küldi hello HTTP/S kérelem toohello IP-címet.  
7. hello kérelem hello app példányok hello App Service Environment-környezetek egyikén futó egyikén fog megérkezni.

hello konzol a következő ábrán egy DNS-címkeresés hello sample app egyéni tartomány sikeresen feloldó tooan app példány hello három minta App Service Environment-környezetek egyikén futó (ebben az esetben hello második hello három App Service Environment-környezetek):

![DNS-címkeresés][DNSLookup] 

## <a name="additional-links-and-information"></a>További hivatkozások és információk
Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Hello Powershell dokumentációja [Azure Resource Manager Traffic Manager-támogatás][ARMTrafficManager].  

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: ../traffic-manager/traffic-manager-manage-profiles.md
[ARMTrafficManager]: ../traffic-manager/traffic-manager-powershell-arm.md
[RegisterCustomDomain]: app-service-web-tutorial-custom-domain.md


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
