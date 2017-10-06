---
title: "aaaListing az alkalmazás hello Azure Active Directory alkalmazáskatalógusában"
description: "Hogyan toolist egy alkalmazás, amely támogatja az egyszeri bejelentkezést a hello Azure Active Directory-gyűjtemény |} A Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 09ccd3b4645a180059b9a9d502e39f1b8933c988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="listing-your-application-in-hello-azure-active-directory-application-gallery"></a>Az alkalmazás hello Azure Active Directory alkalmazáskatalógusában listázása
egy alkalmazás, amely támogatja az egyszeri bejelentkezés az Azure Active Directoryban a hello toolist [az Azure AD-gyűjtemény](https://azure.microsoft.com/marketplace/active-directory/all/), először meg kell tooimplement a következő integrációs módok hello hello alkalmazás:

* **OpenID Connect** - közvetlen integráció az Azure AD OpenID Connect használata a hitelesítéshez és az Azure AD hozzájárulási API konfiguráció hello. Ha az alkalmazás nem támogatja az SAML-alapú integrációs csak indításakor, majd ez érték hello ajánlott módja.
* **SAML** – az alkalmazás már rendelkezik hello képességét tooconfigure harmadik fél Identitásszolgáltatók hello SAML protokoll használatával.

Listaelem követelmények az egyes módokban alatt van.

## <a name="openid-connect-integration"></a>OpenID Connect integráció
toointegrate az alkalmazás az Azure AD, a következő hello [fejlesztői útmutatás](active-directory-authentication-scenarios.md). Ezután fejezze be az alábbi kérdésekre hello és küldése toowaadpartners@microsoft.com.

* Azokat a hitelesítő adatokat egy tesztelési bérlőn vagy a fiókot, amely az Azure AD-csoport tootest hello integrációs hello használhatja az alkalmazással.  
* Hogyan hello Azure AD-csoport is bejelentkezhet, és csatlakozzon az Azure AD tooyour alkalmazást hello egy példányát vonatkozó utasítások [az Azure AD hozzájárulási keretrendszer](active-directory-integrating-applications.md#overview-of-the-consent-framework). 
* Adja meg a szükséges hello Azure AD a további utasításokat csapat tootest egyszeri bejelentkezést az alkalmazással. 
* Az alábbi hello adatainak megadása:

> Vállalat neve:
> 
> Vállalati webhely:
> 
> Alkalmazás neve:
> 
> Az alkalmazás leírása (200 karakter lehet):
> 
> Alkalmazás webhely (tájékoztató):
> 
> Alkalmazás technikai támogatási webhely vagy kapcsolattartási adatokat:
> 
> Alkalmazásazonosító hello alkalmazás látható módon: https://portal.azure.com hello alkalmazás részletei:
> 
> Alkalmazás Sign-Up URL-Címének toosign a felhasználók hová és/vagy beszerzési hello alkalmazás:
> 
> Válassza ki a felsorolt (a kategóriák lásd az Azure Active Directory Marketplace hello) alkalmazás toobe toothree kategóriáit fel:
> 
> Csatolja az alkalmazás kis ikon (PNG-fájlt, 45px 45px, teli háttérszíne szerint):
> 
> Csatolja az alkalmazás nagy méretben (PNG-fájlt, 215px 215px, teli háttérszíne szerint):
> 
> Csatolja az alkalmazás embléma (PNG-fájlt, 122px, háttérszínt által 150px):
> 
> 

## <a name="saml-integration"></a>SAML-integráció
Bármely alkalmazás, amely támogatja az SAML 2.0 integrálható közvetlenül az Azure AD-bérlő segítségével [ezen utasításokat tooadd egyéni alkalmazás](../active-directory-saas-custom-apps.md). Miután ellenőrizte, hogy működik-e az alkalmazások integrálása az Azure ad szolgáltatással, küldjön hello túl a következő információk<mailto:waadpartners@microsoft.com>.

* Azokat a hitelesítő adatokat egy tesztelési bérlőn vagy a fiókot, amely az Azure AD-csoport tootest hello integrációs hello használhatja az alkalmazással.  
* Adja meg a hello SAML bejelentkezési URL-címe, kiállítójának URL-címe (entitás azonosítója), és az alkalmazás értékei válasz URL-CÍMEN (helyességi feltétel fogyasztói szolgáltatás) leírtak [Itt](../active-directory-saas-custom-apps.md). Ha általában megadja ezeket az értékeket a SAML-metaadatfájl részeként, majd küldje, amely is.
* Adjon meg egy rövid leírást, hogy hogyan tooconfigure az Azure AD az alkalmazás SAML 2.0 használatával az identitás-szolgáltatóként. Ha az alkalmazás konfigurálása az Azure AD felügyeleti önkiszolgáló portál keresztül identitás-szolgáltatóként támogatja, majd győződjön meg arról fent megadott hello hitelesítő adatok is tartalmazzák hello képességét tooset a mentést.
* Az alábbi hello adatainak megadása:

> Vállalat neve:
> 
> Vállalati webhely:
> 
> Alkalmazás neve:
> 
> Az alkalmazás leírása (200 karakter lehet):
> 
> Alkalmazás webhely (tájékoztató):
> 
> Alkalmazás technikai támogatási webhely vagy kapcsolattartási adatokat:
> 
> Alkalmazás Sign-Up URL-Címének toosign a felhasználók hová és/vagy beszerzési hello alkalmazás:
> 
> Válassza ki a felsorolt alkalmazások toobe toothree kategóriáit be (További kategóriák: hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Csatolja az alkalmazás kis ikon (PNG-fájlt, 45px 45px, teli háttérszíne szerint):
> 
> Csatolja az alkalmazás nagy méretben (PNG-fájlt, 215px 215px, teli háttérszíne szerint):
> 
> Csatolja az alkalmazás embléma (PNG-fájlt, 122px, háttérszínt által 150px):
> 
> 

