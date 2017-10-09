---
title: "az Azure Active Directory hitelesített AppSource aaaHow tooget |} Microsoft Docs"
description: "Hogyan tooget az alkalmazás AppSource hitelesített az Azure Active Directory az adatokat."
services: active-directory
documentationcenter: 
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: e9f07e9220afcba1120b987122fe770fe5225eed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-appsource-certified-for-azure-active-directory"></a>Az Azure Active Directory hitelesített AppSource tooget hogyan
[Microsoft AppSource](https://appsource.microsoft.com/) egy cél az üzleti felhasználók toodiscover, próbálja meg, és üzleti SaaS-alkalmazásokhoz (önálló SaaS és bővítmény tooexisting Microsoft SaaS-termékekben) kezelése.

toolist önálló AppSource a SaaS-alkalmazáshoz, az alkalmazás el kell fogadnia az egyszeri bejelentkezés a munkahelyi fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban. hello bejelentkezési folyamat kell használnia a hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) vagy [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokollokat. SAML-integráció AppSource hitelesítő el nem fogadott.

## <a name="guides-and-code-samples"></a>Útmutatók és mintakódok
Ha azt szeretné, hogy hogyan toointegrate az Azure Active Directoryval azonosítójával nyissa meg az alkalmazás csatlakozzon kapcsolatos toolearn, útmutatókat követve, és a hello Kódminták [Azure Active Directory fejlesztői útmutatója](./active-directory-developers-guide.md#get-started "az első lépései Az Azure AD-fejlesztőknek").

## <a name="multi-tenant-applications"></a>Több-bérlős alkalmazásokhoz

A vállalat vagy szervezet rendelkező felhasználók Azure Active Directory anélkül, hogy egy külön példányát, a konfiguráció vagy a központi telepítési bejelentkezések elfogadó alkalmazás is ismert, egy *több-bérlős alkalmazás*. AppSource javasolja, hogy az alkalmazások végrehajtása több-bérlős tooenable hello *kattintással* ingyenes próbaverziója.

A sorrend tooenable több-bérlős az alkalmazásra:
- Állítsa be `Multi-Tenanted` tulajdonság túl`Yes` hello az alkalmazás regisztrációs információit a [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (alapértelmezés szerint a hello Azure-portálon létrehozott alkalmazások beállításuk *single-bérlő*)
- Frissítse a kódot toosend kérelmek toohello "`common`" végpont (a hello végpontjának frissítése *https://login.microsoftonline.com/ {yourtenant}* túl*https://login.microsoftonline.com/common*)
- Egyes platformokon, például az ASP.NET kell is tooupdate a kód tooaccept több kibocsátók

Több vállalat kiszolgálása kapcsolatos további információkért lásd: [hogyan bármely Azure Active Directory (AD) felhasználó használatával toosign hello több-bérlős alkalmazásminta](./active-directory-devhowto-multi-tenant-overview.md).

### <a name="single-tenant-applications"></a>Single-bérlői alkalmazások
Alkalmazások bejelentkezéseket a felhasználóktól egy meghatározott Azure Active Directory-példány csak elfogadó nevezik *single-bérlő alkalmazás*. (Beleértve a munkahelyi vagy iskolai fiókok el más szervezetek vagy személyes fiók) külső felhasználók bejelentkezhetnek tooa single-bérlő alkalmazás minden felhasználó hozzáadása után *vendégfiókba* toohello Azure Active Directory-példány hello alkalmazás van regisztrálva. Mint Vendég fiókok tooan Azure Active Directory hello keresztül is hozzáadhat felhasználókat [ *Azure AD B2B együttműködés* ](../active-directory-b2b-what-is-azure-ad-b2b.md) -is elvégezhető, és [programozottan](../active-directory-b2b-code-samples.md). Ha egy felhasználó Vendég fiók tooan Azure Active Directory, az meghívó e-mailt küld toohello, rendelkező felhasználó tooaccept hello meghívó hello meghívó e-mailben hello hivatkozásra kattintva. Meghívót küldött tooan további felhasználói hívja fel a szervezeten, amely tagja a fiókpartner-szervezet hello nem szükséges tooaccept egy meghívó toosign a.

Single-bérlői alkalmazások engedélyezheti hello *forduljon Me* tapasztalhat, de ha tooenable hello kattintással / ingyenes próbaverziója AppSource képesek készletbeállítást, engedélyezze az alkalmazásban több-bérlős helyette.


## <a name="appsource-trial-experiences"></a>Próba AppSource során lép fel.

### <a name="free-trial-customer-led-trial-experience"></a>Ingyenes próbaverzió (ügyfél-vezetett próbaverziója) 
Hello *ügyfél által vezetett próbaverzió* hello folyamat, amely AppSource javasolja, egy kattintással indítható access tooyour alkalmazás kínál. Az ábra bemutatja, hogyan alatt ez a felület néz ki:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Felhasználói megkeresi az alkalmazást az AppSource webhelyen</li><li>"Ingyenes" beállítás kiválasztása</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>AppSource átirányítja a felhasználókat a webhely felhasználói tooa URL-címe</li><li>A webhely elindul hello <i>single-sign-on</i> folyamat automatikusan (betöltési)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Felhasználói az átirányított tooMicrosoft bejelentkezési oldal</li><li>A felhasználó megadja a hitelesítő adatok toosign</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Felhasználó hozzájárulását adja az alkalmazáshoz</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Bejelentkezés befejezése és a felhasználó átirányított hátsó tooyour webhely</li><li>Felhasználói indítását hello ingyenes próbaverzió</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Megkereshetnek (Partner által vezetett próbaverziója)
Hello *próbaverziója partneri* is használható, ha a kézi vagy hosszú távú művelet toohappen tooprovision hello felhasználói / vállalati: például az alkalmazás kell tooprovision virtuális gépek, adatbázis-példány, vagy mennyi időt toocomplete tévő műveletek. Ebben az esetben a felhasználó kiválaszt hello után *kérelem próbaverzió* gombra, és Ön hello felhasználó kapcsolattartási adatokat küld egy űrlap AppSource kitölti. Ezek az információk fogadását követően újból hello környezet kiépítése és hello utasításokat toohello felhasználó hogyan tooaccess hello próbaverziója küldeni:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Felhasználó az alkalmazás AppSource webhelyen talál.</li><li>"Forduljon Me" beállítás kiválasztása</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>Kitölti a kapcsolattartási adatokat az űrlap</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>Megjelenik a felhasználói adatok</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Környezet beállítása</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Próba információval kapcsolattartási felhasználó</td>
        </tr>
        </table><br/><br/>
        <ul><li>Felhasználói adatokat és telepítő próba példány</li><li>Az alkalmazás toohello felhasználói érdeklődést hello hivatkozás tooaccess</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Felhasználó hozzáfér az alkalmazás és a teljes hello single-sign-on folyamat</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Felhasználó hozzájárulását adja az alkalmazáshoz</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Bejelentkezés befejezése és a felhasználó átirányított hátsó tooyour webhely</li><li>Felhasználói indítását hello ingyenes próbaverzió</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>További információ
Hello AppSource próbaverziója kapcsolatos további információkért lásd: [Ez a videó](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Következő lépések

- Az Azure Active Directory bejelentkezések támogató alkalmazások további információkért lásd: [az Azure Active Directory hitelesítési forgatókönyvei](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios) 

- Hogyan toolist a SaaS-alkalmazás a AppSource, nyissa meg talál tájékoztatást [AppSource partneradatok](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Támogatás kérése
Az Azure Active Directory integráció használjuk [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) hello közösségi tooprovide támogatásával. 

Először a Stack Overflow kérje meg a kérdéseket, és keresse meg a már meglévő problémák toosee, ha valaki feltette-e már a kérdését előtt erősen ajánlott. Győződjön meg arról, hogy a kérdéseit vagy megjegyzéseit címkével rendelkező `[azure-active-directory]`.

A következő megjegyzések szakasz tooprovide visszajelzés hello használja, és segítenek pontosítsa, valamint a tartalom.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->