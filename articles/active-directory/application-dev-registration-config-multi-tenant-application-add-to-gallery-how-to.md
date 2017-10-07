---
title: "egy több-bérlős alkalmazás toohello az Azure AD application gallery aaaHow tooadd |} Microsoft Docs"
description: "Azt ismerteti, hogyan listázhatja a hello Azure AD Application Gallery saját fejlesztésű több-bérlős alkalmazásához"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2dc6e0d783835d2639a7e6dda172110ee860a977
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-multi-tenant-application-toohello-azure-ad-application-gallery"></a>Hogyan tooadd egy több-bérlős alkalmazás toohello az Azure AD application gallery

## <a name="what-is-hello-azure-ad-application-gallery"></a>Mi az Azure AD Application Gallery hello?

hello Azure AD Application Gallery egy kiváló módja tooget az alkalmazás Azure Active Directory felhasználók toobroaden hello hatással lehet az összes hello több millió elé, és az alkalmazás hello piactéren elérni. hello következő lépések azt ismertetik, hogyan listázhatja hello Azure AD Application Gallery az alkalmazást.

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Ha az alkalmazás támogatja az SAML-alapú vagy OpenIDConnect
Ha azt szeretné, hogy az Azure AD Application Gallery hello toolist egy több-bérlős-alkalmazást, először győződjön meg arról, hogy az alkalmazás támogatja a következő egyszeri bejelentkezési technológiák hello egyikét:

1. **OpenID Connect** - közvetlen integráció az Azure AD OpenID Connect használata a hitelesítéshez és az Azure AD hozzájárulási API konfiguráció hello. Ha az alkalmazás nem támogatja az SAML-alapú integrációs csak indításakor, majd ez érték hello ajánlott módja.
2. **SAML** – az alkalmazás már rendelkezik hello képességét tooconfigure harmadik fél Identitásszolgáltatók hello SAML protokoll használatával.

Ha az alkalmazás támogatja ezeket egyszeri bejelentkezési módok egyikét, és azt szeretné, hogy toolist az Azure AD Application Gallery hello több-bérlős alkalmazást, a lépésekkel hello hello dokumentum alatt. gyorsan tooget küldjön egy e-mailt túl**waadpartners@microsoft.com**.

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Ha az alkalmazás nem támogatja az SAML-alapú vagy OpenIDConnect
Akkor is, ha az alkalmazás nem támogatja e két beállítás közül, azt továbbra is integrálható, a gyűjtemény, a jelszót az egyszeri bejelentkezés technológiával. Ha azt szeretné, hogy tooexplore ezt a beállítást, küldhet e-mailt túl**waadpartners@microsoft.com**.

## <a name="next-steps"></a>Következő lépések
[Hogyan toolist az alkalmazás hello Azure Active Directory alkalmazáskatalógusában](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
