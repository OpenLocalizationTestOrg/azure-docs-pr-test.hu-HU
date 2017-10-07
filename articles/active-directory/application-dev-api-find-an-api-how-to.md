---
title: "egy adott API-t egy egyéni által fejlesztett alkalmazás szükséges aaaHow toofind |} Microsoft Docs"
description: "Hogyan tooconfigure hello engedélyek tooaccess adott API-t az egyéni szükséges fejlesztése az Azure AD-alkalmazást"
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
ms.openlocfilehash: 7331129204d8b34b4ef9671749bd702f893768ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-a-specific-api-needed-for-a-custom-developed-application"></a>Hogyan toofind egy adott API szükséges egy egyéni által fejlesztett alkalmazás

Hozzáférés tooAPIs megkövetelése a hozzáférési hatókörök és a szerepkörök konfigurációja. Ha azt szeretné, tooexpose az erőforrás alkalmazás webes API-k tooclient alkalmazások, szükség van tooconfigure hozzáférési hatókörök és szerepkörök hello API. Ha azt szeretné, hogy egy ügyfél alkalmazás tooaccess webes API-k, kell tooconfigure engedélyek tooaccess hello API hello app regisztrációhoz.

## <a name="configuring-a-resource-application-tooexpose-web-apis"></a>Egy erőforrás tooexpose webes API-k konfigurálása

Amikor teszi ki a webes API-t hello API hello jeleníthető meg **API kiválasztása** engedélyek tooan app regisztrációs hozzáadásakor listában. tooadd hozzáférési hatókörök, kövesse az ismertetett hello lépéseket [tooyour erőforrás alkalmazás hozzáadása hozzáférési hatóköröket](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-tooaccess-web-apis"></a>Egy ügyfél tooaccess webes API-k konfigurálása

Engedélyek tooyour app regisztrációs hozzáadásakor is **API-hozzáférés hozzáadása** tooexposed webes API-k. tooaccess webes API-khoz, hello című rész lépéseit követve [adja hozzá a hitelesítő adatokat vagy engedélyek tooaccess webes API-khoz](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Következő lépések

-   [Alkalmazások integrálása az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Hello Azure Active Directory alkalmazásjegyzékének megismerése](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


