---
title: "Az Azure Active Directory B2C: Egyéni szabályzatokkal kapcsolatos problémák elhárítása |} Microsoft Docs"
description: "Megismerni az megközelítések toosolving hibák egyéni házirendek használata az Azure Active Directoryban."
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: joroja
ms.openlocfilehash: b9e1b46df1ddb29cdb90953e4a0d6f1dd085441f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Egyéni házirendek az Azure AD B2C és identitás élmény keretrendszer hibaelhárítása

Ha az Azure Active Directory B2C használata egyéni házirendeket (az Azure AD B2C), Ön is szembesülhet állítja be a házirend language XML formátumban identitás élmény keretrendszer hello kihívást.  Learning toowrite egyéni házirendek lehet például egy új nyelvi tanulási. Ebben a cikkben azt ismertetik, eszközök és tippek, amelyek segítségével gyorsan felderítése és problémák megoldására. 

> [!NOTE]
> Ez a cikk foglalkozik hibaelhárítása az Azure AD B2C egyéni házirend-konfigurációt. Azt nem cím hello függő alkalmazás vagy az identitás-könyvtárban.

## <a name="xml-editing"></a>XML-szerkesztése

hello leggyakrabban előforduló hiba egyéni csoportházirendek beállításakor: nem megfelelően formázott XML. Majdnem elengedhetetlen a helyes XML-szerkesztő. A megfelelő XML-szerkesztő meg XML natív módon, tartalom Tartománykereső, közös feltételek prefills, XML-elemek indexelt tartja, valamint ellenőrizhesse a séma. Az alábbiakban a kedvenc XML-szerkesztő két:

* [Visual Studio Code](https://code.visualstudio.com/)
* [A Jegyzettömb ++](https://notepad-plus-plus.org/)

XML-sémaérvényesítése észleli a hibákat, mielőtt feltölti az XML-fájl. A gyökérmappában hello hello alapszintű csomag hello XML schema definíció TrustFrameworkPolicy_0.3.0.0.xsd beolvasása. További információ az XML-szerkesztőt hello dokumentációjában keresse meg *XML-eszközök* és *XML-érvényesítés*.

Bizonyára hasznosnak találja az XML-szabályok áttekintése hasznos. Az Azure AD B2C formázási hibákat észlel, XML-elutasítja. Alkalmanként helytelen formátumú XML okozhat, amelyek félrevezető hibaüzeneteket.

## <a name="upload-policies-and-policy-validation"></a>Házirendek és házirend érvényesítésének feltöltése

 XML-fájl feltöltése érvényesítés automatikus. A legtöbb hiba következtében hello feltöltés toofail. Érvényesítési feltölteni kívánt házirendfájlt hello tartalmazza. Hello lánc hello feltöltendő fájl hivatkozik túl fájlok is tartalmaz (függő entitás házirendfájl hello bővítmények fájl és hello alap fájl hello). 
 
 Gyakori ellenőrzési hibák hello következők.

Hiba kódtöredékre:`... makes a reference tooClaimType with id "displaName" but neither hello policy nor any of its base policies contain such an element`
* hello ClaimType érték lehet, hogy hibásan írta be, vagy nem létezik a hello séma.
* Hello fájlok hello házirend legalább egy ClaimType értékeket kell meghatározni. 
    Például:` <ClaimType Id="socialIdpUserId">`
* Ha ClaimType a hello bővítmények fájlban van definiálva, de is használatban van egy alapszintű hello fájl TechnicalProfile értéket, alapszintű hello-fájl feltöltése hibát eredményez.

Hiba kódtöredékre:`...makes a reference tooa ClaimsTransformation with id...`
* hello okok az hello hiba előfordulhat, hogy lehet hello ugyanaz, mint hello ClaimType hiba.

Hiba kódtöredékre:`Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order toomanage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`
* Ellenőrizze, hogy hello hello TenantId érték  **\<TrustFrameworkPolicy\>**  és  **\<BasePolicy\>**  elemek felel meg a cél Azure AD B2C-vel Bérlői.  

## <a name="troubleshoot-hello-runtime"></a>Hello futásidejű hibaelhárítása

* Használjon `Run Now` és `https://jwt.io` tootest a házirendek a web- vagy mobilalkalmazás függetlenül. A webhely úgy viselkedik, mint egy függő entitás alkalmazást. Hello JSON webes jogkivonat (JWT) az Azure AD B2C-házirend által létrehozott hello tartalmát jeleníti meg. egy teszt alkalmazás identitását élmény keretében, a következő értékeket használja hello toocreate:
    * Name: TestApp
    * Webalkalmazást vagy webes API: nincs
    * Natív ügyfél: nincs

* az ügyfél böngészője és az Azure AD B2C-ben használata között üzenetek tootrace hello exchange [Fiddler](http://www.telerik.com/fiddler). Ez segítheti utalhat, hogy ha a felhasználó használatában a vezénylési lépésekből a sikertelen.

* A **fejlesztői mód**, használjon **Application Insights** tootrace hello tevékenységét a identitás élmény keretrendszer felhasználói használatában. A **fejlesztői mód**, hello cseréjének közötti hello identitás élmény keretrendszer figyelheti és hello különböző jogcím-szolgáltatók, amelyeket műszaki profilokhoz, például a Identitásszolgáltatók, API-alapú szolgáltatásokhoz hello Azure AD B2C felhasználói könyvtárba, és más szolgáltatások, például az Azure több-Factor-hitelesítés.  

## <a name="recommended-practices"></a>Ajánlott eljárások

**Tartsa meg az esetek több verziója. Az alkalmazás a projekthez csoportosításához.** hello talál, a fájlnévkiterjesztéseket, valamint a függő entitás fájlok is közvetlenül egymástól függenek. Csoportként mentse őket. Új szolgáltatások jelentek meg tooyour házirendek, figyeljen külön működő verziók. Szakasz működő verziók saját fájlrendszer léphetnek kapcsolatba az hello alkalmazás kóddal.  Az alkalmazások számos különböző függő entitás házirendek bérlőjében előfordulhat, hogy meghívni. Ezek az Azure AD B2C-házirendek várható hello jogcímeket a függő válhatnak.

**Kialakításához, és műszaki profilokat az ismert felhasználói utak teszteléséhez.** Tesztelt alapszintű csomag házirendek tooset be a műszaki profilokat használja. Ezeket külön-külön előtt tesztelje a saját felhasználói utak beépítse őket.

**Kialakításához, és a felhasználó utak tesztelt műszaki profilok teszteléséhez.** Egy felhasználó út hello vezénylési lépésekből Növekményesen módosítása. A tervezett forgatókönyvek fokozatosan felépítéséhez.

## <a name="next-steps"></a>Következő lépések

* A Githubon töltse le a hello [active-directory-b2c-custom-policy-starterpack] (https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) .zip fájlt.
