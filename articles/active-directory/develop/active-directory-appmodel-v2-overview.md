---
title: "az Active Directory v2.0-végponttól aaaAzure |} Microsoft Docs"
description: "Egy bevezető toobuilding-alkalmazások a Microsoft Account, mind az Azure Active Directory-bejelentkezés."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae5946b02c50ae5a6137dc1decae66c96647e875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Bejelentkezés Microsoft-Account & egyetlen alkalmazást az Azure AD-felhasználók
A hello részhez, az alkalmazás fejlesztőjének számára toosupport személyes Microsoft-fiókot is, és az Azure Active Directory munkahelyi fiókok volt szükséges toointegrate két külön rendszerrel.  Hello **az Azure AD v2.0-végponttól** bevezet egy új hitelesítési API-verzió, amely lehetővé teszi mindkét típusú fiókok használatával egy egyszerű integráció toosign.  Hello v2.0-végponttól használó alkalmazások is felhasználhat REST API-k a hello [Microsoft Graph](https://graph.microsoft.io) bármely típusú fiókot használja.

## <a name="getting-started"></a>Első lépések
Előnyben részesített platformját a következő lista toobuild hello olyan alkalmazást válasszon a nyílt forráskódú szalagtárak & keretrendszerek használatával.  Azt is megteheti használja az OAuth 2.0-s & OpenID Connect protokoll dokumentációja toosend & protokoll üzeneteket fogadni közvetlenül egy hitelesítési tár használata nélkül.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Újdonságok
Itt hello információk hasznosak bizonyulnak a Mi & Mi nem lehetséges hello v2.0-végponttal ismertetése.

* További tudnivalók: hello [típusú alkalmazásokat hozhat létre hello v2.0-végponttal](active-directory-v2-flows.md).
* Hello megértéséhez [korlátozások, korlátozások és megkötések](active-directory-v2-limitations.md) hello v2.0-végponttal.
* Ez az áttekintő videó hello v2.0-végpontra a kivétel:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Referencia
Ezek a hivatkozások akkor lehet hasznos, felderítését lehetővé tevő hello platform részletesen:

* [v2.0 protokoll referenciái](active-directory-v2-protocols.md)
* [v2.0 jogkivonat referenciái](active-directory-v2-tokens.md)
* [v2.0 kódtár – dokumentáció](active-directory-v2-libraries.md)
* [Hatókörök és hello v2.0-végponttól jóváhagyása](active-directory-v2-scopes.md)
* [a Microsoft Graph hello](https://graph.microsoft.io)

## <a name="help--support"></a>Súgó és támogatás
Ezek a hello legjobb helyen tooget segítségre fejlesztése az Azure Active Directoryban.

* [A Stack Overflow `azure-active-directory` és `adal` címkéi](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [Visszajelzés az Azure Active Directoryról](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> Ha csak a munkahelyi és iskolai fiókok az Azure Active Directory toosign, kell kezdődnie, a [az Azure AD – fejlesztői útmutató](active-directory-developers-guide.md).  hello v2.0-végpontra a explicit módon kell toosign személyes Microsoft-fiókok a fejlesztőknek szól.

