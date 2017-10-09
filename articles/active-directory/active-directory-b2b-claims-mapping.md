---
title: "aaaB2B együttműködés a felhasználói jogcímek hozzárendelése az Azure Active Directoryban |} Microsoft Docs"
description: "a jogcímek referencia az Azure Active Directory B2B együttműködés leképezése"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 9e26085e91a6004b2f11286ae9c1df133bd47341
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>B2B együttműködés a felhasználói jogcímek hozzárendelése az Azure Active Directoryban

Az Azure Active Directory (Azure AD) támogatja az SAML-jogkivonat hello B2B együttműködés felhasználók számára kiállított hello jogcímek testreszabása. Amikor a felhasználók hitelesítése toohello alkalmazás, az Azure AD kibocsát egy SAML token toohello alkalmazást, amely tartalmazza az adatokat (vagy a jogcímek), amely egyedileg azonosítja hello felhasználóról. Alapértelmezés szerint ez magában foglalja a hello felhasználói felhasználó nevét, e-mail címét, Utónév és Vezetéknév. Megtekintheti vagy hello hello attribútumok lap SAML-jogkivonat toohello alkalmazást küldi hello jogcímek szerkesztése.

Ennek oka két miért szükség lehet a hello SAML-jogkivonat kiadott tooedit hello jogcímeket.

1. hello alkalmazás írása egy másik jogcím URI-azonosítók beállítása vagy jogcímértékek toorequire

2. Az alkalmazás által igényelt hello NameIdentifier jogcím toobe nem hello egyszerű felhasználónév az Azure Active Directoryban tárolja.

  ![nézet jogcímek SAML-jogkivonat](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Hogyan tooadd és Szerkesztés jogcímek információkért tekintse meg a jogcímek testreszabása, ez a cikk [hello SAML-jogkivonat előzetesen beépített alkalmazások az Azure Active Directoryban a kiállított jogcímek testreszabása](develop/active-directory-saml-claims-customization.md). A B2B együttműködés a felhasználók, NameID és UPN kereszt-bérlő leképezési biztonsági okokból nem.


## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználó tulajdonságai](active-directory-b2b-user-properties.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [B2bB együttműködés meghívókat delegálása](active-directory-b2b-delegate-invitations.md)
* [Dinamikus csoportok és a B2B együttműködés](active-directory-b2b-dynamic-groups.md)
* [B2B együttműködés kód és a PowerShell-példák](active-directory-b2b-code-samples.md)
* [B2B együttműködés SaaS-alkalmazások konfigurálása](active-directory-b2b-configure-saas-apps.md)
* [Külső Office 365-megosztás](active-directory-b2b-o365-external-user.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)
