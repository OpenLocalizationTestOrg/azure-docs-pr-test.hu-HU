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
# <a name="how-tooget-appsource-certified-for-azure-active-directory"></a><span data-ttu-id="a2c96-103">Az Azure Active Directory hitelesített AppSource tooget hogyan</span><span class="sxs-lookup"><span data-stu-id="a2c96-103">How tooget AppSource Certified for Azure Active Directory</span></span>
<span data-ttu-id="a2c96-104">[Microsoft AppSource](https://appsource.microsoft.com/) egy cél az üzleti felhasználók toodiscover, próbálja meg, és üzleti SaaS-alkalmazásokhoz (önálló SaaS és bővítmény tooexisting Microsoft SaaS-termékekben) kezelése.</span><span class="sxs-lookup"><span data-stu-id="a2c96-104">[Microsoft AppSource](https://appsource.microsoft.com/) is a destination for business users toodiscover, try, and manage line-of-business SaaS applications (standalone SaaS and add-on tooexisting Microsoft SaaS products).</span></span>

<span data-ttu-id="a2c96-105">toolist önálló AppSource a SaaS-alkalmazáshoz, az alkalmazás el kell fogadnia az egyszeri bejelentkezés a munkahelyi fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="a2c96-105">toolist a standalone SaaS application on AppSource, your application must accept single sign-on from work accounts from any company or organization that has Azure Active Directory.</span></span> <span data-ttu-id="a2c96-106">hello bejelentkezési folyamat kell használnia a hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) vagy [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokollokat.</span><span class="sxs-lookup"><span data-stu-id="a2c96-106">hello sign-in process must use hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) or [OAuth 2.0](./active-directory-protocols-oauth-code.md) protocols.</span></span> <span data-ttu-id="a2c96-107">SAML-integráció AppSource hitelesítő el nem fogadott.</span><span class="sxs-lookup"><span data-stu-id="a2c96-107">SAML integration is not accepted for AppSource certification.</span></span>

## <a name="guides-and-code-samples"></a><span data-ttu-id="a2c96-108">Útmutatók és mintakódok</span><span class="sxs-lookup"><span data-stu-id="a2c96-108">Guides and code samples</span></span>
<span data-ttu-id="a2c96-109">Ha azt szeretné, hogy hogyan toointegrate az Azure Active Directoryval azonosítójával nyissa meg az alkalmazás csatlakozzon kapcsolatos toolearn, útmutatókat követve, és a hello Kódminták [Azure Active Directory fejlesztői útmutatója](./active-directory-developers-guide.md#get-started "az első lépései Az Azure AD-fejlesztőknek").</span><span class="sxs-lookup"><span data-stu-id="a2c96-109">If you want toolearn about how toointegrate your application with Azure Active Directory using Open ID connect, follow our guides and code samples in hello [Azure Active Directory developer's guide](./active-directory-developers-guide.md#get-started "Get Started with Azure AD for developers").</span></span>

## <a name="multi-tenant-applications"></a><span data-ttu-id="a2c96-110">Több-bérlős alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="a2c96-110">Multi-tenant applications</span></span>

<span data-ttu-id="a2c96-111">A vállalat vagy szervezet rendelkező felhasználók Azure Active Directory anélkül, hogy egy külön példányát, a konfiguráció vagy a központi telepítési bejelentkezések elfogadó alkalmazás is ismert, egy *több-bérlős alkalmazás*.</span><span class="sxs-lookup"><span data-stu-id="a2c96-111">An application that accepts sign-ins from users from any company or organization that have Azure Active Directory without requiring a separate instance, configuration, or deployment is known as a *multi-tenant application*.</span></span> <span data-ttu-id="a2c96-112">AppSource javasolja, hogy az alkalmazások végrehajtása több-bérlős tooenable hello *kattintással* ingyenes próbaverziója.</span><span class="sxs-lookup"><span data-stu-id="a2c96-112">AppSource recommends that applications implement multi-tenancy tooenable hello *single-click* free trial experience.</span></span>

<span data-ttu-id="a2c96-113">A sorrend tooenable több-bérlős az alkalmazásra:</span><span class="sxs-lookup"><span data-stu-id="a2c96-113">In order tooenable multi-tenancy on your application:</span></span>
- <span data-ttu-id="a2c96-114">Állítsa be `Multi-Tenanted` tulajdonság túl`Yes` hello az alkalmazás regisztrációs információit a [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (alapértelmezés szerint a hello Azure-portálon létrehozott alkalmazások beállításuk *single-bérlő*)</span><span class="sxs-lookup"><span data-stu-id="a2c96-114">Set `Multi-Tenanted` property too`Yes` on your application registration's information in hello [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (by default, applications created in hello Azure Portal are configured as *single-tenant*)</span></span>
- <span data-ttu-id="a2c96-115">Frissítse a kódot toosend kérelmek toohello "`common`" végpont (a hello végpontjának frissítése *https://login.microsoftonline.com/ {yourtenant}* túl*https://login.microsoftonline.com/common*)</span><span class="sxs-lookup"><span data-stu-id="a2c96-115">Update your code toosend requests toohello '`common`' endpoint (update hello endpoint from *https://login.microsoftonline.com/{yourtenant}* too*https://login.microsoftonline.com/common*)</span></span>
- <span data-ttu-id="a2c96-116">Egyes platformokon, például az ASP.NET kell is tooupdate a kód tooaccept több kibocsátók</span><span class="sxs-lookup"><span data-stu-id="a2c96-116">For some platforms, like ASP.NET, you need also tooupdate your code tooaccept multiple issuers</span></span>

<span data-ttu-id="a2c96-117">Több vállalat kiszolgálása kapcsolatos további információkért lásd: [hogyan bármely Azure Active Directory (AD) felhasználó használatával toosign hello több-bérlős alkalmazásminta](./active-directory-devhowto-multi-tenant-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2c96-117">For more information about multi-tenancy, see: [How toosign in any Azure Active Directory (AD) user using hello multi-tenant application pattern](./active-directory-devhowto-multi-tenant-overview.md).</span></span>

### <a name="single-tenant-applications"></a><span data-ttu-id="a2c96-118">Single-bérlői alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a2c96-118">Single-tenant applications</span></span>
<span data-ttu-id="a2c96-119">Alkalmazások bejelentkezéseket a felhasználóktól egy meghatározott Azure Active Directory-példány csak elfogadó nevezik *single-bérlő alkalmazás*.</span><span class="sxs-lookup"><span data-stu-id="a2c96-119">Applications that only accept sign-ins from users of a defined Azure Active Directory instance are known as *single-tenant application*.</span></span> <span data-ttu-id="a2c96-120">(Beleértve a munkahelyi vagy iskolai fiókok el más szervezetek vagy személyes fiók) külső felhasználók bejelentkezhetnek tooa single-bérlő alkalmazás minden felhasználó hozzáadása után *vendégfiókba* toohello Azure Active Directory-példány hello alkalmazás van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="a2c96-120">External users (including Work or School accounts from other organizations, or personal account) can sign in tooa single-tenant application after adding each user as *guest account* toohello Azure Active Directory instance that hello application is registered.</span></span> <span data-ttu-id="a2c96-121">Mint Vendég fiókok tooan Azure Active Directory hello keresztül is hozzáadhat felhasználókat [ *Azure AD B2B együttműködés* ](../active-directory-b2b-what-is-azure-ad-b2b.md) -is elvégezhető, és [programozottan](../active-directory-b2b-code-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a2c96-121">You can add users as guest accounts tooan Azure Active Directory via hello [*Azure AD B2B collaboration*](../active-directory-b2b-what-is-azure-ad-b2b.md) - and it can be done [programatically](../active-directory-b2b-code-samples.md).</span></span> <span data-ttu-id="a2c96-122">Ha egy felhasználó Vendég fiók tooan Azure Active Directory, az meghívó e-mailt küld toohello, rendelkező felhasználó tooaccept hello meghívó hello meghívó e-mailben hello hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="a2c96-122">When you add a user as guest account tooan Azure Active Directory, an invitation email is sent toohello user, who has tooaccept hello invitation by clicking on hello link in hello invitation email.</span></span> <span data-ttu-id="a2c96-123">Meghívót küldött tooan további felhasználói hívja fel a szervezeten, amely tagja a fiókpartner-szervezet hello nem szükséges tooaccept egy meghívó toosign a.</span><span class="sxs-lookup"><span data-stu-id="a2c96-123">Invitations that are sent tooan additional user in an inviting organization that is also a member of hello partner organization are not required tooaccept an invitation toosign in.</span></span>

<span data-ttu-id="a2c96-124">Single-bérlői alkalmazások engedélyezheti hello *forduljon Me* tapasztalhat, de ha tooenable hello kattintással / ingyenes próbaverziója AppSource képesek készletbeállítást, engedélyezze az alkalmazásban több-bérlős helyette.</span><span class="sxs-lookup"><span data-stu-id="a2c96-124">Single-tenant applications can enable hello *Contact Me* experience, but if you want tooenable hello single-click/ free trial experience that AppSource recommends, enable multi-tenancy on your application instead.</span></span>


## <a name="appsource-trial-experiences"></a><span data-ttu-id="a2c96-125">Próba AppSource során lép fel.</span><span class="sxs-lookup"><span data-stu-id="a2c96-125">AppSource trial experiences</span></span>

### <a name="free-trial-customer-led-trial-experience"></a><span data-ttu-id="a2c96-126">Ingyenes próbaverzió (ügyfél-vezetett próbaverziója)</span><span class="sxs-lookup"><span data-stu-id="a2c96-126">Free Trial (Customer-led trial experience)</span></span> 
<span data-ttu-id="a2c96-127">Hello *ügyfél által vezetett próbaverzió* hello folyamat, amely AppSource javasolja, egy kattintással indítható access tooyour alkalmazás kínál.</span><span class="sxs-lookup"><span data-stu-id="a2c96-127">hello *customer-led trial* is hello experience that AppSource recommends as it offers a single-click access tooyour application.</span></span> <span data-ttu-id="a2c96-128">Az ábra bemutatja, hogyan alatt ez a felület néz ki:</span><span class="sxs-lookup"><span data-stu-id="a2c96-128">Below an illustration of how this experience looks like:</span></span><br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="a2c96-129">Felhasználói megkeresi az alkalmazást az AppSource webhelyen</span><span class="sxs-lookup"><span data-stu-id="a2c96-129">User finds your application in AppSource Web Site</span></span></li><li><span data-ttu-id="a2c96-130">"Ingyenes" beállítás kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a2c96-130">Selects ‘Free trial’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li><span data-ttu-id="a2c96-131">AppSource átirányítja a felhasználókat a webhely felhasználói tooa URL-címe</span><span class="sxs-lookup"><span data-stu-id="a2c96-131">AppSource redirects user tooa URL in your web site</span></span></li><li><span data-ttu-id="a2c96-132">A webhely elindul hello <i>single-sign-on</i> folyamat automatikusan (betöltési)</span><span class="sxs-lookup"><span data-stu-id="a2c96-132">Your web site starts hello <i>single-sign-on</i> process automatically (on page load)</span></span></li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="a2c96-133">Felhasználói az átirányított tooMicrosoft bejelentkezési oldal</span><span class="sxs-lookup"><span data-stu-id="a2c96-133">User is redirected tooMicrosoft Sign-in page</span></span></li><li><span data-ttu-id="a2c96-134">A felhasználó megadja a hitelesítő adatok toosign</span><span class="sxs-lookup"><span data-stu-id="a2c96-134">User provides credentials toosign in</span></span></li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="a2c96-135">Felhasználó hozzájárulását adja az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a2c96-135">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="a2c96-136">Bejelentkezés befejezése és a felhasználó átirányított hátsó tooyour webhely</span><span class="sxs-lookup"><span data-stu-id="a2c96-136">Sign-in completes and user is redirected back tooyour web site</span></span></li><li><span data-ttu-id="a2c96-137">Felhasználói indítását hello ingyenes próbaverzió</span><span class="sxs-lookup"><span data-stu-id="a2c96-137">User starts hello free trial</span></span></li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a><span data-ttu-id="a2c96-138">Megkereshetnek (Partner által vezetett próbaverziója)</span><span class="sxs-lookup"><span data-stu-id="a2c96-138">Contact Me (Partner-led trial experience)</span></span>
<span data-ttu-id="a2c96-139">Hello *próbaverziója partneri* is használható, ha a kézi vagy hosszú távú művelet toohappen tooprovision hello felhasználói / vállalati: például az alkalmazás kell tooprovision virtuális gépek, adatbázis-példány, vagy mennyi időt toocomplete tévő műveletek.</span><span class="sxs-lookup"><span data-stu-id="a2c96-139">hello *partner trial experience* can be used when a manual or a long-term operation needs toohappen tooprovision hello user/ company: for example, your application needs tooprovision virtual machines, database instances, or operations that take much time toocomplete.</span></span> <span data-ttu-id="a2c96-140">Ebben az esetben a felhasználó kiválaszt hello után *kérelem próbaverzió* gombra, és Ön hello felhasználó kapcsolattartási adatokat küld egy űrlap AppSource kitölti.</span><span class="sxs-lookup"><span data-stu-id="a2c96-140">In this case, after user selects hello *'Request Trial'* button and fills out a form, AppSource sends you hello user's contact information.</span></span> <span data-ttu-id="a2c96-141">Ezek az információk fogadását követően újból hello környezet kiépítése és hello utasításokat toohello felhasználó hogyan tooaccess hello próbaverziója küldeni:</span><span class="sxs-lookup"><span data-stu-id="a2c96-141">Upon receiving this information, you then provision hello environment and send hello instructions toohello user on how tooaccess hello trial experience:</span></span><br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="a2c96-142">Felhasználó az alkalmazás AppSource webhelyen talál.</span><span class="sxs-lookup"><span data-stu-id="a2c96-142">User finds your application in AppSource web site</span></span></li><li><span data-ttu-id="a2c96-143">"Forduljon Me" beállítás kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a2c96-143">Selects ‘Contact Me’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li><span data-ttu-id="a2c96-144">Kitölti a kapcsolattartási adatokat az űrlap</span><span class="sxs-lookup"><span data-stu-id="a2c96-144">Fills out a form with contact information</span></span></li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td><span data-ttu-id="a2c96-145">Megjelenik a felhasználói adatok</span><span class="sxs-lookup"><span data-stu-id="a2c96-145">You receive user information</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td><span data-ttu-id="a2c96-146">Környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="a2c96-146">Setup environment</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td><span data-ttu-id="a2c96-147">Próba információval kapcsolattartási felhasználó</span><span class="sxs-lookup"><span data-stu-id="a2c96-147">Contact user with trial info</span></span></td>
        </tr>
        </table><br/><br/>
        <ul><li><span data-ttu-id="a2c96-148">Felhasználói adatokat és telepítő próba példány</span><span class="sxs-lookup"><span data-stu-id="a2c96-148">You receive user's information and setup trial instance</span></span></li><li><span data-ttu-id="a2c96-149">Az alkalmazás toohello felhasználói érdeklődést hello hivatkozás tooaccess</span><span class="sxs-lookup"><span data-stu-id="a2c96-149">You send hello hyperlink tooaccess your application toohello user</span></span></li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="a2c96-150">Felhasználó hozzáfér az alkalmazás és a teljes hello single-sign-on folyamat</span><span class="sxs-lookup"><span data-stu-id="a2c96-150">User accesses your application and complete hello single-sign-on process</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="a2c96-151">Felhasználó hozzájárulását adja az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a2c96-151">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="a2c96-152">Bejelentkezés befejezése és a felhasználó átirányított hátsó tooyour webhely</span><span class="sxs-lookup"><span data-stu-id="a2c96-152">Sign-in completes and user is redirected back tooyour web site</span></span></li><li><span data-ttu-id="a2c96-153">Felhasználói indítását hello ingyenes próbaverzió</span><span class="sxs-lookup"><span data-stu-id="a2c96-153">User starts hello free trial</span></span></li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a><span data-ttu-id="a2c96-154">További információ</span><span class="sxs-lookup"><span data-stu-id="a2c96-154">More information</span></span>
<span data-ttu-id="a2c96-155">Hello AppSource próbaverziója kapcsolatos további információkért lásd: [Ez a videó](https://aka.ms/trialexperienceforwebapps).</span><span class="sxs-lookup"><span data-stu-id="a2c96-155">For more information about hello AppSource trial experience, see [this video](https://aka.ms/trialexperienceforwebapps).</span></span> 
 
## <a name="next-steps"></a><span data-ttu-id="a2c96-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2c96-156">Next Steps</span></span>

- <span data-ttu-id="a2c96-157">Az Azure Active Directory bejelentkezések támogató alkalmazások további információkért lásd: [az Azure Active Directory hitelesítési forgatókönyvei](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span><span class="sxs-lookup"><span data-stu-id="a2c96-157">For more information on building applications that support Azure Active Directory sign-ins, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span></span> 

- <span data-ttu-id="a2c96-158">Hogyan toolist a SaaS-alkalmazás a AppSource, nyissa meg talál tájékoztatást [AppSource partneradatok](https://appsource.microsoft.com/partners)</span><span class="sxs-lookup"><span data-stu-id="a2c96-158">For information on how toolist your SaaS application in AppSource, go see [AppSource Partner Information](https://appsource.microsoft.com/partners)</span></span>


## <a name="get-support"></a><span data-ttu-id="a2c96-159">Támogatás kérése</span><span class="sxs-lookup"><span data-stu-id="a2c96-159">Get Support</span></span>
<span data-ttu-id="a2c96-160">Az Azure Active Directory integráció használjuk [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) hello közösségi tooprovide támogatásával.</span><span class="sxs-lookup"><span data-stu-id="a2c96-160">For Azure Active Directory integration, we use [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) with hello community tooprovide support.</span></span> 

<span data-ttu-id="a2c96-161">Először a Stack Overflow kérje meg a kérdéseket, és keresse meg a már meglévő problémák toosee, ha valaki feltette-e már a kérdését előtt erősen ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a2c96-161">We highly recommend you ask your questions on Stack Overflow first and browse existing issues toosee if someone has asked your question before.</span></span> <span data-ttu-id="a2c96-162">Győződjön meg arról, hogy a kérdéseit vagy megjegyzéseit címkével rendelkező `[azure-active-directory]`.</span><span class="sxs-lookup"><span data-stu-id="a2c96-162">Make sure that your questions or comments are tagged with `[azure-active-directory]`.</span></span>

<span data-ttu-id="a2c96-163">A következő megjegyzések szakasz tooprovide visszajelzés hello használja, és segítenek pontosítsa, valamint a tartalom.</span><span class="sxs-lookup"><span data-stu-id="a2c96-163">Use hello following comments section tooprovide feedback and help us refine and shape our content.</span></span>

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->