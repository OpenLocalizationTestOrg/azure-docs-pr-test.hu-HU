---
title: "Az Azure Active Directory B2C: Egyéni házirendek |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C egyéni házirendek"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1ff398a4-2079-4615-94f1-57de22c0aad6
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: bada9cf5f1a86f3dd047536f6fd9359c0231a384
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Az Azure Active Directory B2C: Egyéni házirendek

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>Mik azok a egyéni házirendeket?

Egyéni házirendek olyan konfigurációs fájlokat, amelyek meghatározzák az Azure AD B2C-bérlő hello viselkedését. Mivel **beépített házirendek** hello általános identitás feladatokat, egyéni házirendekkel lehet hello Azure AD B2C-portálon előre teljesen szerkesztheti az identitás fejlesztői toocomplete közel korlátlan számú feladatok. Olvassa el a toodetermine, ha egyéni házirend jobbra, és az identitás használatára.

**Nincs egyéni házirend szerkesztése mindenki számára.** a kötelező hello tanulási görbére, hello indítási idő már, és a jövőbeli változásokról toocustom házirendek hasonló szakértői toomaintain szükséges. Beépített házirendek gondosan tekintendő először a forgatókönyvnek előtt egyéni házirendekkel.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Beépített és egyéni házirendeket összehasonlítása

| | Beépített házirendek | Egyéni házirendek |
|-|-------------------|-----------------|
|Azon felhasználók megcélzása | Minden alkalmazásfejlesztők vagy identitás szakértői nélkül | Identitás szakemberek: rendszerintegrátoroktól, tanácsadókkal és házon belüli identitás csoportok. OpenIDConnect folyamatokkal részeként, és identitás-szolgáltatóktól és szolgáltatáson alapuló Jogcímalapú hitelesítés |
|A konfigurációs módszert | Az Azure portál egy felhasználóbarát felhasználói felületen | XML-fájlok közvetlen szerkesztésével és majd feltöltése az Azure-portálon toohello |
|Felhasználói felület testreszabása | Teljes felhasználói felület testreszabásával, HTML, CSS, jscript támogatja (egyéni tartomány igényel)<br><br>Egyéni karakterláncok többnyelvű támogatás | Azonos |
| Attribútum testreszabása | Szabványos és az egyéni attribútumok | Azonos |
|Token és munkamenet-kezelés | Egyéni token és több munkamenet-beállításokkal | Azonos |
|Identitás-szolgáltatóktól| **Ma**: előre meghatározott helyi, közösségi szolgáltató<br><br>**Jövőbeli**: szabványalapú OIDC, SAML, OAuth | **Ma**: OIDC szabványalapú OAUTH, a SAML<br><br>**Jövőbeli**: WsFed |
|Identitáskezelési tevékenységek (példák) | Regisztráció vagy bejelentkezés helyi és sok közösségi fiókok<br><br>Új jelszó önkiszolgáló kérése<br><br>Profil szerkesztése<br><br>Többtényezős hitelesítéssel kapcsolatos forgatókönyvek<br><br>Jogkivonatok és munkamenetek testreszabása<br><br>Hozzáférési jogkivonat adatfolyamok | Végezze el az egyéni identitás-szolgáltatóktól beépített házirendek azonos feladatokat hello, vagy használjon egyéni hatókörök<br><br>A regisztráció hello időpontban egy másik system felhasználó létesítése<br><br>Egy üdvözlő e-mailek küldése a saját e-mail-szolgáltató<br><br>A felhasználókhoz tartozó tárolóban kívül B2C használata<br><br>Ellenőrzi, hogy a felhasználó megadott információkat egy megbízható rendszer API-n keresztül |

## <a name="policy-files"></a>Házirend-fájlok

Egyéni házirendet egy jelenik meg egy vagy több XML-formátumú fájlok hierarchikus lánc tooeach vonatkoznak. hello XML-elemek meghatározása: jogcímek séma jogcímek átalakítások, a tartalom definíciókat, a jogcím-szolgáltatók és technikai profilok és a Userjourney vezénylési lépésekből egyéb elemek mellett.

Három típusú házirend fájlok hello használatát javasoljuk:

- **A KIINDULÓ fájl**, legtöbb hello definíciókat és amelyhez Azure biztosít-e egy teljes mintát, amely tartalmazza.  Javasoljuk, hogy egy toothis fájlt a hibaelhárításban toohelp változások minimális száma, és a hosszú távú karbantartási a házirendek
- **egy fájl kiterjesztések** hello egyedi konfigurációs módosításokat, amely tartalmazza a bérlő számára
- **a függő entitás (RP) fájl** Ez az hello egyetlen feladat-arra irányul, fájl, amelyet közvetlenül hello alkalmazáshoz vagy szolgáltatáshoz (más néven függő entitás).  Hello a cikk elolvasása házirend fájl definíciók további információt.  Minden egyedi feladathoz saját RP, és attól függően, hogy a követelmények branding hello száma "teljes kérelmek teljes száma a használati esetek x" lehet.


Beépített házirendek az Azure AD B2C kövesse hello 3-fájlminta csak láthatják hello függő entitás (RP) fájlt, miközben hello portal módosítást hajt végre a hello háttér toohello EXTenstions fájlban hello fejlesztői azonban a fenti ábrázolva.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Ha egyéni házirendekkel tudnia kell alapvető jellemzői

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Azure felhasználói identitások és hozzáférések kezelése (CIAM) szolgáltatás. hello szolgáltatást tartalmazza:

1. Egy felhasználó directory hello képernyőn egy speciális Azure Active Directory Microsoft Graph, és amely a helyi és összevont fiókok felhasználói adatokat tároló keresztül érhető el 
2. Hozzáférési toohello **identitás élmény keretrendszer** amely koordinálja a felhasználók és entitások közötti megbízhatósági kapcsolat, és jogcímek átadja közöttük toocomplete egy identitás/access fájlkezelési feladat 
3. Biztonságijogkivonat-szolgáltatás (STS) kiadásához azonosító jogkivonatokat, frissítési jogkivonatokat, és a hozzáférési jogkivonatok (vagy egyenértékű SAML helyességi feltételek), és tooprotect erőforrások ellenőrzését végzi.

Az Azure AD B2C együttműködő Identitásszolgáltatók, a felhasználók, a más rendszerekkel, és helyi felhasználói Directoryval hello feladatütemezési tooachieve identitás feladatról (pl. a felhasználó bejelentkezési regisztrálni egy új felhasználót, a jelszó alaphelyzetbe állítása). hello alapul szolgáló platform, amely több résztvevős megbízhatósági kapcsolatot hoz létre, és végrehajtja ezeket a lépéseket nevezik hello identitás élmény keretrendszer és egy (más néven az felhasználói út vagy egy megbízhatósági keretrendszer házirend) explicit módon meghatározza hello szereplője, hello műveletek hello protokollok és lépéseket toocomplete hello sorrendjét.

### <a name="identity-experience-framework"></a>Identity Experience Framework

Teljes mértékben konfigurálhatók, házirend-vezérelt, felhőalapú Azure platformon, például OpenIDConnect, OAuth, SAML, WSFed szabványos protokoll formátumokban (nagyjából Jogcímszolgáltatók) entitások közötti megbízhatósági kapcsolat vezénylő, és néhány nem szabványos néhányat a meglévők közül (pl. REST API-n alapú--rendszerről jogcímek cseréjét). hello I2E hoz felhasználóbarát, whitelabelled észlel, amely támogatja a HTML, CSS és jscript.  Ma hello identitás élmény keretrendszer csak környezetében hello hello Azure AD B2C-vel és a kapcsolódó feladatok tooCIAM előrébb.

### <a name="built-in-policies"></a>Beépített házirendek

Előre definiált konfigurációs fájlokat, hogy az Azure AD B2C tooperform leggyakrabban használt hello identitás közvetlen hello viselkedését feladatokat (azaz felhasználói regisztráció, bejelentkezés, jelszó-visszaállítás) és a megbízható entitások, amelynek kapcsolat is az Azure AD B2C (az előre definiált pl. Facebook identitásszolgáltató, LinkedIn, Microsoft Account, Google-fiókot).  A jövőbeli hello beépített házirendek is rendelkezhetnek testreszabási Identitásszolgáltatók, amelyek általában hello vállalati tartomány például az Azure Active Directory Premium, az Active Directory vagy az AD FS, a Salesforce azonosító szolgáltató stb.


### <a name="custom-policies"></a>Egyéni házirendek

Az Azure AD B2C-bérlő identitását élmény keretrendszer hello viselkedését meghatározó konfigurációs fájlok. Egyéni házirendet a következő helyen érhető el egy vagy több XML-fájlként (lásd a Házirendfájljait-definíciók) amely hello identitás élmény keretrendszer egy függő entitás (pl. egy alkalmazás) általi meghívásakor hajt végre. Egyéni házirendek lehetnek közvetlenül szerkesztheti az identitás fejlesztői toocomplete közel korlátlan számú feladatok. Egyéni házirendek konfigurálásakor meg kell adnia a fejlesztők hello gondos részletes tooinclude metaadatok végpontok lévő megbízható kapcsolatok, pontos jogcímek exchange-definíciókat, és minden identitásszolgáltatótól igény szerint konfigurálhatja a titkos kulcsokat, a kulcsoknak és tanúsítványoknak.

## <a name="policy-file-definitions-for-identity-experience-framework-trustframeworks"></a>Identitás élmény keretrendszer Trustframeworks házirendek fájl definíciói

### <a name="policy-files"></a>Házirend-fájlok

Egyéni házirendet egy jelenik meg egy vagy több XML-formátumú fájlok hierarchikus lánc tooeach vonatkoznak. hello XML-elemek meghatározása: jogcímek séma jogcímek átalakítások, a tartalom definíciókat, a jogcím-szolgáltatók és technikai profilok és a Userjourney vezénylési lépésekből egyéb elemek mellett.  Három típusú házirend fájlok hello használatát javasoljuk:

- **A KIINDULÓ fájl**, legtöbb hello definíciókat és amelyhez Azure biztosít-e egy teljes mintát, amely tartalmazza.  Javasoljuk, hogy egy toothis fájlt a hibaelhárításban toohelp változások minimális száma, és a hosszú távú karbantartási a házirendek
- **egy fájl kiterjesztések** hello egyedi konfigurációs módosításokat, amely tartalmazza a bérlő számára
- **a függő entitás (RP) fájl** Ez az hello egyetlen feladat-arra irányul, fájl, amelyet közvetlenül hello alkalmazáshoz vagy szolgáltatáshoz (más néven függő entitás).  Hello a cikk elolvasása házirend fájl definíciók további információt.  Minden egyedi feladathoz saját RP, és attól függően, hogy a követelmények branding hello száma "teljes kérelmek teljes száma a használati esetek x" lehet.

![Házirend fájltípusok](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| Házirend fájl típusa | Példák fájl neve | Ajánlott használata | Örököl |
|---------------------|--------------------|-----------------|---------------|
| ALAPJA |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_BASE1.xml | Hello core jogcímek séma, a jogcímek átalakítása, a Jogcímszolgáltatók és a felhasználó utak állította be a Microsoft tartalmazza<br><br>Minimális módosításait toothis fájl | None |
| A bővítmény (kiterjesztés) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_EXT.xml | A módosítások toohello alap fájl itt összesítése<br><br>Módosított Jogcímszolgáltatók<br><br>Módosított felhasználói utak<br><br>A saját egyéni sémadefiníciók | Alap fájl |
| Függő Entitásonkénti (RP) | B2C_1A_sign_up_sign_in.XML| Token alakzat és módosítása munkamenet Itt| Extensions(ext) fájl |

### <a name="inheritance-model"></a>Öröklési modellt

Amikor egy alkalmazás hello RP házirendfájl meghívja, hello identitás élmény keretrendszere B2C hozzáadja összes hello elem alapja, majd a BŐVÍTMÉNYEKET, és végül a hello RP házirend fájl tooassemble hello aktuális házirend érvényben.  Hello azonos írja be, és nevezze hello RP-fájl elemeinek felülírják hello bővítmények és bővítmények felülbírálások talál.

**Beépített házirendek** az Azure AD B2C kövesse a hello 3-fájlminta csak láthatják hello függő entitás (RP) fájlt, miközben hello portal módosítást hajt végre a hello háttér toohello EXTenstions fájlban hello fejlesztői azonban a fenti ábrázolva.  Az összes Azure AD B2C közösen használja az alap házirendfájl, amely hello Azure B2C team hello ellenőrzése alatt, és gyakran frissíteni.
