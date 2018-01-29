---
title: "Belső terheléselosztó létrehozása és használata Azure App Service Environmenttel"
description: "Részletes információk az internettől elzárt Azure App Service Environment létrehozásáról"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/13/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 9f7343102cf7af6d7f2ba6b4b2f08b7b855da6f8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/04/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Belső terheléselosztó létrehozása és használata App Service Environmenttel #

 Az Azure App Service Environment az Azure App Service egy olyan példánya, amelyet egy Azure virtuális hálózat alhálózatában helyeztek üzembe. Az App Service Environment (ASE) üzembe helyezésének két módja van: 

- Egy virtuális IP-cím vagy külső IP-cím, azaz külső ASE használatával.
- Egy virtuális IP-cím vagy belső IP-cím, azaz ILB ASE használatával, amelyet azért neveznek így, mert a belső végpont egy belső terheléselosztó (ILB). 

Ebből a cikkből megtudhatja, hogyan hozhat létre egy ILB ASE környezetet. Az ASE áttekintését [az App Service Environmentet bemutató][Intro] témakör tartalmazza. A külső ASE létrehozásának folyamatát a [külső ASE létrehozását][MakeExternalASE] ismertető témakor tartalmazza.

## <a name="overview"></a>Áttekintés ##

Az ASE környezetet közvetlenül az internethez csatlakozó végponttal vagy a virtuális hálózatán belüli IP-címmel is telepítheti. Ahhoz, hogy egy IP-címet hozzárendeljen egy virtuális hálózati címhez, az ASE környezetet ILB terheléskiegyenlítővel kell üzembe helyezni. Az ASE ILB-vel történő üzembe helyezéséhez az alábbiak szükségesek:

-   Saját tartomány, amelyet az alkalmazások létrehozásakor használ.
-   A HTTPS-hez használt tanúsítvány.
-   Tartománya DNS-felügyelete.

Cserébe többek között az alábbiakat teheti meg:

-   Intranet-alkalmazásokat üzemeltethet biztonságosan a felhőben, amelyeket helyek közötti vagy Azure ExpressRoute VPN használatával érhet el.
-   A nyilvános DNS-kiszolgálókon nem szereplő alkalmazásokat üzemeltethet a felhőben.
-   Internettől elzárt háttéralkalmazásokat hozhat létre, amelyekkel az előtéri alkalmazások biztonságosan integrálódhatnak.

### <a name="disabled-functionality"></a>Letiltott funkciók ###

Néhány dolog, amit nem tehet meg ILB ASE használata esetén:

-   IP-alapú SSL használata.
-   IP-címek hozzárendelése az egyes alkalmazásokhoz.
-   Tanúsítvány vásárlása és használata egy alkalmazáshoz az Azure Portalon keresztül. A tanúsítványokat közvetlenül hitelesítésszolgáltatótól szerezheti be, és azokat használhatja az alkalmazásaihoz. Az Azure Portalon keresztül nem szerezheti be őket.

## <a name="create-an-ilb-ase"></a>ILB ASE létrehozása ##

Az ILB ASE létrehozása:

1. Az Azure Portalon válassza az **Új** > **Web + Mobil** > **App Service Environment** elemet.

2. Válassza ki előfizetését.

3. Válasszon ki vagy hozzon létre egy erőforráscsoportot.

4. Válasszon ki vagy hozzon létre egy virtuális hálózatot.

5. Ha már létező virtuális hálózatot választ, létre kell hoznia egy alhálózatot az ASE környezethez. Gondoskodjon arról, hogy az alhálózat mérete elég nagy legyen az ASE jövőbeli növekedésének biztosítására. Mi a `/25` méretet javasoljuk, amely 128 címet tartalmaz, és képes kezelni egy maximális méretű ASE környezetet. A `/28` a kiválasztható legkisebb méret. Az infrastrukturális igényeket figyelembe véve ez a méret legfeljebb 11 példányra skálázható fel.

    * Az alapértelmezett maximális 100 példányszámot az App Service-csomagban emelheti meg.

    * Skálázhat 100 példány körüli méretre, de gyorsabb előtéri skálázással.

6. Válassza a **Virtuális hálózat/hely** > **Virtuális hálózati konfiguráció** elemet. A **VIP típust** állítsa **Belsőre**.

7. Adjon meg egy tartománynevet. Ezt a tartományt használják az ASE környezetben létrehozott alkalmazások. Van néhány korlátozás. A név nem lehet:

    * net   

    * azurewebsites.net

    * p.azurewebsites.net

    * &lt;ase neve&gt;.p.azurewebsites.net

   Az alkalmazásokhoz használt egyéni tartománynév és az ASE által használt egyéni tartománynév nem egyezhet meg. ILB ASE esetében a _contoso.com_ tartománynév használata esetén az alábbi egyéni tartományneveket nem használhatja alkalmazásaihoz:

    * www.contoso.com

    * abcd.def.contoso.com

    * abcd.contoso.com

   Ha ismeri alkalmazásai egyéni tartományneveit, válasszon olyan tartományt az ILB ASE környezetnek, amely nem ütközik az egyéni tartománynevekkel. Ebben a példában használhatja a *contoso-internal.com* nevet az ASE tartományaként, mert az nem ütközik *.contoso.com* végződésű tartománynevekkel.

8. Kattintson az **OK**, majd a **Létrehozás** gombra.

    ![ASE létrehozása][1]

A **Virtuális hálózat** panelen található a **Virtuális hálózat konfigurációja**. Használatával kiválaszthat egy külső vagy belső virtuális IP-címet. Az alapértelmezett érték a **Külső**. Ha a **Külső** lehetőséget választja, az ASE közvetlenül az internethez csatlakozó virtuális IP-címet fog használni. Ha a **Belső** lehetőséget választja, az ASE ILB-vel vagy virtuális hálózati IP-címmel lesz konfigurálva.

A **Belső** lehetőség kiválasztását követően további IP-címek nem adhatók hozzá az ASE környezethez. Ehelyett tartományt kell biztosítania az ASE részére. Külső virtuális IP-címmel rendelkező ASE esetén az ASE környezeten belül létrehozott alkalmazások az ASE nevét használják tartományként.

Ha a **VIP típusa** beállítást **Belsőre** állítja, akkor nem az ASE neve lesz az ASE környezet tartománya. Külön megadhatja a tartományt. Ha a tartomány *contoso.corp.net* és az ASE környezetben létrehoz egy *timereporting* nevű alkalmazást, akkor az alkalmazás URL-címe timereporting.contoso.corp.net lesz.


## <a name="create-an-app-in-an-ilb-ase"></a>Alkalmazás létrehozása az ILB ASE környezetben ##

Az ILB ASE környezetben a sima ASE környezettel megegyező módon hozhat létre alkalmazást.

1. Az Azure Portalon válassza az **Új** > **Web + Mobil** > **Web** vagy **Mobil** vagy **API-alkalmazás** lehetőséget.

2. Adja meg az alkalmazás nevét.

3. Válassza ki az előfizetést.

4. Válasszon ki vagy hozzon létre egy erőforráscsoportot.

5. Válassza ki vagy hozzon létre egy App Service-csomagot. Amennyiben új App Service-csomagot szeretne létrehozni, válassza az ASE környezetet helyként. Válassza ki a feldolgozókészletet, amelyben az App Service-csomagot szeretné létrehozni. Amikor létrehozza az új App Service-csomagot, válassza az ASE környezetet helyként és feldolgozókészletként. Az alkalmazás nevének megadásakor az alkalmazás tartománya az ASE tartománynevére cserélődik.

6. Kattintson a **Létrehozás** gombra. Ha szeretné, hogy az alkalmazás megjelenjen az irányítópulton, jelölje be a **Rögzítés az irányítópulton** jelölőnégyzetet.

    ![App Service-csomag létrehozása][2]

    Az **Alkalmazásnév** területen a tartománynév frissült az ASE tartományának megfelelően.

## <a name="post-ilb-ase-creation-validation"></a>Érvényesítés az ILB ASE létrehozása után ##

Az ILB ASE kissé különbözik az ILB nélküli ASE környezettől. A fenti információknak megfelelően Önnek kell kezelnie a saját DNS-ét. A HTTPS-csatlakozáshoz is saját tanúsítványt kell biztosítania.

Az ASE létrehozását követően a tartomány az Ön által megadott tartománynevet jeleníti meg. A **Beállítás** menüben egy **ILB-tanúsítvány** nevű új elem jelenik meg. Az ASE olyan tanúsítvánnyal jött létre, amely nem határozza meg az ILB ASE tartományát. Amennyiben ezzel a tanúsítvánnyal használja az ASE környezetet, a böngészője érvénytelennek fogja nyilvánítani. A tanúsítvány egyszerűbbé teszi a HTTPS tesztelését, de fel kell töltenie saját, az ILB ASE környezethez kötött rögzített tanúsítványát. Ez a lépés szükséges, függetlenül attól, hogy a tanúsítvány önaláírt vagy hitelesítésszolgáltatótól származik.

![ILB ASE tartománynév][3]

Az ILB ASE környezetnek szüksége van egy érvényes SSL-tanúsítványra. Használjon belső tanúsítványszolgáltatót, vásároljon tanúsítványt külső kiállítótól vagy használjon önaláírt tanúsítványt. Az SSL-tanúsítvány forrásától függetlenül az alábbi tanúsítványattribútumokat megfelelően kell konfigurálni:

* **Tulajdonos**: Ezt az attribútumot állítsa *.az-ön-gyökértartománya értékre.
* **Tulajdonos alternatív neve**: Ennek az attribútumnak tartalmaznia kell a **.az-ön-gyökértartománya* és a **.scm.az-ön-gyökértartománya* neveket. Az SCM/Kudu helyhez társított alkalmazások SSL-kapcsolatai egy *az-ön-alkalmazásának-neve.scm.az-ön-gyökértartománya* formátumú címet használnak.

Konvertálja/mentse az SSL-tanúsítványt .pfx fájlként. A .pfx fájlnak tartalmaznia kell az összes köztes és főtanúsítványt. Jelszóval gondoskodjon a védelméről.

Amennyiben önaláírt tanúsítványt szeretne létrehozni, itt használhatja a PowerShell-parancsokat. Ügyeljen arra, hogy a saját ILB ASE tartományát használja az *internal.contoso.com* helyett: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

A böngészők megjelölik a PowerShell-parancs által létrehozott tanúsítványokat, mert a tanúsítványt nem a böngésző megbízhatósági láncában szereplő egyik hitelesítésszolgáltató hozta létre. A böngésző által is elfogadott tanúsítvány beszerzéséhez vegyen egyet a böngésző megbízhatósági láncában megtalálható üzleti hitelesítésszolgáltatók egyikétől. 

![ILB-tanúsítvány beállítása][4]

Saját tanúsítványának feltöltéséhez és a hozzáférés teszteléséhez tegye a következőket:

1. Az ASE létrehozása után keresse fel az ASE felhasználói felületét. Válassza az **ASE** > **Beállítások** > **ILB-tanúsítvány** lehetőséget.

2. Az ILB-tanúsítvány beállításához válassza ki a tanúsítvány .pfx fájlját, majd adja meg a jelszót. Ennek a lépésnek a feldolgozása beletelhet egy kis időbe. Egy üzenet jelzi, hogy egy feltöltési művelet van folyamatban.

3. Szerezze be az ASE ILB-címét. Válassza ki az **ASE** > **Tulajdonságok** > **Virtuális IP-cím** elemet.

4. Az ASE létrehozását követően hozzon létre egy webalkalmazást az ASE környezetben.

5. Amennyiben nincs virtuális gépe abban a virtuális hálózatban, hozzon létre egyet.

    > [!NOTE] 
    > Ne próbálja az ASE környezettel azonos alhálózatban létrehozni a virtuális gépet, mert az sikertelen lehet vagy problémákat okozhat.
    >
    >

6. Állítsa be az ASE tartomány DNS-ét. A DNS-ben helyettesítő karaktert is használhat tartományhoz. Pár egyszerű teszt végrehajtásához állítsa a webalkalmazás nevét a virtuális IP-címére a virtuális gép hosts fájljának szerkesztésével:

    a. Ha az ASE tartományneve _.ilbase.com_ és egy _mytestapp_ nevű webalkalmazást hoz létre, akkor annak címe _mytestapp.ilbase.com_ lesz. Ezután állítsa be, hogy a _mytestapp.ilbase.com_ tartománynév az ILB-címre mutasson. (Windows rendszeren a hosts fájl a _C:\Windows\System32\drivers\etc\_ elérési úton található.)

    b. A webtelepítés közzétételének teszteléséhez vagy a haladó konzol eléréséhez hozzon létre egy rekordot a _mytestapp.scm.ilbase.com_ tartományban.

7. A virtuális gép böngészőjével keresse fel a http://mytestapp.ilbase.com oldalt. (Vagy keresse fel webalkalmazásának nevét, a saját tartománnyal.)

8. A virtuális gép böngészőjével keresse fel a https://mytestapp.ilbase.com oldalt. Ha önaláírt tanúsítványt használ, fogadja el a biztonsági hiányosságot.

    Az ILB IP-címe az **IP-címek** területen látható. Ez a lista tartalmazza a külső virtuális IP-címet és a bejövő felügyeleti forgalmak IP-címeit is.

    ![ILB IP-címe][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>WebJobs-feladatok, a Functions és az ILB ASE ##

Az ILB ASE támogatja a Functionst és a WebJobs-feladatokat is, de ahhoz, hogy a portál is működjön velük, rendelkeznie kell hálózati hozzáféréssel egy SCM helyhez.  Ez azt jelenti, hogy a böngészőnek olyan gazdagépen kell lennie, amely csatlakozik a virtuális hálózathoz, vagy azon belül van.  

Az Azure Functions ILB ASE környezetben történő használatakor az alábbi hibaüzenet jelenhet meg: „Nem sikerült beolvasni a függvényeket. Próbálkozzon újra később.” Ez a hiba azért fordul elő, mert a Functions felhasználói felülete az SCM helyet használja a HTTPS helyett, és a főtanúsítványt nem tartalmazza a böngésző megbízhatósági lánca. A WebJobs-feladatok hasonló problémába ütköznek. A probléma elkerülése érdekében tegye a következők egyikét:

- Vegye fel a tanúsítványt a megbízható tanúsítványok tárolójába. Ez megszünteti az Edge és az Internet Explorer blokkolását.
- A Chrome-ban először keresse fel az SCM helyet, fogadja el a nem megbízható tanúsítványt, majd menjen a portálra.
- Használja böngészője megbízhatósági láncának egyik kereskedelmi tanúsítványát.  Ez a legjobb lehetőség.  

## <a name="dns-configuration"></a>DNS-konfiguráció ##

Külső virtuális IP-cím használata esetén az Azure kezeli a DNS-t. Az ASE környezetben létrehozott összes alkalmazás automatikusan hozzáadódik Azure DNS-hez, amely egy nyilvános DNS. Az ILB ASE környezetben Önnek kell kezelnie a saját DNS-ét. Egy megadott tartomány, például a _contoso.net_ esetében olyan DNS A-rekordokat kell létrehoznia, amelyek az ILB címére mutatnak:

- *.contoso.net
- *.scm.contoso.net

Ha az ILB ASE tartományt az ASE környezeten kívül más dolgokra is használja, lehetséges, hogy a DNS-t alkalmazásnevenként kell kezelnie. Ez a módszer nehézkes, mivel az alkalmazásnevek létrehozásakor egyesével kell hozzáadni azokat a DNS-hez. Ezért javasoljuk egy dedikált tartomány használatát.

## <a name="publish-with-an-ilb-ase"></a>Közzététel ILB ASE környezetben ##

Minden létrehozott alkalmazásnak két végpontja van. Az ILB ASE környezetben, az *&lt;alkalmazásnév>.&lt;ILB ASE tartomány>* és az *&lt;alkalmazásnév>.scm.&lt;ILB ASE tartomány>* áll rendelkezésre. 

Az SCM helynév a Kudu konzolhoz irányítja, az Azure Portal **Speciális portál** részére. A Kudu konzol a környezeti változók megtekintését, a lemez vizsgálatát, a konzol használatát és még sok más funkciót kínál. További információ: [Kudu konzol az Azure App Service-ben][Kudu]. 

A több-bérlős App Service-ben és a külső ASE környezetben az Azure Portal és a Kudu konzol között egyszeri bejelentkezés van. Az ILB ASE esetében viszont közzétételi hitelesítő adatai használatával jelentkezhet be a Kudu konzolba.

A GitHubhoz és Visual Studio Team Services-hez hasonló internetes alapú CI rendszerek nem működnek az ILB ASE környezettel, mivel a közzététel végpontja nem elérhető az internetről. Ehelyett egy lekérési modellt használó CI-rendszert kell használnia, ilyen például a Dropbox.

Az ILB ASE alkalmazásainak közzétételi végpontjai az ILB ASE létrehozásakor megadott tartományt használják. Ez a tartomány az alkalmazás közzétételi profilján és az alkalmazás portálpanelén jelenik meg (**Áttekintés** > **Alapvető szolgáltatások** és **Tulajdonságok**). Ha az ILB ASE altartománya *contoso.net* és az alkalmazás neve *mytest*, akkor FTP-hez használja a *mytest.contoso.net*, webes telepítéshez pedig az *mytest.scm.contoso.net* tartományneveket.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>ILB ASE párosítása WAF-eszközzel ##

Az Azure App Service számos biztonsági eszközt kínál a rendszer védelmére. Ezek az eszközök azt is segítenek megállapítani, hogy feltörték-e az alkalmazást. A legjobb védelem webalkalmazások részére egy, az Azure App Service-hez hasonló üzemeltetési platform párosítása egy webalkalmazás-tűzfallal (WAF). Mivel az ILB ASE rendelkezik hálózattól elzárt alkalmazásvégpontokkal, így alkalmas erre a használati módra.

Az ILB ASE WAF eszközhöz való konfigurálásáról további információkat a [webalkalmazás-tűzfal és az App Service Environment konfigurálását ismertető cikk][ASEWAF] tartalmaz. Ez a cikk leírja, hogyan használhatja együtt a Barracuda virtuális berendezést az ASE környezettel. Másik lehetőségként az Azure Application Gateway is használható. Az Application Gateway az alapvető OWASP-szabályokat használja a mögötte elhelyezett alkalmazások biztonságossá tételére. Az Application Gatewayről további információt az [Azure webalkalmazási tűzfal bevezető][AppGW] cikke tartalmaz.

## <a name="get-started"></a>Bevezetés ##

* Az ASE használatával kapcsolatos első lépésekről [Az App Service Environment bemutatása][Intro] témakörben olvashat.
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
