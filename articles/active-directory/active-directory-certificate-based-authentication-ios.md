---
title: "IOS rendszerű eszközökön az Azure Active Directory-alapú hitelesítés |} Microsoft Docs"
description: "További tudnivalók a támogatott forgatókönyveket és a megoldások konfigurálását tanúsítvány alapú hitelesítést követelményei az iOS-eszközök"
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
ms.openlocfilehash: c781f3f054fad5c5092fed5058c932fd4e97cf35
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a><span data-ttu-id="0aedb-103">IOS rendszerű eszközökön az Azure Active Directory-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0aedb-103">Azure Active Directory certificate-based authentication on iOS</span></span>

<span data-ttu-id="0aedb-104">Tanúsítvány alapú hitelesítést (CBA) lehetővé teszi, hogy hitelesítse Azure Active Directory ügyféltanúsítvánnyal egy Windows-, Android vagy iOS eszközön az Exchange online fiókot való csatlakozáskor:</span><span class="sxs-lookup"><span data-stu-id="0aedb-104">Certificate-based authentication (CBA) enables you to be authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="0aedb-105">Office-mobilalkalmazások például a Microsoft Outlook és a Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="0aedb-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="0aedb-106">Exchange ActiveSync (EAS) ügyfeleket</span><span class="sxs-lookup"><span data-stu-id="0aedb-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="0aedb-107">Ez a funkció beállítása szükségtelenné teszi adjon meg egy felhasználónév és jelszó kombinációjával egyes mail és a Microsoft Office-alkalmazásokat a mobileszközön.</span><span class="sxs-lookup"><span data-stu-id="0aedb-107">Configuring this feature eliminates the need to enter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="0aedb-108">Ez a témakör a követelményeket és a támogatott forgatókönyveket CBA konfigurálásához a felhasználók számára a bérlő az Office 365 Enterprise, vállalati, oktatási, USA szövetségi kormányzatának, Kína iOS(Android)-eszközön, és Németország tervez.</span><span class="sxs-lookup"><span data-stu-id="0aedb-108">This topic provides you with the requirements and the supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>

<span data-ttu-id="0aedb-109">Ez a funkció érhető el az előzetes verzió az Office 365 US Government védelmet és szövetségi csomagokban.</span><span class="sxs-lookup"><span data-stu-id="0aedb-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>




## <a name="office-mobile-applications-support"></a><span data-ttu-id="0aedb-110">Office-mobilalkalmazások támogatja</span><span class="sxs-lookup"><span data-stu-id="0aedb-110">Office mobile applications support</span></span>

| <span data-ttu-id="0aedb-111">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="0aedb-111">Apps</span></span> | <span data-ttu-id="0aedb-112">Támogatás</span><span class="sxs-lookup"><span data-stu-id="0aedb-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="0aedb-113">Azure Information Protection alkalmazás</span><span class="sxs-lookup"><span data-stu-id="0aedb-113">Azure Information Protection app</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="0aedb-115">Microsoft Teams</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="0aedb-117">OneNote</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="0aedb-119">OneDrive</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="0aedb-121">Outlook</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-123">A Power BI mobilalkalmazásokkal</span><span class="sxs-lookup"><span data-stu-id="0aedb-123">Power BI mobile apps</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-125">A Skype vállalati verzió</span><span class="sxs-lookup"><span data-stu-id="0aedb-125">Skype for Business</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-127">Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="0aedb-127">Word / Excel / PowerPoint</span></span> |![Jelölőnégyzet][1] |
| <span data-ttu-id="0aedb-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="0aedb-129">Yammer</span></span> |![Jelölőnégyzet][1] |


## <a name="requirements"></a><span data-ttu-id="0aedb-131">Követelmények</span><span class="sxs-lookup"><span data-stu-id="0aedb-131">Requirements</span></span> 

<span data-ttu-id="0aedb-132">Az eszköz operációs rendszere verziójúnak kell lennie az iOS 9-es vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="0aedb-132">The device OS version must be iOS 9 and above</span></span> 

<span data-ttu-id="0aedb-133">Az összevonási kiszolgáló be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="0aedb-133">A federation server must be configured.</span></span>  

<span data-ttu-id="0aedb-134">A Microsoft Authenticator Office-alkalmazások IOS szükség.</span><span class="sxs-lookup"><span data-stu-id="0aedb-134">Microsoft Authenticator is required for Office applications on iOS.</span></span>  

<span data-ttu-id="0aedb-135">Az Azure Active Directory ügyféltanúsítvány visszavonni az AD FS jogkivonat a következő jogcímeket kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="0aedb-135">For Azure Active Directory to revoke a client certificate, the ADFS token must have the following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="0aedb-136">(A sorozatszámát az ügyféltanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="0aedb-136">(The serial number of the client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="0aedb-137">(Az ügyfél-tanúsítvány kiállítója karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="0aedb-137">(The string for the issuer of the client certificate)</span></span> 

<span data-ttu-id="0aedb-138">Az Azure Active Directory ezeket a jogcímeket, hogy a frissítési jogkivonat ad hozzá, ha azok elérhetők az AD FS jogkivonat (vagy bármely más SAML-jogkivonat).</span><span class="sxs-lookup"><span data-stu-id="0aedb-138">Azure Active Directory adds these claims to the refresh token if they are available in the ADFS token (or any other SAML token).</span></span> <span data-ttu-id="0aedb-139">Ha a frissítési token érvényesíthető, ezek az információk segítségével ellenőrizze a visszavont tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="0aedb-139">When the refresh token needs to be validated, this information is used to check the revocation.</span></span> 

<span data-ttu-id="0aedb-140">Ajánlott eljárásként a az AD FS hibalapok frissítenie kell a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="0aedb-140">As a best practice, you should update the ADFS error pages with the following:</span></span>

* <span data-ttu-id="0aedb-141">IOS rendszerű eszközökön a Microsoft Authenticator telepítésével kapcsolatos követelmények</span><span class="sxs-lookup"><span data-stu-id="0aedb-141">The requirement for installing the Microsoft Authenticator on iOS</span></span>
* <span data-ttu-id="0aedb-142">Útmutatás felhasználói tanúsítvány beszerzésére.</span><span class="sxs-lookup"><span data-stu-id="0aedb-142">Instructions on how to get a user certificate.</span></span> 

<span data-ttu-id="0aedb-143">További részletekért lásd: [az AD FS bejelentkezési oldalainak testreszabása](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="0aedb-143">For more details, see [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>

<span data-ttu-id="0aedb-144">Néhány Office-alkalmazások (a modern hitelesítést) küldése "*kérdezzen rá a bejelentkezési =*" az Azure AD a kérelemben.</span><span class="sxs-lookup"><span data-stu-id="0aedb-144">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ to Azure AD in their request.</span></span> <span data-ttu-id="0aedb-145">Alapértelmezés szerint az Azure AD fordítja le a az AD FS a kérésben '*wauth = usernamepassworduri*"(kéri U/P hitelesítés elvégzéséhez AD FS) és"*wfresh = 0*"(kéri az AD FS figyelmen kívül az egyszeri bejelentkezési állapotát, és egy friss hitelesítést).</span><span class="sxs-lookup"><span data-stu-id="0aedb-145">By default, Azure AD translates this in the request to ADFS to ‘*wauth=usernamepassworduri*’ (asks ADFS to do U/P auth) and ‘*wfresh=0*’ (asks ADFS to ignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="0aedb-146">Ha szeretné engedélyezni a tanúsítvány alapú hitelesítést ezekhez az alkalmazásokhoz, módosítania az Azure AD alapértelmezés.</span><span class="sxs-lookup"><span data-stu-id="0aedb-146">If you want to enable certificate-based authentication for these apps, you need to modify the default Azure AD behavior.</span></span> <span data-ttu-id="0aedb-147">Állítson be a "*PromptLoginBehavior*"az összevont tartományt beállításait úgy, hogy"*letiltott*".</span><span class="sxs-lookup"><span data-stu-id="0aedb-147">Just set the ‘*PromptLoginBehavior*’ in your federated domain settings to ‘*Disabled*‘.</span></span> <span data-ttu-id="0aedb-148">Használhatja a [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) parancsmag, ez a feladat végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="0aedb-148">You can use the [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet to perform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="0aedb-149">Exchange ActiveSync-ügyfelek támogatását.</span><span class="sxs-lookup"><span data-stu-id="0aedb-149">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="0aedb-150">IOS 9-es vagy újabb a natív iOS e-mail ügyfél támogatott.</span><span class="sxs-lookup"><span data-stu-id="0aedb-150">On iOS 9 or later, the native iOS mail client is supported.</span></span> <span data-ttu-id="0aedb-151">Minden Exchange ActiveSync alkalmazások annak meghatározásához, ha ezt támogatja, lépjen kapcsolatba az alkalmazás fejlesztőjének.</span><span class="sxs-lookup"><span data-stu-id="0aedb-151">For all other Exchange ActiveSync applications, to determine if this feature is supported, contact your application developer.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="0aedb-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0aedb-152">Next steps</span></span>

<span data-ttu-id="0aedb-153">Ha Tanúsítványalapú hitelesítés konfigurálása a környezetben, tekintse meg a [első lépések az Android-alapú hitelesítés](active-directory-certificate-based-authentication-get-started.md) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="0aedb-153">If you want to configure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
