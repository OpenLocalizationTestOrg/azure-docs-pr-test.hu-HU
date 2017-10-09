---
title: "aaaAzure Active Directory-alapú hitelesítés IOS |} Microsoft Docs"
description: "További tudnivalók a hello támogatott forgatókönyvek és megoldások a Tanúsítványalapú hitelesítés konfigurálása iOS-eszközök hello követelményei"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: 26a6fc54-0153-44fb-b970-9b432c99e9f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 4486ff5239c2897b3bc187053f31d74807430301
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a><span data-ttu-id="c1335-103">IOS rendszerű eszközökön az Azure Active Directory-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="c1335-103">Azure Active Directory certificate-based authentication on iOS</span></span>

<span data-ttu-id="c1335-104">Tanúsítvány alapú hitelesítést (CBA) lehetővé teszi az Exchange online-fiókját az kapcsolódáskor egy Windows-, Android vagy iOS eszközön ügyféltanúsítvánnyal az Azure Active Directory hitelesített toobe:</span><span class="sxs-lookup"><span data-stu-id="c1335-104">Certificate-based authentication (CBA) enables you toobe authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="c1335-105">Office-mobilalkalmazások például a Microsoft Outlook és a Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="c1335-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="c1335-106">Exchange ActiveSync (EAS) ügyfeleket</span><span class="sxs-lookup"><span data-stu-id="c1335-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="c1335-107">Ez a funkció beállítása nem hello kell tooenter egy felhasználónév és jelszó kombináció bizonyos mail és a Microsoft Office-alkalmazásokat a mobileszközön.</span><span class="sxs-lookup"><span data-stu-id="c1335-107">Configuring this feature eliminates hello need tooenter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="c1335-108">Ez a témakör hello követelmények és támogatott hello forgatókönyvek CBA konfigurálásához a felhasználók számára a bérlő az Office 365 Enterprise, vállalati, oktatási, USA szövetségi kormányzatának, Kína iOS(Android)-eszközön, és Németország tervez.</span><span class="sxs-lookup"><span data-stu-id="c1335-108">This topic provides you with hello requirements and hello supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>

<span data-ttu-id="c1335-109">Ez a funkció érhető el az előzetes verzió az Office 365 US Government védelmet és szövetségi csomagokban.</span><span class="sxs-lookup"><span data-stu-id="c1335-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>




## <a name="office-mobile-applications-support"></a><span data-ttu-id="c1335-110">Office-mobilalkalmazások támogatja</span><span class="sxs-lookup"><span data-stu-id="c1335-110">Office mobile applications support</span></span>

| <span data-ttu-id="c1335-111">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c1335-111">Apps</span></span> | <span data-ttu-id="c1335-112">Támogatás</span><span class="sxs-lookup"><span data-stu-id="c1335-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="c1335-113">Azure Information Protection alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c1335-113">Azure Information Protection app</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="c1335-115">Microsoft Teams</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="c1335-117">OneNote</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="c1335-119">OneDrive</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="c1335-121">Outlook</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-123">A Power BI mobilalkalmazásokkal</span><span class="sxs-lookup"><span data-stu-id="c1335-123">Power BI mobile apps</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-125">A Skype vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="c1335-125">Skype for Business</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-127">Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="c1335-127">Word / Excel / PowerPoint</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="c1335-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="c1335-129">Yammer</span></span> |![Jelölőnégyzet][1] |


## <a name="requirements"></a><span data-ttu-id="c1335-131">Követelmények</span><span class="sxs-lookup"><span data-stu-id="c1335-131">Requirements</span></span> 

<span data-ttu-id="c1335-132">hello az eszköz operációs rendszere verziójúnak kell lennie iOS 9-es vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="c1335-132">hello device OS version must be iOS 9 and above</span></span> 

<span data-ttu-id="c1335-133">Az összevonási kiszolgáló be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="c1335-133">A federation server must be configured.</span></span>  

<span data-ttu-id="c1335-134">A Microsoft Authenticator Office-alkalmazások IOS szükség.</span><span class="sxs-lookup"><span data-stu-id="c1335-134">Microsoft Authenticator is required for Office applications on iOS.</span></span>  

<span data-ttu-id="c1335-135">Az Azure Active Directory toorevoke ügyféltanúsítványt hello az AD FS jogkivonat a következő jogcímeket hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="c1335-135">For Azure Active Directory toorevoke a client certificate, hello ADFS token must have hello following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="c1335-136">(hello hello ügyfél tanúsítvány sorozatszáma)</span><span class="sxs-lookup"><span data-stu-id="c1335-136">(hello serial number of hello client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="c1335-137">(hello kibocsátó hello ügyféltanúsítvány hello karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="c1335-137">(hello string for hello issuer of hello client certificate)</span></span> 

<span data-ttu-id="c1335-138">Az Azure Active Directory a jogcímek toohello frissítési jogkivonat ad hozzá, ha ilyenek hello az AD FS jogkivonat (vagy bármely más SAML-jogkivonat).</span><span class="sxs-lookup"><span data-stu-id="c1335-138">Azure Active Directory adds these claims toohello refresh token if they are available in hello ADFS token (or any other SAML token).</span></span> <span data-ttu-id="c1335-139">Ha hello frissítési jogkivonat toobe ellenőrizni kell, ez az információ használt toocheck hello visszavont tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="c1335-139">When hello refresh token needs toobe validated, this information is used toocheck hello revocation.</span></span> 

<span data-ttu-id="c1335-140">Ajánlott eljárásként frissítenie kell az AD FS hibalapok hello hello következőre:</span><span class="sxs-lookup"><span data-stu-id="c1335-140">As a best practice, you should update hello ADFS error pages with hello following:</span></span>

* <span data-ttu-id="c1335-141">hello követelmény Microsoft Authenticator hello telepítéséhez IOS rendszerű eszközökön</span><span class="sxs-lookup"><span data-stu-id="c1335-141">hello requirement for installing hello Microsoft Authenticator on iOS</span></span>
* <span data-ttu-id="c1335-142">Útmutatás felhasználói tanúsítvány tooget.</span><span class="sxs-lookup"><span data-stu-id="c1335-142">Instructions on how tooget a user certificate.</span></span> 

<span data-ttu-id="c1335-143">További részletekért lásd: [AD FS-bejelentkezés hello oldalainak testreszabása](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1335-143">For more details, see [Customizing hello AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>

<span data-ttu-id="c1335-144">Néhány Office-alkalmazások (a modern hitelesítést) küldése "*kérdezzen rá a bejelentkezési =*" tooAzure AD a kérelemben.</span><span class="sxs-lookup"><span data-stu-id="c1335-144">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ tooAzure AD in their request.</span></span> <span data-ttu-id="c1335-145">Alapértelmezés szerint az Azure AD fordítja le ez a hello kérelem tooADFS túl "*wauth = usernamepassworduri*" (az AD FS toodo U/P auth kéri) és "*wfresh = 0*" (az AD FS tooignore SSO állapot kéri és egy friss hitelesítést) .</span><span class="sxs-lookup"><span data-stu-id="c1335-145">By default, Azure AD translates this in hello request tooADFS too‘*wauth=usernamepassworduri*’ (asks ADFS toodo U/P auth) and ‘*wfresh=0*’ (asks ADFS tooignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="c1335-146">Ha azt szeretné, hogy az alkalmazások tooenable tanúsítvány alapú hitelesítést, meg kell toomodify hello alapértelmezés az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1335-146">If you want tooenable certificate-based authentication for these apps, you need toomodify hello default Azure AD behavior.</span></span> <span data-ttu-id="c1335-147">Csak a készlet hello "*PromptLoginBehavior*" az összevont tartományt beállításaiban túl "*letiltott*".</span><span class="sxs-lookup"><span data-stu-id="c1335-147">Just set hello ‘*PromptLoginBehavior*’ in your federated domain settings too‘*Disabled*‘.</span></span> <span data-ttu-id="c1335-148">Használhatja a hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) parancsmag tooperform ezt a feladatot:</span><span class="sxs-lookup"><span data-stu-id="c1335-148">You can use hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet tooperform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="c1335-149">Exchange ActiveSync-ügyfelek támogatását.</span><span class="sxs-lookup"><span data-stu-id="c1335-149">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="c1335-150">Hello natív iOS e-mail ügyfélprogramokba IOS 9-es vagy újabb rendszeren támogatott.</span><span class="sxs-lookup"><span data-stu-id="c1335-150">On iOS 9 or later, hello native iOS mail client is supported.</span></span> <span data-ttu-id="c1335-151">Minden Exchange ActiveSync alkalmazások toodetermine Ha ezt támogatja, lépjen kapcsolatba az alkalmazás fejlesztőjének.</span><span class="sxs-lookup"><span data-stu-id="c1335-151">For all other Exchange ActiveSync applications, toodetermine if this feature is supported, contact your application developer.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="c1335-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1335-152">Next steps</span></span>

<span data-ttu-id="c1335-153">Ha tooconfigure tanúsítvány alapú hitelesítést a környezetben, tekintse meg a [első lépések az Android-alapú hitelesítés](active-directory-certificate-based-authentication-get-started.md) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="c1335-153">If you want tooconfigure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
