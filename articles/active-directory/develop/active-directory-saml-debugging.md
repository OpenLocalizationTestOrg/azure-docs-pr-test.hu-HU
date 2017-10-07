---
title: "SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban aaaHow toodebug |} Microsoft Docs"
description: "Megtudhatja, hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 846c7b3497c8842947c5b406f4362b9e06785b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-saml-based-single-sign-on-tooapplications-in-azure-active-directory"></a>Hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban
Hibakeresési információ egy SAML-alapú alkalmazások integrálása esetén gyakran hasznos toouse egy eszköz, például [Fiddler](http://www.telerik.com/fiddler) toosee hello hello tényleges SAML-jogkivonat toohello alkalmazás kiadott, a SAML kérelem és a hello SAML-válasz. Hello SAML-jogkivonat vizsgálatával győződjön meg arról, hogy az összes hello szükséges attribútumok, felhasználónév hello SAML tárgy hello és hello issuer URI a várt módon keresztül érkeznek.

![][1]

hello Azure ad-hello SAML-jogkivonat tartalmazó esetében általában hello egyet, amely egy HTTP 302 átirányítási után a https://login.windows.net, és a elküldött toohello **válasz URL-CÍMEN** hello alkalmazás. 

Válassza ezt a sort, és jelölje be hello megtekintheti hello SAML-jogkivonat **ellenőrök > WebForms** lapon hello jobb oldali panelen. Ott, kattintson a jobb gombbal hello **SAMLResponse** értékét, és válassza ki **tooTextWizard küldése**. Válassza ki **a Base64** a hello **átalakítási** menü toodecode hello jogkivonatot, és tekintse meg annak tartalmát.

**Megjegyzés:**: toosee hello tartalmát, a HTTP kérelem Fiddler késztethetik, HTTPS-forgalmat, amelyeket érdemes tooconfigure visszafejtése toodo.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](../active-directory-apps-index.md)
* [Egyszeri bejelentkezés tooapplications, amelyek nincsenek hello Azure Active Directory alkalmazáskatalógusában konfigurálása](../active-directory-saas-custom-apps.md)
* [Hogyan tooCustomize jogcím a kiállított hello SAML-jogkivonat Pre-Integrated alkalmazások](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png