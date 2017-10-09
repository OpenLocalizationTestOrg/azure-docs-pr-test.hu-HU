---
title: "külön hálózatok és helyek összekötő csoportokat használnak az Azure AD alkalmazás Proxy aaaPublishing alkalmazásokhoz |} Microsoft Docs"
description: "Magában foglalja az hogyan toocreate és az Azure AD alkalmazásproxy összekötők csoportok kezelése."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 8c9a84b365eab28eaaeb343d4d1e2e6990537fec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Külön hálózatok és helyek összekötő csoportokat használnak az alkalmazások közzététele
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [klasszikus Azure portál](active-directory-application-proxy-connectors.md)
>

Az Azure AD-alkalmazásproxy több és több forgatókönyvek és az alkalmazások használatát. Így hajtottunk alkalmazásnak Proxy rugalmasabb még további topológiák engedélyezésével. Alkalmazásproxy-összekötő csoportokat hozhat létre, így rendelhet adott összekötők tooserve bizonyos alkalmazásokat. Ez a funkció lehetővé teszi az további ellenőrzési és módon toooptimize Application Proxy telepítéssel. 

Minden alkalmazásproxy-összekötőhöz tooa összekötő csoport van hozzárendelve. Az összes hello azonos összekötő csoport működni a magas rendelkezésre állás és a terheléselosztási külön egységként toohello tartozó összekötőket. Összekötők tooa összekötő csoport tartozik. Ne hozzon létre csoportokat, ha az összekötők alapértelmezett csoport vannak. A rendszergazda új csoportok létrehozása és hozzárendelése összekötők toothem a hello Azure-portálon. 

Minden alkalmazás tooa összekötő csoporthoz vannak rendelve. Ha nem hozza létre a csoportokat, majd az alkalmazások hozzárendelt tooa alapértelmezett csoport. De ha az összekötők csoportokba minden alkalmazás toowork megadhatja egy adott összekötőt csoporttal. Ebben az esetben csak hello összekötők csoport kiszolgálására hello alkalmazás kérésre. Ez a szolgáltatás akkor hasznos, ha az alkalmazások más-más helyen üzemeltetett. Összekötő csoportok alapján a hely, hozhat létre, így az alkalmazások mindig rendelkezésre, amelyek fizikailag Bezárás toothem összekötők által.

>[!TIP] 
>Ha nagy alkalmazásproxy-telepítést, ne rendeljen bármely alkalmazások toohello alapértelmezett összekötő csoport. Ezzel a módszerrel új összekötőket nem bármely élő forgalom fogadására amíg hozzá nem rendeli azokat tooan aktív összekötő csoport. Ez a konfiguráció lehetővé teszi egy tétlen üzemmódban tooput összekötők hátsó toohello alapértelmezett csoport áthelyezéssel, hogy a karbantartási a felhasználók befolyásolása nélkül végezheti el.

## <a name="prerequisites"></a>Előfeltételek
toogroup az összekötők toomake meg arról, hogy Ön [több összekötő telepítve](active-directory-application-proxy-enable.md). Új összekötő telepítésekor automatikusan bekerül hello **alapértelmezett** összekötő csoport.

## <a name="create-connector-groups"></a>Összekötő csoportok létrehozása
Használja ezeket a lépéseket toocreate összekötő csoportok. 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
1. Válassza ki **Azure Active Directory** > **vállalati alkalmazások** > **alkalmazásproxy**.
2. Válassza ki **új összekötő csoport**. hello új összekötő panel jelenik meg.

   ![Új összekötő csoport kiválasztása](./media/active-directory-application-proxy-connectors-azure-portal/new-group.png)

3. Nevezze el az új összekötő csoporthoz, majd hello legördülő menü tooselect az ebbe a csoportba tartozó összekötők használatával.
4. Kattintson a **Mentés** gombra.

## <a name="assign-applications-tooyour-connector-groups"></a>Alkalmazások tooyour összekötő csoportok hozzárendelése
Ezen lépések közzétett minden alkalmazáshoz az alkalmazásproxy. Amikor először közzététele, vagy ezek lépéseket toochange hello-hozzárendeléssel azt is meghatározhatja, bármikor tooa összekötő alkalmazáscsoport rendelhet hozzá.   

1. Hello kezelési irányítópult a címtáron, válassza ki **vállalati alkalmazások** > **összes alkalmazás** > tooassign tooa összekötő csoport kívánt alkalmazás hello >  **Alkalmazásproxy**.
2. Használjon hello **összekötő csoport** legördülő menü tooselect hello csoport kívánt alkalmazás toouse hello.
3. Válassza ki **mentése** tooapply hello módosítása.

## <a name="use-cases-for-connector-groups"></a>Összekötő csoportok alkalmazási helyzetei 

Összekötő csoportok hasznosak több, különböző esetekre, beleértve:

### <a name="sites-with-multiple-interconnected-datacenters"></a>A több összekapcsolt adatközpontokkal helyek

Számos szervezet rendelkezik több összekapcsolt adatközpontokban. Ebben az esetben érdemes tookeep mértékű forgalom hello adatközponton belül a lehető mert kereszt-datacenter hivatkozások drága, és a lassú. Minden egyes datacenter tooserve csak hello alkalmazásokhoz hello adatközponton belül összekötők telepítése. Ez a megközelítés minimálisra csökkenti a kereszt-datacenter hivatkozások, és teljesen átlátszó élményt nyújt akkor tooyour felhasználók.

### <a name="applications-installed-on-isolated-networks"></a>Elkülönített hálózatok a telepített alkalmazások

Alkalmazások hálózatokban, amelyek nincsenek hello fő vállalati hálózat részei lehet üzemeltetni. Összekötő csoportok dedikált tooinstall összekötők elkülönített hálózatok tooalso elkülönítheti alkalmazások toohello hálózaton használható. Ez általában akkor fordul elő, ha egy külső gyártó kezeli a szervezet számára az adott alkalmazást. 

Összekötő csoportok lehetővé teszik a akkor tooinstall dedikált ezekhez a hálózatokhoz, amely csak bizonyos alkalmazásokat, így megkönnyíti közzétenni az összekötők és biztonságosabb toooutsource Alkalmazáskezelés, toothird féltől.

### <a name="applications-installed-on-iaas"></a>Infrastruktúra-szolgáltatási a telepített alkalmazások 

Összekötő csoportok felhőalapú férhetnek hozzá az infrastruktúra-szolgáltatási a telepített alkalmazások, adja meg a közös szolgáltatás toosecure hello hozzáférés tooall hello alkalmazások. Összekötő csoportok nem további függőség létrehozása a vállalati hálózaton, vagy darabolható hello felhasználói élményt. Összekötők minden felhőbeli adatközpontot is telepíthető, és csak erre a hálózatra telepített alkalmazások kiszolgálására. Több összekötők tooachieve magas rendelkezésre állású telepítése.

Olyan szervezet, amely több virtuális gépet csatlakoztatott tootheir saját üzemeltetett IaaS virtuális hálózati példaként érvénybe. az alkalmazottak toouse tooallow ezek az alkalmazások e magánhálózatok csatlakoztatott toohello vállalati hálózati telephelyek közötti VPN használatával. A megfelelő környezet biztosít az alkalmazottak, amelyek a helyszínen. Azonban nem lehet ideális, ha távoli alkalmazottak, mert további helyszíni infrastruktúra tooroute hozzáférés, ahogy az alábbi ábrán hello látja:

![Infrastruktúra-szolgáltatási hálózati AzureAD](./media/application-proxy-publish-apps-separate-networks/application-proxy-iaas-network.png)
  
Azure AD alkalmazásproxy-összekötő csoportokkal engedélyezheti a közös szolgáltatás toosecure hello tooall alkalmazásokat a vállalati hálózaton további függőség létrehozása nélkül:

![AzureAD infrastruktúra több felhőalapú szállítók](./media/application-proxy-publish-apps-separate-networks/application-proxy-multiple-cloud-vendors.png)

### <a name="multi-forest--different-connector-groups-for-each-forest"></a>Többerdős – különböző összekötő csoportok az egyes erdőkhöz

A legtöbb ügyfelek, akik alkalmazásproxy telepítették az egyszeri bejelentkezéses (SSO) képességeket használ Kerberos által korlátozott delegálás (KCD) elvégzésével. tooachieve ez hello csatlakozó gépek kell toobe illesztett tooa tartományhoz, amely delegálhatja hello felhasználók hello alkalmazás felé. Kerberos által korlátozott Delegálás támogatja az erdők közötti képességeit. De különböző Többerdős környezetben egymás között nincs megbízhatóság rendelkező vállalatok esetében egyetlen összekötőt nem használható az összes erdőben. 

Ebben az esetben adott összekötők erdőnként is telepíthető, és a közzétett tooserve volt beállítva tooserve alkalmazások csak a felhasználók az adott erdőben hello. Minden egyes összekötőt csoport egy másik erdőben jelöli. Hello bérlő és a legtöbb hello élmény az összes erdőben van egyesített, amíg a felhasználók tootheir erdő az alkalmazásoknak az Azure AD-csoportok rendelhetők.
 
### <a name="disaster-recovery-sites"></a>Vész-helyreállítási helyeken

Kétféleképpen különböző egy vész-helyreállítási helyen, attól függően, hogy a helyek kialakításával hogyan hajthatók végre:

* A vész-Helyreállítási hely épül, ahol pontosan például hello fő helye van, és rendelkezik aktív-aktív módban hello azonos hálózati és az AD-beállítások, létrehozhat hello hello vész-Helyreállítási hely hello összekötők azonos összekötő csoport hello fő helyként. Ez lehetővé teszi, hogy az Azure AD toodetect feladatátvételek meg.
* Ha a vész-Helyreállítási hely külön hello fő helyről, létrehozhat egy másik összekötő csoportot hello vész-Helyreállítási helyen, és akár 1.) kell biztonsági mentést végző alkalmazások vagy a 2.) manuálisan átirányít az hello meglévő toohello vész-Helyreállítási összekötő alkalmazáscsoport igény szerint.
 
### <a name="serve-multiple-companies-from-a-single-tenant"></a>Több vállalat kiszolgálására egyetlen bérlőtől

A modell egyetlen szolgáltatót telepíti és kezeli az Azure AD tooimplement kapcsolódó több vállalatot-szolgáltatások számos különböző módja van. Összekötő csoportok segítségével hello összekötők és az alkalmazások különböző csoportok elkülönítse Üdvözöljük a rendszergazdákat. Van toohave egyetlen egyik módja, amely megfelelő kisvállalkozások, míg a hello a különböző vállalatok a saját tartomány neve és a hálózatok Azure AD-bérlő. Ez igaz is M & A forgatókönyvek és helyzetekben ahol egyetlen IT-részleg szolgál több vállalat szabályozási és üzleti okokból. 

## <a name="sample-configurations"></a>A minta-konfigurációk

Amely is alkalmazható, például a következő összekötő-csoportok hello.
 
### <a name="default-configuration--no-use-for-connector-groups"></a>Alapértelmezett konfigurációja – összekötő csoportok nem használható

Ha nem adja meg összekötő csoportok, a konfiguráció néznek ki:

![AzureAD összekötő csoport](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-1.png)
 
Ez a konfiguráció is elegendő a kisebb telepítésekhez és teszteket. Is, ha a szervezete egy egyszerű hálózati topológia fog működni.
 
### <a name="default-configuration-and-an-isolated-network"></a>Alapértelmezett konfiguráció és az elkülönített hálózat

Ez a konfiguráció még hello alapértelmezett, ahol van egy adott alkalmazást, amely egy elkülönített hálózaton, például a virtuális hálózati infrastruktúra fut: 

![AzureAD összekötő csoport](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-2.png)
 
### <a name="recommended-configuration--several-specific-groups-and-a-default-group-for-idle"></a>Ajánlott konfiguráció – több meghatározott csoportok és egy alapértelmezett csoport üresjárati

a nagy és összetett konfigurációs ajánlott hello toohave hello alapértelmezett összekötő csoport, mint egy csoportot, amely lehessen az alkalmazásokat, és inaktív vagy újonnan telepített összekötők használható. Minden alkalmazás szolgáltatott egyéni összekötő csoportok használatával. Ez lehetővé teszi, hogy a fent leírt hello forgatókönyvek összes hello összetettsége.

Hello az alábbi példában hello vállalati van két adatközpont, A és B, az átadott minden hely két összekötőt. Minden hely rendelkezik a különböző alkalmazások, amelyek akkor futnak. 

![AzureAD összekötő csoport](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-3.png)
 
## <a name="next-steps"></a>Következő lépések

* [Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)
* [Egyszeri bejelentkezés engedélyezése](application-proxy-sso-overview.md)


