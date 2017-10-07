---
title: "aaaCreating és a belső Terheléselosztók használatával az App Service-környezetek |} Microsoft Docs"
description: "Létrehozása és egy ASE használata egy ILB"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 20799f260993d6e81499408e5b547a2e21430174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>A belső Terheléselosztók használatával az App Service-környezet

> [!NOTE] 
> Ez a cikk hello App Service Environment-környezet v1 tárgya. App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve. több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).
>

hello App Service-környezet (ASE) szolgáltatás az Azure App Service egy továbbfejlesztett konfigurációs funkció, amely nem érhető el a hello több-bérlős bélyegzők letölti a Premium szolgáltatás lehetőség. hello ASE funkció tulajdonképpen hello Azure App Service szolgáltatásban az Azure virtuális Network(VNet) a telepíti. App Service Environment-környezetek megértése hello képességeket kínál toogain olvasási hello [Mi az az App Service-környezetek] [ WhatisASE] dokumentációját. Ha nem ismeri a Vneten belül működő hello előnyeit, olvassa el hello [Azure virtuális hálózat – gyakori kérdések][virtualnetwork]. 

## <a name="overview"></a>Áttekintés
Egy ASE is telepíthető, az interneten elérhető végpontok vagy a virtuális IP-címmel. Az order tooset hello IP-cím a tooa VNet cím toodeploy a ASE rendelkező egy belső terheléselosztási Balancer(ILB) van szüksége. Ha a ASE van adva egy ILB az megadnia:

* saját tartomány és altartomány. toomake egyszerűen, ez a dokumentum azt feltételezi, hogy Ön altartomány konfigurálható mindkét módszer. 
* a HTTPS-hez használt hello tanúsítvány
* DNS-kezelési a altartomány. 

Ismét műveleteket végezheti el, mint:

* állomás intranetes alkalmazások, például az üzletági alkalmazások biztonságosan hello a felhőalapú mely hozzáférést egy hely tooSite vagy ExpressRoute VPN
* állomás alkalmazások hello felhőben, amelyek nem szerepelnek a nyilvános DNS-kiszolgálók
* az előtér-alkalmazások biztonságosan integrálható elkülönített internet háttér alkalmazások létrehozása

#### <a name="disabled-functionality"></a>Letiltott funkció
Néhány dolgot, amely egy ILB ASE használatakor nem hajtható végre. Ezeket a beállításokat a következők:

* IPSSL használatával
* IP-címek hozzárendelése toospecific alkalmazások
* vételi és hello portálon keresztül egy alkalmazás olyan tanúsítványt használ. Természetesen továbbra is közvetlenül a hitelesítésszolgáltatót a tanúsítványok beszerzése, és az alkalmazásokat, nem csupán a hello Azure-portálon keresztül együtt használja azt.

## <a name="creating-an-ilb-ase"></a>Egy ILB ASE létrehozása
Egy ILB ASE létrehozása nincs hozzon létre egy ASE általában sokkal különböző. Egy ASE létrehozása mélyebb döntéseken olvasási [hogyan tooCreate az App Service-környezetek][HowtoCreateASE]. hello folyamat toocreate egy ILB ASE van hello ugyanaz a virtuális hálózat létrehozása ASE létrehozása során, vagy jelöljön ki egy már meglévő virtuális hálózat között. egy ILB ASE toocreate: 

1. Az Azure portálon válassza hello **új -> Web + mobil -> az App Service Environment-környezet**
2. Jelölje ki az előfizetését
3. Válassza ki vagy hozzon létre egy erőforráscsoportot
4. Válasszon vagy hozzon létre egy Vnetet
5. Hozzon létre egy alhálózatot, ha egy virtuális hálózat kiválasztása
6. Válassza ki **virtuális hálózati/hely VNet konfigurációja ->** és set hello VIP típus tooInternal
7. Adja meg (Ez lesz a ASE létrehozott alkalmazások használt hello altartomány) altartománynév
8. Kattintson az OK gombra, majd hozza létre

![][1]

Hello virtuális hálózat panel belül van egy virtuális hálózat konfigurációs beállítást. Ez lehetővé teszi egy külső VIP vagy VIP belső között választja. hello alapértelmezett érték a külső. Ha rendelkezik tooExternal állítva a ASE az interneten elérhető VIP fogja használni. Ha belső, a ASE egy ILB a Vneten belül egy IP-cím van konfigurálva. 

Belső kiválasztása, után több IP-címet ASE eltávolítják tooyour képességét tooadd hello, és ehelyett meg kell tooprovide hello altartománya hello ASE. Egy külső virtuális IP-hello a Service-környezetben hello hajlamosnak neve hello altartomány használatban adott ASE létrehozott alkalmazások. Ha a ASE hívták ***contosotest*** és az alkalmazás az adott ASE hívták ***mytest*** akkor hello altartomány hello formátum ***contosotest.p.azurewebsites.net*** és hello URL-címet az adott alkalmazáshoz lenne ***mytest.contosotest.p.azurewebsites.net***. Ha VIP típus tooInternal hello, a ASE neve nincs használatban hello altartomány a hello ASE. Hello altartomány explicit módon adja meg. Ha a altartomány ***contoso.corp.net*** és abban a ASE nevű alkalmazás végrehajtott ***timereporting*** majd hello URL-cím, mert a az alkalmazás ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Alkalmazás-Példánynak környezetben
Egy alkalmazás-Példánynak környezetben létrehozása a ugyanaz, mint az alkalmazások létrehozása a Service-környezetben általában hello. 

1. Hello Azure portálon válassza ki a **új -> Web + mobil -> webkiszolgáló** vagy **Mobile** vagy **API-alkalmazás**
2. Adja meg az alkalmazás nevét
3. Előfizetés kiválasztása
4. Válassza ki vagy erőforráscsoport létrehozása
5. Válassza ki, vagy hozzon létre az App Service Plan(ASP). Ha egy új ASP majd létrehozásához jelölje ki a ASE hello helyét és select hello feldolgozókészletek azt szeretné, a létrehozott ASP toobe. Hello ASP létrehozásakor kiválasztható a ASE hello helyét és hello munkavégző készletét. Látni fogja, hogy a hello altartomány hello alkalmazás hello nevét megadásakor a alkalmazásnév lép hello altartományt terheli a ASE. 
6. Válassza ki a létrehozása. Ki kell választania a hello **PIN-kód toodashboard** jelölőnégyzetet, ha azt szeretné, hello app tooshow fel az irányítópulton. 

![][2]

A hello app name hello altartománynév lekérdezi frissített tooreflect hello altartomány a hajlamosnak. 

## <a name="post-ilb-ase-creation-validation"></a>POST ILB ASE létrehozásának ellenőrzése
Egy ILB ASE mint hello nem - Példánynak ASE némileg eltérő. A már nincs jelezve toomanage a saját DNS-kiszolgáló van szüksége, és akkor is tooprovide HTTPS-kapcsolatok a tanúsítvány. 

Miután létrehozta a ASE láthatja, hogy hello altartomány és jeleníti meg a hello altartomány megadott hello egy új elem van **beállítás** nevű menü **ILB tanúsítvány**. egy önaláírt tanúsítványt, így azokat könnyebben tootest HTTPS hello ASE hozza létre. hello portál jelzi, hogy szüksége tooprovide saját tanúsítványt a HTTPS-hez, de ez az toodrive toohave olyan tanúsítvány, amely a altartomány együtt. 

![][3]

Ha egyszerűen próbált dolog és nem tudja, hogyan toocreate egy tanúsítványt, használhatja a hello IIS MMC konzol alkalmazás toocreate maga a tanúsítvány aláírására használatos. A már létrehozott egy .pfx fájlba exportálni, és majd töltse fel az hello ILB tanúsítvány felhasználói Felületét. Ha nincs lejáró toohello meggátoló toovalidate hello tanúsítvány biztonságos hozzáférést, a hely által védett egy önaláírt tanúsítványt, a böngésző kap figyelmezteti rá, hogy a hely elérésére hello. Ha azt szeretné, tooavoid adott figyelmeztetés van szüksége a megfelelő aláírt tanúsítvány, amely megfelel a altartomány, és nem ismeri fel a böngésző megbízhatósági láncot.

![][6]

Ha azt szeretné, tootry hello saját tanúsítványokkal flow, és a HTTP és HTTPS tooyour ASE teszteléséhez:

1. Lépjen a felhasználói felület tooASE ASE létrehozása után **ASE -> Beállítások -> ILB tanúsítványok**
2. Állítsa be a ILB tanúsítvány tanúsítvány pfx-fájljának kiválasztásával, és adja meg a jelszót. Közben tooprocess kissé veszi ezt a lépést, és a skálázási művelet van folyamatban, hello üzenet jelenik meg.
3. Hello ILB cím lekérése a ASE (**ASE Tulajdonságok ->-a virtuális IP-cím >**)
4. A webalkalmazás létrehozása az ASE létrehozása után 
5. Virtuális gép létrehozása, ha még nincs fiókja, hogy a virtuális (nem a hello azonos alhálózaton hello ASE vagy dolgot break)
6. Állítsa be a DNS a altartomány. Helyettesítő karakter használata a altartomány a DNS-ben, vagy toodo egyszerű tesztek, módosítsa a virtuális gép tooset web app name tooVIP IP-cím hello hosts fájlt. Ha a ASE hello altartománynév. ilbase.com és végzett hello web app mytestapp, hogy a volna kell címzett mytestapp.ilbase.com, majd állítsa be, amely a hosts fájl. (A Windows hello hosts fájl jelenleg C:\Windows\System32\drivers\etc\)
7. Egy böngészőt, hogy a virtuális gépen alkalmazza, és toohttp://mytestapp.ilbase.com (vagy bármilyen a webes alkalmazás neve a altartomány rendelkező)
8. Használja ezt a virtuális Gépet egy böngészőt, és folytassa a toohttps://mytestapp.ilbase.com tooaccept hello hiánya biztonsági lesz, ha egy önaláírt tanúsítványt használ. 

hello IP-címet a Példánynak van megadva a tulajdonságai, hello virtuális IP-cím

![][4]

## <a name="using-an-ilb-ase"></a>Egy ILB ASE használatával
#### <a name="network-security-groups"></a>Network Security Groups (Hálózati biztonsági csoportok)
Egy ILB ASE lehetővé teszi, hogy az alkalmazások hálózatelkülönítés, hello alkalmazások nem elérhetők, és akkor is ismert által hello internet. Ez a kiváló, például az üzletági alkalmazásokat az intranetes helyek üzemeltetéséhez. Ha szüksége van toorestrict még további továbbra is használhatja hálózati biztonsági Groups(NSGs) toocontrol hozzáférés hello szintjén. 

Ha toouse NSG-k toofurther korlátozza a hozzáférést, majd meg arról, hogy nem törés az hello kommunikációs toomake van szüksége, hogy hello ASE rendelés toooperate igényeinek. Annak ellenére, hogy hello HTTP/HTTPS-hozzáférés csak keresztül hello ILB által használt hello ASE hello ASE továbbra is hello virtuális hálózaton kívül erőforrás függ. milyen hálózati hozzáférés toosee továbbra is szükséges hello információk hello dokumentumban tekintse meg a [bejövő forgalom szabályozásának tooan App Service Environment-környezet] [ ControlInbound] és hello dokumentum [hálózati Az ExpressRoute App Service-környezetek konfigurációs adatait][ExpressRoute]. 

az NSG-k szüksége tooknow hello IP cím tooconfigure Azure toomanage használja a ASE. Az IP-címet esetén is hello kimenő IP-címet a ASE internetes kérelmek teszi. a ASE hello kimenő IP-cím statikus a hajlamosnak hello során változatlan marad. Ha töröl, és hozza létre újra a ASE, elérhetővé válik egy új IP-címet. túl nyissa meg az IP-cím toofind**beállítások -> Tulajdonságok** és hello található **kimenő IP-cím**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Általános ILB ASE kezelése
Egy ILB ASE kezelése van nagymértékben hello ugyanaz, mint egy ASE általában kezelése. Tooscale kell a munkavégző készletek toohost fel további ASP-példányokat, és a a HTTP/HTTPS-forgalmat előtérbeli kiszolgálók toohandle nagyobb mennyiségű méret. Egy hajlamosnak hello konfiguráció kezelése általános információkért olvassa el hello dokumentum a [az App Service-környezetek konfigurálása][ASEConfig]. 

hello további felügyeleti elemek tanúsítványkezelés és a DNS-kezelési. Tooobtain kell és ILB ASE létrehozása után a HTTPS-hez használt hello-tanúsítvány feltöltése, és cserélje le az Érvényesség lejárata előtt. Mivel Azure hello alap tartománnyal rendelkezik tanúsítványok is nyújtunk a ASEs egy külső virtuális IP-címre. Mivel egy ILB ASE által használt hello altartomány bármi lehet, kell tooprovide saját tanúsítvány HTTPS-hez. 

#### <a name="dns-configuration"></a>DNS-konfiguráció
Ha egy külső virtuális IP-hello Azure által kezelt DNS használatával. Bármely alkalmazás a ASE létre automatikusan tooAzure DNS, amely egy nyilvános DNS-ben. -Példánynak környezetben van toomanage a saját DNS-kiszolgáló. Egy adott altartomány contoso.corp.net például szüksége toocreate DNS A rögzíti pont tooyour ILB címet:

    * 
    *.SCM ftp-közzététel 


## <a name="getting-started"></a>Bevezetés
Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Lásd az App Service-környezetek lépései tooget [bemutatása tooApp Service-környezetek][WhatisASE]

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
