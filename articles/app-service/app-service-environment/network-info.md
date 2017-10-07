---
title: "az Azure App Service-környezetek aaaNetworking megfontolások"
description: "Hello ASE hálózati forgalom ismerteti, hogyan tooset NSG-ket és a mértékéig udr-EK"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ccompy
ms.openlocfilehash: d4d3000f4d4d75814b1e6d47079d967334eb1a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="networking-considerations-for-an-app-service-environment"></a>App Service-környezet hálózati szempontjai #

## <a name="overview"></a>Áttekintés ##

 Azure [App Service Environment-környezet] [ Intro] egy alhálózatba az Azure virtuális hálózatban (VNet) az Azure App Service üzemelő példány. Nincsenek App Service-környezet (ASE) két központi telepítési típusokat:

- **Külső ASE**: tesz elérhetővé hello ASE által kezelt alkalmazások az internetről elérhető IP-címet. További információkért lásd: [hozzon létre egy külső ASE][MakeExternalASE].
- **ILB ASE**: tesz elérhetővé hello ASE által kezelt alkalmazások az IP-címet a virtuális hálózaton belül. hello belső végpont esetében egy belső terheléselosztó (ILB), ezért azt egy ILB ASE hívják. További információkért lásd: [létrehozása és használata egy ILB ASE][MakeILBASE].

Most már két verziója van App Service Environment-környezet: ASEv1 és ASEv2. A ASEv1 információkért lásd: [bemutatása tooApp Service-környezet v1][ASEv1Intro]. ASEv1 klasszikus vagy erőforrás-kezelő VNet is telepíthető. ASEv2 csak egy erőforrás-kezelő virtuális hálózaton történő telepíthetők.

Egy ASE összes hívásait toohello nyissa meg az internet hagyja hello virtuális hálózaton keresztül hello ASE hozzárendelt virtuális IP-címhez. hello nyilvános IP-címe a VIP majd hello forrás IP-cím hello ASE összes hívásait toohello nyissa meg az internethez. Ha a ASE hello alkalmazások hívások tooresources a virtuális hálózat vagy VPN-en keresztül, hello forrás IP-cím akkor használják a ASE hello alhálózat IP-címek hello egyik. Hello ASE hello virtuális hálózaton belül van, mert erőforrások hello VNet további konfiguráció nélkül is hozzáférhet. Ha hello VNet csatlakoztatott tooyour a helyszíni hálózat, a ASE alkalmazások is hozzáférést tooresources van. Nincs szükség tooconfigure hello ASE vagy az alkalmazás minden további.

![Külső ASE][1] 

Ha egy külső ASE, hello nyilvános VIP is, hogy a ASE alkalmazások megoldásához toofor hello végpont:

* A HTTP/S 
* FTP/MP. 
* A webes telepítése.
* Távoli hibakeresés.

![ILB ASE][2]

Ha egy ILB ASE, hello ILB hello IP-címe HTTP/S, az FTP/S, a webes telepítési és a távoli hibakeresés hello végpontja.

hello normál app hozzáférési portok a következők:

| Használat | A | túl|
|----------|---------|-------------|
|  A HTTP/HTTPS  | Felhasználó által konfigurálható |  80, 443 |
|  FTP/FTPS    | Felhasználó által konfigurálható |  21, 990, 10001-10020 |
|  A Visual Studio távoli hibakeresés  |  Felhasználó által konfigurálható |  4016, 4018, 4020, 4022 |

Ez érvényét veszti, ha egy külső ASE vagy egy ILB ASE. Ha a számítógép egy külső ASE, ezeket a portokat a hello nyilvános VIP kattint. Ha a számítógép egy ILB ASE, ezeket a portokat a hello ILB kattint. Ha zárolását a 443-as porton, néhány funkció elérhetővé tett hello portálon hatással lehet. További információkért lásd: [Portal függőségek](#portaldep).

## <a name="ase-dependencies"></a>ASE függőségek ##

Egy ASE befelé függőség:

| Használat | A | túl|
|-----|------|----|
| Kezelés | App Service management címek | ASE alhálózati: 454, 455 |
|  Belső kommunikációs ASE | ASE alhálózati: minden port | ASE alhálózati: minden port
|  Engedélyezi az Azure terheléselosztó bejövő | Az Azure terheléselosztó | ASE alhálózati: minden port
|  IP-címek hozzárendelt alkalmazás | Címek hozzárendelt alkalmazás | ASE alhálózati: minden port

hello bejövő forgalom biztosít parancs és hello ASE hozzáadása toosystem figyelési irányítását. hello forrás IP-címek a forgalom hello szereplő [ASE felügyeleti címek] [ ASEManagement] dokumentum. hello hálózati biztonsági beállításokat kell tooallow a hozzáférést a 454 és a 455 portok összes IP-címére.

Nincsenek hello ASE alhálózaton belüli belső összetevő-kommunikációhoz használt sok portot, és módosíthatja.  Ehhez szükséges összes hello portjainak hello ASE alhálózati toobe hello ASE alhálózati érhető el. 

Hello Azure terheléselosztó és hello ASE alhálózati hello minimális portok hello kommunikációját, hogy nyitva kell toobe 454, 455 és 16001. Tartsa életben forgalom hello terheléselosztó és hello ASE közötti hello 16001 portot használja. Ha egy ILB ASE használ majd zárolhatja a forgalom toojust hello 454, 455, 16001 portok le.  Ha egy külső ASE használ majd kell tootake a fiók hello normál app hozzáférési portok.  Hozzárendelt alkalmazás címek használata tooopen kell azt tooall portok.  Amikor egy cím hozzá van rendelve tooa adott alkalmazást, majd hello terheléselosztó portot használja előre toosend HTTP és HTTPS-forgalom toohello ASE nem ismert.

Hozzárendelt alkalmazás IP-címek használata IP-címek hozzárendelve tooyour alkalmazások toohello ASE alhálózati hello tooallow forgalmát kell.

A kimenő hozzáférés érdekében egy ASE több külső rendszer függ. E rendszer függőségek rendelkező DNS-nevek, és nem képez le IP-címeket rögzített tooa. Ebből kifolyólag hello ASE hello ASE alhálózati tooall a kimenő hozzáférésre van szüksége a portok számos külső IP-címek. Egy ASE rendelkezik a következő kimenő függőségek hello:

| Használat | A | túl|
|-----|------|----|
| Azure Storage | ASE alhálózati | TABLE.Core.Windows.NET, blob.core.windows.net, queue.core.windows.net, file.core.windows.net: 80-as, a 443-as, a 445-ös (445-ös csak szükséges ASEv1.) |
| Azure SQL Database | ASE alhálózati | Database.Windows.NET: 1433-as számú 11000-11999, 14000-14999 (további információkért lásd: [SQL Database 12-es port használati](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).)|
| Azure felügyelet | ASE alhálózati | Management.Core.Windows.NET, management.azure.com: 443 
| SSL-tanúsítvány ellenőrzése |  ASE alhálózati            |  OCSP.msocsp.com, mscrl.microsoft.com, crl.microsoft.com: 443
| Azure Active Directory        | ASE alhálózati            |  Internet: 443
| Az alkalmazásszolgáltatási management        | ASE alhálózati            |  Internet: 443
| Azure DNS                     | ASE alhálózati            |  Internet: 53
| Belső kommunikációs ASE    | ASE alhálózati: minden port |  ASE alhálózati: minden port

Hello ASE toothese függőségek elveszítette a hozzáférését, ha a rendszer nem működik. Ha hosszú elég, hello ASE történik-e, amely fel van függesztve.

### <a name="customer-dns"></a>Ügyfél DNS ###

Ha hello VNet van konfigurálva egy DNS-ügyfél által megadott kiszolgálón, hello bérlők munkaterheléseihez használhatja. hello ASE továbbra is hozzá kell az Azure DNS toocommunicate felügyelet céljából. 

Ha hello VNet működik együtt az ügyfél DNS hello VPN másik oldalán, hello DNS-kiszolgáló, amely tartalmazza a hello ASE hello alhálózatból elérhetőnek kell lennie.

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portál függőségek ##

A hozzáadása toohello ASE működési függőségek nincsenek néhány további elemek kapcsolódó toohello portál élmény. Néhány hello Azure-portálon hello képessége közvetlen hozzáférést too_SCM site_ függ. Minden az Azure App Service-alkalmazást a két URL-címek vannak. hello első URL-cím van tooaccess az alkalmazást. hello második URL-címe: hello néven is ismert tooaccess hello SCM hely _Kudu konzol_. Hello SCM helyet használó szolgáltatások a következők:

-   Webes feladatok
-   Functions
-   Adatfolyam-napló
-   Kudu
-   Bővítmények
-   Process Explorer
-   Konzol

Egy ILB ASE használja, nem a hello SCM hely és az internet hello virtuális hálózaton kívülről hozzáférhető. Ha az alkalmazás egy ILB ASE üzemelteti, bizonyos funkciók nem működnek hello portálról.  

Ezekre a képességekre, amelyek hello SCM hely számos közvetlenül hello Kudu konzolon érhető el. Tooit is elérheti, hanem közvetlenül hello portál használatával. Ha az app Service-Példánynak környezetben, használja a közzétételi hitelesítő adatok toosign a. hello URL-cím tooaccess hello SCM hely ILB Service-környezetben üzemeltetett alkalmazás rendelkezik hello a következő formátumban: 

```
<appname>.scm.<domain name hello ILB ASE was created with> 
```

Ha a ILB ASE hello tartománynév *contoso.net* és az alkalmazás neve *testapp*, hello app éri el a *testapp.contoso.net*. hello SCM hely vele együtt áthelyeződik éri el a *testapp.scm.contoso.net*.

### <a name="functions-and-web-jobs"></a>Függvények és a webes feladatok ###

Függvények és a webes feladatok hello SCM hely függ, de akkor is, ha az alkalmazások-Példánynak környezetben, feltéve, hogy a böngésző hello SCM helyen érhető el a hello portál használata támogatott.  Ha önaláírt tanúsítványt használ a ILB mértékéig, szüksége lesz tooenable a böngésző tootrust, hogy a tanúsítvány.  Az Internet Explorer és a szegély, amely azt jelenti, hogy a hello tanúsítvány rendelkezik toobe hello számítógép megbízható kapcsolat tárolja.  Ha Chrome használja majd, amely azt jelenti, hogy elfogadott hello tanúsítvány hello böngészőben korábbi által a vélhetően elérés közvetlenül hello scm hely.  hello legjobb megoldás, toouse hello böngésző lánc megbízhatósági kereskedelmi tanúsítványokat.  

## <a name="ase-ip-addresses"></a>ASE IP-címek ##

Egy ASE néhány IP címek toobe tisztában van. Ezek a következők:

- **Nyilvános bejövő IP-cím**: app Service-külső környezetben, és felügyeleti forgalom egy külső ASE és egy ILB ASE is használatos.
- **Kimenő nyilvános IP-cím**: hello "feladó" IP hello ASE kimenő kapcsolatokat, hogy hagyja hello virtuális hálózatot, amely nem útválasztásos le egy VPN használatos.
- **ILB IP-cím**: Ha egy ILB ASE használja.
- **Alkalmazás által hozzárendelt IP-alapú SSL-címek**: csak akkor lehetséges, egy külső mértékéig és IP-alapú SSL be van állítva.

A következő IP-címek láthatók egyszerűen egy ASEv2 a hello Azure-portálon a hello ASE felhasználói felületén. Ha egy ILB ASE, hello ILB hello IP-Címek szerepel.

![IP-címek][3]

### <a name="app-assigned-ip-addresses"></a>Alkalmazás által hozzárendelt IP-címek ###

Egy külső mértékéig rendelhet hozzá IP-címek tooindividual alkalmazásokat. Nem hajthatja végre, amely egy ILB mértékéig. További információt a tooconfigure az alkalmazás toohave a saját IP-cím, lásd: [kötése egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások](../../app-service-web/app-service-web-tutorial-custom-ssl.md).

Ha egy alkalmazás a saját IP-alapú SSL-címmel rendelkezik, hello ASE fenntartja két portok toomap toothat IP-címet. Egy port a HTTP-forgalom, pedig hello más portot a HTTPS-hez. Ezeket a portokat hello ASE UI hello IP-címek szakaszban találhatók. Forgalom kell lennie a képes tooreach azokat a portokat a hello VIP vagy hello alkalmazások, amelyek nem érhetők el. Ez a követelmény fontos tooremember kell, ha hálózati biztonsági csoportokkal (NSG-k).

## <a name="network-security-groups"></a>Network Security Groups (Hálózati biztonsági csoportok) ##

[Hálózati biztonsági csoportok] [ NSGs] hello képességét toocontrol hálózati hozzáférést egy Vneten belül. Ha hello portál, van egy implicit mindent: hello legalacsonyabb prioritású toodeny szabály Megtagadás. Állít össze vannak a szabályokat.

-Környezetben nincs hozzáférési toohello használt virtuális gépek toohost hello ASE magát. Is a Microsoft által kezelt előfizetést. Toorestrict hozzáférés toohello alkalmazások hello ASE, állítsa az NSG-k hello ASE alhálózaton. Ennek során toohello ASE függőségek alapos figyelmet fordítania. Ha letiltja az összes függőséget, a hello ASE leáll.

Az NSG-k hello Azure-portálon keresztül vagy a PowerShell segítségével konfigurálható. hello itt tartalmazza hello Azure-portálon. Létrehozásához és kezeléséhez az NSG-ket hello portálon, a legfelső szintű erőforrásként **hálózati**.

Hello bejövő és kimenő követelmények figyelembe kell venni, amikor hello NSG-ket alábbihoz hasonló toohello NSG-ket ebben a példában látható módon. virtuális hálózat címtartománya hello van _192.168.250.0/16_, és hello alhálózatot, amely hello ASE _192.168.251.128/25_.

hello első két bejövő követelményei hello ASE toofunction hello lista ebben a példában hello tetején látható. Ezek ASE és felügyeletét teszi lehetővé hello ASE toocommunicate önmagával engedélyezése. hello más bejegyzések összes bérlői konfigurálható és hálózati hozzáférési toohello ASE által üzemeltetett alkalmazások képesek felügyelni. 

![Bejövő biztonsági szabályok][4]

Alapértelmezett szabály lehetővé teszi, hogy az IP-címek hello hello VNet tootalk toohello ASE alhálózaton. Egy másik alapértelmezett szabály lehetővé teszi, hogy a hello terheléselosztóhoz, más néven hello nyilvános VIP, a hello ASE toocommunicate. toosee hello alapértelmezett szabályokat, válassza ki **alapértelmezett szabályok** következő toohello **Hozzáadás** ikonra. Ha a Megtagadás, minden más szabály, miután hello NSG-szabályok látható, hogy akadályozza meg a hello VIP és hello ASE közötti forgalmat. tooprevent adatforgalma a hello VNet belül adja hozzá a saját szabály tooallow bejövő. A forrás egyenlő tooAzureLoadBalancer használata a cél **bármely** és egy porttartomány  **\*** . Mivel hello NSG-szabály alkalmazott toohello ASE alhálózati, nincs szükség a toobe specifikus hello cél.

Ha egy IP-cím tooyour alkalmazáshoz rendelt, ellenőrizze, hogy hello portok megnyitásához mindig. toosee hello-porttal rendelkezik, válasszon **App Service Environment-környezet** > **IP-címek**.  

A következő kimenő szabályok hello látható minden hello elem van szükség, hello utolsó elem kivételével. Hálózati hozzáférés toohello ASE függőségei vannak, az ebben a cikkben észleltek lehetővé teszik. Ha letiltja az e valamelyiket, a ASE működése leáll. hello lista utolsó elemére hello lehetővé teszi, hogy a ASE toocommunicate a virtuális hálózat más erőforrásokat.

![Kimenő biztonsági szabályok][5]

Az NSG-k meghatározása után rendelje hozzá őket a toohello alhálózatot, amely a ASE a. Ha nem emlékszik hello ASE virtuális hálózat vagy az alhálózat, láthatja a hello ASE felügyeleti portálról. tooassign NSG tooyour alhálózati hello nyissa toohello alhálózati felhasználói felület és válassza ki a hello NSG.

## <a name="routes"></a>Útvonalak ##

Útvonalak váltak problémássá leggyakrabban a virtuális hálózat az Azure ExpressRoute konfigurálásakor. Az útvonalak a Vneten belül három típusa van:

-   Rendszerútvonalak
-   BGP-útvonalak
-   Felhasználó által definiált útvonalak (udr-EK)

BGP-útvonalakat a rendszer útvonalak bírálja felül. Udr-EK bírálja felül a BGP-útvonalakat. Az Azure virtuális hálózataihoz útvonalakkal kapcsolatban további információkért lásd: [felhasználó által definiált útvonalak áttekintése][UDRs].

hello Azure SQL-adatbázist, hogy hello ASE toomanage hello rendszer használja-e tűzfal van. A hello ASE nyilvános VIP kommunikációs toooriginate igényel. Kapcsolatok toohello SQL-adatbázis hello ASE elutasításra kerülne, ha elküldi őket hello ExpressRoute-kapcsolat és egy másik IP-cím.

Válaszok tooincoming felügyeleti kérések érkeznek hello ExpressRoute le, ha hello válasz címe különbözik hello eredeti célra. Ez az eltérés hello TCP kommunikációs megszakad.

A ASE toowork közben a virtuális hálózat része egy ExpressRoute hello legegyszerűbb dolog toodo esetén:

-   Konfigurálja az ExpressRoute tooadvertise _0.0.0.0/0_. Alapértelmezés szerint azt kényszerített bújtatás minden kimenő forgalom a helyszínen.
-   Hozzon létre egy UDR. Alkalmazza azt az egy címelőtagot hello ASE tartalmazó toohello alhálózati _0.0.0.0/0_ és következőugrás-típusú _Internet_.

Ha a módosítások két, a hello ASE alhálózati származó internet felé forgalom nem működik hello ExpressRoute és hello ASE kényszerített. 

> [!IMPORTANT]
> egy UDR definiált hello útvonalak bármely hello ExpressRoute-konfiguráció által hirdetett útvonalakat elég konkrétan fogalmaz ahhoz tootake sorrend kell lennie. hello előző példa hello széleskörű 0.0.0.0/0 címtartományt. Az esetlegesen véletlenül felülbírálhatja pontosabb címtartományai használó útvonal-hirdetéseinek.
>
> ASEs határokon hirdetési hello nyilvános társviszony elérési toohello privát társviszony elérési útvonalak ExpressRoute beállításokkal nem támogatottak. A nyilvános társviszony konfigurált ExpressRoute-konfigurációk útvonal-hirdetéseinek kapni a Microsofttól. a Microsoft Azure IP-címtartományok nagy hello hirdetményeket tartalmaz. Ha hello címtartományai határokon meghirdetett hello privát társviszony elérési úton, hello ASE tartozó alhálózati minden kimenő hálózati csomagok kényszerített bújtatott tooa az ügyfél a helyi hálózati infrastruktúra. A hálózati folyamat jelenleg nem támogatott a ASEs. Az egyik megoldás toothis probléma toostop cross-hirdetési útvonalak hello nyilvános társviszony elérési toohello privát társviszony elérési útról.

egy UDR toocreate kövesse az alábbi lépéseket:

1. Nyissa meg az Azure portál toohello. Válassza ki **hálózati** > **útvonaltáblát**.

2. Hozzon létre egy új útvonaltábla hello és a virtuális hálózat ugyanabban a régióban.

3. Az útvonaltábla felhasználói felületén belül jelölje ki **útvonalak** > **Hozzáadás**.

4. Set hello **a következő ugrás típusa** túl**Internet** és hello **címelőtag** túl**0.0.0.0/0**. Kattintson a **Mentés** gombra.

    Ekkor megjelenik az alábbihoz hasonló hello:

    ![Funkcionális útvonalak][6]

5. Hello új útvonaltábla létrehozása után nyissa meg a ASE tartalmazó toohello alhálózat. Válassza ki az útvonaltábla hello portal hello listájáról. Hello módosítás mentjük, majd megtekintheti az hello NSG-ket és ezekkel az alhálózati útvonalakat.

    ![Az NSG-k és útvonalak][7]

### <a name="deploy-into-existing-azure-virtual-networks-that-are-integrated-with-expressroute"></a>Üzembe helyezés meglévő Azure virtuális hálózataihoz ExpressRoute integrált ###

toodeploy be egy Vnetet, ExpressRoute, integrálva van a ASE hello ASE telepíteni kívánt hello alhálózati előkonfigurálására szolgálnak. A Resource Manager sablon toodeploy használja azt. a Vneten belül egy ASE toocreate már rendelkezik ExpressRoute konfigurálva:

- Hozzon létre egy alhálózat toohost hello ASE.

    > [!NOTE]
    > Nincs más lehet hello alhálózati, de hello ASE. Lehet, hogy toochoose, amely lehetővé teszi a jövőbeli növekedésre címteret. Később Ez a beállítás nem módosítható. Azt javasoljuk, hogy a méretet `/25` 128-címekkel.

- Hozzon létre udr-EK (például az útvonaltáblák), a fentebb leírt módon, és állítsa be, amely hello alhálózaton.
- Hello ASE létrehozása a Resource Manager sablonnal [egy ASE létrehozása egy Resource Manager-sablon használatával][MakeASEfromTemplate].

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png

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
[ASEManagement]: ./management-addresses.md
