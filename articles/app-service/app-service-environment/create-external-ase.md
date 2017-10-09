---
title: "aaaCreate külső Azure App Service-környezet"
description: "Azt ismerteti, hogyan toocreate közben App Service-környezet létrehozása egy alkalmazás vagy önálló"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: f8619534ddd889ea65063733ac6ec11b206e799c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-external-app-service-environment"></a>Külső App Service-környezet létrehozása #

Az Azure App Service Environment-környezet az Azure App Service egy Azure virtuális hálózatot (VNet) lévő alhálózatot történő központi telepítését. Nincsenek két módon toodeploy az App Service-környezetek (ASE):

- A virtuális IP-címre a külső IP-cím, egy külső ASE gyakran nevezik.
- Hello VIP, a belső IP-cím, az úgynevezett egy ILB ASE mert hello belső végpont egy belső terheléselosztón (ILB).

Ez a cikk bemutatja, hogyan toocreate egy külső ASE. Hello ASE áttekintését lásd: [egy bevezető toohello App Service Environment-környezet][Intro]. Hogyan toocreate egy ILB ASE, tekintse meg az információkat [létrehozása és használata egy ILB ASE][MakeILBASE].

## <a name="before-you-create-your-ase"></a>A ASE létrehozása előtt ##

Miután létrehozta a ASE, hello következő nem módosíthatja:

- Hely
- Előfizetés
- Erőforráscsoport
- A virtuális hálózaton használt
- Használt
- Alhálózat mérete

> [!NOTE]
> Ha egy virtuális hálózat kiválasztása, és adjon meg egy alhálózatot, győződjön meg arról, hogy elég nagy tooaccommodate jövőbeli növekedésre. Azt javasoljuk, hogy a méretet `/25` 128-címekkel.
>

## <a name="three-ways-toocreate-an-ase"></a>Háromféleképpen toocreate egy ASE ##

Három módon toocreate egy ASE van:

- **Az App Service-csomag létrehozása során**. Ez a módszer egy lépésben hello ASE és hello App Service-csomagot hoz létre.
- **Önálló műveletként**. Ez a módszer hoz létre önálló ASE, ez az egy a lapokat ASE. Ez a módszer egy speciális folyamat toocreate egy ASE. Használata egy ASE toocreate egy Példánynak.
- **Az Azure Resource Manager sablon**. Ez a módszer van a tapasztalt felhasználók számára. További információkért lásd: [egy ASE létrehozása sablonból][MakeASEfromTemplate].

Egy külső ASE rendelkezik nyilvános virtuális IP-címhez, ami azt jelenti, hogy minden HTTP/HTTPS-forgalom toohello alkalmazás hello ASE a találatok internetről elérhető IP-címet. Egy ASE egy ILB rendelkező rendelkezik hello alhálózati hello ASE által használt IP-címeit. hello ILB Service-környezetben üzemeltetett alkalmazásokhoz nem kitett közvetlenül toohello internet.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Hozzon létre egy ASE és az App Service-csomag együtt ##

App Service-csomag hello egy olyan tároló, az alkalmazások. Ha alkalmazást hoz létre az App Service-ben, akkor válasszon, vagy hozzon létre az App Service-csomag. hello tároló modell környezetek tartsa App Service-csomagok, és az App Service-csomagok tárolására az alkalmazások.

toocreate egy ASE az App Service-csomag létrehozása során:

1. A hello [Azure-portálon](https://portal.azure.com/), jelölje be **új** > **Web + mobil** > **webalkalmazás**.

    ![Webalkalmazás létrehozása][1]

2. Válassza ki előfizetését. hello app valamint hello ASE hoz létre a hello azonos előfizetések.

3. Válasszon ki vagy hozzon létre egy erőforráscsoportot. Az erőforráscsoportok egységként kezelheti a kapcsolódó Azure-erőforrások. Erőforráscsoportok is hasznosak, ha az alkalmazások szerepköralapú hozzáférés-vezérlés szabályok létrehozása. További információkért lásd: hello [Azure Resource Manager áttekintése][ARMOverview].

4. Hello App Service-csomagra, majd válassza ki és **hozzon létre új**.

    ![Új App Service-csomag][2]

5. A hello **hely** legördülő listából válassza ki, ahová toocreate hello ASE válassza hello régió. Ha egy meglévő ASE, egy új ASE nem jön létre. App Service-csomag hello hello kiválasztott ASE jön létre. 

6. Válassza ki **tarifacsomag**, és válassza ki az egyiket hello **elszigetelt** termékváltozatok árképzési. Ha úgy dönt, egy **elszigetelt** SKU kártya és olyan helyre, amely nem egy ASE, egy új ASE azon a helyen jön létre. Válassza ki a toostart hello folyamat toocreate egy ASE **válasszon**. Hello **elszigetelt** SKU csak egy mértékéig párhuzamosan érhető el. Is nem használhatja semmilyen más árképzési SKU-környezetben eltérő **elszigetelt**.

    ![Tarifacsomag kiválasztása][3]

7. Adja meg a ASE hello nevét. Ez a név hello megcímezhető nevét az alkalmazások használatos. Ha hello ASE hello neve _appsvcenvdemo_, hello tartománynév *. appsvcenvdemo.p.azurewebsites.net*. Ha nevű alkalmazást hoz létre *mytestapp*, megcímezhető: mytestapp.appsvcenvdemo.p.azurewebsites.net. Szóköz hello nevében nem használható. Ha használhatnak nagybetűket, hello tartománynév hello teljes kis verziója ugyanez a neve.

    ![Új App Service-csomag neve][4]

8. Adja meg az Azure virtuális hálózati adatait. Válassza ki vagy **új** vagy **válasszon meglévő**. hello beállítás tooselect egy meglévő virtuális hálózatot áll rendelkezésre, csak akkor, ha egy virtuális hálózat rendelkezik hello a kiválasztott régióban. Ha **hozzon létre új**, adjon meg egy nevet a virtuális hálózat hello. Ilyen nevű új erőforrás-kezelő VNet létrejön. Hello címterület használ `192.168.250.0/23` hello a kiválasztott régióban. Ha **meglévő**, kell:

    a. Hello virtuális hálózat címterülete, akkor válassza, ha egynél több.

    b. Adjon meg egy új alhálózat neve.

    c. Válassza ki a hello alhálózati hello méretét. *Ne felejtse el egy méretének elég nagy tooaccommodate jövőbeli növekedésének megfelelően a ASE tooselect.* Ajánlott `/25`, amely 128-címekkel rendelkezik, és kezelni tud a maximális méretű ASE. Nem ajánlott `/28`, például mert csak 16 címek érhetők el. Infrastruktúra legalább öt címet használ. Az egy `/28` alhálózathoz folyamatban hagyta a 11 példányok maximális skálázás.

    d. Válassza ki a hello alhálózat IP-címtartományt.

9. Válassza ki **létrehozása** toocreate hello ASE. A folyamat is hello App Service-csomag és hello alkalmazást hoz létre. hello ASE, az App Service-csomag, és az alkalmazás összes alatt állnak hello ugyanahhoz az előfizetéshez, és emellett a hello azonos erőforráscsoportot. Ha a ASE kell egy külön erőforráscsoportot, vagy ha egy ILB ASE van szüksége, kövesse a hello lépéseket toocreate egy ASE önmagában.

## <a name="create-an-ase-by-itself"></a>Hozzon létre egy ASE önmagában ##

Ha létrehoz egy ASE önálló, lejárt, semmi nem azt. Egy üres ASE továbbra is azt eredményezi azok háromszorosa hello infrastruktúra havi járnak. Kövesse ezeket lépéseket toocreate egy ASE egy ILB az vagy egy ASE toocreate saját erőforráscsoportban. Miután létrehozta a ASE, létrehozhat alkalmazások azt hello szokásos folyamat használatával. Válassza ki az új ASE hello helyként.

1. Keresési hello Azure piactérre **App Service Environment-környezet**, vagy válasszon **új** > **webes mobil** > **App Service Környezet**. 

2. Adja meg a ASE hello nevét. Ez a név hello ASE létrehozott hello alkalmazásokhoz használható. Ha hello neve *mynewdemoase*, hello altartománynév van *. mynewdemoase.p.azurewebsites.net*. Ha nevű alkalmazást hoz létre *mytestapp*, megcímezhető: mytestapp.mynewdemoase.p.azurewebsites.net. Szóköz hello nevében nem használható. Ha használhatnak nagybetűket, hello tartománynév hello teljes kis verziója hello nevét. Egy ILB használatakor a ASE neve nem szerepel a altartomány, de ehelyett explicit módon megadott ASE létrehozása során.

    ![ASE elnevezése][5]

3. Válassza ki előfizetését. Ez az előfizetés esetében is hello hello ASE összes alkalmazást használni. A ASE nem helyezhető el, amely egy másik előfizetésben található a Vneten belül.

4. Válassza ki vagy adja meg egy új erőforráscsoportot. a ASE használt hello erőforráscsoport hello azonos egy, a virtuális hálózat használt kell lennie. Ha egy meglévő Vnetet, hello erőforráscsoport kiválasztása a ASE a frissített tooreflect-e, a virtuális hálózat. *Egy erőforráscsoport, amely nem azonos a hello VNet erőforráscsoportot, egy Resource Manager-sablon egy ASE hozhatja létre.* egy ASE toocreate sablonból, lásd: [App Service-környezet létrehozása sablonból][MakeASEfromTemplate].

    ![Erőforráscsoport kiválasztása][6]

5. Válassza ki a virtuális hálózat és a helyet. Hozzon létre új virtuális hálózatot, vagy válasszon egy meglévő virtuális hálózatot: 

    * Ha új virtuális hálózatot választja, megadhatja a nevét és helyét. hello új virtuális hálózat tartozik hello cím tartomány 192.168.250.0/23 és alapértelmezett nevű alhálózat. hello alhálózati 192.168.250.0/24 típusúként van definiálva. Egy erőforrás-kezelő virtuális hálózat jelölhet ki. Hello **VIP típus** kijelölés határozza meg, ha a ASE közvetlenül elérhető az internet (külső) hello, vagy ha egy ILB használ. További információ a kapcsolókról, toolearn lásd [létrehozása és használata az App Service-környezet belső terheléselosztót][MakeILBASE]. 

      * Ha **külső** hello a **VIP-típus**, kiválaszthatja az IP-címeket hány külső hello rendszere IP-alapú SSL célokra jön létre. 
    
      * Ha **belső** a hello **VIP típus**, meg kell adnia a ASE használó hello tartományhoz. Egy ASE egy virtuális hálózat által használt nyilvános és magánhálózati címtartományai történő telepítése. egy Vnetet egy nyilvános-címtartománnyal rendelkező toouse, meg kell toocreate hello VNet időben. 
    
    * Ha egy meglévő virtuális hálózatot választ, egy új alhálózat létre van hozva hello ASE létrehozásakor. *Nem használhat egy korábban létrehozott alhálózati hello portálon. Létrehozhat egy ASE egy létező alhálózatot a Resource Manager-sablon használatakor.* egy ASE toocreate sablonból, lásd: [egy App Service Environment-környezet létrehozása sablonból][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service-környezet v1 ##

App Service Environment-környezet (ASEv1) első verziójában hello példányait továbbra is létrehozhatja. amely feldolgozza, a keresési hello piactér toostart a **App Service Environment-környezet v1**. Hello hello ASE hoz létre, hogy hozzon létre hello önálló ASE azonos módon. Amikor elkészült, a ASEv1 van két előtér-webkiszolgálóinak és két alkalmazottak. A ASEv1 hello előtér-webkiszolgálóinak és alkalmazottak kell kezelni. Azok még nem automatikusan az App Service-csomagok létrehozásakor. hello előtér-webkiszolgálóinak hello HTTP/HTTPS-végpontként személyekről, és a forgalom küldése toohello munkavállalók. hello munkavállalók hello szerepkör, az alkalmazásokat futtató is. A ASE létrehozása után módosíthatja is az előtér-webkiszolgálóinak és alkalmazottak hello mennyisége. 

toolearn ASEv1, kapcsolatos további információkért lásd: [bemutatása toohello App Service Environment-környezet v1][ASEv1Intro]. További információ a méretezés, kezelésére és ASEv1, figyelés: [hogyan tooconfigure az App Service-környezetek][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png



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
