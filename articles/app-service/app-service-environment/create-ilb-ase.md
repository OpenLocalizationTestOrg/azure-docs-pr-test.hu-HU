---
title: "aaaCreate és a belső terheléselosztók az Azure App Service-környezet használata"
description: "Megtudhatja, hogyan toocreate és egy internet-egymástól el vannak különítve Azure App Service-környezet használata"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: d019ca6f231c3acfdab4cd380db375a076802f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Létrehozhat és használhat belső terheléselosztót az App Service-környezet #

 Az Azure App Service Environment-környezet az Azure App Service egy Azure virtuális hálózatot (VNet) lévő alhálózatot történő központi telepítését. Nincsenek két módon toodeploy az App Service-környezetek (ASE): 

- A virtuális IP-címre a külső IP-cím, egy külső ASE gyakran nevezik.
- A virtuális IP-címre a belső IP-címet gyakran nevezik egy ILB ASE mert hello belső végpont egy belső terheléselosztón (ILB). 

Ez a cikk bemutatja, hogyan toocreate egy ILB ASE. A hello ASE áttekintéséért lásd: [bemutatása tooApp Service-környezetek][Intro]. Hogyan toocreate egy külső ASE: toolearn [hozzon létre egy külső ASE][MakeExternalASE].

## <a name="overview"></a>Áttekintés ##

A virtuális hálózat telepítheti egy ASE az internetről elérhető végpontok vagy IP-címet. tooset hello IP-címek tooa VNet-címet, hello ASE egy ILB együtt kell telepíteni. Ha az egy ILB a ASE telepít, meg kell adnia:

-   A saját tartomány használni, amikor a alkalmazásai létrehozására.
-   hello a HTTPS-hez használt tanúsítvány.
-   A tartomány DNS-kezelés.

Ismét műveleteket végezheti el, mint:

-   Intranetes alkalmazások üzemeltetését biztonságosan felhőben hello, amelyek egy-webhelyek vagy Azure ExpressRoute VPN keresztül érhető el.
-   Állomás alkalmazások hello felhőben a nyilvános DNS-kiszolgálók nem jelennek meg.
-   Hozzon létre internet elszigetelt háttér-alkalmazásokat, az előtér-alkalmazások biztonságosan integrálható.

### <a name="disabled-functionality"></a>Letiltott funkció ###

Néhány dolgot, amely egy ILB ASE használatakor nem hajtható végre:

-   IP-alapú SSL használatára.
-   IP-cím hozzárendelése toospecific alkalmazásokat.
-   Vásároljon és tanúsítványt használjon olyan alkalmazással hello Azure-portálon keresztül. Tanúsítványok beszerzése hitelesítésszolgáltatótól származó közvetlenül, és használhatja azokat az alkalmazásokat. Nem lehet beszerezni azokat, hello Azure-portálon keresztül.

## <a name="create-an-ilb-ase"></a>Hozzon létre egy ILB ASE ##

egy ILB ASE toocreate:

1. Hello Azure-portálon, válassza ki **új** > **Web + mobil** > **App Service Environment-környezet**.

2. Válassza ki előfizetését.

3. Válasszon ki vagy hozzon létre egy erőforráscsoportot.

4. Válassza ki, vagy hozzon létre egy Vnetet.

5. Ha egy meglévő virtuális hálózatot választ ki, egy alhálózat toohold hello ASE toocreate kell. Ellenőrizze, hogy tooset alhálózat méretének elég nagy tooaccommodate a ASE jövőbeli növekedésének. Azt javasoljuk, hogy a méretet `/25`, amely 128-címekkel rendelkezik, és kezelni tud a maximális méretű ASE. hello minimális méret választhat egy `/28`. Után szükség van az infrastruktúra, ez a méret lehet méretezett tooa legfeljebb 11 példányok.

    * Az App Service-csomagokról túlmutató hello alapértelmezett legfeljebb 100 példányok.

    * Méretezhető, 100 közelében, de gyorsabb előtér-méretezés.

6. Válassza ki **virtuális hálózati/hely** > **virtuális hálózati konfiguráció**. Set hello **VIP típus** túl**belső**.

7. Adja meg a tartomány nevét. Ebben a tartományban, egy a ASE létre alkalmazásokat a hello. Nincsenek bizonyos korlátozások vonatkoznak. Nem lehet:

    * nettó   

    * azurewebsites.NET

    * p.azurewebsites.NET

    * &lt;asename környezet&gt;. p.azurewebsites.net

   alkalmazások használt hello egyéni tartománynevet, és használja a ASE hello tartománynév nem lehet átfedésben. Az egy ILB ASE hello tartománynévvel _contoso.com_, nem használható egyéni tartománynevek az alkalmazásokhoz, például:

    * www.contoso.com

    * ABCD.def.contoso.com

    * ABCD.contoso.com

   Ha ismeri az egyéni tartománynevek hello az alkalmazásokat, válassza ki egy tartományt a hello ILB ASE, amelyek nem rendelkeznek ezen egyéni tartománynevekkel ütközés. Ebben a példában, használhatja a következőhöz hasonlóan *contoso-internal.com* hello tartomány a hajlamosnak mert, amelyek nem ütköznek egyéni tartománynevek végződő *. contoso.com*.

8. Válassza ki **OK**, majd válassza ki **létrehozása**.

    ![ASE létrehozása][1]

A hello **virtuális hálózati** panelen van egy **virtuális hálózati konfiguráció** lehetőséget. Használat tooselect egy külső VIP vagy VIP belső. hello alapértelmezett érték a **külső**. Ha **külső**, a ASE egy internetről elérhető VIP használja. Ha **belső**, a ASE egy ILB a Vneten belül egy IP-cím van konfigurálva.

Miután kiválasztotta a **belső**, hello képességét tooadd több IP-címet tooyour ASE törlődik. Ehelyett tooprovide hello tartományának hello ASE van szüksége. Service-környezetben egy külső virtuális IP-címre, ASE hello tartomány szolgál, hogy ASE létrehozott alkalmazások hello hello nevét.

Ha **VIP típus** túl**belső**, a ASE név nem szolgál hello tartomány hello ASE. Hello tartomány explicit módon adja meg. Ha a tartomány *contoso.corp.net* és abban a ASE nevű alkalmazást hoz létre *timereporting*, hello URL-cím, az alkalmazás timereporting.contoso.corp.net érték.


## <a name="create-an-app-in-an-ilb-ase"></a>Hozzon létre egy alkalmazást-Példánynak környezetben ##

A hello-Példánynak környezetben alkalmazást hoz létre, hogy hozzon létre egy app Service-környezetben általában azonos módon.

1. Hello Azure-portálon, válassza ki **új** > **Web + mobil** > **webes** vagy **Mobile** vagy  **API-alkalmazás**.

2. Adja meg a hello alkalmazás hello nevét.

3. Válassza ki a hello előfizetést.

4. Válasszon ki vagy hozzon létre egy erőforráscsoportot.

5. Válassza ki, vagy hozzon létre egy App Service-csomag. Ha egy új App Service-csomag toocreate, jelölje be a ASE hello helyként. Válassza ki a létrehozott App Service-csomag toobe, ahová hello feldolgozókészletek. Hello App Service-csomag létrehozásakor válassza ki a ASE hello helyét és hello munkavégző készletét. Hello alkalmazás hello nevét adja meg, ha az alkalmazás neve alatt hello tartomány lép hello tartomány által a ASE.

6. Kattintson a **Létrehozás** gombra. Ha hello app tooappear az irányítópulton, jelölje be a **PIN-kód toodashboard** jelölőnégyzetet.

    ![App Service-csomag létrehozása][2]

    A **alkalmazásnév**, hello tartománynév megadása a ASE frissített tooreflect hello tartományán.

## <a name="post-ilb-ase-creation-validation"></a>POST-Példánynak ASE létrehozásának ellenőrzése ##

Egy ILB ASE mint hello nem - Példánynak ASE némileg eltérő. Már beállításértékeket, mint a saját DNS toomanage akkor kell. Akkor is tooprovide saját tanúsítványát a HTTPS-kapcsolatoknál.

Miután létrehozta a ASE, hello tartománynév megadott hello tartomány jeleníti meg. Új elem megjelenik a **beállítás** nevű menü **ILB tanúsítvány**. hello ASE olyan tanúsítvány, amely nem adja meg a hello ILB ASE tartományt hozza létre. Hello ASE tanúsítványt használja, ha a böngésző megtudhatja, hogy érvénytelen. Ez a tanúsítvány teszi, hogy könnyebben tootest HTTPS, de a saját tanúsítványban kapcsolt tooyour ILB ASE tartományt kell tooupload. Ez a lépés nem szükséges, függetlenül attól, hogy a tanúsítvány önaláírt vagy a hitelesítésszolgáltatótól beszerzett.

![ILB ASE tartománynév][3]

A ILB ASE érvényes SSL-tanúsítvány szükséges. Belső hitelesítésszolgáltatók használja, vásárolhat egy tanúsítványt egy külső kibocsátó, vagy használhat önaláírt tanúsítványt. Hello SSL-tanúsítvány hello forrását, függetlenül hello következő tanúsítvány attribútumokkal kell toobe megfelelően konfigurálva:

* **Tulajdonos**: Ez az attribútum too*.your-gyökér-tartományi-Itt állítható be.
* **Tulajdonos alternatív neve**: ennek az attribútumnak kell tartalmaznia a **.your-gyökér-tartományi-Itt* és **.scm.your-gyökér-tartományi-Itt*. Minden alkalmazáshoz kapcsolódó SCM/Kudu webhely SSL-kapcsolatok toohello hello űrlap cím használata *your-app-name.scm.your-root-domain-here*.

A konvertálás/save hello SSL-tanúsítvány egy .pfx fájlba. hello .pfx fájl tartalmazza az összes köztes kell, és a legfelső szintű tanúsítványok. A biztonság jelszóval.

Ha azt szeretné, hogy toocreate egy önaláírt tanúsítványt, hello PowerShell-parancsok itt is használhatja. Lehet, hogy toouse a ILB ASE tartománynév helyett *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

a PowerShell-parancsok generáló hello tanúsítvány böngésző megjelölt, mert hello tanúsítványt a hitelesítésszolgáltatótól, amely megbízhatósági lánc a böngésző nem hozta létre. tooget olyan tanúsítvány, amely megbízik a böngésző, be kell szereznie, a böngésző lánc megbízhatósági kereskedelmi hitelesítésszolgáltatótól származó. 

![Állítsa be a ILB tanúsítványt][4]

tooupload saját tanúsítványokat és a vizsgálati hozzáférést:

1. Hello ASE létrehozása után nyissa meg toohello ASE felhasználói felületén. Válassza ki **ASE** > **beállítások** > **ILB tanúsítvány**.

2. tooset hello ILB tanúsítvány, válasszon hello .pfx fájlt, majd írja be a hello jelszót. Ez a lépés néhány alkalommal tooprocess vesz igénybe. Egy üzenet jelenik meg, amely meghatározza, hogy, hogy folyamatban van egy feltöltési művelet.

3. A ASE hello ILB cím beolvasása. Válassza ki **ASE** > **tulajdonságok** > **virtuális IP-cím**.

4. Webalkalmazás létrehozása a ASE hello ASE létrehozása után.

5. Virtuális gép létrehozása, ha még nincs fiókja, hogy a Vneten belül.

    > [!NOTE] 
    > A virtuális gép ne toocreate hello ugyanazon az alhálózaton, hello ASE, mert sikertelen, vagy problémákhoz.
    >
    >

6. A ASE tartomány DNS hello beállítása. A tartomány a DNS-ben helyettesítő karakter használható. toodo néhány egyszerű teszteli, hello hosts fájlt a virtuális gép tooset hello web app name toohello VIP IP-cím szerkesztése:

    a. Ha a ASE hello tartomány neve _. ilbase.com_ nevű hello-webalkalmazás létrehozása és _mytestapp_, a címzett _mytestapp.ilbase.com_. Ezután _mytestapp.ilbase.com_ tooresolve toohello ILB cím. (A Windows hello hosts fájl jelenleg _C:\Windows\System32\drivers\etc\_.)

    b. tootest webalkalmazás telepítési közzététel vagy a hozzáférés toohello konzol, speciális rekord létrehozása _mytestapp.scm.ilbase.com_.

7. Használja ezt a virtuális Gépet egy böngészőt, és navigáljon a http://mytestapp.ilbase.com. (Vagy a tartományban van a webes alkalmazás neve toowhatever.)

8. Használja ezt a virtuális Gépet egy böngészőt, és navigáljon a https://mytestapp.ilbase.com. Ha egy önaláírt tanúsítványt használ, fogadja el a biztonsági hello hiánya.

    hello IP-címet a ILB megtalálható-e **IP-címek**. Ez a lista a hello IP-címek hello külső VIP és bejövő felügyeleti adatforgalomhoz használt is rendelkezik.

    ![ILB IP-cím][5]

### <a name="web-jobs-functions-and-hello-ilb-ase"></a>Webalkalmazás-feladatok, funkciók és ILB ASE hello

Egy ILB ASE funkciók és a webes feladatok is támogatottak, de hello portál toowork velük, rendelkeznie kell hálózati hozzáférési toohello SCM helyet.  Ez azt jelenti, hogy a böngészőt kell lennie egy gazdagépen, vagy vagy toohello virtuális hálózathoz csatlakoztatva.  

Ha az Azure Functions egy ILB ASE használja, akkor előfordulhat, hogy hiba jelenik, amely szerint a "azt válnak a függvények jelenleg képes tooretrieve. Próbálkozzon újra később." Ez a hiba akkor fordul elő, mert hello funkciók felhasználói felület hello SCM webhely használja a HTTPS-KAPCSOLATON keresztül, és hello legfelső szintű tanúsítvány nem hello böngésző lánc megbízhatósági. Webes feladatok hasonló problémát észlelt. tooavoid probléma hello valamelyikét teheti:

- Adja hozzá a hello tanúsítvány tooyour megbízható tanúsítványok tárolójába. Ez feloldja széle és az Internet Explorerben.
- Használja a Chrome és toohello SCM hely először lépnek, hello nem megbízható tanúsítvány elfogadásához, és folytassa a toohello portálon.
- A böngésző megbízhatósági lánc a kereskedelmi tanúsítványt használjon.  Ez a lehetőség hello ajánlott.  

## <a name="dns-configuration"></a>DNS-konfiguráció ##

Egy külső VIP használatakor hello DNS Azure kezeli. Bármely alkalmazás a ASE létrehozott automatikusan hozzáadása tooAzure DNS, amely egy nyilvános DNS-ben. A saját DNS-Példánynak környezetben kell kezelni. Például egy adott tartomány _contoso.net_, a DNS-ben rögzíti DNS A pont tooyour ILB címet toocreate van szüksége:

- *. contoso.net
- *. scm.contoso.net

Ha a ILB ASE tartományt a ASE kívül több dolgot, szükség lehet a DNS toomanage /-alkalmazás-neve alapján. Ez a módszer van kihívást, mert kell tooadd minden új alkalmazás neve a DNS-be történő létrehozásakor. Ezért azt javasoljuk, hogy dedikált tartományt használ.

## <a name="publish-with-an-ilb-ase"></a>Egy ILB mértékéig közzététele ##

Minden alkalmazást, amely jön létre nincsenek két végpontot. -Példánynak környezetben van  *&lt;alkalmazás neve >.&lt; ILB ASE tartományi >* és  *&lt;alkalmazás neve > .scm.&lt; ILB ASE tartományi >*. 

hello SCM helynév viszi toohello Kudu konzol hello nevű **speciális portal**, melyhez hello Azure-portálon. hello Kudu konzol segítségével megtekintheti a környezeti változók, hello lemez vizsgálatát, használja a konzolon, és még sok más. További információkért lásd: [Kudu konzol az Azure App Service-][Kudu]. 

Hello több-bérlős App Service és a külső Service-környezetben van az egyszeri bejelentkezés hello Azure-portálon és hello Kudu konzol között. A hello ILB ASE azonban kell toouse a közzétételi hitelesítő adatok toosign hello Kudu konzolba.

Internet alapú CI rendszereknek, például a Githubon és a Visual Studio Team Services, egy ILB mértékéig nem működnek, mert hello közzétételi végpont nem érhető el az internet. Ehelyett egy CI rendszer, amely lekéréses modellt használ, például a Dropbox toouse van szüksége.

hello közzétételi végpontok-Példánynak környezetben alkalmazások használata hello tartomány adott hello ILB ASE hozták létre. Ez a tartomány hello alkalmazás közzétételi profil és a hello alkalmazás portálpaneljéhez jelenik meg (**áttekintése** > **Essentials** és is **tulajdonságok**). Ha egy ILB ASE a hello altartomány *contoso.net* és az alkalmazás neve *mytest*, használjon *mytest.contoso.net* az FTP és *mytest.scm.contoso.net*  központi telepítésére.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Egy ILB ASE gömbcsatlakozók WAF-eszközzel rendelkező ##

Az Azure App Service számos biztonsági intézkedéseket hello rendszer védelmét biztosítja. Hogy egy alkalmazás lett megtámadott is segítenek toodetermine. hello legjobb védelmet webalkalmazás toocouple egy üzemeltetési platformtól, például az Azure App Service-ben a webalkalmazási tűzfal (waf-ot). Mivel hello ILB ASE egy elszigetelt hálózati alkalmazás végponttal rendelkezik, célszerű a használatra.

További információk hogyan tooconfigure a ILB ASE WAF-eszközzel rendelkező: toolearn [webalkalmazási tűzfal konfigurálása az App Service-környezet][ASEWAF]. Ez a cikk bemutatja, hogyan toouse egy Barracuda virtuális készülék a mértékéig. Másik lehetőség is toouse Azure Application Gateway. Alkalmazásátjáró hello OWASP core szabályok toosecure alkalmazások mögé azt használja. Alkalmazásátjáró kapcsolatos további információkért lásd: [bemutatása toohello Azure webalkalmazási tűzfal][AppGW].

## <a name="get-started"></a>Bevezetés ##

Minden cikkeket, és hogyan-tooinstructions ASEs a érhetők el a [App Service-környezetek – fontos fájl][ASEReadme].

* tooget használatába ASEs, lásd: [bemutatása tooApp Service-környezetek][Intro].
* Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).
 
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
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
