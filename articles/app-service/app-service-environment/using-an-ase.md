---
title: "az Azure App Service-környezetek aaaUse"
description: "Hogyan toocreate, közzétételét és alkalmazásokat az Azure App Service-környezet skálázása"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 30c89e384efc07c560254856c0ca7d4eb4b1f010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-app-service-environment"></a>App Service-környezet használata #

## <a name="overview"></a>Áttekintés ##

Az Azure App Service Environment-környezet az Azure App Service egy alhálózatba Azure virtuális hálózatban egy ügyfél központi telepítését. Áll:

- **Előtérkiszolgáló-végpontok**: hello előtér-webkiszolgálóinak, ahol a HTTP/HTTPS egy App Service-környezetben (ASE) leáll.
- **Feldolgozók**: hello munkavállalók az alkalmazásokat futtató hello erőforrások.
- **Adatbázis**: hello adatbázis tárolja az információt, amely meghatározza a hello környezet.
- **Tárolási**: hello tároló használt toohost hello ügyfél közzétett alkalmazások.

> [!NOTE]
> App Service Environment-környezet két verziója van: ASEv1 és ASEv2. ASEv1 hello erőforrásokat kell kezelni, mielőtt használhatná azokat. toolearn hogyan tooconfigure és ASEv1 kezelése című [konfigurálása egy App Service-környezet v1][ConfigureASEv1]. Ez a cikk többi hello ASEv2 összpontosít.
>
>

Telepíthet egy ASE (ASEv1 és ASEv2) egy külső vagy belső virtuális IP-címre az alkalmazás eléréséhez. egy külső virtuális IP-címre hello telepítési gyakran nevezik egy külső ASE. hello belső verzió hello ILB ASE nevezik, mert egy belső terheléselosztón (ILB) használ. toolearn hello ILB ASE kapcsolatos további információkért lásd: [létrehozása és használata egy ILB ASE][MakeILBASE].

## <a name="create-a-web-app-in-an-ase"></a>Webalkalmazás létrehozása a Service-környezetben ##

toocreate egy web app Service-környezetben, használja ugyanezt a folyamatot hello létrehozásakor ez általában megegyezik, azonban néhány kisebb különbség. Mikor hozzon létre egy új App Service-csomagot:

- Földrajzi hely kiválasztása az melyik toodeploy az alkalmazás, helyett egy ASE az helyeként válassza.
- -Környezetben létrehozott összes App Service-csomagok IP-címek egy elszigetelt kell lennie.

Ha egy ASE nincs, akkor létrehozhat hello utasításait követve [App Service-környezet létrehozása][MakeExternalASE].

toocreate egy web app Service-környezetben:

1. Válassza ki **új** > **Web + mobil** > **webalkalmazás**.

2. Adja meg a hello a webalkalmazás nevét. Ha már ki az App Service-csomag Service-környezetben, hello tartománynév hello alkalmazás tükrözi hello ASE hello tartománynevet.

    ![Webes alkalmazás nevének kiválasztása][1]

3. Válasszon egy előfizetést.

4. Adjon meg egy új erőforráscsoport nevét, vagy válasszon **meglévő** , és válassza ki a hello legördülő listából.

5. Válassza ki a ASE meglévő App Service-csomagot, vagy hozzon létre egy újat az alábbiak szerint:

    a. Válassza ki **új**.

    b. Adja meg az App Service-csomag hello nevét.

    c. Válassza ki a ASE hello **hely** legördülő listából.

    d. Válasszon egy **elszigetelt** tarifacsomagra vált. Válassza ki **válasszon**.

    e. Kattintson az **OK** gombra.
    
    ![Elkülönített tarifacsomagok][2]

6. Kattintson a **Létrehozás** gombra.

## <a name="how-scale-works"></a>Hogyan méretezése működik ##

Az App Service-csomag minden App Service-alkalmazást futtat. hello tároló modell környezetek App Service-csomagok tárolására, de App Service-csomagok tárolására az alkalmazások. Akkor az alkalmazások, hello App Service-csomag vertikális, és így minden hello alkalmazás méretezése hello csomagot.

A ASEv2 akkor az App Service-csomag hello szükséges infrastruktúra automatikusan kerül. Nincs késleltetés tooscale műveletek során hello infrastruktúra kerül. A ASEv1 hello szükséges infrastruktúra hozzá kell adnia hoz létre, vagy az App Service-csomag kiterjesztése. 

Hello több-bérlős App Service, a méretezés oka általában azonnali erőforrás azonnal elérhetők legyenek toosupport azt. -Környezetben nincs ilyen puffer, és szükség esetén erőforrásokat.

-Környezetben költenie too100 példányok. 100 logikailemez egy egyetlen App Service-csomag összes lehetnek, vagy több App Service-csomagokról pontjain.

## <a name="ip-addresses"></a>IP-címek ##

App Service rendelkezik hello képességét tooallocate egy dedikált IP-cím tooan alkalmazást. Ez a funkció érhető el az IP-alapú SSL konfigurálása után a [kötése egy meglévő egyéni SSL tanúsítvány tooAzure webalkalmazások][ConfigureSSL]. -Környezetben van azonban a figyelmet a jelentősebb kivételt. Nem adhat hozzá további IP-címek az IP-alapú SSL-Példánynak környezetben használt toobe.

A ASEv1 tooallocate hello IP-címek erőforrásként előtt kell azokat. ASEv2 használhatja őket az alkalmazásból, mint hogy az App Service hello több-bérlős. Nincs mindig egy tartalék cím ASEv2 too30 IP-címeit. Minden alkalommal, amikor valamelyik használja, egy másik jelenik meg, hogy egy címet mindig könnyen hozzáférhető legyen. A késés idő szükséges tooallocate egy másik IP-címet, amely megakadályozza a hozzáadása IP címek gyors egymásutánban.

## <a name="front-end-scaling"></a>Előtér-skálázás ##

A ASEv2, amikor az App Service-csomagokról a horizontális munkavállalók automatikusan toosupport őket. Minden ASE két előtér-webkiszolgálóinak hozza létre. Ezenkívül hello előtér-webkiszolgálóinak automatikusan kiterjesztése minden 15 példányok egy előtér sebességgel az App Service-csomagokról. Például ha 15, akkor rendelkezik három előtér-webkiszolgálóinak. Ha vertikális too30 példányok, majd, hogy négy első befejeződik, és így tovább.

Ez a szám előtér-webkiszolgálóinak bőven elegendő a legtöbb forgatókönyvek kell lennie. Azonban ki lehet terjeszteni gyorsabban. Minden öt példányok egy előtér kevés tooas hello arány módosíthatja. Nincs hello arány módosítására járnak. További információkért lásd: [Azure App Service szolgáltatás díjszabása][Pricing].

Előtér-erőforrások hello HTTP/HTTPS-végpont hello ASE a rendszer. Hello alapértelmezett előtér-konfigurációval előtér / memóriahasználatát érték következetesen körülbelül 60 %. Ügyfél munkaterheléseinek előtér nem futtatható. hello kulcsfontosságú tényező a tekintetben tooscale rendelkező előtér hello CPU, amelyek célja a elsősorban HTTPS-forgalmat.

## <a name="app-access"></a>Alkalmazás-hozzáférés ##

-Külső környezetben alkalmazások létrehozásakor használt hello tartomány eltér a hello több-bérlős App Service. Ez magában foglalja a hello ASE hello nevét. További információt a toocreate egy külső ASE, lásd: [App Service-környezet létrehozása][MakeExternalASE]. hello tartománynév-külső környezetben a következőképpen néz *.&lt; asename környezet&gt;. p.azurewebsites.net*. Például, ha a ASE nevű _külső-ase_ és nevű alkalmazás működteti _contoso_ az adott ASE, lépjen a következő URL-címek hello:

- contoso.external-ase.p.azurewebsites.net
- contoso.SCM.external-ase.p.azurewebsites.net

hello URL-cím contoso.scm.external-ase.p.azurewebsites.net használt tooaccess hello Kudu konzol vagy a közzétételt a webes alkalmazás telepítése. A Kudu konzolon hello információkért lásd: [Kudu konzol az Azure App Service-][Kudu]. hello Kudu konzol lehetővé teszi egy webes felhasználói felület hibakeresés, fájlok feltöltése, szerkesztheti, és még sok más.

-Példánynak környezetben hello tartomány központi telepítéskor meghatározásához. További információ a hogyan toocreate egy ILB ASE, lásd: [létrehozása és használata egy ILB ASE][MakeILBASE]. Ha hello tartománynevet ad meg _ilb-ase.info_, alkalmazások hello abban a ASE használja, hogy a tartomány létrehozása során. Hello alkalmazás nevű _contoso_, hello URL-címei:

- contoso.ilb-ase.info
- contoso.SCM.ilb-ase.info

## <a name="publishing"></a>Közzététel ##

Hello több-bérlős App Service, a Service-környezetben is közzéteheti a:

- A webes telepítése.
- AZ FTP.
- Folyamatos integrációt.
- Adatértékmezők áthúzása hello Kudu konzolon.
- Egy IDE, mint például a Visual Studio, az eclipse-ben vagy az IntelliJ IDEA.

Egy külső mértékéig összes viselkednek a közzétételi beállítások hello azonos. További információkért lásd: [telepítése az Azure App Service][AppDeploy]. 

hello fő különbség a közzététel a tekintetben tooan ILB ASE jelenti. Egy ILB mértékéig hello közzétételi végpontok érhetők el minden csak hello ILB keresztül. hello ILB hello ASE alhálózat privát IP-van hello virtuális hálózaton. Ha hálózati hozzáférési toohello ILB nincs, olyan alkalmazások, hogy ASE nem tehető közzé. Leírtaknak megfelelően [létrehozása és használata egy ILB ASE][MakeILBASE], tooconfigure DNS hello alkalmazások hello rendszer szükséges. Hello SCM végpontot, amely tartalmaz. Ha ezek még nincs megfelelően beállítva, nem tehető közzé. A IDEs is kell toohave hálózati hozzáférési toohello ILB rendelés toopublish közvetlenül tooit.

Internet alapú CI rendszereknek, például a Githubon és a Visual Studio Team Services, egy ILB mértékéig nem működnek, mert hello közzétételi végpont nem érhető el az Internet. Ehelyett egy CI rendszer, amely lekéréses modellt használ, például a Dropbox toouse van szüksége.

hello közzétételi végpontok-Példánynak környezetben alkalmazások használata hello tartomány adott hello ILB ASE hozták létre. Hello alkalmazás közzétételi profil és a hello alkalmazás portálpaneljéhez láthatja (a **áttekintése** > **Essentials** valamint **tulajdonságok**). 

## <a name="pricing"></a>Díjszabás ##

nevű SKU árképzési hello **elszigetelt** csak ASEv2 való használatra készült. Minden ASEv2 tárolt App Service-csomagokról a hello elszigetelt árképzési Termékváltozat. App Service-csomag elkülönített díjszabás régiónként eltérőek lehetnek. 

Ezenkívül toohello ár az App Service-csomagok, és van egy simán arány ASE magát. hello simán arány nem változik a ASE hello mérettel és fizet hello ASE infrastruktúra, az alapértelmezett 1 további aránya skálázás előtér minden 15 App Service-csomag példányok.  

Ha hello alapértelmezett méretezési minden 15 App Service-csomag példányok 1 előtér mérték nem elég gyors, módosíthatja a hello arány első vége hozzáadott vagy hello hello első-végpontok méretét.  Ha módosítja a hello arány vagy mérete, kell fizetnie hello előtér-mag, amely alapértelmezés szerint nem lesz hozzáadva.  

Például ha hello méretezési arány too10, előtér megjelenik minden 10 példányok az App Service-csomagokról. hello strukturálatlan díj hozzá van rendelve egy előtér minden 15-példányok méretezési mértéke. A 10-es méretarány díjat hello harmadik előtér, amely hozzá van adva a hello 10 App Service-csomag példányok fizetnie. Az toopay nincs szükség a 15 példányok elérésekor, mert automatikusan hozzáadják.

Ha hello méretet hello első-végpontok too2 magszámra vonatkozó követelmény, de nem módosíthatja hello arány, akkor kell fizetnie hello extra magok.  Egy ASE 2 első részek, így még akkor is hello automatikus méretezési küszöbérték akkor kell fizetnie 2 extra mag Ha növeli a hello mérete too2 core első-végpontok alatt hozza létre.

További információkért lásd: [Azure App Service szolgáltatás díjszabása][Pricing].

## <a name="delete-an-ase"></a>Egy ASE törlése ##

egy ASE toodelete: 

1. Használjon **törlése** hello hello tetején **App Service Environment-környezet** panelen. 

2. A megjeleníteni kívánt toodelete ASE tooconfirm hello nevét adja meg azt. Ha töröl egy ASE, akkor törölje az összes hello tartalmakra azt is. 

    ![ASE törlése][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


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
