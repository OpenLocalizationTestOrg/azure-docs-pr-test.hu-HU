---
title: "aaaAzure AD v2 JS SPA interaktív telepítés – bevezetés |} Microsoft Docs"
description: "Hogyan JavaScript SPA-alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 7e4250ccca837a17489a99603da167009cc2e3d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Hívás hello Microsoft Graph API a egy JavaScript egyetlen lap alkalmazás (SPA)

Ez az útmutató ismerteti, hogyan bejelentkezhet az egy JavaScript egyetlen oldal alkalmazás (SPA) a személyes, munkahelyi és iskolai fiókok szereznie egy hozzáférési jogkivonatot, és hívja meg Microsoft Graph API hello vagy más API-k, a hozzáférési jogkivonatok igénylő hello Azure Active Directory v2 végpont.

### <a name="how-this-guide-works"></a>Ez az útmutató működése

![Ez az útmutató által generált hello mintaalkalmazás működése](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>További információ

Ez az útmutató által létrehozott hello mintaalkalmazás lehetővé teszi, hogy a JavaScript SPA tooquery hello Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el. Ebben a forgatókönyvben követően a felhasználó bejelentkezik, olyan hozzáférési jogkivonatot kért és hozzáadott tooHTTP kérelmek keresztül hello authorization fejlécet. Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Szalagtárak

Ez az útmutató a következő könyvtár hello használja:

|Részletes ismertetés|Leírás|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|A JavaScript Preview Microsoft hitelesítési kódtár|

> [!NOTE]
> *msal.js* célok hello *Azure Active Directory v2 végpont* – amely lehetővé teszi, hogy a személyes, iskolai és munkahelyi fiókokat toosign, és beszerezni a jogkivonatokat. Hello *Azure Active Directory v2 végpont* rendelkezik [bizonyos korlátozások](..\active-directory-v2-limitations.md). Ha érdekli, csak a munkahelyi és iskolai fiókok, *adal.js* és hello *V1 végpont*. hello v1 és v2 végpontok közötti toounderstand különbségek olvasási hello [v1-v2 összehasonlító](..\active-directory-v2-compare.md).

<!--end-collapse-->
