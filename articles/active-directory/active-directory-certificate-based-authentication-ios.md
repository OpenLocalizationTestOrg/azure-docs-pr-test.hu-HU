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
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>IOS rendszerű eszközökön az Azure Active Directory-alapú hitelesítés

Tanúsítvány alapú hitelesítést (CBA) lehetővé teszi az Exchange online-fiókját az kapcsolódáskor egy Windows-, Android vagy iOS eszközön ügyféltanúsítvánnyal az Azure Active Directory hitelesített toobe: 

* Office-mobilalkalmazások például a Microsoft Outlook és a Microsoft Word   
* Exchange ActiveSync (EAS) ügyfeleket 

Ez a funkció beállítása nem hello kell tooenter egy felhasználónév és jelszó kombináció bizonyos mail és a Microsoft Office-alkalmazásokat a mobileszközön. 

Ez a témakör hello követelmények és támogatott hello forgatókönyvek CBA konfigurálásához a felhasználók számára a bérlő az Office 365 Enterprise, vállalati, oktatási, USA szövetségi kormányzatának, Kína iOS(Android)-eszközön, és Németország tervez.

Ez a funkció érhető el az előzetes verzió az Office 365 US Government védelmet és szövetségi csomagokban.




## <a name="office-mobile-applications-support"></a>Office-mobilalkalmazások támogatja

| Alkalmazások | Támogatás |
| --- | --- |
| Azure Information Protection alkalmazás |![Jelölőnégyzet][1] |
| Microsoft Teams |![Jelölőnégyzet][1] |
| OneNote |![Jelölőnégyzet][1] |
| OneDrive |![Jelölőnégyzet][1] |
| Outlook |![Jelölőnégyzet][1] |
| A Power BI mobilalkalmazásokkal |![Jelölőnégyzet][1] |
| A Skype vállalati verzió |![Jelölőnégyzet][1] |
| Word / Excel / PowerPoint |![Jelölőnégyzet][1] |
| Yammer |![Jelölőnégyzet][1] |


## <a name="requirements"></a>Követelmények 

hello az eszköz operációs rendszere verziójúnak kell lennie iOS 9-es vagy újabb verzió 

Az összevonási kiszolgáló be kell állítani.  

A Microsoft Authenticator Office-alkalmazások IOS szükség.  

Az Azure Active Directory toorevoke ügyféltanúsítványt hello az AD FS jogkivonat a következő jogcímeket hello kell rendelkeznie:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (hello hello ügyfél tanúsítvány sorozatszáma) 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (hello kibocsátó hello ügyféltanúsítvány hello karakterlánc) 

Az Azure Active Directory a jogcímek toohello frissítési jogkivonat ad hozzá, ha ilyenek hello az AD FS jogkivonat (vagy bármely más SAML-jogkivonat). Ha hello frissítési jogkivonat toobe ellenőrizni kell, ez az információ használt toocheck hello visszavont tanúsítványok. 

Ajánlott eljárásként frissítenie kell az AD FS hibalapok hello hello következőre:

* hello követelmény Microsoft Authenticator hello telepítéséhez IOS rendszerű eszközökön
* Útmutatás felhasználói tanúsítvány tooget. 

További részletekért lásd: [AD FS-bejelentkezés hello oldalainak testreszabása](https://technet.microsoft.com/library/dn280950.aspx).

Néhány Office-alkalmazások (a modern hitelesítést) küldése "*kérdezzen rá a bejelentkezési =*" tooAzure AD a kérelemben. Alapértelmezés szerint az Azure AD fordítja le ez a hello kérelem tooADFS túl "*wauth = usernamepassworduri*" (az AD FS toodo U/P auth kéri) és "*wfresh = 0*" (az AD FS tooignore SSO állapot kéri és egy friss hitelesítést) . Ha azt szeretné, hogy az alkalmazások tooenable tanúsítvány alapú hitelesítést, meg kell toomodify hello alapértelmezés az Azure AD. Csak a készlet hello "*PromptLoginBehavior*" az összevont tartományt beállításaiban túl "*letiltott*". Használhatja a hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) parancsmag tooperform ezt a feladatot:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync-ügyfelek támogatását.
Hello natív iOS e-mail ügyfélprogramokba IOS 9-es vagy újabb rendszeren támogatott. Minden Exchange ActiveSync alkalmazások toodetermine Ha ezt támogatja, lépjen kapcsolatba az alkalmazás fejlesztőjének.  


## <a name="next-steps"></a>Következő lépések

Ha tooconfigure tanúsítvány alapú hitelesítést a környezetben, tekintse meg a [első lépések az Android-alapú hitelesítés](active-directory-certificate-based-authentication-get-started.md) utasításokat.


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
