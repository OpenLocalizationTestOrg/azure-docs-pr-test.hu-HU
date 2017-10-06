---
title: "aaaSingle bejelentkezés az alkalmazásproxy |} Microsoft Docs"
description: "Ismerteti, hogyan tooprovide egyszeri bejelentkezés az Azure AD-alkalmazásproxy használatával."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: ded0d9c9-45f6-47d7-bd0f-3f7fd99ab621
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 0047e834cd42e057a75ebc0c5dcf860734464a05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-tooyour-apps-with-application-proxy"></a>Kerberos által korlátozott delegálás egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval

Egyszeri bejelentkezés biztosítható a helyszíni alkalmazások proxyn keresztül történő titkosított integrált Windows-hitelesítésre van közzétéve. Ezek az alkalmazások a Kerberos jegy megkövetelése a hozzáféréshez. Alkalmazásproxy használ Kerberos által korlátozott delegálás (KCD) toosupport ezeket az alkalmazásokat. 

Integrált Windows-hitelesítéssel (IWA) használó alkalmazásproxy összekötők engedélyt adjon tooimpersonate Active Directory-felhasználók az egyszeri bejelentkezés tooyour alkalmazások engedélyezéséhez. hello összekötők ezen engedély toosend használja, és a nevében jogkivonatokat fogadni.

## <a name="how-single-sign-on-with-kcd-works"></a>Hogyan egyszeri bejelentkezéshez a Kerberos által korlátozott Delegálás works
Ez az ábra ismerteti hello folyamat, amikor egy felhasználó próbál tooaccess integrált Windows-Hitelesítést használó helyszíni alkalmazások.

![Microsoft AAD hitelesítési folyamatábrája](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. a beírt hello hello URL-cím tooaccess hello helyszíni application Proxy használatával.
2. Alkalmazásproxy átirányítja a felhasználókat hello kérelem tooAzure AD hitelesítési szolgáltatások toopreauthenticate. Ezen a ponton az Azure AD alkalmazza a megfelelő hitelesítési és engedélyezési házirendeket, mint a többtényezős hitelesítés. Hello felhasználó érvényesítését, ha az Azure AD létrehoz egy tokent, és elküldi toohello felhasználó.
3. hello felhasználói hello token tooApplication Proxy továbbítja.
4. Alkalmazásproxy érvényesíti hello jogkivonatot lekéri hello egyszerű felhasználónév (UPN), és majd küld kérelmet hello hello UPN és hello egyszerű szolgáltatásnév (SPN) toohello összekötő dually hitelesített biztonságos csatornán keresztül.
5. hello összekötő helyszíni hello Kerberos által korlátozott delegálás (KCD) egyeztetést végez AD hello felhasználói tooget egy Kerberos-token toohello alkalmazás megszemélyesít.
6. Az Active Directory küldi hello Kerberos-jogkivonata hello alkalmazás toohello összekötő.
7. hello összekötő küldi hello eredeti kérelem toohello, használó alkalmazáskiszolgálón hello Kerberos-jogkivonata kapta meg az Active Directoryból.
8. hello alkalmazás hello válasz toohello összekötőt, amely ezután visszaérkezik toohello alkalmazásproxy-szolgáltatás, és végül toohello felhasználói küld.

## <a name="prerequisites"></a>Előfeltételek
Mielőtt hozzáfogna, az egyszeri bejelentkezés alkalmazásokhoz integrált Windows-Hitelesítést, győződjön meg arról, hogy a környezet a hello készen áll a következő beállítások és konfigurációk:

* Az alkalmazások, például a SharePoint Web apps vannak beállítva toouse integrált Windows-hitelesítést. További információkért lásd: [Kerberos-hitelesítés támogatásának engedélyezése](https://technet.microsoft.com/library/dd759186.aspx), vagy a SharePoint lásd [a SharePoint 2013 Kerberos-hitelesítés tervezése](https://technet.microsoft.com/library/ee806870.aspx).
* Minden alkalmazás rendelkezik [egyszerű szolgáltatásnevek](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).
* hello kiszolgálón futó hello összekötő és hello alkalmazást futtató hello kiszolgáló tartományhoz csatlakoztatott és hello részét ugyanabban a tartományban vagy egymásban megbízó tartományokban. A tartományhoz való csatlakozást további információkért lásd: [egy számítógép tooa tartományhoz csatlakoztatás](https://technet.microsoft.com/library/dd807102.aspx).
* hello futtató hello összekötő attribútuma hozzáférés tooread hello értékűnek a felhasználók számára. Ez az alapértelmezett beállítás lehet, hogy rendelkezik lett által érintett biztonsági hello környezet korlátozására.

### <a name="configure-active-directory"></a>Az Active Directory konfigurálása
Active Directory-konfiguráció hello változik, attól függően, hogy az alkalmazásproxy-összekötő és hello alkalmazáskiszolgáló-e a hello ugyanabban a tartományban, vagy nem.

#### <a name="connector-and-application-server-in-hello-same-domain"></a>Összekötő és a hello alkalmazáskiszolgáló ugyanabban a tartományban
1. Az Active Directoryban, nyissa meg túl**eszközök** > **felhasználók és számítógépek**.
2. Válassza ki a hello-összekötőt futtató hello kiszolgálót.
3. Kattintson a jobb gombbal, és válassza ki **tulajdonságok** > **delegálás**.
4. Válassza ki **a számítógépen csak a delegálás toospecified szolgáltatások**. 
5. A **szolgáltatások toowhich ezt a fiókot használhat delegált hitelesítő adatokat** az SPN-identitásnak hello alkalmazáskiszolgáló hello hello értéket adjon meg. Ez lehetővé teszi a hello alkalmazásproxy-összekötő tooimpersonate felhasználók Active Directory hello listában definiált hello alkalmazások ellen.

   ![Összekötő-SVR tulajdonságai ablakban képernyőképe](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Összekötő és eltérő tartományokban alkalmazáskiszolgáló
1. A tartományok közötti Kerberos által korlátozott Delegálás végzett munka Előfeltételek listájáért lásd: [tartományok közötti Kerberos általi korlátozott delegálás](https://technet.microsoft.com/library/hh831477.aspx).
2. Használjon hello `principalsallowedtodelegateto` hello összekötő kiszolgáló tooenable hello alkalmazásproxy toodelegate hello összekötő kiszolgáló tulajdonságát. hello alkalmazáskiszolgáló `sharepointserviceaccount` és kiszolgáló delegálása hello `connectormachineaccount`. Windows 2012 R2-ben például ezt a kódot használja:

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount

Sharepointserviceaccount hello szervizcsomag-verzió számítógépfiók vagy a szolgáltatás mely hello szervizcsomag-verzió alkalmazáskészlet futtató fiók is lehet.

## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása 
1. Az alkalmazás toohello utasításokat leírtak szerint közzétételére [alkalmazások közzétételére az alkalmazásproxy](application-proxy-publish-azure-portal.md). Győződjön meg arról, hogy tooselect **Azure Active Directory** , hello **előhitelesítési módszer**.
2. Miután az alkalmazás a vállalati alkalmazások hello listájában jelenik meg, válassza ki azt, majd kattintson a **egyszeri bejelentkezés**.
3. Hello egyszeri bejelentkezés mód beállítása túl**integrált Windows-hitelesítés**.  
4. Adja meg a hello **belső alkalmazás SPN** hello alkalmazáskiszolgáló. Ebben a példában a közzétett alkalmazáshoz SPN hello http/www.contoso.com. Ez egyszerű Szolgáltatásnevet kell toobe szolgáltatások toowhich hello összekötő hello közül használhat delegált hitelesítő adatokat. 
5. Válassza ki a hello **meghatalmazott bejelentkezési identitás** a hello összekötő toouse a felhasználók nevében. További információkért lásd: [különböző helyszíni és a felhőbeli identitások használata](#Working-with-different-on-premises-and-cloud-identities)

   ![Speciális alkalmazás konfigurációja](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>Egyszeri bejelentkezés a Windows alkalmazások
Kerberos-delegálás folyamata a Azure AD alkalmazásproxy hello akkor kezdődik, amikor az Azure AD akkor hitelesíti a hello felhasználói hello felhőben. Hello kérelem érkezik a helyszínen, ha az Azure AD-alkalmazásproxy connector kiállítja a Kerberos jegy hello felhasználó nevében rajzolt hello hello helyi Active Directoryban. A folyamat be nem hivatkozott tooas Kerberos által korlátozott delegálás (KCD). Hello a következő fázis kérelmet küld a Kerberos jegyet a toohello a háttéralkalmazás. Nincsenek több protokollok, amelyek meghatározzák, hogyan toosend ilyen kérelmeket. A legtöbb-Windows kiszolgálók várt Negotiate/SPNego mostantól támogatott az Azure AD-alkalmazásproxyval oldható meg.

Kerberos kapcsolatos további információkért lásd: [minden kívánt tooknow kapcsolatos Kerberos által korlátozott delegálás (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

Nem Windows-alkalmazások általában felhasználói felhasználónevek vagy SAM fióknevek tartomány helyett e-mail címet. Ilyen esetben tooyour alkalmazások vonatkozik, ha szüksége tooconfigure meghatalmazott hello bejelentkezési identitás mező tooconnect a felhőbeli identitások tooyour alkalmazás identitások. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>A különböző helyszíni és felhőbeli identitások
Alkalmazásproxy feltételezi, hogy a felhasználók rendelkeznek pontosan hello hello felhőben és helyszíni ugyanazzal az identitással. Ha ez nem hello eset, is továbbra is használhatja a Kerberos által korlátozott az egyszeri bejelentkezést. Konfigurálja a **bejelentkezési identitás meghatalmazott** minden alkalmazás toospecify, mely identitás kell használni, amikor az egyszeri bejelentkezést hajt végre a.  

Ez a funkció lehetővé teszi, hogy a számos szervezet különböző helyszíni és felhőbeli identitások toohave SSO hello felhő tooon helyszíni alkalmazásokból anélkül, hogy a hello felhasználók tooenter különböző felhasználóneveket és jelszavakat. Ez magában foglalja a szervezetek, amelyek:

* Belső több tartományban vannak (joe@us.contoso.com, joe@eu.contoso.com) és a tartományra hello felhőben (joe@contoso.com).
* Belső nem átirányítható tartomány neve lehet (joe@contoso.usa) és egy jogi hello felhőben.
* Nem használható tartománynevek belső (joe)
* Használjon különböző aliasnevet a helyszínen és hello felhőben található. Például joe-johns@contoso.com vs.joej@contoso.com  

A Proxy kiválaszthatja, mely identitás toouse tooobtain hello Kerberos-jegy. Ez a beállítás akkor alkalmazásonként. A beállítások egy része, amely nem fogadja el az e-mail cím formátumú rendszerek alkalmas, mások számára készültek, alternatív bejelentkezési.

![Delegált bejelentkezési identitás paraméter képernyőképe](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Delegált bejelentkezési identitás használata esetén hello érték nem feltétlenül egyedi az összes hello tartományok és erdők a szervezet közötti. Ezt a problémát elkerülheti, ha a ezek kétszer csoportokkal két különböző összekötő alkalmazás-közzététel. Minden alkalmazás rendelkezik egy másik felhasználó célközönség, mivel csatlakozhat az összekötők tooa másik tartományt.

### <a name="configure-sso-for-different-identities"></a>Egyszeri bejelentkezés konfigurálása különböző identitások
1. Konfigurálja az Azure AD Connect beállításait, így hello fő identitás hello e-mail címet (mail). Ebben az esetben, hello részét folyamat testreszabása hello módosításával **egyszerű felhasználónév** hello szinkronizálási beállítások mezőbe. Ezek a beállítások azt is meghatározza, hogy a felhasználók bejelentkezését tooOffice365 Windows10 eszközöket és más Azure AD az identitás tárolóként használó alkalmazások.  
   ![Felhasználók képernyőfelvétel - egyszerű felhasználónév legördülő azonosítása](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Hello alkalmazás konfigurációs beállításaiban hello alkalmazás milyen toomodify, jelölje be hello **meghatalmazott bejelentkezési identitás** használt toobe:

   * Egyszerű felhasználónév (például joe@contoso.com)
   * Alternatív egyszerű felhasználónév (például joed@contoso.local)
   * Egyszerű felhasználónév (például joe) felhasználónév részét
   * Felhasználónév részét alternatív egyszerű felhasználónév (például joed)
   * A helyi SAM-neve (a tartományvezérlő-konfiguráció hello függ)

### <a name="troubleshooting-sso-for-different-identities"></a>Hibaelhárítási SSO különböző identitások
Ha nem sikerül a hello Egyszeri folyamat, azt hello összekötő gép eseménynaplóban megjelenik a [hibaelhárítás](application-proxy-back-end-kerberos-constrained-delegation-how-to.md).
Azonban néhány esetben hello kérelem sikeresen zajlik toohello a háttéralkalmazás közben a különböző HTTP-válaszok válaszol az alkalmazás. Ezekben az esetekben hibaelhárítási eseményszám 24029 hello alkalmazásproxy munkamenet eseménynaplóban hello összekötő gépen megvizsgálásával kell kezdődnie. hello felhasználói identitás delegálásához használt hello "user" mező hello esemény részleteit belül jelenik meg. a munkamenetnapló, tooturn válassza **megjelenítése elemzési és hibakeresési naplókat** hello esemény megjelenítő Nézet menü.

## <a name="next-steps"></a>Következő lépések

* [Hogyan tooconfigure az alkalmazásproxy alkalmazás toouse Kerberos által korlátozott delegálás](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Az alkalmazásproxy problémák elhárítása](active-directory-application-proxy-troubleshoot.md)


Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)

