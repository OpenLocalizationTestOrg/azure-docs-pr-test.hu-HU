---
title: "Speciális tanúsítvány-aláírási előre integrált alkalmazások az Azure Active Directoryban a SAML-jogkivonat beállítások |} Microsoft Docs"
description: "Speciális tanúsítvány-aláírási előre integrált alkalmazások az Azure Active Directoryban a SAML-jogkivonat beállítások használata"
services: active-directory
documentationcenter: 
author: jeevansd
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 9c035dcb55af451d0dae71d7a0f5548a6ba3c0ed
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Speciális tanúsítvány-aláírási beállítások a SAML-jogkivonat galéria-alkalmazásokhoz az Azure Active Directoryban
Azure Active Directory (Azure AD) ma támogatja több ezer előre integrált alkalmazások az Azure Active Directory-Alkalmazásgyűjtemény. A telefonszám az 500-nál több alkalmazás, amely támogatja az egyszeri bejelentkezéshez a SAML 2.0 protokoll használatával tartalmaz. Amikor egy felhasználó egy alkalmazást az Azure AD a SAML használatával végez hitelesítést, az Azure AD egy token küld az alkalmazás (HTTP POST). Ezt követően az alkalmazás ellenőrzi, és jelentkezzen be a felhasználó nem kér felhasználónevet és jelszót a jogkivonat alapján. Ezeket a SAML-jogkivonatokat bejelentkezve az Azure AD-ben és a szabványos algoritmusok által létrehozott egyedi tanúsítvány.

Az Azure AD az alapértelmezett beállításokat használja, a gyűjtemény-alkalmazások. Az alapértelmezett értékek vannak beállítva az alkalmazás követelményei alapján.

Az Azure AD támogatja a speciális tanúsítvány-aláírási beállításait. Ezek a beállítások kiválasztásához először válassza ki azt a **megjelenítése speciális tanúsítvány-aláírási beállítások** jelölőnégyzetet:

![Speciális tanúsítvány-aláírási beállítások megjelenítése][1]

Miután kiválasztotta ezt a jelölőnégyzetet, állíthat be a tanúsítvány-aláírási beállítások és a tanúsítvány-aláírási algoritmus.

## <a name="certificate-signing-options"></a>A tanúsítvány-aláírási beállítások

Az Azure AD által támogatott három tanúsítvány-aláírási beállítások:

* **SAML-előfeltétel jelentkezzen**. A gyűjtemény alkalmazások többségét Ez az alapértelmezett beállítás be van állítva. Ezt a beállítást, ha az Azure AD szolgáltatásba az IdP aláírja a SAML-előfeltétel és a tanúsítványt, amelynek a X509 tanúsítvány az alkalmazás. Az aláírási algoritmus, amely a kijelölt használ is, a **aláírási algoritmus** legördülő listában.

* **SAML-válasz jelentkezzen**. Ezt a beállítást, ha az Azure AD szolgáltatásba az IdP aláírja a X509 az SAML-válasz tanúsítvány az alkalmazás. Az aláírási algoritmus, amely a kijelölt használ is, a **aláírási algoritmus** legördülő listában.

* **Jelentkezzen be a SAML-válasz és helyességi feltétel**. Ezt a beállítást, ha az Azure AD szolgáltatásba az IdP aláírja a teljes SAML-jogkivonat a X509 a tanúsítvány az alkalmazás. Az aláírási algoritmus, amely a kijelölt használ is, a **aláírási algoritmus** legördülő listában.

    ![A tanúsítvány-aláírási beállítások][4]

## <a name="certificate-signing-algorithms"></a>A tanúsítvány-aláírási algoritmus

Az Azure AD a SAML-válasz aláírásához két aláírási algoritmust támogat:

* **AZ SHA-256**. Az Azure AD az alapértelmezett algoritmus használ az SAML-válasz aláírásához. A legújabb algoritmus és a rendszer több biztonságos SHA-1-nél. Az alkalmazások többsége támogatja az SHA-256 algoritmust. Ha egy alkalmazás csak az SHA-1, az aláírási algoritmus támogatja, módosíthatja. Ellenkező esetben azt javasoljuk, hogy az SHA-256 algoritmust használ a SAML-válasz aláírását.

    ![Az SHA-256 tanúsítvány-aláírási algoritmus][3]

* **AZ SHA-1**. Ez a régebbi algoritmust, és akkor a rendszer kevesebb biztonságos SM-256-nál. Ha egy alkalmazás csak az aláírási algoritmus támogatja, válassza ezt a lehetőséget a **aláírási algoritmus** legördülő listából. Az Azure AD majd az SHA-1 algoritmussal SAML-válasz jelentkezik.

    ![Tanúsítvány-aláírási algoritmus az SHA-1][2]

## <a name="next-steps"></a>Következő lépések
* [Alkalmazások kezelése az Azure Active Directoryban cikk indexe](active-directory-apps-index.md)
* [Egyszeri bejelentkezés alkalmazásokhoz, amelyek nincsenek rajta az Azure Active Directory-Alkalmazásgyűjtemény konfigurálása](application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [SAML-alapú egyszeri bejelentkezést hibaelhárítása](develop/active-directory-saml-debugging.md)

<!--Image references-->

[1]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-advance-certificate.png
[2]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-algo-sha1.png
[3]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-algo-sha256.png
[4]: ./media/active-directory-enterprise-apps-advance-certificate-options/saml-signing-options.png
